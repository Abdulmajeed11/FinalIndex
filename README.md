# Server
### Table of contents 
- [Command 1061](#1061)
- [Command 1063](#1063)
- [Command 1100](#1100)  (Mobile to Almond)
- [Command 1100](#1100)  (Almond to Mobile)

#### Note: In Commands, MS = MOBILE SERVER, AS = ALMOND SERVER, BS = BACKGROUND SERVER

<a name="1061"></a>
## 1) Command 1061
    Command no 
    1061- JSON format
 
    Required 
    Command,CommandType,Payload,almondMAC

    Redis
    3.hgetall on AL_<AlmondMAC>      //(AlmondMAC = <packet.parsedPayload.AlmondMAC>)    (MS)
    4.get on ICID_<string>           // here <string> = random string data)              (MS)
    5.setex on ICID_<string>        // (prefix + key), value = SERVER_NAME               (MS)
    10.get on ICID_<result.unicastID>         (ALMOND SERVER)

    Queue
    7.Send response to config.SERVER_NAME   /*(payload,command,almondMAC) to queue*/ (MOBILE SERVER)
    11.Send Response to queue           // (ALMOND SERVER)                                   
    
    Functional 
    1.Command 1061             (MOBILE SERVER)
    2.Send listResponse,commandLengthType ToMobile  //where listResponse = payload   (MOBILE SERVER)
    6.delete store[commandID]    (MOBILE SERVER)
    8.Command 1062               (ALMOND SERVER)
    9.Send Res,CommandLengthType to Almond       (ALMOND SERVER)
    12.Append result.almondMAC to offlineMACS.txt       (ALMOND SERVER)

    Flow - (MOBILE SERVER)
    socket(packet)->validator(do)->processor(do)->almond(onlyUnicast)->dispatcher(dispatchResponse)->dispatcher(unicast)->broadcaster(unicast)

<a name="1063"></a>
## 2) Command 1063
    Command no 
    1063- JSON format
 
    Required 
    Command,CommandType,ICID,Payload

    Redis
    2.Get ICID_<packet.ICID>             (ALMOND SERVER)

    Queue
    3.send Response to queue        (ALMOND SERVER)

    Functional 
    1.Command 1063              (ALMOND SERVER)
    4.Command 1064              (MOBILE SERVER)
    5.delete store[unicastID]    (MOBILE SERVER)
    6.Send Res,commandLengthType to Mobile       (MOBILE SERVER)

    FLow - (ALMOND SERVER)
    almondProtocol(packet)-> processor(do)->processor(validate)->almondUsers(dummyModel)->processor(dispatchResponses),processor(unicast)->broadcaster(unicast)

<a name="1100"></a>
## 3) Command 1100
    Command no 
    1100- JSON format
 
    Required 
    Command,CommandType,Payload,almondMAC

    Redis
    3.hgetall on AL_<AlmondMAC>          //(AlmondMAC = <packet.parsedPayload.AlmondMAC>)   (MS)
    4.get on ICID_<string>              // here <string> = random string data)      (MS)
    5.setex on ICID_<string>            // (prefix + key), value = SERVER_NAME      (MS)
    10.get on ICID_<result.unicastID>       (ALMOND SERVER)

    Queue
    7.Send response to config.SERVER_NAME        // (payload,command,almondMAC) to queue   (MS)
    11.Send Response to queue           (ALMOND SERVER)

    Functional 
    1.Command 1100           (MOBILE SERVER)
    2.Send listResponse,commandLengthType ToMobile      //where listResponse = payload    (MS)
    6.delete store[commandID]               (MOBILE SERVER)
    8.Command 1100            (ALMOND SERVER)
    9.Send Res,CommandLengthType to Almond       (ALMOND SERVER)
    12.Append result.almondMAC to offlineMACS.txt       (ALMOND SERVER)

    Flow- (MOBILE SERVER)
    socket(packet)->validator(do)->processor(do)->almond(onlyUnicast)->dispatcher(dispatchResponse)->dispatcher(unicast)->broadcaster(unicast)

<a name="1100"></a>
## 4) Command 1100
    Command no 
    1100- JSON format
 
    Required 
    Command,CommandType,ICID,Payload

    Redis
    2.Get ICID_<packet.ICID>              (ALMOND SERVER)

    Queue
    3.send Response to queue             (ALMOND SERVER)

    Functional 
    1.Command 1100         (ALMOND SERVER)
    4.Command 153          (MOBILE SERVER)
    5.delete store[unicastID]    (MOBILE SERVER)
    6.Send Res,commandLengthType to Mobile    (MOBILE SERVER)


    FLow - (ALMOND SERVER)
    almondProtocol(packet)-> processor(do)->processor(validate)->almondUsers(dummyModel)->processor(dispatchResponses),processor(unicast)->broadcaster(unicast)
