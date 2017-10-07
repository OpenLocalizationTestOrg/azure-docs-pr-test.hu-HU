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
# <a name="azure-database-for-mysql-use-nodejs-tooconnect-and-query-data"></a>MySQL az Azure-adatbázishoz: használata Node.js tooconnect és lekérdezési adatok
A gyors üzembe helyezés bemutatja, hogyan tooconnect tooan Azure adatbázis a MySQL használata [Node.js](https://nodejs.org/) a Windows, Ubuntu Linux és Mac rendszerek. Azt illusztrálja, hogyan toouse SQL utasítás tooquery beszúrási, frissítési és törlési hello adatbázis adatait. hello cikkben leírt lépések azt feltételezik, hogy ismeri a Node.js használatával történő fejlesztéséhez, és, hogy-e új tooworking MySQL az Azure-adatbázissal.

## <a name="prerequisites"></a>Előfeltételek
A gyors üzembe helyezés kiindulási pontként ezek az útmutatók valamelyikével létrehozott hello erőforrást használ:
- [Azure-adatbázis létrehozása MySQL-kiszolgálóhoz az Azure Portal használatával](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [Azure-adatbázis létrehozása MySQL-kiszolgálóhoz az Azure CLI használatával](./quickstart-create-mysql-server-database-using-azure-cli.md)

Emellett a következőket kell elvégezni:
- Telepítse a hello [Node.js](https://nodejs.org) futásidejű.
- Telepítés [mysql2](https://www.npmjs.com/package/mysql2) tooconnect tooMySQL hello Node.js-alkalmazás a csomagot. 

## <a name="install-nodejs-and-hello-mysql-connector"></a>Telepítse a Node.js és hello MySQL-összekötő
Attól függően, hogy a platform hello megfelelő utasításokat tooinstall Node.js kövesse. Használja az npm tooinstall hello mysql2 csomagot és annak függőségeit a projekt mappába.

### <a name="windows"></a>**Windows**
1. Látogasson el a hello [Node.js letölti lap](https://nodejs.org/en/download/) válassza ki a kívánt Windows installer lehetőségeket.
2. Hozzon létre egy helyi projektmappát, például: `nodejsmysql`. 
3. Indítsa el hello parancssort, és cd hello projekt mappába, mint`cd c:\nodejsmysql\`
4. Futtassa a hello NPM eszköz tooinstall hello mysql2 könyvtár hello projekt mappába.

   ```cmd
   cd c:\nodejsmysql\
   "C:\Program Files\nodejs\npm" install mysql2
   "C:\Program Files\nodejs\npm" list
   ```

5. Ellenőrizze a hello telepítést hello ellenőrzésével `npm list` szövege kimeneti `mysql2@1.3.5`.

### <a name="linux-ubuntu"></a>**Linux (Ubuntu)**
1. Futtatási hello következő parancsok tooinstall **Node.js** és **npm** hello Csomagkezelőt a Node.js.

   ```bash
   sudo apt-get install -y nodejs npm
   ```

2. Futtassa a következő parancsok toomake hello a projektmappa `mysqlnodejs` és hello mysql2 telepítéséhez ebbe a mappába.

   ```bash
   mkdir nodejsmysql
   cd nodejsmysql
   npm install --save mysql2
   npm list
   ```
3. Hello telepítésének ellenőrzése npm lista kimeneti szövege ellenőrzésével `mysql2@1.3.5`.

### <a name="mac-os"></a>**Mac OS**
1. Adja meg a következő parancsok tooinstall hello **brew**, egy könnyen használható Csomagkezelőt a Mac OS X és **Node.js**.

   ```bash
   ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
   brew install node
   ```
2. Futtassa a következő parancsok toomake hello a projektmappa `mysqlnodejs` és hello mysql2 telepítéséhez ebbe a mappába.

   ```bash
   mkdir nodejsmysql
   cd nodejsmysql
   npm install --save mysql2
   npm list
   ```

3. Ellenőrizze a hello telepítést hello ellenőrzésével `npm list` szövege kimeneti `mysql2@1.3.6`. hello verziószáma változhat kiadott új javítások.

## <a name="get-connection-information"></a>Kapcsolatadatok lekérése
MySQL hello kapcsolat szükséges információkat tooconnect toohello Azure adatbázis beolvasása. Teljesen minősített kiszolgáló nevét és a bejelentkezési hitelesítő adatokat hello van szüksége.

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).
2. Hello bal oldali ablaktáblában kattintson **összes erőforrás**, majd keresse meg a létrehozott hello server (például **myserver4demo**).
3. Hello kiszolgáló nevére kattint **myserver4demo**.
4. Jelölje be hello server **tulajdonságok** lap. Jegyezze fel a hello **kiszolgálónév** és **kiszolgálói rendszergazda bejelentkezési név**.
 ![MySQL-hez készült Azure-adatbázis – Kiszolgáló-rendszergazdai bejelentkezés](./media/connect-nodejs/1_server-properties-name-login.png)
5. Ha elfelejti a kiszolgálói bejelentkezési adatok, keresse meg a toohello **áttekintése** tooview hello kiszolgálói rendszergazda bejelentkezési név lapon, és ha szükséges, állítsa vissza a hello jelszót.

## <a name="running-hello-javascript-code-in-nodejs"></a>A node.js hello JavaScript-kód futtatása
1. Hello JavaScript-kód beillesztése szövegfájlok, és mentse a fájl kiterjesztése .js, például C:\nodejsmysql\createtable.js vagy /home/username/nodejsmysql/createtable.js projekt mappába
2. Indítsa el a hello parancssort, vagy a bash rendszerhéjat. Módosítsa a könyvtárat a projektmappájára: `cd nodejsmysql`.
3. toorun hello alkalmazás, írja be a hello csomópont parancsot, majd hello fájl nevét, például a `node createtable.js`.
4. A Windows Ha hello csomópont alkalmazás nem szerepel a környezeti változó elérési útját, szükség lehet toouse hello teljes elérési útja toolaunch hello csomópont alkalmazás, például a`"C:\Program Files\nodejs\node.exe" createtable.js`

## <a name="connect-create-table-and-insert-data"></a>Csatlakozás, táblák létrehozása és adatok beszúrása
Használjon hello következő code tooconnect, és betölti a hello használatával végzett **CREATE TABLE** és **INSERT INTO** SQL-utasításokat.

Hello [mysql.createConnection()](https://github.com/mysqljs/mysql#establishing-connections) metódus használt toointerface hello MySQL-kiszolgálóval. Hello [csatlakozás](https://github.com/mysqljs/mysql#establishing-connections) függvény használt tooestablish hello kapcsolat toohello kiszolgáló. Hello [query()](https://github.com/mysqljs/mysql#performing-queries) függvény használt tooexecute hello SQL-lekérdezést a MySQL-adatbázis. 

Cserélje le a hello `host`, `user`, `password`, és `database` paraméterek hello kiszolgáló és az adatbázis létrehozásakor adott hello értékekkel.

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

## <a name="read-data"></a>Adatok olvasása
Használjon hello alábbi code tooconnect, és hello adatok segítségével olvassa a **kiválasztása** SQL-utasításban. 

Hello [mysql.createConnection()](https://github.com/mysqljs/mysql#establishing-connections) metódus használt toointerface hello MySQL-kiszolgálóval. Hello [csatlakozás](https://github.com/mysqljs/mysql#establishing-connections) metódus használt tooestablish hello kapcsolat toohello kiszolgáló. Hello [query()](https://github.com/mysqljs/mysql#performing-queries) metódus használt tooexecute hello SQL-lekérdezést a MySQL-adatbázis. hello eredmények használt toohold hello hello lekérdezés eredményében.

Cserélje le a hello `host`, `user`, `password`, és `database` paraméterek hello kiszolgáló és az adatbázis létrehozásakor adott hello értékekkel.

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

## <a name="update-data"></a>Adatok frissítése
Használjon hello alábbi code tooconnect, és olvasott hello adatok egy **frissítés** SQL-utasításban. 

Hello [mysql.createConnection()](https://github.com/mysqljs/mysql#establishing-connections) metódus használt toointerface hello MySQL-kiszolgálóval. Hello [csatlakozás](https://github.com/mysqljs/mysql#establishing-connections) metódus használt tooestablish hello kapcsolat toohello kiszolgáló. Hello [query()](https://github.com/mysqljs/mysql#performing-queries) metódus használt tooexecute hello SQL-lekérdezést a MySQL-adatbázis. 

Cserélje le a hello `host`, `user`, `password`, és `database` paraméterek hello kiszolgáló és az adatbázis létrehozásakor adott hello értékekkel.

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

## <a name="delete-data"></a>Adat törlése
Használjon hello alábbi code tooconnect, és olvasott hello adatok egy **törlése** SQL-utasításban. 

Hello [mysql.createConnection()](https://github.com/mysqljs/mysql#establishing-connections) metódus használt toointerface hello MySQL-kiszolgálóval. Hello [csatlakozás](https://github.com/mysqljs/mysql#establishing-connections) metódus használt tooestablish hello kapcsolat toohello kiszolgáló. Hello [query()](https://github.com/mysqljs/mysql#performing-queries) metódus használt tooexecute hello SQL-lekérdezést a MySQL-adatbázis. 

Cserélje le a hello `host`, `user`, `password`, és `database` paraméterek hello kiszolgáló és az adatbázis létrehozásakor adott hello értékekkel.

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

## <a name="next-steps"></a>Következő lépések
> [!div class="nextstepaction"]
> [Adatbázis migrálása exportálással és importálással](./concepts-migrate-import-export.md)
