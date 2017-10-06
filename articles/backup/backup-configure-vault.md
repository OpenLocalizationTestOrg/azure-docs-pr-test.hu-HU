---
title: "aaaUse Azure Backup agent tooback fájlok és mappák |} Microsoft Docs"
description: "Hello Microsoft Azure Backup agent tooback Windows fájlok és mappák tooAzure másolatot használja. Recovery Services-tároló létrehozása, hello Backup szolgáltatás ügynökének telepítése, hello biztonsági mentési házirend meghatározása és hello kezdeti biztonsági mentés futtatására hello fájlok és mappák."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keywords: "mentési tároló; Készítsen biztonsági másolatot a Windows server; biztonsági mentési Időablakok;"
ms.assetid: 7f5b1943-b3c1-4ddb-8fb7-3560533c68d5
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 8/15/2017
ms.author: markgal;trinadhk;
ms.openlocfilehash: be203c24841971872b5c6e7cf260a2fa5c58ac47
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="back-up-a-windows-server-or-client-tooazure-using-hello-resource-manager-deployment-model"></a>Készítsen biztonsági másolatot a Windows Server vagy az ügyfél tooAzure hello Resource Manager telepítési modell segítségével
> [!div class="op_single_selector"]
> * [Azure Portal](backup-configure-vault.md)
> * [Klasszikus portál](backup-configure-vault-classic.md)
>
>

Ez a cikk azt ismerteti, hogyan tooback készítése a Windows Server (vagy a Windows-ügyfél) fájlok és mappák tooAzure az Azure Backup használatával hello Resource Manager üzembe helyezési modellben.

[!INCLUDE [learn-about-deployment-models](../../includes/backup-deployment-models.md)]

![Biztonsági mentési folyamat lépései](./media/backup-configure-vault/initial-backup-process.png)

## <a name="before-you-start"></a>Előkészületek
egy kiszolgáló vagy az ügyfél tooAzure tooback, kell az Azure-fiók. Ha még nincs fiókja, létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/) néhány percig.

## <a name="create-a-recovery-services-vault"></a>Recovery Services-tároló létrehozása
Recovery Services-tároló olyan entitás, amely minden hello biztonsági mentések és helyreállítási pontokat hoz létre adott idő alatt tárolja. hello Recovery Services-tárolónak a hello alkalmazza a biztonsági mentési házirend toohello védett fájlok és mappák is tartalmaz. Recovery Services-tároló létrehozásakor kell is lehetőséggel hello megfelelő tárolási redundanciát.

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

    * Válassza ki **hozzon létre új** Ha azt szeretné, hogy toocreate egy új erőforráscsoportot.
    Vagy
    * Válassza ki **meglévő** kattintson hello legördülő menü toosee hello elérhető erőforráscsoportok listáját.

  Az erőforráscsoportok, tanulmányozza hello [Azure Resource Manager áttekintése](../azure-resource-manager/resource-group-overview.md).

7. Kattintson a **hely** tooselect hello földrajzi régióban hello tároló. Ez a beállítás meghatározza, hogy hello földrajzi régiót, ahol a biztonsági mentési adatokat küldi el.

8. A Recovery Services-tároló panel hello hello alján kattintson **létrehozása**.

  Recovery Services-tároló létrehozása toobe hello több percet is igénybe vehet. Hello állapot értesítések hello felső jobb területen hello portál figyelése. A tároló létrehozása után hello Recovery Services-tárolók listája jelenik meg. Ha több perc után sem látja a tárolót, kattintson a **Frissítés** gombra.

  ![Kattintson a Frissítés gombra](./media/backup-try-azure-backup-in-10-mins/refresh-button.png)</br>

  A tároló a Recovery Services-tárolók hello listája jelenik meg, ha készen áll a tooset hello adattároló redundanciája, amely áll.


### <a name="set-storage-redundancy"></a>Set adattároló redundanciája, amely
Amikor először hoz létre Recovery Services-tárolót, meghatározza a tároló replikálásának módját.

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

Most, hogy létrehozta a tárolót, készítse elő az infrastruktúra tooback mappák és fájlok letöltése és hello Microsoft Azure Recovery Services Agent ügynök telepítése, töltse le a tároló, és ezen hitelesítő adatok tooregister hello ügynök használata hello tárolóban.

## <a name="configure-hello-vault"></a>Hello tároló konfigurálása

1. A Recovery Services tároló paneljén (hello tároló most létrehozott), a Bevezetés című szakaszt hello hello, kattintson a **biztonsági mentés**, végül a hello **Ismerkedés a biztonsági mentés** panelen válassza  **Biztonsági mentési cél**.

  ![A biztonsági mentés célja panel megnyitása](./media/backup-try-azure-backup-in-10-mins/open-backup-settings.png)

  Hello **biztonsági mentési cél** panel nyílik meg. Ha a Recovery Services-tároló hello korábban volt konfigurálva, majd hello **biztonsági mentési cél** paneleken kattintva megjelenik **biztonsági mentés** hello Recovery Services tároló panel.

  ![A biztonsági mentés célja panel megnyitása](./media/backup-try-azure-backup-in-10-mins/backup-goal-blade.png)

