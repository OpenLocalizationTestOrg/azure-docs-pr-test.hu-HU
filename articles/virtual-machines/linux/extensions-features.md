---
title: "aaaVirtual gép bővítményeket és szolgáltatásokat Linux |} Microsoft Docs"
description: "Ismerje meg, milyen bővítmények érhetők el, mi akkor adja meg, vagy javítania szerint csoportosítva, az Azure virtuális gépeken."
services: virtual-machines-linux
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: 52f5d0ec-8f75-49e7-9e15-88d46b420e63
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 04/26/2017
ms.author: nepeters
ms.openlocfilehash: e0d2ce794c76815ccc6743e8788ee5d9d931e9a4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="virtual-machine-extensions-and-features-for-linux"></a>Virtuálisgép-bővítmények és a Linux funkcióit

Az Azure virtuálisgép-bővítmények kis alkalmazásokat, amelyek telepítés utáni konfigurációs és automatizálási feladatok Azure virtuális gépeken. Például ha egy virtuális gépet a Szoftvertelepítés, a víruskereső védelmet, vagy a Docker konfigurációs igényel, a Virtuálisgép-bővítmény is használt toocomplete kell ezeket a feladatokat. Az Azure Virtuálisgép-bővítmények hello Azure CLI PowerShell, Azure Resource Manager sablonok segítségével, és hello Azure-portálon. Bővítmények mellékelhető egy új virtuálisgép-telepítést, vagy bármely létező rendszert futtathat.

Ez a dokumentum áttekintést Virtuálisgép-bővítmények, Azure Virtuálisgép-bővítmények és útmutatás toodetect, hogyan kezelheti, és távolítsa el a Virtuálisgép-bővítmények használatának előfeltételei. A dokumentum általános információkat tartalmaz a Virtuálisgép-bővítmények érhetők el, mert egyes potenciálisan egyedi konfigurációval. Minden egyes dokumentum adott toohello egyedi bővítmény részletei bővítmény-specifikus megtalálhatók.

## <a name="use-cases-and-samples"></a>Használati esetek és minták

Számos különböző Azure Virtuálisgép-bővítmények érhetők el, az adott használati eset. Néhány példa:

- PowerShell kívánt állapot konfigurációk tooa virtuális gépek DSC-bővítményt hello Linux alkalmazni. További információkért lásd: [Azure kívánt állapot konfigurációs kiterjesztése](https://github.com/Azure/azure-linux-extensions/tree/master/DSC).
- A virtuális gép a Microsoft Monitoring Agent Virtuálisgép-bővítmény hello figyelés konfigurálása. További információkért lásd: [hogyan toomonitor Linux virtuális gép](tutorial-monitoring.md).
- Az Azure infrastruktúra hello Datadog bővítmény a figyelés konfigurálása. További információkért lásd: hello [Datadog blog](https://www.datadoghq.com/blog/introducing-azure-monitoring-with-one-click-datadog-deployment/).
- A Docker állomás konfigurálása Azure virtuális géphez hello Docker Virtuálisgép-bővítmény használatával. További információkért lásd: [Docker Virtuálisgép-bővítmény](dockerextension.md).

Továbbá tooprocess-specifikus bővítmények, egy egyéni parancsprogramok futtatására szolgáló bővítmény érhető el a Windows és Linux virtuális gépek. hello Linux rendszeren egyéni parancsprogramok futtatására szolgáló bővítmény lehetővé teszi, hogy bármely Bash parancsfájl toobe futtasson egy virtuális gépen. Egyéni parancsfájlok túl milyen natív Azure tooling biztosíthat beállításokra van szükség Azure központi telepítések tervezése során hasznosak. További információkért lásd: [Linux virtuális gép egyéni parancsprogramok futtatására szolgáló bővítmény](extensions-customscript.md).


## <a name="prerequisites"></a>Előfeltételek

Minden egyes virtuálisgép-bővítmény előfordulhat, hogy rendelkeznek a saját előfeltételek. Például hello Docker Virtuálisgép-bővítmény van egy támogatott Linux-disztribúció előfeltétele. Egyes bővítmények követelményeinek részletes leírást talál hello bővítmény vonatkozó dokumentációt.

### <a name="azure-vm-agent"></a>Azure virtuálisgép-ügynök

hello Azure Virtuálisgép-ügynök kezeli a hello Azure fabric controller és az Azure virtuális gépek közötti kommunikációt. hello Virtuálisgép-ügynök telepítése és kezelése az Azure virtuális gépeken, beleértve a Virtuálisgép-bővítmények futtató sok működési jellemzői felelős. hello Azure Virtuálisgép-ügynök telepítve van az Azure piactéren elérhető rendszerkép és támogatott operációs rendszeren manuálisan telepíthető.

Információ a támogatott operációs rendszerek és a telepítési utasításokat: [Azure virtuális gépi ügynöke](../windows/classic/agents-and-extensions.md).

## <a name="discover-vm-extensions"></a>Virtuálisgép-bővítmények felderítése

Számos különböző Virtuálisgép-bővítmények állnak rendelkezésre az Azure virtuális gépekkel. toosee teljes listáját, futtassa hello hello Azure CLI-parancsot a következő, hello példa lecserélését hello megfelelő helyre.

```azurecli
az vm extension image list --location westus -o table
```

## <a name="run-vm-extensions"></a>Futtassa a Virtuálisgép-bővítmények

Az Azure virtuálisgép-bővítmények futtatható meglévő virtuális gépeken, amelyek hasznosak, ha kell toomake konfigurációs módosításokat, vagy állítsa helyre a kapcsolatot egy már telepített virtuális gépen. Virtuálisgép-bővítmények is Azure Resource Manager sablon központi telepítéseket is telepíthet. Bővítmények a Resource Manager-sablonok segítségével az Azure virtuális gépek is telepítését és konfigurálását telepítés utáni beavatkozás nélkül.

a következő módszerek hello használt toorun egy bővítmény egy meglévő virtuális gépek ellen lehet.

### <a name="azure-cli"></a>Azure CLI

Az Azure virtuálisgép-bővítmények is futtathatja egy meglévő virtuális gépet hello segítségével `az vm extension set` parancsot. Ebben a példában egy virtuális gép fut hello egyéni parancsprogramok futtatására szolgáló bővítmény.

```azurecli
az vm extension set `
  --resource-group exttest `
  --vm-name exttest `
  --name customScript `
  --publisher Microsoft.Azure.Extensions `
  --settings '{"fileUris": ["https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"],"commandToExecute": "./hello.sh"}'
```

hello parancsfájl előállított kimeneti hasonló toohello a következő szöveget:

```azurecli
info:    Executing command vm extension set
+ Looking up hello VM "myVM"
+ Installing extension "CustomScript", VM: "mvVM"
info:    vm extension set command OK
```

### <a name="azure-portal"></a>Azure Portal

Virtuálisgép-bővítmények alkalmazott tooan meglévő virtuális gép hello Azure-portálon keresztül lehet. toodo Igen, jelölje ki a hello virtuális gépet, válassza a **bővítmények**, és kattintson a **Hozzáadás**. Szeretné, hogy a rendelkezésre álló bővítmények hello listából, és hello hello varázsló utasításait követve hello bővítmény kiválasztása.

hello következő kép bemutatja hello hello Linux egyéni parancsprogramok futtatására szolgáló bővítmény hello Azure-portálon való telepítését.

![Egyéni parancsfájl-kiterjesztés telepítése](./media/extensions-features/installscriptextensionlinux.png)

### <a name="azure-resource-manager-templates"></a>Azure Resource Manager-sablonok

Virtuálisgép-bővítmények hozzáadott tooan Azure Resource Manager-sablon és hello sablon hello telepítés végrehajtása. Egy bővítmény a sablon telepítésekor hozhat létre teljesen konfigurált Azure-környezetekhez. Például egy Resource Manager-sablon a következő JSON hello származik. hello sablon olyan virtuális gépek elosztott terhelésű és az Azure SQL-adatbázis telepíti, és telepíti a .NET Core alkalmazások minden egyes virtuális gépen. Virtuálisgép-bővítmény hello hello Szoftvertelepítés gondoskodik.

További információkért tekintse meg a teljes hello [Resource Manager-sablon](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).

```json
{
    "apiVersion": "2015-06-15",
    "type": "extensions",
    "name": "config-app",
    "location": "[resourceGroup().location]",
    "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', concat(variables('vmName'),copyindex()))]"
    ],
    "tags": {
    "displayName": "config-app"
    },
    "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "CustomScript",
    "typeHandlerVersion": "2.0",
    "autoUpgradeMinorVersion": true,
    "settings": {
        "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/scripts/config-music.sh"
        ]
    },
    "protectedSettings": {
        "commandToExecute": "[concat('sudo sh config-music.sh ',variables('musicStoreSqlName'), ' ', parameters('adminUsername'), ' ', parameters('sqlAdminPassword'))]"
    }
    }
}
```

További információkért lásd: [Azure Resource Manager-sablonok készítése](../windows/template-description.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#extensions).

## <a name="secure-vm-extension-data"></a>Virtuálisgép-bővítmény adatvédelem

Ha egy Virtuálisgép-bővítmény, szükséges tooinclude bizalmas információk, például a hitelesítő adatokat, a tárfiókok neve és a fiók tárelérési kulcsok lehet. Több Virtuálisgép-bővítmények közé tartozik a védett konfigurációkat, amelyek titkosítja az adatokat, és csak visszafejti hello cél virtuális gépen belül. Mindegyik bővítményt egy adott védett konfigurációs séma van, és az egyes részleteit a bővítmény-specifikus dokumentációját.

a következő példa hello hello egyéni parancsprogramok futtatására szolgáló bővítmény példányának Linux jelennek meg. Figyelje meg, hogy hello parancs tooexecute hitelesítő adatokat tartalmazza. Ebben a példában hello parancs tooexecute nem lesznek titkosítva.


```json
{
  "apiVersion": "2015-06-15",
  "type": "extensions",
  "name": "config-app",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', concat(variables('vmName'),copyindex()))]"
  ],
  "tags": {
    "displayName": "config-app"
  },
  "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "CustomScript",
    "typeHandlerVersion": "2.0",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/scripts/config-music.sh"
      ],
      "commandToExecute": "[concat('sudo sh config-music.sh ',variables('musicStoreSqlName'), ' ', parameters('adminUsername'), ' ', parameters('sqlAdminPassword'))]"
    }
  }
}
```

Áthelyezése hello **parancs tooexecute** tulajdonság toohello **védett** konfigurációs hello végrehajtási karakterlánc biztonságossá tételére.

```json
{
  "apiVersion": "2015-06-15",
  "type": "extensions",
  "name": "config-app",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', concat(variables('vmName'),copyindex()))]"
  ],
  "tags": {
    "displayName": "config-app"
  },
  "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "CustomScript",
    "typeHandlerVersion": "2.0",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/scripts/config-music.sh"
      ]
    },
    "protectedSettings": {
      "commandToExecute": "[concat('sudo sh config-music.sh ',variables('musicStoreSqlName'), ' ', parameters('adminUsername'), ' ', parameters('sqlAdminPassword'))]"
    }
  }
}
```

## <a name="troubleshoot-vm-extensions"></a>Virtuálisgép-bővítmények hibaelhárítása

Minden egyes Virtuálisgép-bővítmény hibaelhárítási lépéseket adott toohello bővítmény is rendelkezhet. Például ha az egyéni parancsprogramok futtatására szolgáló bővítmény hello használata esetén parancsfájl végrehajtásának részletes adatai található helyileg hello bővítmény futtatása hello virtuális gépen. Bővítmény-specifikus hibaelhárítási lépéseket részletes leírást talál bővítmény vonatkozó dokumentációt.

a következő hibaelhárítási lépéseket hello tooall virtuálisgép-bővítmények alkalmazni.

### <a name="view-extension-status"></a>Bővítmény állapotának megtekintése

A virtuálisgép-bővítmény futtatása a virtuális gépek ellen, után használja az hello Azure CLI parancs tooreturn bővítmény állapota a következő. Példa paraméterneveknek cserélje le a saját értékeit.

```azurecli
az vm extension list --resource-group myResourceGroup --vm-name myVM -o table
```

hello kimenete a következő szöveg hello néz ki:

```azurecli
AutoUpgradeMinorVersion    Location    Name          ProvisioningState    Publisher                   ResourceGroup      TypeHandlerVersion  VirtualMachineExtensionType
-------------------------  ----------  ------------  -------------------  --------------------------  ---------------  --------------------  -----------------------------
True                       westus      customScript  Succeeded            Microsoft.Azure.Extensions  exttest                             2  customScript
```

Bővítmény végrehajtási állapotát hello Azure-portálon is megtalálhatók. egy bővítmény, jelölje be hello virtuális gép, tooview hello állapotának kiválasztása **bővítmények**, és válassza ki a kívánt bővítmény hello.

### <a name="rerun-a-vm-extension"></a>Futtassa újra a Virtuálisgép-bővítmény

Előfordulhat, hogy esetekben, ahol a virtuálisgép-bővítmény kell toobe futtassa újra. Egy bővítmény akkor távolítsa el, majd futtassa újból a hello bővítmény egy végrehajtási módszer az Ön által választott újrafuttathatja. egy bővítmény tooremove hello Azure CLI-parancsot a következő hello futtatásához. Példa paraméterneveknek cserélje le a saját értékeit.

```azurecli
az vm extension delete --name customScript --resource-group myResourceGroup --vm-name myVM
```

Kiterjesztéssel megszüntetheti a lépések az Azure-portálon hello hello használata:

1. Válasszon ki egy virtuális gépet.
2. Válasszon **bővítmények**.
3. Jelölje ki a kívánt hello bővítményt.
4. Válasszon **eltávolítása**.

## <a name="common-vm-extension-reference"></a>Virtuálisgép-bővítmény közös hivatkozása
| Bővítmény neve | Leírás | További információ |
| --- | --- | --- |
| Linux rendszeren egyéni parancsprogramok futtatására szolgáló bővítmény |Parancsfájlok futtatásához egy Azure virtuális gépen |[Linux rendszeren egyéni parancsprogramok futtatására szolgáló bővítmény](extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) |
| Docker-bővítmény |Telepítse a hello Docker démon toosupport távoli Docker parancsok. |[Docker virtuálisgép-bővítmény](dockerextension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) |
| Virtuálisgép-hozzáférési bővítmény |Hozzáférés tooan Azure virtuális gép helyreállításához |[Virtuálisgép-hozzáférési bővítmény](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess) |
| Azure Diagnostics-bővítményt |Az Azure Diagnostics kezelése |[Azure Diagnostics-bővítményt](https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/) |
| Az Azure virtuális gép hozzáférési bővítmény |Felhasználók és a hitelesítő adatok kezelése |[Linux virtuális gép hozzáférési bővítmény](https://azure.microsoft.com/en-us/blog/using-vmaccess-extension-to-reset-login-credentials-for-linux-vm/) |
