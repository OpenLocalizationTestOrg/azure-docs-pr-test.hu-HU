---
title: "aaaMonitor és a virtuális gépek és fizikai kiszolgálók protection hibáinak elhárítása |} Microsoft Docs"
description: "Az Azure Site Recovery koordinálja hello replikáció, feladatátvétel és a helyszíni kiszolgálók tooAzure vagy egy másodlagos adatközpontba található virtuális gépek helyreállítása. Ez a cikk toomonitor használja, és a Virtual Machine Manager vagy a Hyper-V hely protection hibáinak elhárítása."
services: site-recovery
documentationcenter: 
author: ruturaj
manager: mkjain
editor: 
ms.assetid: 0fc8e368-0c0e-4bb1-9d50-cffd5ad0853f
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/05/2017
ms.author: rajanaki
ms.openlocfilehash: d790375db5f30e98f009c8d8272558188c51934c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-and-troubleshoot-protection-for-virtual-machines-and-physical-servers"></a>Figyelése és hibaelhárítása szempontjából a virtuális gépek és fizikai kiszolgálók védelme
A figyelési és hibaelhárítási útmutató segítségével megismerheti, hogyan tootrack replikálás állapotának és technikák hibaelhárítása az Azure Site Recovery.

## <a name="understand-hello-components"></a>Hello összetevők megismerése
### <a name="vmware-virtual-machine-or-physical-server-site-deployment-for-replication-between-on-premises-and-azure"></a>VMware virtuális gép vagy fizikai kiszolgálók helyen történő központi telepítés a helyszíni és az Azure közötti replikáció
helyszíni VMware virtuális gép vagy fizikai kiszolgálók és az Azure közötti adatbázis-helyreállítási tooset, kell tooset hello konfigurációs kiszolgáló, a fő célkiszolgáló és a folyamat kiszolgáló-összetevők a virtuális gép vagy a kiszolgálón. Hello forráskiszolgáló védelmének engedélyezésekor Azure Site Recovery telepíti a Microsoft Azure App Service Mobile Apps szolgáltatása hello. A helyszíni kimaradás és után hello forrás server tooAzure feladatátvételt hajt végre az Azure-ban egy folyamatkiszolgálóra és egy helyi toorebuild hello forráskiszolgálón a helyszíni fő célkiszolgáló tooset kell.

![VMware vagy fizikai helyen történő központi telepítés, a helyszíni és az Azure közötti replikáció](media/site-recovery-monitoring-and-troubleshooting/image18.png)

### <a name="virtual-machine-manager-site-deployment-for-replication-between-on-premises-sites"></a>A Virtual Machine Manager helyen történő központi telepítés a helyszíni helyek közötti replikáció
adatbázis helyreállítási két között tooset a helyszíni helyeken, toodownload hello Azure Site Recovery provider kell, és telepítse azt hello a Virtual Machine Manager kiszolgálón. hello ellátót Internet kapcsolat tooensure, hogy az összes hello művelet által kiváltott hello Azure-portálon a lefordított tooon helyszíni műveleteinek beolvasása.

![A Virtual Machine Manager helyen történő központi telepítés a helyszíni helyek közötti replikáció](media/site-recovery-monitoring-and-troubleshooting/image1.png)

### <a name="virtual-machine-manager-site-deployment-for-replication-between-on-premises-locations-and-azure"></a>A Virtual Machine Manager helyen történő központi telepítés a helyszíni helyek és az Azure közötti replikáció
A helyszíni helyek és az Azure közötti helyreállítás beállításakor toodownload hello Azure Site Recovery provider kell, és telepítse azt hello a Virtual Machine Manager kiszolgálón. Szükség tooinstall hello Azure Recovery Services Agent, amely minden Hyper-V gazdagépen telepített toobe szükséges. [További](site-recovery-hyper-v-azure-architecture.md) további információt.

![A Virtual Machine Manager helyen történő központi telepítés a helyszíni helyek és az Azure közötti replikáció](media/site-recovery-monitoring-and-troubleshooting/image2.png)

### <a name="hyper-v-site-deployment-for-replication-between-on-premises-locations-and-azure"></a>Hyper-V hely központi telepítésének a helyszíni helyek és az Azure közötti replikáció
Ez a folyamat hasonló tooVirtual Machine Manager telepítési. hello csak különbség hello Azure Site Recovery provider és az Azure Recovery Services Agent első telepített hello Hyper-V gazdagéphez. [További információk](site-recovery-hyper-v-azure-architecture.md). .

