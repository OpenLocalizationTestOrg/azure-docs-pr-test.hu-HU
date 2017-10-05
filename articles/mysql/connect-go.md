---
title: "Csatlakozás a MySQL-hez készült Azure-adatbázishoz a Go használatával | Microsoft Docs"
description: "Ez a rövid útmutató több Go-mintakódot biztosít, amelyekkel csatlakozhat a MySQL-hez készült Azure-adatbázishoz, illetve adatokat kérdezhet le róla."
services: mysql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.custom: mvc
ms.devlang: go
ms.topic: hero-article
ms.date: 07/18/2017
ms.openlocfilehash: 42a6b1c37de08971674c8b38f1e13bfd657f8b03
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/29/2017
---
# <a name="azure-database-for-mysql-use-go-language-to-connect-and-query-data"></a><span data-ttu-id="87156-103">MySQL-hez készült Azure-adatbázis: Csatlakozás és adatok lekérdezése a Go használatával</span><span class="sxs-lookup"><span data-stu-id="87156-103">Azure Database for MySQL: Use Go language to connect and query data</span></span>
<span data-ttu-id="87156-104">Ez a rövid útmutató azt ismerteti, hogyan lehet csatlakozni a MySQL-hez készült Azure-adatbázishoz [Go](https://golang.org/) nyelven írt kóddal, Windows, Ubuntu Linux és Apple macOS platformról.</span><span class="sxs-lookup"><span data-stu-id="87156-104">This quickstart demonstrates how to connect to an Azure Database for MySQL using code written in the [Go](https://golang.org/) language from Windows, Ubuntu Linux, and Apple macOS platforms.</span></span> <span data-ttu-id="87156-105">Azt is bemutatja, hogyan lehet SQL-utasítások használatával adatokat lekérdezni, beszúrni, frissíteni és törölni az adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="87156-105">It shows how to use SQL statements to query, insert, update, and delete data in the database.</span></span> <span data-ttu-id="87156-106">A cikk feltételezi, hogy Ön ismeri a Go használatával történő fejlesztést, de még járatlan a MySQL-hez készült Azure-adatbázis használatában.</span><span class="sxs-lookup"><span data-stu-id="87156-106">This article assumes you are familiar with development using Go, but that you are new to working with Azure Database for MySQL.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="87156-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="87156-107">Prerequisites</span></span>
<span data-ttu-id="87156-108">Ebben a rövid útmutatóban a következő útmutatók valamelyikében létrehozott erőforrásokat használunk kiindulási pontként:</span><span class="sxs-lookup"><span data-stu-id="87156-108">This quickstart uses the resources created in either of these guides as a starting point:</span></span>
- [<span data-ttu-id="87156-109">Azure-adatbázis létrehozása MySQL-kiszolgálóhoz az Azure Portal használatával</span><span class="sxs-lookup"><span data-stu-id="87156-109">Create an Azure Database for MySQL server using Azure portal</span></span>](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [<span data-ttu-id="87156-110">Azure-adatbázis létrehozása MySQL-kiszolgálóhoz az Azure CLI használatával</span><span class="sxs-lookup"><span data-stu-id="87156-110">Create an Azure Database for MySQL server using Azure CLI</span></span>](./quickstart-create-mysql-server-database-using-azure-cli.md)

## <a name="install-go-and-mysql-connector"></a><span data-ttu-id="87156-111">A Go és a MySQL-összekötő telepítése</span><span class="sxs-lookup"><span data-stu-id="87156-111">Install Go and MySQL connector</span></span>
<span data-ttu-id="87156-112">Telepítse a [Go](https://golang.org/doc/install)-t és a [go-sql-driver for MySQL](https://github.com/go-sql-driver/mysql#installation) illesztőt a saját számítógépére.</span><span class="sxs-lookup"><span data-stu-id="87156-112">Install [Go](https://golang.org/doc/install) and the [go-sql-driver for MySQL](https://github.com/go-sql-driver/mysql#installation) on your own machine.</span></span> <span data-ttu-id="87156-113">Kövesse az egyes platformoknak megfelelő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="87156-113">Depending on your platform, follow the steps:</span></span>

### <a name="windows"></a><span data-ttu-id="87156-114">Windows</span><span class="sxs-lookup"><span data-stu-id="87156-114">Windows</span></span>
1. <span data-ttu-id="87156-115">[Töltse le](https://golang.org/dl/) és telepítse a Microsoft Windowshoz készült Go-t a [telepítési utasítások](https://golang.org/doc/install) szerint.</span><span class="sxs-lookup"><span data-stu-id="87156-115">[Download](https://golang.org/dl/) and install Go for Microsoft Windows according to the [installation instructions](https://golang.org/doc/install).</span></span>
2. <span data-ttu-id="87156-116">Nyissa meg a parancssort a Start menüből.</span><span class="sxs-lookup"><span data-stu-id="87156-116">Launch the command prompt from the start menu.</span></span>
3. <span data-ttu-id="87156-117">Hozzon létre egy mappát a projekt számára, például</span><span class="sxs-lookup"><span data-stu-id="87156-117">Make a folder for your project such.</span></span> <span data-ttu-id="87156-118">`mkdir  %USERPROFILE%\go\src\mysqlgo`.</span><span class="sxs-lookup"><span data-stu-id="87156-118">`mkdir  %USERPROFILE%\go\src\mysqlgo`.</span></span>
4. <span data-ttu-id="87156-119">Nyissa meg a projektmappát (például `cd %USERPROFILE%\go\src\mysqlgo`).</span><span class="sxs-lookup"><span data-stu-id="87156-119">Change directory into the project folder, such as `cd %USERPROFILE%\go\src\mysqlgo`.</span></span>
5. <span data-ttu-id="87156-120">Úgy állítsa be a GOPATH környezeti változóját, hogy a forráskód könyvtárára mutasson.</span><span class="sxs-lookup"><span data-stu-id="87156-120">Set the environment variable for GOPATH to point to the source code directory.</span></span> <span data-ttu-id="87156-121">`set GOPATH=%USERPROFILE%\go`.</span><span class="sxs-lookup"><span data-stu-id="87156-121">`set GOPATH=%USERPROFILE%\go`.</span></span>
6. <span data-ttu-id="87156-122">Telepítse a [go-sql-driver for mysql](https://github.com/go-sql-driver/mysql#installation) illesztőt a `go get github.com/go-sql-driver/mysql` parancs futtatásával.</span><span class="sxs-lookup"><span data-stu-id="87156-122">Install the [go-sql-driver for mysql](https://github.com/go-sql-driver/mysql#installation) by running the `go get github.com/go-sql-driver/mysql` command.</span></span>

   <span data-ttu-id="87156-123">Összefoglalva, telepítse a Go-t, majd futtassa ezeket a parancsokat a parancssorban:</span><span class="sxs-lookup"><span data-stu-id="87156-123">In summary, install Go, then run these commands in the command prompt:</span></span>
   ```cmd
   mkdir  %USERPROFILE%\go\src\mysqlgo
   cd %USERPROFILE%\go\src\mysqlgo
   set GOPATH=%USERPROFILE%\go
   go get github.com/go-sql-driver/mysql
   ```

### <a name="linux-ubuntu"></a><span data-ttu-id="87156-124">Linux (Ubuntu)</span><span class="sxs-lookup"><span data-stu-id="87156-124">Linux (Ubuntu)</span></span>
1. <span data-ttu-id="87156-125">Indítsa el a Bash felületet.</span><span class="sxs-lookup"><span data-stu-id="87156-125">Launch the Bash shell.</span></span> 
2. <span data-ttu-id="87156-126">Telepítse a Go-t a `sudo apt-get install golang-go` parancs futtatásával.</span><span class="sxs-lookup"><span data-stu-id="87156-126">Install Go by running `sudo apt-get install golang-go`.</span></span>
3. <span data-ttu-id="87156-127">Hozzon létre egy mappát a projekt számára a kezdőkönyvtárban (például `mkdir -p ~/go/src/mysqlgo/`).</span><span class="sxs-lookup"><span data-stu-id="87156-127">Make a folder for your project in your home directory, such as `mkdir -p ~/go/src/mysqlgo/`.</span></span>
4. <span data-ttu-id="87156-128">Nyissa meg a projektmappát (például `cd ~/go/src/mysqlgo/`).</span><span class="sxs-lookup"><span data-stu-id="87156-128">Change directory into the folder, such as `cd ~/go/src/mysqlgo/`.</span></span>
5. <span data-ttu-id="87156-129">Úgy állítsa be a GOPATH környezeti változót, hogy egy érvényes forráskönyvtárra mutasson, például az aktuális kezdőkönyvtár Go mappájára.</span><span class="sxs-lookup"><span data-stu-id="87156-129">Set the GOPATH environment variable to point to a valid source directory, such as your current home directory's go folder.</span></span> <span data-ttu-id="87156-130">A bash felületen futtassa az `export GOPATH=~/go` parancsot, amellyel a Go könyvtárát GOPATH útvonalként adhatja meg az aktuális felületi munkamenetre.</span><span class="sxs-lookup"><span data-stu-id="87156-130">At the bash shell, run `export GOPATH=~/go` to add the go directory as the GOPATH for the current shell session.</span></span>
6. <span data-ttu-id="87156-131">Telepítse a [go-sql-driver for mysql](https://github.com/go-sql-driver/mysql#installation) illesztőt a `go get github.com/go-sql-driver/mysql` parancs futtatásával.</span><span class="sxs-lookup"><span data-stu-id="87156-131">Install the [go-sql-driver for mysql](https://github.com/go-sql-driver/mysql#installation) by running the `go get github.com/go-sql-driver/mysql` command.</span></span>

   <span data-ttu-id="87156-132">Összefoglalva, futtassa ezeket a bash-parancsokat:</span><span class="sxs-lookup"><span data-stu-id="87156-132">In summary, run these bash commands:</span></span>
   ```bash
   sudo apt-get install golang-go
   mkdir -p ~/go/src/mysqlgo/
   cd ~/go/src/mysqlgo/
   export GOPATH=~/go/
   go get github.com/go-sql-driver/mysql
   ```

### <a name="apple-macos"></a><span data-ttu-id="87156-133">Apple macOS</span><span class="sxs-lookup"><span data-stu-id="87156-133">Apple macOS</span></span>
1. <span data-ttu-id="87156-134">Töltse le és telepítse a Go-t a platformjának megfelelő [telepítési utasítások](https://golang.org/doc/install) szerint.</span><span class="sxs-lookup"><span data-stu-id="87156-134">Download and install Go according to the [installation instructions](https://golang.org/doc/install)  matching your platform.</span></span> 
2. <span data-ttu-id="87156-135">Indítsa el a Bash felületet.</span><span class="sxs-lookup"><span data-stu-id="87156-135">Launch the Bash shell.</span></span> 
3. <span data-ttu-id="87156-136">Hozzon létre egy mappát a projekt számára a kezdőkönyvtárban (például `mkdir -p ~/go/src/mysqlgo/`).</span><span class="sxs-lookup"><span data-stu-id="87156-136">Make a folder for your project in your home directory, such as `mkdir -p ~/go/src/mysqlgo/`.</span></span>
4. <span data-ttu-id="87156-137">Nyissa meg a projektmappát (például `cd ~/go/src/mysqlgo/`).</span><span class="sxs-lookup"><span data-stu-id="87156-137">Change directory into the folder, such as `cd ~/go/src/mysqlgo/`.</span></span>
5. <span data-ttu-id="87156-138">Úgy állítsa be a GOPATH környezeti változót, hogy egy érvényes forráskönyvtárra mutasson, például az aktuális kezdőkönyvtár Go mappájára.</span><span class="sxs-lookup"><span data-stu-id="87156-138">Set the GOPATH environment variable to point to a valid source directory, such as your current home directory's go folder.</span></span> <span data-ttu-id="87156-139">A bash felületen futtassa az `export GOPATH=~/go` parancsot, amellyel a Go könyvtárát GOPATH útvonalként adhatja meg az aktuális felületi munkamenetre.</span><span class="sxs-lookup"><span data-stu-id="87156-139">At the bash shell, run `export GOPATH=~/go` to add the go directory as the GOPATH for the current shell session.</span></span>
6. <span data-ttu-id="87156-140">Telepítse a [go-sql-driver for mysql](https://github.com/go-sql-driver/mysql#installation) illesztőt a `go get github.com/go-sql-driver/mysql` parancs futtatásával.</span><span class="sxs-lookup"><span data-stu-id="87156-140">Install the [go-sql-driver for mysql](https://github.com/go-sql-driver/mysql#installation) by running the `go get github.com/go-sql-driver/mysql` command.</span></span>

   <span data-ttu-id="87156-141">Összefoglalva, telepítse a Go-t, majd futtassa ezeket a bash-parancsokat:</span><span class="sxs-lookup"><span data-stu-id="87156-141">In summary, install Go, then run these bash commands:</span></span>
   ```bash
   mkdir -p ~/go/src/mysqlgo/
   cd ~/go/src/mysqlgo/
   export GOPATH=~/go/
   go get github.com/go-sql-driver/mysql
   ```

## <a name="get-connection-information"></a><span data-ttu-id="87156-142">Kapcsolatadatok lekérése</span><span class="sxs-lookup"><span data-stu-id="87156-142">Get connection information</span></span>
<span data-ttu-id="87156-143">Kérje le a MySQL-hez készült Azure Database-hez való csatlakozáshoz szükséges kapcsolatadatokat.</span><span class="sxs-lookup"><span data-stu-id="87156-143">Get the connection information needed to connect to the Azure Database for MySQL.</span></span> <span data-ttu-id="87156-144">Ehhez szükség lesz a teljes kiszolgálónévre és bejelentkezési hitelesítő adatokra.</span><span class="sxs-lookup"><span data-stu-id="87156-144">You need the fully qualified server name and login credentials.</span></span>

1. <span data-ttu-id="87156-145">Jelentkezzen be az [Azure portálra](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="87156-145">Log in to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="87156-146">Az Azure Portal bal oldali menüjében kattintson az **Összes erőforrás** lehetőségre, és keressen rá a létrehozott kiszolgálóra (például: **myserver4demo**).</span><span class="sxs-lookup"><span data-stu-id="87156-146">From the left-hand menu in Azure portal, click **All resources** and search for the server you have creased, such as **myserver4demo**.</span></span>
3. <span data-ttu-id="87156-147">Kattintson a **myserver4demo** kiszolgálónévre.</span><span class="sxs-lookup"><span data-stu-id="87156-147">Click the server name **myserver4demo**.</span></span>
4. <span data-ttu-id="87156-148">Válassza a kiszolgáló **tulajdonságlapját**.</span><span class="sxs-lookup"><span data-stu-id="87156-148">Select the server's **Properties** page.</span></span> <span data-ttu-id="87156-149">Jegyezze fel a **Kiszolgálónevet** és a **Kiszolgáló-rendszergazdai bejelentkezési nevet**.</span><span class="sxs-lookup"><span data-stu-id="87156-149">Make a note of the **Server name** and **Server admin login name**.</span></span>
 <span data-ttu-id="87156-150">![MySQL-hez készült Azure-adatbázis – Kiszolgáló-rendszergazdai bejelentkezés](./media/connect-go/1_server-properties-name-login.png)</span><span class="sxs-lookup"><span data-stu-id="87156-150">![Azure Database for MySQL - Server Admin Login](./media/connect-go/1_server-properties-name-login.png)</span></span>
5. <span data-ttu-id="87156-151">Amennyiben elfelejtette a kiszolgálója bejelentkezési adatait, lépjen az **Áttekintés** oldalra, ahol kikeresheti a kiszolgáló-rendszergazda bejelentkezési nevét, valamint szükség esetén új jelszót kérhet.</span><span class="sxs-lookup"><span data-stu-id="87156-151">If you forget your server login information, navigate to the **Overview** page to view the Server admin login name and, if necessary, reset the password.</span></span>
   

## <a name="build-and-run-go-code"></a><span data-ttu-id="87156-152">Go kód felépítése és futtatása</span><span class="sxs-lookup"><span data-stu-id="87156-152">Build and run Go code</span></span> 
1. <span data-ttu-id="87156-153">Golang-kód írásához használhat egy egyszerű szövegszerkesztőt, ilyen például Microsoft Windows rendszeren a Jegyzettömb, Ubuntu rendszeren a [vi](http://manpages.ubuntu.com/manpages/xenial/man1/nvi.1.html#contenttoc5) vagy a [Nano](https://www.nano-editor.org/), macOS rendszeren pedig a TextEdit.</span><span class="sxs-lookup"><span data-stu-id="87156-153">To write Golang code, you can use a simple text editor, such as Notepad in Microsoft Windows, [vi](http://manpages.ubuntu.com/manpages/xenial/man1/nvi.1.html#contenttoc5) or [Nano](https://www.nano-editor.org/) in Ubuntu, or TextEdit in macOS.</span></span> <span data-ttu-id="87156-154">Ha a funkciógazdagabb interaktív fejlesztési környezeteket (IDE-ket) részesít előnyben, próbálja ki a Jetbrains [Gogland](https://www.jetbrains.com/go/) a Microsoft [Visual Studio Code](https://code.visualstudio.com/) vagy az [Atom](https://atom.io/) eszközt.</span><span class="sxs-lookup"><span data-stu-id="87156-154">If you prefer a richer Interactive Development Environment (IDE) try [Gogland](https://www.jetbrains.com/go/) by Jetbrains, [Visual Studio Code](https://code.visualstudio.com/) by Microsoft, or [Atom](https://atom.io/).</span></span>
2. <span data-ttu-id="87156-155">Az alábbi szakaszokban található Go-kódokat illessze be szövegfájlokba, és mentse a fájlokat a projektmappába \*.go kiterjesztéssel, például `%USERPROFILE%\go\src\mysqlgo\createtable.go` (Windows) vagy `~/go/src/mysqlgo/createtable.go` (Linux) elérési úton.</span><span class="sxs-lookup"><span data-stu-id="87156-155">Paste the Go code from the sections below into text files, and save into your project folder with file extension \*.go, such as Windows path `%USERPROFILE%\go\src\mysqlgo\createtable.go` or Linux path `~/go/src/mysqlgo/createtable.go`.</span></span>
3. <span data-ttu-id="87156-156">Keresse meg a `HOST`, a `DATABASE`, a `USER` és a `PASSWORD` állandót a kódban, és a példaértékeket cserélje le a saját értékeire.</span><span class="sxs-lookup"><span data-stu-id="87156-156">Locate the `HOST`, `DATABASE`, `USER`, and `PASSWORD` constants in the code, and replace the example values with your own values.</span></span> 
4. <span data-ttu-id="87156-157">Nyissa meg a parancssort vagy a bash rendszerhéjat.</span><span class="sxs-lookup"><span data-stu-id="87156-157">Launch the command prompt or bash shell.</span></span> <span data-ttu-id="87156-158">Lépjen a projektmappára.</span><span class="sxs-lookup"><span data-stu-id="87156-158">Change directory into your project folder.</span></span> <span data-ttu-id="87156-159">Windows rendszer például a következővel: `cd %USERPROFILE%\go\src\mysqlgo\`.</span><span class="sxs-lookup"><span data-stu-id="87156-159">For example, on Windows `cd %USERPROFILE%\go\src\mysqlgo\`.</span></span> <span data-ttu-id="87156-160">Linuxon: `cd ~/go/src/mysqlgo/`.</span><span class="sxs-lookup"><span data-stu-id="87156-160">On Linux `cd ~/go/src/mysqlgo/`.</span></span>  <span data-ttu-id="87156-161">A fentiekben említettek közül egyes IDE-szerkesztők hibakeresési és futásidejű képességeket biztosítanak anélkül, hogy rendszerhéjparancsokra lenne szükség.</span><span class="sxs-lookup"><span data-stu-id="87156-161">Some of the IDE editors mentioned offer debug and runtime capabilities without requiring shell commands.</span></span>
5. <span data-ttu-id="87156-162">Futtassa a kódot a `go run createtable.go` parancs beírásával az alkalmazás fordításához és futtatásához.</span><span class="sxs-lookup"><span data-stu-id="87156-162">Run the code by typing the command `go run createtable.go` to compile the application and run it.</span></span> 
6. <span data-ttu-id="87156-163">Vagy a kód natív alkalmazásba való beépítéséhez írja be a `go build createtable.go` parancsot, majd indítsa el a `createtable.exe` fájlt az alkalmazás futtatásához.</span><span class="sxs-lookup"><span data-stu-id="87156-163">Alternatively, to build the code into a native application, `go build createtable.go`, then launch `createtable.exe` to run the application.</span></span>

## <a name="connect-create-table-and-insert-data"></a><span data-ttu-id="87156-164">Csatlakozás, táblák létrehozása és adatok beszúrása</span><span class="sxs-lookup"><span data-stu-id="87156-164">Connect, create table, and insert data</span></span>
<span data-ttu-id="87156-165">Az alábbi kód használatával csatlakozhat a kiszolgálóhoz, hozhat létre táblát, és töltheti be az adatokat egy **INSERT** SQL-utasítással.</span><span class="sxs-lookup"><span data-stu-id="87156-165">Use the following code to connect to the server, create a table, and load the data using an **INSERT** SQL statement.</span></span> 

<span data-ttu-id="87156-166">A kód három csomagot importál: az [sql package](https://golang.org/pkg/database/sql/) és a [go sql driver for mysql](https://github.com/go-sql-driver/mysql#installation) csomagot illesztőként a MySQL-hez készült Azure adatbázissal való kommunikációhoz, illetve az [fmt package](https://golang.org/pkg/fmt/) csomagot a nyomtatott bemenetekhez és kimenetekhez a parancssoron.</span><span class="sxs-lookup"><span data-stu-id="87156-166">The code imports three packages: the [sql package](https://golang.org/pkg/database/sql/), the [go sql driver for mysql](https://github.com/go-sql-driver/mysql#installation) as a driver to communicate with the Azure Database for MySQL, and the [fmt package](https://golang.org/pkg/fmt/) for printed input and output on the command line.</span></span>

<span data-ttu-id="87156-167">A kód meghívja az [sql.Open()](http://go-database-sql.org/accessing.html) metódust a MySQL-hez készült Azure-adatbázishoz való csatlakozáshoz, majd a [db.Ping()](https://golang.org/pkg/database/sql/#DB.Ping) metódussal ellenőrzi a kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="87156-167">The code calls method [sql.Open()](http://go-database-sql.org/accessing.html) to connect to Azure Database for MySQL, and checks the connection using method [db.Ping()](https://golang.org/pkg/database/sql/#DB.Ping).</span></span> <span data-ttu-id="87156-168">A rendszer a folyamat során [adatbázis-leírót](https://golang.org/pkg/database/sql/#DB) használ, amely az adatbázis-kiszolgáló kapcsolatkészletét tárolja.</span><span class="sxs-lookup"><span data-stu-id="87156-168">A [database handle](https://golang.org/pkg/database/sql/#DB) is used throughout, holding the connection pool for the database server.</span></span> <span data-ttu-id="87156-169">A kód többször is meghívja az [Exec()](https://golang.org/pkg/database/sql/#DB.Exec) metódust, hogy különböző DDL-parancsokat futtasson.</span><span class="sxs-lookup"><span data-stu-id="87156-169">The code calls the [Exec()](https://golang.org/pkg/database/sql/#DB.Exec) method several times to run several DDL commands.</span></span> <span data-ttu-id="87156-170">A kód a [Prepare()](http://go-database-sql.org/prepared.html) és az Exec() metódussal előkészített utasításokat is futtat különböző paraméterekkel három sor beszúrásához.</span><span class="sxs-lookup"><span data-stu-id="87156-170">The code also uses the [Prepare()](http://go-database-sql.org/prepared.html) and Exec() to run prepared statements with different parameters to insert three rows.</span></span> <span data-ttu-id="87156-171">A rendszer minden alkalommal egyéni checkError() metódust használ a hibák ellenőrzéséhez és a kilépéshez.</span><span class="sxs-lookup"><span data-stu-id="87156-171">Each time a custom checkError() method is used to check if an error occurred and panic to exit.</span></span>

<span data-ttu-id="87156-172">Cserélje le a `host`, `database`, `user` és `password` állandókat a saját értékeire.</span><span class="sxs-lookup"><span data-stu-id="87156-172">Replace the `host`, `database`, `user`, and `password` constants with your own values.</span></span> 

```Go
package main

import (
    "database/sql"
    "fmt"

    _ "github.com/go-sql-driver/mysql"
)

const (
    host     = "myserver4demo.mysql.database.azure.com"
    database = "quickstartdb"
    user     = "myadmin@myserver4demo"
    password = "yourpassword"
)

func checkError(err error) {
    if err != nil {
        panic(err)
    }
}

func main() {

    // Initialize connection string.
    var connectionString = fmt.Sprintf("%s:%s@tcp(%s:3306)/%s?allowNativePasswords=true", user, password, host, database)

    // Initialize connection object.
    db, err := sql.Open("mysql", connectionString)
    checkError(err)
    defer db.Close()

    err = db.Ping()
    checkError(err)
    fmt.Println("Successfully created connection to database.")

    // Drop previous table of same name if one exists.
    _, err = db.Exec("DROP TABLE IF EXISTS inventory;")
    checkError(err)
    fmt.Println("Finished dropping table (if existed).")

    // Create table.
    _, err = db.Exec("CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);")
    checkError(err)
    fmt.Println("Finished creating table.")

    // Insert some data into table.
    sqlStatement, err := db.Prepare("INSERT INTO inventory (name, quantity) VALUES (?, ?);")
    res, err := sqlStatement.Exec("banana", 150)
    checkError(err)
    rowCount, err := res.RowsAffected()
    fmt.Printf("Inserted %d row(s) of data.\n", rowCount)

    res, err = sqlStatement.Exec("orange", 154)
    checkError(err)
    rowCount, err = res.RowsAffected()
    fmt.Printf("Inserted %d row(s) of data.\n", rowCount)

    res, err = sqlStatement.Exec("apple", 100)
    checkError(err)
    rowCount, err = res.RowsAffected()
    fmt.Printf("Inserted %d row(s) of data.\n", rowCount)
    fmt.Println("Done.")
}

```

## <a name="read-data"></a><span data-ttu-id="87156-173">Adatok olvasása</span><span class="sxs-lookup"><span data-stu-id="87156-173">Read data</span></span>
<span data-ttu-id="87156-174">A következő kóddal csatlakozhat, és beolvashatja az adatokat a **SELECT** SQL-utasítással.</span><span class="sxs-lookup"><span data-stu-id="87156-174">Use the following code to connect and read the data using a **SELECT** SQL statement.</span></span> 

<span data-ttu-id="87156-175">A kód három csomagot importál: az [sql package](https://golang.org/pkg/database/sql/) és a [go sql driver for mysql](https://github.com/go-sql-driver/mysql#installation) csomagot illesztőként a MySQL-hez készült Azure adatbázissal való kommunikációhoz, illetve az [fmt package](https://golang.org/pkg/fmt/) csomagot a nyomtatott bemenetekhez és kimenetekhez a parancssoron.</span><span class="sxs-lookup"><span data-stu-id="87156-175">The code imports three packages: the [sql package](https://golang.org/pkg/database/sql/), the [go sql driver for mysql](https://github.com/go-sql-driver/mysql#installation) as a driver to communicate with the Azure Database for MySQL, and the [fmt package](https://golang.org/pkg/fmt/) for printed input and output on the command line.</span></span>

<span data-ttu-id="87156-176">A kód meghívja az [sql.Open()](http://go-database-sql.org/accessing.html) metódust a MySQL-hez készült Azure-adatbázishoz való csatlakozáshoz, majd a [db.Ping()](https://golang.org/pkg/database/sql/#DB.Ping) metódussal ellenőrzi a kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="87156-176">The code calls method [sql.Open()](http://go-database-sql.org/accessing.html) to connect to Azure Database for MySQL, and checks the connection using method [db.Ping()](https://golang.org/pkg/database/sql/#DB.Ping).</span></span> <span data-ttu-id="87156-177">A rendszer a folyamat során [adatbázis-leírót](https://golang.org/pkg/database/sql/#DB) használ, amely az adatbázis-kiszolgáló kapcsolatkészletét tárolja.</span><span class="sxs-lookup"><span data-stu-id="87156-177">A [database handle](https://golang.org/pkg/database/sql/#DB) is used throughout, holding the connection pool for the database server.</span></span> <span data-ttu-id="87156-178">A kód meghívja a [Query()](https://golang.org/pkg/database/sql/#DB.Query) metódust a kiválasztott parancs futtatásához.</span><span class="sxs-lookup"><span data-stu-id="87156-178">The code calls the [Query()](https://golang.org/pkg/database/sql/#DB.Query) method to run the select command.</span></span> <span data-ttu-id="87156-179">Ezután a [Next()](https://golang.org/pkg/database/sql/#Rows.Next) metódust futtatja az eredmények közötti iterációhoz, a [Scan()](https://golang.org/pkg/database/sql/#Rows.Scan) használatával elemzi az oszlopértékeket, és menti az értéket a változókba.</span><span class="sxs-lookup"><span data-stu-id="87156-179">Then it runs [Next()](https://golang.org/pkg/database/sql/#Rows.Next) to iterate through the result set and [Scan()](https://golang.org/pkg/database/sql/#Rows.Scan) to parse the column values, saving the value into variables.</span></span> <span data-ttu-id="87156-180">A rendszer minden alkalommal egyéni checkError() metódust használ a hibák ellenőrzéséhez és a kilépéshez.</span><span class="sxs-lookup"><span data-stu-id="87156-180">Each time a custom checkError() method is used to check if an error occurred and panic to exit.</span></span>

<span data-ttu-id="87156-181">Cserélje le a `host`, `database`, `user` és `password` állandókat a saját értékeire.</span><span class="sxs-lookup"><span data-stu-id="87156-181">Replace the `host`, `database`, `user`, and `password` constants with your own values.</span></span> 

```Go
package main

import (
    "database/sql"
    "fmt"

    _ "github.com/go-sql-driver/mysql"
)

const (
    host     = "myserver4demo.mysql.database.azure.com"
    database = "quickstartdb"
    user     = "myadmin@myserver4demo"
    password = "yourpassword"
)

func checkError(err error) {
    if err != nil {
        panic(err)
    }
}

func main() {

    // Initialize connection string.
    var connectionString = fmt.Sprintf("%s:%s@tcp(%s:3306)/%s?allowNativePasswords=true", user, password, host, database)

    // Initialize connection object.
    db, err := sql.Open("mysql", connectionString)
    checkError(err)
    defer db.Close()

    err = db.Ping()
    checkError(err)
    fmt.Println("Successfully created connection to database.")

    // Variables for printing column data when scanned.
    var (
        id       int
        name     string
        quantity int
    )

    // Read some data from the table.
    rows, err := db.Query("SELECT id, name, quantity from inventory;")
    checkError(err)
    defer rows.Close()
    fmt.Println("Reading data:")
    for rows.Next() {
        err := rows.Scan(&id, &name, &quantity)
        checkError(err)
        fmt.Printf("Data row = (%d, %s, %d)\n", id, name, quantity)
    }
    err = rows.Err()
    checkError(err)
    fmt.Println("Done.")
}
```

## <a name="update-data"></a><span data-ttu-id="87156-182">Adatok frissítése</span><span class="sxs-lookup"><span data-stu-id="87156-182">Update data</span></span>
<span data-ttu-id="87156-183">A következő kód használatával csatlakozhat, és frissítheti az adatokat az **UPDATE** SQL-utasítással.</span><span class="sxs-lookup"><span data-stu-id="87156-183">Use the following code to connect and update the data using a **UPDATE** SQL statement.</span></span> 

<span data-ttu-id="87156-184">A kód három csomagot importál: az [sql package](https://golang.org/pkg/database/sql/) és a [go sql driver for mysql](https://github.com/go-sql-driver/mysql#installation) csomagot illesztőként a MySQL-hez készült Azure adatbázissal való kommunikációhoz, illetve az [fmt package](https://golang.org/pkg/fmt/) csomagot a nyomtatott bemenetekhez és kimenetekhez a parancssoron.</span><span class="sxs-lookup"><span data-stu-id="87156-184">The code imports three packages: the [sql package](https://golang.org/pkg/database/sql/), the [go sql driver for mysql](https://github.com/go-sql-driver/mysql#installation) as a driver to communicate with the Azure Database for MySQL, and the [fmt package](https://golang.org/pkg/fmt/) for printed input and output on the command line.</span></span>

<span data-ttu-id="87156-185">A kód meghívja az [sql.Open()](http://go-database-sql.org/accessing.html) metódust a MySQL-hez készült Azure-adatbázishoz való csatlakozáshoz, majd a [db.Ping()](https://golang.org/pkg/database/sql/#DB.Ping) metódussal ellenőrzi a kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="87156-185">The code calls method [sql.Open()](http://go-database-sql.org/accessing.html) to connect to Azure Database for MySQL, and checks the connection using method [db.Ping()](https://golang.org/pkg/database/sql/#DB.Ping).</span></span> <span data-ttu-id="87156-186">A rendszer a folyamat során [adatbázis-leírót](https://golang.org/pkg/database/sql/#DB) használ, amely az adatbázis-kiszolgáló kapcsolatkészletét tárolja.</span><span class="sxs-lookup"><span data-stu-id="87156-186">A [database handle](https://golang.org/pkg/database/sql/#DB) is used throughout, holding the connection pool for the database server.</span></span> <span data-ttu-id="87156-187">A kód meghívja az [Exec()](https://golang.org/pkg/database/sql/#DB.Exec) metódust a frissítési parancs futtatásához.</span><span class="sxs-lookup"><span data-stu-id="87156-187">The code calls the [Exec()](https://golang.org/pkg/database/sql/#DB.Exec) method to run the update command.</span></span> <span data-ttu-id="87156-188">A rendszer minden alkalommal egyéni checkError() metódust használ a hibák ellenőrzéséhez és a kilépéshez.</span><span class="sxs-lookup"><span data-stu-id="87156-188">Each time a custom checkError() method is used to check if an error occurred and panic to exit.</span></span>

<span data-ttu-id="87156-189">Cserélje le a `host`, `database`, `user` és `password` állandókat a saját értékeire.</span><span class="sxs-lookup"><span data-stu-id="87156-189">Replace the `host`, `database`, `user`, and `password` constants with your own values.</span></span> 

```Go
package main

import (
    "database/sql"
    "fmt"

    _ "github.com/go-sql-driver/mysql"
)

const (
    host     = "myserver4demo.mysql.database.azure.com"
    database = "quickstartdb"
    user     = "myadmin@myserver4demo"
    password = "yourpassword"
)

func checkError(err error) {
    if err != nil {
        panic(err)
    }
}

func main() {

    // Initialize connection string.
    var connectionString = fmt.Sprintf("%s:%s@tcp(%s:3306)/%s?allowNativePasswords=true", user, password, host, database)

    // Initialize connection object.
    db, err := sql.Open("mysql", connectionString)
    checkError(err)
    defer db.Close()

    err = db.Ping()
    checkError(err)
    fmt.Println("Successfully created connection to database.")

    // Modify some data in table.
    rows, err := db.Exec("UPDATE inventory SET quantity = ? WHERE name = ?", 200, "banana")
    checkError(err)
    rowCount, err := rows.RowsAffected()
    fmt.Printf("Deleted %d row(s) of data.\n", rowCount)
    fmt.Println("Done.")
}
```

## <a name="delete-data"></a><span data-ttu-id="87156-190">Adat törlése</span><span class="sxs-lookup"><span data-stu-id="87156-190">Delete data</span></span>
<span data-ttu-id="87156-191">A következő kód használatával csatlakozhat, és eltávolíthatja az adatokat a **DELETE** SQL-utasítással.</span><span class="sxs-lookup"><span data-stu-id="87156-191">Use the following code to connect and remove data using a **DELETE** SQL statement.</span></span> 

<span data-ttu-id="87156-192">A kód három csomagot importál: az [sql package](https://golang.org/pkg/database/sql/) és a [go sql driver for mysql](https://github.com/go-sql-driver/mysql#installation) csomagot illesztőként a MySQL-hez készült Azure adatbázissal való kommunikációhoz, illetve az [fmt package](https://golang.org/pkg/fmt/) csomagot a nyomtatott bemenetekhez és kimenetekhez a parancssoron.</span><span class="sxs-lookup"><span data-stu-id="87156-192">The code imports three packages: the [sql package](https://golang.org/pkg/database/sql/), the [go sql driver for mysql](https://github.com/go-sql-driver/mysql#installation) as a driver to communicate with the Azure Database for MySQL, and the [fmt package](https://golang.org/pkg/fmt/) for printed input and output on the command line.</span></span>

<span data-ttu-id="87156-193">A kód meghívja az [sql.Open()](http://go-database-sql.org/accessing.html) metódust a MySQL-hez készült Azure-adatbázishoz való csatlakozáshoz, majd a [db.Ping()](https://golang.org/pkg/database/sql/#DB.Ping) metódussal ellenőrzi a kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="87156-193">The code calls method [sql.Open()](http://go-database-sql.org/accessing.html) to connect to Azure Database for MySQL, and checks the connection using method [db.Ping()](https://golang.org/pkg/database/sql/#DB.Ping).</span></span> <span data-ttu-id="87156-194">A rendszer a folyamat során [adatbázis-leírót](https://golang.org/pkg/database/sql/#DB) használ, amely az adatbázis-kiszolgáló kapcsolatkészletét tárolja.</span><span class="sxs-lookup"><span data-stu-id="87156-194">A [database handle](https://golang.org/pkg/database/sql/#DB) is used throughout, holding the connection pool for the database server.</span></span> <span data-ttu-id="87156-195">A kód meghívja az [Exec()](https://golang.org/pkg/database/sql/#DB.Exec) metódust a törlési parancs futtatásához.</span><span class="sxs-lookup"><span data-stu-id="87156-195">The code calls the [Exec()](https://golang.org/pkg/database/sql/#DB.Exec) method to run the delete command.</span></span> <span data-ttu-id="87156-196">A rendszer minden alkalommal egyéni checkError() metódust használ a hibák ellenőrzéséhez és a kilépéshez.</span><span class="sxs-lookup"><span data-stu-id="87156-196">Each time a custom checkError() method is used to check if an error occurred and panic to exit.</span></span>

<span data-ttu-id="87156-197">Cserélje le a `host`, `database`, `user` és `password` állandókat a saját értékeire.</span><span class="sxs-lookup"><span data-stu-id="87156-197">Replace the `host`, `database`, `user`, and `password` constants with your own values.</span></span> 

```Go
package main

import (
    "database/sql"
    "fmt"
    _ "github.com/go-sql-driver/mysql"
)

const (
    host     = "myserver4demo.mysql.database.azure.com"
    database = "quickstartdb"
    user     = "myadmin@myserver4demo"
    password = "yourpassword"
)

func checkError(err error) {
    if err != nil {
        panic(err)
    }
}

func main() {

    // Initialize connection string.
    var connectionString = fmt.Sprintf("%s:%s@tcp(%s:3306)/%s?allowNativePasswords=true", user, password, host, database)

    // Initialize connection object.
    db, err := sql.Open("mysql", connectionString)
    checkError(err)
    defer db.Close()

    err = db.Ping()
    checkError(err)
    fmt.Println("Successfully created connection to database.")

    // Modify some data in table.
    rows, err := db.Exec("DELETE FROM inventory WHERE name = ?", "orange")
    checkError(err)
    rowCount, err := rows.RowsAffected()
    fmt.Printf("Deleted %d row(s) of data.\n", rowCount)
    fmt.Println("Done.")
}
```

## <a name="next-steps"></a><span data-ttu-id="87156-198">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="87156-198">Next steps</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="87156-199">Adatbázis migrálása exportálással és importálással</span><span class="sxs-lookup"><span data-stu-id="87156-199">Migrate your database using Export and Import</span></span>](./concepts-migrate-import-export.md)