2. A hello **a számítási feladatok futtató?** legördülő menüben válassza **helyszíni**.

  Azért kell a **Helyszíni** lehetőséget választania, mert a Windows Server- vagy Windows-számítógépe egy fizikai gép, amely nem az Azure része.

3. A hello **miről szeretné, hogy toobackup?** menüjében válassza **fájlok és mappák**, és kattintson a **OK**.

  ![Fájlok és mappák konfigurálása](./media/backup-try-azure-backup-in-10-mins/set-file-folder.png)

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
> Lehetővé teszi biztonsági mentés hello Azure-portálon keresztül még nem érhető el. A fájlok és mappák hello Microsoft Azure Recovery Services Agent tooback használja.
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

## <a name="network-and-connectivity-requirements"></a>Hálózati és kapcsolati követelmények

Ha a machine /-proxy korlátozott internet-hozzáféréssel, győződjön meg arról, hogy tűzfal beállításait a hello machine /-proxy konfigurált tooallow hello következő URL-címek: <br>
    1. www.msftncsi.com
    2. *.Microsoft.com
    3. *.WindowsAzure.com
    4. *.microsoftonline.com
    5. *.windows.ne


## <a name="create-hello-backup-policy"></a>Hello biztonsági mentési házirend létrehozása
a biztonsági mentési házirend hello hello ütemezés akkor, ha a helyreállítási pontokat készít, és mennyi ideig hello helyreállítási pontok megmaradnak hello. Hello Microsoft Azure Backup agent toocreate hello biztonsági mentési házirendet használja a fájlok és mappák.

### <a name="toocreate-a-backup-schedule"></a>a biztonsági mentés ütemezését toocreate
1. Nyissa meg a Microsoft Azure Backup szolgáltatás ügynökének hello. A megkereséséhez keressen rá a gépen a **Microsoft Azure Backup** kifejezésre.

    ![Indítsa el a hello Azure Backup szolgáltatás ügynöke](./media/backup-configure-vault/snap-in-search.png)
2. A hello Backup szolgáltatás ügynökének **műveletek** ablaktáblán kattintson a **biztonsági mentés ütemezése** toolaunch hello ütemezett biztonsági mentés varázsló.

    ![Windows Server biztonsági mentés ütemezése](./media/backup-configure-vault/schedule-first-backup.png)

3. A hello **bevezetés** hello ütemezett biztonsági mentés varázsló oldalán kattintson **következő**.
4. A hello **elemek kijelölése tooBackup** kattintson **elemek hozzáadása**.

  hello elemek kijelölése párbeszédpanel nyílik meg.

5. Válassza ki a hello fájlokat és mappákat, hogy szeretné, hogy tooprotect, és kattintson a **OK**.
6. A hello **elemek kijelölése tooBackup** kattintson **következő**.
7. A hello **adja meg a biztonsági mentés ütemezése** adja meg azokat a hello biztonsági mentési ütemezést, és kattintson **következő**.

    Napi (legfeljebb napi háromszori) vagy heti biztonsági mentéseket ütemezhet.

    ![Windows Server biztonsági mentés elemei](./media/backup-configure-vault/specify-backup-schedule-close.png)

   > [!NOTE]
   > Hogyan toospecify hello biztonsági mentés ütemezése kapcsolatos további információkért lásd: hello cikk [használata Azure biztonsági mentési tooreplace a szalag infrastruktúra](backup-azure-backup-cloud-as-tape.md).
   >
   >

8. A hello **válassza ki az adatmegőrzési** lapon válassza ki a hello vélt megőrzési házirendek hello hello biztonsági másolatot, majd kattintson **következő**.

    hello adatmegőrzési mely hello biztonsági másolatot hello időtartamát határozza meg. Ahelyett, hogy csak adja meg az összes biztonsági mentési pont "egyszerű policy", a másik adatmegőrzési hello biztonsági mentés esetén alapján is megadhat. Hello napi, heti, havi és éves megőrzési házirendek toomeet módosíthatja az igényeinek.
9. Hello kezdeti biztonsági mentési típusának kiválasztása lapon válassza a hello kezdeti biztonsági mentés típusát. Hagyja hello beállítást **automatikusan hello hálózaton keresztül** kiválasztva, és kattintson **következő**.

    Biztonsági másolatot készíthet automatikusan hello hálózaton keresztül, vagy a biztonsági mentést készíthet offline állapotba. hello a cikk hátralévő része a biztonsági mentési automatikusan hello folyamatot írja le. Ha jobban szeret toodo offline biztonsági másolat, tekintse át a hello cikk [az Azure Backup Offline biztonsági másolat munkafolyamat](backup-azure-backup-import-export.md) további információt.
