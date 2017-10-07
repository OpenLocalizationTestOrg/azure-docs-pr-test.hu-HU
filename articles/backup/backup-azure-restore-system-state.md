---
title: "Az Azure Backup: Visszaállítási rendszerállapot tooa Windows Server |} Microsoft Docs"
description: "Lépés által lépés magyarázat a Windows Server rendszerállapot helyreállítása egy biztonsági másolatból, az Azure-ban."
services: backup
documentationcenter: 
author: saurabhsensharma
manager: shivamg
editor: 
ms.assetid: 
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 8/18/2017
ms.author: saurse;trinadhk;markgal;
ms.openlocfilehash: a45506507f53e2744350d3b6b2e52f1db415de4f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="restore-system-state-toowindows-server"></a>Rendszerállapot visszaállítása tooWindows kiszolgáló

Ez a cikk azt ismerteti, hogyan toorestore Windows kiszolgáló rendszerállapotának készített biztonsági másolat az Azure Recovery Services tároló. Rendszerállapot toorestore, rendelkeznie kell a rendszerállapot (hello utasításait használatával létrehozott [rendszerállapot biztonsági mentése](backup-azure-system-state.md#back-up-windows-server-system-state-preview)), és győződjön meg arról, hogy telepítette a hello [legújabb verzióját a Microsoft Azure Recovery hello Szolgáltatások (MARS) ügynök](http://aka.ms/azurebackup_agent). Végezze el az Azure Recovery Services-tároló a Windows Server rendszerállapot-adatok két lépésből áll:

1. Rendszerállapot visszaállítása az Azure Backup-fájlok formájában. Rendszerállapot visszaállítása az Azure Backup-fájlok formájában, lehetőségek közül választhat:
  * Rendszerállapot visszaállítása toohello ahol hello biztonsági mentések vették, ugyanarra a kiszolgálóra vagy
  * Rendszerállapot visszaállítása fájl tooan másodlagos kiszolgáló.

2. Vissza hello rendszerállapot fájlok tooa Windows Server alkalmazni.


## <a name="recover-system-state-files-toohello-same-server"></a>Rendszerállapot helyreállítása fájlok toohello ugyanarra a kiszolgálóra
hello lépések azt ismertetik, hogyan tooroll biztonsági másolatot a Windows Server konfigurációs tooa korábbi állapotába. Működés közbeni a kiszolgáló konfigurációs hátsó tooa ismert, stabil állapotot, rendkívül fontos lehet. hello követő lépéseket visszaállítási hello kiszolgáló rendszerállapotának a Recovery Services-tároló. 

1. Nyissa meg hello **a Microsoft Azure Backup szolgáltatás** beépülő modult. Ha nem tudja, hol hello beépülő modul lett telepítve, a keresés hello számítógép vagy kiszolgáló **a Microsoft Azure Backup szolgáltatás**.

    egy asztali alkalmazás hello hello keresési eredmények jelenjenek meg.

2. Kattintson a **adatok helyreállítása** toostart hello varázsló.

    ![Adatok helyreállítása](./media/backup-azure-restore-windows-server/recover.png)

3. A hello **bevezetés** ablaktáblában toorestore hello adatok toohello ugyanazon a kiszolgálón vagy a számítógépen, válassza ki a **ehhez a kiszolgálóhoz (`<server name>`)** kattintson **következő**.

    ![Válassza ki a kiszolgáló beállítás toorestore hello adatok toohello egyazon számítógépen](./media/backup-azure-restore-system-state/samemachine.png)

4. A hello **válassza ki a helyreállítási mód** ablaktáblán válassza **rendszerállapot** , majd **következő**.

    ![Fájlok tallózása](./media/backup-azure-restore-system-state/recover-type-selection.png)

5. A hello naptárban **mennyiség kiválasztása és a dátum** ablaktáblán válasszon ki egy helyreállítási pontot. 

    Bármelyik helyreállítási pontra állíthatja vissza időben. A dátumok **félkövér** jelző hello legalább egy helyreállítási pont rendelkezésre állását. Miután egy dátumot, ha több helyreállítási pont nem érhető el, hello adott helyreállítási pontot választhat hello **idő** legördülő menü.

    ![Kötet és a dátum](./media/backup-azure-restore-system-state/select-date.png)

6. Miután kiválasztotta a hello helyreállítási pont toorestore, kattintson a **következő**.

    Azure biztonsági mentés hello helyi helyreállítási pontot csatlakoztatja, és egy helyreállítási kötet használja.

7. A következő ablaktábla hello adja meg a hello hello céljának helyreállított fájlok rendszerállapot, majd kattintson **Tallózás** tooopen Windows Explorer és hello fájlok és mappák. lehetőségnél hello **példányt hoz létre, hogy mindkét változatához**, az egyes fájlok másolatokat készít a meglévő rendszerállapot fájl archiválhatja hello másolási hello teljes rendszerállapot-archívum létrehozása helyett.

    ![Helyreállítási beállítások](./media/backup-azure-restore-system-state/recover-as-files.png)

8. Ellenőrizze a hello helyreállítási hello adatait **megerősítő** kattintson **helyreállítása**.

   ![Kattintson a helyreállítás tooacknowledge hello helyreállítás művelet](./media/backup-azure-restore-system-state/confirm-recovery.png)

9. Másolás hello *WindowsImageBackup* könyvtárban lévő hello helyreállítási cél tooa nem kritikus kötet hello kiszolgáló. Általában a Windows operációs rendszer mennyiségi hello hello kritikus kötet jelenti.

10. Ha sikeres hello helyreállítási, lépésekkel hello hello területen [alkalmaz vissza rendszerállapot fájlok toohello Windows Server](backup-azure-restore-system-state.md#apply-restored-system-state-files-to-the-windows-server), toocomplete hello rendszerállapot-helyreállítási folyamatot.

## <a name="recover-system-state-files-tooan-alternate-server"></a>Rendszerállapot helyreállítása fájlok tooan másodlagos kiszolgáló

Ha a Windows Server sérült vagy nem érhető el, és azt szeretné, hogy toorestore azt tooa stabil állapotot által helyreállítása hello Windows kiszolgáló rendszerállapotának, hello sérült kiszolgáló rendszerállapotának állíthatja vissza egy másik kiszolgálóra. Következő lépések toohello visszaállítási rendszerállapot külön kiszolgálón hello használja.  

hello terminológia a következő lépéseket tartalmazza:

- *Forrásgép* – hello eredeti számítógép melyik hello biztonsági mentésből készült, és amelyen jelenleg nem érhető el.
- *Célszámítógép* – hello gép toowhich hello adatok helyreállítása folyamatban.
- *Minta tároló* – hello Recovery Services-tároló toowhich hello *forrásgép* és *célgépen* van regisztrálva. <br/>

> [!NOTE]
> Egyik gépről készített biztonsági másolatok hello operációs rendszer korábbi verzióját futtató visszaállított tooa gépet nem lehet. Például egy Windows Server 2016 gép nem lehet biztonsági másolatokat vissza tooWindows Server 2012 R2-ben. Hello inverz azonban lehetőség. Használhatja a Windows Server 2012 R2 toorestore Windows Server 2016 készített biztonsági másolat.
>

1. Nyissa meg hello **a Microsoft Azure Backup szolgáltatás** beépülő hello *célgépen*.
2. Győződjön meg arról, hogy hello *célgépen* és hello *forrásgép* azonos Recovery Services-tároló regisztrált toohello vannak.
3. Kattintson a **adatok helyreállítása** tooinitiate hello munkafolyamat.

    ![Adatok helyreállítása](./media/backup-azure-restore-windows-server-classic/recover.png)

4. Válassza ki **egy másik kiszolgáló**

    ![Egy másik kiszolgáló](./media/backup-azure-restore-system-state/anotherserver.png)

5. Adja meg a hello tárolói hitelesítő adatok fájlját, amely megfelel a toohello *minta tároló*. Ha hello tárolói hitelesítő adatok fájlját érvénytelen (vagy lejárt), egy új tárolói hitelesítő adatok fájlját letöltését hello *minta tároló* a hello Azure-portálon. Hello tárolói hitelesítő adatok fájlját valósul meg, miután hello Recovery Services-tároló hello tárolói hitelesítő adatok fájlját társított jelenik meg.

6. Hello biztonsági mentés kiszolgáló kiválasztása panelén válassza ki a hello *forrásgép* megjelenített gépek hello listája.

    ![Számítógépek listája](./media/backup-azure-restore-windows-server-classic/machinelist.png)

7. Hello válassza ki a helyreállítási mód panelén válassza **rendszerállapot** kattintson **következő**. 

    ![Keresés](./media/backup-azure-restore-system-state/recover-type-selection.png)

8. A naptár hello a hello **mennyiség kiválasztása és a dátum** ablaktáblán válasszon ki egy helyreállítási pontot. Bármelyik helyreállítási pontra állíthatja vissza időben. A dátumok **félkövér** jelző hello legalább egy helyreállítási pont rendelkezésre állását. Miután egy dátumot, ha több helyreállítási pont nem érhető el, hello adott helyreállítási pontot választhat hello **idő** legördülő menü. 

    ![Keresési elemek](./media/backup-azure-restore-system-state/select-date.png)

9. Miután kiválasztotta a hello helyreállítási pont toorestore, kattintson a **következő**.

10. A hello **válasszon rendszer állapota helyreállítási módban** ablaktáblán hello helyet, ahol rendszerállapot helyreállítása toobe fájlok adja meg, majd kattintson **következő**.

    ![Titkosítás](./media/backup-azure-restore-system-state/recover-as-files.png)

    lehetőségnél hello **példányt hoz létre, hogy mindkét változatához**, az egyes fájlok másolatokat készít a meglévő rendszerállapot fájl archiválhatja hello másolási hello teljes rendszerállapot-archívum létrehozása helyett.

11. Ellenőrizze a hello megerősítő panelen helyreállítási hello részleteit, majd kattintson **helyreállítása**. 

    ![Kattintson a hello helyreállítás gomb tooconfirm hello helyreállítási folyamat](./media/backup-azure-restore-system-state/confirm-recovery.png)

12. Másolás hello *WindowsImageBackup* directory tooa nem kritikus kötet hello kiszolgáló (például D:\). Általában a Windows operációs rendszer mennyiségi hello a hello kritikus kötet.

13. toocomplete hello helyreállítási folyamat használata hello következő szakasz túl[vissza hello rendszerállapot fájlok a Windows Server alkalmazása](backup-azure-restore-system-state.md#apply-restored-system-state-on-a-windows-server).




## <a name="apply-restored-system-state-on-a-windows-server"></a>A visszaállított rendszerállapot alkalmazni a Windows Server

Miután helyreállított rendszerállapot-fájlként Azure Recovery Services Agent, használjon hello Windows Server biztonsági másolat segédprogram tooapply hello helyre rendszerállapot tooWindows kiszolgáló. Windows Server biztonsági másolat segédprogram hello már hello kiszolgálón érhetők el. hello lépések azt ismertetik, hogyan tooapply hello helyre a rendszer állapotát.

1. Használjon hello következő parancsok tooreboot a kiszolgáló *a címtárszolgáltatás-javító módbeli*. A rendszergazda jogú parancssorból:

    ```
    PS C:\> Bcdedit /set safeboot dsrepair
    PS C:\> Shutdown /r /t 0
    ```

2. Hello az újraindítást követően hello Windows Server biztonsági másolat beépülő modul megnyitásához. Ha nem tudja, hol hello beépülő modul lett telepítve, a keresés hello számítógép vagy kiszolgáló **Windows Server biztonsági másolat**.

    hello egy asztali alkalmazás hello keresési eredmények között megjelenik.

3. Válassza ki a beépülő modul hello **helyi biztonsági másolat**.

    ![Válassza ki a helyi biztonsági másolat toorestore onnan](./media/backup-azure-restore-system-state/win-server-backup-local-backup.png)

4. A hello helyi biztonsági másolat konzoljának hello **műveletek panel**, kattintson a **helyreállítása** tooopen hello helyreállítási varázsló.

5. Hello a beállítást választja, **más helyen tárolt biztonsági másolat**, és kattintson a **következő**.

   ![Válassza ki a toorecover tooa másik kiszolgálóra](./media/backup-azure-restore-system-state/backup-stored-in-diff-location.png)

6. Hello tárhely típusának megadása esetén válassza ki a **távoli megosztott mappa** Ha a rendszervédelmi biztonsági másolatot a helyreállított tooanother kiszolgáló volt. Ha a rendszerállapot helyileg lett helyreállítva, akkor válasszon **helyi meghajtók**. 

    ![Válassza ki e a helyi kiszolgáló vagy egy másik toorecovery](./media/backup-azure-restore-system-state/ss-recovery-remote-shared-folder.png)

7. Adja meg a hello elérési toohello *WindowsImageBackup* könyvtár, vagy válassza ki a könyvtárat (például D:\WindowsImageBackup), Azure helyreállítással hello rendszerállapot fájlok helyreállítási helyre hello helyi meghajtót Ügynök, és kattintson a szolgáltatások **következő**.

    ![toohello megosztott fájl elérési útja](./media/backup-azure-restore-system-state/ss-recovery-remote-folder.png)

8. Jelölje be hello rendszerállapot verzió toorestore szeretné, majd kattintson **következő**.

9. Hello helyreállítási típus kiválasztása ablaktáblában jelölje ki **rendszerállapot** kattintson **következő**.

10. Rendszerállapot-helyreállítás hello hello helyét, válassza a **eredeti helyére**, és kattintson a **tovább**.

11. Hello megerősítő részletes leírását, ellenőrizze a hello újraindítás beállításait, és kattintson **helyreállítása** tooapplly hello rendszerállapot fájlok visszaállítása.

    ![indítási hello rendszerállapot fájlok helyreállítása](./media/backup-azure-restore-system-state/launch-ss-recovery.png)

## <a name="special-considerations-for-system-state-recovery-on-active-directory-server"></a>Különleges szempontok a rendszerállapot-helyreállítást az Active Directory-kiszolgálón

Rendszerállapot biztonsági mentésének Active Directory-adatokat. Használja a hello követő lépéseket toorestore Active Directory tartományi szolgáltatások (AD DS) az aktuális állapot tooa korábbi állapotába.

1. Indítsa újra a hello tartományvezérlő a Címtárszolgáltatások helyreállító módban (DSRM).
2. Hello lépésekkel [Itt](https://technet.microsoft.com/en-us/library/cc794755(v=ws.10).aspx) toouse Windows Server biztonsági másolat parancsmagjai toorecover Active Directory tartományi Szolgáltatásokban.


## <a name="troubleshoot-failed-system-state-restore"></a>Sikertelen rendszerállapot-visszaállítást hibaelhárítása

Ha hello előző folyamat, amely során a rendszerállapot nem fejeződik, használja a Windows Server hello Windows helyreállítási környezet (Win RE) toorecover. hello következő részben megtudhatja, hogyan toorecover Win újbóli használata. Használja ezt a beállítást csak akkor, ha a Windows Server normál esetben ne indítsa a rendszerállapot-visszaállítást követően. a folyamatot követve hello nem rendszerszintű adatot, legyen körültekintő töröl. 

1. A Windows Server indul hello Windows helyreállítási környezet (Win RE).

2. Válassza ki a hibaelhárítás hello három rendelkezésre álló lehetőségek közül.

    ![a menü megnyitása](./media/backup-azure-restore-system-state/winre-1.png)

3. A hello **speciális beállítások** képernyőn válassza ki **parancssor** és hello kiszolgálón rendszergazdai jogosultságú felhasználónevet és jelszót.

   ![a menü megnyitása](./media/backup-azure-restore-system-state/winre-2.png)

4. Hello kiszolgálón rendszergazdai jogosultságú felhasználónevet és jelszót adjon meg.

    ![a menü megnyitása](./media/backup-azure-restore-system-state/winre-3.png)

5. Amikor megnyitja a hello parancssort rendszergazdai módban, futtassa a következő parancs tooget hello rendszerállapot biztonságimásolat-verziók.

    ```
    Wbadmin get versions -backuptarget:<Volume where WindowsImageBackup folder is copied>:
    ```
    ![Rendszerállapot biztonságimásolat-verziók beolvasása](./media/backup-azure-restore-system-state/winre-4.png)

6. Futtassa a következő parancs tooget hello összes kötet elérhető hello biztonsági mentése.

    ```
    Wbadmin get items -version:<copy version from above step> -backuptarget:<Backup volume>
    ```

    ![Rendszerállapot biztonságimásolat-verziók beolvasása](./media/backup-azure-restore-system-state/winre-5.png)

7. hello következő parancs helyreállítja hello rendszerállapot biztonsági mentését részét képező összes kötetet. Vegye figyelembe, hogy a ezt a lépést csak hello kritikus kötetek hello rendszerállapot részét képező állítja helyre. Nem rendszermeghajtó minden adat törlődik.

    ```
    Wbadmin start recovery -items:C: -itemtype:Volume -version:<Backupversion> -backuptarget:<backup target volume>
    ```
     ![Rendszerállapot biztonságimásolat-verziók beolvasása](./media/backup-azure-restore-system-state/winre-6.png)



## <a name="next-steps"></a>Következő lépések
* Most, hogy a fájlok és mappák már helyreállítva, akkor [kezelheti a biztonsági másolatok](backup-azure-manage-windows-server.md).
