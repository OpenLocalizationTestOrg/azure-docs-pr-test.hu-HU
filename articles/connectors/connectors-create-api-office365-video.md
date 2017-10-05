---
title: "Használja az Office 365 videó összekötőt a Logic apps |} Microsoft Docs"
description: "Ismerkedés a Microsoft Azure App service Logic apps az Office 365 videó összekötővel"
services: 
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: 738e5aa7-2523-4116-8b65-211b9063852d
ms.service: multiple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/18/2016
ms.author: mandia; ladocs
ms.openlocfilehash: f0e3613d4a3fd5478787c0365eb7a0bcde886c81
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-office365-video-connector"></a><span data-ttu-id="8b60e-103">Az Office365 videó összekötő az első lépései</span><span class="sxs-lookup"><span data-stu-id="8b60e-103">Get started with the Office365 Video connector</span></span>
<span data-ttu-id="8b60e-104">Csatlakoztassa Office 365 videó videó az Office 365 adatainak beolvasása, videókat és egyéb listájának beolvasása.</span><span class="sxs-lookup"><span data-stu-id="8b60e-104">Connect to Office 365 Video to get information about an Office 365 video, get a list of videos, and more.</span></span> <span data-ttu-id="8b60e-105">Az Office 365 videó a következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="8b60e-105">With Office 365 Video, you can:</span></span>

* <span data-ttu-id="8b60e-106">Az üzleti folyamat Office 365 videó származó adatok alapján történő létrehozása.</span><span class="sxs-lookup"><span data-stu-id="8b60e-106">Build your business flow based on the data you get from Office 365 Video.</span></span> 
* <span data-ttu-id="8b60e-107">A videó portál állapotának, egy csatornát, és több összes videokártya listáját műveleteket használni.</span><span class="sxs-lookup"><span data-stu-id="8b60e-107">Use actions that check the video portal status, get a list of all video in a channel, and more.</span></span> <span data-ttu-id="8b60e-108">Ezeket a műveleteket válaszol, és végezze el a kimeneti más műveletek érhető el.</span><span class="sxs-lookup"><span data-stu-id="8b60e-108">These actions get a response, and then make the output available for other actions.</span></span> <span data-ttu-id="8b60e-109">Például a Bing keresési connector használata az Office 365 videók keressen, és az Office 365 video-összekötő segítségével, hogy a videó adatainak beolvasása.</span><span class="sxs-lookup"><span data-stu-id="8b60e-109">For example, you can use the Bing Search connector to search for Office 365 videos, and then use the Office 365 video connector to get information about that video.</span></span> <span data-ttu-id="8b60e-110">Ha a videó megfelel a követelményeknek, a Facebook-on is közzétesz ezt a videót.</span><span class="sxs-lookup"><span data-stu-id="8b60e-110">If the video meets your requirements, you can post this video on Facebook.</span></span> 

<span data-ttu-id="8b60e-111">Most hozzon létre egy logic app kezdheti, lásd: [logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="8b60e-111">You can get started by creating a logic app now, see [Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="create-a-connection-to-office365-video-connector"></a><span data-ttu-id="8b60e-112">Kapcsolatot létesíthet Office365 videó összekötő</span><span class="sxs-lookup"><span data-stu-id="8b60e-112">Create a connection to Office365 Video connector</span></span>
<span data-ttu-id="8b60e-113">Ezt az összekötőt a logic apps hozzáadásakor kell bejelentkezés az Office 365 videó fiókját, és lehetővé teszik a logic Apps alkalmazásokat fiókjához.</span><span class="sxs-lookup"><span data-stu-id="8b60e-113">When you add this connector to your logic apps, you must sign-in to your Office 365 Video account and allow logic apps to connect to your account.</span></span>

> [!INCLUDE [Steps to create a connection to Office 365 Video](../../includes/connectors-create-api-office365video.md)]
> 
> 

<span data-ttu-id="8b60e-114">Miután létrehozta a kapcsolatot, adja meg az Office 365 videó tulajdonságait, például a bérlő neve, vagy csatorna azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="8b60e-114">After you create the connection, you enter the Office 365 video properties, like the tenant name or channel ID.</span></span> 


## <a name="connector-specific-details"></a><span data-ttu-id="8b60e-115">Összekötő-specifikus részletei</span><span class="sxs-lookup"><span data-stu-id="8b60e-115">Connector-specific details</span></span>

<span data-ttu-id="8b60e-116">Bármely eseményindítók és a swagger definiált műveletek megtekintése, és semmilyen határnak a Lásd még: a [connector részleteket](/connectors/office365videoconnector/).</span><span class="sxs-lookup"><span data-stu-id="8b60e-116">View any triggers and actions defined in the swagger, and also see any limits in the [connector details](/connectors/office365videoconnector/).</span></span>

## <a name="more-connectors"></a><span data-ttu-id="8b60e-117">További összekötők</span><span class="sxs-lookup"><span data-stu-id="8b60e-117">More connectors</span></span>
<span data-ttu-id="8b60e-118">Lépjen vissza a [API-k lista](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="8b60e-118">Go back to the [APIs list](apis-list.md).</span></span>