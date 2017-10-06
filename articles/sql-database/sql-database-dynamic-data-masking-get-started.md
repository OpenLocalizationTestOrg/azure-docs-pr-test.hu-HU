---
title: "SQL-adatbázis dinamikus adatmaszkolási aaaAzure |} Microsoft docs"
description: "SQL-adatbázis dinamikus adatmaszkolási korlátozza a bizalmas adatok veszélyeztetettségének maszkolás azt toonon jogosultságú felhasználók által"
services: sql-database
documentationcenter: 
author: ronitr
manager: jhubbard
editor: 
ms.assetid: 4b36d78e-7749-4f26-9774-eed1120a9182
ms.service: sql-database
ms.custom: security
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 03/09/2017
ms.author: ronitr; ronmat
ms.openlocfilehash: 68b55128dc096f7e3dd0e5ed1427b39da5d64736
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="sql-database-dynamic-data-masking"></a>SQL-adatbázis dinamikus adatmaszkolási

SQL-adatbázis dinamikus adatmaszkolási maszkolás azt toonon jogosultságú felhasználók által bizalmas adatok veszélyeztetettségének korlátozása. 

Dinamikus adatmaszkolási megakadályozza, hogy jogosulatlan hozzáférés toosensitive adatokat azáltal, hogy az ügyfelek toodesignate mennyi hello bizalmas adatok tooreveal hello alkalmazás réteg gyakorolt minimális hatás mellett. Egy csoportházirend-alapú biztonsági funkció, hello bizalmas adatok hello eredménykészletben lekérdezés elrejti a kijelölt adatbázis mezők keresztül közben hello adatbázis hello adatai nem változik.

Például a képviselőjével egy ügyfélszolgálatával, előfordulhat, hogy azonosíthatja a hívók a hitelkártya több számjegye, de ilyen elemek nem kell teljesen adatok kitett toohello munkatársának. Egy maszkolási szabályra meghatározása, hogy minden maszkok, de hello utolsó négy számjegye bármely hitelkártya hello eredménykészletben bármely lekérdezés. Másik példaként a megfelelő adatok maszk lehet meghatározott tooprotect személyes azonosításra alkalmas adatokat (PII) adatok, úgy, hogy a fejlesztő lekérdezheti a termelési környezetben hibaelhárítási célból megfelelőségi szabályzat megsértése nélkül.

## <a name="sql-database-dynamic-data-masking-basics"></a>SQL-adatbázis dinamikus adatmaszkolási alapjai
Beállította a dinamikus adatmaszkolási hello-szabályzat Azure-portálon dinamikus adatmaszkolási hello műveletet az SQL-adatbázis konfigurációs panel vagy a beállítások panel kiválasztásával.

### <a name="dynamic-data-masking-permissions"></a>Dinamikus adatok maszkolása engedélyek
Dinamikus adatmaszkolási hello Azure adatbázis admin, kiszolgálói rendszergazda vagy biztonsági tisztviselő szerepköröket konfigurálhatja.

### <a name="dynamic-data-masking-policy"></a>Dinamikus adatok maszkolása házirend
* **Az SQL-felhasználók ki zárva a maszkolásból** – SQL-felhasználók csoportja A vagy AAD identitásokat tartalmaz, amelyek látható adatok beolvasása a hello SQL-lekérdezés eredménye. Rendszergazdai jogosultságokkal rendelkező felhasználók vannak mindig ki vannak zárva a maszkolásból, és hello eredeti adatokat bármely maszk nélkül.
* **Szabályok maszkolás** -maszkolandó mezők toobe kijelölt hello és maszkolás használt függvény hello meghatározó szabályok készlete. a kijelölt mezők hello meghatározása egy séma adatbázisnév, a tábla neve és az oszlop neve.
* **Maszkolás funkciók** -hello elérhetővé tegyék az adatok különböző helyzetek kezelésére szabályozó módszerek készlete.

| Maszkolási függvényt | Maszkolási logika |
| --- | --- |
| **Alapértelmezett** |**Teljes maszkolás függően toohello adatok hello típusú kijelölt mezők**<br/><br/>• Az XXXX vagy kevesebb Xs Ha hello hello mező mérete legalább 4 karakterből kell állnia a karakterláncos adattípusokkal (nchar, ntext, nvarchar).<br/>• A numerikus adattípusú (bigint, bit, decimal, int, pénzt, numerikus, smallint, kis pénz típusú értékké, tinyint, lebegőpontos, valós) nulla értéket használja.<br/>• Az 01-01-1900 dátum és idő adattípus (dátum, datetime2, datetime, datetimeoffset, smalldatetime, idő).<br/>• Az SQL variant, hello alapértelmezett érték hello aktuális típusú szolgál.<br/>• A XML-hello dokumentum <masked/> szolgál.<br/>• Különleges adattípust alkotnak üres értéket használ (időbélyeg táblázatra, hierarchyid, GUID, binary, image, varbinary térbeli típusok). |
| **Hitelkártya** |**Maszkolás metódus, amely felfedi a hello utolsó négy számjegyét kijelölt mezők hello** , és hozzáadja egy állandó karakterlánc hello képernyőn hitelkártya előtagjaként.<br/><br/>XXXX-XXXX-XXXX-1234 |
| **E-mailek** |**Maszkolás metódus, amely hello első betűjének közzététele és hello tartomány lecseréli XXX.com** a állandó karakterlánc előtag hello képernyőn egy e-mail cím használatával.<br/><br/>aXX@XXXX.com |
| **Véletlenszerű számot** |**Módszer, amely véletlenszerűen generálja maszkolás** toohello szerint kiválasztott határokat és a tényleges adattípusokat. Ha a határok kijelölt hello egyenlő, maszkolás függvény hello számának állandó.<br/><br/>![Navigációs ablaktábla](./media/sql-database-dynamic-data-masking-get-started/1_DDM_Random_number.png) |
| **Egyéni szöveg** |**Módszer, mely tesz elérhetővé hello első és utolsó karaktere maszkolás** , és hozzáadja az egyéni kitöltési karakterlánc hello közepén. Ha hello eredeti karakterlánc rövidebb, mint a közzétett hello előtag és utótagot, csak a karakterlánc kitöltési hello szolgál. <br/>[kitöltési] előtag utótag<br/><br/>![Navigációs ablaktábla](./media/sql-database-dynamic-data-masking-get-started/2_DDM_Custom_text.png) |

<a name="Anchor1"></a>

### <a name="recommended-fields-toomask"></a>Ajánlott mezők toomask
hello DDM javaslatok motor jelzők egyes mezőket az adatbázisból potenciálisan bizalmas mezői, esetleg maszkolási használható jól. Hello dinamikus Adatmaszkolás paneljét a hello portál, az oszlopok az Ön adatbázisához ajánlott hello jelenik meg. Toodo szüksége kattintson **maszk hozzáadása** egy vagy több oszlopnál, majd **mentése** tooapply ezeket a mezőket maszkot.

## <a name="set-up-dynamic-data-masking-for-your-database-using-powershell-cmdlets"></a>A Powershell-parancsmagok használatával adatbázis dinamikus adatmaszkolási beállítása
Lásd: [Azure SQL adatbázis parancsmagok](https://msdn.microsoft.com/library/azure/mt574084.aspx).

## <a name="set-up-dynamic-data-masking-for-your-database-using-rest-api"></a>A REST API használatával adatbázis dinamikus adatmaszkolási beállítása
Lásd: [műveletek az Azure SQL-adatbázisok](https://msdn.microsoft.com/library/dn505719.aspx).

