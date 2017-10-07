---
title: "a VMM és a Hyper-V replikáció tooa másodlagos hely az Azure Site Recovery aaaSet |} Microsoft Docs"
description: "Ismerteti, hogyan tooset System Center VMM-kiszolgálók és a Hyper-V gazdagépek replikációs tooa másodlagos VMM-hely."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: d0389e3b-3737-496c-bda6-77152264dd98
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/30/2017
ms.author: raynew
ms.openlocfilehash: 677bf6d38328ccc425e3b0f056d03159a52da428
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="step-4-set-up-vmm-and-hyper-v-for-hyper-v-vm-replication-tooa-secondary-site"></a>4. lépés: Állítsa be a VMM és a Hyper-V Hyper-V virtuális gép replikációs tooa másodlagos hely 

Után előkészítése után a hálózatkezeléshez, a System Center Virtual Machine Manager (VMM) kiszolgáló és a Hyper-V gazdagépeken a Hyper-V virtuális gép (VM) replikációs tooa másodlagos helyhez beállítása használatával [Azure Site Recovery](site-recovery-overview.md) a hello Azure-portálon. 

A cikk elolvasása után fűzhetnek bármely hello lap alján, vagy a hello [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).



## <a name="prepare-vmm-servers"></a>VMM-kiszolgáló előkészítése 

központi telepítés tooprepare:


