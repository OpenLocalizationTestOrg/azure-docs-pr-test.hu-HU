---
title: "Be annak az Azure ExpressRoute Microsoft társviszony-létesítés: portál |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan tooconfigure útvonal szűrők Microsoft Peering használatára vonatkozó hello Azure-portálon"
documentationcenter: na
services: expressroute
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/25/2017
ms.author: ganesr;cherylmc
ms.openlocfilehash: 2a47d465ec5f175d9510cef94606f70f036f0862
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-route-filters-for-microsoft-peering"></a><span data-ttu-id="ac801-103">Microsoft társviszony-létesítés útvonalszűrőinek konfigurálása</span><span class="sxs-lookup"><span data-stu-id="ac801-103">Configure route filters for Microsoft peering</span></span>

<span data-ttu-id="ac801-104">Útvonal-szűrők olyan módon tooconsume a Microsoft társviszony-létesítés keresztül támogatott szolgáltatások egy részhalmaza.</span><span class="sxs-lookup"><span data-stu-id="ac801-104">Route filters are a way tooconsume a subset of supported services through Microsoft peering.</span></span> <span data-ttu-id="ac801-105">Ez a cikk a Súgó konfigurálását és felügyeletét útvonal szűrők az ExpressRoute-Kapcsolatcsoportok hello szükséges lépések.</span><span class="sxs-lookup"><span data-stu-id="ac801-105">hello steps in this article help you configure and manage route filters for ExpressRoute circuits.</span></span>

