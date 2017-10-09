---
title: "Be annak az Azure ExpressRoute Microsoft társviszony-létesítés: PowerShell |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan tooconfigure útvonal szűrése a Microsoft Peering PowerShell használatával"
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
ms.date: 08/16/2017
ms.author: ganesr;cherylmc
ms.openlocfilehash: 395bcf7d6ad43c643ff041b154f8e4b797083e7f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-route-filters-for-microsoft-peering"></a><span data-ttu-id="84098-103">Microsoft társviszony-létesítés útvonalszűrőinek konfigurálása</span><span class="sxs-lookup"><span data-stu-id="84098-103">Configure route filters for Microsoft peering</span></span>

<span data-ttu-id="84098-104">Útvonal-szűrők olyan módon tooconsume a Microsoft társviszony-létesítés keresztül támogatott szolgáltatások egy részhalmaza.</span><span class="sxs-lookup"><span data-stu-id="84098-104">Route filters are a way tooconsume a subset of supported services through Microsoft peering.</span></span> <span data-ttu-id="84098-105">Ez a cikk a Súgó konfigurálását és felügyeletét útvonal szűrők az ExpressRoute-Kapcsolatcsoportok hello szükséges lépések.</span><span class="sxs-lookup"><span data-stu-id="84098-105">hello steps in this article help you configure and manage route filters for ExpressRoute circuits.</span></span>

