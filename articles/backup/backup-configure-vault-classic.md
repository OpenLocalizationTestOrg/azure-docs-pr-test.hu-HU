---
title: "Windows server vagy a munkaállomás tooAzure (klasszikus modell) mentése aaaBack |} Microsoft Docs"
description: "A biztonsági mentés Windows kiszolgálókat vagy ügyfeleket tooa mentési tárolóból, amelyben az Azure-ban. Halad át a fájlok és mappák tooa védelméhez alapjai Backup-tárolóban hello Azure Backup szolgáltatás ügynökének használatával."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keywords: "mentési tároló; Készítsen biztonsági másolatot a Windows server; biztonsági mentési Időablakok;"
ms.assetid: 3b543bfd-8978-4f11-816a-0498fe14a8ba
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/11/2017
ms.author: markgal;trinadhk;
ms.openlocfilehash: c8f2a9bed1e5885f5c272c65b9282ededc05850a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="back-up-a-windows-server-or-workstation-tooazure-using-hello-classic-portal"></a>Készítsen biztonsági másolatot a Windows server vagy a munkaállomás tooAzure hello klasszikus portál használatával
> [!div class="op_single_selector"]
> * [Klasszikus portál](backup-configure-vault-classic.md)
> * [Azure Portal](backup-configure-vault.md)
>
>

Ez a cikk a toofollow tooprepare a környezetre van szükség, és készítsen biztonsági másolatot a Windows server (vagy munkaállomás) tooAzure hello eljárásokat ismerteti. A biztonsági mentési megoldások telepítésének szempontjai is magában foglalja. Ha szeretné használni az Azure biztonsági mentés közben a hello először, ebben a cikkben gyorsan végigvezeti hello folyamatán.

Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: Resource Manager és klasszikus. Ez a cikk hello klasszikus telepítési modell használatát bemutatja. A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja.

## <a name="before-you-start"></a>Előkészületek
egy kiszolgáló vagy az ügyfél tooAzure tooback, kell az Azure-fiók. Ha még nincs fiókja, létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/) néhány percig.

## <a name="create-a-backup-vault"></a>Backup-tároló létrehozása
fájlok és mappák kiszolgálói vagy ügyfél tooback, toocreate egy mentési tárolót, ahová toostore hello adatokat hello földrajzi régióban kell.

> [!IMPORTANT]
> 2017. március-től kezdődően már nem használható hello klasszikus portál toocreate mentési tárolókban.
>
> Most már frissítheti a mentési tárolók tooRecovery szolgáltatások tárolókban. További információkért lásd: hello cikk [frissíteni a biztonsági mentési tároló tooa Recovery Services-tároló](backup-azure-upgrade-backup-to-recovery-services.md). A Microsoft tooupgrade támogatja a mentési tárolókat tooRecovery szolgáltatások tárolók.<br/> **2017. október 15**, akkor nem fog tudni toouse PowerShell toocreate mentési tárolókban. <br/> **2017. november 1-től kezdődően**:
>- A többi biztonsági mentési tárolók lesz automatikusan frissített tooRecovery szolgáltatások tárolók.
>- Ön nem fogja tudni tooaccess a biztonsági mentési adatok hello a klasszikus portálon. Ehelyett használjon hello Azure portál tooaccess a biztonsági mentési adatok a Recovery Services-tárolók.
>


## <a name="download-hello-vault-credential-file"></a>Hello tárolói hitelesítő adatok fájlját letöltése
hello a helyi számítógépen kell toobe biztonsági mentését adatok tooAzure is azelőtt hitelesíthetők, a mentési tárolóban. hello hitelesítési sorrendekben *hitelesítő adatokat tároló*. hello tárolói hitelesítő adatok fájlját letöltődik a klasszikus portálon hello egy biztonságos csatornán keresztül. hello tanúsítvány titkos kulcsa nem maradnak hello portálon vagy hello szolgáltatást.

### <a name="toodownload-hello-vault-credential-file-tooa-local-machine"></a>toodownload hello tárolói hitelesítő adatok fájl tooa helyi számítógép
1. Hello bal oldali navigációs ablaktábláján kattintson **Recovery Services**, majd válassza ki a létrehozott hello mentési tároló.

    ![IR befejezve](./media/backup-configure-vault-classic/rs-left-nav.png)
