# Update a container user-defined field by calling an API

This topic describes the guidelines on how to customize the system to update a container user-defined field by calling an API.

## Scenario

Your organization configured the user-defined field **userDefinedCode7** as a container attribute and named it FeederDeepsea Flag. When a container is saved in the inventory database, this attribute is updated by a user to indicate whether the container is assigned to a feeder (F) or deep-sea (D) vessel visit. For a container, the FeederDeepsea Flag value is based on the vessel visit Berth Type:

* If Berth Type is Lighter or Domestic, the user updates FeederDeepsea Flag to F.
* If Berth Type is International, the user updates FeederDeepsea Flag to D.

For a transshipment (TS) container, FeederDeepsea Flag takes two values because it is assigned to an inbound and an outbound vessel visit. For example, if a container’s inbound vessel visit is Domestic and its outbound vessel visit is International, the user updates FeederDeepsea Flag to FD.

To update container details instantaneously, your organization wants the system to automatically update the FeederDeepsea Flag value when a container shipping status is updated to TS.

## Solution

You can use the built-in CNTR_UPDATE event to initiate a series of automatic actions when a container shipping status is updated to TS. The sequence of actions is as follows:

1. The system triggers CNTR_UPDATE event and initiates an execute code event action named RUN CNTR UPDATE CODE.
1. The RUN CNTR UPDATE CODE event action executes an event handler code extension named CNTR_UPDATE_CODE, which is used to fetch the FeederDeepsea Flag value based on the vessel visit Berth Type.
1. To fetch the vessel visit Berth Type, the system executes a code library code extension named COMMON_GET_CODE.
1. To update the FeederDeepsea Flag value based on the vessel visit Berth Type, the system executes a code library code extension named COMMON_UPDATE_CODE.

To implement this solution, you need to create COMMON_GET_CODE, followed by the COMMON_UPDATE_CODE, and then CNTR_UPDATE_CODE. Lastly, you need to add RUN CNTR UPDATE CODE to CNTR_UPDATE. Follow this sequence because a previous item is an input to the next item. The following topics describe how to create and configure the code extensions and event action.

## Step 1: Create the COMMON_GET_CODE code library

1. Go to **Home > Admin Menu > Code Extension**.
2. Click **Add Code Extension**.
3. Configure the following code extension attributes:

    a. **Code**: COMMON_GET_CODE

    b. **Name**: COMMON GET CODE

    c. **Extension Type**: Code Library

    d. **Target Object**: ContainerUnit

    e. **Language**: Groovy

4. In the **Contents** field, enter the following code used to fetch vessel visit information using GET Vessel Visit API:
    
    ```groovy
    import java.util.Map
    import java.util.HashMap
    import zodiac.zara.invoke.agent.context.EventContextService
    import zodiac.zara.zerba.base.event.def.NoticeEvent
    import zodiac.zara.invoke.agent.NoticeEventHandler
    import zodiac.zara.base.app.api.dto.ApiResponseDto
    import zodiac.zara.base.dto.ResponseDetailDto
    import zodiac.zara.base.enumeration.ResponseCode
    import zodiac.zara.base.enumeration.ResponseMessageType
    import zodiac.zara.invoke.agent.service.ApiService
    import groovy.json.JsonOutput
     
    class CommonGetService{
      public Object getVesselVisitInfo(String vvdCode, EventContextService ctxSvc) {
        Map visit = ctxSvc.getApiService().get('zodiac/core/api/v1/vessel-visits/'+vvdCode, null, new HashMap(), Map.class);
        return visit;
      }
    }
    ```
    
5. Click **Save**.

## Step 2: Create the COMMON_UPDATE_CODE code library

1. Go to **Home > Admin Menu > Code Extension**.
2. Click **Add Code Extension**.
3. Configure the following code extension attributes:

    a. **Code**: COMMON_UPDATE_CODE

    b. **Name**: COMMON UPDATE CODE

    c. **Extension Type**: Code Library

    d. **Target Object**: ContainerUnit

    e. **Language**: Groovy

4. In the **Contents** field, enter the following code used to update container information using POST Container Update API:
    
    ```groovy
    import java.util.Map
    import java.util.HashMap
    import zodiac.zara.invoke.agent.context.EventContextService
    class CommonUpdateService{
      public class ContainerUpdateDto {
        String txSuffix = 'CODE_EXT';
        String validatePossibleVal = '1';
        String overrideWarning = '1';
        String userDefinedCode7; //Feeder Deepsea Flag
      }
     
      public ContainerUpdateDto getContainerUpdateDto(){
        return new ContainerUpdateDto();
      }
     
      public void updateContainer(String cntrNo, ContainerUpdateDto cntrUpdate, EventContextService ctxSvc) {
        def log = ctxSvc.getLogger();
        try{
          def cntrResponse = ctxSvc.getApiService().post('zodiac/tms/api/v2/equipment/'+ cntrNo +'/update', cntrUpdate, new HashMap(), Map.class);
          log.info("Container Update Response" + cntrResponse);
        }catch (Exception inBizViolation)
          this.log('updateContainer groovy plugin error: ' + inBizViolation);
      }
    }
    ```
    
