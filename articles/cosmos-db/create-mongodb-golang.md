---
cím: aaa "Azure Cosmos DB: egy Golang a MongoDB API-Konzolalkalmazás létrehozása és hello Azure portálon |} Microsoft Docs"Leírás: egy Golang kódminta tooconnect tooand is használhatja az Azure Cosmos DB szolgáltatások lekérdezése megadja: cosmos-db Szerző: Durgaprasad-Budhwani manager: jhubbard szerkesztő: mimig1

MS.Service: cosmos-db ms.topic: hero-article ms.date: 07/21/2017 ms.author: mimig
---

# <a name="azure-cosmos-db-build-a-mongodb-api-console-app-with-golang-and-hello-azure-portal"></a>Azure Cosmos DB: Egy Golang a MongoDB API-Konzolalkalmazás létrehozása és hello Azure-portálon

Az Azure Cosmos DB a Microsoft globálisan elosztott többmodelles adatbázis-szolgáltatása. Gyorsan hozzon létre, és a dokumentum, a kulcs/érték és a graph adatbázisok, amelyek kihasználhassa hello globális terjesztési és horizontális skálázhatóságot képességekről az Azure-Cosmos adatbázis hello core lekérdezése.

A gyors üzembe helyezési bemutatja, hogyan egy meglévő toouse [MongoDB](https://docs.microsoft.com/en-us/azure/cosmos-db/mongodb-introduction) írt app [Golang](https://golang.org/) , és csatlakoztassa tooyour Azure Cosmos DB adatbázis, amely támogatja a MongoDB-ügyfélkapcsolatokat.

Más szóval a Golang alkalmazás csak tudja, hogy az tooa adatbázis MongoDB API-k használatával csatlakoznak. Áttetsző toohello alkalmazás, amely adatok hello Azure Cosmos DB van tárolva.

## <a name="prerequisites"></a>Előfeltételek

- Azure-előfizetés. Ha nem rendelkezik Azure-előfizetéssel, mindössze néhány perc alatt létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free) a virtuális gép létrehozásának megkezdése előtt.
- [Nyissa meg](https://golang.org/dl/) és hello vonatkozó általános ismeretekre [Ugrás](https://golang.org/) nyelv.
- Egy IDE– [Gogland](https://www.jetbrains.com/go/) a Jetbrains-től, [Visual Studio Code](https://code.visualstudio.com/) a Microsofttól vagy [Atom](https://atom.io/). Ebben az oktatóanyagban a Goglang használatára kerül sor.

<a id="create-account"></a>
## <a name="create-a-database-account"></a>Adatbázisfiók létrehozása

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount-mongodb.md)]

## <a name="clone-hello-sample-application"></a>Klónozza a mintaalkalmazást hello

Klónozza a mintaalkalmazást hello, és telepítse a szükséges hello csomagok.

