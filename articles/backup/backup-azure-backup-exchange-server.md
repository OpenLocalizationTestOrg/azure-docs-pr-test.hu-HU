---
title: "az Exchange server tooAzure System Center 2012 R2 DPM biztonsági másolatának mentése aaaBack |} Microsoft Docs"
description: "Megtudhatja, hogyan mentése az Exchange server tooAzure tooback biztonsági mentését, a System Center 2012 R2 DPM használatával"
services: backup
documentationcenter: 
author: MaanasSaran
manager: NKolli1
editor: 
ms.assetid: 13f32256-888e-416e-a78b-40c2a26a5939
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2016
ms.author: masaran;jimpark;delhan;trinadhk;markgal
ms.openlocfilehash: fa99296d095c180333474b6d419ebc5ec727547a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="back-up-an-exchange-server-tooazure-backup-with-system-center-2012-r2-dpm"></a>Készítsen biztonsági másolatot az Exchange server tooAzure a System Center 2012 R2 DPM biztonsági mentése
Ez a cikk ismerteti, hogyan tooconfigure be a Microsoft Exchange-kiszolgálót a System Center 2012 R2 Data Protection Manager (DPM) kiszolgáló tooback túl Azure biztonsági mentés.  

## <a name="updates"></a>Frissítések
toosuccessfully register hello DPM-kiszolgáló Azure Backup szolgáltatásban, telepítenie kell a hello legújabb kumulatív frissítés a System Center 2012 R2 DPM és a hello hello Azure Backup szolgáltatás ügynökének legújabb verzióját. Hello legújabb kumulatív beszerezni hello [Microsoft Catalog](http://catalog.update.microsoft.com/v7/site/Search.aspx?q=System%20Center%202012%20R2%20Data%20protection%20manager).

> [!NOTE]
> A cikkben szereplő példák hello hello Azure Backup szolgáltatás ügynökének 2.0.8719.0 verziója van telepítve, és 6. kumulatív frissítés a System Center 2012 R2 DPM telepítve van.
>
>

## <a name="prerequisites"></a>Előfeltételek
A folytatás előtt győződjön meg arról, hogy az összes hello [Előfeltételek](backup-azure-dpm-introduction.md#prerequisites) a Microsoft Azure Backup segítségével tooprotect munkaterhelések teljesült. Az Előfeltételek hello alábbiakat foglalja magába:

* Egy hello Azure hely a biztonsági mentési tároló létrehozása.
* Ügynök és a tároló hitelesítő adatok már letöltött toohello DPM-kiszolgálón.
* hello ügynök hello DPM-kiszolgálóra van telepítve.
* hello tárolói hitelesítő adatok lettek használt tooregister hello DPM-kiszolgálón.
* Exchange 2016 véd, akkor frissítse a 2012 R2 UR9 tooDPM vagy újabb verzió

## <a name="dpm-protection-agent"></a>A DPM védelmi ügynök
tooinstall hello DPM védelmi ügynök hello Exchange-kiszolgálón, kövesse az alábbi lépéseket:

1. Győződjön meg arról, hogy hello tűzfalak helyesen van-e konfigurálva. Lásd: [hello ügynök tűzfalkivétel konfigurálása](https://technet.microsoft.com/library/Hh758204.aspx).
2. Hello ügynök telepíthető hello Exchange-kiszolgálóhoz gombra kattintva **felügyeleti > ügynökökkel > telepítése** a DPM felügyeleti konzolján. Lásd: [hello DPM védelmi ügynök telepítéséhez](https://technet.microsoft.com/library/hh758186.aspx?f=255&MSPPError=-2147217396) a részletes lépéseket.

## <a name="create-a-protection-group-for-hello-exchange-server"></a>Hello Exchange-kiszolgáló védelmi csoport létrehozása
1. Hello DPM felügyeleti konzolt, kattintson **védelmi**, és kattintson a **új** a hello eszköz menüszalag tooopen hello **új védelmi csoport létrehozása** varázsló.
2. A hello **üdvözlő** hello varázslóban kattintson a képernyő **következő**.
3. A hello **védelmi csoport típusának kiválasztása** képernyőn, jelölje be **kiszolgálók** kattintson **következő**.
4. Jelölje be hello Exchange server-adatbázis tooprotect szeretné, majd kattintson **következő**.

   > [!NOTE]
   > Ha az Exchange 2013 védelmét, ellenőrizze a hello [Exchange 2013 előfeltételei](https://technet.microsoft.com/library/dn751029.aspx).
   >
   >

    A következő példa hello hello Exchange 2010 adatbázis van kiválasztva.

    ![Csoporttagok kiválasztása](./media/backup-azure-backup-exchange-server/select-group-members.png)
5. Hello adatvédelmi módszer kiválasztása.

    Hello védelmi csoport neve, majd válassza ki mindkét alábbi beállítások hello:

   * Rövid távú lemezes védelmet szeretnék.
   * Online védelmet szeretnék.
6. Kattintson a **Tovább** gombra.
7. Jelölje be hello **futtassa az Eseutil toocheck adatintegritást** lehetőséget, ha azt szeretné, hogy az Exchange Server-adatbázisok hello toocheck hello integritását.

    Miután kiválasztotta ezt a beállítást, biztonsági mentés konzisztencia-ellenőrzést fog futni a hello DPM server tooavoid hello i/o-forgalmat hello futtatásával **eseutil** parancs hello Exchange-kiszolgálón.

   > [!NOTE]
   > toouse ezt a beállítást, át kell másolnia hello Ese.dll és az Eseutil.exe fájlok toohello C:\Program Files\Microsoft System Center 2012 R2\DPM\DPM\bin directory hello DPM-kiszolgálón. Ellenkező esetben a következő hiba hello akkor váltódik ki:  
   > ![az Eseutil hiba](./media/backup-azure-backup-exchange-server/eseutil-error.png)
   >
   >
8. Kattintson a **Tovább** gombra.
9. A SELECT hello adatbázis **másolásos biztonsági mentésre**, és kattintson a **következő**.

   > [!NOTE]
   > Ha nincs bejelölve a "Teljes biztonsági másolat" adatbázis másolatának legalább egy DAG, naplók nem lesznek csonkolva.
   >
   >
10. Hello célokat konfigurálása **rövid távú biztonsági mentés**, és kattintson a **következő**.
11. Tekintse át a hello rendelkezésre álló szabad lemezterület, majd **következő**.
12. Válassza ki a hello idő, mely hello DPM kiszolgáló létrehoz hello kezdeti replikálást, és kattintson a **következő**.
13. Hello konzisztencia-ellenőrzési beállítások kiválasztása, és kattintson **következő**.
14. Adja meg, hogy szeretné, hogy tooback tooAzure fel, és kattintson hello adatbázis **következő**. Példa:

    ![Online védelem adatainak megadása](./media/backup-azure-backup-exchange-server/specify-online-protection-data.png)
15. Hello ütemezésének megadása **Azure biztonsági mentés**, és kattintson a **következő**. Példa:

    ![Adja meg az online biztonsági mentés ütemezése](./media/backup-azure-backup-exchange-server/specify-online-backup-schedule.png)

    > [!NOTE]
    > Jegyezze fel az Online helyreállítási pontok alapuló Expressz teljes helyreállítási pontokat. Ezért úgy kell ütemeznie hello online helyreállítási pont hello később megadott hello az expressz teljes helyreállítási pontot.
    >
    >
16. Hello megőrzési házirend konfigurálásában az **Azure biztonsági mentés**, és kattintson a **következő**.
17. Válasszon egy online replikációs lehetőséget, és kattintson a **következő**.

    Ha nagy adatbázis, a kezdeti biztonsági mentési toobe hello hello hálózaton keresztül létrehozott hosszú ideig eltarthat. tooavoid probléma hozhat létre offline biztonsági másolat.  

    ![Online megőrzési szabály megadása](./media/backup-azure-backup-exchange-server/specify-online-retention-policy.png)
18. Erősítse meg hello beállításait, és kattintson a **csoport létrehozása**.
19. Kattintson a **Bezárás** gombra.

## <a name="recover-hello-exchange-database"></a>Hello Exchange-adatbázis helyreállítása
1. Kattintson az Exchange-adatbázis toorecover **helyreállítási** a DPM felügyeleti konzol hello.
2. Keresse meg, hogy szeretné-e toorecover hello Exchange-adatbázis.
3. Az online helyreállítási pontot válasszon hello *helyreállításkor* legördülő listából.
4. Kattintson a **helyreállítása** toostart hello **helyreállítási varázsló**.

Az online helyreállítási pontok, öt helyreállítási típusa van:

* **Helyreállítás toooriginal Exchange-kiszolgálón:** hello adatokat kell helyreállított toohello eredeti Exchange-kiszolgálón.
* **Exchange Server tooanother adatbázis helyreállítása:** hello adatokat kell helyreállított tooanother egy másik Exchange server-adatbázisba.
* **Helyreállítás helyreállítási adatbázisba tooa:** hello adatokat fogja a helyreállított tooan Exchange helyreállítási adatbázis (Rekordadatbázis).
* **Tooa hálózati mappa másolása:** hello adatokat fogja a helyreállított tooa hálózati mappába.
* **Másolja a tootape:** Ha rendelkezik egy szalagtárat, vagy egy önálló szalagos meghajtót csatlakoztatott és a beállított hello DPM-kiszolgáló, hello helyreállítási pont lesz tooa szabad szalagra másolni.

    ![Válassza ki az online replikációs](./media/backup-azure-backup-exchange-server/choose-online-replication.png)

## <a name="next-steps"></a>Következő lépések
* [Az Azure biztonsági mentési – gyakori kérdések](backup-azure-backup-faq.md)
