---
title: "aaaCommon FabricClient kivételek |} Microsoft Docs"
description: "Hello közös kivételeket és az alkalmazás és a fürt felügyeleti műveletek során hello FabricClient API-k által is okozott hibák ismerteti."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: bb821313-b221-479f-b08e-36cf07e60a07
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/01/2017
ms.author: ryanwi
ms.openlocfilehash: 55bb556b25150524ebc28756eb1bd3e91dc37853
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="common-exceptions-and-errors-when-working-with-hello-fabricclient-apis"></a>Közös kivételeket és hibák az hello FabricClient API-k használatakor
Hello [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient#System_Fabric_FabricClient) API-k engedélyezése a fürt és a alkalmazás rendszergazdák tooperform felügyeleti feladatok a Service Fabric-alkalmazás, szolgáltatás vagy fürt. Például alkalmazás központi telepítése, frissítése és eltávolítási, hello állapotát a fürt ellenőrzése, vagy egy szolgáltatás tesztelése. Alkalmazásfejlesztők és a fürt rendszergazdák eszközeivel hello FabricClient API-k toodevelop hello Service Fabric-fürt és alkalmazásokat kezeléséhez.

Nincsenek számos különböző típusú műveleteket, amelyek FabricClient használatával végezheti el.  Az egyes módszerek is throw kivételek tooincorrect bemeneti, futásidejű hibák vagy átmeneti infrastruktúra problémák miatt hibák.  Lásd: hello API referencia dokumentáció toofind mely kivételek egy adott módszerrel. Bizonyos kivételek vannak, azonban számos által is okozhat, amelyek különböző [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient#System_Fabric_FabricClient) API-k. hello következő táblázatban hello kivételek hello FabricClient API-k által közösen használt.

| Kivétel | Mikor történt |
| --- |:--- |
| [System.Fabric.FabricObjectClosedException](https://docs.microsoft.com/dotnet/api/system.fabric.fabricobjectclosedexception#System_Fabric_FabricObjectClosedException) |Hello [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient#System_Fabric_FabricClient) objektum zárt állapotban van. Hello rendelkezési [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient#System_Fabric_FabricClient) objektum használja, és a hozható létre egy új [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient#System_Fabric_FabricClient) objektum. |
| [System.TimeoutException](https://docs.microsoft.com/dotnet/core/api/system.timeoutexception#System_TimeoutException) |hello művelet túllépte az időkorlátot. [OperationTimedOut](https://docs.microsoft.com/dotnet/api/system.fabric.fabricerrorcode#System_Fabric_FabricErrorCode) ad vissza, ha több, mint a konfigurált MaxOperationTimeout toocomplete hello művelet vesz igénybe. |
| [System.UnauthorizedAccessException](https://docs.microsoft.com/dotnet/core/api/system.unauthorizedaccessexception#System_UnauthorizedAccessException) |hozzáférés-ellenőrzés hello hello a művelethez nem sikerült. E_ACCESSDENIED adja vissza. |
| [System.Fabric.FabricException](https://docs.microsoft.com/dotnet/api/system.fabric.fabricexception#System_Fabric_FabricException) |Futási hiba történt a hello művelet végrehajtása közben. Hello FabricClient módszereket potenciálisan throw [FabricException](https://docs.microsoft.com/dotnet/api/system.fabric.fabricexception#System_Fabric_FabricException), hello [ErrorCode](https://docs.microsoft.com/dotnet/api/system.fabric.fabricexception#System_Fabric_FabricException_ErrorCode) tulajdonság jelzi hello kivétel hello pontos okát. Hibakódok definiált hello [FabricErrorCode](https://docs.microsoft.com/dotnet/api/system.fabric.fabricerrorcode#System_Fabric_FabricErrorCode) enumerálása. |
| [System.Fabric.FabricTransientException](https://docs.microsoft.com/dotnet/api/system.fabric.fabrictransientexception#System_Fabric_FabricTransientException) |hello művelet valamilyen állapotának tooa átmeneti hiba miatt sikertelen volt. Egy művelet például meghiúsulhat, mert replikák másodlagosak átmenetileg nem érhető el. Átmeneti kivételek toofailed követően újra megkísérelhető a műveletek megegyeznek. |

Néhány gyakori [FabricErrorCode](https://docs.microsoft.com/dotnet/api/system.fabric.fabricerrorcode#System_Fabric_FabricErrorCode) a hibákat, a adhatók vissza a [FabricException](https://docs.microsoft.com/dotnet/api/system.fabric.fabricexception#System_Fabric_FabricException):

| Hiba | Az állapot |
| --- |:--- |
| CommunicationError |Kommunikációs hiba miatt hello művelet toofail újrapróbálkozási hello műveletet. |
| InvalidCredentialType |hello hitelesítőadat-típus érvénytelen. |
| InvalidX509FindType |hello X509FindType érvénytelen. |
| InvalidX509StoreLocation |hello X509 tárolási helye érvénytelen. |
| InvalidX509StoreName |hello X509 tároló neve érvénytelen. |
| InvalidX509Thumbprint |hello X509 tanúsítvány ujjlenyomata karakterlánc érvénytelen. |
| InvalidProtectionLevel |hello védelmi szint érvénytelen. |
| InvalidX509Store |nem lehet megnyitni a tanúsítványtárolót hello X509. |
| InvalidSubjectName |hello tulajdonos neve érvénytelen. |
| InvalidAllowedCommonNameList |köznapi név lista karakterlánc hello formátuma érvénytelen. Vesszővel tagolt listájának kell lennie. |

