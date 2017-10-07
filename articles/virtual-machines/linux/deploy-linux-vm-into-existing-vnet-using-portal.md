---
title: "Linux virtuális gépek aaaDeploy a meglévő hálózati Azure portállal |} Microsoft Docs"
description: "Linux virtuális gép üzembe helyezés hello portálon meglévő Azure virtuális hálózat."
services: virtual-machines-linux
documentationcenter: virtual-machines-linux
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: 51272b8cdb1dc7f3fe768d2ebbf4ebe0d754cf19
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toodeploy-a-linux-virtual-machine-into-an-existing-azure-virtual-network-with-hello-azure-portal"></a>Hogyan toodeploy hello Azure-portálon a meglévő Azure virtuális hálózat a Linux virtuális gépek

Ez a cikk bemutatja, hogyan toodeploy egy virtuális gép (VM) be egy meglévő virtuális hálózatot (VNet). Az Azure eszközök, például a virtuális hálózatokat és a hálózati biztonsági csoportok statikusnak kell lennie, és hosszabb élettartamú ritkán telepített erőforrások esetén. Miután a virtuális hálózaton van telepítve, azt is adhat meg újból állandó redeployments bármely kedvezőtlen hatással van a toohello infrastruktúra nélkül. Egy Vnetet, hogy egy hagyományos gondolat hardver hálózati kapcsoló - tooconfigure egyes központi telepítések kapcsoló egy új hardver nem kell.  

Egy megfelelően konfigurált virtuális hálózaton folytathatja toodeploy új kiszolgálók be, hogy a VNet és újra hello VNet hello élettartama során szükséges néhány, az esetleges módosításokat.

## <a name="create-hello-resource-group"></a>Hello erőforráscsoport létrehozása

Először hozzon létre egy erőforrás csoport tooorganize mindent hoz létre ebben a forgatókönyvben. Azure erőforráscsoport-sablonok kapcsolatos további információkért lásd: [Azure Resource Manager áttekintése](../../azure-resource-manager/resource-group-overview.md)

![createResourceGroup](./media/deploy-linux-vm-into-existing-vnet-using-portal/createResourceGroup.png)


## <a name="create-hello-vnet"></a>Hello virtuális hálózat létrehozása

A következő hozhat létre. a virtuális hálózat toolaunch hello a virtuális gépek. hello VNet egy alhálózat és társított hálózati biztonsági csoport hello Ez az alhálózat egy későbbi lépésben.

![createVNet](./media/deploy-linux-vm-into-existing-vnet-using-portal/createVNet.png)

## <a name="add-a-vnic-toohello-subnet"></a>Adjon hozzá egy virtuális hálózati toohello alhálózatot

Virtuális hálózati adapterek (VNics) fontosak, mivel toodifferent virtuális gépek is elérheti őket. Ez a megközelítés hello virtuális hálózati tartja a statikus erőforrásként hello virtuális gépek ideiglenes lehetnek. Hozzon létre egy virtuális hálózati, és társítsa azt az hello alhálózati hello előző lépésben létrehozott.

![createVNic](./media/deploy-linux-vm-into-existing-vnet-using-portal/createVNic.png)

## <a name="create-hello-network-security-group"></a>Hello hálózati biztonsági csoport létrehozása

Az Azure hálózati biztonsági csoportok állnak egyenértékű tooa tűzfal hello hálózati rétegben. Az Azure hálózati biztonsági csoportokkal kapcsolatos további információkért lásd: [Mi az a hálózati biztonsági csoport](../../virtual-network/virtual-networks-nsg.md).

![createNSG](./media/deploy-linux-vm-into-existing-vnet-using-portal/createNSG.png)

## <a name="add-an-inbound-ssh-allow-rule"></a>Egy bejövő SSH engedélyezési szabály hozzáadása

virtuális gép hello hozzá kell férnie a hello internet, így lehetővé téve a 22-es portot a bejövő forgalom toobe szabály továbbítja hello hálózati tooport 22 hello virtuális gép létrehozása.

![createInboundSSH](./media/deploy-linux-vm-into-existing-vnet-using-portal/createInboundSSH.png)

## <a name="associate-hello-nsg-with-hello-subnet"></a>Hello alhálózati hello NSG társítása

Hello virtuális hálózat és a létrehozott hello alhálózati hello alhálózati hello hálózati biztonsági csoport társítani. Lehet, hogy a hálózati biztonsági csoportok vagy egy teljes alhálózatot, vagy egy különálló virtuális hálózati társítani. Hello tűzfal forgalom szűrésénél hello alhálózati szinten minden VNics és hello alhálózaton belüli virtuális gépek hello által védett hello hálózati biztonsági csoport. hello más megközelítést van hello hálózati biztonsági csoport alatt csak egyetlen virtuális hálózati társított, és csak egy virtuális gép védelmét.

![associateNSG](./media/deploy-linux-vm-into-existing-vnet-using-portal/associateNSG.png)


## <a name="deploy-hello-vm-into-hello-vnet-and-nsg"></a>Hello virtuális hálózatot a virtuális gép hello és NSG központi telepítése

Hello Azure-portálon, Linux virtuális gép hello használata meglévő Azure-erőforráscsoportot, virtuális hálózatot, alhálózatot és virtuális hálózati telepített toohello.

![createVM](./media/deploy-linux-vm-into-existing-vnet-using-portal/createVM.png)

Hello portál toochoose meglévő erőforrásokat használ, kérje a Azure toodeploy hello VM hello meglévő hálózaton belül. Egy VNet és alhálózat van telepítve, ha azok statikus vagy állandó erőforráshoz, az Azure-régió, mint maradhatnak.  

## <a name="next-steps"></a>Következő lépések

* [Az Azure Resource Manager sablon toocreate egy adott központi telepítés használata](../windows/cli-deploy-templates.md)
* [Saját egyéni környezet létrehozása Linux virtuális gép számára közvetlenül Azure parancssori felület parancsait használva](create-cli-complete.md)
* [Linux virtuális gép létrehozása az Azure-ban sablonok használatával](create-ssh-secured-vm-from-template.md)
