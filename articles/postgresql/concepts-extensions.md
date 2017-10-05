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
# <a name="postgresql-extensions-in-azure-database-for-postgresql"></a><span data-ttu-id="6086f-103">PostgreSQL-kiterjesztések az Azure PostgreSQL-adatbázishoz</span><span class="sxs-lookup"><span data-stu-id="6086f-103">PostgreSQL Extensions in Azure Database for PostgreSQL</span></span>
<span data-ttu-id="6086f-104">PostgreSQL teszi lehetővé az adatbázis bővítmény használatával bővítése.</span><span class="sxs-lookup"><span data-stu-id="6086f-104">PostgreSQL provides the ability to extend the functionality of your database using extensions.</span></span> <span data-ttu-id="6086f-105">Bővítmények lehetővé teszik a több kapcsolódó SQL objektumok kell egybe kötegelik csomagban és a betöltése vagy eltávolítja az adatbázisból egyetlen paranccsal.</span><span class="sxs-lookup"><span data-stu-id="6086f-105">Extensions allow for multiple related SQL objects to be bundled together in a single package and can be loaded or removed from your database with a single command.</span></span> <span data-ttu-id="6086f-106">Egyszer betölteni az adatbázis bővítmény hasonlóan a épített funkciók működjön.</span><span class="sxs-lookup"><span data-stu-id="6086f-106">Extensions once loaded into the database can function just like features that are built in.</span></span> <span data-ttu-id="6086f-107">PostgreSQL-bővítmények további információkért lásd: [csomagolás kapcsolódó objektumok egy bővítmény](https://www.postgresql.org/docs/9.6/static/extend-extensions.html).</span><span class="sxs-lookup"><span data-stu-id="6086f-107">For more information on PostgreSQL extensions, see [Packaging Related Objects into an Extension](https://www.postgresql.org/docs/9.6/static/extend-extensions.html).</span></span>

## <a name="how-to-use-postgresql-extensions"></a><span data-ttu-id="6086f-108">Hogyan PostgreSQL-bővítmények használatát?</span><span class="sxs-lookup"><span data-stu-id="6086f-108">How to use PostgreSQL extensions?</span></span>
<span data-ttu-id="6086f-109">PostgreSQL-bővítmények telepítve kell lennie az adatbázis előtt is használhatja.</span><span class="sxs-lookup"><span data-stu-id="6086f-109">PostgreSQL extensions need to be installed for your database before you can use them.</span></span> <span data-ttu-id="6086f-110">Egy adott bővítmény telepítéséhez futtassa a [kiterjesztés létrehozása](https://www.postgresql.org/docs/9.6/static/sql-createextension.html) parancs psql eszközből a csomagolt objektumok betölteni az adatbázisba.</span><span class="sxs-lookup"><span data-stu-id="6086f-110">To install a particular extension, run the [CREATE EXTENSION](https://www.postgresql.org/docs/9.6/static/sql-createextension.html) command from psql tool to load the packaged objects into your database.</span></span>

<span data-ttu-id="6086f-111">Az itt felsorolt Azure PostgreSQL-adatbázishoz támogatja kulcs bővítmények egy részét.</span><span class="sxs-lookup"><span data-stu-id="6086f-111">Azure Database for PostgreSQL supports a subset of key extensions as listed here.</span></span> <span data-ttu-id="6086f-112">Az alább felsorolt, túl egyéb bővítményei nem támogatottak.</span><span class="sxs-lookup"><span data-stu-id="6086f-112">Beyond the ones listed, other extensions are not supported.</span></span> <span data-ttu-id="6086f-113">Nem hozható létre saját bővítmény Azure Database PostgreSQL-szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="6086f-113">You cannot create your own extension with Azure Database for PostgreSQL service.</span></span>

## <a name="extensions-supported-by-azure-database-for-postgresql"></a><span data-ttu-id="6086f-114">Azure-adatbázis által támogatott PostgreSQL bővítmények</span><span class="sxs-lookup"><span data-stu-id="6086f-114">Extensions supported by Azure Database for PostgreSQL</span></span>
<span data-ttu-id="6086f-115">Az alábbi táblázatok a szabványos PostgreSQL-bővítmények PostgreSQL Azure-adatbázis által jelenleg támogatott.</span><span class="sxs-lookup"><span data-stu-id="6086f-115">The following tables list the standard PostgreSQL extensions that are currently supported by Azure Database for PostgreSQL.</span></span> <span data-ttu-id="6086f-116">Ezeket az információkat szerezzen be pg lekérdezésével\_elérhető\_bővítmények.</span><span class="sxs-lookup"><span data-stu-id="6086f-116">You can also obtain this information by querying pg\_available\_extensions.</span></span> 

### <a name="data-types-extensions"></a><span data-ttu-id="6086f-117">Adattípus-bővítmények</span><span class="sxs-lookup"><span data-stu-id="6086f-117">Data types extensions</span></span>

> [!div class="mx-tableFixed"]
| <span data-ttu-id="6086f-118">**Bővítmény**</span><span class="sxs-lookup"><span data-stu-id="6086f-118">**Extension**</span></span> | <span data-ttu-id="6086f-119">**Leírás**</span><span class="sxs-lookup"><span data-stu-id="6086f-119">**Description**</span></span> |
|---|---|
| [<span data-ttu-id="6086f-120">citext</span><span class="sxs-lookup"><span data-stu-id="6086f-120">citext</span></span>](https://www.postgresql.org/docs/9.6/static/citext.html) | <span data-ttu-id="6086f-121">Nem betűérzékeny karakter karakterlánc típusú biztosít</span><span class="sxs-lookup"><span data-stu-id="6086f-121">Provides a case-insensitive character string type</span></span> |
| [<span data-ttu-id="6086f-122">hstore</span><span class="sxs-lookup"><span data-stu-id="6086f-122">hstore</span></span>](https://www.postgresql.org/docs/9.6/static/hstore.html) | <span data-ttu-id="6086f-123">Adattípus-készletek kulcs/érték párok tárolására biztosít</span><span class="sxs-lookup"><span data-stu-id="6086f-123">Provides data type for storing sets of key/value pairs</span></span> |

### <a name="functions-extensions"></a><span data-ttu-id="6086f-124">Funkciók bővítmények</span><span class="sxs-lookup"><span data-stu-id="6086f-124">Functions extensions</span></span>

> [!div class="mx-tableFixed"]
| <span data-ttu-id="6086f-125">**Bővítmény**</span><span class="sxs-lookup"><span data-stu-id="6086f-125">**Extension**</span></span> | <span data-ttu-id="6086f-126">**Leírás**</span><span class="sxs-lookup"><span data-stu-id="6086f-126">**Description**</span></span> |
|---|---|
| [<span data-ttu-id="6086f-127">fuzzystrmatch</span><span class="sxs-lookup"><span data-stu-id="6086f-127">fuzzystrmatch</span></span>](https://www.postgresql.org/docs/9.6/static/fuzzystrmatch.html) | <span data-ttu-id="6086f-128">Az Hasonlóságok és karakterláncok közötti távolság meghatározásához számos funkciókat biztosít.</span><span class="sxs-lookup"><span data-stu-id="6086f-128">Provides several functions to determine similarities and distance between strings.</span></span> |
| [<span data-ttu-id="6086f-129">intarray</span><span class="sxs-lookup"><span data-stu-id="6086f-129">intarray</span></span>](https://www.postgresql.org/docs/9.6/static/intarray.html) | <span data-ttu-id="6086f-130">Biztosítja a funkciók és operátorok kezelésére szolgáló tömb null-mentes egész számok.</span><span class="sxs-lookup"><span data-stu-id="6086f-130">Provides functions and operators for manipulating null-free arrays of integers.</span></span> |
| [<span data-ttu-id="6086f-131">pgcrypto</span><span class="sxs-lookup"><span data-stu-id="6086f-131">pgcrypto</span></span>](https://www.postgresql.org/docs/9.6/static/pgcrypto.html) | <span data-ttu-id="6086f-132">Titkosítási funkciókat biztosít</span><span class="sxs-lookup"><span data-stu-id="6086f-132">Provides cryptographic functions</span></span> |
| [<span data-ttu-id="6086f-133">a védelmi\_partman</span><span class="sxs-lookup"><span data-stu-id="6086f-133">pg\_partman</span></span>](https://pgxn.org/dist/pg_partman/doc/pg_partman.html) | <span data-ttu-id="6086f-134">Kezeli a particionált táblákat idő vagy azonosítója</span><span class="sxs-lookup"><span data-stu-id="6086f-134">Manages partitioned tables by time or ID</span></span> |
| [<span data-ttu-id="6086f-135">a védelmi\_trgm</span><span class="sxs-lookup"><span data-stu-id="6086f-135">pg\_trgm</span></span>](https://www.postgresql.org/docs/9.6/static/pgtrgm.html) | <span data-ttu-id="6086f-136">Függvények és operátorok biztosít a hasonlóság trigram megfelelő alfanumerikus szöveg meghatározása</span><span class="sxs-lookup"><span data-stu-id="6086f-136">Provides functions and operators for determining the similarity of alphanumeric text based on trigram matching</span></span> |
| [<span data-ttu-id="6086f-137">UUID-ossp</span><span class="sxs-lookup"><span data-stu-id="6086f-137">uuid-ossp</span></span>](https://www.postgresql.org/docs/9.6/static/uuid-ossp.html) | <span data-ttu-id="6086f-138">Univerzálisan egyedi azonosítók (UUID-k) létrehozása</span><span class="sxs-lookup"><span data-stu-id="6086f-138">Generate universally unique identifiers (UUIDs)</span></span> |

### <a name="full-text-search-extensions"></a><span data-ttu-id="6086f-139">Teljes szöveges keresési bővítmények</span><span class="sxs-lookup"><span data-stu-id="6086f-139">Full-text Search extensions</span></span>

> [!div class="mx-tableFixed"]
| <span data-ttu-id="6086f-140">**Bővítmény**</span><span class="sxs-lookup"><span data-stu-id="6086f-140">**Extension**</span></span> | <span data-ttu-id="6086f-141">**Leírás**</span><span class="sxs-lookup"><span data-stu-id="6086f-141">**Description**</span></span> |
|---|---|
| [<span data-ttu-id="6086f-142">unaccent</span><span class="sxs-lookup"><span data-stu-id="6086f-142">unaccent</span></span>](https://www.postgresql.org/docs/9.6/static/unaccent.html) | <span data-ttu-id="6086f-143">Szöveg (diakritikus jeleket) ékezetek eltávolítása lexemes keresési szótár.</span><span class="sxs-lookup"><span data-stu-id="6086f-143">A text search dictionary that removes accents (diacritic signs) from lexemes.</span></span> |

### <a name="index-types-extensions"></a><span data-ttu-id="6086f-144">Index típusú kiterjesztések</span><span class="sxs-lookup"><span data-stu-id="6086f-144">Index Types extensions</span></span>

> [!div class="mx-tableFixed"]
| <span data-ttu-id="6086f-145">**Bővítmény**</span><span class="sxs-lookup"><span data-stu-id="6086f-145">**Extension**</span></span> | <span data-ttu-id="6086f-146">**Leírás**</span><span class="sxs-lookup"><span data-stu-id="6086f-146">**Description**</span></span> |
|---|---|
| [<span data-ttu-id="6086f-147">fa\_gin</span><span class="sxs-lookup"><span data-stu-id="6086f-147">btree\_gin</span></span>](https://www.postgresql.org/docs/9.6/static/btree-gin.html) | <span data-ttu-id="6086f-148">A minta GIN operátor osztályokat, amelyek megvalósítják a B-fa egyes adattípusok viselkedése például biztosít.</span><span class="sxs-lookup"><span data-stu-id="6086f-148">Provides sample GIN operator classes that implement B-tree like behavior for certain data types.</span></span> |
| [<span data-ttu-id="6086f-149">fa\_gist</span><span class="sxs-lookup"><span data-stu-id="6086f-149">btree\_gist</span></span>](https://www.postgresql.org/docs/9.6/static/btree-gist.html) | <span data-ttu-id="6086f-150">GiST index operátor osztályokat, amelyek megvalósítják az B-fa biztosít.</span><span class="sxs-lookup"><span data-stu-id="6086f-150">Provides GiST index operator classes that implement B-tree.</span></span> |

### <a name="language-extensions"></a><span data-ttu-id="6086f-151">Nyelvi bővítmények</span><span class="sxs-lookup"><span data-stu-id="6086f-151">Language extensions</span></span>

> [!div class="mx-tableFixed"]
| <span data-ttu-id="6086f-152">**Bővítmény**</span><span class="sxs-lookup"><span data-stu-id="6086f-152">**Extension**</span></span> | <span data-ttu-id="6086f-153">**Leírás**</span><span class="sxs-lookup"><span data-stu-id="6086f-153">**Description**</span></span> |
|---|---|
| [<span data-ttu-id="6086f-154">plpgsql</span><span class="sxs-lookup"><span data-stu-id="6086f-154">plpgsql</span></span>](https://www.postgresql.org/docs/9.6/static/plpgsql.html) | <span data-ttu-id="6086f-155">PL/pgSQL betölthető eljárási nyelv</span><span class="sxs-lookup"><span data-stu-id="6086f-155">PL/pgSQL loadable procedural language</span></span> |

### <a name="miscellaneous-extensions"></a><span data-ttu-id="6086f-156">Vegyes bővítmények</span><span class="sxs-lookup"><span data-stu-id="6086f-156">Miscellaneous extensions</span></span>

> [!div class="mx-tableFixed"]
| <span data-ttu-id="6086f-157">**Bővítmény**</span><span class="sxs-lookup"><span data-stu-id="6086f-157">**Extension**</span></span> | <span data-ttu-id="6086f-158">**Leírás**</span><span class="sxs-lookup"><span data-stu-id="6086f-158">**Description**</span></span> |
|---|---|
| [<span data-ttu-id="6086f-159">a védelmi\_buffercache</span><span class="sxs-lookup"><span data-stu-id="6086f-159">pg\_buffercache</span></span>](https://www.postgresql.org/docs/9.6/static/pgbuffercache.html) | <span data-ttu-id="6086f-160">Arra szolgál, hogy mi történik a megosztott pufferből gyorsítótárban a valós idejű vizsgálata.</span><span class="sxs-lookup"><span data-stu-id="6086f-160">Provides a means for examining what's happening in the shared buffer cache in real time.</span></span> |
| [<span data-ttu-id="6086f-161">a védelmi\_prewarm</span><span class="sxs-lookup"><span data-stu-id="6086f-161">pg\_prewarm</span></span>](https://www.postgresql.org/docs/9.6/static/pgprewarm.html) | <span data-ttu-id="6086f-162">Relációs adatok betöltése a puffer gyorsítótárába lehetőséget biztosít.</span><span class="sxs-lookup"><span data-stu-id="6086f-162">Provides a way to load relation data into the buffer cache.</span></span> |
| [<span data-ttu-id="6086f-163">a védelmi\_stat\_utasítások</span><span class="sxs-lookup"><span data-stu-id="6086f-163">pg\_stat\_statements</span></span>](https://www.postgresql.org/docs/9.6/static/pgstatstatements.html) | <span data-ttu-id="6086f-164">Lehetővé teszi a kiszolgáló által végrehajtott összes SQL-utasítások végrehajtása statisztikáinak nyomon követése.</span><span class="sxs-lookup"><span data-stu-id="6086f-164">Provides a means for tracking execution statistics of all SQL statements executed by a server.</span></span> |
| [<span data-ttu-id="6086f-165">postgres\_fdw</span><span class="sxs-lookup"><span data-stu-id="6086f-165">postgres\_fdw</span></span>](https://www.postgresql.org/docs/9.6/static/postgres-fdw.html) | <span data-ttu-id="6086f-166">A külső PostgreSQL-kiszolgálókon tárolt adatok eléréséhez használt idegen-adatok burkoló</span><span class="sxs-lookup"><span data-stu-id="6086f-166">Foreign-data wrapper used to access data stored in external PostgreSQL servers</span></span> |

### <a name="postgis"></a><span data-ttu-id="6086f-167">PostGIS</span><span class="sxs-lookup"><span data-stu-id="6086f-167">PostGIS</span></span>

> [!div class="mx-tableFixed"]
| <span data-ttu-id="6086f-168">**Bővítmény**</span><span class="sxs-lookup"><span data-stu-id="6086f-168">**Extension**</span></span> | <span data-ttu-id="6086f-169">**Leírás**</span><span class="sxs-lookup"><span data-stu-id="6086f-169">**Description**</span></span> |
|---|---|
| <span data-ttu-id="6086f-170">[PostGIS](http://www.postgis.net/), postgis\_topológia, postgis\_tiger\_geocoder, postgis\_sfcgal</span><span class="sxs-lookup"><span data-stu-id="6086f-170">[PostGIS](http://www.postgis.net/), postgis\_topology, postgis\_tiger\_geocoder, postgis\_sfcgal</span></span> | <span data-ttu-id="6086f-171">Térbeli és földrajzi objektumok PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="6086f-171">Spatial and geographic objects for PostgreSQL.</span></span> |
| <span data-ttu-id="6086f-172">cím\_standardizer, cím\_standardizer\_adatok\_us</span><span class="sxs-lookup"><span data-stu-id="6086f-172">address\_standardizer, address\_standardizer\_data\_us</span></span> | <span data-ttu-id="6086f-173">Egy címet értelmezhető alkotóelemeket is.</span><span class="sxs-lookup"><span data-stu-id="6086f-173">Used to parse an address into constituent elements.</span></span> <span data-ttu-id="6086f-174">Használja a geokódolás cím normalizálási lépés támogatásához.</span><span class="sxs-lookup"><span data-stu-id="6086f-174">Used to support geocoding address normalization step.</span></span> |
| [<span data-ttu-id="6086f-175">pgrouting</span><span class="sxs-lookup"><span data-stu-id="6086f-175">pgrouting</span></span>](http://pgrouting.org/) | <span data-ttu-id="6086f-176">Kiterjeszti a PostGIS / PostgreSQL földrajzi adatbázis adataival kínálnak a földrajzi útválasztási funkciót.</span><span class="sxs-lookup"><span data-stu-id="6086f-176">Extends the PostGIS / PostgreSQL geospatial database to provide geospatial routing functionality.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="6086f-177">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6086f-177">Next steps</span></span>
<span data-ttu-id="6086f-178">Nem jelenik meg egy bővítmény szeretné használni?</span><span class="sxs-lookup"><span data-stu-id="6086f-178">Don't see an extension you'd like to use?</span></span> <span data-ttu-id="6086f-179">Tudassa velünk.</span><span class="sxs-lookup"><span data-stu-id="6086f-179">Please let us know.</span></span> <span data-ttu-id="6086f-180">Meglévő kérelmek szavazzon, vagy hozzon létre új visszajelzések és a kívánja a [ügyfél-visszajelzési fórumon](https://feedback.azure.com/forums/597976-azure-database-for-postgresql).</span><span class="sxs-lookup"><span data-stu-id="6086f-180">Vote for existing requests or create new feedback and wishes in our [Customer feedback forum](https://feedback.azure.com/forums/597976-azure-database-for-postgresql).</span></span>