2. Hello gyors kezdés lapon kattintson **letöltési tárolói hitelesítő adatokat**.

   hello klasszikus portál hello tároló nevére és hello aktuális dátum együttes használatával hoz létre a tároló hitelesítő adatait. hello tárolói hitelesítő adatok fájlja csak hello regisztrációs munkamenet során használt, és 48 óra múlva lejár.

   hello tárolói hitelesítő adatok fájlját hello portal letölthetők.
3. Kattintson a **mentése** toodownload hello tárolói hitelesítő adatok fájl toohello Letöltések mappába hello helyi fiók. Igény szerint kiválaszthatja **Mentés másként** a hello **mentése** menü toospecify hello tárolói hitelesítő adatok fájlját helyét.

   > [!NOTE]
   > Ellenőrizze, hogy hello tárolói hitelesítő adatok fájlját az egy helyre mentik el, hogy a számítógép elérhető. Ha egy fájl megosztás vagy kiszolgálói üzenetblokk tárolódik, ellenőrizze, hogy rendelkezik-e hello engedélyek tooaccess azt.
   >
   >

## <a name="download-install-and-register-hello-backup-agent"></a>Letöltése, telepítése és regisztrálása hello Backup szolgáltatás ügynöke
Miután létrehozta a mentési tároló hello és letöltési hello tárolói hitelesítő adatok fájlját, az ügynököt telepíteni kell a Windows-alapú gépek minden egyes.

### <a name="toodownload-install-and-register-hello-agent"></a>toodownload, telepítése, és hello ügynök regisztrálása
1. Kattintson a **Recovery Services**, majd válassza ki, hogy szeretné-e a kiszolgálóval tooregister hello mentési tároló.
2. A hello gyors kezdés lapon kattintson a hello ügynök **ügynök Windows vagy a System Center Data Protection Manager vagy a Windows ügyfél**. Ezután kattintson a **Save** (Mentés) gombra.

    ![Ügynök mentése](./media/backup-configure-vault-classic/agent.png)
3. Miután sikeresen letöltötte a hello MARSagentinstaller.exe fájlt, kattintson **futtatása** (vagy kettős kattintással **MARSAgentInstaller.exe** hello mentett helyről).
4. Válassza ki a hello telepítési mappa és a gyorsítótár mappája hello ügynök számára szükséges, és kattintson a **következő**. hello gyorsítótár helyét adja meg, hogy egyenlő szabad terület tooat rendelkeznie kell legalább 5 százalékát hello biztonsági mentési adatokat.
5. Folytatás tooconnect toohello interneten keresztül hello alapértelmezett proxybeállításokat.             Használatakor a proxy server tooconnect toohello Internet, hello Proxy konfigurálása lapon válassza ki a hello **egyéni proxybeállításainak használata** jelölje be a jelölőnégyzetet, és írja be a hello proxy kiszolgáló adatait. Ha egy hitelesített proxykiszolgálót használ, írja be a hello felhasználói nevet és jelszót adatait, és kattintson **következő**.
6. Kattintson a **telepítése** toobegin hello az ügynök telepítése. hello Backup szolgáltatás ügynökének telepíti a .NET-keretrendszer 4.5 és Windows PowerShell (Ha még nincs telepítve) toocomplete hello telepítését.
7. Hello ügynök telepítése után kattintson **tooRegistration folytatható** toocontinue hello a munkafolyamathoz.
8. Keresse meg hello tároló azonosítás lapja, tooand válassza hello tárolói hitelesítő adatok fájlját korábban letöltött.

    csak 48 órán hello portálról nincs letöltve hello tárolói hitelesítő adatok fájlját esetén érvényes. Ha hibát észlel a ezen a lapon (például "tárolói hitelesítő adatok megadott fájlja lejárt"), toohello portálon bejelentkezhet, és töltse le ismét hello tárolói hitelesítő adatok fájlját.

    Győződjön meg arról, hogy hello tárolói hitelesítő adatok fájlját elérhető hello telepítő alkalmazás által elérhető helyen. Ha a hozzáféréssel kapcsolatos hibákat tapasztal, másolja a hello tárolói hitelesítő adatok tooa ideiglenes helye a hello azonos számítógéphez, és próbálja megismételni a műveletet hello.

    Ha például a "érvénytelen tárolóban megadott hitelesítő adatok" a tárolói hitelesítő adatok hiba merül fel, hello fájl sérült, vagy nem rendelkezik hello hello helyreállítási szolgáltatáshoz tartozó legújabb hitelesítő adatokat. Zónák hello egy új tárolói hitelesítő adatok fájlját letöltése hello portálról. Ez a hiba akkor is előfordulhat, ha egy felhasználó a hello **letöltési tárolóhitelesítő adatokat** beállítás többször gyors egymásutánban. Ebben az esetben csak hello utolsó tárolói hitelesítő adatok fájlját esetén érvényes.
