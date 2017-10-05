---
title: "Adja hozzá az Office 365-felhasználók összekötőt a Logic Apps |} Microsoft Docs"
description: "Office 365-felhasználók összekötő REST API-paraméterekkel rendelkező áttekintése"
services: 
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: b2146481-9105-4f56-b4c2-7ae340cb922f
ms.service: multiple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 08/18/2016
ms.author: mandia; ladocs
ms.openlocfilehash: 330f733440932a769eb0fe6031cd0d947a820080
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-office-365-users-connector"></a><span data-ttu-id="81dc5-103">Az Office 365-felhasználók összekötő az első lépései</span><span class="sxs-lookup"><span data-stu-id="81dc5-103">Get started with the Office 365 Users connector</span></span>
<span data-ttu-id="81dc5-104">Office 365 felhasználói profilok, a keresés felhasználói és több csatlakozni.</span><span class="sxs-lookup"><span data-stu-id="81dc5-104">Connect to Office 365 Users to get profiles, search users, and more.</span></span> <span data-ttu-id="81dc5-105">Az Office 365-felhasználók a következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="81dc5-105">With Office 365 Users, you can:</span></span>

* <span data-ttu-id="81dc5-106">Az üzleti folyamat Office 365-felhasználók származó adatok alapján történő létrehozása.</span><span class="sxs-lookup"><span data-stu-id="81dc5-106">Build your business flow based on the data you get from Office 365 Users.</span></span> 
* <span data-ttu-id="81dc5-107">Közvetlen beosztottai első használata műveleteket vezető felhasználói profil, és több kapják meg.</span><span class="sxs-lookup"><span data-stu-id="81dc5-107">Use actions that get direct reports, get a manager's user profile, and more.</span></span> <span data-ttu-id="81dc5-108">Ezeket a műveleteket válaszol, és végezze el a kimeneti más műveletek érhető el.</span><span class="sxs-lookup"><span data-stu-id="81dc5-108">These actions get a response, and then make the output available for other actions.</span></span> <span data-ttu-id="81dc5-109">Például lekérni egy személy közvetlen beosztottai, majd ezt az adatot és SQL Azure-adatbázis frissítéséhez.</span><span class="sxs-lookup"><span data-stu-id="81dc5-109">For example, get a person's direct reports, and then take this information and update a SQL Azure database.</span></span> 

<span data-ttu-id="81dc5-110">Most hozzon létre egy logic app kezdheti, lásd: [logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="81dc5-110">You can get started by creating a logic app now, see [Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="create-a-connection-to-office-365-users"></a><span data-ttu-id="81dc5-111">Kapcsolatot létesíthet Office 365-felhasználók</span><span class="sxs-lookup"><span data-stu-id="81dc5-111">Create a connection to Office 365 Users</span></span>
<span data-ttu-id="81dc5-112">Ezt az összekötőt a logic Apps alkalmazások hozzáadásakor kell bejelentkezés az Office 365-felhasználó fiókját, és lehetővé teszik a logic Apps alkalmazásokat fiókjához.</span><span class="sxs-lookup"><span data-stu-id="81dc5-112">When you add this connector to your logic apps, you must sign-in to your Office 365 Users account and allow logic apps to connect to your account.</span></span>

> [!INCLUDE [Steps to create a connection to Office 365 Users](../../includes/connectors-create-api-office365users.md)]
> 
> 

<span data-ttu-id="81dc5-113">Miután létrehozta a kapcsolatot, akkor adja meg az Office 365-felhasználók tulajdonságai, például a felhasználói azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="81dc5-113">After you create the connection, you enter the Office 365 Users properties, like the user ID.</span></span> <span data-ttu-id="81dc5-114">A **REST API-referenciában** a témakörben ismertetett ezeket a tulajdonságokat.</span><span class="sxs-lookup"><span data-stu-id="81dc5-114">The **REST API reference** in this topic describes these properties.</span></span>

## <a name="connector-specific-details"></a><span data-ttu-id="81dc5-115">Összekötő-specifikus részletei</span><span class="sxs-lookup"><span data-stu-id="81dc5-115">Connector-specific details</span></span>

<span data-ttu-id="81dc5-116">Bármely eseményindítók és a swagger definiált műveletek megtekintése, és semmilyen határnak a Lásd még: a [connector részleteket](/connectors/officeusers/).</span><span class="sxs-lookup"><span data-stu-id="81dc5-116">View any triggers and actions defined in the swagger, and also see any limits in the [connector details](/connectors/officeusers/).</span></span>

## <a name="more-connectors"></a><span data-ttu-id="81dc5-117">További összekötők</span><span class="sxs-lookup"><span data-stu-id="81dc5-117">More connectors</span></span>
<span data-ttu-id="81dc5-118">Lépjen vissza a [API-k lista](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="81dc5-118">Go back to the [APIs list](apis-list.md).</span></span>