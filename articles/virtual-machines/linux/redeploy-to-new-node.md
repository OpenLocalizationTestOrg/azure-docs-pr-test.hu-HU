---
title: "Linux virtuális gépek Azure-ban aaaRedeploy |} Microsoft Docs"
description: "Hogyan tooredeploy Linux virtuális gépek Azure toomitigate SSH-kapcsolat ad ki."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
tags: azure-resource-manager,top-support-issue
ms.assetid: e9530dd6-f5b0-4160-b36b-d75151d99eb7
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: support-article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/23/2017
ms.author: iainfou
ms.openlocfilehash: 9adfd1b11f262d362133366b2bba5e69c70c9b82
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="redeploy-linux-virtual-machine-toonew-azure-node"></a>Telepítse újra a Linux virtuális gép toonew Azure csomópont
Ha Ön szembesülhetnek SSH hibaelhárítási problémák vagy alkalmazás tooa Linux virtuális gép (VM) elérni az Azure-ban, újratelepíteni a virtuális gép hello segíthet. A virtuális gép újbóli telepítésének, hello VM tooa új csomópont belül hello Azure-infrastruktúra helyezi át, és majd bekapcsolja azt vissza. A konfigurációs beállításokat és a kapcsolódó erőforrások jelennek meg. Ez a cikk bemutatja, hogyan tooredeploy a virtuális gépet az Azure CLI vagy hello Azure-portálon.

> [!NOTE]
> Miután a virtuális gép újbóli telepítésének, hello mennyiségű ideiglenes lemezes elvesztését, és dinamikus IP-címek társított virtuális hálózati illesztő frissítése. 

A virtuális gépek hello a következő lehetőségek egyikének használatával központilag telepítheti. Csak kell toochoose egy beállítás tooredeploy a virtuális Gépet:

- [Azure CLI 2.0](#azure-cli-20)
- [Azure CLI 1.0](#azure-cli-10)
- [Azure Portal](#using-azure-portal)

## <a name="use-hello-azure-cli-20"></a>Azure CLI 2.0 hello használata
Legutóbbi telepítés hello [Azure CLI 2.0](/cli/azure/install-az-cli2) tooan Azure-fiók használatával jelentkezzen [az bejelentkezési](/cli/azure/#login).

Telepítse újra a virtuális Gépet a [az vm helyezze üzembe újra](/cli/azure/vm#redeploy). a következő példa redeploys hello hello nevű virtuális gép *myVM* nevű hello erőforráscsoportban *myResourceGroup*:

```azurecli
az vm redeploy --resource-group myResourceGroup --name myVM 
```

## <a name="use-hello-azure-cli-10"></a>Hello Azure CLI 1.0 használata
Hello telepítése [Azure CLI legújabb 1.0](../../cli-install-nodejs.md), jelentkezzen be Azure-fiók tooan, és győződjön meg arról, hogy éppen erőforrás-kezelő módban (`azure config mode arm`).

a következő példa redeploys hello hello nevű virtuális gép *myVM* nevű hello erőforráscsoportban *myResourceGroup*:

```azurecli
azure vm redeploy --resource-group myResourceGroup --vm-name myVM 
```

[!INCLUDE [virtual-machines-common-redeploy-to-new-node](../../../includes/virtual-machines-common-redeploy-to-new-node.md)]

## <a name="next-steps"></a>Következő lépések
Csatlakozás a virtuális gép tooyour problémát tapasztal, ha található segítséget [SSH-kapcsolatok hibáinak elhárítása](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) vagy [részletes hibaelhárítási SSH](detailed-troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Ha nem fér hozzá a virtuális gép futó alkalmazást, is olvasható [problémák elhárítása alkalmazás](troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

