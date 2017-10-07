---
title: "a virtuálisgép-méretezési alkalmazás aaaDeploy beállítása"
description: "Azure virtuálisgép-méretezési csoportok bővítmények toodepoy egy alkalmazás használatát."
services: virtual-machine-scale-sets
documentationcenter: 
author: thraka
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f8892199-f2e2-4b82-988a-28ca8a7fd1eb
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/26/2017
ms.author: adegeo
ms.openlocfilehash: 5f3988b9511d80370a8be1fc042c21fee212506e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-your-application-on-virtual-machine-scale-sets"></a>A virtuálisgép-méretezési csoportok az alkalmazás központi telepítése

Ez a cikk ismerteti, hogyan tooinstall szoftver hello idő hello léptékű be különböző módokat lett kiépítve.

Érdemes lehet tooreview hello [méretezési beállítása kialakítás áttekintése](virtual-machine-scale-sets-design-overview.md) cikket, amely ismerteti az egyes hello határoz meg küszöbértéket virtuálisgép-méretezési csoportok.

## <a name="capture-and-reuse-an-image"></a>Rögzítheti és újra felhasználhatja a képfájl

Használhat egy virtuális gépet az Azure tooprepare egy alap-lemezképet a bővített adott meg. A folyamat egy felügyelt lemezes tárolás, a fiókot is hivatkozni lehessen hello alapjául szolgáló lemezképhez, a méretezési készlet hoz létre. 

Hello a következő lépéseket:

1. Egy Azure virtuális gép létrehozása
   * [Linux][linux-vm-create]
   * [Windows][windows-vm-create]

2. A távoli virtuális gép hello és hello rendszer tooyour tetszőlegesen testreszabása.

   Ha azt szeretné, az alkalmazás most már telepítheti. Azonban tudnia, hogy az alkalmazás most telepítésével készíthet az alkalmazás bonyolultabb frissítése, mert tooremove szükség lehet az első. Ehelyett használhatja ezt a lépést tooinstall előfeltételeket az alkalmazás esetleg, például egy adott runtime vagy az operációs rendszer szolgáltatása.

3. Az oktatóanyag hello "gép rögzítése" bármelyik [Linux] [ linux-vm-capture] vagy [Windows][windows-vm-capture].

4. Hozzon létre egy [virtuálisgép-méretezési csoport] [ vmss-create] hello rendelkező URI hello előző lépésben rögzített rendszerkép.

Lemezek kapcsolatos további információkért lásd: [felügyelt lemezekhez – áttekintés](../virtual-machines/windows/managed-disks-overview.md) és [használata adatlemezt csatolni](virtual-machine-scale-sets-attached-disks.md).

## <a name="install-when-hello-scale-set-is-provisioned"></a>Amikor hello méretezési ki van építve telepítése

Virtuálisgép-bővítmények lehet tooa virtuálisgép-méretezési csoport alkalmazza. Egy virtuálisgép-bővítménnyel testre szabhatja a virtuális gépek hello terjedő skálán állítja be a teljes csoport. További információ a bővítményekről: [virtuálisgép-bővítmények](../virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Három fő kiterjesztések is használhatja, ha az operációs rendszertől függően Linux- vagy Windows-alapú.

### <a name="windows"></a>Windows

Egy Windows-alapú operációs rendszeren használja vagy hello **egyéni parancsfájl v1.8** kiterjesztés vagy a hello **PowerShell DSC** bővítmény.

#### <a name="custom-script"></a>Egyéni parancsfájl

Egyéni parancsprogramok futtatására szolgáló bővítmény hello hello méretezési csoportban lévő virtuális gép feltünteti parancsfájlt futtat. A konfigurációs fájlban vagy a változó azt jelzi, mely fájlok legyenek letöltött toohello virtuális gépet, majd milyen parancsot futtatja. A telepítő toorun, egy parancsfájlt, egy kötegfájlt, végrehajtható fájlok például sikerült használja.

PowerShell egy kivonattáblát hello beállításokat használ. Ez a példa hello egyéni parancsfájl kiterjesztése toorun egy PowerShell-parancsfájlt, amely telepíti az IIS konfigurálása.

```powershell
# Setup extension configuration hashtable variable
$customConfig = @{
  "fileUris" = @("https://raw.githubusercontent.com/MicrosoftDocs/azure-cloud-services-files/temp/install-iis.ps1");
  "commandToExecute" = "PowerShell -ExecutionPolicy Unrestricted .\install-iis.ps1 >> `"%TEMP%\StartupLog.txt`" 2>&1";
};

# Add hello extension toohello config
Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmssConfig -Publisher Microsoft.Compute -Type CustomScriptExtension -TypeHandlerVersion 1.8 -Name "customscript1" -Setting $customConfig

