---
title: " Az Azure (erőforrás-kezelő) folyamat kiszolgáló kezeléséhez |} Microsoft Docs"
description: "Ez a cikk ismerteti a hogyan tooset be egy feladat-visszavétel feldolgozni a kiszolgáló (Resource Manager) az Azure-ban."
services: site-recovery
documentationcenter: 
author: AnoopVasudavan
manager: gauravd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/29/2017
ms.author: anoopkv
ms.openlocfilehash: 70426b96095cc42befff6c4114fb56536284a667
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-a-process-server-running-in-azure-resource-manager"></a>(Erőforrás-kezelő) Azure-ban futó folyamat kiszolgáló kezelése
> [!div class="op_single_selector"]
> * [Resource Manager](./site-recovery-vmware-setup-azure-ps-resource-manager.md)
> * [Klasszikus](./site-recovery-vmware-setup-azure-ps-classic.md)

A feladat-visszavétel során nagy késleltetésű hello Azure virtuális hálózat és a helyszíni hálózat között esetén ajánlott toodeploy folyamatkiszolgáló az Azure-ban. Ez a cikk ismerteti, hogyan beállításához, konfigurálásához és hello folyamat kiszolgálók Azure-beli kezeléséhez.

> [!NOTE]
> Ez a cikk használható, ha a használt toobe **erőforrás-kezelő** hello telepítési modell hello virtuális gépek a feladatátvétel során. Ha követte **klasszikus** hello telepítési modell, kövesse a lépéseket hello [hogyan tooset akár & a feladat-visszavételi folyamatkiszolgáló (klasszikus) konfigurálása](./site-recovery-vmware-setup-azure-ps-classic.md)

## <a name="prerequisites"></a>Előfeltételek

[!INCLUDE [site-recovery-vmware-process-server-prerequ](../../includes/site-recovery-vmware-azure-process-server-prereq.md)]

## <a name="deploy-a-process-server-on-azure"></a>Az Azure-on folyamat-kiszolgáló központi telepítése
1. A tároló hello > **Site Recovery-infrastruktúra** (a hello "Kezelése" fejléc) > **konfigurációs kiszolgálók** (a "A VMware és fizikai gépek" fejléc), válassza a hello konfigurációs kiszolgáló.
2. Hello konfigurációs kiszolgáló részleteit megjelenítő oldalon megnyíló kattintson "+ feldolgozni a kiszolgáló"

  ![Folyamat server gyűjtemény hozzáadása](./media/site-recovery-vmware-setup-azure-ps-arm/add-ps.png)

3.  A hello **Hozzáadás folyamatkiszolgáló** lapon, jelölje be a következő értékek hello

  ![Adja hozzá a folyamatkiszolgáló katalóguseleméhez](./media/site-recovery-vmware-setup-azure-ps-arm/add-ps-page-1.png)
|**Mező neve**|**Érték**|
|-|-|
|Adja meg, ahol toodeploy a folyamatkiszolgálót|Adja meg a hello értéket **az Azure-ban feladat-visszavételi folyamatkiszolgáló telepítése** |
|Előfizetés|Ha feladatátvételt hello virtuális gépek Azure-előfizetés hello kiválasztása|
|Erőforráscsoport|Hozzon létre egy erőforráscsoportot toodeploy a folyamatkiszolgáló, vagy válasszon egy meglévő erőforráscsoportot toodeploy hello folyamatkiszolgáló|
|Hely|Válassza ki a hello Azure-adatközpont hello virtuális gépek az adott történő feladatátvételt|
|Azure Network-hálózat|Jelölje be hello Azure virtuális gépek hello virtuális Network(VNet) Ha a feladatátvételt. Ha több Azure Vnetekhez történő sikertelen a virtuális gépeket, majd van szüksége a folyamatkiszolgáló telepítése egy virtuális hálózat|

4. Töltse ki hello többi hello folyamatkiszolgáló hello tulajdonságai

  ![Összegzési folyamat-kiszolgáló hozzáadása](./media/site-recovery-vmware-setup-azure-ps-arm/add-ps-page-2.png)
|**Mező neve**|**Érték**|
|-|-|
|Kiszolgáló neve|Megjelenített név és a folyamat kiszolgáló virtuális gép állomásnevét|
| Felhasználónév|A felhasználónevet, amely a virtuális gépen a rendszergazda válik|
|Tárfiók|Hello Tárfiókot, ahol kerülnek, hello virtuális gép virtuális lemez neve|
|Alhálózat|hello alhálózati hello Azure VNet toowhich hello virtuális gép csatlakozik|
| IP-cím|IP-címet, hogy milyen hello folyamat server tooassume után elinduló|
5. Kattintson a hello OK gomb toostart hello folyamat kiszolgáló virtuális gép telepítését.

> [!NOTE]
> toobe képes toouse a folyamatkiszolgálót a feladat-visszavétel tooregister kell azt hello helyszíni konfigurációs kiszolgáló.

## <a name="registering-hello-process-server-running-in-azure-tooa-configuration-server-running-on-premises"></a>Hello folyamat (az Azure-ban futtató) tooa konfigurációs kiszolgáló (a helyszínen fut) regisztrálása

[!INCLUDE [site-recovery-vmware-register-process-server](../../includes/site-recovery-vmware-register-process-server.md)]

## <a name="upgrading-hello-process-server-toolatest-version"></a>Hello folyamat server toolatest verziójának frissítése.

[!INCLUDE [site-recovery-vmware-upgrade-process-server](../../includes/site-recovery-vmware-upgrade-process-server.md)]

## <a name="unregistering-hello-process-server-running-in-azure-from-a-configuration-server-running-on-premises"></a>Beállításjegyzékből való törlésekor (Azure-beli) hello folyamatkiszolgáló a konfigurációs kiszolgálóról (a helyszínen fut)

[!INCLUDE [site-recovery-vmware-unregister-process-server](../../includes/site-recovery-vmware-unregister-process-server.md)]
