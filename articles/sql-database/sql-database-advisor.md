---
title: "aaaPerformance ajánlásokkal – az Azure SQL Database |} Microsoft Docs"
description: "hello Azure SQL-adatbázis az SQL-adatbázisok, amelyek az aktuális lekérdezés teljesítményének vonatkozó javaslatokkal szolgál."
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: monicar
ms.assetid: 1db441ff-58f5-45da-8d38-b54dc2aa6145
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 07/05/2017
ms.author: sstein
ms.openlocfilehash: 77db338a0a395aec78c9e02849ae5ba4f2d01680
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="performance-recommendations"></a>Teljesítmény javaslatok

Az Azure SQL Database Tanulja meg, és alkalmazkodik az alkalmazással, és testreszabott ajánlásaiban így lehetővé teszi az SQL-adatbázisok toomaximize hello teljesítményét. A teljesítmény folyamatosan megfelelőségét ellenőrizni kell az SQL-adatbázis használati előzmények elemzésével. hello javaslatok, amelyek egy adatbázis egyedi munkaterhelés mintát alapulnak, és a teljesítmény növelése érdekében.

> [!NOTE]
> Javaslatok használatának ajánlott úgy, hogy az adatbázis automatikus hangolása engedélyezését. További információkért lásd: [automatikus hangolása](sql-database-automatic-tuning.md).
>

## <a name="create-index-recommendations"></a>Ajánlott index létrehozása
Azure SQL-adatbázis folyamatosan figyeli végrehajtott hello lekérdezések és javíthatja a teljesítményt hello hello indexek azonosítja. Miután abban, hogy elegendő épül, amely egy bizonyos index hiányoznak, egy új **Create index** javaslat jön létre. Az Azure SQL Database buildek abban, hogy úgy nyereség hello teljesítményindex idő keresztül kellene kapcsolja. Attól függően, hogy a becsült hello jobb teljesítménye javaslatok kategóriába sorolni magas, közepes vagy alacsony. 

Javaslatok használatával létrehozott indexek mindig auto_created indexek szerint vannak megjelölve. Láthatja, hogy mely indexek auto_created sys.indexes nézet megtekintésével. Automatikusan létrehozott indexek ALTER/átnevezése parancsok nincs blokkolás. Ha toodrop hello oszlop, amely egy automatikusan létrehozott index kapcsolattal rendelkezik, hello parancs továbbítja, és a létrehozott index hello automatikus megszakad a hello paranccsal is. Szabályos indexei hello ALTER/RENAME parancs indexelt oszlopokon le fog állni.

Hello létrehozása után ajánlott index alkalmazzák, akkor az Azure SQL Database összehasonlítja hello alapteljesítményének hello lekérdezések teljesítményét. Ha új index fejlesztései a hello teljesítményét, javaslat lesz megjelölve, sikeres, és a hatás jelentés lesz elérhető. Abban az esetben hello index nem kapcsolja a hello előnyeit, azt a rendszer automatikusan visszaállítja. Így az Azure SQL Database biztosítja, hogy javaslatokat használatával csak javítja hello adatbázis teljesítménye.

Bármely **Create index** javaslat rendelkezik egy biztonsági házirendet, amely nem engedélyezi a hello javaslat alkalmazása, ha az elmúlt 20 perc, vagy ha hello tárhely 90 %-a használati 80 % fölötti volt hello adatbázis vagy a készlet DTU-használatát ki. Ebben az esetben hello javaslat lesz elhalasztva.

## <a name="drop-index-recommendations"></a>Indexek elvetésére vonatkozó javaslatok
Továbbá toodetecting egy hiányzó index, az Azure SQL Database folyamatosan elemzi a létező indexek hello teljesítményét. Ha az index nem használható, az Azure SQL Database azt javasolja, eldobása. Az index elvetése két esetekben ajánlott:
* Index megegyezik egy másik index (azonos indexelt és oszlop, a partíciós séma és a szűrők része)
* Az index az nem használt hosszabb időre (93 nap)

Indexek elvetésére vonatkozó javaslatok is halad át hello ellenőrzés végrehajtása után. Hello teljesítmény akkor javul, ha hello hatás jelentés lesz elérhető. Abban az esetben, ha teljesítménycsökkenést észlel, a javaslat vissza lesznek vonva.


## <a name="parameterize-queries-recommendations"></a>Lekérdezések javaslatok parametrizálja
**Lekérdezések parametrizálja** javaslatok jelennek meg, ha rendelkezik ilyennel, vagy további lekérdezések, amelyek folyamatosan újrafordítani, de végül hello lekérdezés végrehajtási csomagot. Ez a feltétel egy lehetőség tooapply (egyszerű) paraméterezéssel, amely lehetővé teszi a lekérdezésterveket toobe a gyorsítótárban, és fel újra a jövőbeli, a teljesítmény fokozása és alacsonyabb erőforrás-használatot hello kényszerített megnyílik. 

Minden egyes lekérdezés kezdetben az SQL Server állították ki kell összeállítani toobe toogenerate egy végrehajtási terv. Minden létrehozott terv toohello terv gyorsítótár és a későbbi végrehajtások során kerül a hello ugyanabban a lekérdezésben a terv hello gyorsítótárból, így nem kell további fordítási hello is felhasználhatja. 

