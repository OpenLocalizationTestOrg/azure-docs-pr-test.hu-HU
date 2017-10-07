---
title: a Media Services SDK for .NET hello aaaRetry logika |} Microsoft Docs
description: "hello témakör áttekintést nyújt az újrapróbálkozási logika a Media Services SDK hello a .NET-hez."
author: Juliako
manager: cfowler
editor: 
services: media-services
documentationcenter: 
ms.assetid: 527b61a6-c862-4bd8-bcbc-b9aea1ffdee3
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: juliako
ms.openlocfilehash: 18d0a9d68e55a48bc769fb6ae5711ddba78ed8e6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="retry-logic-in-hello-media-services-sdk-for-net"></a>Újrapróbálkozási logika a Media Services SDK hello a .NET-hez
Átmeneti akkor fordulhat elő, az Microsoft Azure-szolgáltatások használatakor. Ha egy átmeneti hiba akkor fordul elő, a legtöbb esetben néhány újrapróbálkozás után hello művelet sikeres lesz. Media Services SDK for .NET hello hello újrapróbálkozási logika toohandle átmeneti hibák kivételeket és webes kérelmek lekérdezések mentése érintő változtatások, valamint tárolási műveletek végrehajtása miatt fellépő hibák társított valósítja meg.  Alapértelmezés szerint a Media Services SDK for .NET hello négy újrapróbálkozási lehetőségbe végrehajtja a hello kivétel tooyour alkalmazás újbóli kiváltása előtt. az alkalmazás hello kód majd kezelni kell ezt a kivételt megfelelően.  

 hello alábbiakban egy webes kérelem, a tárolás, a lekérdezés és a SaveChanges metódus házirendek rövid eszközöket:  

* blob storage műveletek (feltöltések vagy az eszköz fájlok letöltésére) hello tárolt házirend használható.  
* hello webkérelem-házirend általános webes kérelmek (például egy hitelesítési token és a megoldásának hello felhasználók fürt végpont) használható.  
* Lekérdezési szabály hello használt REST (például mediaContext.Assets.Where(...)) az entitás lekérdezése.  
* a SaveChanges metódus házirend hello bármi, amely módosítja az adatokat (például egy entitás hívja service függvény művelet frissítése entitás létrehozása) hello szolgáltatáson belül használható.  
  
  Ez a témakör felsorolja a kivétel típusa és hibakódok .NET-keretrendszerhez készült Media Services SDK hello által végrehajtott újrapróbálkozási logika.  

## <a name="exception-types"></a>Kivétel típusa
hello következő táblázat ismerteti kivételek, hogy a .NET-leírók Media Services SDK hello, vagy nem kezeli az egyes műveletek, amelyek átmeneti okozhatnak.  

| Kivétel | Webes kérelem | Storage | Lekérdezés | A SaveChanges metódus |
| --- | --- | --- | --- | --- |
| WebException<br/>További információkért lásd: hello [WebException állapotkódok](media-services-retry-logic-in-dotnet-sdk.md#WebExceptionStatus) szakasz. |Igen |Igen |Igen |Igen |
| DataServiceClientException<br/> További információkért lásd: [HTTP-állapotkódok hiba](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode). |Nem |Igen |Igen |Igen |
| DataServiceQueryException<br/> További információkért lásd: [HTTP-állapotkódok hiba](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode). |Nem |Igen |Igen |Igen |
| DataServiceRequestException<br/> További információkért lásd: [HTTP-állapotkódok hiba](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode). |Nem |Igen |Igen |Igen |
| DataServiceTransportException |Nem |Nem |Igen |Igen |
| TimeoutException |Igen |Igen |Igen |Nem |
| SocketException |Igen |Igen |Igen |Igen |
| StorageException |Nem |Igen |Nem |Nem |
| Ioexception kivétel |Nem |Igen |Nem |Nem |

### <a name="WebExceptionStatus"></a>WebException állapotkódok
hello következő táblázatban WebException hibajelentésben kódok hello újrapróbálkozási logika van megvalósítva. Hello [WebExceptionStatus](http://msdn.microsoft.com/library/system.net.webexceptionstatus.aspx) számbavételi hello állapotkódok határozza meg.  

| status | Webes kérelem | Storage | Lekérdezés | A SaveChanges metódus |
| --- | --- | --- | --- | --- |
| ConnectFailure |Igen |Igen |Igen |Igen |
| NameResolutionFailure |Igen |Igen |Igen |Igen |
| ProxyNameResolutionFailure |Igen |Igen |Igen |Igen |
| SendFailure |Igen |Igen |Igen |Igen |
| PipelineFailure |Igen |Igen |Igen |Nem |
| ConnectionClosed |Igen |Igen |Igen |Nem |
| KeepAliveFailure |Igen |Igen |Igen |Nem |
| UnknownError |Igen |Igen |Igen |Nem |
| ReceiveFailure |Igen |Igen |Igen |Nem |
| RequestCanceled |Igen |Igen |Igen |Nem |
| Időtúllépés |Igen |Igen |Igen |Nem |
| Protokollhiba lépett <br/>Protokollhiba lépett hello kísérletek hello HTTP-állapot kód kezelési vezérli. További információkért lásd: [HTTP-állapotkódok hiba](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode). |Igen |Igen |Igen |Igen |

### <a name="HTTPStatusCode"></a>HTTP-állapotkódok hiba
Ha a lekérdezés és a SaveChanges metódus műveletek throw DataServiceClientException, DataServiceQueryException vagy DataServiceQueryException, hello HTTP-állapotkód hiba eredmény abban az esetben hello StatusCode tulajdonság.  hello következő táblázatban a hibakódok hello újrapróbálkozási logika van megvalósítva.  

| status | Webes kérelem | Storage | Lekérdezés | A SaveChanges metódus |
| --- | --- | --- | --- | --- |
| 401 |Nem |Igen |Nem |Nem |
| 403 |Nem |Igen<br/>Kezeli a hosszabb ideig vár az újrapróbálkozások. |Nem |Nem |
| 408 |Igen |Igen |Igen |Igen |
| 429 |Igen |Igen |Igen |Igen |
| 500 |Igen |Igen |Igen |Nem |
| 502 |Igen |Igen |Igen |Nem |
| 503 |Igen |Igen |Igen |Igen |
| 504 |Igen |Igen |Igen |Nem |

Ha szeretne egy pillantást hello Media Services SDK tényleges végrehajtásának hello tootake .NET újrapróbálkozási logika, lásd: [azure-sdk-az-media-services](https://github.com/Azure/azure-sdk-for-media-services/tree/dev/src/net/Client/TransientFaultHandling).

## <a name="next-steps"></a>Következő lépések
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

