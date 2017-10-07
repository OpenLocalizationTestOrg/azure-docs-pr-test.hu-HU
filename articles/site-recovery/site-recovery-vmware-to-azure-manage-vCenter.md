---
title: " Az Azure Site Recovery VMware vCenter-kiszolgáló kezelése |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan hozzáadása és kezelése az Azure Site Recovery a VMware vcenter programban."
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
ms.workload: backup-recovery
ms.date: 06/29/2017
ms.author: anoopkv
ms.openlocfilehash: 5be995f137d0c0efaf3050b5366a107098cae15a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-vmware-vcenter-server-in-azure-site-recovery"></a>VMware vCenter Server az Azure Site Recovery kezelése
Ez a cikk ismerteti, amelyek hello a VMware vCenter végrehajtható különböző Site Recovery-műveleteket.

## <a name="prerequisites"></a>Előfeltételek

**Támogatja a VMware vCenter és az ESX-gazdagépen VMware vSphere** | **Részletek** |
|--- | --- |
|**Helyszíni VMware-kiszolgálók** | Egy vagy több VMware vSphere, futtató kiszolgálók 6.0, 5.5, 5.1 legújabb frissítéseit. Kiszolgálók hello azonos hálózati hello konfigurációs kiszolgáló (vagy különálló folyamatkiszolgálót) kell elhelyezni.<br/><br/> Javasoljuk a vCenter server toomanage állomások, 6.0 vagy 5.5 rendszerű hello legújabb frissítéseit. Csak azok a szolgáltatások által biztosított 5.5 6.0-s verzió telepítésekor támogatottak.|

## <a name="prepare-an-account-for-automatic-discovery"></a>Az automatikus felderítési fiók előkészítése
A Site Recovery hozzáférés tooVMware van szüksége, a virtuális gépek észlelése hello folyamat server tooautomatically, és a feladatátvétel és a feladat-visszavételt a virtuális gépek.

* **Telepítse át**: csak toomigrate VMware virtuális gépek tooAzure nélkül szeretné legalább egyszer sikertelen vissza őket, ha használható a VMware-fiók egy csak olvasható szerepkör. Ilyen szerepkör feladatátvételi futtatható, de nem lehet leállítani a védett forrásgépek. Erre nincs szükség áttelepítésre.
* **Replikálás/Recover**: Ha toodeploy (replikálja, feladatátvétel, feladat-visszavétel) teljes replikáció hello fióknak kell lennie a képes toorun műveletek, például a létrehozásával és lemezek eltávolításával, a virtuális gép bekapcsolása.
* **Automatikus felderítés**: legalább egy csak olvasható fiókot kell megadni.


|**Feladatok** | **Szükséges fiók/szerepkör** | **Engedélyek** | **Részletek**|
|--- | --- | --- | ---|
|**A folyamatkiszolgáló automatikusan felderíti a VMware virtuális gépek** | Legalább egy írásvédett felhasználó van szüksége | Adatközpont objektum –> propagálása tooChild objektum szerepkör = csak olvasható | Felhasználó datacenter szinten hozzárendelt, és hozzáférést tooall hello objektumok hello adatközpontban.<br/><br/> toorestrict hozzáférés hozzárendelése hello **nem lehet hozzáférni** hello szerepkört **toochild propagálása** objektum, toohello gyermekobjektum (vSphere-gazdagép, datastores, virtuális gépek és hálózatok).|
|**Feladatátvétel** | Legalább egy írásvédett felhasználó van szüksége | Adatközpont objektum –> propagálása tooChild objektum szerepkör = csak olvasható | Felhasználó datacenter szinten hozzárendelt, és hozzáférést tooall hello objektumok hello adatközpontban.<br/><br/> toorestrict hozzáférés hozzárendelése hello **nem lehet hozzáférni** hello szerepkört **toochild propagálása** toohello gyermekobjektum (vSphere-gazdagép, datastores, virtuális gépek és hálózatok) objektum.<br/><br/> Akkor hasznos, ha az áttelepítési célokból, de nem a teljes replikáció, feladatátvétel, feladat-visszavétel.|
|**A feladatátvételi és a feladat-visszavétel** | Javasoljuk, hogy (AzureSiteRecoveryRole) szerepkör létrehozása hello szükséges engedélyekkel, és hozzárendelheti a hello szerepkör tooa VMware felhasználó vagy csoport | Adatközpont objektum –> propagálása tooChild objektum szerepkör = AzureSiteRecoveryRole<br/><br/> Datastore foglaljon le terület ->, keresse meg az adattároló, alacsony szintű fájlműveletek, távolítsa el a fájlt, frissítse a virtuális géphez tartozó fájlokat<br/><br/> Hálózat -> hálózat hozzárendelése<br/><br/> Erőforrás -> rendelje hozzá a virtuális gép tooresource készlet, energiaforrással rendelkező ki a virtuális gép áttelepítése, energiaforrással rendelkező, a virtuális gép áttelepítése<br/><br/> Feladatok -> hozzon létre feladat, a frissítési feladat<br/><br/> Virtuális gép konfigurációja -><br/><br/> Virtuálisgép -> kommunikáció -> válasz kérdést, az eszköz kapcsolat, a CD media konfigurálásához, a konfigurálásához a hajlékonylemezes adathordozót, kapcsolja ki, a bekapcsolás, VMware-eszközök telepítése<br/><br/> Virtuálisgép -> leltár -> létrehozási, regisztrálása, unregister<br/><br/> Virtuálisgép -> kiépítés engedélyezése virtuális gép letöltési ->, hogy a virtuálisgép-fájlok feltöltése<br/><br/> Virtuális gép pillanatkép ->-Remove pillanatképek > | Felhasználó datacenter szinten hozzárendelt, és hozzáférést tooall hello objektumok hello adatközpontban.<br/><br/> toorestrict hozzáférés hozzárendelése hello **nem lehet hozzáférni** hello szerepkört **toochild propagálása** objektum, toohello gyermekobjektum (vSphere-gazdagép, datastores, virtuális gépek és hálózatok).|

