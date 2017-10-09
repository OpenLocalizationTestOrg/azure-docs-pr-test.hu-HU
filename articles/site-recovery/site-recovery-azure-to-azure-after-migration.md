---
title: "aaaPrepare gépek tooset mentése után a Site Recovery segítségével áttelepítési tooAzure Azure-régiók közötti vész-helyreállítási |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan tooprepare gépek tooset katasztrófa utáni helyreállítás után áttelepítési tooAzure Azure Site Recovery segítségével Azure-régiók közötti fel."
services: site-recovery
documentationcenter: 
author: ponatara
manager: abhemraj
editor: 
ms.assetid: 9126f5e8-e9ed-4c31-b6b4-bf969c12c184
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: ponatara
ms.openlocfilehash: b6274e3df210c1d8a7b8289cc85868ee6414e523
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-azure-vms-tooanother-region-after-migration-tooazure-by-using-azure-site-recovery"></a>Azure virtuális gépek tooanother régió replikálása után áttelepítési tooAzure Azure Site Recovery segítségével

>[!NOTE]
> Az Azure virtuális gépek (VM) az Azure Site Recovery replikációs jelenleg előzetes verzió.

## <a name="overview"></a>Áttekintés

Ez a cikk segít az Azure virtuális gépek előkészítése után ezek a gépek áttelepített egy a helyszíni környezet tooAzure Azure Site Recovery segítségével két Azure régiók közötti replikáció.

## <a name="disaster-recovery-and-compliance"></a>Vész-helyreállítási és megfelelőség
Napjainkban egyre több vállalatok áthelyezi a munkaterhelések tooAzure. A kritikus fontosságú helyszíni éles áthelyezése vállalatok munkaterhelések tooAzure, az ilyen terhelések vész-helyreállítási beállítása megadása kötelező, megfelelőségi és toosafeguard szemben Azure-régióban esetleges megszakadását.

## <a name="steps-for-preparing-migrated-machines-for-replication"></a>Felkészülés az áttelepített gépek replikációjának lépései
tooprepare állíthatja be az Azure-régió replikációs tooanother gépeket telepített át:

1. Az áttelepítés végrehajtásához.
2. Hello Azure ügynök szükség esetén telepítse.
3. Távolítsa el a mobilitási szolgáltatás hello.  
4. Indítsa újra a virtuális gép hello.

Bővebben a következő részekben hello ezeket a lépéseket ismerteti.

### <a name="step-1-migrate-workloads-running-on-hyper-v-vms-vmware-vms-and-physical-servers-toorun-on-azure-vms"></a>1. lépés:, Telepítse át a Hyper-V virtuális gépek, a VMware virtuális gépek és fizikai kiszolgálók toorun Azure virtuális gépeken futó számítási feladatok

tooset replikációs kialakításához, és telepítse át a helyszíni Hyper-V, VMware és fizikai munkaterhelésekhez tooAzure kövesse hello lépéseit hello [át Azure IaaS virtuális gépeket az Azure Site Recovery Azure-régiók közötti](site-recovery-migrate-to-azure.md) cikk. 

Az áttelepítés után nem toocommit kell, vagy törölje a feladatátvétel. Ehelyett válassza ki a hello **az áttelepítés végrehajtásához** lehetőséget az egyes gépek toomigrate szeretne:
1. A **replikált elemek**, kattintson a jobb gombbal a virtuális gép hello, és kattintson a **az áttelepítés végrehajtásához**. Kattintson a **OK** toocomplete hello lépés. A folyamat állapotát a virtuális gép tulajdonságok hello nyomon követéséhez hello teljes áttelepítési feladatot a figyelés **Site Recovery-feladatok**.
2. Hello **az áttelepítés végrehajtásához** művelet hello áttelepítési folyamat befejeződik, hello gép replikálása eltávolítja, és leállítja a Site Recovery számlázási hello gép.

   ![teljesáttelepítés](./media/site-recovery-hyper-v-site-to-azure/migrate.png)

