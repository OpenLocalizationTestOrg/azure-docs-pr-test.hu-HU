---
title: "Csatlakozás a MySQL-hez készült Azure-adatbázishoz a Pythonnal | Microsoft Docs"
description: "Ez a rövid útmutató számos Python-mintakódot biztosít, amelyekkel csatlakozhat a MySQL-hez készült Azure-adatbázishoz, illetve adatokat kérdezhet le róla."
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
ms.openlocfilehash: 4c3a2e65b83fab6fe5b8b7778782a747bb5e9cf9
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/29/2017
---
# <a name="azure-database-for-mysql-use-python-to-connect-and-query-data"></a><span data-ttu-id="de912-103">A MySQL-hez készült Azure-adatbázis: Csatlakozás és adatlekérdezés a Python használatával</span><span class="sxs-lookup"><span data-stu-id="de912-103">Azure Database for MySQL: Use Python to connect and query data</span></span>
<span data-ttu-id="de912-104">Ez a rövid útmutató ismerteti, hogyan használható a [Python](https://python.org) a MySQL-hez készült Azure-adatbázishoz való csatlakozáshoz.</span><span class="sxs-lookup"><span data-stu-id="de912-104">This quickstart demonstrates how to use [Python](https://python.org) to connect to an Azure Database for MySQL.</span></span> <span data-ttu-id="de912-105">Az SQL-utasítások használatával kérdez le, szúr be, frissít és töröl adatokat az adatbázisban a Mac OS, Ubuntu Linux és a Windows platformról.</span><span class="sxs-lookup"><span data-stu-id="de912-105">It uses SQL statements to query, insert, update, and delete data in the database from Mac OS, Ubuntu Linux, and Windows platforms.</span></span> <span data-ttu-id="de912-106">A jelen cikkben ismertetett lépések feltételezik, hogy Ön rendelkezik fejlesztési tapasztalatokkal a Python használatával kapcsolatosan, a MySQL-hez készült Azure-adatbázis használatában pedig még járatlan.</span><span class="sxs-lookup"><span data-stu-id="de912-106">The steps in this article assume that you are familiar with developing using Python and are new to working with Azure Database for MySQL.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="de912-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="de912-107">Prerequisites</span></span>
<span data-ttu-id="de912-108">Ebben a rövid útmutatóban a következő útmutatók valamelyikében létrehozott erőforrásokat használunk kiindulási pontként:</span><span class="sxs-lookup"><span data-stu-id="de912-108">This quickstart uses the resources created in either of these guides as a starting point:</span></span>
- [<span data-ttu-id="de912-109">Azure-adatbázis létrehozása MySQL-kiszolgálóhoz az Azure Portal használatával</span><span class="sxs-lookup"><span data-stu-id="de912-109">Create an Azure Database for MySQL server using Azure portal</span></span>](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [<span data-ttu-id="de912-110">Azure-adatbázis létrehozása MySQL-kiszolgálóhoz az Azure CLI használatával</span><span class="sxs-lookup"><span data-stu-id="de912-110">Create an Azure Database for MySQL server using Azure CLI</span></span>](./quickstart-create-mysql-server-database-using-azure-cli.md)

## <a name="install-python-and-the-mysql-connector"></a><span data-ttu-id="de912-111">A Python és a MySQL-összekötő telepítése</span><span class="sxs-lookup"><span data-stu-id="de912-111">Install Python and the MySQL connector</span></span>
<span data-ttu-id="de912-112">Telepítse a [Pythont](https://www.python.org/downloads/) és a [Python MySQL-összekötőjét](https://dev.mysql.com/downloads/connector/python/) a saját gépére.</span><span class="sxs-lookup"><span data-stu-id="de912-112">Install [Python](https://www.python.org/downloads/) and the [MySQL connector for Python](https://dev.mysql.com/downloads/connector/python/) on your own machine.</span></span> <span data-ttu-id="de912-113">Kövesse az egyes platformoknak megfelelő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="de912-113">Depending on your platform, follow the steps:</span></span>

### <a name="windows"></a><span data-ttu-id="de912-114">Windows</span><span class="sxs-lookup"><span data-stu-id="de912-114">Windows</span></span>
1. <span data-ttu-id="de912-115">Töltse le és telepítse a Python 2.7-es verziót a [python.org](https://www.python.org/downloads/windows/) webhelyről.</span><span class="sxs-lookup"><span data-stu-id="de912-115">Download and Install Python 2.7 from [python.org](https://www.python.org/downloads/windows/).</span></span> 
2. <span data-ttu-id="de912-116">A parancssor elindításával ellenőrizze a Python telepítését.</span><span class="sxs-lookup"><span data-stu-id="de912-116">Check the Python installation by launching the command prompt.</span></span> <span data-ttu-id="de912-117">Futtassa a `C:\python27\python.exe -V` parancsot a nagybetűs V kapcsolóval a verziószám megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="de912-117">Run the command `C:\python27\python.exe -V` using the uppercase V switch to see the version number.</span></span>
3. <span data-ttu-id="de912-118">Telepítse a MySQL a Python verziójának megfelelő Python-összekötőjét a [mysql.com](https://dev.mysql.com/downloads/connector/python/) webhelyről.</span><span class="sxs-lookup"><span data-stu-id="de912-118">Install the Python connector for MySQL from [mysql.com](https://dev.mysql.com/downloads/connector/python/) corresponding to your version of Python.</span></span>

### <a name="linux-ubuntu"></a><span data-ttu-id="de912-119">Linux (Ubuntu)</span><span class="sxs-lookup"><span data-stu-id="de912-119">Linux (Ubuntu)</span></span>
1. <span data-ttu-id="de912-120">A Linux (Ubuntu) rendszeren a Python általában az alapértelmezett telepítés részeként van telepítve.</span><span class="sxs-lookup"><span data-stu-id="de912-120">In Linux (Ubuntu), Python is typically installed as part of the default installation.</span></span>
2. <span data-ttu-id="de912-121">A bash rendszerhéj elindításával ellenőrizze a Python telepítését.</span><span class="sxs-lookup"><span data-stu-id="de912-121">Check the Python installation by launching the bash shell.</span></span> <span data-ttu-id="de912-122">Futtassa a `python -V` parancsot a nagybetűs V kapcsolóval a verziószám megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="de912-122">Run the command `python -V` using the uppercase V switch to see the version number.</span></span>
3. <span data-ttu-id="de912-123">A PIP telepítésének ellenőrzéséhez futtassa a `pip show pip -V` parancsot a verziószám megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="de912-123">Check the PIP installation by running the `pip show pip -V` command to see the version number.</span></span> 
4. <span data-ttu-id="de912-124">A PIP szerepelhet a Python egyes verzióiban.</span><span class="sxs-lookup"><span data-stu-id="de912-124">PIP may be included in some versions of Python.</span></span> <span data-ttu-id="de912-125">Ha a PIP nincs telepítve, a [PIP] (https://pip.pypa.io/en/stable/installing/) csomagot a `sudo apt-get install python-pip` parancs futtatásával telepítheti.</span><span class="sxs-lookup"><span data-stu-id="de912-125">If PIP is not installed, you may install the [PIP] (https://pip.pypa.io/en/stable/installing/) package, by running command `sudo apt-get install python-pip`.</span></span>
5. <span data-ttu-id="de912-126">A PIP legújabb verziójára való frissítéshez futtassa a `pip install -U pip` parancsot.</span><span class="sxs-lookup"><span data-stu-id="de912-126">Update PIP to the latest version, by running the `pip install -U pip` command.</span></span>
6. <span data-ttu-id="de912-127">A PIP paranccsal telepítse a Python MySQL-összekötőjét és annak függőségeit:</span><span class="sxs-lookup"><span data-stu-id="de912-127">Install the MySQL connector for Python, and its dependencies by using the PIP command:</span></span>

   ```bash
   sudo pip install mysql-connector-python-rf
   ```
 
### <a name="macos"></a><span data-ttu-id="de912-128">MacOS</span><span class="sxs-lookup"><span data-stu-id="de912-128">MacOS</span></span>
1. <span data-ttu-id="de912-129">A Mac OS rendszeren a Python általában az alapértelmezett telepítés részeként van telepítve.</span><span class="sxs-lookup"><span data-stu-id="de912-129">In Mac OS, Python is typically installed as part of the default OS installation.</span></span>
2. <span data-ttu-id="de912-130">A bash rendszerhéj elindításával ellenőrizze a Python telepítését.</span><span class="sxs-lookup"><span data-stu-id="de912-130">Check the Python installation by launching the bash shell.</span></span> <span data-ttu-id="de912-131">Futtassa a `python -V` parancsot a nagybetűs V kapcsolóval a verziószám megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="de912-131">Run the command `python -V` using the uppercase V switch to see the version number.</span></span>
3. <span data-ttu-id="de912-132">A PIP telepítésének ellenőrzéséhez futtassa a `pip show pip -V` parancsot a verziószám megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="de912-132">Check the PIP installation by running the `pip show pip -V` command to see the version number.</span></span>
4. <span data-ttu-id="de912-133">A PIP szerepelhet a Python egyes verzióiban.</span><span class="sxs-lookup"><span data-stu-id="de912-133">PIP may be included in some versions of Python.</span></span> <span data-ttu-id="de912-134">Ha a PIP nincs telepítve, telepítheti a [PIP](https://pip.pypa.io/en/stable/installing/) csomagot.</span><span class="sxs-lookup"><span data-stu-id="de912-134">If PIP is not installed, you may install the [PIP](https://pip.pypa.io/en/stable/installing/) package.</span></span>
5. <span data-ttu-id="de912-135">A PIP legújabb verziójára való frissítéshez futtassa a `pip install -U pip` parancsot.</span><span class="sxs-lookup"><span data-stu-id="de912-135">Update PIP to the latest version, by running the `pip install -U pip` command.</span></span>
6. <span data-ttu-id="de912-136">A PIP paranccsal telepítse a Python MySQL-összekötőjét és annak függőségeit:</span><span class="sxs-lookup"><span data-stu-id="de912-136">Install the MySQL connector for Python, and its dependencies by using the PIP command:</span></span>

   ```bash
   pip install mysql-connector-python-rf
   ```

## <a name="get-connection-information"></a><span data-ttu-id="de912-137">Kapcsolatadatok lekérése</span><span class="sxs-lookup"><span data-stu-id="de912-137">Get connection information</span></span>
<span data-ttu-id="de912-138">Kérje le a MySQL-hez készült Azure Database-hez való csatlakozáshoz szükséges kapcsolatadatokat.</span><span class="sxs-lookup"><span data-stu-id="de912-138">Get the connection information needed to connect to the Azure Database for MySQL.</span></span> <span data-ttu-id="de912-139">Ehhez szükség lesz a teljes kiszolgálónévre és bejelentkezési hitelesítő adatokra.</span><span class="sxs-lookup"><span data-stu-id="de912-139">You need the fully qualified server name and login credentials.</span></span>

1. <span data-ttu-id="de912-140">Jelentkezzen be az [Azure portálra](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="de912-140">Log in to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="de912-141">Az Azure Portal bal oldali menüjében kattintson az **Összes erőforrás** lehetőségre, és keressen rá a létrehozott kiszolgálóra (például: **myserver4demo**).</span><span class="sxs-lookup"><span data-stu-id="de912-141">From the left-hand menu in Azure portal, click **All resources** and search for the server you have creased, such as **myserver4demo**.</span></span>
3. <span data-ttu-id="de912-142">Kattintson a **myserver4demo** kiszolgálónévre.</span><span class="sxs-lookup"><span data-stu-id="de912-142">Click the server name **myserver4demo**.</span></span>
4. <span data-ttu-id="de912-143">Válassza a kiszolgáló **tulajdonságlapját**.</span><span class="sxs-lookup"><span data-stu-id="de912-143">Select the server's **Properties** page.</span></span> <span data-ttu-id="de912-144">Jegyezze fel a **Kiszolgálónevet** és a **Kiszolgáló-rendszergazdai bejelentkezési nevet**.</span><span class="sxs-lookup"><span data-stu-id="de912-144">Make a note of the **Server name** and **Server admin login name**.</span></span>
 <span data-ttu-id="de912-145">![MySQL-hez készült Azure-adatbázis – Kiszolgáló-rendszergazdai bejelentkezés](./media/connect-python/1_server-properties-name-login.png)</span><span class="sxs-lookup"><span data-stu-id="de912-145">![Azure Database for MySQL - Server Admin Login](./media/connect-python/1_server-properties-name-login.png)</span></span>
5. <span data-ttu-id="de912-146">Amennyiben elfelejtette a kiszolgálója bejelentkezési adatait, lépjen az **Áttekintés** oldalra, ahol kikeresheti a kiszolgáló-rendszergazda bejelentkezési nevét, valamint szükség esetén új jelszót kérhet.</span><span class="sxs-lookup"><span data-stu-id="de912-146">If you forget your server login information, navigate to the **Overview** page to view the Server admin login name and, if necessary, reset the password.</span></span>
   

## <a name="run-python-code"></a><span data-ttu-id="de912-147">A Python-kód futtatása</span><span class="sxs-lookup"><span data-stu-id="de912-147">Run Python Code</span></span>
- <span data-ttu-id="de912-148">Mentse a kódot egy szövegfájlba, és mentse a fájlt egy projektmappába .py kiterjesztéssel, például a C:\pythonmysql\createtable.py vagy /home/username/pythonmysql/createtable.py elérési úton.</span><span class="sxs-lookup"><span data-stu-id="de912-148">Paste the code into a text file, and save the file into a project folder with file extension .py, such as C:\pythonmysql\createtable.py or /home/username/pythonmysql/createtable.py</span></span>
- <span data-ttu-id="de912-149">A kód futtatásához nyissa meg a parancssort vagy a bash rendszerhéjat.</span><span class="sxs-lookup"><span data-stu-id="de912-149">To run the code, launch the command prompt or bash shell.</span></span> <span data-ttu-id="de912-150">Módosítsa a könyvtárat a projektmappájára: `cd pythonmysql`.</span><span class="sxs-lookup"><span data-stu-id="de912-150">Change directory into your project folder `cd pythonmysql`.</span></span> <span data-ttu-id="de912-151">Ezután írja be a python-parancsot, majd a fájlnevet (`python createtable.py`) az alkalmazás futtatásához.</span><span class="sxs-lookup"><span data-stu-id="de912-151">Then type the python command followed by the file name `python createtable.py` to run the application.</span></span> <span data-ttu-id="de912-152">Ha a Windows operációs rendszeren nem található a python.exe, megadhatja a futtatható fájl teljes elérési útját, vagy a Python-útvonalat a path környezeti változóhoz adhatja.</span><span class="sxs-lookup"><span data-stu-id="de912-152">On the Windows OS, if python.exe is not found, you may provide the full path to the executable, or add the Python path into the path environment variable.</span></span> `C:\python27\python.exe createtable.py`

## <a name="connect-create-table-and-insert-data"></a><span data-ttu-id="de912-153">Csatlakozás, táblák létrehozása és adatok beszúrása</span><span class="sxs-lookup"><span data-stu-id="de912-153">Connect, create table, and insert data</span></span>
<span data-ttu-id="de912-154">Az alábbi kód használatával csatlakozhat a kiszolgálóhoz, hozhat létre táblát, és töltheti be az adatokat egy **INSERT** SQL-utasítással.</span><span class="sxs-lookup"><span data-stu-id="de912-154">Use the following code to connect to the server, create a table, and load the data using an **INSERT** SQL statement.</span></span> 

<span data-ttu-id="de912-155">A kódban a mysql.connector-kódtár lesz importálva.</span><span class="sxs-lookup"><span data-stu-id="de912-155">In the code, the mysql.connector library is imported.</span></span> <span data-ttu-id="de912-156">A [connect()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysql-connector-connect.html) függvénnyel csatlakozhat a MySQL-hez készült Azure-adatbázishoz a config gyűjtemény [kapcsolati argumentumaival](https://dev.mysql.com/doc/connector-python/en/connector-python-connectargs.html).</span><span class="sxs-lookup"><span data-stu-id="de912-156">The [connect()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysql-connector-connect.html) function is used to connect to Azure Database for MySQL using the [connection arguments](https://dev.mysql.com/doc/connector-python/en/connector-python-connectargs.html) in the config collection.</span></span> <span data-ttu-id="de912-157">A kód egy kurzort használ a kapcsolaton, és a [cursor.execute()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-execute.html) metódus hajtja végre az SQL-lekérdezést a MySQL-adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="de912-157">The code uses a cursor on the connection, and [cursor.execute()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-execute.html) method executes the SQL query against MySQL database.</span></span> 

<span data-ttu-id="de912-158">Cserélje le a `host`, `user`, `password` és `database` paramétert azokkal az értékekkel, amelyeket a kiszolgáló és az adatbázis létrehozásakor adott meg.</span><span class="sxs-lookup"><span data-stu-id="de912-158">Replace the `host`, `user`, `password`, and `database` parameters with the values that you specified when you created the server and database.</span></span>

```Python
import mysql.connector
from mysql.connector import errorcode

# Obtain connection string information from the portal
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
    print("Something is wrong with the user name or password")
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

## <a name="read-data"></a><span data-ttu-id="de912-159">Adatok beolvasása</span><span class="sxs-lookup"><span data-stu-id="de912-159">Read data</span></span>
<span data-ttu-id="de912-160">A következő kóddal csatlakozhat, és beolvashatja az adatokat a **SELECT** SQL-utasítással.</span><span class="sxs-lookup"><span data-stu-id="de912-160">Use the following code to connect and read the data using a **SELECT** SQL statement.</span></span> 

<span data-ttu-id="de912-161">A kódban a mysql.connector-kódtár lesz importálva.</span><span class="sxs-lookup"><span data-stu-id="de912-161">In the code, the mysql.connector library is imported.</span></span> <span data-ttu-id="de912-162">A [connect()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysql-connector-connect.html) függvénnyel csatlakozhat a MySQL-hez készült Azure-adatbázishoz a config gyűjtemény [kapcsolati argumentumaival](https://dev.mysql.com/doc/connector-python/en/connector-python-connectargs.html).</span><span class="sxs-lookup"><span data-stu-id="de912-162">The [connect()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysql-connector-connect.html) function is used to connect to Azure Database for MySQL using the [connection arguments](https://dev.mysql.com/doc/connector-python/en/connector-python-connectargs.html) in the config collection.</span></span> <span data-ttu-id="de912-163">A kód egy kurzort használ a kapcsolaton, és a [cursor.execute()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-execute.html) metódus hajtja végre az SQL-utasítást a MySQL-adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="de912-163">The code uses a cursor on the connection, and [cursor.execute()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-execute.html) method executes the SQL statement against MySQL database.</span></span> <span data-ttu-id="de912-164">Az adatsorokat a rendszer a [fetchall()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-fetchall.html) metódussal olvassa be.</span><span class="sxs-lookup"><span data-stu-id="de912-164">The data rows are read using the [fetchall()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-fetchall.html) method.</span></span> <span data-ttu-id="de912-165">Az eredményhalmaz egy gyűjteménysorba kerül, és a sorokon történő végighaladásra egy „for” iterátor szolgál.</span><span class="sxs-lookup"><span data-stu-id="de912-165">The result set is kept in a collection row and a for iterator is used to loop over the rows.</span></span>

<span data-ttu-id="de912-166">Cserélje le a `host`, `user`, `password` és `database` paramétert azokkal az értékekkel, amelyeket a kiszolgáló és az adatbázis létrehozásakor adott meg.</span><span class="sxs-lookup"><span data-stu-id="de912-166">Replace the `host`, `user`, `password`, and `database` parameters with the values that you specified when you created the server and database.</span></span>

```Python
import mysql.connector
from mysql.connector import errorcode

# Obtain connection string information from the portal
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
    print("Something is wrong with the user name or password")
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

## <a name="update-data"></a><span data-ttu-id="de912-167">Adatok frissítése</span><span class="sxs-lookup"><span data-stu-id="de912-167">Update data</span></span>
<span data-ttu-id="de912-168">A következő kód használatával csatlakozhat, és frissítheti az adatokat az **UPDATE** SQL-utasítással.</span><span class="sxs-lookup"><span data-stu-id="de912-168">Use the following code to connect and update the data using a **UPDATE** SQL statement.</span></span> 

<span data-ttu-id="de912-169">A kódban a mysql.connector-kódtár lesz importálva.</span><span class="sxs-lookup"><span data-stu-id="de912-169">In the code, the mysql.connector library is imported.</span></span>  <span data-ttu-id="de912-170">A [connect()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysql-connector-connect.html) függvénnyel csatlakozhat a MySQL-hez készült Azure-adatbázishoz a config gyűjtemény [kapcsolati argumentumaival](https://dev.mysql.com/doc/connector-python/en/connector-python-connectargs.html).</span><span class="sxs-lookup"><span data-stu-id="de912-170">The [connect()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysql-connector-connect.html) function is used to connect to Azure Database for MySQL using the [connection arguments](https://dev.mysql.com/doc/connector-python/en/connector-python-connectargs.html) in the config collection.</span></span> <span data-ttu-id="de912-171">A kód egy kurzort használ a kapcsolaton, és a [cursor.execute()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-execute.html) metódus hajtja végre az SQL-utasítást a MySQL-adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="de912-171">The code uses a cursor on the connection, and [cursor.execute()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-execute.html) method executes the SQL statement against MySQL database.</span></span> 

<span data-ttu-id="de912-172">Cserélje le a `host`, `user`, `password` és `database` paramétert azokkal az értékekkel, amelyeket a kiszolgáló és az adatbázis létrehozásakor adott meg.</span><span class="sxs-lookup"><span data-stu-id="de912-172">Replace the `host`, `user`, `password`, and `database` parameters with the values that you specified when you created the server and database.</span></span>

```Python
import mysql.connector
from mysql.connector import errorcode

# Obtain connection string information from the portal
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
    print("Something is wrong with the user name or password")
  elif err.errno == errorcode.ER_BAD_DB_ERROR:
    print("Database does not exist")
  else:
    print(err)
else:
  cursor = conn.cursor()

  # Update a data row in the table
  cursor.execute("UPDATE inventory SET quantity = %s WHERE name = %s;", (200, "banana"))
  print("Updated",cursor.rowcount,"row(s) of data.")

  # Cleanup
  conn.commit()
  cursor.close()
  conn.close()
  print("Done.")
```

## <a name="delete-data"></a><span data-ttu-id="de912-173">Adat törlése</span><span class="sxs-lookup"><span data-stu-id="de912-173">Delete data</span></span>
<span data-ttu-id="de912-174">A következő kód használatával csatlakozhat, és eltávolíthatja az adatokat a **DELETE** SQL-utasítással.</span><span class="sxs-lookup"><span data-stu-id="de912-174">Use the following code to connect and remove data using a **DELETE** SQL statement.</span></span> 

<span data-ttu-id="de912-175">A kódban a mysql.connector-kódtár lesz importálva.</span><span class="sxs-lookup"><span data-stu-id="de912-175">In the code, the mysql.connector library is imported.</span></span>  <span data-ttu-id="de912-176">A [connect()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysql-connector-connect.html) függvénnyel csatlakozhat a MySQL-hez készült Azure-adatbázishoz a config gyűjtemény [kapcsolati argumentumaival](https://dev.mysql.com/doc/connector-python/en/connector-python-connectargs.html).</span><span class="sxs-lookup"><span data-stu-id="de912-176">The [connect()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysql-connector-connect.html) function is used to connect to Azure Database for MySQL using the [connection arguments](https://dev.mysql.com/doc/connector-python/en/connector-python-connectargs.html) in the config collection.</span></span> <span data-ttu-id="de912-177">A kód egy kurzort használ a kapcsolaton, és a [cursor.execute()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-execute.html) metódus hajtja végre az SQL-lekérdezést a MySQL-adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="de912-177">The code uses a cursor on the connection, and [cursor.execute()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-execute.html) method executes the SQL query against MySQL database.</span></span> 

<span data-ttu-id="de912-178">Cserélje le a `host`, `user`, `password` és `database` paramétert azokkal az értékekkel, amelyeket a kiszolgáló és az adatbázis létrehozásakor adott meg.</span><span class="sxs-lookup"><span data-stu-id="de912-178">Replace the `host`, `user`, `password`, and `database` parameters with the values that you specified when you created the server and database.</span></span>

```Python
import mysql.connector
from mysql.connector import errorcode

# Obtain connection string information from the portal
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
    print("Something is wrong with the user name or password.")
  elif err.errno == errorcode.ER_BAD_DB_ERROR:
    print("Database does not exist.")
  else:
    print(err)
else:
  cursor = conn.cursor()

  # Delete a data row in the table
  cursor.execute("DELETE FROM inventory WHERE name=%(param1)s;", {'param1':"orange"})
  print("Deleted",cursor.rowcount,"row(s) of data.")

  # Cleanup
  conn.commit()
  cursor.close()
  conn.close()
  print("Done.")
```

## <a name="next-steps"></a><span data-ttu-id="de912-179">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="de912-179">Next steps</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="de912-180">Adatbázis migrálása exportálással és importálással</span><span class="sxs-lookup"><span data-stu-id="de912-180">Migrate your database using Export and Import</span></span>](./concepts-migrate-import-export.md)
