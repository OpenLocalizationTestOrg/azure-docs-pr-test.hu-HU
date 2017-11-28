---
title: "aaaAdd hello Office 365-felhasználók összekötőt a Logic Apps |} Microsoft Docs"
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
ms.openlocfilehash: 2fae1c80d195a368b5f6c1ad650905a0d6e94c83
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-office-365-users-connector"></a><span data-ttu-id="2a521-103">Hello Office 365-felhasználók összekötő az első lépései</span><span class="sxs-lookup"><span data-stu-id="2a521-103">Get started with hello Office 365 Users connector</span></span>
<span data-ttu-id="2a521-104">Csatlakozás tooOffice 365 felhasználók tooget profilok, a keresés felhasználói és egyebek.</span><span class="sxs-lookup"><span data-stu-id="2a521-104">Connect tooOffice 365 Users tooget profiles, search users, and more.</span></span> <span data-ttu-id="2a521-105">Az Office 365-felhasználók a következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="2a521-105">With Office 365 Users, you can:</span></span>

* <span data-ttu-id="2a521-106">Az Office 365 felhasználóktól kapott hello adatok alapján üzleti folyamatot létrehozni.</span><span class="sxs-lookup"><span data-stu-id="2a521-106">Build your business flow based on hello data you get from Office 365 Users.</span></span> 
* <span data-ttu-id="2a521-107">Közvetlen beosztottai első használata műveleteket vezető felhasználói profil, és több kapják meg.</span><span class="sxs-lookup"><span data-stu-id="2a521-107">Use actions that get direct reports, get a manager's user profile, and more.</span></span> <span data-ttu-id="2a521-108">Ezeket a műveleteket válaszol, és végezze el hello kimeneti más műveletek érhető el.</span><span class="sxs-lookup"><span data-stu-id="2a521-108">These actions get a response, and then make hello output available for other actions.</span></span> <span data-ttu-id="2a521-109">Például lekérni egy személy közvetlen beosztottai, majd ezt az adatot és SQL Azure-adatbázis frissítéséhez.</span><span class="sxs-lookup"><span data-stu-id="2a521-109">For example, get a person's direct reports, and then take this information and update a SQL Azure database.</span></span> 

<span data-ttu-id="2a521-110">Most hozzon létre egy logic app kezdheti, lásd: [logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="2a521-110">You can get started by creating a logic app now, see [Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="create-a-connection-toooffice-365-users"></a><span data-ttu-id="2a521-111">Kapcsolat tooOffice 365 felhasználók létrehozása</span><span class="sxs-lookup"><span data-stu-id="2a521-111">Create a connection tooOffice 365 Users</span></span>
<span data-ttu-id="2a521-112">Az összekötő tooyour a logic apps hozzáadásakor kell bejelentkezés tooyour Office 365 felhasználói fiókot, és a logic apps tooconnect tooyour fiók használatának engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="2a521-112">When you add this connector tooyour logic apps, you must sign-in tooyour Office 365 Users account and allow logic apps tooconnect tooyour account.</span></span>

> [!INCLUDE [Steps toocreate a connection tooOffice 365 Users](../../includes/connectors-create-api-office365users.md)]
> 
> 

<span data-ttu-id="2a521-113">Hello kapcsolat létrehozása után meg hello Office 365-felhasználók tulajdonságai, például hello felhasználói azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="2a521-113">After you create hello connection, you enter hello Office 365 Users properties, like hello user ID.</span></span> <span data-ttu-id="2a521-114">Hello **REST API-referenciában** a témakörben ismertetett ezeket a tulajdonságokat.</span><span class="sxs-lookup"><span data-stu-id="2a521-114">hello **REST API reference** in this topic describes these properties.</span></span>

## <a name="connector-specific-details"></a><span data-ttu-id="2a521-115">Összekötő-specifikus részletei</span><span class="sxs-lookup"><span data-stu-id="2a521-115">Connector-specific details</span></span>

<span data-ttu-id="2a521-116">Bármely eseményindítók és hello swagger definiált műveletek megtekintése, és semmilyen határnak hello a Lásd még: [connector részleteket](/connectors/officeusers/).</span><span class="sxs-lookup"><span data-stu-id="2a521-116">View any triggers and actions defined in hello swagger, and also see any limits in hello [connector details](/connectors/officeusers/).</span></span>

## <a name="more-connectors"></a><span data-ttu-id="2a521-117">További összekötők</span><span class="sxs-lookup"><span data-stu-id="2a521-117">More connectors</span></span>
<span data-ttu-id="2a521-118">Lépjen vissza toohello [API-k lista](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="2a521-118">Go back toohello [APIs list](apis-list.md).</span></span>
