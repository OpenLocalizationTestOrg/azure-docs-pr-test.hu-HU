---
title: "Csatlakozás PostgreSQL-hez készült Azure-adatbázishoz a Go nyelv használatával | Microsoft Docs"
description: "Ez a rövid útmutató egy Go programozási nyelven írt mintát biztosít, amellyel csatlakozhat a PostgreSQL-hez készült Azure-adatbázishoz, és adatokat is lekérdezhet róla."
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
ms.openlocfilehash: a7555464879826c5e4f55929d23163b002664e81
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="azure-database-for-postgresql-use-go-language-to-connect-and-query-data"></a><span data-ttu-id="f056a-103">PostgreSQL-hez készült Azure-adatbázis: Csatlakozás és adatok lekérdezése a Go nyelv használatával</span><span class="sxs-lookup"><span data-stu-id="f056a-103">Azure Database for PostgreSQL: Use Go language to connect and query data</span></span>
<span data-ttu-id="f056a-104">Ez a rövid útmutató azt ismerteti, hogyan lehet csatlakozni a PostgreSQL-hez készült Azure-adatbázishoz [Go](https://golang.org/) nyelven írt kóddal (golang).</span><span class="sxs-lookup"><span data-stu-id="f056a-104">This quickstart demonstrates how to connect to an Azure Database for PostgreSQL using code written in the [Go](https://golang.org/) language (golang).</span></span> <span data-ttu-id="f056a-105">Bemutatjuk, hogy az SQL-utasítások használatával hogyan kérdezhetők le, illeszthetők be, frissíthetők és törölhetők az adatok az adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="f056a-105">It shows how to use SQL statements to query, insert, update, and delete data in the database.</span></span> <span data-ttu-id="f056a-106">A cikk feltételezi, hogy Ön ismeri a Go-t használó fejlesztéseket, de még járatlan a PostgreSQL-hez készült Azure-adatbázis használatában.</span><span class="sxs-lookup"><span data-stu-id="f056a-106">This article assumes you are familiar with development using Go, but that you are new to working with Azure Database for PostgreSQL.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f056a-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="f056a-107">Prerequisites</span></span>
<span data-ttu-id="f056a-108">A rövid útmutató az alábbi útmutatók valamelyikében létrehozott erőforrásokat használja kiindulópontként:</span><span class="sxs-lookup"><span data-stu-id="f056a-108">This quickstart uses the resources created in either of these guides as a starting point:</span></span>
- [<span data-ttu-id="f056a-109">DB létrehozása – portál</span><span class="sxs-lookup"><span data-stu-id="f056a-109">Create DB - Portal</span></span>](quickstart-create-server-database-portal.md)
- [<span data-ttu-id="f056a-110">DB létrehozása – Azure CLI</span><span class="sxs-lookup"><span data-stu-id="f056a-110">Create DB - Azure CLI</span></span>](quickstart-create-server-database-azure-cli.md)

## <a name="install-go-and-pq-connector"></a><span data-ttu-id="f056a-111">A Go és a pq-összekötő telepítése</span><span class="sxs-lookup"><span data-stu-id="f056a-111">Install Go and pq connector</span></span>
<span data-ttu-id="f056a-112">Telepítse a [Go](https://golang.org/doc/install)-t és a [Pure Go Postgres illesztőprogramot (pq)](https://github.com/lib/pq) a saját számítógépére.</span><span class="sxs-lookup"><span data-stu-id="f056a-112">Install [Go](https://golang.org/doc/install) and the [Pure Go Postgres driver (pq)](https://github.com/lib/pq) on your own machine.</span></span> <span data-ttu-id="f056a-113">Kövesse az egyes platformoknak megfelelő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="f056a-113">Depending on your platform, follow the steps:</span></span>

### <a name="windows"></a><span data-ttu-id="f056a-114">Windows</span><span class="sxs-lookup"><span data-stu-id="f056a-114">Windows</span></span>
1. <span data-ttu-id="f056a-115">[Töltse le](https://golang.org/dl/) és telepítse a Microsoft Windowshoz készült Go-t a [telepítési utasítások](https://golang.org/doc/install) szerint.</span><span class="sxs-lookup"><span data-stu-id="f056a-115">[Download](https://golang.org/dl/) and install Go for Microsoft Windows according to the [installation instructions](https://golang.org/doc/install).</span></span>
2. <span data-ttu-id="f056a-116">Nyissa meg a parancssort a Start menüből.</span><span class="sxs-lookup"><span data-stu-id="f056a-116">Launch the command prompt from the start menu.</span></span>
3. <span data-ttu-id="f056a-117">Hozzon létre egy mappát a projekt számára, például</span><span class="sxs-lookup"><span data-stu-id="f056a-117">Make a folder for your project such.</span></span> <span data-ttu-id="f056a-118">`mkdir  %USERPROFILE%\go\src\postgresqlgo`.</span><span class="sxs-lookup"><span data-stu-id="f056a-118">`mkdir  %USERPROFILE%\go\src\postgresqlgo`.</span></span>
4. <span data-ttu-id="f056a-119">Nyissa meg a projektmappát (például `cd %USERPROFILE%\go\src\postgresqlgo`).</span><span class="sxs-lookup"><span data-stu-id="f056a-119">Change directory into the project folder, such as `cd %USERPROFILE%\go\src\postgresqlgo`.</span></span>
5. <span data-ttu-id="f056a-120">Úgy állítsa be a GOPATH környezeti változóját, hogy a forráskód könyvtárára mutasson.</span><span class="sxs-lookup"><span data-stu-id="f056a-120">Set the environment variable for GOPATH to point to the source code directory.</span></span> <span data-ttu-id="f056a-121">`set GOPATH=%USERPROFILE%\go`.</span><span class="sxs-lookup"><span data-stu-id="f056a-121">`set GOPATH=%USERPROFILE%\go`.</span></span>
6. <span data-ttu-id="f056a-122">Telepítse a [Pure Go Postgres illesztőt (pq)](https://github.com/lib/pq) a `go get github.com/lib/pq` parancs futtatásával.</span><span class="sxs-lookup"><span data-stu-id="f056a-122">Install the [Pure Go Postgres driver (pq)](https://github.com/lib/pq) by running the `go get github.com/lib/pq` command.</span></span>

   <span data-ttu-id="f056a-123">Összefoglalva, telepítse a Go-t, majd futtassa ezeket a parancsokat a parancssorban:</span><span class="sxs-lookup"><span data-stu-id="f056a-123">In summary, install Go, then run these commands in the command prompt:</span></span>
   ```cmd
   mkdir  %USERPROFILE%\go\src\postgresqlgo
   cd %USERPROFILE%\go\src\postgresqlgo
   set GOPATH=%USERPROFILE%\go
   go get github.com/lib/pq
   ```

### <a name="linux-ubuntu"></a><span data-ttu-id="f056a-124">Linux (Ubuntu)</span><span class="sxs-lookup"><span data-stu-id="f056a-124">Linux (Ubuntu)</span></span>
1. <span data-ttu-id="f056a-125">Indítsa el a Bash felületet.</span><span class="sxs-lookup"><span data-stu-id="f056a-125">Launch the Bash shell.</span></span> 
2. <span data-ttu-id="f056a-126">Telepítse a Go-t a `sudo apt-get install golang-go` parancs futtatásával.</span><span class="sxs-lookup"><span data-stu-id="f056a-126">Install Go by running `sudo apt-get install golang-go`.</span></span>
3. <span data-ttu-id="f056a-127">Hozzon létre egy mappát a projekt számára a kezdőkönyvtárban (például `mkdir -p ~/go/src/postgresqlgo/`).</span><span class="sxs-lookup"><span data-stu-id="f056a-127">Make a folder for your project in your home directory, such as `mkdir -p ~/go/src/postgresqlgo/`.</span></span>
4. <span data-ttu-id="f056a-128">Nyissa meg a projektmappát (például `cd ~/go/src/postgresqlgo/`).</span><span class="sxs-lookup"><span data-stu-id="f056a-128">Change directory into the folder, such as `cd ~/go/src/postgresqlgo/`.</span></span>
5. <span data-ttu-id="f056a-129">Úgy állítsa be a GOPATH környezeti változót, hogy egy érvényes forráskönyvtárra mutasson, például az aktuális kezdőkönyvtár Go mappájára.</span><span class="sxs-lookup"><span data-stu-id="f056a-129">Set the GOPATH environment variable to point to a valid source directory, such as your current home directory's go folder.</span></span> <span data-ttu-id="f056a-130">A bash felületen futtassa az `export GOPATH=~/go` parancsot, amellyel a Go könyvtárát GOPATH útvonalként adhatja meg az aktuális felületi munkamenetre.</span><span class="sxs-lookup"><span data-stu-id="f056a-130">At the bash shell, run `export GOPATH=~/go` to add the go directory as the GOPATH for the current shell session.</span></span>
6. <span data-ttu-id="f056a-131">Telepítse a [Pure Go Postgres illesztőt (pq)](https://github.com/lib/pq) a `go get github.com/lib/pq` parancs futtatásával.</span><span class="sxs-lookup"><span data-stu-id="f056a-131">Install the [Pure Go Postgres driver (pq)](https://github.com/lib/pq) by running the `go get github.com/lib/pq` command.</span></span>

   <span data-ttu-id="f056a-132">Összefoglalva, futtassa ezeket a bash-parancsokat:</span><span class="sxs-lookup"><span data-stu-id="f056a-132">In summary, run these bash commands:</span></span>
   ```bash
   sudo apt-get install golang-go
   mkdir -p ~/go/src/postgresqlgo/
   cd ~/go/src/postgresqlgo/
   export GOPATH=~/go/
   go get github.com/lib/pq
   ```

### <a name="apple-macos"></a><span data-ttu-id="f056a-133">Apple macOS</span><span class="sxs-lookup"><span data-stu-id="f056a-133">Apple macOS</span></span>
1. <span data-ttu-id="f056a-134">Töltse le és telepítse a Go-t a platformjának megfelelő [telepítési utasítások](https://golang.org/doc/install) szerint.</span><span class="sxs-lookup"><span data-stu-id="f056a-134">Download and install Go according to the [installation instructions](https://golang.org/doc/install)  matching your platform.</span></span> 
2. <span data-ttu-id="f056a-135">Indítsa el a Bash felületet.</span><span class="sxs-lookup"><span data-stu-id="f056a-135">Launch the Bash shell.</span></span> 
3. <span data-ttu-id="f056a-136">Hozzon létre egy mappát a projekt számára a kezdőkönyvtárban (például `mkdir -p ~/go/src/postgresqlgo/`).</span><span class="sxs-lookup"><span data-stu-id="f056a-136">Make a folder for your project in your home directory, such as `mkdir -p ~/go/src/postgresqlgo/`.</span></span>
4. <span data-ttu-id="f056a-137">Nyissa meg a projektmappát (például `cd ~/go/src/postgresqlgo/`).</span><span class="sxs-lookup"><span data-stu-id="f056a-137">Change directory into the folder, such as `cd ~/go/src/postgresqlgo/`.</span></span>
5. <span data-ttu-id="f056a-138">Úgy állítsa be a GOPATH környezeti változót, hogy egy érvényes forráskönyvtárra mutasson, például az aktuális kezdőkönyvtár Go mappájára.</span><span class="sxs-lookup"><span data-stu-id="f056a-138">Set the GOPATH environment variable to point to a valid source directory, such as your current home directory's go folder.</span></span> <span data-ttu-id="f056a-139">A bash felületen futtassa az `export GOPATH=~/go` parancsot, amellyel a Go könyvtárát GOPATH útvonalként adhatja meg az aktuális felületi munkamenetre.</span><span class="sxs-lookup"><span data-stu-id="f056a-139">At the bash shell, run `export GOPATH=~/go` to add the go directory as the GOPATH for the current shell session.</span></span>
6. <span data-ttu-id="f056a-140">Telepítse a [Pure Go Postgres illesztőt (pq)](https://github.com/lib/pq) a `go get github.com/lib/pq` parancs futtatásával.</span><span class="sxs-lookup"><span data-stu-id="f056a-140">Install the [Pure Go Postgres driver (pq)](https://github.com/lib/pq) by running the `go get github.com/lib/pq` command.</span></span>

   <span data-ttu-id="f056a-141">Összefoglalva, telepítse a Go-t, majd futtassa ezeket a bash-parancsokat:</span><span class="sxs-lookup"><span data-stu-id="f056a-141">In summary, install Go, then run these bash commands:</span></span>
   ```bash
   mkdir -p ~/go/src/postgresqlgo/
   cd ~/go/src/postgresqlgo/
   export GOPATH=~/go/
   go get github.com/lib/pq
   ```

## <a name="get-connection-information"></a><span data-ttu-id="f056a-142">Kapcsolatadatok lekérése</span><span class="sxs-lookup"><span data-stu-id="f056a-142">Get connection information</span></span>
<span data-ttu-id="f056a-143">Kérje le a PostgreSQL-hez készült Azure-adatbázishoz való csatlakozáshoz szükséges kapcsolatadatokat.</span><span class="sxs-lookup"><span data-stu-id="f056a-143">Get the connection information needed to connect to the Azure Database for PostgreSQL.</span></span> <span data-ttu-id="f056a-144">Szüksége lesz a teljes kiszolgálónévre és a bejelentkezési hitelesítő adatokra.</span><span class="sxs-lookup"><span data-stu-id="f056a-144">You need the fully qualified server name and login credentials.</span></span>

1. <span data-ttu-id="f056a-145">Jelentkezzen be az [Azure portálra](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="f056a-145">Log in to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="f056a-146">Az Azure Portal bal oldali menüjében kattintson az **Összes erőforrás** lehetőségre, és keressen rá a létrehozott kiszolgálóra (például **mypgserver-20170401**).</span><span class="sxs-lookup"><span data-stu-id="f056a-146">From the left-hand menu in Azure portal, click **All resources** and search for the server you have created, such as **mypgserver-20170401**.</span></span>
3. <span data-ttu-id="f056a-147">Kattintson a **mypgserver-20170401** kiszolgálónévre.</span><span class="sxs-lookup"><span data-stu-id="f056a-147">Click the server name **mypgserver-20170401**.</span></span>
4. <span data-ttu-id="f056a-148">Válassza ki a kiszolgáló **Áttekintés** oldalát.</span><span class="sxs-lookup"><span data-stu-id="f056a-148">Select the server's **Overview** page.</span></span> <span data-ttu-id="f056a-149">Jegyezze fel a **Kiszolgálónevet** és a **Kiszolgáló-rendszergazdai bejelentkezési nevet**.</span><span class="sxs-lookup"><span data-stu-id="f056a-149">Make a note of the **Server name** and **Server admin login name**.</span></span>
 <span data-ttu-id="f056a-150">![PostgreSQL-hez készült Azure-adatbázis – Kiszolgáló-rendszergazdai bejelentkezés](./media/connect-go/1-connection-string.png)</span><span class="sxs-lookup"><span data-stu-id="f056a-150">![Azure Database for PostgreSQL - Server Admin Login](./media/connect-go/1-connection-string.png)</span></span>
5. <span data-ttu-id="f056a-151">Amennyiben elfelejtette a kiszolgálója bejelentkezési adatait, lépjen az **Áttekintés** oldalra, és keresse ki a kiszolgáló-rendszergazda bejelentkezési nevét.</span><span class="sxs-lookup"><span data-stu-id="f056a-151">If you forget your server login information, navigate to the **Overview** page, and view the Server admin login name.</span></span> <span data-ttu-id="f056a-152">Szükség esetén kérjen új jelszót.</span><span class="sxs-lookup"><span data-stu-id="f056a-152">If necessary, reset the password.</span></span>

## <a name="build-and-run-go-code"></a><span data-ttu-id="f056a-153">Go kód felépítése és futtatása</span><span class="sxs-lookup"><span data-stu-id="f056a-153">Build and run Go code</span></span> 
1. <span data-ttu-id="f056a-154">Golang-kód írásához használhat egy egyszerű szövegszerkesztőt, ilyen például Microsoft Windows rendszeren a Jegyzettömb, Ubuntu rendszeren a [vi](http://manpages.ubuntu.com/manpages/xenial/man1/nvi.1.html#contenttoc5) vagy a [Nano](https://www.nano-editor.org/), macOS rendszeren pedig a TextEdit.</span><span class="sxs-lookup"><span data-stu-id="f056a-154">To write Golang code, you can use a simple text editor, such as Notepad in Microsoft Windows, [vi](http://manpages.ubuntu.com/manpages/xenial/man1/nvi.1.html#contenttoc5) or [Nano](https://www.nano-editor.org/) in Ubuntu, or TextEdit in macOS.</span></span> <span data-ttu-id="f056a-155">Ha a funkciógazdagabb interaktív fejlesztési környezeteket (IDE-ket) részesít előnyben, próbálja ki a Jetbrains [Gogland](https://www.jetbrains.com/go/) a Microsoft [Visual Studio Code](https://code.visualstudio.com/) vagy az [Atom](https://atom.io/) eszközt.</span><span class="sxs-lookup"><span data-stu-id="f056a-155">If you prefer a richer Interactive Development Environment (IDE) try [Gogland](https://www.jetbrains.com/go/) by Jetbrains, [Visual Studio Code](https://code.visualstudio.com/) by Microsoft, or [Atom](https://atom.io/).</span></span>
2. <span data-ttu-id="f056a-156">Az alábbi szakaszokban található Golang-kódokat illessze be szövegfájlokba, és mentse a fájlokat a projektmappába \*.go kiterjesztéssel, például `%USERPROFILE%\go\src\postgresqlgo\createtable.go` (Windows) vagy `~/go/src/postgresqlgo/createtable.go` (Linux) elérési úton.</span><span class="sxs-lookup"><span data-stu-id="f056a-156">Paste the Golang code from the sections below into text files, and save into your project folder with file extension \*.go, such as Windows path `%USERPROFILE%\go\src\postgresqlgo\createtable.go` or Linux path `~/go/src/postgresqlgo/createtable.go`.</span></span>
3. <span data-ttu-id="f056a-157">Keresse meg a `HOST`, a `DATABASE`, a `USER` és a `PASSWORD` állandót a kódban, és a példaértékeket cserélje le a saját értékeire.</span><span class="sxs-lookup"><span data-stu-id="f056a-157">Locate the `HOST`, `DATABASE`, `USER`, and `PASSWORD` constants in the code, and replace the example values with your own values.</span></span>  
4. <span data-ttu-id="f056a-158">Nyissa meg a parancssort vagy a bash rendszerhéjat.</span><span class="sxs-lookup"><span data-stu-id="f056a-158">Launch the command prompt or bash shell.</span></span> <span data-ttu-id="f056a-159">Lépjen a projektmappára.</span><span class="sxs-lookup"><span data-stu-id="f056a-159">Change directory into your project folder.</span></span> <span data-ttu-id="f056a-160">Windows rendszer például a következővel: `cd %USERPROFILE%\go\src\postgresqlgo\`.</span><span class="sxs-lookup"><span data-stu-id="f056a-160">For example, on Windows `cd %USERPROFILE%\go\src\postgresqlgo\`.</span></span> <span data-ttu-id="f056a-161">Linuxon: `cd ~/go/src/postgresqlgo/`.</span><span class="sxs-lookup"><span data-stu-id="f056a-161">On Linux `cd ~/go/src/postgresqlgo/`.</span></span> <span data-ttu-id="f056a-162">A fentiekben említettek közül egyes IDE-környezetek hibakeresési és futásidejű képességeket biztosítanak anélkül, hogy rendszerhéjparancsokra lenne szükség.</span><span class="sxs-lookup"><span data-stu-id="f056a-162">Some of the IDE environments mentioned offer debug and runtime capabilities without requiring shell commands.</span></span>
5. <span data-ttu-id="f056a-163">A kód futtatásához írja be a `go run createtable.go` parancsot az alkalmazás lefordításához és futtatásához.</span><span class="sxs-lookup"><span data-stu-id="f056a-163">Run the code by typing the command `go run createtable.go` to compile the application and run it.</span></span> 
6. <span data-ttu-id="f056a-164">Vagy a kód natív alkalmazásba való beépítéséhez írja be a `go build createtable.go` parancsot, majd indítsa el a `createtable.exe` fájlt az alkalmazás futtatásához.</span><span class="sxs-lookup"><span data-stu-id="f056a-164">Alternatively, to build the code into a native application, `go build createtable.go`, then launch `createtable.exe` to run the application.</span></span>

## <a name="connect-and-create-a-table"></a><span data-ttu-id="f056a-165">Csatlakozás és tábla létrehozása</span><span class="sxs-lookup"><span data-stu-id="f056a-165">Connect and create a table</span></span>
<span data-ttu-id="f056a-166">A következő kód használatával csatlakozhat, és létrehozhat egy táblát a **CREATE TABLE** SQL-utasítással, majd az **INSERT INTO** SQL-utasításokkal sorokat adhat hozzá a táblához.</span><span class="sxs-lookup"><span data-stu-id="f056a-166">Use the following code to connect and create a table using **CREATE TABLE** SQL statement, followed by **INSERT INTO** SQL statements to add rows into the table.</span></span>

<span data-ttu-id="f056a-167">A kód három csomagot importál, az [sql package](https://golang.org/pkg/database/sql/) és a [pq package](http://godoc.org/github.com/lib/pq) csomagot illesztőprogramként a Postgres-kiszolgálóval való kommunikációhoz, illetve az [fmt package](https://golang.org/pkg/fmt/) csomagot a nyomtatott bemenetekhez és kimenetekhez a parancssoron.</span><span class="sxs-lookup"><span data-stu-id="f056a-167">The code imports three packages: the [sql package](https://golang.org/pkg/database/sql/), the [pq package](http://godoc.org/github.com/lib/pq) as a driver to communicate with the Postgres server, and the [fmt package](https://golang.org/pkg/fmt/) for printed input and output on the command line.</span></span>

<span data-ttu-id="f056a-168">A kód meghívja az [sql.Open()](http://godoc.org/github.com/lib/pq#Open) metódust a PostgreSQL-hez készült Azure-adatbázishoz való csatlakozáshoz, majd a [db.Ping()](https://golang.org/pkg/database/sql/#DB.Ping) metódussal ellenőrzi a kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="f056a-168">The code calls method [sql.Open()](http://godoc.org/github.com/lib/pq#Open) to connect to Azure Database for PostgreSQL, and checks the connection using method [db.Ping()](https://golang.org/pkg/database/sql/#DB.Ping).</span></span> <span data-ttu-id="f056a-169">A rendszer a folyamat során [adatbázis-leírót](https://golang.org/pkg/database/sql/#DB) használ, amely az adatbázis-kiszolgáló kapcsolatkészletét tárolja.</span><span class="sxs-lookup"><span data-stu-id="f056a-169">A [database handle](https://golang.org/pkg/database/sql/#DB) is used throughout, holding the connection pool for the database server.</span></span> <span data-ttu-id="f056a-170">A kód többször is meghívja az [Exec()](https://golang.org/pkg/database/sql/#DB.Exec) metódust, hogy különböző SQL-parancsokat futtasson.</span><span class="sxs-lookup"><span data-stu-id="f056a-170">The code calls the [Exec()](https://golang.org/pkg/database/sql/#DB.Exec) method several times to run several SQL commands.</span></span> <span data-ttu-id="f056a-171">A rendszer minden alkalommal egyéni checkError() metódust hív meg, hogy ellenőrizze a hibákat, és hiba esetén kilépjen.</span><span class="sxs-lookup"><span data-stu-id="f056a-171">Each time a custom checkError() method to check if an error occurred and panic to exit if an error occurs.</span></span>

<span data-ttu-id="f056a-172">Cserélje le a `HOST`, `DATABASE`, `USER` és `PASSWORD` paramétereket a saját értékeire.</span><span class="sxs-lookup"><span data-stu-id="f056a-172">Replace the `HOST`, `DATABASE`, `USER`, and `PASSWORD` parameters with your own values.</span></span> 

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
    fmt.Println("Successfully created connection to database")

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

## <a name="read-data"></a><span data-ttu-id="f056a-173">Adatok beolvasása</span><span class="sxs-lookup"><span data-stu-id="f056a-173">Read data</span></span>
<span data-ttu-id="f056a-174">A következő kóddal csatlakozhat, és beolvashatja az adatokat a **SELECT** SQL-utasítással.</span><span class="sxs-lookup"><span data-stu-id="f056a-174">Use the following code to connect and read the data using a **SELECT** SQL statement.</span></span> 

<span data-ttu-id="f056a-175">A kód három csomagot importál, az [sql package](https://golang.org/pkg/database/sql/) és a [pq package](http://godoc.org/github.com/lib/pq) csomagot illesztőprogramként a Postgres-kiszolgálóval való kommunikációhoz, illetve az [fmt package](https://golang.org/pkg/fmt/) csomagot a nyomtatott bemenetekhez és kimenetekhez a parancssoron.</span><span class="sxs-lookup"><span data-stu-id="f056a-175">The code imports three packages: the [sql package](https://golang.org/pkg/database/sql/), the [pq package](http://godoc.org/github.com/lib/pq) as a driver to communicate with the Postgres server, and the [fmt package](https://golang.org/pkg/fmt/) for printed input and output on the command line.</span></span>

<span data-ttu-id="f056a-176">A kód meghívja az [sql.Open()](http://godoc.org/github.com/lib/pq#Open) metódust a PostgreSQL-hez készült Azure-adatbázishoz való csatlakozáshoz, majd a [db.Ping()](https://golang.org/pkg/database/sql/#DB.Ping) metódussal ellenőrzi a kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="f056a-176">The code calls method [sql.Open()](http://godoc.org/github.com/lib/pq#Open) to connect to Azure Database for PostgreSQL, and checks the connection using method [db.Ping()](https://golang.org/pkg/database/sql/#DB.Ping).</span></span> <span data-ttu-id="f056a-177">A rendszer a folyamat során [adatbázis-leírót](https://golang.org/pkg/database/sql/#DB) használ, amely az adatbázis-kiszolgáló kapcsolatkészletét tárolja.</span><span class="sxs-lookup"><span data-stu-id="f056a-177">A [database handle](https://golang.org/pkg/database/sql/#DB) is used throughout, holding the connection pool for the database server.</span></span> <span data-ttu-id="f056a-178">A választó lekérdezést a [db.Query()](https://golang.org/pkg/database/sql/#DB.Query) metódus meghívásával lehet futtatni, és az így kapott sorokat a rendszer egy [rows](https://golang.org/pkg/database/sql/#Rows) (sorok) típusú változóban tárolja.</span><span class="sxs-lookup"><span data-stu-id="f056a-178">The select query is run by calling method [db.Query()](https://golang.org/pkg/database/sql/#DB.Query), and the resulting rows is kept in a variable of type [rows](https://golang.org/pkg/database/sql/#Rows).</span></span> <span data-ttu-id="f056a-179">A kód beolvassa az aktuális sorban található oszlopok adatértékeit a [rows.Scan()](https://golang.org/pkg/database/sql/#Rows.Scan) metódussal, majd a [rows.Next()](https://golang.org/pkg/database/sql/#Rows.Next) iterátor használatával ismétlődően fut a sorokon, amíg azok el nem fogynak.</span><span class="sxs-lookup"><span data-stu-id="f056a-179">The code reads the column data values in the current row using method [rows.Scan()](https://golang.org/pkg/database/sql/#Rows.Scan) and loops over the rows using the iterator [rows.Next()](https://golang.org/pkg/database/sql/#Rows.Next) until no more rows exist.</span></span> <span data-ttu-id="f056a-180">Az egyes sorok oszlopértékei megjelennek a konzolon. A rendszer minden alkalommal egyéni checkError() metódust hív meg, hogy ellenőrizze a hibákat, és hiba esetén kilépjen.</span><span class="sxs-lookup"><span data-stu-id="f056a-180">Each row's column values are printed to the console out. Each time a custom checkError() method to check if an error occurred and panic to exit if an error occurs.</span></span>

<span data-ttu-id="f056a-181">Cserélje le a `HOST`, `DATABASE`, `USER` és `PASSWORD` paramétereket a saját értékeire.</span><span class="sxs-lookup"><span data-stu-id="f056a-181">Replace the `HOST`, `DATABASE`, `USER`, and `PASSWORD` parameters with your own values.</span></span> 

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
    fmt.Println("Successfully created connection to database")

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

## <a name="update-data"></a><span data-ttu-id="f056a-182">Adatok frissítése</span><span class="sxs-lookup"><span data-stu-id="f056a-182">Update data</span></span>
<span data-ttu-id="f056a-183">A következő kód használatával csatlakozhat, és frissítheti az adatokat az **UPDATE** SQL-utasítással.</span><span class="sxs-lookup"><span data-stu-id="f056a-183">Use the following code to connect and update the data using a **UPDATE** SQL statement.</span></span>

<span data-ttu-id="f056a-184">A kód három csomagot importál, az [sql package](https://golang.org/pkg/database/sql/) és a [pq package](http://godoc.org/github.com/lib/pq) csomagot illesztőprogramként a Postgres-kiszolgálóval való kommunikációhoz, illetve az [fmt package](https://golang.org/pkg/fmt/) csomagot a nyomtatott bemenetekhez és kimenetekhez a parancssoron.</span><span class="sxs-lookup"><span data-stu-id="f056a-184">The code imports three packages: the [sql package](https://golang.org/pkg/database/sql/), the [pq package](http://godoc.org/github.com/lib/pq) as a driver to communicate with the Postgres server, and the [fmt package](https://golang.org/pkg/fmt/) for printed input and output on the command line.</span></span>

<span data-ttu-id="f056a-185">A kód meghívja az [sql.Open()](http://godoc.org/github.com/lib/pq#Open) metódust a PostgreSQL-hez készült Azure-adatbázishoz való csatlakozáshoz, majd a [db.Ping()](https://golang.org/pkg/database/sql/#DB.Ping) metódussal ellenőrzi a kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="f056a-185">The code calls method [sql.Open()](http://godoc.org/github.com/lib/pq#Open) to connect to Azure Database for PostgreSQL, and checks the connection using method [db.Ping()](https://golang.org/pkg/database/sql/#DB.Ping).</span></span> <span data-ttu-id="f056a-186">A rendszer a folyamat során [adatbázis-leírót](https://golang.org/pkg/database/sql/#DB) használ, amely az adatbázis-kiszolgáló kapcsolatkészletét tárolja.</span><span class="sxs-lookup"><span data-stu-id="f056a-186">A [database handle](https://golang.org/pkg/database/sql/#DB) is used throughout, holding the connection pool for the database server.</span></span> <span data-ttu-id="f056a-187">A kód meghívja az [Exec()](https://golang.org/pkg/database/sql/#DB.Exec) metódust, hogy futtassa a táblát frissítő SQL-utasítást.</span><span class="sxs-lookup"><span data-stu-id="f056a-187">The code calls the [Exec()](https://golang.org/pkg/database/sql/#DB.Exec) method to run the SQL statement that updates the table.</span></span> <span data-ttu-id="f056a-188">Egyéni checkError() metódust hív meg, hogy ellenőrizze a hibákat, és hiba esetén kilépjen.</span><span class="sxs-lookup"><span data-stu-id="f056a-188">A custom checkError() method to check if an error occurred and panic to exit if an error occurs.</span></span>

<span data-ttu-id="f056a-189">Cserélje le a `HOST`, `DATABASE`, `USER` és `PASSWORD` paramétereket a saját értékeire.</span><span class="sxs-lookup"><span data-stu-id="f056a-189">Replace the `HOST`, `DATABASE`, `USER`, and `PASSWORD` parameters with your own values.</span></span> 
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
    fmt.Println("Successfully created connection to database")

    // Modify some data in table.
    sql_statement := "UPDATE inventory SET quantity = $2 WHERE name = $1;"
    _, err = db.Exec(sql_statement, "banana", 200)
    checkError(err)
    fmt.Println("Updated 1 row of data")
}
```

## <a name="delete-data"></a><span data-ttu-id="f056a-190">Adat törlése</span><span class="sxs-lookup"><span data-stu-id="f056a-190">Delete data</span></span>
<span data-ttu-id="f056a-191">A következő kód használatával csatlakozhat, és beolvashatja az adatokat a **DELETE** SQL-utasítással.</span><span class="sxs-lookup"><span data-stu-id="f056a-191">Use the following code to connect and read the data using a **DELETE** SQL statement.</span></span> 

<span data-ttu-id="f056a-192">A kód három csomagot importál, az [sql package](https://golang.org/pkg/database/sql/) és a [pq package](http://godoc.org/github.com/lib/pq) csomagot illesztőprogramként a Postgres-kiszolgálóval való kommunikációhoz, illetve az [fmt package](https://golang.org/pkg/fmt/) csomagot a nyomtatott bemenetekhez és kimenetekhez a parancssoron.</span><span class="sxs-lookup"><span data-stu-id="f056a-192">The code imports three packages: the [sql package](https://golang.org/pkg/database/sql/), the [pq package](http://godoc.org/github.com/lib/pq) as a driver to communicate with the Postgres server, and the [fmt package](https://golang.org/pkg/fmt/) for printed input and output on the command line.</span></span>

<span data-ttu-id="f056a-193">A kód meghívja az [sql.Open()](http://godoc.org/github.com/lib/pq#Open) metódust a PostgreSQL-hez készült Azure-adatbázishoz való csatlakozáshoz, majd a [db.Ping()](https://golang.org/pkg/database/sql/#DB.Ping) metódussal ellenőrzi a kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="f056a-193">The code calls method [sql.Open()](http://godoc.org/github.com/lib/pq#Open) to connect to Azure Database for PostgreSQL, and checks the connection using method [db.Ping()](https://golang.org/pkg/database/sql/#DB.Ping).</span></span> <span data-ttu-id="f056a-194">A rendszer a folyamat során [adatbázis-leírót](https://golang.org/pkg/database/sql/#DB) használ, amely az adatbázis-kiszolgáló kapcsolatkészletét tárolja.</span><span class="sxs-lookup"><span data-stu-id="f056a-194">A [database handle](https://golang.org/pkg/database/sql/#DB) is used throughout, holding the connection pool for the database server.</span></span> <span data-ttu-id="f056a-195">A kód meghívja az [Exec()](https://golang.org/pkg/database/sql/#DB.Exec) metódust, hogy futtassa a táblát frissítő SQL-utasítást.</span><span class="sxs-lookup"><span data-stu-id="f056a-195">The code calls the [Exec()](https://golang.org/pkg/database/sql/#DB.Exec) method to run the SQL statement that updates the table.</span></span> <span data-ttu-id="f056a-196">Egyéni checkError() metódust hív meg, hogy ellenőrizze a hibákat, és hiba esetén kilépjen.</span><span class="sxs-lookup"><span data-stu-id="f056a-196">A custom checkError() method to check if an error occurred and panic to exit if an error occurs.</span></span>

<span data-ttu-id="f056a-197">Cserélje le a `HOST`, `DATABASE`, `USER` és `PASSWORD` paramétereket a saját értékeire.</span><span class="sxs-lookup"><span data-stu-id="f056a-197">Replace the `HOST`, `DATABASE`, `USER`, and `PASSWORD` parameters with your own values.</span></span> 
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
    fmt.Println("Successfully created connection to database")

    // Delete some data from table.
    sql_statement := "DELETE FROM inventory WHERE name = $1;"
    _, err = db.Exec(sql_statement, "orange")
    checkError(err)
    fmt.Println("Deleted 1 row of data")
}
```

## <a name="next-steps"></a><span data-ttu-id="f056a-198">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f056a-198">Next steps</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="f056a-199">Adatbázis migrálása exportálással és importálással</span><span class="sxs-lookup"><span data-stu-id="f056a-199">Migrate your database using Export and Import</span></span>](./howto-migrate-using-export-and-import.md)