5. Click **Save**.

## Step 3: Create the CNTR_UPDATE_CODE event handler

1. Go to **Home > Admin Menu > Code Extension**.
2. Click **Add Code Extension**.
3. Configure the following code extension attributes:

    a. **Code**: CNTR_UPDATE_CODE

    b. **Name**: Update Container Inventory Code

    c. **Extension Type**: Notification Event Handler

    d. **Target Object**: ContainerUnit

    e. **Language**: Groovy

4. In the **Contents** field, enter the following code. This code consists of two methods:
   * `getFeederDeepseaFlag()`: This is used to fetch the Berth Type value of the inbound and outbound vessel visits using the COMMON_GET_CODE code library.
   * `onNotice()`: This is used to update the userDefinedCode7 field of a container using the COMMON_UPDATE_CODE code library.
    
    ```groovy
    import zodiac.zara.invoke.agent.context.EventContextService
    import zodiac.zara.zerba.base.event.def.NoticeEvent
    import zodiac.zara.invoke.agent.NoticeEventHandler
    import java.util.Map
    import java.util.HashMap
    import zodiac.zara.base.app.api.dto.ApiResponseDto
    import zodiac.zara.base.dto.ResponseDetailDto
    import zodiac.zara.base.enumeration.ResponseCode
    import zodiac.zara.base.enumeration.ResponseMessageType
    import zodiac.zara.invoke.agent.service.ApiService
    import groovy.json.JsonOutput
     
    class CntrUpdateHandler implements NoticeEventHandler {
      ApiResponseDto onNotice(Map noticeEvent, EventContextService ctxSvc, Map context) {
        def log = ctxSvc.getLogger();
        def targetObject = noticeEvent.data.target;
        def changeObject = noticeEvent.data.change;
        def originalObject = noticeEvent.data.original;
        def updateFlag = null;
        log.info("Target Object" + targetObject);
        log.info("Change Object" + changeObject);
     
        //Get containerUpdateDto
        def updateInformation =  ctxSvc.getLibrary('COMMON_UPDATE_CODE');
        def containerUpdate = updateInformation.getContainerUpdateDto();
     
        //Update FeederDeepseaFlag for Transshipment
        if(changeObject && changeObject.inVisit != null || changeObject.outVisit != null){
          if (targetObject && targetObject.shippingStatus == 'TS'){
            String feederDeepseaFlag = this.getFeederDeepseaFlag(targetObject, ctxSvc);
            ontainerUpdate.userDefinedCode7 = feederDeepseaFlag;
            updateFlag = 'Y';
            log.info("feederDeepseaFlag : " + feederDeepseaFlag + " for container : " + targetObject.eqpNbr);
          }
        }
        if (updateFlag == 'Y'){
          updateInformation.updateContainer(targetObject.eqpNbr,containerUpdate,ctxSvc);
        }
        ctxSvc.concludeSuccess();
      }
     
      public String getFeederDeepseaFlag(Object targetObject, EventContextService ctxSvc) {
        def inVisit = targetObject.inVisit;
        def outVisit = targetObject.outVisit;
        String feederDeepseaFlag = null;
     
        if (actualInVisit != null && actualInVisit != '' && actualOutVisit != null && actualOutVisit != ''){
          def commonGetCode = ctxSvc.getLibrary('COMMON_GET_CODE');
          def inVVD = commonGetCode.getVesselVisitInfo(inVisit,ctxSvc);
          def outVVD = commonGetCode.getVesselVisitInfo(outVisit,ctxSvc);
     
          if (inVVD != null && inVVD.content != null && outVVD != null && outVVD.content != null) {
            def inBerthType = inVVD.content.berthType;
            def outBerthType = outVVD.content.berthType;
     
            if (inBerthType == 'LIGHTER' || inBerthType == 'DOMESTIC') {
              if (outBerthType == 'INTERNATIONAL') {
                feederDeepseaFlag = 'FD';
              } else if (outBerthType == 'LIGHTER' || outBerthType == 'DOMESTIC') {
                feederDeepseaFlag = 'FF';
              }
            } else if (inBerthType == 'INTERNATIONAL') {
              if (outBerthType == 'INTERNATIONAL') {
                feederDeepseaFlag = 'DD';
              } else if (outBerthType == 'LIGHTER' || outBerthType == 'DOMESTIC') {
                feederDeepseaFlag = 'DF';
              }
            }
          }
        }
        return feederDeepseaFlag;
      }
    }
    ```
    
5. Click **Save**.

## Step 4: Add the RUN CNTR UPDATE CODE event action to the CNTR_UPDATE event type

1. Go to **Home > Admin Menu > Event Type**.
2. Search the **CNTR_UPDATE** event type, and then click **View**.
3. Click **Add Event Action**.
4. Configure the following event action attributes:

    a. **Event Action Name:** RUN CNTR UPDATE CODE

    b. **Event Action Type**: Execute Code

    c. **Code Extension**: CNTR_UPDATE_CODE

5. Click **Save**.