## <a name="monitor-configuration-protection-and-recovery-operations"></a>Konfigurációs, védelmi és helyreállítási műveletek figyelése
Az Azure Site Recovery minden művelet naplózva, és nyomon követheti a hello **feladatok** fülre. Konfigurációs, védelmi és helyreállítási hiba, lépjen a toohello **feladatok** lapra, és keresse meg a hibákat.

![hello sikertelen szűrő hello feladatok lap](media/site-recovery-monitoring-and-troubleshooting/image3.png)

Ha a talált hibák a hello **feladatok** fülre, kattintson a hello feladat, majd **hiba legutolsó részletes adatai** , hogy a feladat.

![hello hiba legutolsó részletes adatai gomb](media/site-recovery-monitoring-and-troubleshooting/image4.png)

hello hiba legutolsó részletes adatai segítségével azonosíthatók a lehetséges ok és javaslat hello problémára.

![Egy adott feladat kapcsolatos hiba részleteit megjelenítő párbeszédpanel](media/site-recovery-monitoring-and-troubleshooting/image5.png)

Hello előző példában egy másik művelet folyamatban van, úgy tűnik, hello védelmi konfiguráció toofail okozó toobe. Hello a javaslaton alapuló hello probléma megoldásához, és kattintson a **RESART** tooinitiate hello műveletet.

![hello Újraindítás gombra hello feladatok lap](media/site-recovery-monitoring-and-troubleshooting/image6.png)

Hello **indítsa újra a** , nincs lehetőség minden műveletnél. Ha egy művelet nem rendelkezik hello **indítsa újra a** lehetőséget, lépjen vissza toohello objektum, majd végezze el újra a művelettel hello újra. Bármely, a folyamatban lévő feladat megszakíthatja hello segítségével **Mégse** gombra.

![hello Mégse gomb](media/site-recovery-monitoring-and-troubleshooting/image7.png)

## <a name="monitor-replication-health-for-virtual-machines"></a>A virtuális gépek replikálás állapotának figyelése
Az egyes védett hello entitások hello Azure portál tooremotely figyelése az Azure Site Recovery szolgáltatók is használhatja. Kattintson a **védett elemek**, és kattintson a **VMM-FELHŐKBEN** vagy **védelmi csoportok**. Hello **VMM-FELHŐKBEN** lap csak az, hogy a Virtual Machine Manager a megadott központi telepítéseknél használható. Az egyéb forgatókönyvek, védett hello entitások hello alatt állnak **védelmi csoportok** fülre.

![hello VMM-Felhőkben és védelmi csoportok beállításai](media/site-recovery-monitoring-and-troubleshooting/image8.png)

Válassza a védett entitás hello megfelelő felhő vagy a védelmi csoport toosee elérhető műveletek hello alsó ablaktáblán látható.

![A kijelölt védett entitás elérhető lehetőségek](media/site-recovery-monitoring-and-troubleshooting/image9.png)

Ahogy az előző képernyőképet hello, hello virtuális gép állapotát az **kritikus**. Kattinthat a hello **hiba legutolsó részletes adatai** hello alsó toosee hello hiba gombjára. Hello alapján **lehetséges okok** és **javaslat**, hello probléma megoldása érdekében.

![hello hiba legutolsó részletes adatai gomb](media/site-recovery-monitoring-and-troubleshooting/image10.png)

![Hibák és javaslatok hello hiba legutolsó részletes adatai párbeszédpanel](media/site-recovery-monitoring-and-troubleshooting/image11.png)

> [!NOTE]
> Ha minden aktív műveletek végrehajtása közben, vagy sikertelen volt, válassza a toohello **feladatok** említett korábbi tooview hello hiba történt egy adott feladat nézetként.
>
>

## <a name="troubleshoot-on-premises-hyper-v-issues"></a>A helyszíni Hyper-V problémák elhárítása
Toohello helyszíni Hyper-V manager konzolon válassza hello virtuális gép, csatlakozzon, és tekintse meg a hello replikációs állapotát.

![A beállítás tooview replikálás állapotának hello Hyper-V manager konzolon](media/site-recovery-monitoring-and-troubleshooting/image12.png)

Ebben az esetben **replikálás állapotának** van **kritikus**. Kattintson a jobb gombbal a hello virtuális gépet, és kattintson **replikációs** > **replikálás állapotának megtekintése** toosee hello részleteit.

![Egy adott virtuális gép replikációs állapotát](media/site-recovery-monitoring-and-troubleshooting/image13.png)

