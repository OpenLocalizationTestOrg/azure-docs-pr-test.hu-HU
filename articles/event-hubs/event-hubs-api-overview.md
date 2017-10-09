---
title: "aaaAzure Event Hubs API – áttekintés |} Microsoft Docs"
description: "Elérhető Azure Event Hubs API-kat áttekintése"
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 3f221a0c-182d-4e39-9f3d-3a3c16c5c6ed
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/15/2017
ms.author: sethm
ms.openlocfilehash: 46dfcc544ff92642cfd7a967f9ec38a0d8e2bd5d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="available-event-hubs-apis"></a>Rendelkezésre álló Event Hubs API-k

## <a name="runtime-apis"></a>Futásidejű API-k

hello minden jelenleg elérhető Azure Event Hubs futásidejű ügyfelek leírása látható. Ezek a könyvtárak is közé korlátozott felügyeleti funkciók, amíg vannak is [adott könyvtárakat](#management-apis) dedikált toomanagement műveletek. Ezeket a könyvtárakat hello core fókusz toosend és üzenetek fogadása egy eseményközpontba.

Lásd: [további információt](#additional-information) hello aktuális állapota minden egyes futásidejű kódtár olvashat.

| Nyelvi/Platform | Ügyfélcsomag | EventProcessorHost csomag | Tárház |
| --- | --- | --- | --- |
| .NET-szabvány | [NuGet](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) | [NuGet](https://www.nuget.org/packages/Microsoft.Azure.EventHubs.Processor/) | [GitHub](https://github.com/azure/azure-event-hubs-dotnet) |
| .NET-keretrendszer | [NuGet](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) | [NuGet](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/) | N/A |
| Java | [Maven 3](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22) | [Maven 3](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs-eph%22) | [GitHub](https://github.com/Azure/azure-event-hubs-java) |
| Csomópont | [NPM](https://www.npmjs.com/package/azure-event-hubs) | N/A | [GitHub](https://github.com/Azure/azure-event-hubs-node) |
| C# | N/A | N/A | [GitHub](https://github.com/Azure/azure-event-hubs-c) |

### <a name="additional-information"></a>További információ

#### <a name="net"></a>.NET
hello .NET ökoszisztéma több futtatókörnyezetek rendelkezik, ezért van több .NET-kódtárakra az Event Hubs számára. hello .NET szabványos függvénytár is futtatható, használja a .NET Core vagy a .NET-keretrendszer hello közben hello .NET Framework könyvtár csak a .NET-keretrendszer környezetben futtatható. További információ a .NET-keretrendszer: [keretrendszer-verziók](https://docs.microsoft.com/dotnet/articles/standard/frameworks#framework-versions).

#### <a name="node"></a>Csomópont

hello Node.js kódtár jelenleg előzetes verzióban érhetők, és a Microsoft alkalmazottai és külső közreműködő szerepkörrel rendelkező személyek által kezelt oldal projektként. Beleértve az adatforrás összes hozzájárulást üdvözlő és felül kell vizsgálni.

## <a name="management-apis"></a>API-val

hello minden jelenleg elérhető felügyeleti adott könyvtárak listája látható. Ezeket a könyvtárakat nincs futásidejű műveletek tartalmazhat, és a hello azzal a céllal, az Event Hubs entitások kezelése.

| Nyelvi/Platform | Felügyeleti csomag | Tárház |
| --- | --- | --- | --- |
| .NET-szabvány | [NuGet](https://www.nuget.org/packages/Microsoft.Azure.Management.EventHub) | [GitHub](https://github.com/Azure/azure-sdk-for-net/tree/AutoRest/src/ResourceManagement/EventHub) |

## <a name="next-steps"></a>Következő lépések
További információ az Event Hubs érhetők el a következő hivatkozások hello:

* [Event Hubs – áttekintés](event-hubs-what-is-event-hubs.md)
* [Eseményközpont létrehozása](event-hubs-create.md)
* [Event Hubs – gyakori kérdések](event-hubs-faq.md)