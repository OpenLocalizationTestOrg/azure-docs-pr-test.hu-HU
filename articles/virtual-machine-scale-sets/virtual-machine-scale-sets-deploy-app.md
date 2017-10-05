---
title: "A virtuálisgép-méretezési csoportok alkalmazás telepítése"
description: "A bővítmények számára az alkalmazások depoy használata Azure virtuálisgép-méretezési készlet."
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
ms.openlocfilehash: fa7d9d3bef4cb326844ede76171e8c566e87116b
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="deploy-your-application-on-virtual-machine-scale-sets"></a>A virtuálisgép-méretezési csoportok az alkalmazás központi telepítése

Ez a cikk ismerteti a szoftver telepítése a méretezési ki van építve időpontjában különböző módokat.

Előfordulhat, hogy szeretné megtekinteni a [méretezési beállítása kialakítás áttekintése](virtual-machine-scale-sets-design-overview.md) cikket, amely bemutat néhányat a határoz meg küszöbértéket virtuálisgép-méretezési készlet.

## <a name="capture-and-reuse-an-image"></a>Rögzítheti és újra felhasználhatja a képfájl

A base-lemezkép előkészítése a scale Azure-ban beállított virtuális gép is használhatja. A folyamat egy felügyelt lemezes tárolás, a fiókot is hivatkozni lehessen az alapjául szolgáló lemezképhez, a méretezési készlet hoz létre. 

Hajtsa végre az alábbi lépéseket:

1. Egy Azure virtuális gép létrehozása
   * [Linux][linux-vm-create]
   * [Windows][windows-vm-create]

2. A virtuális gép be távolról és testre szabhatja a tetszőlegesen a rendszer.

   Ha azt szeretné, az alkalmazás most már telepítheti. Azonban tudnia, hogy az alkalmazás most telepíti, lehetséges, hogy tegyen bonyolultabb az alkalmazás frissítése, mert szükség lehet, először távolítsa el azt. Ehelyett az ebben a lépésben segítségével telepítenie az előfeltételeket, az alkalmazás esetleg, például egy adott runtime vagy az operációs rendszer szolgáltatása.

3. Az oktatóanyag a "gép rögzítése" bármelyik [Linux] [ linux-vm-capture] vagy [Windows][windows-vm-capture].

4. Hozzon létre egy [virtuálisgép-méretezési csoport] [ vmss-create] a lemezképpel az előző lépésben rögzített URI.

Lemezek kapcsolatos további információkért lásd: [felügyelt lemezekhez – áttekintés](../virtual-machines/windows/managed-disks-overview.md) és [használata adatlemezt csatolni](virtual-machine-scale-sets-attached-disks.md).

## <a name="install-when-the-scale-set-is-provisioned"></a>Ha a méretezési ki van építve telepítése

A virtuálisgép-méretezési csoport virtuálisgép-bővítmények alkalmazhatja. Egy virtuálisgép-bővítménnyel testre szabhatja a virtuális gépek terjedő skálán állítja be a teljes csoport. További információ a bővítményekről: [virtuálisgép-bővítmények](../virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Három fő kiterjesztések is használhatja, ha az operációs rendszertől függően Linux- vagy Windows-alapú.

### <a name="windows"></a>Windows

Használja a Windows-alapú operációs rendszer esetén a **egyéni parancsfájl v1.8** kiterjesztést, vagy a **PowerShell DSC** bővítmény.

#### <a name="custom-script"></a>Egyéni parancsfájl

Az egyéni parancsprogramok futtatására szolgáló bővítmény parancsfájlt futtat a méretezési csoportban lévő egyes virtuálisgép-példányt. A konfigurációs fájlban vagy a változó azt jelzi, mely fájlokat szeretne letölteni a virtuális géphez, majd milyen parancsot futtatja. Telepítőhöz, egy parancsfájlt, egy kötegfájlt, végrehajtható fájlok például futtatásához használhatja.

PowerShell egy kivonattáblát a beállításokat használja. Ebben a példában az egyéni parancsprogramok futtatására szolgáló bővítmény futtatni egy PowerShell-parancsfájlt, amely telepíti az IIS konfigurálása.

```powershell
# Setup extension configuration hashtable variable
$customConfig = @{
  "fileUris" = @("https://raw.githubusercontent.com/MicrosoftDocs/azure-cloud-services-files/temp/install-iis.ps1");
  "commandToExecute" = "PowerShell -ExecutionPolicy Unrestricted .\install-iis.ps1 >> `"%TEMP%\StartupLog.txt`" 2>&1";
};

