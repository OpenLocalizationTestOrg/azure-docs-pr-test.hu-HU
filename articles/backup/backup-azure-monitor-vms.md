---
title: "aaaMonitor erőforrás-kezelő telepített virtuális gépek biztonsági mentéseinek |} Microsoft Docs"
description: "Események és erőforrás-kezelő telepített virtuális gépek biztonsági mentéseinek riasztások figyelése. A riasztások alapján e-mail küldése."
services: backup
documentationcenter: dev-center-name
author: markgalioto
manager: carmonm
editor: 
ms.assetid: fed32015-2db2-44f8-b204-d89f6fd1bea2
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/21/2016
ms.author: markgal;trinadhk;giridham;
ms.openlocfilehash: bf45cbaa05621b2365c26bafa1bd8223a444c1fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-alerts-for-azure-virtual-machine-backups"></a>Azure-beli virtuális gépek biztonsági mentésével kapcsolatos riasztások figyelése
A riasztások jelezhetik, hogy az esemény küszöbét teljesül vagy túllépése hello szolgáltatás válaszát. Annak ismerete, ha problémák start lehet kritikus tookeeping üzleti költségek le. Riasztások általában nem történik meg, ütemezés szerint, és így ez hasznos tooknow minél hamarabb után riasztás akkor jön létre. Például ha egy biztonsági mentési vagy helyreállítási feladat sikertelen lesz, egy riasztás hello hiba öt percen belül. Hello tároló irányítópultjának a biztonsági riasztások csempe hello kritikus és a figyelmeztetési szintű eseményeket jeleníti meg. Hello biztonsági riasztások beállításainak megtekintheti az összes esemény. De mi a teendő, ha egy riasztás, ha egy külön probléma dolgozik? Ha nem tudja, ha hello riasztás történik, akkor lehet, hogy egy kisebb kellemetlenségért, vagy az adatok biztonságát veszélyeztető. riasztás - toomake meg arról, hogy hello megfelelő személyek ismerjék következik be, amikor konfigurálása hello szolgáltatás toosend riasztási értesítéseket e-mailben. E-mail értesítések beállításával kapcsolatos részletekért lásd: [értesítések konfigurálása](backup-azure-monitor-vms.md#configure-notifications).

## <a name="how-do-i-find-information-about-hello-alerts"></a>Hogyan találhatom hello riasztások adatai?
tooview információk hello eseményről, amelyek kivételt váltott ki riasztást, meg kell nyitnia hello biztonsági riasztások panel. Nincsenek a két módon tooopen hello biztonsági riasztások panel: biztonsági riasztások csempe hello tároló irányítópult hello, vagy hello riasztások és események panelen.

tooopen hello biztonsági riasztások panel a biztonsági riasztások csempén:

* A hello **biztonsági mentési riasztások** hello tároló irányítópult csempét, kattintson a **kritikus** vagy **figyelmeztetés** tooview hello adott súlyossági szint működési eseményeit.

    ![Biztonsági riasztások csempe](./media/backup-azure-monitor-vms/backup-alerts-tile.png)

tooopen hello biztonsági riasztások panel hello riasztások és események panelen:

1. Hello tároló irányítópultján kattintson **összes beállítás**. ![Minden beállítások gomb](./media/backup-azure-monitor-vms/all-settings-button.png)
2. A hello **beállítások** panelen kattintson a **riasztások és események**. ![Riasztások és események gomb](./media/backup-azure-monitor-vms/alerts-and-events-button.png)
3. A hello **riasztások és események** panelen kattintson a **biztonsági mentési riasztások**. ![Biztonsági riasztások gomb](./media/backup-azure-monitor-vms/backup-alerts.png)

    Hello **biztonsági mentési riasztások** panel nyílik meg, és megjeleníti hello szűrt riasztásokat.

    ![Biztonsági riasztások csempe](./media/backup-azure-monitor-vms/backup-alerts-critical.png)
4. tooview részletes információk egy adott riasztás események listája hello kattintva hello riasztási tooopen a **részletek** panelen.

    ![Esemény részletei](./media/backup-azure-monitor-vms/audit-logs-event-detail.png)

    toocustomize hello attribútumok hello lista megjelenik, lásd: [további esemény attribútumok megtekintése](backup-azure-monitor-vms.md#view-additional-event-attributes)

## <a name="configure-notifications"></a>Értesítések konfigurálása
 Hello szolgáltatás toosend értesítő e-mailek keresztül hello óránként túlra, vagy ha az adott esemény történik előfordult hello riasztások konfigurálása

a riasztások értesítő e-mailek mentése tooset

1. Biztonsági riasztások menüjének hello **értesítések konfigurálása**

    ![Biztonsági riasztások menü](./media/backup-azure-monitor-vms/backup-alerts-menu.png)

    hello konfigurálása értesítések panel nyílik meg.

    ![Értesítések panel konfigurálása](./media/backup-azure-monitor-vms/configure-notifications.png)
2. Hello konfigurálása értesítések paneljén értesítő e-mailek kattintson **a**.

    hello címzettek és súlyosság párbeszédpanelek lehetnek egy csillag következő toothem, mert az adatszolgáltatás. Adjon meg legalább egy e-mail címet, és legalább egy súlyosságának kiválasztása.
3. A hello **címzettek (E-mail)** párbeszédpanelen típus hello tartozó e-mail-címeket hello értesítéseket kapnak. Hello formátum használata: username@domainname.com. Az e-mail címeket pontosvesszővel (;) kell elválasztani.
4. A hello **értesítendő** területen válasszon **egy riasztás** toosend értesítés hello megadott riasztás, vagy **óránkénti kivonatoló** toosend az elmúlt egy órában hello összegzését.
5. A hello **súlyossági** párbeszédpanelen válassza ki a megjeleníteni kívánt tootrigger egy vagy több szint e-mailes értesítések.
6. Kattintson a **Save** (Mentés) gombra.

   ### <a name="what-alert-types-are-available-for-azure-iaas-vm-backup"></a>Milyen típusú érhetők el az Azure infrastruktúra-szolgáltatási virtuális gép biztonsági mentése?
   | Riasztási szint | Értesítések küldése |
   | --- | --- |
   | Kritikus |Sikertelen biztonsági mentéshez, helyreállítási hiba |
   | Figyelmeztetés |None |
   | Tájékoztató |None |

### <a name="are-there-situations-where-email-isnt-sent-even-if-notifications-are-configured"></a>Olyankor is előfordul, hogy nem érkezik meg az e-mail, ha az értesítések be vannak állítva?
Nincsenek olyan helyzetekben, ahol a rendszer nem küld figyelmeztetést, annak ellenére, hogy hello értesítések megfelelően vannak konfigurálva. Hello a következő helyzetekben értesítő e-mailek nem kap riasztási zaj tooavoid:

* Ha az értesítések konfigurált tooHourly Digest, és egy riasztás jelenik meg, és hello órán belül megoldott.
* hello feladat megszakadt.
* A biztonsági mentési feladat elindul, és akkor sikertelen lesz, és folyamatban van egy másik biztonsági mentési feladat.
* A Resource Manager-kompatibilis virtuális gépek ütemezett biztonsági mentési feladat indítja el, de hello virtuális gép már nem létezik.

## <a name="customize-your-view-of-events"></a>A nézet események testreszabása
Hello **naplók** beállítást tartalmaz egy előre definiált szűrők és oszlopok működési események adatait. Testre szabhatja a hello nézet, hogy mikor hello **események** panel nyílik meg, ez azt mutatja meg hello kívánt információkat.

1. A hello [tároló irányítópult](backup-azure-manage-vms.md#open-a-recovery-services-vault-in-the-dashboard), kattintson a Tallózás tooand **naplók** tooopen hello **események** panelen.

    ![Naplók](./media/backup-azure-monitor-vms/audit-logs-1606-1.png)

    Hello **események** panel megnyitása toohello csak az aktuális tárolóban hello szűrt működési eseményeit.

    ![Naplózási szűrő naplók](./media/backup-azure-monitor-vms/audit-logs-filter.png)

    hello panel listáját jeleníti meg hello kritikus, hiba, figyelmeztetés és információs események, hello az elmúlt héten. hello időtartamának az alapértelmezett érték a hello beállítása **szűrő**. Hello **események** panel is bemutatja a dokumentumkövetést, miközben hello események sávdiagram. Ha nem szeretné, hogy toosee hello sávdiagram, a hello **események** menüben kattintson a **elrejtése diagram** tootoggle hello diagram ki. hello alapértelmezett nézet események művelet, állapotáról, erőforrás, és időt információkat jeleníti meg. Az ilyen esemény további attribútumok kapcsolatos információkért lásd: hello szakasz [eseményadatok bővülő](backup-azure-monitor-vms.md#view-additional-event-attributes).
2. További információt a hello működési eseménynél **művelet** oszlop, egy működési események tooopen kattintson a panel. hello panel hello események részletes információkat tartalmaz. Események a korrelációs azonosító és hello események hello időtartamának történt szerint vannak csoportosítva.

    ![Művelet részletei](./media/backup-azure-monitor-vms/audit-logs-details-window.png)
3. tooview részletes információk egy adott esemény események listája hello kattintva hello esemény tooopen a **részletek** panelen.

    ![Esemény részletei](./media/backup-azure-monitor-vms/audit-logs-details-window-deep.png)

    hello Eseményszint információk részletes adatokat lekérdezi hello is. Ha inkább a jelent meg minden esemény ennyi adatait, és szeretné tooadd Ez sokkal részletességi toohello **események** panelen hello című [eseményadatok bővülő](backup-azure-monitor-vms.md#view-additional-event-attributes).

## <a name="customize-hello-event-filter"></a>Hello szűrő testreszabása
Használjon hello **szűrő** tooadjust vagy egy adott panelen megjelenő adatok hello válasszon. toofilter hello eseményadatok:

1. A hello [tároló irányítópult](backup-azure-manage-vms.md#open-a-recovery-services-vault-in-the-dashboard), kattintson a Tallózás tooand **naplók** tooopen hello **események** panelen.

    ![Naplók](./media/backup-azure-monitor-vms/audit-logs-1606-1.png)

    Hello **események** panel megnyitása toohello csak az aktuális tárolóban hello szűrt működési eseményeit.

    ![Naplózási szűrő naplók](./media/backup-azure-monitor-vms/audit-logs-filter.png)
2. A hello **események** menüben kattintson a **szűrő** tooopen adott panelhez.

    ![Nyissa meg a szűrő panelre](./media/backup-azure-monitor-vms/audit-logs-filter-button.png)
3. A hello **szűrő** panelen hello beállítása **szint**, **időtartamának**, és **hívó** szűrők. hello más szűrők nem érhetők el óta tooprovide hello aktuális információk voltak beállítva a hello Recovery Services-tároló.

    ![Naplózási naplók-lekérdezés részletei](./media/backup-azure-monitor-vms/filter-blade.png)

    Megadhatja a hello **szint** esemény: kritikus, hiba, figyelmeztető vagy tájékoztató. Eseményszinttel bármilyen kombinációját kiválaszthatja, de rendelkeznie kell legalább egy kijelölt szint. Váltás hello szint a be- és kikapcsolása. Hello **időtartamának** szűrő segítségével toospecify hello mennyi ideig az események rögzítésére. Ha használja az egyéni időtartama, beállíthatja hello kezdési és befejezési időpontja.
4. Ha készen áll a tooquery hello műveletek használatával a naplókat, a szűrő, kattintson a **frissítés**. eredményekben hello hello **események** panelen.

    ![Művelet részletei](./media/backup-azure-monitor-vms/edited-list-of-events.png)

### <a name="view-additional-event-attributes"></a>Az esemény attribútumok megtekintése
Hello segítségével **oszlopok** gomb, engedélyezheti az esemény attribútumok tooappear hello hello listában szereplő **események** panelen. események listája hello alapértelmezett műveletet, állapotáról, erőforrás, és időt információit jeleníti meg. tooenable további attribútumok:

1. A hello **események** panelen kattintson a **oszlopok**.

    ![Nyissa meg oszlopok](./media/backup-azure-monitor-vms/audi-logs-column-button.png)

    Hello **oszlopok kiválasztása** panel nyílik meg.

    ![Oszlopok panel](./media/backup-azure-monitor-vms/columns-blade.png)
2. tooselect hello attribútumot, hello jelölőnégyzetet. hello attribútum jelölőnégyzet váltja ki- és kikapcsolható.
3. Kattintson a **alaphelyzetbe** tooreset hello lista hello attribútumok **események** panelen. Miután hozzáadása vagy attribútumok eltávolítása hello listájáról, használja **alaphelyzetbe** tooview hello új esemény attribútumok listáját.
4. Kattintson a **frissítés** tooupdate hello adatok hello esemény attribútumok. a következő táblázat hello minden attribútum információkat biztosít.

| Oszlop neve | Leírás |
| --- | --- |
| Művelet |hello hello művelet neve |
| Szint |hello szint hello művelet, az értékek lehetnek: információs, figyelmeztetési, hiba vagy kritikus |
| status |Hello műveletet leíró állapota |
| Erőforrás |URL-címet, amely azonosítja a hello erőforrás; más néven hello erőforrás-azonosító |
| Time |Idő mért hello aktuális idő, amikor hello esemény történt |
| Hívó |Ki vagy mi nevezik, és aktivált hello esemény; hello rendszer vagy egy felhasználó lehet |
| időbélyeg |Ha hello esemény lett elindítva, amely hello idő |
| Erőforráscsoport |hello kapcsolódó erőforráscsoport |
| Erőforrás típusa |hello használt belső erőforrástípus-kezelő által |
| Előfizetés azonosítója |hello tartozó előfizetés-azonosító |
| Kategória |Kategória hello esemény |
| Korrelációs azonosító |Az ezzel kapcsolatos eseményeket közös azonosítója |

## <a name="use-powershell-toocustomize-alerts"></a>PowerShell toocustomize riasztások
Hello feladatok egyéni riasztási értesítéseket kaphat hello portálon. Ezek a feladatok tooget hello működési szabályok naplóz eseményeket PowerShell-alapú riasztás határozza meg. Használjon *PowerShell 1.3.0 verzió vagy újabb*.

a biztonsági mentési hibák, az értesítő üzenet egyéni szövegében tooalert toodefine hasonló hello parancsfájl a következő parancsot használja:

```
PS C:\> $actionEmail = New-AzureRmAlertRuleEmail -CustomEmail contoso@microsoft.com
PS C:\> Add-AzureRmLogAlertRule -Name backupFailedAlert -Location "East US" -ResourceGroup RecoveryServices-DP2RCXUGWS3MLJF4LKPI3A3OMJ2DI4SRJK6HIJH22HFIHZVVELRQ-East-US -OperationName Microsoft.RecoveryServices/recoveryServicesVault/Backup -Status Failed -TargetResourceId /subscriptions/86eeac34-eth9a-4de3-84db-7a27d121967e/resourceGroups/RecoveryServices-DP2RCXUGWS3MLJF4LKPI3A3OMJ2DI4SRJK6HIJH22HFIHZVVELRQ-East-US/providers/Microsoft.RecoveryServices/vaults/trinadhVault -Actions $actionEmail
```

**ResourceId** : letölthető ResourceId hello naplókat. hello ResourceId hello hello műveletnaplók oszlopában erőforrás a megadott URL-címet.

**OperationName** : hello formátumban van OperationName "Microsoft.RecoveryServices/recoveryServicesVault/*EventName*" ahol *EventName* lehet:<br/>

* Regisztráljon <br/>
* Regisztrálás törlése <br/>
* ConfigureProtection <br/>
* Biztonsági mentés <br/>
* Visszaállítás <br/>
* StopProtection <br/>
* DeleteBackupData <br/>
* CreateProtectionPolicy <br/>
* DeleteProtectionPolicy <br/>
* UpdateProtectionPolicy <br/>

**Állapot** : támogatott értékek a következők elindítva, sikeres vagy sikertelen.

**Erőforráscsoport** : Ez a hello erőforráscsoporthoz toowhich hello erőforrás tartozik. Hello erőforráscsoport oszlop toohello létrehozott naplók adhat hozzá. Erőforráscsoport egy hello elérhető típusú események adatait.

**Név** : hello riasztási szabály nevét.

**CustomEmail** : Adja meg a kívánt riasztási értesítéshez toosend hello egyéni e-mail cím toowhich

**SendToServiceOwners** : Ez a beállítás elküldi a riasztási értesítések tooall rendszergazdák és a társadminisztrátorok hello előfizetés. A használat **New-AzureRmAlertRuleEmail** parancsmag

### <a name="limitations-on-alerts"></a>Riasztások korlátozásai
Eseményalapú riasztások az éppen tulajdonos toohello a következő korlátozások vonatkoznak:

1. A figyelmeztetéseket hello Recovery Services-tároló összes virtuális gépeken. A Recovery Services-tároló virtuális gépek egy részét hello riasztás nem szabhatja testre.
2. A funkció jelenleg előzetes verzió. [További információ](../monitoring-and-diagnostics/insights-powershell-samples.md#create-metric-alerts)
3. Küldi a riasztásokat "alerts-noreply@mail.windowsazure.com". Hello e-mailt olyasvalaki jelenleg nem módosíthatók.

## <a name="next-steps"></a>Következő lépések
Eseménynaplók kiváló levágást engedélyezése, és naplózási hello biztonsági mentési műveletek támogatása. a következő műveletek hello bejelentkezett:

* Regisztráljon
* Regisztrálás törlése
* A védelem konfigurálása
* Biztonsági mentés (mindkettő ütemezett és igény szerinti biztonsági mentést)
* Visszaállítás
* Állítsa le a védelmet
* biztonsági mentési adatok törlése
* Házirend hozzáadása
* Szabályzat törlése
* Házirend frissítése
* Megszakítása

Széles körű leírását az események, a műveletek és a naplók közötti hello Azure-szolgáltatásokkal, tekintse meg hello cikket, [események megtekintése és a naplók](../monitoring-and-diagnostics/insights-debugging-with-events.md).

Hozza létre újra a virtuális gép helyreállítási pontból információkért tekintse meg [visszaállítása az Azure virtuális gépek](backup-azure-restore-vms.md). Ha a virtuális gépek védelméről tájékoztatásra van szüksége, tekintse meg [először: készítsen biztonsági mentést a Recovery Services-tároló virtuális gépek tooa](backup-azure-vms-first-look-arm.md). További tudnivalók: hello felügyeleti feladatokat a virtuális gép biztonsági mentésekhez hello cikkben [kezelése Azure virtuális gépek biztonsági mentéseinek](backup-azure-manage-vms.md).