1. Hozza létre a CosmosDBSample hello GOROOT\src mappába, amely C:\Go\ alapértelmezés szerint.
2. Futtassa a következő parancsot a git terminálablakot használatával például a git bash tooclone hello minta tárház hello CosmosDBSample mappába hello. 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-mongodb-golang-getting-started.git
    ```
3.  Futtassa a következő parancs tooget hello mgo csomag hello. 

    ```
    go get gopkg.in/mgo.v2
    ```

Hello [mgo](http://labix.org/mgo) illesztőprogram (mint hangsúlyozottan *mango*) van egy [MongoDB](http://www.mongodb.org/) hello illesztőprogramját [nyelvi lépjen](http://golang.org/) , amely megvalósítja a gazdag, és jól tesztelése a következő ugrás szabványos idioms nagyon egyszerű API-t a szolgáltatások kiválasztása.

<a id="connection-string"></a>

## <a name="update-your-connection-string"></a>A kapcsolati karakterlánc frissítése

Most lépjen vissza az Azure portál tooget toohello kapcsolati karakterlánc adatainak és hello alkalmazásba másolja.

1. Kattintson a **gyors üzembe helyezési** a bal oldali navigációs menü hello, és kattintson a **más** tooview hello kapcsolati karakterlánc által kért információkat hello Ugrás alkalmazás.

2. A Goglang nyissa meg a hello main.go fájl hello GOROOT\CosmosDBSample könyvtárban, és frissítse az alábbi kódsorokat hello kapcsolati karakterlánc adatait hello Azure-portál használatával, ahogy az alábbi képernyőfelvétel a hello hello. 

    hello adatbázis nevének megadása hello hello előtagja **állomás** érték hello Azure portál kapcsolati karakterlánc ablaktáblán. Hello fiók hello az alábbi képen látható a hello adatbázis nevének megadása golang-véleményvezérek.

    ```go
    Database: "hello prefix of hello Host value in hello Azure portal",
    Username: "hello Username in hello Azure portal",
    Password: "hello Password in hello Azure portal",
    ```

    ![Gyors üzembe helyezési ablak, más hello Azure portál ábrázoló hello kapcsolati karakterlánc információi lap](./media/create-mongodb-golang/cosmos-db-golang-connection-string.png)

3. Hello main.go fájl mentéséhez.

## <a name="review-hello-code"></a>Tekintse át a hello kódot

Most Meggyőződünk arról, mi történik a hello main.go fájlban gyors áttekintése. 

### <a name="connecting-hello-go-app-tooazure-cosmos-db"></a>Nyissa meg app tooAzure Cosmos DB hello csatlakozás

Azure Cosmos DB hello SSL Protokollt használó MongoDB támogatja. tooconnect tooan MongoDB SSL engedélyezve van, szüksége toodefine hello **DialServer** működni [mgo. DialInfo](http://gopkg.in/mgo.v2#DialInfo), és hogy hello használata [tls. *Telefonos kapcsolat* ](http://golang.org/pkg/crypto/tls#Dial) tooperform hello kapcsolat működik.

a következő kódrészletet Golang hello Azure Cosmos DB MongoDB API hello Ugrás app kapcsolódik. Hello *DialInfo* osztály tartalmaz egy MongoDB fürttel munkamenet létrehozásának beállításai.

```go
// DialInfo holds options for establishing a session with a MongoDB cluster.
dialInfo := &mgo.DialInfo{
    Addrs:    []string{"golang-couch.documents.azure.com:10255"}, // Get HOST + PORT
    Timeout:  60 * time.Second,
    Database: "database", // It can be anything
    Username: "username", // Username
    Password: "Azure database connect password from Azure Portal", // PASSWORD
    DialServer: func(addr *mgo.ServerAddr) (net.Conn, error) {
        return tls.Dial("tcp", addr.String(), &tls.Config{})
    },
}

// Create a session which maintains a pool of socket connections
// tooour Azure Cosmos DB MongoDB database.
session, err := mgo.DialWithInfo(dialInfo)

if err != nil {
    fmt.Printf("Can't connect toomongo, go error %v\n", err)
    os.Exit(1)
}

defer session.Close()

// SetSafe changes hello session safety mode.
// If hello safe parameter is nil, hello session is put in unsafe mode, 
// and writes become fire-and-forget,
// without error checking. hello unsafe mode is faster since operations won't hold on waiting for a confirmation.
// 
session.SetSafe(&mgo.Safe{})
```

Hello **mgo. Dial()** módszert használja, amikor nincs SSL-kapcsolatot. Az SSL-kapcsolat hello **mgo. DialWithInfo()** módszerre szükség.

Hello példányának **DialWIthInfo {}** használt toocreate hello munkamenet-objektumok. Hello munkamenet, miután a hello gyűjteményhez a következő kódrészletet hello segítségével végezheti el:

```go
collection := session.DB(“database”).C(“package”)
```

<a id="create-document"></a>

### <a name="create-a-document"></a>Dokumentum létrehozása

```go
// Model
type Package struct {
    Id bson.ObjectId  `bson:"_id,omitempty"`
    FullName      string
    Description   string
    StarsCount    int
    ForksCount    int
    LastUpdatedBy string
}

// insert Document in collection
err = collection.Insert(&Package{
    FullName:"react",
    Description:"A framework for building native apps with React.",
    ForksCount: 11392,
    StarsCount:48794,
    LastUpdatedBy:"shergin",

})

