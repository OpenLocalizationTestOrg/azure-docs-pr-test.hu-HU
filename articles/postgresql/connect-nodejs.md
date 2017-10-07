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
# <a name="azure-database-for-postgresql-use-nodejs-tooconnect-and-query-data"></a>Azure PostgreSQL-adatbázishoz: használata Node.js tooconnect és lekérdezési adatok
A gyors üzembe helyezés bemutatja, hogyan tooconnect tooan Azure adatbázis PostgreSQL használatára vonatkozó [Node.js](https://nodejs.org/). Azt illusztrálja, hogyan toouse SQL utasítás tooquery beszúrási, frissítési és törlési hello adatbázis adatait. hello cikkben leírt lépések azt feltételezik, hogy ismeri a Node.js használatával történő fejlesztéséhez, és, hogy-e új tooworking PostgreSQL az Azure-adatbázissal.

## <a name="prerequisites"></a>Előfeltételek
A gyors üzembe helyezés kiindulási pontként ezek az útmutatók valamelyikével létrehozott hello erőforrást használ:
- [DB létrehozása – portál](quickstart-create-server-database-portal.md)
- [DB létrehozása – CLI](quickstart-create-server-database-azure-cli.md)

Emellett a következőket kell elvégezni:
- [Node.js](https://nodejs.org) telepítése

## <a name="install-pg-client"></a>pg-ügyfél telepítése
Telepítés [pg](https://www.npmjs.com/package/pg), ez a Node.js PostgreSQL-ügyfél.

toodo tehát futtassa hello csomópont Csomagkezelő (npm) JavaScript a parancssor tooinstall hello pg ügyfélről.
```bash
npm install pg
```

Ellenőrizze a hello telepítést hello telepített csomagok listáját.
```bash
npm list
```

## <a name="get-connection-information"></a>Kapcsolatadatok lekérése
Hello kapcsolat szükséges információkat tooconnect toohello Azure adatbázis beolvasása PostgreSQL. Teljesen minősített kiszolgáló nevét és a bejelentkezési hitelesítő adatokat hello van szüksége.

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).
2. A hello Azure-portálon a bal oldali menüből, kattintson az **összes erőforrás** , és keresse meg az imént létrehozott hello kiszolgáló.
3. Hello kiszolgáló nevére kattint.
4. Jelölje be hello server **áttekintése** lap. Jegyezze fel a hello **kiszolgálónév** és **kiszolgálói rendszergazda bejelentkezési név**.
 ![PostgreSQL-hez készült Azure-adatbázis – Kiszolgáló-rendszergazdai bejelentkezés](./media/connect-nodejs/1-connection-string.png)
5. Ha elfelejti a kiszolgálói bejelentkezési adatok, keresse meg a toohello **áttekintése** tooview hello kiszolgálói rendszergazda bejelentkezési név lapon, és ha szükséges, állítsa vissza a hello jelszót.

## <a name="running-hello-javascript-code-in-nodejs"></a>A node.js hello JavaScript-kód futtatása
Előfordulhat, hogy indíthatja el Node.js hello bash rendszerhéjat vagy windows parancssorból történő beírásával `node`, majd hello például JavaScript-kód interaktív futtatását példány által, és alakzatot hello kérdés illeszti. Azt is megteheti, előfordulhat, hogy mentse hello JavaScript-kódot egy szövegfájlt, és indítsa el az `node filename.js` nevű hello fájlban egy paraméter toorun azt.

## <a name="connect-create-table-and-insert-data"></a>Csatlakozás, táblák létrehozása és adatok beszúrása
Használjon hello következő code tooconnect, és betölti a hello használatával végzett **CREATE TABLE** és **INSERT INTO** SQL-utasításokat.
Hello [pg. Ügyfél](https://github.com/brianc/node-postgres/wiki/Client) objektum használt toointerface hello PostgreSQL-kiszolgálóval. Hello [pg. Client.Connect()](https://github.com/brianc/node-postgres/wiki/Client#method-connect) függvény használt tooestablish hello kapcsolat toohello kiszolgáló. Hello [pg. Client.Query()](https://github.com/brianc/node-postgres/wiki/Query) függvény PostgreSQL-adatbázishoz használt tooexecute hello SQL-lekérdezést. 

Cserélje le a hello gazdagép, a dbname, a felhasználó és a password paraméter hello kiszolgáló és az adatbázis létrehozásakor adott hello értékekkel.

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

## <a name="read-data"></a>Adatok olvasása
Használjon hello alábbi code tooconnect, és hello adatok segítségével olvassa a **kiválasztása** SQL-utasításban. Hello [pg. Ügyfél](https://github.com/brianc/node-postgres/wiki/Client) objektum használt toointerface hello PostgreSQL-kiszolgálóval. Hello [pg. Client.Connect()](https://github.com/brianc/node-postgres/wiki/Client#method-connect) függvény használt tooestablish hello kapcsolat toohello kiszolgáló. Hello [pg. Client.Query()](https://github.com/brianc/node-postgres/wiki/Query) függvény PostgreSQL-adatbázishoz használt tooexecute hello SQL-lekérdezést. 

Cserélje le a hello gazdagép, a dbname, a felhasználó és a password paraméter hello kiszolgáló és az adatbázis létrehozásakor adott hello értékekkel. 

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

## <a name="update-data"></a>Adatok frissítése
Használjon hello alábbi code tooconnect, és olvasott hello adatok egy **frissítés** SQL-utasításban. Hello [pg. Ügyfél](https://github.com/brianc/node-postgres/wiki/Client) objektum használt toointerface hello PostgreSQL-kiszolgálóval. Hello [pg. Client.Connect()](https://github.com/brianc/node-postgres/wiki/Client#method-connect) függvény használt tooestablish hello kapcsolat toohello kiszolgáló. Hello [pg. Client.Query()](https://github.com/brianc/node-postgres/wiki/Query) függvény PostgreSQL-adatbázishoz használt tooexecute hello SQL-lekérdezést. 

Cserélje le a hello gazdagép, a dbname, a felhasználó és a password paraméter hello kiszolgáló és az adatbázis létrehozásakor adott hello értékekkel. 

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

## <a name="delete-data"></a>Adat törlése
Használjon hello alábbi code tooconnect, és olvasott hello adatok egy **törlése** SQL-utasításban. Hello [pg. Ügyfél](https://github.com/brianc/node-postgres/wiki/Client) objektum használt toointerface hello PostgreSQL-kiszolgálóval. Hello [pg. Client.Connect()](https://github.com/brianc/node-postgres/wiki/Client#method-connect) függvény használt tooestablish hello kapcsolat toohello kiszolgáló. Hello [pg. Client.Query()](https://github.com/brianc/node-postgres/wiki/Query) függvény PostgreSQL-adatbázishoz használt tooexecute hello SQL-lekérdezést. 

Cserélje le a hello gazdagép, a dbname, a felhasználó és a password paraméter hello kiszolgáló és az adatbázis létrehozásakor adott hello értékekkel. 

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

## <a name="next-steps"></a>Következő lépések
> [!div class="nextstepaction"]
> [Adatbázis migrálása exportálással és importálással](./howto-migrate-using-export-and-import.md)
