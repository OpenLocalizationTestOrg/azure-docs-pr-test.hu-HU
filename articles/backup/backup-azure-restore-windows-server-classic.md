---
title: "aaaRestore adatok tooa Windows Server vagy a Windows ügyfél Azure használja a klasszikus üzembe helyezési modellel hello |} Microsoft Docs"
description: "Megtudhatja, hogyan toorestore egy Windows Server vagy a Windows-ügyfél."
services: backup
documentationcenter: 
author: saurabhsensharma
manager: shivamg
editor: 
ms.assetid: 85585dfc-c764-4e8c-8f0e-40b969640ac2
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/11/2017
ms.author: saurse;trinadhk;markgal;
ms.openlocfilehash: 4d1458d5233c4f55004ecfa95cbf7b3b18a03dde
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="restore-files-tooa-windows-server-or-windows-client-machine-using-hello-classic-deployment-model"></a>Fájlok tooa Windows server vagy a Windows hello klasszikus üzembe helyezési modellt használó ügyfélszámítógép visszaállítása
> [!div class="op_single_selector"]
> * [Klasszikus portál](backup-azure-restore-windows-server-classic.md)
> * [Azure Portal](backup-azure-restore-windows-server.md)
>
>

Ez a cikk azt ismerteti, hogyan toorecover adatokat egy biztonsági másolatból tároló, és visszaállíthatja a tooa kiszolgálón vagy számítógépen. -Től kezdődően. március 2017, már nem készíthetők mentési tárolók hello a klasszikus portálon.

> [!IMPORTANT]
> Most már frissítheti a mentési tárolók tooRecovery szolgáltatások tárolókban. További információkért lásd: hello cikk [frissíteni a biztonsági mentési tároló tooa Recovery Services-tároló](backup-azure-upgrade-backup-to-recovery-services.md). A Microsoft tooupgrade támogatja a mentési tárolókat tooRecovery szolgáltatások tárolók.<br/> **2017. október 15**, akkor nem fog tudni toouse PowerShell toocreate mentési tárolókban. <br/> **2017. november 1-től kezdődően**:
>- A többi biztonsági mentési tárolók lesz automatikusan frissített tooRecovery szolgáltatások tárolók.
>- Ön nem fogja tudni tooaccess a biztonsági mentési adatok hello a klasszikus portálon. Ehelyett használjon hello Azure portál tooaccess a biztonsági mentési adatok a Recovery Services-tárolók.
>

toorestore adatok hello adatok helyreállítása varázsló használható a Microsoft Azure Recovery Services (MARS) ügynök hello. Adatok helyreállításakor lehetősége:

* Ugyanaz a számítógép melyik hello biztonsági másolatból visszaállítási adatok toohello vették.
* Állítsa vissza az adatok tooan másik gépen.

2017. január, a Microsoft, amely egy előzetes frissítés toohello MARS agent. Hibajavításokat tartalmaz, valamint a frissítés lehetővé teszi, hogy azonnali visszaállítása, amely lehetővé teszi toomount írható helyreállítási pont pillanatkép helyreállítási kötetként. Megismerheti a hello helyreállítási kötet, és másolja fájlok tooa helyi számítógép ezáltal visszaállítása fájlok majd.

