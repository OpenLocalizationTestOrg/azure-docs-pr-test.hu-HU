---
title: "Indítsa újra a virtuális gépek CLI parancsfájl minta - aaaAzure |} Microsoft Docs"
description: "Az Azure CLI parancsfájl minta - újraindítás virtuális gépek kódcímke és -azonosító szerint"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: allclark
manager: douge
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/01/2017
ms.author: allclark
ms.custom: mvc
ms.openlocfilehash: a1ae07bd1d2be906553bef817385a4a333a10eca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="restart-vms"></a>Indítsa újra a virtuális gépek

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

A következő példában látható módon tooget néhány egyes virtuális gépek, és indítsa újra őket.

hello minden hello virtuális gép első újraindítása hello erőforráscsoportban.

```bash
az vm restart --ids $(az vm list --resource-group myResourceGroup --query "[].id" -o tsv)
```

hello második lekérdezi hello címkézett használó virtuális gépek `az resouce list` szűrők toohello erőforrások virtuális gépek láthatók, és újraindítja a virtuális gépek.

```bash
az vm restart --ids $(az resource list --tag "restart-tag" --query "[?type=='Microsoft.Compute/virtualMachines'].id" -o tsv)
```

Ez a minta a rendszerhéjakba működik. Az Azure parancssori felület parancsfájlok futtatásához a Windows-ügyfelén beállítások, lásd: [a Windows hello Azure CLI-t futtató](../windows/cli-options.md).


## <a name="sample-script"></a>Mintaparancsfájl

hello minta három parancsfájlok rendelkezik.
hello először egy kiosztja hello a virtuális gépeket.
Használ hello no-wait, hello parancs minden virtuális gép toobe kiépített várakozás nélkül adja vissza.
hello második megvárja-e a hello virtuális gépek toobe teljesen kiépítve.
hello harmadik parancsfájl hello virtuális gépeinek kiépített újraindul, és majd csak hello címkézett virtuális gépeket.

### <a name="provision-hello-vms"></a>Virtuális gépek hello kiépítése

Ez a parancsfájl létrehoz egy erőforráscsoport, és három virtuális gépek toorestart létrehozza majd.
Kettő címkével rendelkeznek.

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/restart-by-tag/provision.sh "Provision hello VMs")]

### <a name="wait"></a>Várakozás

A hello előkészítési állapotát minden 20 másodperc kiépített összes három virtuális gépet, vagy egyiket tooprovision sikertelen lesz. a szkript ellenőrzi.

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/restart-by-tag/wait.sh "Wait for hello VMs toobe provisioned")]

### <a name="restart-hello-vms"></a>Indítsa újra a hello virtuális gépek

Ezt a parancsfájlt minden hello virtuális gép újraindul hello erőforráscsoportban, és majd csak címkézett hello VMs újraindul.

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/restart-by-tag/restart.sh "Restart VMs by tag")]

## <a name="clean-up-deployment"></a>Az üzemelő példány eltávolítása 

Hello parancsfájl minta futtatása után a következő parancs hello lehet használt tooremove hello erőforráscsoportok, a virtuális gépek és az összes kapcsolódó erőforrások.

```azurecli-interactive 
az group delete -n myResourceGroup --no-wait --yes
```

## <a name="script-explanation"></a>Parancsfájl ismertetése

A parancsfájl a következő parancsok toocreate erőforráscsoport, a virtuális gép, a rendelkezésre állási csoport, a terheléselosztó és a minden kapcsolódó erőforrások hello. Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.

| Parancs | Megjegyzések |
|---|---|
| [az csoport létrehozása](https://docs.microsoft.com/cli/azure/group#create) | Az összes erőforrás tároló erőforrás csoportot hoz létre. |
| [az virtuális gép létrehozása](https://docs.microsoft.com/cli/azure/vm/availability-set#create) | Létrehozza a hello virtuális gépeket.  |
| [az vm listája](https://docs.microsoft.com/cli/azure/vm#list) | A használt `--query` tooensure hello virtuális gépek kiépített őket újraindítása előtt, és ezután tooget hello hello virtuális gépek toorestart azonosítói őket. |
| [az erőforrások listája](https://docs.microsoft.com/cli/azure/vm#list) | A használt `--query` tooget hello azonosítók hello virtuális gépek hello címke használata. |
| [az a virtuális gép újraindítása](https://docs.microsoft.com/cli/azure/vm#list) | Hello virtuális gépek újraindul. |
| [az csoport törlése](https://docs.microsoft.com/cli/azure/vm/extension#set) | Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése. |

## <a name="next-steps"></a>Következő lépések

Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).

További virtuális gép CLI parancsfájl minták hello található [Azure Linux virtuális dokumentációját](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
