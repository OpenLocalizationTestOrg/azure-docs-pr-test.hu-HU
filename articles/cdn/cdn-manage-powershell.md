---
title: "aaaManage Azure CDN szolgáltatás használata a PowerShell-lel |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse hello Azure PowerShell-parancsmagok toomanage Azure CDN szolgáltatás használata."
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
ms.openlocfilehash: 269373136d4ef018e4d31f147456b4be2253b463
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-cdn-with-powershell"></a><span data-ttu-id="b725a-103">A PowerShell segítségével az Azure CDN kezelése</span><span class="sxs-lookup"><span data-stu-id="b725a-103">Manage Azure CDN with PowerShell</span></span>
<span data-ttu-id="b725a-104">PowerShell biztosít egy hello legrugalmasabb módszerek toomanage az Azure CDN-profil és a végpontok.</span><span class="sxs-lookup"><span data-stu-id="b725a-104">PowerShell provides one of hello most flexible methods toomanage your Azure CDN profiles and endpoints.</span></span>  <span data-ttu-id="b725a-105">Használhatja a PowerShell, interaktív vagy parancsfájlok írásával tooautomate felügyeleti feladatok.</span><span class="sxs-lookup"><span data-stu-id="b725a-105">You can use PowerShell interactively or by writing scripts tooautomate management tasks.</span></span>  <span data-ttu-id="b725a-106">Ez az oktatóanyag azt mutatja be, több hello leggyakoribb feladatokat is elérheti a PowerShell toomanage az Azure CDN-profil és a végpontok.</span><span class="sxs-lookup"><span data-stu-id="b725a-106">This tutorial demonstrates several of hello most common tasks you can accomplish with PowerShell toomanage your Azure CDN profiles and endpoints.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b725a-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="b725a-107">Prerequisites</span></span>
<span data-ttu-id="b725a-108">toouse PowerShell toomanage az Azure CDN-profil és a végpontok hello Azure PowerShell modul telepítve kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="b725a-108">toouse PowerShell toomanage your Azure CDN profiles and endpoints, you must have hello Azure PowerShell module installed.</span></span>  <span data-ttu-id="b725a-109">toolearn hogyan tooinstall Azure PowerShell, és csatlakozzon a hello segítségével tooAzure `Login-AzureRmAccount` parancsmag, lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="b725a-109">toolearn how tooinstall Azure PowerShell and connect tooAzure using hello `Login-AzureRmAccount` cmdlet, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b725a-110">Jelentkezzen be az `Login-AzureRmAccount` Azure PowerShell-parancsmagok végrehajtás előtt.</span><span class="sxs-lookup"><span data-stu-id="b725a-110">You must log in with `Login-AzureRmAccount` before you can execute Azure PowerShell cmdlets.</span></span>
> 
> 

## <a name="listing-hello-azure-cdn-cmdlets"></a><span data-ttu-id="b725a-111">Hello Azure CDN parancsmagok listázása</span><span class="sxs-lookup"><span data-stu-id="b725a-111">Listing hello Azure CDN cmdlets</span></span>
<span data-ttu-id="b725a-112">Minden hello Azure CDN-parancsmaggal hello listázhatja `Get-Command` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="b725a-112">You can list all hello Azure CDN cmdlets using hello `Get-Command` cmdlet.</span></span>

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

## <a name="getting-help"></a><span data-ttu-id="b725a-113">Segítségkérés</span><span class="sxs-lookup"><span data-stu-id="b725a-113">Getting help</span></span>
<span data-ttu-id="b725a-114">Segítséget kaphat, a parancsmagok használatával hello `Get-Help` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="b725a-114">You can get help with any of these cmdlets using hello `Get-Help` cmdlet.</span></span>  <span data-ttu-id="b725a-115">`Get-Help`használati és szintaxist, és opcionálisan a példákat mutat.</span><span class="sxs-lookup"><span data-stu-id="b725a-115">`Get-Help` provides usage and syntax, and optionally shows examples.</span></span>

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
    toosee hello examples, type: "get-help Get-AzureRmCdnProfile -examples".
    For more information, type: "get-help Get-AzureRmCdnProfile -detailed".
    For technical information, type: "get-help Get-AzureRmCdnProfile -full".

