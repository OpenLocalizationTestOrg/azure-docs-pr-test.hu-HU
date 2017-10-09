---
title: "aaaGet Azure hibrid kapcsolatok a .NET használatába |} Microsoft Docs"
description: "C#-konzolalkalmazást hozhat létre a hibrid Azure Relay-kapcsolatokhoz."
services: service-bus-relay
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: d1386900-b942-4abf-acfc-38d2ef826253
ms.service: service-bus-relay
ms.devlang: tbd
ms.topic: get-started-article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 07/07/2017
ms.author: sethm
ms.openlocfilehash: 1e4af28e7cd4393c8ca965a149a0b83ebcc44f22
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-relay-hybrid-connections"></a>Ismerkedés a hibrid Relay-kapcsolatokkal
[!INCLUDE [relay-selector-hybrid-connections](../../includes/relay-selector-hybrid-connections.md)]

Ez az oktatóanyag bemutatja túl[Azure hibrid kapcsolatok](relay-what-is-it.md#hybrid-connections), és bemutatja, hogyan toouse .NET toocreate egy ügyfél-alkalmazás által küldött üzenetek tooa megfelelő figyelő alkalmazás. 

## <a name="what-will-be-accomplished"></a>Az oktatóanyag célja
Hibrid kapcsolatok egy ügyfél és kiszolgáló-összetevők van szükség, mert a hello oktatóanyag két konzol alkalmazást hoz létre. Az alábbiakban hello lépéseket:

1. Hello Azure-portál használatával, továbbító névtér létrehozása.
2. A hibrid kapcsolat létrehozása az adott névtérben, hello Azure-portál használatával.
3. A kiszolgáló (figyelő) konzol alkalmazás tooreceive üzenetet írni.
4. Egy ügyfélkonzol (küldő) alkalmazás toosend üzenetet írni.

## <a name="prerequisites"></a>Előfeltételek

toocomplete ebben az oktatóanyagban szüksége lesz a következő előfeltételek hello:

1. [Visual Studio 2015 vagy újabb](http://www.visualstudio.com). az oktatóanyagban szereplő példák hello használja a Visual Studio 2017.
2. Azure-előfizetés.

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="1-create-a-namespace-using-hello-azure-portal"></a>1. Hello Azure-portál használatával névtér létrehozása
Ha már létrehozott egy továbbító névtér, jump toohello [hello Azure portál használata hibrid kapcsolat létrehozása](#2-create-a-hybrid-connection-using-the-azure-portal) szakasz.

[!INCLUDE [relay-create-namespace-portal](../../includes/relay-create-namespace-portal.md)]

## <a name="2-create-a-hybrid-connection-using-hello-azure-portal"></a>2. A hibrid kapcsolat létrehozása használatával hello Azure-portálon
Ha már létrehozott egy hibrid kapcsolat, jump toohello [hozzon létre egy kiszolgálói alkalmazás](#3-create-a-server-application-listener) szakasz.

[!INCLUDE [relay-create-hybrid-connection-portal](../../includes/relay-create-hybrid-connection-portal.md)]

## <a name="3-create-a-server-application-listener"></a>3. Kiszolgálói alkalmazás (figyelő) létrehozása
toolisten és üzeneteket fogadni a hello továbbítási, azt fog kiírni, a C# konzolalkalmazást a Visual Studio használatával.

[!INCLUDE [relay-hybrid-connections-dotnet-get-started-server](../../includes/relay-hybrid-connections-dotnet-get-started-server.md)]

## <a name="4-create-a-client-application-sender"></a>4. Ügyfélalkalmazás létrehozása (küldő)
toosend üzenetek toohello továbbítják, azt fogja írni egy C#-konzolalkalmazást a Visual Studio használatával.

[!INCLUDE [relay-hybrid-connections-dotnet-get-started-client](../../includes/relay-hybrid-connections-dotnet-get-started-client.md)]

## <a name="5-run-hello-applications"></a>5. Hello alkalmazások futtatásához
1. Futtassa a hello server alkalmazást.
2. Hello ügyfélalkalmazás futtatása, és adjon meg szöveget.
3. Győződjön meg arról, hogy hello kiszolgálón alkalmazás konzol kimenetek hello szöveg hello ügyfélalkalmazás megadott.

![running-applications](./media/relay-hybrid-connections-dotnet-get-started/running-applications.png)

Gratulálunk, végpontok közötti hibrid kapcsolatok alkalmazást hozott létre.

## <a name="next-steps"></a>Következő lépések:
* [Relay – gyakori kérdések](relay-faq.md)
* [Névtér létrehozása](relay-create-namespace-portal.md)
* [Bevezetés a Node használatába](relay-hybrid-connections-node-get-started.md)

