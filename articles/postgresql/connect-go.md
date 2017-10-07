---
title: "Nyissa meg nyelv használatával PostgreSQL adatbázis aaaConnect tooAzure |} Microsoft Docs"
description: "A gyors üzembe helyezés Ugrás programozási nyelv minta PostgreSQL lekérdezése a Azure-adatbázis adatait és tooconnect használhatja itt."
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.custom: mvc
ms.devlang: go
ms.topic: quickstart
ms.date: 06/29/2017
ms.openlocfilehash: aa3c93da03116b8fcb54557494dccfad558e5f1d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-database-for-postgresql-use-go-language-tooconnect-and-query-data"></a>Azure PostgreSQL-adatbázishoz: nyelvi tooconnect és lekérdezési adatok használata nyissa meg
A gyors üzembe helyezés bemutatja, hogyan tooconnect tooan Azure adatbázis PostgreSQL használatára vonatkozó code nyelven írt hello [Ugrás](https://golang.org/) nyelvi (golang). Azt illusztrálja, hogyan toouse SQL utasítás tooquery beszúrási, frissítési és törlési hello adatbázis adatait. Ez a cikk feltételezi, hogy jártas használatával nyissa meg, azonban, hogy új tooworking PostgreSQL az Azure-adatbázissal.

## <a name="prerequisites"></a>Előfeltételek
A gyors üzembe helyezés kiindulási pontként ezek az útmutatók valamelyikével létrehozott hello erőforrást használ:
- [DB létrehozása – portál](quickstart-create-server-database-portal.md)
- [DB létrehozása – Azure CLI](quickstart-create-server-database-azure-cli.md)

## <a name="install-go-and-pq-connector"></a>A Go és a pq-összekötő telepítése
Telepítse [Ugrás](https://golang.org/doc/install) és hello [tiszta Ugrás Postgres illesztőprogram (pq)](https://github.com/lib/pq) a saját számítógépén. Attól függően, hogy a platform a hello lépésekkel:

### <a name="windows"></a>Windows
1. [Töltse le](https://golang.org/dl/) és nyissa meg a Microsoft Windows toohello szerint telepítse [telepítési utasításokat](https://golang.org/doc/install).
2. Indítsa el a parancssor hello hello start menüből.
3. Hozzon létre egy mappát a projekt számára, például `mkdir  %USERPROFILE%\go\src\postgresqlgo`.
4. Például módosítsa a könyvtárat hello projekt mappába `cd %USERPROFILE%\go\src\postgresqlgo`.
5. Állítsa be a hello környezeti változó GOPATH kód toopoint toohello forráskönyvtár keresése. `set GOPATH=%USERPROFILE%\go`.
6. Telepítse a hello [tiszta Ugrás Postgres illesztőprogram (pq)](https://github.com/lib/pq) hello futtatásával `go get github.com/lib/pq` parancs.

   Összefoglalva nyissa meg telepítette, akkor a hello parancssorban futtassa az alábbi parancsokat:
   ```cmd
   mkdir  %USERPROFILE%\go\src\postgresqlgo
   cd %USERPROFILE%\go\src\postgresqlgo
   set GOPATH=%USERPROFILE%\go
   go get github.com/lib/pq
   ```

### <a name="linux-ubuntu"></a>Linux (Ubuntu)
1. Indítsa el a hello Bash rendszerhéjat. 
2. Telepítse a Go-t a `sudo apt-get install golang-go` parancs futtatásával.
3. Hozzon létre egy mappát a projekt számára a kezdőkönyvtárban (például `mkdir -p ~/go/src/postgresqlgo/`).
4. Például módosítsa a könyvtárat hello mappába `cd ~/go/src/postgresqlgo/`.
5. Set hello GOPATH környezeti változó toopoint tooa érvényes forrás címtár, például az aktuális otthoni könyvtár mappában nyissa meg. Hello rendszerhéjakba, futni `export GOPATH=~/go` tooadd hello GOPATH hello hello aktuális rendszerhéj munkamenetet, nyissa meg könyvtár.
6. Telepítse a hello [tiszta Ugrás Postgres illesztőprogram (pq)](https://github.com/lib/pq) hello futtatásával `go get github.com/lib/pq` parancs.

   Összefoglalva, futtassa ezeket a bash-parancsokat:
   ```bash
   sudo apt-get install golang-go
   mkdir -p ~/go/src/postgresqlgo/
   cd ~/go/src/postgresqlgo/
   export GOPATH=~/go/
   go get github.com/lib/pq
   ```

### <a name="apple-macos"></a>Apple macOS
1. Töltse le és telepítse a Go szerint toohello [telepítési utasításokat](https://golang.org/doc/install) a platformhoz megfelelő. 
2. Indítsa el a hello Bash rendszerhéjat. 
3. Hozzon létre egy mappát a projekt számára a kezdőkönyvtárban (például `mkdir -p ~/go/src/postgresqlgo/`).
4. Például módosítsa a könyvtárat hello mappába `cd ~/go/src/postgresqlgo/`.
5. Set hello GOPATH környezeti változó toopoint tooa érvényes forrás címtár, például az aktuális otthoni könyvtár mappában nyissa meg. Hello rendszerhéjakba, futni `export GOPATH=~/go` tooadd hello GOPATH hello hello aktuális rendszerhéj munkamenetet, nyissa meg könyvtár.
6. Telepítse a hello [tiszta Ugrás Postgres illesztőprogram (pq)](https://github.com/lib/pq) hello futtatásával `go get github.com/lib/pq` parancs.

   Összefoglalva, telepítse a Go-t, majd futtassa ezeket a bash-parancsokat:
   ```bash
   mkdir -p ~/go/src/postgresqlgo/
   cd ~/go/src/postgresqlgo/
   export GOPATH=~/go/
   go get github.com/lib/pq
   ```

## <a name="get-connection-information"></a>Kapcsolatadatok lekérése
Hello kapcsolat szükséges információkat tooconnect toohello Azure adatbázis beolvasása PostgreSQL. Teljesen minősített kiszolgáló nevét és a bejelentkezési hitelesítő adatokat hello van szüksége.

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).
2. A hello Azure-portálon a bal oldali menüből, kattintson az **összes erőforrás** , és keressen a létrehozott, például a hello server **mypgserver-20170401**.
3. Hello kiszolgáló nevére kattint **mypgserver-20170401**.
4. Jelölje be hello server **áttekintése** lap. Jegyezze fel a hello **kiszolgálónév** és **kiszolgálói rendszergazda bejelentkezési név**.
 ![PostgreSQL-hez készült Azure-adatbázis – Kiszolgáló-rendszergazdai bejelentkezés](./media/connect-go/1-connection-string.png)
5. Ha elfelejti a kiszolgálói bejelentkezési adatok, keresse meg a toohello **áttekintése** lap, és a nézet hello rendszergazda bejelentkezési nevet. Ha szükséges, jelszó-átállítási hello.

## <a name="build-and-run-go-code"></a>Go kód felépítése és futtatása 
1. toowrite Golang kódot, használhatja egy egyszerű szövegszerkesztőben, például a Jegyzettömbben a Microsoft Windows [vi](http://manpages.ubuntu.com/manpages/xenial/man1/nvi.1.html#contenttoc5) vagy [Nano](https://www.nano-editor.org/) Ubuntu, vagy a macOS TextEdit. Ha a funkciógazdagabb interaktív fejlesztési környezeteket (IDE-ket) részesít előnyben, próbálja ki a Jetbrains [Gogland](https://www.jetbrains.com/go/) a Microsoft [Visual Studio Code](https://code.visualstudio.com/) vagy az [Atom](https://atom.io/) eszközt.
2. Hello szakaszokat hello Golang kód beillesztése szövegfájlok, és mentse a projektmappa kiterjesztésű az \*.go, például a Windows útvonalhoz `%USERPROFILE%\go\src\postgresqlgo\createtable.go` vagy Linux elérési `~/go/src/postgresqlgo/createtable.go`.
3. Keresse meg a hello `HOST`, `DATABASE`, `USER`, és `PASSWORD` konstansok hello kódot, és a név felülírandó hello példaértékeket a saját értékekkel.  
4. Indítsa el a hello parancssort, vagy a bash rendszerhéjat. Lépjen a projektmappára. Windows rendszer például a következővel: `cd %USERPROFILE%\go\src\postgresqlgo\`. Linuxon: `cd ~/go/src/postgresqlgo/`. Hibakeresési és futásidejű képességek anélkül, hogy a rendszerhéj-parancsok kínál néhány említett hello IDE környezetben.
5. A kódra hello hello parancs beírásával `go run createtable.go` toocompile hello alkalmazást, és futtassa azt. 
6. Másik lehetőségként toobuild hello kód natív alkalmazásba `go build createtable.go`, majd indítsa el `createtable.exe` toorun hello alkalmazás.

## <a name="connect-and-create-a-table"></a>Csatlakozás és tábla létrehozása
Használjon hello alábbi code tooconnect, és hozzon létre egy táblát az **CREATE TABLE** SQL-utasítást, és **INSERT INTO** SQL utasítás tooadd sorok hello táblába.

hello kód importálja három csomagot: hello [sql csomag](https://golang.org/pkg/database/sql/), hello [pq csomag](http://godoc.org/github.com/lib/pq) , egy illesztőprogram toocommunicate hello Postgres server és a hello [fmt csomag](https://golang.org/pkg/fmt/) a lista tartalmazza bemeneti és kimeneti hello parancssorban.

hello kód metódus meghívja [sql. Open()](http://godoc.org/github.com/lib/pq#Open) tooconnect tooAzure PostgreSQL és ellenőrzések hello kapcsolat metódussal adatbázis [db. Ping()](https://golang.org/pkg/database/sql/#DB.Ping). A [adatbázis-leíró](https://golang.org/pkg/database/sql/#DB) használja a rendszer keresztül, hello kapcsolatkészlet hello adatbázis-kiszolgáló rendelkezik. hello kód hívások hello [Exec()](https://golang.org/pkg/database/sql/#DB.Exec) metódus többször toorun számos SQL-parancsokat. Minden egyes egy egyéni checkError() metódus toocheck ideje, ha hiba történt, és vészhelyzeti tooexit, ha a hiba akkor fordul elő.

Cserélje le a hello `HOST`, `DATABASE`, `USER`, és `PASSWORD` paraméterek saját értékekkel. 

```go
package main

import (
    "database/sql"
    "fmt"
    _ "github.com/lib/pq"
)

const (
    // Initialize connection constants.
    HOST     = "mypgserver-20170401.postgres.database.azure.com"
    DATABASE = "mypgsqldb"
    USER     = "mylogin@mypgserver-20170401"
    PASSWORD = "<server_admin_password>"
)

func checkError(err error) {
    if err != nil {
        panic(err)
    }
}

func main() {
    // Initialize connection string.
    var connectionString string = fmt.Sprintf("host=%s user=%s password=%s dbname=%s sslmode=require", HOST, USER, PASSWORD, DATABASE)

    // Initialize connection object.
    db, err := sql.Open("postgres", connectionString)
    checkError(err)

    err = db.Ping()
    checkError(err)
    fmt.Println("Successfully created connection toodatabase")

    // Drop previous table of same name if one exists.
    _, err = db.Exec("DROP TABLE IF EXISTS inventory;")
    checkError(err)
    fmt.Println("Finished dropping table (if existed)")

    // Create table.
    _, err = db.Exec("CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);")
    checkError(err)
    fmt.Println("Finished creating table")

    // Insert some data into table.
    sql_statement := "INSERT INTO inventory (name, quantity) VALUES ($1, $2);"
    _, err = db.Exec(sql_statement, "banana", 150)
    checkError(err)
    _, err = db.Exec(sql_statement, "orange", 154)
    checkError(err)
    _, err = db.Exec(sql_statement, "apple", 100)
    checkError(err)
    fmt.Println("Inserted 3 rows of data")
}
```

## <a name="read-data"></a>Adatok olvasása
Használjon hello alábbi code tooconnect, és hello adatok segítségével olvassa a **kiválasztása** SQL-utasításban. 

hello kód importálja három csomagot: hello [sql csomag](https://golang.org/pkg/database/sql/), hello [pq csomag](http://godoc.org/github.com/lib/pq) , egy illesztőprogram toocommunicate hello Postgres server és a hello [fmt csomag](https://golang.org/pkg/fmt/) a lista tartalmazza bemeneti és kimeneti hello parancssorban.

hello kód metódus meghívja [sql. Open()](http://godoc.org/github.com/lib/pq#Open) tooconnect tooAzure PostgreSQL és ellenőrzések hello kapcsolat metódussal adatbázis [db. Ping()](https://golang.org/pkg/database/sql/#DB.Ping). A [adatbázis-leíró](https://golang.org/pkg/database/sql/#DB) használja a rendszer keresztül, hello kapcsolatkészlet hello adatbázis-kiszolgáló rendelkezik. hello select lekérdezés futtatása metódus meghívásával [db. Query()](https://golang.org/pkg/database/sql/#DB.Query), és eredményül kapott sor hello maradjanak típusú változó [sorok](https://golang.org/pkg/database/sql/#Rows). hello kód beolvassa hello oszlop adatértékekkel hello aktuális sor metódussal [sorokat. Scan()](https://golang.org/pkg/database/sql/#Rows.Scan) és a sorok hello hello iterátor keresztül hurkok [sorokat. Next()](https://golang.org/pkg/database/sql/#Rows.Next) amíg több sor nem létezik. Minden egyes sorára oszlop értékei nyomtatott toohello konzol ki. Minden egyes egy egyéni checkError() metódus toocheck ideje, ha hiba történt, és vészhelyzeti tooexit, ha a hiba akkor fordul elő.

Cserélje le a hello `HOST`, `DATABASE`, `USER`, és `PASSWORD` paraméterek saját értékekkel. 

```go
package main

import (
    "database/sql"
    "fmt"
    _ "github.com/lib/pq"
)

const (
    // Initialize connection constants.
    HOST     = "mypgserver-20170401.postgres.database.azure.com"
    DATABASE = "mypgsqldb"
    USER     = "mylogin@mypgserver-20170401"
    PASSWORD = "<server_admin_password>"
)

func checkError(err error) {
    if err != nil {
        panic(err)
    }
}

func main() {

    // Initialize connection string.
    var connectionString string = fmt.Sprintf("host=%s user=%s password=%s dbname=%s sslmode=require", HOST, USER, PASSWORD, DATABASE)

    // Initialize connection object.
    db, err := sql.Open("postgres", connectionString)
    checkError(err)

    err = db.Ping()
    checkError(err)
    fmt.Println("Successfully created connection toodatabase")

    // Read rows from table.
    var id int
    var name string
    var quantity int

    sql_statement := "SELECT * from inventory;"
    rows, err := db.Query(sql_statement)
    checkError(err)

    for rows.Next() {
        switch err := rows.Scan(&id, &name, &quantity); err {
        case sql.ErrNoRows:
            fmt.Println("No rows were returned")
        case nil:
            fmt.Printf("Data row = (%d, %s, %d)\n", id, name, quantity)
        default:
            checkError(err)
        }
    }
}
```

## <a name="update-data"></a>Adatok frissítése
Használjon hello következő code tooconnect, és frissítse a hello adatok segítségével egy **frissítése** SQL-utasításban.

hello kód importálja három csomagot: hello [sql csomag](https://golang.org/pkg/database/sql/), hello [pq csomag](http://godoc.org/github.com/lib/pq) , egy illesztőprogram toocommunicate hello Postgres server és a hello [fmt csomag](https://golang.org/pkg/fmt/) a lista tartalmazza bemeneti és kimeneti hello parancssorban.

hello kód metódus meghívja [sql. Open()](http://godoc.org/github.com/lib/pq#Open) tooconnect tooAzure PostgreSQL és ellenőrzések hello kapcsolat metódussal adatbázis [db. Ping()](https://golang.org/pkg/database/sql/#DB.Ping). A [adatbázis-leíró](https://golang.org/pkg/database/sql/#DB) használja a rendszer keresztül, hello kapcsolatkészlet hello adatbázis-kiszolgáló rendelkezik. hello kód hívások hello [Exec()](https://golang.org/pkg/database/sql/#DB.Exec) metódus toorun hello hello tábla SQL-utasításban. Egy egyéni checkError() metódus toocheck hiba történt, és vészhelyzeti tooexit, ha hiba történik.

Cserélje le a hello `HOST`, `DATABASE`, `USER`, és `PASSWORD` paraméterek saját értékekkel. 
```go
package main

import (
  "database/sql"
  _ "github.com/lib/pq"
  "fmt"
)

const (
    // Initialize connection constants.
    HOST     = "mypgserver-20170401.postgres.database.azure.com"
    DATABASE = "mypgsqldb"
    USER     = "mylogin@mypgserver-20170401"
    PASSWORD = "<server_admin_password>"
)

func checkError(err error) {
    if err != nil {
        panic(err)
    }
}

func main() {
    
    // Initialize connection string.
    var connectionString string = 
        fmt.Sprintf("host=%s user=%s password=%s dbname=%s sslmode=require", HOST, USER, PASSWORD, DATABASE)

    // Initialize connection object.
    db, err := sql.Open("postgres", connectionString)
    checkError(err)

    err = db.Ping()
    checkError(err)
    fmt.Println("Successfully created connection toodatabase")

    // Modify some data in table.
    sql_statement := "UPDATE inventory SET quantity = $2 WHERE name = $1;"
    _, err = db.Exec(sql_statement, "banana", 200)
    checkError(err)
    fmt.Println("Updated 1 row of data")
}
```

## <a name="delete-data"></a>Adat törlése
Használjon hello alábbi code tooconnect, és olvasott hello adatok egy **törlése** SQL-utasításban. 

hello kód importálja három csomagot: hello [sql csomag](https://golang.org/pkg/database/sql/), hello [pq csomag](http://godoc.org/github.com/lib/pq) , egy illesztőprogram toocommunicate hello Postgres server és a hello [fmt csomag](https://golang.org/pkg/fmt/) a lista tartalmazza bemeneti és kimeneti hello parancssorban.

hello kód metódus meghívja [sql. Open()](http://godoc.org/github.com/lib/pq#Open) tooconnect tooAzure PostgreSQL és ellenőrzések hello kapcsolat metódussal adatbázis [db. Ping()](https://golang.org/pkg/database/sql/#DB.Ping). A [adatbázis-leíró](https://golang.org/pkg/database/sql/#DB) használja a rendszer keresztül, hello kapcsolatkészlet hello adatbázis-kiszolgáló rendelkezik. hello kód hívások hello [Exec()](https://golang.org/pkg/database/sql/#DB.Exec) metódus toorun hello hello tábla SQL-utasításban. Egy egyéni checkError() metódus toocheck hiba történt, és vészhelyzeti tooexit, ha hiba történik.

Cserélje le a hello `HOST`, `DATABASE`, `USER`, és `PASSWORD` paraméterek saját értékekkel. 
```go
package main

import (
  "database/sql"
  _ "github.com/lib/pq"
  "fmt"
)

const (
    // Initialize connection constants.
    HOST     = "mypgserver-20170401.postgres.database.azure.com"
    DATABASE = "mypgsqldb"
    USER     = "mylogin@mypgserver-20170401"
    PASSWORD = "<server_admin_password>"
)

func checkError(err error) {
    if err != nil {
        panic(err)
    }
}

func main() {
    
    // Initialize connection string.
    var connectionString string = 
        fmt.Sprintf("host=%s user=%s password=%s dbname=%s sslmode=require", HOST, USER, PASSWORD, DATABASE)

    // Initialize connection object.
    db, err := sql.Open("postgres", connectionString)
    checkError(err)

    err = db.Ping()
    checkError(err)
    fmt.Println("Successfully created connection toodatabase")

    // Delete some data from table.
    sql_statement := "DELETE FROM inventory WHERE name = $1;"
    _, err = db.Exec(sql_statement, "orange")
    checkError(err)
    fmt.Println("Deleted 1 row of data")
}
```

## <a name="next-steps"></a>Következő lépések
> [!div class="nextstepaction"]
> [Adatbázis migrálása exportálással és importálással](./howto-migrate-using-export-and-import.md)
