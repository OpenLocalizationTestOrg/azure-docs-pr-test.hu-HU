---
title: "egy Azure SQL Database adatbázist egy több-bérlős alkalmazásban aaaRestore |} Microsoft Docs"
description: "Ismerje meg, hogyan toorestore egyetlen bérlők SQL adatbázis véletlenül az adatok törlése után"
keywords: "sql database-oktatóanyag"
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: scale out apps
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: billgib;sstein
ms.openlocfilehash: 8507ecec2424c135f1859b88ebf2bb4e17538a58
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="restore-a-wingtip-saas-tenants-sql-database"></a>Bérlők Wingtip SaaS SQL-adatbázis visszaállítása

hello Wingtip SaaS-alkalmazás épül, ahol az egyes bérlők rendelkezik saját adatbázis egy adatbázis-/-bérlős modell használatával. Ez a modell hello előnyei egyike, hogy az egyszerű toorestore egy egybérlős adatai elkülönítve a többi bérlő befolyásolása nélkül.

Ebben az oktatóanyagban elsajátíthatja, két adatok helyreállítási minták:

> [!div class="checklist"]

> * Egy párhuzamos adatbázisba (párhuzamos-) adatbázis visszaállítása
> * Egy helyen adatbázis visszaállítása


|||
|:--|:--|
| **Bérlői tooa előzetes pont visszaállítása időpontra, egy párhuzamos adatbázisba** | Ebben a mintában is használható a hello bérlő tekintse át, a naplózás, a megfelelőségi, stb. hello eredeti adatbázis online és változatlan marad. |
| **Bérlői helyben visszaállítása** | Ebben a mintában általánosan használt toorecover egy bérlő tooa előzetes pontot jelöl, miután a bérlő véletlenül törli az adatokat. hello eredeti adatbázis offline állapotba, és a visszaállított adatbázis hello helyére. |
|||

Ebben az oktatóanyagban a következő előfeltételek győződjön meg arról, hogy hello elvégzése toocomplete:

* hello Wingtip SaaS-alkalmazás telepítve van. toodeploy öt percen belül, lásd: [központi telepítése és felfedezése hello Wingtip SaaS-alkalmazáshoz](sql-database-saas-tutorial.md)
* Az Azure PowerShell telepítve van. A részletekért lásd: [Ismerkedés az Azure PowerShell-lel](https://docs.microsoft.com/powershell/azure/get-started-azureps)

## <a name="introduction-toohello-saas-tenant-restore-pattern"></a>Bevezetés toohello SaaS bérlői visszaállítási minta

Hello bérlő mintát visszaállításához nincsenek egy adott bérlő adatok visszaállításához két egyszerű mintát. Bérlői adatbázisok el különítve egymástól, mert egy bérlő visszaállítása nem hatást gyakorol semmilyen más bérlővel adatokat.

Hello első mintában új adatbázisba visszaállítani adatait. hello bérlői majd kap toothis adatbázist a termelési adatokkal együtt. Ebben a mintában lehetővé teszi, hogy a egy Bérlői rendszergazda tooreview hello vissza adatokat, és esetleg azt tooselectively felülírja a jelenlegi adatértékekkel. Már fel toohello SaaS app Tervező toodetermine milyen kifinomult hello adat-helyreállítási beállítások kell lennie. Egyszerűen alatt álló adatokat képes tooreview hello állapotba, amelyben egy időben minden egyes esetekben szükséges lehet. Ha hello adatbázis használ [georeplikáció](sql-database-geo-replication-overview.md), javasoljuk, hogy hello szükséges az adatok másolása vissza hello másolási hello eredeti adatbázisba. Ha lecseréli a hello eredeti adatbázis visszaállítása hello adatbázissal, meg kell tooreconfigure, majd szinkronizálja újra a georeplikáció.

Hello második mintát, amely azt feltételezi, hogy hello bérlőre szenvedett elvesztése vagy sérülése, adatok, a hello bérlői éles adatbázis visszaállítása korábbi pont tooa időben. Hely mintában hello visszaállítás egy korábbi hello bérlői offline állapotba egy rövid ideig hello adatbázis visszaállítása, és automatikusan visszaáll online állapotba. hello eredeti adatbázis törlődik, de továbbra is visszaállíthatók a Ha toogo hátsó tooan kell időben is korábbi állapotba. Ez a minta egy változata névre történő átnevezése volt hello adatbázis helyett a törlés, bár átnevezési hello adatbázis nincs további adatbiztonsági előnyösebb kínál.

## <a name="get-hello-wingtip-application-scripts"></a>Hello Wingtip alkalmazás parancsfájlok beolvasása

hello Wingtip Szolgáltatottszoftver-parancsfájlok és az alkalmazás forráskódjához találhatók hello [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS) github-tárház. [Lépések toodownload hello Wingtip SaaS parancsfájlok](sql-database-wtp-overview.md#download-and-unblock-the-wingtip-saas-scripts).

## <a name="simulate-a-tenant-accidentally-deleting-data"></a>A bérlő véletlenül törli az adatok szimulálása

toodemonstrate helyreállítási forgatókönyvekben kell túl*véletlenül* hello bérlői adatbázisok közül az egyik adatokat törli. Rekordot törölheti, amíg a hivatkozási integritás megsértésének hello következő lépésben állítja be hello bemutató toonot beolvasása blokkolja! Bővíti Ezenkívül egyes jegy beszerzési adatokat is használhatja a hello *Wingtip SaaS Analytics oktatóanyagok*.

Hello jegy generátor parancsfájl futtatása, és hozzon létre további adatokat. hello jegy generátor szándékosan nem vásárolja meg a jegyektől az egyes bérlők utolsó eseményekhez.

1. Nyissa meg... \\Tanulási modulok\\segédprogramok\\*bemutató-TicketGenerator.ps1* a hello *PowerShell ISE*
1. Nyomja le az **F5** toorun parancsfájl hello és ügyfelek készítése és értékesítési adatokat jegyet.


### <a name="open-hello-events-app-tooreview-hello-current-events"></a>Nyissa meg a hello események app tooreview hello aktuális események

1. Nyissa meg hello *események Hub* (http://events.wtp.&lt; felhasználói&gt;. trafficmanager.net), majd **Contoso energiaoptimalizálást egyszerre Hall**:

   ![eseményközpont](media/sql-database-saas-tutorial-restore-single-tenant/events-hub.png)

1. Görgessen események hello listáját, és jegyezze fel az utolsó esemény hello hello listában:

   ![utolsó esemény](media/sql-database-saas-tutorial-restore-single-tenant/last-event.png)


### <a name="run-hello-demo-scenario-tooaccidentally-delete-hello-last-event"></a>Futtassa a hello bemutató forgatókönyvet tooaccidentally delete hello utolsó esemény

1. Nyissa meg... \\Tanulási modulok\\az üzletmenet folytonossága és vészhelyreállítás\\RestoreTenant\\*bemutató-RestoreTenant.ps1* a hello *PowerShell ISE*, és a set hello a következő értéket:
   * **$DemoScenario** = **1**, túl beállíthatja**1** -jegy értékesítés nélküli események törlése.
1. Nyomja le az **F5** toorun parancsfájl hello és hello utolsó esemény törlése. Egy megerősítő üzenet hasonló toohello következő kell megjelennie:

   ```Console
   Deleting unsold events from Contoso Concert Hall ...
   Deleted event 'Seriously Strauss' from Contoso Concert Hall venue.
   ```

1. Megnyílik a hello Contoso események lapot. Görgessen lefelé, és győződjön meg arról hello esemény már nem. Ha hello esemény továbbra is a hello listában, kattintson a frissítés, és győződjön meg arról eltűnt.

   ![utolsó esemény](media/sql-database-saas-tutorial-restore-single-tenant/last-event-deleted.png)


## <a name="restore-a-tenant-database-in-parallel-with-hello-production-database"></a>Hello éles adatbázis párhuzamosan bérlői adatbázis visszaállítása

Ebben a gyakorlatban hello Contoso energiaoptimalizálást egyszerre Hall adatbázis tooa pont visszaállítása előtti hello esemény törölve lett. Hello esemény hello előző lépésekben törlését követően szeretné, hogy toorecover, és törölni hello adatok megtekintéséhez. Nem kell toorestore az éles adatbázis törlése hello rekorddal, de más üzleti okokból kell toorecover hello régi adatbázis tooaccess hello régi adatokat.

 Hello *visszaállítási-TenantInParallel.ps1* parancsfájl adatbázis és a párhuzamos katalógusbejegyzés mindkét nevű hoz létre egy párhuzamos bérlői *ContosoConcertHall\_régi*. Ebben a mintában a visszaállítás olyan kisebb adatvesztés helyreállítás vagy megfelelőségi és naplózási helyreállítási forgatókönyveit. Egyúttal hello használata esetén az ajánlott megközelítést alkalmazva [georeplikáció](sql-database-geo-replication-overview.md).

1. Teljes hello [véletlenül törli az adatokat a felhasználó szimulálása](#simulate-a-tenant-accidentally-deleting-data) szakasz.
1. Nyissa meg... \\Tanulási modulok\\az üzletmenet folytonossága és vészhelyreállítás\\RestoreTenant\\_bemutató-RestoreTenant.ps1_ a hello *PowerShell ISE*.
1. Állítsa be **$DemoScenario** = **2**, állítsa ezt túl**2** túl*párhuzamos visszaállítási bérlői*.
1. Nyomja le az **F5** toorun hello parancsfájl.

hello parancsfájl hello bérlői adatbázis (tooa párhuzamos adatbázis) tooa pont visszaállítja az időben hello előző szakaszban hello esemény törlése előtt. Ez létrehoz egy második adatbázist, eltávolítja a meglévő katalógus metaadatokat, hogy létezik az adatbázisban, és hozzáadja hello adatbázis toohello katalógust a hello *ContosoConcertHall\_régi* bejegyzés.

hello bemutató-parancsfájl hello események lapot a böngészőben nyílik meg. Megjegyzés: a hello URL-CÍMRŐL: ```http://events.wtp.&lt;user&gt;.trafficmanager.net/contosoconcerthall_old``` , hogy ez látható hello visszaállított adatbázis ahol *_old* toohello neve kerül.

Görgessen hello felsorolt események hello böngésző tooconfirm, amely az előző szakaszban hello törölt esemény hello vissza lett állítva.

Vegye figyelembe a exposing vissza hello bérlőre, egy további bérlői, a saját események app toobrowse jegyek valószínű toobe hogyan kellene megadnia egy bérlő access toorestored adatokat, de működik tooeasily bemutatják hello visszaállítási mintát.

A valóságban ugyanúgy valószínűleg csak tartsa meg a visszaállított adatbázis számára meghatározott ideig. Vissza hello bérlői bejegyzés törlése után a hívó hello befejezte *Remove-RestoredTenant.ps1* parancsfájl.

1. Állítsa be **$DemoScenario** túl**4** tooselect hello *vissza eltávolítása bérlői* forgatókönyv.
1. **Végrehajtás** **használatával** **F5**
1. Hello *ContosoConcertHall\_régi* bejegyzés törlődik a hello katalógusból. Lépjen tovább, és zárja be a hello események lapot ennél a bérlőnél a böngészőben.


## <a name="restore-a-tenant-in-place-replacing-hello-existing-tenant-database"></a>A bérlő helyen, cseréje hello meglévő bérlő adatbázis visszaállítása

Ebben a gyakorlatban hello Contoso energiaoptimalizálást egyszerre Hall bérlői tooa pont visszaállítása előtti hello esemény törölve lett. Hello *visszaállítási-TenantInPlace* parancsfájl adatbázisát állítja vissza a hello aktuális bérlőhöz adatbázis tooa új időben tooa korábbi időpontra mutat, és törli az eredeti adatbázis hello. Ebben a mintában a visszaállítás olyan a bérlő hello jelentős adatvesztés lehet súlyos adatsérülés helyreállva tooaccommodate lenne.

1. Nyissa meg **bemutató-RestoreTenant.ps1** a PowerShell ISE fájl
1. Állítsa be **$DemoScenario** túl**5** tooselect hello *állítsa vissza a bérlő helye forgatókönyvben*.
1. Végrehajtás használatával **F5**.

hello parancsfájl visszaállítja hello bérlői adatbázis tooa pont öt perc elteltével hello esemény törölve lett. Ennek érdekében első bérlői offline így további hello Contoso energiaoptimalizálást egyszerre Hall véve frissíti toohello adatokat. Egy párhuzamos adatbázis visszaállítása hello visszaállítási pont által létrehozott és nevű, majd az időbélyegzőnek tooensure hello adatbázis neve nem ütköznek a hello meglévő bérlő adatbázis nevét. A következő hello régi bérlői adatbázis törlődik, és a visszaállított adatbázis hello átnevezett toohello eredeti adatbázis neve. Végezetül a Contoso energiaoptimalizálást egyszerre Hall kerüléséig online tooallow vissza toohello hello app adatbázist.

Sikeresen visszaállította hello adatbázis tooa pont előtti hello esemény törölve lett. hello események lap nyílik, ezért ellenőrizze hello utolsó esemény vissza lett állítva.


## <a name="next-steps"></a>Következő lépések

Ez az oktatóanyag bemutatta, hogyan végezheti el az alábbi műveleteket:

> [!div class="checklist"]

> * Egy párhuzamos adatbázisba (párhuzamos-) adatbázis visszaállítása
> * Egy helyen adatbázis visszaállítása

[Bérlői adatbázisséma kezelése](sql-database-saas-tutorial-schema-management.md)

## <a name="additional-resources"></a>További források

* További [oktatóprogramot kínál, amelyek hello Wingtip SaaS-alkalmazás épül](sql-database-wtp-overview.md#sql-database-wingtip-saas-tutorials)
* [Az Azure SQL Database üzletmenet áttekintése](sql-database-business-continuity.md)
* [További tudnivalók az SQL-adatbázis biztonsági mentése](sql-database-automated-backups.md)