### <a name="step-2-install-hello-azure-vm-agent-on-hello-virtual-machine"></a>2. lépés: Hello Azure Virtuálisgép-ügynök telepítése hello virtuális gépen
hello Azure [Virtuálisgép-ügynök](../virtual-machines/windows/classic/agents-and-extensions.md#azure-vm-agents-for-windows-and-linux) hello virtuális gépen telepítve kell, a hello Site Recovery bővítmény toowork és toohelp hello virtuális gép védelmét.

>[!IMPORTANT]
>A Windows virtuális gépek 9.7.0.0, verziójától hello mobilitási szolgáltatások telepítőjének is telepíti hello legújabb elérhető Azure Virtuálisgép-ügynök. Az áttelepítési hello virtuális gép megfelel-e a bármely Virtuálisgép-bővítmény, belefoglalja hello Site Recovery használatára vonatkozó előfeltételek telepítése. hello Azure virtuális gép ügynök igények toobe manuálisan telepíteni, csak akkor, ha hello hello a mobilitási szolgáltatás áttelepített gépet 9.6 vagy korábbi verziója.

hello következő táblázat további információt tartalmaz hello Virtuálisgép-ügynök telepítése és az ellenőrzése, hogy telepítette:

| **Művelet** | **Windows** | **Linux** |
| --- | --- | --- |
| Hello Virtuálisgép-ügynök telepítése |Töltse le és telepítse a hello [ügynök MSI](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Rendszergazdai jogosultságok toocomplete hello telepítési van szüksége. |Telepítse legújabb hello [Linux-ügynök](../virtual-machines/linux/agent-user-guide.md). Rendszergazdai jogosultságok toocomplete hello telepítési van szüksége. Azt javasoljuk, hogy a terjesztési adattárból hello ügynök telepítése. A Microsoft *nem javasoljuk* közvetlenül a Githubból hello Linux Virtuálisgép-ügynök telepítése.  |
| Hello Virtuálisgép-ügynök telepítésének ellenőrzése |1. Keresse meg a toohello C:\WindowsAzure\Packages mappájában hello Azure virtuális Gépen. Megtekintheti az hello WaAppAgent.exe fájlt. <br>2. Kattintson a jobb gombbal a hello fájlt, nyissa meg túl**tulajdonságok**, majd hello **részletek** külön-külön hello **termékverzió** mező lehet 2.6.1198.718 vagy újabb. |N/A |


### <a name="step-3-remove-hello-mobility-service-from-hello-migrated-virtual-machine"></a>3. lépés: Az áttelepített virtuális gép eltávolítása hello hello a mobilitási szolgáltatás

Ha áttelepítette a helyszíni VMware gépek vagy windowsos/Linuxos fizikai kiszolgálók, akkor toomanually távolítsa el, illetve az Eltávolítás hello mobilitásiszolgáltatás hello áttelepített virtuális gépről.

>[!IMPORTANT]
>Ez a lépés nincs szükség a Hyper-V virtuális gépek áttelepített tooAzure.

#### <a name="uninstall-hello-mobility-service-on-a-windows-server-vm"></a>Távolítsa el a hello mobilitási szolgáltatás egy Windows Server virtuális gépen
Hello módszerek toouninstall hello mobilitási szolgáltatás egy Windows Server-számítógépen a következő egyikét használhatja.

##### <a name="uninstall-by-using-hello-windows-ui"></a>Távolítsa el a Windows felhasználói felületének hello használatával
1. Hello Vezérlőpultot, válassza ki **programok**.
2. Válassza ki **Microsoft Azure Site helyreállítási mobilitási szolgáltatás/fő célkiszolgáló**, majd válassza ki **Eltávolítás**.

##### <a name="uninstall-at-a-command-prompt"></a>Távolítsa el a parancssorba
1. Nyisson meg egy parancssori ablakot rendszergazdaként.
2. toouninstall hello mobilitási szolgáltatást, futtassa a következő parancs hello:

   ```
   MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1} /L+*V "C:\ProgramData\ASRSetupLogs\UnifiedAgentMSIUninstall.log"
   ```

#### <a name="uninstall-hello-mobility-service-on-a-linux-computer"></a>Távolítsa el a mobilitási szolgáltatás hello egy Linux-számítógép
1. A Linux-kiszolgálón jelentkezzen be egy **legfelső szintű** felhasználó.
2. A Terminálszolgáltatások lépjen túl/felhasználó/helyi/automatikus rendszer-Helyreállítás.
3. toouninstall hello mobilitási szolgáltatást, futtassa a következő parancs hello:

   ```
   uninstall.sh -Y
   ```

### <a name="step-4-restart-hello-vm"></a>4. lépés: Indítsa újra a virtuális gép hello

Hello mobilitási szolgáltatás eltávolítása után előtt a virtuális gép újraindítása hello beállítása replikációs tooanother Azure-régiót.


## <a name="next-steps"></a>Következő lépések
- A munkaterhelések számára a védelmének megkezdéséhez [Azure virtuális gépek replikálásához](site-recovery-azure-to-azure.md).
- További információ [útmutató az Azure virtuális gépek replikálása a hálózat](site-recovery-azure-to-azure-networking-guidance.md).
