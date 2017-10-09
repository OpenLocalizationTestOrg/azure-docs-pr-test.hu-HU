---
title: "aaaGet Azure hibrid kapcsolatok csomópontban használatába |} Microsoft Docs"
description: "Node.js-konzolalkalmazást hozhat létre a hibrid Azure Relay-kapcsolatokhoz."
services: service-bus-relay
documentationcenter: node
author: sethmanheim
manager: timlt
editor: 
ms.assetid: e44e4867-3cf3-46be-8f8a-7671e2013bc4
ms.service: service-bus-relay
ms.devlang: tbd
ms.topic: get-started-article
ms.tgt_pltfrm: node
ms.workload: na
ms.date: 07/07/2017
ms.author: sethm
ms.openlocfilehash: 235548399570074f7fd160fec28de8d3633625c5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-relay-hybrid-connections"></a>Ismerkedés a hibrid Relay-kapcsolatokkal

[!INCLUDE [relay-selector-hybrid-connections](../../includes/relay-selector-hybrid-connections.md)]

Ez az oktatóanyag bemutatja túl[Azure hibrid kapcsolatok](relay-what-is-it.md#hybrid-connections), és bemutatja, hogyan toouse Node.js toocreate egy ügyfél-alkalmazás által küldött üzenetek tooa megfelelő figyelő alkalmazás. 

## <a name="what-will-be-accomplished"></a>Az oktatóanyag célja

A hibrid kapcsolatokhoz egy ügyfélre és egy kiszolgáló-összetevőre is szükség van, így ebben az oktatóanyagban két konzolalkalmazást hozunk létre. Az alábbiakban hello lépéseket:

1. Hello Azure-portál használatával, továbbító névtér létrehozása.
2. Hozzon létre egy hibrid kapcsolat hello Azure-portál használatával.
3. A kiszolgáló konzol alkalmazás tooreceive üzenetet írni.
4. Egy ügyfél konzol alkalmazás toosend üzenetet írni.

## <a name="prerequisites"></a>Előfeltételek

1. [Node.js](https://nodejs.org/en/).
2. Azure-előfizetés.

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="1-create-a-namespace-using-hello-azure-portal"></a>1. Hello Azure-portál használatával névtér létrehozása

Ha már létrehozott egy továbbító névtér, jump toohello [hello Azure portál használata hibrid kapcsolat létrehozása](#2-create-a-hybrid-connection-using-the-azure-portal) szakasz.

[!INCLUDE [relay-create-namespace-portal](../../includes/relay-create-namespace-portal.md)]

## <a name="2-create-a-hybrid-connection-using-hello-azure-portal"></a>2. A hibrid kapcsolat létrehozása használatával hello Azure-portálon

Ha már rendelkezik egy hibrid kapcsolat létrehozása, jump toohello [hozzon létre egy kiszolgálói alkalmazás](#3-create-a-server-application-listener) szakasz.

[!INCLUDE [relay-create-hybrid-connection-portal](../../includes/relay-create-hybrid-connection-portal.md)]

## <a name="3-create-a-server-application-listener"></a>3. Kiszolgálói alkalmazás (figyelő) létrehozása

toolisten és üzenetek fogadása hello továbbító, a Node.js-Konzolalkalmazás írja azt.

[!INCLUDE [relay-hybrid-connections-node-get-started-server](../../includes/relay-hybrid-connections-node-get-started-server.md)]

## <a name="4-create-a-client-application-sender"></a>4. Ügyfélalkalmazás létrehozása (küldő)

toosend üzenetek toohello továbbítási, azt fogja írni a Node.js-Konzolalkalmazás.

[!INCLUDE [relay-hybrid-connections-node-get-started-client](../../includes/relay-hybrid-connections-node-get-started-client.md)]

## <a name="5-run-hello-applications"></a>5. Hello alkalmazások futtatásához

1. Hello server alkalmazás futtatásához: Node.js parancssori típusból `node listener.js`.
2. Hello ügyfélalkalmazás futtatása: a Node.js parancssori típusból `node sender.js`, és adjon meg szöveget.
3. Győződjön meg arról, hogy hello kiszolgálón alkalmazás konzol kimenetek hello szöveg hello ügyfélalkalmazás megadott.

![running-applications](./media/relay-hybrid-connections-node-get-started/running-applications.png)

Gratulálunk, végpontok közötti hibrid kapcsolatok alkalmazást hozott létre a Node.js használatával.

## <a name="next-steps"></a>Következő lépések:

* [Relay – gyakori kérdések](relay-faq.md)
* [Névtér létrehozása](relay-create-namespace-portal.md)
* [Ismerkedés a .NET-tel](relay-hybrid-connections-dotnet-get-started.md)
* [Bevezetés a Node használatába](relay-hybrid-connections-node-get-started.md)

