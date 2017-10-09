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
# <a name="manage-azure-cdn-with-powershell"></a>A PowerShell segítségével az Azure CDN kezelése
PowerShell biztosít egy hello legrugalmasabb módszerek toomanage az Azure CDN-profil és a végpontok.  Használhatja a PowerShell, interaktív vagy parancsfájlok írásával tooautomate felügyeleti feladatok.  Ez az oktatóanyag azt mutatja be, több hello leggyakoribb feladatokat is elérheti a PowerShell toomanage az Azure CDN-profil és a végpontok.

## <a name="prerequisites"></a>Előfeltételek
toouse PowerShell toomanage az Azure CDN-profil és a végpontok hello Azure PowerShell modul telepítve kell rendelkeznie.  toolearn hogyan tooinstall Azure PowerShell, és csatlakozzon a hello segítségével tooAzure `Login-AzureRmAccount` parancsmag, lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).

> [!IMPORTANT]
> Jelentkezzen be az `Login-AzureRmAccount` Azure PowerShell-parancsmagok végrehajtás előtt.
> 
> 

## <a name="listing-hello-azure-cdn-cmdlets"></a>Hello Azure CDN parancsmagok listázása
Minden hello Azure CDN-parancsmaggal hello listázhatja `Get-Command` parancsmag.

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

## <a name="getting-help"></a>Segítségkérés
Segítséget kaphat, a parancsmagok használatával hello `Get-Help` parancsmag.  `Get-Help`használati és szintaxist, és opcionálisan a példákat mutat.

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

## <a name="listing-existing-azure-cdn-profiles"></a>Meglévő Azure CDN-profilra listázása
Hello `Get-AzureRmCdnProfile` paraméterek nélkül parancsmag beolvassa a meglévő CDN profilok.

```powershell
Get-AzureRmCdnProfile
```

A számbavétel védőeszközön toocmdlets lehet.

```powershell
# Output hello name of all profiles on this subscription.
Get-AzureRmCdnProfile | ForEach-Object { Write-Host $_.Name }

# Return only **Azure CDN from Verizon** profiles.
Get-AzureRmCdnProfile | Where-Object { $_.Sku.Name -eq "StandardVerizon" }
```

Egyetlen profilhoz hello profil nevét és az erőforrás csoport megadásával is visszaadják.

```powershell
Get-AzureRmCdnProfile -ProfileName CdnDemo -ResourceGroupName CdnDemoRG
```

> [!TIP]
> Több CDN hello ugyanaz a neve, a profilok, mindaddig, amíg a különböző erőforráscsoportokra lehetséges toohave.  Hello kihagyásával `ResourceGroupName` paraméter egyező nevű minden profilban adja vissza.
> 
> 

## <a name="listing-existing-cdn-endpoints"></a>Listaelem meglévő CDN-végpontok
`Get-AzureRmCdnEndpoint`lekérheti egyes végpont vagy a profilok hello végpontjai.  

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

## <a name="creating-cdn-profiles-and-endpoints"></a>CDN-profilok és a végpontok létrehozása
`New-AzureRmCdnProfile`és `New-AzureRmCdnEndpoint` használt toocreate CDN-profil és a végpontok.

```powershell
# Create a new profile
New-AzureRmCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -Sku StandardAkamai -Location "Central US"

# Create a new endpoint
New-AzureRmCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -Location "Central US" -EndpointName cdnposhdoc -OriginName "Contoso" -OriginHostName "www.contoso.com"

# Create a new profile and endpoint (same as above) in one line
New-AzureRmCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -Sku StandardAkamai -Location "Central US" | New-AzureRmCdnEndpoint -EndpointName cdnposhdoc -OriginName "Contoso" -OriginHostName "www.contoso.com"

```

## <a name="checking-endpoint-name-availability"></a>Végpont név foglaltságának ellenőrzése
`Get-AzureRmCdnEndpointNameAvailability`Visszaad egy objektumot, amely azt jelzi, ha egy végpont nevének érhető el.

```powershell
# Retrieve availability
$availability = Get-AzureRmCdnEndpointNameAvailability -EndpointName "cdnposhdoc"

# If available, write a message toohello console.
If($availability.NameAvailable) { Write-Host "Yes, that endpoint name is available." }
Else { Write-Host "No, that endpoint name is not available." }
```

## <a name="adding-a-custom-domain"></a>Egyéni tartomány hozzáadása
`New-AzureRmCdnCustomDomain`az egyéni tartomány nevét tooan meglévő végpont hozzáadása.

> [!IMPORTANT]
> Be kell állítania hello CNAME REKORDOT a DNS-szolgáltatónál leírtak [hogyan toomap egyéni tartomány tooContent Delivery Network (CDN) végpont](cdn-map-content-to-custom-domain.md).  Hello leképezés a végpontot a módosítása előtt tesztelheti `Test-AzureRmCdnCustomDomain`.
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

## <a name="modifying-an-endpoint"></a>A végpont módosítása
`Set-AzureRmCdnEndpoint`módosítja egy végpontot.

```powershell
# Get an existing endpoint
$endpoint = Get-AzureRmCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -EndpointName cdnposhdoc

# Set up content compression
$endpoint.IsCompressionEnabled = $true
$endpoint.ContentTypesToCompress = "text/javascript","text/css","application/json"

# Save hello changed endpoint and apply hello changes
Set-AzureRmCdnEndpoint -CdnEndpoint $endpoint
```

## <a name="purgingpre-loading-cdn-assets"></a>Kiürítése/előtti-loading CDN eszközök
`Unpublish-AzureRmCdnEndpointContent`pon gyorsítótárazott eszközök, amíg a `Publish-AzureRmCdnEndpointContent` előre betölti az eszközök támogatott végpontokon.

```powershell
# Purge some assets.
Unpublish-AzureRmCdnEndpointContent -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo -PurgeContent "/images/kitten.png","/video/rickroll.mp4"

# Pre-load some assets.
Publish-AzureRmCdnEndpointContent -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo -LoadContent "/images/kitten.png","/video/rickroll.mp4"

# Purge everything in /images/ on all endpoints.
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint | Unpublish-AzureRmCdnEndpointContent -PurgeContent "/images/*"
```

## <a name="startingstopping-cdn-endpoints"></a>Indítása/leállítása CDN-végpontok
`Start-AzureRmCdnEndpoint`és `Stop-AzureRmCdnEndpoint` használt toostart tehető, és állítsa le az egyes végpontok vagy csoportok végpontok.

```powershell
# Stop hello cdndocdemo endpoint
Stop-AzureRmCdnEndpoint -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo

# Stop all endpoints
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint | Stop-AzureRmCdnEndpoint

# Start all endpoints
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint | Start-AzureRmCdnEndpoint
```

## <a name="deleting-cdn-resources"></a>CDN-erőforrások törlése
`Remove-AzureRmCdnProfile`és `Remove-AzureRmCdnEndpoint` használt tooremove profilok vagy végpontok.

```powershell
# Remove a single endpoint
Remove-AzureRmCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -EndpointName cdnposhdoc

# Remove all hello endpoints on a profile and skip confirmation (-Force)
Get-AzureRmCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG | Get-AzureRmCdnEndpoint | Remove-AzureRmCdnEndpoint -Force

# Remove a single profile
Remove-AzureRmCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG
```

## <a name="next-steps"></a>Következő lépések
Megtudhatja, hogyan tooautomate Azure CDN rendelkező [.NET](cdn-app-dev-net.md) vagy [Node.js](cdn-app-dev-node.md).

toolearn CDN szolgáltatásai, lásd: [CDN áttekintésével](cdn-overview.md).

