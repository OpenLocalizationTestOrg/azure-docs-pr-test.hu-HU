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
# <a name="azure-database-for-postgresql-use-python-tooconnect-and-query-data"></a><span data-ttu-id="d5782-103">Azure PostgreSQL-adatbázishoz: használata Python tooconnect és lekérdezési adatok</span><span class="sxs-lookup"><span data-stu-id="d5782-103">Azure Database for PostgreSQL: Use Python tooconnect and query data</span></span>
<span data-ttu-id="d5782-104">A gyors üzembe helyezés bemutatja, hogyan toouse [Python](https://python.org) tooconnect tooan PostgreSQL az Azure-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="d5782-104">This quickstart demonstrates how toouse [Python](https://python.org) tooconnect tooan Azure Database for PostgreSQL.</span></span> <span data-ttu-id="d5782-105">Azt is bemutatja, hogyan toouse SQL utasítás tooquery beszúrási, frissítési és törlési hello adatbázisban macOS Ubuntu Linux és Windows-platform.</span><span class="sxs-lookup"><span data-stu-id="d5782-105">It also demonstrates how toouse SQL statements tooquery, insert, update, and delete data in hello database from macOS, Ubuntu Linux, and Windows platforms.</span></span> <span data-ttu-id="d5782-106">Ez a cikk hello lépések azt feltételezik, ismeri a Python használatával történő fejlesztéséhez, és a rendszer új tooworking PostgreSQL az Azure-adatbázissal.</span><span class="sxs-lookup"><span data-stu-id="d5782-106">hello steps in this article assume that you are familiar with developing using Python and are new tooworking with Azure Database for PostgreSQL.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d5782-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="d5782-107">Prerequisites</span></span>
<span data-ttu-id="d5782-108">A gyors üzembe helyezés kiindulási pontként ezek az útmutatók valamelyikével létrehozott hello erőforrást használ:</span><span class="sxs-lookup"><span data-stu-id="d5782-108">This quickstart uses hello resources created in either of these guides as a starting point:</span></span>
- [<span data-ttu-id="d5782-109">DB létrehozása – portál</span><span class="sxs-lookup"><span data-stu-id="d5782-109">Create DB - Portal</span></span>](quickstart-create-server-database-portal.md)
- [<span data-ttu-id="d5782-110">DB létrehozása – CLI</span><span class="sxs-lookup"><span data-stu-id="d5782-110">Create DB - CLI</span></span>](quickstart-create-server-database-azure-cli.md)

<span data-ttu-id="d5782-111">Emellett a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="d5782-111">You also need:</span></span>
- <span data-ttu-id="d5782-112">telepített [python](https://www.python.org/downloads/)</span><span class="sxs-lookup"><span data-stu-id="d5782-112">[python](https://www.python.org/downloads/) installed</span></span>
- <span data-ttu-id="d5782-113">telepített [pip](https://pip.pypa.io/en/stable/installing/) csomag (a pip már telepítve van, ha a [python.org](https://python.org) webhelyről letöltött Python 2 >=2.7.9 vagy Python 3 >=3.4 bináris fájlokat használ.</span><span class="sxs-lookup"><span data-stu-id="d5782-113">[pip](https://pip.pypa.io/en/stable/installing/) package installed (pip is already installed if you're working with Python 2 >=2.7.9 or Python 3 >=3.4 binaries downloaded from [python.org](https://python.org).</span></span>

## <a name="install-hello-python-connection-libraries-for-postgresql"></a><span data-ttu-id="d5782-114">Hello Python adatkapcsolattárak PostgreSQL telepítése</span><span class="sxs-lookup"><span data-stu-id="d5782-114">Install hello Python connection libraries for PostgreSQL</span></span>
<span data-ttu-id="d5782-115">Telepítse a hello [psycopg2](http://initd.org/psycopg/docs/install.html) csomag, amely lehetővé teszi tooconnect és lekérdezés hello adatbázis.</span><span class="sxs-lookup"><span data-stu-id="d5782-115">Install hello [psycopg2](http://initd.org/psycopg/docs/install.html) package, which enables you tooconnect and query hello database.</span></span> <span data-ttu-id="d5782-116">psycopg2 van [PyPI elérhető](https://pypi.python.org/pypi/psycopg2/) hello formájában [kerék](http://pythonwheels.com/) csomagok hello leggyakoribb platformokhoz (Linux, os x, Windows).</span><span class="sxs-lookup"><span data-stu-id="d5782-116">psycopg2 is [available on PyPI](https://pypi.python.org/pypi/psycopg2/) in hello form of [wheel](http://pythonwheels.com/) packages for hello most common platforms (Linux, OSX, Windows).</span></span> <span data-ttu-id="d5782-117">Használja a pip tooget hello bináris verzió hello modul, beleértve az összes hello függőségek telepítése.</span><span class="sxs-lookup"><span data-stu-id="d5782-117">Use pip install tooget hello binary version of hello module including all hello dependencies.</span></span>

1. <span data-ttu-id="d5782-118">A számítógépen indítsa el a parancssori felületet:</span><span class="sxs-lookup"><span data-stu-id="d5782-118">On your own computer, launch a command-line interface:</span></span>
    - <span data-ttu-id="d5782-119">Linux indítsa el a hello Bash rendszerhéjat.</span><span class="sxs-lookup"><span data-stu-id="d5782-119">On Linux, launch hello Bash shell.</span></span>
    - <span data-ttu-id="d5782-120">A macOS indítsa el a Terminálszolgáltatások hello.</span><span class="sxs-lookup"><span data-stu-id="d5782-120">On macOS, launch hello Terminal.</span></span>
    - <span data-ttu-id="d5782-121">A Windows indítsa el a hello parancssorból származó hello Start menüben.</span><span class="sxs-lookup"><span data-stu-id="d5782-121">On Windows, launch hello Command Prompt from hello Start Menu.</span></span>
2. <span data-ttu-id="d5782-122">Győződjön meg arról, hogy használ hello legújabb verziója a pip parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="d5782-122">Ensure that you are using hello most current version of pip by running a command such as:</span></span>
    ```cmd
    pip install -U pip
    ```

3. <span data-ttu-id="d5782-123">Futtassa a következő parancs tooinstall hello psycopg2 csomag hello:</span><span class="sxs-lookup"><span data-stu-id="d5782-123">Run hello following command tooinstall hello psycopg2 package:</span></span>
    ```cmd
    pip install psycopg2
    ```

## <a name="get-connection-information"></a><span data-ttu-id="d5782-124">Kapcsolatadatok lekérése</span><span class="sxs-lookup"><span data-stu-id="d5782-124">Get connection information</span></span>
<span data-ttu-id="d5782-125">Hello kapcsolat szükséges információkat tooconnect toohello Azure adatbázis beolvasása PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="d5782-125">Get hello connection information needed tooconnect toohello Azure Database for PostgreSQL.</span></span> <span data-ttu-id="d5782-126">Teljesen minősített kiszolgáló nevét és a bejelentkezési hitelesítő adatokat hello van szüksége.</span><span class="sxs-lookup"><span data-stu-id="d5782-126">You need hello fully qualified server name and login credentials.</span></span>

1. <span data-ttu-id="d5782-127">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="d5782-127">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="d5782-128">A hello Azure-portálon a bal oldali menüből, kattintson az **összes erőforrás** keresse meg a **mypgserver-20170401** (az imént létrehozott hello kiszolgáló).</span><span class="sxs-lookup"><span data-stu-id="d5782-128">From hello left-hand menu in Azure portal, click **All resources** and search for **mypgserver-20170401** (hello server you just created).</span></span>
3. <span data-ttu-id="d5782-129">Hello kiszolgáló nevére kattint **mypgserver-20170401**.</span><span class="sxs-lookup"><span data-stu-id="d5782-129">Click hello server name **mypgserver-20170401**.</span></span>
4. <span data-ttu-id="d5782-130">Jelölje be hello server **áttekintése** lapon, majd jegyezze fel a hello **kiszolgálónév** és **kiszolgálói rendszergazda bejelentkezési név**.</span><span class="sxs-lookup"><span data-stu-id="d5782-130">Select hello server's **Overview** page, and then make a note of hello **Server name** and **Server admin login name**.</span></span>
 <span data-ttu-id="d5782-131">![PostgreSQL-hez készült Azure-adatbázis – Kiszolgáló-rendszergazdai bejelentkezés](./media/connect-python/1-connection-string.png)</span><span class="sxs-lookup"><span data-stu-id="d5782-131">![Azure Database for PostgreSQL - Server Admin Login](./media/connect-python/1-connection-string.png)</span></span>
5. <span data-ttu-id="d5782-132">Ha elfelejti a kiszolgálói bejelentkezési adatok, keresse meg a toohello **áttekintése** tooview hello kiszolgálói rendszergazda bejelentkezési név lapon, és ha szükséges, állítsa vissza a hello jelszót.</span><span class="sxs-lookup"><span data-stu-id="d5782-132">If you forget your server login information, navigate toohello **Overview** page tooview hello Server admin login name and, if necessary, reset hello password.</span></span>

## <a name="how-toorun-python-code"></a><span data-ttu-id="d5782-133">Hogyan toorun Python kódot</span><span class="sxs-lookup"><span data-stu-id="d5782-133">How toorun Python code</span></span>
<span data-ttu-id="d5782-134">Ez a témakör összesen négy kódmintát tartalmaz, amelyek mindegyike egy adott funkciót hajt végre.</span><span class="sxs-lookup"><span data-stu-id="d5782-134">This topic contains a total of four code samples, each of which performs a specific function.</span></span> <span data-ttu-id="d5782-135">hello következő utasítások jelzik, hogy hogyan toocreate egy szövegfájlt beszúrni a kódblokk, és mentse hello fájlt, így később is futtathatja.</span><span class="sxs-lookup"><span data-stu-id="d5782-135">hello following instructions indicate how toocreate a text file, insert a code block, and then save hello file so that you can run it later.</span></span> <span data-ttu-id="d5782-136">Meg arról, hogy toocreate négy külön fájlt, egy az egyes kódblokk lehet.</span><span class="sxs-lookup"><span data-stu-id="d5782-136">Be sure toocreate four separate files, one for each code block.</span></span>

- <span data-ttu-id="d5782-137">Hozzon létre egy új fájlt a kedvenc szövegszerkesztőjével.</span><span class="sxs-lookup"><span data-stu-id="d5782-137">Using your favorite text editor, create a new file.</span></span>
- <span data-ttu-id="d5782-138">Másolással illessze be egy hello Kódminták a következő részekben hello szövegfájlba hello.</span><span class="sxs-lookup"><span data-stu-id="d5782-138">Copy and paste one of hello code samples in hello following sections into hello text file.</span></span> <span data-ttu-id="d5782-139">Cserélje le a hello **állomás**, **dbname**, **felhasználói**, és **jelszó** hello értékekkel hello létrehozásakor megadott paraméterek kiszolgáló és az adatbázis.</span><span class="sxs-lookup"><span data-stu-id="d5782-139">Replace hello **host**, **dbname**, **user**, and **password** parameters with hello values that you specified when you created hello server and database.</span></span>
- <span data-ttu-id="d5782-140">A projekt mappába hello .py bővítmény (például postgres.py) mentse hello fájlt.</span><span class="sxs-lookup"><span data-stu-id="d5782-140">Save hello file with hello .py extension (for example postgres.py) into your project folder.</span></span> <span data-ttu-id="d5782-141">Ha hello Windows operációs rendszer fut, lehet, hogy tooselect UTF-8 kódolást hello fájl mentése során.</span><span class="sxs-lookup"><span data-stu-id="d5782-141">If you are running hello Windows OS, be sure tooselect UTF-8 encoding when saving hello file.</span></span> 
- <span data-ttu-id="d5782-142">Indítsa el a hello parancssor vagy a Bash rendszerhéjat, és módosítsa az hello könyvtár tooyour projekt mappájában, például `cd postgres`.</span><span class="sxs-lookup"><span data-stu-id="d5782-142">Launch hello Command Prompt or Bash shell and then change hello directory tooyour project folder, for example `cd postgres`.</span></span>
-  <span data-ttu-id="d5782-143">toorun hello kód típus hello Python parancsot, majd hello fájl nevét, például `Python postgres.py`.</span><span class="sxs-lookup"><span data-stu-id="d5782-143">toorun hello code, type hello Python command followed by hello file name, for example `Python postgres.py`.</span></span>

> [!NOTE]
> <span data-ttu-id="d5782-144">A 3-as verziójú Python-től kezdődően jelenhetnek hello hiba `SyntaxError: Missing parentheses in call too'print'` a jelentés futtatásakor a következő kódblokkok hello.</span><span class="sxs-lookup"><span data-stu-id="d5782-144">Starting in Python version 3, you may see hello error `SyntaxError: Missing parentheses in call too'print'` when running hello following code blocks.</span></span> <span data-ttu-id="d5782-145">Ha ez előfordul, cserélje le minden hívás toohello parancs `print "string"` zárójeleket tartalmazhat, mint például használatával függvény hívással `print("string")`.</span><span class="sxs-lookup"><span data-stu-id="d5782-145">If that happens, replace each call toohello command `print "string"` with a function call using parenthesis, such as `print("string")`.</span></span>

## <a name="connect-create-table-and-insert-data"></a><span data-ttu-id="d5782-146">Csatlakozás, táblák létrehozása és adatok beszúrása</span><span class="sxs-lookup"><span data-stu-id="d5782-146">Connect, create table, and insert data</span></span>
<span data-ttu-id="d5782-147">Használjon hello következő code tooconnect, és betölti a hello használatával végzett [psycopg2.connect](http://initd.org/psycopg/docs/connection.html) és **BESZÚRÁSA** SQL-utasításban.</span><span class="sxs-lookup"><span data-stu-id="d5782-147">Use hello following code tooconnect and load hello data using [psycopg2.connect](http://initd.org/psycopg/docs/connection.html) function with **INSERT** SQL statement.</span></span> <span data-ttu-id="d5782-148">Hello [cursor.execute](http://initd.org/psycopg/docs/cursor.html#execute) függvény PostgreSQL-adatbázishoz használt tooexecute hello SQL-lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="d5782-148">hello [cursor.execute](http://initd.org/psycopg/docs/cursor.html#execute) function is used tooexecute hello SQL query against PostgreSQL database.</span></span> <span data-ttu-id="d5782-149">Cserélje le a hello gazdagép, a dbname, a felhasználó és a password paraméter hello kiszolgáló és az adatbázis létrehozásakor adott hello értékekkel.</span><span class="sxs-lookup"><span data-stu-id="d5782-149">Replace hello host, dbname, user, and password parameters with hello values that you specified when you created hello server and database.</span></span>

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

<span data-ttu-id="d5782-150">Miután hello kód befejeződött, hello kimenete a következőképpen jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="d5782-150">After hello code runs successfully, hello output appears as follows:</span></span>

![Parancssori kimenet](media/connect-python/2-example-python-output.png)

## <a name="read-data"></a><span data-ttu-id="d5782-152">Adatok olvasása</span><span class="sxs-lookup"><span data-stu-id="d5782-152">Read data</span></span>
<span data-ttu-id="d5782-153">Használjon hello következő kódot tooread hello adatokat beszúrni a használatával [cursor.execute](http://initd.org/psycopg/docs/cursor.html#execute) működik az **VÁLASSZA** SQL-utasításban.</span><span class="sxs-lookup"><span data-stu-id="d5782-153">Use hello following code tooread hello data inserted using [cursor.execute](http://initd.org/psycopg/docs/cursor.html#execute) function with **SELECT** SQL statement.</span></span> <span data-ttu-id="d5782-154">Ezt a függvényt fogad el egy lekérdezést, és csak egy eredményt, amely képes többször keresztül hello használatával [cursor.fetchall()](http://initd.org/psycopg/docs/cursor.html#cursor.fetchall).</span><span class="sxs-lookup"><span data-stu-id="d5782-154">This function accepts a query and returns a result set that can be iterated over with hello use of [cursor.fetchall()](http://initd.org/psycopg/docs/cursor.html#cursor.fetchall).</span></span> <span data-ttu-id="d5782-155">Cserélje le a hello gazdagép, a dbname, a felhasználó és a password paraméter hello kiszolgáló és az adatbázis létrehozásakor adott hello értékekkel.</span><span class="sxs-lookup"><span data-stu-id="d5782-155">Replace hello host, dbname, user, and password parameters with hello values that you specified when you created hello server and database.</span></span>

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

## <a name="update-data"></a><span data-ttu-id="d5782-156">Adatok frissítése</span><span class="sxs-lookup"><span data-stu-id="d5782-156">Update data</span></span>
<span data-ttu-id="d5782-157">Használjon hello következő tooupdate hello készlet sor előzőleg beszúrt használatával kódot [cursor.execute](http://initd.org/psycopg/docs/cursor.html#execute) és **frissítés** SQL-utasításban.</span><span class="sxs-lookup"><span data-stu-id="d5782-157">Use hello following code tooupdate hello inventory row that you previously inserted using [cursor.execute](http://initd.org/psycopg/docs/cursor.html#execute) function with **UPDATE** SQL statement.</span></span> <span data-ttu-id="d5782-158">Cserélje le a hello gazdagép, a dbname, a felhasználó és a password paraméter hello kiszolgáló és az adatbázis létrehozásakor adott hello értékekkel.</span><span class="sxs-lookup"><span data-stu-id="d5782-158">Replace hello host, dbname, user, and password parameters with hello values that you specified when you created hello server and database.</span></span>

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

## <a name="delete-data"></a><span data-ttu-id="d5782-159">Adat törlése</span><span class="sxs-lookup"><span data-stu-id="d5782-159">Delete data</span></span>
<span data-ttu-id="d5782-160">Használjon hello következő toodelete korábban beszúrt használatával készlet elem kódot [cursor.execute](http://initd.org/psycopg/docs/cursor.html#execute) működik az **törlése** SQL-utasításban.</span><span class="sxs-lookup"><span data-stu-id="d5782-160">Use hello following code toodelete an inventory item that you previously inserted using [cursor.execute](http://initd.org/psycopg/docs/cursor.html#execute) function with **DELETE** SQL statement.</span></span> <span data-ttu-id="d5782-161">Cserélje le a hello gazdagép, a dbname, a felhasználó és a password paraméter hello kiszolgáló és az adatbázis létrehozásakor adott hello értékekkel.</span><span class="sxs-lookup"><span data-stu-id="d5782-161">Replace hello host, dbname, user, and password parameters with hello values that you specified when you created hello server and database.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="d5782-162">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d5782-162">Next steps</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="d5782-163">Adatbázis migrálása exportálással és importálással</span><span class="sxs-lookup"><span data-stu-id="d5782-163">Migrate your database using Export and Import</span></span>](./howto-migrate-using-export-and-import.md)
