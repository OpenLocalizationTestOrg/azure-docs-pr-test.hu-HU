---
title: "Azure Mobile Engagement Service API-k elérésére a REST API használatával"
description: "A Mobile Engagement REST API-k használata az Azure Mobile Engagement Service API-k elérésére"
services: mobile-engagement
documentationcenter: mobile
author: wesmc7777
manager: erikre
editor: 
ms.assetid: e8df4897-55ee-45df-b41e-ff187e3d9d12
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 10/05/2016
ms.author: wesmc;ricksal
ms.openlocfilehash: 4b21bca6fee7012ce1dba96950ae075eded414f8
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="using-rest-to-access-azure-mobile-engagement-service-apis"></a><span data-ttu-id="3c94a-103">Azure Mobile Engagement Service API-k elérésére használt többi</span><span class="sxs-lookup"><span data-stu-id="3c94a-103">Using REST to access Azure Mobile Engagement Service APIs</span></span>
<span data-ttu-id="3c94a-104">Az Azure Mobile Engagement biztosít a [Azure Mobile Engagement REST API](https://msdn.microsoft.com/library/azure/mt683754.aspx) felügyelheti az eszközöket, Reach/leküldéses kampányokra stb.</span><span class="sxs-lookup"><span data-stu-id="3c94a-104">Azure Mobile Engagement provides the [Azure Mobile Engagement REST API](https://msdn.microsoft.com/library/azure/mt683754.aspx) for you to manage Devices, Reach/Push campaigns etc.</span></span>

> [!NOTE]
> <span data-ttu-id="3c94a-105">Az Azure Mobile Engagement szolgáltatást 2018 márciusától megszüntetjük, és jelenleg csak meglévő ügyfelek számára érhető el.</span><span class="sxs-lookup"><span data-stu-id="3c94a-105">The Azure Mobile Engagement service will be retired March 2018 and is currently only available to existing customers.</span></span> <span data-ttu-id="3c94a-106">További információkért lásd: [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).</span><span class="sxs-lookup"><span data-stu-id="3c94a-106">For more information, see [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).</span></span>

<span data-ttu-id="3c94a-107">Ha nem szeretné, hogy közvetlenül a REST API-k használatával, azt is adja meg a [Swagger-fájl](https://github.com/Azure/azure-rest-api-specs/blob/master/arm-mobileengagement/2014-12-01/swagger/mobile-engagement.json) használható az eszközök SDK-k létrehozására az a kívánt nyelvet.</span><span class="sxs-lookup"><span data-stu-id="3c94a-107">If you do not want to use the REST APIs directly, we also provide a [Swagger file](https://github.com/Azure/azure-rest-api-specs/blob/master/arm-mobileengagement/2014-12-01/swagger/mobile-engagement.json) that you can use with tools to generate SDKs for your preferred language.</span></span> <span data-ttu-id="3c94a-108">Azt javasoljuk, a [AutoRest](https://github.com/Azure/AutoRest) eszköz használatával hozzon létre az SDK a Swagger-fájl.</span><span class="sxs-lookup"><span data-stu-id="3c94a-108">We recommend using the [AutoRest](https://github.com/Azure/AutoRest) tool to generate your SDK from our Swagger file.</span></span> <span data-ttu-id="3c94a-109">A .NET SDK létrehoztunk egy hasonló módon, amely teszi lehetővé ezen API-k együttműködhet egy C# burkoló használatával, és nem kell a hitelesítési jogkivonat egyeztetést és frissítse magát.</span><span class="sxs-lookup"><span data-stu-id="3c94a-109">We have created a .NET SDK in a similar manner which allows you to interact with these APIs using a C# wrapper and you don't have to do the authentication token negotiation and refresh yourself.</span></span> <span data-ttu-id="3c94a-110">Lásd: [API a .NET SDK minta](mobile-engagement-dotnet-sdk-service-api.md) megtudhatja, hogyan használhatja a .net SDK API-hoz.</span><span class="sxs-lookup"><span data-stu-id="3c94a-110">See [Service API .NET SDK Sample](mobile-engagement-dotnet-sdk-service-api.md) to learn how to use the .net SDK for API</span></span>
