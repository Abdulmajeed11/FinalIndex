# Server
### Table of contents 
### ALMOND TO MOBILE (Sending to Different server Commands)

- [RouterSummary,GetWirelessSettings,SetWirelessSettings,RebootRouter,SendLogs,FirmwareUpdate(Command 1100)](#1100a)
- [AlmondModeChange,AlmondNameChange (Command 63)](#63)
- [DynamicAlmondProperties (Command 1050)](#1050) 
- [DynamicAlmondNameChange (Command 49)](#49)
- [DynamicAlmondModeChange (Command 153)](#153)
- [CloudReset (Command 8)](#8)
- [DynamicDeviceUpdated,DynamicDeviceAdded (Command 1200)](#1200a)
- [DynamicDeviceList (Command 1200)](#1200b)
- [DynamicAllDevicesRemoved (Command 1200)](#1200c)
- [DynamicDeviceRemoved (Command 1200)](#1200d)
- [DynamicSceneAdded,DynamicSceneActivated,DynamicSceneUpdated (Command 1300)](#1300a)
- [DynamicSceneRemoved,DynamicAllSceneRemoved (Command 1300)](#1300b)
- [DynamicRuleAdded,DynamicRuleUpdated (Command 1400)](#1400a)
- [DynamicRuleRemoved (Command 1400)](#1400b)
- [DynamicAllRulesRemoved (Command 1400)](#1400c)
- [DynamicClientAdded (Command 1500)](#1500a)
- [DynamicClientUpdated (Command 1500)](#1500b)
- [DynamicClientRemoved (Command 1500)](#1500c)
- [DynamicClientList (Command 1500)](#1500d)
- [DynamicAllClientsRemoved (Command 1500)](#1500e)
- [DynamicClientJoined,DynamicClientLeft (Command 1500)](#1500f)
- [DynamicIndexUpdated (Command 1200)](#1200e)
- [AffiliationCompleteResponse (Command 1020)](#1020a)
- [ReadyToAddAsSlave,AddWiredSlaveMobile (Command 1600)](#1600)
----------------------------------------------------------------------------------------------
### ALMOND TO MOBILE (In the Same Server commands)

- [AlmondHello (Command 1040,Command 31)](#1040||31)
- [AffiliationAlmondRequest (Command 21)](#21)
- [KeepAlive (Command 104)](#104)
- [AlmondReset (Command 1030)](#1030)
- [AffiliationAlmondRequest (Command 1020)](#1020b)
- [AlmondValidationRequest (Command 106)](#106)
- [HashList (Command 1702)](#1702)
----------------------------------------------------------------------------------------------
### MOBILE TO ALMOND (Sending to Different Server Commands)

- [AddRule,UpdateRule,RemoveRule,RemoveAllRules,ValidateRule,GetDeviceIndex,UpdateDeviceName,
UpdateAlmondName,DeviceOnlineCheck,UpdateClient,RemoveClient,RemoveAllClients,WifiClients,
AddScene,SetScene,ActivateScene,DeleteScene,DeleteAllScene,UpdateDeviceIndex,UpdateScene,RemoveScene,RemoveAlllScenes (Command 1061,Command 1063)](#1061)
- [RouterSummary,GetWirelessSettings,SetWirelessSettings,RebootRouter,SendLogs,FirmwareUpdate(Command 1100)](#1100a)  
- [AffiliationUserRequest (Command 23)](#23)
- [AlmondModeChange (Command 61)](#61)
- [AffiliationUserRequest (Command 1023)](#1023)
- [UpdateDevicePreference (Command 1700)](#1700a)
- [UpdateClientPreferences (Command 1700)](#1700b)
- [UpdateClientPreferences (Command 1525)](#1525)
- [NotificationPreferences (Command 300)](#300)
- [Logoutall (Command 4)](#4)
- [Restore (Command 2222)](#2222)
- [ChangeUser (Command 1060,Action:Add)](#1060a)
- [ChangeUser (Command 1060,Action:Update)](#1060b)
- [UnlinkAlmondRequest (Command 1110)](#1110a)
- [UserInviteRequest (Command 1110)](#1110b)
- [UpdateUserProfileRequest (Command 1110)](#1110c)
- [DeleteSecondaryUserRequest (Command 1110)](#1110d)
- [DeleteMeAsSecondaryUserRequest (Command 1110)](#1110e)
- [ChangePasswordRequest (Command 1110)](#1110f)
- [DeleteAccountRequest (Command 1110)](#1110g)
- [SubscribeMe (Command 1011)](#1011a)
- [PaymentDetails (Command 1011)](#1011b)
- [UpdateCard (Command 1011)](#1011c)
- [DeleteSubscription (Command 1011)](#1011d)
----------------------------------------------------------------------------------------------
### MOBILE TO ALMOND (In the Same Server commands)

- [DeviceList (Command 1200)](#1200ms)
- [SceneList (Command 1300)](#1300ms)
- [RuleList (Command 1400)](#1400ms)
- [ClientList (Command 1500)](#1500ms)
- [AlmondProperties (Command 1050)](#1050ms)
- [GetDevicePreferences (Command 1700)](#1700c)
- [GetClientPreferences (Command 1700)](#1700d)
- [IOTScanResults (Command 1013)](#1013)
- [UpdateNotificationRegistration (Command 1800)](#1800)
- [Logout (Command 1900)](#1900)
- [GetAlmondList (Command 1112)](#1112)
- [GetClientPreferences (Command 1526)](#1526)
- [Login (Command 1003,Command 1)](#1003)
- [UserProfileRequest (Command 1110)](#1110ms)
- [Logout (Command 3)](#3)
- [Signup (Command 6)](#6)
- [CloudSanity (Command 102)](#102)
- [NotificationPreferenceList (Command 113)](#113)
- [AlmondModeRequest (Command 151)](#151)
- [NotificationAddRegistration (Command 281)](#281)
- [NotificationDeleteRegistration (Command 283)](#283)
- [Command 804 (Client)](#804a)
- [Command 804 (Device)](#804b)
- [Command 806 ](#806)
- [Command 800](#800)
- [SUPER_LOGIN (Command 1004)](#1004)

----------------------------------------------------------------------------------------------

### ALMOND TO MOBILE (Sending to Different server Commands)

<a name="1100a"></a>
## 1) Command 1100   (Almond to Mobile)
    Command no 
    1100- JSON format
 
    REQUIRED - 
    Command,CommandType,ICID,Payload

    REDIS - 
    (ALMOND SERVER)
    2.Get ICID_<packet.ICID>              

    QUEUE - 
    (ALMOND SERVER)
    3.send Response to queue             

    FUNCTIONAL - 
    (ALMOND SERVER)
    1.Command 1100     

    (MOBILE SERVER)   
    4.Command 1100          
    5.delete store[unicastID]    
    6.Send Res,commandLengthType to Mobile   

    FLOW - (ALMOND SERVER)
    almondProtocol(packet)-> processor(do)->processor(validate)->almondUsers(dummyModel)->processor(dispatchResponses),processor(unicast)->broadcaster(unicast)

<a name="63"></a>
## 2)Command 63
    Command no
    63- XML format
   
    REQUIRED -
    Command,ICID,UID,Payload

    REDIS -
    (ALMOND SERVER)
    2.Get ICID_<packet.ICID>              // value = null
    4.hgetall on UID_<user_list>          // Returns all the queues for users in user_list

    QUEUE -
    (ALMOND SERVER)
    3.Send Respose to queue from step 2
    5.Send Response to All Queues returned in Step 4

    FUNCTIONAL -
    (ALMOND SERVER)
    1.Command 63

    (MOBILE SERVER)
    6.Command 64
    7.Send Res,commandLengthType to Mobile

    FLOW - (ALMOND SERVER)
    almondProtocol(packet)-> processor(do)->processor(validate)->almondUsers(dummyModel)->processor(dispatchResponses),processor(unicast)->broadcaster(unicast) 

<a name="1050"></a>
## 3) DynamicAlmondProperties (Command 1050)
    Command no
    1050- JSON format
   
    REQUIRED -
    Command,AlmondMAC,Payload,CommandType

    SQL -
    (BACKGROUND SERVER)
    9.Select on ALMONDPROPERTIES
      params:AlmondMAC
    10.Update on AlmondProperties2
      params:AlmondMAC
    11.select from NotificationID 
      params:UserID
    23.Update on AlmondplusDB.NotificationID
        params:RegID

    /*if (oldRegid && oldRegid.length > 0) */
    24.Delete on AlmondplusDB.NotificationID
      params: RegI
    25.Select on SCSIDB.CMSAffiliations,AlmondplusDB.AlmondUsers,SCSIDB.CMS
       params: CA.CMSCode,AU.AlmondMAC 

    REDIS -
    (ALMOND SERVER)
    2.hmset AL_<AlmondMAC>              // value = [mapper.hashColumn, payload.HashNow]        
    5.hgetall on UID_<user_list>         // Returns all the queues for users in user_list

    (BACKGROUND SERVER)
    8.hmset on AL_<AlmondMAC>                   // params: [redisKey[key], data[key]]
    12.hmget on AL_<almondMAC>                     // params: ["name"]
    13.LPUSH on AlmondMAC_Device                // params: redisData

    /* if (res > count + 1) */
    14.LTRIM on AlmondMAC_Device                //here count = 9, res = Result from step 13
               
                (or)

    /* if (res == 1) */
    14.expire on AlmondMAC_Device             //here, res = Result from step 13

    15.LPUSH on AlmondMAC_All               //params: redisData
    /* if (res > count + 1) */
    16.LTRIM on AlmondMAC_All                //here count = 19, res = Result from step 15
               
               (or)

    /* if (res == 1) */
    16.expire on AlmondMAC_All             //here, res = Result from above step 15

    POSTGRES -
    (BACKGROUND SERVER)
    17.Insert on recentactivity
       params: mac, id, time, index_id, index_name, name, type, value

    CASSANDRA -
    (BACKGROUND SERVER)
    20.Insert on notification_store.notification_records
       params: usr_id, noti_time, i_time, msg
    21.Update on notification_store.badger  
       params: user_id  
    22.Select on notification_store.badger
       params: usr_id
    
    QUEUE -
    (ALMOND SERVER)
    4.Send AlmondProperties to BACKGROUND_QUEUE 
    6.Send Response to All Queues returned in Step 5

    FUNCTIONAL - 
    (ALMOND SERVER)
    1.Command 1050
    3.Send AlmondPropertiesResponse to Almond

    (BACKGROUND SERVER)
    7.Command 1050
    18.delete ans.AlmondMAC;
       delete ans.CommandType;
       delete ans.Action;
       delete ans.HashNow;
       delete ans.Devices;
       delete ans.epoch;
    19.delete input.users;

    FLOW - (ALMOND SERVER)
    almondProtocol(packet)->processor(do)->processor(validate)->almondUsers(properties)->processor(dispatchResponses),processor(sendToBackground)->broadcaster(sendToBackground)

    FLOW - (BACKGROUND SERVER)
    socket(packet)->controller(processor)->preprocessor(doNothing)->almondCommands(DynamicAlmondProperties)->genericModel(get)->receive(mainFunction)->receive(almondProperties)->generator(propertiesNotification)->cassQueries(qtoCassHistory)->cassQueries(qtoCassConverter)->msgService(notificationHandler)->msgService(handleResponse)->scsi(sendFinal)

<a name="49"></a>
## 4) DynamicAlmondNameChange (Command 49)
    Command no
    49- XML format

    REQUIRED -
    Command,UID,CommandType,Payload

    REDIS - 
    (ALMOND SERVER)
    4.hgetall on UID_<user_list>       // Returns all the queues for users in user_list

    (BACKGROUND SERVER)
    7.hmset on AL_<AlmondMAC>          // params: [redisKey[key], data[key]

    QUEUE - 
    (ALMOND SERVER)
    3.Send DynamicAlmondNameChange to BACKGROUND_QUEUE
    5.Send Response to All Queues returned in Step 4

    FUNCTIONAL -
    (ALMOND SERVER)
    1.Command 49
    2.Send DynamicAlmondNameChangeResponse to Almond

    (BACKGROUND SERVER)
    6.Command 49

    FLOW - (ALMOND SERVER)
    almondProtocol(packet)-> processor(do)->processor(validate)->almondUsers(dummyModel)->processor(dispatchResponses),processor(sendToBackground)->broadcaster(sendToBackground)->processor(broadcast)->broadcaster(send)

    FLOW - (BACKGROUND SERVER)
    socket(packet)->controller(processor)->preprocessor(doNothing)->almondCommands(almond_name_change)

<a name="153"></a>
## 5) DynamicAlmondModeChange (Command 153)
    Command no
    153- XML format

    REQUIRED -
    Command,UID,CommandType,Payload

    REDIS - 
    (ALMOND SERVER)
    4.hgetall on UID_<user_list>       // Returns all the queues for users in user_list

    (BACKGROUND SERVER)
    7.hmset on AL_<AlmondMAC>          // params: [redisKey[key], data[key]     

    QUEUE - 
    (ALMOND SERVER)
    3.Send DynamicAlmondModeChange to BACKGROUND_QUEUE
    5.Send Response to All Queues returned in Step 4

    FUNCTIONAL - 
    (ALMOND SERVER)
    1.Command 153
    2.Send DynamicAlmondModeChangeResponse to Almond

    (BACKGROUND SERVER)
    6.Command 153

    FLOW - (ALMOND SERVER)
    almondProtocol(packet)-> processor(do)->processor(validate)->almondUsers(almondmode_change)->processor(dispatchResponses),processor(sendToBackground)->broadcaster(sendToBackground)->processor(broadcast)->broadcaster(send)

    FLOW - (BACKGROUND SERVER)
    socket(packet)->controller(processor)->preprocessor(doNothing)->almondCommands(almond_name_change)

<a name="8"></a>
## 6) CloudReset (Command 8)
    Command no
    8- XML format

    REQUIRED -
    Command,CommandType,Payload

    REDIS -
    (ALMOND SERVER)
    4.hgetall on UID_<user_list>       // Returns all the queues for users in user_list

    QUEUE - 
    (ALMOND SERVER)
    3.Send CloudReset to BACKGROUND_QUEUE
    5.Send Response to All Queues returned in Step 4

    FUNCTIONAL - 
    (ALMOND SERVER)
    1.Command 8
    2.Send CloudResetResponse to Almond

    (MOBILE SERVER)
    6.Command 8
    7.Send Res,commandLengthType to Mobile

    FLOW - (ALMOND SERVER)
    almondProtocol(packet)-> processor(do)->processor(validate)->almondUsers(almondReset)->processor(dispatchResponses)->processor(sendToBackground)->broadcaster(sendToBackground)->processor(broadcast)->broadcaster(send)

<a name="1200a"></a>
## 7) DynamicDeviceUpdated,DynamicDeviceAdded (Command 1200)
    Command no
    1200- JSON format
   
    REQUIRED -
    Command,UID,CommandType,AlmondMAC,Payload

    SQL -
    (BACKGROUND SERVER)
    9.Insert on AlmondplusDB.DEVICE_DATA
      params:AlmondMAC
    10.Select on SCSIDB.CMSAffiliations,AlmondplusDB.AlmondUsers,SCSIDB.CMS
       params: CA.CMSCode,AU.AlmondMAC

    REDIS -
    (ALMOND SERVER)
    2.hmset on AL_<AlmondMAC>              // value = [mapper.hashColumn, payload.HashNow]  
    5.hgetall on UID_<user_list>           // Returns all the queues for users in user_list

    (BACKGROUND SERVER)
    /* if(Object.keys(variables).length==0) */
    multi   
    8.hmset on MAC:<AlmondMAC>:Key, variables    
    //Above, key = Device keys, variables = Device Values
     
                       (or)

     /* if (deviceArray.length>0) */
     multi
    8.hmset on MAC:<AlmondMAC>,deviceArray        //Where deviceAray =Device keys in Payload

    QUEUE -
    (ALMOND SERVER)
    4.Send DynamicDeviceUpdated||DynamicDeviceAdded to BACKGROUND_QUEUE
    6.Send Response to All Queues returned in Step 5

    FUNCTIONAL -
    (ALMOND SERVER)
    1.Command 1200
    3.Send DynamicDeviceUpdatedResponse||DynamicDeviceAddedResponse to Almond

    (BACKGROUND SERVER)
    7.Command 1200

    FLOW - (ALMOND SERVER)
    almondProtocol(packet)-> processor(do)->processor(validate)->almondUsers(execute)->processor(dispatchResponses),processor(sendToBackground)->broadcaster(sendToBackground)->processor(broadcast)->broadcaster(send)

    FLOW - (BCACKGROUND SERVER)
    consumer(processMessage)->controller(processor)->preprocessor(dynamicDeviceUpdated)->device(execute)->redisDeviceValue(update)->genericModel(update), genericModel(add)->receive(mainFunction)->scsi(sendFinal)

<a name="1200b"></a>
## 8) DynamicDeviceList (Command 1200)
    Command no
    1200- JSON format

    REQUIRED -
    Command,UID,CommandType,Payload

    SQL -
    (BACKGROUND SERVER)
    9.Select on DEVICE_DATA
      params:AlmondMAC
    13.Delete on DEVICE_DATA
      params: AlmondMAC
    14.Insert on AlmondplusDB.DEVICE_DATA
      params: AlmondMAC
    18.Select on SCSIDB.CMSAffiliations,AlmondplusDB.AlmondUsers,SCSIDB.CMS
      params: CA.CMSCode,AU.AlmondMAC

    REDIS -   
    (ALMOND SERVER)
    2.hmset on AL_<AlmondMAC>           //value = [mapper.hashColumn, payload.HashNow]
    5.hgetall on UID_<user_list>        // Returns all the queues for users in user_list

    (BACKGROUND SERVER)
    8.hgetall on MAC:<AlmondMAC>

    multi
    10.hmset on AL_<AlmondMAC>       // values = [deviceRestore,restore]

    multi
    11.hgetall on MAC:<AlmondMAC>:deviceIDs

    multi
    15.del on MAC:<almondMAC>

    multi
    16.del on MAC:<payload AlmondMAC>:<removeIds>

    /* if(Object.keys(variables).length==0) */
    multi   
    17.hmset on MAC:<AlmondMAC>:Key, variables    
    //Above key = Device keys, variables = Device Values
     
                       (or)

     /* if (deviceArray.length>0) */
     multi
    17.hmset on MAC:<AlmondMAC>,deviceArray        //Where deviceAray =Device keys in Payload

    CASSANDRA - -
    (BACKGROUND SERVER)
    12.Insert on notification_store.almondhistory
      params: mac,type,data,time

    QUEUE -
    (ALMOND SERVER)
    4.Send DynamicDeviceList to BACKGROUND_QUEUE
    6.Send Response to All Queues returned in Step 5

    FUNCTIONAL - 
    (ALMOND SERVER)
    1.Command 1200
    3.Send DynamicDeviceListResponse to Almond

    (BACKGROUND SERVER)
    7.Command 1200

    FLOW - (ALMOND SERVER)
    almondProtocol(packet)-> processor(do)->processor(validate)->almondUsers(execute)->processor(dispatchResponses),processor(sendToBackground)->broadcaster(sendToBackground)->processor(broadcast)->broadcaster(send)

    FLOW - (BACKGROUND SERVER)
    consumer(processMessage)->controller(processor)->preprocessor(dymamicAddAllDevice)->device(addAll)->redisDeviceValue(getDeviceList)->genericModel(insertBackUpAndUpdate)->device(getDevices)->genericModel(get)->redisDeviceValue(getFormatted)->genericModel(removeAndInsert)->redisDeviceValue(addAll)->receive(mainFunction)->scsi(sendFinal)

<a name="1200c"></a>
## 9) DynamicAllDevicesRemoved (Command 1200)
    Command no
    1200- JSON format

    REQUIRED -
    Command,UID,CommandType,Payload

    SQL -
    (BACKGROUND SERVER)
    13.Delete on DEVICE_DATA
      params: AlmondMAC
    14.Select on NotificationID
      params: UserID
    26.Update on AlmondplusDB.NotificationID
       params:RegID

    /*if (oldRegid && oldRegid.length > 0) */
    27.Delete on AlmondplusDB.NotificationID
        params: RegID
    28.Select on SCSIDB.CMSAffiliations,AlmondplusDB.AlmondUsers,SCSIDB.CMS
        params: CA.CMSCode,AU.AlmondMAC

    REDIS -
    (ALMOND SERVER)   
    2.hmset on AL_<AlmondMAC>          //value = [mapper.hashColumn, payload.HashNow]
    5.hgetall on UID_<user_list>        // Returns all the queues for users in user_list

    (BACKGROUND SERVER)
    8.hgetall on MAC:<AlmondMAC>

    multi
    9.del on MAC:<AlmondMAC>

    multi 
    10.del on  MAC:<AlmondMAC>:deviceIds

    multi
    11.hdel on AL_<AlmondMAC>           // value = deviceRestore

    15.hmget on AL_<AlmondMAC>
    16.LPUSH on AlmondMAC_Device                // params: redisData

    /* if (res > count + 1) */
    17.LTRIM on AlmondMAC_Device                //here count = 9, res = Result from step 16
               
                (or)

    /* if (res == 1) */
    17.expire on AlmondMAC_Device             //here, res = Result from step 16

    18.LPUSH on AlmondMAC_All               // params: redisData
    /* if (res > count + 1) */
    19.LTRIM on AlmondMAC_All                //here count = 19, res = Result from step 18
               
              (or)

    /* if (res == 1) */
    19.expire on AlmondMAC_All             //here, res = Result from above step 18

    POSTGRES - 
    (BACKGROUND SERVER)
    20.Insert on recentactivity
       params: mac, id, time, index_id, index_name, name, type, value

    CASSANDRA - 
    (BACKGROUND SERVER)
    12. Insert on  notification_store.almondhistory
        params: mac,type,data,time
    23.Insert on notification_store.notification_records
        params: usr_id, noti_time, i_time, msg
    24.Update on notification_store.badger  
        params: user_id  
    25.Select on notification_store.badger
        params: usr_id 

    QUEUE -
    (ALMOND SERVER)
    4.Send DynamicAllDevicesRemoved to BACKGROUND_QUEUE
    6.Send Response to All Queues returned in Step 5

    FUNCTIONAL -
    (ALMOND SERVER)
    1.Command 1200
    3.Send DynamicAllDevicesRemovedResponse to Almond

    (BACKGROUND SERVER)
    7.Command 1200
    21.delete ans.AlmondMAC;
       delete ans.CommandType;
       delete ans.Action;
       delete ans.HashNow;
       delete ans.Devices;
       delete ans.epoch
    22.delete input.users

    FLOW - (ALMOND SERVER)
    almondProtocol(packet)-> processor(do)->processor(validate)->almondUsers(execute)->processor(dispatchResponses),processor(sendToBackground)->broadcaster(sendToBackground)->processor(broadcast)->broadcaster(send)

    FLOW - (BACKGROUND SERVER)
    socket(packet)->controller(processor)->preprocessor(doNothing)->device(execute)->redisDeviceValue(removeAll)->genericModel(removeAll)->receive(mainFunction)->receive(AlwaysTrue)->generator(wifiNotificationGenerator)->cassQueries(qtoCassHistory)->cassQueries(qtoCassConverter)->msgService(notificationHandler)->msgService(handleResponse)->scsi(sendFinal)

<a name="1200d"></a>
## 10) DynamicDeviceRemoved (Command 1200)
    Command no
    1200- JSON format

    REQUIRED -
    Command,UID,CommandType,Payload

    SQL -
    (BACKGROUND SERVER)
    10.Delete on DEVICE_DATA
      params:AlmondMAC
    11.Select on SCSIDB.CMSAffiliations,AlmondplusDB.AlmondUsers,SCSIDB.CMS
      params: CA.CMSCode,AU.AlmondMAC

    REDIS -
    (ALMOND SERVER)   
    2.hmset on AL_<AlmondMAC>          //value = [mapper.hashColumn, payload.HashNow]
    5.hgetall on UID_<user_list>        // Returns all the queues for users in user_list

    (BACKGROUND SERVER)
    multi
    8.hdel on MAC:<AlmondMAC>,deviceID
     
    multi
    9.del on MAC:<AlmondMAC>:deviceID

    QUEUE -
    (ALMOND SERVER)
    4.Send DynamicDeviceRemoved to BACKGROUND_QUEUE
    6.Send Response to All Queues returned in Step 5

    FUNCTIONAL -
    (ALMOND SERVER)
    1.Command 1200
    3.Send DynamicDeviceRemovedResponse to Almond

    (BACKGROUND SERVER)
    7.Command 1200

    FLOW - (ALMOND SERVER)
    almondProtocol(packet)-> processor(do)->processor(validate)->almondUsers(execute)->processor(dispatchResponses),processor(sendToBackground)->broadcaster(sendToBackground)->processor(broadcast)->broadcaster(send)

    FLOW - (BACKGROUND SERVER)
    socket(packet)->controller(processor)->preprocessor(doNothing)->device(execute)->redisDeviceValue(remove)->genericModel(remove)->receive(mainFunction)->scsi(sendFinal)

<a name="1300a"></a>
## 11) DynamicSceneAdded,DynamicSceneActivated,DynamicSceneUpdated (Command 1300)
    Command no
    1300- JSON format
   
    REQUIRED -
    Command,UID,CommandType,Payload,AlmondMAC

    SQL -
    (BACKGROUND SERVER)
    8.Insert on AlmondplusDB.SCENE
      params:AlmondMAC

    REDIS -
    (ALMOND SERVER)
    2.hmset on AL_<AlmondMAC>          // value = [mapper.hashColumn, payload.HashNow]     
    5.hgetall on UID_<user_list>       // Returns all the queues for users in user_list

    QUEUE -
    (ALMOND SERVER)
    4.Send DynamicSceneAdded||DynamicSceneActivated||DynamicSceneUpdated to BACKGROUND_QUEUE
    6.Send Response to All Queues returned in Step 5

    FUNCTIONAL -
    (ALMOND SERVER)
    1.Command 1300
    3.Send DynamicSceneAddedResponse||DynamicSceneActivatedResponse|| DynamicSceneUpdatedResponse to Almond

    (BACKGROUND SERVER)
    7.Command 1300

    FLOW - (ALMOND SERVER)
    almondProtocol(packet)-> processor(do)->processor(validate)->almondUsers(execute)->processor(dispatchResponses),processor(sendToBackground)->broadcaster(sendToBackground)->processor(broadcast)->broadcaster(send) 

    FLOW - (BACKGROUND SERVER) 
    // For DynamicSceneAdded
    socket(packet)->controller(processor)->preprocessor(doNothing)->genericModel(execute)->genericModel(add) 

    // For DynamicSceneActivated,DynamicSceneUpdated
    socket(packet)->controller(processor)->preprocessor(doNothing)->genericModel(execute)->genericModel(update)

<a name="1300b"></a>
## 12) DynamicSceneRemoved,DynamicAllSceneRemoved (Command 1300)
    Command no
    1300- JSON format
   
    REQUIRED -
    Command,UID,CommandType,Payload,AlmondMAC

    SQL -
    (BACKGROUND SERVER)
    8.Delete on SCENE
      params:AlmondMAC

    REDIS -
    (ALMOND SERVER)
    2.hmset on AL_<AlmondMAC>          // value = [mapper.hashColumn, payload.HashNow]    
    5.hgetall on UID_<user_list>       // Returns all the queues for users in user_list

    QUEUE -
    (ALMOND SERVER)
    4.Send DynamicSceneRemoved||DynamicAllSceneRemoved to BACKGROUND_QUEUE
    6.Send Response to All Queues returned in Step 5

    FUNCTIONAL -
    (ALMOND SERVER)
    1.Command 1300
    3.Send DynamicSceneRemovedResponse||DynamicAllSceneRemoved to Almond

    (BACKGROUND SERVER)
    7.Command 1300

    FLOW - (ALMOND SERVER)
    almondProtocol(packet)->processor(do)->processor(validate)->almondUsers(execute)->processor(dispatchResponses),processor(sendToBackground)->broadcaster(sendToBackground)->processor(broadcast)->broadcaster(send)

    FLOW - (BACKGROUND SERVER)
    socket(packet)->controller(processor)->preprocessor(doNothing)->genericModel(execute)->genericModel(remove)

<a name="1400a"></a>
## 13) DynamicRuleAdded,DynamicRuleUpdated (Command 1400)
    Command no
    1400- JSON format

    REQUIRED -
    Command,UID,CommandType,Payload,AlmondMAC

    SQL -
    (BACKGROUND SERVER)
    8.Insert on AlmondplusDB.RULE
      params: AlmondMAC

    REDIS -
    (ALMOND SERVER)
    2.hmset on AL_<AlmondMAC>               // value = [mapper.hashColumn, payload.HashNow]
    5.hgetall on UID_<user_list>            // Returns all the queues for users in user_list

    QUEUE -
    (ALMOND SERVER)
    4.Send DynamicRuleAdded||DynamicRuleUpdated to BACKGROUND_QUEUE
    6.Send Response to All Queues returned in Step 5

    FUNCTIONAL -
    (ALMOND SERVER)
    1.Command 1400
    3.Send DynamicRuleAddedResponse||DynamicRuleUpdatedResponse to Almond

    (BACKGROUND SERVER)
    7.Command 1400

    FLOW - (ALMOND SERVER)
    almondProtocol(packet)->processor(do)->processor(validate)->almondUsers(execute)->processor(dispatchResponses),processor(sendToBackground)->broadcaster(sendToBackground)->processor(broadcast)->broadcaster(send)

    FLOW - (BACKGROUND SERVER)
    
    //For,DynamicRuleAdded
    consumer(processMessage)->controller(processor)->preprocessor(doNothing)->genericModel(execute),genericModel(add)

    //For,DynamicRuleUpdated
    consumer(processMessage)->controller(processor)->preprocessor(doNothing)->genericModel(execute),genericModel(Update)

<a name="1400b"></a>
## 14) DynamicRuleRemoved (Command 1400)
    Command no
    1400- JSON format

    REQUIRED -
    Command,UID,CommandType,Payload,AlmondMAC

    SQL - 
    (BACKGROUND SERVER)
    8.Delete on RULE
      params: AlmondMAC

    REDIS -
    (ALMOND SERVER)
    2.hmset on AL_<AlmondMAC>           // value = [mapper.hashColumn, payload.HashNow]
    5.hgetall on UID_<user_list>        // Returns all the queues for users in user_list

    QUEUE -
    (ALMOND SERVER)
    4.Send DynamicRuleRemoved to BACKGROUND_QUEUE
    6.Send Response to All Queues returned in Step 5

    FUNCTIONAL -
    (ALMOND SERVER)
    1.Command 1400
    3.Send DynamicRuleRemovedResponse to Almond

    (BACKGROUND SERVER)
    7.Command 1400

    FLOW - (ALMOND SERVER)
    almondProtocol(packet)->processor(do)->processor(validate)->almondUsers(execute)->processor(dispatchResponses),processor(sendToBackground)->broadcaster(sendToBackground)->processor(broadcast)->broadcaster(send)

    FLOW - (BACKGROUND SERVER)
    consumer(processMessage)->controller(processor)->preprocessor(doNothing)->genericModel(execute),genericModel(remove)

<a name="1400c"></a>
## 15) DynamicAllRulesRemoved (Command 1400)
    Command no
    1400- JSON format

    REQUIRED -
    Command,UID,CommandType,Payload,AlmondMAC

    CASSANDRA -
    (BACKGROUND SERVER)
    8.Insert on notification_store.almondhistory
      params: mac,type,data,time

    SQL -
    (BACKGROUND SERVER)
    9.Delete on RULE
      params: AlmondMAC

    REDIS -
    (ALMOND SERVER)
    2.hmset on AL_<AlmondMAC>           // value = [mapper.hashColumn, payload.HashNow]
    5.hgetall on UID_<user_list>        // Returns all the queues for users in user_list

    QUEUE -
    (ALMOND SERVER)
    4.Send DynamicAllRulesRemoved to BACKGROUND_QUEUE
    6.Send Response to All Queues returned in Step 5

    FUNCTIONAL -
    (ALMOND SERVER)
    1.Command 1400
    3.Send DynamicAllRulesRemovedResponse to Almond

    (BACKGROUND SERVER)
    7.Command 1400

    FLOW - (ALMOND SERVER)
    almondProtocol(packet)->processor(do)->processor(validate)->almondUsers(execute)->processor(dispatchResponses),processor(sendToBackground)->broadcaster(sendToBackground)->processor(broadcast)->broadcaster(send)

    FLOW - (BACKGROUND SERVER)
    consumer(processMessage)->controller(processor)->preprocessor(doNothing)->genericModel(execute),genericModel(removeAll)

<a name="1500a"></a>
## 16) DynamicClientAdded (Command 1500)
    Command no
    1500- JSON format

    REQUIRED -
    Command,UID,CommandType,Payload,AlmondMAC
    
    SQL -
    (BACKGROUND SERVER)
    8.Insert on AlmondplusDB.WIFICLIENTS
      params: AlmondMAC
    9.Select on NotificationID
      params: UserID
    21.Update on AlmondplusDB.NotificationID
       params:RegID
    
    /*if (oldRegid && oldRegid.length > 0) */
    22.Delete on AlmondplusDB.NotificationID
       params: RegID
    
    REDIS -
    (ALMOND SERVER)
    2.hmset on AL_<AlmondMAC>           // value = [mapper.hashColumn, payload.HashNow]
    5.hgetall on UID_<user_list>        // Returns all the queues for users in user_list

    (BACKGROUND SERVER)
    10.hmget on AL_<AlmondMAC>                   // params: ["name"]
    11.LPUSH on AlmondMAC_Client                 // params: redisData

    /* if (res > count + 1) */
    12.LTRIM on AlmondMAC_Client                //here count = 9, res = Result from step 11
               
               (or)

    /* if (res == 1) */
    12.expire on AlmondMAC_Client             //here, res = Result from step 11
    
    13.LPUSH on AlmondMAC_All                // params: redisData
    /* if (res > count + 1) */
    14.LTRIM on AlmondMAC_All                //here count = 19, res = Result from step 13
               
               (or)

    /* if (res == 1) */
    14.expire on AlmondMAC_All             //here, res = Result from above step 13

    POSTGRES -
    (BACKGROUND SERVER)
    15.Insert on recentactivity
      params: mac, id, time, index_id, client_id, name, type, value

    CASSANDRA -
    (BACKGROUND SERVER)
    18.Insert on notification_store.notification_records
       params: usr_id, noti_time, i_time, msg 
    19.Update on notification_store.badger
       params: usr_id
    20.Select on notification_store.badger
       params: usr_id 

    QUEUE -
    (ALMOND SERVER)
    4.Send DynamicClientAdded to BACKGROUND_QUEUE
    6.Send Response to All Queues returned in Step 5

    FUNCTIONAL -
    (ALMOND SERVER)
    1.Command 1500
    3.Send DynamicClientAddedResponse to Almond

    (BACKGROUND SERVER)
    7.Command 1500
    16.delete ans.AlmondMAC;
      delete ans.CommandType;
      delete ans.Action;
      delete ans.HashNow;
      delete ans.Devices;
      delete ans.epoch;
    17.delete input.users;

    FLOW - (ALMOND SERVER)
    almondProtocol(packet)->processor(do)->processor(validate)->almondUsers(execute)->processor(dispatchResponses),processor(sendToBackground)->broadcaster(sendToBackground)->processor(broadcast)->broadcaster(send)

    FLOW - (BACKGROUND SERVER)
    socket(packet)->controller(processor)->preprocessor(doNothing)->genericModel(execute)->genericModel(add)->receive(mainFunction)->notify(sendAlwaysClient)->generator(wifiNotificationGenerator)->CASSANDRA -(qtoCassHistory)->CASSANDRA -(qtoCassConverter)->msgService(notificationHandler)->msgService(handleResponse)

<a name="1500b"></a>
## 17) DynamicClientUpdated (Command 1500)
    Command no
    1500- JSON format

    REQUIRED -
    Command,UID,CommandType,Payload,AlmondMAC

    SQL -
    (BACKGROUND SERVER)
    8.Insert on AlmondplusDB.WIFICLIENTS
      params: AlmondMAC

    REDIS -
    (ALMOND SERVER)
    2.hmset on AL_<AlmondMAC>              // value = [mapper.hashColumn, payload.HashNow]
    5.hgetall on UID_<user_list>           // Returns all the queues for users in user_list

    QUEUE -
    (ALMOND SERVER)
    4.Send DynamicClientUpdated to BACKGROUND_QUEUE
    6.Send Response to All Queues returned in Step 5

    FUNCTIONAL -
    (ALMOND SERVER)
    1.Command 1500
    3.Send DynamicClientUpdatedResponse to Almond

    (BACKGROUND SERVER)
    7.Command 1500

    FLOW - (ALMOND SERVER)
    almondProtocol(packet)-> processor(do)->processor(validate)->almondUsers(execute)->processor(dispatchResponses),processor(sendToBackground)->broadcaster(sendToBackground)->processor(broadcast)->broadcaster(send)

    FLOW - (BACKGROUND SERVER)
    socket(packet)->controller(processor)->preprocessor(doNothing)->genericModel(execute)->genericModel(update)->receive(mainFunction)

<a name="1500c"></a>
## 18) DynamicClientRemoved (Command 1500)
    Command no
    1500- JSON format

    REQUIRED -
    Command,UID,CommandType,Payload,AlmondMAC

    SQL -
    (BACKGROUND SERVER)
    8.Select on WifiClients
      params: AlmondMAC,ClientID
    9.Delete on WIFICLIENTS
      params: AlmondMAC
    10.Select on NotificationID
      params: UserID
    22.Update on AlmondplusDB.NotificationID
        params:RegID

     /*if (oldRegid && oldRegid.length > 0) */
     23.Delete on AlmondplusDB.NotificationID
        params: RegID

    REDIS -
    (ALMOND SERVER) 
    2.hmset on AL_<AlmondMAC>          //value = [mapper.hashColumn, payload.HashNow]
    5.hgetall on UID_<user_list>       // Returns all the queues for users in user_list

    (BACKGROUND SERVER)
    11.hmget on AL_<AlmondMAC>                 //// params: ["name"]
    12.LPUSH on AlmondMAC_Client               // params: redisData

     /* if (res > count + 1) */
    13.LTRIM on AlmondMAC_Client                //here count = 9, res = Result from step 12
               
                (or)

     /* if (res == 1) */
    13.expire on AlmondMAC_Client             //here, res = Result from step 12

    14.LPUSH on AlmondMAC_All               // params: redisData
     /* if (res > count + 1) */
    15.LTRIM on AlmondMAC_All                //here count = 19, res = Result from step 14
               
               (or)

    /* if (res == 1) */
    15.expire on AlmondMAC_All             //here, res = Result from above step 14

    POSTGRES -
    (BACKGROUND SERVER)
    16.Insert on recentactivity
       params: mac, id, time, index_id, client_id, name, type, value

    CASSANDRA -
    (BACKGROUND SERVER)
    19.Insert on notification_store.notification_records
       params: usr_id, noti_time, i_time, msg
    20.Update on notification_store.badger  
       params: user_id  
    21.Select on notification_store.badger
       params: usr_id 

    QUEUE -
    (ALMOND SERVER)
    4.Send DynamicClientRemoved to BACKGROUND_QUEUE
    6.Send Response to All Queues returned in Step 5

    FUNCTIONAL - 
    (ALMOND SERVER)
    1.Command 1500
    3.Send DynamicClientRemovedResponse to Almond

    (BACKGROUND SERVER)
    7.Command 1500
    17.delete ans.AlmondMAC;
        delete ans.CommandType;
        delete ans.Action;
        delete ans.HashNow;
        delete ans.Devices;
        delete ans.epoch;
     18.delete input.users;

    FLOW - (ALMOND SERVER) 
    almondProtocol(packet)->processor(do)->processor(validate)->almondUsers(execute)->processor(dispatchResponses),processor(sendToBackground)->broadcaster(sendToBackground)->processor(broadcast)->broadcaster(send)

    FLOW - (BACKGROUND SERVER)
    socket(packet)->controller(processor)->preprocessor(dynamicClientRemoved)->genericModel(execute)->genericModel(remove)->receive(mainFunction)->receive(sendAlwaysClient)->generator(wifiNotificationGenerator)->cassQueries(qtoCassHistory)->cassQueries(qtoCassConverter)->msgService(notificationHandler)->msgService(handleResponse)

<a name="1500d"></a>
## 19) DynamicClientList (Command 1500)
    Command no
    1500- JSON format

    REQUIRED -
    Command,UID,CommandType,Payload

    SQL -
    (BACKGROUND SERVER)
    8.Insert on  AlmondplusDB.WIFICLIENTS
      params: AlmondMAC
    9.Delete on WIFICLIENTS
      params: AlmondMAC
    10.Insert on  AlmondplusDB.WIFICLIENTS
      params: AlmondMAC

    REDIS -
    (ALMOND SERVER)   
    2.hmset on AL_<AlmondMAC>           // value = [mapper.hashColumn, payload.HashNow]
    5.hgetall on UID_<user_list>        // Returns all the queues for users in user_list

    QUEUE -
    (ALMOND SERVER)
    4.Send DynamicClientList to BACKGROUND_QUEUE
    6.Send Response to All Queues returned in Step 5

    FUNCTIONAL -
    (ALMOND SERVER)
    1.Command 1500
    3.Send DynamicClientListResponse to Almond

    (BACKGROUND SERVER)
    7.Command 1500

    FLOW - (ALMOND SERVER)
    almondProtocol(packet)->processor(do)->processor(validate)->almondUsers(execute)->processor(dispatchResponses),processor(sendToBackground)->broadcaster(sendToBackground)->processor(broadcast)->broadcaster(send)

    FLOW -  (BACKGROUND SERVER)
    socket(packet)->controller(processor)->preprocessor(doNothing)->genericModel(execute)->genericModel(addAll),genericModel(insertBackUpAndUpdate),genericModel(get),CASSANDRA -(execute)->genericModel(removeAndInsert)->receive(mainFunction)

<a name="1500e"></a>
## 20) DynamicAllClientsRemoved (Command 1500)
    Command no
    1500- JSON format

    REQUIRED -
    Command,UID,CommandType,Payload

    SQL -
    (BACKGROUND SERVER)
    9.Delete on WIFICLIENTS
      params: AlmondMAC
    10.Select on NotificationID
      params: UserID
    17.Update on AlmondplusDB.NotificationID
       params:RegID

     /*if (oldRegid && oldRegid.length > 0) */
    18.Delete on AlmondplusDB.NotificationID
        params: RegID

    REDIS -
    (ALMOND SERVER)    
    2.hmset on AL_<AlmondMAC>           // value = [mapper.hashColumn, payload.HashNow]
    5.hgetall on UID_<user_list>        // Returns all the queues for users in user_list
    
    (BACKGROUND SERVER)
    11.hmget on AL_<AlmondMAC>

    CASSANDRA -
    (BACKGROUND SERVER) 
    8.Insert on notification_store.almondhistory
      params: mac,type,data,time
    14.Insert on notification_store.notification_records
      params: usr_id, noti_time, i_time, msg
    15.Update on notification_store.badger  
       params: user_id  
    16.Select on notification_store.badger
       params: usr_id 

    QUEUE -
    (ALMOND SERVER)
    4.Send DynamicAllClientRemoved to BACKGROUND_QUEUE
    6.Send Response to All Queues returned in Step 5

    FUNCTIONAL -
    (ALMOND SERVER)
    1.Command 1500
    3.Send DynamicAllClientsRemovedResponse to Almond

    (BACKGROUD SERVER)
    7.Command 1500
    12.delete ans.AlmondMAC;
        delete ans.CommandType;
        delete ans.Action;
        delete ans.HashNow;
        delete ans.Devices;
        delete ans.epoch;
     13.delete input.users;

    FLOW - (ALMOND SERVER)
    almondProtocol(packet)->processor(do)->processor(validate)->almondUsers(execute)->processor(dispatchResponses),processor(sendToBackground)->broadcaster(sendToBackground)->processor(broadcast)->broadcaster(send)

    FLOW - (BACKGROUND SERVER)
    socket(packet)->controller(processor)->preprocessor(doNothing)->genericModel(execute)->genericModel(removeAll)->receive(mainFunction)->receive(AlwaysTrue)->generator(wifiNotificationGenerator)->cassQueries(qtoCassConverter)->msgService(notificationHandler)->msgService(handleResponse)

<a name="1500f"></a>
## 21) DynamicClientJoined,DynamicClientLeft (Command 1500)
    Command no
    1500- JSON format

    REQUIRED -
    Command,UID,CommandType,Payload

    SQL -
    (BACKGROUND SERVER)
    8.Insert on AlmondplusDB.WIFICLIENTS
    params: AlmondMAC
    9.Select on AlmondplusDB.WifiClientsNotificationPreferences
      params: AlmondMAC,ClientID,UserID
    10.Select on NotificationID
      params: UserID
    22.Update on AlmondplusDB.NotificationID
       params:RegID

    /*if (oldRegid && oldRegid.length > 0) */
    23.Delete on AlmondplusDB.NotificationID
       params: RegID

    REDIS -
    (ALMOND SERVER)   
    2.hmset on AL_<AlmondMAC>           // value = [mapper.hashColumn, payload.HashNow]
    5.hgetall on UID_<user_list>        // Returns all the queues for users in user_list

    (BACKGROUND SERVER)
    11.hmget on AL_<almondMAC>           // params: ["name"]
    12.LPUSH on AlmondMAC_Client         // params: redisData

    /* if (res > count + 1) */
    13.LTRIM on AlmondMAC_Client                //here count = 9, res = Result from step 12
               
                (or)

    /* if (res == 1) */
    13.expire on AlmondMAC_Client             //here, res = Result from step 12

    14.LPUSH on AlmondMAC_All               //params: redisData
    /* if (res > count + 1) */
    15.LTRIM on AlmondMAC_All                //here count = 19, res = Result from step 14
               
               (or)

    /* if (res == 1) */
    15.expire on AlmondMAC_All             //here, res = Result from above step 14

    POSTGRES -
    (BACKGROUND SERVER)
    16.Insert on recentactivity
       params: mac, id, time, index_id, client_id, name, type, value

    CASSANDRA -
    (BACKGROUND SERVER)
    19.Insert on notification_store.notification_records
       params: usr_id, noti_time, i_time, msg
    20.Update on notification_store.badger  
       params: user_id  
    21.Select on notification_store.badger
       params: usr_id

    QUEUE -
    (ALMOND SERVER)
    4.Send DynamicClientJoined||DynamicClientLeft to BACKGROUND_QUEUE
    6.Send Response to All Queues returned in Step 5

    FUNCTIONAL -
    (ALMOND SERVER)
    1.Command 1500
    3.Send DynamicClientJoinedResponse||DynamicClientLeftResponse to Almond

    (BACKGROUND SERVER)
    7.Command 1500
    17. delete ans.AlmondMAC;
        delete ans.CommandType;
        delete ans.Action;
        delete ans.HashNow;
        delete ans.Devices;
        delete ans.epoch;
    18.delete input.users;

    FLOW - (ALMOND SERVER)
    almondProtocol(packet)->processor(do)->processor(validate)->almondUsers(execute)->processor(dispatchResponses),processor(sendToBackground)->broadcaster(sendToBackground)->processor(broadcast)->broadcaster(send)

    FLOW - (BACKGROUND SERVER)
    socket(packet)->controller(processor)->preprocessor(doNothing)->genericModel(execute)->genericModel(update)->receive(mainFunction)->receive(checkClientPreference)->generator(wifiNotificationGenerator)->cassQueries(qtoCassHistory)->cassQueries(qtoCassConverter)->msgService(notificationHandler)->msgService(handleResponse)

<a name="1200e"></a>
## 22) DynamicIndexUpdated (Command 1200)
    Command no
    1200- JSON format
   
    REQUIRED -
    Command,UID,AlmondMAC,CommandType,Payload

    SQL -
    (BACKGROUND SERVER)
    12.Select on AlmondplusDB.NotificationPreferences
       params: AlmondMAC,DeviceID,UserID
    13.Select on NotificationID
      params:UserID
    14.Select on DeviceData
       params: AlmondMAC,DeviceID
    25.Update on AlmondplusDB.NotificationID
       params:RegID

    /*if (oldRegid && oldRegid.length > 0) */
    26.Delete on AlmondplusDB.NotificationID
       params: RegID
    27.Select on SCSIDB.CMSAffiliations,AlmondplusDB.AlmondUsers,SCSIDB.CMS
       params: CA.CMSCode,AU.AlmondMAC 

    REDIS -
    (ALMOND SERVER)
    2.hmset on AL_<AlmondMAC>             // value = [mapper.hashColumn, payload.HashNow]
    7.hgetall on UID_<user_list>         // Returns all the queues for users in user_list

    (BACKGROUND SERVER)
    /*if (data.index==0 && packet.cmsCode)*/
    10. hgetall on MAC:<AlmondMAC>,data.DeviceID   
                    
             (or)

    10. Return

    /* if(Object.keys(variables).length==0) */
      multi   
    11.hmset on MAC:<AlmondMAC>:Key, variables    
    //Above, key = Device keys, variables = Device Values
     
                       (or)

    /* if (deviceArray.length>0) */
     multi
    11.hmset on MAC:<AlmondMAC>,deviceArray        //Where deviceAray =Device keys in Payload
    15.LPUSH on AlmondMAC_Device                // params: redisData

    /* if (res > count + 1) */
    16.LTRIM on AlmondMAC_Device                //here count = 9, res = Result from step 15
               
                (or)

    /* if (res == 1) */
    16.expire on AlmondMAC_Device             //here, res = Result from step 15
    17.LPUSH on AlmondMAC_All               // params: redisData

    /* if (res > count + 1) */
    18.LTRIM on AlmondMAC_All                //here count = 19, res = Result from step 17
               
               (or)

    /* if (res == 1) */
    18.expire on AlmondMAC_All             //here, res = Result from above step 17

    POSTGRES -
    (BACKGROUND SERVER)
    19.Insert on recentactivity
       params: mac, id, time, index_id, index_name, name, type, value

    CASSANDRA -
    22.Insert on notification_store.notification_records
        params: usr_id, noti_time, i_time, msg
    23.Update on notification_store.badger  
        params: user_id  
    24.Select on notification_store.badger
        params: usr_id

    QUEUE -
    (ALMOND SERVER)
    4.Send DynamicIndexUpdated to config.ALEXA_QUEUE
    6.Send DynamicIndexUpdated to BACKGROUND_QUEUE
    8.Send Response to All Queues returned in Step 7

    FUNCTIONAL -
    (ALMOND SERVER)
    1.Command 1200
    3.Send DynamicIndexUpdatedResponse to Almond
    5.delete packet.alexa

    (BACKGROUND SERVER)
    9.Command 1200
    20.delete ans.AlmondMAC;
       delete ans.CommandType;
       delete ans.Action;
       delete ans.HashNow;
       delete ans.Devices;
       delete ans.epoch
    21.delete input.users

    FLOW -
    (ALMOND SERVER)
    almondProtocol(packet)-> processor(do)->processor(validate)->almondUsers(execute)->processor(dispatchResponses),processor(sendToBackground)->broadcaster(sendToBackground)->processor(broadcast)->broadcaster(send)    

<a name="1020a"></a>
## 23) AffiliationCompleteResponse (Command 1020)
    Command no
    1020- JSON format

    REQUIRED -
    Command,CommandType,Payload,AlmondMAC

    SQL -
    (ALMOND SERVER)
    3.INSERT on AlmondUsers
      Parameters: userID,AlmondMAC,AlmondName,LongSecret,ownership,FirmwareVersion,AffiliationTS
    
    // If(rows are affected)
    4.INSERT on AlmondProperties2
      Parameters: AlmondMAC,Properties,MobileProperties
    5.Select on Subscriptions
      params: AlmondMAC

    // if ((rsData.CMSCode && sRows[i].CMSCode == rsData.CMSCode) || (!rsData.CMSCode &&
    !sRows[i].CMSCode))
    6.Update on Subscriptions
      params: AlmondMAC, UserID

    //else
    7.Update on Subscriptions
      Params: AlmondMAC,UserID

    //check alexa compatible
    8.Select on UserTempPasswords
      Params: UserID

    REDIS -
    (ALMOND SERVER)
    2.Get CODE:<code>                 // value = null
    9.Perform multi:
     i. hmset on UID_<UserID>       // value = (PMAC_<AlmondMAC>,1)
     ii. hmset on AL_<pMAC>         // value = (userID,key)
     iii. hdel on AL_<pMAC>         // value = [subscriptionToken]

    12.Get ICID_<packet.ICID>        // value = null
    14.hgetall on UID_<user_list>    // Returns all the queues for users in user_list

    QUEUE -
    (ALMOND SERVER)
    13.Send Response to Queue from step 12 
    15.Send Response to All Queues returned in Step 14

    FUNCTIONAL -
    (ALMOND SERVER)
    1.Command 1020
    10.Delete affiliationStore[data.AlmondMAC]
    11.Send AffiliationAlmondCompleteResponse to Almond

    (MOBILE SERVER)
    16.Command 1020
    17.Send Res,commandLengthType to Mobile

    FLOW - (ALMOND SERVER)
    almondProtocol(packet)-> processor(do)->processor(validate)->almondUsers(affiliation_almond_complete),almondUsers(verify_affiliation_complete)-> processor(dispatchResponses),processor(unicast)->broadcaster(unicast)->processor(broadcaster)->broadcaster(send)

<a name="1600"></a>
## 24) ReadyToAddAsSlave,AddWiredSlaveMobile (Command 1600)
    Command no
    1600- JSON format

    REQUIRED -
    Command,CommandType,Payload,AlmondMAC
    
    REDIS -
    (ALMOND SERVER)
    2.hgetall on UID_<user_list>    // Returns all the queues for users in user_list

    QUEUE -
    (ALMOND SERVER)
    3.Send Response to All Queues returned in Step 2

    FUNCTIONAL -
    (ALMOND SERVER)
    1.Command 1600
    
    (MOBILE SERVER)
    4.Command 1600
    5.Send Res,commandLengthType to Mobile

    FLOW - (ALMOND SERVER)
    almondProtocol(packet)-> processor(do)->processor(validate)->almondUsers(dummyModel)->processor(broadcaster)->broadcaster(send)

----------------------------------------------------------------------------------------------
 ## Commands in the same server (ALMOND SERVER)

<a name="1040||31"></a>
## 1) AlmondHello (Command 1040|| Command 31)
     Command no -
     1040||31 - JSON||XML format

     REQUIRED -
     Command,UID,AlmondMAC,Payload,CommandType

     REDIS -
     2.hgetall on AL_<AlmondMAC>        // value = [mapper.hashColumn, payload.HashNow]
     5.Perform multi:
      i. hmset on UID_<UserID>      //value = (PMAC_<data.AlmondMAC>,1)
      ii. hmset on AL_<pMAC>        //key = (M.ALMOND + pMAC), value = (userID,key)
      iii. hdel on AL_<pMAC>        //key = (M.ALMOND + pMAC), value = [subscriptionToken]  

     SQL -    
     3.Select on AlmondUsers
       Parameters: AlmondMAC
     4.Select on AlmondSecondaryUsers
       Parameters: AlmondMAC

     FUNCTIONAL -
     1.Command 1040||31
     6.Send AlmondHelloResponse to Almond

     FLOW -
     almondProtocol(packet)->processor(do)->processor(validate)->almondUsers(almond_hello)->processor(dispatchResponses)

<a name="21"></a>
## 2) AffiliationAlmondRequest (Command 21)
    Command no =
    21- XML format

    REQUIRED -
    Command,CommandType,Payload,ALmondMAC

    SQL - 
    2.Select on AlmondUsers
      params: AlmondMAC, ownership
    3.Select on AllAlmondPlus
      params:AlmondMAC
    4.Insert on AllAlmondPlus
      params: AlmondMAC, AlmondID, FactoryDate, FactoryAdmin

    REDIS -
    5.hgetall on CODE:code                  // where code = random string 
    6.setex on CODE:code  
    // where code = random string,values =AlmondMAC + config.Connections.RabbitMQ.QUEUE -

    FUNCTIONAL - 
    1.Command 21
    7.Send AffiliationAlmondRequestResponse to Almond

    FLOW - 
    almondProtocol(packet)-> processor(do)->processor(validate)->almondUsers(affiliation_almond)->affiliate-almond(affiliate_almond)->processor(dispatchResponses)

<a name="104"></a>
## 3) KeepAlive (Command 104)
    Command no
    104- JSON format

    REQUIRED -
    Command,CommandType,Payload,AlmondMAC

    REDIS - 
    3.hmset on AL_<AlmondMAC>        // values = status,1,server,config.Connections.RabbitMQ.Queue

    // if (socket.zenAlmond)
    CASSANDRA - 
    4.Update on cloudlogs.connection_log
      params: dateyear, mac
     
    FUNCTIONAL - 
    1.Command 104
    2.delete socket.almondOnline

    FLOW - 
    almondProtocol(packet)-> processor(do)->processor(validate)->almondStore(keepAlive)

<a name="1030"></a>
## 4) AlmondReset (Command 1030)
    Command no
    1030- JSON format

    REQUIRED -
    Command,CommandType,Payload,AlmondMAC

    FUNCTIONAL - 
    1.Command 1030
    2.Send AlmondResetResponse to Almond

    FLOW - 
    almondProtocol(packet)-> processor(do)->processor(validate)->almondUsers(almondReset)->processor(dispatchResponses)

<a name="1020b"></a>
## 5) AffiliationAlmondRequest (Command 1020)
    Command no
    1020- JSON format

    REQUIRED -
    Command,CommandType,Payload,AlmondMAC

    SQL -
    2.Select on AlmondUsers
      params: AlmondMAC, ownership
    3.Select on AllAlmondPlus
      params:AlmondMAC
    4.Insert on AllAlmondPlus
      params: AlmondMAC, AlmondID, FactoryDate, FactoryAdmin

    REDIS -
    5.hgetall on CODE:code                  // where code = random string 
    6.setex on CODE:code  
    // where code = random string,values =AlmondMAC + config.Connections.RabbitMQ.Queue

    FUNCTIONAL -
    1.Command 1020
    7.Send AffiliationAlmondRequestResponse to Almond

    FLOW -
    almondProtocol(packet)-> processor(do)->processor(validate)->almondUsers(affiliation_almond)->affiliate-almond(affiliate_almond)->processor(dispatchResponses)

<a name="106"></a>
## 6) AlmondValidationRequest (Command 106)
    Command no
    106- JSON format

    REQUIRED -
    Command,CommandType,Payload,AlmondMAC
    
    SQL -
    2. Select on AllAlmondPlus
       params:  AlmondMAC

    FUNCTIONAL -
    1.Command 106
    3.Send AlmondValidationRequestResponse to Almond

    FLOW -
    almondProtocol(packet)-> processor(do)->processor(validate)->almondUsers(Almond_Validate)-> processor(dispatchResponses)

<a name="1702"></a>
## 7) HashList (Command 1702)
    Command no
    1702- JSON format

    REQUIRED -
    Command,CommandType,Payload,AlmondMAC
    
    REDIS -
    3.hmset on AL_<AlmondMAC>        // values = [router, payload.RouterMode = '%s']

    FUNCIONAL -
    1.Command 1702
    2.delete socket[i]     // i = number of sockets
    4.Send HashListResponse to Almond

    FLOW -
    almondProtocol(packet)-> processor(do)->processor(validate)->almondUsers(checkAllHash)-> processor(dispatchResponses)


----------------------------------------------------------------------------------------------

## MOBILE TO ALMOND  (Sending to Different Server Commands)

<a name="1061"></a>
## 1) Command 1061
    Command no 
    1061- JSON format
 
    REQUIRED - 
    Command,CommandType,Payload,almondMAC

    REDIS - 
    (MOBILE SERVER)
    3.hgetall on AL_<AlmondMAC>      //(AlmondMAC = <packet.parsedPayload.AlmondMAC>)    
    4.get on ICID_<string>           // here <string> = random string data)              
    5.setex on ICID_<string>        // (prefix + key), value = SERVER_NAME     

    (ALMOND SERVER)
    11.Get ICID_<packet.ICID> 

    QUEUE - 
    (MOBILE SERVER)
    7.Send response to config.SERVER_NAME   /*(payload,command,almondMAC) to Queue*/                                            
    
    (ALMOND SERVER)
    12.send Response to Queue    

    FUNCTIONAL - 
    (MOBILE SERVER) 
    1.Command 1061             
    2.Send listResponse,commandLengthType ToMobile  //where listResponse = payload   
    6.delete store[commandID]  
    13.Command 1064                 
    14.Send Res,commandLengthType to Mobile   

    (ALMOND SERVER)
    8.Command 1062               
    9.Send Res,CommandLengthType to Almond
    10.Command 1063            

    FLOW - (MOBILE SERVER)
    socket(packet)->validator(do)->processor(do)->almond(onlyUnicast)->dispatcher(dispatchResponse)->dispatcher(unicast)->broadcaster(unicast)

    FLOW - (ALMOND SERVER)
    almondProtocol(packet)-> processor(do)->processor(validate)->almondUsers(dummyModel)->processor(dispatchResponses),processor(unicast)->broadcaster(unicast)

<a name="1100b"></a>
## 2) Command 1100    
    Command no 
    1100- JSON format
 
    REQUIRED - 
    Command,CommandType,Payload,almondMAC

    REDIS 
    (MOBILE SERVER)
    3.hgetall on AL_<AlmondMAC>          //(AlmondMAC = <packet.parsedPayload.AlmondMAC>)   
    4.get on ICID_<string>              // here <string> = random string data)      
    5.setex on ICID_<string>            // (prefix + key), value = SERVER_NAME    

    QUEUE -
    (MOBILE SERVER)
    7.Send response to config.SERVER_NAME        // (payload,command,almondMAC) to Queue            

    FUNCTIONAL -
    (MOBILE SERVER)
    1.Command 1100           
    2.Send listResponse,commandLengthType ToMobile      //where listResponse = payload    
    6.delete store[commandID] 

    (ALMOND SERVER)              
    8.Command 1100            
    9.Send Res,CommandLengthType to Almond             

    FLOW -(MOBILE SERVER)
    socket(packet)->validator(do)->processor(do)->almond(onlyUnicast)->dispatcher(dispatchResponse)->dispatcher(unicast)->broadcaster(unicast)

<a name="23"></a>
## 3) AffiliationUserRequest (Command 23),AffiliationAlmondComplete (Command 25)
    Command no 
    23- XML format
 
    REQUIRED - 
    Command,CommandType,Payload

    REDIS -
    (MOBILE SERVER)
    2.get on CODE:<data.code>  
    4.get on ICID_<string>              // here <string> = random string data)
    5.setex on ICID_<string>            // (prefix + key), value = SERVER_NAME

    (ALMOND SERVER)
    12.Get CODE:<code>                 // value = null
    19.Perform multi:
     i. hmset on UID_<UserID>       // value = (PMAC_<AlmondMAC>,1)
     ii. hmset on AL_<pMAC>         // value = (userID,key)
     iii. hdel on AL_<pMAC>         // value = [subscriptionToken]
    22.Get ICID_<packet.ICID>        // value = null
    24.hgetall on UID_<user_list>    // Returns all the queues for users in user_list
        
    SQL -
    (MOBILE SERVER)
    3.Select on Users
      params: UserID

    (ALMOND SERVER)
    13.INSERT on AlmondUsers
      Params: userID,AlmondMAC,AlmondName,LongSecret,ownership,FirmwareVersion,AffiliationTS
    
    // If(rows are affected)
    14.INSERT on AlmondProperties2
      Params: AlmondMAC,Properties,MobileProperties
    15.Select on Subscriptions
      params: AlmondMAC

    // if ((rsData.CMSCode && sRows[i].CMSCode == rsData.CMSCode) || (!rsData.CMSCode &&
    !sRows[i].CMSCode))
    16.Update on Subscriptions
      params: AlmondMAC, UserID

    //else
    17.Update on Subscriptions
      Params: AlmondMAC,UserID

    //check alexa compatible
    18.Select on UserTempPasswords
      Params: UserID
     
    QUEUE -
    (MOBILE SERVER)
    8.Send AffiliationUserRequestResponse to rows.server

    (ALMOND SERVER)
    23.Send Response to queue returned in Step 22
    25.Send Response to All Queues returned in Step 24
 
    FUNCTIONAL -
    (MOBILE SERVER)
    1.Command 23
    6.Send listResponse,commandLengthType ToMobile       //where listResponse = payload
    7.delete store[row.CommandID]

    (ALMOND SERVER)
    9.Command 26
    10.Send Res,CommandLengthType to Almond
    11.Command 25
    20.Delete affiliationStore[data.AlmondMAC]
    21.Send AffiliationAlmondCompleteResponse to Almond

    FLOW - (MOBILE SERVER)
    socket(packet)->validator(do)->processor(do)->affiliation(execute)->redisManager(getCode)->sqlManager(getEmail)->cid-bid(incCommandID)->newRowBuilder(affiliationError)->dispatcher(dispatchResponse)->dispatcher(unicast)->broadcaster(unicast)

    FLOW - (ALMOND SERVER)
    almondProtocol(packet)-> processor(do)->processor(validate)->almondUsers(affiliation_almond_complete),almondUsers(verify_affiliation_complete)-> processor(dispatchResponses),processor(unicast)->broadcaster(unicast)->processor(broadcaster)->broadcaster(send)

<a name="61"></a>
## 4) AlmondModeChange (Command 61)
    Command no 
    61- XML format
 
    REQUIRED - 
    Command,CommandType,Payload,almondMAC

    REDIS -
    (MOBILE SERVER)
    3.hgetall on AL_<AlmondMAC>          //(AlmondMAC = <packet.parsedPayload.AlmondMAC>)
    4.get on ICID_<string>              // here <string> = random string data)
    5.setex on ICID_<string>            // (prefix + key), value = SERVER_NAME
  
    QUEUE -
    (MOBILE SERVER)
    7.Send response to config.SERVER_NAME       // (payload,command,almondMAC) to QUEUE -

    FUNCTIONAL - 
    (MOBILE SERVER)
    1.Command 61
    2.Send listResponse,commandLengthType ToMobile         //where listResponse = payload
    6.delete store[commandID]

    (ALMOND SERVER)
    8.Command 62
    9.Send Res,CommandLengthType to Almond

    FLOW -
    (MOBILE SERVER)
    socket(packet)->validator(do)->processor(do)->almond(onlyUnicast)->dispatcher(dispatchResponse)->dispatcher(unicast)->broadcaster(unicast)

<a name="1023"></a>
## 5) AffiliationUserRequest (Command 1023)
    Command no 
    1023- JSON format
 
    REQUIRED - 
    Command,CommandType,Payload,AlmondMAC,UserID

    REDIS -
    (MOBILE SERVER)
    2.get on CODE:<data.code>  
    4.get on ICID_<string>              // here <string> = random string data)
    5.setex on ICID_<string>            // (prefix + key), value = SERVER_NAME
    7.setex on CODE:<data.code>     
    //values =res.mac,SERVER_NAME, socket.userid,commandID,res.emailID
        
    SQL - 
    (MOBILE SERVER)
    3.Select on Users
      params: UserID
    6.Select on SCSIDB.CMSAffiliations
      params:AlmondMAC
 
    QUEUE -
    (MOBILE SERVER)
    10. Send AffiliationAlmondCompleteResponse to rows.server  
   
    FUNCTIONAL -
    (MOBILE SERVER)
    1.Command 1023
    8.Send listResponse,commandLengthType ToMobile       //where listResponse = payload
    9.delete store[res.commandID]

    (ALMOND SERVER)
    11.Command 1026
    12.Send Res,CommandLengthType to Almond

    FLOW - (MOBILE SERVER)
    socket(packet)->validator(do)->processor(do)->affiliation(execute)->redisManager(getCode)->sqlManager(getEmail)->cid-bid(incCommandID)->newRowBuilder(affiliationError)->dispatcher(dispatchResponse)->dispatcher(unicast)->broadcaster(unicast)

<a name="1700a"></a>
## 6) UpdateDevicePreference (Command 1700) 
    Command no
    1700- JSON format 

    REQUIRED -
    Command,CommandType,Payload

    REDIS -
    (MOBILE SERVER)
    multi
    5.hgetall on UID_<userID>             /here, multi is done on every userID in UserList

    SQL -
    (MOBILE SERVER)
    2.Delete on NotificationPreferences
      params: AlmondMAC,UserID

    QUEUE -
    (MOBILE SERVER)
    6.Send DynamicDevicePreferencesResponse to MobileQueue   

    FUNCTIONAL -
    (MOBILE SERVER)
    1.Command 1700
    3.Send listResponse,commandLengthType ToMobile               //where listResponse = payload
    4.Send DynamicDevicePreferencesResponse ToMobile

    (MOBILE SERVER)
    7.Command 1700
    8.Send Res,CommandLengthType to Mobile

    FLOW - (MOBILE SERVER)
    socket(packet)->validator(do)->processor(do)->notiPrefs(do)->genericModel(delete)->oldRowBuilder(newPref)->dispatcher(dispatchResponse)->dispatcher(broadcast)->broadcastBuilder(preferences)

<a name="1700b"></a>
## 7) UpdateClientPreferences (Command 1700) 
    Command no
    1700- JSON format 

    REQUIRED -
    Command,CommandType,Payload

    REDIS -
    (MOBILE SERVER)
    multi
    5.hgetall on UID_<userID>          //here, multi is done on every userID in UserList

    SQL -
    (MOBILE SERVER)
    2.Delete on ClientPreferences
      params: AlmondMAC,UserID
    
    QUEUE -
    (MOBILE SERVER) 
    6.Send DynamicClientPreferencesResponse to MobileQueue            

    FUNCTIONAL -
    (MOBILE SERVER)
    1.Command 1700
    3.Send listResponse,commandLengthType ToMobile             //where listResponse = payload
    4.Send DynamicClientPreferencesResponse ToMobile

    (MOBILE SERVER)
    7.Command 1700
    8.Send Res,CommandLengthType to Mobile

    FLOW - (MOBILE SERVER)
    socket(packet)->validator(do)->processor(do)->notiPrefs(do)->genericModel(delete)->oldRowBuilder(newPref)->dispatcher(dispatchResponse)->dispatcher(broadcast)->broadcastBuilder(preferences)

<a name="1525"></a>
## 8) UpdateClientPreferences (Command 1525)
    Command no 
    1525- JSON format
 
    REQUIRED - 
    Command,CommandType,Payload,UserID

    REDIS -
    (MOBILE SERVER)
    multi
    5.hgetall on UID_<userID>          //here, multi is done on every userID in UserList

    SQL -
    (MOBILE SERVER)
    2.Insert on WifiClientsNotificationPreferences
      params:AlmondMAC,ClientID,UserID,NotificationType

    QUEUE -
    (MOBILE SERVER)
    6.Send UpdateClientPreferencesResponse to MobileQueue
    
    FUNCTIONAL -
    (MOBILE SERVER)
    1.Command 1525
    3.Send listResponse,commandLengthType ToMobile       //where listResponse = payload
    4.Send Response ToMobile                            //where Response = command,payload

    (MOBILE SERVER)
    7.Command 1525
    8.Send Res,CommandLengthType to Mobile

    FLOW - (MOBILE SERVER)
    socket(packet)->validator(do)->processor(do)->notificationPreferences(change_wificlient_notification_preferences)->oldRowBuilder(wifiNotificationPreferences)->dispatcher(dispatchResponse)->broadcastBuilder(defaultXML)->broadcaster(broadcast)

<a name="300"></a>
## 9) NotificationPreferences (Command 300)
    Command no 
    300- XML format
 
    REQUIRED - 
    Command,CommandType,Payload

    REDIS -
    (MOBILE SERVER)
    multi
    4.hgetall on UID_<userID>     //here, multi is done on every userID in UserList

    QUEUE -
    (MOBILE SERVER)
    5.Send UserProfileResponse to MobileQueue            

    SQL -
    (MOBILE SERVER)
    2.Insert on NotificationPreferences
      params: AlmondMAC, DeviceID, UserID,IndexVal, NotificationType

    FUNCTIONAL -
    (MOBILE SERVER)
    1.Command 300
    3.Send listResponse,commandLengthType ToMobile       //where listResponse = payload

    (MOBILE SERVER)
    6.Command 300
    7.Send Res,CommandLengthType to Mobile

    FLOW - (MOBILE SERVER)
    socket(packet)->validator(do)->processor(do)->notificationPreferences(update_notification_preferences)->oldRowBuilder(notificationPreferences)->dispacher(dispatchResponse)->dispatcher(broadcast)->broadcastBuilder(defaultXML)->broadcaster(broadcast)

<a name="4"></a>
## 10) Logoutall (Command 4)
    Command no 
    4- XML format
 
    REQUIRED - 
    Command,CommandType,Payload,UserID
  
    REDIS -
    (MOBILE SERVER)
    7.hmset on UID_<userid>        // where values = [Q_config.SERVER_NAME,0]
  
    multi
    8.hgetall on UID_<userID>          //here, multi is done on every userID in UserList

    SQL -
    (MOBILE SERVER)
    2.Select on Users
      params: UserID
    3.Delete on UserTempPasswords
      params: UserID
    4.Delete on NotificationID
     params: UserID

    QUEUE -
    (MOBILE SERVER)
    9.Send UserProfileResponse to MobileQueue            

    FUNCTIONAL -
    (MOBILE SERVER)
    1.Command 4
    5.Send listResponse,commandLengthType ToMobile       //where listResponse = payload
    6.delete socketStore[userid]

    (MOBILE SERVER)
    10.Command 4
    11.Send Res,CommandLengthType to Mobile

    FLOW - (MOBILE SERVER)
    socket(packet)->validator(do)->validator(checkCredentials)->sqlManager(getUser)->processor(do)->login(logoutAll)->connection-pool(queryFunction)->oldRowBuilder(logoutAll)->dispacher(dispatchResponse)->mongo-store(removeAll)->dispatcher(broadcast)->broadCastBuilder(removeAll)->broadcaster(broadcast)

<a name="2222"></a>
## 11) Restore (Command 2222)
    Command no 
    2222- JSON format
 
    REQUIRED - 
    Command,CommandType,Payload,AlmondMAC

    CASSANDRA -
    (MOBILE SERVER)
    2.Select on almondhistory
    params: mac,type

    REDIS -
    (MOBILE SERVER)
    4.hgetall on AL_<AlmondMAC>
    5.get on ICID_<code>                //where code = random string
    6.setex on ICID_<code>            //where code = random string

    QUEUE -
    (MOBILE SERVER)
    8.Send RestoreResponse to  config.SERVER_NAME

    FUNCTIONAL -
    (MOBILE SERVER)
    1.Command 2222
    3.Send listResponse,commandLengthType ToMobile       //where listResponse = payload
    7.delete store[commandID] 

    (ALMOND SERVER)
    9.Command 1700
    10.Send Res,CommandLengthType to Almond

    FLOW - (MOBILE SERVER)
    socket(packet)->validator(do)->processor(do)->notificationFetcher(almondHistroy)->newRowBuilder(almondHistroy)->dispatcher(dispatchResponse)->dispatcher(unicast)->newRowBuilder(restoreData)->broadcaster(unicast)

<a name="1060a"></a>
## 12) ChangeUser (Command 1060,Action:Add) 
    Command no 
    1060- JSON format
 
    REQUIRED - 
    Command,CommandType,Payload,AlmondMAC,UserID

    REDIS -
    (MOBILE SERVER)
    3.hgetall on AL_<payload.AlmondMAC>

    multi
    5.hgetall on UID_<userID>          //here, multi is done on every userID in UserList

    QUEUE -
    (MOBILE SERVER)
    6.Send UserProfileResponse to MobileQueue            

    SQL -
    (MOBILE SERVER)
    2.update on AlmondplusDB.WifiClients
    params:UserName,AlmondMAC,ClientID

    FUNCTIONAL -
    (MOBILE SERVER)
    1.Command 1060
    4.Send listResponse,commandLengthType ToMobile       //where listResponse = payload

    (MOBILE SERVER)
    7.Command 1060
    8.Send Res,CommandLengthType to Mobile

    FLOW - (MOBILE SERVER)
    socket(packet)->validator(do)->processor(do)->clientModel(update)->newRowBuilder(change_user)->dispatcher(broadcast)->broadcastBuilder(change_user)->broadcaster(broadcast)

<a name="1060b"></a>
## 13) ChangeUser (Command 1060,Action:Update) 
    Command no 
    1060- JSON format
 
    REQUIRED - 
    Command,CommandType,Payload,AlmondMAC,UserID

    REDIS -
    (MOBILE SERVER)
    4.hgetall on AL_<payload.AlmondMAC>

    multi
    6.hgetall on UID_<userID>         //here, multi is done on every userID in UserList

    QUEUE - 
    (MOBILE SERVER)
    7.Send UserProfileResponse to MobileQueue           

    SQL -
    (MOBILE SERVER)
    2.Update on AlmondplusDB.WifiClients 
      params: UserName,AlmondMAC, UserName
    3.Update on AlmondplusDB.WifiClients 
      params: UserName,AlmondMAC,ClientID

    FUNCTIONAL - 
    (MOBILE SERVER)
    1.Command 1060
    5.Send listResponse,commandLengthType ToMobile       //where listResponse = payload

    (MOBILE SERVER)
    8.Command 1060
    9.Send Res,CommandLengthType to Mobile

    FLOW - (MOBILE SERVER)
    socket(packet)->validator(do)->processor(do)->clientModel(update)->newRowBuilder(change_user)->dispatcher(broadcast)->broadcastBuilder(change_user)->broadcaster(broadcast)

<a name="1110a"></a>
## 14) UnlinkAlmondRequest (Command 1110) 
    Command no 
    1110- JSON format
 
    REQUIRED - 
    Command,CommandType,Payload,AlmondMAC
     
    REDIS -
    (MOBILE SERVER)
    4.hgetall on AL_<data.AlmondMAC>

    SQL -
    (MOBILE SERVER)
    2.Select on Users
      params:EmailID

    QUEUE -
    (MOBILE SERVER)
    5.Send UnlinkAlmondRequestResponse to AlmondServer

    FUNCTIONAL -
    (MOBILE SERVER)
    1.Command 1110
    3.Send listResponse,commandLengthType ToMobile       //where listResponse = payload

    (ALMOND SERVER)
    6.Command 1110
    7.Send Res,CommandLengthType to Almond
 
    FLOW - (MOBILE SERVER)
    socket(packet)->validator(do)->validator(unlink),validator(checkCredentials)->sqlManager(getUser)->processor(do)->account-manager-json(UnlinkAlmond)->rowBuilder(defaultReply)->dispatcher(unicast)->dispatcher(dispatchResponse)->account-manager-json(getAlmond)->broadcastBuilder(unlink)->broadcaster(unicast)

<a name="1110b"></a>
## 15) UserInviteRequest (Command 1110) 
    Command no 
    1110- JSON format
 
    REQUIRED - 
    Command,CommandType,Payload,AlmondMAC

    SQL -
    (MOBILE SERVER)
    2.Select on Users
      params: UserID
    4.Select on AlmondSecondaryUsers
      params: AlmondMAC,UserID
    5.Insert on AlmondSecondaryUsers
      params:AlmondMAC,userID
    8.Select on Users
      params: UserID

    REDIS -
    (MOBILE SERVER)
    3.hgetall on UID_<userid>
    6.hmset on AL_<JsonObj.AlmondMAC>      //where value = [SUSER_<UserID>,1]
    7.hmset on UID_<userid>               //where value = [SMAC_<JsonObj.AlmondMAC>,1]
    9.hgetall on AL_<AlmondMAC>
    11.hgetall on AL_<AlmondMAC>
    12.get on ICID_<code>                //where code = random string
    13.setex on ICID_<code>            //where code = random string

    multi
    17.hgetall on UID_<data.SecondaryUsers>     //here, multi is done on every SecondaryUsers 

    QUEUE -
    (MOBILE SERVER)
    15.Send UserInviteRequestResponse to config.SERVER_NAME
    18.Send UserInviteRequestResponse to MobileQueue
   
    FUNCTIONAL -
    (MOBILE SERVER)
    1.Command 1110
    10.Send listResponse,commandLengthType ToMobile       //where listResponse = payload
    14.delete store[commandID]
    16.Send listResponse,commandLengthType ToMobile       //where listResponse = payload

    (MOBILE SERVER)
    19.Command 1110
    20.Send Res,CommandLengthType to Mobile

    FLOW - (MOBILE SERVER)
    socket(packet)->validator(do)->processor(do)->account-manager-json(UserInvite)->sqlManager(checkSecondary),sqlManager(addSecondaryAlmond)->redisManager(addSecondaryAlmond)->sqlManager(getEmail)->redisManager(getAlmond)->rowBuilder(defaultReply)->dispatcher(dispatchResponse)->dispatcher(unicast)->rowBuilder(userChange)->rowBuilder(userChange)->broadcaster(unicast)->dispatcher(broadcast)->broadcastBuilder(almondAdd),broadcastBuilder(userAdd)->broadcaster(broadcast)

<a name="1110c"></a>
## 16) UpdateUserProfileRequest (Command 1110) 
    Command no 
    1110- JSON format
 
    REQUIRED - 
    Command,CommandType,Payload,UserID

    REDIS - 
    (MOBILE SERVER)
    multi
    4.hgetall on UID_<userID>         //here, multi is done on every userID in UserList
    
    SQL - 
    (MOBILE SERVER) 
    2.Update on Users
      params: UserID

    QUEUE - 
    (MOBILE SERVER)
    5.Send UserProfileResponse to MobileQueue 
   
    FUNCTIONAL - 
    (MOBILE SERVER)
    1.Command 1110
    3.Send listResponse,commandLengthType ToMobile       //where listResponse = payload
    
    (MOBILE SERVER)
    6.Command 1110
    7.Send Res,CommandLengthType to Mobile

    FLOW -  (MOBILE SERVER)
    socket(packet)->validator(do)->processor(do)->account-manager-json(UpdateUserProfile)->rowBuilder(UpdateUserProfile)->dispatcher(dispatchResponse)-> dispatcher(broadcast)->broadcastBuilder(userProfileUpdate)->broadcaster(broadcast)

<a name="1110d"></a>
## 17) DeleteSecondaryUserRequest (Command 1110) 
    Command no 
    1110- JSON format
 
    REQUIRED - 
    Command,CommandType,Payload,UserID,AlmondMAC

    SQL - 
    (MOBILE SERVER)
    2.Delete on AlmondSecondaryUsers
      params: AlmondMAC, userID
    3.Delete on NotificationPreferences
      params:  AlmondMAC, userID

    REDIS - 
    (MOBILE SERVER)
    4.hdel on  AL_<AlmondMAC>             // value = SUSER_<secondaryUser>
    5.hdel on UID_<secondaryUser>         //value = SMAC_<AlmondMAC>
    7.hgetall on AL_<AlmondMAC>
    8.get on ICID_<code>                //where code = random string
    9.setex on ICID_<code>            //where code = random string    

    multi
    13.hgetall on UID_<userList>   
      /* here, multi is done on userList where userList = data.userListAlmondDelete,data.userListUserDelete */

    QUEUE - 
    (MOBILE SERVER)
    11.Send DynamicUserChangeResponse to config.SERVER_NAME
    14.Send DynamicAlmondDeleteResponse,DynamicUserDeleteResponse to MobileQueue

    FUNCTIONAL - 
    (MOBILE SERVER)
    1.Command 1110
    6.Send listResponse,commandLengthType ToMobile       //where listResponse = payload
    10.delete store[commandID]
    12.Send listResponse,commandLengthType ToMobile       //where listResponse = payload

    (MOBILE SERVER)
    15.Command 1110
    16.Send Res,CommandLengthType to Mobile

    FLOW - (MOBILE SERVER)
    socket(packet)->validator(do)->processor(do)->account-manager-json(DeleteSecondaryUser)->sqlManager(deleteUser)->redisManager(redisExecute),redisManager(deleteAccount)->rowBuilder(defaultReply)->dispatcher(dispatchResponse)->dispatcher(unicast)-> rowBuilder(userChange)->broadcaster(unicast)->dispatcher(broadcast)->broadcastBuilder(almondDelete),broadcastBuilder(userDelete)->broadcaster(broadcast)

<a name="1110e"></a>
## 18) DeleteMeAsSecondaryUserRequest (Command 1110) 
    Command no 
    1110- JSON format
 
    REQUIRED - 
    Command,CommandType,Payload,UserID,AlmondMAC

    SQL -
    (MOBILE SERVER)    
    3.Delete on AlmondSecondaryUsers
      params: AlmondMAC, userID
    4.Delete on NotificationPreferences
      params:  AlmondMAC, userID

    REDIS -
    (MOBILE SERVER)
    2.hgetall on AL_<AlmondMAC>
    5.hdel on  AL_<AlmondMAC>             // value = SUSER_<secondaryUser>
    6.hdel on UID_<secondaryUser>         //value = SMAC_<AlmondMAC>
    8.hgetall on AL_<AlmondMAC>
    9.get on ICID_<code>                //where code = random string
    10.setex on ICID_<code>            //where code = random string

    multi
    14.hgetall on UID_<userList>   
      /* here, multi is done on userList where userList = data.userListAlmondDelete,data.userListUserDelete */

    QUEUE - 
    (MOBILE SERVER)
    12.Send DynamicUserChangeResponse to config.SERVER_NAME
    15.Send DynamicAlmondDeleteResponse,DynamicUserDeleteResponse to MobileQueue
    
    FUNCTIONAL - 
    (MOBILE SERVER)
    1.Command 1110
    7.Send listResponse,commandLengthType ToMobile       //where listResponse = payload
    11.delete store[commandID] 
    13.Send listResponse,commandLengthType ToMobile       //where listResponse = payload

    (MOBILE SERVER)
    16.Command 1110
    17.Send Res,CommandLengthType to Mobile

    FLOW - (MOBILE SERVER)
    socket(packet)->validator(do)->processor(do)->account-manager-json(DeleteMeAsSecondaryUserResponse)->redisManager(getAlmond)->sqlManager(deleteUser)->redisManager(redisExecute),redisManager(deleteAccount)->rowBuilder(defaultReply)->dispatcher(dispatchResponse)->dispatcher(unicast)-> rowBuilder(userChange)->broadcaster(unicast)->dispatcher(broadcast)->broadcastBuilder(almondDelete),broadcastBuilder(userDelete)->broadcaster(broadcast)

<a name="1110f"></a>
## 19) ChangePasswordRequest (Command 1110) 
    Command no 
    1110- JSON format
 
    REQUIRED - 
    Command,CommandType,Payload,UserID,AlmondMAC

    SQL -
    (MOBILE SERVER)
    2.Select on Users
     params:EmailID
    3.Update on Users
     params:Password,EmailID     
    4.Delete on UserTempPasswords         //if (rows.affectedRows == 1)
     params:UserID
    5.Delete on NotificationID
     params:UserID

            (or)

     4.return            //if (rows.affectedRows == 0)
    /* note: considered that rows are affected = 1 */

    REDIS -
    (MOBILE SERVER)
    7.hmset on UID_<socket.userid>        //values = Q_<config.SERVER_NAME,userSession.length-1>
    
    multi
    9.hgetall on UID_<userID>          //here multi is done on every userID in userList

    QUEUE -
    (MOBILE SERVER)
    10.Send ChangePasswordRequestResponse to MobileQueue

    FUNCTIONAL -
    (MOBILE SERVER)
    1.Command 1110
    6.Send listResponse,commandLengthType ToMobile       //where listResponse = payload
    8.delete socketStore[userid]

    (MOBILE SERVER)
    11.Command 1110
    12.Send Res,CommandLengthType to Mobile

    FLOW - (MOBILE SERVER)
    socket(packet)->validator(do)->validator(checkCredentials)->processor(do)->account-manager-json(ChangePassword)->connection-pool(queryFunction)->rowBuilder(defaultReply)->dispatcher(dispatchResponse)->mongo-store(removeAllExceptCurrent)->dispatcher(broadcast)->broadcastBuilder(removeAll)->broadcaster(broadcast)

<a name="1110g"></a>
## 20) DeleteAccountRequest (Command 1110) 
    Command no 
    1110- JSON format
 
    REQUIRED - 
    Command,CommandType,Payload,UserID,AlmondMAC

    SQL -
    (MOBILE SERVER) 
    2.Select on Users
      params:EmailID
    3.Delete on Users
      params: UserID
    9.Delete on AlmondUsers
      params: AlmondMAC
    10.Update on AllAlmondPlus
      params: AlmondMAC 

    REDIS -
    (MOBILE SERVER)
    4.hgetall on UID_<packet.userid>
    5.del on UID_<packet.userid>

    multi
    6.hgetall on AL_<pMACs>

    multi
    7.del on AL_<pMACs>

    multi
    8.hdel on UID_<Entry[1]>           //values = SMAC_<AlmondMAC>, Entry[1] =Secondary UserID 

    13.hmset on UID_<data.userid>   //values = (Q_<config.SERVER_NAME>,0)

    multi
    14.hgetall on UID_<userID>          //here multi is done on every userID in userList

    QUEUE -
    (MOBILE SERVER)
    15.Send DeleteAccountRequestResponse to MobileQueue
    16.Send DeleteAccountResponse to config.HTTP_SERVER_NAME

    FUNCTIONAL -
    (MOBILE SERVER)
    1.Command 1110
    11.Send listResponse,commandLengthType ToMobile       //where listResponse = payload
    12.delete socketStore[data.userid]

    (MOBILE SERVER)
    17.Command 1110
    18.Send Res,CommandLengthType to Mobile
 
    FLOW - (MOBILE SERVER)
    socket(packet)->validator(do)->validator(checkCredentials)->processor(do)->account-manager-json(DeleteAccount)->sqlManager(deleteUser)->redisManager(redisExecute),redisManager(deleteAccount)->rowBuilder(defaultReply)->dispatcher(dispatchResponse)->mongo-store(removeAll)->dispatcher(broadcast)->broadcastBuilder(removeAll)->broadcaster(broadcast)->dispatcher(broadcastToAllAlmonds)->broadcaster(broadcastModel)

<a name="1011a"></a>
## 21) SubscribeMe (Command 1011) 
    Command no
    1011- JSON format 

    REQUIRED -
    Command,CommandType,Payload

    REDIS - 
    (MOBILE SERVER)
    2.hgetall on UID_<packet.userid>      // values = PMAC_<AlmondMAC>
     
    QUEUE -
    (MOBILE SERVER)
    4.Send SubscribeMeResponse to config.HTTP_SERVER_NAME
    
    FUNCTIONAL -
    (MOBILE SERVER)
    1.Command 1011
    3.delete store[data.UnicastID]                           
    5.Send listResponse,commandLengthType ToMobile           //where listResponse = payload
  
    FLOW - (MOBILE SERVER)
    socket(packet)->validator(do)->processor(do)->subscriptionCommands(subscriptionCommands)-
    >redisManager(getAlmonds)->mongo-store(getSocket)->requestQueue(set)->producer(sendToQueue)->dispatcher(dispatchResponse)

<a name="1011b"></a>
## 22) PaymentDetails (Command 1011) 
    Command no 
    1011- JSON format
 
    REQUIRED - 
    Command,CommandType,Payload,UserID,AlmondMAC
    
    REDIS -
    (MOBILE SERVER)
    2.hgetall on UID_<packet.userid>      // values = PMAC_<AlmondMAC>

    QUEUE - 
    4.Send PaymentDetailsResponse to config.HTTP_SERVER_NAME

    FUNCTIONAL - 
    (MOBILE SERVER)
    1.Command 1011
    3.delete store[data.UnicastID]
    5.Send listResponse,commandLengthType ToMobile           //where listResponse = payload

    FLOW - (MOBILE SERVER)
    socket(packet)->validator(do)->processor(do)->subscriptionCommands(subscriptionCommands)-
    >redisManager(getAlmonds)->mongo-store(getSocket)->requestQueue(set)->producer(sendToQueue)->dispatcher(dispatchResponse)

<a name="1011c"></a>
## 23) UpdateCard (Command 1011) 
    Command no 
    1011- JSON format
 
    REQUIRED - 
    Command,CommandType,Payload,UserID,AlmondMAC

    REDIS -
    (MOBILE SERVER)
    2.hgetall on UID_<packet.userid>      // values = PMAC_<AlmondMAC>

    QUEUE - 
    (MOBILE SERVER)
    4.Send UpdateCardResponse to config.HTTP_SERVER_NAME

    FUNCTIONAL - 
    (MOBILE SERVER)
    1.Command 1011
    3.delete store[data.UnicastID]
    5.Send listResponse,commandLengthType ToMobile           //where listResponse = payload

    FLOW -  (MOBILE SERVER)
    socket(packet)->validator(do)->processor(do)->subscriptionCommands(subscriptionCommands)-
    >redisManager(getAlmonds)->mongo-store(getSocket)->requestQueue(set)->producer(sendToQueue)->dispatcher(dispatchResponse)
    
<a name="1011d"></a>
## 24) DeleteSubscription (Command 1011) 
    Command no 
    1011- JSON format
 
    REQUIRED - 
    Command,CommandType,Payload,UserID,AlmondMAC

    REDIS - 
    (MOBILE SERVER)
    2.hgetall on UID_<packet.userid>      // values = PMAC_<AlmondMAC>

    QUEUE - 
    (MOBILE SERVER)
    4.Send DeleteSubscriptionResponse to config.HTTP_SERVER_NAME

    FUNCTIONAL - 
    (MOBILE SERVER)
    1.Command 1011
    3.delete store[data.UnicastID]
    5.Send listResponse,commandLengthType ToMobile           //where listResponse = payload

    FLOW - (MOBILE SERVER)
    socket(packet)->validator(do)->processor(do)->subscriptionCommands(subscriptionCommands)-
    >redisManager(getAlmonds)->mongo-store(getSocket)->requestQueue(set)->producer(sendToQueue)->dispatcher(dispatchResponse)

----------------------------------------------------------------------------------------------
 ## Commands in the same server (MOBILE SERVER)

<a name="1200ms"></a>
## 1) DeviceList (Command 1200)
    Command no 
    1200- JSON format
 
    REQUIRED - 
    Command,CommandType,Payload

    SQL - 
    2.Select on DEVICE_DATA 
      params: AlmondMAC

    REDIS - 
    multi
    3.hgetall on (MAC:<AlmondMAC>,DeviceValues) 
    //Here, multi is done on all AlmondMAC and the IDs present in DeviceValues  

    FUNCTIONAL - 
    1.Command 1200
    4.Send listResponse,commandLengthType ToMobile          //where listResponse = payload
    
    FLOW - 
    socket(packet)->validator(do)->processor(do)->device(execute)->redisDeviceValue(get)->genericModel(get)->connection-pool(queryFunction)->newRowBuilder(devices)->dispatcher(dispatchResponse)

<a name="1300ms"></a>
## 2) SceneList (Command 1300)
    Command no 
    1300- JSON format
 
    REQUIRED - 
    Command,CommandType,Payload,AlmondMAC

    SQL - 
    2.Select on SCENES 
      params: AlmondMAC

    FUNCTIONAL -  
    1.Command 1300
    3.Send listResponse,commandLengthType ToMobile          //where listResponse = payload
    
    FLOW - 
    socket(packet)->validator(do)->processor(do)->genericModel(execute),genericModel(get)->newRowBuilder(scenes)->dispatcher(dispatchResponse)

<a name="1400ms"></a>
## 3) RuleList (Command 1400)
    Command no 
    1400- JSON format
 
    REQUIRED - 
    Command,CommandType,Payload

    SQL - 
    2.Select on RULE 
      params: AlmondMAC

    FUNCTIONAL -  
    1.Command 1400
    3.Send listResponse,commandLengthType ToMobile         //where listResponse = payload
    
    FLOW - 
    socket(packet)->validator(do)->processor(do)->genericModel(execute),genericModel(get)->newRowBuilder(Rules)->dispatcher(dispatchResponse)

<a name="1500ms"></a>
## 4) ClientList (Command 1500)
    Command no 
    1500- JSON format
 
    REQUIRED - 
    Command,CommandType,Payload

    SQL - 
    2.Select on WIFICLIENTS 
      params: AlmondMAC

    FUNCTIONAL - 
    1.Command 1500
    3.Send listResponse,commandLengthType ToMobile         //where listResponse = payload
    
    FLOW - 
    socket(packet)->validator(do)->processor(do)->genericModel(execute),genericModel(get)->newRowBuilder(clients)->dispatcher(dispatchResponse)

<a name="1050ms"></a>
## 5) AlmondProperties (Command 1050) 
    Command no
    1050- JSON format 

    REQUIRED -
    Command,CommandType,Payload,almondMAC

    SQL -
    2.Select on AlmondProperties2
     params: AlmondMAC

    FUNCTIONAL - 
    1.Command 1050
    3.Send listResponse,commandLengthType ToMobile           //where listResponse = payload

    FLOW -
    socket(packet)->validator(do)->processor(do)->almond(AlmondProperties)->newRowBuilder(almondProperties)->dispatcher(dispatchResponse)

<a name="1700c"></a>
## 6) GetDevicePreferences (Command 1700) 
    Command no
    1700- JSON format 

    REQUIRED -
    Command,CommandType,Payload

    SQL -
    2.Select on NotificationPreferences
      params: AlmondMAC,UserID

    FUNCTIONAL - 
    1.Command 1700
    3.Send listResponse,commandLengthType ToMobile            //where listResponse = payload

    FLOW -
    socket(packet)->validator(do)->processor(do)->newPref(do)->genericModel(select)->oldRowBuilder(newPref)->dispatcher(dispatchResponse)->dispatcher(broadcast)->broadcastBuilder(preferences)

<a name="1700d"></a>
## 7) GetClientPreferences (Command 1700) 
    Command no
    1700- JSON format 

    REQUIRED -
    Command,CommandType,Payload

    SQL -
    2.Select on ClientPreferences
      params: AlmondMAC,UserID

    FUNCTIONAL -
    1.Command 1700 
    3.Send listResponse,commandLengthType ToMobile      //where listResponse = payload
 
    FLOW -
    socket(packet)->validator(do)->processor(do)->notiPrefs(do)->genericModel(select)->oldRowBuilder(newPref)->dispatcher(dispatchResponse)->dispatcher(broadcast)->broadcastBuilder(preferences)

<a name="1013"></a>
## 8) IOTScanResults (Command 1013)
    Command no 
    1013- JSON format
 
    REQUIRED - 
    Command,CommandType,Payload,AlmondMAC
    
    SQL -
    2.Select on IOT_Scanner
      params: mac

    FUNCTIONAL -
    1.Command 1013
    3.Send listResponse,commandLengthType ToMobile       //where listResponse = payload

    FLOW -
    socket(packet)->validator(do)->processor(do)->almond(IOTScan)->newRowBuilder(IOTScan)->dispatcher(dispatchResponse)

<a name="1800"></a>
## 9) UpdateNotificationRegistration (Command 1800)
    Command no 
    1800- JSON format
 
    REQUIRED - 
    Command,CommandType,Payload

    SQL -
    2.Insert on NotificationID
      // Here Params = parsedPayload

    FUNCTIONAL -
    1.Command 1800
    3.Send listResponse,commandLengthType ToMobile       //where listResponse = payload

    FLOW -
    socket(packet)->validator(do)->processor(do)->notificationNew(do)->genericModel(insertOrUpdate)->oldRowBuilder(notificationNew)->dispatcher(dispatchResponse)

<a name="1900"></a>
## 10) Logout (Command 1900)
    Command no 
    1900- JSON format
 
    REQUIRED - 
    Command,CommandType,Payload

    REDIS -
    4.hmset on UID_<socket.userid>     // values = Q_config.SERVER_NAME,userSession.length - 1

    SQL -
    2.Delete on UserTempPasswords
      params: UserID,TempPassword

    6.Delete on NotificationID
      // here, params = parsedPayload
    
    FUNCTIONAL -
    1.Command 1900
    3.Send listResponse,commandLengthType ToMobile       //where listResponse = payload
    5.delete socketStore[userid]                  //if (userSession.length == 1) 
    7.Send listResponse,commandLengthType ToMobile       //where listResponse = payload

    FLOW -
    socket(packet)->validator(do)->processor(do)->Login(Logout)->connection-pool(queryFunction)->oldRowBuilder(logoutJSON)->dispatcher(dispatchResponse)->dispatcher(socketHandler)->mongo-store(remove)->redisManager(redisExecute)->secondaryModels(notificationNew.do)->oldRowBuilder(notificationNew)->dispatcher(dispatchResponse)

<a name="1112"></a>
## 11) GetAlmondList (Command 1112)
    Command no 
    1112- JSON format
 
    REQUIRED - 
    Command,CommandType,Payload,AlmondMAC,UserID

    REDIS - 
    2.hgetall on UID_<packet.userid>
    3.hgetall on AL_<MACs>

    SQL - 
    4.Select on SCSIDB.CMS
      params: CMSCode
    6.Select on Users
      params: UserID

    FUNCTIONAL - 
    1.Command 1112
    5.Send listResponse,commandLengthType ToMobile       //where listResponse = payload
    7.Send listResponse,commandLengthType ToMobile       //where listResponse = payload

    FLOW - 
    socket(packet)->validator(do)->processor(do)->almond(create_almond_list)->redisManager(getAllAlmonds),redisManager(redisExecuteAll)->oldRowBuilder(create_almond_list)->dispatcher(dispatchResponse)->almond(AlmondAffiliationData)->oldRowBuilder(AffiliationData)->dispatcher(dispatchResponse)

<a name="1526"></a>
## 12) GetClientPreferences (Command 1526)
    Command no 
    1526- JSON format
 
    REQUIRED - 
    Command,CommandType,Payload,UserID

    SQL - 
    2.Select on NotificationPreferences
     params: AlmondMAC,UserID

    FUNCTIONAL - 
    1.Command 1526
    3.Send listResponse,commandLengthType ToMobile       //where listResponse = payload

    FLOW - 
    socket(packet)->validator(do)->processor(do)->notificationPreferences(get_wifi_notification_preferences)->oldRowBuilder(wifiNotificationPreferences)->dispatcher(dispatchResponse)

<a name="1003"></a>
## 13) Login (Command 1003,Command 1)
    Command no 
    1003- JSON format
 
    REQUIRED - 
    Command,CommandType,Payload,UserID,AlmondMAC

    REDIS - 
    5.hgetall on UID_<data.UserID> 
    7.hincrby on UID_<data.UserID>         //values = (Q_<config.SERVER_NAME>,1)

    CASSANDRA - 
    2.Insert on logging.error_log
      params: date,time,ip,server,category,error
    
    SQL - 
    3.Select on Users
      params: EmailID
    4.Insert on UserTempPasswords
      params:UserID,TempPassword,LastUsedTime
    8.Select on Subscriptions
      params: AlmondMAC

    FUNCTIONAL - 
    1.Command 1003 || 1
    6.Send listResponse,commandLengthType ToMobile       //where listResponse = payload
    9.Send listResponse,commandLengthType ToMobile       //where listResponse = payload

    FLOW - 
    socket(packet)->validator(do)->validator(login)->logging(errorLog)->processor(do)->login(Mob_Login)->redisManager(getAllAlmonds)->oldRowBuilder(loginJSON)->dispatcher(dispatchResponse)->mongo-store(add)->redisManager(redisExecute)->login(GetSubscriptions)->oldRowBuilder(subscriptions)->dispatcher(dispatchResponse)

<a name="1110ms"></a>
## 14) UserProfileRequest (Command 1110)
    Command no 
    1110- JSON format
 
    REQUIRED - 
    Command,CommandType,Payload,UserID

    SQL - 
    2.Select on Users
      params: UserID

    FUNCTIONAL - 
    1.Command 1110
    3.Send listResponse,commandLengthType ToMobile       //where listResponse = payload

    FLOW - -
    socket(packet)->validator(do)->processor(do)->account-manager-json(UserProfile)->newRowBuilder(UserProfile)->dispacher(dispatchResponse)

<a name="3"></a>
## 15) Logout (Command 3)
    Command no 
    3- XML format
 
    REQUIRED - 
    Command,CommandType,Payload,UserID
 
    REDIS - 
    4.hmset on UID_<socket.userid>      //values = [Q_config.SERVER_NAME,userSession.length-1]

    SQL - 
    2.Delete on UserTempPasswords
     params: UserID,TemmpPassword

    FUNCTIONAL - 
    1.Command 3
    3.Send listResponse,commandLengthType ToMobile       //where listResponse = payload
   
    //if (userSession.length == 1)
    5.delete socketStore[socket.userid]

    FLOW - 
    socket(packet)->validator(do)->processor(do)->login(logout)->oldRowBuilder(logout)->dispacher(dispatchResponse)->mongo-store(remove)

<a name="6"></a>
## 16) Signup (Command 6)
    Command no 
    6- XML format
 
    REQUIRED - 
    Command,CommandType,Payload,UserID

    REDIS - 
    4.hmset on UID_<socket.userid>      //values = [Q_config.SERVER_NAME,userSession.length-1] 

    SQL - 
    2.Select on Users
      params: EmailID 

    FUNCTIONAL -  
    1.Command 6
    3.Send listResponse,commandLengthType ToMobile       //where listResponse = payload

    //if (userSession.length == 1)
    5.delete socketStore[socket.userid]
    
    FLOW - 
    socket(packet)->validator(do)->processor(do)->accountSetup(Mob_Signup)->oldRowBuilder(accountSetup)->dispacher(dispatchResponse)->mongo-store(remove)

<a name="102"></a>
## 17) CloudSanity (Command 102)
    Command no 
    102- XML format
 
    REQUIRED - 
    Command,CommandType,Payload

    FUNCTIONAL - 
    1.Command 102
    2.Send listResponse,commandLengthType ToMobile       //where listResponse = payload

    FLOW - 
    socket(packet)->validator(do)->processor(do)->almond(sanity_check)->oldRowBuilder(dummy)->dispacher(dispatchResponse)

<a name="113"></a>
## 18) NotificationPreferenceList (Command 113)
    Command no 
    113- XML format
 
    REQUIRED - 
    Command,CommandType,Payload

    SQL - 
    2.Select on NotificationPreferences
      params: AlmondMAC,UserID

    FUNCIONAL -
    1.Command 113
    3.Send listResponse,commandLengthType ToMobile       //where listResponse = payload

    FLOW - 
    socket(packet)->validator(do)->processor(do)->notificationPreferences(get_notification_preference_list)->oldRowBuilder(get_notification_preference_list)->dispacher(dispatchResponse)

<a name="151"></a>
## 19) AlmondModeRequest (Command 151)
    Command no 
    151- XML format
 
    REQUIRED - 
    Command,CommandType,Payload

    SQL - 
    2.select on AlmondPreferences
    params:T1.AlmondMAC

    FUNCTIONAL - 
    1.Command 151
    3.Send listResponse,commandLengthType ToMobile       //where listResponse = payload

    FLOW - 
    socket(packet)->validator(do)->processor(do)->almond(get_almondmode)->oldRowBuilder(get_almondmode)->dispacher(dispatchResponse)

<a name="281"></a>
## 20) NotificationAddRegistration (Command 281)
    Command no 
    281- XML format
 
    REQUIRED - 
    Command,CommandType,Payload

    SQL - 
    2.Select on NotificationID
      params:HashVal

    //if (rows.length == 0)
    3.Insert on NotificationID
      params: HashVal, RegID, UserID, Platform

            (or)

    //if (rows.length == 1)
    3.Update on NotificationID
      params:  UserID,Platform,RegID

    FUNCTIONAL -   
    4.Send listResponse,commandLengthType ToMobile       //where listResponse = payload

    FLOW - 
    socket(packet)->validator(do)->processor(do)->notification(Mobile_Notification_Registration)->oldRowBuilder(notificationAddRegistration)->dispacher(dispatchResponse)

<a name="283"></a>
## 21) NotificationDeleteRegistration (Command 283)
    Command no 
    283- XML format
 
    REQUIRED - 
    Command,CommandType,Payload

    SQL - 
    2.Delete on NotificationID
      params:HashVal,RegID,UserID,Platform

    FUNCTIONAL - 
    1.Commad 283
    3.Send listResponse,commandLengthType ToMobile       //where listResponse = payload

    FLOW - 
    socket(packet)->validator(do)->processor(do)->notification(Mobile_Notification_Delete_Registration)->oldRowBuilder(notificationDeleteRegistration)->dispacher(dispatchResponse)

<a name="804a"></a>
## 22) Command 804 (Client)
    Command no 
    804- XML format
 
    REQUIRED - 
    Command,CommandType,Payload,AlmondMAC,ClientID

    SQL - 
    2.Select on WifiClients
      params:AlmondMAC,ClientID
   
    CASSANDRA - 
    3.Select on dynamic_log
      params: mac,id
   
    FUNCTIONAL - 
    1.Command 804
    4.Send listResponse,commandLengthType ToMobile       //where listResponse = payload

    FLOW - 
    socket(packet)->validator(do)->processor(do)->notification(get_logs)->notificationFetcher(getLogs)->oldRowBuilder(getLogs)->dispacher(dispatchResponse)

<a name="804b"></a>
## 23) Command 804 (Device)
    Command no 
    804- XML format
 
    REQUIRED - 
    Command,CommandType,Payload,DeviceID
       
    CASSANDRA - 
    2.Select on dynamic_log
      params: mac,id

    FUNCTIONAL - 
    1.Command 804
    3.Send listResponse,commandLengthType ToMobile       //where listResponse = payload

    FLOW - 
    socket(packet)->validator(do)->processor(do)->notification(get_logs)->notificationFetcher(getLogs)->oldRowBuilder(getLogs)->dispacher(dispatchResponse)

<a name="806"></a>
## 24) Command 806 
    Command no 
    806- XML format
 
    REQUIRED - 
    Command,CommandType,Payload

    FUNCTIONAL - 
    1.Command 806
    2.Send listResponse,commandLengthType ToMobile       //where listResponse = payload

    FLOW - 
    socket(packet)->validator(do)->processor(do)->notificationFetcher(makeBadgeZero)->oldRowBuilder(clear_the_badge)->dispacher(dispatchResponse)

<a name="800"></a>
## 25) Command 800
    Command no 
    800- XML format
 
    REQUIRED - 
    Command,CommandType,Payload

    CASSANDRA - 
    2.Select on notification_store.notification_records 
    params:usr_id

    FUNCTIONAL - 
    1.Command 800
    3.Send listResponse,commandLengthType ToMobile       //where listResponse = payload

    FLOW - 
    socket(packet)->validator(do)->processor(do)->notificationFetcher(getNotifications)->oldRowBuilder(get_notifications)->dispacher(dispatchResponse)

<a name="1004"></a>
## 26) SUPER_LOGIN (Command 1004) 
    Command no 
    1004- JSON format
 
    REQUIRED - 
    Command,CommandType,Payload

    REDIS - 
    5.hgetall on UID_<data.UserID> 
    7.hincrby on UID_<data.UserID>         //values = (Q_<config.SERVER_NAME>,1)
  
    CASSANDRA - 
    2.Insert on logging.error_log
      params: date,time,ip,server,category,error
    
    SQL - 
    3.Select on Users
      params: EmailID
    4.Insert on UserTempPasswords
      params:UserID,TempPassword,LastUsedTime
    
    FUNCTIONAL - 
    1.Command 1004
    6.Send listResponse,commandLengthType ToMobile       //where listResponse = payload

    FLOW - 
    socket(packet)->validator(do)->validator(login)->logging(errorLog)->processor(do)->login(Mob_Login)->redisManager(getAllAlmonds)->oldRowBuilder(loginJSON)->dispatcher(dispatchResponse)->mongo-store(add)->redisManager(redisExecute)









