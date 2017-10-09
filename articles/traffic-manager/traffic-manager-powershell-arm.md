---
title: aaaUsing PowerShell toomanage az Azure Traffic Manager |} Microsoft Docs
description: "A Traffic Manager az Azure Resource Manager eszközzel PowerShell használatával"
services: traffic-manager
documentationcenter: na
author: kumudd
manager: timlt
ms.assetid: bc247448-1d2e-4104-ac03-42b59ebde065
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/16/2017
ms.author: kumud
ms.openlocfilehash: 018c37db63beb82fdad54cfd7e13ab3cb645723a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-powershell-toomanage-traffic-manager"></a><span data-ttu-id="e2e41-103">PowerShell toomanage Traffic Manager használatával</span><span class="sxs-lookup"><span data-stu-id="e2e41-103">Using PowerShell toomanage Traffic Manager</span></span>

<span data-ttu-id="e2e41-104">Az Azure Resource Manager rendszer hello előnyben részesített felügyeleti szolgáltatások az Azure-ban,</span><span class="sxs-lookup"><span data-stu-id="e2e41-104">Azure Resource Manager is hello preferred management interface for services in Azure.</span></span> <span data-ttu-id="e2e41-105">Az Azure Traffic Manager-profilok használatával az Azure Resource Manager-alapú API-k és eszközök is felügyelhetők.</span><span class="sxs-lookup"><span data-stu-id="e2e41-105">Azure Traffic Manager profiles can be managed using Azure Resource Manager-based APIs and tools.</span></span>

## <a name="resource-model"></a><span data-ttu-id="e2e41-106">Erőforrás-modellje</span><span class="sxs-lookup"><span data-stu-id="e2e41-106">Resource model</span></span>

<span data-ttu-id="e2e41-107">Az Azure Traffic Manager a Traffic Manager-profil neve beállítások segítségével konfigurálható.</span><span class="sxs-lookup"><span data-stu-id="e2e41-107">Azure Traffic Manager is configured using a collection of settings called a Traffic Manager profile.</span></span> <span data-ttu-id="e2e41-108">Ez a profil tartalmazza a DNS-beállítások, a forgalom útválasztásához tartozó beállításokat, a figyelési beállításokat végpont, és a végpontok toowhich forgalmának listáját történik.</span><span class="sxs-lookup"><span data-stu-id="e2e41-108">This profile contains DNS settings, traffic routing settings, endpoint monitoring settings, and a list of service endpoints toowhich traffic is routed.</span></span>

<span data-ttu-id="e2e41-109">Minden egyes Traffic Manager-profil "TrafficManagerProfiles" típusú erőforrást jelképezi.</span><span class="sxs-lookup"><span data-stu-id="e2e41-109">Each Traffic Manager profile is represented by a resource of type 'TrafficManagerProfiles'.</span></span> <span data-ttu-id="e2e41-110">A hello REST API-szintet az egyes profilok URI hello a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="e2e41-110">At hello REST API level, hello URI for each profile is as follows:</span></span>

    https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Network/trafficManagerProfiles/{profile-name}?api-version={api-version}

## <a name="setting-up-azure-powershell"></a><span data-ttu-id="e2e41-111">Azure PowerShell telepítése</span><span class="sxs-lookup"><span data-stu-id="e2e41-111">Setting up Azure PowerShell</span></span>

<span data-ttu-id="e2e41-112">Ezek az utasítások a Microsoft Azure PowerShell használata.</span><span class="sxs-lookup"><span data-stu-id="e2e41-112">These instructions use Microsoft Azure PowerShell.</span></span> <span data-ttu-id="e2e41-113">hello alábbi cikk azt ismerteti, hogyan tooinstall Azure PowerShell és konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="e2e41-113">hello following article explains how tooinstall and configure Azure PowerShell.</span></span>

