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
# <a name="get-started-with-relay-hybrid-connections"></a><span data-ttu-id="6d18a-103">Ismerkedés a hibrid Relay-kapcsolatokkal</span><span class="sxs-lookup"><span data-stu-id="6d18a-103">Get started with Relay Hybrid Connections</span></span>

[!INCLUDE [relay-selector-hybrid-connections](../../includes/relay-selector-hybrid-connections.md)]

<span data-ttu-id="6d18a-104">Ez az oktatóanyag bemutatja túl[Azure hibrid kapcsolatok](relay-what-is-it.md#hybrid-connections), és bemutatja, hogyan toouse Node.js toocreate egy ügyfél-alkalmazás által küldött üzenetek tooa megfelelő figyelő alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="6d18a-104">This tutorial provides an introduction too[Azure Relay Hybrid Connections](relay-what-is-it.md#hybrid-connections), and shows how toouse Node.js toocreate a client application that sends messages tooa corresponding listener application.</span></span> 

## <a name="what-will-be-accomplished"></a><span data-ttu-id="6d18a-105">Az oktatóanyag célja</span><span class="sxs-lookup"><span data-stu-id="6d18a-105">What will be accomplished</span></span>

<span data-ttu-id="6d18a-106">A hibrid kapcsolatokhoz egy ügyfélre és egy kiszolgáló-összetevőre is szükség van, így ebben az oktatóanyagban két konzolalkalmazást hozunk létre.</span><span class="sxs-lookup"><span data-stu-id="6d18a-106">Because Hybrid Connections requires both a client and a server component, we will create two console applications in this tutorial.</span></span> <span data-ttu-id="6d18a-107">Az alábbiakban hello lépéseket:</span><span class="sxs-lookup"><span data-stu-id="6d18a-107">Here are hello steps:</span></span>

1. <span data-ttu-id="6d18a-108">Hello Azure-portál használatával, továbbító névtér létrehozása.</span><span class="sxs-lookup"><span data-stu-id="6d18a-108">Create a Relay namespace, using hello Azure portal.</span></span>
2. <span data-ttu-id="6d18a-109">Hozzon létre egy hibrid kapcsolat hello Azure-portál használatával.</span><span class="sxs-lookup"><span data-stu-id="6d18a-109">Create a hybrid connection, using hello Azure portal.</span></span>
3. <span data-ttu-id="6d18a-110">A kiszolgáló konzol alkalmazás tooreceive üzenetet írni.</span><span class="sxs-lookup"><span data-stu-id="6d18a-110">Write a server console application tooreceive messages.</span></span>
4. <span data-ttu-id="6d18a-111">Egy ügyfél konzol alkalmazás toosend üzenetet írni.</span><span class="sxs-lookup"><span data-stu-id="6d18a-111">Write a client console application toosend messages.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6d18a-112">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="6d18a-112">Prerequisites</span></span>

1. <span data-ttu-id="6d18a-113">[Node.js](https://nodejs.org/en/).</span><span class="sxs-lookup"><span data-stu-id="6d18a-113">[Node.js](https://nodejs.org/en/).</span></span>
2. <span data-ttu-id="6d18a-114">Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="6d18a-114">An Azure subscription.</span></span>

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="1-create-a-namespace-using-hello-azure-portal"></a><span data-ttu-id="6d18a-115">1. Hello Azure-portál használatával névtér létrehozása</span><span class="sxs-lookup"><span data-stu-id="6d18a-115">1. Create a namespace using hello Azure portal</span></span>

<span data-ttu-id="6d18a-116">Ha már létrehozott egy továbbító névtér, jump toohello [hello Azure portál használata hibrid kapcsolat létrehozása](#2-create-a-hybrid-connection-using-the-azure-portal) szakasz.</span><span class="sxs-lookup"><span data-stu-id="6d18a-116">If you already have a Relay namespace created, jump toohello [Create a hybrid connection using hello Azure portal](#2-create-a-hybrid-connection-using-the-azure-portal) section.</span></span>

[!INCLUDE [relay-create-namespace-portal](../../includes/relay-create-namespace-portal.md)]

## <a name="2-create-a-hybrid-connection-using-hello-azure-portal"></a><span data-ttu-id="6d18a-117">2. A hibrid kapcsolat létrehozása használatával hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="6d18a-117">2. Create a hybrid connection using hello Azure portal</span></span>

<span data-ttu-id="6d18a-118">Ha már rendelkezik egy hibrid kapcsolat létrehozása, jump toohello [hozzon létre egy kiszolgálói alkalmazás](#3-create-a-server-application-listener) szakasz.</span><span class="sxs-lookup"><span data-stu-id="6d18a-118">If you already have a hybrid connection created, jump toohello [Create a server application](#3-create-a-server-application-listener) section.</span></span>

[!INCLUDE [relay-create-hybrid-connection-portal](../../includes/relay-create-hybrid-connection-portal.md)]

## <a name="3-create-a-server-application-listener"></a><span data-ttu-id="6d18a-119">3. Kiszolgálói alkalmazás (figyelő) létrehozása</span><span class="sxs-lookup"><span data-stu-id="6d18a-119">3. Create a server application (listener)</span></span>

<span data-ttu-id="6d18a-120">toolisten és üzenetek fogadása hello továbbító, a Node.js-Konzolalkalmazás írja azt.</span><span class="sxs-lookup"><span data-stu-id="6d18a-120">toolisten and receive messages from hello Relay, we will write a Node.js console application.</span></span>

[!INCLUDE [relay-hybrid-connections-node-get-started-server](../../includes/relay-hybrid-connections-node-get-started-server.md)]

## <a name="4-create-a-client-application-sender"></a><span data-ttu-id="6d18a-121">4. Ügyfélalkalmazás létrehozása (küldő)</span><span class="sxs-lookup"><span data-stu-id="6d18a-121">4. Create a client application (sender)</span></span>

<span data-ttu-id="6d18a-122">toosend üzenetek toohello továbbítási, azt fogja írni a Node.js-Konzolalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="6d18a-122">toosend messages toohello Relay, we will write a Node.js console application.</span></span>

[!INCLUDE [relay-hybrid-connections-node-get-started-client](../../includes/relay-hybrid-connections-node-get-started-client.md)]

## <a name="5-run-hello-applications"></a><span data-ttu-id="6d18a-123">5. Hello alkalmazások futtatásához</span><span class="sxs-lookup"><span data-stu-id="6d18a-123">5. Run hello applications</span></span>

1. <span data-ttu-id="6d18a-124">Hello server alkalmazás futtatásához: Node.js parancssori típusból `node listener.js`.</span><span class="sxs-lookup"><span data-stu-id="6d18a-124">Run hello server application: from a Node.js command prompt type `node listener.js`.</span></span>
2. <span data-ttu-id="6d18a-125">Hello ügyfélalkalmazás futtatása: a Node.js parancssori típusból `node sender.js`, és adjon meg szöveget.</span><span class="sxs-lookup"><span data-stu-id="6d18a-125">Run hello client application: from a Node.js command prompt type `node sender.js`, and enter some text.</span></span>
3. <span data-ttu-id="6d18a-126">Győződjön meg arról, hogy hello kiszolgálón alkalmazás konzol kimenetek hello szöveg hello ügyfélalkalmazás megadott.</span><span class="sxs-lookup"><span data-stu-id="6d18a-126">Ensure that hello server application console outputs hello text that was entered in hello client application.</span></span>

![running-applications](./media/relay-hybrid-connections-node-get-started/running-applications.png)

<span data-ttu-id="6d18a-128">Gratulálunk, végpontok közötti hibrid kapcsolatok alkalmazást hozott létre a Node.js használatával.</span><span class="sxs-lookup"><span data-stu-id="6d18a-128">Congratulations, you have created an end-to-end Hybrid Connections application using Node.js!</span></span>

## <a name="next-steps"></a><span data-ttu-id="6d18a-129">Következő lépések:</span><span class="sxs-lookup"><span data-stu-id="6d18a-129">Next steps:</span></span>

* [<span data-ttu-id="6d18a-130">Relay – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="6d18a-130">Relay FAQ</span></span>](relay-faq.md)
* [<span data-ttu-id="6d18a-131">Névtér létrehozása</span><span class="sxs-lookup"><span data-stu-id="6d18a-131">Create a namespace</span></span>](relay-create-namespace-portal.md)
* [<span data-ttu-id="6d18a-132">Ismerkedés a .NET-tel</span><span class="sxs-lookup"><span data-stu-id="6d18a-132">Get started with .NET</span></span>](relay-hybrid-connections-dotnet-get-started.md)
* [<span data-ttu-id="6d18a-133">Bevezetés a Node használatába</span><span class="sxs-lookup"><span data-stu-id="6d18a-133">Get started with Node</span></span>](relay-hybrid-connections-node-get-started.md)

