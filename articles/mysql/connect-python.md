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
# <a name="azure-database-for-mysql-use-python-tooconnect-and-query-data"></a><span data-ttu-id="99d45-103">MySQL az Azure-adatbázishoz: használata Python tooconnect és lekérdezési adatok</span><span class="sxs-lookup"><span data-stu-id="99d45-103">Azure Database for MySQL: Use Python tooconnect and query data</span></span>
<span data-ttu-id="99d45-104">A gyors üzembe helyezés bemutatja, hogyan toouse [Python](https://python.org) tooconnect tooan MySQL az Azure-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="99d45-104">This quickstart demonstrates how toouse [Python](https://python.org) tooconnect tooan Azure Database for MySQL.</span></span> <span data-ttu-id="99d45-105">A Mac OS, az Ubuntu Linux és a Windows platformokon hello adatbázisban SQL utasítás tooquery, insert, update és adatok használ.</span><span class="sxs-lookup"><span data-stu-id="99d45-105">It uses SQL statements tooquery, insert, update, and delete data in hello database from Mac OS, Ubuntu Linux, and Windows platforms.</span></span> <span data-ttu-id="99d45-106">Ez a cikk hello lépések azt feltételezik, ismeri a Python használatával történő fejlesztéséhez, és a rendszer új tooworking MySQL az Azure-adatbázissal.</span><span class="sxs-lookup"><span data-stu-id="99d45-106">hello steps in this article assume that you are familiar with developing using Python and are new tooworking with Azure Database for MySQL.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="99d45-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="99d45-107">Prerequisites</span></span>
<span data-ttu-id="99d45-108">A gyors üzembe helyezés kiindulási pontként ezek az útmutatók valamelyikével létrehozott hello erőforrást használ:</span><span class="sxs-lookup"><span data-stu-id="99d45-108">This quickstart uses hello resources created in either of these guides as a starting point:</span></span>
- [<span data-ttu-id="99d45-109">Azure-adatbázis létrehozása MySQL-kiszolgálóhoz az Azure Portal használatával</span><span class="sxs-lookup"><span data-stu-id="99d45-109">Create an Azure Database for MySQL server using Azure portal</span></span>](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [<span data-ttu-id="99d45-110">Azure-adatbázis létrehozása MySQL-kiszolgálóhoz az Azure CLI használatával</span><span class="sxs-lookup"><span data-stu-id="99d45-110">Create an Azure Database for MySQL server using Azure CLI</span></span>](./quickstart-create-mysql-server-database-using-azure-cli.md)

## <a name="install-python-and-hello-mysql-connector"></a><span data-ttu-id="99d45-111">Telepítse a Python és hello MySQL-összekötő</span><span class="sxs-lookup"><span data-stu-id="99d45-111">Install Python and hello MySQL connector</span></span>
<span data-ttu-id="99d45-112">Telepítés [Python](https://www.python.org/downloads/) és hello [Python MySQL-összekötő](https://dev.mysql.com/downloads/connector/python/) a saját számítógépén.</span><span class="sxs-lookup"><span data-stu-id="99d45-112">Install [Python](https://www.python.org/downloads/) and hello [MySQL connector for Python](https://dev.mysql.com/downloads/connector/python/) on your own machine.</span></span> <span data-ttu-id="99d45-113">Attól függően, hogy a platform a hello lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="99d45-113">Depending on your platform, follow hello steps:</span></span>

### <a name="windows"></a><span data-ttu-id="99d45-114">Windows</span><span class="sxs-lookup"><span data-stu-id="99d45-114">Windows</span></span>
1. <span data-ttu-id="99d45-115">Töltse le és telepítse a Python 2.7-es verziót a [python.org](https://www.python.org/downloads/windows/) webhelyről.</span><span class="sxs-lookup"><span data-stu-id="99d45-115">Download and Install Python 2.7 from [python.org](https://www.python.org/downloads/windows/).</span></span> 
2. <span data-ttu-id="99d45-116">Ellenőrizze a hello Python telepítését hello parancssorból indítsa el.</span><span class="sxs-lookup"><span data-stu-id="99d45-116">Check hello Python installation by launching hello command prompt.</span></span> <span data-ttu-id="99d45-117">Hello paranccsal `C:\python27\python.exe -V` hello nagybetűs V kapcsoló toosee hello verziószám használatával.</span><span class="sxs-lookup"><span data-stu-id="99d45-117">Run hello command `C:\python27\python.exe -V` using hello uppercase V switch toosee hello version number.</span></span>
3. <span data-ttu-id="99d45-118">A MySQL hello Python-összekötő telepítése [mysql.com](https://dev.mysql.com/downloads/connector/python/) Python megfelelő tooyour verziója.</span><span class="sxs-lookup"><span data-stu-id="99d45-118">Install hello Python connector for MySQL from [mysql.com](https://dev.mysql.com/downloads/connector/python/) corresponding tooyour version of Python.</span></span>

### <a name="linux-ubuntu"></a><span data-ttu-id="99d45-119">Linux (Ubuntu)</span><span class="sxs-lookup"><span data-stu-id="99d45-119">Linux (Ubuntu)</span></span>
1. <span data-ttu-id="99d45-120">Linux (Ubuntu), a Python általában hello alapértelmezett telepítés részeként telepíthető.</span><span class="sxs-lookup"><span data-stu-id="99d45-120">In Linux (Ubuntu), Python is typically installed as part of hello default installation.</span></span>
2. <span data-ttu-id="99d45-121">Ellenőrizze a hello Python telepítését hello rendszerhéjakba indításával.</span><span class="sxs-lookup"><span data-stu-id="99d45-121">Check hello Python installation by launching hello bash shell.</span></span> <span data-ttu-id="99d45-122">Hello paranccsal `python -V` hello nagybetűs V kapcsoló toosee hello verziószám használatával.</span><span class="sxs-lookup"><span data-stu-id="99d45-122">Run hello command `python -V` using hello uppercase V switch toosee hello version number.</span></span>
3. <span data-ttu-id="99d45-123">Ellenőrizze a hello PIP telepítési hello futtatásával `pip show pip -V` toosee hello verziószám parancsot.</span><span class="sxs-lookup"><span data-stu-id="99d45-123">Check hello PIP installation by running hello `pip show pip -V` command toosee hello version number.</span></span> 
4. <span data-ttu-id="99d45-124">A PIP szerepelhet a Python egyes verzióiban.</span><span class="sxs-lookup"><span data-stu-id="99d45-124">PIP may be included in some versions of Python.</span></span> <span data-ttu-id="99d45-125">Ha a PIP nincs telepítve, előfordulhat, hogy telepítése hello [PIP] (https://pip.pypa.io/en/stable/installing/) csomag, a parancs futtatásával `sudo apt-get install python-pip`.</span><span class="sxs-lookup"><span data-stu-id="99d45-125">If PIP is not installed, you may install hello [PIP] (https://pip.pypa.io/en/stable/installing/) package, by running command `sudo apt-get install python-pip`.</span></span>
5. <span data-ttu-id="99d45-126">Frissítés a PIP toohello legújabb verzió hello futtatásával `pip install -U pip` parancsot.</span><span class="sxs-lookup"><span data-stu-id="99d45-126">Update PIP toohello latest version, by running hello `pip install -U pip` command.</span></span>
6. <span data-ttu-id="99d45-127">Hello MySQL-összekötő telepítése a Python és annak függőségeit hello PIP parancs használatával:</span><span class="sxs-lookup"><span data-stu-id="99d45-127">Install hello MySQL connector for Python, and its dependencies by using hello PIP command:</span></span>

   ```bash
   sudo pip install mysql-connector-python-rf
   ```
 
### <a name="macos"></a><span data-ttu-id="99d45-128">MacOS</span><span class="sxs-lookup"><span data-stu-id="99d45-128">MacOS</span></span>
1. <span data-ttu-id="99d45-129">A Mac OS Python általában telepítve hello alapértelmezett operációs rendszer telepítésének részeként.</span><span class="sxs-lookup"><span data-stu-id="99d45-129">In Mac OS, Python is typically installed as part of hello default OS installation.</span></span>
2. <span data-ttu-id="99d45-130">Ellenőrizze a hello Python telepítését hello rendszerhéjakba indításával.</span><span class="sxs-lookup"><span data-stu-id="99d45-130">Check hello Python installation by launching hello bash shell.</span></span> <span data-ttu-id="99d45-131">Hello paranccsal `python -V` hello nagybetűs V kapcsoló toosee hello verziószám használatával.</span><span class="sxs-lookup"><span data-stu-id="99d45-131">Run hello command `python -V` using hello uppercase V switch toosee hello version number.</span></span>
3. <span data-ttu-id="99d45-132">Ellenőrizze a hello PIP telepítési hello futtatásával `pip show pip -V` toosee hello verziószám parancsot.</span><span class="sxs-lookup"><span data-stu-id="99d45-132">Check hello PIP installation by running hello `pip show pip -V` command toosee hello version number.</span></span>
4. <span data-ttu-id="99d45-133">A PIP szerepelhet a Python egyes verzióiban.</span><span class="sxs-lookup"><span data-stu-id="99d45-133">PIP may be included in some versions of Python.</span></span> <span data-ttu-id="99d45-134">Ha nincs telepítve a PIP, előfordulhat, hogy telepítése hello [PIP](https://pip.pypa.io/en/stable/installing/) csomag.</span><span class="sxs-lookup"><span data-stu-id="99d45-134">If PIP is not installed, you may install hello [PIP](https://pip.pypa.io/en/stable/installing/) package.</span></span>
5. <span data-ttu-id="99d45-135">Frissítés a PIP toohello legújabb verzió hello futtatásával `pip install -U pip` parancsot.</span><span class="sxs-lookup"><span data-stu-id="99d45-135">Update PIP toohello latest version, by running hello `pip install -U pip` command.</span></span>
6. <span data-ttu-id="99d45-136">Hello MySQL-összekötő telepítése a Python és annak függőségeit hello PIP parancs használatával:</span><span class="sxs-lookup"><span data-stu-id="99d45-136">Install hello MySQL connector for Python, and its dependencies by using hello PIP command:</span></span>

   ```bash
   pip install mysql-connector-python-rf
   ```

## <a name="get-connection-information"></a><span data-ttu-id="99d45-137">Kapcsolatadatok lekérése</span><span class="sxs-lookup"><span data-stu-id="99d45-137">Get connection information</span></span>
<span data-ttu-id="99d45-138">MySQL hello kapcsolat szükséges információkat tooconnect toohello Azure adatbázis beolvasása.</span><span class="sxs-lookup"><span data-stu-id="99d45-138">Get hello connection information needed tooconnect toohello Azure Database for MySQL.</span></span> <span data-ttu-id="99d45-139">Teljesen minősített kiszolgáló nevét és a bejelentkezési hitelesítő adatokat hello van szüksége.</span><span class="sxs-lookup"><span data-stu-id="99d45-139">You need hello fully qualified server name and login credentials.</span></span>

1. <span data-ttu-id="99d45-140">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="99d45-140">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="99d45-141">A hello Azure-portálon a bal oldali menüből, kattintson az **összes erőforrás** , és keresse meg rendelkezik gyűrött, például a hello kiszolgáló **myserver4demo**.</span><span class="sxs-lookup"><span data-stu-id="99d45-141">From hello left-hand menu in Azure portal, click **All resources** and search for hello server you have creased, such as **myserver4demo**.</span></span>
3. <span data-ttu-id="99d45-142">Hello kiszolgáló nevére kattint **myserver4demo**.</span><span class="sxs-lookup"><span data-stu-id="99d45-142">Click hello server name **myserver4demo**.</span></span>
4. <span data-ttu-id="99d45-143">Jelölje be hello server **tulajdonságok** lap.</span><span class="sxs-lookup"><span data-stu-id="99d45-143">Select hello server's **Properties** page.</span></span> <span data-ttu-id="99d45-144">Jegyezze fel a hello **kiszolgálónév** és **kiszolgálói rendszergazda bejelentkezési név**.</span><span class="sxs-lookup"><span data-stu-id="99d45-144">Make a note of hello **Server name** and **Server admin login name**.</span></span>
 <span data-ttu-id="99d45-145">![MySQL-hez készült Azure-adatbázis – Kiszolgáló-rendszergazdai bejelentkezés](./media/connect-python/1_server-properties-name-login.png)</span><span class="sxs-lookup"><span data-stu-id="99d45-145">![Azure Database for MySQL - Server Admin Login](./media/connect-python/1_server-properties-name-login.png)</span></span>
5. <span data-ttu-id="99d45-146">Ha elfelejti a kiszolgálói bejelentkezési adatok, keresse meg a toohello **áttekintése** tooview hello kiszolgálói rendszergazda bejelentkezési név lapon, és ha szükséges, állítsa vissza a hello jelszót.</span><span class="sxs-lookup"><span data-stu-id="99d45-146">If you forget your server login information, navigate toohello **Overview** page tooview hello Server admin login name and, if necessary, reset hello password.</span></span>
   

## <a name="run-python-code"></a><span data-ttu-id="99d45-147">A Python-kód futtatása</span><span class="sxs-lookup"><span data-stu-id="99d45-147">Run Python Code</span></span>
- <span data-ttu-id="99d45-148">Hello kód beillesztése egy szövegfájlt, és mentse a hello fájlt a fájl kiterjesztése .py, például C:\pythonmysql\createtable.py vagy /home/username/pythonmysql/createtable.py projekt mappába</span><span class="sxs-lookup"><span data-stu-id="99d45-148">Paste hello code into a text file, and save hello file into a project folder with file extension .py, such as C:\pythonmysql\createtable.py or /home/username/pythonmysql/createtable.py</span></span>
- <span data-ttu-id="99d45-149">toorun hello kódot, indítsa el a hello parancssor, vagy a bash rendszerhéjat.</span><span class="sxs-lookup"><span data-stu-id="99d45-149">toorun hello code, launch hello command prompt or bash shell.</span></span> <span data-ttu-id="99d45-150">Módosítsa a könyvtárat a projektmappájára: `cd pythonmysql`.</span><span class="sxs-lookup"><span data-stu-id="99d45-150">Change directory into your project folder `cd pythonmysql`.</span></span> <span data-ttu-id="99d45-151">Írja be hello python parancsot hello fájl neve követ `python createtable.py` toorun hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="99d45-151">Then type hello python command followed by hello file name `python createtable.py` toorun hello application.</span></span> <span data-ttu-id="99d45-152">A Windows operációs rendszer hello, python.exe nem található, ha, előfordulhat, hogy adjon meg hello teljes elérési útja toohello végrehajtható, vagy hello Python elérési út hozzáadása a hello path környezeti változóba.</span><span class="sxs-lookup"><span data-stu-id="99d45-152">On hello Windows OS, if python.exe is not found, you may provide hello full path toohello executable, or add hello Python path into hello path environment variable.</span></span> `C:\python27\python.exe createtable.py`

## <a name="connect-create-table-and-insert-data"></a><span data-ttu-id="99d45-153">Csatlakozás, táblák létrehozása és adatok beszúrása</span><span class="sxs-lookup"><span data-stu-id="99d45-153">Connect, create table, and insert data</span></span>
<span data-ttu-id="99d45-154">Használja hello alábbi tooconnect toohello server code, hozzon létre egy táblát, és hello használatával végzett betölteni egy **BESZÚRÁSA** SQL-utasításban.</span><span class="sxs-lookup"><span data-stu-id="99d45-154">Use hello following code tooconnect toohello server, create a table, and load hello data using an **INSERT** SQL statement.</span></span> 

<span data-ttu-id="99d45-155">Hello kódban hello mysql.connector könyvtár importálva van.</span><span class="sxs-lookup"><span data-stu-id="99d45-155">In hello code, hello mysql.connector library is imported.</span></span> <span data-ttu-id="99d45-156">Hello [csatlakozás](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysql-connector-connect.html) függvény használt tooconnect tooAzure hello segítségével MySQL adatbázis [kapcsolat argumentumok](https://dev.mysql.com/doc/connector-python/en/connector-python-connectargs.html) hello config gyűjteményben.</span><span class="sxs-lookup"><span data-stu-id="99d45-156">hello [connect()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysql-connector-connect.html) function is used tooconnect tooAzure Database for MySQL using hello [connection arguments](https://dev.mysql.com/doc/connector-python/en/connector-python-connectargs.html) in hello config collection.</span></span> <span data-ttu-id="99d45-157">hello használ a kurzor hello kapcsolaton, és [cursor.execute()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-execute.html) metódus elindít hello MySQL-adatbázis SQL-lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="99d45-157">hello code uses a cursor on hello connection, and [cursor.execute()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-execute.html) method executes hello SQL query against MySQL database.</span></span> 

<span data-ttu-id="99d45-158">Cserélje le a hello `host`, `user`, `password`, és `database` paraméterek hello kiszolgáló és az adatbázis létrehozásakor adott hello értékekkel.</span><span class="sxs-lookup"><span data-stu-id="99d45-158">Replace hello `host`, `user`, `password`, and `database` parameters with hello values that you specified when you created hello server and database.</span></span>

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

## <a name="read-data"></a><span data-ttu-id="99d45-159">Adatok olvasása</span><span class="sxs-lookup"><span data-stu-id="99d45-159">Read data</span></span>
<span data-ttu-id="99d45-160">Használjon hello alábbi code tooconnect, és hello adatok segítségével olvassa a **kiválasztása** SQL-utasításban.</span><span class="sxs-lookup"><span data-stu-id="99d45-160">Use hello following code tooconnect and read hello data using a **SELECT** SQL statement.</span></span> 

<span data-ttu-id="99d45-161">Hello kódban hello mysql.connector könyvtár importálva van.</span><span class="sxs-lookup"><span data-stu-id="99d45-161">In hello code, hello mysql.connector library is imported.</span></span> <span data-ttu-id="99d45-162">Hello [csatlakozás](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysql-connector-connect.html) függvény használt tooconnect tooAzure hello segítségével MySQL adatbázis [kapcsolat argumentumok](https://dev.mysql.com/doc/connector-python/en/connector-python-connectargs.html) hello config gyűjteményben.</span><span class="sxs-lookup"><span data-stu-id="99d45-162">hello [connect()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysql-connector-connect.html) function is used tooconnect tooAzure Database for MySQL using hello [connection arguments](https://dev.mysql.com/doc/connector-python/en/connector-python-connectargs.html) in hello config collection.</span></span> <span data-ttu-id="99d45-163">hello használ a kurzor hello kapcsolaton, és [cursor.execute()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-execute.html) metódus elindít hello MySQL-adatbázis SQL-utasításban.</span><span class="sxs-lookup"><span data-stu-id="99d45-163">hello code uses a cursor on hello connection, and [cursor.execute()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-execute.html) method executes hello SQL statement against MySQL database.</span></span> <span data-ttu-id="99d45-164">hello adatsorok hello olvassa el [fetchall()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-fetchall.html) metódust.</span><span class="sxs-lookup"><span data-stu-id="99d45-164">hello data rows are read using hello [fetchall()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-fetchall.html) method.</span></span> <span data-ttu-id="99d45-165">hello eredménykészlet tartják gyűjtemény egymás és egy iterátor használt tooloop: hello sorok fölé.</span><span class="sxs-lookup"><span data-stu-id="99d45-165">hello result set is kept in a collection row and a for iterator is used tooloop over hello rows.</span></span>

<span data-ttu-id="99d45-166">Cserélje le a hello `host`, `user`, `password`, és `database` paraméterek hello kiszolgáló és az adatbázis létrehozásakor adott hello értékekkel.</span><span class="sxs-lookup"><span data-stu-id="99d45-166">Replace hello `host`, `user`, `password`, and `database` parameters with hello values that you specified when you created hello server and database.</span></span>

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

## <a name="update-data"></a><span data-ttu-id="99d45-167">Adatok frissítése</span><span class="sxs-lookup"><span data-stu-id="99d45-167">Update data</span></span>
<span data-ttu-id="99d45-168">Használjon hello következő code tooconnect, és frissítse a hello adatok segítségével egy **frissítése** SQL-utasításban.</span><span class="sxs-lookup"><span data-stu-id="99d45-168">Use hello following code tooconnect and update hello data using a **UPDATE** SQL statement.</span></span> 

<span data-ttu-id="99d45-169">Hello kódban hello mysql.connector könyvtár importálva van.</span><span class="sxs-lookup"><span data-stu-id="99d45-169">In hello code, hello mysql.connector library is imported.</span></span>  <span data-ttu-id="99d45-170">Hello [csatlakozás](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysql-connector-connect.html) függvény használt tooconnect tooAzure hello segítségével MySQL adatbázis [kapcsolat argumentumok](https://dev.mysql.com/doc/connector-python/en/connector-python-connectargs.html) hello config gyűjteményben.</span><span class="sxs-lookup"><span data-stu-id="99d45-170">hello [connect()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysql-connector-connect.html) function is used tooconnect tooAzure Database for MySQL using hello [connection arguments](https://dev.mysql.com/doc/connector-python/en/connector-python-connectargs.html) in hello config collection.</span></span> <span data-ttu-id="99d45-171">hello használ a kurzor hello kapcsolaton, és [cursor.execute()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-execute.html) metódus elindít hello MySQL-adatbázis SQL-utasításban.</span><span class="sxs-lookup"><span data-stu-id="99d45-171">hello code uses a cursor on hello connection, and [cursor.execute()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-execute.html) method executes hello SQL statement against MySQL database.</span></span> 

<span data-ttu-id="99d45-172">Cserélje le a hello `host`, `user`, `password`, és `database` paraméterek hello kiszolgáló és az adatbázis létrehozásakor adott hello értékekkel.</span><span class="sxs-lookup"><span data-stu-id="99d45-172">Replace hello `host`, `user`, `password`, and `database` parameters with hello values that you specified when you created hello server and database.</span></span>

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

## <a name="delete-data"></a><span data-ttu-id="99d45-173">Adat törlése</span><span class="sxs-lookup"><span data-stu-id="99d45-173">Delete data</span></span>
<span data-ttu-id="99d45-174">Használjon hello alábbi code tooconnect, és távolítsa el az adatokat egy **törlése** SQL-utasításban.</span><span class="sxs-lookup"><span data-stu-id="99d45-174">Use hello following code tooconnect and remove data using a **DELETE** SQL statement.</span></span> 

<span data-ttu-id="99d45-175">Hello kódban hello mysql.connector könyvtár importálva van.</span><span class="sxs-lookup"><span data-stu-id="99d45-175">In hello code, hello mysql.connector library is imported.</span></span>  <span data-ttu-id="99d45-176">Hello [csatlakozás](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysql-connector-connect.html) függvény használt tooconnect tooAzure hello segítségével MySQL adatbázis [kapcsolat argumentumok](https://dev.mysql.com/doc/connector-python/en/connector-python-connectargs.html) hello config gyűjteményben.</span><span class="sxs-lookup"><span data-stu-id="99d45-176">hello [connect()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysql-connector-connect.html) function is used tooconnect tooAzure Database for MySQL using hello [connection arguments](https://dev.mysql.com/doc/connector-python/en/connector-python-connectargs.html) in hello config collection.</span></span> <span data-ttu-id="99d45-177">hello használ a kurzor hello kapcsolaton, és [cursor.execute()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-execute.html) metódus elindít hello MySQL-adatbázis SQL-lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="99d45-177">hello code uses a cursor on hello connection, and [cursor.execute()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-execute.html) method executes hello SQL query against MySQL database.</span></span> 

<span data-ttu-id="99d45-178">Cserélje le a hello `host`, `user`, `password`, és `database` paraméterek hello kiszolgáló és az adatbázis létrehozásakor adott hello értékekkel.</span><span class="sxs-lookup"><span data-stu-id="99d45-178">Replace hello `host`, `user`, `password`, and `database` parameters with hello values that you specified when you created hello server and database.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="99d45-179">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="99d45-179">Next steps</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="99d45-180">Adatbázis migrálása exportálással és importálással</span><span class="sxs-lookup"><span data-stu-id="99d45-180">Migrate your database using Export and Import</span></span>](./concepts-migrate-import-export.md)
