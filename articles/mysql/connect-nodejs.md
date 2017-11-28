---
title: "Node.js-ből MySQL-adatbázis tooAzure kapcsolati |} Microsoft Docs"
description: "A gyors üzembe helyezés biztosít több Node.js mintakódok tooconnect használhat és MySQL az Azure-adatbázis adatait lekérdezése."
services: mysql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.custom: mvc
ms.devlang: nodejs
ms.topic: hero-article
ms.date: 07/17/2017
ms.openlocfilehash: ed9a39b5ab003e8216cef1c0f6a95a75c3db8458
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-database-for-mysql-use-nodejs-tooconnect-and-query-data"></a><span data-ttu-id="06153-103">MySQL az Azure-adatbázishoz: használata Node.js tooconnect és lekérdezési adatok</span><span class="sxs-lookup"><span data-stu-id="06153-103">Azure Database for MySQL: Use Node.js tooconnect and query data</span></span>
<span data-ttu-id="06153-104">A gyors üzembe helyezés bemutatja, hogyan tooconnect tooan Azure adatbázis a MySQL használata [Node.js](https://nodejs.org/) a Windows, Ubuntu Linux és Mac rendszerek.</span><span class="sxs-lookup"><span data-stu-id="06153-104">This quickstart demonstrates how tooconnect tooan Azure Database for MySQL using [Node.js](https://nodejs.org/) from Windows, Ubuntu Linux, and Mac platforms.</span></span> <span data-ttu-id="06153-105">Azt illusztrálja, hogyan toouse SQL utasítás tooquery beszúrási, frissítési és törlési hello adatbázis adatait.</span><span class="sxs-lookup"><span data-stu-id="06153-105">It shows how toouse SQL statements tooquery, insert, update, and delete data in hello database.</span></span> <span data-ttu-id="06153-106">hello cikkben leírt lépések azt feltételezik, hogy ismeri a Node.js használatával történő fejlesztéséhez, és, hogy-e új tooworking MySQL az Azure-adatbázissal.</span><span class="sxs-lookup"><span data-stu-id="06153-106">hello steps in this article assume that you are familiar with developing using Node.js, and that you are new tooworking with Azure Database for MySQL.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="06153-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="06153-107">Prerequisites</span></span>
<span data-ttu-id="06153-108">A gyors üzembe helyezés kiindulási pontként ezek az útmutatók valamelyikével létrehozott hello erőforrást használ:</span><span class="sxs-lookup"><span data-stu-id="06153-108">This quickstart uses hello resources created in either of these guides as a starting point:</span></span>
- [<span data-ttu-id="06153-109">Azure-adatbázis létrehozása MySQL-kiszolgálóhoz az Azure Portal használatával</span><span class="sxs-lookup"><span data-stu-id="06153-109">Create an Azure Database for MySQL server using Azure portal</span></span>](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [<span data-ttu-id="06153-110">Azure-adatbázis létrehozása MySQL-kiszolgálóhoz az Azure CLI használatával</span><span class="sxs-lookup"><span data-stu-id="06153-110">Create an Azure Database for MySQL server using Azure CLI</span></span>](./quickstart-create-mysql-server-database-using-azure-cli.md)

<span data-ttu-id="06153-111">Emellett a következőket kell elvégezni:</span><span class="sxs-lookup"><span data-stu-id="06153-111">You also need to:</span></span>
- <span data-ttu-id="06153-112">Telepítse a hello [Node.js](https://nodejs.org) futásidejű.</span><span class="sxs-lookup"><span data-stu-id="06153-112">Install hello [Node.js](https://nodejs.org) runtime.</span></span>
- <span data-ttu-id="06153-113">Telepítés [mysql2](https://www.npmjs.com/package/mysql2) tooconnect tooMySQL hello Node.js-alkalmazás a csomagot.</span><span class="sxs-lookup"><span data-stu-id="06153-113">Install [mysql2](https://www.npmjs.com/package/mysql2) package tooconnect tooMySQL from hello Node.js application.</span></span> 

## <a name="install-nodejs-and-hello-mysql-connector"></a><span data-ttu-id="06153-114">Telepítse a Node.js és hello MySQL-összekötő</span><span class="sxs-lookup"><span data-stu-id="06153-114">Install Node.js and hello MySQL connector</span></span>
<span data-ttu-id="06153-115">Attól függően, hogy a platform hello megfelelő utasításokat tooinstall Node.js kövesse.</span><span class="sxs-lookup"><span data-stu-id="06153-115">Depending on your platform, follow hello appropriate instructions tooinstall Node.js.</span></span> <span data-ttu-id="06153-116">Használja az npm tooinstall hello mysql2 csomagot és annak függőségeit a projekt mappába.</span><span class="sxs-lookup"><span data-stu-id="06153-116">Use npm tooinstall hello mysql2 package and its dependencies into your project folder.</span></span>

### <a name="windows"></a><span data-ttu-id="06153-117">**Windows**</span><span class="sxs-lookup"><span data-stu-id="06153-117">**Windows**</span></span>
1. <span data-ttu-id="06153-118">Látogasson el a hello [Node.js letölti lap](https://nodejs.org/en/download/) válassza ki a kívánt Windows installer lehetőségeket.</span><span class="sxs-lookup"><span data-stu-id="06153-118">Visit hello [Node.js downloads page](https://nodejs.org/en/download/) and select your desired Windows installer option.</span></span>
2. <span data-ttu-id="06153-119">Hozzon létre egy helyi projektmappát, például: `nodejsmysql`.</span><span class="sxs-lookup"><span data-stu-id="06153-119">Make a local project folder such as `nodejsmysql`.</span></span> 
3. <span data-ttu-id="06153-120">Indítsa el hello parancssort, és cd hello projekt mappába, mint`cd c:\nodejsmysql\`</span><span class="sxs-lookup"><span data-stu-id="06153-120">Launch hello command prompt and cd into hello project folder, such as `cd c:\nodejsmysql\`</span></span>
4. <span data-ttu-id="06153-121">Futtassa a hello NPM eszköz tooinstall hello mysql2 könyvtár hello projekt mappába.</span><span class="sxs-lookup"><span data-stu-id="06153-121">Run hello NPM tool tooinstall hello mysql2 library into hello project folder.</span></span>

   ```cmd
   cd c:\nodejsmysql\
   "C:\Program Files\nodejs\npm" install mysql2
   "C:\Program Files\nodejs\npm" list
   ```

5. <span data-ttu-id="06153-122">Ellenőrizze a hello telepítést hello ellenőrzésével `npm list` szövege kimeneti `mysql2@1.3.5`.</span><span class="sxs-lookup"><span data-stu-id="06153-122">Verify hello installation by checking hello `npm list` output text for `mysql2@1.3.5`.</span></span>

### <a name="linux-ubuntu"></a><span data-ttu-id="06153-123">**Linux (Ubuntu)**</span><span class="sxs-lookup"><span data-stu-id="06153-123">**Linux (Ubuntu)**</span></span>
1. <span data-ttu-id="06153-124">Futtatási hello következő parancsok tooinstall **Node.js** és **npm** hello Csomagkezelőt a Node.js.</span><span class="sxs-lookup"><span data-stu-id="06153-124">Run hello following commands tooinstall **Node.js** and **npm** hello package manager for Node.js.</span></span>

   ```bash
   sudo apt-get install -y nodejs npm
   ```

2. <span data-ttu-id="06153-125">Futtassa a következő parancsok toomake hello a projektmappa `mysqlnodejs` és hello mysql2 telepítéséhez ebbe a mappába.</span><span class="sxs-lookup"><span data-stu-id="06153-125">Run hello following commands toomake a project folder `mysqlnodejs` and install hello mysql2 package into that folder.</span></span>

   ```bash
   mkdir nodejsmysql
   cd nodejsmysql
   npm install --save mysql2
   npm list
   ```
3. <span data-ttu-id="06153-126">Hello telepítésének ellenőrzése npm lista kimeneti szövege ellenőrzésével `mysql2@1.3.5`.</span><span class="sxs-lookup"><span data-stu-id="06153-126">Verify hello installation by checking npm list output text for `mysql2@1.3.5`.</span></span>

### <a name="mac-os"></a><span data-ttu-id="06153-127">**Mac OS**</span><span class="sxs-lookup"><span data-stu-id="06153-127">**Mac OS**</span></span>
1. <span data-ttu-id="06153-128">Adja meg a következő parancsok tooinstall hello **brew**, egy könnyen használható Csomagkezelőt a Mac OS X és **Node.js**.</span><span class="sxs-lookup"><span data-stu-id="06153-128">Enter hello following commands tooinstall **brew**, an easy-to-use package manager for Mac OS X and **Node.js**.</span></span>

   ```bash
   ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
   brew install node
   ```
2. <span data-ttu-id="06153-129">Futtassa a következő parancsok toomake hello a projektmappa `mysqlnodejs` és hello mysql2 telepítéséhez ebbe a mappába.</span><span class="sxs-lookup"><span data-stu-id="06153-129">Run hello following commands toomake a project folder `mysqlnodejs` and install hello mysql2 package into that folder.</span></span>

   ```bash
   mkdir nodejsmysql
   cd nodejsmysql
   npm install --save mysql2
   npm list
   ```

3. <span data-ttu-id="06153-130">Ellenőrizze a hello telepítést hello ellenőrzésével `npm list` szövege kimeneti `mysql2@1.3.6`.</span><span class="sxs-lookup"><span data-stu-id="06153-130">Verify hello installation by checking hello `npm list` output text for `mysql2@1.3.6`.</span></span> <span data-ttu-id="06153-131">hello verziószáma változhat kiadott új javítások.</span><span class="sxs-lookup"><span data-stu-id="06153-131">hello version number may vary as new patches are released.</span></span>

## <a name="get-connection-information"></a><span data-ttu-id="06153-132">Kapcsolatadatok lekérése</span><span class="sxs-lookup"><span data-stu-id="06153-132">Get connection information</span></span>
<span data-ttu-id="06153-133">MySQL hello kapcsolat szükséges információkat tooconnect toohello Azure adatbázis beolvasása.</span><span class="sxs-lookup"><span data-stu-id="06153-133">Get hello connection information needed tooconnect toohello Azure Database for MySQL.</span></span> <span data-ttu-id="06153-134">Teljesen minősített kiszolgáló nevét és a bejelentkezési hitelesítő adatokat hello van szüksége.</span><span class="sxs-lookup"><span data-stu-id="06153-134">You need hello fully qualified server name and login credentials.</span></span>

1. <span data-ttu-id="06153-135">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="06153-135">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="06153-136">Hello bal oldali ablaktáblában kattintson **összes erőforrás**, majd keresse meg a létrehozott hello server (például **myserver4demo**).</span><span class="sxs-lookup"><span data-stu-id="06153-136">In hello left pane, click **All resources**, and then search for hello server you have created (for example, **myserver4demo**).</span></span>
3. <span data-ttu-id="06153-137">Hello kiszolgáló nevére kattint **myserver4demo**.</span><span class="sxs-lookup"><span data-stu-id="06153-137">Click hello server name **myserver4demo**.</span></span>
4. <span data-ttu-id="06153-138">Jelölje be hello server **tulajdonságok** lap.</span><span class="sxs-lookup"><span data-stu-id="06153-138">Select hello server's **Properties** page.</span></span> <span data-ttu-id="06153-139">Jegyezze fel a hello **kiszolgálónév** és **kiszolgálói rendszergazda bejelentkezési név**.</span><span class="sxs-lookup"><span data-stu-id="06153-139">Make a note of hello **Server name** and **Server admin login name**.</span></span>
 <span data-ttu-id="06153-140">![MySQL-hez készült Azure-adatbázis – Kiszolgáló-rendszergazdai bejelentkezés](./media/connect-nodejs/1_server-properties-name-login.png)</span><span class="sxs-lookup"><span data-stu-id="06153-140">![Azure Database for MySQL - Server Admin Login](./media/connect-nodejs/1_server-properties-name-login.png)</span></span>
5. <span data-ttu-id="06153-141">Ha elfelejti a kiszolgálói bejelentkezési adatok, keresse meg a toohello **áttekintése** tooview hello kiszolgálói rendszergazda bejelentkezési név lapon, és ha szükséges, állítsa vissza a hello jelszót.</span><span class="sxs-lookup"><span data-stu-id="06153-141">If you forget your server login information, navigate toohello **Overview** page tooview hello Server admin login name and, if necessary, reset hello password.</span></span>

## <a name="running-hello-javascript-code-in-nodejs"></a><span data-ttu-id="06153-142">A node.js hello JavaScript-kód futtatása</span><span class="sxs-lookup"><span data-stu-id="06153-142">Running hello JavaScript code in Node.js</span></span>
1. <span data-ttu-id="06153-143">Hello JavaScript-kód beillesztése szövegfájlok, és mentse a fájl kiterjesztése .js, például C:\nodejsmysql\createtable.js vagy /home/username/nodejsmysql/createtable.js projekt mappába</span><span class="sxs-lookup"><span data-stu-id="06153-143">Paste hello JavaScript code into text files, and save into a project folder with file extension .js, such as C:\nodejsmysql\createtable.js or /home/username/nodejsmysql/createtable.js</span></span>
2. <span data-ttu-id="06153-144">Indítsa el a hello parancssort, vagy a bash rendszerhéjat.</span><span class="sxs-lookup"><span data-stu-id="06153-144">Launch hello command prompt or bash shell.</span></span> <span data-ttu-id="06153-145">Módosítsa a könyvtárat a projektmappájára: `cd nodejsmysql`.</span><span class="sxs-lookup"><span data-stu-id="06153-145">Change directory into your project folder `cd nodejsmysql`.</span></span>
3. <span data-ttu-id="06153-146">toorun hello alkalmazás, írja be a hello csomópont parancsot, majd hello fájl nevét, például a `node createtable.js`.</span><span class="sxs-lookup"><span data-stu-id="06153-146">toorun hello application, type hello node command followed by hello file name, such as `node createtable.js`.</span></span>
4. <span data-ttu-id="06153-147">A Windows Ha hello csomópont alkalmazás nem szerepel a környezeti változó elérési útját, szükség lehet toouse hello teljes elérési útja toolaunch hello csomópont alkalmazás, például a`"C:\Program Files\nodejs\node.exe" createtable.js`</span><span class="sxs-lookup"><span data-stu-id="06153-147">On Windows, if hello node application is not in your environment variable path, you may need toouse hello full path toolaunch hello node application, such as `"C:\Program Files\nodejs\node.exe" createtable.js`</span></span>

## <a name="connect-create-table-and-insert-data"></a><span data-ttu-id="06153-148">Csatlakozás, táblák létrehozása és adatok beszúrása</span><span class="sxs-lookup"><span data-stu-id="06153-148">Connect, create table, and insert data</span></span>
<span data-ttu-id="06153-149">Használjon hello következő code tooconnect, és betölti a hello használatával végzett **CREATE TABLE** és **INSERT INTO** SQL-utasításokat.</span><span class="sxs-lookup"><span data-stu-id="06153-149">Use hello following code tooconnect and load hello data using **CREATE TABLE** and  **INSERT INTO** SQL statements.</span></span>

<span data-ttu-id="06153-150">Hello [mysql.createConnection()](https://github.com/mysqljs/mysql#establishing-connections) metódus használt toointerface hello MySQL-kiszolgálóval.</span><span class="sxs-lookup"><span data-stu-id="06153-150">hello [mysql.createConnection()](https://github.com/mysqljs/mysql#establishing-connections) method is used toointerface with hello MySQL server.</span></span> <span data-ttu-id="06153-151">Hello [csatlakozás](https://github.com/mysqljs/mysql#establishing-connections) függvény használt tooestablish hello kapcsolat toohello kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="06153-151">hello [connect()](https://github.com/mysqljs/mysql#establishing-connections) function is used tooestablish hello connection toohello server.</span></span> <span data-ttu-id="06153-152">Hello [query()](https://github.com/mysqljs/mysql#performing-queries) függvény használt tooexecute hello SQL-lekérdezést a MySQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="06153-152">hello [query()](https://github.com/mysqljs/mysql#performing-queries) function is used tooexecute hello SQL query against MySQL database.</span></span> 

<span data-ttu-id="06153-153">Cserélje le a hello `host`, `user`, `password`, és `database` paraméterek hello kiszolgáló és az adatbázis létrehozásakor adott hello értékekkel.</span><span class="sxs-lookup"><span data-stu-id="06153-153">Replace hello `host`, `user`, `password`, and `database` parameters with hello values that you specified when you created hello server and database.</span></span>

```javascript
const mysql = require('mysql2');

var config =
{
    host: 'myserver4demo.mysql.database.azure.com',
    user: 'myadmin@myserver4demo',
    password: 'your_password',
    database: 'quickstartdb',
    port: 3306,
    ssl: true
};

const conn = new mysql.createConnection(config);

conn.connect(
    function (err) { 
    if (err) { 
        console.log("!!! Cannot connect !!! Error:");
        throw err;
    }
    else
    {
       console.log("Connection established.");
           queryDatabase();
    }   
});

function queryDatabase(){
       conn.query('DROP TABLE IF EXISTS inventory;', function (err, results, fields) { 
            if (err) throw err; 
            console.log('Dropped inventory table if existed.');
        })
       conn.query('CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);', 
            function (err, results, fields) {
                if (err) throw err;
            console.log('Created inventory table.');
        })
       conn.query('INSERT INTO inventory (name, quantity) VALUES (?, ?);', ['banana', 150], 
            function (err, results, fields) {
                if (err) throw err;
            else console.log('Inserted ' + results.affectedRows + ' row(s).');
        })
       conn.query('INSERT INTO inventory (name, quantity) VALUES (?, ?);', ['orange', 154], 
            function (err, results, fields) {
                if (err) throw err;
            console.log('Inserted ' + results.affectedRows + ' row(s).');
        })
       conn.query('INSERT INTO inventory (name, quantity) VALUES (?, ?);', ['apple', 100], 
        function (err, results, fields) {
                if (err) throw err;
            console.log('Inserted ' + results.affectedRows + ' row(s).');
        })
       conn.end(function (err) { 
        if (err) throw err;
        else  console.log('Done.') 
        });
};
```

## <a name="read-data"></a><span data-ttu-id="06153-154">Adatok olvasása</span><span class="sxs-lookup"><span data-stu-id="06153-154">Read data</span></span>
<span data-ttu-id="06153-155">Használjon hello alábbi code tooconnect, és hello adatok segítségével olvassa a **kiválasztása** SQL-utasításban.</span><span class="sxs-lookup"><span data-stu-id="06153-155">Use hello following code tooconnect and read hello data using a **SELECT** SQL statement.</span></span> 

<span data-ttu-id="06153-156">Hello [mysql.createConnection()](https://github.com/mysqljs/mysql#establishing-connections) metódus használt toointerface hello MySQL-kiszolgálóval.</span><span class="sxs-lookup"><span data-stu-id="06153-156">hello [mysql.createConnection()](https://github.com/mysqljs/mysql#establishing-connections) method is used toointerface with hello MySQL server.</span></span> <span data-ttu-id="06153-157">Hello [csatlakozás](https://github.com/mysqljs/mysql#establishing-connections) metódus használt tooestablish hello kapcsolat toohello kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="06153-157">hello [connect()](https://github.com/mysqljs/mysql#establishing-connections) method is used tooestablish hello connection toohello server.</span></span> <span data-ttu-id="06153-158">Hello [query()](https://github.com/mysqljs/mysql#performing-queries) metódus használt tooexecute hello SQL-lekérdezést a MySQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="06153-158">hello [query()](https://github.com/mysqljs/mysql#performing-queries) method is used tooexecute hello SQL query against MySQL database.</span></span> <span data-ttu-id="06153-159">hello eredmények használt toohold hello hello lekérdezés eredményében.</span><span class="sxs-lookup"><span data-stu-id="06153-159">hello results array is used toohold hello results of hello query.</span></span>

<span data-ttu-id="06153-160">Cserélje le a hello `host`, `user`, `password`, és `database` paraméterek hello kiszolgáló és az adatbázis létrehozásakor adott hello értékekkel.</span><span class="sxs-lookup"><span data-stu-id="06153-160">Replace hello `host`, `user`, `password`, and `database` parameters with hello values that you specified when you created hello server and database.</span></span>

```javascript
const mysql = require('mysql2');

var config =
{
    host: 'myserver4demo.mysql.database.azure.com',
    user: 'myadmin@myserver4demo',
    password: 'your_password',
    database: 'quickstartdb',
    port: 3306,
    ssl: true
};

const conn = new mysql.createConnection(config);

conn.connect(
    function (err) { 
        if (err) { 
            console.log("!!! Cannot connect !!! Error:");
            throw err;
        }
        else {
            console.log("Connection established.");
            readData();
        }   
    });

function readData(){
        conn.query('SELECT * FROM inventory', 
            function (err, results, fields) {
                if (err) throw err;
                else console.log('Selected ' + results.length + ' row(s).');
                for (i = 0; i < results.length; i++) {
                    console.log('Row: ' + JSON.stringify(results[i]));
                }
                console.log('Done.');
            })
       conn.end(
           function (err) { 
                if (err) throw err;
                else  console.log('Closing connection.') 
        });
};
```

## <a name="update-data"></a><span data-ttu-id="06153-161">Adatok frissítése</span><span class="sxs-lookup"><span data-stu-id="06153-161">Update data</span></span>
<span data-ttu-id="06153-162">Használjon hello alábbi code tooconnect, és olvasott hello adatok egy **frissítés** SQL-utasításban.</span><span class="sxs-lookup"><span data-stu-id="06153-162">Use hello following code tooconnect and read hello data using a **UPDATE** SQL statement.</span></span> 

<span data-ttu-id="06153-163">Hello [mysql.createConnection()](https://github.com/mysqljs/mysql#establishing-connections) metódus használt toointerface hello MySQL-kiszolgálóval.</span><span class="sxs-lookup"><span data-stu-id="06153-163">hello [mysql.createConnection()](https://github.com/mysqljs/mysql#establishing-connections) method is used toointerface with hello MySQL server.</span></span> <span data-ttu-id="06153-164">Hello [csatlakozás](https://github.com/mysqljs/mysql#establishing-connections) metódus használt tooestablish hello kapcsolat toohello kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="06153-164">hello [connect()](https://github.com/mysqljs/mysql#establishing-connections) method is used tooestablish hello connection toohello server.</span></span> <span data-ttu-id="06153-165">Hello [query()](https://github.com/mysqljs/mysql#performing-queries) metódus használt tooexecute hello SQL-lekérdezést a MySQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="06153-165">hello [query()](https://github.com/mysqljs/mysql#performing-queries) method is used tooexecute hello SQL query against MySQL database.</span></span> 

<span data-ttu-id="06153-166">Cserélje le a hello `host`, `user`, `password`, és `database` paraméterek hello kiszolgáló és az adatbázis létrehozásakor adott hello értékekkel.</span><span class="sxs-lookup"><span data-stu-id="06153-166">Replace hello `host`, `user`, `password`, and `database` parameters with hello values that you specified when you created hello server and database.</span></span>

```javascript
const mysql = require('mysql2');

var config =
{
    host: 'myserver4demo.mysql.database.azure.com',
    user: 'myadmin@myserver4demo',
    password: 'your_password',
    database: 'quickstartdb',
    port: 3306,
    ssl: true
};

const conn = new mysql.createConnection(config);

conn.connect(
    function (err) { 
        if (err) { 
            console.log("!!! Cannot connect !!! Error:");
            throw err;
        }
        else {
            console.log("Connection established.");
            updateData();
        }   
    });

function updateData(){
       conn.query('UPDATE inventory SET quantity = ? WHERE name = ?', [200, 'banana'], 
            function (err, results, fields) {
                if (err) throw err;
                else console.log('Updated ' + results.affectedRows + ' row(s).');
        })
       conn.end(
           function (err) { 
                if (err) throw err;
                else  console.log('Done.') 
        });
};
```

## <a name="delete-data"></a><span data-ttu-id="06153-167">Adat törlése</span><span class="sxs-lookup"><span data-stu-id="06153-167">Delete data</span></span>
<span data-ttu-id="06153-168">Használjon hello alábbi code tooconnect, és olvasott hello adatok egy **törlése** SQL-utasításban.</span><span class="sxs-lookup"><span data-stu-id="06153-168">Use hello following code tooconnect and read hello data using a **DELETE** SQL statement.</span></span> 

<span data-ttu-id="06153-169">Hello [mysql.createConnection()](https://github.com/mysqljs/mysql#establishing-connections) metódus használt toointerface hello MySQL-kiszolgálóval.</span><span class="sxs-lookup"><span data-stu-id="06153-169">hello [mysql.createConnection()](https://github.com/mysqljs/mysql#establishing-connections) method is used toointerface with hello MySQL server.</span></span> <span data-ttu-id="06153-170">Hello [csatlakozás](https://github.com/mysqljs/mysql#establishing-connections) metódus használt tooestablish hello kapcsolat toohello kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="06153-170">hello [connect()](https://github.com/mysqljs/mysql#establishing-connections) method is used tooestablish hello connection toohello server.</span></span> <span data-ttu-id="06153-171">Hello [query()](https://github.com/mysqljs/mysql#performing-queries) metódus használt tooexecute hello SQL-lekérdezést a MySQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="06153-171">hello [query()](https://github.com/mysqljs/mysql#performing-queries) method is used tooexecute hello SQL query against MySQL database.</span></span> 

<span data-ttu-id="06153-172">Cserélje le a hello `host`, `user`, `password`, és `database` paraméterek hello kiszolgáló és az adatbázis létrehozásakor adott hello értékekkel.</span><span class="sxs-lookup"><span data-stu-id="06153-172">Replace hello `host`, `user`, `password`, and `database` parameters with hello values that you specified when you created hello server and database.</span></span>

```javascript
const mysql = require('mysql2');

var config =
{
    host: 'myserver4demo.mysql.database.azure.com',
    user: 'myadmin@myserver4demo',
    password: 'your_password',
    database: 'quickstartdb',
    port: 3306,
    ssl: true
};

const conn = new mysql.createConnection(config);

conn.connect(
    function (err) { 
        if (err) { 
            console.log("!!! Cannot connect !!! Error:");
            throw err;
        }
        else {
            console.log("Connection established.");
            deleteData();
        }   
    });

function deleteData(){
       conn.query('DELETE FROM inventory WHERE name = ?', ['orange'], 
            function (err, results, fields) {
                if (err) throw err;
                else console.log('Deleted ' + results.affectedRows + ' row(s).');
        })
       conn.end(
           function (err) { 
                if (err) throw err;
                else  console.log('Done.') 
        });
};
```

## <a name="next-steps"></a><span data-ttu-id="06153-173">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="06153-173">Next steps</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="06153-174">Adatbázis migrálása exportálással és importálással</span><span class="sxs-lookup"><span data-stu-id="06153-174">Migrate your database using Export and Import</span></span>](./concepts-migrate-import-export.md)