<span data-ttu-id="84098-106">Dynamics 365 szolgáltatások és a vállalati, például az Exchange Online, SharePoint Online és Skype Office 365-szolgáltatásokhoz hello Microsoft társviszony-létesítés keresztül érhetők el.</span><span class="sxs-lookup"><span data-stu-id="84098-106">Dynamics 365 services, and Office 365 services such as Exchange Online, SharePoint Online, and Skype for Business, are accessible through hello Microsoft peering.</span></span> <span data-ttu-id="84098-107">Ha a Microsoft társviszony-létesítést az ExpressRoute-kapcsolatcsoportot van konfigurálva, az összes előtagok kapcsolódó toothese szolgáltatás van-e hirdetve keresztül létesített hello BGP-munkamenetek.</span><span class="sxs-lookup"><span data-stu-id="84098-107">When Microsoft peering is configured in an ExpressRoute circuit, all prefixes related toothese services are advertised through hello BGP sessions that are established.</span></span> <span data-ttu-id="84098-108">A BGP közösségi értéke csatolt tooevery előtag tooidentify hello ajánlott szolgáltatás hello előtag keresztül.</span><span class="sxs-lookup"><span data-stu-id="84098-108">A BGP community value is attached tooevery prefix tooidentify hello service that is offered through hello prefix.</span></span> <span data-ttu-id="84098-109">Hello BGP logikai értékeket és rendeli hello szolgáltatások listáját lásd: [BGP Közösségek](expressroute-routing.md#bgp).</span><span class="sxs-lookup"><span data-stu-id="84098-109">For a list of hello BGP community values and hello services they  map to, see [BGP communities](expressroute-routing.md#bgp).</span></span>

<span data-ttu-id="84098-110">Ha a kapcsolat tooall szolgáltatások van szüksége, nagyszámú előtagok van-e hirdetve BGP keresztül.</span><span class="sxs-lookup"><span data-stu-id="84098-110">If you require connectivity tooall services, a large number of prefixes are advertised through BGP.</span></span> <span data-ttu-id="84098-111">Ez jelentősen növeli a belül a hálózati útválasztók által fenntartott hello útvonaltáblák hello méretét.</span><span class="sxs-lookup"><span data-stu-id="84098-111">This significantly increases hello size of hello route tables maintained by routers within your network.</span></span> <span data-ttu-id="84098-112">Ha azt tervezi, hogy csak a szolgáltatások egy részhalmaza kínált Microsoft társviszony-létesítés tooconsume, csökkentheti az útvonaltáblák kétféleképpen hello méretét.</span><span class="sxs-lookup"><span data-stu-id="84098-112">If you plan tooconsume only a subset of services offered through Microsoft peering, you can reduce hello size of your route tables in two ways.</span></span> <span data-ttu-id="84098-113">A következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="84098-113">You can:</span></span>

- <span data-ttu-id="84098-114">Nem kívánt előtagok szűrheti a BGP Közösségek útvonal szűrők alkalmazásával.</span><span class="sxs-lookup"><span data-stu-id="84098-114">Filter out unwanted prefixes by applying route filters on BGP communities.</span></span> <span data-ttu-id="84098-115">Ez egy szabványos hálózatkezelési eljárás, és sok hálózatok általában arra használják.</span><span class="sxs-lookup"><span data-stu-id="84098-115">This is a standard networking practice and is used commonly within many networks.</span></span>

- <span data-ttu-id="84098-116">Útvonal-szűrők, és alkalmazni azokat tooyour ExpressRoute-kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="84098-116">Define route filters and apply them tooyour ExpressRoute circuit.</span></span> <span data-ttu-id="84098-117">Útvonal szűrő egy új erőforrást kiválaszthatja hello listája szolgáltatások, tervezze meg a Microsoft társviszony-létesítés tooconsume.</span><span class="sxs-lookup"><span data-stu-id="84098-117">A route filter is a new resource that lets you select hello list of services you plan tooconsume through Microsoft peering.</span></span> <span data-ttu-id="84098-118">Csak az ExpressRoute útválasztók továbbítják hello útvonal szűrő toohello szolgáltatás tartozó előtagok hello listája.</span><span class="sxs-lookup"><span data-stu-id="84098-118">ExpressRoute routers only send hello list of prefixes that belong toohello services identified in hello route filter.</span></span>

### <span data-ttu-id="84098-119"><a name="about"></a>Útvonal-szűrők</span><span class="sxs-lookup"><span data-stu-id="84098-119"><a name="about"></a>About route filters</span></span>

<span data-ttu-id="84098-120">Ha a Microsoft társviszony-létesítés konfigurálva van az ExpressRoute-kapcsolatcsoportot, hello Microsoft peremhálózati útválasztók létesíteni két BGP-munkameneteket indíthat más hello peremhálózati útválasztók (saját vagy a kapcsolat szolgáltatóját).</span><span class="sxs-lookup"><span data-stu-id="84098-120">When Microsoft peering is configured on your ExpressRoute circuit, hello Microsoft edge routers establish a pair of BGP sessions with hello edge routers (yours or your connectivity provider's).</span></span> <span data-ttu-id="84098-121">Nincs útvonal hirdetett tooyour hálózati.</span><span class="sxs-lookup"><span data-stu-id="84098-121">No routes are advertised tooyour network.</span></span> <span data-ttu-id="84098-122">tooenable útvonal hirdetmények tooyour hálózati, társítania kell egy útvonal-szűrőt.</span><span class="sxs-lookup"><span data-stu-id="84098-122">tooenable route advertisements tooyour network, you must associate a route filter.</span></span>

<span data-ttu-id="84098-123">Útvonal szűrő lehetővé teszi azt szeretné, hogy az ExpressRoute-kapcsolatcsoportot Microsoft társviszony-létesítés keresztül tooconsume szolgáltatás azonosítására.</span><span class="sxs-lookup"><span data-stu-id="84098-123">A route filter lets you identify services you want tooconsume through your ExpressRoute circuit's Microsoft peering.</span></span> <span data-ttu-id="84098-124">Lényegében egy fehér lista hello BGP közösségi értékek is.</span><span class="sxs-lookup"><span data-stu-id="84098-124">It is essentially a white list of all hello BGP community values.</span></span> <span data-ttu-id="84098-125">Amikor egy útvonal szűrő erőforrás van definiálva, és tooan ExpressRoute-kapcsolatcsoportot csatlakoztatva, összes toohello BGP közösségi értékek megfeleltetni előtagok hirdetett tooyour hálózati.</span><span class="sxs-lookup"><span data-stu-id="84098-125">Once a route filter resource is defined and attached tooan ExpressRoute circuit, all prefixes that map toohello BGP community values are advertised tooyour network.</span></span>

<span data-ttu-id="84098-126">toobe képes tooattach útvonal szűrők az Office 365 szolgáltatásaival rajtuk, rendelkeznie kell engedélyezési tooconsume Office 365-szolgáltatásokhoz ExpressRoute keresztül.</span><span class="sxs-lookup"><span data-stu-id="84098-126">toobe able tooattach route filters with Office 365 services on them, you must have authorization tooconsume Office 365 services through ExpressRoute.</span></span> <span data-ttu-id="84098-127">Ha nem engedélyezett tooconsume Office 365-szolgáltatások díjairól ExpressRoute, hello művelet tooattach útvonal szűrők sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="84098-127">If you are not authorized tooconsume Office 365 services through ExpressRoute, hello operation tooattach route filters fails.</span></span> <span data-ttu-id="84098-128">Hello engedélyezési folyamattal kapcsolatos további információkért lásd: [Azure ExpressRoute az Office 365](https://support.office.com/article/Azure-ExpressRoute-for-Office-365-6d2534a2-c19c-4a99-be5e-33a0cee5d3bd).</span><span class="sxs-lookup"><span data-stu-id="84098-128">For more information about hello authorization process, see [Azure ExpressRoute for Office 365](https://support.office.com/article/Azure-ExpressRoute-for-Office-365-6d2534a2-c19c-4a99-be5e-33a0cee5d3bd).</span></span> <span data-ttu-id="84098-129">Kapcsolat tooDynamics 365 használatához nem szükséges az előzetes engedélyek.</span><span class="sxs-lookup"><span data-stu-id="84098-129">Connectivity tooDynamics 365 services does NOT require any prior authorization.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="84098-130">Előzetes tooAugust 1 Microsoft társviszony-létesítést az ExpressRoute-Kapcsolatcsoportok volt beállítva, akkor 2017 fog rendelkezni a Microsoft társviszony-létesítést, meghirdetett összes szolgáltatás előtagot, akkor is, ha nincsenek megadva útvonal szűrők.</span><span class="sxs-lookup"><span data-stu-id="84098-130">Microsoft peering of ExpressRoute circuits that were configured prior tooAugust 1, 2017 will have all service prefixes advertised through Microsoft peering, even if route filters are not defined.</span></span> <span data-ttu-id="84098-131">A Microsoft társviszony-létesítést az ExpressRoute-Kapcsolatcsoportok vannak konfigurálva, vagy azt követően 2017. augusztus 1. nem rendelkezik a előtagokat meghirdetett, amíg egy útvonal-szűrő nem csatlakoztatja toohello körön.</span><span class="sxs-lookup"><span data-stu-id="84098-131">Microsoft peering of ExpressRoute circuits that are configured on or after August 1, 2017 will not have any prefixes advertised until a route filter is attached toohello circuit.</span></span>
> 
> 

### <span data-ttu-id="84098-132"><a name="workflow"></a>Munkafolyamat</span><span class="sxs-lookup"><span data-stu-id="84098-132"><a name="workflow"></a>Workflow</span></span>

<span data-ttu-id="84098-133">toobe képes toosuccessfully tooservices csatlakozhat a Microsoft társviszony-létesítést, végre kell hajtania a következő konfigurációs lépések hello:</span><span class="sxs-lookup"><span data-stu-id="84098-133">toobe able toosuccessfully connect tooservices through Microsoft peering, you must complete hello following configuration steps:</span></span>

- <span data-ttu-id="84098-134">Rendelkeznie kell egy aktív van a Microsoft társviszony kiosztott ExpressRoute-kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="84098-134">You must have an active ExpressRoute circuit that has Microsoft peering provisioned.</span></span> <span data-ttu-id="84098-135">A következő utasításokat tooaccomplish hello használhatja ezeket a feladatokat:</span><span class="sxs-lookup"><span data-stu-id="84098-135">You can use hello following instructions tooaccomplish these tasks:</span></span>
  - <span data-ttu-id="84098-136">[ExpressRoute-kapcsolatcsoportot létrehozni](expressroute-howto-circuit-arm.md) , és folytassa a kapcsolat szolgáltatójánál előtt által engedélyezett hello áramkör.</span><span class="sxs-lookup"><span data-stu-id="84098-136">[Create an ExpressRoute circuit](expressroute-howto-circuit-arm.md) and have hello circuit enabled by your connectivity provider before you proceed.</span></span> <span data-ttu-id="84098-137">hello ExpressRoute-kapcsolatcsoportot kiépített és engedélyezett állapotban kell lennie.</span><span class="sxs-lookup"><span data-stu-id="84098-137">hello ExpressRoute circuit must be in a provisioned and enabled state.</span></span>
  - <span data-ttu-id="84098-138">[Hozzon létre a Microsoft társviszony-létesítés](expressroute-circuit-peerings.md) hello BGP-közvetlenül munkamenet kezelése.</span><span class="sxs-lookup"><span data-stu-id="84098-138">[Create Microsoft peering](expressroute-circuit-peerings.md) if you manage hello BGP session directly.</span></span> <span data-ttu-id="84098-139">Vagy a kapcsolat szolgáltatójánál rendelkezik a kör társviszony Microsoft kiépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="84098-139">Or, have your connectivity provider provision Microsoft peering for your circuit.</span></span>

-  <span data-ttu-id="84098-140">Hozzon létre és útvonal-szűrő konfigurálnia kell.</span><span class="sxs-lookup"><span data-stu-id="84098-140">You must create and configure a route filter.</span></span>
    - <span data-ttu-id="84098-141">Azonosítsa hello szolgáltatásokhoz, a Microsoft társviszony-létesítés keresztül tooconsume</span><span class="sxs-lookup"><span data-stu-id="84098-141">Identify hello services you with tooconsume through Microsoft peering</span></span>
    - <span data-ttu-id="84098-142">Azonosítsa a BGP-Közösség értékek hello szolgáltatásokkal társított hello listája</span><span class="sxs-lookup"><span data-stu-id="84098-142">Identify hello list of BGP community values associated with hello services</span></span>
    - <span data-ttu-id="84098-143">Hozzon létre egy szabály tooallow hello előtag lista egyező hello BGP logikai értékek</span><span class="sxs-lookup"><span data-stu-id="84098-143">Create a rule tooallow hello prefix list matching hello BGP community values</span></span>

-  <span data-ttu-id="84098-144">Hello útvonal szűrő toohello ExpressRoute-kapcsolatcsoportot kell csatolnia.</span><span class="sxs-lookup"><span data-stu-id="84098-144">You must attach hello route filter toohello ExpressRoute circuit.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="84098-145">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="84098-145">Before you begin</span></span>

<span data-ttu-id="84098-146">A konfigurálás elkezdése előtt ellenőrizze a következő feltételek hello:</span><span class="sxs-lookup"><span data-stu-id="84098-146">Before you begin configuration, make sure you meet hello following criteria:</span></span>

 - <span data-ttu-id="84098-147">Hello hello Azure Resource Manager PowerShell-parancsmagok legújabb verziójának telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="84098-147">Install hello latest version of hello Azure Resource Manager PowerShell cmdlets.</span></span> <span data-ttu-id="84098-148">További információkért lásd: [telepítése és konfigurálása az Azure PowerShelll](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="84098-148">For more information, see [Install and configure Azure PowerShelll](/powershell/azure/install-azurerm-ps).</span></span>

  > [!NOTE]
  > <span data-ttu-id="84098-149">Hello legújabb verzió letöltéséhez hello PowerShell-galériában, hanem hello Installer használatával.</span><span class="sxs-lookup"><span data-stu-id="84098-149">Download hello latest version from hello PowerShell Gallery, rather than using hello Installer.</span></span> <span data-ttu-id="84098-150">Telepítő hello jelenleg nem támogatja a szükséges hello parancsmagok.</span><span class="sxs-lookup"><span data-stu-id="84098-150">hello Installer currently does not support hello required cmdlets.</span></span>
  > 

 - <span data-ttu-id="84098-151">Felülvizsgálati hello [Előfeltételek](expressroute-prerequisites.md) és [munkafolyamatok](expressroute-workflows.md) konfigurálás elkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="84098-151">Review hello [prerequisites](expressroute-prerequisites.md) and [workflows](expressroute-workflows.md) before you begin configuration.</span></span>

 - <span data-ttu-id="84098-152">Egy aktív ExpressRoute-kapcsolatcsoportra lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="84098-152">You must have an active ExpressRoute circuit.</span></span> <span data-ttu-id="84098-153">Útmutatás alapján hello túl[ExpressRoute-kapcsolatcsoportot létrehozni](expressroute-howto-circuit-arm.md) , és folytassa a kapcsolat szolgáltatójánál előtt által engedélyezett hello áramkör.</span><span class="sxs-lookup"><span data-stu-id="84098-153">Follow hello instructions too[Create an ExpressRoute circuit](expressroute-howto-circuit-arm.md) and have hello circuit enabled by your connectivity provider before you proceed.</span></span> <span data-ttu-id="84098-154">hello ExpressRoute-kapcsolatcsoportot kiépített és engedélyezett állapotban kell lennie.</span><span class="sxs-lookup"><span data-stu-id="84098-154">hello ExpressRoute circuit must be in a provisioned and enabled state.</span></span>

 - <span data-ttu-id="84098-155">Rendelkeznie kell egy aktív Microsoft társviszony-létesítés.</span><span class="sxs-lookup"><span data-stu-id="84098-155">You must have an active Microsoft peering.</span></span> <span data-ttu-id="84098-156">Kövesse az utasításokat, [létrehozása, és társviszony-létesítési konfigurációjának módosítása](expressroute-circuit-peerings.md)</span><span class="sxs-lookup"><span data-stu-id="84098-156">Follow instructions at [Create and modifying peering configuration](expressroute-circuit-peerings.md)</span></span>

### <a name="log-in-tooyour-azure-account"></a><span data-ttu-id="84098-157">Jelentkezzen be tooyour Azure-fiók</span><span class="sxs-lookup"><span data-stu-id="84098-157">Log in tooyour Azure account</span></span>

<span data-ttu-id="84098-158">Ez a konfiguráció megkezdése előtt be kell jelentkeznie tooyour Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="84098-158">Before beginning this configuration, you must log in tooyour Azure account.</span></span> <span data-ttu-id="84098-159">hello parancsmag kéri hello bejelentkezési hitelesítő adatait az Azure-fiókjával.</span><span class="sxs-lookup"><span data-stu-id="84098-159">hello cmdlet prompts you for hello login credentials for your Azure account.</span></span> <span data-ttu-id="84098-160">A bejelentkezés után az tölti le a fiók beállításait,-e elérhető tooAzure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="84098-160">After logging in, it downloads your account settings so they are available tooAzure PowerShell.</span></span>

<span data-ttu-id="84098-161">Nyissa meg a PowerShell-konzolt emelt szintű jogosultságokkal, és csatlakozzon tooyour fiók.</span><span class="sxs-lookup"><span data-stu-id="84098-161">Open your PowerShell console with elevated privileges, and connect tooyour account.</span></span> <span data-ttu-id="84098-162">A következő példa toohelp csatlakozás hello használata:</span><span class="sxs-lookup"><span data-stu-id="84098-162">Use hello following example toohelp you connect:</span></span>

```powershell
Login-AzureRmAccount
```

<span data-ttu-id="84098-163">Ha több Azure-előfizetéssel rendelkezik, ellenőrizze a hello fiókhoz hello előfizetések.</span><span class="sxs-lookup"><span data-stu-id="84098-163">If you have multiple Azure subscriptions, check hello subscriptions for hello account.</span></span>

```powershell
Get-AzureRmSubscription
```

<span data-ttu-id="84098-164">Adja meg, hogy szeretné-e toouse hello előfizetés.</span><span class="sxs-lookup"><span data-stu-id="84098-164">Specify hello subscription that you want toouse.</span></span>

```powershell
Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"
```

## <span data-ttu-id="84098-165"><a name="prefixes"></a>1. lépés.</span><span class="sxs-lookup"><span data-stu-id="84098-165"><a name="prefixes"></a>Step 1.</span></span> <span data-ttu-id="84098-166">Előtagok és BGP közösségi értékek listájának beolvasása</span><span class="sxs-lookup"><span data-stu-id="84098-166">Get a list of prefixes and BGP community values</span></span>

### <a name="1-get-a-list-of-bgp-community-values"></a><span data-ttu-id="84098-167">1. BGP-Közösség értékek listájának beolvasása</span><span class="sxs-lookup"><span data-stu-id="84098-167">1. Get a list of BGP community values</span></span>

<span data-ttu-id="84098-168">A következő parancsmag tooget hello elérhető a Microsoft társviszony-létesítés szolgáltatásokkal társított BGP közösségi értékek listája hello használja, és előtaglistát kezel a hozzájuk társított runbookokat hello:</span><span class="sxs-lookup"><span data-stu-id="84098-168">Use hello following cmdlet tooget hello list of BGP community values associated with services accessible through Microsoft peering, and hello list of prefixes associated with them:</span></span>

```powershell
Get-AzureRmBgpServiceCommunity
```
### <a name="2-make-a-list-of-hello-values-that-you-want-toouse"></a><span data-ttu-id="84098-169">2. Ellenőrizze a hello értékből álló lista, amelyet az toouse</span><span class="sxs-lookup"><span data-stu-id="84098-169">2. Make a list of hello values that you want toouse</span></span>

<span data-ttu-id="84098-170">A BGP kívánt logikai értékek listáját toouse teszi hello útvonal szűrő.</span><span class="sxs-lookup"><span data-stu-id="84098-170">Make a list of BGP community values you want toouse in hello route filter.</span></span> <span data-ttu-id="84098-171">Tegyük fel hello BGP közösségi érték Dynamics 365 szolgáltatások 12076:5040.</span><span class="sxs-lookup"><span data-stu-id="84098-171">As an example, hello BGP community value for Dynamics 365 services is 12076:5040.</span></span>

## <span data-ttu-id="84098-172"><a name="filter"></a>2. lépés.</span><span class="sxs-lookup"><span data-stu-id="84098-172"><a name="filter"></a>Step 2.</span></span> <span data-ttu-id="84098-173">Útvonal-szűrő, ezért a szűrési szabály létrehozása</span><span class="sxs-lookup"><span data-stu-id="84098-173">Create a route filter and a filter rule</span></span>

<span data-ttu-id="84098-174">Útvonal szűrő lehet csak egy szabályt, és hello szabály a "Engedélyezés" típusúnak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="84098-174">A route filter can have only one rule, and hello rule must be of type 'Allow'.</span></span> <span data-ttu-id="84098-175">Ez a szabály társítva BGP közösségi értékből álló lista lehet.</span><span class="sxs-lookup"><span data-stu-id="84098-175">This rule can have a list of BGP community values associated with it.</span></span>

### <a name="1-create-a-route-filter"></a><span data-ttu-id="84098-176">1. Útvonal szűrő létrehozása</span><span class="sxs-lookup"><span data-stu-id="84098-176">1. Create a route filter</span></span>

<span data-ttu-id="84098-177">Először hozzon létre hello útvonal szűrő.</span><span class="sxs-lookup"><span data-stu-id="84098-177">First, create hello route filter.</span></span> <span data-ttu-id="84098-178">"New-AzureRmRouteFilter" Hello parancs csak egy útvonal-szűrő erőforrás hoz létre.</span><span class="sxs-lookup"><span data-stu-id="84098-178">hello command 'New-AzureRmRouteFilter' only creates a route filter resource.</span></span> <span data-ttu-id="84098-179">Hello erőforrás létrehozása után meg kell majd hozzon létre egy szabályt és mellékelje toohello útvonal szűrő objektum.</span><span class="sxs-lookup"><span data-stu-id="84098-179">After you create hello resource, you must then create a rule and attach it toohello route filter object.</span></span> <span data-ttu-id="84098-180">Futtassa a következő parancs toocreate hello útvonal szűrő erőforrás:</span><span class="sxs-lookup"><span data-stu-id="84098-180">Run hello following command toocreate a route filter resource:</span></span>

```powershell
New-AzureRmRouteFilter -Name "MyRouteFilter" -ResourceGroupName "MyResourceGroup" -Location "West US"
```

### <a name="2-create-a-filter-rule"></a><span data-ttu-id="84098-181">2. Állapotszűrő szabály létrehozása</span><span class="sxs-lookup"><span data-stu-id="84098-181">2. Create a filter rule</span></span>

<span data-ttu-id="84098-182">Megadhat egy készletében BGP hajtsa végre egy vesszővel tagolt lista formájában, hello példában látható módon.</span><span class="sxs-lookup"><span data-stu-id="84098-182">You can specify a set of BGP communities as a comma-separated list, as shown in hello example.</span></span> <span data-ttu-id="84098-183">Futtassa a következő parancs toocreate hello egy új szabályt:</span><span class="sxs-lookup"><span data-stu-id="84098-183">Run hello following command toocreate a new rule:</span></span>
 
```powershell
$rule = New-AzureRmRouteFilterRuleConfig -Name "Allow-EXO-D365" -Access Allow -RouteFilterRuleType Community -CommunityList "12076:5010,12076:5040"
```

### <a name="3-add-hello-rule-toohello-route-filter"></a><span data-ttu-id="84098-184">3. Hello szabály toohello útvonal-szűrő hozzáadása</span><span class="sxs-lookup"><span data-stu-id="84098-184">3. Add hello rule toohello route filter</span></span>

<span data-ttu-id="84098-185">Futtassa a következő parancs tooadd hello szűrési szabály toohello útvonal szűrő hello:</span><span class="sxs-lookup"><span data-stu-id="84098-185">Run hello following command tooadd hello filter rule toohello route filter:</span></span>
 
```powershell
$routefilter = Get-AzureRmRouteFilter -Name "RouteFilterName" -ResourceGroupName "ExpressRouteResourceGroupName"
$routefilter.Rules.Add($rule)
Set-AzureRmRouteFilter -RouteFilter $routefilter
```

## <span data-ttu-id="84098-186"><a name="attach"></a>3. lépés.</span><span class="sxs-lookup"><span data-stu-id="84098-186"><a name="attach"></a>Step 3.</span></span> <span data-ttu-id="84098-187">Hello útvonal szűrő tooan ExpressRoute-kapcsolatcsoportot csatolása</span><span class="sxs-lookup"><span data-stu-id="84098-187">Attach hello route filter tooan ExpressRoute circuit</span></span>

<span data-ttu-id="84098-188">Futtassa a következő parancs tooattach hello útvonal szűrő toohello ExpressRoute-kapcsolatcsoportot, feltéve, hogy csak a Microsoft társviszony-létesítés rendelkezik hello:</span><span class="sxs-lookup"><span data-stu-id="84098-188">Run hello following command tooattach hello route filter toohello ExpressRoute circuit, assuming you have only Microsoft peering:</span></span>

```powershell
$ckt.Peerings[0].RouteFilter = $routefilter 
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

## <span data-ttu-id="84098-189"><a name="getproperties"></a>egy útvonal szűrő tooget hello tulajdonságait</span><span class="sxs-lookup"><span data-stu-id="84098-189"><a name="getproperties"></a>tooget hello properties of a route filter</span></span>

<span data-ttu-id="84098-190">egy útvonal szűrő tooget hello tulajdonságait a lépéseket követve hello használata:</span><span class="sxs-lookup"><span data-stu-id="84098-190">tooget hello properties of a route filter, use hello following steps:</span></span>

1. <span data-ttu-id="84098-191">Futtassa a következő parancs tooget hello útvonal szűrő erőforrás hello:</span><span class="sxs-lookup"><span data-stu-id="84098-191">Run hello following command tooget hello route filter resource:</span></span>

  ```powershell
  $routefilter = Get-AzureRmRouteFilter -Name "RouteFilterName" -ResourceGroupName "ExpressRouteResourceGroupName"
  ```
2. <span data-ttu-id="84098-192">Hello útvonal Állapotszűrő szabályok lekérése hello útvonal-szűrő erőforrás hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="84098-192">Get hello route filter rules for hello route-filter resource by running hello following command:</span></span>

  ```powershell
  $routefilter = Get-AzureRmRouteFilter -Name "RouteFilterName" -ResourceGroupName "ExpressRouteResourceGroupName"
  $rule = $routefilter.Rules[0]
  ```

## <span data-ttu-id="84098-193"><a name="updateproperties"></a>egy útvonal szűrő tooupdate hello tulajdonságait</span><span class="sxs-lookup"><span data-stu-id="84098-193"><a name="updateproperties"></a>tooupdate hello properties of a route filter</span></span>

<span data-ttu-id="84098-194">Ha hello útvonal szűrő már hozzá van rendelve tooa áramkör, frissítések toohello BGP közösségi lista automatikusan tölti ki a megfelelő előtaggal hirdetmény módosítások létrehozott hello BGP-munkamenetek keresztül.</span><span class="sxs-lookup"><span data-stu-id="84098-194">If hello route filter is already attached tooa circuit, updates toohello BGP community list automatically propagates appropriate prefix advertisement changes through hello established BGP sessions.</span></span> <span data-ttu-id="84098-195">Hello BGP közösségi listája az útvonal szűrő hello a következő parancs használatával frissítheti:</span><span class="sxs-lookup"><span data-stu-id="84098-195">You can update hello BGP community list of your route filter using hello following command:</span></span>

```powershell
$routefilter = Get-AzureRmRouteFilter -Name "RouteFilterName" -ResourceGroupName "ExpressRouteResourceGroupName"
$routefilter.rules[0].Communities = "12076:5030", "12076:5040"
Set-AzureRmRouteFilter -RouteFilter $routefilter
```

## <span data-ttu-id="84098-196"><a name="detach"></a>az ExpressRoute-kapcsolatcsoportot útvonal szűrő toodetach</span><span class="sxs-lookup"><span data-stu-id="84098-196"><a name="detach"></a>toodetach a route filter from an ExpressRoute circuit</span></span>

<span data-ttu-id="84098-197">Miután egy útvonal szűrő le van választva, az ExpressRoute-kapcsolatcsoportot hello, előtagok nélkül van-e hirdetve hello BGP-kapcsolaton keresztül.</span><span class="sxs-lookup"><span data-stu-id="84098-197">Once a route filter is detached from hello ExpressRoute circuit, no prefixes are advertised through hello BGP session.</span></span> <span data-ttu-id="84098-198">Útvonal szűrő használatával a következő parancs hello ExpressRoute-kapcsolatcsoportot az választhatják le:</span><span class="sxs-lookup"><span data-stu-id="84098-198">You can detach a route filter from an ExpressRoute circuit using hello following command:</span></span>
  
```powershell
$ckt.Peerings[0].RouteFilter = $null
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

## <span data-ttu-id="84098-199"><a name="delete"></a>toodelete útvonal szűrő</span><span class="sxs-lookup"><span data-stu-id="84098-199"><a name="delete"></a>toodelete a route filter</span></span>

<span data-ttu-id="84098-200">Csak a törlés egy útvonal-szűrő, ha nem csatlakoztatott tooany kör is.</span><span class="sxs-lookup"><span data-stu-id="84098-200">You can only delete a route filter if it is not attached tooany circuit.</span></span> <span data-ttu-id="84098-201">Győződjön meg arról, hogy hello útvonal szűrő nem csatlakoztatott tooany áramkör toodelete megkísérlése előtt azt.</span><span class="sxs-lookup"><span data-stu-id="84098-201">Ensure that hello route filter is not attached tooany circuit before attempting toodelete it.</span></span> <span data-ttu-id="84098-202">A következő parancs hello útvonal szűrő törlése:</span><span class="sxs-lookup"><span data-stu-id="84098-202">You can delete a route filter using hello following command:</span></span>

```powershell
Remove-AzureRmRouteFilter -Name "MyRouteFilter" -ResourceGroupName "MyResourceGroup"
```

## <a name="next-steps"></a><span data-ttu-id="84098-203">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="84098-203">Next steps</span></span>

<span data-ttu-id="84098-204">ExpressRoute kapcsolatos további információkért lásd: hello [ExpressRoute – gyakori kérdések](expressroute-faqs.md).</span><span class="sxs-lookup"><span data-stu-id="84098-204">For more information about ExpressRoute, see hello [ExpressRoute FAQ](expressroute-faqs.md).</span></span>
