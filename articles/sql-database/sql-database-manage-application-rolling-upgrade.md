---
title: "aaaRolling alkalmazásfrissítések - Azure SQL Database |} Microsoft Docs"
description: "Megtudhatja, hogyan toouse Azure SQL Database georeplikációja toosupport online frissítései a felhőalapú alkalmazásnál."
services: sql-database
documentationcenter: 
author: anosov1960
manager: jhubbard
editor: monicar
ms.assetid: 58f42859-1e37-463c-a3d8-a3ca2e867148
ms.service: sql-database
ms.custom: business continuity
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/16/2016
ms.author: sashan
ms.openlocfilehash: 18c56300916d129bff141624cc5c416b500408d4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="managing-rolling-upgrades-of-cloud-applications-using-sql-database-active-geo-replication"></a>Az SQL-adatbázis aktív georeplikáció használatával a felhőalapú alkalmazásokhoz működés közbeni frissítések kezelése
> [!NOTE]
> [Aktív georeplikáció](sql-database-geo-replication-overview.md) érhető el az összes olyan adatbázis az összes rétege.
> 

Megtudhatja, hogyan toouse [georeplikáció](sql-database-geo-replication-overview.md) az SQL-adatbázis tooenable működés közbeni frissítés a felhő alkalmazás. Mivel a frissítés egy zavaró művelet, azt az üzleti folytonossági tervezési és kialakítási részének kell lennie. Ebben a cikkben úgy tekintünk kétféleképpen koordinálása hello frissítési folyamat, és hello előnyei és az egyes lehetőségek kompromisszumot ismertetik. Ez a cikk hello alkalmazásában egy egyszerű alkalmazást alkotó webhely csatlakoztatott tooa egy adatbázist, az adatok rétegként használjuk. Célunk tooupgrade 1 hello alkalmazás tooversion 2 bármely jelentős befolyásolása nélkül futó verziójával hello végfelhasználói élmény. 

Frissítési beállítások hello kiértékelésekor hello a következő tényezőket kell figyelembe vennie:

* Érintő forgalomkiesés alkalmazás frissítéskor. Mennyi ideig hello alkalmazás függvény korlátozható vagy csökkentett teljesítményű.
* Egy frissítési hiba esetén vissza tooroll képességét.
* A biztonsági rés hello alkalmazás hello frissítés során nem végzetes hiba esetén.
* Amerikai dollár teljes költsége.  Ez magában foglalja a további redundancia és többletköltségei hello hello frissítési folyamat által használt ideiglenes összetevőket. 

## <a name="upgrading-applications-that-rely-on-database-backups-for-disaster-recovery"></a>A vész-helyreállítási mentéseket használó alkalmazások frissítése
Az alkalmazás automatikus mentését alapul, és vész-helyreállítási georedundáns helyreállítás használ, célszerű általában telepített tooa egyetlen Azure-régiót. Ebben az esetben hello frissítési magában foglalja a biztonsági mentési központi telepítés összes alkalmazás-összetevő hello frissítés részt. toominimize hello végfelhasználói megszűnésének hello feladatátvételi profillal fogja használni a Azure Traffic Manager (WATM).  a következő diagram hello hello működési környezetbe előzetes toohello frissítési folyamatát mutatja be. végpont hello <i>contoso-1.azurewebsites.net</i> hello alkalmazás, amelyet a frissített toobe éles helyét jelöli. tooenable hello képességét tooroll vissza hello frissítését, szakasz tárhely hello alkalmazás teljesen szinkronizált másolatát kell létrehozása. hello következő lépések végrehajtására szükség tooprepare hello alkalmazás hello frissítésre:

1. Hello frissítési fázis tárhely létrehozása. Hozzon létre egy másodlagos adatbázist (1), és az azonos webhelyen hello telepítését toodo azonos Azure-régiót. Ha hello összehangolása folyamat befejeződött, figyelje a hello másodlagos toosee.
2. Hozzon létre egy feladatátvevő profilt az WATM a <i>contoso-1.azurewebsites.net</i> online végpontként és <i>contoso-2.azurewebsites.net</i> offline állapotúként. 