# Add the extension to the config
Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmssConfig -Publisher Microsoft.Compute -Type CustomScriptExtension -TypeHandlerVersion 1.8 -Name "customscript1" -Setting $customConfig

# Send the new config to Azure
Update-AzureRmVmss -ResourceGroupName $rg -Name "MyVmssTest143"  -VirtualMachineScaleSet $vmssConfig
```

>[!IMPORTANT]
>Használja a `-ProtectedSetting` bármely beállítások bizalmas információkat tartalmazó váltani.

---------


Az Azure parancssori felület a beállításokat egy json-fájlt használ. Ebben a példában az egyéni parancsprogramok futtatására szolgáló bővítmény futtatni egy PowerShell-parancsfájlt, amely telepíti az IIS konfigurálása. A következő json fájl mentése másként _settings.json_.

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
>Használja a `--protected-settings` bármely beállítások bizalmas információkat tartalmazó váltani.

### <a name="powershell-dsc"></a>A PowerShell DSC

PowerShell DSC segítségével testre szabhatja a méretezési készlet virtuálisgép-példányok. A **DSC** bővítmény által közzétett **Microsoft.Powershell** telepíti és futtatja a megadott DSC-konfiguráció minden egyes virtuálisgép-példányt. A konfigurációs fájlban vagy a változó közli a bővítmény ahol *.zip* csomag van, és amely _parancsfájl-függvény_ kombinációja futtatásához.

PowerShell egy kivonattáblát a beállításokat használja. Ebben a példában a DSC-csomagot, amely telepíti az IIS szolgáltatást telepíti.

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

# Add the extension to the config
Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmssConfig -Publisher Microsoft.Powershell -Type DSC -TypeHandlerVersion 2.24 -Name "dsc1" -Setting $dscConfig

# Send the new config to Azure
Update-AzureRmVmss -ResourceGroupName $rg -Name "myscaleset1"  -VirtualMachineScaleSet $vmssConfig
```

>[!IMPORTANT]
>Használja a `-ProtectedSetting` bármely beállítások bizalmas információkat tartalmazó váltani.

-----------

Az Azure CLI json a beállításokat használja. Ebben a példában a DSC-csomagot, amely telepíti az IIS szolgáltatást telepíti. A következő json fájl mentése másként _settings.json_.

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
>Használja a `--protected-settings` bármely beállítások bizalmas információkat tartalmazó váltani.

### <a name="linux"></a>Linux

Linux használhatja a **egyéni parancsfájl v2.0** kiterjesztést, vagy használjon **felhő inicializálás** létrehozása során.

Egyéni parancsfájl olyan egyszerű bővítménnyel, amely letölti a fájlokat a virtuálisgép-példánya, és a parancsot futtatja.

#### <a name="custom-script"></a>Egyéni parancsfájl

A következő json fájl mentése másként _settings.json_.

```json
{
  "fileUris": [
    "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vmss-bottle-autoscale/installserver.sh",
    "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vmss-bottle-autoscale/workserver.py"
  ],
  "commandToExecute": "bash installserver.sh"
}
```

Az Azure parancssori felület használatával adja hozzá ezt a bővítményt egy meglévő virtuálisgép-méretezési csoport. A méretezési készletben automatikusan a virtuális gépeken futtatja a bővítmény.

```azurecli
az vmss extension set --publisher Microsoft.Azure.Extensions --version 2.0 --name CustomScript --resource-group myResourceGroup --vmss-name myScaleSet --settings @settings.json
```

>[!IMPORTANT]
>Használja a `--protected-settings` bármely beállítások bizalmas információkat tartalmazó váltani.

#### <a name="cloud-init"></a>Felhő inicializálás

