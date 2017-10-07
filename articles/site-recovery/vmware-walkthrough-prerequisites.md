---
title: "az Azure Site Recovery segítségével VMware tooAzure replikációs aaaPrerequisites |} Microsoft Docs"
description: "VMware virtuális gépek tooAzure hello Azure Site Recovery szolgáltatással munkaterheléseinek replikálása hello előfeltételeit foglalja össze."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 318156ba-793b-48d0-98d4-cc5436bf28a3
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: 14f8e9713b42a1d8da71223fbbb7a6b7c4303adb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="step-2-review-hello-prerequisites-for-vmware-tooazure-replication"></a>2. lépés: A VMware tooAzure replikációs hello előfeltételeinek áttekintése

Olvassa el a hello Előfeltételek hello a következő táblázat foglalja össze.

**Előfeltétel** | **Részletek**
--- | ---
**Azure** | További tudnivalók [Azure-követelmények](site-recovery-prereq.md#azure-requirements)
**Helyszíni konfigurációs kiszolgáló** | Windows Server 2012 R2 rendszerű VMware virtuális gép van szüksége, vagy később. Ez a kiszolgáló beállítása a Site Recovery üzembe helyezése során.<br/><br/> Alapértelmezett hello folyamat kiszolgáló és a fő célkiszolgáló is települnek a virtuális Gépet. Vertikális felskálázás, előfordulhat, hogy különálló folyamatkiszolgálót, és rendelkezik hello ugyanazok vonatkoznak, mint hello konfigurációs kiszolgáló.<br/><br/> További információ ezen összetevők [Itt](site-recovery-set-up-vmware-to-azure.md#configuration-server-minimum-requirements)
**Helyszíni VMware-kiszolgálók** | Egy vagy több VMware vSphere, futtató kiszolgálók 6.5, 6.0, 5.5, 5.1 legújabb frissítéseit. Kiszolgálók hello azonos hálózati hello konfigurációs kiszolgáló (vagy különálló folyamatkiszolgálót) kell elhelyezni.<br/><br/> Ajánlott egy vCenter-kiszolgáló toomanage állomások 6.5, 6.0 vagy 5.5 rendszerű hello legújabb frissítéseit.
**A helyszíni virtuális gépek** | Virtuális gépek kívánt tooreplicate rendszerűnek kell lennie egy [támogatott operációs rendszer](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions), és adott a specifikációknak való [Azure-Előfeltételek](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements). Virtuális gép VMware-eszközök futó kell rendelkeznie.
**URL-címek** | meg kell hello konfigurációs kiszolgálót toothese URL hozzáférés:<br/><br/> [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]<br/><br/> Ha IP-címeken alapuló tűzfalszabályok szabály, győződjön meg arról, kommunikációs tooAzure lehetővé teszik.<br/></br> Hello engedélyezése [Azure Datacenter IP-címtartományok](https://www.microsoft.com/download/confirmation.aspx?id=41653), és a HTTPS (443) port hello.<br/></br> Lehetővé teszi az IP-címtartományok hello Azure-régió, az előfizetés, valamint az USA nyugati (hozzáférés-vezérlés és az Identitáskezeléshez használt).<br/><br/> Engedélyezi a hello MySQL letöltési URL-címe: http://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi.
**Mobilitási szolgáltatás** | Minden replikált virtuális gép telepítve.




## <a name="limitations"></a>Korlátozások

Győződjön meg arról, hogy hello korlátozások központi telepítése előtt hello táblázat foglalja össze.

**A korlátozás** | **Részletek**
--- | ---
**Azure** | Tárolási és hálózati fiókoknak kell lenniük a hello és hello-tárolónak ugyanabban a régióban<br/><br/> A prémium szintű storage-fiókok használatához is szükség van egy standard fiók toostore replikálási naplók tárolásához<br/><br/> Közép-és Dél-Indiában toopremium fiókok nem replikálja.
**Helyszíni konfigurációs kiszolgáló** | VMware virtuális hálózatiadapter-típus VMXNET3 kell lennie. Ha nem, [a frissítés telepítése](https://kb.vmware.com/selfservice/microsites/search.do?cmd=displayKC&docType=kc&externalId=2110245&sliceId=1&docTypeID=DT_KB_1_1&dialogID=26228401&stateId=1)<br/><br/> a vSphere PowerCLI 6.0 kell telepíteni.<br/><br> hello gépet nem lehet tartományvezérlő. hello gépnek rendelkeznie kell egy statikus IP-címet.<br/><br/> hello állomásnév 15 karakter lehet ne legyen, és az operációs rendszer angol nyelven kell lennie.
**VMware** | A Site Recovery nem támogatja az új vCenter és vSphere 6.5-ös és 6.0 szolgáltatások például a kereszt-vCenter vMotion, a virtuális kötetek és a tárolási DRS.
**Virtuális gépek** | Győződjön meg arról [Azure virtuális gép korlátozásai](site-recovery-prereq.md#azure-requirements)<br/><br/> Titkosított lemezzel rendelkező virtuális gépeket, vagy az UEFI/EFI-rendszerbetöltés virtuális gépeken nem replikálja.<br/><br> A megosztott lemez nem használhatók. Ha hello forrás virtuális gép rendelkezik hálózati adapterek összevonása, azt konvertálja tooa egyetlen hálózati adapter a feladatátvételt követően.<br/><br/> Ha a virtuális gép iSCSI-lemez van, a Site Recovery átalakítja tooa VHD-fájlt a feladatátvételt követően. Hello iSCSI-tároló hello Azure virtuális gép elérhető, ha tooit csatlakozik, és azt és a virtuális merevlemez hello látja. Ha ez történik, válassza le a hello iSCSI-tároló.<br/><br/> Ha azt szeretné, hogy a virtuális Gépre kiterjedő konzisztencia tooenable, amely lehetővé teszi a munkaterhelés ugyanazon toobe helyre hello futó virtuális gépeket együtt tooa azonos adatokat mutasson, nyissa meg a virtuális gép hello 20004 portot.<br/><br/> Windows hello C meghajtó telepítve kell lennie. az operációsrendszer-lemez hello alapvető, és nem dinamikus kell lennie. hello adatlemez dinamikus lehet.<br/><br/> Linux/Etc/Hosts fájlt a virtuális gépek bejegyzéseit, amelyek hozzárendeléséről hello helyi állomás neve tooIP minden hálózati adapterekkel társított kell tartalmaznia. állomásnév, csatlakoztatási pontokat, eszköz neve, elérési utak és fájlnevek hello (/ etc; / usr) kell lennie csak angol nyelven.<br/><br/> Adott típusú [Linux tárolási](site-recovery-support-matrix-to-azure.md#support-for-storage) támogatottak.<br/><br/>Hozzon létre, vagy állítsa be **disk.enableUUID=true** hello virtuális gép beállításai. Egységes UUID toohello VMDK, ez biztosítja, így a megfelelő csatlakoztatja, és biztosítja, hogy az csak a változási különbözeteket átvitt hátsó tooon helyszíni feladat-visszavétel, anélkül, hogy a teljes replikáció során.


## <a name="next-steps"></a>Következő lépések

- Ha teljes körű telepítésére végzett, nyissa meg túl[3. lépés: kapacitástervezés](vmware-walkthrough-capacity.md)
- Ha egy egyszerű próbatelepítés végzett, nyissa meg túl[4. lépés: hálózati terv](vmware-walkthrough-network.md).
