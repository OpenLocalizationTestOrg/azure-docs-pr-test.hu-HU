---
title: "aaaRestore adatok Azure tooa Windows Server vagy Windows-számítógép |} Microsoft Docs"
description: "Ismerje meg, hogyan toorestore az tárolt Azure tooa Windows Server vagy a Windows rendszerű számítógépen."
services: backup
documentationcenter: 
author: saurabhsensharma
manager: shivamg
editor: 
ms.assetid: 742f4b9e-c0ab-4eeb-8e22-ee29b83c22c4
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 8/16/2017
ms.author: saurse;trinadhk;markgal;
ms.openlocfilehash: 11335495e448986a74f1733406f87e04331641d7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="restore-files-tooa-windows-server-or-windows-client-machine-using-resource-manager-deployment-model"></a>Fájlok tooa Windows server vagy a Windows ügyfél gép Resource Manager üzembe helyezési modellben visszaállítása
> [!div class="op_single_selector"]
> * [Azure Portal](backup-azure-restore-windows-server.md)
> * [Klasszikus portál](backup-azure-restore-windows-server-classic.md)
>
>

Ez a cikk azt ismerteti, hogyan toorestore adatokat a biztonsági mentési tárolóból. toorestore adatok hello adatok helyreállítása varázsló használható a Microsoft Azure Recovery Services (MARS) ügynök hello. Adatok helyreállításakor lehetősége:

* Ugyanaz a számítógép melyik hello biztonsági másolatból visszaállítási adatok toohello vették.
* Állítsa vissza az adatok tooan másik gépen.

2017. január, a Microsoft, amely egy előzetes frissítés toohello MARS agent. Hibajavításokat tartalmaz, valamint a frissítés lehetővé teszi, hogy azonnali visszaállítása, amely lehetővé teszi toomount írható helyreállítási pont pillanatkép helyreállítási kötetként. Megismerheti a hello helyreállítási kötet, és másolja fájlok tooa helyi számítógép ezáltal visszaállítása fájlok majd.

> [!NOTE]
> Hello [2017. január Azure Backup frissítését](https://support.microsoft.com/en-us/help/3216528?preview) szükség, ha azt szeretné, hogy toouse azonnali visszaállítása toorestore adatokat. Hello biztonsági mentési adatok is a tárolók az hello támogatási cikkben szereplő területi beállításokhoz kell védeni. Tekintse át a hello [2017. január Azure Backup frissítését](https://support.microsoft.com/en-us/help/3216528?preview) hello területi beállításokat, amelyek támogatják a azonnali visszaállítása a legfrissebb listáját. Azonnali visszaállítás **nem** elérhetők az összes területi beállításokat.
>

Azonnali visszaállítási érhető el a Recovery Services-tárolók az Azure-portálon hello használata és a biztonsági mentési tárolók hello a klasszikus portálon. Ha azt szeretné, hogy azonnali visszaállítása toouse, hello MARS frissítés letöltése, és kövesse a hello eljárások, amelyek említik azonnali visszaállítása.

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]

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
    > Ha leválasztási gombra nem kattint, hello helyreállítási kötet marad csatlakoztatott hat órán át hello óta, amikor csatlakoztatva lett. Azonban hello csatlakoztatási ideje kiterjesztett legfeljebb 24 óra egy folyamatban lévő fájlmásolás esetén. Nincs biztonsági mentési műveletek fog futni, amíg hello kötet csatlakoztatva van. Minden ütemezett biztonsági mentési művelet toorun hello időszakban, amikor hello kötet csatlakoztatva van, miután hello helyreállítási kötet nem fog futni.
    >


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
    > Ha leválasztási gombra nem kattint, hello helyreállítási kötet marad csatlakoztatott hat órán át hello óta, amikor csatlakoztatva lett. Azonban hello csatlakoztatási ideje kiterjesztett legfeljebb 24 óra egy folyamatban lévő fájlmásolás esetén. Nincs biztonsági mentési műveletek fog futni, amíg hello kötet csatlakoztatva van. Minden ütemezett biztonsági mentési művelet toorun hello időszakban, amikor hello kötet csatlakoztatva van, miután hello helyreállítási kötet nem fog futni.
    >

