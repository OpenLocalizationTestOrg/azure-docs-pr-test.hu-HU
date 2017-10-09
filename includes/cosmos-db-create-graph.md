Hello Data Explorer eszköz már használhatja, ha az Azure portál toocreate egy grafikonon adatbázis hello. 

1. Hello hello bal oldali navigációs menü, Azure-portálon kattintson **adatok kezelővel (előzetes verzió)**. 
2. A hello **adatok kezelővel (előzetes verzió)** panelen kattintson a **új Graph**, használja a következő információ hello hello lapon töltse ki.

    ![Az Azure-portálon hello adatkezelő](./media/cosmos-db-create-graph/azure-cosmosdb-data-explorer.png)

    Beállítás|Ajánlott érték|Leírás
    ---|---|---
    Adatbázis azonosítója|sample-database|az új adatbázis hello azonosítója. Az adatbázis neve 1–255 karakter hosszúságú lehet, és nem tartalmazhat `/ \ # ?` karaktereket vagy záró szóközt.
    Gráfazonosító|sample-graph|az új diagram hello azonosítója. Graph nevek rendelkezik hello azonos követelmények karakter, adatbázis-azonosító.
    Tárkapacitás| 10 GB|Hagyja hello alapértelmezett értéket. Ez a hello tárolási kapacitás hello adatbázis.
    Teljesítmény|400 kérelemegység|Hagyja hello alapértelmezett értéket. Legfeljebb hello átviteli később Ha azt szeretné, hogy tooreduce késés.
    Partíciókulcs|/userid|A partíciós kulcs, amely egyenletes elosztása adatok tooeach partíció. Megfelelő partíciókulcs fontos létrehozni egy performant hello kiválasztásával diagramot, további tájékoztatást talál a [a particionálás tervezése](../articles/cosmos-db/partition-data.md#designing-for-partitioning).

3. Amikor hello űrlap ki van töltve, kattintson a **OK**.