<span data-ttu-id="ac801-106">Dynamics 365 szolgáltatások és a vállalati, például az Exchange Online, SharePoint Online és Skype Office 365-szolgáltatásokhoz hello Microsoft társviszony-létesítés keresztül érhetők el.</span><span class="sxs-lookup"><span data-stu-id="ac801-106">Dynamics 365 services, and Office 365 services such as Exchange Online, SharePoint Online, and Skype for Business, are accessible through hello Microsoft peering.</span></span> <span data-ttu-id="ac801-107">Ha a Microsoft társviszony-létesítést az ExpressRoute-kapcsolatcsoportot van konfigurálva, az összes előtagok kapcsolódó toothese szolgáltatás van-e hirdetve keresztül létesített hello BGP-munkamenetek.</span><span class="sxs-lookup"><span data-stu-id="ac801-107">When Microsoft peering is configured in an ExpressRoute circuit, all prefixes related toothese services are advertised through hello BGP sessions that are established.</span></span> <span data-ttu-id="ac801-108">A BGP közösségi értéke csatolt tooevery előtag tooidentify hello ajánlott szolgáltatás hello előtag keresztül.</span><span class="sxs-lookup"><span data-stu-id="ac801-108">A BGP community value is attached tooevery prefix tooidentify hello service that is offered through hello prefix.</span></span> <span data-ttu-id="ac801-109">Hello BGP logikai értékeket és rendeli hello szolgáltatások listáját lásd: [BGP Közösségek](expressroute-routing.md#bgp).</span><span class="sxs-lookup"><span data-stu-id="ac801-109">For a list of hello BGP community values and hello services they  map to, see [BGP communities](expressroute-routing.md#bgp).</span></span>

<span data-ttu-id="ac801-110">Ha a kapcsolat tooall szolgáltatások van szüksége, nagyszámú előtagok van-e hirdetve BGP keresztül.</span><span class="sxs-lookup"><span data-stu-id="ac801-110">If you require connectivity tooall services, a large number of prefixes are advertised through BGP.</span></span> <span data-ttu-id="ac801-111">Ez jelentősen növeli a belül a hálózati útválasztók által fenntartott hello útvonaltáblák hello méretét.</span><span class="sxs-lookup"><span data-stu-id="ac801-111">This significantly increases hello size of hello route tables maintained by routers within your network.</span></span> <span data-ttu-id="ac801-112">Ha azt tervezi, hogy csak a szolgáltatások egy részhalmaza kínált Microsoft társviszony-létesítés tooconsume, csökkentheti az útvonaltáblák kétféleképpen hello méretét.</span><span class="sxs-lookup"><span data-stu-id="ac801-112">If you plan tooconsume only a subset of services offered through Microsoft peering, you can reduce hello size of your route tables in two ways.</span></span> <span data-ttu-id="ac801-113">A következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="ac801-113">You can:</span></span>

- <span data-ttu-id="ac801-114">Nem kívánt előtagok szűrheti a BGP Közösségek útvonal szűrők alkalmazásával.</span><span class="sxs-lookup"><span data-stu-id="ac801-114">Filter out unwanted prefixes by applying route filters on BGP communities.</span></span> <span data-ttu-id="ac801-115">Ez egy szabványos hálózatkezelési eljárás, és sok hálózatok általában arra használják.</span><span class="sxs-lookup"><span data-stu-id="ac801-115">This is a standard networking practice and is used commonly within many networks.</span></span>

- <span data-ttu-id="ac801-116">Útvonal-szűrők, és alkalmazni azokat tooyour ExpressRoute-kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="ac801-116">Define route filters and apply them tooyour ExpressRoute circuit.</span></span> <span data-ttu-id="ac801-117">Útvonal szűrő egy új erőforrást kiválaszthatja hello listája szolgáltatások, tervezze meg a Microsoft társviszony-létesítés tooconsume.</span><span class="sxs-lookup"><span data-stu-id="ac801-117">A route filter is a new resource that lets you select hello list of services you plan tooconsume through Microsoft peering.</span></span> <span data-ttu-id="ac801-118">Csak az ExpressRoute útválasztók továbbítják hello útvonal szűrő toohello szolgáltatás tartozó előtagok hello listája.</span><span class="sxs-lookup"><span data-stu-id="ac801-118">ExpressRoute routers only send hello list of prefixes that belong toohello services identified in hello route filter.</span></span>

### <span data-ttu-id="ac801-119"><a name="about"></a>Útvonal-szűrők</span><span class="sxs-lookup"><span data-stu-id="ac801-119"><a name="about"></a>About route filters</span></span>

<span data-ttu-id="ac801-120">Ha a Microsoft társviszony-létesítés konfigurálva van az ExpressRoute-kapcsolatcsoportot, hello Microsoft peremhálózati útválasztók létesíteni két BGP-munkameneteket indíthat más hello peremhálózati útválasztók (saját vagy a kapcsolat szolgáltatóját).</span><span class="sxs-lookup"><span data-stu-id="ac801-120">When Microsoft peering is configured on your ExpressRoute circuit, hello Microsoft edge routers establish a pair of BGP sessions with hello edge routers (yours or your connectivity provider's).</span></span> <span data-ttu-id="ac801-121">Nincs útvonal hirdetett tooyour hálózati.</span><span class="sxs-lookup"><span data-stu-id="ac801-121">No routes are advertised tooyour network.</span></span> <span data-ttu-id="ac801-122">tooenable útvonal hirdetmények tooyour hálózati, társítania kell egy útvonal-szűrőt.</span><span class="sxs-lookup"><span data-stu-id="ac801-122">tooenable route advertisements tooyour network, you must associate a route filter.</span></span>

<span data-ttu-id="ac801-123">Útvonal szűrő lehetővé teszi azt szeretné, hogy az ExpressRoute-kapcsolatcsoportot Microsoft társviszony-létesítés keresztül tooconsume szolgáltatás azonosítására.</span><span class="sxs-lookup"><span data-stu-id="ac801-123">A route filter lets you identify services you want tooconsume through your ExpressRoute circuit's Microsoft peering.</span></span> <span data-ttu-id="ac801-124">Lényegében egy fehér lista hello BGP közösségi értékek is.</span><span class="sxs-lookup"><span data-stu-id="ac801-124">It is essentially a white list of all hello BGP community values.</span></span> <span data-ttu-id="ac801-125">Amikor egy útvonal szűrő erőforrás van definiálva, és tooan ExpressRoute-kapcsolatcsoportot csatlakoztatva, összes toohello BGP közösségi értékek megfeleltetni előtagok hirdetett tooyour hálózati.</span><span class="sxs-lookup"><span data-stu-id="ac801-125">Once a route filter resource is defined and attached tooan ExpressRoute circuit, all prefixes that map toohello BGP community values are advertised tooyour network.</span></span>

<span data-ttu-id="ac801-126">toobe képes tooattach útvonal szűrők az Office 365 szolgáltatásaival rajtuk, rendelkeznie kell engedélyezési tooconsume Office 365-szolgáltatásokhoz ExpressRoute keresztül.</span><span class="sxs-lookup"><span data-stu-id="ac801-126">toobe able tooattach route filters with Office 365 services on them, you must have authorization tooconsume Office 365 services through ExpressRoute.</span></span> <span data-ttu-id="ac801-127">Ha nem engedélyezett tooconsume Office 365-szolgáltatások díjairól ExpressRoute, hello művelet tooattach útvonal szűrők sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="ac801-127">If you are not authorized tooconsume Office 365 services through ExpressRoute, hello operation tooattach route filters fails.</span></span> <span data-ttu-id="ac801-128">Hello engedélyezési folyamattal kapcsolatos további információkért lásd: [Azure ExpressRoute az Office 365](https://support.office.com/article/Azure-ExpressRoute-for-Office-365-6d2534a2-c19c-4a99-be5e-33a0cee5d3bd).</span><span class="sxs-lookup"><span data-stu-id="ac801-128">For more information about hello authorization process, see [Azure ExpressRoute for Office 365](https://support.office.com/article/Azure-ExpressRoute-for-Office-365-6d2534a2-c19c-4a99-be5e-33a0cee5d3bd).</span></span> <span data-ttu-id="ac801-129">Kapcsolat tooDynamics 365 használatához nem szükséges az előzetes engedélyek.</span><span class="sxs-lookup"><span data-stu-id="ac801-129">Connectivity tooDynamics 365 services does NOT require any prior authorization.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ac801-130">Előzetes tooAugust 1 Microsoft társviszony-létesítést az ExpressRoute-Kapcsolatcsoportok volt beállítva, akkor 2017 fog rendelkezni a Microsoft társviszony-létesítést, meghirdetett összes szolgáltatás előtagot, akkor is, ha nincsenek megadva útvonal szűrők.</span><span class="sxs-lookup"><span data-stu-id="ac801-130">Microsoft peering of ExpressRoute circuits that were configured prior tooAugust 1, 2017 will have all service prefixes advertised through Microsoft peering, even if route filters are not defined.</span></span> <span data-ttu-id="ac801-131">A Microsoft társviszony-létesítést az ExpressRoute-Kapcsolatcsoportok vannak konfigurálva, vagy azt követően 2017. augusztus 1. nem rendelkezik a előtagokat meghirdetett, amíg egy útvonal-szűrő nem csatlakoztatja toohello körön.</span><span class="sxs-lookup"><span data-stu-id="ac801-131">Microsoft peering of ExpressRoute circuits that are configured on or after August 1, 2017 will not have any prefixes advertised until a route filter is attached toohello circuit.</span></span>
> 
> 

### <span data-ttu-id="ac801-132"><a name="workflow"></a>Munkafolyamat</span><span class="sxs-lookup"><span data-stu-id="ac801-132"><a name="workflow"></a>Workflow</span></span>

<span data-ttu-id="ac801-133">toobe képes toosuccessfully tooservices csatlakozhat a Microsoft társviszony-létesítést, végre kell hajtania a következő konfigurációs lépések hello:</span><span class="sxs-lookup"><span data-stu-id="ac801-133">toobe able toosuccessfully connect tooservices through Microsoft peering, you must complete hello following configuration steps:</span></span>

- <span data-ttu-id="ac801-134">Rendelkeznie kell egy aktív van a Microsoft társviszony kiosztott ExpressRoute-kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="ac801-134">You must have an active ExpressRoute circuit that has Microsoft peering provisioned.</span></span> <span data-ttu-id="ac801-135">A következő utasításokat tooaccomplish hello használhatja ezeket a feladatokat:</span><span class="sxs-lookup"><span data-stu-id="ac801-135">You can use hello following instructions tooaccomplish these tasks:</span></span>
  - <span data-ttu-id="ac801-136">[ExpressRoute-kapcsolatcsoportot létrehozni](expressroute-howto-circuit-portal-resource-manager.md) , és folytassa a kapcsolat szolgáltatójánál előtt által engedélyezett hello áramkör.</span><span class="sxs-lookup"><span data-stu-id="ac801-136">[Create an ExpressRoute circuit](expressroute-howto-circuit-portal-resource-manager.md) and have hello circuit enabled by your connectivity provider before you proceed.</span></span> <span data-ttu-id="ac801-137">hello ExpressRoute-kapcsolatcsoportot kiépített és engedélyezett állapotban kell lennie.</span><span class="sxs-lookup"><span data-stu-id="ac801-137">hello ExpressRoute circuit must be in a provisioned and enabled state.</span></span>
  - <span data-ttu-id="ac801-138">[Hozzon létre a Microsoft társviszony-létesítés](expressroute-howto-routing-portal-resource-manager.md) hello BGP-közvetlenül munkamenet kezelése.</span><span class="sxs-lookup"><span data-stu-id="ac801-138">[Create Microsoft peering](expressroute-howto-routing-portal-resource-manager.md) if you manage hello BGP session directly.</span></span> <span data-ttu-id="ac801-139">Vagy a kapcsolat szolgáltatójánál rendelkezik a kör társviszony Microsoft kiépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="ac801-139">Or, have your connectivity provider provision Microsoft peering for your circuit.</span></span>

-  <span data-ttu-id="ac801-140">Hozzon létre és útvonal-szűrő konfigurálnia kell.</span><span class="sxs-lookup"><span data-stu-id="ac801-140">You must create and configure a route filter.</span></span>
    - <span data-ttu-id="ac801-141">Azonosítsa hello szolgáltatásokhoz, a Microsoft társviszony-létesítés keresztül tooconsume</span><span class="sxs-lookup"><span data-stu-id="ac801-141">Identify hello services you with tooconsume through Microsoft peering</span></span>
    - <span data-ttu-id="ac801-142">Azonosítsa a BGP-Közösség értékek hello szolgáltatásokkal társított hello listája</span><span class="sxs-lookup"><span data-stu-id="ac801-142">Identify hello list of BGP community values associated with hello services</span></span>
    - <span data-ttu-id="ac801-143">Hozzon létre egy szabály tooallow hello előtag lista egyező hello BGP logikai értékek</span><span class="sxs-lookup"><span data-stu-id="ac801-143">Create a rule tooallow hello prefix list matching hello BGP community values</span></span>

-  <span data-ttu-id="ac801-144">Hello útvonal szűrő toohello ExpressRoute-kapcsolatcsoportot kell csatolnia.</span><span class="sxs-lookup"><span data-stu-id="ac801-144">You must attach hello route filter toohello ExpressRoute circuit.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="ac801-145">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="ac801-145">Before you begin</span></span>

<span data-ttu-id="ac801-146">A konfigurálás elkezdése előtt ellenőrizze a következő feltételek hello:</span><span class="sxs-lookup"><span data-stu-id="ac801-146">Before you begin configuration, make sure you meet hello following criteria:</span></span>

 - <span data-ttu-id="ac801-147">Felülvizsgálati hello [Előfeltételek](expressroute-prerequisites.md) és [munkafolyamatok](expressroute-workflows.md) konfigurálás elkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="ac801-147">Review hello [prerequisites](expressroute-prerequisites.md) and [workflows](expressroute-workflows.md) before you begin configuration.</span></span>

 - <span data-ttu-id="ac801-148">Egy aktív ExpressRoute-kapcsolatcsoportra lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="ac801-148">You must have an active ExpressRoute circuit.</span></span> <span data-ttu-id="ac801-149">Útmutatás alapján hello túl[ExpressRoute-kapcsolatcsoportot létrehozni](expressroute-howto-circuit-portal-resource-manager.md) , és folytassa a kapcsolat szolgáltatójánál előtt által engedélyezett hello áramkör.</span><span class="sxs-lookup"><span data-stu-id="ac801-149">Follow hello instructions too[Create an ExpressRoute circuit](expressroute-howto-circuit-portal-resource-manager.md) and have hello circuit enabled by your connectivity provider before you proceed.</span></span> <span data-ttu-id="ac801-150">hello ExpressRoute-kapcsolatcsoportot kiépített és engedélyezett állapotban kell lennie.</span><span class="sxs-lookup"><span data-stu-id="ac801-150">hello ExpressRoute circuit must be in a provisioned and enabled state.</span></span>

 - <span data-ttu-id="ac801-151">Rendelkeznie kell egy aktív Microsoft társviszony-létesítés.</span><span class="sxs-lookup"><span data-stu-id="ac801-151">You must have an active Microsoft peering.</span></span> <span data-ttu-id="ac801-152">Kövesse az utasításokat, [létrehozása, és társviszony-létesítési konfigurációjának módosítása](expressroute-howto-routing-portal-resource-manager.md)</span><span class="sxs-lookup"><span data-stu-id="ac801-152">Follow instructions at [Create and modifying peering configuration](expressroute-howto-routing-portal-resource-manager.md)</span></span>


## <span data-ttu-id="ac801-153"><a name="prefixes"></a>1. lépés.</span><span class="sxs-lookup"><span data-stu-id="ac801-153"><a name="prefixes"></a>Step 1.</span></span> <span data-ttu-id="ac801-154">Előtagok és BGP közösségi értékek listájának beolvasása</span><span class="sxs-lookup"><span data-stu-id="ac801-154">Get a list of prefixes and BGP community values</span></span>

### <a name="1-get-a-list-of-bgp-community-values"></a><span data-ttu-id="ac801-155">1. BGP-Közösség értékek listájának beolvasása</span><span class="sxs-lookup"><span data-stu-id="ac801-155">1. Get a list of BGP community values</span></span>

<span data-ttu-id="ac801-156">Elérhető a Microsoft társviszony-létesítés szolgáltatásokkal társított BGP közösségi értékek érhető el hello [ExpressRoute útválasztási követelmények](expressroute-routing.md) lap.</span><span class="sxs-lookup"><span data-stu-id="ac801-156">BGP community values associated with services accessible through Microsoft peering is available in hello [ExpressRoute routing requirements](expressroute-routing.md) page.</span></span>

### <a name="2-make-a-list-of-hello-values-that-you-want-toouse"></a><span data-ttu-id="ac801-157">2. Ellenőrizze a hello értékből álló lista, amelyet az toouse</span><span class="sxs-lookup"><span data-stu-id="ac801-157">2. Make a list of hello values that you want toouse</span></span>

<span data-ttu-id="ac801-158">A BGP kívánt logikai értékek listáját toouse teszi hello útvonal szűrő.</span><span class="sxs-lookup"><span data-stu-id="ac801-158">Make a list of BGP community values you want toouse in hello route filter.</span></span> <span data-ttu-id="ac801-159">Tegyük fel hello BGP közösségi érték Dynamics 365 szolgáltatások 12076:5040.</span><span class="sxs-lookup"><span data-stu-id="ac801-159">As an example, hello BGP community value for Dynamics 365 services is 12076:5040.</span></span>

## <span data-ttu-id="ac801-160"><a name="filter"></a>2. lépés.</span><span class="sxs-lookup"><span data-stu-id="ac801-160"><a name="filter"></a>Step 2.</span></span> <span data-ttu-id="ac801-161">Útvonal-szűrő, ezért a szűrési szabály létrehozása</span><span class="sxs-lookup"><span data-stu-id="ac801-161">Create a route filter and a filter rule</span></span>

<span data-ttu-id="ac801-162">Útvonal szűrő lehet csak egy szabályt, és hello szabály a "Engedélyezés" típusúnak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="ac801-162">A route filter can have only one rule, and hello rule must be of type 'Allow'.</span></span> <span data-ttu-id="ac801-163">Ez a szabály társítva BGP közösségi értékből álló lista lehet.</span><span class="sxs-lookup"><span data-stu-id="ac801-163">This rule can have a list of BGP community values associated with it.</span></span>

### <a name="1-create-a-route-filter"></a><span data-ttu-id="ac801-164">1. Útvonal szűrő létrehozása</span><span class="sxs-lookup"><span data-stu-id="ac801-164">1. Create a route filter</span></span>
<span data-ttu-id="ac801-165">Létrehozhat egy útvonalat szűrőt hello beállítás toocreate kiválasztásával egy új erőforrást.</span><span class="sxs-lookup"><span data-stu-id="ac801-165">You can create a route filter by selecting hello option toocreate a new resource.</span></span> <span data-ttu-id="ac801-166">Kattintson a **új** > **hálózati** > **RouteFilter**, ahogy az a következő kép hello:</span><span class="sxs-lookup"><span data-stu-id="ac801-166">Click **New** > **Networking** > **RouteFilter**, as shown in hello following image:</span></span>

![Útvonal szűrő létrehozása](.\media\how-to-routefilter-portal\CreateRouteFilter1.png)

<span data-ttu-id="ac801-168">Hello útvonal szűrő erőforráscsoportban kell elhelyezni.</span><span class="sxs-lookup"><span data-stu-id="ac801-168">You must place hello route filter in a resource group.</span></span> 

![Útvonal szűrő létrehozása](.\media\how-to-routefilter-portal\CreateRouteFilter.png)

### <a name="2-create-a-filter-rule"></a><span data-ttu-id="ac801-170">2. Állapotszűrő szabály létrehozása</span><span class="sxs-lookup"><span data-stu-id="ac801-170">2. Create a filter rule</span></span>

<span data-ttu-id="ac801-171">Adhat hozzá, és frissítési szabályok hello kiválasztásával kezelheti a útvonal szűrési szabály lapján.</span><span class="sxs-lookup"><span data-stu-id="ac801-171">You can add and update rules by selecting hello manage rule tab for your route filter.</span></span>

![Útvonal szűrő létrehozása](.\media\how-to-routefilter-portal\ManageRouteFilter.png)


<span data-ttu-id="ac801-173">Kiválaszthatja a hello tooconnect toofrom hello kívánt szolgáltatásokban legördülő listából, és végzett hello szabály mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="ac801-173">You can select hello services you want tooconnect toofrom hello drop down list and save hello rule when done.</span></span>

![Útvonal szűrő létrehozása](.\media\how-to-routefilter-portal\AddRouteFilterRule.png)


## <span data-ttu-id="ac801-175"><a name="attach"></a>3. lépés.</span><span class="sxs-lookup"><span data-stu-id="ac801-175"><a name="attach"></a>Step 3.</span></span> <span data-ttu-id="ac801-176">Hello útvonal szűrő tooan ExpressRoute-kapcsolatcsoportot csatolása</span><span class="sxs-lookup"><span data-stu-id="ac801-176">Attach hello route filter tooan ExpressRoute circuit</span></span>

<span data-ttu-id="ac801-177">Csatolhat hello útvonal szűrő tooa kör kiválasztásával hello "kör felvétele" gombra, majd válassza a hello ExpressRoute-kapcsolatcsoportot hello legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="ac801-177">You can attach hello route filter tooa circuit by selecting hello "add Circuit" button and selecting hello ExpressRoute circuit from hello drop down list.</span></span>

![Útvonal szűrő létrehozása](.\media\how-to-routefilter-portal\AddCktToRouteFilter.png)

## <span data-ttu-id="ac801-179"><a name="getproperties"></a>egy útvonal szűrő tooget hello tulajdonságait</span><span class="sxs-lookup"><span data-stu-id="ac801-179"><a name="getproperties"></a>tooget hello properties of a route filter</span></span>

<span data-ttu-id="ac801-180">Hello erőforrás hello portál megnyitásakor egy útvonal szűrő tulajdonságait tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="ac801-180">You can view properties of a route filter when you open hello resource in hello portal.</span></span>

![Útvonal szűrő létrehozása](.\media\how-to-routefilter-portal\ViewRouteFilter.png)


## <span data-ttu-id="ac801-182"><a name="updateproperties"></a>egy útvonal szűrő tooupdate hello tulajdonságait</span><span class="sxs-lookup"><span data-stu-id="ac801-182"><a name="updateproperties"></a>tooupdate hello properties of a route filter</span></span>

<span data-ttu-id="ac801-183">Hello "kezelése szabály" gombra kattintva frissítheti a BGP közösségi értékek csatolt tooa áramkör hello listája.</span><span class="sxs-lookup"><span data-stu-id="ac801-183">You can update hello list of BGP community values attached tooa circuit by selecting hello "Manage rule" button.</span></span>


![Útvonal szűrő létrehozása](.\media\how-to-routefilter-portal\ManageRouteFilter.png)

![Útvonal szűrő létrehozása](.\media\how-to-routefilter-portal\AddRouteFilterRule.png) 


## <span data-ttu-id="ac801-186"><a name="detach"></a>az ExpressRoute-kapcsolatcsoportot útvonal szűrő toodetach</span><span class="sxs-lookup"><span data-stu-id="ac801-186"><a name="detach"></a>toodetach a route filter from an ExpressRoute circuit</span></span>

<span data-ttu-id="ac801-187">hello útvonal szűrő, az expressroute-kapcsolatcsoporthoz toodetach hello körön kattintson a jobb gombbal, majd kattintson a "társítását".</span><span class="sxs-lookup"><span data-stu-id="ac801-187">toodetach a circuit from hello route filter, right click on hello circuit and click on "disassociate".</span></span>

![Útvonal szűrő létrehozása](.\media\how-to-routefilter-portal\DetachRouteFilter.png) 


## <span data-ttu-id="ac801-189"><a name="delete"></a>toodelete útvonal szűrő</span><span class="sxs-lookup"><span data-stu-id="ac801-189"><a name="delete"></a>toodelete a route filter</span></span>

<span data-ttu-id="ac801-190">Útvonal szűrő hello törlése gombra kattintva törölheti.</span><span class="sxs-lookup"><span data-stu-id="ac801-190">You can delete a route filter by selecting hello delete button.</span></span> 

![Útvonal szűrő létrehozása](.\media\how-to-routefilter-portal\DeleteRouteFilter.png) 

## <a name="next-steps"></a><span data-ttu-id="ac801-192">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ac801-192">Next steps</span></span>

<span data-ttu-id="ac801-193">ExpressRoute kapcsolatos további információkért lásd: hello [ExpressRoute – gyakori kérdések](expressroute-faqs.md).</span><span class="sxs-lookup"><span data-stu-id="ac801-193">For more information about ExpressRoute, see hello [ExpressRoute FAQ](expressroute-faqs.md).</span></span>
