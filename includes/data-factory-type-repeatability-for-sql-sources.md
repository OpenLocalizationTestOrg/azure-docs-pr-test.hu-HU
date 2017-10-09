## <a name="repeatability-during-copy"></a>Ismételhetőség másolása során
Amikor adatok tooAzure SQL/SQL Server másolását más adatokat tárol egy igények tookeep ismételhetőség szem előtt tartva tooavoid nem kívánt eredmények. 

SQL-/ SQL Server-adatbázis adatok tooAzure másolásakor másolási tevékenység fog alapértelmezett APPEND hello adatkészlet toohello fogadó tábla alapértelmezés szerint. Például az adatok másolása egy CSV (vesszővel tagolt értékek) fájlt tartalmazó adatforrást két tooAzure SQL/SQL Server-adatbázis rögzíti, amikor ez legyen milyen hello táblázat láthatóhoz hasonló:

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    2            2015-05-01 00:00:00
```

Tegyük fel, hogy a hibákat a forrásfájl és a frissített hello mennyiséget le cső 2 too4 hello forrásfájlban talált. Ha újra futtatni az adott időszakra hello adatszelet, két új bejegyzést fűzött tooAzure SQL/SQL Server-adatbázis találhat. az alábbi hello azt feltételezi, hogy hello tábla oszlopainak hello egyik sincs hello elsődlegeskulcs-megkötés.

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    2            2015-05-01 00:00:00
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    4            2015-05-01 00:00:00
```

tooavoid, toospecify UPSERT szemantikáját hello alatt alábbiakban említett 2 mechanizmusok használatával szüksége lesz.

> [!NOTE]
> A szelet futtatható újra automatikusan az Azure Data Factory megadott hello újrapróbálkozási házirend szerint.
> 
> 

### <a name="mechanism-1"></a>1 mechanizmus
Kihasználhatja **sqlWriterCleanupScript** tulajdonság toofirst törlési művelet végrehajtása a szelet futtatásakor. 

```json
"sink":  
{ 
  "type": "SqlSink", 
  "sqlWriterCleanupScript": "$$Text.Format('DELETE FROM table WHERE ModifiedDate >= \\'{0:yyyy-MM-dd HH:mm}\\' AND ModifiedDate < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
}
```

hello karbantartási parancsprogramot volna hajtható végre első példányát egy adott szelet, amely volna hello adatok törlése hello SQL táblázat megfelelő toothat szelet során. hello tevékenység később lesz hello adatok beszúrása hello SQL táblázat. 

Ha hello szelet most újra futtatni, akkor megkeresi hello mennyiség frissíteni, mivel a szükséges.

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    4            2015-05-01 00:00:00
```

Tegyük fel, hello strukturálatlan szélvédőmosó rekordot a rendszer eltávolítja a hello eredeti csv. Akkor majd az ismételt futtatásával hello szelet hello eredménye a következő eredmény: 

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
7     Down Tube    4            2015-05-01 00:00:00
```
Újdonságunk kellett toobe történik. hello másolási tevékenység hello tisztítás parancsfájl toodelete hello megfelelő adatokat, hogy a szelet futott. Ezután hello bemeneti olvasni (amely majd tartalmazott mindössze 1 bejegyzés) hello csv és illeszteni a hello tábla. 

### <a name="mechanism-2"></a>2 mechanizmus
> [!IMPORTANT]
> sliceIdentifierColumnName jelenleg nem támogatott az Azure SQL Data Warehouse. 

Egy másik mechanizmust tooachieve ismételhetőség van azzal, hogy a kijelölt oszlop (**sliceIdentifierColumnName**) hello a cél a táblában. Ez az oszlop Azure Data Factory tooensure hello forrás és cél maradás szinkronizált használhatják. Ezt a módszert akkor működik, ha módosításával vagy hello cél SQL tábla sémáját definiáló rugalmasságot. 

Ebben az oszlopban által az Azure Data Factory ismételhetőség célból használják, és hello folyamat Azure Data Factory nem teszi meg semmilyen sémát toohello tábla változik. Módon toouse ezt a módszert használja:

1. Hello cél SQL tábla típusú oszlop bináris (32) ad meg. Ebben az oszlopban megkötés kell lennie. Ehhez a példához tegyük nevezze ebben az oszlopban "ColumnForADFuseOnly".
2. Használja az hello másolási tevékenység az alábbiak szerint:
   
    ```json
    "sink":  
    { 
   
        "type": "SqlSink", 
        "sliceIdentifierColumnName": "ColumnForADFuseOnly"
    }
    ```

Az Azure Data Factory feltölti ezt az oszlopot a szükség szerinti tooensure hello forrás és cél szinkronizált maradnak. az oszlop értékeinek hello nem használható ebben a környezetben kívül hello felhasználó. 

Hasonló toomechanism 1, a másolási tevékenység során lesz automatikusan első hello adatok számára megadott szelet a hello célhelyről SQL táblázat, és futtassa az hello másolási tevékenység során általában hello karbantartása az, hogy a szelet forrás toodestination tooinsert hello adatait. 

