---
title: "Segítségével nyissa meg a MySQL-adatbázis tooAzure kapcsolati |} Microsoft Docs"
description: "A gyors üzembe helyezés több Ugrás mintakódok tooconnect használhat és MySQL az Azure-adatbázis adatait lekérdezése biztosít."
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
ms.openlocfilehash: e8067b807ee729e04850c5325f476806bcd54983
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-database-for-mysql-use-go-language-tooconnect-and-query-data"></a><span data-ttu-id="6110e-103">MySQL az Azure-adatbázishoz: nyelvi tooconnect és lekérdezési adatok használata nyissa meg</span><span class="sxs-lookup"><span data-stu-id="6110e-103">Azure Database for MySQL: Use Go language tooconnect and query data</span></span>
<span data-ttu-id="6110e-104">A gyors üzembe helyezés bemutatja, hogyan tooconnect tooan Azure adatbázis MySQL használatára vonatkozó code nyelven írt hello [lépjen](https://golang.org/) Windows, az Ubuntu Linux és az Apple macOS platformok nyelvet.</span><span class="sxs-lookup"><span data-stu-id="6110e-104">This quickstart demonstrates how tooconnect tooan Azure Database for MySQL using code written in hello [Go](https://golang.org/) language from Windows, Ubuntu Linux, and Apple macOS platforms.</span></span> <span data-ttu-id="6110e-105">Azt illusztrálja, hogyan toouse SQL utasítás tooquery beszúrási, frissítési és törlési hello adatbázis adatait.</span><span class="sxs-lookup"><span data-stu-id="6110e-105">It shows how toouse SQL statements tooquery, insert, update, and delete data in hello database.</span></span> <span data-ttu-id="6110e-106">Ez a cikk feltételezi, hogy jártas használatával nyissa meg, azonban, hogy új tooworking MySQL az Azure-adatbázissal.</span><span class="sxs-lookup"><span data-stu-id="6110e-106">This article assumes you are familiar with development using Go, but that you are new tooworking with Azure Database for MySQL.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6110e-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="6110e-107">Prerequisites</span></span>
<span data-ttu-id="6110e-108">A gyors üzembe helyezés kiindulási pontként ezek az útmutatók valamelyikével létrehozott hello erőforrást használ:</span><span class="sxs-lookup"><span data-stu-id="6110e-108">This quickstart uses hello resources created in either of these guides as a starting point:</span></span>
- [<span data-ttu-id="6110e-109">Azure-adatbázis létrehozása MySQL-kiszolgálóhoz az Azure Portal használatával</span><span class="sxs-lookup"><span data-stu-id="6110e-109">Create an Azure Database for MySQL server using Azure portal</span></span>](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [<span data-ttu-id="6110e-110">Azure-adatbázis létrehozása MySQL-kiszolgálóhoz az Azure CLI használatával</span><span class="sxs-lookup"><span data-stu-id="6110e-110">Create an Azure Database for MySQL server using Azure CLI</span></span>](./quickstart-create-mysql-server-database-using-azure-cli.md)

## <a name="install-go-and-mysql-connector"></a><span data-ttu-id="6110e-111">A Go és a MySQL-összekötő telepítése</span><span class="sxs-lookup"><span data-stu-id="6110e-111">Install Go and MySQL connector</span></span>
<span data-ttu-id="6110e-112">Telepítse [lépjen](https://golang.org/doc/install) és hello [Ugrás-sql-illesztőprogramját MySQL](https://github.com/go-sql-driver/mysql#installation) a saját számítógépén.</span><span class="sxs-lookup"><span data-stu-id="6110e-112">Install [Go](https://golang.org/doc/install) and hello [go-sql-driver for MySQL](https://github.com/go-sql-driver/mysql#installation) on your own machine.</span></span> <span data-ttu-id="6110e-113">Attól függően, hogy a platform a hello lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="6110e-113">Depending on your platform, follow hello steps:</span></span>

### <a name="windows"></a><span data-ttu-id="6110e-114">Windows</span><span class="sxs-lookup"><span data-stu-id="6110e-114">Windows</span></span>
1. <span data-ttu-id="6110e-115">[Töltse le](https://golang.org/dl/) és nyissa meg a Microsoft Windows toohello szerint telepítse [telepítési utasításokat](https://golang.org/doc/install).</span><span class="sxs-lookup"><span data-stu-id="6110e-115">[Download](https://golang.org/dl/) and install Go for Microsoft Windows according toohello [installation instructions](https://golang.org/doc/install).</span></span>
2. <span data-ttu-id="6110e-116">Indítsa el a parancssor hello hello start menüből.</span><span class="sxs-lookup"><span data-stu-id="6110e-116">Launch hello command prompt from hello start menu.</span></span>
3. <span data-ttu-id="6110e-117">Hozzon létre egy mappát a projekt számára, például</span><span class="sxs-lookup"><span data-stu-id="6110e-117">Make a folder for your project such.</span></span> <span data-ttu-id="6110e-118">`mkdir  %USERPROFILE%\go\src\mysqlgo`.</span><span class="sxs-lookup"><span data-stu-id="6110e-118">`mkdir  %USERPROFILE%\go\src\mysqlgo`.</span></span>
4. <span data-ttu-id="6110e-119">Például módosítsa a könyvtárat hello projekt mappába `cd %USERPROFILE%\go\src\mysqlgo`.</span><span class="sxs-lookup"><span data-stu-id="6110e-119">Change directory into hello project folder, such as `cd %USERPROFILE%\go\src\mysqlgo`.</span></span>
5. <span data-ttu-id="6110e-120">Állítsa be a hello környezeti változó GOPATH kód toopoint toohello forráskönyvtár keresése.</span><span class="sxs-lookup"><span data-stu-id="6110e-120">Set hello environment variable for GOPATH toopoint toohello source code directory.</span></span> <span data-ttu-id="6110e-121">`set GOPATH=%USERPROFILE%\go`.</span><span class="sxs-lookup"><span data-stu-id="6110e-121">`set GOPATH=%USERPROFILE%\go`.</span></span>
6. <span data-ttu-id="6110e-122">Telepítse a hello [nyissa meg az sql-illesztőprogram a mysql](https://github.com/go-sql-driver/mysql#installation) hello futtatásával `go get github.com/go-sql-driver/mysql` parancs.</span><span class="sxs-lookup"><span data-stu-id="6110e-122">Install hello [go-sql-driver for mysql](https://github.com/go-sql-driver/mysql#installation) by running hello `go get github.com/go-sql-driver/mysql` command.</span></span>

   <span data-ttu-id="6110e-123">Összefoglalva nyissa meg telepítette, akkor a hello parancssorban futtassa az alábbi parancsokat:</span><span class="sxs-lookup"><span data-stu-id="6110e-123">In summary, install Go, then run these commands in hello command prompt:</span></span>
   ```cmd
   mkdir  %USERPROFILE%\go\src\mysqlgo
   cd %USERPROFILE%\go\src\mysqlgo
   set GOPATH=%USERPROFILE%\go
   go get github.com/go-sql-driver/mysql
   ```

### <a name="linux-ubuntu"></a><span data-ttu-id="6110e-124">Linux (Ubuntu)</span><span class="sxs-lookup"><span data-stu-id="6110e-124">Linux (Ubuntu)</span></span>
1. <span data-ttu-id="6110e-125">Indítsa el a hello Bash rendszerhéjat.</span><span class="sxs-lookup"><span data-stu-id="6110e-125">Launch hello Bash shell.</span></span> 
2. <span data-ttu-id="6110e-126">Telepítse a Go-t a `sudo apt-get install golang-go` parancs futtatásával.</span><span class="sxs-lookup"><span data-stu-id="6110e-126">Install Go by running `sudo apt-get install golang-go`.</span></span>
3. <span data-ttu-id="6110e-127">Hozzon létre egy mappát a projekt számára a kezdőkönyvtárban (például `mkdir -p ~/go/src/mysqlgo/`).</span><span class="sxs-lookup"><span data-stu-id="6110e-127">Make a folder for your project in your home directory, such as `mkdir -p ~/go/src/mysqlgo/`.</span></span>
4. <span data-ttu-id="6110e-128">Például módosítsa a könyvtárat hello mappába `cd ~/go/src/mysqlgo/`.</span><span class="sxs-lookup"><span data-stu-id="6110e-128">Change directory into hello folder, such as `cd ~/go/src/mysqlgo/`.</span></span>
5. <span data-ttu-id="6110e-129">Set hello GOPATH környezeti változó toopoint tooa érvényes forrás címtár, például az aktuális otthoni könyvtár mappában nyissa meg.</span><span class="sxs-lookup"><span data-stu-id="6110e-129">Set hello GOPATH environment variable toopoint tooa valid source directory, such as your current home directory's go folder.</span></span> <span data-ttu-id="6110e-130">Hello rendszerhéjakba, futni `export GOPATH=~/go` tooadd hello GOPATH hello hello aktuális rendszerhéj munkamenetet, nyissa meg könyvtár.</span><span class="sxs-lookup"><span data-stu-id="6110e-130">At hello bash shell, run `export GOPATH=~/go` tooadd hello go directory as hello GOPATH for hello current shell session.</span></span>
6. <span data-ttu-id="6110e-131">Telepítse a hello [nyissa meg az sql-illesztőprogram a mysql](https://github.com/go-sql-driver/mysql#installation) hello futtatásával `go get github.com/go-sql-driver/mysql` parancs.</span><span class="sxs-lookup"><span data-stu-id="6110e-131">Install hello [go-sql-driver for mysql](https://github.com/go-sql-driver/mysql#installation) by running hello `go get github.com/go-sql-driver/mysql` command.</span></span>

   <span data-ttu-id="6110e-132">Összefoglalva, futtassa ezeket a bash-parancsokat:</span><span class="sxs-lookup"><span data-stu-id="6110e-132">In summary, run these bash commands:</span></span>
   ```bash
   sudo apt-get install golang-go
   mkdir -p ~/go/src/mysqlgo/
   cd ~/go/src/mysqlgo/
   export GOPATH=~/go/
   go get github.com/go-sql-driver/mysql
   ```

### <a name="apple-macos"></a><span data-ttu-id="6110e-133">Apple macOS</span><span class="sxs-lookup"><span data-stu-id="6110e-133">Apple macOS</span></span>
1. <span data-ttu-id="6110e-134">Töltse le és telepítse a Go szerint toohello [telepítési utasításokat](https://golang.org/doc/install) a platformhoz megfelelő.</span><span class="sxs-lookup"><span data-stu-id="6110e-134">Download and install Go according toohello [installation instructions](https://golang.org/doc/install)  matching your platform.</span></span> 
2. <span data-ttu-id="6110e-135">Indítsa el a hello Bash rendszerhéjat.</span><span class="sxs-lookup"><span data-stu-id="6110e-135">Launch hello Bash shell.</span></span> 
3. <span data-ttu-id="6110e-136">Hozzon létre egy mappát a projekt számára a kezdőkönyvtárban (például `mkdir -p ~/go/src/mysqlgo/`).</span><span class="sxs-lookup"><span data-stu-id="6110e-136">Make a folder for your project in your home directory, such as `mkdir -p ~/go/src/mysqlgo/`.</span></span>
4. <span data-ttu-id="6110e-137">Például módosítsa a könyvtárat hello mappába `cd ~/go/src/mysqlgo/`.</span><span class="sxs-lookup"><span data-stu-id="6110e-137">Change directory into hello folder, such as `cd ~/go/src/mysqlgo/`.</span></span>
5. <span data-ttu-id="6110e-138">Set hello GOPATH környezeti változó toopoint tooa érvényes forrás címtár, például az aktuális otthoni könyvtár mappában nyissa meg.</span><span class="sxs-lookup"><span data-stu-id="6110e-138">Set hello GOPATH environment variable toopoint tooa valid source directory, such as your current home directory's go folder.</span></span> <span data-ttu-id="6110e-139">Hello rendszerhéjakba, futni `export GOPATH=~/go` tooadd hello GOPATH hello hello aktuális rendszerhéj munkamenetet, nyissa meg könyvtár.</span><span class="sxs-lookup"><span data-stu-id="6110e-139">At hello bash shell, run `export GOPATH=~/go` tooadd hello go directory as hello GOPATH for hello current shell session.</span></span>
6. <span data-ttu-id="6110e-140">Telepítse a hello [nyissa meg az sql-illesztőprogram a mysql](https://github.com/go-sql-driver/mysql#installation) hello futtatásával `go get github.com/go-sql-driver/mysql` parancs.</span><span class="sxs-lookup"><span data-stu-id="6110e-140">Install hello [go-sql-driver for mysql](https://github.com/go-sql-driver/mysql#installation) by running hello `go get github.com/go-sql-driver/mysql` command.</span></span>

   <span data-ttu-id="6110e-141">Összefoglalva, telepítse a Go-t, majd futtassa ezeket a bash-parancsokat:</span><span class="sxs-lookup"><span data-stu-id="6110e-141">In summary, install Go, then run these bash commands:</span></span>
   ```bash
   mkdir -p ~/go/src/mysqlgo/
   cd ~/go/src/mysqlgo/
   export GOPATH=~/go/
   go get github.com/go-sql-driver/mysql
   ```

## <a name="get-connection-information"></a><span data-ttu-id="6110e-142">Kapcsolatadatok lekérése</span><span class="sxs-lookup"><span data-stu-id="6110e-142">Get connection information</span></span>
<span data-ttu-id="6110e-143">MySQL hello kapcsolat szükséges információkat tooconnect toohello Azure adatbázis beolvasása.</span><span class="sxs-lookup"><span data-stu-id="6110e-143">Get hello connection information needed tooconnect toohello Azure Database for MySQL.</span></span> <span data-ttu-id="6110e-144">Teljesen minősített kiszolgáló nevét és a bejelentkezési hitelesítő adatokat hello van szüksége.</span><span class="sxs-lookup"><span data-stu-id="6110e-144">You need hello fully qualified server name and login credentials.</span></span>

1. <span data-ttu-id="6110e-145">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="6110e-145">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="6110e-146">A hello Azure-portálon a bal oldali menüből, kattintson az **összes erőforrás** , és keresse meg rendelkezik gyűrött, például a hello kiszolgáló **myserver4demo**.</span><span class="sxs-lookup"><span data-stu-id="6110e-146">From hello left-hand menu in Azure portal, click **All resources** and search for hello server you have creased, such as **myserver4demo**.</span></span>
3. <span data-ttu-id="6110e-147">Hello kiszolgáló nevére kattint **myserver4demo**.</span><span class="sxs-lookup"><span data-stu-id="6110e-147">Click hello server name **myserver4demo**.</span></span>
4. <span data-ttu-id="6110e-148">Jelölje be hello server **tulajdonságok** lap.</span><span class="sxs-lookup"><span data-stu-id="6110e-148">Select hello server's **Properties** page.</span></span> <span data-ttu-id="6110e-149">Jegyezze fel a hello **kiszolgálónév** és **kiszolgálói rendszergazda bejelentkezési név**.</span><span class="sxs-lookup"><span data-stu-id="6110e-149">Make a note of hello **Server name** and **Server admin login name**.</span></span>
 <span data-ttu-id="6110e-150">![MySQL-hez készült Azure-adatbázis – Kiszolgáló-rendszergazdai bejelentkezés](./media/connect-go/1_server-properties-name-login.png)</span><span class="sxs-lookup"><span data-stu-id="6110e-150">![Azure Database for MySQL - Server Admin Login](./media/connect-go/1_server-properties-name-login.png)</span></span>
5. <span data-ttu-id="6110e-151">Ha elfelejti a kiszolgálói bejelentkezési adatok, keresse meg a toohello **áttekintése** tooview hello kiszolgálói rendszergazda bejelentkezési név lapon, és ha szükséges, állítsa vissza a hello jelszót.</span><span class="sxs-lookup"><span data-stu-id="6110e-151">If you forget your server login information, navigate toohello **Overview** page tooview hello Server admin login name and, if necessary, reset hello password.</span></span>
   

## <a name="build-and-run-go-code"></a><span data-ttu-id="6110e-152">Go kód felépítése és futtatása</span><span class="sxs-lookup"><span data-stu-id="6110e-152">Build and run Go code</span></span> 
1. <span data-ttu-id="6110e-153">toowrite Golang kódot, használhatja egy egyszerű szövegszerkesztőben, például a Jegyzettömbben a Microsoft Windows [vi](http://manpages.ubuntu.com/manpages/xenial/man1/nvi.1.html#contenttoc5) vagy [Nano](https://www.nano-editor.org/) Ubuntu, vagy a macOS TextEdit.</span><span class="sxs-lookup"><span data-stu-id="6110e-153">toowrite Golang code, you can use a simple text editor, such as Notepad in Microsoft Windows, [vi](http://manpages.ubuntu.com/manpages/xenial/man1/nvi.1.html#contenttoc5) or [Nano](https://www.nano-editor.org/) in Ubuntu, or TextEdit in macOS.</span></span> <span data-ttu-id="6110e-154">Ha a funkciógazdagabb interaktív fejlesztési környezeteket (IDE-ket) részesít előnyben, próbálja ki a Jetbrains [Gogland](https://www.jetbrains.com/go/) a Microsoft [Visual Studio Code](https://code.visualstudio.com/) vagy az [Atom](https://atom.io/) eszközt.</span><span class="sxs-lookup"><span data-stu-id="6110e-154">If you prefer a richer Interactive Development Environment (IDE) try [Gogland](https://www.jetbrains.com/go/) by Jetbrains, [Visual Studio Code](https://code.visualstudio.com/) by Microsoft, or [Atom](https://atom.io/).</span></span>
2. <span data-ttu-id="6110e-155">Illessze be a hello hello szakaszokat kódot nyissa meg a szövegfájlok, és mentse a projekt mappába kiterjesztésű \*.go, például a Windows útvonalhoz `%USERPROFILE%\go\src\mysqlgo\createtable.go` vagy Linux elérési `~/go/src/mysqlgo/createtable.go`.</span><span class="sxs-lookup"><span data-stu-id="6110e-155">Paste hello Go code from hello sections below into text files, and save into your project folder with file extension \*.go, such as Windows path `%USERPROFILE%\go\src\mysqlgo\createtable.go` or Linux path `~/go/src/mysqlgo/createtable.go`.</span></span>
3. <span data-ttu-id="6110e-156">Keresse meg a hello `HOST`, `DATABASE`, `USER`, és `PASSWORD` konstansok hello kódot, és a név felülírandó hello példaértékeket a saját értékekkel.</span><span class="sxs-lookup"><span data-stu-id="6110e-156">Locate hello `HOST`, `DATABASE`, `USER`, and `PASSWORD` constants in hello code, and replace hello example values with your own values.</span></span> 
4. <span data-ttu-id="6110e-157">Indítsa el a hello parancssort, vagy a bash rendszerhéjat.</span><span class="sxs-lookup"><span data-stu-id="6110e-157">Launch hello command prompt or bash shell.</span></span> <span data-ttu-id="6110e-158">Lépjen a projektmappára.</span><span class="sxs-lookup"><span data-stu-id="6110e-158">Change directory into your project folder.</span></span> <span data-ttu-id="6110e-159">Windows rendszer például a következővel: `cd %USERPROFILE%\go\src\mysqlgo\`.</span><span class="sxs-lookup"><span data-stu-id="6110e-159">For example, on Windows `cd %USERPROFILE%\go\src\mysqlgo\`.</span></span> <span data-ttu-id="6110e-160">Linuxon: `cd ~/go/src/mysqlgo/`.</span><span class="sxs-lookup"><span data-stu-id="6110e-160">On Linux `cd ~/go/src/mysqlgo/`.</span></span>  <span data-ttu-id="6110e-161">Néhány hello IDE szerkesztők említett ajánlatot hibakeresési és futásidejű képességek anélkül, hogy a rendszerhéj-parancsok.</span><span class="sxs-lookup"><span data-stu-id="6110e-161">Some of hello IDE editors mentioned offer debug and runtime capabilities without requiring shell commands.</span></span>
5. <span data-ttu-id="6110e-162">A kódra hello hello parancs beírásával `go run createtable.go` toocompile hello alkalmazást, és futtassa azt.</span><span class="sxs-lookup"><span data-stu-id="6110e-162">Run hello code by typing hello command `go run createtable.go` toocompile hello application and run it.</span></span> 
6. <span data-ttu-id="6110e-163">Másik lehetőségként toobuild hello kód natív alkalmazásba `go build createtable.go`, majd indítsa el `createtable.exe` toorun hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="6110e-163">Alternatively, toobuild hello code into a native application, `go build createtable.go`, then launch `createtable.exe` toorun hello application.</span></span>

## <a name="connect-create-table-and-insert-data"></a><span data-ttu-id="6110e-164">Csatlakozás, táblák létrehozása és adatok beszúrása</span><span class="sxs-lookup"><span data-stu-id="6110e-164">Connect, create table, and insert data</span></span>
<span data-ttu-id="6110e-165">Használja hello alábbi tooconnect toohello server code, hozzon létre egy táblát, és hello használatával végzett betölteni egy **BESZÚRÁSA** SQL-utasításban.</span><span class="sxs-lookup"><span data-stu-id="6110e-165">Use hello following code tooconnect toohello server, create a table, and load hello data using an **INSERT** SQL statement.</span></span> 

<span data-ttu-id="6110e-166">hello kód importálja három csomagot: hello [sql csomag](https://golang.org/pkg/database/sql/), hello [mysql nyissa meg az sql-illesztőprogramját](https://github.com/go-sql-driver/mysql#installation) , egy illesztőprogram toocommunicate hello MySQL az Azure-adatbázis és hello [fmt csomag](https://golang.org/pkg/fmt/)nyomtatott bemeneti és kimeneti hello parancssorban.</span><span class="sxs-lookup"><span data-stu-id="6110e-166">hello code imports three packages: hello [sql package](https://golang.org/pkg/database/sql/), hello [go sql driver for mysql](https://github.com/go-sql-driver/mysql#installation) as a driver toocommunicate with hello Azure Database for MySQL, and hello [fmt package](https://golang.org/pkg/fmt/) for printed input and output on hello command line.</span></span>

<span data-ttu-id="6110e-167">hello kód metódus meghívja [sql. Open()](http://go-database-sql.org/accessing.html) tooconnect tooAzure MySQL és az ellenőrzések hello kapcsolat metódussal adatbázis [db. Ping()](https://golang.org/pkg/database/sql/#DB.Ping).</span><span class="sxs-lookup"><span data-stu-id="6110e-167">hello code calls method [sql.Open()](http://go-database-sql.org/accessing.html) tooconnect tooAzure Database for MySQL, and checks hello connection using method [db.Ping()](https://golang.org/pkg/database/sql/#DB.Ping).</span></span> <span data-ttu-id="6110e-168">A [adatbázis-leíró](https://golang.org/pkg/database/sql/#DB) használja a rendszer keresztül, hello kapcsolatkészlet hello adatbázis-kiszolgáló rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="6110e-168">A [database handle](https://golang.org/pkg/database/sql/#DB) is used throughout, holding hello connection pool for hello database server.</span></span> <span data-ttu-id="6110e-169">hello kód hívások hello [Exec()](https://golang.org/pkg/database/sql/#DB.Exec) metódus többször toorun több DDL-parancsok.</span><span class="sxs-lookup"><span data-stu-id="6110e-169">hello code calls hello [Exec()](https://golang.org/pkg/database/sql/#DB.Exec) method several times toorun several DDL commands.</span></span> <span data-ttu-id="6110e-170">hello kódot is használ hello [Prepare()](http://go-database-sql.org/prepared.html) és Exec() toorun előkészített utasítás eltérő paraméterekkel rendelkező tooinsert három sort.</span><span class="sxs-lookup"><span data-stu-id="6110e-170">hello code also uses hello [Prepare()](http://go-database-sql.org/prepared.html) and Exec() toorun prepared statements with different parameters tooinsert three rows.</span></span> <span data-ttu-id="6110e-171">Minden alkalommal, amikor egy egyéni checkError() módszer esetén használt toocheck hiba történt, és tooexit vészhelyzeti.</span><span class="sxs-lookup"><span data-stu-id="6110e-171">Each time a custom checkError() method is used toocheck if an error occurred and panic tooexit.</span></span>

<span data-ttu-id="6110e-172">Cserélje le a hello `host`, `database`, `user`, és `password` állandók saját értékekkel.</span><span class="sxs-lookup"><span data-stu-id="6110e-172">Replace hello `host`, `database`, `user`, and `password` constants with your own values.</span></span> 

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
    fmt.Println("Successfully created connection toodatabase.")

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

## <a name="read-data"></a><span data-ttu-id="6110e-173">Adatok olvasása</span><span class="sxs-lookup"><span data-stu-id="6110e-173">Read data</span></span>
<span data-ttu-id="6110e-174">Használjon hello alábbi code tooconnect, és hello adatok segítségével olvassa a **kiválasztása** SQL-utasításban.</span><span class="sxs-lookup"><span data-stu-id="6110e-174">Use hello following code tooconnect and read hello data using a **SELECT** SQL statement.</span></span> 

<span data-ttu-id="6110e-175">hello kód importálja három csomagot: hello [sql csomag](https://golang.org/pkg/database/sql/), hello [mysql nyissa meg az sql-illesztőprogramját](https://github.com/go-sql-driver/mysql#installation) , egy illesztőprogram toocommunicate hello MySQL az Azure-adatbázis és hello [fmt csomag](https://golang.org/pkg/fmt/)nyomtatott bemeneti és kimeneti hello parancssorban.</span><span class="sxs-lookup"><span data-stu-id="6110e-175">hello code imports three packages: hello [sql package](https://golang.org/pkg/database/sql/), hello [go sql driver for mysql](https://github.com/go-sql-driver/mysql#installation) as a driver toocommunicate with hello Azure Database for MySQL, and hello [fmt package](https://golang.org/pkg/fmt/) for printed input and output on hello command line.</span></span>

<span data-ttu-id="6110e-176">hello kód metódus meghívja [sql. Open()](http://go-database-sql.org/accessing.html) tooconnect tooAzure MySQL és az ellenőrzések hello kapcsolat metódussal adatbázis [db. Ping()](https://golang.org/pkg/database/sql/#DB.Ping).</span><span class="sxs-lookup"><span data-stu-id="6110e-176">hello code calls method [sql.Open()](http://go-database-sql.org/accessing.html) tooconnect tooAzure Database for MySQL, and checks hello connection using method [db.Ping()](https://golang.org/pkg/database/sql/#DB.Ping).</span></span> <span data-ttu-id="6110e-177">A [adatbázis-leíró](https://golang.org/pkg/database/sql/#DB) használja a rendszer keresztül, hello kapcsolatkészlet hello adatbázis-kiszolgáló rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="6110e-177">A [database handle](https://golang.org/pkg/database/sql/#DB) is used throughout, holding hello connection pool for hello database server.</span></span> <span data-ttu-id="6110e-178">hello kód hívások hello [Query()](https://golang.org/pkg/database/sql/#DB.Query) metódus toorun hello kijelölési parancs.</span><span class="sxs-lookup"><span data-stu-id="6110e-178">hello code calls hello [Query()](https://golang.org/pkg/database/sql/#DB.Query) method toorun hello select command.</span></span> <span data-ttu-id="6110e-179">Majd futása [Next()](https://golang.org/pkg/database/sql/#Rows.Next) keresztül hello eredménykészlet tooiterate és [Scan()](https://golang.org/pkg/database/sql/#Rows.Scan) tooparse hello oszlop értékek, a változók hello érték mentése.</span><span class="sxs-lookup"><span data-stu-id="6110e-179">Then it runs [Next()](https://golang.org/pkg/database/sql/#Rows.Next) tooiterate through hello result set and [Scan()](https://golang.org/pkg/database/sql/#Rows.Scan) tooparse hello column values, saving hello value into variables.</span></span> <span data-ttu-id="6110e-180">Minden alkalommal, amikor egy egyéni checkError() módszer esetén használt toocheck hiba történt, és tooexit vészhelyzeti.</span><span class="sxs-lookup"><span data-stu-id="6110e-180">Each time a custom checkError() method is used toocheck if an error occurred and panic tooexit.</span></span>

<span data-ttu-id="6110e-181">Cserélje le a hello `host`, `database`, `user`, és `password` állandók saját értékekkel.</span><span class="sxs-lookup"><span data-stu-id="6110e-181">Replace hello `host`, `database`, `user`, and `password` constants with your own values.</span></span> 

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
    fmt.Println("Successfully created connection toodatabase.")

    // Variables for printing column data when scanned.
    var (
        id       int
        name     string
        quantity int
    )

    // Read some data from hello table.
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

## <a name="update-data"></a><span data-ttu-id="6110e-182">Adatok frissítése</span><span class="sxs-lookup"><span data-stu-id="6110e-182">Update data</span></span>
<span data-ttu-id="6110e-183">Használjon hello következő code tooconnect, és frissítse a hello adatok segítségével egy **frissítése** SQL-utasításban.</span><span class="sxs-lookup"><span data-stu-id="6110e-183">Use hello following code tooconnect and update hello data using a **UPDATE** SQL statement.</span></span> 

<span data-ttu-id="6110e-184">hello kód importálja három csomagot: hello [sql csomag](https://golang.org/pkg/database/sql/), hello [mysql nyissa meg az sql-illesztőprogramját](https://github.com/go-sql-driver/mysql#installation) , egy illesztőprogram toocommunicate hello MySQL az Azure-adatbázis és hello [fmt csomag](https://golang.org/pkg/fmt/)nyomtatott bemeneti és kimeneti hello parancssorban.</span><span class="sxs-lookup"><span data-stu-id="6110e-184">hello code imports three packages: hello [sql package](https://golang.org/pkg/database/sql/), hello [go sql driver for mysql](https://github.com/go-sql-driver/mysql#installation) as a driver toocommunicate with hello Azure Database for MySQL, and hello [fmt package](https://golang.org/pkg/fmt/) for printed input and output on hello command line.</span></span>

<span data-ttu-id="6110e-185">hello kód metódus meghívja [sql. Open()](http://go-database-sql.org/accessing.html) tooconnect tooAzure MySQL és az ellenőrzések hello kapcsolat metódussal adatbázis [db. Ping()](https://golang.org/pkg/database/sql/#DB.Ping).</span><span class="sxs-lookup"><span data-stu-id="6110e-185">hello code calls method [sql.Open()](http://go-database-sql.org/accessing.html) tooconnect tooAzure Database for MySQL, and checks hello connection using method [db.Ping()](https://golang.org/pkg/database/sql/#DB.Ping).</span></span> <span data-ttu-id="6110e-186">A [adatbázis-leíró](https://golang.org/pkg/database/sql/#DB) használja a rendszer keresztül, hello kapcsolatkészlet hello adatbázis-kiszolgáló rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="6110e-186">A [database handle](https://golang.org/pkg/database/sql/#DB) is used throughout, holding hello connection pool for hello database server.</span></span> <span data-ttu-id="6110e-187">hello kód hívások hello [Exec()](https://golang.org/pkg/database/sql/#DB.Exec) metódus toorun hello frissítés parancsot.</span><span class="sxs-lookup"><span data-stu-id="6110e-187">hello code calls hello [Exec()](https://golang.org/pkg/database/sql/#DB.Exec) method toorun hello update command.</span></span> <span data-ttu-id="6110e-188">Minden alkalommal, amikor egy egyéni checkError() módszer esetén használt toocheck hiba történt, és tooexit vészhelyzeti.</span><span class="sxs-lookup"><span data-stu-id="6110e-188">Each time a custom checkError() method is used toocheck if an error occurred and panic tooexit.</span></span>

<span data-ttu-id="6110e-189">Cserélje le a hello `host`, `database`, `user`, és `password` állandók saját értékekkel.</span><span class="sxs-lookup"><span data-stu-id="6110e-189">Replace hello `host`, `database`, `user`, and `password` constants with your own values.</span></span> 

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
    fmt.Println("Successfully created connection toodatabase.")

    // Modify some data in table.
    rows, err := db.Exec("UPDATE inventory SET quantity = ? WHERE name = ?", 200, "banana")
    checkError(err)
    rowCount, err := rows.RowsAffected()
    fmt.Printf("Deleted %d row(s) of data.\n", rowCount)
    fmt.Println("Done.")
}
```

## <a name="delete-data"></a><span data-ttu-id="6110e-190">Adat törlése</span><span class="sxs-lookup"><span data-stu-id="6110e-190">Delete data</span></span>
<span data-ttu-id="6110e-191">Használjon hello alábbi code tooconnect, és távolítsa el az adatokat egy **törlése** SQL-utasításban.</span><span class="sxs-lookup"><span data-stu-id="6110e-191">Use hello following code tooconnect and remove data using a **DELETE** SQL statement.</span></span> 

<span data-ttu-id="6110e-192">hello kód importálja három csomagot: hello [sql csomag](https://golang.org/pkg/database/sql/), hello [mysql nyissa meg az sql-illesztőprogramját](https://github.com/go-sql-driver/mysql#installation) , egy illesztőprogram toocommunicate hello MySQL az Azure-adatbázis és hello [fmt csomag](https://golang.org/pkg/fmt/)nyomtatott bemeneti és kimeneti hello parancssorban.</span><span class="sxs-lookup"><span data-stu-id="6110e-192">hello code imports three packages: hello [sql package](https://golang.org/pkg/database/sql/), hello [go sql driver for mysql](https://github.com/go-sql-driver/mysql#installation) as a driver toocommunicate with hello Azure Database for MySQL, and hello [fmt package](https://golang.org/pkg/fmt/) for printed input and output on hello command line.</span></span>

<span data-ttu-id="6110e-193">hello kód metódus meghívja [sql. Open()](http://go-database-sql.org/accessing.html) tooconnect tooAzure MySQL és az ellenőrzések hello kapcsolat metódussal adatbázis [db. Ping()](https://golang.org/pkg/database/sql/#DB.Ping).</span><span class="sxs-lookup"><span data-stu-id="6110e-193">hello code calls method [sql.Open()](http://go-database-sql.org/accessing.html) tooconnect tooAzure Database for MySQL, and checks hello connection using method [db.Ping()](https://golang.org/pkg/database/sql/#DB.Ping).</span></span> <span data-ttu-id="6110e-194">A [adatbázis-leíró](https://golang.org/pkg/database/sql/#DB) használja a rendszer keresztül, hello kapcsolatkészlet hello adatbázis-kiszolgáló rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="6110e-194">A [database handle](https://golang.org/pkg/database/sql/#DB) is used throughout, holding hello connection pool for hello database server.</span></span> <span data-ttu-id="6110e-195">hello kód hívások hello [Exec()](https://golang.org/pkg/database/sql/#DB.Exec) metódus toorun hello törlése paranccsal.</span><span class="sxs-lookup"><span data-stu-id="6110e-195">hello code calls hello [Exec()](https://golang.org/pkg/database/sql/#DB.Exec) method toorun hello delete command.</span></span> <span data-ttu-id="6110e-196">Minden alkalommal, amikor egy egyéni checkError() módszer esetén használt toocheck hiba történt, és tooexit vészhelyzeti.</span><span class="sxs-lookup"><span data-stu-id="6110e-196">Each time a custom checkError() method is used toocheck if an error occurred and panic tooexit.</span></span>

<span data-ttu-id="6110e-197">Cserélje le a hello `host`, `database`, `user`, és `password` állandók saját értékekkel.</span><span class="sxs-lookup"><span data-stu-id="6110e-197">Replace hello `host`, `database`, `user`, and `password` constants with your own values.</span></span> 

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
    fmt.Println("Successfully created connection toodatabase.")

    // Modify some data in table.
    rows, err := db.Exec("DELETE FROM inventory WHERE name = ?", "orange")
    checkError(err)
    rowCount, err := rows.RowsAffected()
    fmt.Printf("Deleted %d row(s) of data.\n", rowCount)
    fmt.Println("Done.")
}
```

## <a name="next-steps"></a><span data-ttu-id="6110e-198">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6110e-198">Next steps</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="6110e-199">Adatbázis migrálása exportálással és importálással</span><span class="sxs-lookup"><span data-stu-id="6110e-199">Migrate your database using Export and Import</span></span>](./concepts-migrate-import-export.md)
