---
title: "Az Azure Traffic Manager kezelése a PowerShell használatával |} Microsoft Docs"
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
ms.openlocfilehash: 1cd7bd7e32c96398d72c7cd3b51e2b456d60f01d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="using-powershell-to-manage-traffic-manager"></a><span data-ttu-id="60202-103">A Traffic Manager kezelése a PowerShell segítségével</span><span class="sxs-lookup"><span data-stu-id="60202-103">Using PowerShell to manage Traffic Manager</span></span>

<span data-ttu-id="60202-104">Az Azure Resource Manager rendszer az előnyben részesített felügyeleti szolgáltatások az Azure-ban,</span><span class="sxs-lookup"><span data-stu-id="60202-104">Azure Resource Manager is the preferred management interface for services in Azure.</span></span> <span data-ttu-id="60202-105">Az Azure Traffic Manager-profilok használatával az Azure Resource Manager-alapú API-k és eszközök is felügyelhetők.</span><span class="sxs-lookup"><span data-stu-id="60202-105">Azure Traffic Manager profiles can be managed using Azure Resource Manager-based APIs and tools.</span></span>

## <a name="resource-model"></a><span data-ttu-id="60202-106">Erőforrás-modellje</span><span class="sxs-lookup"><span data-stu-id="60202-106">Resource model</span></span>

<span data-ttu-id="60202-107">Az Azure Traffic Manager a Traffic Manager-profil neve beállítások segítségével konfigurálható.</span><span class="sxs-lookup"><span data-stu-id="60202-107">Azure Traffic Manager is configured using a collection of settings called a Traffic Manager profile.</span></span> <span data-ttu-id="60202-108">Ehhez a profilhoz DNS-beállítások, forgalom útválasztásához tartozó beállításokat, végpontmonitoring beállításai, és, amelyhez továbbítódik végpontok listáját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="60202-108">This profile contains DNS settings, traffic routing settings, endpoint monitoring settings, and a list of service endpoints to which traffic is routed.</span></span>

<span data-ttu-id="60202-109">Minden egyes Traffic Manager-profil "TrafficManagerProfiles" típusú erőforrást jelképezi.</span><span class="sxs-lookup"><span data-stu-id="60202-109">Each Traffic Manager profile is represented by a resource of type 'TrafficManagerProfiles'.</span></span> <span data-ttu-id="60202-110">A REST API-szintű URI-JÁNAK minden profil a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="60202-110">At the REST API level, the URI for each profile is as follows:</span></span>

    https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Network/trafficManagerProfiles/{profile-name}?api-version={api-version}

## <a name="setting-up-azure-powershell"></a><span data-ttu-id="60202-111">Azure PowerShell telepítése</span><span class="sxs-lookup"><span data-stu-id="60202-111">Setting up Azure PowerShell</span></span>

<span data-ttu-id="60202-112">Ezek az utasítások a Microsoft Azure PowerShell használata.</span><span class="sxs-lookup"><span data-stu-id="60202-112">These instructions use Microsoft Azure PowerShell.</span></span> <span data-ttu-id="60202-113">A következő cikk ismerteti az Azure PowerShell telepítése és konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="60202-113">The following article explains how to install and configure Azure PowerShell.</span></span>

* [<span data-ttu-id="60202-114">How to install and configure Azure PowerShell (Az Azure PowerShell telepítése és konfigurálása)</span><span class="sxs-lookup"><span data-stu-id="60202-114">How to install and configure Azure PowerShell</span></span>](/powershell/azure/overview)

<span data-ttu-id="60202-115">Ebben a cikkben szereplő példák azt feltételezik, hogy rendelkezik-e egy meglévő erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="60202-115">The examples in this article assume that you have an existing resource group.</span></span> <span data-ttu-id="60202-116">Létrehozhat egy olyan erőforráscsoport, a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="60202-116">You can create a resource group using the following command:</span></span>

```powershell
New-AzureRmResourceGroup -Name MyRG -Location "West US"
```

> [!NOTE]
> <span data-ttu-id="60202-117">Az Azure Resource Manager megköveteli, hogy a hely összes erőforráscsoport.</span><span class="sxs-lookup"><span data-stu-id="60202-117">Azure Resource Manager requires that all resource groups have a location.</span></span> <span data-ttu-id="60202-118">Ezen a helyen az alapértelmezett szolgál az erőforráscsoport létrejött erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="60202-118">This location is used as the default for resources created in that resource group.</span></span> <span data-ttu-id="60202-119">Azonban mivel a Traffic Manager-profil erőforrások globális, nem pedig regionális, a választott erőforráscsoport helye nincs hatással van az Azure Traffic Manager.</span><span class="sxs-lookup"><span data-stu-id="60202-119">However, since Traffic Manager profile resources are global, not regional, the choice of resource group location has no impact on Azure Traffic Manager.</span></span>

## <a name="create-a-traffic-manager-profile"></a><span data-ttu-id="60202-120">A Traffic Manager-profil létrehozása</span><span class="sxs-lookup"><span data-stu-id="60202-120">Create a Traffic Manager Profile</span></span>

<span data-ttu-id="60202-121">Traffic Manager-profil létrehozásához használja a `New-AzureRmTrafficManagerProfile` parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="60202-121">To create a Traffic Manager profile, use the `New-AzureRmTrafficManagerProfile` cmdlet:</span></span>

```powershell
$profile = New-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName contoso -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
```

<span data-ttu-id="60202-122">A következő táblázat a paramétereket:</span><span class="sxs-lookup"><span data-stu-id="60202-122">The following table describes the parameters:</span></span>

