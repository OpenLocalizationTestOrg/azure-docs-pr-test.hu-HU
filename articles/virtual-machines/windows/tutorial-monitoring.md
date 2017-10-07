---
title: "aaaAzure figyelés és a Windows virtuális gépek |} Microsoft Docs"
description: "Az oktatóanyag - figyelő egy Windows rendszerű virtuális gép az Azure PowerShell használatával"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/04/2017
ms.author: davidmu
ms.custom: mvc
ms.openlocfilehash: 191dc5a30d41c25a9e38f8ec2a32efdc05e03015
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-a-windows-virtual-machine-with-azure-powershell"></a>Az Azure PowerShell használatával Windows virtuális gépek figyelése

Azure figyelési ügynökök toocollect rendszerindító és Azure virtuális gépeken, a teljesítményadatokat az Azure storage tárolja ezeket az adatokat, és tegye elérhetővé a portálon, a hello Azure PowerShell modul és a hello Azure CLI segítségével az. Eben az oktatóanyagban az alábbiakkal fog megismerkedni:

> [!div class="checklist"]
> * A virtuális gép rendszerindítási diagnosztika engedélyezése
> * Rendszerindítási diagnosztika megtekintése
> * Virtuális gép gazdagép-metrikák megtekintése
> * Hello diagnosztika-kiterjesztés telepítése
> * Nézet VM metrikák
> * Riasztás létrehozása
> * Speciális figyelés beállítása

Ez az oktatóanyag hello Azure PowerShell 3,6 vagy újabb verziója szükséges. Futtatás ` Get-Module -ListAvailable AzureRM` toofind hello verziója. Ha tooupgrade van szüksége, tekintse meg [telepítése Azure PowerShell modul](/powershell/azure/install-azurerm-ps).

Ebben az oktatóanyagban toocomplete hello példában rendelkeznie kell egy meglévő virtuális gépet. Ha szükséges, ez [parancsfájl minta](../scripts/virtual-machines-windows-powershell-sample-create-vm.md) hozhat létre egyet. Ha hello oktatóanyag dolgozik, cserélje le a hello erőforráscsoportot, a virtuális gép nevét és a helyet, ha szükséges.

## <a name="view-boot-diagnostics"></a>Rendszerindítási diagnosztika megtekintése

Indítsa el a Windows virtuális gépek, mivel hello rendszerindítási diagnosztikai ügynök rögzíti a képernyő kimeneti hibaelhárítási céllal használható. Ez a funkció alapértelmezés szerint engedélyezve van. hello rögzítése a képernyő pillanatfelvételek tárolódnak az Azure storage-fiók, amely alapértelmezés szerint is létrejön. 

Hello rendszerindítási diagnosztikai adatok hello kaphat [Get-AzureRmVMBootDiagnosticsData](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmbootdiagnosticsdata) parancsot. A következő példa hello, rendszerindítási diagnosztika hello letöltött toohello gyökerébe * c:\* meghajtó. 

```powershell
Get-AzureRmVMBootDiagnosticsData -ResourceGroupName myResourceGroup -Name myVM -Windows -LocalPath "c:\"
```

## <a name="view-host-metrics"></a>Gazdagép-metrikák megtekintése

Egy Windows virtuális gép a gazdagép dedikált virtuális gépek rendelkezik, amely hatással van az Azure-ban. Metrikák automatikusan gyűjtendő hello állomás, és megtekinthetők az hello Azure-portálon.

1. Hello Azure-portálon, kattintson **erőforráscsoportok**, jelölje be **myResourceGroup**, majd válassza ki **myVM** hello erőforrás listában.
2. Kattintson a **metrikák** a hello VM panelen, majd válassza ki bármelyik hello gazdagép metrikák alapján **elérhető** toosee hogyan hello gazdagép VM hajt végre.

    ![Gazdagép-metrikák megtekintése](./media/tutorial-monitoring/tutorial-monitor-host-metrics.png)

## <a name="install-diagnostics-extension"></a>Diagnosztika-kiterjesztés telepítése

hello alapvető gazdagép metrikák érhetők el, de a részletesebb toosee, és a Virtuálisgép-specifikus metrika, akkor tooneed tooinstall hello Azure diagnostics bővítményt a virtuális gép hello. hello Azure diagnostics bővítmény lehetővé teszi, hogy további felügyeleti és diagnosztikai adatok toobe hello VM lekért. A metrikák megtekintheti, és hozzon létre a riasztások alapján hogyan hello VM hajt végre. hello diagnosztikai telepítve van a bővítmény hello Azure-portálon keresztül az alábbiak szerint:

1. Hello Azure-portálon, kattintson **erőforráscsoportok**, jelölje be **myResourceGroup**, majd válassza ki **myVM** hello erőforrás listában.
2. Kattintson a **diagnosztikai beállítások**. hello lista azt mutatja, hogy *rendszerindítási diagnosztika* már engedélyezve van az előző szakasz hello. Kattintson a megfelelő jelölőnégyzet hello *alapvető metrikák*.
3. Kattintson a hello **vendégszintű a figyelés bekapcsolható** gombra.

    ![Nézet diagnosztikai metrikák](./media/tutorial-monitoring/enable-diagnostics-extension.png)