# Send hello new config tooAzure
Update-AzureRmVmss -ResourceGroupName $rg -Name "MyVmssTest143"  -VirtualMachineScaleSet $vmssConfig
```

>[!IMPORTANT]
>Használjon hello `-ProtectedSetting` bármely beállítások bizalmas információkat tartalmazó váltani.

---------


Az Azure CLI hello-beállítások egy json-fájlt használ. Ez a példa hello egyéni parancsfájl kiterjesztése toorun egy PowerShell-parancsfájlt, amely telepíti az IIS konfigurálása. Mentse a json-fájl, a következő hello _settings.json_.

```json
{
  "fileUris": [
    "https://raw.githubusercontent.com/MicrosoftDocs/azure-cloud-services-files/temp/install-iis.ps1"
  ],
  "commandToExecute": "PowerShell -ExecutionPolicy Unrestricted .\install-iis.ps1 >> \"%TEMP%\StartupLog.txt\" 2>&1"
}
```

Ezután futtassa az Azure CLI parancsot.

```azurecli
az vmss extension set --publisher Microsoft.Compute --version 1.8 --name CustomScriptExtension --resource-group myResourceGroup --vmss-name myScaleSet --settings @settings.json
```

>[!IMPORTANT]
>Használjon hello `--protected-settings` bármely beállítások bizalmas információkat tartalmazó váltani.

### <a name="powershell-dsc"></a>A PowerShell DSC

Használhatja a PowerShell DSC toocustomize hello méretezési készlet virtuálisgép-példányok. Hello **DSC** bővítmény által közzétett **Microsoft.Powershell** telepíti, és futtatja a megadott hello DSC-konfiguráció minden egyes virtuálisgép-példányon. A konfigurációs fájlban vagy a változó közli hello bővítmény ahol *.zip* csomag van, és amely _parancsfájl-függvény_ kombinációja toorun.

PowerShell egy kivonattáblát hello beállításokat használ. Ebben a példában a DSC-csomagot, amely telepíti az IIS szolgáltatást telepíti.

```powershell
# Setup extension configuration hashtable variable
$dscConfig = @{
  "wmfVersion" = "latest";
  "configuration" = @{
    "url" = "https://github.com/MicrosoftDocs/azure-cloud-services-files/raw/temp/dsc.zip";
    "script" = "configure-http.ps1";
    "function" = "WebsiteTest";
  };
}

# Add hello extension toohello config
Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmssConfig -Publisher Microsoft.Powershell -Type DSC -TypeHandlerVersion 2.24 -Name "dsc1" -Setting $dscConfig

# Send hello new config tooAzure
Update-AzureRmVmss -ResourceGroupName $rg -Name "myscaleset1"  -VirtualMachineScaleSet $vmssConfig
```

>[!IMPORTANT]
>Használjon hello `-ProtectedSetting` bármely beállítások bizalmas információkat tartalmazó váltani.

-----------

Az Azure CLI json hello beállításokat használ. Ebben a példában a DSC-csomagot, amely telepíti az IIS szolgáltatást telepíti. Mentse a json-fájl, a következő hello _settings.json_.

```json
{
  "wmfVersion": "latest",
  "configuration": {
    "url": "https://github.com/MicrosoftDocs/azure-cloud-services-files/raw/temp/dsc.zip",
    "script": "configure-http.ps1",
    "function": "WebsiteTest"
  }
}
```

Ezután futtassa az Azure CLI parancsot.

```azurecli
az vmss extension set --publisher Microsoft.Powershell --version 2.24 --name DSC --resource-group myResourceGroup --vmss-name myScaleSet --settings @settings.json
```

>[!IMPORTANT]
>Használjon hello `--protected-settings` bármely beállítások bizalmas információkat tartalmazó váltani.

### <a name="linux"></a>Linux

Linux használhatja bármelyik hello **egyéni parancsfájl v2.0** kiterjesztést, vagy használjon **felhő inicializálás** létrehozása során.

Egyéni parancsfájl olyan egyszerű bővítménnyel, amely letölti a fájlokat toohello virtuálisgép-példánya, és a parancsot futtatja.

#### <a name="custom-script"></a>Egyéni parancsfájl

Mentse a json-fájl, a következő hello _settings.json_.

```json
{
  "fileUris": [
    "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vmss-bottle-autoscale/installserver.sh",
    "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vmss-bottle-autoscale/workserver.py"
  ],
  "commandToExecute": "bash installserver.sh"
}
```

Hello Azure CLI tooadd a meglévő virtuálisgép-méretezési csoport kiterjesztése tooan használja. Minden virtuális gép hello méretezési automatikusan fut hello virtuáliskapcsoló-kiterjesztés beállítása.

```azurecli
az vmss extension set --publisher Microsoft.Azure.Extensions --version 2.0 --name CustomScript --resource-group myResourceGroup --vmss-name myScaleSet --settings @settings.json
```

>[!IMPORTANT]
>Használjon hello `--protected-settings` bármely beállítások bizalmas információkat tartalmazó váltani.

#### <a name="cloud-init"></a>Felhő inicializálás

Felhő inicializálás hello méretezési csoport létrehozásakor használható. Először hozzon létre egy helyi fájlt _felhő-init.txt_ , és adja hozzá a konfigurációs tooit. Lásd például: [a gist](https://gist.github.com/Thraka/27bd66b1fb79e11904fb62b7de08a8a6#file-cloud-init-txt)

Használjon hello Azure CLI toocreate terjedő skálán beállítása. Hello `--custom-data` a mezőbe egy felhő-init parancsfájl hello neve.

```azurecli
az vmss create \
  --resource-group myResourceGroupScaleSet \
  --name myScaleSet \
  --image Canonical:UbuntuServer:14.04.4-LTS:latest \
  --upgrade-policy-mode automatic \
  --custom-data cloud-init.txt \
  --admin-username azureuser \
  --generate-ssh-keys      