> [!NOTE]
> Megjegyzés: hello előkészítő lépések nem befolyásolja az éles tárolóhelyre hello hello alkalmazás, és teljes hozzáférési módban működéséhez.
>  

![SQL adatbázis Ugrás-replikációs konfiguráció. Felhőalapú vészhelyreállítás.](media/sql-database-manage-application-rolling-upgrade/Option1-1.png)

Ha megadta hello előkészítő lépések hello alkalmazás készen áll a hello tényleges frissítés. a következő diagram hello hello lépéseit hello frissítési folyamatát mutatja be. 

1. Állítsa be az elsődleges adatbázis hello hello éles tárhely tooread csak módban (3). Ez garantálja, hogy hello hello alkalmazás (V1) éles példánya marad írásvédett megelőzve hello adateltérésekkel hello V1 és V2 adatbázis-példány között hello frissítés során.  
2. Válassza le a másodlagos adatbázis hello hello tervezett lezárást módban (4). Hello elsődleges adatbázis teljesen szinkronizált független másolatot hoz létre. Ez az adatbázis lesz frissítve.
3. Hello elsődleges adatbázis tooread-írási üzemmód és parancsprogrammal hello frissítési hello szakasz tárolóhelye (5).     

![SQL-adatbázis georeplikáció konfigurációs. Felhőalapú vészhelyreállítás.](media/sql-database-manage-application-rolling-upgrade/Option1-2.png)

Ha hello frissítése sikeresen befejeződött, most már áll készen tooswitch hello végfelhasználók előkészített toohello másolási hello alkalmazás. Most válik hello alkalmazás hello éles tárolóhelyre.  Ebbe beletartozik a következő diagram hello módon néhány további lépéseket.

1. Túl kapcsoló hello online végpont hello WATM profil<i>contoso-2.azurewebsites.net</i>, milyen pontok toohello V2 verziójú hello webhely (6). Változik a hello V2 alkalmazás hello éles tárolóhelyre és hello végfelhasználói forgalma irányított tooit.  
2. Ha már nincs szüksége hello V1-es alkalmazás-összetevők, nyugodtan távolítsa el őket (7).   

![SQL-adatbázis georeplikáció konfigurációs. Felhőalapú vészhelyreállítás.](media/sql-database-manage-application-rolling-upgrade/Option1-3.png)

Ha hello frissítési folyamat sikertelen, például hello frissítési parancsfájl, a tooan hiba miatt hello szakasz tárolóhely tekintendő sérült. tooroll biztonsági hello toohello frissítés előtti alkalmazásállapot hello alkalmazás hello éles tárhely toofull Access egyszerűen állítsa vissza. hello lépéseit hello következő diagramon láthatók.    

1. Állítsa be a hello adatbázis másolása tooread-írási módban (8). A művelettel visszaállítja hello teljes V1 funkcionálisan a hello éles tárolóhelyre.
2. Hajtsa végre a hello kiváltó okának elemzése, és távolítsa el a sérült hello összetevők hello szakasz tárolóhelye (9). 

Ezen a ponton hello alkalmazás teljesen működőképes és hello frissítési lépések megismételhető.

> [!NOTE]
> hello visszaállítása nem szükséges WATM profil változásait, már mutat túl<i>contoso-1.azurewebsites.net</i> , hello aktív végpontot.
> 
> 

![SQL-adatbázis georeplikáció konfigurációs. Felhőalapú vészhelyreállítás.](media/sql-database-manage-application-rolling-upgrade/Option1-4.png)

hello kulcs **előny** ezt a beállítást az, hogy egy alkalmazás egyszerű ismertetett lépések segítségével egy régió frissítheti. hello dollár költség hello frissítés viszonylag alacsony. fő hello **kompromisszumot** , amely hello frissítési hello helyreállítási toohello frissítés előtti állapotot során végzetes hiba esetén tartalmazzon hello alkalmazás egy másik régióban található és a visszaállítási hello adatbázis újbóli telepítése biztonsági mentés georedundáns helyreállítás. Ez a folyamat jelentős állásidőt eredményez.   

