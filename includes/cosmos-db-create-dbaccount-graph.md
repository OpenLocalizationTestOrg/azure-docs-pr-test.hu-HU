1. Egy új ablakban toohello bejelentkezés [Azure-portálon](https://portal.azure.com/).
2. Hello bal oldali ablaktáblában kattintson **új**, kattintson a **adatbázisok**, majd a **Azure Cosmos DB**, kattintson a **létrehozása**.
   
   ![Az Azure Portal Adatbázisok panelje](./media/cosmos-db-create-dbaccount-graph/create-nosql-db-databases-json-tutorial-1.png)

3. A hello **új fiók** panelen adja meg az Azure Cosmos DB fiókhoz használni kívánt hello konfigurációt. 

    Az Azure Cosmos DB használata esetén négy programozási modell közül választhat: Gremlin (Graph), MongoDB, SQL (DocumentDB) vagy Tábla (kulcs-érték). Jelenleg mindegyikhez külön fiókra van szükség.
       
    A gyors üzembe helyezési cikkben azt program elleni hello Graph API-val, tehát **Gremlin (diagramhoz)** , hello űrlap kitöltötte. Ha katalógusalkalmazásból származó dokumentumadatokkal, kulcs/érték (tábla) típusú adatokkal vagy MongoDB-alkalmazásból migrált adatokkal dolgozik, vegye figyelembe, hogy az Azure Cosmos DB magas rendelkezésre állású, globálisan elosztott adatbázis-szolgáltatási platformot tud biztosítani az összes alapvető fontosságú alkalmazáshoz.

    Töltse ki a hello hello mezőket **új fiók** panelen használatával hello információi hello következő útmutatóként – képernyőfelvétel az értékek eltérhetnek hello képernyőfelvétel a hello értékeket.
 
    ![Új fiók panelen hello Azure Cosmos DB](./media/cosmos-db-create-dbaccount-graph/create-nosql-db-databases-json-tutorial-2.png)

    Beállítás|Ajánlott érték|Leírás
    ---|---|---
    ID (Azonosító)|*Egyedi érték*|Az Azure Cosmos DB-fiókot azonosító egyedi név. Mivel *documents.azure.com* van hozzáfűzött toohello adni toocreate az URI, használja a egyedi, de azonosítható ID azonosítójával. hello azonosítója csak kisbetűket, számokat és kötőjel (-) karakter hello tartalmaznia kell, és a 3 too50 karaktereket kell tartalmaznia.
    API|Gremlin (gráf)|Azt a program hello elleni [Graph API](../articles/cosmos-db/graph-introduction.md) című cikkben.|
    Előfizetés|*Az Ön előfizetése*|hello Azure-előfizetést, amelyet az toouse Azure Cosmos DB ehhez a fiókhoz. 
    Erőforráscsoport|*hello azonos érték-azonosító*|hello új erőforráscsoport neve a fiókjához. Az egyszerűség kedvéért is használhatja ugyanazt a nevet hello a azonosítójával. 
    Hely|*hello régió legközelebbi tooyour felhasználók*|földrajzi helyet, mely toohost hello Azure Cosmos DB fiókját. Válassza ki a hello helyét legközelebbi tooyour felhasználók toogive leggyorsabb hozzáférést toohello adatok hello őket.

4. Kattintson a **létrehozása** toocreate hello fiók.
5. A hello felső eszköztáron kattintson a hello **értesítések** ikon ![hello értesítési ikon](./media/cosmos-db-create-dbaccount-graph/notification-icon.png) toomonitor hello telepítési folyamat.

    ![hello Azure portál értesítések ablaktábla](./media/cosmos-db-create-dbaccount-graph/notification.png)

6.  Ha hello értesítések ablakban azt jelzi, hello központi telepítés sikeresen befejeződött, zárja be hello értesítési ablakban, és új fiókot nyitott hello hello **összes erőforrás** hello irányítópult csempét. 

    ![Minden erőforrás csempe hello a DocumentDB-fiók](./media/cosmos-db-create-dbaccount-graph/azure-documentdb-all-resources.png)
