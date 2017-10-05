---
title: "Onedrive vállalati verzió |} Microsoft Docs"
description: "Az Azure App service Logic Apps alkalmazások létrehozása Csatlakoztassa a fájlok kezelését onedrive vállalati verzióhoz. Különböző műveletek végezhetők feltöltési, például frissítse, beolvasása, és törli a fájlokat."
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: cf9484e9-7a20-4de0-93c8-0fa132221f2b
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 08/18/2016
ms.author: mandia; ladocs
ms.openlocfilehash: 783d6a640d9626508bcabc5f991dc5b6fc22eaf4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-onedrive-for-business-connector"></a><span data-ttu-id="ab0ea-105">A onedrive vállalati verzió üzleti összekötő az első lépései</span><span class="sxs-lookup"><span data-stu-id="ab0ea-105">Get started with the OneDrive for Business connector</span></span>
<span data-ttu-id="ab0ea-106">Csatlakoztassa a fájlok kezelését onedrive vállalati verzióhoz.</span><span class="sxs-lookup"><span data-stu-id="ab0ea-106">Connect to OneDrive for Business to manage your files.</span></span> <span data-ttu-id="ab0ea-107">Különböző műveletek végezhetők feltöltési, például frissítse, beolvasása, és törli a fájlokat.</span><span class="sxs-lookup"><span data-stu-id="ab0ea-107">You can perform various actions such as upload, update, get, and delete on files.</span></span>

<span data-ttu-id="ab0ea-108">Most hozzon létre egy logic app kezdheti, lásd: [logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="ab0ea-108">You can get started by creating a logic app now, see [Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="create-a-connection-to-onedrive-for-business"></a><span data-ttu-id="ab0ea-109">Kapcsolatot létesíthet a onedrive vállalati verzió</span><span class="sxs-lookup"><span data-stu-id="ab0ea-109">Create a connection to OneDrive for Business</span></span>
<span data-ttu-id="ab0ea-110">A Logic apps onedrive vállalati verzió létrehozásához, létre kell hoznia egy **kapcsolat** adja meg a részleteket a következő tulajdonságokkal:</span><span class="sxs-lookup"><span data-stu-id="ab0ea-110">To create Logic apps with OneDrive for Business, you must first create a **connection** then provide the details for the following properties:</span></span>

| <span data-ttu-id="ab0ea-111">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="ab0ea-111">Property</span></span> | <span data-ttu-id="ab0ea-112">Szükséges</span><span class="sxs-lookup"><span data-stu-id="ab0ea-112">Required</span></span> | <span data-ttu-id="ab0ea-113">Leírás</span><span class="sxs-lookup"><span data-stu-id="ab0ea-113">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ab0ea-114">Token</span><span class="sxs-lookup"><span data-stu-id="ab0ea-114">Token</span></span> |<span data-ttu-id="ab0ea-115">Igen</span><span class="sxs-lookup"><span data-stu-id="ab0ea-115">Yes</span></span> |<span data-ttu-id="ab0ea-116">Adja meg a OneDrive for Businesshez tartozó hitelesítő adatait</span><span class="sxs-lookup"><span data-stu-id="ab0ea-116">Provide OneDrive for Business Credentials</span></span> |

<span data-ttu-id="ab0ea-117">Miután létrehozta a kapcsolatot, használhatja a műveletek végrehajtása és a jelen cikkben ismertetett eseményindítók figyelni.</span><span class="sxs-lookup"><span data-stu-id="ab0ea-117">After you create the connection, you can use it to execute the actions and listen for the triggers described in this article.</span></span>

> [!INCLUDE [Steps to create a connection to OneDrive for Business](../../includes/connectors-create-api-onedriveforbusiness.md)]
> 

## <a name="connector-specific-details"></a><span data-ttu-id="ab0ea-118">Összekötő-specifikus részletei</span><span class="sxs-lookup"><span data-stu-id="ab0ea-118">Connector-specific details</span></span>

<span data-ttu-id="ab0ea-119">Bármely eseményindítók és a swagger definiált műveletek megtekintése, és semmilyen határnak a Lásd még: a [connector részleteket](/connectors/onedriveforbusinessconnector/).</span><span class="sxs-lookup"><span data-stu-id="ab0ea-119">View any triggers and actions defined in the swagger, and also see any limits in the [connector details](/connectors/onedriveforbusinessconnector/).</span></span>

## <a name="more-connectors"></a><span data-ttu-id="ab0ea-120">További összekötők</span><span class="sxs-lookup"><span data-stu-id="ab0ea-120">More connectors</span></span>
<span data-ttu-id="ab0ea-121">Lépjen vissza a [API-k lista](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="ab0ea-121">Go back to the [APIs list](apis-list.md).</span></span>