Alkalmazások, amelyek a lekérdezéseket, többek között nem paraméterezett értékek, küldenek vezethet tooa teljesítményigény, ahol minden egyes ilyen lekérdezés különböző paraméterértékekkel hello végrehajtási terv fordítását, akkor újra. Sok esetben hello értékek készítése különböző paraméterrel azonos lekérdezések hello azonos végrehajtási terveket, de ezek a csomagok továbbra is külön-külön hozzáadott toohello terv gyorsítótár. Végrehajtási tervek újrafordítás adatbázis erőforrásainak használatához, növelje a hello időtartama dátum- és túlcsordulás hello lekérdezésterv gyorsítótár, amely a tervek toobe hello gyorsítótár a fürtből. Ez a viselkedés az SQL Server is módosítható úgy, hogy hello adatbázison (egyszerű) paraméterezéssel beállítást kényszerített hello. 

Ez a javaslat hello hatását becslései toohelp, áll megadott a hello tényleges CPU összehasonlítása használati és hello tervezett CPU-használat (mintha hello javaslat alkalmazott). Ezenkívül tooCPU kevésbé kell kihasználni, a lekérdezés időtartama csökkenti a hello idő a fordítás. Is lesz sokkal kevesebb terhet tervgyorsítótár, így hello többsége a gyorsítótárban toostay tervek és használható fel újra. Is alkalmazhat, ez a javaslat gyorsan és egyszerűen kattintson a hello **alkalmaz** parancsot. 

Ha ez a javaslat alkalmaz, lehetővé teszi az kényszerített (egyszerű) paraméterezéssel az adatbázis percen belül, és figyelési folyamattal, amely körülbelül 24 óráig tart hello kezdődik. Ezt követően történik a képes toosee hello ellenőrzési jelentés bemutatja az adatbázis CPU-használat előtt 24 óra lehet, és hello a javaslat alkalmazását követően. SQL Database Advisor segédprogramot, amely automatikusan helyreállítja az alkalmazott hello javaslat, abban az esetben, ha a teljesítmény regressziós észlelt biztonsági mechanizmussal rendelkezik.

## <a name="fix-schema-issues-recommendations"></a>Javítsa ki a séma problémák javaslatok
**Séma kapcsolatos problémák megoldása** javaslatok jelennek meg, ha hello SQL adatbázis-szolgáltatás egy anomáliadetektálási hello számú séma kapcsolatos SQL hiba történt az Azure SQL Database az értesítések. Ez a javaslat jellemzően akkor jelenik meg, amikor a adatbázisnál több séma kapcsolatos hibákat (Érvénytelen oszlopnév, érvénytelen objektumnév stb.) egy órán belül.

"Séma problémák" szintaktikai hibák az SQL Server fordulhat elő, ha nincsenek egyeztetve hello SQL-lekérdezés hello meghatározását és hello adatbázisséma hello meghatározását osztály. Például egy hello lekérdezés által várt hello oszlopok esetleg hiányzik a céltábla hello, vagy fordítva. 

Az Azure SQL Database szolgáltatás észleli az anomáliadetektálási hello számú séma kapcsolatos SQL hiba történt az Azure SQL Database az ajánlás "Javítsa ki a hibát a séma" jelenik meg. a következő táblázat azt mutatja be hello hibák, amelyek kapcsolódó tooschema problémák hello:

| SQL-hibakód: | Üzenet |
| --- | --- |
| 201 |Az eljárás vagy függvény "*"paramétert vár"*", amely nem lett megadva. |
| 207 |Érvénytelen oszlopnév "*". |
| 208 |Érvénytelen objektum neve "*". |
| 213 |Oszlop neve vagy a megadott érték nem egyezik a tábla definíciójában. |
| 2812 |Nem található a tárolt eljárás ' *'. |
| 8144 |Az eljárás vagy függvény * megadott túl sok argumentum tartozik. |

## <a name="next-steps"></a>Következő lépések
A javaslatok figyelése, és továbbra is tooapply őket toorefine teljesítményét. Adatbázis-terhelések dinamikusak, és folyamatosan módosítása. SQL Database advisor segédprogramot toomonitor folytatódik, és adja meg a javaslatok, javíthatja az adatbázis teljesítménye. 

* Lásd: [teljesítmény javaslatok az Azure-portálon hello](sql-database-advisor-portal.md) lépései hogyan toouse teljesítmény javaslatait hello Azure-portálon.
* Lásd: [lekérdezési teljesítmény Insights](sql-database-query-performance.md) toolearn kapcsolatban, tekintse meg a teljesítményre gyakorolt hatását a leggyakoribb lekérdezések hello.

## <a name="additional-resources"></a>További források
* [A Lekérdezéstár](https://msdn.microsoft.com/library/dn817826.aspx)
* [INDEX LÉTREHOZÁSA](https://msdn.microsoft.com/library/ms188783.aspx)
* [Szerepköralapú hozzáférés-vezérlés](../active-directory/role-based-access-control-what-is.md)

