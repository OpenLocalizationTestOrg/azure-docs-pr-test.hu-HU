---
title: "Az Azure felhőalapú rendszerhéj (előzetes verzió) szolgáltatások PowerShell |} Microsoft Docs"
description: "Azure Cloud rendszerhéj PowerShell funkcióinak áttekintése"
services: Azure
documentationcenter: 
author: maertendMSFT
manager: timlt
tags: azure-resource-manager
ms.assetid: 
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 01/16/2018
ms.author: damaerte
ms.openlocfilehash: 1113589507934d814bc8924267c07c99f8864657
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 01/22/2018
---
# <a name="features--tools-for-powershell-in-azure-cloud-shell-preview"></a>Szolgáltatások és eszközök PowerShell Azure Cloud rendszerhéj (előzetes verzió)

[!include [features-introblock](../../includes/cloud-shell-features-introblock.md)]

> [!TIP]
> Szolgáltatások és az eszközök [Bash](features.md) is rendelkezésre áll.

PowerShell felhő rendszerhéj (előzetes verzió) futó `Windows Server 2016`.

## <a name="features"></a>Szolgáltatások

### <a name="secure-automatic-authentication"></a>Biztonságos automatikus hitelesítés

A felhő rendszerhéj (előzetes verzió) PowerShell biztonságosan és automatikusan hitelesíti felhasználóifiók-hozzáférés az Azure PowerShell.

### <a name="files-persistence-across-sessions"></a>Adatmegőrzési fájlok munkamenetei között

A munkamenetek között a fájlok továbbra is fennáll, felhőalapú rendszerhéj bemutatja, hogyan első indítsa el a Microsoft Azure fájlok megosztásra csatolni.
Ezt követően felhő rendszerhéj automatikusan elvégzi a tároló (mint csatlakoztatott `$home\clouddrive`) minden későbbi.
Mivel minden kérelmet a felhő rendszerhéj lefoglalni egy ideiglenes gép, fájlok kívül a `$home\clouddrive` és a gép állapota nem maradnak meg a munkamenetek között.

[További tudnivalók az Azure fájlmegosztások csatolása felhő rendszerhéj.](persisting-shell-storage-powershell.md)

### <a name="azure-drive-azure"></a>Az Azure meghajtó (Azure:)

A felhő rendszerhéj (előzetes verzió) PowerShell elindul Azure meghajtóban (`Azure:`).
Az Azure meghajtó lehetővé teszi, hogy könnyen felderítés és az Azure-erőforrások, például a számítási, hálózati, tárolási stb filesystem navigációs hasonló navigációs.
Továbbra is a megszokott [Azure PowerShell-parancsmagok](https://docs.microsoft.com/powershell/azure) kezelheti ezeket az erőforrásokat.
Az Azure-erőforrások, vagy közvetlenül az Azure portálon vagy az Azure PowerShell-parancsmagokkal végzett módosítások azonnal megjelennek az Azure meghajtó.

![](media/features-powershell/azure-drive.png)

#### <a name="contextual-awareness"></a>Környezetfüggő figyelése

- **Erőforrás gazdagépcsoport hatókörének beállítása**: Ha egy erőforrás-csoport elérési útja az Azure meghajtó kontextusában (`Azure:`), az erőforráscsoport neve automatikusan átkerül az Azure PowerShell-parancsmagokat.

    ![](media/features-powershell/resource-group-autocomplete.png)

- **Get-AzureRmCommand**: Ez a parancsmag listáját adja vissza a parancsok a környezetében az Azure meghajtó helyére vonatkozó (`Azure:`). Például azt illusztrálja, csak a tárolással kapcsolatos parancsok a felhasználó esetén`Azure:\<subscription name>\StorageAccounts`

    ![](media/features-powershell/get-azurermcommand.png)

### <a name="rich-powershell-script-editing"></a>Gazdag PowerShell parancsfájl szerkesztése

PowerShell fájlok szerkesztésére VIM használatos (`.ps1,.psm1,.psd1`), automatikusan kapott szintaxis kiemelését és IntelliSense támogatás.
IntelliSense támogatási vim-beépülő modul, amely egy helyi példányával együttműködik keresztül van megvalósítva [PowerShell szerkesztő szolgáltatások](https://github.com/PowerShell/PowerShellEditorServices).

> [!TIP]
> Használjon `TAB` és befejezési (IntelliSense) kérjen a parancs nevét, a paraméterek nevei és a paraméterértékek, (adott esetben).

![](media/features-powershell/powershell-editing-vim.png)

### <a name="extensible-model"></a>Bővíthető modell

Használatával [PowerShellGet](https://docs.microsoft.com/powershell/module/powershellget), egyszerűen telepítse (és frissítése) az egyéni modulok és a parancsfájlok a [PowerShell-galériában](https://www.powershellgallery.com).
A telepítés után a modulok automatikusan megmaradnak felhő rendszerhéj-munkamenetek között.

> [!TIP]
> Felhasználók által telepített modulok lesznek mentve a `$Home\CloudDrive\.pscloudshell\WindowsPowerShell` mappát. Ez a mappa a szimbolikus hivatkozást a felhasználó Dokumentumok mappában jön létre (`$home\Documents\WindowsPowerShell`).

![](media/features-powershell/powershellget-module.png)

### <a name="management-of-guest-vms"></a>A Vendég virtuális gépek kezelése

Két beépített parancsok - használatával `Enter-AzureRmVM` és `Invoke-AzureRmVMCommand`, kezelhető távolról, az Azure virtuális gépeken.
Ezek a parancsok PowerShell távoli eljáráshívás épülnek, és az Azure virtuális gépek PowerShell kapcsolatot igényelnek.

## <a name="tools"></a>Eszközök

|**Kategória**    |**Name (Név)**                                 |
|----------------|-----------------------------------------|
|Azure-eszközök     |[Azure PowerShell (5.1.1)](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-5.1.1)<br> [Azure CLI (2.0.25)](https://docs.microsoft.com/cli/azure/overview)|
|A szerkesztő szövege    |VIM<br> nano                             |
|A Package Manager |PowerShellGet<br> PackageManagement<br> npm<br> a pip |
|A verziókövetési rendszerrel  |git                                      |
|Adatbázisok       |[SQL Server-modul](https://www.powershellgallery.com/packages/SqlServer)<br> [Az Sqlcmd segédprogram használatával](https://docs.microsoft.com/sql/tools/sqlcmd-utility)      |
|Az eszközök tesztelése      |Pester                                   |

## <a name="language-support"></a>Nyelvi támogatás

|**Nyelv**|**Verzió**|
|------------|-----------|
|.NET        |4.6        |
|Node.js     |6.10       |
|PowerShell  |5.1 és [6.0 (béta)](https://github.com/PowerShell/powershell/releases)       |
|Python      |2.7        |

## <a name="next-steps"></a>További lépések

[Gyors üzembe helyezése a PowerShell használatával a felhő rendszerhéj (előzetes verzió)](quickstart-powershell.md)

[További tudnivalók az Azure PowerShell](https://docs.microsoft.com/powershell/azure/)