## <a name="upgrading-applications-that-rely-on-database-geo-replication-for-disaster-recovery"></a>A database georeplikációja a vész-helyreállítási használó alkalmazások frissítése
Ha az alkalmazás kihasználja az üzletmenet folytonossága érdekében georeplikáció, telepített tooat legalább két különböző régiókban az elsődleges régióban egy aktív központi telepítés és a készenléti biztonsági mentés a régióban. Ezenkívül toohello tényezők a korábban említett hello frissítési folyamat garantálnia kell, hogy:

* hello alkalmazás védve marad az végzetes hibák mindig hello frissítési folyamat során
* hello georedundáns összetevő hello alkalmazás párhuzamosan hello aktív összetevők frissítésének

Ezen célok fogja használni, Azure Traffic Manager (WATM) használatával hello feladatátvételi profil egy aktív és három biztonsági mentés végpontok tooachieve.  a következő diagram hello hello működési környezetbe előzetes toohello frissítési folyamatát mutatja be. webhelyek hello <i>contoso-1.azurewebsites.net</i> és <i>contoso-dr.azurewebsites.net</i> egy éles tárolóhelyre hello alkalmazás teljes földrajzi redundanciával képviseli. tooenable hello képességét tooroll vissza hello frissítését, szakasz tárhely hello alkalmazás teljesen szinkronizált másolatát kell létrehozása. Tooensure, amely hello alkalmazás gyorsan helyreállíthatja, ha egy végzetes hiba bekövetkezése hello frissítési folyamat során van szükség, mert hello szakasz tárolóhely kell toobe georedundáns is. hello következő lépések végrehajtására szükség tooprepare hello alkalmazás hello frissítésre:

1. Hello frissítési fázis tárhely létrehozása. Hozzon létre egy másodlagos adatbázist (1), és a webhely hello hello azonos példányának telepítését toodo azonos Azure-régiót. Ha hello összehangolása folyamat befejeződött, figyelje a hello másodlagos toosee.
2. Hozzon létre egy georedundáns másodlagos adatbázist hello szakasz tárolóhelye földrajzi replikálása hello másodlagos adatbázis toohello biztonsági mentési terület (Ez a lehetőség "kapcsolt georeplikáció"). Ha folyamat összehangolása hello befejezett (3) a biztonsági mentés másodlagos toosee hello figyelése
3. Hello biztonsági mentési régióban hello webhely készenléti másolatának létrehozása hozzákapcsolja azt toohello georedundáns másodlagos (4).  
4. Hello további végpontok hozzáadása <i>contoso-2.azurewebsites.net</i> és <i>contoso-3.azurewebsites.net</i> toohello feladatátvételi profil WATM a kapcsolat nélküli végpontként (5). 

> [!NOTE]
> Megjegyzés: hello előkészítő lépések nem befolyásolja az éles tárolóhelyre hello hello alkalmazás, és teljes hozzáférési módban működéséhez.
> 
> 

![SQL-adatbázis georeplikáció konfigurációs. Felhőalapú vészhelyreállítás.](media/sql-database-manage-application-rolling-upgrade/Option2-1.png)

Hello előkészítő lépések elvégzése után hello szakasz tárolóhely hello frissítés készen áll. a következő diagram hello hello frissítés lépéseit mutatja be.

1. Hello elsődleges adatbázis hello éles tárhely csak tooread módra beállítva (6). Ez garantálja, hogy hello hello alkalmazás (V1) éles példánya marad írásvédett megelőzve hello adateltérésekkel hello V1 és V2 adatbázis-példány között hello frissítés során.  
2. Válassza le a másodlagos adatbázis hello hello a ugyanabban a régióban használatával hello tervezett lezárást mód (7). Hello elsődleges adatbázis, amely egy elsődleges automatikusan válik hello leállása után teljesen szinkronizált független másolatot hoz létre. Ez az adatbázis lesz frissítve.
3. Kapcsolja be az elsődleges adatbázis hello hello szakasz tárolóhely tooread-írási módban, és futtassa a hello frissítési parancsfájl (8).    

![SQL-adatbázis georeplikáció konfigurációs. Felhőalapú vészhelyreállítás.](media/sql-database-manage-application-rolling-upgrade/Option2-2.png)

