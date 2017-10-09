---
title: "az Azure Data Factory aaaRepeatable másolása |} Microsoft Docs"
description: "Ismerje meg, hogyan tooavoid másolatot készít, annak ellenére, hogy a szelet, amely másolja az adatokat csak egyszer fut-e."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: 
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2017
ms.author: jingwang
ms.openlocfilehash: 79a3fde2b700bf0a0e167479d6a86c5bee1bf7ec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="repeatable-copy-in-azure-data-factory"></a>Az Azure Data Factory ismételhető másolása

## <a name="repeatable-read-from-relational-sources"></a>A relációs források ismételhető Olvasás
Amikor az adatok másolása relációs adattároló, tartsa ismételhetőség szem előtt tartva tooavoid nem kívánt eredmények. Az Azure Data Factoryben futtathatja a szelet manuálisan. Beállíthatja úgy is egy adatkészlet újrapróbálkozási házirendje, hogy a szelet akkor fut újra, ha hiba történik. A szelet akkor fut újra, vagy módon, ha van szüksége arról, hogy ugyanazokat az adatokat hello toomake hogyan olvasható függetlenül attól, hogy hányszor a szelet futtatása.  
 
> [!NOTE]
> hello következő mintákat az Azure SQL azonban találhatók, amely támogatja a téglalap alakú adatkészletek alkalmazható tooany adattár. Előfordulhat, hogy tooadjust hello **típus** a forrás- és hello **lekérdezés** tulajdonság (például: lekérdezés helyett sqlReaderQuery) hello adatok tárolására.   

Általában relációs áruházakból olvasásakor érdemes tooread toothat szelet megfelelő csak hello adatokat. Egy módon toodo így lenne hello WindowStart és WindowEnd rendszerváltozók érhető el az Azure Data Factory használatával. További információ a hello változók és függvényeket az Azure Data Factory itt hello [Azure Data Factory - funkciók és rendszerváltozók](data-factory-functions-variables.md) cikk. Példa: 

```json
"source": {
    "type": "SqlSource",
    "sqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm\\'', WindowStart, WindowEnd)"
},
```
Ez a lekérdezés MyTable hello táblából (WindowStart -> WindowEnd) hello szelet időtartama közé eső adatok beolvasása. Futtassa újból a szeletet a is minden esetben gondoskodjon arról, hogy adott hello olvasható ugyanazokat az adatokat. 

Más esetekben lehet szeretnének tooread hello teljes táblázat és a következőképpen határozhatnak meg hello sqlReaderQuery:

```json
"source": 
{            
    "type": "SqlSource",
    "sqlReaderQuery": "select * from MyTable"
},
```

## <a name="repeatable-write-toosqlsink"></a>Ismételhető írási tooSqlSink
Ha az adatok másolásának túl**Azure SQL-/ SQL Server** az egyéb adattárakhoz tookeep ismételhetőség a szem előtt tartva tooavoid kell nem kívánt eredmények. 

Adatok tooAzure SQL/SQL Server-adatbázis másolásakor hello másolási tevékenység hozzáfűzi toohello fogadó adattábla alapértelmezés szerint. Mondja ki másolja át adatokat a egy CSV (vesszővel tagolt) fájlt tartalmazó két rekordok toohello a következő táblázat egy Azure SQL-/ SQL Server-adatbázisban. A szelet futtatásakor hello két rekordok olyan másolt toohello SQL táblázat. 

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    2            2015-05-01 00:00:00
```

Tegyük fel, hogy hiba található a forrásfájl és hello mennyiséget le cső 2 too4 frissítése. Ha hello adatszelet manuálisan adott időszakra Újrafuttatja, két új bejegyzést fűzött tooAzure SQL/SQL Server-adatbázis találhat. Ez a példa feltételezi, hogy nincs hello tábla oszlopainak hello rendelkezik-e hello elsődlegeskulcs-megkötés.

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    2            2015-05-01 00:00:00
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    4            2015-05-01 00:00:00
```

tooavoid ezt a viselkedést kell toospecify UPSERT szemantikáját használja a következő két mechanizmus hello egyikét:

### <a name="mechanism-1-using-sqlwritercleanupscript"></a>1. módszer: sqlWriterCleanupScript használatával
Használhatja a hello **sqlWriterCleanupScript** tulajdonság tooclean mentése előtt hello adatok beszúrása szelet futtatásakor hello fogadó tábla adatait. 

```json
"sink":  
{ 
  "type": "SqlSink", 
  "sqlWriterCleanupScript": "$$Text.Format('DELETE FROM table WHERE ModifiedDate >= \\'{0:yyyy-MM-dd HH:mm}\\' AND ModifiedDate < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
}
```

A szelet futtatásakor hello tisztítás parancsfájl futtatása első toodelete adat, amely megfelel a toohello szelet hello SQL-táblából. hello másolási tevékenység majd adatbeszúrást hello SQL táblázat. Hello szelet akkor fut újra, ha hello mennyiség frissül igény szerint.

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    4            2015-05-01 00:00:00
```

Tegyük fel, hello strukturálatlan szélvédőmosó rekordot a rendszer eltávolítja a hello eredeti csv. Majd újrafuttatásakor valószínűleg megtörténik a hello szelet hozna hello eredménye a következő: 

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
7     Down Tube    4            2015-05-01 00:00:00
```

hello másolási tevékenység hello tisztítás parancsfájl toodelete hello megfelelő adatokat, hogy a szelet futott. Ezután hello bemeneti olvasni (amely csak egy bejegyzés majd tartalmazott) hello csv és illeszteni a hello tábla. 

### <a name="mechanism-2-using-sliceidentifiercolumnname"></a>2. módszer: sliceIdentifierColumnName használatával
> [!IMPORTANT]
> SliceIdentifierColumnName az Azure SQL Data Warehouse nem támogatott. 

hello második mechanizmus tooachieve ismételhetőség van azzal, hogy a kijelölt oszlop (sliceIdentifierColumnName) hello cél tábla. Ez az oszlop Azure Data Factory tooensure hello forrás és cél maradás szinkronizált használhatják. Ezt a módszert akkor működik, ha módosításával vagy hello cél SQL tábla sémáját definiáló rugalmasságot. 

Ez az oszlop Azure Data Factory ismételhetőség célra használja, és hello folyamat Azure Data Factory nem végzi a séma toohello tábla változik. Módon toouse ezt a módszert használja:

1. Adja meg a típusú oszlop **bináris (32)** hello cél SQL táblázat. Ebben az oszlopban megkötés kell lennie. Ehhez a példához tegyük nevezze ebben az oszlopban AdfSliceIdentifier.


    Forrástábla:

    ```sql
    CREATE TABLE [dbo].[Student](
       [Id] [varchar](32) NOT NULL,
       [Name] [nvarchar](256) NOT NULL
    )
    ```

    Céltábla: 

    ```sql
    CREATE TABLE [dbo].[Student](
       [Id] [varchar](32) NOT NULL,
       [Name] [nvarchar](256) NOT NULL,
       [AdfSliceIdentifier] [binary](32) NULL
    )
    ```

2. Használja az hello másolási tevékenység az alábbiak szerint:
   
    ```json
    "sink":  
    { 
   
        "type": "SqlSink", 
        "sliceIdentifierColumnName": "AdfSliceIdentifier"
    }
    ```

Az Azure Data Factory tölti fel ezt az oszlopot a szükség szerinti tooensure hello forrás és cél szinkronizált maradnak. az oszlop értékeinek hello kívül ebben a környezetben nem használható. 

Hasonló toomechanism 1, a másolási tevékenység során automatikusan törli a szükségtelenné megadott szelet a hello célhelyről SQL táblázat hello hello adatait. Toohello céltáblázatban forrásból származó adatok majd beszúrása. 

## <a name="next-steps"></a>Következő lépések
Tekintse át a következő összekötő cikket, amely a JSON-példák befejezéséhez hello: 

- [Azure SQL Database](data-factory-azure-sql-connector.md)
- [Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md)
- [SQL Server](data-factory-sqlserver-connector.md)