if err != nil {
    log.Fatal("Problem inserting data: ", err)
    return
}
```

### <a name="query-or-read-a-document"></a>Dokumentum lekérdezése vagy olvasása

Az Azure Cosmos DB támogatja az egyes gyűjteményekben tárolt JSON-dokumentumokon végzett részletes lekérdezéseket. hello alábbi mintakód bemutatja egy lekérdezést, amely alapján futtathatók hello dokumentumok a gyűjteményben.

```go
// Get a Document from hello collection
result := Package{}
err = collection.Find(bson.M{"fullname": "react"}).One(&result)
if err != nil {
    log.Fatal("Error finding record: ", err)
    return
}

fmt.Println("Description:", result.Description)
```


### <a name="update-a-document"></a>Dokumentum frissítése

```go
// Update a document
updateQuery := bson.M{"_id": result.Id}
change := bson.M{"$set": bson.M{"fullname": "react-native"}}
err = collection.Update(updateQuery, change)
if err != nil {
    log.Fatal("Error updating record: ", err)
    return
}
```

### <a name="delete-a-document"></a>Dokumentum törlése

Az Azure Cosmos DB támogatja a JSON-dokumentumok törlését.

```go
// Delete a document
query := bson.M{"_id": result.Id}
err = collection.Remove(query)
if err != nil {
   log.Fatal("Error deleting record: ", err)
   return
}
```
    
## <a name="run-hello-app"></a>Hello alkalmazás futtatása

1. Goglang, győződjön meg arról, hogy a GOPATH (kategóriában elérhető **fájl**, **beállítások**, **lépjen**, **GOPATH**) adnia hello helyet mely hello gopkg lett telepíti, amely USERPROFILE\go alapértelmezés szerint. 
2. Hello sorokat törölni hello dokumentumot, 91-96, sorok megjegyzéssé, hogy láthatóvá hello dokumentum után hello futó alkalmazást.
3. A Goglangban kattintson a **Run** (Futtatás), majd a **Run 'Build main.go and run'** (A main.go létrehozása és futtatás) parancsra.

    hello app befejeződik, és megjeleníti a létrehozott hello dokumentum hello leírását [dokumentum létrehozása](#create-document).
    
    ```
    Description: A framework for building native apps with React.
    
    Process finished with exit code 0
    ```

    ![Goglang hello kimeneti hello alkalmazás megjelenítése](./media/create-mongodb-golang/goglang-cosmos-db.png)
    
## <a name="review-your-document-in-data-explorer"></a>A dokumentum ellenőrzése az Adatkezelőben

Lépjen vissza az Azure portál toosee toohello adatkezelő a dokumentumot.

1. Kattintson a **adatok kezelővel (előzetes verzió)** hello bal oldali navigációs menü, bontsa ki a **golang-véleményvezérek**, **csomag**, és kattintson a **dokumentumok**. A hello **dokumentumok** lapra, majd hello \_azonosító toodisplay hello dokumentum hello jobb oldali ablaktáblán. 

    ![Adatok megjelenítése az újonnan létrehozott hello dokumentum](./media/create-mongodb-golang/golang-cosmos-db-data-explorer.png)
    
2. Ezután hello dokumentum beágyazott dolgozni, és kattintson a **frissítés** toosave azt. Hello dokumentum törlése, vagy hozzon létre új dokumentumok és lekérdezések is.

## <a name="review-slas-in-hello-azure-portal"></a>Tekintse át a szolgáltatásiszint-szerződések a hello Azure-portálon

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Az erőforrások eltávolítása

Toocontinue toouse az alkalmazás nem fog, ha törli az összes erőforrást hozta létre a gyors üzembe helyezés hello az Azure-portálon az alábbi lépésekkel hello:

1. A hello hello Azure-portálon a bal oldali menüből, kattintson az **erőforráscsoportok** és kattintson a létrehozott hello erőforrás hello nevét. 
2. Az erőforrás csoport lapján kattintson a **törlése**, írja be a hello szövegmező hello erőforrás toodelete hello nevét, és kattintson **törlése**.

## <a name="next-steps"></a>Következő lépések

A gyors üzembe helyezés mér megismerte, hogyan toocreate Azure Cosmos DB fiók és futtatható a Golang app hello API mongodb-protokolltámogatással. További adatok tooyour Cosmos DB fiókot most importálhatja. 

> [!div class="nextstepaction"]
> [Adatok importálása az Azure Cosmos DB a hello MongoDB API](mongodb-migrate.md)
