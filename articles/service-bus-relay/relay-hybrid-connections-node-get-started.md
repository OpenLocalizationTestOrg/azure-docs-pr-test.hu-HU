---
title: "Ismerkedés az Azure Relay Hibrid-kapcsolatokkal a Node-ban | Microsoft Docs"
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
ms.openlocfilehash: c3bfc45969f250059988129f532edd12dfe3dcfe
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="get-started-with-relay-hybrid-connections"></a><span data-ttu-id="a5c3a-103">Ismerkedés a hibrid Relay-kapcsolatokkal</span><span class="sxs-lookup"><span data-stu-id="a5c3a-103">Get started with Relay Hybrid Connections</span></span>

[!INCLUDE [relay-selector-hybrid-connections](../../includes/relay-selector-hybrid-connections.md)]

<span data-ttu-id="a5c3a-104">Ez az oktatóprogram bevezetést nyújt a [hibrid Azure Relay-kapcsolatok](relay-what-is-it.md#hybrid-connections) használatába, és bemutatja egy olyan ügyfélalkalmazás Node.js használatával való létrehozását, amely üzeneteket küld egy kapcsolódó figyelőalkalmazásnak.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-104">This tutorial provides an introduction to [Azure Relay Hybrid Connections](relay-what-is-it.md#hybrid-connections), and shows how to use Node.js to create a client application that sends messages to a corresponding listener application.</span></span> 

## <a name="what-will-be-accomplished"></a><span data-ttu-id="a5c3a-105">Az oktatóanyag célja</span><span class="sxs-lookup"><span data-stu-id="a5c3a-105">What will be accomplished</span></span>

<span data-ttu-id="a5c3a-106">A hibrid kapcsolatokhoz egy ügyfélre és egy kiszolgáló-összetevőre is szükség van, így ebben az oktatóanyagban két konzolalkalmazást hozunk létre.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-106">Because Hybrid Connections requires both a client and a server component, we will create two console applications in this tutorial.</span></span> <span data-ttu-id="a5c3a-107">A lépések a következők:</span><span class="sxs-lookup"><span data-stu-id="a5c3a-107">Here are the steps:</span></span>

1. <span data-ttu-id="a5c3a-108">Relay-névtér létrehozása az Azure Portal használatával.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-108">Create a Relay namespace, using the Azure portal.</span></span>
2. <span data-ttu-id="a5c3a-109">Hibrid kapcsolat létrehozása az Azure Portal használatával.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-109">Create a hybrid connection, using the Azure portal.</span></span>
3. <span data-ttu-id="a5c3a-110">Kiszolgálói konzolalkalmazás írása üzenetfogadási céllal.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-110">Write a server console application to receive messages.</span></span>
4. <span data-ttu-id="a5c3a-111">Ügyfél-konzolalkalmazás írása üzenetküldési céllal.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-111">Write a client console application to send messages.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a5c3a-112">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="a5c3a-112">Prerequisites</span></span>

1. <span data-ttu-id="a5c3a-113">[Node.js](https://nodejs.org/en/).</span><span class="sxs-lookup"><span data-stu-id="a5c3a-113">[Node.js](https://nodejs.org/en/).</span></span>
2. <span data-ttu-id="a5c3a-114">Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-114">An Azure subscription.</span></span>

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="1-create-a-namespace-using-the-azure-portal"></a><span data-ttu-id="a5c3a-115">1. Névtér létrehozása az Azure Portal használatával</span><span class="sxs-lookup"><span data-stu-id="a5c3a-115">1. Create a namespace using the Azure portal</span></span>

<span data-ttu-id="a5c3a-116">Ha a Relay-névteret már létrehozta, folytassa a [Hibrid kapcsolat létrehozása az Azure Portal használatával](#2-create-a-hybrid-connection-using-the-azure-portal) szakasszal.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-116">If you already have a Relay namespace created, jump to the [Create a hybrid connection using the Azure portal](#2-create-a-hybrid-connection-using-the-azure-portal) section.</span></span>

[!INCLUDE [relay-create-namespace-portal](../../includes/relay-create-namespace-portal.md)]

## <a name="2-create-a-hybrid-connection-using-the-azure-portal"></a><span data-ttu-id="a5c3a-117">2. Hibrid kapcsolat létrehozása az Azure Portal használatával</span><span class="sxs-lookup"><span data-stu-id="a5c3a-117">2. Create a hybrid connection using the Azure portal</span></span>

<span data-ttu-id="a5c3a-118">Ha már rendelkezik egy létrehozott hibrid kapcsolattal, folytassa a [Kiszolgálói alkalmazás létrehozása](#3-create-a-server-application-listener) szakasszal.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-118">If you already have a hybrid connection created, jump to the [Create a server application](#3-create-a-server-application-listener) section.</span></span>

[!INCLUDE [relay-create-hybrid-connection-portal](../../includes/relay-create-hybrid-connection-portal.md)]

## <a name="3-create-a-server-application-listener"></a><span data-ttu-id="a5c3a-119">3. Kiszolgálói alkalmazás (figyelő) létrehozása</span><span class="sxs-lookup"><span data-stu-id="a5c3a-119">3. Create a server application (listener)</span></span>

<span data-ttu-id="a5c3a-120">Node.js konzolalkalmazást írunk az üzenetek figyeléséhez és a Relay-től való fogadásához.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-120">To listen and receive messages from the Relay, we will write a Node.js console application.</span></span>

[!INCLUDE [relay-hybrid-connections-node-get-started-server](../../includes/relay-hybrid-connections-node-get-started-server.md)]

## <a name="4-create-a-client-application-sender"></a><span data-ttu-id="a5c3a-121">4. Ügyfélalkalmazás létrehozása (küldő)</span><span class="sxs-lookup"><span data-stu-id="a5c3a-121">4. Create a client application (sender)</span></span>

<span data-ttu-id="a5c3a-122">Node.js konzolalkalmazást írunk az üzenetek Relay-be való küldéséhez.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-122">To send messages to the Relay, we will write a Node.js console application.</span></span>

[!INCLUDE [relay-hybrid-connections-node-get-started-client](../../includes/relay-hybrid-connections-node-get-started-client.md)]

## <a name="5-run-the-applications"></a><span data-ttu-id="a5c3a-123">5. Az alkalmazások futtatása</span><span class="sxs-lookup"><span data-stu-id="a5c3a-123">5. Run the applications</span></span>

1. <span data-ttu-id="a5c3a-124">Futtassa a kiszolgálóalkalmazást: egy Node.js-parancssorba írja be a következőt: `node listener.js`.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-124">Run the server application: from a Node.js command prompt type `node listener.js`.</span></span>
2. <span data-ttu-id="a5c3a-125">Futtassa az ügyfélalkalmazást: egy Node.js parancssorba írja be a `node sender.js` parancsot, majd írjon be szöveget.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-125">Run the client application: from a Node.js command prompt type `node sender.js`, and enter some text.</span></span>
3. <span data-ttu-id="a5c3a-126">Győződjön meg arról, hogy az alkalmazás konzolja kiírja a szöveget, amely az ügyfélalkalmazásban lett megadva.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-126">Ensure that the server application console outputs the text that was entered in the client application.</span></span>

![running-applications](./media/relay-hybrid-connections-node-get-started/running-applications.png)

<span data-ttu-id="a5c3a-128">Gratulálunk, végpontok közötti hibrid kapcsolatok alkalmazást hozott létre a Node.js használatával.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-128">Congratulations, you have created an end-to-end Hybrid Connections application using Node.js!</span></span>

## <a name="next-steps"></a><span data-ttu-id="a5c3a-129">Következő lépések:</span><span class="sxs-lookup"><span data-stu-id="a5c3a-129">Next steps:</span></span>

* [<span data-ttu-id="a5c3a-130">Relay – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="a5c3a-130">Relay FAQ</span></span>](relay-faq.md)
* [<span data-ttu-id="a5c3a-131">Névtér létrehozása</span><span class="sxs-lookup"><span data-stu-id="a5c3a-131">Create a namespace</span></span>](relay-create-namespace-portal.md)
* [<span data-ttu-id="a5c3a-132">Ismerkedés a .NET-tel</span><span class="sxs-lookup"><span data-stu-id="a5c3a-132">Get started with .NET</span></span>](relay-hybrid-connections-dotnet-get-started.md)
* [<span data-ttu-id="a5c3a-133">Bevezetés a Node használatába</span><span class="sxs-lookup"><span data-stu-id="a5c3a-133">Get started with Node</span></span>](relay-hybrid-connections-node-get-started.md)

