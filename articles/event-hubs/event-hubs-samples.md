---
title: "az Event Hubs minták aaaAzure |} Microsoft Docs"
description: "Azure Event Hubs-minták"
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/15/2017
ms.author: sethm
ms.openlocfilehash: f01f52e6c13f9e885999a6726143440bbc70446d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="event-hubs-samples"></a>Event Hubs – minták 

hello Azure Event Hubs kódmintát bemutatja a legfontosabb jellemzők [Azure Event Hubs](/azure/event-hubs/). Ez a cikk kategorizálja, és elérhető, hivatkozások tooeach hello mintákat ismerteti.

Az Event Hubs minták írásának hello időpontban számos különböző helyeken találhatók:

- [MSDN fejlesztői mintakódok](https://code.msdn.microsoft.com/site/search?query=event%20hubs&f%5B0%5D.Value=event%20hubs&f%5B0%5D.Type=SearchText&ac=5)
- [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples)

Hello .NET-keretrendszer különböző verzióival kapcsolatos további információkért lásd: [keretrendszerek és célok](/dotnet/articles/standard/frameworks).

Több minta lesz hozzáadva adott idő alatt, ezért vissza ide gyakran frissítések keresése.

## <a name="net-standard"></a>.NET-szabvány

hello következő mintákat bemutatják, hogyan toosend és hello eseményeket fogadni [Event Hubs-ügyfél](https://github.com/Azure/azure-event-hubs-dotnet/blob/master/readme.md) a hello [.NET szabványos függvénytár](/dotnet/articles/standard/library).

### <a name="send-events"></a>Események küldése 

Hello [Ismerkedés küldése](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender) a példa bemutatja, hogyan toowrite a .NET Core Konzolalkalmazás által küldött események tooan eseményközpontot.

### <a name="receive-events"></a>Események fogadása 

Hello [Ismerkedés fogadását az Event Processor Host hello](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleEphReceiver) minta a .NET Core-Konzolalkalmazás, amely üzeneteket fogad az eseményközpontok hello Event Processor Host használatával.

## <a name="net-framework"></a>.NET-keretrendszer   

Azure Event hubs hello célzó különböző más funkciókat fogunk bemutatni, ezeket a mintákat [.NET Framework könyvtár](/dotnet/framework/index).
 
### <a name="notify-users-of-events-received"></a>Értesítse a felhasználókat a fogadott események

Hello [AppToNotifyUsers](https://github.com/Azure-Samples/event-hubs-dotnet-user-notifications) minta értesíti a felhasználókat az érzékelők vagy más rendszerek érkező adatokat.

### <a name="get-started-with-event-hubs"></a>Bevezetés az Event Hubs használatába 

Hello [Event Hubs használatának első lépéseit](https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097) hello alapszintű Event Hubs, például hogyan toocreate eseményközpontban, tooan eseményközpont események küldése és hello segítségével események felhasználásához képességeit mutatja be [Event Processor Host](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/).

### <a name="scale-out-event-processing"></a>Horizontális felskálázás esemény feldolgozása 

Hello [bővített kapacitású esemény feldolgozása](https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3) minta bemutatja, hogyan toouse hello [Event Processor Host](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/) toodistribute hello munkaterhelés Event Hubs adatfolyam-fogyasztás. Azt illusztrálja, hogyan tooimplement hello **EventProcessor** és **EventProcessorFactory** objektumok toomanage hello eseményfelhasználó. 

###  <a name="pull-data-from-sql-into-an-event-hub"></a>SQL eseményközpontnak olvasnak be adatokat

Hello [húzza SQL adatok](https://github.com/Azure-Samples/event-hubs-dotnet-import-from-sql) a példa bemutatja, hogyan toopull adatait egy SQL táblázat, és leküldeni tooan eseményközpontot, az alárendelt analitikus alkalmazásokban bemenetként toouse.

### <a name="pull-web-data-into-an-event-hub"></a>Lekéréses webhely adatok eseményközpontba 

Hello [adatokat importálhat hello webes](https://github.com/Azure-Samples/event-hubs-dotnet-importfromweb) a példa bemutatja, hogyan toopull adatait a nyilvánosság elől hírcsatornák (például egy részleg a szállítására a forgalmi információk hírcsatorna hello), és leküldeni tooan eseményközpontot.

## <a name="next-steps"></a>Következő lépések

További tudnivalók a .NET-keretrendszer-verziók érhetők el a következő hivatkozások hello:

- [Keretrendszerek és célok](/dotnet/articles/standard/frameworks)
- [.NET-keretrendszer 4.6 és 4.5](/dotnet/framework/index)

További információ az Event Hubs a következő cikkek hello:

- [Event Hubs – áttekintés](event-hubs-what-is-event-hubs.md)
- [Eseményközpont létrehozása](event-hubs-create.md)
- [Event Hubs – gyakori kérdések](event-hubs-faq.md)