## <a name="view-vm-metrics"></a>Nézet VM metrikák

Hello VM metrikák hello megtekintheti, hogy megtekinthetők-e hello gazdagép VM metrikák ugyanúgy:

1. Hello Azure-portálon, kattintson **erőforráscsoportok**, jelölje be **myResourceGroup**, majd válassza ki **myVM** hello erőforrás listában.
2. toosee hogyan hello VM hajt végre, kattintson a **metrikák** a hello VM panelen, majd válassza ki bármelyik hello diagnosztika mérőszámok alapján **elérhető**.

    ![Nézet VM metrikák](./media/tutorial-monitoring/monitor-vm-metrics.png)

## <a name="create-alerts"></a>Riasztások létrehozása

Riasztások adott mérőszámok alapján hozhat létre. Riasztás például akkor, amikor az átlagos CPU-használat meghaladja az egy bizonyos küszöb vagy a rendelkezésre álló szabad lemezterületet egy adott értékre, alá süllyed használt toonotify. Riasztások jelennek meg hello Azure-portálon, vagy e-mailben küldhetők el. A válasz tooalerts generálása Azure Automation-forgatókönyveket vagy Azure Logic Apps is kiválthatják.

hello alábbi példa riasztást hoz létre az átlagos CPU-használat.

1. Hello Azure-portálon, kattintson **erőforráscsoportok**, jelölje be **myResourceGroup**, majd válassza ki **myVM** hello erőforrás listában.
2. Kattintson a **riasztási szabályok** hello VM panelen, majd kattintson a **metrika riasztás hozzáadása** keresztül hello hello riasztások panel tetején.
4. Adjon meg egy **neve** a riasztás például *myAlertRule*
5. tootrigger riasztást, amikor a processzor 1.0 meghaladja 5 percig, hagyja összes hello többi kijelölt alapértelmezett értéket.
6. Szükség esetén jelölje be a hello jelölőnégyzetet *E-mail-tulajdonosok, közreműködőknek és olvasóknak* toosend e-mailben értesítést. hello alapértelmezett művelete toopresent hello portálon értesítést.
7. Kattintson a hello **OK** gombra.

## <a name="advanced-monitoring"></a>Speciális figyelés 

Fejlettebb, figyelés, a virtuális gép segítségével teheti [Operations Management Suite](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview). Ha még nem tette meg, akkor regisztrálhatnak az egy [ingyenes próbaverzió](https://www.microsoft.com/en-us/cloud-platform/operations-management-suite-trial) az Operations Management Suite szolgáltatásban.

Ha a hozzáférési toohello OMS-portálon, hello munkaterület kulcs és a munkaterület-azonosítót a hello-beállítások panelen található. Használjon hello [Set-AzureRmVMExtension](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmextension) cmmand tootooadd hello OMS bővítmény toohello virtuális gép. Frissítés hello változó értékekkel az alábbi minta tooreflect hello OMS-munkaterület kulcs és a munkaterület azonosítóját.  

```powershell
$omsId = "<Replace with your OMS Id>"
$omsKey = "<Replace with your OMS key>"

Set-AzureRmVMExtension -ResourceGroupName myResourceGroup `
  -ExtensionName "Microsoft.EnterpriseCloud.Monitoring" `
  -VMName myVM `
  -Publisher "Microsoft.EnterpriseCloud.Monitoring" `
  -ExtensionType "MicrosoftMonitoringAgent" `
  -TypeHandlerVersion 1.0 `
  -Settings @{"workspaceId" = $omsId} `
  -ProtectedSettings @{"workspaceKey" = $omsKey} `
  -Location eastus
```

Néhány perc múlva megtekintheti az új virtuális gép hello OMS-munkaterület hello. 

![OMS panel](./media/tutorial-monitoring/tutorial-monitor-oms.png)

## <a name="next-steps"></a>Következő lépések
Ebben az oktatóanyagban konfigurálva, és tekintse át a virtuális gépek az Azure Security Center. Megismerte, hogyan végezheti el az alábbi műveleteket:

> [!div class="checklist"]
> * Virtuális hálózat létrehozása
> * Egy erőforráscsoport és a virtuális gép létrehozása 
> * Rendszerindítási diagnosztika a virtuális gép hello engedélyezése
> * Rendszerindítási diagnosztika megtekintése
> * Gazdagép-metrikák megtekintése
> * Hello diagnosztika-kiterjesztés telepítése
> * Nézet VM metrikák
> * Riasztás létrehozása
> * Speciális figyelés beállítása

Az Azure security Centerrel kapcsolatos útmutató toolearn következő toohello előzetes.

> [!div class="nextstepaction"]
> [A virtuális gépek biztonságának kezelése](./tutorial-azure-security.md)