```

## <a name="how-do-i-manage-application-updates"></a>Hogyan kezelhetem az alkalmazás frissítései?

Ha az alkalmazás egy bővítmény használatával telepítette, az alter hello bővítmény definíciójának valamilyen módon. Ez a módosítás hatására hello bővítmény újratelepítése toobe tooall virtuálisgép-példánya. Valami **kell** hello bővítményről, például a hivatkozott fájl átnevezése, ellenkező esetben lehet módosítani az Azure biztosítja, nem a bővítmény hello lásd megváltozott.

Hello alkalmazás a saját operációs rendszer lemezképén bővíthetőség akkor, ha egy automatikus telepítési folyamat használata alkalmazásfrissítéseket. Tervezze meg a gyors egy előkészített méretezési készletben éles környezetben a csere architektúra toofacilitate. Ez a módszer jól szemlélteti hello [Azure Spinnaker illesztőprogram munkahelyi](https://github.com/spinnaker/deck/tree/master/app/scripts/modules/azure) - [http://www.spinnaker.io/](http://www.spinnaker.io/).

[Csomagoló](https://www.packer.io/) és [Terraform](https://www.terraform.io/) támogatási Azure Resource Manager, így is adja meg a képek "kódú" és build őket az Azure, majd használja a méretezési csoportban lévő virtuális merevlemez hello. Azonban ez úgy válna piactéren elérhető rendszerkép, ahol bővítmények/egyéni parancsfájlok válnak fontosabb óta, nem közvetlenül kezelheti a piactérről bits problémákat okozhatnak.

## <a name="what-happens-when-a-scale-set-scales-out"></a>Mi történik, ha egy méretezési méretezik el?
Egy vagy több virtuális gépek tooa méretezési hozzáadásakor hello alkalmazás automatikusan települ. A példa hello méretezési van az definiált bővítmények, számítógépen futnak egy új virtuális gép minden alkalommal létrejön. Ha hello méretezési egyéni lemezkép alapul, bármely új virtuális gép hello forrás egyéni lemezkép egy példányát. Ha hello méretezési készlet virtuális gépeket tároló gazdagépeken, majd lehetséges, hogy indítási kód tooload hello tárolók az egy egyéni parancsprogramok futtatására szolgáló bővítmény. Vagy egy bővítmény a céllal telepíthet olyan ügynök, amely egy fürt orchestrator, például az Azure Tárolószolgáltatás regisztrálja.


## <a name="how-do-you-roll-out-an-os-update-across-update-domains"></a>Hogyan tegye megkezdik az operációs rendszer frissítése frissítési tartományok között?
Tegyük fel, hogy tooupdate az operációsrendszer-lemezképek hello virtuálisgép-méretezési futó megtartásával. PowerShell és az Azure parancssori felület hello hello virtuálisgép-lemezképeket, egyszerre egy virtuális gép frissíthető. Hello [frissítéséhez egy virtuálisgép-méretezési csoportban](./virtual-machine-scale-sets-upgrade-scale-set.md) cikkben további információkat is biztosít a milyen lehetőségek állnak rendelkezésre tooperform az operációs rendszer frissítése a virtuálisgép-méretezési csoport között.

## <a name="next-steps"></a>Következő lépések

* [PowerShell toomanage használja a méretezési készlet.](virtual-machine-scale-sets-windows-manage.md)
* [A méretezési készlet sablont létrehozni.](virtual-machine-scale-sets-mvss-start.md)


[linux-vm-create]: ../virtual-machines/linux/tutorial-manage-vm.md
[windows-vm-create]: ../virtual-machines/windows/tutorial-manage-vm.md
[linux-vm-capture]: ../virtual-machines/linux/capture-image.md
[windows-vm-capture]: ../virtual-machines/windows/capture-image.md 
[vmss-create]: virtual-machine-scale-sets-create.md