1. Győződjön meg arról, hogy a VMM-kiszolgálók megfelelnek a hello [igényeinek támogatására](site-recovery-support-matrix-to-sec-site.md#on-premises-servers), és [telepítésének előfeltételei](vmm-to-vmm-walkthrough-prerequisites.md).
2. Gondoskodjon arról, hogy a VMM-kiszolgálók csatlakoztatott toohello internet access toothese URL-címek pedig.
    
    [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]
    
    - Ha IP-címeken alapuló tűzfalszabályok szabály, győződjön meg arról, kommunikációs tooAzure lehetővé teszik.
    - Hello engedélyezése [Azure Datacenter IP-címtartományok](https://www.microsoft.com/download/confirmation.aspx?id=41653), és a HTTPS (443) port hello.
    - Lehetővé teszi az IP-címtartományok hello Azure-régió, az előfizetés, valamint az USA nyugati (hozzáférés-vezérlés és az Identitáskezeléshez használt).
3. Győződjön meg arról, hogy a VMM-kiszolgáló hello [előkészítve a hálózatra való leképezés](vmm-to-vmm-walkthrough-network.md#prepare-for-network-mapping)


## <a name="prepare-hyper-v-hostsclusters"></a>Hyper-V gazdagép vagy fürt előkészítése

1. Győződjön meg arról, hogy a Hyper-V gazdagép vagy fürt ahhoz hello [igényeinek támogatására](site-recovery-support-matrix-to-sec-site.md#on-premises-servers), és [telepítésének előfeltételei](vmm-to-vmm-walkthrough-prerequisites.md).
2. Ellenőrizze a hello követelményei [Hyper-V virtuális gépek](site-recovery-support-matrix-to-sec-site.md#support-for-replicated-machine-os-versions)
3. Győződjön meg arról [hálózati](site-recovery-support-matrix-to-sec-site.md#network-configuration) és [tárolási](site-recovery-support-matrix-to-sec-site.md#storage) követelményeinek.
4. Gondoskodjon arról, hogy a Hyper-V-gazdagépek csatlakoztatott toohello internet access toothese URL-címek pedig.
    
    [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]
    
    - Ha IP-címeken alapuló tűzfalszabályok szabály, győződjön meg arról, kommunikációs tooAzure lehetővé teszik.
    - Hello engedélyezése [Azure Datacenter IP-címtartományok](https://www.microsoft.com/download/confirmation.aspx?id=41653), és a HTTPS (443) port hello.
    - Lehetővé teszi az IP-címtartományok hello Azure-régió, az előfizetés, valamint az USA nyugati (hozzáférés-vezérlés és az Identitáskezeléshez használt).

## <a name="prepare-for-single-server-deployment"></a>Az egykiszolgálós telepítés előkészítése


Egyetlen VMM-kiszolgáló csak akkor hajthatja végre a replikálást virtuális gépek Hyper-V gazdagépek a VMM-felhő hello túl[Azure](hyper-v-site-walkthrough-overview.md) vagy tooa másodlagos VMM-felhő, ebben a dokumentumban leírt módon. Hello első lehetőség zökkenőmentes felhők közötti replikálása nem ajánlott.

Ha szeretné, hogy felhők közötti tooreplicate, replikálhatja egyetlen önálló VMM-kiszolgálóhoz, vagy felhőbe archivált Windows fürtben telepített egyetlen VMM-kiszolgálóhoz

### <a name="replicate-with-a-standalone-vmm-server"></a>Az önálló VMM-kiszolgáló replikálása

Ebben a forgatókönyvben hello egyetlen VMM-kiszolgáló telepítése virtuális gépként hello elsődleges helyen, és replikálja a virtuális gép tooa másodlagos helyet a Site Recovery és a Hyper-V replika használata.

1. **Állítsa be a Hyper-V virtuális gép VMM**. Javasoljuk, hogy Ön tartozó hello hello a VMM által használt SQL Server-példány közös elhelyezése azonos virtuális gép. Ezzel időt takaríthat meg, csak egy virtuális gép rendelkezik létrehozott toobe. Ha azt szeretné, hogy az SQL Server távoli példányát toouse, és egy esetleges leálláskor, toorecover annak a példánynak előtt kell a VMM állíthatja helyre.
2. **Győződjön meg arról hello VMM-kiszolgáló van konfigurálva, legalább két felhők**. Egy felhőalapú virtuális gépek szeretné, hogy tooreplicate és más felhőalapú hello hello másodlagos helyet fog szolgálni hello fogja tartalmazni. felhőbe, amely tartalmazza a kívánt tooprotect meg kell felelnie hello virtuális gépek hello [Előfeltételek](#prerequisites).
3. Állítsa be a Site Recovery, ebben a cikkben leírtak szerint. Hozzon létre és regisztrálja hello VMM-kiszolgálót a tárolóban, egy replikációs házirendnek és engedélyezze a replikálást. hello forrás és cél VMM neve azonos lesz hello. Adja meg, hogy a kezdeti replikáció hello hálózaton keresztül történik.
4. A hálózatleképezés beállításához le kell képezni hello hello elsődleges felhő toohello Virtuálisgép-hálózat hello másodlagos felhőre vonatkozó Virtuálisgép-hálózatot.
5. Hello Hyper-V Manager konzolon engedélyezze a Hyper-V replika hello Hyper-V gazdagépen, amely tartalmazza a hello VMM virtuális gép, és engedélyezze a replikálást a virtuális gép hello. Ellenőrizze, hogy a hely helyreállítását követően tooensure, hogy a Hyper-V replika beállításait nem felülbírálják a Site Recovery által védett hello VMM virtuális gép tooclouds nem tölti fel.
6. Használhatja a feladatátvételi helyreállítási tervek létrehozásakor hello forrás megegyező VMM-kiszolgálóhoz, és a cél.
7. Egy teljes leállás feladatátvételt, és a következőképpen helyre:

   1. Nem tervezett feladatátvétel hello Hyper-V Manager konzolon hello másodlagos helyen, toofail keresztül hello elsődleges VMM virtuális gépek toohello másodlagos helyen.
   2. Győződjön meg arról, hogy hello VMM virtuális gép működik-e és fut, és hello tárolóban, futtatható egy nem tervezett feladatátvétel toofail keresztüli hello virtuális gépek elsődleges toosecondary felhőből. Véglegesítse hello feladatátvételi, és válassza ki a másik helyreállítási pontot, ha szükséges.
   3. Hello nem tervezett feladatátvétel befejezése után összes erőforrás elérhető hello elsődleges hely újra.
   4. Hello elsődleges hely nem érhető el ebben az esetben a Hyper-V Manager konzol hello hello másodlagos hely, engedélyezze a hello VMM virtuális gép fordított replikációját. A másodlagos tooprimary replikálása hello virtuális gép elindul.
   5. A tervezett feladatátvétel végrehajtása hello Hyper-V Manager konzolon hello másodlagos helyen, toofail hello VMM virtuális gépek toohello elsődleges hely alatt. Hello feladatátvételi véglegesítése. A fordított replikáció, majd engedélyezze, hogy hello VMM virtuális gép újra replikálása működik az elsődleges toosecondary.
   6. A Recovery Services-tároló hello engedélyezze a fordított replikáció indítása virtuális gépeket, replikálásához őket másodlagos tooprimary toostart hello munkaterhelés.
   7. A hello Recovery Services-tároló, futtassa a tervezett feladatátvétel toofail hátsó hello munkaterhelési virtuális gépek toohello az elsődleges hely. Hello feladatátvételi toocomplete véglegesítése azt. Engedélyezze a visszirányú replikálás toostart replikációs hello munkaterhelési virtuális gépekhez az elsődleges toosecondary.

### <a name="replicate-with-a-stretched-vmm-cluster"></a>Replikálhat a felhőbe archivált VMM-fürthöz

Helyett egy önálló VMM-kiszolgáló telepítésének tooa másodlagos helyre replikált virtuális, akkor is használhatja a VMM magas rendelkezésre állású Windows feladatátvevő fürtben virtuális gépként telepíti azokat. Így lehetővé teszi a rugalmas munkaterhelést és a hardver meghibásodása elleni védelem. a Site Recovery hello VMM virtuális gépek toodeploy földrajzilag különálló helyen stretch fürtben kell telepíteni. toodo ezt:

1. Telepítse a VMM egy virtuális gépen egy Windows feladatátvevő fürtben, és válassza a hello beállítás toorun hello kiszolgáló magas rendelkezésre állásúként a telepítés során.
2. a VMM által használt hello SQL Server-példány replikálni kell az SQL Server AlwaysOn rendelkezésre állási csoportok, úgy, hogy hello adatbázis hello másodlagos helyen.
3. Kövesse a cikk toocreate egy tárolót hello utasításait, hello kiszolgáló regisztrálásához és védelmének beállítása. Recovery Services-tároló hello a fürt minden VMM-kiszolgálót a hello tooregister van szüksége. toodo, telepítse a szolgáltató hello aktív csomópontra, és regisztrálja hello VMM-kiszolgálót. Majd telepítse a szolgáltató hello többi csomóponton.
4. Nem tervezett kimaradás esetén hello VMM-kiszolgáló a megfelelő SQL Server-adatbázist a feladatátvételt és hello másodlagos helyről elérhető.



## <a name="next-steps"></a>Következő lépések

Nyissa meg túl[5. lépés: állítson be egy tároló](vmm-to-vmm-walkthrough-create-vault.md).
