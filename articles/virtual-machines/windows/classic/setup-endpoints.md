---
title: "végpontok a klasszikus windowsos virtuális gép mentése aaaSet |} Microsoft Docs"
description: "Ismerje meg, tooset végpont mentése a Windows virtuális gépek a hello Azure klasszikus portál tooallow kommunikáció a Windows Azure virtuális gép."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 8afc21c2-d3fb-43a3-acce-aa06be448bb6
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: cynthn
ms.openlocfilehash: e817076f16d3a245a8d19add7b2f2cf5e3baa17e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooset-up-endpoints-on-a-classic-windows-virtual-machine-in-azure"></a>Hogyan tooset be a klasszikus Windows rendszerű virtuális gép az Azure-végpontok
Az Azure-ban hello klasszikus üzembe helyezési modellel létrehozott virtuális gépek automatikusan kommunikálhatnak más virtuális gépekkel a magánhálózat-csatornán keresztül az összes Windows hello ugyanazt a felhőalapú szolgáltatás, vagy a virtuális hálózat. Hello Internet vagy egyéb virtuális hálózaton lévő számítógépek azonban végpontok toodirect hello bejövő hálózati forgalom tooa virtuális gép szükséges. Ez a cikk érhető el is [Linux virtuális gépek](../../linux/classic/setup-endpoints.md).

> [!IMPORTANT]
> Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../resource-manager-deployment-model.md). Ez a cikk hello klasszikus telepítési modell használatát bemutatja. A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja.

A hello **erőforrás-kezelő** üzembe helyezési modelljével végpontok használatával konfigurálhatók **hálózati biztonsági csoportokkal (NSG-k)**. További információkért lásd: [engedélyezése külső hozzáférés tooyour VM használatával hello Azure-portálon](../nsg-quickstart-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Hello Azure-portálon, a Windows rendszerű virtuális gép létrehozásakor közös végpontok, például a távoli asztal és a Windows PowerShell távoli eljáráshívás általában meg automatikusan létrejönnek. További végpontok hello virtuális gép létrehozása során vagy azt követően szükség szerint konfigurálhatja.

[!INCLUDE [virtual-machines-common-classic-setup-endpoints](../../../../includes/virtual-machines-common-classic-setup-endpoints.md)]

## <a name="next-steps"></a>Következő lépések
* Lásd az Azure PowerShell-parancsmag tooset be egy virtuális gép végpontjának toouse [Add-AzureEndpoint](https://msdn.microsoft.com/library/azure/dn495300.aspx).
* Lásd az Azure PowerShell-parancsmag toomanage egy végponti ACL toouse [kezelése hozzáférés listák (ACL) végpontok PowerShell használatával](../../../virtual-network/virtual-networks-acl-powershell.md).
* Ha egy virtuális gép hello Resource Manager üzembe helyezési modellel létrehozott, használhatja az Azure PowerShell túl[hálózati biztonsági csoportok létrehozása](../../../virtual-network/virtual-networks-create-nsg-arm-ps.md) toocontrol forgalom toohello virtuális gép.