Ha a replikációs hello virtuális gép fel van függesztve, kattintson a jobb gombbal a hello virtuális gép, és kattintson **replikációs** > **replikálás folytatása**.

![A beállítás tooresume replikációs hello Hyper-V manager konzolon](media/site-recovery-monitoring-and-troubleshooting/image19.png)

Ha egy virtuális gép áttelepítést végez egy új Hyper-V gazdagépre belül hello fürt vagy egy önálló gépre, és hello Hyper-v rendszerű gazdagépen keresztül Azure Site Recovery van konfigurálva, hello virtuális gép replikálása nem tudnák negatív hatással lehet. Győződjön meg arról, hogy hello új Hyper-V gazdagép teljesíti az összes hello előfeltételt Azure Site Recovery segítségével konfigurálhatók.

### <a name="event-log"></a>Eseménynapló
| Eseményforrások | Részletek |
| --- |:--- |
| **Alkalmazások és a szolgáltatás Logs/Microsoft/VirtualMachineManager/kiszolgáló/Admin** (Virtual Machine Manager-kiszolgáló) |Hasznos naplózási tootroubleshoot biztosít számos különböző a Virtual Machine Manager problémákat. |
| **Alkalmazások és a naplók vagy MicrosoftAzureRecoveryServices/replikációs szolgáltatás** (Hyper-V gazdagép) |Hasznos naplózási tootroubleshoot biztosít a Microsoft Azure Recovery Services Agent számos problémájának. <br/> ![A Hyper-V gazdagép replikációs eseményforrás helye](media/site-recovery-monitoring-and-troubleshooting/eventviewer03.png) |
| **Alkalmazások és a szolgáltatás Logs/Microsoft/Azure Site Recovery/szolgáltató/műveleti** (Hyper-V gazdagép) |Itt hasznos naplózási tootroubleshoot számos Microsoft Azure Site Recovery szolgáltatás problémákat. <br/> ![A Hyper-V gazdagép működési események forrás helye](media/site-recovery-monitoring-and-troubleshooting/eventviewer02.png) |
| **Alkalmazások és a Service Logs/Microsoft/Windows/Hyper-V-VMMS/Admin** (Hyper-V gazdagép) |Hasznos naplózási tootroubleshoot sok Hyper-V Virtuálisgép-felügyeleti problémák biztosít. <br/> ![Hyper-V gazdagéphez a Virtual Machine Manager forrás helye](media/site-recovery-monitoring-and-troubleshooting/eventviewer01.png) |

### <a name="hyper-v-replication-logging-options"></a>Hyper-V replikációs naplózási beállítások
Az összes, amelyek a projektjükhöz tooHyper-V replikáció eseményeket hello Hyper-V-VMMS\\rendszergazda naplóban található alkalmazások és szolgáltatásnaplók\\Microsoft\\Windows. Emellett egy Hyper-V virtuális gép felügyeleti szolgáltatás hello elemzési naplóban is engedélyezheti. tooenable ez jelentkeznek, először ellenőrizze a hello elemzési és hibakeresési naplók az Eseménynapló hello megtekinthető. Nyissa meg az eseménynaplót, és kattintson **nézet** > **elemzési és hibakeresési naplók megjelenítése**.

![hello elemzési és hibakeresési naplók megjelenítése lehetőséget.](media/site-recovery-monitoring-and-troubleshooting/image14.png)

Az elemzési log utasítás végrehajtása alatt látható **Hyper-V-VMMS**.

![hello elemzési jelentkezzen be hello Eseménynapló fa](media/site-recovery-monitoring-and-troubleshooting/image15.png)

A hello **műveletek** ablaktáblán kattintson a **napló engedélyezése**. Az engedélyezése után megjelenik **Teljesítményfigyelő** , egy **eseménykövető munkamenet** alatt **adatgyűjtő-csoportosítók.**

![Esemény-nyomkövetési munkamenet hello Teljesítményfigyelő fában](media/site-recovery-monitoring-and-troubleshooting/image16.png)

tooview hello az összegyűjtött információk, akkor először állítsa le a hello napló letiltásával hello nyomkövetési munkamenet. Mentse hello naplót, és nyissa meg ismét az eseménynaplóban, vagy más eszközök tooconvert használja azt a kívánt módon működjenek.