9. Az oldalon hello titkosítási beállítás egy hozzáférési kódot létrehozni, vagy adjon meg egy jelszót (minimum 16 karakter). Ne felejtse el toosave hello jelszót biztonságos helyen.
10. Kattintson a **Befejezés** gombra. Kiszolgáló regisztrálása varázsló hello hello-kiszolgáló regisztrálása biztonsági mentéssel.

    > [!WARNING]
    > Ha elveszíti vagy elfelejti hello jelszót, a Microsoft nem segítséget hello biztonsági mentési adatokat. Ön a tulajdonosa hello titkosítási jelszó, és a Microsoft nem látnak bele, amelyekkel hello jelszót. Hello fájlt menteni egy biztonságos helyre, mert lesz szükség a helyreállítási művelet során.
    >
    >

11. Hello titkosítási kulcs beállítása után hagyja hello **indítsa el a Microsoft Azure Recovery Services Agent** jelölőnégyzetet, és kattintson a **Bezárás**.

## <a name="complete-hello-initial-backup"></a>Teljes hello kezdeti biztonsági másolatot
hello kezdeti biztonsági másolatot a két fő feladatokból áll:

* Hello biztonsági mentési ütemezés létrehozása
* Fájlok és mappák biztonsági mentése a hello első alkalommal

A biztonsági mentési házirend hello hello kezdeti biztonsági mentés befejezése után is használhatja, ha szüksége van toorecover hello adatok biztonsági mentési pontok hoz létre. a biztonsági mentési házirend hello ezt a meghatározott hello ütemezés szerint végzi.

### <a name="tooschedule-hello-backup"></a>tooschedule hello biztonsági mentése
1. Nyissa meg a Microsoft Azure Backup szolgáltatás ügynökének hello. (Az nyílik meg automatikusan Ha hello hagyta **indítsa el a Microsoft Azure Recovery Services Agent** jelölőnégyzet be van jelölve hello kiszolgáló regisztrálása varázsló bezárásakor.) A megkereséséhez keressen rá a gépen a **Microsoft Azure Backup** kifejezésre.

    ![Indítsa el a hello Azure Backup szolgáltatás ügynöke](./media/backup-configure-vault-classic/snap-in-search.png)
2. Hello Backup szolgáltatás ügynökének, kattintson **biztonsági mentés ütemezése**.

    ![Windows Server biztonsági mentés ütemezése](./media/backup-configure-vault-classic/schedule-backup-close.png)
3. A hello bevezetés hello ütemezett biztonsági mentés varázsló lapján, kattintson a **következő**.
4. Kattintson hello elemek kijelölése tooBackup lap **elemek hozzáadása**.
5. Válassza ki a hello fájlok és mappák tooback szeretné, hogy fel, és kattintson a **gépházban**.
6. Kattintson a **Tovább** gombra.
7. A hello **adja meg a biztonsági mentés ütemezése** adja meg azokat a hello **biztonsági mentés ütemezése** kattintson **következő**.

    Napi (legfeljebb napi háromszori) vagy heti biztonsági mentéseket ütemezhet.

    ![Windows Server biztonsági mentés elemei](./media/backup-configure-vault-classic/specify-backup-schedule-close.png)

   > [!NOTE]
   > Hogyan toospecify hello biztonsági mentés ütemezése kapcsolatos további információkért lásd: hello cikk [használata Azure biztonsági mentési tooreplace a szalag infrastruktúra](backup-azure-backup-cloud-as-tape.md).
   >
   >

8. A hello **válassza ki az adatmegőrzési** lapra, jelölje be hello **adatmegőrzési** a hello biztonsági másolatot.

    hello adatmegőrzési, amelynek hello biztonsági másolatot a rendszer hello időtartamát határozza meg. Ahelyett, hogy csak adja meg az összes biztonsági mentési pont "egyszerű policy", a másik adatmegőrzési hello biztonsági mentés esetén alapján is megadhat. Hello napi, heti, havi és éves megőrzési házirendek toomeet módosíthatja az igényeinek.