## <a name="troubleshooting"></a>Hibaelhárítás
Ha az Azure Backup szolgáltatás sikeresen csatlakoztatható hello helyreállítási kötet több percig való kattintás után is **csatlakoztatási** vagy sikertelen toomount hello helyreállítási kötet egy vagy több hiba, kövesse az alábbi helyreállítása általában toobegin hello lépéseket.

1.  Szakítsa meg hello folyamatban lévő csatlakozási folyamat, abban az esetben, ha több percig futott.

2.  Győződjön meg arról, hogy hello hello Azure Backup szolgáltatás ügynökének legújabb verzióját használja. toofind hello verzió adatok Azure Backup szolgáltatás ügynökének, kattintson a **kapcsolatban a Microsoft Azure Recovery Services Agent** a hello **műveletek** ablaktábla a Microsoft Azure Backup szolgáltatás konzolon, és győződjön meg arról, hogy hello  **Verzió** értéke nagyobb, mint az említett hello verzió egyenlő tooor [Ez a cikk](https://go.microsoft.com/fwlink/?linkid=229525). Letöltheti a legújabb verziót hello [Itt](https://go.microsoft.com/fwLink/?LinkID=288905)

3.  Nyissa meg túl**Eszközkezelő** -> **tárolóvezérlők** győződjön meg arról, keresse meg és **Microsoft iSCSI-kezdeményező**. Ha azok kikereshetők, közvetlenül nyissa meg az alábbi 7 toostep. 

4.  Ha a Microsoft iSCSI-kezdeményező szolgáltatás nem találja, ahogy azt korábban említettük, a 3. lépésben, ellenőrizze toosee, ha a bejegyzés alatt található **Eszközkezelő** -> **tárolóvezérlők** nevű  **Ismeretlen eszköz** hardver azonosítójú **ROOT\ISCSIPRT**.

5.  Kattintson a jobb gombbal **ismeretlen eszköz** válassza **illesztőprogram frissítése**.

6.  Hello illesztőprogram frissítése hello beállítással túl **automatikusan frissített illesztőprogram keresése**. Hello frissítés megvalósításának meg kell változtatni **ismeretlen eszköz** túl**Microsoft iSCSI-kezdeményező** alább látható módon. 

    ![Titkosítás](./media/backup-azure-restore-windows-server/UnknowniSCSIDevice.png)

7.  Nyissa meg túl**Feladatkezelő** -> **szolgáltatások (helyi)** -> **Microsoft iSCSI-kezdeményező szolgáltatás**. 

    ![Titkosítás](./media/backup-azure-restore-windows-server/MicrosoftInitiatorServiceRunning.png)
    
8.  Indítsa újra a hello Microsoft iSCSI-kezdeményező szolgáltatás hello szolgáltatásban, ehhez kattintson a jobb gombbal kattintva **leállítása** és a jobb gombbal kattint rá újra, és kattintson a Tovább **Start**.

9.  Ismételje meg a helyreállítás azonnali visszaállítás segítségével. 

Hello helyreállítási továbbra is sikertelen, ha indítsa újra a kiszolgálót vagy Windows-ügyfélen. Ha a számítógép újraindítása nem kívánatos vagy hello helyreállítási továbbra sem sikerül hello kiszolgáló újraindítása után is, végezze el egy másik gép, és forduljon a Azure támogatja címen túl[Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) és a támogatási kérelem elküldése.

## <a name="next-steps"></a>Következő lépések
* Most, hogy a fájlok és mappák már helyreállítva, akkor [kezelheti a biztonsági másolatok](backup-azure-manage-windows-server.md).
