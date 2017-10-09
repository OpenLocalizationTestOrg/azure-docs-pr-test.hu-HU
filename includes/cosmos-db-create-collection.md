Most már használhatja a hello Data Explorer eszköz hello Azure portál toocreate adatbázis és gyűjtemény. 

1. Hello hello bal oldali navigációs menü, Azure-portálon kattintson **adatok kezelővel (előzetes verzió)**. 

2. A hello **adatok kezelővel (előzetes verzió)** panelen kattintson a **új gyűjtemény**, majd adja meg a következő információ hello:

    ![hello Azure portál adatkezelő panel](./media/cosmos-db-create-collection/azure-cosmosdb-data-explorer.png)

    Beállítás|Ajánlott érték|Leírás
    ---|---|---
    Adatbázis azonosítója|Feladatok|az új adatbázis hello nevét. Az adatbázis neve 1–255 karakter hosszúságú lehet, és nem tartalmazhat /, \\, #, ? karaktereket vagy záró szóközt.
    Gyűjtemény azonosítója|Elemek|az új gyűjtemény hello nevét. Gyűjteménynevek rendelkezik hello azonos karakter adatbázis-azonosítók követelményekkel.
    Tárkapacitás| Rögzített méretű (10 GB)|Hello alapértelmezett értéket használja. Ez az érték hello tárolási kapacitás hello adatbázis.
    Teljesítmény|400 kérelemegység|Hello alapértelmezett értéket használja. Ha azt szeretné, hogy tooreduce késés, később is növelheti hello átviteli sebesség.
    Partíciókulcs|/kategória|A partíciós kulcs, amely egyenlően osztja el az adatok tooeach partíció. Kiválasztásával hello megfelelő partíciókulcs fontos performant gyűjtemény létrehozása. több, lásd: toolearn [a particionálás tervezése](../articles/cosmos-db/partition-data.md#designing-for-partitioning).    
3. Miután végrehajtotta hello képernyő, kattintson a **OK**.

Adatok Explorer jeleníti meg az új adatbázis és gyűjtemény hello. 
