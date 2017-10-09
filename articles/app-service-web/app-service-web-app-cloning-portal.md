---
title: "aaaWeb App Klónozás Azure portál használatával"
description: "Megtudhatja, hogyan tooclone a Web Apps toonew webalkalmazások Azure portál használatával."
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
ms.openlocfilehash: 605c4879f34d568e9981c34109f9496731c9ed18
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-app-service-app-cloning-using-azure-portal"></a><span data-ttu-id="ad00b-103">Azure App Service alkalmazás Klónozás Azure-portál használatával</span><span class="sxs-lookup"><span data-stu-id="ad00b-103">Azure App Service App Cloning Using Azure Portal</span></span>
<span data-ttu-id="ad00b-104">a szolgáltatás a Klónozás hello [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714) megadható, hogy könnyen klónozza a meglévő alkalmazások tooa az újonnan létrehozott webalkalmazás egy másik régióban található, vagy a hello ugyanabban a régióban.</span><span class="sxs-lookup"><span data-stu-id="ad00b-104">hello cloning feature in [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714) lets you easily clone existing web apps tooa newly created app in a different region or in hello same region.</span></span> <span data-ttu-id="ad00b-105">Ezzel a lépéssel engedélyezi az ügyfelek toodeploy alkalmazások számos különböző régiókban között gyors és egyszerű.</span><span class="sxs-lookup"><span data-stu-id="ad00b-105">This will enable customers toodeploy a number of apps across different regions quickly and easily.</span></span>

<span data-ttu-id="ad00b-106">Alkalmazás Klónozás jelenleg csak a premium szint app service-csomagokról támogatott.</span><span class="sxs-lookup"><span data-stu-id="ad00b-106">App cloning is currently only supported for premium tier app service plans.</span></span> <span data-ttu-id="ad00b-107">új hello szolgáltatás által használt azonos hello korlátozások Web Apps biztonsági másolat szolgáltatás, lásd: [készítsen biztonsági másolatot egy webalkalmazást az Azure App Service](web-sites-backup.md).</span><span class="sxs-lookup"><span data-stu-id="ad00b-107">hello new feature uses hello same limitations as Web Apps Backup feature, see [Back up a web app in Azure App Service](web-sites-backup.md).</span></span>

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="cloning-an-existing-app"></a><span data-ttu-id="ad00b-108">A Klónozás egy meglévő alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="ad00b-108">Cloning an existing App</span></span>
<span data-ttu-id="ad00b-109">hello webalkalmazás futnia kell a hello **prémium** ahhoz, hogy a klónozott hello webalkalmazás toocreate mód.</span><span class="sxs-lookup"><span data-stu-id="ad00b-109">hello web app must be running in hello **Premium** mode in order for you toocreate a clone for hello web app.</span></span>

