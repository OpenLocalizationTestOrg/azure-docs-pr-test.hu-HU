---
title: "aaaPrepare helyszíni VMware erőforrásokat az Azure Site Recovery replikációs tooAzure |} Microsoft Docs"
description: "Összefoglalja azokat a VMware virtuális gépek tooAzure tárolási munkaterheléseinek replikálásához kell hello lépéseket"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 6aba0e89-ad7c-467e-9db2-cfb3bfe4c7d6
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: 09d81f15f6ee764135a62f5555e458c55fa30048
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="step-6-prepare-on-premises-vmware-replication-tooazure"></a>6. lépés: Felkészülés a helyszíni VMware replikációs tooAzure

Ez a cikk tooprepare helyszíni VMware kiszolgálók toointeract az Azure Site Recovery használata hello utasításokat, és készítse elő a VMWare virtuális gépek hello mobilitási szolgáltatás telepítéséhez. hello mobilitási szolgáltatás ügynököt telepíteni kell az összes helyszíni virtuális gépeken, amelyet az tooreplicate tooAzure.

## <a name="prepare-for-automatic-discovery"></a>Automatikus felderítés előkészítése

A Site Recovery automatikusan észleli a vSphere ESXi-gazdagépek (függetlenül a vCenter-kiszolgáló) futó virtuális gépeket. Az automatikus felderítést, a Site Recovery kell egy fiók tooaccess gazdagépek és a kiszolgálók:

