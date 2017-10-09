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
# <a name="get-started-with-relay-hybrid-connections"></a><span data-ttu-id="75cf6-103">Ismerkedés a hibrid Relay-kapcsolatokkal</span><span class="sxs-lookup"><span data-stu-id="75cf6-103">Get started with Relay Hybrid Connections</span></span>
[!INCLUDE [relay-selector-hybrid-connections](../../includes/relay-selector-hybrid-connections.md)]

<span data-ttu-id="75cf6-104">Ez az oktatóanyag bemutatja túl[Azure hibrid kapcsolatok](relay-what-is-it.md#hybrid-connections), és bemutatja, hogyan toouse .NET toocreate egy ügyfél-alkalmazás által küldött üzenetek tooa megfelelő figyelő alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="75cf6-104">This tutorial provides an introduction too[Azure Relay Hybrid Connections](relay-what-is-it.md#hybrid-connections), and shows how toouse .NET toocreate a client application that sends messages tooa corresponding listener application.</span></span> 

## <a name="what-will-be-accomplished"></a><span data-ttu-id="75cf6-105">Az oktatóanyag célja</span><span class="sxs-lookup"><span data-stu-id="75cf6-105">What will be accomplished</span></span>
<span data-ttu-id="75cf6-106">Hibrid kapcsolatok egy ügyfél és kiszolgáló-összetevők van szükség, mert a hello oktatóanyag két konzol alkalmazást hoz létre.</span><span class="sxs-lookup"><span data-stu-id="75cf6-106">Because Hybrid Connections requires both a client and a server component, hello tutorial creates two console applications.</span></span> <span data-ttu-id="75cf6-107">Az alábbiakban hello lépéseket:</span><span class="sxs-lookup"><span data-stu-id="75cf6-107">Here are hello steps:</span></span>

1. <span data-ttu-id="75cf6-108">Hello Azure-portál használatával, továbbító névtér létrehozása.</span><span class="sxs-lookup"><span data-stu-id="75cf6-108">Create a Relay namespace, using hello Azure portal.</span></span>
2. <span data-ttu-id="75cf6-109">A hibrid kapcsolat létrehozása az adott névtérben, hello Azure-portál használatával.</span><span class="sxs-lookup"><span data-stu-id="75cf6-109">Create a hybrid connection in that namespace, using hello Azure portal.</span></span>
3. <span data-ttu-id="75cf6-110">A kiszolgáló (figyelő) konzol alkalmazás tooreceive üzenetet írni.</span><span class="sxs-lookup"><span data-stu-id="75cf6-110">Write a server (listener) console application tooreceive messages.</span></span>
4. <span data-ttu-id="75cf6-111">Egy ügyfélkonzol (küldő) alkalmazás toosend üzenetet írni.</span><span class="sxs-lookup"><span data-stu-id="75cf6-111">Write a client (sender) console application toosend messages.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="75cf6-112">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="75cf6-112">Prerequisites</span></span>

<span data-ttu-id="75cf6-113">toocomplete ebben az oktatóanyagban szüksége lesz a következő előfeltételek hello:</span><span class="sxs-lookup"><span data-stu-id="75cf6-113">toocomplete this tutorial, you'll need hello following prerequisites:</span></span>

1. <span data-ttu-id="75cf6-114">[Visual Studio 2015 vagy újabb](http://www.visualstudio.com).</span><span class="sxs-lookup"><span data-stu-id="75cf6-114">[Visual Studio 2015 or higher](http://www.visualstudio.com).</span></span> <span data-ttu-id="75cf6-115">az oktatóanyagban szereplő példák hello használja a Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="75cf6-115">hello examples in this tutorial use Visual Studio 2017.</span></span>
2. <span data-ttu-id="75cf6-116">Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="75cf6-116">An Azure subscription.</span></span>

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="1-create-a-namespace-using-hello-azure-portal"></a><span data-ttu-id="75cf6-117">1. Hello Azure-portál használatával névtér létrehozása</span><span class="sxs-lookup"><span data-stu-id="75cf6-117">1. Create a namespace using hello Azure portal</span></span>
<span data-ttu-id="75cf6-118">Ha már létrehozott egy továbbító névtér, jump toohello [hello Azure portál használata hibrid kapcsolat létrehozása](#2-create-a-hybrid-connection-using-the-azure-portal) szakasz.</span><span class="sxs-lookup"><span data-stu-id="75cf6-118">If you have already created a Relay namespace, jump toohello [Create a hybrid connection using hello Azure portal](#2-create-a-hybrid-connection-using-the-azure-portal) section.</span></span>

[!INCLUDE [relay-create-namespace-portal](../../includes/relay-create-namespace-portal.md)]

## <a name="2-create-a-hybrid-connection-using-hello-azure-portal"></a><span data-ttu-id="75cf6-119">2. A hibrid kapcsolat létrehozása használatával hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="75cf6-119">2. Create a hybrid connection using hello Azure portal</span></span>
<span data-ttu-id="75cf6-120">Ha már létrehozott egy hibrid kapcsolat, jump toohello [hozzon létre egy kiszolgálói alkalmazás](#3-create-a-server-application-listener) szakasz.</span><span class="sxs-lookup"><span data-stu-id="75cf6-120">If you have already created a hybrid connection, jump toohello [Create a server application](#3-create-a-server-application-listener) section.</span></span>

[!INCLUDE [relay-create-hybrid-connection-portal](../../includes/relay-create-hybrid-connection-portal.md)]

## <a name="3-create-a-server-application-listener"></a><span data-ttu-id="75cf6-121">3. Kiszolgálói alkalmazás (figyelő) létrehozása</span><span class="sxs-lookup"><span data-stu-id="75cf6-121">3. Create a server application (listener)</span></span>
<span data-ttu-id="75cf6-122">toolisten és üzeneteket fogadni a hello továbbítási, azt fog kiírni, a C# konzolalkalmazást a Visual Studio használatával.</span><span class="sxs-lookup"><span data-stu-id="75cf6-122">toolisten and receive messages from hello Relay, we will write a C# console application using Visual Studio.</span></span>

[!INCLUDE [relay-hybrid-connections-dotnet-get-started-server](../../includes/relay-hybrid-connections-dotnet-get-started-server.md)]

## <a name="4-create-a-client-application-sender"></a><span data-ttu-id="75cf6-123">4. Ügyfélalkalmazás létrehozása (küldő)</span><span class="sxs-lookup"><span data-stu-id="75cf6-123">4. Create a client application (sender)</span></span>
<span data-ttu-id="75cf6-124">toosend üzenetek toohello továbbítják, azt fogja írni egy C#-konzolalkalmazást a Visual Studio használatával.</span><span class="sxs-lookup"><span data-stu-id="75cf6-124">toosend messages toohello Relay, we will write a C# console application using Visual Studio.</span></span>

[!INCLUDE [relay-hybrid-connections-dotnet-get-started-client](../../includes/relay-hybrid-connections-dotnet-get-started-client.md)]

## <a name="5-run-hello-applications"></a><span data-ttu-id="75cf6-125">5. Hello alkalmazások futtatásához</span><span class="sxs-lookup"><span data-stu-id="75cf6-125">5. Run hello applications</span></span>
1. <span data-ttu-id="75cf6-126">Futtassa a hello server alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="75cf6-126">Run hello server application.</span></span>
2. <span data-ttu-id="75cf6-127">Hello ügyfélalkalmazás futtatása, és adjon meg szöveget.</span><span class="sxs-lookup"><span data-stu-id="75cf6-127">Run hello client application and enter some text.</span></span>
3. <span data-ttu-id="75cf6-128">Győződjön meg arról, hogy hello kiszolgálón alkalmazás konzol kimenetek hello szöveg hello ügyfélalkalmazás megadott.</span><span class="sxs-lookup"><span data-stu-id="75cf6-128">Ensure that hello server application console outputs hello text that was entered in hello client application.</span></span>

![running-applications](./media/relay-hybrid-connections-dotnet-get-started/running-applications.png)

<span data-ttu-id="75cf6-130">Gratulálunk, végpontok közötti hibrid kapcsolatok alkalmazást hozott létre.</span><span class="sxs-lookup"><span data-stu-id="75cf6-130">Congratulations, you have created an end-to-end Hybrid Connections application.</span></span>

## <a name="next-steps"></a><span data-ttu-id="75cf6-131">Következő lépések:</span><span class="sxs-lookup"><span data-stu-id="75cf6-131">Next steps:</span></span>
* [<span data-ttu-id="75cf6-132">Relay – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="75cf6-132">Relay FAQ</span></span>](relay-faq.md)
* [<span data-ttu-id="75cf6-133">Névtér létrehozása</span><span class="sxs-lookup"><span data-stu-id="75cf6-133">Create a namespace</span></span>](relay-create-namespace-portal.md)
* [<span data-ttu-id="75cf6-134">Bevezetés a Node használatába</span><span class="sxs-lookup"><span data-stu-id="75cf6-134">Get started with Node</span></span>](relay-hybrid-connections-node-get-started.md)

