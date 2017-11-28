---
title: "Webes alkalmazás Klónozás Azure portál használatával"
description: "Megtudhatja, hogyan klónozhatja a webalkalmazások Azure portál segítségével új webes alkalmazásokhoz."
services: app-service\web
documentationcenter: 
author: ahmedelnably
manager: stefsch
editor: 
ms.assetid: 20b0ae4e-67e8-4bae-9d74-8a24dc445cce
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/08/2016
ms.author: aelnably
ms.openlocfilehash: 9ebfa91c7972ab3c264032ead8376c23c1197a4b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-app-service-app-cloning-using-azure-portal"></a><span data-ttu-id="1a3d2-103">Azure App Service alkalmazás Klónozás Azure-portál használatával</span><span class="sxs-lookup"><span data-stu-id="1a3d2-103">Azure App Service App Cloning Using Azure Portal</span></span>
<span data-ttu-id="1a3d2-104">A Klónozási szolgáltatásának [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714) megadható, hogy könnyen klónozását meglévő webalkalmazások egy újonnan létrehozott alkalmazást, egy másik régióban vagy ugyanabban a régióban.</span><span class="sxs-lookup"><span data-stu-id="1a3d2-104">The cloning feature in [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714) lets you easily clone existing web apps to a newly created app in a different region or in the same region.</span></span> <span data-ttu-id="1a3d2-105">Ez lehetővé teszi az ügyfelek központi telepítése a alkalmazások számos különböző régiókban teljes gyorsan és egyszerűen.</span><span class="sxs-lookup"><span data-stu-id="1a3d2-105">This will enable customers to deploy a number of apps across different regions quickly and easily.</span></span>

<span data-ttu-id="1a3d2-106">Alkalmazás Klónozás jelenleg csak a premium szint app service-csomagokról támogatott.</span><span class="sxs-lookup"><span data-stu-id="1a3d2-106">App cloning is currently only supported for premium tier app service plans.</span></span> <span data-ttu-id="1a3d2-107">Az új szolgáltatás ugyanazokkal a korlátozásokkal használja, mint a Web Apps biztonsági másolat szolgáltatás című [készítsen biztonsági másolatot egy webalkalmazást az Azure App Service](web-sites-backup.md).</span><span class="sxs-lookup"><span data-stu-id="1a3d2-107">The new feature uses the same limitations as Web Apps Backup feature, see [Back up a web app in Azure App Service](web-sites-backup.md).</span></span>

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="cloning-an-existing-app"></a><span data-ttu-id="1a3d2-108">A Klónozás egy meglévő alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="1a3d2-108">Cloning an existing App</span></span>
<span data-ttu-id="1a3d2-109">A webalkalmazás futniuk kell a **prémium szintű** ahhoz, hogy a webalkalmazás klónozhatja mód.</span><span class="sxs-lookup"><span data-stu-id="1a3d2-109">The web app must be running in the **Premium** mode in order for you to create a clone for the web app.</span></span>

