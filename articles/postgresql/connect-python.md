---
title: "A Python PostgreSQL-tooAzure adatbázis kapcsolati |} Microsoft Docs"
description: "A gyors üzembe helyezés biztosít egy Python kódminta, hogy tooconnect használ, és PostgreSQL lekérdezése a Azure-adatbázis adatait."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.custom: mvc
ms.devlang: python
ms.topic: quickstart
ms.date: 08/15/2017
ms.openlocfilehash: 7d6d9f5424fb39ad8837999d4788b4363c818887
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-database-for-postgresql-use-python-tooconnect-and-query-data"></a>Azure PostgreSQL-adatbázishoz: használata Python tooconnect és lekérdezési adatok
A gyors üzembe helyezés bemutatja, hogyan toouse [Python](https://python.org) tooconnect tooan PostgreSQL az Azure-adatbázis. Azt is bemutatja, hogyan toouse SQL utasítás tooquery beszúrási, frissítési és törlési hello adatbázisban macOS Ubuntu Linux és Windows-platform. Ez a cikk hello lépések azt feltételezik, ismeri a Python használatával történő fejlesztéséhez, és a rendszer új tooworking PostgreSQL az Azure-adatbázissal.

## <a name="prerequisites"></a>Előfeltételek
A gyors üzembe helyezés kiindulási pontként ezek az útmutatók valamelyikével létrehozott hello erőforrást használ:
- [DB létrehozása – portál](quickstart-create-server-database-portal.md)
- [DB létrehozása – CLI](quickstart-create-server-database-azure-cli.md)

Emellett a következőkre van szükség:
- telepített [python](https://www.python.org/downloads/)
- telepített [pip](https://pip.pypa.io/en/stable/installing/) csomag (a pip már telepítve van, ha a [python.org](https://python.org) webhelyről letöltött Python 2 >=2.7.9 vagy Python 3 >=3.4 bináris fájlokat használ.

## <a name="install-hello-python-connection-libraries-for-postgresql"></a>Hello Python adatkapcsolattárak PostgreSQL telepítése
Telepítse a hello [psycopg2](http://initd.org/psycopg/docs/install.html) csomag, amely lehetővé teszi tooconnect és lekérdezés hello adatbázis. psycopg2 van [PyPI elérhető](https://pypi.python.org/pypi/psycopg2/) hello formájában [kerék](http://pythonwheels.com/) csomagok hello leggyakoribb platformokhoz (Linux, os x, Windows). Használja a pip tooget hello bináris verzió hello modul, beleértve az összes hello függőségek telepítése.

1. A számítógépen indítsa el a parancssori felületet:
    - Linux indítsa el a hello Bash rendszerhéjat.
    - A macOS indítsa el a Terminálszolgáltatások hello.
    - A Windows indítsa el a hello parancssorból származó hello Start menüben.
2. Győződjön meg arról, hogy használ hello legújabb verziója a pip parancs futtatásával:
    ```cmd
    pip install -U pip
    ```

3. Futtassa a következő parancs tooinstall hello psycopg2 csomag hello:
    ```cmd
    pip install psycopg2
    ```

## <a name="get-connection-information"></a>Kapcsolatadatok lekérése
Hello kapcsolat szükséges információkat tooconnect toohello Azure adatbázis beolvasása PostgreSQL. Teljesen minősített kiszolgáló nevét és a bejelentkezési hitelesítő adatokat hello van szüksége.

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).
2. A hello Azure-portálon a bal oldali menüből, kattintson az **összes erőforrás** keresse meg a **mypgserver-20170401** (az imént létrehozott hello kiszolgáló).
3. Hello kiszolgáló nevére kattint **mypgserver-20170401**.
4. Jelölje be hello server **áttekintése** lapon, majd jegyezze fel a hello **kiszolgálónév** és **kiszolgálói rendszergazda bejelentkezési név**.
 ![PostgreSQL-hez készült Azure-adatbázis – Kiszolgáló-rendszergazdai bejelentkezés](./media/connect-python/1-connection-string.png)
5. Ha elfelejti a kiszolgálói bejelentkezési adatok, keresse meg a toohello **áttekintése** tooview hello kiszolgálói rendszergazda bejelentkezési név lapon, és ha szükséges, állítsa vissza a hello jelszót.

## <a name="how-toorun-python-code"></a>Hogyan toorun Python kódot
Ez a témakör összesen négy kódmintát tartalmaz, amelyek mindegyike egy adott funkciót hajt végre. hello következő utasítások jelzik, hogy hogyan toocreate egy szövegfájlt beszúrni a kódblokk, és mentse hello fájlt, így később is futtathatja. Meg arról, hogy toocreate négy külön fájlt, egy az egyes kódblokk lehet.

- Hozzon létre egy új fájlt a kedvenc szövegszerkesztőjével.
- Másolással illessze be egy hello Kódminták a következő részekben hello szövegfájlba hello. Cserélje le a hello **állomás**, **dbname**, **felhasználói**, és **jelszó** hello értékekkel hello létrehozásakor megadott paraméterek kiszolgáló és az adatbázis.
- A projekt mappába hello .py bővítmény (például postgres.py) mentse hello fájlt. Ha hello Windows operációs rendszer fut, lehet, hogy tooselect UTF-8 kódolást hello fájl mentése során. 
- Indítsa el a hello parancssor vagy a Bash rendszerhéjat, és módosítsa az hello könyvtár tooyour projekt mappájában, például `cd postgres`.
-  toorun hello kód típus hello Python parancsot, majd hello fájl nevét, például `Python postgres.py`.

> [!NOTE]
> A 3-as verziójú Python-től kezdődően jelenhetnek hello hiba `SyntaxError: Missing parentheses in call too'print'` a jelentés futtatásakor a következő kódblokkok hello. Ha ez előfordul, cserélje le minden hívás toohello parancs `print "string"` zárójeleket tartalmazhat, mint például használatával függvény hívással `print("string")`.

## <a name="connect-create-table-and-insert-data"></a>Csatlakozás, táblák létrehozása és adatok beszúrása
Használjon hello következő code tooconnect, és betölti a hello használatával végzett [psycopg2.connect](http://initd.org/psycopg/docs/connection.html) és **BESZÚRÁSA** SQL-utasításban. Hello [cursor.execute](http://initd.org/psycopg/docs/cursor.html#execute) függvény PostgreSQL-adatbázishoz használt tooexecute hello SQL-lekérdezést. Cserélje le a hello gazdagép, a dbname, a felhasználó és a password paraméter hello kiszolgáló és az adatbázis létrehozásakor adott hello értékekkel.

```Python
import psycopg2

# Update connection string information obtained from hello portal
host = "mypgserver-20170401.postgres.database.azure.com"
user = "mylogin@mypgserver-20170401"
dbname = "mypgsqldb"
password = "<server_admin_password>"
sslmode = "require"

# Construct connection string
conn_string = "host={0} user={1} dbname={2} password={3} sslmode={4}".format(host, user, dbname, password, sslmode)
conn = psycopg2.connect(conn_string) 
print "Connection established"

cursor = conn.cursor()

# Drop previous table of same name if one exists
cursor.execute("DROP TABLE IF EXISTS inventory;")
print "Finished dropping table (if existed)"

# Create table
cursor.execute("CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);")
print "Finished creating table"

# Insert some data into table
cursor.execute("INSERT INTO inventory (name, quantity) VALUES (%s, %s);", ("banana", 150))
cursor.execute("INSERT INTO inventory (name, quantity) VALUES (%s, %s);", ("orange", 154))
cursor.execute("INSERT INTO inventory (name, quantity) VALUES (%s, %s);", ("apple", 100))
print "Inserted 3 rows of data"

# Cleanup
conn.commit()
cursor.close()
conn.close()
```

Miután hello kód befejeződött, hello kimenete a következőképpen jelenik meg:

![Parancssori kimenet](media/connect-python/2-example-python-output.png)

## <a name="read-data"></a>Adatok olvasása
Használjon hello következő kódot tooread hello adatokat beszúrni a használatával [cursor.execute](http://initd.org/psycopg/docs/cursor.html#execute) működik az **VÁLASSZA** SQL-utasításban. Ezt a függvényt fogad el egy lekérdezést, és csak egy eredményt, amely képes többször keresztül hello használatával [cursor.fetchall()](http://initd.org/psycopg/docs/cursor.html#cursor.fetchall). Cserélje le a hello gazdagép, a dbname, a felhasználó és a password paraméter hello kiszolgáló és az adatbázis létrehozásakor adott hello értékekkel.

```Python
import psycopg2

# Update connection string information obtained from hello portal
host = "mypgserver-20170401.postgres.database.azure.com"
user = "mylogin@mypgserver-20170401"
dbname = "mypgsqldb"
password = "<server_admin_password>"
sslmode = "require"

# Construct connection string
conn_string = "host={0} user={1} dbname={2} password={3} sslmode={4}".format(host, user, dbname, password, sslmode)
conn = psycopg2.connect(conn_string) 
print "Connection established"

cursor = conn.cursor()

# Fetch all rows from table
cursor.execute("SELECT * FROM inventory;")
rows = cursor.fetchall()

# Print all rows
for row in rows:
    print "Data row = (%s, %s, %s)" %(str(row[0]), str(row[1]), str(row[2]))

# Cleanup
conn.commit()
cursor.close()
conn.close()
```

## <a name="update-data"></a>Adatok frissítése
Használjon hello következő tooupdate hello készlet sor előzőleg beszúrt használatával kódot [cursor.execute](http://initd.org/psycopg/docs/cursor.html#execute) és **frissítés** SQL-utasításban. Cserélje le a hello gazdagép, a dbname, a felhasználó és a password paraméter hello kiszolgáló és az adatbázis létrehozásakor adott hello értékekkel.

```Python
import psycopg2

# Update connection string information obtained from hello portal
host = "mypgserver-20170401.postgres.database.azure.com"
user = "mylogin@mypgserver-20170401"
dbname = "mypgsqldb"
password = "<server_admin_password>"
sslmode = "require"

# Construct connection string
conn_string = "host={0} user={1} dbname={2} password={3} sslmode={4}".format(host, user, dbname, password, sslmode)
conn = psycopg2.connect(conn_string) 
print "Connection established"

cursor = conn.cursor()

# Update a data row in hello table
cursor.execute("UPDATE inventory SET quantity = %s WHERE name = %s;", (200, "banana"))
print "Updated 1 row of data"

# Cleanup
conn.commit()
cursor.close()
conn.close()
```

## <a name="delete-data"></a>Adat törlése
Használjon hello következő toodelete korábban beszúrt használatával készlet elem kódot [cursor.execute](http://initd.org/psycopg/docs/cursor.html#execute) működik az **törlése** SQL-utasításban. Cserélje le a hello gazdagép, a dbname, a felhasználó és a password paraméter hello kiszolgáló és az adatbázis létrehozásakor adott hello értékekkel.

```Python
import psycopg2

# Update connection string information obtained from hello portal
host = "mypgserver-20170401.postgres.database.azure.com"
user = "mylogin@mypgserver-20170401"
dbname = "mypgsqldb"
password = "<server_admin_password>"
sslmode = "require"

# Construct connection string
conn_string = "host={0} user={1} dbname={2} password={3} sslmode={4}".format(host, user, dbname, password, sslmode)
conn = psycopg2.connect(conn_string) 
print "Connection established"

cursor = conn.cursor()

# Delete data row from table
cursor.execute("DELETE FROM inventory WHERE name = %s;", ("orange",))
print "Deleted 1 row of data"

# Cleanup
conn.commit()
cursor.close()
conn.close()
```

## <a name="next-steps"></a>Következő lépések
> [!div class="nextstepaction"]
> [Adatbázis migrálása exportálással és importálással](./howto-migrate-using-export-and-import.md)
