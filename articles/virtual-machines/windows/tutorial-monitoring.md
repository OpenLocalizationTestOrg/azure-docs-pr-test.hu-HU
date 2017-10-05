---
title: "Az Azure figyelése és a Windows virtuális gépek |} Microsoft Docs"
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
ms.openlocfilehash: 42a475092bdd615c23154e5f813861b0acefcf29
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="monitor-a-windows-virtual-machine-with-azure-powershell"></a>Az Azure PowerShell használatával Windows virtuális gépek figyelése

Ügynökök Azure figyelés használ a rendszerindító és teljesítményadatokat gyűjteni Azure virtuális gépeken, ezek az adatok tárolása az Azure storage, és lehetővé teszi a portál, az Azure PowerShell modul és az Azure parancssori felület keresztül érhető el. Eben az oktatóanyagban az alábbiakkal fog megismerkedni:

> [!div class="checklist"]
> * A virtuális gép rendszerindítási diagnosztika engedélyezése
> * Rendszerindítási diagnosztika megtekintése
> * Virtuális gép gazdagép-metrikák megtekintése
> * A diagnosztika-kiterjesztés telepítése
> * Nézet VM metrikák
> * Riasztás létrehozása
> * Speciális figyelés beállítása

Az oktatóanyaghoz az Azure PowerShell-modul 3.6-os vagy újabb verziójára lesz szükség. A verzió azonosításához futtassa a következőt: ` Get-Module -ListAvailable AzureRM`. Ha frissítenie kell, lásd: [telepítése Azure PowerShell modul](/powershell/azure/install-azurerm-ps).

A példa az oktatóanyag elvégzéséhez rendelkeznie kell egy meglévő virtuális gépet. Ha szükséges, ez [parancsfájl minta](../scripts/virtual-machines-windows-powershell-sample-create-vm.md) hozhat létre egyet. Az oktatóanyag lépéseinek használatakor, cserélje ki az erőforráscsoportot, a virtuális gép nevét és a helyet, ha szükséges.

## <a name="view-boot-diagnostics"></a>Rendszerindítási diagnosztika megtekintése

Indítsa el a Windows virtuális gépek, a rendszerindítási diagnosztikai ügynök rögzíti a képernyő kimeneti hibaelhárítási céllal használható. Ez a funkció alapértelmezés szerint engedélyezve van. A rögzített képernyőképek tárolódnak az Azure storage-fiók, amely alapértelmezés szerint is létrejön. 

A rendszerindítási diagnosztikai adatok kaphat a [Get-AzureRmVMBootDiagnosticsData](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmbootdiagnosticsdata) parancsot. A következő példában a rendszerindítási diagnosztika gyökerébe letöltődnek a * c:\* meghajtó. 

```powershell
Get-AzureRmVMBootDiagnosticsData -ResourceGroupName myResourceGroup -Name myVM -Windows -LocalPath "c:\"
```

## <a name="view-host-metrics"></a>Gazdagép-metrikák megtekintése

Egy Windows virtuális gép a gazdagép dedikált virtuális gépek rendelkezik, amely hatással van az Azure-ban. Metrikák automatikusan összegyűjtött ahhoz, hogy a gazdagép és az Azure portálon is megtekinthetők.

1. Az Azure portálon kattintson **erőforráscsoportok**, jelölje be **myResourceGroup**, majd válassza ki **myVM** erőforrás listájában.
2. Kattintson a **metrikák** a virtuális gép panelen, majd válassza ki a gazdagép-metrikák bármelyikét **elérhető** tekintheti meg, hogyan működik-e a gazdagép virtuális.

    ![Gazdagép-metrikák megtekintése](./media/tutorial-monitoring/tutorial-monitor-host-metrics.png)

## <a name="install-diagnostics-extension"></a>Diagnosztika-kiterjesztés telepítése

Az alapvető állomás adatok gyűjtése le elérhető, de a részletesebb és Virtuálisgép-specifikus metrika, meg kell telepítenie az Azure diagnostics bővítményt a virtuális Gépen. Az Azure diagnostics bővítmény lehetővé teszi, hogy további figyelési és diagnosztikai adatokat beolvasni a virtuális gépről. Megtekintheti a metrikák és riasztások alapján hogyan hajtja végre a virtuális gép létrehozása. A diagnosztikai bővítmény telepítve van az Azure portálon keresztül az alábbiak szerint:

1. Az Azure portálon kattintson **erőforráscsoportok**, jelölje be **myResourceGroup**, majd válassza ki **myVM** erőforrás listájában.
2. Kattintson a **diagnosztikai beállítások**. A lista azt mutatja, hogy *rendszerindítási diagnosztika* már engedélyezve van az előző szakaszából. Jelölje be a jelölőnégyzetet a *alapvető metrikák*.
3. Kattintson a **vendégszintű a figyelés bekapcsolható** gombra.

    ![Nézet diagnosztikai metrikák](./media/tutorial-monitoring/enable-diagnostics-extension.png)

## <a name="view-vm-metrics"></a>Nézet VM metrikák

A virtuális gép mérni, hogy megtekinthetők-e a gazdagép VM metrikák azonos módon tekintheti meg:

1. Az Azure portálon kattintson **erőforráscsoportok**, jelölje be **myResourceGroup**, majd válassza ki **myVM** erőforrás listájában.
2. Hogyan működik-e a virtuális gép megtekintéséhez kattintson **metrikák** a virtuális gép panelen, majd válassza ki a diagnosztika mérőszámok alapján bármelyikét **elérhető**.

    ![Nézet VM metrikák](./media/tutorial-monitoring/monitor-vm-metrics.png)

## <a name="create-alerts"></a>Riasztások létrehozása

Riasztások adott mérőszámok alapján hozhat létre. Riasztások értesíti, amikor az átlagos CPU-használat meghaladja az egy bizonyos küszöb vagy a rendelkezésre álló szabad lemezterületet alá csökken egy adott értékre, például használható. Riasztások jelennek meg az Azure portálon, vagy e-mailben küldhetők el. Riasztás generálása Azure Automation-forgatókönyveket vagy Azure Logic Apps is elindítható.

A következő példa az átlagos processzorhasználat riasztást hoz létre.

1. Az Azure portálon kattintson **erőforráscsoportok**, jelölje be **myResourceGroup**, majd válassza ki **myVM** erőforrás listájában.
2. Kattintson a **riasztási szabályok** virtuális gép paneljén kattintson a **metrika riasztás hozzáadása** a riasztások panel tetején.
4. Adjon meg egy **neve** a riasztás például *myAlertRule*
5. Riasztást vált ki, ha processzor 1.0 meghaladja 5 percig, hagyja a többi alapértelmezett kiválasztva.
6. Szükség esetén jelölje be a *E-mail-tulajdonosok, közreműködőknek és olvasóknak* e-mail értesítést küldeni. Az alapértelmezett művelet is értesítést megjeleníteni a portálon.
7. Kattintson az **OK** gombra.

## <a name="advanced-monitoring"></a>Speciális figyelés 

Fejlettebb, figyelés, a virtuális gép segítségével teheti [Operations Management Suite](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview). Ha még nem tette meg, akkor regisztrálhatnak az egy [ingyenes próbaverzió](https://www.microsoft.com/en-us/cloud-platform/operations-management-suite-trial) az Operations Management Suite szolgáltatásban.

Ha rendelkezik az OMS-portállal, találja a kulcsát és a munkaterület azonosítóját a beállítások panelen. Használja a [Set-AzureRmVMExtension](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmextension) cmmand gombra az OMS-bővítmény hozzáadása a virtuális géphez. Frissíti a változó értékét az alábbi minta megfelelően, OMS-munkaterület kulcs és a munkaterület azonosítóját.  

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

Néhány perc múlva megtekintheti az új virtuális Gépet az OMS-munkaterület. 

![OMS panel](./media/tutorial-monitoring/tutorial-monitor-oms.png)

## <a name="next-steps"></a>Következő lépések
Ebben az oktatóanyagban konfigurálva, és tekintse át a virtuális gépek az Azure Security Center. Megtudta, hogyan, hogy:

> [!div class="checklist"]
> * Virtuális hálózat létrehozása
> * Egy erőforráscsoport és a virtuális gép létrehozása 
> * Rendszerindítási diagnosztika a virtuális Gépre engedélyezése
> * Rendszerindítási diagnosztika megtekintése
> * Gazdagép-metrikák megtekintése
> * A diagnosztika-kiterjesztés telepítése
> * Nézet VM metrikák
> * Riasztás létrehozása
> * Speciális figyelés beállítása

A következő oktatóanyag az Azure security Centerrel kapcsolatos további továbblépés.

> [!div class="nextstepaction"]
> [A virtuális gépek biztonságának kezelése](./tutorial-azure-security.md)