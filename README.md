# Server
### Table of contents 
- [AddRule,UpdateRule,RemoveRule,RemoveAllRules,ValidateRule,GetDeviceIndex,UpdateDeviceName,
UpdateAlmondName,DeviceOnlineCheck,UpdateClient,RemoveClient,RemoveAllClients,WifiClients,
AddScene,SetScene,ActivateScene,DeleteScene,DeleteAllScene (Command 1061)](#1061)
- [UpdateDeviceIndex,UpdateDeviceName,AddScene,ActiveScene,UpdateScene,RemoveScene,RemoveAlllScenes,
AddRule,ValidateRule,UpdateRule,RemoveRule,RemoveAllRules,UpdateClient,RemoveClient (Command 1063 (Command 1063)](#1063)
- [RouterSummary,GetWirelessSettings,SetWirelessSettings,RebootRouter,SendLogs,FirmwareUpdate (Command 1100)](#1100a)  (Mobile to Almond)
- [RouterSummary,GetWirelessSettings,SetWirelessSettings,RebootRouter,SendLogs,FirmwareUpdate (Command 1100)](#1100b)  (Almond to Mobile)
- [AffiliationUserRequest (Command 23)](#23)
- [AffiliationAlmondComplete (Command 25)](#25)
- [AlmondModeChange (Command 63)](#63a)
- [AlmondNameChange (Command 63)](#63b)
- [AlmondProperties (Command 1050)](#1050) 
- [DynamicAlmondNameChange (Command 49)](#49)
- [DynamicAlmondModeChange (Command 153)](#153)
- [CloudReset (Command 8)](#8)

#### Note: In Commands, MS = MOBILE SERVER, AS = ALMOND SERVER, BS = BACKGROUND SERVER

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
## 3) Command 1100
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
## 4) Command 1100
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
## 9) AlmondProperties (Command 1050)
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