1. <span data-ttu-id="1a3d2-110">Az a [Azure Portal](https://portal.azure.com/), nyissa meg a webalkalmazás panelen.</span><span class="sxs-lookup"><span data-stu-id="1a3d2-110">In the [Azure Portal](https://portal.azure.com/), open your web app's blade.</span></span>
2. <span data-ttu-id="1a3d2-111">Kattintson a **eszközök**.</span><span class="sxs-lookup"><span data-stu-id="1a3d2-111">Click **Tools**.</span></span> <span data-ttu-id="1a3d2-112">Ezt követően a a **eszközök** panelen kattintson a **Klónozott alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="1a3d2-112">Then, in the **Tools** blade, click **Clone App**.</span></span>
   
    ![][1]
   
   > [!NOTE]
   > <span data-ttu-id="1a3d2-113">Ha a webalkalmazás már nem része a **prémium** mód, kapni fog egy üzenetet, a támogatott módok app Klónozás.</span><span class="sxs-lookup"><span data-stu-id="1a3d2-113">If the web app is not already in the **Premium** mode, you will receive a message indicating the supported modes for app cloning.</span></span> <span data-ttu-id="1a3d2-114">Ezen a ponton rendelkezik választhatja **frissítése**.</span><span class="sxs-lookup"><span data-stu-id="1a3d2-114">At this point, you have the option to select **Upgrade**.</span></span>
   > 
   > 
3. <span data-ttu-id="1a3d2-115">Az a **Klónozott alkalmazás** panelen adja meg egy nevet az új webalkalmazásba, a erőforráscsoport és az App Service-csomag.</span><span class="sxs-lookup"><span data-stu-id="1a3d2-115">In the **Clone App** blade provide a name of the new web app, Resource Group, and App Service Plan.</span></span> <span data-ttu-id="1a3d2-116">A felhasználó fog is, hogy a klón forrás a webalkalmazás-beállítások több e vagy sem.</span><span class="sxs-lookup"><span data-stu-id="1a3d2-116">Also the user will be able to choose whether to clone a number of source web app settings or not.</span></span>
   
    ![][2]
4. <span data-ttu-id="1a3d2-117">Miután rákattintott **létrehozása** a platform kezdő a forrás-webalkalmazás a klón létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="1a3d2-117">After clicking **create** the platform will start working on creating a clone of the source web app.</span></span>

## <a name="cloning-an-existing-app-to-an-app-service-environment"></a><span data-ttu-id="1a3d2-118">Egy meglévő alkalmazást az App Service-környezetek való klónozása</span><span class="sxs-lookup"><span data-stu-id="1a3d2-118">Cloning an existing App to an App Service Environment</span></span>
<span data-ttu-id="1a3d2-119">Az a **Klónozott alkalmazás** panel az ügyfél van egy meglévő App Service Environment-környezet egy alkalmazáskészlet kiválasztásához.</span><span class="sxs-lookup"><span data-stu-id="1a3d2-119">In the **Clone App** blade the customer will have the option to choose an app pool in an existing App Service Environment.</span></span>

## <a name="current-restrictions"></a><span data-ttu-id="1a3d2-120">Aktuális korlátozások</span><span class="sxs-lookup"><span data-stu-id="1a3d2-120">Current Restrictions</span></span>
<span data-ttu-id="1a3d2-121">Ez a funkció jelenleg előzetes verzióban érhetők, új képességeket adhat a időbeli dolgozunk, a következő lista rendszer app klónozást Azure-portálon az aktuális támogatási ismert korlátozásai:</span><span class="sxs-lookup"><span data-stu-id="1a3d2-121">This feature is currently in preview, we are working to add new capabilities over time, the following list are the known restrictions on the current support of app cloning in Azure Portal:</span></span>

* <span data-ttu-id="1a3d2-122">Az Azure Traffic Manager-beállításai nem klónozható vannak</span><span class="sxs-lookup"><span data-stu-id="1a3d2-122">Azure Traffic Manager settings are not cloned</span></span>
* <span data-ttu-id="1a3d2-123">Automatikus skálázási beállításokat a rendszer nem klónozható</span><span class="sxs-lookup"><span data-stu-id="1a3d2-123">Auto scale settings are not cloned</span></span>
* <span data-ttu-id="1a3d2-124">Biztonsági mentés ütemezése beállítások vannak nem klónozható</span><span class="sxs-lookup"><span data-stu-id="1a3d2-124">Backup schedule settings are not cloned</span></span>
* <span data-ttu-id="1a3d2-125">Vannak nem klónozható a VNET beállításait</span><span class="sxs-lookup"><span data-stu-id="1a3d2-125">VNET settings are not cloned</span></span>
* <span data-ttu-id="1a3d2-126">App Insights nem automatikusan be vannak állítva a célként megadott webalkalmazásban</span><span class="sxs-lookup"><span data-stu-id="1a3d2-126">App Insights are not automatically set up on the destination web app</span></span>
* <span data-ttu-id="1a3d2-127">Egyszerű hitelesítési beállítások a rendszer nem klónozható</span><span class="sxs-lookup"><span data-stu-id="1a3d2-127">Easy Auth settings are not cloned</span></span>
* <span data-ttu-id="1a3d2-128">A kudu bővítmény nem klónozható vannak</span><span class="sxs-lookup"><span data-stu-id="1a3d2-128">Kudu Extension are not cloned</span></span>
* <span data-ttu-id="1a3d2-129">Tipp szabályok nem klónozható vannak</span><span class="sxs-lookup"><span data-stu-id="1a3d2-129">TiP rules are not cloned</span></span>
* <span data-ttu-id="1a3d2-130">Adatbázis-tartalom nem klónozható vannak</span><span class="sxs-lookup"><span data-stu-id="1a3d2-130">Database content are not cloned</span></span>

### <a name="references"></a><span data-ttu-id="1a3d2-131">Referencia</span><span class="sxs-lookup"><span data-stu-id="1a3d2-131">References</span></span>
* [<span data-ttu-id="1a3d2-132">Webes alkalmazás Klónozás PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="1a3d2-132">Web App Cloning using PowerShell</span></span>](app-service-web-app-cloning.md)
* [<span data-ttu-id="1a3d2-133">Készítsen biztonsági másolatot egy webalkalmazást az Azure App Service-ben</span><span class="sxs-lookup"><span data-stu-id="1a3d2-133">Back up a web app in Azure App Service</span></span>](web-sites-backup.md)
* [<span data-ttu-id="1a3d2-134">App Service Environment létrehozása</span><span class="sxs-lookup"><span data-stu-id="1a3d2-134">How to Create an App Service Environment</span></span>](app-service-web-how-to-create-an-app-service-environment.md)
* [<span data-ttu-id="1a3d2-135">Webalkalmazás létrehozása App Service Environment-környezetben</span><span class="sxs-lookup"><span data-stu-id="1a3d2-135">Create a web app in an App Service Environment</span></span>](app-service-web-how-to-create-a-web-app-in-an-ase.md)
* [<span data-ttu-id="1a3d2-136">Az App Service Environment bemutatása</span><span class="sxs-lookup"><span data-stu-id="1a3d2-136">Introduction to App Service Environment</span></span>](app-service-app-service-environment-intro.md)

<!--Image references-->
[1]: ./media/app-service-web-app-cloning-portal/CloningBlade.png
[2]: ./media/app-service-web-app-cloning-portal/CloneSettings.png
