---
title: "parancssori felület parancsfájl minta - Load balance forgalom tooVMs a magas rendelkezésre állású aaaAzure |} Microsoft Docs"
description: "Az Azure CLI parancsfájl minta - Load balance forgalom tooVMs a magas rendelkezésre állású"
services: load-balancer
documentationcenter: load-balancer
author: KumudD
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: load-balancer
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 07/07/2017
ms.author: kumud
ms.openlocfilehash: 0954b5c261512724dfb9c6e7be123c9d45624f4d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="load-balance-traffic-toovms-for-high-availability"></a>Egyenleg forgalom tooVMs a magas rendelkezésre állású betöltése

Ez a minta parancsfájlt hoz létre minden szükséges toorun több Ubuntu virtuális gépek magas rendelkezésre állású konfigurálva és betölteni a konfigurációs elosztott terhelésű. Három virtuális gépet, a csatlakoztatott tooan Azure rendelkezésre állási csoportban, hogy után hello a parancsfájl futtatását, és az Azure Load Balancer keresztül érhető el. 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Mintaparancsfájl

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-nlb/create-vm-nlb.sh "Quick Create VM")]

## <a name="clean-up-deployment"></a>Az üzemelő példány eltávolítása 

Futtassa a következő parancs tooremove hello erőforráscsoport, virtuális gép és minden kapcsolódó erőforrások hello.

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Parancsfájl ismertetése

A parancsfájl a következő parancsok toocreate erőforráscsoport, a virtuális gép, a rendelkezésre állási csoport, a terheléselosztó és a minden kapcsolódó erőforrások hello. Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.

| Parancs | Megjegyzések |
|---|---|
| [az csoport létrehozása](https://docs.microsoft.com/cli/azure/group#create) | Az összes erőforrás tároló erőforrás csoportot hoz létre. |
| [az hálózati virtuális hálózat létrehozása](https://docs.microsoft.com/cli/azure/network/vnet#create) | Létrehoz egy Azure-beli virtuális hálózat és az alhálózatot. |
| [az hálózati nyilvános ip-létrehozása](https://docs.microsoft.com/cli/azure/network/public-ip#create) | Hoz létre egy nyilvános IP-cím egy statikus IP-címet és egy társított DNS-névvel. |
| [az hálózati terheléselosztó létrehozása](https://docs.microsoft.com/cli/azure/network/lb#create) | Egy Azure terheléselosztót hoz létre. |
| [az hálózati lb mintavétel létrehozása](https://docs.microsoft.com/cli/azure/network/lb/probe#create) | Terheléselosztói mintavétel létrehozása. Terheléselosztói mintavétel használt toomonitor hello load balancer készlet minden egyes virtuális gép van. Ha minden virtuális gép elérhetetlenné válik, akkor kimenő forgalomról nem útválasztásos toohello VM. |
| [az hálózati terheléselosztó szabály létrehozása](https://docs.microsoft.com/cli/azure/network/lb/rule#create) | Létrehoz egy terheléselosztó szabályhoz. Ez a példa létrejön egy szabály 80-as porton. HTTP-forgalom érkezik hello terheléselosztót, mert már irányított tooport 80 hello hello LB beállítása virtuális gépeinek egyikét. |
| [az hálózati terheléselosztó bejövő forgalmat kezelő nat-szabályok létrehozása](https://docs.microsoft.com/cli/azure/network/lb/inbound-nat-rule#create) | Terheléselosztó hálózati címfordítás (NAT) szabály hoz létre.  NAT-szabályok hello load balancer tooa port a virtuális gép port hozzárendelését. Ez a példa egy NAT-szabály jön létre SSH forgalom tooeach VM hello load balancer készletében.  |
| [az hálózati nsg létrehozása](https://docs.microsoft.com/cli/azure/network/nsg#create) | Hálózati biztonsági csoport (NSG), amely a biztonsági határ hello internet és hello virtuális gépek közötti hoz létre. |
| [az hálózati nsg-szabály létrehozása](https://docs.microsoft.com/cli/azure/network/nsg/rule#create) | Létrehoz egy NSG-szabály tooallow érkező bejövő forgalmat. Ez a példa 22-es portot SSH forgalom van megnyitva. |
| [az hálózati hálózati adapter létrehozása](https://docs.microsoft.com/cli/azure/network/nic#create) | A virtuális hálózati kártya létrehozza, és csatolja toohello virtuális hálózati alhálózat és NSG. |
| [az virtuális gép rendelkezésre állási-csoport létrehozása](https://docs.microsoft.com/cli/azure/network/lb/rule#create) | Létrehoz egy rendelkezésre állási csoportot. Rendelkezésre állási készletek elosztásával hello virtuális gépek a fizikai erőforrások között, úgy, hogy hiba esetén nem történik teljes készletét megtisztítja hello alkalmazás hasznos üzemidő biztosítása. |
| [az virtuális gép létrehozása](/cli/azure/vm#create) | Hello virtuális gépet hoz létre, és kapcsolódik, akkor toohello hálózati kártya, virtuális hálózatot, alhálózatot és NSG. Ez a parancs is meghatározza a használt hello virtuálisgép-lemezkép toobe és rendszergazdai hitelesítő adatait.  |
| [az csoport törlése](https://docs.microsoft.com/cli/azure/vm/extension#set) | Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése. |

## <a name="next-steps"></a>Következő lépések

Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).

További Azure-hálózat CLI parancsfájl minták hello található [Azure Networking dokumentáció](../cli-samples.md).
