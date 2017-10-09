---
title: "Állapotkonfiguráció Azure áttekintés aaaDesired |} Microsoft Docs"
description: "Hello Microsoft Azure-kiterjesztés kezelője segítségével a PowerShell célállapot-konfiguráció áttekintése. Többek között az Előfeltételek, architektúra, parancsmagok..."
services: virtual-machines-windows
documentationcenter: 
author: zjalexander
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
keywords: 
ms.assetid: bbacbc93-1e7b-4611-a3ec-e3320641f9ba
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: na
ms.date: 01/09/2017
ms.author: zachal
ms.openlocfilehash: b0337a2f1124f35e5e40c1478bd7530427e59d44
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-toohello-azure-desired-state-configuration-extension-handler"></a>Bevezetés toohello Azure célállapot-konfiguráció bővítmény kezelő
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

hello Azure virtuális gép ügynökének és kapcsolódó bővítmények a Microsoft Azure infrastruktúra-szolgáltatásokat hello részét képezik. Virtuálisgép-bővítmények olyan szoftverösszetevők, amelyek hello VM bővíthetők, és egyszerűbbé tehető a virtuális gép különböző felügyeleti műveleteket. Például hello VMAccess bővítmény használt tooreset a rendszergazda jelszavát, és egyéni parancsfájl hello kiterjesztése használt tooexecute egy parancsfájlt a virtuális gép hello lehet.

Ez a cikk hello PowerShell kívánt állapot konfigurációs szolgáltatása (DSC) bővítmény be Azure virtuális gépek hello Azure PowerShell SDK részeként. Új parancsmagok tooupload használja, és a PowerShell DSC-konfiguráció alkalmazása egy Azure virtuális gépen engedélyezve van a PowerShell DSC-bővítményt hello. hello PowerShell DSC bővítmény hívások a PowerShell DSC tooenact hello DSC-konfiguráció a virtuális gép hello kapott. Ez a funkció hello Azure-portálon keresztül is érhető el.

## <a name="prerequisites"></a>Előfeltételek
**Helyi számítógép** toointeract a hello Azure Virtuálisgép-bővítmény, Azure PowerShell SDK hello vagy szükséges toouse vagy hello Azure-portálon. 