```

## <a name="listing-existing-azure-cdn-profiles"></a><span data-ttu-id="b725a-116">Meglévő Azure CDN-profilra listázása</span><span class="sxs-lookup"><span data-stu-id="b725a-116">Listing existing Azure CDN profiles</span></span>
<span data-ttu-id="b725a-117">Hello `Get-AzureRmCdnProfile` paraméterek nélkül parancsmag beolvassa a meglévő CDN profilok.</span><span class="sxs-lookup"><span data-stu-id="b725a-117">hello `Get-AzureRmCdnProfile` cmdlet without any parameters retrieves all your existing CDN profiles.</span></span>

```powershell
Get-AzureRmCdnProfile
```

<span data-ttu-id="b725a-118">A számbavétel védőeszközön toocmdlets lehet.</span><span class="sxs-lookup"><span data-stu-id="b725a-118">This output can be piped toocmdlets for enumeration.</span></span>

```powershell
# Output hello name of all profiles on this subscription.
Get-AzureRmCdnProfile | ForEach-Object { Write-Host $_.Name }

# Return only **Azure CDN from Verizon** profiles.
Get-AzureRmCdnProfile | Where-Object { $_.Sku.Name -eq "StandardVerizon" }
```

<span data-ttu-id="b725a-119">Egyetlen profilhoz hello profil nevét és az erőforrás csoport megadásával is visszaadják.</span><span class="sxs-lookup"><span data-stu-id="b725a-119">You can also return a single profile by specifying hello profile name and resource group.</span></span>

```powershell
Get-AzureRmCdnProfile -ProfileName CdnDemo -ResourceGroupName CdnDemoRG
```

> [!TIP]
> <span data-ttu-id="b725a-120">Több CDN hello ugyanaz a neve, a profilok, mindaddig, amíg a különböző erőforráscsoportokra lehetséges toohave.</span><span class="sxs-lookup"><span data-stu-id="b725a-120">It is possible toohave multiple CDN profiles with hello same name, so long as they are in different resource groups.</span></span>  <span data-ttu-id="b725a-121">Hello kihagyásával `ResourceGroupName` paraméter egyező nevű minden profilban adja vissza.</span><span class="sxs-lookup"><span data-stu-id="b725a-121">Omitting hello `ResourceGroupName` parameter returns all profiles with a matching name.</span></span>
> 
> 

## <a name="listing-existing-cdn-endpoints"></a><span data-ttu-id="b725a-122">Listaelem meglévő CDN-végpontok</span><span class="sxs-lookup"><span data-stu-id="b725a-122">Listing existing CDN endpoints</span></span>
<span data-ttu-id="b725a-123">`Get-AzureRmCdnEndpoint`lekérheti egyes végpont vagy a profilok hello végpontjai.</span><span class="sxs-lookup"><span data-stu-id="b725a-123">`Get-AzureRmCdnEndpoint` can retrieve an individual endpoint or all hello endpoints on a profile.</span></span>  

```powershell
# Get a single endpoint.
Get-AzureRmCdnEndpoint -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo

# Get all of hello endpoints on a given profile. 
Get-AzureRmCdnEndpoint -ProfileName CdnDemo -ResourceGroupName CdnDemoRG

# Return all of hello endpoints on all of hello profiles.
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint

# Return all of hello endpoints in this subscription that are currently running.
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint | Where-Object { $_.ResourceState -eq "Running" }
```

## <a name="creating-cdn-profiles-and-endpoints"></a><span data-ttu-id="b725a-124">CDN-profilok és a végpontok létrehozása</span><span class="sxs-lookup"><span data-stu-id="b725a-124">Creating CDN profiles and endpoints</span></span>
<span data-ttu-id="b725a-125">`New-AzureRmCdnProfile`és `New-AzureRmCdnEndpoint` használt toocreate CDN-profil és a végpontok.</span><span class="sxs-lookup"><span data-stu-id="b725a-125">`New-AzureRmCdnProfile` and `New-AzureRmCdnEndpoint` are used toocreate CDN profiles and endpoints.</span></span>

```powershell
# Create a new profile
New-AzureRmCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -Sku StandardAkamai -Location "Central US"

