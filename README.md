# Server
### Table of contents 
### ALMOND TO MOBILE (Sending to Different server Commands)

- [UpdateDeviceIndex,UpdateDeviceName,AddScene,ActiveScene,UpdateScene,RemoveScene,
RemoveAlllScenes,AddRule,ValidateRule,UpdateRule,RemoveRule,RemoveAllRules,UpdateClient,
RemoveClient (Command 1063(Command 1063)](#1063)
- [RouterSummary,GetWirelessSettings,SetWirelessSettings,RebootRouter,SendLogs,FirmwareUpdate(Command 1100)](#1100a)
- [AffiliationAlmondComplete (Command 25)](#25)
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

### ALMOND TO MOBILE (In the Same Server commands)
- [AlmondHello (Command 1040,Command 31)](#1040||31)
- [AffiliationAlmondRequest (Command 21)](#21)
- [KeepAlive (Command 104)](#104)
- [AlmondReset (Command 1030)](#1030)
----------------------------------------------------------------------------------------------
### MOBILE TO ALMOND (Sending to Different Server Commands)

- [AddRule,UpdateRule,RemoveRule,RemoveAllRules,ValidateRule,GetDeviceIndex,UpdateDeviceName,
UpdateAlmondName,DeviceOnlineCheck,UpdateClient,RemoveClient,RemoveAllClients,WifiClients,
AddScene,SetScene,ActivateScene,DeleteScene,DeleteAllScene(Command 1061)](#1061)
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
----------------------------------------------------------------------------------------------

### ALMOND TO MOBILE (Sending to Different server Commands)

<a name="1063"></a>
## 1) Command 1063
    Command no 
    1063- JSON format
 
    Required 
    Command,CommandType,ICID,Payload

    REDIS -
    (ALMOND SERVER)
    2.Get ICID_<packet.ICID>             

    QUEUE -
    (ALMOND SERVER)
    3.send Response to queue        

    FUNCTIONAL -
    (ALMOND SERVER)
    1.Command 1063 

    (MOBILE SERVER)          
    4.Command 1064              
    5.delete store[unicastID]    
    6.Send Res,commandLengthType to Mobile      

    FLow - (ALMOND SERVER)
    almondProtocol(packet)-> processor(do)->processor(validate)->almondUsers(dummyModel)->processor(dispatchResponses),processor(unicast)->broadcaster(unicast)

<a name="1100a"></a>
## 2) Command 1100    (Almond to Mobile)
    Command no 
    1100- JSON format
 
    Required 
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

    FLow - (ALMOND SERVER)
    almondProtocol(packet)-> processor(do)->processor(validate)->almondUsers(dummyModel)->processor(dispatchResponses),processor(unicast)->broadcaster(unicast)

<a name="25"></a>
## 3) AffiliationAlmondComplete (Command 25)
    Command no
    25- JSON format

    Required
    Command,ICID,Payload,AlmondMAC,CommandType

    SQL -
    (ALMOND SERVER)
    3.INSERT on AlmondUsers
      Params: userID,AlmondMAC,AlmondName,LongSecret,ownership,FirmwareVersion,AffiliationTS
    
    // If(rows are affected)
    4.INSERT on AlmondProperties2
      Params: AlmondMAC,Properties,MobileProperties
    5.Select on Subscriptions
      params: AlmondMAC

    // if ((rsData.CMSCode && sRows[i].CMSCode == rsData.CMSCode) || (!rsData.CMSCode &&
    !sRows[i].CMSCode))
    6.Update on Subscriptions
      params: AlmondMAC, UserID

    //else
    7.Update on Subscriptions
      Params: AlmondMAC,USerID

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
    13.Send Response to queue returned in Step 12
    15.Send Response to All Queues returned in Step 14
    
    /*note: both the queues undergoes same process */

    FUNCTIONAL -
    (ALMOND SERVER)
    1.Command 25
    10.Delete affiliationStore[data.AlmondMAC]
    11.Send AffiliationAlmondCompleteResponse to Almond

    (MOBILE SERVER)
    16.Command 25
    17.delete store[unicastID]
    18.Send Res,commandLengthType to Mobile

    Flow - (ALMOND SERVER)
    almondProtocol(packet)-> processor(do)->processor(validate)->almondUsers(affiliation_almond_complete),almondUsers(verify_affiliation_complete)-> processor(dispatchResponses),processor(unicast)->broadcaster(unicast)->processor(broadcaster)->broadcaster(send)

<a name="63"></a>
## 4)Command 63
    Command no
    63- JSON format
   
    Required
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
    7.delete store[unicastID]
    8.Send Res,commandLengthType to Mobile

    Flow - (ALMOND SERVER)
    almondProtocol(packet)-> processor(do)->processor(validate)->almondUsers(dummyModel)->processor(dispatchResponses),processor(unicast)->broadcaster(unicast) 

<a name="1050"></a>
## 5) DynamicAlmondProperties (Command 1050)
    Command no
    1050- JSON format
   
    Required
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

    (MOBILE SERVER)
    26.Command 1050
    27.delete store[unicastID]
    28.Send Res,commandLengthType to Mobile

    Flow - (ALMOND SERVER)
    almondProtocol(packet)->processor(do)->processor(validate)->almondUsers(properties)->processor(dispatchResponses),processor(sendToBackground)->broadcaster(sendToBackground)

    Flow  - (BACKGROUND SERVER)
    socket(packet)->controller(processor)->preprocessor(doNothing)->almondCommands(DynamicAlmondProperties)->genericModel(get)->receive(mainFunction)->receive(almondProperties)->generator(propertiesNotification)->cassQueries(qtoCassHistory)->cassQueries(qtoCassConverter)->msgService(notificationHandler)->msgService(handleResponse)->scsi(sendFinal)

<a name="49"></a>
## 6) DynamicAlmondNameChange (Command 49)
    Command no
    49- JSON format

    Required
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

    (MOBILE SERVER)
    8.Command 49
    9.delete store[unicastID]
    10.Send Res,commandLengthType to Mobile

    Flow - (ALMOND SERVER)
    almondProtocol(packet)-> processor(do)->processor(validate)->almondUsers(dummyModel)->processor(dispatchResponses),processor(sendToBackground)->broadcaster(sendToBackground)->processor(broadcast)->broadcaster(send)

    Flow - (BACKGROUND SERVER)
    socket(packet)->controller(processor)->preprocessor(doNothing)->almondCommands(almond_name_change)

<a name="153"></a>
## 7) DynamicAlmondModeChange (Command 153)
    Command no
    153- JSON format

    Required
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

    (MOBILE SERVER)
    8.Command 153
    9.delete store[unicastID]
    10.Send Res,commandLengthType to Mobile

    Flow - (ALMOND SERVER)
    almondProtocol(packet)-> processor(do)->processor(validate)->almondUsers(almondmode_change)->processor(dispatchResponses),processor(sendToBackground)->broadcaster(sendToBackground)->processor(broadcast)->broadcaster(send)

    Flow - (BACKGROUND SERVER)
    socket(packet)->controller(processor)->preprocessor(doNothing)->almondCommands(almond_name_change)

<a name="8"></a>
## 8) CloudReset (Command 8)
    Command no
    8- JSON format

    Required
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
    7.delete store[unicastID]
    8.Send Res,commandLengthType to Mobile

    Flow - (ALMOND SERVER)
    almondProtocol(packet)-> processor(do)->processor(validate)->almondUsers(almondReset)->processor(dispatchResponses)->processor(sendToBackground)->broadcaster(sendToBackground)->processor(broadcast)->broadcaster(send)

<a name="1200a"></a>
## 9) DynamicDeviceUpdated,DynamicDeviceAdded (Command 1200)
    Command no
    1200- JSON format
   
    Required
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

    (MOBILE SERVER)
    11.Command 1200
    12.delete store[unicastID]
    13.Send Res,commandLengthType to Mobile

    Flow - (ALMOND SERVER)
    almondProtocol(packet)-> processor(do)->processor(validate)->almondUsers(execute)->processor(dispatchResponses),processor(sendToBackground)->broadcaster(sendToBackground)->processor(broadcast)->broadcaster(send)

    Flow - (BCACKGROUND SERVER)
    consumer(processMessage)->controller(processor)->preprocessor(dynamicDeviceUpdated)->device(execute)->redisDeviceValue(update)->genericModel(update), genericModel(add)->receive(mainFunction)->scsi(sendFinal)

<a name="1200b"></a>
## 10) DynamicDeviceList (Command 1200)
    Command no
    1200- JSON format

    Required
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

    CASSANDRA -
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

    (MOBILE SERVER)
    19.Command 1200
    20.delete store[unicastID]
    21.Send Res,commandLengthType to Mobile

    Flow - (ALMOND SERVER)
    almondProtocol(packet)-> processor(do)->processor(validate)->almondUsers(execute)->processor(dispatchResponses),processor(sendToBackground)->broadcaster(sendToBackground)->processor(broadcast)->broadcaster(send)

    Flow - (BACKGROUND SERVER)
    consumer(processMessage)->controller(processor)->preprocessor(dymamicAddAllDevice)->device(addAll)->redisDeviceValue(getDeviceList)->genericModel(insertBackUpAndUpdate)->device(getDevices)->genericModel(get)->redisDeviceValue(getFormatted)->genericModel(removeAndInsert)->redisDeviceValue(addAll)->receive(mainFunction)->scsi(sendFinal)

<a name="1200c"></a>
## 11) DynamicAllDevicesRemoved (Command 1200)
    Command no
    1200- JSON format

    Required
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

    (MOBILE SERVER)
    29.Command 1200
    30.delete store[unicastID]
    31.Send Res,commandLengthType to Mobile

    Flow - (ALMOND SERVER)
    almondProtocol(packet)-> processor(do)->processor(validate)->almondUsers(execute)->processor(dispatchResponses),processor(sendToBackground)->broadcaster(sendToBackground)->processor(broadcast)->broadcaster(send)

    Flow - (BACKGROUND SERVER)
    socket(packet)->controller(processor)->preprocessor(doNothing)->device(execute)->redisDeviceValue(removeAll)->genericModel(removeAll)->receive(mainFunction)->receive(AlwaysTrue)->generator(wifiNotificationGenerator)->cassQueries(qtoCassHistory)->cassQueries(qtoCassConverter)->msgService(notificationHandler)->msgService(handleResponse)->scsi(sendFinal)

<a name="1200d"></a>
## 12) DynamicDeviceRemoved (Command 1200)
    Command no
    1200- JSON format

    Required
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

    (MOBILE SERVER)
    12.Command 1200
    13.delete store[unicastID]
    14.Send Res,commandLengthType to Mobile

    Flow - (ALMOND SERVER)
    almondProtocol(packet)-> processor(do)->processor(validate)->almondUsers(execute)->processor(dispatchResponses),processor(sendToBackground)->broadcaster(sendToBackground)->processor(broadcast)->broadcaster(send)

    Flow - (BACKGROUND SERVER)
    socket(packet)->controller(processor)->preprocessor(doNothing)->device(execute)->redisDeviceValue(remove)->genericModel(remove)->receive(mainFunction)->scsi(sendFinal)

<a name="1300a"></a>
## 13) DynamicSceneAdded,DynamicSceneActivated,DynamicSceneUpdated (Command 1300)
    Command no
    1300- JSON format
   
    Required
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

    (MOBILE SERVER)
    9.Command 1300
    10.delete store[unicastID]
    11.Send Res,commandLengthType to Mobile

    Flow - (ALMOND SERVER)
    almondProtocol(packet)-> processor(do)->processor(validate)->almondUsers(execute)->processor(dispatchResponses),processor(sendToBackground)->broadcaster(sendToBackground)->processor(broadcast)->broadcaster(send) 

    Flow - (BACKGROUND SERVER) 
    // For DynamicSceneAdded
    socket(packet)->controller(processor)->preprocessor(doNothing)->genericModel(execute)->genericModel(add) 

    // For DynamicSceneActivated,DynamicSceneUpdated
    socket(packet)->controller(processor)->preprocessor(doNothing)->genericModel(execute)->genericModel(update)

<a name="1300b"></a>
## 14) DynamicSceneRemoved,DynamicAllSceneRemoved (Command 1300)
    Command no
    1300- JSON format
   
    Required
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

    (MOBILE SERVER)
    9.Command 1300
    10.delete store[unicastID]
    11.Send Res,commandLengthType to Mobile

    Flow - (ALMOND SERVER)
    almondProtocol(packet)->processor(do)->processor(validate)->almondUsers(execute)->processor(dispatchResponses),processor(sendToBackground)->broadcaster(sendToBackground)->processor(broadcast)->broadcaster(send)

    Flow - (BACKGROUND SERVER)
    socket(packet)->controller(processor)->preprocessor(doNothing)->genericModel(execute)->genericModel(remove)

<a name="1400a"></a>
## 15) DynamicRuleAdded,DynamicRuleUpdated (Command 1400)
    Command no
    1400- JSON format

    Required
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

    (MOBILE SERVER)
    9.Command 1400
    10.delete store[unicastID]
    11.Send Res,commandLengthType to Mobile

    Flow - (ALMOND SERVER)
    almondProtocol(packet)->processor(do)->processor(validate)->almondUsers(execute)->processor(dispatchResponses),processor(sendToBackground)->broadcaster(sendToBackground)->processor(broadcast)->broadcaster(send)

    Flow - (BACKGROUND SERVER)
    
    //For,DynamicRuleAdded
    consumer(processMessage)->controller(processor)->preprocessor(doNothing)->genericModel(execute),genericModel(add)

    //For,DynamicRuleUpdated
    consumer(processMessage)->controller(processor)->preprocessor(doNothing)->genericModel(execute),genericModel(Update)

<a name="1400b"></a>
## 16) DynamicRuleRemoved (Command 1400)
    Command no
    1400- JSON format

    Required
    Command,UID,CommandType,Payload,AlmondMAC

    SQL- 
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
    
    (MOBILE SERVER)
    9.Command 1400
    10.delete store[unicastID]
    11.Send Res,commandLengthType to Mobile

    Flow - (ALMOND SERVER)
    almondProtocol(packet)->processor(do)->processor(validate)->almondUsers(execute)->processor(dispatchResponses),processor(sendToBackground)->broadcaster(sendToBackground)->processor(broadcast)->broadcaster(send)

    Flow - (BACKGROUND SERVER)
    consumer(processMessage)->controller(processor)->preprocessor(doNothing)->genericModel(execute),genericModel(remove)

<a name="1400c"></a>
## 17) DynamicAllRulesRemoved (Command 1400)
    Command no
    1400- JSON format

    Required
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

    (MOBILE SERVER)
    10.Command 1400
    11.delete store[unicastID]
    12.Send Res,commandLengthType to Mobile

    Flow - (ALMOND SERVER)
    almondProtocol(packet)->processor(do)->processor(validate)->almondUsers(execute)->processor(dispatchResponses),processor(sendToBackground)->broadcaster(sendToBackground)->processor(broadcast)->broadcaster(send)

    Flow - (BACKGROUND SERVER)
    consumer(processMessage)->controller(processor)->preprocessor(doNothing)->genericModel(execute),genericModel(removeAll)

<a name="1500a"></a>
## 18) DynamicClientAdded (Command 1500)
    Command no
    1500- JSON format

    Required
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

    (MOBILE SERVER)
    23.Command 1500
    24.delete store[unicastID]
    25.Send Res,commandLengthType to Mobile

    Flow - (ALMOND SERVER)
    almondProtocol(packet)->processor(do)->processor(validate)->almondUsers(execute)->processor(dispatchResponses),processor(sendToBackground)->broadcaster(sendToBackground)->processor(broadcast)->broadcaster(send)

    Flow - (BACKGROUND SERVER)
    socket(packet)->controller(processor)->preprocessor(doNothing)->genericModel(execute)->genericModel(add)->receive(mainFunction)->notify(sendAlwaysClient)->generator(wifiNotificationGenerator)->cassandra(qtoCassHistory)->cassandra(qtoCassConverter)->msgService(notificationHandler)->msgService(handleResponse)

<a name="1500b"></a>
## 19) DynamicClientUpdated (Command 1500)
    Command no
    1500- JSON format

    Required
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

    (MOBILE SERVER)
    9.Command 1500
    10.delete store[unicastID]
    11.Send Res,commandLengthType to Mobile

    Flow - (ALMOND SERVER)
    almondProtocol(packet)-> processor(do)->processor(validate)->almondUsers(execute)->processor(dispatchResponses),processor(sendToBackground)->broadcaster(sendToBackground)->processor(broadcast)->broadcaster(send)

    Flow - (BACKGROUND SERVER)
    socket(packet)->controller(processor)->preprocessor(doNothing)->genericModel(execute)->genericModel(update)->receive(mainFunction)

<a name="1500c"></a>
## 20) DynamicClientRemoved (Command 1500)
    Command no
    1500- JSON format

    Required
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

    (MOBILE SERVER)
    24.Command 1500
    25.delete store[unicastID]
    26.Send Res,commandLengthType to Mobile

    Flow - (ALMOND SERVER) 
    almondProtocol(packet)->processor(do)->processor(validate)->almondUsers(execute)->processor(dispatchResponses),processor(sendToBackground)->broadcaster(sendToBackground)->processor(broadcast)->broadcaster(send)

    Flow - (BACKGROUND SERVER)
    socket(packet)->controller(processor)->preprocessor(dynamicClientRemoved)->genericModel(execute)->genericModel(remove)->receive(mainFunction)->receive(sendAlwaysClient)->generator(wifiNotificationGenerator)->cassQueries(qtoCassHistory)->cassQueries(qtoCassConverter)->msgService(notificationHandler)->msgService(handleResponse)

<a name="1500d"></a>
## 21) DynamicClientList (Command 1500)
    Command no
    1500- JSON format

    Required
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

    (MOBILE SERVER)
    11.Command 1500
    12.delete store[unicastID]- [AlmondHello (Command 1040)](#1040)
    13.Send Res,commandLengthType to Mobile

    Flow - (ALMOND SERVER)
    almondProtocol(packet)->processor(do)->processor(validate)->almondUsers(execute)->processor(dispatchResponses),processor(sendToBackground)->broadcaster(sendToBackground)->processor(broadcast)->broadcaster(send)

    Flow - (BACKGROUND SERVER)
    socket(packet)->controller(processor)->preprocessor(doNothing)->genericModel(execute)->genericModel(addAll),genericModel(insertBackUpAndUpdate),genericModel(get),cassandra(execute)->genericModel(removeAndInsert)->receive(mainFunction)

<a name="1500e"></a>
## 22) DynamicAllClientsRemoved (Command 1500)
    Command no
    1500- JSON format

    Required
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

    (MOBILE SERVER)
    19.Command 1500
    20.delete store[unicastID]
    21.Send Res,commandLengthType to Mobile

    Flow - (ALMOND SERVER)
    almondProtocol(packet)->processor(do)->processor(validate)->almondUsers(execute)->processor(dispatchResponses),processor(sendToBackground)->broadcaster(sendToBackground)->processor(broadcast)->broadcaster(send)

    Flow - (BACKGROUND SERVER)
    socket(packet)->controller(processor)->preprocessor(doNothing)->genericModel(execute)->genericModel(removeAll)->receive(mainFunction)->receive(AlwaysTrue)->generator(wifiNotificationGenerator)->cassQueries(qtoCassConverter)->msgService(notificationHandler)->msgService(handleResponse)

<a name="1500f"></a>
## 23) DynamicClientJoined,DynamicClientLeft (Command 1500)
    Command no
    1500- JSON format

    Required
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

    (MOBILE SERVER)
    24.Command 1500
    25.delete store[unicastID]
    26.Send Res,commandLengthType to Mobile

    Flow - (ALMOND SERVER)
    almondProtocol(packet)->processor(do)->processor(validate)->almondUsers(execute)->processor(dispatchResponses),processor(sendToBackground)->broadcaster(sendToBackground)->processor(broadcast)->broadcaster(send)

    Flow - (BACKGROUND SERVER)
    socket(packet)->controller(processor)->preprocessor(doNothing)->genericModel(execute)->genericModel(update)->receive(mainFunction)->receive(checkClientPreference)->generator(wifiNotificationGenerator)->cassQueries(qtoCassHistory)->cassQueries(qtoCassConverter)->msgService(notificationHandler)->msgService(handleResponse)
----------------------------------------------------------------------------------------------------------------------------
### Commands in the same server (ALMOND SERVER)

<a name="1040||31"></a>
## 24) AlmondHello (Command 1040|| Command 31)
     Command no -
     1040||31 - JSON format

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

     Flow -
     almondProtocol(packet)->processor(do)->processor(validate)->almondUsers(almond_hello)->processor(dispatchResponses)

<a name="21"></a>
## 25) AffiliationAlmondRequest (Command 21)
    Command no =
    21- JSON format

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
    // where code = random string,values =AlmondMAC + config.Connections.RabbitMQ.Queue

    FUNCTIONAL -
    1.Command 21
    7.Send AffiliationAlmondRequestResponse to Almond

    Flow -
    almondProtocol(packet)-> processor(do)->processor(validate)->almondUsers(affiliation_almond)->affiliate-almond(affiliate_almond)->processor(dispatchResponses)

<a name="104"></a>
## 26) KeepAlive (Command 104)
    Command no
    104- JSON format

    Required
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

    Flow -
    almondProtocol(packet)-> processor(do)->processor(validate)->almondStore(keepAlive)

<a name="1030"></a>
## 27) AlmondReset (Command 1030)
    Command no
    1030- JSON format

    Required
    Command,CommandType,Payload,AlmondMAC

    FUNCTIONAL -
    1.Command 1030
    2.Send AlmondResetResponse to Almond

    Flow -
    almondProtocol(packet)-> processor(do)->processor(validate)->almondUsers(almondReset)->processor(dispatchResponses)

----------------------------------------------------------------------------------------------

### MOBILE TO ALMOND  (Sending to Different Server Commands)

<a name="1061"></a>
## 1) Command 1061
    Command no 
    1061- JSON format
 
    Required 
    Command,CommandType,Payload,almondMAC

    REDIS -
    (MOBILE SERVER)
    3.hgetall on AL_<AlmondMAC>      //(AlmondMAC = <packet.parsedPayload.AlmondMAC>)    
    4.get on ICID_<string>           // here <string> = random string data)              
    5.setex on ICID_<string>        // (prefix + key), value = SERVER_NAME     

    QUEUE -
    (MOBILE SERVER)
    7.Send response to config.SERVER_NAME   /*(payload,command,almondMAC) to queue*/                                            
    
    FUNCTIONAL -
    (MOBILE SERVER) 
    1.Command 1061             
    2.Send listResponse,commandLengthType ToMobile  //where listResponse = payload   
    6.delete store[commandID]    

    (ALMOND SERVER)
    8.Command 1062               
    9.Send Res,CommandLengthType to Almond           

    Flow - (MOBILE SERVER)
    socket(packet)->validator(do)->processor(do)->almond(onlyUnicast)->dispatcher(dispatchResponse)->dispatcher(unicast)->broadcaster(unicast)

<a name="1100b"></a>
## 2) Command 1100    
    Command no 
    1100- JSON format
 
    Required 
    Command,CommandType,Payload,almondMAC

    REDIS -
    (MOBILE SERVER)
    3.hgetall on AL_<AlmondMAC>          //(AlmondMAC = <packet.parsedPayload.AlmondMAC>)   
    4.get on ICID_<string>              // here <string> = random string data)      
    5.setex on ICID_<string>            // (prefix + key), value = SERVER_NAME    

    QUEUE -
    (MOBILE SERVER)
    7.Send response to config.SERVER_NAME        // (payload,command,almondMAC) to queue             

    FUNCTIONAL -
    (MOBILE SERVER)
    1.Command 1100           
    2.Send listResponse,commandLengthType ToMobile      //where listResponse = payload    
    6.delete store[commandID] 

    (ALMOND SERVER)              
    8.Command 1100            
    9.Send Res,CommandLengthType to Almond             

    Flow- (MOBILE SERVER)
    socket(packet)->validator(do)->processor(do)->almond(onlyUnicast)->dispatcher(dispatchResponse)->dispatcher(unicast)->broadcaster(unicast)

<a name="23"></a>
## 3)AffiliationUserRequest (Command 23)
    Command no 
    23- JSON format
 
    Required 
    Command,CommandType,Payload

    REDIS -
    (MOBILE SERVER)
    2.get on CODE:<data.code>  
    4.get on ICID_<string>              // here <string> = random string data)
    5.setex on ICID_<string>            // (prefix + key), value = SERVER_NAME
        
    SQL -
    (MOBILE SERVER)
    3.Select on Users
      params: UserID
     
    QUEUE
    (MOBILE SERVER)
    8.Send AffiliationUserRequestResponse to rows.server
 
    FUNCTIONAL -
    (MOBILE SERVER)
    1.Command 23
    6.Send listResponse,commandLengthType ToMobile       //where listResponse = payload
    7.delete store[row.CommandID]

    (ALMOND SERVER)
    9.Command 26
    10.Send Res,CommandLengthType to Almond

    Flow - (MOBILE SERVER)
    socket(packet)->validator(do)->processor(do)->affiliation(execute)->redisManager(getCode)->sqlManager(getEmail)->cid-bid(incCommandID)->newRowBuilder(affiliationError)->dispatcher(dispatchResponse)->dispatcher(unicast)->broadcaster(unicast)

<a name="61"></a>
## 4)AlmondModeChange (Command 61)
    Command no 
    61- JSON format
 
    Required 
    Command,CommandType,Payload,almondMAC

    REDIS -
    (MOBILE SERVER)
    3.hgetall on AL_<AlmondMAC>          //(AlmondMAC = <packet.parsedPayload.AlmondMAC>)
    4.get on ICID_<string>              // here <string> = random string data)
    5.setex on ICID_<string>            // (prefix + key), value = SERVER_NAME
  
    QUEUE -
    (MOBILE SERVER)
    7.Send response to config.SERVER_NAME       // (payload,command,almondMAC) to queue

    FUNCTIONAL - 
    (MOBILE SERVER)
    1.Command 61
    2.Send listResponse,commandLengthType ToMobile         //where listResponse = payload
    6.delete store[commandID]

    (ALMOND SERVER)
    8.Command 62
    9.Send Res,CommandLengthType to Almond

    Flow -
    (MOBILE SERVER)
    socket(packet)->validator(do)->processor(do)->almond(onlyUnicast)->dispatcher(dispatchResponse)->dispatcher(unicast)->broadcaster(unicast)

<a name="1023"></a>
## 5)AffiliationUserRequest (Command 1023)
    Command no 
    1023- JSON format
 
    Required 
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

    Flow - (MOBILE SERVER)
    socket(packet)->validator(do)->processor(do)->affiliation(execute)->redisManager(getCode)->sqlManager(getEmail)->cid-bid(incCommandID)->newRowBuilder(affiliationError)->dispatcher(dispatchResponse)->dispatcher(unicast)->broadcaster(unicast)

<a name="1700a"></a>
## 6)UpdateDevicePreference (Command 1700) 
    Command no
    1700- JSON format 

    Required
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

    (ALMOND SERVER)
    7.Command 1700
    8.Send Res,CommandLengthType to Almond

    Flow - (MOBILE SERVER)
    socket(packet)->validator(do)->processor(do)->notiPrefs(do)->genericModel(delete)->oldRowBuilder(newPref)->dispatcher(dispatchResponse)->dispatcher(broadcast)->broadcastBuilder(preferences)

<a name="1700b"></a>
## 7)UpdateClientPreferences (Command 1700) 
    Command no
    1700- JSON format 

    Required
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

    (ALMOND SERVER)
    7.Command 1700
    8.Send Res,CommandLengthType to Almond

    Flow - (MOBILE SERVER)
    socket(packet)->validator(do)->processor(do)->notiPrefs(do)->genericModel(delete)->oldRowBuilder(newPref)->dispatcher(dispatchResponse)->dispatcher(broadcast)->broadcastBuilder(preferences)

<a name="1525"></a>
## 8)UpdateClientPreferences (Command 1525)
    Command no 
    1525- JSON format
 
    Required 
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

    (ALMOND SERVER)
    7.Command 1525
    8.Send Res,CommandLengthType to Almond

    Flow - (MOBILE SERVER)
    socket(packet)->validator(do)->processor(do)->notificationPreferences(change_wificlient_notification_preferences)->oldRowBuilder(wifiNotificationPreferences)->dispatcher(dispatchResponse)->broadcastBuilder(defaultXML)->broadcaster(broadcast)

<a name="300"></a>
## 9)NotificationPreferences (Command 300)
    Command no 
    300- JSON format
 
    Required 
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

    (ALMOND SERVER)
    6.Command 300
    7.Send Res,CommandLengthType to Almond

    Flow - (MOBILE SERVER)
    socket(packet)->validator(do)->processor(do)->notificationPreferences(update_notification_preferences)->oldRowBuilder(notificationPreferences)->dispacher(dispatchResponse)->dispatcher(broadcast)->broadcastBuilder(defaultXML)->broadcaster(broadcast)

<a name="4"></a>
## 10)Logoutall (Command 4)
    Command no 
    4- JSON format
 
    Required 
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

    (ALMOND SERVER)
    10.Command 4
    11.Send Res,CommandLengthType to Almond

    Flow - (MOBILE SERVER)
    socket(packet)->validator(do)->validator(checkCredentials)->sqlManager(getUser)->processor(do)->login(logoutAll)->connection-pool(queryFunction)->oldRowBuilder(logoutAll)->dispacher(dispatchResponse)->mongo-store(removeAll)->dispatcher(broadcast)->broadCastBuilder(removeAll)->broadcaster(broadcast)

<a name="2222"></a>
## 11)Restore (Command 2222)
    Command no 
    2222- JSON format
 
    Required 
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

    Flow - (MOBILE SERVER)
    socket(packet)->validator(do)->processor(do)->notificationFetcher(almondHistroy)->newRowBuilder(almondHistroy)->dispatcher(dispatchResponse)->dispatcher(unicast)->newRowBuilder(restoreData)->broadcaster(unicast)

<a name="1060a"></a>
## 12)ChangeUser (Command 1060,Action:Add) 
    Command no 
    1060- JSON format
 
    Required 
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

    (ALMOND SERVER)
    7.Command 1060
    8.Send Res,CommandLengthType to Almond

    Flow - (MOBILE SERVER)
    socket(packet)->validator(do)->processor(do)->clientModel(update)->newRowBuilder(change_user)->dispatcher(broadcast)->broadcastBuilder(change_user)->broadcaster(broadcast)

<a name="1060b"></a>
## 13)ChangeUser (Command 1060,Action:Update) 
    Command no 
    1060- JSON format
 
    Required 
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

    (ALMOND SERVER)
    8.Command 1060
    9.Send Res,CommandLengthType to Almond

    Flow - (MOBILE SERVER)
    socket(packet)->validator(do)->processor(do)->clientModel(update)->newRowBuilder(change_user)->dispatcher(broadcast)->broadcastBuilder(change_user)->broadcaster(broadcast)

<a name="1110a"></a>
## 14.UnlinkAlmondRequest (Command 1110) 
    Command no 
    1110- JSON format
 
    Required 
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
 
    Flow - (MOBILE SERVER)
    socket(packet)->validator(do)->validator(unlink),validator(checkCredentials)->sqlManager(getUser)->processor(do)->account-manager-json(UnlinkAlmond)->rowBuilder(defaultReply)->dispatcher(unicast)->dispatcher(dispatchResponse)->account-manager-json(getAlmond)->broadcastBuilder(unlink)->broadcaster(unicast)

<a name="1110b"></a>
## 15.UserInviteRequest (Command 1110) 
    Command no 
    1110- JSON format
 
    Required 
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
    14.Send UserInviteRequestResponse to config.SERVER_NAME
    15.Send UserInviteRequestResponse to MobileQueue
    
    FUNCTIONAL -
    (MOBILE SERVER)
    1.Command 1110
    10.Send listResponse,commandLengthType ToMobile       //where listResponse = payload
    14.delete store[commandID]
    16.Send listResponse,commandLengthType ToMobile       //where listResponse = payload

    (ALMOND SERVER)
    18.Command 1110
    19.Send Res,CommandLengthType to Almond

    Flow - (MOBILE SERVER)
    socket(packet)->validator(do)->processor(do)->account-manager-json(UserInvite)->sqlManager(checkSecondary),sqlManager(addSecondaryAlmond)->redisManager(addSecondaryAlmond)->sqlManager(getEmail)->redisManager(getAlmond)->rowBuilder(defaultReply)->dispatcher(dispatchResponse)->dispatcher(unicast)->rowBuilder(userChange)->rowBuilder(userChange)->broadcaster(unicast)->dispatcher(broadcast)->broadcastBuilder(almondAdd),broadcastBuilder(userAdd)->broadcaster(broadcast)

<a name="1110c"></a>
## 16.UpdateUserProfileRequest (Command 1110) 
    Command no 
    1110- JSON format
 
    Required 
    Command,CommandType,Payload,UserID

    REDIS -
    (MOBILE SERVER)
    multi
    5.hgetall on UID_<userID>         //here, multi is done on every userID in UserList
    
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
    
    (ALMOND SERVER)
    6.Command 1110
    7.Send Res,CommandLengthType to Almond

    Flow - (MOBILE SERVER)
    socket(packet)->validator(do)->processor(do)->account-manager-json(UpdateUserProfile)->rowBuilder(UpdateUserProfile)->dispatcher(dispatchResponse)-> dispatcher(broadcast)->broadcastBuilder(userProfileUpdate)->broadcaster(broadcast)

<a name="1110d"></a>
## 17.DeleteSecondaryUserRequest (Command 1110) 
    Command no 
    1110- JSON format
 
    Required 
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

    (ALMOND SERVER)
    15.Command 1110
    16.Send Res,CommandLengthType to Almond

    Flow - (MOBILE SERVER)
    socket(packet)->validator(do)->processor(do)->account-manager-json(DeleteSecondaryUser)->sqlManager(deleteUser)->redisManager(redisExecute),redisManager(deleteAccount)->rowBuilder(defaultReply)->dispatcher(dispatchResponse)->dispatcher(unicast)-> rowBuilder(userChange)->broadcaster(unicast)->dispatcher(broadcast)->broadcastBuilder(almondDelete),broadcastBuilder(userDelete)->broadcaster(broadcast)

<a name="1110e"></a>
## 18.DeleteMeAsSecondaryUserRequest (Command 1110) 
    Command no 
    1110- JSON format
 
    Required 
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

    (ALMOND SERVER)
    16.Command 1110
    17.Send Res,CommandLengthType to Almond

    Flow - (MOBILE SERVER)
    socket(packet)->validator(do)->processor(do)->account-manager-json(DeleteMeAsSecondaryUserResponse)->redisManager(getAlmond)->sqlManager(deleteUser)->redisManager(redisExecute),redisManager(deleteAccount)->rowBuilder(defaultReply)->dispatcher(dispatchResponse)->dispatcher(unicast)-> rowBuilder(userChange)->broadcaster(unicast)->dispatcher(broadcast)->broadcastBuilder(almondDelete),broadcastBuilder(userDelete)->broadcaster(broadcast)

<a name="1110f"></a>
## 19.ChangePasswordRequest (Command 1110) 
    Command no 
    1110- JSON format
 
    Required 
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

    (ALMOND SERVER)
    11.Command 1110
    12.Send Res,CommandLengthType to Almond

    Flow - (MOBILE SERVER)
    socket(packet)->validator(do)->validator(checkCredentials)->processor(do)->account-manager-json(ChangePassword)->connection-pool(queryFunction)->rowBuilder(defaultReply)->dispatcher(dispatchResponse)->mongo-store(removeAllExceptCurrent)->dispatcher(broadcast)->broadcastBuilder(removeAll)->broadcaster(broadcast)

<a name="1110g"></a>
## 20.DeleteAccountRequest (Command 1110) 
    Command no 
    1110- JSON format
 
    Required 
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

    (ALMOND SERVER)
    17.Command 1110
    18.Send Res,CommandLengthType to Almond
 
    Flow - (MOBILE SERVER)
    socket(packet)->validator(do)->validator(checkCredentials)->processor(do)->account-manager-json(DeleteAccount)->sqlManager(deleteUser)->redisManager(redisExecute),redisManager(deleteAccount)->rowBuilder(defaultReply)->dispatcher(dispatchResponse)->mongo-store(removeAll)->dispatcher(broadcast)->broadcastBuilder(removeAll)->broadcaster(broadcast)->dispatcher(broadcastToAllAlmonds)->broadcaster(broadcastModel)















