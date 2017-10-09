---
title: "az affinitáscsoport tooa régióban (klasszikus) Azure virtuális hálózat aaaMigrate |} Microsoft Docs"
description: "Ismerje meg, hogyan toomigrate a virtuális hálózat (klasszikus) kapcsolatot csoportosítani tooa régióban."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 84febcb9-bb8b-4e79-ab91-865ad9de41cb
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
ms.openlocfilehash: e3a5c0f21e748912e29e2e8d03f4295720e63637
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-a-virtual-network-classic-from-an-affinity-group-tooa-region"></a>A virtuális hálózat (klasszikus) át egy affinitáscsoportba tooa terület

> [!IMPORTANT]
> Az Azure két különböző üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Ez a cikk hello klasszikus telepítési modell használatát bemutatja. A Microsoft azt javasolja, hogy az új telepítések esetén hello erőforrás-kezelő telepítési modellt használja.

Affinitáscsoportok győződjön meg arról, hogy erőforrásokat ugyanabban az affinitáscsoportban fizikailag közel vannak egymáshoz, ezen erőforrások gyorsabb toocommunicate engedélyezése-kiszolgálók által szolgáltatott hello jött létre. Az elmúlt hello a virtuális hálózatok (klasszikus) létrehozására vonatkozó követelmény volt affinitáscsoportokra. Ugyanakkor a hello hálózati kezelő szolgáltatás, amely kezeli a virtuális hálózatok (klasszikus) esetleg csak fog működni a fizikai kiszolgálók vagy a méretezési egység belül. Az architektúra fejlesztései hello hatókör hálózati felügyeleti tooa régió növekedett.

Architekturális fejlesztéseknek miatt affinitáscsoportok már nem ajánlott, vagy a virtuális hálózatok (klasszikus) szükséges. virtuális hálózatok (klasszikus) affinitáscsoportok hello használata régiók váltotta fel. Virtuális hálózatok (klasszikus) régiók társított regionális virtuális hálózatokba nevezzük.

Azt javasoljuk, hogy általában nem használ affinitáscsoportokat. Hello a virtuális hálózati követelményt, vezérelt affinitáscsoportok is fontos toouse tooensure erőforrások, például (klasszikus) számítási és tárolási (klasszikus), kerültek egymást. Azonban hello aktuális Azure hálózati architektúra elhelyezési követelménynek már nincs szükség.

> [!IMPORTANT]
> Bár továbbra is technikailag lehetséges toocreate egy virtuális hálózatot, amely egy affinitáscsoporthoz tartozik, nincs tehát nincs kényszerítő ok toodo. Számos virtuális hálózati szolgáltatás, például a hálózati biztonsági csoportok, csak elérhető regionális virtuális hálózat használatával, és nem érhetők el az affinitáscsoportok társított virtuális hálózatok.
> 
> 

## <a name="edit-hello-network-configuration-file"></a>Hello hálózati konfigurációs fájl szerkesztése

1. Hello hálózati konfigurációs fájl exportálása. Hogyan tooexport a hálózati konfigurációs fájlt a PowerShell használatával, vagy hello Azure parancssori felület (CLI) 1.0-s toolearn lásd [hálózati konfigurációs fájl használatával virtuális hálózat konfigurálása](virtual-networks-using-network-configuration-file.md#export).
2. Hello hálózati konfigurációs fájl szerkesztésével cseréje **AffinityGroup** rendelkező **hely**. Megad egy Azure [régió](https://azure.microsoft.com/regions) a **hely**.
   
   > [!NOTE]
   > Hello **hely** hello régió tartozik, amelyet a virtuális hálózat (klasszikus) társított hello affinitáscsoport. Például, ha a virtuális hálózat (klasszikus) társítva, amely áttelepítésekor, USA nyugati régiója található affinitáscsoport a **hely** tooWest USA kell mutatnia. 
   > 
   > 
   
    A következő sorokat a hálózati konfigurációs fájlban, hello értékeket cserélje le a saját hello szerkesztése: 
   
    **Régi érték:** \<VirtualNetworkSitename = "VNetUSWest" AffinityGroup = "VNetDemoAG"\> 
   
    **Új érték:** \<VirtualNetworkSitename = "VNetUSWest" Location "USA nyugati régiója" =\>
3. A módosítások mentéséhez és [importálása](virtual-networks-using-network-configuration-file.md#import) hálózati konfiguráció tooAzure hello.

> [!NOTE]
> Ez az áttelepítés nem okoz leállási tooyour szolgáltatások.
> 
> 

## <a name="what-toodo-if-you-have-a-vm-classic-in-an-affinity-group"></a>Milyen toodo, ha az affinitáscsoportokban lévő virtuális gép (klasszikus)
VMs (klasszikus), amelyek jelenleg az affinitáscsoportokban lévő hello affinitás csoportból eltávolított toobe nem szükséges. Miután a virtuális gép van telepítve, már telepített tooa egy méretezési egység. Affinitáscsoportok korlátozhatja az egy új virtuális gép üzembe helyezéséhez elérhető Virtuálisgép-méretek hello készletét, de bármely telepített meglévő virtuális gép már korlátozott toohello beállítása a virtuális gép méretének elérhető hello méretezési egység, mely hello virtuális gép van telepítve. Hello a virtuális gép már van telepített tooa méretezési egység, mert egy virtuális gép eltávolítása egy affinitáscsoporthoz nincs hatással van a virtuális gép hello.