## <a name="create-an-account-tooconnect-toovmware-vcenter-server-vmware-vsphere-exsi-host"></a>Hozzon létre egy fiókot tooconnect tooVMware vCenter-kiszolgáló / VMware vSphere EXSi állomás
1. Bejelentkezés a hello konfigurációs kiszolgáló, és indítsa el hello cspsconfigtool.exe használatával hello helyi asztali hello helyezni.
2. Kattintson a **fiók hozzáadása** a hello **fiók kezelése** fülre.

  ![fiók hozzáadása](./media/site-recovery-vmware-to-azure-manage-vcenter/addaccount.png)
3. Adjon meg hello fiókadatok, majd kattintson az OK tooadd hello fiók. hello fióknak hello felsorolt hello jogosultsággal kell rendelkeznie [fiók előkészítése automatikus felderítés](#prepare-an-account-for-automatic-discovery) szakasz.

  >[!NOTE]
  Hello fiók információk toobe hello Site Recovery szolgáltatás szinkronizálta körülbelül 15 percet vesz igénybe.


## <a name="associate-a-vmware-vcenter-vmware-vsphere-esx-host-add-vcenter"></a>Társítsa a VMware vCenter / VMware vSphere ESX-gazdagép (Hozzáadás vCenter)
* Az Azure-portálon hello, keresse meg a túl*YourRecoveryServicesVault* > **Site Recovery-infrastruktúra** > **konfigurációs kiszolgálója**  >  *ConfigurationServer*
* A hello konfigurációs kiszolgáló részleteit megjelenítő oldalon kattintson a hello + vCenter gombra.

[!INCLUDE [site-recovery-add-vcenter](../../includes/site-recovery-add-vcenter.md)]

## <a name="modify-credentials-used-tooconnect-toohello-vcenter-server-vsphere-esxi-host"></a>Módosíthatja a használt hitelesítő adatok tooconnect toohello vCenter-kiszolgáló vagy vSphere ESXi-állomáson

1. Bejelentkezés a hello konfigurációs kiszolgáló, és indítsa el hello cspsconfigtool.exe
2. Kattintson a **fiók hozzáadása** a hello **fiók kezelése** fülre.

  ![fiók hozzáadása](./media/site-recovery-vmware-to-azure-manage-vcenter/addaccount.png)
3. Adjon meg hello új fiók adatait, majd kattintson az OK tooadd hello fiók. hello fióknak hello felsorolt hello jogosultsággal kell rendelkeznie [fiók előkészítése automatikus felderítés](#prepare-an-account-for-automatic-discovery) szakasz.
4. Az Azure-portálon hello, keresse meg a túl*YourRecoveryServicesVault* > **Site Recovery-infrastruktúra** > **konfigurációs kiszolgálója**  >  *ConfigurationServer*
5. A hello konfigurációs kiszolgáló részleteit megjelenítő oldalon kattintson a hello **kiszolgáló frissítését** gombra.
6. Miután hello frissítési kiszolgáló feladat befejeződött, válassza ki a hello vCenter Server tooopen hello vCenter összefoglalás lapon.
7. Jelölje be hello újonnan hozzáadott fiók hello **vCenter kiszolgáló vagy vSphere-gazdagép fiókja** mezőben, majd kattintson a hello **mentése** gombra.

  ![-fiók módosítása](./media/site-recovery-vmware-to-azure-manage-vcenter/modify-vcente-creds.png)

## <a name="delete-a-vcenter-in-azure-site-recovery"></a>Az Azure Site Recovery vCenter törlése
1. Az Azure-portálon hello, keresse meg a túl*YourRecoveryServicesVault* > **Site Recovery-infrastruktúra** > **konfigurációs kiszolgálója**  >  *ConfigurationServer*
2. Válassza ki hello vCenter Server tooopen hello vCenter összefoglalás lapon hello konfigurációs kiszolgáló részletei lapon.
3. Kattintson a hello **törlése** gomb toodelete hello vCenter

  ![törlés-fiók](./media/site-recovery-vmware-to-azure-manage-vcenter/delete-vcenter.png)

> [!NOTE]
Ha toomodify hello Vcenter IP-cím vagy teljes tartománynév kell, akkor kapcsolóport részletei toodelete hello vCenter-kiszolgáló szükséges, és vissza újra hozzáadni.
