---
title: "aaaUsing PostgreSQL-kiterjesztések az Azure-adatbázis a PostgreSQL |} Microsoft Docs"
description: "Az Azure-adatbázis bővítményekkel PostgreSQL az adatbázis az hello képességét tooextend hello funkcióit mutatja be."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 06/29/2017
ms.openlocfilehash: af2462d7a923b934bc0329153be7079ba86e8856
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="postgresql-extensions-in-azure-database-for-postgresql"></a>PostgreSQL-kiterjesztések az Azure PostgreSQL-adatbázishoz
PostgreSQL hello képességét tooextend hello funkciókat az adatbázis bővítmény használatával biztosítja. Bővítmények lehetővé teszik a több kapcsolódó SQL objektumok toobe egybe kötegelik csomagban és a betöltése vagy eltávolítva az adatbázisból, egyetlen parancs. Egyszer betölti hello adatbázis bővítmény hasonlóan a épített funkciók működjön. PostgreSQL-bővítmények további információkért lásd: [csomagolás kapcsolódó objektumok egy bővítmény](https://www.postgresql.org/docs/9.6/static/extend-extensions.html).

## <a name="how-toouse-postgresql-extensions"></a>Hogyan toouse PostgreSQL-bővítmények?
PostgreSQL bővítményekhez tartoznia kell toobe az adatbázis telepítve előtt is használhatja. tooinstall egy adott kiterjesztéssel, futtassa a [kiterjesztés létrehozása](https://www.postgresql.org/docs/9.6/static/sql-createextension.html) psql eszköz tooload parancsot hello csomagolt objektumok az adatbázisba.

Az itt felsorolt Azure PostgreSQL-adatbázishoz támogatja kulcs bővítmények egy részét. Túl az alább felsorolt hello más bővítmények nem támogatottak. Nem hozható létre saját bővítmény Azure Database PostgreSQL-szolgáltatás.

## <a name="extensions-supported-by-azure-database-for-postgresql"></a>Azure-adatbázis által támogatott PostgreSQL bővítmények
hello alábbi táblázatokban hello szabványos PostgreSQL kiterjesztések listájából PostgreSQL Azure-adatbázis által jelenleg támogatott. Ezeket az információkat szerezzen be pg lekérdezésével\_elérhető\_bővítmények. 

### <a name="data-types-extensions"></a>Adattípus-bővítmények

> [!div class="mx-tableFixed"]
| **Bővítmény** | **Leírás** |
|---|---|
| [citext](https://www.postgresql.org/docs/9.6/static/citext.html) | Nem betűérzékeny karakter karakterlánc típusú biztosít |
| [hstore](https://www.postgresql.org/docs/9.6/static/hstore.html) | Adattípus-készletek kulcs/érték párok tárolására biztosít |

### <a name="functions-extensions"></a>Funkciók bővítmények

> [!div class="mx-tableFixed"]
| **Bővítmény** | **Leírás** |
|---|---|
| [fuzzystrmatch](https://www.postgresql.org/docs/9.6/static/fuzzystrmatch.html) | Itt több funkciók toodetermine Hasonlóságok és karakterláncok közötti távolságot. |
| [intarray](https://www.postgresql.org/docs/9.6/static/intarray.html) | Biztosítja a funkciók és operátorok kezelésére szolgáló tömb null-mentes egész számok. |
| [pgcrypto](https://www.postgresql.org/docs/9.6/static/pgcrypto.html) | Titkosítási funkciókat biztosít |
| [a védelmi\_partman](https://pgxn.org/dist/pg_partman/doc/pg_partman.html) | Kezeli a particionált táblákat idő vagy azonosítója |
| [a védelmi\_trgm](https://www.postgresql.org/docs/9.6/static/pgtrgm.html) | Függvények és operátorok biztosít hello hasonlóság trigram megfelelő alfanumerikus szöveg meghatározása |
| [UUID-ossp](https://www.postgresql.org/docs/9.6/static/uuid-ossp.html) | Univerzálisan egyedi azonosítók (UUID-k) létrehozása |

### <a name="full-text-search-extensions"></a>Teljes szöveges keresési bővítmények

> [!div class="mx-tableFixed"]
| **Bővítmény** | **Leírás** |
|---|---|
| [unaccent](https://www.postgresql.org/docs/9.6/static/unaccent.html) | Szöveg (diakritikus jeleket) ékezetek eltávolítása lexemes keresési szótár. |

### <a name="index-types-extensions"></a>Index típusú kiterjesztések

> [!div class="mx-tableFixed"]
| **Bővítmény** | **Leírás** |
|---|---|
| [fa\_gin](https://www.postgresql.org/docs/9.6/static/btree-gin.html) | A minta GIN operátor osztályokat, amelyek megvalósítják a B-fa egyes adattípusok viselkedése például biztosít. |
| [fa\_gist](https://www.postgresql.org/docs/9.6/static/btree-gist.html) | GiST index operátor osztályokat, amelyek megvalósítják az B-fa biztosít. |

### <a name="language-extensions"></a>Nyelvi bővítmények

> [!div class="mx-tableFixed"]
| **Bővítmény** | **Leírás** |
|---|---|
| [plpgsql](https://www.postgresql.org/docs/9.6/static/plpgsql.html) | PL/pgSQL betölthető eljárási nyelv |

### <a name="miscellaneous-extensions"></a>Vegyes bővítmények

> [!div class="mx-tableFixed"]
| **Bővítmény** | **Leírás** |
|---|---|
| [a védelmi\_buffercache](https://www.postgresql.org/docs/9.6/static/pgbuffercache.html) | Arra szolgál, hogy mi történik a megosztott hello puffer gyorsítótár valós idejű vizsgálata. |
| [a védelmi\_prewarm](https://www.postgresql.org/docs/9.6/static/pgprewarm.html) | Adja meg egy módon tooload kapcsolat adatait a hello puffer gyorsítótárába. |
| [a védelmi\_stat\_utasítások](https://www.postgresql.org/docs/9.6/static/pgstatstatements.html) | Lehetővé teszi a kiszolgáló által végrehajtott összes SQL-utasítások végrehajtása statisztikáinak nyomon követése. |
| [postgres\_fdw](https://www.postgresql.org/docs/9.6/static/postgres-fdw.html) | Idegen-adatok burkoló használt külső PostgreSQL-kiszolgálókon tárolt tooaccess adatok |

### <a name="postgis"></a>PostGIS

> [!div class="mx-tableFixed"]
| **Bővítmény** | **Leírás** |
|---|---|
| [PostGIS](http://www.postgis.net/), postgis\_topológia, postgis\_tiger\_geocoder, postgis\_sfcgal | Térbeli és földrajzi objektumok PostgreSQL. |
| cím\_standardizer, cím\_standardizer\_adatok\_us | Használt tooparse alkotó elemekre történő címet. Toosupport geokódolás cím normalizálási lépés használt. |
| [pgrouting](http://pgrouting.org/) | Kiterjeszti a hello PostGIS / PostgreSQL földrajzi adatbázis tooprovide földrajzi útválasztási funkcióra. |

## <a name="next-steps"></a>Következő lépések
Nem látja azt szeretné, hogy toouse kiterjesztéssel? Tudassa velünk. Meglévő kérelmek szavazzon, vagy hozzon létre új visszajelzések és a kívánja a [ügyfél-visszajelzési fórumon](https://feedback.azure.com/forums/597976-azure-database-for-postgresql).
