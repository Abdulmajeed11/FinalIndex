# Server
### Table of contents 
- [AddRule,UpdateRule,RemoveRule,RemoveAllRules,ValidateRule,GetDeviceIndex,UpdateDeviceName,
UpdateAlmondName,DeviceOnlineCheck,UpdateClient,RemoveClient,RemoveAllClients,WifiClients,
AddScene,SetScene,ActivateScene,DeleteScene,DeleteAllScene(Command 1061)](#1061)
- [UpdateDeviceIndex,UpdateDeviceName,AddScene,ActiveScene,UpdateScene,RemoveScene,RemoveAlllScenes,
AddRule,ValidateRule,UpdateRule,RemoveRule,RemoveAllRules,UpdateClient,RemoveClient (Command 1063(Command 1063)](#1063)
- [RouterSummary,GetWirelessSettings,SetWirelessSettings,RebootRouter,SendLogs,FirmwareUpdate(Command 1100)](#1100a)  (Mobile to Almond)
- [RouterSummary,GetWirelessSettings,SetWirelessSettings,RebootRouter,SendLogs,FirmwareUpdate(Command 1100)](#1100b)  (Almond to Mobile)
- [AffiliationUserRequest (Command 23)](#23)
- [AffiliationAlmondComplete (Command 25)](#25)
- [AlmondModeChange (Command 63)](#63a)
- [AlmondNameChange (Command 63)](#63b)
- [DynamicAlmondProperties (Command 1050)](#1050) 
- [DynamicAlmondNameChange (Command 49)](#49)
- [DynamicAlmondModeChange (Command 153)](#153)
- [CloudReset (Command 8)](#8)
- [DynamicDeviceUpdated (Command 1200)](#1200a)
- [DynamicDeviceList (Command 1200)](#1200b)
- [DynamicDeviceAdded (Command 1200)](#1200c)
- [DynamicAllDevicesRemoved (Command 1200)](#1200d)
- [DynamicDeviceRemoved (Command 1200)](#1200e)
- [DynamicSceneAdded (Command 1300)](#1300a)
- [DynamicSceneActivated (Command 1300)](#1300b)
- [DynamicSceneUpdated (Command 1300)](#1300c)
- [DynamicSceneRemoved (Command 1300)](#1300d)
- [DynamicAllSceneRemoved (Command 1300)](#1300e)
- [DynamicRuleAdded (Command 1400)](#1400a)
- [DynamicRuleUpdated (Command 1400)](#1400b)
- [DynamicRuleRemoved (Command 1400)](#1400c)
- [DynamicAllRulesRemoved (Command 1400)](#1400d)
- [DynamicClientAdded (Command 1500)](#1500a)
- [DynamicClientUpdated (Command 1500)](#1500b)
- [DynamicClientRemoved (Command 1500)](#1500c)
- [DynamicClientList (Command 1500)](#1500d)
- [DynamicAllClientsRemoved (Command 1500)](#1500e)
- [DynamicClientJoined (Command 1500)](#1500f)
- [DynamicClientLeft (Command 1500)](#1500g)

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

    (ALMOND SERVER)               
    10.get on ICID_<result.unicastID>      

    QUEUE -
    (MOBILE SERVER)
    7.Send response to config.SERVER_NAME   /*(payload,command,almondMAC) to queue*/ 
   
    (ALMOND SERVER)
    11.Send Response to queue                                               
    
    FUNCTIONAL -
    (MOBILE SERVER) 
    1.Command 1061             
    2.Send listResponse,commandLengthType ToMobile  //where listResponse = payload   
    6.delete store[commandID]    

    (ALMOND SERVER)
    8.Command 1062               
    9.Send Res,CommandLengthType to Almond       
    12.Append result.almondMAC to offlineMACS.txt       

    Flow - (MOBILE SERVER)
    socket(packet)->validator(do)->processor(do)->almond(onlyUnicast)->dispatcher(dispatchResponse)->dispatcher(unicast)->broadcaster(unicast)

<a name="1063"></a>
## 2) Command 1063
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
## 3) Command 1100     (Mobile to Almond)
    Command no 
    1100- JSON format
 
    Required 
    Command,CommandType,Payload,almondMAC

    REDIS -
    (MOBILE SERVER)
    3.hgetall on AL_<AlmondMAC>          //(AlmondMAC = <packet.parsedPayload.AlmondMAC>)   
    4.get on ICID_<string>              // here <string> = random string data)      
    5.setex on ICID_<string>            // (prefix + key), value = SERVER_NAME   

    (ALMOND SERVER)
    10.get on ICID_<result.unicastID>  

    QUEUE -
    (MOBILE SERVER)
    7.Send response to config.SERVER_NAME        // (payload,command,almondMAC) to queue   
    
    (ALMOND SERVER)
    11.Send Response to queue           

    FUNCTIONAL -
    (MOBILE SERVER)
    1.Command 1100           
    2.Send listResponse,commandLengthType ToMobile      //where listResponse = payload    
    6.delete store[commandID] 

    (ALMOND SERVER)              
    8.Command 1100            
    9.Send Res,CommandLengthType to Almond       
    12.Append result.almondMAC to offlineMACS.txt       

    Flow- (MOBILE SERVER)
    socket(packet)->validator(do)->processor(do)->almond(onlyUnicast)->dispatcher(dispatchResponse)->dispatcher(unicast)->broadcaster(unicast)

<a name="1100b"></a>
## 4) Command 1100    (Almond to Mobile)
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
    4.Command 153          
    5.delete store[unicastID]    
    6.Send Res,commandLengthType to Mobile   

    FLow - (ALMOND SERVER)
    almondProtocol(packet)-> processor(do)->processor(validate)->almondUsers(dummyModel)->processor(dispatchResponses),processor(unicast)->broadcaster(unicast)

<a name="23"></a>
## 5)AffiliationUserRequest (Command 23)
    Command no 
    23- JSON format
 
    Required 
    Command,CommandType,Payload

    REDIS -
    (MOBILE SERVER)
    2.get on CODE:<data.code>  
    4.get on ICID_<string>              // here <string> = random string data)
    5.setex on ICID_<string>            // (prefix + key), value = SERVER_NAME

    (ALMOND SERVER)
    11.get on ICID_<result.unicastID>
        
    SQL -
    (MOBILE SERVER)
    3.Select on Users
      params: UserID
     
    QUEUE
    (MOBILE SERVER)
    8.Send AffiliationUserRequestResponse to rows.server

    (ALMOND SERVER)
    12.Send Response to queue
 
    FUNCTIONAL -
    (MOBILE SERVER)
    1.Command 23
    6.Send listResponse,commandLengthType ToMobile       //where listResponse = payload
    7.delete store[row.CommandID]

    (ALMOND SERVER)
    9.Command 24
    10.Send Res,CommandLengthType to Almond
    13.Append result.almondMAC to offlineMACS.txt

    Flow - (MOBILE SERVER)
    socket(packet)->validator(do)->processor(do)->affiliation(execute)->redisManager(getCode)->sqlManager(getEmail)->cid-bid(incCommandID)->newRowBuilder(affiliationError)->dispatcher(dispatchResponse)->dispatcher(unicast)->broadcaster(unicast)

<a name="25"></a>
## 6) AffiliationAlmondComplete (Command 25)
    Command no
    25- JSON format

    Required
    Command,ICID,Payload,AlmondMAC,CommandType

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

<a name="63a"></a>
## 7)AlmondModeChange (Command 63)
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

<a name="63b"></a>
## 8)AlmondNameChange (Command 63)
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
## 9) DynamicAlmondProperties (Command 1050)
    Command no
    1050- JSON format
   
    Required
    Command,AlmondMAC,Payload,CommandType

    SQL
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
## 10) DynamicAlmondNameChange (Command 49)
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
## 11) DynamicAlmondModeChange (Command 153)
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
## 12) CloudReset (Command 8)
    Command no
    8- JSON format

    Required
    Command,CommandType,Payload

    REDIS -
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
## 13) DynamicDeviceUpdated (Command 1200)
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
    4.Send DynamicDeviceUpdated to BACKGROUND_QUEUE
    6.Send Response to All Queues returned in Step 5

    FUNCTIONAL -
    (ALMOND SERVER)
    1.Command 1200
    3.Send DynamicDeviceUpdatedResponse to Almond

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
## 14) DynamicDeviceList (Command 1200)
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
    2.hmset on AL_<AlmondMAC>          //value = [mapper.hashColumn, payload.HashNow]
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
    11.hmset on MAC:<AlmondMAC>:Key, variables    
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
## 15) DynamicDeviceAdded (Command 1200)
    Command no
    1200- JSON format

    Required
    Command,UID,CommandType,Payload

    SQL -
    (BACKGROUND SERVER)
    9.Insert on AlmondplusDB.DEVICE_DATA
      params: AlmondMAC
    10.Select on SCSIDB.CMSAffiliations,AlmondplusDB.AlmondUsers,SCSIDB.CMS
      params: CA.CMSCode,AU.AlmondMAC

    REDIS -
    (ALMOND SERVER)   
    2.hmset on AL_<AlmondMAC>          //value = [mapper.hashColumn, payload.HashNow]
    5.hgetall on UID_<user_list>        // Returns all the queues for users in user_list

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
    4.Send DynamicDevieAdded to BACKGROUND_QUEUE
    6.Send Response to All Queues returned in Step 5

    FUNCTIONAL -
    (ALMOND SERVER)
    1.Command 1200
    3.Send DynamicDeviceAddedResponse to Almond

    (BACKGROUND SERVER)
    7.Command 1200

    (MOBILE SERVER)
    11.Command 1200
    12.delete store[unicastID]
    13.Send Res,commandLengthType to Mobile

    Flow - (ALMOND SERVER)
    almondProtocol(packet)-> processor(do)->processor(validate)->almondUsers(execute)->processor(dispatchResponses),processor(sendToBackground)->broadcaster(sendToBackground)->processor(broadcast)->broadcaster(send)

    Flow - (BACKGROUND SERVER)
    consumer(processMessage)->controller(processor)->preprocessor(dymamicAddAllDevice)->device(execute)->redisDeviceValue(add)->genericModel(add)->receive(mainFunction)->scsi(sendFinal)

<a name="1200d"></a>
## 16) DynamicAllDevicesRemoved (Command 1200)
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

<a name="1200e"></a>
## 17) DynamicDeviceRemoved (Command 1200)
    Command no
    1200- JSON format

    Required
    Command,UID,CommandType,Payload

    SQL -
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
## 18) DynamicSceneAdded (Command 1300)
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
    4.Send DynamicSceneAdded to BACKGROUND_QUEUE
    6.Send Response to All Queues returned in Step 5

    FUNCTIONAL -
    (ALMOND SERVER)
    1.Command 1300
    3.Send DynamicSceneAddedResponse to Almond

    (BACKGROUND SERVER)
    7.Command 1300

    (MOBILE SERVER)
    9.Command 1300
    10.delete store[unicastID]
    11.Send Res,commandLengthType to Mobile

    Flow - (ALMOND SERVER)
    almondProtocol(packet)-> processor(do)->processor(validate)->almondUsers(execute)->processor(dispatchResponses),processor(sendToBackground)->broadcaster(sendToBackground)->processor(broadcast)->broadcaster(send) 

    Flow - (BACKGROUND SERVER)
    socket(packet)->controller(processor)->preprocessor(doNothing)->genericModel(execute)->genericModel(add) 

<a name="1300b"></a>
## 19) DynamicSceneActivated (Command 1300)
    Command no
    1300- JSON format
   
    Required
    Command,UID,CommandType,Payload,AlmondMAC

    SQL - 
    8.Insert on AlmondplusDB.SCENE
      params:AlmondMAC

    REDIS -
    (ALMOND SERVER)
    2.hmset on AL_<AlmondMAC>      // value = [mapper.hashColumn, payload.HashNow]        
    5.hgetall on UID_<user_list>       // Returns all the queues for users in user_list

    QUEUE -
    (ALMOND SERVER)
    4.Send DynamicSceneActivated to BACKGROUND_QUEUE
    6.Send Response to All Queues returned in Step 5

    FUNCTIONAL -
    (ALMOND SERVER)
    1.Command 1300
    3.Send DynamicSceneActivatedResponse to Almond

    (BACKGROUND SEVER)
    7.Command 1300

    (MOBILE SERVER)
    1.Command 1300
    2.delete store[unicastID]
    3.Send Res,commandLengthType to Mobile

    Flow - (ALMOND SERVER)
    almondProtocol(packet)-> processor(do)->processor(validate)->almondUsers(execute)->processor(dispatchResponses),processor(sendToBackground)->broadcaster(sendToBackground)->processor(broadcast)->broadcaster(send)

    Flow - (BACKGROUND SERVER)
    socket(packet)->controller(processor)->preprocessor(doNothing)->genericModel(execute)->genericModel(update)

<a name="1300c"></a>
## 20) DynamicSceneUpdated (Command 1300)
    Command no
    1300- JSON format
   
    Required
    Command,UID,CommandType,Payload,AlmondMAC

    SQL -
    8.Insert on AlmondplusDB.SCENE
      params:AlmondMAC

    REDIS -
    (ALMOND SERVER)  
    2.hmset on AL_<AlmondMAC>          // value = [mapper.hashColumn, payload.HashNow]      
    5.hgetall on UID_<user_list>       // Returns all the queues for users in user_list

    QUEUE -
    (ALMOND SERVER)
    4.Send DynamicSceneUpdated to BACKGROUND_QUEUE
    6.Send Response to All Queues returned in Step 5

    FUNCTIONAL - 
    (ALMOND SERVER)
    1.Command 1300
    3.Send DynamicSceneUpdatedResponse to Almond

    (BACKGROUND SERVER)
    7.Command 130

    (MOBILE SERVER)
    9.Command 1300
    10.delete store[unicastID]
    11.Send Res,commandLengthType to Mobile

    Flow - (ALMOND SERVER)
    almondProtocol(packet)->processor(do)->processor(validate)->almondUsers(execute)->processor(dispatchResponses),processor(sendToBackground)->broadcaster(sendToBackground)->processor(broadcast)->broadcaster(send)

    Flow - (BACKGROUND SERVER)
    socket(packet)->controller(processor)->preprocessor(doNothing)->genericModel(execute)->genericModel(update)

<a name="1300d"></a>
## 21) DynamicSceneRemoved (Command 1300)
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
    4.Send DynamicSceneRemoved to BACKGROUND_QUEUE
    6.Send Response to All Queues returned in Step 5

    FUNCTIONAL -
    (ALMOND SERVER)
    1.Command 1300
    3.Send DynamicSceneRemovedResponse to Almond

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

<a name="1300e"></a>
## 22) DynamicAllSceneRemoved (Command 1300)
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
    2.hmset on AL_<AlmondMAC>             // value = [mapper.hashColumn, payload.HashNow]       
    5.hgetall on UID_<user_list>         // Returns all the queues for users in user_list

    QUEUE -
    (ALMOND SERVER)
    4.Send DynamicAllSceneRemoved to BACKGROUND_QUEUE
    6.Send Response to All Queues returned in Step 5

    FUNCTIONAL -
    (ALMOND SERVER)
    1.Command 1300
    3.Send DynamicAllSceneRemovedResponse to Almond

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
## 23) DynamicRuleAdded (Command 1400)
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
    4.Send DynamicRuleAdded to BACKGROUND_QUEUE
    6.Send Response to All Queues returned in Step 5

    FUNCTIONAL -
    (ALMOND SERVER)
    1.Command 1400
    3.Send DynamicRuleAddedResponse to Almond

    (BACKGROUND SERVER)
    7.Command 1400

    (MOBILE SERVER)
    9.Command 1400
    10.delete store[unicastID]
    11.Send Res,commandLengthType to Mobile

    Flow - (ALMOND SERVER)
    almondProtocol(packet)->processor(do)->processor(validate)->almondUsers(execute)->processor(dispatchResponses),processor(sendToBackground)->broadcaster(sendToBackground)->processor(broadcast)->broadcaster(send)

    Flow - (BACKGROUND SERVER)
    consumer(processMessage)->controller(processor)->preprocessor(doNothing)->genericModel(execute),genericModel(add)

<a name="1400b"></a>
## 24) DynamicRuleUpdated (Command 1400)
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
    2.hmset on AL_<AlmondMAC>              // value = [mapper.hashColumn, payload.HashNow]    
    5.hgetall on UID_<user_list>           // Returns all the queues for users in user_list

    QUEUE - 
    (ALMOND SERVER)
    4.Send DynamicRuleUpdated to BACKGROUND_QUEUE
    6.Send Response to All Queues returned in Step 5

    FUNCTIONAL -
    (ALMOND SERVER)
    1.Command 1400
    3.Send DynamicRuleUpdatedResponse to Almond

    (BACKGROUND SERVER)
    7.Command 1400

    (MOBILE SERVER)
    9.Command 1400
    10.delete store[unicastID]
    11.Send Res,commandLengthType to Mobile

    Flow - (ALMOND SERVER)
    almondProtocol(packet)->processor(do)->processor(validate)->almondUsers(execute)->processor(dispatchResponses),processor(sendToBackground)->broadcaster(sendToBackground)->processor(broadcast)->broadcaster(send)

    Flow - (BACKGROUND)
    consumer(processMessage)->controller(processor)->preprocessor(doNothing)->genericModel(execute),genericModel(Update)

<a name="1400c"></a>
## 25) DynamicRuleRemoved (Command 1400)
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

<a name="1400d"></a>
## 26) DynamicAllRulesRemoved (Command 1400)
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
## 27) DynamicClientAdded (Command 1500)
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
## 28) DynamicClientUpdated (Command 1500)
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
## 29) DynamicClientRemoved (Command 1500)
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
## 30) DynamicClientList (Command 1500)
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
    12.delete store[unicastID]
    13.Send Res,commandLengthType to Mobile

    Flow - (ALMOND SERVER)
    almondProtocol(packet)->processor(do)->processor(validate)->almondUsers(execute)->processor(dispatchResponses),processor(sendToBackground)->broadcaster(sendToBackground)->processor(broadcast)->broadcaster(send)

    Flow - (BACKGROUND SERVER)
    socket(packet)->controller(processor)->preprocessor(doNothing)->genericModel(execute)->genericModel(addAll),genericModel(insertBackUpAndUpdate),genericModel(get),cassandra(execute)->genericModel(removeAndInsert)->receive(mainFunction)

<a name="1500e"></a>
## 31) DynamicAllClientsRemoved (Command 1500)
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
## 32) DynamicClientJoined (Command 1500)
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
    4.Send DynamicClientJoined to BACKGROUND_QUEUE
    6.Send Response to All Queues returned in Step 5

    FUNCTIONAL -
    (ALMOND SERVER)
    1.Command 1500
    3.Send DynamicClientJoinedResponse to Almond

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

<a name="1500g"></a>
## 33) DynamicClientLeft (Command 1500)
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
    4.Send DynamicClientLeft to BACKGROUND_QUEUE
    6.Send Response to All Queues returned in Step 5

    FUNCTIONAL -
    (ALMOND SERVER)
    1.Command 1500
    3.Send DynamicClientLeftResponse to Almond

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
