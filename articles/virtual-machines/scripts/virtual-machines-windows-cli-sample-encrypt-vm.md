---
title: "CLI parancsfájl minta - aaaAzure egy Windows virtuális gép titkosítása |} Microsoft Docs"
description: "Az Azure CLI Script Sample – a Windows virtuális gép titkosítása"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 06/02/2017
ms.author: iainfou
ms.openlocfilehash: 7a9928467f818cd5358d3d19bff5129d8fa21313
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="encrypt-a-windows-virtual-machine-in-azure"></a>A Windows Azure virtuális gép titkosítása

Ez a parancsfájl létrehoz egy biztonságos Azure Key Vault, a titkosítási kulcsokat, az Azure Active Directory szolgáltatás egyszerű és a Windows rendszerű virtuális gép (VM). virtuális gép hello majd titkosított hello titkosítási kulcs a Key Vault és a szolgáltatás egyszerű hitelesítő adatok használatával.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Mintaparancsfájl

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/encrypt-disks/encrypt_windows_vm.sh "Encrypt VM disks")]

## <a name="clean-up-deployment"></a>Az üzemelő példány eltávolítása 

Futtassa a következő parancs tooremove hello erőforráscsoport, virtuális gép és minden kapcsolódó erőforrások hello.

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Parancsfájl ismertetése

A parancsfájl a következő parancsok toocreate hello erőforrás csoport, az Azure Key Vault szolgáltatás egyszerű, virtuális gép, és minden kapcsolódó erőforrásokat. Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.

| Parancs | Megjegyzések |
|---|---|
| [az csoport létrehozása](https://docs.microsoft.com/cli/azure/group#create) | Az összes erőforrás tároló erőforrás csoportot hoz létre. |
| [az keyvault létrehozása](https://docs.microsoft.com/cli/azure/keyvault#create) | Az Azure Key Vault toostore biztonságos adatokat például a titkosítási kulcsokat hoz létre. |
| [az keyvault kulcs létrehozása](https://docs.microsoft.com/cli/azure/keyvault/key#create) | A Key Vault egy titkosítási kulcsot hoz létre. |
| [az ad sp létrehozása-az-rbac](https://docs.microsoft.com/cli/azure/ad/sp#create-for-rbac) | Egy Azure Active Directory szolgáltatás egyszerű toosecurely hitelesítéséhez, és szabályozhatja a hívóbetűk tooencryption hoz létre. |
| [az keyvault-szabály beállítása](https://docs.microsoft.com/cli/azure/keyvault#set-policy) | Olyan engedélyeket ad a hello Key Vault toogrant hello szolgáltatás egyszerű tooencryption hívóbetűk. |
| [az virtuális gép létrehozása](https://docs.microsoft.com/cli/azure/vm#create) | Hello virtuális gépet hoz létre, és kapcsolódik, akkor toohello hálózati kártya, virtuális hálózatot, alhálózatot és NSG. Ez a parancs is meghatározza hello virtuálisgép-lemezkép toobe használt, és rendszergazdai hitelesítő adatait.  |
| [az vm-titkosítás engedélyezése](https://docs.microsoft.com/cli/azure/vm/encryption#enable) | Engedélyezi a titkosítás használatát a virtuális gépek hello szolgáltatás egyszerű hitelesítő adatait, és a titkosítási kulcs használatával. |
| [az vm titkosítási megjelenítése](https://docs.microsoft.com/cli/azure/vm/encryption#show) | Virtuális gép titkosítási folyamat hello hello állapotát jeleníti meg. |
| [az csoport törlése](https://docs.microsoft.com/cli/azure/vm/extension#set) | Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése. |

## <a name="next-steps"></a>Következő lépések

Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).

További virtuális gép CLI parancsfájl minták hello található [Azure Windows virtuális dokumentációját](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%windows%2ftoc.json).