> [!NOTE]
> Hello [2017. január Azure Backup frissítését](https://support.microsoft.com/en-us/help/3216528?preview) szükség, ha azt szeretné, hogy toouse azonnali visszaállítása toorestore adatokat. Hello biztonsági mentési adatok is a tárolók az hello támogatási cikkben szereplő területi beállításokhoz kell védeni. Tekintse át a hello [2017. január Azure Backup frissítését](https://support.microsoft.com/en-us/help/3216528?preview) hello területi beállításokat, amelyek támogatják a azonnali visszaállítása a legfrissebb listáját. Azonnali visszaállítás **nem** elérhetők az összes területi beállításokat.
>

Azonnali visszaállítási érhető el a Recovery Services-tárolók az Azure-portálon hello használata és a biztonsági mentési tárolók hello a klasszikus portálon. Ha azt szeretné, hogy azonnali visszaállítása toouse, hello MARS frissítés letöltése, és kövesse a hello eljárások, amelyek említik azonnali visszaállítása.


## <a name="use-instant-restore-toorecover-data-toohello-same-machine"></a>Azonnali visszaállítása toorecover adatok toohello használja egyazon számítógépen

Ha véletlenül törli a fájlt, és szeretné toorestore az egyazon számítógépen (mely hello a biztonsági mentés használatban van), a következő hello lépések toohello segít hello adatok helyreállítását.

1. Nyissa meg hello **a Microsoft Azure Backup szolgáltatás** az illesztési. Ha nem tudja, ahová a hello beépülő modul telepítve van, a keresés hello számítógép vagy kiszolgáló **a Microsoft Azure Backup szolgáltatás**.

    egy asztali alkalmazás hello hello keresési eredmények jelenjenek meg.

2. Kattintson a **adatok helyreállítása** toostart hello varázsló.

    ![Adatok helyreállítása](./media/backup-azure-restore-windows-server/recover.png)

3. A hello **bevezetés** ablaktáblában toorestore hello adatok toohello ugyanazon a kiszolgálón vagy a számítógépen, válassza ki a **ehhez a kiszolgálóhoz (`<server name>`)** kattintson **következő**.

    ![Válassza ki a kiszolgáló beállítás toorestore hello adatok toohello egyazon számítógépen](./media/backup-azure-restore-windows-server/samemachine_gettingstarted_instantrestore.png)

4. A hello **válassza ki a helyreállítási mód** ablaktáblán válassza **egyes fájlok és mappák** , majd **tovább**.

    ![Fájlok tallózása](./media/backup-azure-restore-windows-server/samemachine_selectrecoverymode_instantrestore.png)

5. A hello **mennyiség kiválasztása és a dátum** ablaktáblában hello fájlok és/vagy mappákról, amelyeket toorestore tartalmazó select hello kötetet.

    Hello naptárban válasszon ki egy helyreállítási pontot. Bármelyik helyreállítási pontra állíthatja vissza időben. A dátumok **félkövér** jelző hello legalább egy helyreállítási pont rendelkezésre állását. Miután egy dátumot, ha több helyreállítási pont nem érhető el, hello adott helyreállítási pontot választhat hello **idő** legördülő menü.

    ![Kötet és a dátum](./media/backup-azure-restore-windows-server/samemachine_selectvolumedate_instantrestore.png)

6. Miután kiválasztotta a hello helyreállítási pont toorestore, kattintson a **csatlakoztatási**.

    Azure biztonsági mentés hello helyi helyreállítási pontot csatlakoztatja, és egy helyreállítási kötet használja.

7. A hello **tallózással keresse meg és a fájlok helyreállítása** ablaktáblában kattintson **Tallózás** tooopen Windows Explorer és hello fájlok és mappák.

    ![Helyreállítási beállítások](./media/backup-azure-restore-windows-server/samemachine_browserecover_instantrestore.png)


8. A Windows Intézőben, hello fájlok másolása és/vagy mappák toorestore szeretné, és illessze be a tooany hely helyi toohello kiszolgálón vagy számítógépen. Nyissa meg vagy adatfolyam-fájlok hello közvetlenül hello helyreállítási kötet, és ellenőrizze a megfelelő verzió helyreállítás hello.

    ![Másolja és illessze be a fájlok és mappák csatlakoztatott kötet toolocal helyről](./media/backup-azure-restore-windows-server/samemachine_copy_instantrestore.png)

9. Ha elkészült a visszaállítási hello fájlok és/vagy mappák hello **tallózással keresse meg és a helyreállítási fájlokat** ablaktáblán kattintson a **leválasztási**. Kattintson a **Igen** tooconfirm, amelyet az toounmount hello kötet.

    ![Válassza le a hello kötet, és erősítse meg](./media/backup-azure-restore-windows-server/samemachine_unmount_instantrestore.png)

    > [!Important]
    > Ha nem kattint az leválasztási, hello helyreállítási kötet marad csatlakoztatott hello idő, amikor csatlakoztatva lett a hat órán át. Nincs biztonsági mentési műveletek fog futni, amíg hello kötet csatlakoztatva van. Minden ütemezett biztonsági mentési művelet toorun hello időszakban, amikor hello kötet csatlakoztatva van, miután hello helyreállítási kötet nem fog futni.
    >


## <a name="recover-data-toohello-same-machine"></a>Adatok toohello helyreállítása egyazon számítógépen
Ha véletlenül törli a fájlt, és szeretné toorestore az egyazon számítógépen (mely hello a biztonsági mentés használatban van), a következő hello lépések toohello segít hello adatok helyreállítását.

1. Nyissa meg hello **a Microsoft Azure Backup szolgáltatás** az illesztési.
2. Kattintson a **adatok helyreállítása** tooinitiate hello munkafolyamat.

    ![Adatok helyreállítása](./media/backup-azure-restore-windows-server-classic/recover.png)
3. Jelölje be hello  **ehhez a kiszolgálóhoz (*yourmachinename*) ** beállítás toorestore hello fájl biztonsági másolatát a hello azonos gép.

    ![Ugyanaz a gép](./media/backup-azure-restore-windows-server-classic/samemachine.png)
4. Válassza ki a túl**fájlok tallózása** vagy **fájlok keresése**.

    Hagyja hello alapértelmezett beállítást, ha azt tervezi, hogy toorestore elérési úton található egy vagy több fájlt. Ha hello mappaszerkezet kapcsolatban kérdése, de szeretné tenni, toosearch egy fájlhoz, válassza ki a hello **fájlok keresése** lehetőséget. A jelen szakasz hello célra azt folytatja hello alapértelmezett beállítás.

    ![Fájlok tallózása](./media/backup-azure-restore-windows-server-classic/browseandsearch.png)
5. Válassza ki, amelyből toorestore hello fájlt kívánja hello kötetet.

    Minden olyan pont állíthatja vissza időben. Megjelenő dátumok **félkövér** hello havinaptár-vezérlőben meg hello visszaállítási pont rendelkezésre állását. A kijelölt dátum alapján a biztonsági mentés ütemezése (és a biztonsági mentés sikeres hello), kiválaszthat egy pontot időben a hello **idő** legördülő listán.

    ![Kötet és a dátum](./media/backup-azure-restore-windows-server-classic/volanddate.png)
6. Válassza ki a hello elemek toorecover. Többszörös kiválasztási mappák és fájlok toorestore kívánja is.

    ![Fájlok kiválasztása](./media/backup-azure-restore-windows-server-classic/selectfiles.png)
7. Adjon meg hello helyreállítási paramétereket.

    ![Helyreállítási beállítások](./media/backup-azure-restore-windows-server-classic/recoveroptions.png)

   * Lehetősége van toohello eredeti helyükre (mely hello a fájl vagy mappa volna felülírja) vagy hello tooanother helyére állítja vissza egyazon számítógépen.
   * Ha hello fájl vagy mappa toorestore szerepel hello célhelyet kívánja, létrehozhat másolatok (hello két verziója ugyanazt a fájlt), írja felül a hello célhelyet hello fájlokat vagy hello fájlok hello cél létező hello helyreállításának kihagyása.
   * Erősen ajánlott, hogy hagyja bejelölve a helyreállítás alatt álló hello fájlokat hello ACL-ek visszaállításának hello alapértelmezett beállítás.
8. Amikor a bemeneti adatok érhetők el, kattintson a **következő**. hello helyreállítási munkafolyamat, amely visszaállítja hello fájlok toothis gépet, megkezdődik.

## <a name="recover-tooan-alternate-machine"></a>Tooan alternatív gép helyreállítása
A teljes kiszolgáló nem vesztek el, ha továbbra is helyreállítható adatok Azure biztonsági mentés tooa másik gépre. a lépéseket követve hello hello munkafolyamat mutatják be.  

hello terminológia a következő lépéseket tartalmazza:

* *Forrásgép* – hello eredeti számítógép melyik hello biztonsági mentésből készült, és amelyen jelenleg nem érhető el.
* *Célszámítógép* – hello gép toowhich hello adatok helyreállítása folyamatban.
* *Minta tároló* – hello biztonsági mentési tároló toowhich hello *forrásgép* és *célgépen* van regisztrálva. <br/>

> [!NOTE]
> Gépről készített biztonsági másolatok nem állítható vissza egy korábbi operációs rendszer hello szolgáltatást futtató gépen. Például ha biztonsági mentést készít a Windows 7 gépről, akkor visszaállítása végezhető el a Windows 8 vagy újabb gép. Azonban hello viszont nem rendelkezik igaz.
>
>

1. Nyissa meg hello **a Microsoft Azure Backup szolgáltatás** az illesztési hello *célgépen*.
2. Győződjön meg arról, hogy hello *célgépen* és hello *forrásgép* regisztrált toohello vannak azonos mentési tároló.
3. Kattintson a **adatok helyreállítása** tooinitiate hello munkafolyamat.

    ![Adatok helyreállítása](./media/backup-azure-restore-windows-server-classic/recover.png)
4. Válassza ki **egy másik kiszolgáló**

    ![Egy másik kiszolgáló](./media/backup-azure-restore-windows-server-classic/anotherserver.png)
5. Adja meg a hello tárolói hitelesítő adatok fájlját, amely megfelel a toohello *minta tároló*. Ha a hello tárolói hitelesítő adatok fájlját az érvénytelen (vagy lejárt) le új tárolói hitelesítő adatok fájlt hello *minta tároló* a klasszikus Azure portálon hello. Hello tárolói hitelesítő adatok fájlját valósul meg, miután hello mentési tároló elleni hello tárolói hitelesítő adatok fájlját jelenik meg.
6. Jelölje be hello *forrásgép* megjelenített gépek hello listája.

    ![Számítógépek listája](./media/backup-azure-restore-windows-server-classic/machinelist.png)
7. Válassza ki bármelyik hello **fájlok keresése** vagy **fájlok tallózása** lehetőséget. A jelen szakasz hello célra használjuk hello **fájlok keresése** lehetőséget.

    ![Keresés](./media/backup-azure-restore-windows-server-classic/search.png)
8. Válassza ki a hello mennyiség és a dátum a következő képernyőn hello. Keresési hello fájl/mappa neve toorestore keresi.

    ![Keresési elemek](./media/backup-azure-restore-windows-server-classic/searchitems.png)
9. Ha a hello fájlok toobe vissza kell hello hely kiválasztása.

    ![Hely visszaállítása](./media/backup-azure-restore-windows-server-classic/restorelocation.png)
10. Adja meg, amely során hello titkosítási jelszó *forrás gép* regisztrációs túl*minta tároló*.

    ![Titkosítás](./media/backup-azure-restore-windows-server-classic/encryption.png)
11. Amikor hello bemeneti valósul meg, kattintson a **helyreállítása**, mely eseményindítók hello helyreállítására irányuló biztonsági mentés megadott fájlok toohello célt hello.

## <a name="use-instant-restore-toorestore-data-tooan-alternate-machine"></a>Azonnali visszaállítása toorestore adatok tooan másik számítógépen
A teljes kiszolgáló nem vesztek el, ha továbbra is helyreállítható adatok Azure biztonsági mentés tooa másik gépre. a lépéseket követve hello hello munkafolyamat mutatják be.

hello terminológia a következő lépéseket tartalmazza:

* *Forrásgép* – hello eredeti számítógép melyik hello biztonsági mentésből készült, és amelyen jelenleg nem érhető el.
* *Célszámítógép* – hello gép toowhich hello adatok helyreállítása folyamatban.
* *Minta tároló* – hello Recovery Services-tároló toowhich hello *forrásgép* és *célgépen* van regisztrálva. <br/>

> [!NOTE]
> Biztonsági mentések nem lehet visszaállított tooa hello operációs rendszer korábbi verzióját futtató célszámítógépen. Például egy készült biztonsági másolatok a Windows 7 számítógép lehet vissza a Windows 8-as vagy újabb verzióját, számítógép. Egy készült biztonsági másolatok a Windows 8 számítógépről nem lehet visszaállított tooa Windows 7 számítógép.
>
>

1. Nyissa meg hello **a Microsoft Azure Backup szolgáltatás** az illesztési hello *célgépen*.

2. Győződjön meg arról hello *célgépen* és hello *forrásgép* azonos Recovery Services-tároló regisztrált toohello vannak.

3. Kattintson a **adatok helyreállítása** tooopen hello **adatok helyreállítása varázsló**.

    ![Adatok helyreállítása](./media/backup-azure-restore-windows-server/recover.png)

4. A hello **bevezetés** ablaktáblán válassza előbb **egy másik kiszolgáló**

    ![Egy másik kiszolgáló](./media/backup-azure-restore-windows-server/alternatemachine_gettingstarted_instantrestore.png)

5. Adja meg a hello tárolói hitelesítő adatok fájlját, amely megfelel a toohello *minta tároló*, és kattintson a **tovább**.

    Ha hello tárolói hitelesítő adatok fájlját érvénytelen (vagy lejárt), egy új tárolói hitelesítő adatok fájlját letöltését hello *minta tároló* a hello Azure-portálon. Miután megadta a egy érvényes tároló hitelesítő adatait, megfelelő mentési tároló hello hello neve jelenik meg.

6. A hello **biztonsági mentés kiszolgáló kiválasztása** ablaktáblában válassza hello *forrásgép* megjelenített gépek hello listából, és adja meg a hello jelszót. Ezután kattintson a **Next** (Tovább) gombra.

    ![Számítógépek listája](./media/backup-azure-restore-windows-server/alternatemachine_selectmachine_instantrestore.png)

7. A hello **válassza ki a helyreállítási mód** ablaktáblán válassza ki az **egyes fájlok és mappák** kattintson **következő**.

    ![Keresés](./media/backup-azure-restore-windows-server/alternatemachine_selectrecoverymode_instantrestore.png)

8. A hello **mennyiség kiválasztása és a dátum** ablaktáblában hello fájlok és/vagy mappákról, amelyeket toorestore tartalmazó select hello kötetet.

    Hello naptárban válasszon ki egy helyreállítási pontot. Bármelyik helyreállítási pontra állíthatja vissza időben. A dátumok **félkövér** jelző hello legalább egy helyreállítási pont rendelkezésre állását. Miután egy dátumot, ha több helyreállítási pont nem érhető el, hello adott helyreállítási pontot választhat hello **idő** legördülő menü.

    ![Keresési elemek](./media/backup-azure-restore-windows-server/alternatemachine_selectvolumedate_instantrestore.png)

9. Kattintson a **csatlakoztatási** toolocally csatlakoztatási hello helyreállítási pont a helyreállítási kötetként a *célgépen*.

10. A hello **tallózással keresse meg és a fájlok helyreállítása** ablaktáblában kattintson **Tallózás** tooopen Windows Explorer és hello fájlok és mappák.

    ![Titkosítás](./media/backup-azure-restore-windows-server/alternatemachine_browserecover_instantrestore.png)

11. A Windows Intézőben hello fájlok és/vagy mappák másolása hello helyreállítási kötet, és illessze be a tooyour *célgépen* helyét. Nyissa meg vagy adatfolyam-fájlok hello közvetlenül hello helyreállítási kötet, és ellenőrizze a megfelelő verzió helyreállítás hello.

    ![Titkosítás](./media/backup-azure-restore-windows-server/alternatemachine_copy_instantrestore.png)

12. Ha elkészült a visszaállítási hello fájlok és/vagy mappák hello **tallózással keresse meg és a helyreállítási fájlokat** ablaktáblán kattintson a **leválasztási**. Kattintson a **Igen** tooconfirm, amelyet az toounmount hello kötet.

    ![Titkosítás](./media/backup-azure-restore-windows-server/alternatemachine_unmount_instantrestore.png)

    > [!Important]
    > Ha nem kattint az leválasztási, hello helyreállítási kötet marad csatlakoztatott hello idő, amikor csatlakoztatva lett a hat órán át. Nincs biztonsági mentési műveletek fog futni, amíg hello kötet csatlakoztatva van. Minden ütemezett biztonsági mentési művelet toorun hello időszakban, amikor hello kötet csatlakoztatva van, miután hello helyreállítási kötet nem fog futni.
    >


## <a name="next-steps"></a>Következő lépések
* [Az Azure biztonsági mentési – gyakori kérdések](backup-azure-backup-faq.md)
* A Microsoft hello [Azure biztonsági mentés fórum](http://go.microsoft.com/fwlink/p/?LinkId=290933).

## <a name="learn-more"></a>Részletek
* [Az Azure biztonsági mentés áttekintése](http://go.microsoft.com/fwlink/p/?LinkId=222425)
* [Az Azure virtuális gépek biztonsági mentése](backup-azure-vms-introduction.md)
* [Fel Microsoft-munkaterhelések biztonsági mentése](backup-azure-dpm-introduction.md)
