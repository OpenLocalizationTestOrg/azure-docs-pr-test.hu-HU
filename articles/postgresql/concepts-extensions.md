---
title: "Azure-adatbázis PostgreSQL-bővítményeket használ PostgreSQL |} Microsoft Docs"
description: "Kiterjesztheti az Azure-adatbázis bővítményekkel PostgreSQL az adatbázis működését ismerteti."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 06/29/2017
ms.openlocfilehash: 755d1cf1a921f6be8f28a4a8ae515db08d904fcd
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="postgresql-extensions-in-azure-database-for-postgresql"></a>PostgreSQL-kiterjesztések az Azure PostgreSQL-adatbázishoz
PostgreSQL teszi lehetővé az adatbázis bővítmény használatával bővítése. Bővítmények lehetővé teszik a több kapcsolódó SQL objektumok kell egybe kötegelik csomagban és a betöltése vagy eltávolítja az adatbázisból egyetlen paranccsal. Egyszer betölteni az adatbázis bővítmény hasonlóan a épített funkciók működjön. PostgreSQL-bővítmények további információkért lásd: [csomagolás kapcsolódó objektumok egy bővítmény](https://www.postgresql.org/docs/9.6/static/extend-extensions.html).

## <a name="how-to-use-postgresql-extensions"></a>Hogyan PostgreSQL-bővítmények használatát?
PostgreSQL-bővítmények telepítve kell lennie az adatbázis előtt is használhatja. Egy adott bővítmény telepítéséhez futtassa a [kiterjesztés létrehozása](https://www.postgresql.org/docs/9.6/static/sql-createextension.html) parancs psql eszközből a csomagolt objektumok betölteni az adatbázisba.

Az itt felsorolt Azure PostgreSQL-adatbázishoz támogatja kulcs bővítmények egy részét. Az alább felsorolt, túl egyéb bővítményei nem támogatottak. Nem hozható létre saját bővítmény Azure Database PostgreSQL-szolgáltatás.

## <a name="extensions-supported-by-azure-database-for-postgresql"></a>Azure-adatbázis által támogatott PostgreSQL bővítmények
Az alábbi táblázatok a szabványos PostgreSQL-bővítmények PostgreSQL Azure-adatbázis által jelenleg támogatott. Ezeket az információkat szerezzen be pg lekérdezésével\_elérhető\_bővítmények. 

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
| [fuzzystrmatch](https://www.postgresql.org/docs/9.6/static/fuzzystrmatch.html) | Az Hasonlóságok és karakterláncok közötti távolság meghatározásához számos funkciókat biztosít. |
| [intarray](https://www.postgresql.org/docs/9.6/static/intarray.html) | Biztosítja a funkciók és operátorok kezelésére szolgáló tömb null-mentes egész számok. |
| [pgcrypto](https://www.postgresql.org/docs/9.6/static/pgcrypto.html) | Titkosítási funkciókat biztosít |
| [a védelmi\_partman](https://pgxn.org/dist/pg_partman/doc/pg_partman.html) | Kezeli a particionált táblákat idő vagy azonosítója |
| [a védelmi\_trgm](https://www.postgresql.org/docs/9.6/static/pgtrgm.html) | Függvények és operátorok biztosít a hasonlóság trigram megfelelő alfanumerikus szöveg meghatározása |
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
| [a védelmi\_buffercache](https://www.postgresql.org/docs/9.6/static/pgbuffercache.html) | Arra szolgál, hogy mi történik a megosztott pufferből gyorsítótárban a valós idejű vizsgálata. |
| [a védelmi\_prewarm](https://www.postgresql.org/docs/9.6/static/pgprewarm.html) | Relációs adatok betöltése a puffer gyorsítótárába lehetőséget biztosít. |
| [a védelmi\_stat\_utasítások](https://www.postgresql.org/docs/9.6/static/pgstatstatements.html) | Lehetővé teszi a kiszolgáló által végrehajtott összes SQL-utasítások végrehajtása statisztikáinak nyomon követése. |
| [postgres\_fdw](https://www.postgresql.org/docs/9.6/static/postgres-fdw.html) | A külső PostgreSQL-kiszolgálókon tárolt adatok eléréséhez használt idegen-adatok burkoló |

### <a name="postgis"></a>PostGIS

> [!div class="mx-tableFixed"]
| **Bővítmény** | **Leírás** |
|---|---|
| [PostGIS](http://www.postgis.net/), postgis\_topológia, postgis\_tiger\_geocoder, postgis\_sfcgal | Térbeli és földrajzi objektumok PostgreSQL. |
| cím\_standardizer, cím\_standardizer\_adatok\_us | Egy címet értelmezhető alkotóelemeket is. Használja a geokódolás cím normalizálási lépés támogatásához. |
| [pgrouting](http://pgrouting.org/) | Kiterjeszti a PostGIS / PostgreSQL földrajzi adatbázis adataival kínálnak a földrajzi útválasztási funkciót. |

## <a name="next-steps"></a>Következő lépések
Nem jelenik meg egy bővítmény szeretné használni? Tudassa velünk. Meglévő kérelmek szavazzon, vagy hozzon létre új visszajelzések és a kívánja a [ügyfél-visszajelzési fórumon](https://feedback.azure.com/forums/597976-azure-database-for-postgresql).
