---
title: " Az Azure Recovery Services-tároló törlése |} Microsoft Docs "
description: "Hogyan toodelete az Azure biztonsági mentési és helyreállítási szolgáltatások tároló. Egy mentési tárolót egy Azure-felhőbe tárolóban, vagy az Azure recovery-tároló hívható. Problémák elhárítása hello klasszikus portálon vagy az Azure-portálon a mentési tároló nem törölhető."
services: service-name
documentationcenter: dev-center-name
author: markgalioto
manager: carmonm
editor: 
ms.assetid: 5fa08157-2612-4020-bd90-f9e3c3bc1806
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 08/11/2017
ms.author: markgal;trinadhk
ms.openlocfilehash: 9047f50f4b2c991fbf2454ddcad08073ec7cd975
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="delete-a-recovery-services-vault"></a>Recovery Services-tároló törlése
hello Azure Backup szolgáltatás tárolók - hello Backup-tárolóban és a Recovery Services-tároló hello két típusa van. hello biztonsági mentés tárolójának első kapott. Recovery Services-tároló hello majd mentén toosupport kibontva hello Resource Manager üzembe helyezések kapott. Hello miatt bővített képességek és hello tárolóban, törlés, biztonsági mentési vagy helyreállítási szolgáltatások tárolóban kell tárolni hello információk függőségeit zavaró lehet. Ez a cikk azt ismerteti, hogyan toodelete hello tárolók hello klasszikus portál és hello Azure-portálon.  

| **Központi telepítési típus** | **Portál** | **Tároló neve** |
| --- | --- | --- |
| Klasszikus |Klasszikus |Mentési tároló |
| Resource Manager |Azure |Recovery Services-tároló |

> [!NOTE]
> A Backup-tárolók nem tudják megvédeni a Resource Manager által üzembe helyezett megoldásokat. Azonban használhatja a Recovery Services-tároló tooprotect classically telepített kiszolgálók és virtuális gépek.  
>

> [!IMPORTANT]
> Most már frissítheti a mentési tárolók tooRecovery szolgáltatások tárolókban. További információkért lásd: hello cikk [frissíteni a biztonsági mentési tároló tooa Recovery Services-tároló](backup-azure-upgrade-backup-to-recovery-services.md). A Microsoft tooupgrade támogatja a mentési tárolókat tooRecovery szolgáltatások tárolók.<br/> **2017. október 15**, akkor nem fog tudni toouse PowerShell toocreate mentési tárolókban. <br/> **2017. november 1-től kezdődően**:
>- A többi biztonsági mentési tárolók lesz automatikusan frissített tooRecovery szolgáltatások tárolók.
>- Ön nem fogja tudni tooaccess a biztonsági mentési adatok hello a klasszikus portálon. Ehelyett használjon hello Azure portál tooaccess a biztonsági mentési adatok a Recovery Services-tárolók.
>

Ez a cikk használjuk, hello kifejezés, tároló, toorefer toohello általános űrlapot hello Backup-tárolóban és a Recovery Services-tároló. Ha a szükséges toodistinguish hello tárolók közötti hello formális nevét, a mentési tároló és a Recovery Services-tároló, használjuk.

## <a name="deleting-a-recovery-services-vault"></a>Recovery Services-tároló törlése
Recovery Services-tároló törlése adatbázisunk - *hello tároló nem tartalmaz semmilyen erőforráshoz megadott*. Recovery Services-tároló törlése előtt távolítsa el, vagy törli az összes erőforrást hello tárolóban lévő állapottal. Ha toodelete erőforrásokat tartalmazó kísérli meg, például a következő kép hello hibaüzenetet kap:

![Tároló törlése hiba](./media/backup-azure-delete-vault/vault-deletion-error.png) <br/>

Hello erőforrások hello tárolóból törölve lett, amíg kattintva **újra** előállított hello ugyanezt a hibaüzenetet. Ha most rögzített, a hibaüzenet, kattintson a **Mégse** és használata hello alábbi lépéseit toodelete hello erőforrások hello tárolóban.

### <a name="removing-hello-items-from-a-vault-protecting-a-vm"></a>Hello elemek eltávolítása a tárolóból, a virtuális gépek védelméhez
Ha már rendelkezik hello Recovery Services nyissa meg a tároló, akkor hagyja toohello második.

