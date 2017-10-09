---
title: "aaaAzure CLI parancsfájl minta - terhelésének elosztása több webhely, az Azure CLI hello |} Microsoft Docs"
description: "Az Azure CLI parancsfájl minta - terhelésének elosztása több webhelyek toohello ugyanahhoz a virtuális géphez"
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
ms.openlocfilehash: 136da5d1783fb9f9dc87f1ffad8eec7b95c6bd7b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="load-balance-multiple-websites"></a>Terhelésének elosztása több webhely

Parancsfájl a következő példában két virtuális gépekkel (VM), amelyek egy rendelkezésre állási csoport tagjai hoz létre egy virtuális hálózatot. A terheléselosztó irányítja a forgalmat a két külön IP-címek toohello két virtuális gépeket. Hello a parancsfájl futtatását, miután sikerült web server szoftver toohello virtuális gépek telepítése, és mindegyiket a saját IP-cím több webhely üzemeltetésére.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Mintaparancsfájl


[!code-azurecli-interactive[main](../../../cli_scripts/load-balancer/load-balance-multiple-web-sites-vm/load-balance-multiple-web-sites-vm.sh  "Load balance multiple web sites")]

## <a name="clean-up-deployment"></a>Az üzemelő példány eltávolítása 

Futtassa a következő parancs tooremove hello erőforráscsoport, virtuális gép és minden kapcsolódó erőforrások hello.

```azurecli
az group delete --name myResourceGroup --yes
```

## <a name="script-explanation"></a>Parancsfájl ismertetése

A parancsfájl a következő parancsok toocreate egy erőforráscsoport, virtuális hálózati terheléselosztó, és minden kapcsolódó erőforrások betöltése hello. Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.

| Parancs | Megjegyzések |
|---|---|
| [az csoport létrehozása](https://docs.microsoft.com/cli/azure/group#create) | Az összes erőforrás tároló erőforrás csoportot hoz létre. |
| [az hálózati virtuális hálózat létrehozása](https://docs.microsoft.com/cli/azure/network/vnet#create) | Létrehoz egy Azure-beli virtuális hálózat és az alhálózatot. |
| [az hálózati nyilvános ip-létrehozása](https://docs.microsoft.com/cli/azure/network/public-ip#create) | Hoz létre egy nyilvános IP-cím egy statikus IP-címet és egy társított DNS-névvel. |
| [az hálózati terheléselosztó létrehozása](https://docs.microsoft.com/cli/azure/network/lb#create) | Egy Azure terheléselosztót hoz létre. |
| [az hálózati lb mintavétel létrehozása](https://docs.microsoft.com/cli/azure/network/lb/probe#create) | Terheléselosztói mintavétel létrehozása. Terheléselosztói mintavétel használt toomonitor hello load balancer készlet minden egyes virtuális gép van. Ha minden virtuális gép elérhetetlenné válik, akkor kimenő forgalomról nem útválasztásos toohello VM. |
| [az hálózati terheléselosztó szabály létrehozása](https://docs.microsoft.com/cli/azure/network/lb/rule#create) | Létrehoz egy terheléselosztó szabályhoz. Ez a példa létrejön egy szabály 80-as porton. HTTP-forgalom érkezik hello terheléselosztót, mert már irányított tooport 80 hello load balancer set hello virtuális gépeinek egyikét. |
| [az előtér-IP-hálózati terheléselosztó létrehozása](https://docs.microsoft.com/cli/azure/network/lb/frontend-ip#create) | Hozzon létre egy hello terheléselosztó előtérbeli IP-címet. |
| [az hálózati lb-címkészlet létrehozása](https://docs.microsoft.com/cli/azure/network/lb/address-pool#create) | Létrehoz egy háttér címkészletet. |
| [az hálózati hálózati adapter létrehozása](https://docs.microsoft.com/cli/azure/network/nic#create) | A virtuális hálózati kártya létrehozza, és csatolja toohello virtuális hálózati és alhálózati. |
| [az virtuális gép rendelkezésre állási-csoport létrehozása](https://docs.microsoft.com/cli/azure/network/lb/rule#create) | Létrehoz egy rendelkezésre állási csoportot. Rendelkezésre állási készletek elosztásával hello virtuális gépek a fizikai erőforrások között, úgy, hogy hiba esetén nem történik teljes készletét megtisztítja hello alkalmazás hasznos üzemidő biztosítása. |
| [az hálózati hálózati adapter ip-konfiguráció létrehozása](https://docs.microsoft.com/cli/azure/network/nic/ip-config#create) | Létrehoz egy IP-confiuration. Hello Microsoft.Network/AllowMultipleIpConfigurationsPerNic szolgáltatás engedélyezve van, az előfizetéssel kell rendelkeznie. Csak egy konfigurációs hello elsődleges IP-konfiguráció / NIC hello--ellenőrizze elsődleges jelző használatával ki lehet. |
| [az virtuális gép létrehozása](https://docs.microsoft.com/cli/azure/vm/availability-set#create) | Hello virtuális gépet hoz létre, és kapcsolódik, akkor toohello hálózati kártya, virtuális hálózatot, alhálózatot és NSG. Ez a parancs is meghatározza a használt hello virtuálisgép-lemezkép toobe és rendszergazdai hitelesítő adatait.  |
| [az csoport törlése](https://docs.microsoft.com/cli/azure/vm/extension#set) | Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése. |

## <a name="next-steps"></a>Következő lépések

Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).

További hálózati CLI parancsfájl minták hello található [Azure hálózati áttekintés dokumentáció](../cli-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).