10. A hello megerősítése lapon tekintse át hello adatokat, és kattintson **Befejezés**.
11. Hello biztonsági mentési ütemezés létrehozása hello varázsló befejezése után kattintson **Bezárás**.

### <a name="enable-network-throttling"></a>Hálózati sávszélesség-szabályozás engedélyezése
hello Microsoft Azure Backup szolgáltatás ügynökének biztosít a hálózati sávszélesség-szabályozás. Szabályozza, hogyan adatátvitel során használt hálózati sávszélesség-szabályozás. Ez a vezérlő akkor lehet hasznos, ha tooback adatokat kell munkaidőben, de nem hello biztonsági mentési folyamat toointerfere a más internetes forgalmat. Sávszélesség-szabályozás tooback vonatkozik fel, és állítsa vissza a tevékenységeket.

> [!NOTE]
> Hálózati sávszélesség-szabályozás nem érhető el a Windows Server 2008 R2 SP1, Windows Server 2008 SP2 vagy Windows 7 (szervizcsomagokkal). hello Azure biztonsági mentési hálózati sávszélesség-szabályozás szolgáltatás szolgáltatásminőség (QoS) hello helyi operációs rendszer kapcsolatba lép. Bár az Azure biztonsági mentés az említett operációs rendszerektől védelemmel való ellátása hello verziója érhető el, ezek a rendszerek QoS Azure biztonsági mentési hálózati sávszélesség-szabályozás nem működik. Az összes egyéb hálózati sávszélesség-szabályozás használható [támogatott operációs rendszerek](backup-azure-backup-faq.md).
>
>

**tooenable hálózati sávszélesség-szabályozás**

1. Hello Microsoft Azure Backup szolgáltatás ügynökének, kattintson **tulajdonságainak módosítása**.

    ![Tulajdonságainak módosítása](./media/backup-configure-vault/change-properties.png)
2. A hello **sávszélesség-szabályozási** lapra, jelölje be hello **engedélyezi az internetes sávszélesség szabályozásának a biztonsági mentési műveleteknél** jelölőnégyzetet.

    ![Hálózati sávszélesség-szabályozás](./media/backup-configure-vault/throttling-dialog.png)
3. Miután engedélyezte a sávszélesség-szabályozás, adja meg a biztonsági mentési adatátvitel során sávszélesség engedélyezett hello **időpontokat a munkaidőhöz** és **munkaidőn kívüli**.

    hello sávszélesség értékek 512 kilobit / másodperc (Kbps) kezdődik, és lépjen be too1, 023 megabájt / másodperc (MBps). Is kijelölése hello kezdő és a Befejezés **időpontokat a munkaidőhöz**, és a hello hét mely napján figyelembe vett munkanapok. Óra között útmutatóul szolgálnak a kijelölt munkahelyi kívül óra munkaórákon kívüli időre.
4. Kattintson az **OK** gombra.

### <a name="tooback-up-files-and-folders-for-hello-first-time"></a>az első alkalommal hello mappák és fájlok tooback
1. Hello biztonságimásolat-készítő ügynök, kattintson **biztonsági másolat készítése most** toocomplete hello kezdeti összehangolása hello hálózaton keresztül.

    ![Windows Server biztonsági másolat készítése](./media/backup-configure-vault/backup-now.png)
2. A hello megerősítése lapon, amely a biztonsági mentést most varázsló hello hello beállítások áttekintése hello gép tooback fogja használni. Ezután kattintson a **Biztonsági mentés** gombra.
3. Kattintson a **Bezárás** tooclose hello varázsló. Ha ezt teszi hello biztonsági mentési folyamat befejezése előtt, hello varázsló toorun hello háttérben folytatódik.

Hello kezdeti biztonsági mentés befejezése után hello **feladata befejezve** állapota megjelenik hello biztonsági mentés konzolban.

![IR befejezve](./media/backup-configure-vault/ircomplete.png)

## <a name="questions"></a>Kérdései vannak?
Ha kérdése van, vagy ha bármely új szolgáltatása része, toosee szeretné [visszajelzést küldhet](http://aka.ms/azurebackup_feedback).

## <a name="next-steps"></a>Következő lépések
Virtuális gépek vagy más munkaterhelések biztonsági mentésével kapcsolatos további információkért lásd:

* Most, hogy biztonsági másolatot készített a fájlokról és mappákról, [kezelheti a tárlókat és a kiszolgálókat](backup-azure-manage-windows-server.md).
* Ha szükség toorestore biztonsági másolat, akkor ez a cikk túl[fájlok tooa Windows számítógép visszaállítása](backup-azure-restore-windows-server.md).