1. egy külön fiókot toouse (szinten hello vCenter hello az alábbi táblázatban ismertetett hello engedélyeivel szerepkör létrehozása. Nevezze el, például a **Azure_Site_Recovery**.
2. Ezután hello vSphere gazdagép/vCenter-kiszolgáló egy felhasználó létrehozása, és rendelje hozzá hello szerepkör toohello felhasználót. Ez a felhasználói fiók a Site Recovery üzembe helyezése során adja meg.


### <a name="vmware-account-permissions"></a>VMware fiókjához hozzárendelt jogosultságoktól

A Site Recovery hozzáférés tooVMware van szüksége, hello folyamat server tooautomatically virtuális gépek felderítése, valamint a feladatátvétel és a feladat-visszavételt a virtuális gépek.

- **Telepítse át**: csak toomigrate VMware virtuális gépek tooAzure, nélkül szeretné legalább egyszer sikertelen vissza őket, ha használható a VMware-fiók egy csak olvasható szerepkör. Ilyen szerepkör feladatátvételi futtatható, de nem lehet leállítani a védett forrásgépek. Ez nem szükséges az áttelepítéshez.
- **Replikálás/Recover**: Ha toodeploy (replikálja, feladatátvétel, feladat-visszavétel) teljes replikáció hello fióknak kell lennie a képes toorun műveletek, például a létrehozásával és lemezek eltávolításával, bekapcsolása a virtuális gépek stb.
- **Automatikus felderítés**: legalább egy csak olvasható fiókot kell megadni.


**Tevékenység** | **Szükséges fiók/szerepkör** | **Engedélyek** | **Részletek**
--- | --- | --- | ---
**A folyamatkiszolgáló automatikusan felderíti a VMware virtuális gépek** | Legalább egy írásvédett felhasználó van szüksége | Adatközpont objektum –> propagálása tooChild objektum szerepkör = csak olvasható | Felhasználó datacenter szinten hozzárendelt, és hozzáférést tooall hello objektumok hello adatközpontban.<br/><br/> toorestrict hozzáférés hozzárendelése hello **nem lehet hozzáférni** hello szerepkört **toochild propagálása** objektum toohello gyermekobjektum (vSphere-gazdagép, datastores, virtuális gépek és hálózatok).
**Feladatátvétel** | Legalább egy írásvédett felhasználó van szüksége | Adatközpont objektum –> propagálása tooChild objektum szerepkör = csak olvasható | Felhasználó datacenter szinten hozzárendelt, és hozzáférést tooall hello objektumok hello adatközpontban.<br/><br/> toorestrict hozzáférés hozzárendelése hello **nem lehet hozzáférni** hello szerepkört **toochild propagálása** toohello gyermekobjektum (vSphere-gazdagép, datastores, virtuális gépek és hálózatok) objektum.<br/><br/> Akkor hasznos, ha az áttelepítési célokból, de nem a teljes replikáció, feladatátvétel, feladat-visszavétel.
**A feladatátvételi és a feladat-visszavétel** | Javasoljuk, hogy (Azure_Site_Recovery) szerepkör létrehozása hello szükséges engedélyekkel, és hozzárendelheti a hello szerepkör tooa VMware felhasználó vagy csoport | Adatközpont objektum –> propagálása tooChild objektum szerepkör = Azure_Site_Recovery<br/><br/> Datastore foglaljon le terület ->, keresse meg az adattároló, alacsony szintű fájlműveletek, távolítsa el a fájlt, frissítse a virtuális géphez tartozó fájlokat<br/><br/> Hálózat -> hálózat hozzárendelése<br/><br/> Erőforrás -> rendelje hozzá a virtuális gép tooresource készlet, energiaforrással rendelkező ki a virtuális gép áttelepítése, energiaforrással rendelkező, a virtuális gép áttelepítése<br/><br/> Feladatok -> hozzon létre feladat, a frissítési feladat<br/><br/> Virtuális gép konfigurációja -><br/><br/> Virtuálisgép -> kommunikáció -> válasz kérdést, az eszköz kapcsolat, a CD media konfigurálásához, a konfigurálásához a hajlékonylemezes adathordozót, kapcsolja ki, a bekapcsolás, VMware-eszközök telepítése<br/><br/> Virtuálisgép -> leltár -> létrehozási, regisztrálása, unregister<br/><br/> Virtuálisgép -> kiépítés engedélyezése virtuális gép letöltési ->, hogy a virtuálisgép-fájlok feltöltése<br/><br/> Virtuális gép pillanatkép ->-Remove pillanatképek > | Felhasználó datacenter szinten hozzárendelt, és hozzáférést tooall hello objektumok hello adatközpontban.<br/><br/> toorestrict hozzáférés hozzárendelése hello **nem lehet hozzáférni** hello szerepkört **toochild propagálása** objektum toohello gyermekobjektum (vSphere-gazdagép, datastores, virtuális gépek és hálózatok).


## <a name="prepare-for-push-installation-of-hello-mobility-service"></a>Készítse elő a hello mobilitási szolgáltatás leküldéses telepítéséhez

kell telepíteni a mobilitási szolgáltatás hello tooreplicate kívánt összes virtuális gépet. Számos módon tooinstall hello szolgáltatás, így az ügyfélleküldéses telepítés hello Site Recovery folyamatkiszolgáló, és a telepítési módszerrel, például a System Center Configuration Manager manuális telepítésére.

Ha azt szeretné, hogy toouse ügyfélleküldéses telepítést, egy fiókot, hogy a Site Recovery használhatja-e a virtuális gép tooaccess hello tooprepare kell.

- A tartományi vagy helyi fiók használható
- A Windows Ha nem használ egy olyan tartományi fiók kell toodisable távelérési felhasználói vezérlő hello helyi számítógépen. Ez, hello regisztrálni a toodo **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System**, hello DWORD bejegyzés hozzáadása **LocalAccountTokenFilterPolicy**, 1 értékű.
- Ha a parancssori felület a Windows hello bejegyzés tooadd szeretne, írja be:``REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1.``
- Linux hello fióknak root forráskiszolgálón hello Linux kell lennie.



## <a name="next-steps"></a>Következő lépések

Nyissa meg túl[7. lépés: a tároló létrehozása](vmware-walkthrough-create-vault.md)
