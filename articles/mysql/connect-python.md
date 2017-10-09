---
title: "A Python MySQL-adatbázis tooAzure kapcsolati |} Microsoft Docs"
description: "A gyors üzembe helyezés biztosít több Python Kódminták MySQL Azure-adatbázis tooconnect és lekérdezés adatait is használhatja."
services: mysql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.custom: mvc
ms.devlang: python
ms.topic: hero-article
ms.date: 07/12/2017
ms.openlocfilehash: 9df5211adcab886a502fd138347aed8fb587cd5c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-database-for-mysql-use-python-tooconnect-and-query-data"></a>MySQL az Azure-adatbázishoz: használata Python tooconnect és lekérdezési adatok
A gyors üzembe helyezés bemutatja, hogyan toouse [Python](https://python.org) tooconnect tooan MySQL az Azure-adatbázis. A Mac OS, az Ubuntu Linux és a Windows platformokon hello adatbázisban SQL utasítás tooquery, insert, update és adatok használ. Ez a cikk hello lépések azt feltételezik, ismeri a Python használatával történő fejlesztéséhez, és a rendszer új tooworking MySQL az Azure-adatbázissal.

## <a name="prerequisites"></a>Előfeltételek
A gyors üzembe helyezés kiindulási pontként ezek az útmutatók valamelyikével létrehozott hello erőforrást használ:
- [Azure-adatbázis létrehozása MySQL-kiszolgálóhoz az Azure Portal használatával](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [Azure-adatbázis létrehozása MySQL-kiszolgálóhoz az Azure CLI használatával](./quickstart-create-mysql-server-database-using-azure-cli.md)

## <a name="install-python-and-hello-mysql-connector"></a>Telepítse a Python és hello MySQL-összekötő
Telepítés [Python](https://www.python.org/downloads/) és hello [Python MySQL-összekötő](https://dev.mysql.com/downloads/connector/python/) a saját számítógépén. Attól függően, hogy a platform a hello lépésekkel:

### <a name="windows"></a>Windows
1. Töltse le és telepítse a Python 2.7-es verziót a [python.org](https://www.python.org/downloads/windows/) webhelyről. 
2. Ellenőrizze a hello Python telepítését hello parancssorból indítsa el. Hello paranccsal `C:\python27\python.exe -V` hello nagybetűs V kapcsoló toosee hello verziószám használatával.
3. A MySQL hello Python-összekötő telepítése [mysql.com](https://dev.mysql.com/downloads/connector/python/) Python megfelelő tooyour verziója.

### <a name="linux-ubuntu"></a>Linux (Ubuntu)
1. Linux (Ubuntu), a Python általában hello alapértelmezett telepítés részeként telepíthető.
2. Ellenőrizze a hello Python telepítését hello rendszerhéjakba indításával. Hello paranccsal `python -V` hello nagybetűs V kapcsoló toosee hello verziószám használatával.
3. Ellenőrizze a hello PIP telepítési hello futtatásával `pip show pip -V` toosee hello verziószám parancsot. 
4. A PIP szerepelhet a Python egyes verzióiban. Ha a PIP nincs telepítve, előfordulhat, hogy telepítése hello [PIP] (https://pip.pypa.io/en/stable/installing/) csomag, a parancs futtatásával `sudo apt-get install python-pip`.
5. Frissítés a PIP toohello legújabb verzió hello futtatásával `pip install -U pip` parancsot.
6. Hello MySQL-összekötő telepítése a Python és annak függőségeit hello PIP parancs használatával:

   ```bash
   sudo pip install mysql-connector-python-rf
   ```
 
### <a name="macos"></a>MacOS
1. A Mac OS Python általában telepítve hello alapértelmezett operációs rendszer telepítésének részeként.
2. Ellenőrizze a hello Python telepítését hello rendszerhéjakba indításával. Hello paranccsal `python -V` hello nagybetűs V kapcsoló toosee hello verziószám használatával.
3. Ellenőrizze a hello PIP telepítési hello futtatásával `pip show pip -V` toosee hello verziószám parancsot.
4. A PIP szerepelhet a Python egyes verzióiban. Ha nincs telepítve a PIP, előfordulhat, hogy telepítése hello [PIP](https://pip.pypa.io/en/stable/installing/) csomag.
5. Frissítés a PIP toohello legújabb verzió hello futtatásával `pip install -U pip` parancsot.
6. Hello MySQL-összekötő telepítése a Python és annak függőségeit hello PIP parancs használatával:

   ```bash
   pip install mysql-connector-python-rf
   ```

## <a name="get-connection-information"></a>Kapcsolatadatok lekérése
MySQL hello kapcsolat szükséges információkat tooconnect toohello Azure adatbázis beolvasása. Teljesen minősített kiszolgáló nevét és a bejelentkezési hitelesítő adatokat hello van szüksége.

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).
2. A hello Azure-portálon a bal oldali menüből, kattintson az **összes erőforrás** , és keresse meg rendelkezik gyűrött, például a hello kiszolgáló **myserver4demo**.
3. Hello kiszolgáló nevére kattint **myserver4demo**.
4. Jelölje be hello server **tulajdonságok** lap. Jegyezze fel a hello **kiszolgálónév** és **kiszolgálói rendszergazda bejelentkezési név**.
 ![MySQL-hez készült Azure-adatbázis – Kiszolgáló-rendszergazdai bejelentkezés](./media/connect-python/1_server-properties-name-login.png)
5. Ha elfelejti a kiszolgálói bejelentkezési adatok, keresse meg a toohello **áttekintése** tooview hello kiszolgálói rendszergazda bejelentkezési név lapon, és ha szükséges, állítsa vissza a hello jelszót.
   

## <a name="run-python-code"></a>A Python-kód futtatása
- Hello kód beillesztése egy szövegfájlt, és mentse a hello fájlt a fájl kiterjesztése .py, például C:\pythonmysql\createtable.py vagy /home/username/pythonmysql/createtable.py projekt mappába
- toorun hello kódot, indítsa el a hello parancssor, vagy a bash rendszerhéjat. Módosítsa a könyvtárat a projektmappájára: `cd pythonmysql`. Írja be hello python parancsot hello fájl neve követ `python createtable.py` toorun hello alkalmazás. A Windows operációs rendszer hello, python.exe nem található, ha, előfordulhat, hogy adjon meg hello teljes elérési útja toohello végrehajtható, vagy hello Python elérési út hozzáadása a hello path környezeti változóba. `C:\python27\python.exe createtable.py`

## <a name="connect-create-table-and-insert-data"></a>Csatlakozás, táblák létrehozása és adatok beszúrása
Használja hello alábbi tooconnect toohello server code, hozzon létre egy táblát, és hello használatával végzett betölteni egy **BESZÚRÁSA** SQL-utasításban. 

Hello kódban hello mysql.connector könyvtár importálva van. Hello [csatlakozás](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysql-connector-connect.html) függvény használt tooconnect tooAzure hello segítségével MySQL adatbázis [kapcsolat argumentumok](https://dev.mysql.com/doc/connector-python/en/connector-python-connectargs.html) hello config gyűjteményben. hello használ a kurzor hello kapcsolaton, és [cursor.execute()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-execute.html) metódus elindít hello MySQL-adatbázis SQL-lekérdezést. 

Cserélje le a hello `host`, `user`, `password`, és `database` paraméterek hello kiszolgáló és az adatbázis létrehozásakor adott hello értékekkel.

```Python
import mysql.connector
from mysql.connector import errorcode

# Obtain connection string information from hello portal
config = {
  'host':'myserver4demo.mysql.database.azure.com',
  'user':'myadmin@myserver4demo',
  'password':'yourpassword',
  'database':'quickstartdb'
}

# Construct connection string
try:
   conn = mysql.connector.connect(**config)
   print("Connection established")
except mysql.connector.Error as err:
  if err.errno == errorcode.ER_ACCESS_DENIED_ERROR:
    print("Something is wrong with hello user name or password")
  elif err.errno == errorcode.ER_BAD_DB_ERROR:
    print("Database does not exist")
  else:
    print(err)
else:
  cursor = conn.cursor()

  # Drop previous table of same name if one exists
  cursor.execute("DROP TABLE IF EXISTS inventory;")
  print("Finished dropping table (if existed).")

  # Create table
  cursor.execute("CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);")
  print("Finished creating table.")

  # Insert some data into table
  cursor.execute("INSERT INTO inventory (name, quantity) VALUES (%s, %s);", ("banana", 150))
  print("Inserted",cursor.rowcount,"row(s) of data.")
  cursor.execute("INSERT INTO inventory (name, quantity) VALUES (%s, %s);", ("orange", 154))
  print("Inserted",cursor.rowcount,"row(s) of data.")
  cursor.execute("INSERT INTO inventory (name, quantity) VALUES (%s, %s);", ("apple", 100))
  print("Inserted",cursor.rowcount,"row(s) of data.")

  # Cleanup
  conn.commit()
  cursor.close()
  conn.close()
  print("Done.")
```

## <a name="read-data"></a>Adatok olvasása
Használjon hello alábbi code tooconnect, és hello adatok segítségével olvassa a **kiválasztása** SQL-utasításban. 

Hello kódban hello mysql.connector könyvtár importálva van. Hello [csatlakozás](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysql-connector-connect.html) függvény használt tooconnect tooAzure hello segítségével MySQL adatbázis [kapcsolat argumentumok](https://dev.mysql.com/doc/connector-python/en/connector-python-connectargs.html) hello config gyűjteményben. hello használ a kurzor hello kapcsolaton, és [cursor.execute()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-execute.html) metódus elindít hello MySQL-adatbázis SQL-utasításban. hello adatsorok hello olvassa el [fetchall()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-fetchall.html) metódust. hello eredménykészlet tartják gyűjtemény egymás és egy iterátor használt tooloop: hello sorok fölé.

Cserélje le a hello `host`, `user`, `password`, és `database` paraméterek hello kiszolgáló és az adatbázis létrehozásakor adott hello értékekkel.

```Python
import mysql.connector
from mysql.connector import errorcode

# Obtain connection string information from hello portal
config = {
  'host':'myserver4demo.mysql.database.azure.com',
  'user':'myadmin@myserver4demo',
  'password':'yourpassword',
  'database':'quickstartdb'
}

# Construct connection string
try:
   conn = mysql.connector.connect(**config)
   print("Connection established")
except mysql.connector.Error as err:
  if err.errno == errorcode.ER_ACCESS_DENIED_ERROR:
    print("Something is wrong with hello user name or password")
  elif err.errno == errorcode.ER_BAD_DB_ERROR:
    print("Database does not exist")
  else:
    print(err)
else:
  cursor = conn.cursor()

  # Read data
  cursor.execute("SELECT * FROM inventory;")
  rows = cursor.fetchall()
  print("Read",cursor.rowcount,"row(s) of data.")

  # Print all rows
  for row in rows:
    print("Data row = (%s, %s, %s)" %(str(row[0]), str(row[1]), str(row[2])))

  # Cleanup
  conn.commit()
  cursor.close()
  conn.close()
  print("Done.")
```

## <a name="update-data"></a>Adatok frissítése
Használjon hello következő code tooconnect, és frissítse a hello adatok segítségével egy **frissítése** SQL-utasításban. 

Hello kódban hello mysql.connector könyvtár importálva van.  Hello [csatlakozás](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysql-connector-connect.html) függvény használt tooconnect tooAzure hello segítségével MySQL adatbázis [kapcsolat argumentumok](https://dev.mysql.com/doc/connector-python/en/connector-python-connectargs.html) hello config gyűjteményben. hello használ a kurzor hello kapcsolaton, és [cursor.execute()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-execute.html) metódus elindít hello MySQL-adatbázis SQL-utasításban. 

Cserélje le a hello `host`, `user`, `password`, és `database` paraméterek hello kiszolgáló és az adatbázis létrehozásakor adott hello értékekkel.

```Python
import mysql.connector
from mysql.connector import errorcode

# Obtain connection string information from hello portal
config = {
  'host':'myserver4demo.mysql.database.azure.com',
  'user':'myadmin@myserver4demo',
  'password':'yourpassword',
  'database':'quickstartdb'
}

# Construct connection string
try:
   conn = mysql.connector.connect(**config)
   print("Connection established")
except mysql.connector.Error as err:
  if err.errno == errorcode.ER_ACCESS_DENIED_ERROR:
    print("Something is wrong with hello user name or password")
  elif err.errno == errorcode.ER_BAD_DB_ERROR:
    print("Database does not exist")
  else:
    print(err)
else:
  cursor = conn.cursor()

  # Update a data row in hello table
  cursor.execute("UPDATE inventory SET quantity = %s WHERE name = %s;", (200, "banana"))
  print("Updated",cursor.rowcount,"row(s) of data.")

  # Cleanup
  conn.commit()
  cursor.close()
  conn.close()
  print("Done.")
```

## <a name="delete-data"></a>Adat törlése
Használjon hello alábbi code tooconnect, és távolítsa el az adatokat egy **törlése** SQL-utasításban. 

Hello kódban hello mysql.connector könyvtár importálva van.  Hello [csatlakozás](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysql-connector-connect.html) függvény használt tooconnect tooAzure hello segítségével MySQL adatbázis [kapcsolat argumentumok](https://dev.mysql.com/doc/connector-python/en/connector-python-connectargs.html) hello config gyűjteményben. hello használ a kurzor hello kapcsolaton, és [cursor.execute()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-execute.html) metódus elindít hello MySQL-adatbázis SQL-lekérdezést. 

Cserélje le a hello `host`, `user`, `password`, és `database` paraméterek hello kiszolgáló és az adatbázis létrehozásakor adott hello értékekkel.

```Python
import mysql.connector
from mysql.connector import errorcode

# Obtain connection string information from hello portal
config = {
  'host':'myserver4demo.mysql.database.azure.com',
  'user':'myadmin@myserver4demo',
  'password':'yourpassword',
  'database':'quickstartdb'
}

# Construct connection string
try:
   conn = mysql.connector.connect(**config)
   print("Connection established.")
except mysql.connector.Error as err:
  if err.errno == errorcode.ER_ACCESS_DENIED_ERROR:
    print("Something is wrong with hello user name or password.")
  elif err.errno == errorcode.ER_BAD_DB_ERROR:
    print("Database does not exist.")
  else:
    print(err)
else:
  cursor = conn.cursor()

  # Delete a data row in hello table
  cursor.execute("DELETE FROM inventory WHERE name=%(param1)s;", {'param1':"orange"})
  print("Deleted",cursor.rowcount,"row(s) of data.")

  # Cleanup
  conn.commit()
  cursor.close()
  conn.close()
  print("Done.")
```

## <a name="next-steps"></a>Következő lépések
> [!div class="nextstepaction"]
> [Adatbázis migrálása exportálással és importálással](./concepts-migrate-import-export.md)