Felhő inicializálás a méretezési csoport létrehozásakor használható. Először hozzon létre egy helyi fájlt _felhő-init.txt_ , és a konfigurációs felvétele. Lásd például: [a gist](https://gist.github.com/Thraka/27bd66b1fb79e11904fb62b7de08a8a6#file-cloud-init-txt)

Az Azure parancssori felület használatával hozzon létre egy méretezési készlet. A `--custom-data` mezőben fogadja el a felhő inicializálás parancsfájl nevét.

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

Ha telepítette az alkalmazást egy bővítményével, módosítsa a bővítmény definíciójának valamilyen módon. Ez a módosítás hatására a bővítményt a virtuálisgép-példányok újratelepítése. Valami **kell** a bővítmény, például a hivatkozott fájl átnevezése, ellenkező esetben lehet módosítani az Azure biztosítja nem tekintse meg a kiterjesztése megváltozott.

Ha Ön az alkalmazás a saját operációs rendszer lemezképén bővíthetőség, használjon egy automatikus telepítési folyamat alkalmazás frissítések. Tervezze meg a architektúra lehetővé teszi egy előkészített méretezési készletben éles környezetben gyors csere. Ez a módszer egy jó példa a [Azure Spinnaker illesztőprogram munkahelyi](https://github.com/spinnaker/deck/tree/master/app/scripts/modules/azure) - [http://www.spinnaker.io/](http://www.spinnaker.io/).

[Csomagoló](https://www.packer.io/) és [Terraform](https://www.terraform.io/) támogatási Azure Resource Manager így is adja meg a képek "kódú" és build őket az Azure, majd használja a virtuális merevlemez a méretezési csoportban lévő. Azonban ez úgy válna piactéren elérhető rendszerkép, ahol bővítmények/egyéni parancsfájlok válnak fontosabb óta, nem közvetlenül kezelheti a piactérről bits problémákat okozhatnak.

## <a name="what-happens-when-a-scale-set-scales-out"></a>Mi történik, ha egy méretezési méretezik el?
Amikor egy vagy több virtuális gépeket ad hozzá egy méretezési, az alkalmazás automatikusan települ. A példa a méretezési van az definiált bővítmények, számítógépen futnak egy új virtuális gép minden alkalommal létrejön. Ha a méretezési egyéni lemezkép alapul, bármely új virtuális gép a forrás egyéni lemezkép egy példányát. Esetén a méretezési készlet virtuális gépeket tároló állomások, majd lehetséges, hogy a tárolókat az egy egyéni parancsprogramok futtatására szolgáló bővítmény betöltése indítás kódot. Vagy egy bővítmény a céllal telepíthet olyan ügynök, amely egy fürt orchestrator, például az Azure Tárolószolgáltatás regisztrálja.


## <a name="how-do-you-roll-out-an-os-update-across-update-domains"></a>Hogyan tegye megkezdik az operációs rendszer frissítése frissítési tartományok között?
Tegyük fel, hogy az operációsrendszer-lemezképek frissítéséhez a virtuálisgép-méretezési futó megtartásával. PowerShell és az Azure parancssori felület frissítheti a virtuális gép képfájljait, egyszerre egy virtuális gépet. A [frissítéséhez egy virtuálisgép-méretezési csoportban](./virtual-machine-scale-sets-upgrade-scale-set.md) cikkben további információkat is biztosít a milyen beállítások érhetők el operációs rendszer verziófrissítést végrehajtani egy virtuálisgép-méretezési csoport között.

## <a name="next-steps"></a>Következő lépések

* [A PowerShell szolgáltatás használatával kezelheti a méretezési készlet.](virtual-machine-scale-sets-windows-manage.md)
* [A méretezési készlet sablont létrehozni.](virtual-machine-scale-sets-mvss-start.md)


[linux-vm-create]: ../virtual-machines/linux/tutorial-manage-vm.md
[windows-vm-create]: ../virtual-machines/windows/tutorial-manage-vm.md
[linux-vm-capture]: ../virtual-machines/linux/capture-image.md
[windows-vm-capture]: ../virtual-machines/windows/capture-image.md 
[vmss-create]: virtual-machine-scale-sets-create.md

