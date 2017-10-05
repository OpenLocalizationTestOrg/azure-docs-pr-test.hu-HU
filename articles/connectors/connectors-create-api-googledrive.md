---
title: "Adja hozzá a Google meghajtó összekötőt a logic apps |} Microsoft Docs"
description: "A REST API-paraméterekkel rendelkező Google meghajtó összekötő áttekintése"
services: 
suite: 
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: b2bcebc5-02d2-435b-b0da-ef53bc51c4b6
ms.service: multiple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/07/2016
ms.author: mandia; ladocs
ms.openlocfilehash: c066a10b33e172eb5f16eede43ec407794000c90
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-google-drive-connector"></a><span data-ttu-id="4daf6-103">A Google meghajtó összekötő az első lépései</span><span class="sxs-lookup"><span data-stu-id="4daf6-103">Get started with the Google Drive connector</span></span>
<span data-ttu-id="4daf6-104">Google-meghajtó létrehozása, fájlok, a get sorok és a további csatlakozni.</span><span class="sxs-lookup"><span data-stu-id="4daf6-104">Connect to Google Drive to create files, get rows, and more.</span></span> <span data-ttu-id="4daf6-105">Google-meghajtó a következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="4daf6-105">With Google Drive, you can:</span></span> 

* <span data-ttu-id="4daf6-106">Az üzleti folyamata, a keresés származó adatok alapján történő létrehozása.</span><span class="sxs-lookup"><span data-stu-id="4daf6-106">Build your business flow based on the data you get from your search.</span></span> 
* <span data-ttu-id="4daf6-107">Műveletek segítségével képek keresni, keresse a híreket és még sok más.</span><span class="sxs-lookup"><span data-stu-id="4daf6-107">Use actions to search images, search the news, and more.</span></span> <span data-ttu-id="4daf6-108">Ezeket a műveleteket válaszol, és végezze el a kimeneti más műveletek érhető el.</span><span class="sxs-lookup"><span data-stu-id="4daf6-108">These actions get a response, and then make the output available for other actions.</span></span> <span data-ttu-id="4daf6-109">Például keresse meg a videót, és Twitter használatával, amely egy Twitter-hírcsatorna a videó utáni.</span><span class="sxs-lookup"><span data-stu-id="4daf6-109">For example, you can search for a video, and then use Twitter to post that video to a Twitter feed.</span></span>

<span data-ttu-id="4daf6-110">Most hozzon létre egy logic app kezdheti, lásd: [logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="4daf6-110">You can get started by creating a logic app now, see [Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="create-the-connection-to-google-drive"></a><span data-ttu-id="4daf6-111">Google-meghajtóra kapcsolat létrehozása</span><span class="sxs-lookup"><span data-stu-id="4daf6-111">Create the connection to Google Drive</span></span>
<span data-ttu-id="4daf6-112">Ezt az összekötőt a logic apps hozzáadásakor engedélyeznie kell a logic apps a Google-meghajtóról való kapcsolódáshoz.</span><span class="sxs-lookup"><span data-stu-id="4daf6-112">When you add this connector to your logic apps, you must authorize logic apps to connect to your Google Drive.</span></span>

> [!INCLUDE [Steps to create a connection to googledrive](../../includes/connectors-create-api-googledrive.md)]
> 
> 

<span data-ttu-id="4daf6-113">Miután létrehozta a kapcsolatot, a Google-meghajtó tulajdonságainak, például a mappa elérési útját vagy a fájl nevét adja meg.</span><span class="sxs-lookup"><span data-stu-id="4daf6-113">After you create the connection, you enter the Google Drive properties, like the folder path or file name.</span></span> 

## <a name="connector-specific-details"></a><span data-ttu-id="4daf6-114">Összekötő-specifikus részletei</span><span class="sxs-lookup"><span data-stu-id="4daf6-114">Connector-specific details</span></span>

<span data-ttu-id="4daf6-115">Bármely eseményindítók és a swagger definiált műveletek megtekintése, és semmilyen határnak a Lásd még: a [connector részleteket](/connectors/googledrive/).</span><span class="sxs-lookup"><span data-stu-id="4daf6-115">View any triggers and actions defined in the swagger, and also see any limits in the [connector details](/connectors/googledrive/).</span></span>

## <a name="more-connectors"></a><span data-ttu-id="4daf6-116">További összekötők</span><span class="sxs-lookup"><span data-stu-id="4daf6-116">More connectors</span></span>
<span data-ttu-id="4daf6-117">Lépjen vissza a [API-k lista](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="4daf6-117">Go back to the [APIs list](apis-list.md).</span></span>
