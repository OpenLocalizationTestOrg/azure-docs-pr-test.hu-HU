---
title: "mentése a Windows rendszer állapota tooAzure aaaBack |} Microsoft Docs"
description: "Ismerje meg a Windows Server és/vagy a Windows-számítógépek tooAzure hello rendszerállapotot tooback."
services: backup
documentationcenter: 
author: saurabhsensharma
manager: carmonm
editor: 
keywords: "Hogyan toobackup; Hogyan tooback; a biztonságimásolat-fájlok és mappák"
ms.assetid: 5b15ebf1-2214-4722-b937-96e2be8872bb
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: saurse;markgal
ms.openlocfilehash: be5d4be81af981c10de82add9fe962a730753cf5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="back-up-windows-system-state-in-resource-manager-deployment"></a>A Resource Manager telepítés Windows rendszerállapot biztonsági mentését
Ez a cikk azt ismerteti, hogyan tooback el a Windows Server rendszert állapot tooAzure. Egy oktatóanyag tervezett toowalk hello alapokat nyújt.

Ha azt szeretné, hogy további információk az Azure Backup tooknow, olvassa el ezt [áttekintése](backup-introduction-to-azure-backup.md).

Ha még nincs Azure-előfizetése, hozzon létre egy [ingyenes fiókot](https://azure.microsoft.com/free/), amellyel bármely Azure-szolgáltatást elérhet.

## <a name="create-a-recovery-services-vault"></a>Recovery Services-tároló létrehozása
a fájlok és mappák tooback, meg kell toocreate hello régióban, ahová toostore hello adatokat Recovery Services-tároló. Meg kell toodetermine a replikált tároló módját is.

### <a name="toocreate-a-recovery-services-vault"></a>Recovery Services-tároló toocreate
1. Ha még nem tette meg, jelentkezzen be toohello [Azure Portal](https://portal.azure.com/) használata az Azure-előfizetéshez.
2. Hello központ menüben kattintson a **további szolgáltatások** hello az erőforrások listájához, írja be a **Recovery Services** kattintson **Recovery Services-tárolók**.

    ![Recovery Services-tároló létrehozása – 1. lépés](./media/backup-try-azure-backup-in-10-mins/open-rs-vault-list.png) <br/>

    Ha nincsenek recovery services-tárolók hello az előfizetést, hello tárolók vannak felsorolva.
3. A hello **Recovery Services-tárolók** menüben kattintson a **Hozzáadás**.

    ![Recovery Services-tároló létrehozása – 2. lépés](./media/backup-try-azure-backup-in-10-mins/rs-vault-menu.png)

    hello Recovery Services tároló panel nyílik tooprovide kéri egy **neve**, **előfizetés**, **erőforráscsoport**, és **hely**.

    ![Recovery Services-tároló létrehozása – 3. lépés](./media/backup-try-azure-backup-in-10-mins/rs-vault-step-3.png)

4. A **neve**, adjon meg egy rövid nevet tooidentify hello tárolóban. hello nevének kell toobe egyedi hello Azure-előfizetés esetében. Írjon be egy 2–50 karakter hosszúságú nevet. Ennek egy betűvel kell kezdődnie, és csak betűket, számokat és kötőjeleket tartalmazhat.

5. A hello **előfizetés** területen hello legördülő menü toochoose hello Azure-előfizetés használatára. Ha csak egyetlen előfizetéssel, előfizetés megjelenő használja, és tovább toohello a lépést kihagyhatja. Ha nem biztos abban, hogy melyik előfizetéssel toouse, használhatja az alapértelmezettet hello (vagy javasolt) előfizetés. Csak akkor lesz több választási lehetőség, ha a szervezetéhez tartozó fiók több Azure-előfizetéssel van összekötve.

6. A hello **erőforráscsoport** szakasz:

    * Válassza ki **hozzon létre új** Ha azt szeretné, hogy toocreate egy erőforráscsoportot.
    Vagy
    * Válassza ki **meglévő** kattintson hello legördülő menü toosee hello elérhető erőforráscsoportok listáját.

  Az erőforráscsoportok, tanulmányozza hello [Azure Resource Manager áttekintése](../azure-resource-manager/resource-group-overview.md).

7. Kattintson a **hely** tooselect hello földrajzi régióban hello tároló. Ez a beállítás meghatározza, hogy hello földrajzi régiót, ahol a biztonsági mentési adatokat küldi el.

8. A Recovery Services-tároló panel hello hello alján kattintson **létrehozása**.

    Recovery Services-tároló létrehozása toobe hello több percet is igénybe vehet. Hello állapot értesítések hello felső jobb területen hello portál figyelése. A tároló létrehozása után hello Recovery Services-tárolók listája jelenik meg. Ha több perc után sem látja a tárolót, kattintson a **Frissítés** gombra.

    ![Kattintson a Frissítés gombra](./media/backup-try-azure-backup-in-10-mins/refresh-button.png)</br>

    A tároló a Recovery Services-tárolók hello listája jelenik meg, ha készen áll a tooset hello adattároló redundanciája, amely áll.

### <a name="set-storage-redundancy-for-hello-vault"></a>Állítsa be az adattároló redundanciája, amely hello tároló
Recovery Services-tároló létrehozásakor ellenőrizze, hogy adattároló redundanciája, amely konfigurált hello igényeinek megfelelően.

1. A hello **Recovery Services-tárolók** paneljén kattintson hello új tárolóra.

    ![Válassza ki az új tároló hello a hello Recovery Services-tároló](./media/backup-try-azure-backup-in-10-mins/rs-vault-list.png)

    Hello tároló kiválasztásakor hello **Recovery Services-tároló** panel narrows és hello-beállítások panel (*hello tároló nevére hello hello felső tartalmaz*) és hello tároló Részletek panel megnyitása.

    ![Hello új tároló tárolási konfiguráció megtekintése](./media/backup-try-azure-backup-in-10-mins/set-storage-configuration-2.png)
2. Hello új tároló-beállítások panelen hello függőleges diák tooscroll le toohello kezelése szakasz használja, és kattintson **biztonsági infrastruktúra**.
    hello biztonsági infrastruktúra panel nyílik meg.
3. Hello biztonsági infrastruktúra paneljén kattintson **biztonsági mentési konfigurációhoz** tooopen hello **biztonsági mentési konfigurációhoz** panelen.

    ![Új tároló hello tárolási konfiguráció beállítása](./media/backup-try-azure-backup-in-10-mins/set-storage-configuration.png)
4. Válassza ki a hello megfelelő tárolási replikációs beállítás a tároló számára.

    ![a tároló konfigurálásának lehetőségei](./media/backup-try-azure-backup-in-10-mins/choose-storage-configuration.png)

    Alapértelmezés szerint a tárolója georedundáns tárolással rendelkezik. Azure biztonságimásolat-tároláshoz-végpontként használatakor, továbbra is toouse **georedundáns**. Ha nem adja meg Azure biztonságimásolat-tároláshoz-végpontként, majd válasszon **helyileg redundáns**, amely csökkenti a hello Azure storage költségei. A [georedundáns](../storage/common/storage-redundancy.md#geo-redundant-storage) és a [helyileg redundáns](../storage/common/storage-redundancy.md#locally-redundant-storage) tárolási lehetőségekről többet olvashat ebben a [Tárhely-redundancia áttekintésben](../storage/common/storage-redundancy.md).

Most, hogy létrehozta a tárolót, konfigurálja a Windows rendszerállapotról készít biztonsági mentést.

## <a name="configure-hello-vault"></a>Hello tároló konfigurálása
1. A Recovery Services tároló paneljén (hello tároló most létrehozott), a Bevezetés című szakaszt hello hello, kattintson a **biztonsági mentés**, végül a hello **Ismerkedés a biztonsági mentés** panelen válassza  **Biztonsági mentési cél**.

    ![A biztonsági mentés célja panel megnyitása](./media/backup-try-azure-backup-in-10-mins/open-backup-settings.png)

    Hello **biztonsági mentési cél** panel nyílik meg.

    ![A biztonsági mentés célja panel megnyitása](./media/backup-try-azure-backup-in-10-mins/backup-goal-blade.png)

2. A hello **a számítási feladatok futtató?** legördülő menüben válassza **helyszíni**.

    Azért kell a **Helyszíni** lehetőséget választania, mert a Windows Server- vagy Windows-számítógépe egy fizikai gép, amely nem az Azure része.

3. A hello **miről szeretné, hogy toobackup?** menüjében válassza **rendszerállapot**, és kattintson a **OK**.

    ![Fájlok és mappák konfigurálása](./media/backup-azure-system-state/backup-goal-system-state.png)

    Az OK gombra kattintva jelölje megjelenése után következő túl**biztonsági mentési cél**, és hello **infrastruktúra előkészítése** panel nyílik meg.

    ![Ha konfigurálta a biztonsági mentés célját, a következő lépés az infrastruktúra előkészítése](./media/backup-try-azure-backup-in-10-mins/backup-goal-configed.png)

4. A hello **infrastruktúra előkészítése** panelen kattintson a **letöltése ügynök Windows vagy a Windows ügyfél**.

    ![infrastruktúra előkészítése](./media/backup-try-azure-backup-in-10-mins/choose-agent-for-server-client.png)

    Ha a Windows Server alapvető használ, majd válassza a toodownload hello ügynök a Windows Server alapvető. Felbukkanó toorun kéri, vagy MARSAgentInstaller.exe menti.

    ![MARSAgentInstaller párbeszédpanel](./media/backup-try-azure-backup-in-10-mins/mars-installer-run-save.png)

5. Hello letöltési megjelenő menüben, kattintson a **mentése**.

    Alapértelmezés szerint hello **MARSagentinstaller.exe** fájl tooyour Letöltések mappába kerül. Hello telepítő befejezése után megjelenik egy előugró ablak, amely rákérdez, ha szeretné, hogy toorun hello telepítő, vagy nyissa meg a hello mappa.

    ![infrastruktúra előkészítése](./media/backup-try-azure-backup-in-10-mins/mars-installer-complete.png)

    Tooinstall hello ügynök még nincs szükség. Hello ügynök is telepíthet, miután letöltötte a hello tárolói hitelesítő adatokat.

6. A hello **infrastruktúra előkészítése** panelen kattintson a **letöltése**.

    ![a tároló hitelesítő adatainak letöltése](./media/backup-try-azure-backup-in-10-mins/download-vault-credentials.png)

    hello tárolói hitelesítő adatok letöltése tooyour Letöltések mappába. Hello tárolói hitelesítő adatokat, akkor letöltés befejezése után megjelenik egy előugró ablak, amely rákérdez, ha azt szeretné tooopen, vagy hello hitelesítő adatok mentése. Kattintson a **Save** (Mentés) gombra. Ha véletlenül kattintson **nyitott**, lehetővé teszik a hello párbeszédpanel érintő tooopen hello tárolói hitelesítő adatokat, a művelet sikertelen. Hello tárolói hitelesítő adatok nem nyitható meg. A folytatáshoz toohello következő lépésre. hello tárolói hitelesítő adatok vannak hello Letöltések mappába.   

    ![a tároló hitelesítő adatainak letöltése befejeződött](./media/backup-try-azure-backup-in-10-mins/vault-credentials-downloaded.png)

## <a name="install-and-register-hello-agent"></a>Telepítse és regisztrálja a hello ügynök

> [!NOTE]
> Lehetővé teszi biztonsági mentés hello Azure-portálon keresztül még nem érhető el. Hello Microsoft Azure Recovery Services Agent tooback használja a Windows Server rendszer állapotát.
>

1. Keresse meg és kattintson duplán a hello **MARSagentinstaller.exe** hello Letöltések mappába (vagy más mentett helyről).

    hello telepítője biztosítja az üzenetből bontja ki, mert telepíti, és regisztrálja hello Recovery Services Agent ügynököt.

    ![a Recovery Services-ügynök telepítőjének futtatása, hitelesítő adatok](./media/backup-try-azure-backup-in-10-mins/mars-installer-registration.png)

2. Hello Microsoft Azure Recovery Services ügynök telepítő varázsló befejezéséhez. toocomplete hello varázsló kell:

   * Válassza ki a hello telepítési és a gyorsítótár mappa.
   * Adja meg a proxykiszolgáló kiszolgálóadatok használatakor a proxy server tooconnect toohello internet.
   * Adja meg a felhasználónevének és jelszavának részleteit, ha hitelesített proxyt használ.
   * Adja meg a letöltött hello tárolói hitelesítő adatokat
   * Hello titkosítási jelszó mentse egy biztonságos helyre.

     > [!NOTE]
     > Ha elveszíti vagy elfelejti hello jelszót, a Microsoft nem súgó hello biztonsági mentési adatok helyreállítását. Mentse a hello fájlt biztonságos helyen. A biztonsági mentés szükséges toorestore.
     >
     >

hello ügynök telepítve van, és a számítógép regisztrált toohello tárolóban. Most készen áll a tooconfigure, és a biztonsági mentés ütemezése.

## <a name="back-up-windows-server-system-state-preview"></a>Készítsen biztonsági másolatot a Windows Server rendszer állapota (előzetes verzió)
hello kezdeti biztonsági másolatot a három feladatokból áll:

* Rendszerállapot biztonsági mentésének segítségével hello Azure Backup szolgáltatás ügynökének engedélyezése
* Hello biztonsági mentés ütemezése
* Fájlok és mappák biztonsági mentése a hello első alkalommal

toocomplete hello kezdeti biztonsági másolatot, használjon hello Microsoft Azure Recovery Services Agent ügynököt.

### <a name="tooenable-system-state-backup-using-hello-azure-backup-agent"></a>tooenable rendszerállapot biztonsági mentésének segítségével hello Azure biztonságimásolat-készítő ügynökkel

1. Egy PowerShell-munkamenetet futtassa a következő parancs toostop hello Azure biztonsági mentés motor hello.

  ```
  PS C:\> Net stop obengine
  ```

2. Nyissa meg a Windows beállításjegyzék hello.

  ```
  PS C:\> regedit.exe
  ```

3. Adja hozzá a hello rendszerleíró kulcsot következő hello megadott DWord-értéket.

  | Beállításjegyzékbeli elérési út | Beállításkulcs | Duplaszó |
  |---------------|--------------|-------------|
  | HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config\CloudBackupProvider | TurnOffSSBFeature | 2 |

4. Indítsa újra a hello biztonsági mentés motor hajtja végre a következő parancsot egy rendszergazda jogú parancssort a hello.

  ```
  PS C:\> Net start obengine
  ```

### <a name="tooschedule-hello-backup-job"></a>tooschedule hello biztonsági mentési feladat

1. Nyissa meg a hello Microsoft Azure Recovery Services Agent ügynököt. A megkereséséhez keressen rá a gépen a **Microsoft Azure Backup** kifejezésre.

    ![Indítsa el a hello Azure Recovery Services Agent ügynök](./media/backup-try-azure-backup-in-10-mins/snap-in-search.png)

2. Hello Recovery Services Agent ügynököt, kattintson **biztonsági mentés ütemezése**.

    ![Windows Server biztonsági mentés ütemezése](./media/backup-try-azure-backup-in-10-mins/schedule-first-backup.png)

3. A hello bevezetés hello ütemezett biztonsági mentés varázsló lapján, kattintson a **következő**.

4. Kattintson hello elemek kijelölése tooBackup lap **elemek hozzáadása**.

5. Válassza ki **rendszerállapot** majd **OK**.

6. Kattintson a **Tovább** gombra.

7. hello rendszerállapot biztonsági mentése és a megőrzési ütemezés nem automatikusan állítják be minden vasárnap tooback 9:00 PM helyi idő és hello megőrzési idő van beállítva too60 nap.

   > [!NOTE]
   > Rendszerállapot biztonsági mentési és adatmegőrzési házirend automatikusan konfigurálja. Ha a biztonsági mentést fájlok és mappák továbbá toohello Windows kiszolgáló rendszerállapotának, csak hello biztonsági mentési és adatmegőrzési házirend megadása a fájl mentését hello varázsló. 
   >

8. A hello megerősítése lapon tekintse át hello adatokat, és kattintson **Befejezés**.

9. Hello biztonsági mentési ütemezés létrehozása hello varázsló befejezése után kattintson **Bezárás**.

### <a name="tooback-up-windows-server-system-state-for-hello-first-time"></a>hello először a Windows Server rendszerállapotot tooback

1. Győződjön meg arról, hogy nincsenek függőben lévő frissítések a Windows Server, a számítógép újraindítása szükséges.

2. Hello Recovery Services Agent ügynököt, kattintson **biztonsági másolat készítése most** toocomplete hello kezdeti összehangolása hello hálózaton keresztül.

    ![Windows Server biztonsági másolat készítése](./media/backup-try-azure-backup-in-10-mins/backup-now.png)

3. A hello megerősítése lapon, amely a biztonsági mentést most varázsló hello hello beállítások áttekintése hello gép tooback fogja használni. Ezután kattintson a **Biztonsági mentés** gombra.

4. Kattintson a **Bezárás** tooclose hello varázsló. Ha a biztonsági mentési folyamat hello befejeződése előtt bezárja hello varázslót, hello varázsló toorun hello háttérben folytatódik.

5. Ha a biztonsági mentést fájlok és mappák a kiszolgálón, továbbá toohello Windows Server rendszer állapotát, a hello most biztonsági mentés varázsló csak készít biztonsági másolatot a fájlokat. az ad hoc rendszerállapot biztonsági mentése, a következő PowerShell-parancs használata hello tooperform:

    ```
    PS C:\> Start-OBSystemStateBackup
    ```

  Hello kezdeti biztonsági mentés befejezése után hello **feladata befejezve** állapota megjelenik hello biztonsági mentés konzolban.

  ![IR befejezve](./media/backup-try-azure-backup-in-10-mins/ircomplete.png)

## <a name="frequently-asked-questions"></a>Gyakori kérdések

hello alábbi kérdések és válaszok nyújt kiegészítő információkat.

### <a name="what-is-hello-staging-volume"></a>Mi az az átmeneti kötet hello?

hello átmeneti kötet hello közbenső hely, ahol hello natív módon, a Windows Server biztonsági másolat készít elő hello rendszerállapot biztonsági mentését jelöli. Az Azure Backup szolgáltatás ügynökének akkor tömöríti és titkosítja a köztes biztonsági mentési és elküldi azt biztonságos HTTPS protokoll toohello konfigurált Recovery Services-tároló. **Erősen ajánlott a nem Windows-OS kötet hello átmeneti kötet létrehozása. Azt láthatja, hogy a rendszerállapot biztonsági mentéseit problémáit, a átmeneti kötet hello hely ellenőrzése akkor hello hibaelhárítás első lépése.** 

### <a name="how-can-i-change-hello-staging-volume-path-specified-in-hello-azure-backup-agent"></a>Hogyan módosítható hello átmeneti elérési útjával hello Azure Backup szolgáltatás ügynökének megadott?

hello átmeneti kötet alapértelmezés szerint hello gyorsítótár mappában található. 

1. toochange ezen a helyen, a következő parancsot (a rendszergazda jogú parancssorból) használata hello:
  ```
  PS C:\> Net stop obengine
  ```

2. A következő beállításjegyzék-bejegyzések hello elérési toohello új átmeneti kötet mappa hello frissíteni.

  |Beállításjegyzékbeli elérési út|Beállításkulcs|Érték|
  |-------------|------------|-----|
  |HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config\CloudBackupProvider | SSBStagingPath | Új átmeneti kötet hely |

hello átmeneti elérési út a kis-és nagybetűket, és hello pontos azonos kis-és nagybetűk, mi létezik a hello kiszolgálón kell lennie. 

3. Hello átmeneti kötet elérési út módosítása után indítsa újra a hello biztonsági mentés motor:
  ```
  PS C:\> Net start obengine
  ```
4. toopick megváltozott hello elérési útja, hello nyissa meg a Microsoft Azure Recovery Services Agent ügynököt és egy ad hoc biztonsági mentés rendszerállapot eseményindító.

### <a name="why-is-hello-system-state-default-retention-set-too60-days"></a>Miért érdemes az alapértelmezett megőrzési beállítása too60 nap rendszerállapot hello?

a rendszerállapot biztonsági mentését hello élettartama van hello ugyanaz, mint a hello Windows Server Active Directory szerepkör hello "szemétgyűjtési" beállítást. hello alapértelmezett hello törlésre élettartama bejegyzés értéke 60 nap. Ez az érték beállítható hello címtárszolgáltatás (NTDS) konfigurációs objektum.

### <a name="how-do-i-change-hello-default-backup-and-retention-policy-for-system-state"></a>Hogyan változtathatom meg hello alapértelmezett biztonsági mentési és adatmegőrzési rendszerállapot?

toochange hello alapértelmezett biztonsági mentési és adatmegőrzési rendszerállapot:
1. Állítsa le a hello biztonsági mentést vezérlő. Futtassa a következő parancsot egy emelt szintű parancssorból hello.

  ```
  PS C:\> Net stop obengine
  ```

2. Adja hozzá, vagy frissítse a következő beállításkulcs-bejegyzésekről a HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config\CloudBackupProvider hello.

  |Rendszerleíró adatbázis neve|Leírás|Érték|
  |-------------|-----------|-----|
  |SSBScheduleTime|Hello biztonsági másolat készítésének idején tooconfigure hello használt. Alapértelmezett érték a helyi idő du. 9.|DWord: Formátum HHMM (decimális) például 2130 9:30 PM helyi ideje|
  |SSBScheduleDays|Ha rendszerállapot biztonsági mentését kell végrehajtani hello használt tooconfigure hello nap megadva. Egyes számjegyek hello napot adja meg. 0 a vasárnapot jelenti, 1 hétfő, és így tovább. Alapértelmezett napi biztonsági mentéshez a vasárnapot jelenti.|DWord: nap hello hét toorun mentési (decimális) például 1230 ütemez biztonsági mentések hétfő, kedd, szerda és vasárnap.|
  |SSBRetentionDays|Használt tooconfigure hello nap tooretain biztonsági mentés. Alapértelmezett érték 60. Megengedett maximális értéket 180.|DWord: Nap tooretain biztonsági mentés (decimális).|

3. A következő parancs toorestart hello biztonságimásolat-készítő motor hello használata.
    ```
    PS C:\> Net start obengine
    ```

4. Nyissa meg a hello Microsoft Recovery Services Agent ügynököt.

5. Kattintson a **biztonsági mentés ütemezése** majd **következő** amíg hello módosítások megjelenik.

6. Kattintson a **Befejezés** tooapply hello módosításokat.


## <a name="questions"></a>Kérdései vannak?
Ha kérdése van, vagy ha bármely új szolgáltatása része, toosee szeretné [visszajelzést küldhet](http://aka.ms/azurebackup_feedback).

## <a name="next-steps"></a>Következő lépések
* További részletek a [Windows rendszerű gépek biztonsági mentéséről](backup-configure-vault.md).
* Most, hogy biztonsági másolatot készített a fájlokról és mappákról, [kezelheti a tárlókat és a kiszolgálókat](backup-azure-manage-windows-server.md).
* Ha szükség toorestore biztonsági másolat, akkor ez a cikk túl[fájlok tooa Windows számítógép visszaállítása](backup-azure-restore-windows-server.md).
