---
title: "aaaAzure továbbítási API áttekintése |} Microsoft Docs"
description: "Elérhető Azure továbbítási API-k – áttekintés"
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: fdaa1d2b-bd80-4e75-abb9-0c3d0773af2d
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/03/2017
ms.author: sethm
ms.openlocfilehash: 3c4d737d5fee9a8babce094fa6dfddb28910834b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="available-relay-apis"></a>Rendelkezésre álló továbbítási API-k

## <a name="runtime-apis"></a>Futásidejű API-k

hello következő táblázatban minden jelenleg elérhető továbbítási futásidejű ügyfelek.

Hello [további információt](#additional-information) szakasz mindegyik futásidejű kódtár hello állapotával kapcsolatos további információkat tartalmaz.

| Nyelvi/Platform | A szolgáltatás elérhető | Ügyfélcsomag | Tárház |
| --- | --- | --- | --- |
| .NET-szabvány | Hibrid kapcsolatok | [Microsoft.Azure.Relay](https://www.nuget.org/packages/Microsoft.Azure.Relay/) | [GitHub](https://github.com/azure/azure-relay-dotnet) |
| .NET-keretrendszer | WCF-továbbító | [WindowsAzure.ServiceBus](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) | N/A |
| Csomópont | Hibrid kapcsolatok | [`hyco-ws`](https://www.npmjs.com/package/hyco-ws)<br/>[`hyco-websocket`](https://www.npmjs.com/package/hyco-websocket) | [GitHub](https://github.com/Azure/azure-relay-node) |

### <a name="additional-information"></a>További információ

#### <a name="net"></a>.NET
hello .NET ökoszisztéma több futtatókörnyezetek rendelkezik, ezért van több .NET-kódtárakra az Event Hubs számára. hello .NET szabványos függvénytár is futtatható, használja a .NET Core vagy a .NET-keretrendszer hello közben hello .NET Framework könyvtár csak a .NET-keretrendszer környezetben futtatható. További információ a .NET-keretrendszer: [keretrendszer-verziók](/dotnet/articles/standard/frameworks#framework-versions).

## <a name="next-steps"></a>Következő lépések
További információ az Azure továbbítási toolearn látogasson el ezeket a hivatkozásokat:
* [Mi az az Azure Relay?](relay-what-is-it.md)
* [Relay – gyakori kérdések](relay-faq.md)