---
title: "Azure-portálon aaaOpen portok tooa VM használatával hello |} Microsoft Docs"
description: "Megtudhatja, hogyan tooopen port / hozzon létre egy végpont tooyour VM a Windows hello resource manager üzembe helyezési modellben hello Azure portál használatával"
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: f7cf0319-5ee7-435e-8f94-c484bf5ee6f1
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 08/21/2017
ms.author: iainfou
ms.openlocfilehash: aba789c65254651899aa688f256fe616c3d0126d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooopen-ports-tooa-virtual-machine-with-hello-azure-portal"></a>Hogyan tooopen portok tooa virtuális gép hello Azure-portálon
[!INCLUDE [virtual-machines-common-nsg-quickstart](../../../includes/virtual-machines-common-nsg-quickstart.md)]

## <a name="quick-commands"></a>Gyors parancsok
Emellett [elvégzi ezeket a lépéseket az Azure PowerShell](nsg-quickstart-powershell.md).

Először hozzon létre a hálózati biztonsági csoport. Válasszon egy erőforráscsoportot hello portálon, válassza a **Hozzáadás**, majd keresse meg és jelölje ki **hálózati biztonsági csoport**:

![Hálózati biztonsági csoport hozzáadása](./media/nsg-quickstart-portal/add-nsg.png)

Adja meg a hálózati biztonsági csoport nevét, válassza ki vagy hozzon létre egy erőforráscsoportot, és jelöljön ki egy helyet. Válassza ki **létrehozása** befejezése:

![Hálózati biztonsági csoport létrehozása](./media/nsg-quickstart-portal/create-nsg.png)

Válassza ki az új hálózati biztonsági csoporthoz. Válassza a "Bejövő biztonsági szabályok", majd válassza ki a hello **Hozzáadás** gomb toocreate egy szabályt:

![Egy bejövő forgalomra vonatkozó szabály hozzáadása](./media/nsg-quickstart-portal/add-inbound-rule.png)

Válasszon egy közös **szolgáltatás** hello legördülő menüből, például a *HTTP*. Igény szerint kiválaszthatja *egyéni* tooprovide egy adott portot toouse. Szükség esetén módosítsa a hello prioritású vagy neve. hello prioritása meghatározza hello sorrendben, amelyben a szabályok érvényesek - hello alacsonyabb hello numerikus érték, hello korábbi hello-szabály alkalmazásakor. Igény szerint kiválaszthatja **speciális** a képernyő tooenter hello tetején egy adott forrás IP-blokk vagy port tartomány, például. Amikor elkészült, válassza ki a **OK** toocreate hello szabály:

![Egy bejövő forgalomra vonatkozó szabály létrehozása](./media/nsg-quickstart-portal/create-inbound-rule.png)

Az utolsó lépés a hálózati biztonsági csoport egy alhálózatot vagy egy adott hálózati illesztőn tooassociate. Hello hálózati biztonsági csoport most társítandó alhálózat. Válassza ki **alhálózatok**, majd válassza a **társítása**:

![Hálózati biztonsági csoport társítandó alhálózat](./media/nsg-quickstart-portal/associate-subnet.png)

Válassza ki a virtuális hálózatot, majd válassza ki a megfelelő alhálózati hello:

![Egy hálózati biztonsági csoportot társít a virtuális hálózat](./media/nsg-quickstart-portal/select-vnet-subnet.png)

Hálózati biztonsági csoport, egy bejövő szabályt, amely lehetővé teszi a forgalom 80-as porton, és azt egy alhálózathoz társított létre most hozott létre. A virtuális gépek toothat alhálózati csatlakozás 80-as porton érhetők el.

## <a name="more-information-on-network-security-groups"></a>További információ a hálózati biztonsági csoportok
hello itt gyors parancsok lehetővé teszik tooget be és a forgalom szereplő tooyour virtuális gép futtatása. Hálózati biztonsági csoportok adja meg, hány különleges szolgáltatásait és részletességgel ellenőrző tooyour erőforrások eléréséhez. További tudnivalók [itt szabályok létrehozása a hálózati biztonsági csoport és a hozzáférés-vezérlési lista](../../virtual-network/virtual-networks-create-nsg-arm-ps.md).

Magas rendelkezésre állású webes alkalmazásokhoz helyezze a virtuális gépek az Azure terheléselosztó mögött. hello terheléselosztó osztja el a forgalmat tooVMs forgalomszűrést végez biztosító hálózati biztonsági csoport. További információkért lásd: [hogyan tooload egyenleg Linux virtuális gépek az Azure toocreate a magas rendelkezésre állású alkalmazások](tutorial-load-balancer.md).

## <a name="next-steps"></a>Következő lépések
Ebben a példában létrehozott egy egyszerű szabályt tooallow HTTP-forgalmat. További részletes környezetek létrehozásáról a következő cikkek hello információt talál:

* [Az Azure Resource Manager áttekintése](../../azure-resource-manager/resource-group-overview.md)
* [Mi az a hálózati biztonsági csoport (NSG)?](../../virtual-network/virtual-networks-nsg.md)