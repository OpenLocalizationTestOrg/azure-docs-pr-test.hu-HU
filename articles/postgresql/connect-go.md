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
# <a name="azure-database-for-postgresql-use-go-language-tooconnect-and-query-data"></a><span data-ttu-id="44140-103">Azure PostgreSQL-adatbázishoz: nyelvi tooconnect és lekérdezési adatok használata nyissa meg</span><span class="sxs-lookup"><span data-stu-id="44140-103">Azure Database for PostgreSQL: Use Go language tooconnect and query data</span></span>
<span data-ttu-id="44140-104">A gyors üzembe helyezés bemutatja, hogyan tooconnect tooan Azure adatbázis PostgreSQL használatára vonatkozó code nyelven írt hello [Ugrás](https://golang.org/) nyelvi (golang).</span><span class="sxs-lookup"><span data-stu-id="44140-104">This quickstart demonstrates how tooconnect tooan Azure Database for PostgreSQL using code written in hello [Go](https://golang.org/) language (golang).</span></span> <span data-ttu-id="44140-105">Azt illusztrálja, hogyan toouse SQL utasítás tooquery beszúrási, frissítési és törlési hello adatbázis adatait.</span><span class="sxs-lookup"><span data-stu-id="44140-105">It shows how toouse SQL statements tooquery, insert, update, and delete data in hello database.</span></span> <span data-ttu-id="44140-106">Ez a cikk feltételezi, hogy jártas használatával nyissa meg, azonban, hogy új tooworking PostgreSQL az Azure-adatbázissal.</span><span class="sxs-lookup"><span data-stu-id="44140-106">This article assumes you are familiar with development using Go, but that you are new tooworking with Azure Database for PostgreSQL.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="44140-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="44140-107">Prerequisites</span></span>
<span data-ttu-id="44140-108">A gyors üzembe helyezés kiindulási pontként ezek az útmutatók valamelyikével létrehozott hello erőforrást használ:</span><span class="sxs-lookup"><span data-stu-id="44140-108">This quickstart uses hello resources created in either of these guides as a starting point:</span></span>
- [<span data-ttu-id="44140-109">DB létrehozása – portál</span><span class="sxs-lookup"><span data-stu-id="44140-109">Create DB - Portal</span></span>](quickstart-create-server-database-portal.md)
- [<span data-ttu-id="44140-110">DB létrehozása – Azure CLI</span><span class="sxs-lookup"><span data-stu-id="44140-110">Create DB - Azure CLI</span></span>](quickstart-create-server-database-azure-cli.md)

## <a name="install-go-and-pq-connector"></a><span data-ttu-id="44140-111">A Go és a pq-összekötő telepítése</span><span class="sxs-lookup"><span data-stu-id="44140-111">Install Go and pq connector</span></span>
<span data-ttu-id="44140-112">Telepítse [Ugrás](https://golang.org/doc/install) és hello [tiszta Ugrás Postgres illesztőprogram (pq)](https://github.com/lib/pq) a saját számítógépén.</span><span class="sxs-lookup"><span data-stu-id="44140-112">Install [Go](https://golang.org/doc/install) and hello [Pure Go Postgres driver (pq)](https://github.com/lib/pq) on your own machine.</span></span> <span data-ttu-id="44140-113">Attól függően, hogy a platform a hello lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="44140-113">Depending on your platform, follow hello steps:</span></span>

### <a name="windows"></a><span data-ttu-id="44140-114">Windows</span><span class="sxs-lookup"><span data-stu-id="44140-114">Windows</span></span>
1. <span data-ttu-id="44140-115">[Töltse le](https://golang.org/dl/) és nyissa meg a Microsoft Windows toohello szerint telepítse [telepítési utasításokat](https://golang.org/doc/install).</span><span class="sxs-lookup"><span data-stu-id="44140-115">[Download](https://golang.org/dl/) and install Go for Microsoft Windows according toohello [installation instructions](https://golang.org/doc/install).</span></span>
2. <span data-ttu-id="44140-116">Indítsa el a parancssor hello hello start menüből.</span><span class="sxs-lookup"><span data-stu-id="44140-116">Launch hello command prompt from hello start menu.</span></span>
3. <span data-ttu-id="44140-117">Hozzon létre egy mappát a projekt számára, például</span><span class="sxs-lookup"><span data-stu-id="44140-117">Make a folder for your project such.</span></span> <span data-ttu-id="44140-118">`mkdir  %USERPROFILE%\go\src\postgresqlgo`.</span><span class="sxs-lookup"><span data-stu-id="44140-118">`mkdir  %USERPROFILE%\go\src\postgresqlgo`.</span></span>
4. <span data-ttu-id="44140-119">Például módosítsa a könyvtárat hello projekt mappába `cd %USERPROFILE%\go\src\postgresqlgo`.</span><span class="sxs-lookup"><span data-stu-id="44140-119">Change directory into hello project folder, such as `cd %USERPROFILE%\go\src\postgresqlgo`.</span></span>
5. <span data-ttu-id="44140-120">Állítsa be a hello környezeti változó GOPATH kód toopoint toohello forráskönyvtár keresése.</span><span class="sxs-lookup"><span data-stu-id="44140-120">Set hello environment variable for GOPATH toopoint toohello source code directory.</span></span> <span data-ttu-id="44140-121">`set GOPATH=%USERPROFILE%\go`.</span><span class="sxs-lookup"><span data-stu-id="44140-121">`set GOPATH=%USERPROFILE%\go`.</span></span>
6. <span data-ttu-id="44140-122">Telepítse a hello [tiszta Ugrás Postgres illesztőprogram (pq)](https://github.com/lib/pq) hello futtatásával `go get github.com/lib/pq` parancs.</span><span class="sxs-lookup"><span data-stu-id="44140-122">Install hello [Pure Go Postgres driver (pq)](https://github.com/lib/pq) by running hello `go get github.com/lib/pq` command.</span></span>

   <span data-ttu-id="44140-123">Összefoglalva nyissa meg telepítette, akkor a hello parancssorban futtassa az alábbi parancsokat:</span><span class="sxs-lookup"><span data-stu-id="44140-123">In summary, install Go, then run these commands in hello command prompt:</span></span>
   ```cmd
   mkdir  %USERPROFILE%\go\src\postgresqlgo
   cd %USERPROFILE%\go\src\postgresqlgo
   set GOPATH=%USERPROFILE%\go
   go get github.com/lib/pq
   ```

### <a name="linux-ubuntu"></a><span data-ttu-id="44140-124">Linux (Ubuntu)</span><span class="sxs-lookup"><span data-stu-id="44140-124">Linux (Ubuntu)</span></span>
1. <span data-ttu-id="44140-125">Indítsa el a hello Bash rendszerhéjat.</span><span class="sxs-lookup"><span data-stu-id="44140-125">Launch hello Bash shell.</span></span> 
2. <span data-ttu-id="44140-126">Telepítse a Go-t a `sudo apt-get install golang-go` parancs futtatásával.</span><span class="sxs-lookup"><span data-stu-id="44140-126">Install Go by running `sudo apt-get install golang-go`.</span></span>
3. <span data-ttu-id="44140-127">Hozzon létre egy mappát a projekt számára a kezdőkönyvtárban (például `mkdir -p ~/go/src/postgresqlgo/`).</span><span class="sxs-lookup"><span data-stu-id="44140-127">Make a folder for your project in your home directory, such as `mkdir -p ~/go/src/postgresqlgo/`.</span></span>
4. <span data-ttu-id="44140-128">Például módosítsa a könyvtárat hello mappába `cd ~/go/src/postgresqlgo/`.</span><span class="sxs-lookup"><span data-stu-id="44140-128">Change directory into hello folder, such as `cd ~/go/src/postgresqlgo/`.</span></span>
5. <span data-ttu-id="44140-129">Set hello GOPATH környezeti változó toopoint tooa érvényes forrás címtár, például az aktuális otthoni könyvtár mappában nyissa meg.</span><span class="sxs-lookup"><span data-stu-id="44140-129">Set hello GOPATH environment variable toopoint tooa valid source directory, such as your current home directory's go folder.</span></span> <span data-ttu-id="44140-130">Hello rendszerhéjakba, futni `export GOPATH=~/go` tooadd hello GOPATH hello hello aktuális rendszerhéj munkamenetet, nyissa meg könyvtár.</span><span class="sxs-lookup"><span data-stu-id="44140-130">At hello bash shell, run `export GOPATH=~/go` tooadd hello go directory as hello GOPATH for hello current shell session.</span></span>
6. <span data-ttu-id="44140-131">Telepítse a hello [tiszta Ugrás Postgres illesztőprogram (pq)](https://github.com/lib/pq) hello futtatásával `go get github.com/lib/pq` parancs.</span><span class="sxs-lookup"><span data-stu-id="44140-131">Install hello [Pure Go Postgres driver (pq)](https://github.com/lib/pq) by running hello `go get github.com/lib/pq` command.</span></span>

   <span data-ttu-id="44140-132">Összefoglalva, futtassa ezeket a bash-parancsokat:</span><span class="sxs-lookup"><span data-stu-id="44140-132">In summary, run these bash commands:</span></span>
   ```bash
   sudo apt-get install golang-go
   mkdir -p ~/go/src/postgresqlgo/
   cd ~/go/src/postgresqlgo/
   export GOPATH=~/go/
   go get github.com/lib/pq
   ```

### <a name="apple-macos"></a><span data-ttu-id="44140-133">Apple macOS</span><span class="sxs-lookup"><span data-stu-id="44140-133">Apple macOS</span></span>
1. <span data-ttu-id="44140-134">Töltse le és telepítse a Go szerint toohello [telepítési utasításokat](https://golang.org/doc/install) a platformhoz megfelelő.</span><span class="sxs-lookup"><span data-stu-id="44140-134">Download and install Go according toohello [installation instructions](https://golang.org/doc/install)  matching your platform.</span></span> 
2. <span data-ttu-id="44140-135">Indítsa el a hello Bash rendszerhéjat.</span><span class="sxs-lookup"><span data-stu-id="44140-135">Launch hello Bash shell.</span></span> 
3. <span data-ttu-id="44140-136">Hozzon létre egy mappát a projekt számára a kezdőkönyvtárban (például `mkdir -p ~/go/src/postgresqlgo/`).</span><span class="sxs-lookup"><span data-stu-id="44140-136">Make a folder for your project in your home directory, such as `mkdir -p ~/go/src/postgresqlgo/`.</span></span>
4. <span data-ttu-id="44140-137">Például módosítsa a könyvtárat hello mappába `cd ~/go/src/postgresqlgo/`.</span><span class="sxs-lookup"><span data-stu-id="44140-137">Change directory into hello folder, such as `cd ~/go/src/postgresqlgo/`.</span></span>
5. <span data-ttu-id="44140-138">Set hello GOPATH környezeti változó toopoint tooa érvényes forrás címtár, például az aktuális otthoni könyvtár mappában nyissa meg.</span><span class="sxs-lookup"><span data-stu-id="44140-138">Set hello GOPATH environment variable toopoint tooa valid source directory, such as your current home directory's go folder.</span></span> <span data-ttu-id="44140-139">Hello rendszerhéjakba, futni `export GOPATH=~/go` tooadd hello GOPATH hello hello aktuális rendszerhéj munkamenetet, nyissa meg könyvtár.</span><span class="sxs-lookup"><span data-stu-id="44140-139">At hello bash shell, run `export GOPATH=~/go` tooadd hello go directory as hello GOPATH for hello current shell session.</span></span>
6. <span data-ttu-id="44140-140">Telepítse a hello [tiszta Ugrás Postgres illesztőprogram (pq)](https://github.com/lib/pq) hello futtatásával `go get github.com/lib/pq` parancs.</span><span class="sxs-lookup"><span data-stu-id="44140-140">Install hello [Pure Go Postgres driver (pq)](https://github.com/lib/pq) by running hello `go get github.com/lib/pq` command.</span></span>

   <span data-ttu-id="44140-141">Összefoglalva, telepítse a Go-t, majd futtassa ezeket a bash-parancsokat:</span><span class="sxs-lookup"><span data-stu-id="44140-141">In summary, install Go, then run these bash commands:</span></span>
   ```bash
   mkdir -p ~/go/src/postgresqlgo/
   cd ~/go/src/postgresqlgo/
   export GOPATH=~/go/
   go get github.com/lib/pq
   ```

## <a name="get-connection-information"></a><span data-ttu-id="44140-142">Kapcsolatadatok lekérése</span><span class="sxs-lookup"><span data-stu-id="44140-142">Get connection information</span></span>
<span data-ttu-id="44140-143">Hello kapcsolat szükséges információkat tooconnect toohello Azure adatbázis beolvasása PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="44140-143">Get hello connection information needed tooconnect toohello Azure Database for PostgreSQL.</span></span> <span data-ttu-id="44140-144">Teljesen minősített kiszolgáló nevét és a bejelentkezési hitelesítő adatokat hello van szüksége.</span><span class="sxs-lookup"><span data-stu-id="44140-144">You need hello fully qualified server name and login credentials.</span></span>

1. <span data-ttu-id="44140-145">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="44140-145">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="44140-146">A hello Azure-portálon a bal oldali menüből, kattintson az **összes erőforrás** , és keressen a létrehozott, például a hello server **mypgserver-20170401**.</span><span class="sxs-lookup"><span data-stu-id="44140-146">From hello left-hand menu in Azure portal, click **All resources** and search for hello server you have created, such as **mypgserver-20170401**.</span></span>
3. <span data-ttu-id="44140-147">Hello kiszolgáló nevére kattint **mypgserver-20170401**.</span><span class="sxs-lookup"><span data-stu-id="44140-147">Click hello server name **mypgserver-20170401**.</span></span>
4. <span data-ttu-id="44140-148">Jelölje be hello server **áttekintése** lap.</span><span class="sxs-lookup"><span data-stu-id="44140-148">Select hello server's **Overview** page.</span></span> <span data-ttu-id="44140-149">Jegyezze fel a hello **kiszolgálónév** és **kiszolgálói rendszergazda bejelentkezési név**.</span><span class="sxs-lookup"><span data-stu-id="44140-149">Make a note of hello **Server name** and **Server admin login name**.</span></span>
 <span data-ttu-id="44140-150">![PostgreSQL-hez készült Azure-adatbázis – Kiszolgáló-rendszergazdai bejelentkezés](./media/connect-go/1-connection-string.png)</span><span class="sxs-lookup"><span data-stu-id="44140-150">![Azure Database for PostgreSQL - Server Admin Login](./media/connect-go/1-connection-string.png)</span></span>
5. <span data-ttu-id="44140-151">Ha elfelejti a kiszolgálói bejelentkezési adatok, keresse meg a toohello **áttekintése** lap, és a nézet hello rendszergazda bejelentkezési nevet.</span><span class="sxs-lookup"><span data-stu-id="44140-151">If you forget your server login information, navigate toohello **Overview** page, and view hello Server admin login name.</span></span> <span data-ttu-id="44140-152">Ha szükséges, jelszó-átállítási hello.</span><span class="sxs-lookup"><span data-stu-id="44140-152">If necessary, reset hello password.</span></span>

## <a name="build-and-run-go-code"></a><span data-ttu-id="44140-153">Go kód felépítése és futtatása</span><span class="sxs-lookup"><span data-stu-id="44140-153">Build and run Go code</span></span> 
1. <span data-ttu-id="44140-154">toowrite Golang kódot, használhatja egy egyszerű szövegszerkesztőben, például a Jegyzettömbben a Microsoft Windows [vi](http://manpages.ubuntu.com/manpages/xenial/man1/nvi.1.html#contenttoc5) vagy [Nano](https://www.nano-editor.org/) Ubuntu, vagy a macOS TextEdit.</span><span class="sxs-lookup"><span data-stu-id="44140-154">toowrite Golang code, you can use a simple text editor, such as Notepad in Microsoft Windows, [vi](http://manpages.ubuntu.com/manpages/xenial/man1/nvi.1.html#contenttoc5) or [Nano](https://www.nano-editor.org/) in Ubuntu, or TextEdit in macOS.</span></span> <span data-ttu-id="44140-155">Ha a funkciógazdagabb interaktív fejlesztési környezeteket (IDE-ket) részesít előnyben, próbálja ki a Jetbrains [Gogland](https://www.jetbrains.com/go/) a Microsoft [Visual Studio Code](https://code.visualstudio.com/) vagy az [Atom](https://atom.io/) eszközt.</span><span class="sxs-lookup"><span data-stu-id="44140-155">If you prefer a richer Interactive Development Environment (IDE) try [Gogland](https://www.jetbrains.com/go/) by Jetbrains, [Visual Studio Code](https://code.visualstudio.com/) by Microsoft, or [Atom](https://atom.io/).</span></span>
2. <span data-ttu-id="44140-156">Hello szakaszokat hello Golang kód beillesztése szövegfájlok, és mentse a projektmappa kiterjesztésű az \*.go, például a Windows útvonalhoz `%USERPROFILE%\go\src\postgresqlgo\createtable.go` vagy Linux elérési `~/go/src/postgresqlgo/createtable.go`.</span><span class="sxs-lookup"><span data-stu-id="44140-156">Paste hello Golang code from hello sections below into text files, and save into your project folder with file extension \*.go, such as Windows path `%USERPROFILE%\go\src\postgresqlgo\createtable.go` or Linux path `~/go/src/postgresqlgo/createtable.go`.</span></span>
3. <span data-ttu-id="44140-157">Keresse meg a hello `HOST`, `DATABASE`, `USER`, és `PASSWORD` konstansok hello kódot, és a név felülírandó hello példaértékeket a saját értékekkel.</span><span class="sxs-lookup"><span data-stu-id="44140-157">Locate hello `HOST`, `DATABASE`, `USER`, and `PASSWORD` constants in hello code, and replace hello example values with your own values.</span></span>  
4. <span data-ttu-id="44140-158">Indítsa el a hello parancssort, vagy a bash rendszerhéjat.</span><span class="sxs-lookup"><span data-stu-id="44140-158">Launch hello command prompt or bash shell.</span></span> <span data-ttu-id="44140-159">Lépjen a projektmappára.</span><span class="sxs-lookup"><span data-stu-id="44140-159">Change directory into your project folder.</span></span> <span data-ttu-id="44140-160">Windows rendszer például a következővel: `cd %USERPROFILE%\go\src\postgresqlgo\`.</span><span class="sxs-lookup"><span data-stu-id="44140-160">For example, on Windows `cd %USERPROFILE%\go\src\postgresqlgo\`.</span></span> <span data-ttu-id="44140-161">Linuxon: `cd ~/go/src/postgresqlgo/`.</span><span class="sxs-lookup"><span data-stu-id="44140-161">On Linux `cd ~/go/src/postgresqlgo/`.</span></span> <span data-ttu-id="44140-162">Hibakeresési és futásidejű képességek anélkül, hogy a rendszerhéj-parancsok kínál néhány említett hello IDE környezetben.</span><span class="sxs-lookup"><span data-stu-id="44140-162">Some of hello IDE environments mentioned offer debug and runtime capabilities without requiring shell commands.</span></span>
5. <span data-ttu-id="44140-163">A kódra hello hello parancs beírásával `go run createtable.go` toocompile hello alkalmazást, és futtassa azt.</span><span class="sxs-lookup"><span data-stu-id="44140-163">Run hello code by typing hello command `go run createtable.go` toocompile hello application and run it.</span></span> 
6. <span data-ttu-id="44140-164">Másik lehetőségként toobuild hello kód natív alkalmazásba `go build createtable.go`, majd indítsa el `createtable.exe` toorun hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="44140-164">Alternatively, toobuild hello code into a native application, `go build createtable.go`, then launch `createtable.exe` toorun hello application.</span></span>

## <a name="connect-and-create-a-table"></a><span data-ttu-id="44140-165">Csatlakozás és tábla létrehozása</span><span class="sxs-lookup"><span data-stu-id="44140-165">Connect and create a table</span></span>
<span data-ttu-id="44140-166">Használjon hello alábbi code tooconnect, és hozzon létre egy táblát az **CREATE TABLE** SQL-utasítást, és **INSERT INTO** SQL utasítás tooadd sorok hello táblába.</span><span class="sxs-lookup"><span data-stu-id="44140-166">Use hello following code tooconnect and create a table using **CREATE TABLE** SQL statement, followed by **INSERT INTO** SQL statements tooadd rows into hello table.</span></span>

<span data-ttu-id="44140-167">hello kód importálja három csomagot: hello [sql csomag](https://golang.org/pkg/database/sql/), hello [pq csomag](http://godoc.org/github.com/lib/pq) , egy illesztőprogram toocommunicate hello Postgres server és a hello [fmt csomag](https://golang.org/pkg/fmt/) a lista tartalmazza bemeneti és kimeneti hello parancssorban.</span><span class="sxs-lookup"><span data-stu-id="44140-167">hello code imports three packages: hello [sql package](https://golang.org/pkg/database/sql/), hello [pq package](http://godoc.org/github.com/lib/pq) as a driver toocommunicate with hello Postgres server, and hello [fmt package](https://golang.org/pkg/fmt/) for printed input and output on hello command line.</span></span>

<span data-ttu-id="44140-168">hello kód metódus meghívja [sql. Open()](http://godoc.org/github.com/lib/pq#Open) tooconnect tooAzure PostgreSQL és ellenőrzések hello kapcsolat metódussal adatbázis [db. Ping()](https://golang.org/pkg/database/sql/#DB.Ping).</span><span class="sxs-lookup"><span data-stu-id="44140-168">hello code calls method [sql.Open()](http://godoc.org/github.com/lib/pq#Open) tooconnect tooAzure Database for PostgreSQL, and checks hello connection using method [db.Ping()](https://golang.org/pkg/database/sql/#DB.Ping).</span></span> <span data-ttu-id="44140-169">A [adatbázis-leíró](https://golang.org/pkg/database/sql/#DB) használja a rendszer keresztül, hello kapcsolatkészlet hello adatbázis-kiszolgáló rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="44140-169">A [database handle](https://golang.org/pkg/database/sql/#DB) is used throughout, holding hello connection pool for hello database server.</span></span> <span data-ttu-id="44140-170">hello kód hívások hello [Exec()](https://golang.org/pkg/database/sql/#DB.Exec) metódus többször toorun számos SQL-parancsokat.</span><span class="sxs-lookup"><span data-stu-id="44140-170">hello code calls hello [Exec()](https://golang.org/pkg/database/sql/#DB.Exec) method several times toorun several SQL commands.</span></span> <span data-ttu-id="44140-171">Minden egyes egy egyéni checkError() metódus toocheck ideje, ha hiba történt, és vészhelyzeti tooexit, ha a hiba akkor fordul elő.</span><span class="sxs-lookup"><span data-stu-id="44140-171">Each time a custom checkError() method toocheck if an error occurred and panic tooexit if an error occurs.</span></span>

<span data-ttu-id="44140-172">Cserélje le a hello `HOST`, `DATABASE`, `USER`, és `PASSWORD` paraméterek saját értékekkel.</span><span class="sxs-lookup"><span data-stu-id="44140-172">Replace hello `HOST`, `DATABASE`, `USER`, and `PASSWORD` parameters with your own values.</span></span> 

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

## <a name="read-data"></a><span data-ttu-id="44140-173">Adatok olvasása</span><span class="sxs-lookup"><span data-stu-id="44140-173">Read data</span></span>
<span data-ttu-id="44140-174">Használjon hello alábbi code tooconnect, és hello adatok segítségével olvassa a **kiválasztása** SQL-utasításban.</span><span class="sxs-lookup"><span data-stu-id="44140-174">Use hello following code tooconnect and read hello data using a **SELECT** SQL statement.</span></span> 

<span data-ttu-id="44140-175">hello kód importálja három csomagot: hello [sql csomag](https://golang.org/pkg/database/sql/), hello [pq csomag](http://godoc.org/github.com/lib/pq) , egy illesztőprogram toocommunicate hello Postgres server és a hello [fmt csomag](https://golang.org/pkg/fmt/) a lista tartalmazza bemeneti és kimeneti hello parancssorban.</span><span class="sxs-lookup"><span data-stu-id="44140-175">hello code imports three packages: hello [sql package](https://golang.org/pkg/database/sql/), hello [pq package](http://godoc.org/github.com/lib/pq) as a driver toocommunicate with hello Postgres server, and hello [fmt package](https://golang.org/pkg/fmt/) for printed input and output on hello command line.</span></span>

<span data-ttu-id="44140-176">hello kód metódus meghívja [sql. Open()](http://godoc.org/github.com/lib/pq#Open) tooconnect tooAzure PostgreSQL és ellenőrzések hello kapcsolat metódussal adatbázis [db. Ping()](https://golang.org/pkg/database/sql/#DB.Ping).</span><span class="sxs-lookup"><span data-stu-id="44140-176">hello code calls method [sql.Open()](http://godoc.org/github.com/lib/pq#Open) tooconnect tooAzure Database for PostgreSQL, and checks hello connection using method [db.Ping()](https://golang.org/pkg/database/sql/#DB.Ping).</span></span> <span data-ttu-id="44140-177">A [adatbázis-leíró](https://golang.org/pkg/database/sql/#DB) használja a rendszer keresztül, hello kapcsolatkészlet hello adatbázis-kiszolgáló rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="44140-177">A [database handle](https://golang.org/pkg/database/sql/#DB) is used throughout, holding hello connection pool for hello database server.</span></span> <span data-ttu-id="44140-178">hello select lekérdezés futtatása metódus meghívásával [db. Query()](https://golang.org/pkg/database/sql/#DB.Query), és eredményül kapott sor hello maradjanak típusú változó [sorok](https://golang.org/pkg/database/sql/#Rows).</span><span class="sxs-lookup"><span data-stu-id="44140-178">hello select query is run by calling method [db.Query()](https://golang.org/pkg/database/sql/#DB.Query), and hello resulting rows is kept in a variable of type [rows](https://golang.org/pkg/database/sql/#Rows).</span></span> <span data-ttu-id="44140-179">hello kód beolvassa hello oszlop adatértékekkel hello aktuális sor metódussal [sorokat. Scan()](https://golang.org/pkg/database/sql/#Rows.Scan) és a sorok hello hello iterátor keresztül hurkok [sorokat. Next()](https://golang.org/pkg/database/sql/#Rows.Next) amíg több sor nem létezik.</span><span class="sxs-lookup"><span data-stu-id="44140-179">hello code reads hello column data values in hello current row using method [rows.Scan()](https://golang.org/pkg/database/sql/#Rows.Scan) and loops over hello rows using hello iterator [rows.Next()](https://golang.org/pkg/database/sql/#Rows.Next) until no more rows exist.</span></span> <span data-ttu-id="44140-180">Minden egyes sorára oszlop értékei nyomtatott toohello konzol ki. Minden egyes egy egyéni checkError() metódus toocheck ideje, ha hiba történt, és vészhelyzeti tooexit, ha a hiba akkor fordul elő.</span><span class="sxs-lookup"><span data-stu-id="44140-180">Each row's column values are printed toohello console out. Each time a custom checkError() method toocheck if an error occurred and panic tooexit if an error occurs.</span></span>

<span data-ttu-id="44140-181">Cserélje le a hello `HOST`, `DATABASE`, `USER`, és `PASSWORD` paraméterek saját értékekkel.</span><span class="sxs-lookup"><span data-stu-id="44140-181">Replace hello `HOST`, `DATABASE`, `USER`, and `PASSWORD` parameters with your own values.</span></span> 

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

## <a name="update-data"></a><span data-ttu-id="44140-182">Adatok frissítése</span><span class="sxs-lookup"><span data-stu-id="44140-182">Update data</span></span>
<span data-ttu-id="44140-183">Használjon hello következő code tooconnect, és frissítse a hello adatok segítségével egy **frissítése** SQL-utasításban.</span><span class="sxs-lookup"><span data-stu-id="44140-183">Use hello following code tooconnect and update hello data using a **UPDATE** SQL statement.</span></span>

<span data-ttu-id="44140-184">hello kód importálja három csomagot: hello [sql csomag](https://golang.org/pkg/database/sql/), hello [pq csomag](http://godoc.org/github.com/lib/pq) , egy illesztőprogram toocommunicate hello Postgres server és a hello [fmt csomag](https://golang.org/pkg/fmt/) a lista tartalmazza bemeneti és kimeneti hello parancssorban.</span><span class="sxs-lookup"><span data-stu-id="44140-184">hello code imports three packages: hello [sql package](https://golang.org/pkg/database/sql/), hello [pq package](http://godoc.org/github.com/lib/pq) as a driver toocommunicate with hello Postgres server, and hello [fmt package](https://golang.org/pkg/fmt/) for printed input and output on hello command line.</span></span>

<span data-ttu-id="44140-185">hello kód metódus meghívja [sql. Open()](http://godoc.org/github.com/lib/pq#Open) tooconnect tooAzure PostgreSQL és ellenőrzések hello kapcsolat metódussal adatbázis [db. Ping()](https://golang.org/pkg/database/sql/#DB.Ping).</span><span class="sxs-lookup"><span data-stu-id="44140-185">hello code calls method [sql.Open()](http://godoc.org/github.com/lib/pq#Open) tooconnect tooAzure Database for PostgreSQL, and checks hello connection using method [db.Ping()](https://golang.org/pkg/database/sql/#DB.Ping).</span></span> <span data-ttu-id="44140-186">A [adatbázis-leíró](https://golang.org/pkg/database/sql/#DB) használja a rendszer keresztül, hello kapcsolatkészlet hello adatbázis-kiszolgáló rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="44140-186">A [database handle](https://golang.org/pkg/database/sql/#DB) is used throughout, holding hello connection pool for hello database server.</span></span> <span data-ttu-id="44140-187">hello kód hívások hello [Exec()](https://golang.org/pkg/database/sql/#DB.Exec) metódus toorun hello hello tábla SQL-utasításban.</span><span class="sxs-lookup"><span data-stu-id="44140-187">hello code calls hello [Exec()](https://golang.org/pkg/database/sql/#DB.Exec) method toorun hello SQL statement that updates hello table.</span></span> <span data-ttu-id="44140-188">Egy egyéni checkError() metódus toocheck hiba történt, és vészhelyzeti tooexit, ha hiba történik.</span><span class="sxs-lookup"><span data-stu-id="44140-188">A custom checkError() method toocheck if an error occurred and panic tooexit if an error occurs.</span></span>

<span data-ttu-id="44140-189">Cserélje le a hello `HOST`, `DATABASE`, `USER`, és `PASSWORD` paraméterek saját értékekkel.</span><span class="sxs-lookup"><span data-stu-id="44140-189">Replace hello `HOST`, `DATABASE`, `USER`, and `PASSWORD` parameters with your own values.</span></span> 
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

## <a name="delete-data"></a><span data-ttu-id="44140-190">Adat törlése</span><span class="sxs-lookup"><span data-stu-id="44140-190">Delete data</span></span>
<span data-ttu-id="44140-191">Használjon hello alábbi code tooconnect, és olvasott hello adatok egy **törlése** SQL-utasításban.</span><span class="sxs-lookup"><span data-stu-id="44140-191">Use hello following code tooconnect and read hello data using a **DELETE** SQL statement.</span></span> 

<span data-ttu-id="44140-192">hello kód importálja három csomagot: hello [sql csomag](https://golang.org/pkg/database/sql/), hello [pq csomag](http://godoc.org/github.com/lib/pq) , egy illesztőprogram toocommunicate hello Postgres server és a hello [fmt csomag](https://golang.org/pkg/fmt/) a lista tartalmazza bemeneti és kimeneti hello parancssorban.</span><span class="sxs-lookup"><span data-stu-id="44140-192">hello code imports three packages: hello [sql package](https://golang.org/pkg/database/sql/), hello [pq package](http://godoc.org/github.com/lib/pq) as a driver toocommunicate with hello Postgres server, and hello [fmt package](https://golang.org/pkg/fmt/) for printed input and output on hello command line.</span></span>

<span data-ttu-id="44140-193">hello kód metódus meghívja [sql. Open()](http://godoc.org/github.com/lib/pq#Open) tooconnect tooAzure PostgreSQL és ellenőrzések hello kapcsolat metódussal adatbázis [db. Ping()](https://golang.org/pkg/database/sql/#DB.Ping).</span><span class="sxs-lookup"><span data-stu-id="44140-193">hello code calls method [sql.Open()](http://godoc.org/github.com/lib/pq#Open) tooconnect tooAzure Database for PostgreSQL, and checks hello connection using method [db.Ping()](https://golang.org/pkg/database/sql/#DB.Ping).</span></span> <span data-ttu-id="44140-194">A [adatbázis-leíró](https://golang.org/pkg/database/sql/#DB) használja a rendszer keresztül, hello kapcsolatkészlet hello adatbázis-kiszolgáló rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="44140-194">A [database handle](https://golang.org/pkg/database/sql/#DB) is used throughout, holding hello connection pool for hello database server.</span></span> <span data-ttu-id="44140-195">hello kód hívások hello [Exec()](https://golang.org/pkg/database/sql/#DB.Exec) metódus toorun hello hello tábla SQL-utasításban.</span><span class="sxs-lookup"><span data-stu-id="44140-195">hello code calls hello [Exec()](https://golang.org/pkg/database/sql/#DB.Exec) method toorun hello SQL statement that updates hello table.</span></span> <span data-ttu-id="44140-196">Egy egyéni checkError() metódus toocheck hiba történt, és vészhelyzeti tooexit, ha hiba történik.</span><span class="sxs-lookup"><span data-stu-id="44140-196">A custom checkError() method toocheck if an error occurred and panic tooexit if an error occurs.</span></span>

<span data-ttu-id="44140-197">Cserélje le a hello `HOST`, `DATABASE`, `USER`, és `PASSWORD` paraméterek saját értékekkel.</span><span class="sxs-lookup"><span data-stu-id="44140-197">Replace hello `HOST`, `DATABASE`, `USER`, and `PASSWORD` parameters with your own values.</span></span> 
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

## <a name="next-steps"></a><span data-ttu-id="44140-198">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="44140-198">Next steps</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="44140-199">Adatbázis migrálása exportálással és importálással</span><span class="sxs-lookup"><span data-stu-id="44140-199">Migrate your database using Export and Import</span></span>](./howto-migrate-using-export-and-import.md)