**A Vendégügynök** hello Azure virtuális gép hello DSC-konfiguráció által konfigurált toobe egy operációs rendszer, amely támogatja a Windows Management Framework (WMF) 4.0-s vagy 5.0 kell. hello támogatott operációsrendszer-verziók teljes listája megtalálható hello [DSC-bővítmény verzióelőzmények](https://blogs.msdn.microsoft.com/powershell/2014/11/20/release-history-for-the-azure-dsc-extension/).

## <a name="terms-and-concepts"></a>Kifejezések és fogalmak
Ez az útmutató a következő fogalmak hello ismeretét feltételezi:

Konfiguráció - DSC konfigurációs dokumentum. 

Csomópont - céljának DSC-konfiguráció. Ebben a dokumentumban a "csomópont" mindig tooan Azure virtuális gép hivatkozik.

Konfigurációs adatok - egy .psd1 fájlt a környezeti adatokat tartalmazó konfiguráció

## <a name="architectural-overview"></a>Az architektúra áttekintése
hello Azure DSC-bővítményt használ hello Azure Virtuálisgép-ügynök keretrendszer toodeliver, kihirdeti, és a jelentés az Azure virtuális gépeken futó DSC-konfigurációk. hello DSC-bővítményt vár legalább egy konfigurációs dokumentumot tartalmazó .zip fájlt, és a paraméterek megadott hello Azure PowerShell SDK vagy keresztül hello Azure-portálon.

Meghívásakor hello bővítmény hello az első alkalommal fut a telepítési művelet. Ez a folyamat a következő logika hello segítségével hello Windows Management Framework (WMF) verzióját telepíti:

1. Ha hello Azure virtuális gép operációs rendszere Windows Server 2016-os, semmilyen művelet nem lesz végrehajtva. Windows Server 2016 már telepített PowerShell hello legújabb verzióját.
2. Ha hello `wmfVersion` tulajdonság van megadva, a hello WMF verziója van telepítve, kivéve, ha hello virtuális gép operációs rendszere kompatibilis.
3. Ha nincs `wmfVersion` tulajdonság van megadva, a hello legújabb megfelelő verziója hello WMF telepítve van.

Hello WMF telepítése újraindítást igényel. Az újraindítást követően hello bővítmény letölti a hello hello .zip fájl `modulesUrl` tulajdonság. Ha ezen a helyen az Azure blob Storage tárolóban, SAS-jogkivonat adható hello `sasToken` tulajdonság tooaccess hello fájlt. Hello .zip letöltése és kicsomagolása, után hello definiált konfigurációs függvény `configurationFunction` toogenerate hello MOF-fájl futtatása. hello bővítmény majd futtatja az `Start-DscConfiguration -Force` hello jön létre a MOF-fájlt. hello bővítmény kimeneti rögzíti, és vissza kimenő toohello Azure állapot csatorna írja azt. Ettől kezdve, hello DSC LCM kezeli a figyelés és a szokásos módon javítása. 

## <a name="powershell-cmdlets"></a>PowerShell-parancsmagok
PowerShell-parancsmagok használhatók az Azure Resource Manager vagy a klasszikus telepítési modell toopackage hello, közzétételét és DSC-bővítmény telepítésének figyelése. hello szerepel a következő parancsmagok olyan hello klasszikus üzembe helyezési modulok, de "Azure" "AzureRm" toouse hello Azure Resource Manager modellt lehet cserélni. Például `Publish-AzureVMDscConfiguration` használ hello klasszikus üzembe helyezési modellel, ahol `Publish-AzureRmVMDscConfiguration` Azure Resource Managert használja. 

`Publish-AzureVMDscConfiguration`időt vesz igénybe, a konfigurációs fájlban, keres a DSC tőle függő erőforrások, és létrehoz egy hello mind DSC erőforrások szükséges tooenact hello konfigurációs tartalmazó .zip fájlt. Azt is létrehozhat hello csomagot helyben hello `-ConfigurationArchivePath` paraméter. Ellenkező esetben hello .zip fájl tooAzure blob-tároló tesz közzé, és biztosítja a az SAS-jogkivonatot.

Ez a parancsmag által létrehozott hello .zip fájl hello .ps1 konfigurációs parancsfájl hello archív mappa gyökeréhez hello rendelkezik. Erőforrások van hello modul mappára hello archív mappába helyezi. 

`Set-AzureVMDscExtension`hello beállításokat, amelyek használatával hello PowerShell DSC-bővítményt a Virtuálisgép-konfigurációs objektum esetében. Hello klasszikus üzembe helyezési modellel, hello VM módosítások alkalmazott tooan Azure virtuális Gépen kell lennie a `Update-AzureVM`. 

`Get-AzureVMDscExtension`Lekéri egy adott virtuális gép hello DSC-bővítmény állapotának. 

`Get-AzureVMDscExtensionStatus`lekéri a hello állapotának hello DSC-konfiguráció hello DSC-bővítmény kezelő helyeztek. Ez a művelet végrehajtható egyetlen virtuális gép vagy csoportján.

`Remove-AzureVMDscExtension`hello-kiterjesztés kezelőjének eltávolítása a megadott virtuális géppel. Ez a parancsmag does **nem** hello konfiguráció eltávolítása, távolítsa el a hello WMF vagy hello virtuális gépen hello alkalmazott beállítások módosítása. Csak hello bővítmény kezelő távolítja el. 

**A címterület-kezelés és az Azure Resource Manager parancsmagok főbb változásai**

* Az Azure Resource Manager parancsmagjainak szinkronizáltak. Címterület-kezelési parancsmagok aszinkron jellegűek.
* Erőforráscsoport-név, a VMName, a ArchiveStorageAccountName, a verziót és a hely összes kötelező paraméterek az Azure Resource Manager tartoznak.
* ArchiveResourceGroupName egy új Azure Resource Manager nem kötelező paraméter. Ez a paraméter is megadhat, ha a tárfiók tartozik tooa másik erőforráscsoportban található, mint egy hello ahol hello a virtuális gép létrehozása.
* ConfigurationArchive nevezik ArchiveBlobName az Azure Resource Manager
* ContainerName nevezik ArchiveContainerName az Azure Resource Manager
* StorageEndpointSuffix nevezik ArchiveStorageEndpointSuffix az Azure Resource Manager
* hello AutoUpdate kapcsoló tooAzure erőforrás-kezelő tooenable automatikus frissítés hello kiterjesztés kezelő toohello legújabb verziója, és a rendelkezésre álló bővült. Vegye figyelembe ennek a paraméternek hello lehetséges toocause újraindul a virtuális gép, amikor egy új verziója WMF felszabadul hello hello. 

## <a name="azure-portal-functionality"></a>Az Azure portál funkció
Keresse meg a virtuális gép tooa. A beállítások -> Általános kattintson "Kiterjesztésekkel." Egy új ablak jön létre. Kattintson a "Hozzáadás" gombra, és válassza ki a PowerShell DSC.

hello portál van szüksége.
**Modulok vagy parancsfájl**: A mező kitöltése kötelező. Egy konfigurációs parancsfájl tartalmazó .ps1 fájlt vagy egy .ps1 konfigurációs parancsfájl hello gyökerében és a tőle függő összes erőforrásról, a modul mappákban belül hello .zip .zip fájl szükséges. A hello is létrehozható `Publish-AzureVMDscConfiguration -ConfigurationArchivePath` parancsmag hello Azure PowerShell SDK tartalmazza. hello .zip fájl van töltve a SAS-jogkivonat által védett felhasználói blob-tároló. 

**Konfigurációs adatok psd1 kiterjesztésű fájl**: Ez a mező nem kötelező megadni. Ha a konfiguráció szükséges .psd1 a konfigurációs adatfájlt, használja az ebben a mezőben tooselect azt, és töltse fel tooyour felhasználói blob-tároló, SAS-jogkivonat által védett ahol. 

**Konfiguráció neve modulhoz minősített**: .ps1 fájlok rendelkezhet több konfigurációs funkciók. Hello konfigurációs .ps1 parancsfájl követ hello nevét adja meg a "\' és hello hello konfigurációs függvény neve. Például ha a .ps1 parancsfájl hello neve "configuration.ps1", és hello konfigurációs "IisInstall", írja be:`configuration.ps1\IisInstall`

**Konfigurációs argumentumok**: Ha hello konfigurációs függvény argumentummal, adja meg őket itt hello formátumban `argumentName1=value1,argumentName2=value2`. Megjegyzés: ezt a formátumot más formátumú, mint hogyan konfigurációs argumentumot fogad PowerShell-parancsmagok és a Resource Manager-sablonok használatával. 

## <a name="getting-started"></a>Bevezetés
hello Azure DSC-bővítményt vesz igénybe a DSC-konfiguráció dokumentumok és ír elő őket az Azure virtuális gépeken. A következő konfiguráció egy egyszerű példa. Mentse helyileg "IisInstall.ps1":

```powershell
configuration IISInstall 
{ 
    node "localhost"
    { 
        WindowsFeature IIS 
        { 
            Ensure = "Present" 
            Name = "Web-Server"                       
        } 
    } 
}
```

hello követő lépéseket hely hello IisInstall.ps1 parancsfájl hello a megadott virtuális gép, hello konfigurációs hajtható végre, és jelentéseket küldhetnek vissza állapota.
###<a name="classic-model"></a>Klasszikus modell
```powershell
#Azure PowerShell cmdlets are required
Import-Module Azure

#Use an existing Azure Virtual Machine, 'DscDemo1'
$demoVM = Get-AzureVM DscDemo1

#Publish hello configuration script into user storage.
Publish-AzureVMDscConfiguration -ConfigurationPath ".\IisInstall.ps1" -StorageContext $storageContext -Verbose -Force

#Set hello VM toorun hello DSC configuration
Set-AzureVMDscExtension -VM $demoVM -ConfigurationArchive "IisInstall.ps1.zip" -StorageContext $storageContext -ConfigurationName "IisInstall" -Verbose

#Update hello configuration of an Azure Virtual Machine
$demoVM | Update-AzureVM -Verbose

#check on status
Get-AzureVMDscExtensionStatus -VM $demovm -Verbose
```
###<a name="azure-resource-manager-model"></a>Az Azure Resource Manager modellt

```powershell
$resourceGroup = "dscVmDemo"
$location = "westus"
$vmName = "myVM"
$storageName = "demostorage"
#Publish hello configuration script into user storage
Publish-AzureRmVMDscConfiguration -ConfigurationPath .\iisInstall.ps1 -ResourceGroupName $resourceGroup -StorageAccountName $storageName -force
#Set hello VM toorun hello DSC configuration
Set-AzureRmVmDscExtension -Version 2.21 -ResourceGroupName $resourceGroup -VMName $vmName -ArchiveStorageAccountName $storageName -ArchiveBlobName iisInstall.ps1.zip -AutoUpdate:$true -ConfigurationName "IISInstall"

```

## <a name="logging"></a>Naplózás
Naplók kerülnek:

C:\WindowsAzure\Logs\Plugins\Microsoft.PowerShell.DSC\[verziószám]

## <a name="next-steps"></a>Következő lépések
További információ a PowerShell DSC [látogasson el a hello PowerShell dokumentációs központban](https://msdn.microsoft.com/powershell/dsc/overview). 

Vizsgálja meg a hello [Azure Resource Manager sablon hello DSC-bővítmény](extensions-dsc-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

a PowerShell DSC kezelhető toofind további funkciók [keresse meg a PowerShell-galériában hello](https://www.powershellgallery.com/packages?q=DscResource&x=0&y=0) további DSC-erőforrásokat.

Bizalmas paraméterek átadása a konfigurációk a részletekért lásd: [biztonságosan hello DSC-bővítmény kezelő a hitelesítő adatok kezelése](extensions-dsc-credentials.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