1. <span data-ttu-id="ad00b-110">A hello [Azure Portal](https://portal.azure.com/), nyissa meg a webalkalmazás panelen.</span><span class="sxs-lookup"><span data-stu-id="ad00b-110">In hello [Azure Portal](https://portal.azure.com/), open your web app's blade.</span></span>
2. <span data-ttu-id="ad00b-111">Kattintson a **eszközök**.</span><span class="sxs-lookup"><span data-stu-id="ad00b-111">Click **Tools**.</span></span> <span data-ttu-id="ad00b-112">Ezt követően a hello **eszközök** panelen kattintson a **Klónozott alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="ad00b-112">Then, in hello **Tools** blade, click **Clone App**.</span></span>
   
    ![][1]
   
   > [!NOTE]
   > <span data-ttu-id="ad00b-113">Ha hello webes alkalmazás nem szerepel a hello **prémium** mód, kapni fog egy üzenetet támogatott hello módjainak app Klónozás.</span><span class="sxs-lookup"><span data-stu-id="ad00b-113">If hello web app is not already in hello **Premium** mode, you will receive a message indicating hello supported modes for app cloning.</span></span> <span data-ttu-id="ad00b-114">Ezen a ponton rendelkezik hello beállítás tooselect **frissítése**.</span><span class="sxs-lookup"><span data-stu-id="ad00b-114">At this point, you have hello option tooselect **Upgrade**.</span></span>
   > 
   > 
3. <span data-ttu-id="ad00b-115">A hello **Klónozott alkalmazás** panelen adja meg a hello új webalkalmazást, az erőforráscsoport és az App Service-csomag nevét.</span><span class="sxs-lookup"><span data-stu-id="ad00b-115">In hello **Clone App** blade provide a name of hello new web app, Resource Group, and App Service Plan.</span></span> <span data-ttu-id="ad00b-116">Is hello felhasználó fog tudni toochoose e forrást a webalkalmazás-beállítások több tooclone vagy sem.</span><span class="sxs-lookup"><span data-stu-id="ad00b-116">Also hello user will be able toochoose whether tooclone a number of source web app settings or not.</span></span>
   
    ![][2]
4. <span data-ttu-id="ad00b-117">Miután rákattintott **létrehozása** hello platform kezdő hello forrás webalkalmazás a klón létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="ad00b-117">After clicking **create** hello platform will start working on creating a clone of hello source web app.</span></span>

## <a name="cloning-an-existing-app-tooan-app-service-environment"></a><span data-ttu-id="ad00b-118">A Klónozás egy meglévő App tooan App Service Environment-környezet</span><span class="sxs-lookup"><span data-stu-id="ad00b-118">Cloning an existing App tooan App Service Environment</span></span>
<span data-ttu-id="ad00b-119">A hello **Klónozott alkalmazás** panel hello ügyfél fog rendelkezni a hello beállítás toochoose egy alkalmazáskészlet a meglévő App Service-környezet.</span><span class="sxs-lookup"><span data-stu-id="ad00b-119">In hello **Clone App** blade hello customer will have hello option toochoose an app pool in an existing App Service Environment.</span></span>

## <a name="current-restrictions"></a><span data-ttu-id="ad00b-120">Aktuális korlátozások</span><span class="sxs-lookup"><span data-stu-id="ad00b-120">Current Restrictions</span></span>
<span data-ttu-id="ad00b-121">Ez a funkció jelenleg előzetes verzióban érhetők, jelenleg is dolgozunk tooadd új képességeket adott idő alatt, a következő lista hello vannak hello alkalmazás Azure-portálon Klónozás hello aktuális támogatási ismert korlátozásai:</span><span class="sxs-lookup"><span data-stu-id="ad00b-121">This feature is currently in preview, we are working tooadd new capabilities over time, hello following list are hello known restrictions on hello current support of app cloning in Azure Portal:</span></span>

* <span data-ttu-id="ad00b-122">Az Azure Traffic Manager-beállításai nem klónozható vannak</span><span class="sxs-lookup"><span data-stu-id="ad00b-122">Azure Traffic Manager settings are not cloned</span></span>
* <span data-ttu-id="ad00b-123">Automatikus skálázási beállításokat a rendszer nem klónozható</span><span class="sxs-lookup"><span data-stu-id="ad00b-123">Auto scale settings are not cloned</span></span>
* <span data-ttu-id="ad00b-124">Biztonsági mentés ütemezése beállítások vannak nem klónozható</span><span class="sxs-lookup"><span data-stu-id="ad00b-124">Backup schedule settings are not cloned</span></span>
* <span data-ttu-id="ad00b-125">Vannak nem klónozható a VNET beállításait</span><span class="sxs-lookup"><span data-stu-id="ad00b-125">VNET settings are not cloned</span></span>
* <span data-ttu-id="ad00b-126">App Insights nem automatikusan be vannak állítva hello cél webalkalmazásban</span><span class="sxs-lookup"><span data-stu-id="ad00b-126">App Insights are not automatically set up on hello destination web app</span></span>
* <span data-ttu-id="ad00b-127">Egyszerű hitelesítési beállítások a rendszer nem klónozható</span><span class="sxs-lookup"><span data-stu-id="ad00b-127">Easy Auth settings are not cloned</span></span>
* <span data-ttu-id="ad00b-128">A kudu bővítmény nem klónozható vannak</span><span class="sxs-lookup"><span data-stu-id="ad00b-128">Kudu Extension are not cloned</span></span>
* <span data-ttu-id="ad00b-129">Tipp szabályok nem klónozható vannak</span><span class="sxs-lookup"><span data-stu-id="ad00b-129">TiP rules are not cloned</span></span>
* <span data-ttu-id="ad00b-130">Adatbázis-tartalom nem klónozható vannak</span><span class="sxs-lookup"><span data-stu-id="ad00b-130">Database content are not cloned</span></span>

### <a name="references"></a><span data-ttu-id="ad00b-131">Referencia</span><span class="sxs-lookup"><span data-stu-id="ad00b-131">References</span></span>
* [<span data-ttu-id="ad00b-132">Webes alkalmazás Klónozás PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="ad00b-132">Web App Cloning using PowerShell</span></span>](app-service-web-app-cloning.md)
* [<span data-ttu-id="ad00b-133">Készítsen biztonsági másolatot egy webalkalmazást az Azure App Service-ben</span><span class="sxs-lookup"><span data-stu-id="ad00b-133">Back up a web app in Azure App Service</span></span>](web-sites-backup.md)
* [<span data-ttu-id="ad00b-134">Hogyan tooCreate egy App Service Environment-környezet</span><span class="sxs-lookup"><span data-stu-id="ad00b-134">How tooCreate an App Service Environment</span></span>](app-service-web-how-to-create-an-app-service-environment.md)
* [<span data-ttu-id="ad00b-135">Webalkalmazás létrehozása App Service Environment-környezetben</span><span class="sxs-lookup"><span data-stu-id="ad00b-135">Create a web app in an App Service Environment</span></span>](app-service-web-how-to-create-a-web-app-in-an-ase.md)
* [<span data-ttu-id="ad00b-136">Bevezetés tooApp Service-környezet</span><span class="sxs-lookup"><span data-stu-id="ad00b-136">Introduction tooApp Service Environment</span></span>](app-service-app-service-environment-intro.md)

<!--Image references-->
[1]: ./media/app-service-web-app-cloning-portal/CloningBlade.png
[2]: ./media/app-service-web-app-cloning-portal/CloneSettings.png