1. Nyissa meg a hello Azure-portálon, és az irányítópult hello nyissa meg a kívánt toodelete hello tárolóban.

   Ha még nem rendelkezik a Recovery Services-tároló hello toohello irányítópult, a központ menüben hello rögzítve, kattintson a **több szolgáltatások** hello az erőforrások listájához, írja be a **Recovery Services**. Írja be megkezdése előtt, hello szűrők megjelenítése a megadott feltételeknek. Kattintson a **Recovery Services-tárolók**.

   ![Recovery Services-tároló létrehozása – 1. lépés](./media/backup-azure-delete-vault/open-recovery-services-vault.png) <br/>

   Recovery Services-tárolók hello listája jelenik meg. Hello listáról válassza ki a kívánt toodelete hello tárolóban.

   ![tároló válasszon a listából](./media/backup-azure-work-with-vaults/choose-vault-to-delete.png)
2. A hello tároló nézetben keresse meg a hello **Essentials** ablaktáblán. egy tároló toodelete bármely védett elemek nem lehet. Ha megjelenik egy száma nem nulla, vagy a **biztonsági mentés elemek** vagy **biztonsági mentése a felügyeleti kiszolgálók**, hello tároló törlése előtt el kell távolítania azokat az elemeket.

    ![Nézze meg védett elemek Essentials ablaktábla](./media/backup-azure-delete-vault/contoso-bkpvault-settings.png)

    Virtuális gépek és a fájlok és mappák biztonsági mentés elemek minősülnek, és hello szereplő **biztonsági mentés elemek** hello Essentials ablaktábla területére. Hello szerepel egy DPM-kiszolgáló **Management Server biztonsági másolat** hello Essentials ablaktábla területére. **Replikált elemek** toohello Azure Site Recovery szolgáltatás vonatkoznak.
3. hello eltávolítása toobegin védett elemek hello tárolóból keresés hello elemek hello tárolóban lévő állapottal. Hello tároló irányítópulton kattintson **beállítások**, és kattintson a **biztonsági mentési elemek** tooopen adott panelhez.

    ![tároló válasszon a listából](./media/backup-azure-delete-vault/open-settings-and-backup-items.png)

    Hello **biztonsági mentés elemek** panel külön listák alapján hello elemtípussal rendelkezik: Azure virtuális gépek vagy a fájl-mappák (lásd a kép). hello alapértelmezett típusa listája látható az Azure virtuális gépeken. Fájl-mappák elemek hello tárolóban, jelölje be a tooview hello listáját **-mappákban** hello legördülő menüből.
4. Egy elem törlése a virtuális gépek védelméhez hello tárolóból, előtt kell hello elemhez tartozó biztonsági mentési feladat leállítása és hello helyreállításipont-adatokat törli. Minden elemhez hello tárolóban kövesse az alábbi lépéseket:

    a. A hello **biztonsági másolati elemei** panelen kattintson a jobb gombbal hello elemet, és hello helyi menüből válassza a **Stop biztonsági mentés**.

    ![hello biztonsági mentési feladat leállítása](./media/backup-azure-delete-vault/stop-the-backup-process.png)

    hello állítsa le a biztonsági mentés panel nyílik meg.

    b. A hello **állítsa le a biztonsági mentés** paneljén hello **válasszon egy lehetőséget** menüben válassza **biztonságimásolat-adatok törlése** > hello hello elem neve > kattintson **leállítása biztonsági mentési**.

    Hello típusnév hello elem tooverify toodelete kívánja azt. Hello **állítsa le a biztonsági mentés** gomb akkor aktiválódik, ha hello elem ellenőrzése. Hello párbeszédpanel bezárásához tootype hello hello biztonsági mentési elem neve nem látható, ha a kiválasztott hello **biztonsági mentési adatok megőrzése** lehetőséget.

    ![biztonsági mentési adatok törlése](./media/backup-azure-delete-vault/stop-backup-blade-delete-backup-data.png)

    Igény szerint is adja meg a okát, miért hello adatait törölni, majd adja meg megjegyzéseit. Miután rákattintott **állítsa le a biztonsági mentés**, hello törlési feladat toocomplete engedélyezése előtt toodelete hello tárolóban. tooverify, amely hello feladat befejeződött, hello Azure üzenetek ellenőrzése ![biztonsági mentési adatok törlése](./media/backup-azure-delete-vault/messages.png). <br/>
    Hello feladat befejezése után kapni üzenet jelenik meg a hello biztonsági mentési folyamat le lett állítva, és hello biztonsági mentési adatok számára, hogy az elem törlése megtörtént.

    c. Hello listaelem hello a törlése után **biztonsági mentés elemek** menüben kattintson **frissítése** toosee hello fennmaradó elemek hello tárolóban lévő állapottal.

      ![biztonsági mentési adatok törlése](./media/backup-azure-delete-vault/empty-items-list.png)

      Ha nincsenek elemek hello listában, görgessen toohello **Essentials** hello biztonsági mentési tároló panelen ablaktáblán. Nincs nem lehet bármely **elem biztonsági mentését**, **biztonsági mentése a felügyeleti kiszolgálók**, vagy **replikált elemek** szerepel a listában. Ha elemek még mindig hello tárolóban, toostep három vissza, és válasszon egy másik elem típusa listát.  
