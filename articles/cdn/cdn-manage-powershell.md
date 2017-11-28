---
title: "A PowerShell segítségével az Azure CDN kezelése |} Microsoft Docs"
description: "Útmutató: Azure CDN kezelése az Azure PowerShell-parancsmagok segítségével."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: fb6f57a5-6e26-4847-8fd9-b51fb05a79eb
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 5bd2eed7b34cafa43e8f38279890405d4ae55568
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="manage-azure-cdn-with-powershell"></a><span data-ttu-id="1d171-103">A PowerShell segítségével az Azure CDN kezelése</span><span class="sxs-lookup"><span data-stu-id="1d171-103">Manage Azure CDN with PowerShell</span></span>
<span data-ttu-id="1d171-104">PowerShell biztosítja a leginkább rugalmas módszer kezeli az Azure CDN-profil és a végpontok.</span><span class="sxs-lookup"><span data-stu-id="1d171-104">PowerShell provides one of the most flexible methods to manage your Azure CDN profiles and endpoints.</span></span>  <span data-ttu-id="1d171-105">Használhatja PowerShell interaktív módon vagy parancsfájlok írásával felügyeleti feladatok automatizálására.</span><span class="sxs-lookup"><span data-stu-id="1d171-105">You can use PowerShell interactively or by writing scripts to automate management tasks.</span></span>  <span data-ttu-id="1d171-106">Ez az oktatóanyag azt mutatja be, több leggyakoribb feladatokat is elérheti a PowerShell használatával kezeli az Azure CDN-profil és a végpontok.</span><span class="sxs-lookup"><span data-stu-id="1d171-106">This tutorial demonstrates several of the most common tasks you can accomplish with PowerShell to manage your Azure CDN profiles and endpoints.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1d171-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="1d171-107">Prerequisites</span></span>
<span data-ttu-id="1d171-108">Az Azure CDN-profil és a végpontok kezelése a PowerShell használatával, az Azure PowerShell modul telepítve kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="1d171-108">To use PowerShell to manage your Azure CDN profiles and endpoints, you must have the Azure PowerShell module installed.</span></span>  <span data-ttu-id="1d171-109">Azure PowerShell telepítése és csatlakozás az Azure használatával további információt a `Login-AzureRmAccount` parancsmag, lásd: [telepítése és konfigurálása az Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="1d171-109">To learn how to install Azure PowerShell and connect to Azure using the `Login-AzureRmAccount` cmdlet, see [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1d171-110">Jelentkezzen be az `Login-AzureRmAccount` Azure PowerShell-parancsmagok végrehajtás előtt.</span><span class="sxs-lookup"><span data-stu-id="1d171-110">You must log in with `Login-AzureRmAccount` before you can execute Azure PowerShell cmdlets.</span></span>
> 
> 

## <a name="listing-the-azure-cdn-cmdlets"></a><span data-ttu-id="1d171-111">Az Azure CDN parancsmagok listázása</span><span class="sxs-lookup"><span data-stu-id="1d171-111">Listing the Azure CDN cmdlets</span></span>
<span data-ttu-id="1d171-112">Az Azure CDN-parancsmaggal is listázhatja az összes a `Get-Command` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="1d171-112">You can list all the Azure CDN cmdlets using the `Get-Command` cmdlet.</span></span>

```text
PS C:\> Get-Command -Module AzureRM.Cdn

CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Cmdlet          Get-AzureRmCdnCustomDomain                         2.0.0      AzureRm.Cdn
Cmdlet          Get-AzureRmCdnEndpoint                             2.0.0      AzureRm.Cdn
Cmdlet          Get-AzureRmCdnEndpointNameAvailability             2.0.0      AzureRm.Cdn
Cmdlet          Get-AzureRmCdnOrigin                               2.0.0      AzureRm.Cdn
Cmdlet          Get-AzureRmCdnProfile                              2.0.0      AzureRm.Cdn
Cmdlet          Get-AzureRmCdnProfileSsoUrl                        2.0.0      AzureRm.Cdn
Cmdlet          New-AzureRmCdnCustomDomain                         2.0.0      AzureRm.Cdn
Cmdlet          New-AzureRmCdnEndpoint                             2.0.0      AzureRm.Cdn
Cmdlet          New-AzureRmCdnProfile                              2.0.0      AzureRm.Cdn
Cmdlet          Publish-AzureRmCdnEndpointContent                  2.0.0      AzureRm.Cdn
Cmdlet          Remove-AzureRmCdnCustomDomain                      2.0.0      AzureRm.Cdn
Cmdlet          Remove-AzureRmCdnEndpoint                          2.0.0      AzureRm.Cdn
Cmdlet          Remove-AzureRmCdnProfile                           2.0.0      AzureRm.Cdn
Cmdlet          Set-AzureRmCdnEndpoint                             2.0.0      AzureRm.Cdn
Cmdlet          Set-AzureRmCdnOrigin                               2.0.0      AzureRm.Cdn
Cmdlet          Set-AzureRmCdnProfile                              2.0.0      AzureRm.Cdn
Cmdlet          Start-AzureRmCdnEndpoint                           2.0.0      AzureRm.Cdn
Cmdlet          Stop-AzureRmCdnEndpoint                            2.0.0      AzureRm.Cdn
Cmdlet          Test-AzureRmCdnCustomDomain                        2.0.0      AzureRm.Cdn
Cmdlet          Unpublish-AzureRmCdnEndpointContent                2.0.0      AzureRm.Cdn
```

## <a name="getting-help"></a><span data-ttu-id="1d171-113">Segítségkérés</span><span class="sxs-lookup"><span data-stu-id="1d171-113">Getting help</span></span>
<span data-ttu-id="1d171-114">Segítséget kaphat a parancsmagok használatával, a `Get-Help` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="1d171-114">You can get help with any of these cmdlets using the `Get-Help` cmdlet.</span></span>  <span data-ttu-id="1d171-115">`Get-Help`használati és szintaxist, és opcionálisan a példákat mutat.</span><span class="sxs-lookup"><span data-stu-id="1d171-115">`Get-Help` provides usage and syntax, and optionally shows examples.</span></span>

```text
PS C:\> Get-Help Get-AzureRmCdnProfile

NAME
    Get-AzureRmCdnProfile

SYNOPSIS
    Gets an Azure CDN profile.


SYNTAX
    Get-AzureRmCdnProfile [-ProfileName <String>] [-ResourceGroupName <String>] [-InformationAction
    <ActionPreference>] [-InformationVariable <String>] [<CommonParameters>]


DESCRIPTION
    Gets an Azure CDN profile and all related information.


RELATED LINKS

REMARKS
    To see the examples, type: "get-help Get-AzureRmCdnProfile -examples".
    For more information, type: "get-help Get-AzureRmCdnProfile -detailed".
    For technical information, type: "get-help Get-AzureRmCdnProfile -full".

```

## <a name="listing-existing-azure-cdn-profiles"></a><span data-ttu-id="1d171-116">Meglévő Azure CDN-profilra listázása</span><span class="sxs-lookup"><span data-stu-id="1d171-116">Listing existing Azure CDN profiles</span></span>
<span data-ttu-id="1d171-117">A `Get-AzureRmCdnProfile` paraméterek nélkül parancsmag beolvassa a meglévő CDN profilok.</span><span class="sxs-lookup"><span data-stu-id="1d171-117">The `Get-AzureRmCdnProfile` cmdlet without any parameters retrieves all your existing CDN profiles.</span></span>

```powershell
Get-AzureRmCdnProfile
```

<span data-ttu-id="1d171-118">A kimeneti számbavétel parancsmagjainak átirányítható.</span><span class="sxs-lookup"><span data-stu-id="1d171-118">This output can be piped to cmdlets for enumeration.</span></span>

```powershell
# Output the name of all profiles on this subscription.
Get-AzureRmCdnProfile | ForEach-Object { Write-Host $_.Name }

# Return only **Azure CDN from Verizon** profiles.
Get-AzureRmCdnProfile | Where-Object { $_.Sku.Name -eq "StandardVerizon" }
```

<span data-ttu-id="1d171-119">Egyetlen profilhoz a profil nevét és az erőforrás csoport megadásával is visszaadják.</span><span class="sxs-lookup"><span data-stu-id="1d171-119">You can also return a single profile by specifying the profile name and resource group.</span></span>

```powershell
Get-AzureRmCdnProfile -ProfileName CdnDemo -ResourceGroupName CdnDemoRG
```

> [!TIP]
> <span data-ttu-id="1d171-120">Akkor lehetséges, hogy ugyanazzal a névvel, több CDN-profilra, feltéve, hogy a különböző erőforráscsoportokra vannak.</span><span class="sxs-lookup"><span data-stu-id="1d171-120">It is possible to have multiple CDN profiles with the same name, so long as they are in different resource groups.</span></span>  <span data-ttu-id="1d171-121">Kihagyása a `ResourceGroupName` paraméter egyező nevű minden profilban adja vissza.</span><span class="sxs-lookup"><span data-stu-id="1d171-121">Omitting the `ResourceGroupName` parameter returns all profiles with a matching name.</span></span>
> 
> 

## <a name="listing-existing-cdn-endpoints"></a><span data-ttu-id="1d171-122">Listaelem meglévő CDN-végpontok</span><span class="sxs-lookup"><span data-stu-id="1d171-122">Listing existing CDN endpoints</span></span>
<span data-ttu-id="1d171-123">`Get-AzureRmCdnEndpoint`lekérheti az egyes endpoint vagy a profilok a végpontok.</span><span class="sxs-lookup"><span data-stu-id="1d171-123">`Get-AzureRmCdnEndpoint` can retrieve an individual endpoint or all the endpoints on a profile.</span></span>  

```powershell
# Get a single endpoint.
Get-AzureRmCdnEndpoint -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo

# Get all of the endpoints on a given profile. 
Get-AzureRmCdnEndpoint -ProfileName CdnDemo -ResourceGroupName CdnDemoRG

# Return all of the endpoints on all of the profiles.
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint

# Return all of the endpoints in this subscription that are currently running.
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint | Where-Object { $_.ResourceState -eq "Running" }
```

## <a name="creating-cdn-profiles-and-endpoints"></a><span data-ttu-id="1d171-124">CDN-profilok és a végpontok létrehozása</span><span class="sxs-lookup"><span data-stu-id="1d171-124">Creating CDN profiles and endpoints</span></span>
<span data-ttu-id="1d171-125">`New-AzureRmCdnProfile`és `New-AzureRmCdnEndpoint` hozhatók létre a CDN-profil és a végpontok.</span><span class="sxs-lookup"><span data-stu-id="1d171-125">`New-AzureRmCdnProfile` and `New-AzureRmCdnEndpoint` are used to create CDN profiles and endpoints.</span></span>

```powershell
# Create a new profile
New-AzureRmCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -Sku StandardAkamai -Location "Central US"

# Create a new endpoint
New-AzureRmCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -Location "Central US" -EndpointName cdnposhdoc -OriginName "Contoso" -OriginHostName "www.contoso.com"

# Create a new profile and endpoint (same as above) in one line
New-AzureRmCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -Sku StandardAkamai -Location "Central US" | New-AzureRmCdnEndpoint -EndpointName cdnposhdoc -OriginName "Contoso" -OriginHostName "www.contoso.com"

```

## <a name="checking-endpoint-name-availability"></a><span data-ttu-id="1d171-126">Végpont név foglaltságának ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="1d171-126">Checking endpoint name availability</span></span>
<span data-ttu-id="1d171-127">`Get-AzureRmCdnEndpointNameAvailability`Visszaad egy objektumot, amely azt jelzi, ha egy végpont nevének érhető el.</span><span class="sxs-lookup"><span data-stu-id="1d171-127">`Get-AzureRmCdnEndpointNameAvailability` returns an object indicating if an endpoint name is available.</span></span>

```powershell
# Retrieve availability
$availability = Get-AzureRmCdnEndpointNameAvailability -EndpointName "cdnposhdoc"

# If available, write a message to the console.
If($availability.NameAvailable) { Write-Host "Yes, that endpoint name is available." }
Else { Write-Host "No, that endpoint name is not available." }
```

## <a name="adding-a-custom-domain"></a><span data-ttu-id="1d171-128">Egyéni tartomány hozzáadása</span><span class="sxs-lookup"><span data-stu-id="1d171-128">Adding a custom domain</span></span>
<span data-ttu-id="1d171-129">`New-AzureRmCdnCustomDomain`egyéni tartománynév hozzáadása egy meglévő végpontot.</span><span class="sxs-lookup"><span data-stu-id="1d171-129">`New-AzureRmCdnCustomDomain` adds a custom domain name to an existing endpoint.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1d171-130">Be kell állítania a CNAME REKORDOT a DNS-szolgáltatónál leírtak [hogyan egyéni tartomány leképezése a Content Delivery Network (CDN) végpont](cdn-map-content-to-custom-domain.md).</span><span class="sxs-lookup"><span data-stu-id="1d171-130">You must set up the CNAME with your DNS provider as described in [How to map Custom Domain to Content Delivery Network (CDN) endpoint](cdn-map-content-to-custom-domain.md).</span></span>  <span data-ttu-id="1d171-131">A végpontot a módosítása előtt olvassa el a leképezés tesztelheti `Test-AzureRmCdnCustomDomain`.</span><span class="sxs-lookup"><span data-stu-id="1d171-131">You can test the mapping before modifying your endpoint using `Test-AzureRmCdnCustomDomain`.</span></span>
> 
> 

```powershell
# Get an existing endpoint
$endpoint = Get-AzureRmCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -EndpointName cdnposhdoc

# Check the mapping
$result = Test-AzureRmCdnCustomDomain -CdnEndpoint $endpoint -CustomDomainHostName "cdn.contoso.com"

# Create the custom domain on the endpoint
If($result.CustomDomainValidated){ New-AzureRmCdnCustomDomain -CustomDomainName Contoso -HostName "cdn.contoso.com" -CdnEndpoint $endpoint }
```

## <a name="modifying-an-endpoint"></a><span data-ttu-id="1d171-132">A végpont módosítása</span><span class="sxs-lookup"><span data-stu-id="1d171-132">Modifying an endpoint</span></span>
<span data-ttu-id="1d171-133">`Set-AzureRmCdnEndpoint`módosítja egy végpontot.</span><span class="sxs-lookup"><span data-stu-id="1d171-133">`Set-AzureRmCdnEndpoint` modifies an existing endpoint.</span></span>

```powershell
# Get an existing endpoint
$endpoint = Get-AzureRmCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -EndpointName cdnposhdoc

# Set up content compression
$endpoint.IsCompressionEnabled = $true
$endpoint.ContentTypesToCompress = "text/javascript","text/css","application/json"

# Save the changed endpoint and apply the changes
Set-AzureRmCdnEndpoint -CdnEndpoint $endpoint
```

## <a name="purgingpre-loading-cdn-assets"></a><span data-ttu-id="1d171-134">Kiürítése/előtti-loading CDN eszközök</span><span class="sxs-lookup"><span data-stu-id="1d171-134">Purging/Pre-loading CDN assets</span></span>
<span data-ttu-id="1d171-135">`Unpublish-AzureRmCdnEndpointContent`pon gyorsítótárazott eszközök, amíg a `Publish-AzureRmCdnEndpointContent` előre betölti az eszközök támogatott végpontokon.</span><span class="sxs-lookup"><span data-stu-id="1d171-135">`Unpublish-AzureRmCdnEndpointContent` purges cached assets, while `Publish-AzureRmCdnEndpointContent` pre-loads assets on supported endpoints.</span></span>

```powershell
# Purge some assets.
Unpublish-AzureRmCdnEndpointContent -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo -PurgeContent "/images/kitten.png","/video/rickroll.mp4"

# Pre-load some assets.
Publish-AzureRmCdnEndpointContent -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo -LoadContent "/images/kitten.png","/video/rickroll.mp4"

# Purge everything in /images/ on all endpoints.
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint | Unpublish-AzureRmCdnEndpointContent -PurgeContent "/images/*"
```

## <a name="startingstopping-cdn-endpoints"></a><span data-ttu-id="1d171-136">Indítása/leállítása CDN-végpontok</span><span class="sxs-lookup"><span data-stu-id="1d171-136">Starting/Stopping CDN endpoints</span></span>
<span data-ttu-id="1d171-137">`Start-AzureRmCdnEndpoint`és `Stop-AzureRmCdnEndpoint` indítása és leállítása az egyes végpontok vagy csoportok végpontok is használható.</span><span class="sxs-lookup"><span data-stu-id="1d171-137">`Start-AzureRmCdnEndpoint` and `Stop-AzureRmCdnEndpoint` can be used to start and stop individual endpoints or groups of endpoints.</span></span>

```powershell
# Stop the cdndocdemo endpoint
Stop-AzureRmCdnEndpoint -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo

# Stop all endpoints
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint | Stop-AzureRmCdnEndpoint

# Start all endpoints
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint | Start-AzureRmCdnEndpoint
```

## <a name="deleting-cdn-resources"></a><span data-ttu-id="1d171-138">CDN-erőforrások törlése</span><span class="sxs-lookup"><span data-stu-id="1d171-138">Deleting CDN resources</span></span>
<span data-ttu-id="1d171-139">`Remove-AzureRmCdnProfile`és `Remove-AzureRmCdnEndpoint` segítségével távolítsa el a profilok és a végpontok.</span><span class="sxs-lookup"><span data-stu-id="1d171-139">`Remove-AzureRmCdnProfile` and `Remove-AzureRmCdnEndpoint` can be used to remove profiles and endpoints.</span></span>

```powershell
# Remove a single endpoint
Remove-AzureRmCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -EndpointName cdnposhdoc

# Remove all the endpoints on a profile and skip confirmation (-Force)
Get-AzureRmCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG | Get-AzureRmCdnEndpoint | Remove-AzureRmCdnEndpoint -Force

# Remove a single profile
Remove-AzureRmCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG
```

## <a name="next-steps"></a><span data-ttu-id="1d171-140">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1d171-140">Next Steps</span></span>
<span data-ttu-id="1d171-141">Ismerje meg, hogyan automatizálhatja az Azure CDN-t a [.NET](cdn-app-dev-net.md) vagy a [Node.js](cdn-app-dev-node.md) segítségével.</span><span class="sxs-lookup"><span data-stu-id="1d171-141">Learn how to automate Azure CDN with [.NET](cdn-app-dev-net.md) or [Node.js](cdn-app-dev-node.md).</span></span>

<span data-ttu-id="1d171-142">CDN-funkciókkal kapcsolatos további tudnivalókért lásd: [CDN áttekintésével](cdn-overview.md).</span><span class="sxs-lookup"><span data-stu-id="1d171-142">To learn about CDN features, see [CDN Overview](cdn-overview.md).</span></span>

