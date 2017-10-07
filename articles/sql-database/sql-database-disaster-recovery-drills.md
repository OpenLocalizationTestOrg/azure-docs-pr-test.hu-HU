---
title: "Adatbázis Vészhelyreállítások részletezésének aaaSQL |} Microsoft Docs"
description: "További útmutatás és ajánlott eljárások az Azure SQL Database tooperform katasztrófa utáni helyreállítás csukja toohelp megőrzése a a kritikus kritikus fontosságú üzleti alkalmazások rugalmas toofailures és a kimaradások esetén."
services: sql-database
documentationcenter: 
author: anosov1960
manager: jhubbard
editor: monicar
ms.assetid: b44a269c-fe2a-404f-b013-290030860bd1
ms.service: sql-database
ms.custom: business continuity
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-management
ms.date: 07/31/2016
ms.author: sashan
ms.openlocfilehash: bf17857a19fdebddf0d4f55e4db3a1b33efb4e8e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="performing-disaster-recovery-drill"></a>Vész-helyreállítási részletezési végrehajtása
Javasoljuk, hogy az alkalmazás felkészültségét helyreállítási munkafolyamat érvényesség rendszeres időközönként. Ellenőrzése hello alkalmazás viselkedését, és adatok elvesztését és/vagy hello megszakítása következményei magában foglalja a, hogy a feladatátvétel mérnöki célszerű. A legtöbb iparági normák szerint üzleti folytonossági hitelesítő részeként követelmény egyben.

A vész-helyreállítási részletezési végrehajtása áll:

* Amelyek adatok réteg leállás
* Helyreállítása
* Alkalmazás integritási utáni helyreállítás ellenőrzése

Attól függően, hogy hogyan meg [az alkalmazás az üzletmenet folytonossága érdekében tervezett](sql-database-business-continuity.md), hello munkafolyamat tooexecute hello részletezési eltérőek lehetnek. Az alábbiakban egy vész-helyreállítási részletezéshez hello környezetében az Azure SQL Database végző hello ajánlott eljárások azt ismertetik.

## <a name="geo-restore"></a>Georedundáns helyreállítás
tooprevent hello lehetséges adatvesztést a vész-helyreállítási részletezési során, ajánlott hello részletezési hello éles környezetben másolatának elkészítése és használja azt a tesztkörnyezet használatával végez tooverify hello alkalmazás feladatátvételi munkafolyamat.

#### <a name="outage-simulation"></a>Kimaradás szimulálása
toosimulate hello kimaradás, törölheti vagy hello source adatbázis átnevezése. Ennek hatására a kapcsolat alkalmazáshibák.

#### <a name="recovery"></a>Helyreállítás
* Hajtsa végre a hello georedundáns helyreállítás hello adatbázis egy másik kiszolgálóra történő leírtak [Itt](sql-database-disaster-recovery.md).
* Változás hello alkalmazás konfigurációs tooconnect toohello helyreállított adatbázis és követi hello [adatbázis konfigurálnia a helyreállítás után](sql-database-disaster-recovery.md) toocomplete hello helyreállítási ismerteti.

#### <a name="validation"></a>Ellenőrzés
* Teljes hello részletezési hello alkalmazás integritási utáni helyreállítás (beleértve a kapcsolati karakterláncok, bejelentkezéseket, alapvető funkciókat tesztelési, illetve más megszokott alkalmazástelepítési signoffs eljárások érvényesítést része) ellenőrzésével.

## <a name="geo-replication"></a>Georeplikáció
Georeplikálási hello részletezési használatával védett adatbázis a gyakorlatban magában foglalja a tervezett feladatátvétel toohello másodlagos adatbázis. hello tervezett feladatátvétel biztosítja, hogy hello elsődleges és másodlagos adatbázisok hello maradjanak szinkronban hello szerepkörök bekapcsolásakor. Ellentétben a nem tervezett feladatátvétel hello, ez a művelet nem eredményez adatvesztés, így hello részletezési végrehajtható hello éles környezetben.

#### <a name="outage-simulation"></a>Kimaradás szimulálása
toosimulate hello kimaradás, letilthatja hello webalkalmazáshoz vagy a virtuális gép csatlakoztatott toohello adatbázis. Az eredmény hello kapcsolathibái hello-ügyfelek számára.

#### <a name="recovery"></a>Helyreállítás
* Győződjön meg arról, hogy hello Alkalmazáskonfiguráció hello vész-Helyreállítási régió pontok toohello volt másodlagos hello válik a teljes elérhető új elsődleges.
* Hajtsa végre [tervezett feladatátvétel](scripts/sql-database-setup-geodr-and-failover-database-powershell.md) toomake hello másodlagos adatbázis-egy új elsődleges
* Hajtsa végre a hello [adatbázis konfigurálnia a helyreállítás után](sql-database-disaster-recovery.md) toocomplete hello helyreállítási ismerteti.

#### <a name="validation"></a>Ellenőrzés
* Teljes hello részletezési hello alkalmazás integritási utáni helyreállítás (beleértve a kapcsolati karakterláncok, bejelentkezéseket, alapvető funkciókat tesztelési, illetve más megszokott alkalmazástelepítési signoffs eljárások érvényesítést része) ellenőrzésével.

## <a name="next-steps"></a>Következő lépések
* üzleti folytonosság-forgatókönyvekkel kapcsolatos toolearn lásd [folytonosságának forgatókönyvek](sql-database-business-continuity.md)
* tudnivalók Azure SQL adatbázis automatikus biztonsági mentés, toolearn lásd: [SQL-adatbázis automatikus biztonsági mentés](sql-database-automated-backups.md)
* toolearn a helyreállításhoz, az automatikus biztonsági mentés használatával kapcsolatban lásd: [adatbázis visszaállítása biztonsági másolatból hello szolgáltatás által kezdeményezett](sql-database-recovery-using-backups.md)
* toolearn gyorsabb helyreállítási beállításokkal kapcsolatban lásd: [aktív georeplikáció](sql-database-geo-replication-overview.md)  