9. Hello kezdeti biztonsági mentési típusának kiválasztása lapon válassza a hello kezdeti biztonsági mentés típusát. Hagyja hello beállítást **automatikusan hello hálózaton keresztül** kiválasztva, és kattintson **következő**.

    Biztonsági másolatot készíthet automatikusan hello hálózaton keresztül, vagy a biztonsági mentést készíthet offline állapotba. hello a cikk hátralévő része a biztonsági mentési automatikusan hello folyamatot írja le. Ha jobban szeret toodo offline biztonsági másolat, tekintse át a hello cikk [az Azure Backup Offline biztonsági másolat munkafolyamat](backup-azure-backup-import-export.md) további információt.
10. A hello megerősítése lapon tekintse át hello adatokat, és kattintson **Befejezés**.
11. Hello biztonsági mentési ütemezés létrehozása hello varázsló befejezése után kattintson **Bezárás**.

### <a name="enable-network-throttling-optional"></a>(Választható)-szabályozás engedélyezése
hello Backup szolgáltatás ügynökének biztosít a hálózati sávszélesség-szabályozás. Szabályozza, hogyan adatátvitel során használt hálózati sávszélesség-szabályozás. Ez a vezérlő akkor lehet hasznos, ha tooback adatokat kell munkaidőben, de nem hello biztonsági mentési folyamat toointerfere a más internetes forgalmat. Sávszélesség-szabályozás tooback vonatkozik fel, és állítsa vissza a tevékenységeket.

**tooenable hálózati sávszélesség-szabályozás**

1. Hello Backup szolgáltatás ügynökének, kattintson **tulajdonságainak módosítása**.

    ![Tulajdonságainak módosítása](./media/backup-configure-vault-classic/change-properties.png)
2. A hello **sávszélesség-szabályozási** lapra, jelölje be hello **engedélyezi az internetes sávszélesség szabályozásának a biztonsági mentési műveleteknél** jelölőnégyzetet.

    ![Hálózati sávszélesség-szabályozás](./media/backup-configure-vault-classic/throttling-dialog.png)
3. Miután engedélyezte a sávszélesség-szabályozás, adja meg a biztonsági mentési adatátvitel során sávszélesség engedélyezett hello **időpontokat a munkaidőhöz** és **munkaidőn kívüli**.

    hello sávszélesség értékek 512 kilobit / másodperc (Kbps) kezdődik, és lépjen be too1, 023 megabájt / másodperc (MBps). Is kijelölése hello kezdő és a Befejezés **időpontokat a munkaidőhöz**, és a hello hét mely napján figyelembe vett munkanapok. Óra között útmutatóul szolgálnak a kijelölt munkahelyi kívül óra munkaórákon kívüli időre.
4. Kattintson az **OK** gombra.

### <a name="tooback-up-now"></a>most már be tooback
1. Hello Backup szolgáltatás ügynökének, kattintson **biztonsági másolat készítése most** toocomplete hello kezdeti összehangolása hello hálózaton keresztül.

    ![Windows Server biztonsági másolat készítése](./media/backup-configure-vault-classic/backup-now.png)
2. A hello megerősítése lapon, amely a biztonsági mentést most varázsló hello hello beállítások áttekintése hello gép tooback fogja használni. Ezután kattintson a **Biztonsági mentés** gombra.
3. Kattintson a **Bezárás** tooclose hello varázsló. Ha ezt teszi hello biztonsági mentési folyamat befejezése előtt, hello varázsló toorun hello háttérben folytatódik.

Hello kezdeti biztonsági mentés befejezése után hello **feladata befejezve** állapota megjelenik hello biztonsági mentés konzolban.

![IR befejezve](./media/backup-configure-vault-classic/ircomplete.png)

## <a name="next-steps"></a>Következő lépések
* Regisztráljon egy [ingyenes Azure-fiókot](https://azure.microsoft.com/free/).

Virtuális gépek vagy más munkaterhelések biztonsági mentésével kapcsolatos további információkért lásd:

* [Készítsen biztonsági másolatot IaaS virtuális gépeket](backup-azure-vms-prepare.md)
* [Készítsen biztonsági másolatot a Microsoft Azure Backup Server munkaterhelések tooAzure](backup-azure-microsoft-azure-backup.md)
* [Készítsen biztonsági másolatot a dpm-mel munkaterhelések tooAzure](backup-azure-dpm-introduction.md)