## <a name="reach-out-for-microsoft-support"></a>For Microsoft Support érheti el
### <a name="log-collection"></a>Naplógyűjtést
A Virtual Machine Manager-hely védelmét, tekintse meg túl[támogatási diagnosztikai Platform (SDP) eszközzel Azure Site Recovery naplógyűjtést](http://social.technet.microsoft.com/wiki/contents/articles/28198.asr-data-collection-and-analysis-using-the-vmm-support-diagnostics-platform-sdp-tool.aspx) toocollect hello szükséges naplókat.

Hyper-V hely védelmét, töltse le a [eszköz](https://dcupload.microsoft.com/tools/win7files/DIAG_ASRHyperV_global.DiagCab) és hello Hyper-V gazdagép toocollect hello napló végrehajtja.

VMware vagy fizikai kiszolgáló használata esetén tekintse meg túl[VMware és fizikai hely védelme az Azure Site Recovery naplógyűjtést](http://social.technet.microsoft.com/wiki/contents/articles/30677.azure-site-recovery-log-collection-for-vmware-and-physical-site-protection.aspx) toocollect hello szükséges naplókat.

hello eszköz helyileg-naplók hello % LocalAppData%\ElevatedDiagnostics egy véletlenszerűen almappájában gyűjt.

![A Hyper-V hely védelem látható minta lépéseket.](media/site-recovery-monitoring-and-troubleshooting/animate01.gif)

### <a name="open-a-support-ticket"></a>Támogatási jegy megnyitása
az Azure Site Recovery támogatási jegy tooraise érheti el tooAzure támogatása az URL-cím segítségével: <http://aka.ms/getazuresupport>.

## <a name="kb-articles"></a>Tudásbáziscikkek
* [Hogyan toopreserve hello meghajtóbetűjelet védett virtuális gépek, amelyek feladatátvétel történt, vagy tooAzure áttelepítése](http://support.microsoft.com/kb/3031135)
* [Hogyan toomanage helyszíni tooAzure védelem hálózati sávszélesség-használat](https://support.microsoft.com/kb/3056159)
* [Az Azure Site Recovery: "hello fürt erőforrás nem található" hibaüzenet jelenik meg a virtuális gép védelmét tooenable](http://support.microsoft.com/kb/3010979)
* [Ismertetése és hibaelhárítása a Hyper-V replikáció útmutató](http://social.technet.microsoft.com/wiki/contents/articles/21948.hyper-v-replica-troubleshooting-guide.aspx)

## <a name="common-azure-site-recovery-errors-and-their-resolutions"></a>Azure Site Recovery előforduló hibákat, és azok megoldásait
Az alábbiakban gyakori hibák és azok megoldásait. Minden egyes hibához egy külön wiki lapján ismertetését.

### <a name="general"></a>Általános kérdések
* <span style="color:green;">ÚJ</span> [feladatai hiba miatt sikertelenül működő "művelet van folyamatban." Hiba 505, 514, 532.](http://social.technet.microsoft.com/wiki/contents/articles/32190.azure-site-recovery-jobs-failing-with-error-an-operation-is-in-progress-error-505-514-532.aspx)
* <span style="color:green;">ÚJ</span> [sikertelen, hiba: "A kiszolgáló nem csatlakoztatott toohello Internet" feladatok. Hiba történt a 25018.](http://social.technet.microsoft.com/wiki/contents/articles/32192.azure-site-recovery-jobs-failing-with-error-server-isn-t-connected-to-the-internet-error-25018.aspx)

### <a name="setup"></a>Beállítás
* [Virtual Machine Manager-kiszolgáló hello tooan belső hiba miatt nem regisztrálható. Hello hiba vonatkozó részletes információért tekintse meg a toohello feladatok nézetben hello Site Recovery portálon. Futtassa a telepítőt újra tooregister hello kiszolgáló.](http://social.technet.microsoft.com/wiki/contents/articles/25570.the-vmm-server-cannot-be-registered-due-to-an-internal-error-please-refer-to-the-jobs-view-in-the-site-recovery-portal-for-more-details-on-the-error-run-setup-again-to-register-the-server.aspx)
* [A kapcsolat nem lehet a meghatározott toohello Hyper-V helyreállítás-kezelő tárolójából. Ellenőrizze a proxybeállítások hello, vagy próbálkozzon újra később.](http://social.technet.microsoft.com/wiki/contents/articles/25571.a-connection-cant-be-established-to-the-hyper-v-recovery-manager-vault-verify-the-proxy-settings-or-try-again-later.aspx)

### <a name="configuration"></a>Konfiguráció
* [Védelmi csoport nem toocreate: Hiba történt a kiszolgálók hello listájának beolvasása.](http://blogs.technet.com/b/somaning/archive/2015/08/12/unable-to-create-the-protection-group-in-azure-site-recovery-portal.aspx)
* [Hyper-V gazdagépfürt legalább egy statikus hálózati adaptert tartalmaz, vagy nem csatlakoztatott adapterek konfigurált toouse DHCP.](http://social.technet.microsoft.com/wiki/contents/articles/25498.hyper-v-host-cluster-contains-at-least-one-static-network-adapter-or-no-connected-adapters-are-configured-to-use-dhcp.aspx)
* [A Virtual Machine Manager nem rendelkezik engedélyekkel toocomplete műveletet.](http://social.technet.microsoft.com/wiki/contents/articles/31110.vmm-does-not-have-permissions-to-complete-an-action.aspx)
* [Nem választható ki olyan hello tárfiók védelmének konfigurálása közben hello előfizetésen belül.](http://social.technet.microsoft.com/wiki/contents/articles/32027.can-t-select-the-storage-account-within-the-subscription-while-configuring-protection.aspx)

### <a name="protection"></a>Védelem
* <span style="color:green;">ÚJ</span> [hiba miatt sikertelenül működő "Védelmi nem sikerült konfigurálni a virtuális gép hello" védelem engedélyezése. Hiba 60007, 40003.](http://social.technet.microsoft.com/wiki/contents/articles/32194.azure-site-recovery-enable-protection-failing-with-error-protection-couldn-t-be-configured-for-the-virtual-machine-error-60007-40003.aspx)
* <span style="color:green;">ÚJ</span> [Védelemengedélyezési hiba miatt sikertelenül működő "Nem engedélyezhető a védelem a virtuális gép hello." Hiba történt a 70094.](http://social.technet.microsoft.com/wiki/contents/articles/32195.azure-site-recovery-enable-protection-failing-with-error-protection-couldn-t-be-enabled-for-the-virtual-machine-error-70094.aspx)
* <span style="color:green;">ÚJ</span> [élő áttelepítési hiba 23848 - hello virtuális gép áthelyezése típussal Live toobe lesz. Ez érvénytelenítheti hello hello virtuális gép helyreállítási védelmi állapotát.](http://social.technet.microsoft.com/wiki/contents/articles/32021.live-migration-error-23848-the-virtual-machine-is-going-to-be-moved-using-type-live-this-could-break-the-recovery-protection-status-of-the-virtual-machine.aspx)
* [Sikertelen volt, mert az ügynök nincs telepítve, a gazdagépen védelmének engedélyezése.](http://social.technet.microsoft.com/wiki/contents/articles/31105.enable-protection-failed-since-agent-not-installed-on-host-machine.aspx)
* [Hello replika virtuális gép megfelelő gazdagép nem található – toolow számítási erőforrások miatt.](http://social.technet.microsoft.com/wiki/contents/articles/25501.a-suitable-host-for-the-replica-virtual-machine-can-t-be-found-due-to-low-compute-resources.aspx)
* [Egy található megfelelő gazdagép hello replika virtuális géphez nem - miatt toono logikai hálózathoz csatlakozik.](http://social.technet.microsoft.com/wiki/contents/articles/25502.a-suitable-host-for-the-replica-virtual-machine-can-t-be-found-due-to-no-logical-network-attached.aspx)
* [Nem lehet kapcsolódni a gazdagép toohello replikagép - kapcsolat nem hozható létre.](http://social.technet.microsoft.com/wiki/contents/articles/31106.cannot-connect-to-the-replica-host-machine-connection-could-not-be-established.aspx)

### <a name="recovery"></a>Helyreállítás
* A Virtual Machine Manager nem tudja végrehajtani a gazdagépműveletet hello:
  * [Toohello kijelölt helyreállítási pont a virtuális gép feladatait: Általános hozzáférés megtagadva hiba.](http://social.technet.microsoft.com/wiki/contents/articles/25504.fail-over-to-the-selected-recovery-point-for-virtual-machine-general-access-denied-error.aspx)
  * [Hyper-V sikertelen toofail keresztül toohello kijelölt helyreállítási pont a virtuális gép: a művelet megszakítva.  Próbálja meg egy újabb helyreállítási pontot. (0x80004004).](http://social.technet.microsoft.com/wiki/contents/articles/25503.hyper-v-failed-to-fail-over-to-the-selected-recovery-point-for-virtual-machine-operation-aborted-try-a-more-recent-recovery-point-0x80004004.aspx)
  * Hello kiszolgálóval kapcsolatot nem lehetett létesített (0x00002EFD).
    * [Hyper-V tooenable fordított replikáció indítása virtuális géphez nem sikerült.](http://social.technet.microsoft.com/wiki/contents/articles/25505.a-connection-with-the-server-could-not-be-established-0x00002efd-hyper-v-failed-to-enable-reverse-replication-for-virtual-machine.aspx)
    * [Hyper-V virtuális gép virtuális gép tooenable replikálása sikertelen volt.](http://social.technet.microsoft.com/wiki/contents/articles/25506.a-connection-with-the-server-could-not-be-established-0x00002efd-hyper-v-failed-to-enable-replication-for-virtual-machine-virtual-machine.aspx)
  * [Virtuális gép nem véglegesíthető.](http://social.technet.microsoft.com/wiki/contents/articles/25508.could-not-commit-failover-for-virtual-machine.aspx)
* [hello helyreállítási tervet tartalmaz a virtuális gépek, amelyek nem tervezett feladatátvételre alkalmas állapotban.](http://social.technet.microsoft.com/wiki/contents/articles/25509.the-recovery-plan-contains-virtual-machines-which-are-not-ready-for-planned-failover.aspx)
* [hello virtuális gép nem tervezett feladatátvételre alkalmas állapotban.](http://social.technet.microsoft.com/wiki/contents/articles/25507.the-virtual-machine-isn-t-ready-for-planned-failover.aspx)
* [Virtuális gép nem fut, és ki nem üzemkész.](http://social.technet.microsoft.com/wiki/contents/articles/25510.virtual-machine-is-not-running-and-is-not-powered-off.aspx)
* [Sávon kívüli művelet történt egy virtuális gép és a feladatátvétel véglegesítését nem sikerült.](http://social.technet.microsoft.com/wiki/contents/articles/25507.the-virtual-machine-isn-t-ready-for-planned-failover.aspx)
* A feladatátvételi teszt
  * [Nem lehet kezdeményezni a feladatátvételt, mert folyamatban van a feladatátvételi teszthez.](http://social.technet.microsoft.com/wiki/contents/articles/31111.failover-could-not-be-initiated-since-test-failover-is-in-progress.aspx)
* <span style="color:green;">ÚJ</span> feladatátvételi időtúllépése "PreFailoverWorkflow feladat WaitForScriptExecutionTaskTimeout" miatt hello virtuális gép vagy a hello alhálózati toowhich hello gép társított hálózati biztonsági csoport hello toohello konfigurációs beállításai tartozik. Tekintse meg a túl["PreFailoverWorkflow feladat WaitForScriptExecutionTaskTimeout"](https://aka.ms/troubleshoot-nsg-issue-azure-site-recovery) részleteiről.

### <a name="configuration-server-process-server-master-target"></a>Konfigurációs kiszolgáló, a folyamatkiszolgáló, a fő célkiszolgáló
* [hello ESXi-állomáson, mely hello a PS/CS üzemeltetett, mivel a virtuális gépek meghiúsul, és a lila képernyő.](http://social.technet.microsoft.com/wiki/contents/articles/31107.vmware-esxi-host-experiences-a-purple-screen-of-death.aspx)

### <a name="remote-desktop-troubleshooting-after-failover"></a>Hibaelhárítás a feladatátvételt követően távoli asztal
* Sok ügyfél tapasztalt problémákkal kapcsolatos problémák tooconnect toohello átadja a feladatokat az Azure virtuális géphez. [Dokumentum tooRDP hibaelhárítási hello virtuális géppé használata hello](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).

#### <a name="adding-a-public-ip-on-a-resource-manager-virtual-machine"></a>Egy nyilvános IP-cím hozzáadása a resource manager virtuális gépen
Ha hello **Connect** hello portálon gomb szürkén jelenik meg, és nem áll az Express Route vagy helyek VPN-kapcsolaton keresztül csatlakoztatott tooAzure, toocreate kell, és rendelje a nyilvános IP-címnek a virtuális gép távoli használata előtt Asztal/megosztott rendszerhéj. Ezután felvehet egy nyilvános IP-cím hello virtuális gép hello hálózati adapteren.  

![Virtuális gép a feladatátvételt hello hálózati adapterének ad hozzá egy nyilvános IP-cím](media/site-recovery-monitoring-and-troubleshooting/createpublicip.gif)