5. Ha nincsenek további elemek hello tároló eszköztárban, kattintson **törlése**.

    ![biztonsági mentési adatok törlése](./media/backup-azure-delete-vault/delete-vault.png)
6. Kattintson a megjeleníteni kívánt toodelete hello tárolóban, tooverify **Igen**.

    hello tároló törlődik, és hello portál által megjelenített toohello **új** szolgáltatás menü.

## <a name="what-if-i-stopped-hello-backup-process-but-retained-hello-data"></a>Mi történik, ha hello biztonsági mentési folyamat leállt, de megőrzi hello adatokat?
De véletlenül a hello biztonsági mentési folyamat leállítása *maradnak* hello adatok, törölnie kell hello biztonsági mentési adatok hello tároló törlése előtt. toodelete hello biztonsági mentési adatokat:

1. A hello **biztonsági másolati elemei** panelen kattintson a jobb gombbal hello elemet, és hello helyi menüben kattintson a **biztonsági mentési adatok törlése**.

    ![biztonsági mentési adatok törlése](./media/backup-azure-delete-vault/delete-backup-data-menu.png)

    Hello **biztonságimásolat-adatok törlése** panel nyílik meg.
2. A hello **biztonságimásolat-adatok törlése** paneljén, típusnév hello hello elemet, majd kattintson **törlése**.

    ![biztonsági mentési adatok törlése](./media/backup-azure-delete-vault/delete-retained-vault.png)

    Hello adatok törlését, ha 4c toostep vissza, és hello folytassa.

## <a name="delete-a-vault-used-tooprotect-a-dpm-server"></a>Egy tároló használt tooprotect a DPM-kiszolgáló törlése
Egy tároló használt tooprotect a DPM-kiszolgáló törlése előtt törölje a létrehozott helyreállítási pontot, és az majd szüntesse meg a hello tárolójából hello kiszolgáló.

egy védelmi csoportba tartozó toodelete hello adatokat:

1. Hello DPM felügyeleti konzolt, kattintson **védelmi** > Válasszon ki egy védelmi csoportot > Válassza ki a védelmi csoport tagjához hello > hello eszközsávon kattintson **eltávolítása**.

  Jelölje be hello védelmi csoport tagjához tooactivate hello **eltávolítása** hello menüszalagon gombjára. Hello példában hello tagja **dummyvm9**. tooselect hello védelmi csoportban, több tagot hello Ctrl billentyűt lenyomva tartva hello tagjain kattintson.

    ![biztonsági mentési adatok törlése](./media/backup-azure-delete-vault/az-portal-delete-protection-group.png)

    Hello **védelem kikapcsolása** párbeszédpanel nyílik meg.
2. A hello **védelem kikapcsolása** párbeszédablakban válassza **védett adatok törlése**, és kattintson a **védelem kikapcsolása**.

    ![biztonsági mentési adatok törlése](./media/backup-azure-delete-vault/delete-dpm-protection-group.png)

    egy tároló toodelete, állítsa törölje, vagy törölje a, hello tároló a védett adatok. Attól függően, hogy a helyreállítási pontokhoz, illetve hello védelmi csoport adatainak hello számát azt is eltarthat néhány másodpercig tooseveral perc toodelete hello adatokat. Hello **védelem kikapcsolása** párbeszédpanel hello állapotát jeleníti meg, amikor hello feladat befejeződött.

    ![biztonsági mentési adatok törlése](./media/backup-azure-delete-vault/success-deleting-protection-group.png)
3. A folyamat folytatásához az összes védelmi csoport összes tagjához.

    Távolítsa el az összes védett adatok és a védelmi csoportban.