# Create a new endpoint
New-AzureRmCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -Location "Central US" -EndpointName cdnposhdoc -OriginName "Contoso" -OriginHostName "www.contoso.com"

# Create a new profile and endpoint (same as above) in one line
New-AzureRmCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -Sku StandardAkamai -Location "Central US" | New-AzureRmCdnEndpoint -EndpointName cdnposhdoc -OriginName "Contoso" -OriginHostName "www.contoso.com"

```

## <a name="checking-endpoint-name-availability"></a><span data-ttu-id="b725a-126">Végpont név foglaltságának ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="b725a-126">Checking endpoint name availability</span></span>
<span data-ttu-id="b725a-127">`Get-AzureRmCdnEndpointNameAvailability`Visszaad egy objektumot, amely azt jelzi, ha egy végpont nevének érhető el.</span><span class="sxs-lookup"><span data-stu-id="b725a-127">`Get-AzureRmCdnEndpointNameAvailability` returns an object indicating if an endpoint name is available.</span></span>

```powershell
# Retrieve availability
$availability = Get-AzureRmCdnEndpointNameAvailability -EndpointName "cdnposhdoc"

# If available, write a message toohello console.
If($availability.NameAvailable) { Write-Host "Yes, that endpoint name is available." }
Else { Write-Host "No, that endpoint name is not available." }
```

## <a name="adding-a-custom-domain"></a><span data-ttu-id="b725a-128">Egyéni tartomány hozzáadása</span><span class="sxs-lookup"><span data-stu-id="b725a-128">Adding a custom domain</span></span>
<span data-ttu-id="b725a-129">`New-AzureRmCdnCustomDomain`az egyéni tartomány nevét tooan meglévő végpont hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="b725a-129">`New-AzureRmCdnCustomDomain` adds a custom domain name tooan existing endpoint.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b725a-130">Be kell állítania hello CNAME REKORDOT a DNS-szolgáltatónál leírtak [hogyan toomap egyéni tartomány tooContent Delivery Network (CDN) végpont](cdn-map-content-to-custom-domain.md).</span><span class="sxs-lookup"><span data-stu-id="b725a-130">You must set up hello CNAME with your DNS provider as described in [How toomap Custom Domain tooContent Delivery Network (CDN) endpoint](cdn-map-content-to-custom-domain.md).</span></span>  <span data-ttu-id="b725a-131">Hello leképezés a végpontot a módosítása előtt tesztelheti `Test-AzureRmCdnCustomDomain`.</span><span class="sxs-lookup"><span data-stu-id="b725a-131">You can test hello mapping before modifying your endpoint using `Test-AzureRmCdnCustomDomain`.</span></span>
> 
> 

```powershell
# Get an existing endpoint
$endpoint = Get-AzureRmCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -EndpointName cdnposhdoc

# Check hello mapping
$result = Test-AzureRmCdnCustomDomain -CdnEndpoint $endpoint -CustomDomainHostName "cdn.contoso.com"

# Create hello custom domain on hello endpoint
If($result.CustomDomainValidated){ New-AzureRmCdnCustomDomain -CustomDomainName Contoso -HostName "cdn.contoso.com" -CdnEndpoint $endpoint }
```

## <a name="modifying-an-endpoint"></a><span data-ttu-id="b725a-132">A végpont módosítása</span><span class="sxs-lookup"><span data-stu-id="b725a-132">Modifying an endpoint</span></span>
<span data-ttu-id="b725a-133">`Set-AzureRmCdnEndpoint`módosítja egy végpontot.</span><span class="sxs-lookup"><span data-stu-id="b725a-133">`Set-AzureRmCdnEndpoint` modifies an existing endpoint.</span></span>

```powershell
# Get an existing endpoint
$endpoint = Get-AzureRmCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -EndpointName cdnposhdoc

# Set up content compression
$endpoint.IsCompressionEnabled = $true
$endpoint.ContentTypesToCompress = "text/javascript","text/css","application/json"

