---
title: az Azure RemoteApp PowerShell-parancsmagok aaaUse |} Microsoft Docs
description: Megtudhatja, hogyan toouse Windows PowerShell-parancsmagok az Azure Remoteappban.
services: remoteapp
documentationcenter: 
author: guscatalano
manager: mbaldwin
editor: 
ms.assetid: 7d3d5ded-6f73-4de6-a8ac-c1d622e842a2
ms.service: remoteapp
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: compute
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: a09cae2093e2c3a4a2ed673b5d148a22ceb935f9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-windows-powershell-cmdlets-with-azure-remoteapp"></a>Az Azure RemoteApp Windows PowerShell-parancsmagok használata
> [!IMPORTANT]
> Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik. Olvasási hello [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) részleteiről.
> 
> 

 Hello Azure RemoteApp PowerShell-parancsmagok tooadminister használja, és a gyűjtemények kezelése. A következő lépések információk tooget hello használata.

## <a name="get-hello-cmdlets"></a>Hello parancsmagok beszerzése
- - -
Először töltse le a hello Azure Powershell-parancsmagok [Itt](http://go.microsoft.com/?linkid=9811175), hello RemoteApp-parancsmagokat is szerepelnek benne. 

Tekintse meg a hello [Azure RemoteApp a parancsmag súgójában talál](/powershell/module/azure?view=azuresmps-3.7.0).

## <a name="configure-azure-cmdlets-toouse-your-subscription"></a>Azure-parancsmagokkal toouse az előfizetés konfigurálása
- - -
Hajtsa végre a [Ez az útmutató](/powershell/azure/overview) , szemben az Azure-előfizetéshez hello parancsmagokat használhatja.

Ezen lépések tooget gyorsan használhatja:

1. Töltse le és telepítse a hello [Azure PowerShell-parancsmagok](http://go.microsoft.com/?linkid=9811175).
2. Indítsa el a Microsoft Azure Powershellt.
3. Futtatás **Add-AzureAccount** tooauthenticate tooyour Azure-előfizetés. Amikor a rendszer kéri, adja meg a hello azonos felhasználónév és jelszó használható toosign tooAzure portálon.  
4. Futtatás **Get-AzureSubscription** toolist hello előfizetések a felhasználói fiókjához. 
5. Futtatás **válasszon-AzureSubscription - SubscriptionName &lt;előfizetés neve&gt;**  vagy **válasszon-AzureSubscription - előfizetés-azonosító &lt;előfizetés-azonosító&gt;**  toospecify hello előfizetés toouse.

Gratulálunk, az Azure PowerShell-konzolon konfigurált és készen állnak a toouse. Vegye figyelembe, hogy szüksége lesz toorepeate lépéseket 2 – 5 hello hello Azure PowerShell konzol minden egyes indításakor.  


## <a name="list-all-collections"></a>Minden gyűjtemény felsorolása
- - -
     Get-AzureRemoteAppCollection

## <a name="delete-a-collection"></a>A gyűjtemény törlése
- - -
    Remove-AzureRemoteAppCollection<enter collection name>

Példa: `Remove-AzureRemoteAppCollection ContosoProduction`.

## <a name="create-a-cloud-collection"></a>Felhőalapú gyűjtemény létrehozása
- - -
Használata egyszerű, futtassa a következő parancs hello:

    New-AzureRemoteAppCollection -Collectionname RAppO365Col1 -ImageName "Office 365 ProPlus (Subscription required)" -Plan Basic -Location "West US" - Description "Office 365 Collection."

hello fent parancs automatikusan közzéteszi a Microsoft Office 365-alkalmazások (Excel, a OneNote, Outlook, a PowerPoint, Visio és Word).

Webhelycsoport létrehozása eltarthat 30 perc vagy hosszabb toocomplete. Ezért ez a parancs visszaadja a követési azonosító, amely a következőképpen használhatja:

    Get-AzureRemoteAppOperationResult -TrackingId xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx

Hello gyűjtemény után a felhasználó toohello gyűjtemény hello a következő parancsot a is hozzáadhat:

    Add-AzureRemoteAppUser -CollectionName RAppO365Col1 -Type microsoftAccount -UserUpn someone@domain.com

És elkészült! A felhasználónak kell lennie képes tooconnect toohello alkalmazást hello Azure RemoteApp-ügyfélen található [Itt](https://www.remoteapp.windowsazure.com/).

## <a name="available-cmdlets"></a>Elérhető parancsmagok
Az egyéb parancsok, amelyek tudunk számos, hello dokumentációja őket hamarosan hamarosan:

Alapszintű RemoteApp-gyűjtemény parancsmagokat: 

* New-AzureRemoteAppCollection
* Get-AzureRemoteAppCollection
* Set-AzureRemoteAppCollection
* Update-AzureRemoteAppCollection
* Remove-AzureRemoteAppCollection
* Add-AzureRemoteAppUser
* Get-AzureRemoteAppUser
* Remove-AzureRemoteAppUser
* Get-AzureRemoteAppSession
* Disconnect-AzureRemoteAppSession
* Invoke-AzureRemoteAppSessionLogoff
* Send-AzureRemoteAppSessionMessage
* Get-AzureRemoteAppProgram
* Get-AzureRemoteAppStartMenuProgram
* Publish-AzureRemoteAppProgram
* Unpublish-AzureRemoteAppProgram
* Get-AzureRemoteAppCollectionUsageDetails
* Get-AzureRemoteAppCollectionUsageSummary
* Get-AzureRemoteAppPlan

RemoteApp virtuális hálózatot parancsmagokat:

* New-AzureRemoteAppVNet
* Get-AzureRemoteAppVNet
* Set-AzureRemoteAppVNet
* Remove-AzureRemoteAppVNet
* Get-AzureRemoteAppVpnDevice
* Get--AzureRemoteAppVpnDeviceConfigScript
* Reset-AzureRemoteAppVpnSharedKey

RemoteApp sablon-rendszerkép parancsmagokat:

* New-AzureRemoteAppTemplateImage
* Get-AzureRemoteAppTemplateImage
* Rename-AzureRemoteAppTemplateImage
* Remove-AzureRemoteAppTemplateImage

Más RemoteApp-parancsmagokat:

* Get-AzureRemoteAppLocation
* Get-AzureRemoteAppWorkspace
* Set-AzureRemoteAppWorkspace
* Get-AzureRemoteAppOperationResult