4. Ha töröl minden tag hello védelmi csoportból, váltson toohello Azure-portálon. Hello tároló irányítópult megnyitásához, és győződjön meg arról, hogy nincsenek nem **biztonsági mentés elemek**, **biztonsági mentése a felügyeleti kiszolgálók**, vagy **replikált elemek**. Hello tároló eszköztáron kattintson **törlése**.

    ![biztonsági mentési adatok törlése](./media/backup-azure-delete-vault/delete-vault.png)

    Ha mentési felügyeleti kiszolgáló regisztrálva toohello tárolóval, akkor is, ha nincs adat hello tárolóban hello tároló nem törölhető. Ha hello biztonságimásolat-felügyeleti kiszolgálók hello tárolóhoz társított törölt, de nincsenek hello felsorolt kiszolgálók **Essentials** ablaktáblában tekintse meg [keresés hello mentési felügyeleti kiszolgálók regisztrált toohello tároló](backup-azure-delete-vault.md#find-the-backup-management-servers-registered-to-the-vault) .
5. Kattintson a megjeleníteni kívánt toodelete hello tárolóban, tooverify **Igen**.

    hello tároló törlődik, és hello portál által megjelenített toohello **új** szolgáltatás menü.

## <a name="delete-a-vault-used-tooprotect-a-production-server"></a>Egy tároló használt tooprotect egy üzemi kiszolgáló törlése
Egy tároló használt tooprotect egy üzemi kiszolgáló törlése előtt törölje, vagy szüntesse meg a hello tárolójából hello kiszolgáló.

toodelete a hello tárolóhoz társított hello az üzemi kiszolgáló:

1. Hello Azure-portálon, a hello tároló irányítópult megnyitásához, és kattintson a **beállítások** > **biztonsági infrastruktúra** > **az üzemi kiszolgálók**.

    ![Nyissa meg az üzemi kiszolgálók panel](./media/backup-azure-delete-vault/delete-production-server.png)

    Hello **az üzemi kiszolgálók** panel megnyílik, és megjeleníti az összes éles kiszolgálók hello tárolóban.

    ![az üzemi kiszolgálók listája](./media/backup-azure-delete-vault/list-of-production-servers.png)
2. A hello **az üzemi kiszolgálók** panelen kattintson a jobb gombbal a hello kiszolgálón, és kattintson a **törlése**.

    ![az üzemi kiszolgáló törlése ](./media/backup-azure-delete-vault/delete-server-on-production-server-blade.png)

    Hello **törlése** panel nyílik meg.

    ![az üzemi kiszolgáló törlése ](./media/backup-azure-delete-vault/delete-blade.png)
3. A hello **törlése** panelen hagyja jóvá a hello kiszolgáló nevét, majd kattintson **törlése**. Adjon megfelelő nevet hello kiszolgáló, tooactivate hello **törlése** gombra.

    Hello tároló a törölt kap egy üzenetet, amely meghatározza, hogy hello tároló törölve lett. Ha töröl minden kiszolgáló hello tárolóban, görgessen hello tároló irányítópult hátsó toohello Essentials ablaktábláján.
4. Hello tároló irányítópult, győződjön meg arról, nincsenek nem **biztonsági mentés elemek**, **biztonsági mentése a felügyeleti kiszolgálók**, vagy **replikált elemek**. Hello tároló eszköztáron kattintson **törlése**.
5. Kattintson a megjeleníteni kívánt toodelete hello tárolóban, tooverify **Igen**.

    hello tároló törlődik, és hello portál által megjelenített toohello **új** szolgáltatás menü.

## <a name="delete-a-backup-vault-in-classic-portal"></a>A klasszikus portálon biztonsági mentési tároló törlése
hello alábbi utasítások alapján vannak a biztonsági másolatok tárolóját a klasszikus portálon hello törléséhez. Hello mentési tároló törlése előtt törölnie kell hello helyreállítási pontokat, vagy biztonsági másolatba mentett elemek, majd távolítsa el a hello regisztrálva kiszolgálók. hello regisztrált kiszolgálókat: hello Windows Server, munkaállomást vagy azelőtt regisztrált toohello tároló virtuális gépet.

1. Nyissa meg hello [klasszikus portál](https://manage.windowsazure.com).

2. A mentési tárolók hello listáról válassza ki a kívánt toodelete hello tárolóban.

    ![biztonsági mentési adatok törlése](./media/backup-azure-delete-vault/classic-portal-delete-vault-open-vault.png)

    hello tároló irányítópult megnyitása. Nézze meg a Windows-kiszolgálók és/vagy Azure virtuális gépek hello tárolóhoz társított hello száma. Is tekintse meg az Azure-ban felhasznált hello teljes tárterület. Állítsa le az összes biztonsági mentési feladat, és hello tároló törlése előtt törölje annak összes adatot.

3. Kattintson a hello **védett elemek** fülre, majd **védelem kikapcsolása**

    ![biztonsági mentési adatok törlése](./media/backup-azure-delete-vault/classic-portal-delete-vault-stop-protect.png)

    Hello **állítsa le a védelmet a tároló"** párbeszédpanel jelenik meg.
4. A hello **állítsa le a védelmet a tároló"** párbeszédpanelen a jelölőnégyzet **Delete tartozó biztonsági mentési adatok** kattintson ![pipa](./media/backup-azure-delete-vault/checkmark.png). <br/>
    Szükség esetén válassza ki a védelem leállítása okát, és adja meg a megjegyzéseit.

    ![biztonsági mentési adatok törlése](./media/backup-azure-delete-vault/classic-portal-delete-vault-verify-stop-protect.png)

    Ha töröl hello elemek hello tárolóban, hello tároló üres lesz.

    ![biztonsági mentési adatok törlése](./media/backup-azure-delete-vault/classic-portal-delete-vault-post-delete-data.png)
5. Hello lévő lapot, kattintson **regisztrált elemek**. Hello **típus** legördülő menü, lehetővé teszi, hogy toochoose hello írja be a regisztrált kiszolgáló toohello tároló. hello típus Windows Server vagy Azure virtuális gép lehet. A következő példa hello, jelölje ki a hello virtuális gép regisztrált toohello tárolóban, és kattintson **Unregister**.

    ![biztonsági mentési adatok törlése](./media/backup-azure-delete-vault/classic-portal-unregister.png)

  Ha azt szeretné, toodelete hello regisztrációja egy, a Windows Server, a hello **típus** legördülő menüben válassza **Windows Server**, kattintson a ![pipa](./media/backup-azure-delete-vault/checkmark.png) toorefresh üdvözlő képernyőt, majd **törlése**. <br/>

  ![Válassza ki a Windows Server](./media/backup-azure-delete-vault/select-windows-server.png)

6. Hello lévő lapot, kattintson **irányítópult** tooopen, amely a lapon. Ellenőrizze, hogy nincsenek regisztrált kiszolgálókat vagy védett hello felhőben Azure virtuális gépeken. Azt is ellenőrizze, nincs megőrzés adat. Kattintson a **törlése** toodelete hello tárolóban.

    ![biztonsági mentési adatok törlése](./media/backup-azure-delete-vault/classic-portal-list-of-tabs-dashboard.png)

    Megnyílik a hello törlése biztonsági mentési tároló visszaigazoló képernyő. Jelöljön ki egy lehetőséget miért törölni hello tárolóban, és kattintson ![pipa](./media/backup-azure-delete-vault/checkmark.png). <br/>

    ![biztonsági mentési adatok törlése](./media/backup-azure-delete-vault/classic-portal-delete-vault-confirmation-1.png)

    hello tároló törlődik, és ismét toohello klasszikus portál Irányítópultjára.

### <a name="find-hello-backup-management-servers-registered-toohello-vault"></a>Hello biztonsági mentés felügyeleti kiszolgálók regisztrált toohello tárolóban található
Ha több kiszolgáló regisztrálva tooa tárolóban, nehéz tooremember lehet őket. toosee hello kiszolgálók toohello tárolóban regisztrált, és törölje őket:

1. Nyissa meg hello tároló irányítópult.
2. A hello **Essentials** ablaktáblán kattintson a **beállítások** tooopen adott panelhez.

    ![Nyissa meg a beállítások panel](./media/backup-azure-delete-vault/backup-vault-click-settings.png)
3. A hello **beállítások panel**, kattintson a **biztonsági infrastruktúra**.
4. A hello **biztonsági infrastruktúra** panelen kattintson a **biztonságimásolat-felügyeleti kiszolgálók**. hello biztonságimásolat-felügyeleti kiszolgálók panel nyílik meg.

    ![biztonságimásolat-felügyeleti kiszolgálók listája](./media/backup-azure-delete-vault/list-of-backup-management-servers.png)
5. toodelete egy kiszolgáló hello listából, kattintson a jobb gombbal a hello hello kiszolgáló nevét, és kattintson a **törlése**.
    Hello **törlése** panel nyílik meg.
6. A hello **törlése** panelen hello hello kiszolgáló nevét adja meg. Ha hosszú a neve, másolja, és illessze be a biztonságimásolat-felügyeleti kiszolgálók hello listája. Kattintson a **törlése**.  