| <span data-ttu-id="60202-123">Paraméter</span><span class="sxs-lookup"><span data-stu-id="60202-123">Parameter</span></span> | <span data-ttu-id="60202-124">Leírás</span><span class="sxs-lookup"><span data-stu-id="60202-124">Description</span></span> |
| --- | --- |
| <span data-ttu-id="60202-125">Név</span><span class="sxs-lookup"><span data-stu-id="60202-125">Name</span></span> |<span data-ttu-id="60202-126">A Traffic Manager-profil erőforrás erőforrás neve.</span><span class="sxs-lookup"><span data-stu-id="60202-126">The resource name for the Traffic Manager profile resource.</span></span> <span data-ttu-id="60202-127">Profilok ugyanabban az erőforráscsoportban egyedi névvel kell rendelkezniük.</span><span class="sxs-lookup"><span data-stu-id="60202-127">Profiles in the same resource group must have unique names.</span></span> <span data-ttu-id="60202-128">Ez a név nem azonos a DNS-lekérdezések használt DNS-név.</span><span class="sxs-lookup"><span data-stu-id="60202-128">This name is separate from the DNS name used for DNS queries.</span></span> |
| <span data-ttu-id="60202-129">erőforráscsoport-név</span><span class="sxs-lookup"><span data-stu-id="60202-129">ResourceGroupName</span></span> |<span data-ttu-id="60202-130">A profil erőforrást tartalmazó erőforráscsoport neve.</span><span class="sxs-lookup"><span data-stu-id="60202-130">The name of the resource group containing the profile resource.</span></span> |
| <span data-ttu-id="60202-131">TrafficRoutingMethod</span><span class="sxs-lookup"><span data-stu-id="60202-131">TrafficRoutingMethod</span></span> |<span data-ttu-id="60202-132">Megadja a forgalom-útválasztási módszert határozza meg, melyik végponthoz válaszként visszaadott a DNS-lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="60202-132">Specifies the traffic-routing method used to determine which endpoint is returned in response a DNS query.</span></span> <span data-ttu-id="60202-133">Lehetséges értékek: "Teljesítmény", "Weighted" vagy "Priority".</span><span class="sxs-lookup"><span data-stu-id="60202-133">Possible values are 'Performance', 'Weighted' or 'Priority'.</span></span> |
| <span data-ttu-id="60202-134">RelativeDnsName</span><span class="sxs-lookup"><span data-stu-id="60202-134">RelativeDnsName</span></span> |<span data-ttu-id="60202-135">Adja meg a Traffic Manager-profil által biztosított DNS-nevét hostname része.</span><span class="sxs-lookup"><span data-stu-id="60202-135">Specifies the hostname portion of the DNS name provided by this Traffic Manager profile.</span></span> <span data-ttu-id="60202-136">Ezt az értéket a DNS-tartománynevét, Azure Traffic Manager által használt a teljes tartománynevét (FQDN) a profil együtt.</span><span class="sxs-lookup"><span data-stu-id="60202-136">This value is combined with the DNS domain name used by Azure Traffic Manager to form the fully qualified domain name (FQDN) of the profile.</span></span> <span data-ttu-id="60202-137">Például "contoso" értékre állítja lesz "contoso.trafficmanager.net."</span><span class="sxs-lookup"><span data-stu-id="60202-137">For example, setting the value of 'contoso' becomes 'contoso.trafficmanager.net.'</span></span> |
| <span data-ttu-id="60202-138">ÉLETTARTAM</span><span class="sxs-lookup"><span data-stu-id="60202-138">TTL</span></span> |<span data-ttu-id="60202-139">Adja meg a DNS idő élettartamát (TTL).</span><span class="sxs-lookup"><span data-stu-id="60202-139">Specifies the DNS Time-to-Live (TTL), in seconds.</span></span> <span data-ttu-id="60202-140">Az élettartam tájékoztatja a helyi DNS-feloldókat és a DNS-ügyfelek mennyi a gyorsítótár DNS-válaszok a Traffic Manager-profil.</span><span class="sxs-lookup"><span data-stu-id="60202-140">This TTL informs the Local DNS resolvers and DNS clients how long to cache DNS responses for this Traffic Manager profile.</span></span> |
| <span data-ttu-id="60202-141">MonitorProtocol</span><span class="sxs-lookup"><span data-stu-id="60202-141">MonitorProtocol</span></span> |<span data-ttu-id="60202-142">Adja meg a végpont állapotának figyeléséhez használni kívánt protokollt.</span><span class="sxs-lookup"><span data-stu-id="60202-142">Specifies the protocol to use to monitor endpoint health.</span></span> <span data-ttu-id="60202-143">Lehetséges értékek: "HTTP" és "HTTPS".</span><span class="sxs-lookup"><span data-stu-id="60202-143">Possible values are 'HTTP' and 'HTTPS'.</span></span> |
| <span data-ttu-id="60202-144">MonitorPort</span><span class="sxs-lookup"><span data-stu-id="60202-144">MonitorPort</span></span> |<span data-ttu-id="60202-145">Adja meg a végpont állapotának figyeléséhez használt TCP-portot.</span><span class="sxs-lookup"><span data-stu-id="60202-145">Specifies the TCP port used to monitor endpoint health.</span></span> |
| <span data-ttu-id="60202-146">MonitorPath</span><span class="sxs-lookup"><span data-stu-id="60202-146">MonitorPath</span></span> |<span data-ttu-id="60202-147">A végpont az végpont health mintavételi használt tartománynév relatív elérési út megadása</span><span class="sxs-lookup"><span data-stu-id="60202-147">Specifies the path relative to the endpoint domain name used to probe for endpoint health.</span></span> |

