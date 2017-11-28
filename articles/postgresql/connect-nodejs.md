---
title: "Node.js-ből PostgreSQL-tooAzure adatbázis kapcsolati |} Microsoft Docs"
description: "A gyors üzembe helyezés biztosít egy Node.js kódminta PostgreSQL lekérdezése a Azure-adatbázis adatait és tooconnect használhatja."
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.custom: mvc
ms.devlang: nodejs
ms.topic: quickstart
ms.date: 06/23/2017
ms.openlocfilehash: 9b269d72068ecc24bcf3fb447a2efeda512c698c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-database-for-postgresql-use-nodejs-tooconnect-and-query-data"></a><span data-ttu-id="f6115-103">Azure PostgreSQL-adatbázishoz: használata Node.js tooconnect és lekérdezési adatok</span><span class="sxs-lookup"><span data-stu-id="f6115-103">Azure Database for PostgreSQL: Use Node.js tooconnect and query data</span></span>
<span data-ttu-id="f6115-104">A gyors üzembe helyezés bemutatja, hogyan tooconnect tooan Azure adatbázis PostgreSQL használatára vonatkozó [Node.js](https://nodejs.org/).</span><span class="sxs-lookup"><span data-stu-id="f6115-104">This quickstart demonstrates how tooconnect tooan Azure Database for PostgreSQL using [Node.js](https://nodejs.org/).</span></span> <span data-ttu-id="f6115-105">Azt illusztrálja, hogyan toouse SQL utasítás tooquery beszúrási, frissítési és törlési hello adatbázis adatait.</span><span class="sxs-lookup"><span data-stu-id="f6115-105">It shows how toouse SQL statements tooquery, insert, update, and delete data in hello database.</span></span> <span data-ttu-id="f6115-106">hello cikkben leírt lépések azt feltételezik, hogy ismeri a Node.js használatával történő fejlesztéséhez, és, hogy-e új tooworking PostgreSQL az Azure-adatbázissal.</span><span class="sxs-lookup"><span data-stu-id="f6115-106">hello steps in this article assume that you are familiar with developing using Node.js, and that you are new tooworking with Azure Database for PostgreSQL.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f6115-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="f6115-107">Prerequisites</span></span>
<span data-ttu-id="f6115-108">A gyors üzembe helyezés kiindulási pontként ezek az útmutatók valamelyikével létrehozott hello erőforrást használ:</span><span class="sxs-lookup"><span data-stu-id="f6115-108">This quickstart uses hello resources created in either of these guides as a starting point:</span></span>
- [<span data-ttu-id="f6115-109">DB létrehozása – portál</span><span class="sxs-lookup"><span data-stu-id="f6115-109">Create DB - Portal</span></span>](quickstart-create-server-database-portal.md)
- [<span data-ttu-id="f6115-110">DB létrehozása – CLI</span><span class="sxs-lookup"><span data-stu-id="f6115-110">Create DB - CLI</span></span>](quickstart-create-server-database-azure-cli.md)

<span data-ttu-id="f6115-111">Emellett a következőket kell elvégezni:</span><span class="sxs-lookup"><span data-stu-id="f6115-111">You also need to:</span></span>
- <span data-ttu-id="f6115-112">[Node.js](https://nodejs.org) telepítése</span><span class="sxs-lookup"><span data-stu-id="f6115-112">Install [Node.js](https://nodejs.org)</span></span>

## <a name="install-pg-client"></a><span data-ttu-id="f6115-113">pg-ügyfél telepítése</span><span class="sxs-lookup"><span data-stu-id="f6115-113">Install pg client</span></span>
<span data-ttu-id="f6115-114">Telepítés [pg](https://www.npmjs.com/package/pg), ez a Node.js PostgreSQL-ügyfél.</span><span class="sxs-lookup"><span data-stu-id="f6115-114">Install [pg](https://www.npmjs.com/package/pg), which is a PostgreSQL client for Node.js.</span></span>

<span data-ttu-id="f6115-115">toodo tehát futtassa hello csomópont Csomagkezelő (npm) JavaScript a parancssor tooinstall hello pg ügyfélről.</span><span class="sxs-lookup"><span data-stu-id="f6115-115">toodo so, run hello node package manager (npm) for JavaScript from your command line tooinstall hello pg client.</span></span>
```bash
npm install pg
```

<span data-ttu-id="f6115-116">Ellenőrizze a hello telepítést hello telepített csomagok listáját.</span><span class="sxs-lookup"><span data-stu-id="f6115-116">Verify hello installation by listing hello packages installed.</span></span>
```bash
npm list
```

## <a name="get-connection-information"></a><span data-ttu-id="f6115-117">Kapcsolatadatok lekérése</span><span class="sxs-lookup"><span data-stu-id="f6115-117">Get connection information</span></span>
<span data-ttu-id="f6115-118">Hello kapcsolat szükséges információkat tooconnect toohello Azure adatbázis beolvasása PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="f6115-118">Get hello connection information needed tooconnect toohello Azure Database for PostgreSQL.</span></span> <span data-ttu-id="f6115-119">Teljesen minősített kiszolgáló nevét és a bejelentkezési hitelesítő adatokat hello van szüksége.</span><span class="sxs-lookup"><span data-stu-id="f6115-119">You need hello fully qualified server name and login credentials.</span></span>

1. <span data-ttu-id="f6115-120">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="f6115-120">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="f6115-121">A hello Azure-portálon a bal oldali menüből, kattintson az **összes erőforrás** , és keresse meg az imént létrehozott hello kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="f6115-121">From hello left-hand menu in Azure portal, click **All resources** and search for hello server you just created.</span></span>
3. <span data-ttu-id="f6115-122">Hello kiszolgáló nevére kattint.</span><span class="sxs-lookup"><span data-stu-id="f6115-122">Click hello server name.</span></span>
4. <span data-ttu-id="f6115-123">Jelölje be hello server **áttekintése** lap.</span><span class="sxs-lookup"><span data-stu-id="f6115-123">Select hello server's **Overview** page.</span></span> <span data-ttu-id="f6115-124">Jegyezze fel a hello **kiszolgálónév** és **kiszolgálói rendszergazda bejelentkezési név**.</span><span class="sxs-lookup"><span data-stu-id="f6115-124">Make a note of hello **Server name** and **Server admin login name**.</span></span>
 <span data-ttu-id="f6115-125">![PostgreSQL-hez készült Azure-adatbázis – Kiszolgáló-rendszergazdai bejelentkezés](./media/connect-nodejs/1-connection-string.png)</span><span class="sxs-lookup"><span data-stu-id="f6115-125">![Azure Database for PostgreSQL - Server Admin Login](./media/connect-nodejs/1-connection-string.png)</span></span>
5. <span data-ttu-id="f6115-126">Ha elfelejti a kiszolgálói bejelentkezési adatok, keresse meg a toohello **áttekintése** tooview hello kiszolgálói rendszergazda bejelentkezési név lapon, és ha szükséges, állítsa vissza a hello jelszót.</span><span class="sxs-lookup"><span data-stu-id="f6115-126">If you forget your server login information, navigate toohello **Overview** page tooview hello Server admin login name and, if necessary, reset hello password.</span></span>

## <a name="running-hello-javascript-code-in-nodejs"></a><span data-ttu-id="f6115-127">A node.js hello JavaScript-kód futtatása</span><span class="sxs-lookup"><span data-stu-id="f6115-127">Running hello JavaScript code in Node.js</span></span>
<span data-ttu-id="f6115-128">Előfordulhat, hogy indíthatja el Node.js hello bash rendszerhéjat vagy windows parancssorból történő beírásával `node`, majd hello például JavaScript-kód interaktív futtatását példány által, és alakzatot hello kérdés illeszti.</span><span class="sxs-lookup"><span data-stu-id="f6115-128">You may launch Node.js from hello bash shell or windows command prompt by typing `node`, then run hello example JavaScript code interactively by copy and pasting it onto hello prompt.</span></span> <span data-ttu-id="f6115-129">Azt is megteheti, előfordulhat, hogy mentse hello JavaScript-kódot egy szövegfájlt, és indítsa el az `node filename.js` nevű hello fájlban egy paraméter toorun azt.</span><span class="sxs-lookup"><span data-stu-id="f6115-129">Alternatively, you may save hello JavaScript code into a text file and launch `node filename.js` with hello file name as a parameter toorun it.</span></span>

## <a name="connect-create-table-and-insert-data"></a><span data-ttu-id="f6115-130">Csatlakozás, táblák létrehozása és adatok beszúrása</span><span class="sxs-lookup"><span data-stu-id="f6115-130">Connect, create table, and insert data</span></span>
<span data-ttu-id="f6115-131">Használjon hello következő code tooconnect, és betölti a hello használatával végzett **CREATE TABLE** és **INSERT INTO** SQL-utasításokat.</span><span class="sxs-lookup"><span data-stu-id="f6115-131">Use hello following code tooconnect and load hello data using **CREATE TABLE** and  **INSERT INTO** SQL statements.</span></span>
<span data-ttu-id="f6115-132">Hello [pg. Ügyfél](https://github.com/brianc/node-postgres/wiki/Client) objektum használt toointerface hello PostgreSQL-kiszolgálóval.</span><span class="sxs-lookup"><span data-stu-id="f6115-132">hello [pg.Client](https://github.com/brianc/node-postgres/wiki/Client) object is used toointerface with hello PostgreSQL server.</span></span> <span data-ttu-id="f6115-133">Hello [pg. Client.Connect()](https://github.com/brianc/node-postgres/wiki/Client#method-connect) függvény használt tooestablish hello kapcsolat toohello kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="f6115-133">hello [pg.Client.connect()](https://github.com/brianc/node-postgres/wiki/Client#method-connect) function is used tooestablish hello connection toohello server.</span></span> <span data-ttu-id="f6115-134">Hello [pg. Client.Query()](https://github.com/brianc/node-postgres/wiki/Query) függvény PostgreSQL-adatbázishoz használt tooexecute hello SQL-lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="f6115-134">hello [pg.Client.query()](https://github.com/brianc/node-postgres/wiki/Query) function is used tooexecute hello SQL query against PostgreSQL database.</span></span> 

<span data-ttu-id="f6115-135">Cserélje le a hello gazdagép, a dbname, a felhasználó és a password paraméter hello kiszolgáló és az adatbázis létrehozásakor adott hello értékekkel.</span><span class="sxs-lookup"><span data-stu-id="f6115-135">Replace hello host, dbname, user, and password parameters with hello values that you specified when you created hello server and database.</span></span>

```javascript
const pg = require('pg');

const config = {
    host: '<your-db-server-name>.postgres.database.azure.com',
    // Do not hard code your username and password.
    // Consider using Node environment variables.
    user: '<your-db-username>',     
    password: '<your-password>',
    database: '<name-of-database>',
    port: 5432,
    ssl: true
};

const client = new pg.Client(config);

client.connect(err => {
    if (err) throw err;
    else {
        queryDatabase();
    }
});

function queryDatabase() {
    const query = `
        DROP TABLE IF EXISTS inventory;
        CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);
        INSERT INTO inventory (name, quantity) VALUES ('banana', 150);
        INSERT INTO inventory (name, quantity) VALUES ('orange', 154);
        INSERT INTO inventory (name, quantity) VALUES ('apple', 100);
    `;

    client
        .query(query)
        .then(() => {
            console.log('Table created successfully!');
            client.end(console.log('Closed client connection'));
        })
        .catch(err => console.log(err))
        .then(() => {
            console.log('Finished execution, exiting now');
            process.exit();
        });
}
```

## <a name="read-data"></a><span data-ttu-id="f6115-136">Adatok olvasása</span><span class="sxs-lookup"><span data-stu-id="f6115-136">Read data</span></span>
<span data-ttu-id="f6115-137">Használjon hello alábbi code tooconnect, és hello adatok segítségével olvassa a **kiválasztása** SQL-utasításban.</span><span class="sxs-lookup"><span data-stu-id="f6115-137">Use hello following code tooconnect and read hello data using a **SELECT** SQL statement.</span></span> <span data-ttu-id="f6115-138">Hello [pg. Ügyfél](https://github.com/brianc/node-postgres/wiki/Client) objektum használt toointerface hello PostgreSQL-kiszolgálóval.</span><span class="sxs-lookup"><span data-stu-id="f6115-138">hello [pg.Client](https://github.com/brianc/node-postgres/wiki/Client) object is used toointerface with hello PostgreSQL server.</span></span> <span data-ttu-id="f6115-139">Hello [pg. Client.Connect()](https://github.com/brianc/node-postgres/wiki/Client#method-connect) függvény használt tooestablish hello kapcsolat toohello kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="f6115-139">hello [pg.Client.connect()](https://github.com/brianc/node-postgres/wiki/Client#method-connect) function is used tooestablish hello connection toohello server.</span></span> <span data-ttu-id="f6115-140">Hello [pg. Client.Query()](https://github.com/brianc/node-postgres/wiki/Query) függvény PostgreSQL-adatbázishoz használt tooexecute hello SQL-lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="f6115-140">hello [pg.Client.query()](https://github.com/brianc/node-postgres/wiki/Query) function is used tooexecute hello SQL query against PostgreSQL database.</span></span> 

<span data-ttu-id="f6115-141">Cserélje le a hello gazdagép, a dbname, a felhasználó és a password paraméter hello kiszolgáló és az adatbázis létrehozásakor adott hello értékekkel.</span><span class="sxs-lookup"><span data-stu-id="f6115-141">Replace hello host, dbname, user, and password parameters with hello values that you specified when you created hello server and database.</span></span> 

```javascript
const pg = require('pg');

const config = {
    host: '<your-db-server-name>.postgres.database.azure.com',
    // Do not hard code your username and password.
    // Consider using Node environment variables.
    user: '<your-db-username>',     
    password: '<your-password>',
    database: '<name-of-database>',
    port: 5432,
    ssl: true
};

const client = new pg.Client(config);

client.connect(err => {
    if (err) throw err;
    else { queryDatabase(); }
});

function queryDatabase() {
  
    console.log(`Running query tooPostgreSQL server: ${config.host}`);

    const query = 'SELECT * FROM inventory;';

    client.query(query)
        .then(res => {
            const rows = res.rows;

            rows.map(row => {
                console.log(`Read: ${JSON.stringify(row)}`);
            });

            process.exit();
        })
        .catch(err => {
            console.log(err);
        });
}
```

## <a name="update-data"></a><span data-ttu-id="f6115-142">Adatok frissítése</span><span class="sxs-lookup"><span data-stu-id="f6115-142">Update data</span></span>
<span data-ttu-id="f6115-143">Használjon hello alábbi code tooconnect, és olvasott hello adatok egy **frissítés** SQL-utasításban.</span><span class="sxs-lookup"><span data-stu-id="f6115-143">Use hello following code tooconnect and read hello data using a **UPDATE** SQL statement.</span></span> <span data-ttu-id="f6115-144">Hello [pg. Ügyfél](https://github.com/brianc/node-postgres/wiki/Client) objektum használt toointerface hello PostgreSQL-kiszolgálóval.</span><span class="sxs-lookup"><span data-stu-id="f6115-144">hello [pg.Client](https://github.com/brianc/node-postgres/wiki/Client) object is used toointerface with hello PostgreSQL server.</span></span> <span data-ttu-id="f6115-145">Hello [pg. Client.Connect()](https://github.com/brianc/node-postgres/wiki/Client#method-connect) függvény használt tooestablish hello kapcsolat toohello kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="f6115-145">hello [pg.Client.connect()](https://github.com/brianc/node-postgres/wiki/Client#method-connect) function is used tooestablish hello connection toohello server.</span></span> <span data-ttu-id="f6115-146">Hello [pg. Client.Query()](https://github.com/brianc/node-postgres/wiki/Query) függvény PostgreSQL-adatbázishoz használt tooexecute hello SQL-lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="f6115-146">hello [pg.Client.query()](https://github.com/brianc/node-postgres/wiki/Query) function is used tooexecute hello SQL query against PostgreSQL database.</span></span> 

<span data-ttu-id="f6115-147">Cserélje le a hello gazdagép, a dbname, a felhasználó és a password paraméter hello kiszolgáló és az adatbázis létrehozásakor adott hello értékekkel.</span><span class="sxs-lookup"><span data-stu-id="f6115-147">Replace hello host, dbname, user, and password parameters with hello values that you specified when you created hello server and database.</span></span> 

```javascript
const pg = require('pg');

const config = {
    host: '<your-db-server-name>.postgres.database.azure.com',
    // Do not hard code your username and password.
    // Consider using Node environment variables.
    user: '<your-db-username>',     
    password: '<your-password>',
    database: '<name-of-database>',
    port: 5432,
    ssl: true
};

const client = new pg.Client(config);

client.connect(err => {
    if (err) throw err;
    else {
        queryDatabase();
    }
});

function queryDatabase() {
    const query = `
        UPDATE inventory 
        SET quantity= 1000 WHERE name='banana';
    `;

    client
        .query(query)
        .then(result => {
            console.log('Update completed');
            console.log(`Rows affected: ${result.rowCount}`);
        })
        .catch(err => {
            console.log(err);
            throw err;
        });
}
```

## <a name="delete-data"></a><span data-ttu-id="f6115-148">Adat törlése</span><span class="sxs-lookup"><span data-stu-id="f6115-148">Delete data</span></span>
<span data-ttu-id="f6115-149">Használjon hello alábbi code tooconnect, és olvasott hello adatok egy **törlése** SQL-utasításban.</span><span class="sxs-lookup"><span data-stu-id="f6115-149">Use hello following code tooconnect and read hello data using a **DELETE** SQL statement.</span></span> <span data-ttu-id="f6115-150">Hello [pg. Ügyfél](https://github.com/brianc/node-postgres/wiki/Client) objektum használt toointerface hello PostgreSQL-kiszolgálóval.</span><span class="sxs-lookup"><span data-stu-id="f6115-150">hello [pg.Client](https://github.com/brianc/node-postgres/wiki/Client) object is used toointerface with hello PostgreSQL server.</span></span> <span data-ttu-id="f6115-151">Hello [pg. Client.Connect()](https://github.com/brianc/node-postgres/wiki/Client#method-connect) függvény használt tooestablish hello kapcsolat toohello kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="f6115-151">hello [pg.Client.connect()](https://github.com/brianc/node-postgres/wiki/Client#method-connect) function is used tooestablish hello connection toohello server.</span></span> <span data-ttu-id="f6115-152">Hello [pg. Client.Query()](https://github.com/brianc/node-postgres/wiki/Query) függvény PostgreSQL-adatbázishoz használt tooexecute hello SQL-lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="f6115-152">hello [pg.Client.query()](https://github.com/brianc/node-postgres/wiki/Query) function is used tooexecute hello SQL query against PostgreSQL database.</span></span> 

<span data-ttu-id="f6115-153">Cserélje le a hello gazdagép, a dbname, a felhasználó és a password paraméter hello kiszolgáló és az adatbázis létrehozásakor adott hello értékekkel.</span><span class="sxs-lookup"><span data-stu-id="f6115-153">Replace hello host, dbname, user, and password parameters with hello values that you specified when you created hello server and database.</span></span> 

```javascript
const pg = require('pg');

const config = {
    host: '<your-db-server-name>.postgres.database.azure.com',
    // Do not hard code your username and password.
    // Consider using Node environment variables.
    user: '<your-db-username>',     
    password: '<your-password>',
    database: '<name-of-database>',
    port: 5432,
    ssl: true
};

const client = new pg.Client(config);

client.connect(err => {
    if (err) {
        throw err;
    } else {
        queryDatabase();
    }
});

function queryDatabase() {
    const query = `
        DELETE FROM inventory 
        WHERE name = 'apple';
    `;

    client
        .query(query)
        .then(result => {
            console.log('Delete completed');
            console.log(`Rows affected: ${result.rowCount}`);
        })
        .catch(err => {
            console.log(err);
            throw err;
        });
}
```

## <a name="next-steps"></a><span data-ttu-id="f6115-154">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f6115-154">Next steps</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="f6115-155">Adatbázis migrálása exportálással és importálással</span><span class="sxs-lookup"><span data-stu-id="f6115-155">Migrate your database using Export and Import</span></span>](./howto-migrate-using-export-and-import.md)