Ha hello frissítése sikeresen befejeződött áll készen tooswitch hello végfelhasználók toohello V2 hello alkalmazás verziója. a következő diagram hello hello lépéseit mutatja be.

1. Túl kapcsoló hello aktív végpontja hello WATM profil<i>contoso-2.azurewebsites.net</i>, mely most mutat toohello V2 verziójú hello webhely (9). Most válik hello V2-alkalmazás az éles tárhely és végfelhasználói forgalma irányított tooit. 
2. Ha már nincs szüksége hello V1-es alkalmazás így biztonságosan eltávolíthatja (10-es és 11).  

![SQL-adatbázis georeplikáció konfigurációs. Felhőalapú vészhelyreállítás.](media/sql-database-manage-application-rolling-upgrade/Option2-3.png)

Ha hello frissítési folyamat sikertelen, például hello frissítési parancsfájl, a tooan hiba miatt hello szakasz tárolóhely tekintendő sérült. tooroll biztonsági hello toohello frissítés előtti alkalmazásállapot egyszerűen állítsa vissza toousing hello alkalmazás teljes hozzáféréssel rendelkező hello éles tárolóhelyre. hello lépéseit hello következő diagramon láthatók.    

1. Set hello elsődleges adatbázis-másolat hello éles tárhely tooread-írási módban (12). A művelettel visszaállítja hello teljes V1 funkcionálisan a hello éles tárolóhelyre.
2. Hajtsa végre a hello kiváltó okának elemzése, és távolítsa el a sérült hello összetevők hello szakasz tárolóhelye (13 és 14). 

Ezen a ponton hello alkalmazás teljesen működőképes és hello frissítési lépések megismételhető.

> [!NOTE]
> hello visszaállítása nem szükséges WATM profil változásait, már mutat túl <i>contoso-1.azurewebsites.net</i> , hello aktív végpontot.
> 
> 

![SQL-adatbázis georeplikáció konfigurációs. Felhőalapú vészhelyreállítás.](media/sql-database-manage-application-rolling-upgrade/Option2-4.png)

hello kulcs **előny** ezt a beállítást az, hogy frissíteni hello alkalmazás és is a georedundáns párhuzamosan hello frissítés során az üzletmenet folytonossága veszélyeztetése nélkül. fő hello **kompromisszumot** igényel minden egyes alkalmazás-összetevő dupla redundanciát, így ezért költséget az magasabb dollár áll. A bonyolultabb munkafolyamat is magában foglalja. 

## <a name="summary"></a>Összefoglalás
hello két frissítési módszerek hello cikkben ismertetett költség összetettségét és hello dollár különböznek, de mindkettő összpontosítani minimalizálja a hello időpontot, amikor a végfelhasználó hello korlátozott csak tooread műveleteket. Ideje közvetlenül hello frissítési parancsfájl hello időtartamát határozza meg. Hello adatbázis mérete nem függ, hello szolgáltatásiréteg-akkor válassza, hello webhely konfigurációja és egyéb tényezőkkel, amelyek könnyen nem tudja beállítani. Ennek az az oka az összes hello előkészítő lépések vannak különválik hello frissítési lépések és hello termelési alkalmazások befolyásolása nélkül végezheti. frissítési parancsprogram hello hello hatékonyságát hello kulcsfontosságú tényező, amely meghatározza egy hello végfelhasználói élmény frissítéskor. Ezért hello fejlesztheti azt legkönnyebben a próbálkozások összpontosító így hello frissítési parancsfájl felhasználását.  

## <a name="next-steps"></a>Következő lépések
* Egy üzleti folytonosság – áttekintés és forgatókönyvek: [üzleti folytonosság – áttekintés](sql-database-business-continuity.md).
* tudnivalók Azure SQL adatbázis automatikus biztonsági mentés, toolearn lásd: [SQL-adatbázis biztonsági mentések automatikus](sql-database-automated-backups.md).
* toolearn a helyreállításhoz, az automatikus biztonsági mentés használatával kapcsolatban lásd: [adatbázis visszaállítása biztonsági másolatból automatizált](sql-database-recovery-using-backups.md).
* toolearn gyorsabb helyreállítási beállításokkal kapcsolatban lásd: [aktív georeplikáció](sql-database-geo-replication-overview.md).