<span data-ttu-id="60202-148">A parancsmag egy Traffic Manager-profil létrehozása az Azure-ban, és egy megfelelő profil objektumot ad vissza a PowerShell.</span><span class="sxs-lookup"><span data-stu-id="60202-148">The cmdlet creates a Traffic Manager profile in Azure and returns a corresponding profile object to PowerShell.</span></span> <span data-ttu-id="60202-149">Ezen a ponton a profil nem tartalmaz végpontok.</span><span class="sxs-lookup"><span data-stu-id="60202-149">At this point, the profile does not contain any endpoints.</span></span> <span data-ttu-id="60202-150">Végpontok Traffic Manager-profil történő hozzáadásával kapcsolatos további információkért lásd: [Traffic Manager-végpontok hozzáadása](#adding-traffic-manager-endpoints).</span><span class="sxs-lookup"><span data-stu-id="60202-150">For more information about adding endpoints to a Traffic Manager profile, see [Adding Traffic Manager Endpoints](#adding-traffic-manager-endpoints).</span></span>

## <a name="get-a-traffic-manager-profile"></a><span data-ttu-id="60202-151">Traffic Manager-profil beolvasása</span><span class="sxs-lookup"><span data-stu-id="60202-151">Get a Traffic Manager Profile</span></span>

<span data-ttu-id="60202-152">Egy meglévő Traffic Manager-profil objektummal lekéréséhez használja a `Get-AzureRmTrafficManagerProfle` parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="60202-152">To retrieve an existing Traffic Manager profile object, use the `Get-AzureRmTrafficManagerProfle` cmdlet:</span></span>

```powershell
$profile = Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG
```

<span data-ttu-id="60202-153">Ez a parancsmag egy Traffic Manager-profil objektumot ad vissza.</span><span class="sxs-lookup"><span data-stu-id="60202-153">This cmdlet returns a Traffic Manager profile object.</span></span>

## <a name="update-a-traffic-manager-profile"></a><span data-ttu-id="60202-154">Traffic Manager-profil frissítése</span><span class="sxs-lookup"><span data-stu-id="60202-154">Update a Traffic Manager Profile</span></span>

<span data-ttu-id="60202-155">Traffic Manager-profilok módosítása a 3. lépés folyamatot követi:</span><span class="sxs-lookup"><span data-stu-id="60202-155">Modifying Traffic Manager profiles follows a 3-step process:</span></span>

1. <span data-ttu-id="60202-156">Le a profil `Get-AzureRmTrafficManagerProfile` , vagy használja a profil által visszaadott `New-AzureRmTrafficManagerProfile`.</span><span class="sxs-lookup"><span data-stu-id="60202-156">Retrieve the profile using `Get-AzureRmTrafficManagerProfile` or use the profile returned by `New-AzureRmTrafficManagerProfile`.</span></span>
2. <span data-ttu-id="60202-157">Módosítsa a profilt.</span><span class="sxs-lookup"><span data-stu-id="60202-157">Modify the profile.</span></span> <span data-ttu-id="60202-158">Adja hozzá, és távolítsa el a végpontok, vagy módosítsa a végpont vagy profil paraméterek.</span><span class="sxs-lookup"><span data-stu-id="60202-158">You can add and remove endpoints or change endpoint or profile parameters.</span></span> <span data-ttu-id="60202-159">A kapcsolat nélküli műveletek, amelyek ezeket a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="60202-159">These changes are off-line operations.</span></span> <span data-ttu-id="60202-160">Csak módosítja a helyi objektum a memóriában, amely a profil jelöli.</span><span class="sxs-lookup"><span data-stu-id="60202-160">You are only changing the local object in memory that represents the profile.</span></span>
3. <span data-ttu-id="60202-161">A módosítások végrehajtásához használja a `Set-AzureRmTrafficManagerProfile` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="60202-161">Commit your changes using the `Set-AzureRmTrafficManagerProfile` cmdlet.</span></span>

<span data-ttu-id="60202-162">Az összes profil tulajdonságai kivételével a profil RelativeDnsName módosíthatók.</span><span class="sxs-lookup"><span data-stu-id="60202-162">All profile properties can be changed except the profile's RelativeDnsName.</span></span> <span data-ttu-id="60202-163">Ha módosítani szeretné a RelativeDnsName, törölnie kell a profil és egy új nevet az új profil.</span><span class="sxs-lookup"><span data-stu-id="60202-163">To change the RelativeDnsName, you must delete profile and a new profile with a new name.</span></span>

<span data-ttu-id="60202-164">A következő példa bemutatja, hogyan módosíthatja a profil élettartam:</span><span class="sxs-lookup"><span data-stu-id="60202-164">The following example demonstrates how to change the profile's TTL:</span></span>

```powershell
$profile = Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG
$profile.Ttl = 300
Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile
```

<span data-ttu-id="60202-165">A Traffic Manager-végpontok három típusa van:</span><span class="sxs-lookup"><span data-stu-id="60202-165">There are three types of Traffic Manager endpoints:</span></span>

1. <span data-ttu-id="60202-166">**Azure-végpontok** Azure-ban üzemeltetett szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="60202-166">**Azure endpoints** are services hosted in Azure</span></span>
2. <span data-ttu-id="60202-167">**Külső végpontok száma** Azure-on kívüli üzemeltetett szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="60202-167">**External endpoints** are services hosted outside of Azure</span></span>
3. <span data-ttu-id="60202-168">**A beágyazott végpontok** beágyazott hierarchiák Traffic Manager-profilok létrehozásához használt.</span><span class="sxs-lookup"><span data-stu-id="60202-168">**Nested endpoints** are used to construct nested hierarchies of Traffic Manager profiles.</span></span> <span data-ttu-id="60202-169">Beágyazott végpontok összetett alkalmazások speciális forgalom-útválasztási konfigurációi engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="60202-169">Nested endpoints enable advanced traffic-routing configurations for complex applications.</span></span>

<span data-ttu-id="60202-170">A három esetben végpontok hozzáadása is lehetséges két módon:</span><span class="sxs-lookup"><span data-stu-id="60202-170">In all three cases, endpoints can be added in two ways:</span></span>

1. <span data-ttu-id="60202-171">A fentiekben ismertetett 3. lépés folyamatok használata.</span><span class="sxs-lookup"><span data-stu-id="60202-171">Using a 3-step process described previously.</span></span> <span data-ttu-id="60202-172">Ez a módszer előnye, hogy több végpont módosítás egy frissítés.</span><span class="sxs-lookup"><span data-stu-id="60202-172">The advantage of this method is that several endpoint changes can be made in a single update.</span></span>
2. <span data-ttu-id="60202-173">A New-AzureRmTrafficManagerEndpoint parancsmag használatával.</span><span class="sxs-lookup"><span data-stu-id="60202-173">Using the New-AzureRmTrafficManagerEndpoint cmdlet.</span></span> <span data-ttu-id="60202-174">Ez a parancsmag egy olyan végpont hozzáadása egy meglévő Traffic Manager-profil egy művelettel.</span><span class="sxs-lookup"><span data-stu-id="60202-174">This cmdlet adds an endpoint to an existing Traffic Manager profile in a single operation.</span></span>

## <a name="adding-azure-endpoints"></a><span data-ttu-id="60202-175">Azure-végpontok hozzáadása</span><span class="sxs-lookup"><span data-stu-id="60202-175">Adding Azure Endpoints</span></span>

<span data-ttu-id="60202-176">Azure-végpontok Azure-ban üzemeltetett szolgáltatások hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="60202-176">Azure endpoints reference services hosted in Azure.</span></span> <span data-ttu-id="60202-177">Azure-végpontok két típusú támogatottak:</span><span class="sxs-lookup"><span data-stu-id="60202-177">Two types of Azure endpoints are supported:</span></span>

1. <span data-ttu-id="60202-178">Azure Web Apps</span><span class="sxs-lookup"><span data-stu-id="60202-178">Azure Web Apps</span></span>
2. <span data-ttu-id="60202-179">Az Azure nyilvános erőforrások (csatolható terheléselosztó vagy egy virtuális gép hálózati adapter).</span><span class="sxs-lookup"><span data-stu-id="60202-179">Azure PublicIpAddress resources (which can be attached to a load-balancer or a virtual machine NIC).</span></span> <span data-ttu-id="60202-180">A nyilvános rendelt használható a Traffic Manager DNS névvel kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="60202-180">The PublicIpAddress must have a DNS name assigned to be used in Traffic Manager.</span></span>

<span data-ttu-id="60202-181">Minden esetben:</span><span class="sxs-lookup"><span data-stu-id="60202-181">In each case:</span></span>

* <span data-ttu-id="60202-182">A szolgáltatás adott meg a "targetresourceid azonosítója" paraméterének `Add-AzureRmTrafficManagerEndpointConfig` vagy `New-AzureRmTrafficManagerEndpoint`.</span><span class="sxs-lookup"><span data-stu-id="60202-182">The service is specified using the 'targetResourceId' parameter of `Add-AzureRmTrafficManagerEndpointConfig` or `New-AzureRmTrafficManagerEndpoint`.</span></span>
* <span data-ttu-id="60202-183">A "Target" és "EndpointLocation" targetresourceid azonosítója által azt jelentette.</span><span class="sxs-lookup"><span data-stu-id="60202-183">The 'Target' and 'EndpointLocation' are implied by the TargetResourceId.</span></span>
* <span data-ttu-id="60202-184">Adja meg a "súly" megadása nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="60202-184">Specifying the 'Weight' is optional.</span></span> <span data-ttu-id="60202-185">Súlyozás csak használják, ha a profil használatára van konfigurálva, a "Weighted" forgalom-útválasztási módszer használatát.</span><span class="sxs-lookup"><span data-stu-id="60202-185">Weights are only used if the profile is configured to use the 'Weighted' traffic-routing method.</span></span> <span data-ttu-id="60202-186">Ellenkező esetben nem veszi őket figyelembe.</span><span class="sxs-lookup"><span data-stu-id="60202-186">Otherwise, they are ignored.</span></span> <span data-ttu-id="60202-187">Ha meg van adva, az érték 1 és 1000 közötti számnak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="60202-187">If specified, the value must be a number between 1 and 1000.</span></span> <span data-ttu-id="60202-188">Az alapértelmezett érték: "1".</span><span class="sxs-lookup"><span data-stu-id="60202-188">The default value is '1'.</span></span>
* <span data-ttu-id="60202-189">Adja meg a "Priority" megadása nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="60202-189">Specifying the 'Priority' is optional.</span></span> <span data-ttu-id="60202-190">Prioritások csak akkor használatosak, ha a profil használatára van konfigurálva, a "Priority" forgalom-útválasztási módszer használatát.</span><span class="sxs-lookup"><span data-stu-id="60202-190">Priorities are only used if the profile is configured to use the 'Priority' traffic-routing method.</span></span> <span data-ttu-id="60202-191">Ellenkező esetben nem veszi őket figyelembe.</span><span class="sxs-lookup"><span data-stu-id="60202-191">Otherwise, they are ignored.</span></span> <span data-ttu-id="60202-192">Érvényes értékek az alacsonyabb magasabb prioritású jelző: 1-1000.</span><span class="sxs-lookup"><span data-stu-id="60202-192">Valid values are from 1 to 1000 with lower values indicating a higher priority.</span></span> <span data-ttu-id="60202-193">Ha meg van adva egy végpontot, azok összes végpontok adható meg.</span><span class="sxs-lookup"><span data-stu-id="60202-193">If specified for one endpoint, they must be specified for all endpoints.</span></span> <span data-ttu-id="60202-194">Ha nincs megadva, "1"-től kezdődő alapértelmezett értékeket, hogy a végpontok felsorolt sorrendben alkalmazza.</span><span class="sxs-lookup"><span data-stu-id="60202-194">If omitted, default values starting from '1' are applied in the order that the endpoints are listed.</span></span>

### <a name="example-1-adding-web-app-endpoints-using-add-azurermtrafficmanagerendpointconfig"></a><span data-ttu-id="60202-195">1. példa: A webalkalmazás végpontjaitól hozzáadása`Add-AzureRmTrafficManagerEndpointConfig`</span><span class="sxs-lookup"><span data-stu-id="60202-195">Example 1: Adding Web App endpoints using `Add-AzureRmTrafficManagerEndpointConfig`</span></span>

<span data-ttu-id="60202-196">Ebben a példában azt a Traffic Manager-profil létrehozása, és adja hozzá a két Web App végpontjaitól az `Add-AzureRmTrafficManagerEndpointConfig` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="60202-196">In this example, we create a Traffic Manager profile and add two Web App endpoints using the `Add-AzureRmTrafficManagerEndpointConfig` cmdlet.</span></span>

```powershell
$profile = New-AzureRmTrafficManagerProfile -Name myprofile -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName myapp -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
$webapp1 = Get-AzureRMWebApp -Name webapp1
Add-AzureRmTrafficManagerEndpointConfig -EndpointName webapp1ep -TrafficManagerProfile $profile -Type AzureEndpoints -TargetResourceId $webapp1.Id -EndpointStatus Enabled
$webapp2 = Get-AzureRMWebApp -Name webapp2
Add-AzureRmTrafficManagerEndpointConfig -EndpointName webapp2ep -TrafficManagerProfile $profile -Type AzureEndpoints -TargetResourceId $webapp2.Id -EndpointStatus Enabled
Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile
```
### <a name="example-2-adding-a-publicipaddress-endpoint-using-new-azurermtrafficmanagerendpoint"></a><span data-ttu-id="60202-197">2. példa: Hozzáadása egy nyilvános végpontot a`New-AzureRmTrafficManagerEndpoint`</span><span class="sxs-lookup"><span data-stu-id="60202-197">Example 2: Adding a publicIpAddress endpoint using `New-AzureRmTrafficManagerEndpoint`</span></span>

<span data-ttu-id="60202-198">Ebben a példában egy nyilvános IP-cím erőforrás a Traffic Manager-profilhoz van adva.</span><span class="sxs-lookup"><span data-stu-id="60202-198">In this example, a public IP address resource is added to the Traffic Manager profile.</span></span> <span data-ttu-id="60202-199">A nyilvános IP-cím rendelkeznie kell egy DNS-név, és a hálózati Adaptert egy virtuális gép vagy egy adott terheléselosztóhoz köthető.</span><span class="sxs-lookup"><span data-stu-id="60202-199">The public IP address must have a DNS name configured, and can be bound either to the NIC of a VM or to a load balancer.</span></span>

```powershell
$ip = Get-AzureRmPublicIpAddress -Name MyPublicIP -ResourceGroupName MyRG
New-AzureRmTrafficManagerEndpoint -Name MyIpEndpoint -ProfileName MyProfile -ResourceGroupName MyRG -Type AzureEndpoints -TargetResourceId $ip.Id -EndpointStatus Enabled
```

## <a name="adding-external-endpoints"></a><span data-ttu-id="60202-200">Külső végpontok hozzáadása</span><span class="sxs-lookup"><span data-stu-id="60202-200">Adding External Endpoints</span></span>

<span data-ttu-id="60202-201">A TRAFFIC Manager külső végpontok segítségével Azure-on kívüli tárolt szolgáltatások forgalmat irányítani.</span><span class="sxs-lookup"><span data-stu-id="60202-201">Traffic Manager uses external endpoints to direct traffic to services hosted outside of Azure.</span></span> <span data-ttu-id="60202-202">Mivel az Azure-végpontok, külső végpontok hozzáadása is lehetséges vagy használatával `Add-AzureRmTrafficManagerEndpointConfig` követ `Set-AzureRmTrafficManagerProfile`, vagy `New-AzureRMTrafficManagerEndpoint`.</span><span class="sxs-lookup"><span data-stu-id="60202-202">As with Azure endpoints, external endpoints can be added either using `Add-AzureRmTrafficManagerEndpointConfig` followed by `Set-AzureRmTrafficManagerProfile`, or `New-AzureRMTrafficManagerEndpoint`.</span></span>

<span data-ttu-id="60202-203">Külső végpontok száma megadásakor:</span><span class="sxs-lookup"><span data-stu-id="60202-203">When specifying external endpoints:</span></span>

* <span data-ttu-id="60202-204">A végpont tartomány nevét kötelező megadni a "Target" paraméter használatával</span><span class="sxs-lookup"><span data-stu-id="60202-204">The endpoint domain name must be specified using the 'Target' parameter</span></span>
* <span data-ttu-id="60202-205">Ha a "Teljesítmény" forgalom-útválasztási módszert használ, a "EndpointLocation" megadása kötelező.</span><span class="sxs-lookup"><span data-stu-id="60202-205">If the 'Performance' traffic-routing method is used, the 'EndpointLocation' is required.</span></span> <span data-ttu-id="60202-206">Ellenkező esetben nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="60202-206">Otherwise it is optional.</span></span> <span data-ttu-id="60202-207">Az értéknek kell lennie egy [érvényes Azure-régiót neve](https://azure.microsoft.com/regions/).</span><span class="sxs-lookup"><span data-stu-id="60202-207">The value must be a [valid Azure region name](https://azure.microsoft.com/regions/).</span></span>
* <span data-ttu-id="60202-208">A "Súly" és "Priority" megadása nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="60202-208">The 'Weight' and 'Priority' are optional.</span></span>

### <a name="example-1-adding-external-endpoints-using-add-azurermtrafficmanagerendpointconfig-and-set-azurermtrafficmanagerprofile"></a><span data-ttu-id="60202-209">1. példa: Használó külső végpontok hozzáadása `Add-AzureRmTrafficManagerEndpointConfig` és`Set-AzureRmTrafficManagerProfile`</span><span class="sxs-lookup"><span data-stu-id="60202-209">Example 1: Adding external endpoints using `Add-AzureRmTrafficManagerEndpointConfig` and `Set-AzureRmTrafficManagerProfile`</span></span>

<span data-ttu-id="60202-210">Ebben a példában azt Traffic Manager-profil létrehozása, adja hozzá a két külső végpont és a változtatások véglegesítése a határidő.</span><span class="sxs-lookup"><span data-stu-id="60202-210">In this example, we create a Traffic Manager profile, add two external endpoints, and commit the changes.</span></span>

```powershell
$profile = New-AzureRmTrafficManagerProfile -Name myprofile -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName myapp -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
Add-AzureRmTrafficManagerEndpointConfig -EndpointName eu-endpoint -TrafficManagerProfile $profile -Type ExternalEndpoints -Target app-eu.contoso.com -EndpointLocation "North Europe" -EndpointStatus Enabled
Add-AzureRmTrafficManagerEndpointConfig -EndpointName us-endpoint -TrafficManagerProfile $profile -Type ExternalEndpoints -Target app-us.contoso.com -EndpointLocation "Central US" -EndpointStatus Enabled
Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile
```

### <a name="example-2-adding-external-endpoints-using-new-azurermtrafficmanagerendpoint"></a><span data-ttu-id="60202-211">2. példa: Hozzáadása használata külső végpontok száma`New-AzureRmTrafficManagerEndpoint`</span><span class="sxs-lookup"><span data-stu-id="60202-211">Example 2: Adding external endpoints using `New-AzureRmTrafficManagerEndpoint`</span></span>

<span data-ttu-id="60202-212">Ebben a példában jelenleg egy olyan külső végpont a egy meglévő profilt kell felvenni.</span><span class="sxs-lookup"><span data-stu-id="60202-212">In this example, we add an external endpoint to an existing profile.</span></span> <span data-ttu-id="60202-213">A profil van megadva, a profil és az erőforrás nevének használatával.</span><span class="sxs-lookup"><span data-stu-id="60202-213">The profile is specified using the profile and resource group names.</span></span>

```powershell
New-AzureRmTrafficManagerEndpoint -Name eu-endpoint -ProfileName MyProfile -ResourceGroupName MyRG -Type ExternalEndpoints -Target app-eu.contoso.com -EndpointStatus Enabled
```

## <a name="adding-nested-endpoints"></a><span data-ttu-id="60202-214">"Nested" végpontok hozzáadása</span><span class="sxs-lookup"><span data-stu-id="60202-214">Adding 'Nested' endpoints</span></span>

<span data-ttu-id="60202-215">Minden egyes Traffic Manager-profil megadja egy forgalom-útválasztási módszert.</span><span class="sxs-lookup"><span data-stu-id="60202-215">Each Traffic Manager profile specifies a single traffic-routing method.</span></span> <span data-ttu-id="60202-216">Vannak azonban forgatókönyv esetén van szükség, kifinomultabb a forgalom útválasztásához, mint egyetlen Traffic Manager-profil által biztosított útválasztását.</span><span class="sxs-lookup"><span data-stu-id="60202-216">However, there are scenarios that require more sophisticated traffic routing than the routing provided by a single Traffic Manager profile.</span></span> <span data-ttu-id="60202-217">Traffic Manager-profilok egynél több forgalom-útválasztási módszer előnyei egyesítése ágyazhatja be.</span><span class="sxs-lookup"><span data-stu-id="60202-217">You can nest Traffic Manager profiles to combine the benefits of more than one traffic-routing method.</span></span> <span data-ttu-id="60202-218">Beágyazott profilok lehetővé teszik a támogatási nagyobb és összetettebb alkalmazások központi telepítésének alapértelmezett Traffic Manager működés felülbírálásához.</span><span class="sxs-lookup"><span data-stu-id="60202-218">Nested profiles allow you to override the default Traffic Manager behavior to support larger and more complex application deployments.</span></span> <span data-ttu-id="60202-219">További részletes példák: [beágyazott Traffic Manager-profilok](traffic-manager-nested-profiles.md).</span><span class="sxs-lookup"><span data-stu-id="60202-219">For more detailed examples, see [Nested Traffic Manager profiles](traffic-manager-nested-profiles.md).</span></span>

<span data-ttu-id="60202-220">Beágyazott végpontok úgy vannak konfigurálva, a szülő profilnál, egy adott típusú végpont, "NestedEndpoints" használatával.</span><span class="sxs-lookup"><span data-stu-id="60202-220">Nested endpoints are configured at the parent profile, using a specific endpoint type, 'NestedEndpoints'.</span></span> <span data-ttu-id="60202-221">Beágyazott végpontok megadásakor:</span><span class="sxs-lookup"><span data-stu-id="60202-221">When specifying nested endpoints:</span></span>

* <span data-ttu-id="60202-222">A végpont a "targetresourceid azonosítója" paraméter használatával kell megadni</span><span class="sxs-lookup"><span data-stu-id="60202-222">The endpoint must be specified using the 'targetResourceId' parameter</span></span>
* <span data-ttu-id="60202-223">Ha a "Teljesítmény" forgalom-útválasztási módszert használ, a "EndpointLocation" megadása kötelező.</span><span class="sxs-lookup"><span data-stu-id="60202-223">If the 'Performance' traffic-routing method is used, the 'EndpointLocation' is required.</span></span> <span data-ttu-id="60202-224">Ellenkező esetben nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="60202-224">Otherwise it is optional.</span></span> <span data-ttu-id="60202-225">Az értéknek kell lennie egy [érvényes Azure-régiót neve](http://azure.microsoft.com/regions/).</span><span class="sxs-lookup"><span data-stu-id="60202-225">The value must be a [valid Azure region name](http://azure.microsoft.com/regions/).</span></span>
* <span data-ttu-id="60202-226">A "Súly" és "Priority" megadása nem kötelező, mint az Azure-végpontok.</span><span class="sxs-lookup"><span data-stu-id="60202-226">The 'Weight' and 'Priority' are optional, as for Azure endpoints.</span></span>
* <span data-ttu-id="60202-227">A "MinChildEndpoints" paraméter nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="60202-227">The 'MinChildEndpoints' parameter is optional.</span></span> <span data-ttu-id="60202-228">Az alapértelmezett érték: "1".</span><span class="sxs-lookup"><span data-stu-id="60202-228">The default value is '1'.</span></span> <span data-ttu-id="60202-229">A rendelkezésre álló végpontok száma a küszöbérték alá csökken, ha a szülő profil úgy ítéli meg, a gyermek profil "csökkentett teljesítményű" és a szülő profil végpontja felé irányuló forgalom hozunk.</span><span class="sxs-lookup"><span data-stu-id="60202-229">If the number of available endpoints falls below this threshold, the parent profile considers the child profile 'degraded' and diverts traffic to the other endpoints in the parent profile.</span></span>

### <a name="example-1-adding-nested-endpoints-using-add-azurermtrafficmanagerendpointconfig-and-set-azurermtrafficmanagerprofile"></a><span data-ttu-id="60202-230">1. példa: Hozzáadása beágyazott végpontjaitól `Add-AzureRmTrafficManagerEndpointConfig` és`Set-AzureRmTrafficManagerProfile`</span><span class="sxs-lookup"><span data-stu-id="60202-230">Example 1: Adding nested endpoints using `Add-AzureRmTrafficManagerEndpointConfig` and `Set-AzureRmTrafficManagerProfile`</span></span>

<span data-ttu-id="60202-231">Ebben a példában azt új Traffic Manager gyermek és szülő profilok létrehozása, a gyermek hozzáadása egy beágyazott végpontként a szülő és a változtatások véglegesítése a határidő.</span><span class="sxs-lookup"><span data-stu-id="60202-231">In this example, we create new Traffic Manager child and parent profiles, add the child as a nested endpoint to the parent, and commit the changes.</span></span>

```powershell
$child = New-AzureRmTrafficManagerProfile -Name child -ResourceGroupName MyRG -TrafficRoutingMethod Priority -RelativeDnsName child -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
$parent = New-AzureRmTrafficManagerProfile -Name parent -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName parent -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
Add-AzureRmTrafficManagerEndpointConfig -EndpointName child-endpoint -TrafficManagerProfile $parent -Type NestedEndpoints -TargetResourceId $child.Id -EndpointStatus Enabled -EndpointLocation "North Europe" -MinChildEndpoints 2
Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile
```

<span data-ttu-id="60202-232">Ebben a példában kivonatosan mutatja azt adta hozzá, más végpontok a gyermek vagy szülő profilok.</span><span class="sxs-lookup"><span data-stu-id="60202-232">For brevity in this example, we did not add any other endpoints to the child or parent profiles.</span></span>

### <a name="example-2-adding-nested-endpoints-using-new-azurermtrafficmanagerendpoint"></a><span data-ttu-id="60202-233">2. példa: Használó beágyazott végpontok hozzáadása`New-AzureRmTrafficManagerEndpoint`</span><span class="sxs-lookup"><span data-stu-id="60202-233">Example 2: Adding nested endpoints using `New-AzureRmTrafficManagerEndpoint`</span></span>

<span data-ttu-id="60202-234">Ebben a példában azt egy meglévő gyermek profil hozzáadása egy beágyazott végpontként egy meglévő szülő profilt.</span><span class="sxs-lookup"><span data-stu-id="60202-234">In this example, we add an existing child profile as a nested endpoint to an existing parent profile.</span></span> <span data-ttu-id="60202-235">A profil van megadva, a profil és az erőforrás nevének használatával.</span><span class="sxs-lookup"><span data-stu-id="60202-235">The profile is specified using the profile and resource group names.</span></span>

```powershell
$child = Get-AzureRmTrafficManagerEndpoint -Name child -ResourceGroupName MyRG
New-AzureRmTrafficManagerEndpoint -Name child-endpoint -ProfileName parent -ResourceGroupName MyRG -Type NestedEndpoints -TargetResourceId $child.Id -EndpointStatus Enabled -EndpointLocation "North Europe" -MinChildEndpoints 2
```

## <a name="update-a-traffic-manager-endpoint"></a><span data-ttu-id="60202-236">A Traffic Manager-végpont frissítése</span><span class="sxs-lookup"><span data-stu-id="60202-236">Update a Traffic Manager Endpoint</span></span>

<span data-ttu-id="60202-237">Két módon lehet egy meglévő Traffic Manager-végpont frissítése:</span><span class="sxs-lookup"><span data-stu-id="60202-237">There are two ways to update an existing Traffic Manager endpoint:</span></span>

1. <span data-ttu-id="60202-238">Használatával a Traffic Manager-profil beolvasása `Get-AzureRmTrafficManagerProfile`, frissítse a profilon belül a végpont tulajdonságait, és véglegesítse a módosításokat `Set-AzureRmTrafficManagerProfile`.</span><span class="sxs-lookup"><span data-stu-id="60202-238">Get the Traffic Manager profile using `Get-AzureRmTrafficManagerProfile`, update the endpoint properties within the profile, and commit the changes using `Set-AzureRmTrafficManagerProfile`.</span></span> <span data-ttu-id="60202-239">Ez a módszer azzal az előnnyel jár, amely nem tudja frissíteni a egy művelettel több végpont.</span><span class="sxs-lookup"><span data-stu-id="60202-239">This method has the advantage of being able to update more than one endpoint in a single operation.</span></span>
2. <span data-ttu-id="60202-240">A Traffic Manager-végpontot a GET `Get-AzureRmTrafficManagerEndpoint`, frissítse a végpont tulajdonságait, és véglegesítse a módosításokat `Set-AzureRmTrafficManagerEndpoint`.</span><span class="sxs-lookup"><span data-stu-id="60202-240">Get the Traffic Manager endpoint using `Get-AzureRmTrafficManagerEndpoint`, update the endpoint properties, and commit the changes using `Set-AzureRmTrafficManagerEndpoint`.</span></span> <span data-ttu-id="60202-241">Ez a módszer is egyszerűbb, mivel a profil a végpontok tömbbe indexelő nincs szükség.</span><span class="sxs-lookup"><span data-stu-id="60202-241">This method is simpler, since it does not require indexing into the Endpoints array in the profile.</span></span>

### <a name="example-1-updating-endpoints-using-get-azurermtrafficmanagerprofile-and-set-azurermtrafficmanagerprofile"></a><span data-ttu-id="60202-242">1. példa: Frissítése végpontjaitól `Get-AzureRmTrafficManagerProfile` és`Set-AzureRmTrafficManagerProfile`</span><span class="sxs-lookup"><span data-stu-id="60202-242">Example 1: Updating endpoints using `Get-AzureRmTrafficManagerProfile` and `Set-AzureRmTrafficManagerProfile`</span></span>

<span data-ttu-id="60202-243">Ebben a példában két végpontot egy meglévő profilon belül a prioritásának módosítására azt.</span><span class="sxs-lookup"><span data-stu-id="60202-243">In this example, we modify the priority on two endpoints within an existing profile.</span></span>

```powershell
$profile = Get-AzureRmTrafficManagerProfile -Name myprofile -ResourceGroupName MyRG
$profile.Endpoints[0].Priority = 2
$profile.Endpoints[1].Priority = 1
Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile
```

### <a name="example-2-updating-an-endpoint-using-get-azurermtrafficmanagerendpoint-and-set-azurermtrafficmanagerendpoint"></a><span data-ttu-id="60202-244">2. példa: Frissítése egy végpontot a `Get-AzureRmTrafficManagerEndpoint` és`Set-AzureRmTrafficManagerEndpoint`</span><span class="sxs-lookup"><span data-stu-id="60202-244">Example 2: Updating an endpoint using `Get-AzureRmTrafficManagerEndpoint` and `Set-AzureRmTrafficManagerEndpoint`</span></span>

<span data-ttu-id="60202-245">Ebben a példában egy meglévő profil egyetlen végpont súlyának módosíthatja azt.</span><span class="sxs-lookup"><span data-stu-id="60202-245">In this example, we modify the weight of a single endpoint in an existing profile.</span></span>

```powershell
$endpoint = Get-AzureRmTrafficManagerEndpoint -Name myendpoint -ProfileName myprofile -ResourceGroupName MyRG -Type ExternalEndpoints
$endpoint.Weight = 20
Set-AzureRmTrafficManagerEndpoint -TrafficManagerEndpoint $endpoint
```

## <a name="enabling-and-disabling-endpoints-and-profiles"></a><span data-ttu-id="60202-246">Engedélyezése és letiltása a végpontok és profilok</span><span class="sxs-lookup"><span data-stu-id="60202-246">Enabling and Disabling Endpoints and Profiles</span></span>

<span data-ttu-id="60202-247">A TRAFFIC Manager lehetővé teszi, hogy engedélyezve van, és le van tiltva, valamint engedélyezése és letiltása teljes profilok lehetővé egyedi végpontok.</span><span class="sxs-lookup"><span data-stu-id="60202-247">Traffic Manager allows individual endpoints to be enabled and disabled, as well as allowing enabling and disabling of entire profiles.</span></span>
<span data-ttu-id="60202-248">Ezeket a módosításokat a végpont vagy profil erőforrások első/frissítése/beállítás módosítható.</span><span class="sxs-lookup"><span data-stu-id="60202-248">These changes can be made by getting/updating/setting the endpoint or profile resources.</span></span> <span data-ttu-id="60202-249">Ezek a gyakori műveletek egyszerűsítésére, támogatottak továbbá akkor dedikált parancsmagok segítségével.</span><span class="sxs-lookup"><span data-stu-id="60202-249">To streamline these common operations, they are also supported via dedicated cmdlets.</span></span>

### <a name="example-1-enabling-and-disabling-a-traffic-manager-profile"></a><span data-ttu-id="60202-250">1. példa: Engedélyezése és letiltása Traffic Manager-profil</span><span class="sxs-lookup"><span data-stu-id="60202-250">Example 1: Enabling and disabling a Traffic Manager profile</span></span>

<span data-ttu-id="60202-251">Engedélyezze a Traffic Manager-profil az `Enable-AzureRmTrafficManagerProfile`.</span><span class="sxs-lookup"><span data-stu-id="60202-251">To enable a Traffic Manager profile, use `Enable-AzureRmTrafficManagerProfile`.</span></span> <span data-ttu-id="60202-252">A profilt a profil objektum adható meg.</span><span class="sxs-lookup"><span data-stu-id="60202-252">The profile can be specified using a profile object.</span></span> <span data-ttu-id="60202-253">A profil objektum használatával vagy keresztül a feldolgozási sor adhatók át a "-TrafficManagerProfile" paraméter.</span><span class="sxs-lookup"><span data-stu-id="60202-253">The profile object can be passed via the pipeline or by using the '-TrafficManagerProfile' parameter.</span></span> <span data-ttu-id="60202-254">Ebben a példában azt adja meg a profilhoz a profil és az erőforrás neve.</span><span class="sxs-lookup"><span data-stu-id="60202-254">In this example, we specify the profile by the profile and resource group name.</span></span>

```powershell
Enable-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyResourceGroup
```

<span data-ttu-id="60202-255">Traffic Manager-profil letiltása:</span><span class="sxs-lookup"><span data-stu-id="60202-255">To disable a Traffic Manager profile:</span></span>

```powershell
Disable-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyResourceGroup
```

<span data-ttu-id="60202-256">A Disable-AzureRmTrafficManagerProfile parancsmag megerősítés kéri.</span><span class="sxs-lookup"><span data-stu-id="60202-256">The Disable-AzureRmTrafficManagerProfile cmdlet prompts for confirmation.</span></span> <span data-ttu-id="60202-257">Ez a kérdés is letiltja, használja a "-Force" paraméter.</span><span class="sxs-lookup"><span data-stu-id="60202-257">This prompt can be suppressed using the '-Force' parameter.</span></span>

### <a name="example-2-enabling-and-disabling-a-traffic-manager-endpoint"></a><span data-ttu-id="60202-258">2. példa: Engedélyezése és letiltása Traffic Manager-végpontot</span><span class="sxs-lookup"><span data-stu-id="60202-258">Example 2: Enabling and disabling a Traffic Manager endpoint</span></span>

<span data-ttu-id="60202-259">A Traffic Manager-végpont engedélyezéséhez az `Enable-AzureRmTrafficManagerEndpoint`.</span><span class="sxs-lookup"><span data-stu-id="60202-259">To enable a Traffic Manager endpoint, use `Enable-AzureRmTrafficManagerEndpoint`.</span></span> <span data-ttu-id="60202-260">Adja meg a végpont két módja van</span><span class="sxs-lookup"><span data-stu-id="60202-260">There are two ways to specify the endpoint</span></span>

1. <span data-ttu-id="60202-261">Egy keresztül a feldolgozási sor átadott TrafficManagerEndpoint objektum használatával vagy az a "-TrafficManagerEndpoint" paraméter</span><span class="sxs-lookup"><span data-stu-id="60202-261">Using a TrafficManagerEndpoint object passed via the pipeline or using the '-TrafficManagerEndpoint' parameter</span></span>
2. <span data-ttu-id="60202-262">A végpont neve, a típusú végpont, a profil neve és az erőforráscsoport-név használatával:</span><span class="sxs-lookup"><span data-stu-id="60202-262">Using the endpoint name, endpoint type, profile name, and resource group name:</span></span>

```powershell
Enable-AzureRmTrafficManagerEndpoint -Name MyEndpoint -Type AzureEndpoints -ProfileName MyProfile -ResourceGroupName MyRG
```

<span data-ttu-id="60202-263">Hasonlóképpen a Traffic Manager-végpont letiltása:</span><span class="sxs-lookup"><span data-stu-id="60202-263">Similarly, to disable a Traffic Manager endpoint:</span></span>

```powershell
Disable-AzureRmTrafficManagerEndpoint -Name MyEndpoint -Type AzureEndpoints -ProfileName MyProfile -ResourceGroupName MyRG -Force
```

<span data-ttu-id="60202-264">A `Disable-AzureRmTrafficManagerProfile`, a `Disable-AzureRmTrafficManagerEndpoint` parancsmag jóváhagyást kér.</span><span class="sxs-lookup"><span data-stu-id="60202-264">As with `Disable-AzureRmTrafficManagerProfile`, the `Disable-AzureRmTrafficManagerEndpoint` cmdlet prompts for confirmation.</span></span> <span data-ttu-id="60202-265">Ez a kérdés is letiltja, használja a "-Force" paraméter.</span><span class="sxs-lookup"><span data-stu-id="60202-265">This prompt can be suppressed using the '-Force' parameter.</span></span>

## <a name="delete-a-traffic-manager-endpoint"></a><span data-ttu-id="60202-266">A Traffic Manager-végpont törlése</span><span class="sxs-lookup"><span data-stu-id="60202-266">Delete a Traffic Manager Endpoint</span></span>

<span data-ttu-id="60202-267">Az egyes végpontok eltávolításához használja a `Remove-AzureRmTrafficManagerEndpoint` parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="60202-267">To remove individual endpoints, use the `Remove-AzureRmTrafficManagerEndpoint` cmdlet:</span></span>

```powershell
Remove-AzureRmTrafficManagerEndpoint -Name MyEndpoint -Type AzureEndpoints -ProfileName MyProfile -ResourceGroupName MyRG
```

<span data-ttu-id="60202-268">Ez a parancsmag felszólítja megerősítést kér.</span><span class="sxs-lookup"><span data-stu-id="60202-268">This cmdlet prompts for confirmation.</span></span> <span data-ttu-id="60202-269">Ez a kérdés is letiltja, használja a "-Force" paraméter.</span><span class="sxs-lookup"><span data-stu-id="60202-269">This prompt can be suppressed using the '-Force' parameter.</span></span>

## <a name="delete-a-traffic-manager-profile"></a><span data-ttu-id="60202-270">Traffic Manager-profil törlése</span><span class="sxs-lookup"><span data-stu-id="60202-270">Delete a Traffic Manager Profile</span></span>

<span data-ttu-id="60202-271">Traffic Manager-profil törléséhez használja a `Remove-AzureRmTrafficManagerProfile` parancsmag, a profil és az erőforrás nevének megadása:</span><span class="sxs-lookup"><span data-stu-id="60202-271">To delete a Traffic Manager profile, use the `Remove-AzureRmTrafficManagerProfile` cmdlet, specifying the profile and resource group names:</span></span>

```powershell
Remove-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG [-Force]
```

<span data-ttu-id="60202-272">Ez a parancsmag felszólítja megerősítést kér.</span><span class="sxs-lookup"><span data-stu-id="60202-272">This cmdlet prompts for confirmation.</span></span> <span data-ttu-id="60202-273">Ez a kérdés is letiltja, használja a "-Force" paraméter.</span><span class="sxs-lookup"><span data-stu-id="60202-273">This prompt can be suppressed using the '-Force' parameter.</span></span>

<span data-ttu-id="60202-274">Törli a profilt a profil objektum használatával is megadható:</span><span class="sxs-lookup"><span data-stu-id="60202-274">The profile to be deleted can also be specified using a profile object:</span></span>

```powershell
$profile = Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG
Remove-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile [-Force]
```

<span data-ttu-id="60202-275">Ebben a sorozatban is átirányítható:</span><span class="sxs-lookup"><span data-stu-id="60202-275">This sequence can also be piped:</span></span>

```powershell
Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG | Remove-AzureRmTrafficManagerProfile [-Force]
```

## <a name="next-steps"></a><span data-ttu-id="60202-276">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="60202-276">Next steps</span></span>

[<span data-ttu-id="60202-277">A TRAFFIC Manager figyelése</span><span class="sxs-lookup"><span data-stu-id="60202-277">Traffic Manager monitoring</span></span>](traffic-manager-monitoring.md)

[<span data-ttu-id="60202-278">A Traffic Manager teljesítményével kapcsolatos megfontolások</span><span class="sxs-lookup"><span data-stu-id="60202-278">Traffic Manager performance considerations</span></span>](traffic-manager-performance-considerations.md)