# Save hello changed endpoint and apply hello changes
Set-AzureRmCdnEndpoint -CdnEndpoint $endpoint
```

## <a name="purgingpre-loading-cdn-assets"></a><span data-ttu-id="b725a-134">Kiürítése/előtti-loading CDN eszközök</span><span class="sxs-lookup"><span data-stu-id="b725a-134">Purging/Pre-loading CDN assets</span></span>
<span data-ttu-id="b725a-135">`Unpublish-AzureRmCdnEndpointContent`pon gyorsítótárazott eszközök, amíg a `Publish-AzureRmCdnEndpointContent` előre betölti az eszközök támogatott végpontokon.</span><span class="sxs-lookup"><span data-stu-id="b725a-135">`Unpublish-AzureRmCdnEndpointContent` purges cached assets, while `Publish-AzureRmCdnEndpointContent` pre-loads assets on supported endpoints.</span></span>

```powershell
# Purge some assets.
Unpublish-AzureRmCdnEndpointContent -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo -PurgeContent "/images/kitten.png","/video/rickroll.mp4"

# Pre-load some assets.
Publish-AzureRmCdnEndpointContent -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo -LoadContent "/images/kitten.png","/video/rickroll.mp4"

# Purge everything in /images/ on all endpoints.
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint | Unpublish-AzureRmCdnEndpointContent -PurgeContent "/images/*"
```

## <a name="startingstopping-cdn-endpoints"></a><span data-ttu-id="b725a-136">Indítása/leállítása CDN-végpontok</span><span class="sxs-lookup"><span data-stu-id="b725a-136">Starting/Stopping CDN endpoints</span></span>
<span data-ttu-id="b725a-137">`Start-AzureRmCdnEndpoint`és `Stop-AzureRmCdnEndpoint` használt toostart tehető, és állítsa le az egyes végpontok vagy csoportok végpontok.</span><span class="sxs-lookup"><span data-stu-id="b725a-137">`Start-AzureRmCdnEndpoint` and `Stop-AzureRmCdnEndpoint` can be used toostart and stop individual endpoints or groups of endpoints.</span></span>

```powershell
# Stop hello cdndocdemo endpoint
Stop-AzureRmCdnEndpoint -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo

# Stop all endpoints
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint | Stop-AzureRmCdnEndpoint

# Start all endpoints
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint | Start-AzureRmCdnEndpoint
```

## <a name="deleting-cdn-resources"></a><span data-ttu-id="b725a-138">CDN-erőforrások törlése</span><span class="sxs-lookup"><span data-stu-id="b725a-138">Deleting CDN resources</span></span>
<span data-ttu-id="b725a-139">`Remove-AzureRmCdnProfile`és `Remove-AzureRmCdnEndpoint` használt tooremove profilok vagy végpontok.</span><span class="sxs-lookup"><span data-stu-id="b725a-139">`Remove-AzureRmCdnProfile` and `Remove-AzureRmCdnEndpoint` can be used tooremove profiles and endpoints.</span></span>

```powershell
# Remove a single endpoint
Remove-AzureRmCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -EndpointName cdnposhdoc

# Remove all hello endpoints on a profile and skip confirmation (-Force)
Get-AzureRmCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG | Get-AzureRmCdnEndpoint | Remove-AzureRmCdnEndpoint -Force

# Remove a single profile
Remove-AzureRmCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG
```

## <a name="next-steps"></a><span data-ttu-id="b725a-140">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b725a-140">Next Steps</span></span>
<span data-ttu-id="b725a-141">Megtudhatja, hogyan tooautomate Azure CDN rendelkező [.NET](cdn-app-dev-net.md) vagy [Node.js](cdn-app-dev-node.md).</span><span class="sxs-lookup"><span data-stu-id="b725a-141">Learn how tooautomate Azure CDN with [.NET](cdn-app-dev-net.md) or [Node.js](cdn-app-dev-node.md).</span></span>

<span data-ttu-id="b725a-142">toolearn CDN szolgáltatásai, lásd: [CDN áttekintésével](cdn-overview.md).</span><span class="sxs-lookup"><span data-stu-id="b725a-142">toolearn about CDN features, see [CDN Overview](cdn-overview.md).</span></span>