* [<span data-ttu-id="e2e41-114">Hogyan tooinstall Azure PowerShell és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e2e41-114">How tooinstall and configure Azure PowerShell</span></span>](/powershell/azure/overview)

<span data-ttu-id="e2e41-115">hello a cikkben szereplő példák azt feltételezik, hogy rendelkezik-e egy meglévő erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="e2e41-115">hello examples in this article assume that you have an existing resource group.</span></span> <span data-ttu-id="e2e41-116">Létrehozhat egy olyan erőforráscsoport, a következő parancs hello használata:</span><span class="sxs-lookup"><span data-stu-id="e2e41-116">You can create a resource group using hello following command:</span></span>

```powershell
New-AzureRmResourceGroup -Name MyRG -Location "West US"
```

> [!NOTE]
> <span data-ttu-id="e2e41-117">Az Azure Resource Manager megköveteli, hogy a hely összes erőforráscsoport.</span><span class="sxs-lookup"><span data-stu-id="e2e41-117">Azure Resource Manager requires that all resource groups have a location.</span></span> <span data-ttu-id="e2e41-118">Ezen a helyen hello alapértelmezett szolgál az erőforráscsoport létrejött erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="e2e41-118">This location is used as hello default for resources created in that resource group.</span></span> <span data-ttu-id="e2e41-119">Azonban mivel a Traffic Manager-profil erőforrások globális, nem pedig regionális, hello választás az erőforráscsoport helye nincs hatással van az Azure Traffic Manager.</span><span class="sxs-lookup"><span data-stu-id="e2e41-119">However, since Traffic Manager profile resources are global, not regional, hello choice of resource group location has no impact on Azure Traffic Manager.</span></span>

## <a name="create-a-traffic-manager-profile"></a><span data-ttu-id="e2e41-120">A Traffic Manager-profil létrehozása</span><span class="sxs-lookup"><span data-stu-id="e2e41-120">Create a Traffic Manager Profile</span></span>

<span data-ttu-id="e2e41-121">Traffic Manager-profil toocreate hello használata `New-AzureRmTrafficManagerProfile` parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="e2e41-121">toocreate a Traffic Manager profile, use hello `New-AzureRmTrafficManagerProfile` cmdlet:</span></span>

```powershell
$profile = New-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName contoso -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
```

<span data-ttu-id="e2e41-122">a következő táblázat hello hello paramétereket ismerteti:</span><span class="sxs-lookup"><span data-stu-id="e2e41-122">hello following table describes hello parameters:</span></span>

| <span data-ttu-id="e2e41-123">Paraméter</span><span class="sxs-lookup"><span data-stu-id="e2e41-123">Parameter</span></span> | <span data-ttu-id="e2e41-124">Leírás</span><span class="sxs-lookup"><span data-stu-id="e2e41-124">Description</span></span> |
| --- | --- |
| <span data-ttu-id="e2e41-125">Név</span><span class="sxs-lookup"><span data-stu-id="e2e41-125">Name</span></span> |<span data-ttu-id="e2e41-126">a Traffic Manager-profil erőforrás hello hello erőforrás neve.</span><span class="sxs-lookup"><span data-stu-id="e2e41-126">hello resource name for hello Traffic Manager profile resource.</span></span> <span data-ttu-id="e2e41-127">Profilok hello az azonos erőforráscsoport neve csak egyedi lehet.</span><span class="sxs-lookup"><span data-stu-id="e2e41-127">Profiles in hello same resource group must have unique names.</span></span> <span data-ttu-id="e2e41-128">Ez a név nem azonos DNS-lekérdezések használt hello DNS-név.</span><span class="sxs-lookup"><span data-stu-id="e2e41-128">This name is separate from hello DNS name used for DNS queries.</span></span> |
| <span data-ttu-id="e2e41-129">erőforráscsoport-név</span><span class="sxs-lookup"><span data-stu-id="e2e41-129">ResourceGroupName</span></span> |<span data-ttu-id="e2e41-130">hello hello erőforrás csoportot tartalmazó hello profil erőforrás neve.</span><span class="sxs-lookup"><span data-stu-id="e2e41-130">hello name of hello resource group containing hello profile resource.</span></span> |
| <span data-ttu-id="e2e41-131">TrafficRoutingMethod</span><span class="sxs-lookup"><span data-stu-id="e2e41-131">TrafficRoutingMethod</span></span> |<span data-ttu-id="e2e41-132">Hello forgalom-útválasztási használt módszer toodetermine melyik végponthoz válaszként visszaadott a DNS-lekérdezés megadása</span><span class="sxs-lookup"><span data-stu-id="e2e41-132">Specifies hello traffic-routing method used toodetermine which endpoint is returned in response a DNS query.</span></span> <span data-ttu-id="e2e41-133">Lehetséges értékek: "Teljesítmény", "Weighted" vagy "Priority".</span><span class="sxs-lookup"><span data-stu-id="e2e41-133">Possible values are 'Performance', 'Weighted' or 'Priority'.</span></span> |
| <span data-ttu-id="e2e41-134">RelativeDnsName</span><span class="sxs-lookup"><span data-stu-id="e2e41-134">RelativeDnsName</span></span> |<span data-ttu-id="e2e41-135">Adja meg a Traffic Manager-profil által meghatározott hello DNS-név hello hostname része.</span><span class="sxs-lookup"><span data-stu-id="e2e41-135">Specifies hello hostname portion of hello DNS name provided by this Traffic Manager profile.</span></span> <span data-ttu-id="e2e41-136">Ez az érték hello Azure Traffic Manager tooform hello teljesen minősített tartománynevét (FQDN) hello-profil által használt DNS-tartománynév együtt.</span><span class="sxs-lookup"><span data-stu-id="e2e41-136">This value is combined with hello DNS domain name used by Azure Traffic Manager tooform hello fully qualified domain name (FQDN) of hello profile.</span></span> <span data-ttu-id="e2e41-137">Például "contoso" hello érték lesz "contoso.trafficmanager.net."</span><span class="sxs-lookup"><span data-stu-id="e2e41-137">For example, setting hello value of 'contoso' becomes 'contoso.trafficmanager.net.'</span></span> |
| <span data-ttu-id="e2e41-138">ÉLETTARTAM</span><span class="sxs-lookup"><span data-stu-id="e2e41-138">TTL</span></span> |<span data-ttu-id="e2e41-139">Hello DNS idő élettartamát (TTL), másodpercben.</span><span class="sxs-lookup"><span data-stu-id="e2e41-139">Specifies hello DNS Time-to-Live (TTL), in seconds.</span></span> <span data-ttu-id="e2e41-140">A TTL arról tájékoztatja a helyi DNS-feloldókat hello és a DNS-ügyfelek mennyi ideig toocache DNS-válaszok a Traffic Manager-profil.</span><span class="sxs-lookup"><span data-stu-id="e2e41-140">This TTL informs hello Local DNS resolvers and DNS clients how long toocache DNS responses for this Traffic Manager profile.</span></span> |
| <span data-ttu-id="e2e41-141">MonitorProtocol</span><span class="sxs-lookup"><span data-stu-id="e2e41-141">MonitorProtocol</span></span> |<span data-ttu-id="e2e41-142">Hello protokoll toouse toomonitor végpont állapota határozza meg.</span><span class="sxs-lookup"><span data-stu-id="e2e41-142">Specifies hello protocol toouse toomonitor endpoint health.</span></span> <span data-ttu-id="e2e41-143">Lehetséges értékek: "HTTP" és "HTTPS".</span><span class="sxs-lookup"><span data-stu-id="e2e41-143">Possible values are 'HTTP' and 'HTTPS'.</span></span> |
| <span data-ttu-id="e2e41-144">MonitorPort</span><span class="sxs-lookup"><span data-stu-id="e2e41-144">MonitorPort</span></span> |<span data-ttu-id="e2e41-145">Meghatározza, hello TCP-portot használja toomonitor végpont állapotát.</span><span class="sxs-lookup"><span data-stu-id="e2e41-145">Specifies hello TCP port used toomonitor endpoint health.</span></span> |
| <span data-ttu-id="e2e41-146">MonitorPath</span><span class="sxs-lookup"><span data-stu-id="e2e41-146">MonitorPath</span></span> |<span data-ttu-id="e2e41-147">Megadja, hello elérési útja relatív toohello végpont tartománynév tooprobe használandó végpont állapotát.</span><span class="sxs-lookup"><span data-stu-id="e2e41-147">Specifies hello path relative toohello endpoint domain name used tooprobe for endpoint health.</span></span> |

<span data-ttu-id="e2e41-148">hello parancsmag Traffic Manager-profil létrehozása az Azure-ban, és a megfelelő profil objektum tooPowerShell adja vissza.</span><span class="sxs-lookup"><span data-stu-id="e2e41-148">hello cmdlet creates a Traffic Manager profile in Azure and returns a corresponding profile object tooPowerShell.</span></span> <span data-ttu-id="e2e41-149">Ezen a ponton hello-profil nem tartalmaz végpontok.</span><span class="sxs-lookup"><span data-stu-id="e2e41-149">At this point, hello profile does not contain any endpoints.</span></span> <span data-ttu-id="e2e41-150">Végpontok tooa Traffic Manager-profil hozzáadásával kapcsolatos további információkért lásd: [Traffic Manager-végpontok hozzáadása](#adding-traffic-manager-endpoints).</span><span class="sxs-lookup"><span data-stu-id="e2e41-150">For more information about adding endpoints tooa Traffic Manager profile, see [Adding Traffic Manager Endpoints](#adding-traffic-manager-endpoints).</span></span>

## <a name="get-a-traffic-manager-profile"></a><span data-ttu-id="e2e41-151">Traffic Manager-profil beolvasása</span><span class="sxs-lookup"><span data-stu-id="e2e41-151">Get a Traffic Manager Profile</span></span>

<span data-ttu-id="e2e41-152">egy meglévő Traffic Manager-profil objektumban, tooretrieve hello használata `Get-AzureRmTrafficManagerProfle` parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="e2e41-152">tooretrieve an existing Traffic Manager profile object, use hello `Get-AzureRmTrafficManagerProfle` cmdlet:</span></span>

```powershell
$profile = Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG
```

<span data-ttu-id="e2e41-153">Ez a parancsmag egy Traffic Manager-profil objektumot ad vissza.</span><span class="sxs-lookup"><span data-stu-id="e2e41-153">This cmdlet returns a Traffic Manager profile object.</span></span>

## <a name="update-a-traffic-manager-profile"></a><span data-ttu-id="e2e41-154">Traffic Manager-profil frissítése</span><span class="sxs-lookup"><span data-stu-id="e2e41-154">Update a Traffic Manager Profile</span></span>

<span data-ttu-id="e2e41-155">Traffic Manager-profilok módosítása a 3. lépés folyamatot követi:</span><span class="sxs-lookup"><span data-stu-id="e2e41-155">Modifying Traffic Manager profiles follows a 3-step process:</span></span>

1. <span data-ttu-id="e2e41-156">Kérje le hello-profil használatával `Get-AzureRmTrafficManagerProfile` , vagy használjon hello-profil által visszaadott `New-AzureRmTrafficManagerProfile`.</span><span class="sxs-lookup"><span data-stu-id="e2e41-156">Retrieve hello profile using `Get-AzureRmTrafficManagerProfile` or use hello profile returned by `New-AzureRmTrafficManagerProfile`.</span></span>
2. <span data-ttu-id="e2e41-157">Hello módosításához.</span><span class="sxs-lookup"><span data-stu-id="e2e41-157">Modify hello profile.</span></span> <span data-ttu-id="e2e41-158">Adja hozzá, és távolítsa el a végpontok, vagy módosítsa a végpont vagy profil paraméterek.</span><span class="sxs-lookup"><span data-stu-id="e2e41-158">You can add and remove endpoints or change endpoint or profile parameters.</span></span> <span data-ttu-id="e2e41-159">A kapcsolat nélküli műveletek, amelyek ezeket a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="e2e41-159">These changes are off-line operations.</span></span> <span data-ttu-id="e2e41-160">Csak módosítjuk hello helyi objektum a memóriában, amely hello-profil jelöli.</span><span class="sxs-lookup"><span data-stu-id="e2e41-160">You are only changing hello local object in memory that represents hello profile.</span></span>
3. <span data-ttu-id="e2e41-161">Hello segítségével a változtatások véglegesítése a határidő `Set-AzureRmTrafficManagerProfile` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="e2e41-161">Commit your changes using hello `Set-AzureRmTrafficManagerProfile` cmdlet.</span></span>

<span data-ttu-id="e2e41-162">Az összes profil tulajdonságai hello-profil RelativeDnsName kivéve módosíthatók.</span><span class="sxs-lookup"><span data-stu-id="e2e41-162">All profile properties can be changed except hello profile's RelativeDnsName.</span></span> <span data-ttu-id="e2e41-163">toochange hello RelativeDnsName, törölnie kell a profil és egy új profilt egy új nevet.</span><span class="sxs-lookup"><span data-stu-id="e2e41-163">toochange hello RelativeDnsName, you must delete profile and a new profile with a new name.</span></span>

<span data-ttu-id="e2e41-164">hello a következő példa bemutatja, hogyan toochange hello-profil élettartam:</span><span class="sxs-lookup"><span data-stu-id="e2e41-164">hello following example demonstrates how toochange hello profile's TTL:</span></span>

```powershell
$profile = Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG
$profile.Ttl = 300
Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile
```

<span data-ttu-id="e2e41-165">A Traffic Manager-végpontok három típusa van:</span><span class="sxs-lookup"><span data-stu-id="e2e41-165">There are three types of Traffic Manager endpoints:</span></span>

1. <span data-ttu-id="e2e41-166">**Azure-végpontok** Azure-ban üzemeltetett szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="e2e41-166">**Azure endpoints** are services hosted in Azure</span></span>
2. <span data-ttu-id="e2e41-167">**Külső végpontok száma** Azure-on kívüli üzemeltetett szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="e2e41-167">**External endpoints** are services hosted outside of Azure</span></span>
3. <span data-ttu-id="e2e41-168">**A beágyazott végpontok** beágyazott használt tooconstruct hierarchia a Traffic Manager-profilokat.</span><span class="sxs-lookup"><span data-stu-id="e2e41-168">**Nested endpoints** are used tooconstruct nested hierarchies of Traffic Manager profiles.</span></span> <span data-ttu-id="e2e41-169">Beágyazott végpontok összetett alkalmazások speciális forgalom-útválasztási konfigurációi engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="e2e41-169">Nested endpoints enable advanced traffic-routing configurations for complex applications.</span></span>

<span data-ttu-id="e2e41-170">A három esetben végpontok hozzáadása is lehetséges két módon:</span><span class="sxs-lookup"><span data-stu-id="e2e41-170">In all three cases, endpoints can be added in two ways:</span></span>

1. <span data-ttu-id="e2e41-171">A fentiekben ismertetett 3. lépés folyamatok használata.</span><span class="sxs-lookup"><span data-stu-id="e2e41-171">Using a 3-step process described previously.</span></span> <span data-ttu-id="e2e41-172">hello ezt a módszert előnye, hogy több végpont módosítás egy frissítés.</span><span class="sxs-lookup"><span data-stu-id="e2e41-172">hello advantage of this method is that several endpoint changes can be made in a single update.</span></span>
2. <span data-ttu-id="e2e41-173">Hello New-AzureRmTrafficManagerEndpoint parancsmag használatával.</span><span class="sxs-lookup"><span data-stu-id="e2e41-173">Using hello New-AzureRmTrafficManagerEndpoint cmdlet.</span></span> <span data-ttu-id="e2e41-174">Ez a parancsmag egy végpont tooan meglévő Traffic Manager-profil hozzáadása egy művelettel.</span><span class="sxs-lookup"><span data-stu-id="e2e41-174">This cmdlet adds an endpoint tooan existing Traffic Manager profile in a single operation.</span></span>

## <a name="adding-azure-endpoints"></a><span data-ttu-id="e2e41-175">Azure-végpontok hozzáadása</span><span class="sxs-lookup"><span data-stu-id="e2e41-175">Adding Azure Endpoints</span></span>

<span data-ttu-id="e2e41-176">Azure-végpontok Azure-ban üzemeltetett szolgáltatások hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="e2e41-176">Azure endpoints reference services hosted in Azure.</span></span> <span data-ttu-id="e2e41-177">Azure-végpontok két típusú támogatottak:</span><span class="sxs-lookup"><span data-stu-id="e2e41-177">Two types of Azure endpoints are supported:</span></span>

1. <span data-ttu-id="e2e41-178">Azure Web Apps</span><span class="sxs-lookup"><span data-stu-id="e2e41-178">Azure Web Apps</span></span>
2. <span data-ttu-id="e2e41-179">Az Azure nyilvános erőforrások (csatolt tooa terheléselosztó és a virtuális gép hálózati adapter is lehet).</span><span class="sxs-lookup"><span data-stu-id="e2e41-179">Azure PublicIpAddress resources (which can be attached tooa load-balancer or a virtual machine NIC).</span></span> <span data-ttu-id="e2e41-180">hello PublicIpAddress hozzárendelt toobe használja őket a Traffic Manager DNS névvel kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="e2e41-180">hello PublicIpAddress must have a DNS name assigned toobe used in Traffic Manager.</span></span>

<span data-ttu-id="e2e41-181">Minden esetben:</span><span class="sxs-lookup"><span data-stu-id="e2e41-181">In each case:</span></span>

* <span data-ttu-id="e2e41-182">hello "targetresourceid azonosítója" paraméterének hello szolgáltatás adott `Add-AzureRmTrafficManagerEndpointConfig` vagy `New-AzureRmTrafficManagerEndpoint`.</span><span class="sxs-lookup"><span data-stu-id="e2e41-182">hello service is specified using hello 'targetResourceId' parameter of `Add-AzureRmTrafficManagerEndpointConfig` or `New-AzureRmTrafficManagerEndpoint`.</span></span>
* <span data-ttu-id="e2e41-183">hello "Target" és "EndpointLocation" vet hello targetresourceid azonosítója által.</span><span class="sxs-lookup"><span data-stu-id="e2e41-183">hello 'Target' and 'EndpointLocation' are implied by hello TargetResourceId.</span></span>
* <span data-ttu-id="e2e41-184">Hello "Súly" megadása nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="e2e41-184">Specifying hello 'Weight' is optional.</span></span> <span data-ttu-id="e2e41-185">Súlyozással beállított toouse hello "Weighted" forgalom-útválasztási módszer esetén is hello-profil csak használt.</span><span class="sxs-lookup"><span data-stu-id="e2e41-185">Weights are only used if hello profile is configured toouse hello 'Weighted' traffic-routing method.</span></span> <span data-ttu-id="e2e41-186">Ellenkező esetben nem veszi őket figyelembe.</span><span class="sxs-lookup"><span data-stu-id="e2e41-186">Otherwise, they are ignored.</span></span> <span data-ttu-id="e2e41-187">Ha meg van adva, a hello érték 1 és 1000 közötti számnak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="e2e41-187">If specified, hello value must be a number between 1 and 1000.</span></span> <span data-ttu-id="e2e41-188">hello alapértelmezett értéke "1".</span><span class="sxs-lookup"><span data-stu-id="e2e41-188">hello default value is '1'.</span></span>
* <span data-ttu-id="e2e41-189">Adja meg hello "Priority" megadása nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="e2e41-189">Specifying hello 'Priority' is optional.</span></span> <span data-ttu-id="e2e41-190">Prioritások csak használhatók, ha hello-profil konfigurált toouse hello "Priority" forgalom-útválasztási módszer.</span><span class="sxs-lookup"><span data-stu-id="e2e41-190">Priorities are only used if hello profile is configured toouse hello 'Priority' traffic-routing method.</span></span> <span data-ttu-id="e2e41-191">Ellenkező esetben nem veszi őket figyelembe.</span><span class="sxs-lookup"><span data-stu-id="e2e41-191">Otherwise, they are ignored.</span></span> <span data-ttu-id="e2e41-192">Érvényes értékek: 1 too1000 a magasabb prioritású jelző alacsonyabb értékeket.</span><span class="sxs-lookup"><span data-stu-id="e2e41-192">Valid values are from 1 too1000 with lower values indicating a higher priority.</span></span> <span data-ttu-id="e2e41-193">Ha meg van adva egy végpontot, azok összes végpontok adható meg.</span><span class="sxs-lookup"><span data-stu-id="e2e41-193">If specified for one endpoint, they must be specified for all endpoints.</span></span> <span data-ttu-id="e2e41-194">Ha nincs megadva, "1"-től kezdődő alapértelmezett értékeket, hogy látható-e a hello végpontok hello sorrendben alkalmazza.</span><span class="sxs-lookup"><span data-stu-id="e2e41-194">If omitted, default values starting from '1' are applied in hello order that hello endpoints are listed.</span></span>

### <a name="example-1-adding-web-app-endpoints-using-add-azurermtrafficmanagerendpointconfig"></a><span data-ttu-id="e2e41-195">1. példa: A webalkalmazás végpontjaitól hozzáadása`Add-AzureRmTrafficManagerEndpointConfig`</span><span class="sxs-lookup"><span data-stu-id="e2e41-195">Example 1: Adding Web App endpoints using `Add-AzureRmTrafficManagerEndpointConfig`</span></span>

<span data-ttu-id="e2e41-196">Ebben a példában azt a Traffic Manager-profil létrehozása, és adja hozzá a két Web App végpontjaitól hello `Add-AzureRmTrafficManagerEndpointConfig` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="e2e41-196">In this example, we create a Traffic Manager profile and add two Web App endpoints using hello `Add-AzureRmTrafficManagerEndpointConfig` cmdlet.</span></span>

```powershell
$profile = New-AzureRmTrafficManagerProfile -Name myprofile -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName myapp -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
$webapp1 = Get-AzureRMWebApp -Name webapp1
Add-AzureRmTrafficManagerEndpointConfig -EndpointName webapp1ep -TrafficManagerProfile $profile -Type AzureEndpoints -TargetResourceId $webapp1.Id -EndpointStatus Enabled
$webapp2 = Get-AzureRMWebApp -Name webapp2
Add-AzureRmTrafficManagerEndpointConfig -EndpointName webapp2ep -TrafficManagerProfile $profile -Type AzureEndpoints -TargetResourceId $webapp2.Id -EndpointStatus Enabled
Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile
```
### <a name="example-2-adding-a-publicipaddress-endpoint-using-new-azurermtrafficmanagerendpoint"></a><span data-ttu-id="e2e41-197">2. példa: Hozzáadása egy nyilvános végpontot a`New-AzureRmTrafficManagerEndpoint`</span><span class="sxs-lookup"><span data-stu-id="e2e41-197">Example 2: Adding a publicIpAddress endpoint using `New-AzureRmTrafficManagerEndpoint`</span></span>

<span data-ttu-id="e2e41-198">Ebben a példában egy nyilvános IP-cím erőforrás kerül toohello Traffic Manager-profil.</span><span class="sxs-lookup"><span data-stu-id="e2e41-198">In this example, a public IP address resource is added toohello Traffic Manager profile.</span></span> <span data-ttu-id="e2e41-199">hello nyilvános IP-cím rendelkeznie kell egy DNS-név konfigurálva, és lehet kötött vagy toohello VM vagy tooa terheléselosztó hálózati adapter.</span><span class="sxs-lookup"><span data-stu-id="e2e41-199">hello public IP address must have a DNS name configured, and can be bound either toohello NIC of a VM or tooa load balancer.</span></span>

```powershell
$ip = Get-AzureRmPublicIpAddress -Name MyPublicIP -ResourceGroupName MyRG
New-AzureRmTrafficManagerEndpoint -Name MyIpEndpoint -ProfileName MyProfile -ResourceGroupName MyRG -Type AzureEndpoints -TargetResourceId $ip.Id -EndpointStatus Enabled
```

## <a name="adding-external-endpoints"></a><span data-ttu-id="e2e41-200">Külső végpontok hozzáadása</span><span class="sxs-lookup"><span data-stu-id="e2e41-200">Adding External Endpoints</span></span>

<span data-ttu-id="e2e41-201">Külső végpontok száma toodirect forgalom tooservices tárolt Azure-on kívüli Manager által használt forgalom.</span><span class="sxs-lookup"><span data-stu-id="e2e41-201">Traffic Manager uses external endpoints toodirect traffic tooservices hosted outside of Azure.</span></span> <span data-ttu-id="e2e41-202">Mivel az Azure-végpontok, külső végpontok hozzáadása is lehetséges vagy használatával `Add-AzureRmTrafficManagerEndpointConfig` követ `Set-AzureRmTrafficManagerProfile`, vagy `New-AzureRMTrafficManagerEndpoint`.</span><span class="sxs-lookup"><span data-stu-id="e2e41-202">As with Azure endpoints, external endpoints can be added either using `Add-AzureRmTrafficManagerEndpointConfig` followed by `Set-AzureRmTrafficManagerProfile`, or `New-AzureRMTrafficManagerEndpoint`.</span></span>

<span data-ttu-id="e2e41-203">Külső végpontok száma megadásakor:</span><span class="sxs-lookup"><span data-stu-id="e2e41-203">When specifying external endpoints:</span></span>

* <span data-ttu-id="e2e41-204">hello végpont tartománynév hello "Target" paraméter használatával kell megadni</span><span class="sxs-lookup"><span data-stu-id="e2e41-204">hello endpoint domain name must be specified using hello 'Target' parameter</span></span>
* <span data-ttu-id="e2e41-205">Hello "Teljesítmény" forgalom-útválasztási módszer használata esetén a "EndpointLocation" hello megadása kötelező.</span><span class="sxs-lookup"><span data-stu-id="e2e41-205">If hello 'Performance' traffic-routing method is used, hello 'EndpointLocation' is required.</span></span> <span data-ttu-id="e2e41-206">Ellenkező esetben nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="e2e41-206">Otherwise it is optional.</span></span> <span data-ttu-id="e2e41-207">hello értéknek kell lennie egy [érvényes Azure-régiót neve](https://azure.microsoft.com/regions/).</span><span class="sxs-lookup"><span data-stu-id="e2e41-207">hello value must be a [valid Azure region name](https://azure.microsoft.com/regions/).</span></span>
* <span data-ttu-id="e2e41-208">hello "Súlyozási" és "Priority" választható.</span><span class="sxs-lookup"><span data-stu-id="e2e41-208">hello 'Weight' and 'Priority' are optional.</span></span>

### <a name="example-1-adding-external-endpoints-using-add-azurermtrafficmanagerendpointconfig-and-set-azurermtrafficmanagerprofile"></a><span data-ttu-id="e2e41-209">1. példa: Használó külső végpontok hozzáadása `Add-AzureRmTrafficManagerEndpointConfig` és`Set-AzureRmTrafficManagerProfile`</span><span class="sxs-lookup"><span data-stu-id="e2e41-209">Example 1: Adding external endpoints using `Add-AzureRmTrafficManagerEndpointConfig` and `Set-AzureRmTrafficManagerProfile`</span></span>

<span data-ttu-id="e2e41-210">Ebben a példában azt Traffic Manager-profil létrehozása, vegye fel a két külső végpont és hello változtatások véglegesítése a határidő.</span><span class="sxs-lookup"><span data-stu-id="e2e41-210">In this example, we create a Traffic Manager profile, add two external endpoints, and commit hello changes.</span></span>

```powershell
$profile = New-AzureRmTrafficManagerProfile -Name myprofile -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName myapp -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
Add-AzureRmTrafficManagerEndpointConfig -EndpointName eu-endpoint -TrafficManagerProfile $profile -Type ExternalEndpoints -Target app-eu.contoso.com -EndpointLocation "North Europe" -EndpointStatus Enabled
Add-AzureRmTrafficManagerEndpointConfig -EndpointName us-endpoint -TrafficManagerProfile $profile -Type ExternalEndpoints -Target app-us.contoso.com -EndpointLocation "Central US" -EndpointStatus Enabled
Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile
```

### <a name="example-2-adding-external-endpoints-using-new-azurermtrafficmanagerendpoint"></a><span data-ttu-id="e2e41-211">2. példa: Hozzáadása használata külső végpontok száma`New-AzureRmTrafficManagerEndpoint`</span><span class="sxs-lookup"><span data-stu-id="e2e41-211">Example 2: Adding external endpoints using `New-AzureRmTrafficManagerEndpoint`</span></span>

<span data-ttu-id="e2e41-212">Ebben a példában a Microsoft egy külső végpont tooan meglévő profil hozzáadásához.</span><span class="sxs-lookup"><span data-stu-id="e2e41-212">In this example, we add an external endpoint tooan existing profile.</span></span> <span data-ttu-id="e2e41-213">hello-profil van megadva, hello-profil és az erőforrás nevének használatával.</span><span class="sxs-lookup"><span data-stu-id="e2e41-213">hello profile is specified using hello profile and resource group names.</span></span>

```powershell
New-AzureRmTrafficManagerEndpoint -Name eu-endpoint -ProfileName MyProfile -ResourceGroupName MyRG -Type ExternalEndpoints -Target app-eu.contoso.com -EndpointStatus Enabled
```

## <a name="adding-nested-endpoints"></a><span data-ttu-id="e2e41-214">"Nested" végpontok hozzáadása</span><span class="sxs-lookup"><span data-stu-id="e2e41-214">Adding 'Nested' endpoints</span></span>

<span data-ttu-id="e2e41-215">Minden egyes Traffic Manager-profil megadja egy forgalom-útválasztási módszert.</span><span class="sxs-lookup"><span data-stu-id="e2e41-215">Each Traffic Manager profile specifies a single traffic-routing method.</span></span> <span data-ttu-id="e2e41-216">Vannak azonban forgatókönyv esetén van szükség, kifinomultabb a forgalom útválasztásához, mint egyetlen Traffic Manager-profil által biztosított hello útválasztást.</span><span class="sxs-lookup"><span data-stu-id="e2e41-216">However, there are scenarios that require more sophisticated traffic routing than hello routing provided by a single Traffic Manager profile.</span></span> <span data-ttu-id="e2e41-217">A Traffic Manager profilok toocombine hello előnyei egynél több forgalom-útválasztási módszert ágyazhatja be.</span><span class="sxs-lookup"><span data-stu-id="e2e41-217">You can nest Traffic Manager profiles toocombine hello benefits of more than one traffic-routing method.</span></span> <span data-ttu-id="e2e41-218">Beágyazott profilok lehetővé teszik toooverride hello alapértelmezett Traffic Manager viselkedés toosupport nagyobb és összetettebb alkalmazások központi telepítéseit.</span><span class="sxs-lookup"><span data-stu-id="e2e41-218">Nested profiles allow you toooverride hello default Traffic Manager behavior toosupport larger and more complex application deployments.</span></span> <span data-ttu-id="e2e41-219">További részletes példák: [beágyazott Traffic Manager-profilok](traffic-manager-nested-profiles.md).</span><span class="sxs-lookup"><span data-stu-id="e2e41-219">For more detailed examples, see [Nested Traffic Manager profiles](traffic-manager-nested-profiles.md).</span></span>

<span data-ttu-id="e2e41-220">Beágyazott végpontok hello szülő profilnál, egy adott típusú végpont, "NestedEndpoints" használatával vannak konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="e2e41-220">Nested endpoints are configured at hello parent profile, using a specific endpoint type, 'NestedEndpoints'.</span></span> <span data-ttu-id="e2e41-221">Beágyazott végpontok megadásakor:</span><span class="sxs-lookup"><span data-stu-id="e2e41-221">When specifying nested endpoints:</span></span>

* <span data-ttu-id="e2e41-222">hello végpont hello "targetresourceid azonosítója" paraméter használatával kell megadni</span><span class="sxs-lookup"><span data-stu-id="e2e41-222">hello endpoint must be specified using hello 'targetResourceId' parameter</span></span>
* <span data-ttu-id="e2e41-223">Hello "Teljesítmény" forgalom-útválasztási módszer használata esetén a "EndpointLocation" hello megadása kötelező.</span><span class="sxs-lookup"><span data-stu-id="e2e41-223">If hello 'Performance' traffic-routing method is used, hello 'EndpointLocation' is required.</span></span> <span data-ttu-id="e2e41-224">Ellenkező esetben nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="e2e41-224">Otherwise it is optional.</span></span> <span data-ttu-id="e2e41-225">hello értéknek kell lennie egy [érvényes Azure-régiót neve](http://azure.microsoft.com/regions/).</span><span class="sxs-lookup"><span data-stu-id="e2e41-225">hello value must be a [valid Azure region name](http://azure.microsoft.com/regions/).</span></span>
* <span data-ttu-id="e2e41-226">hello "Súlyozási" és "Priority" nem kötelező megadni, mint az Azure-végpontok.</span><span class="sxs-lookup"><span data-stu-id="e2e41-226">hello 'Weight' and 'Priority' are optional, as for Azure endpoints.</span></span>
* <span data-ttu-id="e2e41-227">hello "MinChildEndpoints" paraméter nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="e2e41-227">hello 'MinChildEndpoints' parameter is optional.</span></span> <span data-ttu-id="e2e41-228">hello alapértelmezett értéke "1".</span><span class="sxs-lookup"><span data-stu-id="e2e41-228">hello default value is '1'.</span></span> <span data-ttu-id="e2e41-229">Ha hello elérhető végpontok száma a küszöbérték alá esik, hello szülő profil úgy véli, hello gyermek profil "csökkentett teljesítményű", és hozunk forgalom toohello hello szülő profil végpontja.</span><span class="sxs-lookup"><span data-stu-id="e2e41-229">If hello number of available endpoints falls below this threshold, hello parent profile considers hello child profile 'degraded' and diverts traffic toohello other endpoints in hello parent profile.</span></span>

### <a name="example-1-adding-nested-endpoints-using-add-azurermtrafficmanagerendpointconfig-and-set-azurermtrafficmanagerprofile"></a><span data-ttu-id="e2e41-230">1. példa: Hozzáadása beágyazott végpontjaitól `Add-AzureRmTrafficManagerEndpointConfig` és`Set-AzureRmTrafficManagerProfile`</span><span class="sxs-lookup"><span data-stu-id="e2e41-230">Example 1: Adding nested endpoints using `Add-AzureRmTrafficManagerEndpointConfig` and `Set-AzureRmTrafficManagerProfile`</span></span>

<span data-ttu-id="e2e41-231">Ebben a példában a Microsoft új Traffic Manager gyermek és szülő profilok létrehozása hello gyermek hozzáadása beágyazott végpont toohello szülőjeként és hello változtatások véglegesítése a határidő.</span><span class="sxs-lookup"><span data-stu-id="e2e41-231">In this example, we create new Traffic Manager child and parent profiles, add hello child as a nested endpoint toohello parent, and commit hello changes.</span></span>

```powershell
$child = New-AzureRmTrafficManagerProfile -Name child -ResourceGroupName MyRG -TrafficRoutingMethod Priority -RelativeDnsName child -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
$parent = New-AzureRmTrafficManagerProfile -Name parent -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName parent -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
Add-AzureRmTrafficManagerEndpointConfig -EndpointName child-endpoint -TrafficManagerProfile $parent -Type NestedEndpoints -TargetResourceId $child.Id -EndpointStatus Enabled -EndpointLocation "North Europe" -MinChildEndpoints 2
Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile
```

<span data-ttu-id="e2e41-232">Ebben a példában kivonatosan mutatja bármely más végpontok toohello gyermek vagy szülő profilok nem azt adta hozzá.</span><span class="sxs-lookup"><span data-stu-id="e2e41-232">For brevity in this example, we did not add any other endpoints toohello child or parent profiles.</span></span>

### <a name="example-2-adding-nested-endpoints-using-new-azurermtrafficmanagerendpoint"></a><span data-ttu-id="e2e41-233">2. példa: Használó beágyazott végpontok hozzáadása`New-AzureRmTrafficManagerEndpoint`</span><span class="sxs-lookup"><span data-stu-id="e2e41-233">Example 2: Adding nested endpoints using `New-AzureRmTrafficManagerEndpoint`</span></span>

<span data-ttu-id="e2e41-234">Ebben a példában jelenleg egy meglévő gyermek-profilt a beágyazott végpont tooan meglévő szülő tanúsítványprofilként kell felvenni.</span><span class="sxs-lookup"><span data-stu-id="e2e41-234">In this example, we add an existing child profile as a nested endpoint tooan existing parent profile.</span></span> <span data-ttu-id="e2e41-235">hello-profil van megadva, hello-profil és az erőforrás nevének használatával.</span><span class="sxs-lookup"><span data-stu-id="e2e41-235">hello profile is specified using hello profile and resource group names.</span></span>

```powershell
$child = Get-AzureRmTrafficManagerEndpoint -Name child -ResourceGroupName MyRG
New-AzureRmTrafficManagerEndpoint -Name child-endpoint -ProfileName parent -ResourceGroupName MyRG -Type NestedEndpoints -TargetResourceId $child.Id -EndpointStatus Enabled -EndpointLocation "North Europe" -MinChildEndpoints 2
```

## <a name="update-a-traffic-manager-endpoint"></a><span data-ttu-id="e2e41-236">A Traffic Manager-végpont frissítése</span><span class="sxs-lookup"><span data-stu-id="e2e41-236">Update a Traffic Manager Endpoint</span></span>

<span data-ttu-id="e2e41-237">Számos két módon tooupdate egy meglévő Traffic Manager-végpontot.</span><span class="sxs-lookup"><span data-stu-id="e2e41-237">There are two ways tooupdate an existing Traffic Manager endpoint:</span></span>

1. <span data-ttu-id="e2e41-238">Első hello Traffic Manager-profil használatával `Get-AzureRmTrafficManagerProfile`hello profilon belül hello végpont tulajdonságainak frissítéséhez és használatával hello változtatások véglegesítése a határidő `Set-AzureRmTrafficManagerProfile`.</span><span class="sxs-lookup"><span data-stu-id="e2e41-238">Get hello Traffic Manager profile using `Get-AzureRmTrafficManagerProfile`, update hello endpoint properties within hello profile, and commit hello changes using `Set-AzureRmTrafficManagerProfile`.</span></span> <span data-ttu-id="e2e41-239">Ez a módszer hello előnye, hogy képes tooupdate egy művelettel több végpont rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="e2e41-239">This method has hello advantage of being able tooupdate more than one endpoint in a single operation.</span></span>
2. <span data-ttu-id="e2e41-240">Első hello Traffic Manager-végpontot a `Get-AzureRmTrafficManagerEndpoint`hello végpont tulajdonságainak frissítéséhez és használatával hello változtatások véglegesítése a határidő `Set-AzureRmTrafficManagerEndpoint`.</span><span class="sxs-lookup"><span data-stu-id="e2e41-240">Get hello Traffic Manager endpoint using `Get-AzureRmTrafficManagerEndpoint`, update hello endpoint properties, and commit hello changes using `Set-AzureRmTrafficManagerEndpoint`.</span></span> <span data-ttu-id="e2e41-241">Ez a módszer is egyszerűbb, mivel nem igényel indexelő hello-profil hello végpontok tömbbe.</span><span class="sxs-lookup"><span data-stu-id="e2e41-241">This method is simpler, since it does not require indexing into hello Endpoints array in hello profile.</span></span>

### <a name="example-1-updating-endpoints-using-get-azurermtrafficmanagerprofile-and-set-azurermtrafficmanagerprofile"></a><span data-ttu-id="e2e41-242">1. példa: Frissítése végpontjaitól `Get-AzureRmTrafficManagerProfile` és`Set-AzureRmTrafficManagerProfile`</span><span class="sxs-lookup"><span data-stu-id="e2e41-242">Example 1: Updating endpoints using `Get-AzureRmTrafficManagerProfile` and `Set-AzureRmTrafficManagerProfile`</span></span>

<span data-ttu-id="e2e41-243">Ebben a példában azt módosítani egy meglévő profilt belül két végpontokon hello prioritású virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="e2e41-243">In this example, we modify hello priority on two endpoints within an existing profile.</span></span>

```powershell
$profile = Get-AzureRmTrafficManagerProfile -Name myprofile -ResourceGroupName MyRG
$profile.Endpoints[0].Priority = 2
$profile.Endpoints[1].Priority = 1
Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile
```

### <a name="example-2-updating-an-endpoint-using-get-azurermtrafficmanagerendpoint-and-set-azurermtrafficmanagerendpoint"></a><span data-ttu-id="e2e41-244">2. példa: Frissítése egy végpontot a `Get-AzureRmTrafficManagerEndpoint` és`Set-AzureRmTrafficManagerEndpoint`</span><span class="sxs-lookup"><span data-stu-id="e2e41-244">Example 2: Updating an endpoint using `Get-AzureRmTrafficManagerEndpoint` and `Set-AzureRmTrafficManagerEndpoint`</span></span>

<span data-ttu-id="e2e41-245">Ebben a példában azt módosítani egy meglévő profil egyetlen végpont súlyának hello.</span><span class="sxs-lookup"><span data-stu-id="e2e41-245">In this example, we modify hello weight of a single endpoint in an existing profile.</span></span>

```powershell
$endpoint = Get-AzureRmTrafficManagerEndpoint -Name myendpoint -ProfileName myprofile -ResourceGroupName MyRG -Type ExternalEndpoints
$endpoint.Weight = 20
Set-AzureRmTrafficManagerEndpoint -TrafficManagerEndpoint $endpoint
```

## <a name="enabling-and-disabling-endpoints-and-profiles"></a><span data-ttu-id="e2e41-246">Engedélyezése és letiltása a végpontok és profilok</span><span class="sxs-lookup"><span data-stu-id="e2e41-246">Enabling and Disabling Endpoints and Profiles</span></span>

<span data-ttu-id="e2e41-247">A TRAFFIC Manager lehetővé teszi, hogy az egyes végpontok toobe engedélyezve van, és le van tiltva, valamint engedélyezése és letiltása teljes profilok engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="e2e41-247">Traffic Manager allows individual endpoints toobe enabled and disabled, as well as allowing enabling and disabling of entire profiles.</span></span>
<span data-ttu-id="e2e41-248">Ezek a változások első/frissítése/beállítás hello végpont vagy profil erőforrásokat is végezhető.</span><span class="sxs-lookup"><span data-stu-id="e2e41-248">These changes can be made by getting/updating/setting hello endpoint or profile resources.</span></span> <span data-ttu-id="e2e41-249">toostreamline e közös műveletek támogatottak továbbá akkor dedikált parancsmagok segítségével.</span><span class="sxs-lookup"><span data-stu-id="e2e41-249">toostreamline these common operations, they are also supported via dedicated cmdlets.</span></span>

### <a name="example-1-enabling-and-disabling-a-traffic-manager-profile"></a><span data-ttu-id="e2e41-250">1. példa: Engedélyezése és letiltása Traffic Manager-profil</span><span class="sxs-lookup"><span data-stu-id="e2e41-250">Example 1: Enabling and disabling a Traffic Manager profile</span></span>

<span data-ttu-id="e2e41-251">Traffic Manager-profil tooenable használja `Enable-AzureRmTrafficManagerProfile`.</span><span class="sxs-lookup"><span data-stu-id="e2e41-251">tooenable a Traffic Manager profile, use `Enable-AzureRmTrafficManagerProfile`.</span></span> <span data-ttu-id="e2e41-252">hello-profil egy profil objektum adható meg.</span><span class="sxs-lookup"><span data-stu-id="e2e41-252">hello profile can be specified using a profile object.</span></span> <span data-ttu-id="e2e41-253">hello-profil objektumban átadhatók keresztül hello csővezeték vagy hello segítségével "-TrafficManagerProfile" paraméter.</span><span class="sxs-lookup"><span data-stu-id="e2e41-253">hello profile object can be passed via hello pipeline or by using hello '-TrafficManagerProfile' parameter.</span></span> <span data-ttu-id="e2e41-254">Ebben a példában azt adja meg hello-profil hello-profil és az erőforrás neve.</span><span class="sxs-lookup"><span data-stu-id="e2e41-254">In this example, we specify hello profile by hello profile and resource group name.</span></span>

```powershell
Enable-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyResourceGroup
```

<span data-ttu-id="e2e41-255">Traffic Manager-profil toodisable:</span><span class="sxs-lookup"><span data-stu-id="e2e41-255">toodisable a Traffic Manager profile:</span></span>

```powershell
Disable-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyResourceGroup
```

<span data-ttu-id="e2e41-256">hello Disable-AzureRmTrafficManagerProfile parancsmag megerősítés kéri.</span><span class="sxs-lookup"><span data-stu-id="e2e41-256">hello Disable-AzureRmTrafficManagerProfile cmdlet prompts for confirmation.</span></span> <span data-ttu-id="e2e41-257">Ez a kérdés hello letilthatók "-Force" paraméter.</span><span class="sxs-lookup"><span data-stu-id="e2e41-257">This prompt can be suppressed using hello '-Force' parameter.</span></span>

### <a name="example-2-enabling-and-disabling-a-traffic-manager-endpoint"></a><span data-ttu-id="e2e41-258">2. példa: Engedélyezése és letiltása Traffic Manager-végpontot</span><span class="sxs-lookup"><span data-stu-id="e2e41-258">Example 2: Enabling and disabling a Traffic Manager endpoint</span></span>

<span data-ttu-id="e2e41-259">a Traffic Manager-végpont tooenable használja `Enable-AzureRmTrafficManagerEndpoint`.</span><span class="sxs-lookup"><span data-stu-id="e2e41-259">tooenable a Traffic Manager endpoint, use `Enable-AzureRmTrafficManagerEndpoint`.</span></span> <span data-ttu-id="e2e41-260">Nincsenek a két módon toospecify hello végpont</span><span class="sxs-lookup"><span data-stu-id="e2e41-260">There are two ways toospecify hello endpoint</span></span>

1. <span data-ttu-id="e2e41-261">Egy hello csővezeték átadása TrafficManagerEndpoint objektum használatával vagy az hello "-TrafficManagerEndpoint" paraméter</span><span class="sxs-lookup"><span data-stu-id="e2e41-261">Using a TrafficManagerEndpoint object passed via hello pipeline or using hello '-TrafficManagerEndpoint' parameter</span></span>
2. <span data-ttu-id="e2e41-262">Hello a végpont neve, a típusú végpont, a profil nevét és a erőforráscsoport-név használatával:</span><span class="sxs-lookup"><span data-stu-id="e2e41-262">Using hello endpoint name, endpoint type, profile name, and resource group name:</span></span>

```powershell
Enable-AzureRmTrafficManagerEndpoint -Name MyEndpoint -Type AzureEndpoints -ProfileName MyProfile -ResourceGroupName MyRG
```

<span data-ttu-id="e2e41-263">Ehhez hasonlóan toodisable egy Traffic Manager-végpont:</span><span class="sxs-lookup"><span data-stu-id="e2e41-263">Similarly, toodisable a Traffic Manager endpoint:</span></span>

```powershell
Disable-AzureRmTrafficManagerEndpoint -Name MyEndpoint -Type AzureEndpoints -ProfileName MyProfile -ResourceGroupName MyRG -Force
```

<span data-ttu-id="e2e41-264">A `Disable-AzureRmTrafficManagerProfile`, hello `Disable-AzureRmTrafficManagerEndpoint` parancsmag jóváhagyást kér.</span><span class="sxs-lookup"><span data-stu-id="e2e41-264">As with `Disable-AzureRmTrafficManagerProfile`, hello `Disable-AzureRmTrafficManagerEndpoint` cmdlet prompts for confirmation.</span></span> <span data-ttu-id="e2e41-265">Ez a kérdés hello letilthatók "-Force" paraméter.</span><span class="sxs-lookup"><span data-stu-id="e2e41-265">This prompt can be suppressed using hello '-Force' parameter.</span></span>

## <a name="delete-a-traffic-manager-endpoint"></a><span data-ttu-id="e2e41-266">A Traffic Manager-végpont törlése</span><span class="sxs-lookup"><span data-stu-id="e2e41-266">Delete a Traffic Manager Endpoint</span></span>

<span data-ttu-id="e2e41-267">tooremove egyedi végpontok, használja a hello `Remove-AzureRmTrafficManagerEndpoint` parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="e2e41-267">tooremove individual endpoints, use hello `Remove-AzureRmTrafficManagerEndpoint` cmdlet:</span></span>

```powershell
Remove-AzureRmTrafficManagerEndpoint -Name MyEndpoint -Type AzureEndpoints -ProfileName MyProfile -ResourceGroupName MyRG
```

<span data-ttu-id="e2e41-268">Ez a parancsmag felszólítja megerősítést kér.</span><span class="sxs-lookup"><span data-stu-id="e2e41-268">This cmdlet prompts for confirmation.</span></span> <span data-ttu-id="e2e41-269">Ez a kérdés hello letilthatók "-Force" paraméter.</span><span class="sxs-lookup"><span data-stu-id="e2e41-269">This prompt can be suppressed using hello '-Force' parameter.</span></span>

## <a name="delete-a-traffic-manager-profile"></a><span data-ttu-id="e2e41-270">Traffic Manager-profil törlése</span><span class="sxs-lookup"><span data-stu-id="e2e41-270">Delete a Traffic Manager Profile</span></span>

<span data-ttu-id="e2e41-271">Traffic Manager-profil toodelete hello használata `Remove-AzureRmTrafficManagerProfile` parancsmag hello-profil és az erőforrás nevének megadása:</span><span class="sxs-lookup"><span data-stu-id="e2e41-271">toodelete a Traffic Manager profile, use hello `Remove-AzureRmTrafficManagerProfile` cmdlet, specifying hello profile and resource group names:</span></span>

```powershell
Remove-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG [-Force]
```

<span data-ttu-id="e2e41-272">Ez a parancsmag felszólítja megerősítést kér.</span><span class="sxs-lookup"><span data-stu-id="e2e41-272">This cmdlet prompts for confirmation.</span></span> <span data-ttu-id="e2e41-273">Ez a kérdés hello letilthatók "-Force" paraméter.</span><span class="sxs-lookup"><span data-stu-id="e2e41-273">This prompt can be suppressed using hello '-Force' parameter.</span></span>

<span data-ttu-id="e2e41-274">törölt hello-profil toobe is is meg kell adni egy profil objektummal:</span><span class="sxs-lookup"><span data-stu-id="e2e41-274">hello profile toobe deleted can also be specified using a profile object:</span></span>

```powershell
$profile = Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG
Remove-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile [-Force]
```

<span data-ttu-id="e2e41-275">Ebben a sorozatban is átirányítható:</span><span class="sxs-lookup"><span data-stu-id="e2e41-275">This sequence can also be piped:</span></span>

```powershell
Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG | Remove-AzureRmTrafficManagerProfile [-Force]
```

## <a name="next-steps"></a><span data-ttu-id="e2e41-276">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e2e41-276">Next steps</span></span>

[<span data-ttu-id="e2e41-277">A TRAFFIC Manager figyelése</span><span class="sxs-lookup"><span data-stu-id="e2e41-277">Traffic Manager monitoring</span></span>](traffic-manager-monitoring.md)

[<span data-ttu-id="e2e41-278">A Traffic Manager teljesítményével kapcsolatos megfontolások</span><span class="sxs-lookup"><span data-stu-id="e2e41-278">Traffic Manager performance considerations</span></span>](traffic-manager-performance-considerations.md)
