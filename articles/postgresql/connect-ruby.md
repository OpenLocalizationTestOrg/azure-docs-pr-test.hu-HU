---
title: "Csatlakozás a PostgreSQL-hez készült Azure Database-hez a Ruby használatával | Microsoft Docs"
description: "Az alábbi gyors útmutatóban egy olyan Ruby-kódminta található, amely a PostgreSQL-hez készült Azure Database csatlakoztatására és adatlekérdezésre használható."
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.custom: mvc
ms.devlang: ruby
ms.topic: quickstart
ms.date: 06/30/2017
ms.openlocfilehash: 9153a5a843dd5c18f27a3af232fea3b152240fe1
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="azure-database-for-postgresql-use-ruby-to-connect-and-query-data"></a><span data-ttu-id="bf4cb-103">A PostgreSQL-hez készült Azure Database: csatlakozás és adatlekérdezés a Ruby használatával</span><span class="sxs-lookup"><span data-stu-id="bf4cb-103">Azure Database for PostgreSQL: Use Ruby to connect and query data</span></span>
<span data-ttu-id="bf4cb-104">Ebben a gyors útmutatóban azt szemléltetjük, hogy miként lehet egy [Ruby](https://www.ruby-lang.org)-alkalmazás használatával csatlakozni a PostgreSQL-hez készült Azure Database-hez.</span><span class="sxs-lookup"><span data-stu-id="bf4cb-104">This quickstart demonstrates how to connect to an Azure Database for PostgreSQL using a [Ruby](https://www.ruby-lang.org) application.</span></span> <span data-ttu-id="bf4cb-105">Bemutatjuk, hogy az SQL-utasítások használatával hogyan kérdezhetők le, illeszthetők be, frissíthetők és törölhetők az adatok az adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="bf4cb-105">It shows how to use SQL statements to query, insert, update, and delete data in the database.</span></span> <span data-ttu-id="bf4cb-106">A jelen cikk azt feltételezi, hogy Ön már rendelkezik Ruby-fejlesztési tapasztalatokkal, de a PostgreSQL-hez készült Azure Database használatában pedig még járatlan.</span><span class="sxs-lookup"><span data-stu-id="bf4cb-106">This article assumes you are familiar with development using Ruby, but that you are new to working with Azure Database for PostgreSQL.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bf4cb-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="bf4cb-107">Prerequisites</span></span>
<span data-ttu-id="bf4cb-108">A rövid útmutató az alábbi útmutatók valamelyikében létrehozott erőforrásokat használja kiindulópontként:</span><span class="sxs-lookup"><span data-stu-id="bf4cb-108">This quickstart uses the resources created in either of these guides as a starting point:</span></span>
- [<span data-ttu-id="bf4cb-109">DB létrehozása – portál</span><span class="sxs-lookup"><span data-stu-id="bf4cb-109">Create DB - Portal</span></span>](quickstart-create-server-database-portal.md)
- [<span data-ttu-id="bf4cb-110">DB létrehozása – Azure CLI</span><span class="sxs-lookup"><span data-stu-id="bf4cb-110">Create DB - Azure CLI</span></span>](quickstart-create-server-database-azure-cli.md)

## <a name="install-ruby"></a><span data-ttu-id="bf4cb-111">A Ruby telepítése</span><span class="sxs-lookup"><span data-stu-id="bf4cb-111">Install Ruby</span></span>
<span data-ttu-id="bf4cb-112">Telepítse a Rubyt saját számítógépén.</span><span class="sxs-lookup"><span data-stu-id="bf4cb-112">Install Ruby on your own machine.</span></span> 

### <a name="windows"></a><span data-ttu-id="bf4cb-113">Windows</span><span class="sxs-lookup"><span data-stu-id="bf4cb-113">Windows</span></span>
- <span data-ttu-id="bf4cb-114">Töltse le, és telepítse a [Ruby](http://rubyinstaller.org/downloads/) legújabb verzióját.</span><span class="sxs-lookup"><span data-stu-id="bf4cb-114">Download and Install the latest version of [Ruby](http://rubyinstaller.org/downloads/).</span></span>
- <span data-ttu-id="bf4cb-115">Az MSI telepítő befejező képernyőjén jelölje be a „Run 'ridk install' to install MSYS2 and development toolchain” (A ridk install parancs futtatása az MSYS2 és a fejlesztési eszközlánc telepítéséhez) jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="bf4cb-115">On the finish screen of the MSI installer, check the box that says "Run 'ridk install' to install MSYS2 and development toolchain."</span></span> <span data-ttu-id="bf4cb-116">Kattintson a **Befejezés** gombra a következő telepítő elindításához.</span><span class="sxs-lookup"><span data-stu-id="bf4cb-116">Then click **Finish** to launch the next installer.</span></span>
- <span data-ttu-id="bf4cb-117">Ekkor elindul l aRubyInstaller2 for Windows telepítője.</span><span class="sxs-lookup"><span data-stu-id="bf4cb-117">The RubyInstaller2 for Windows installer launches.</span></span> <span data-ttu-id="bf4cb-118">Az MSYS2-adattárfrissítés telepítéséhez nyomja le a 2 billentyűt.</span><span class="sxs-lookup"><span data-stu-id="bf4cb-118">Type 2 to install the MSYS2 repository update.</span></span> <span data-ttu-id="bf4cb-119">Miután befejeződik a telepítés, és visszatér a telepítő parancssorába, zárja be a parancssor ablakát.</span><span class="sxs-lookup"><span data-stu-id="bf4cb-119">After it finishes and returns to the installation prompt, close the command window.</span></span>
- <span data-ttu-id="bf4cb-120">Nyisson meg egy új parancssort (cmd) a Start menüből.</span><span class="sxs-lookup"><span data-stu-id="bf4cb-120">Launch a new command prompt (cmd) from the Start menu.</span></span>
- <span data-ttu-id="bf4cb-121">Ellenőrizze a Ruby-telepítés sikerességét a telepített verzió megtekintésével: `ruby -v`.</span><span class="sxs-lookup"><span data-stu-id="bf4cb-121">Test the Ruby installation `ruby -v` to see the version installed.</span></span>
- <span data-ttu-id="bf4cb-122">Ellenőrizze a Gem-telepítés sikerességét a telepített verzió megtekintésével: `gem -v`.</span><span class="sxs-lookup"><span data-stu-id="bf4cb-122">Test the Gem installation `gem -v` to see the version installed.</span></span>
- <span data-ttu-id="bf4cb-123">A következő parancs futtatásával készítse el a PostgreSQL-modult a Ruby-hoz a Gem használatával: `gem install pg`.</span><span class="sxs-lookup"><span data-stu-id="bf4cb-123">Build the PostgreSQL module for Ruby using Gem by running the command `gem install pg`.</span></span>

### <a name="macos"></a><span data-ttu-id="bf4cb-124">MacOS</span><span class="sxs-lookup"><span data-stu-id="bf4cb-124">MacOS</span></span>
- <span data-ttu-id="bf4cb-125">A következő parancs futtatásával telepítse a Rubyt a Homebrew használatával: `brew install ruby`.</span><span class="sxs-lookup"><span data-stu-id="bf4cb-125">Install Ruby using Homebrew by running the command `brew install ruby`.</span></span> <span data-ttu-id="bf4cb-126">A további telepítési lehetőségekről a Ruby [telepítési dokumentációjából](https://www.ruby-lang.org/en/documentation/installation/#homebrew) tájékozódhat</span><span class="sxs-lookup"><span data-stu-id="bf4cb-126">For more installation options, see the Ruby [installation documentation](https://www.ruby-lang.org/en/documentation/installation/#homebrew)</span></span>
- <span data-ttu-id="bf4cb-127">Ellenőrizze a Ruby-telepítés sikerességét a telepített verzió megtekintésével: `ruby -v`.</span><span class="sxs-lookup"><span data-stu-id="bf4cb-127">Test the Ruby installation `ruby -v` to see the version installed.</span></span>
- <span data-ttu-id="bf4cb-128">Ellenőrizze a Gem-telepítés sikerességét a telepített verzió megtekintésével: `gem -v`.</span><span class="sxs-lookup"><span data-stu-id="bf4cb-128">Test the Gem installation `gem -v` to see the version installed.</span></span>
- <span data-ttu-id="bf4cb-129">A következő parancs futtatásával készítse el a PostgreSQL-modult a Ruby-hoz a Gem használatával: `gem install pg`.</span><span class="sxs-lookup"><span data-stu-id="bf4cb-129">Build the PostgreSQL module for Ruby using Gem by running the command `gem install pg`.</span></span>

### <a name="linux-ubuntu"></a><span data-ttu-id="bf4cb-130">Linux (Ubuntu)</span><span class="sxs-lookup"><span data-stu-id="bf4cb-130">Linux (Ubuntu)</span></span>
- <span data-ttu-id="bf4cb-131">A következő parancs futtatásával telepítse a Rubyt: `sudo apt-get install ruby-full`.</span><span class="sxs-lookup"><span data-stu-id="bf4cb-131">Install Ruby by running the command `sudo apt-get install ruby-full`.</span></span> <span data-ttu-id="bf4cb-132">A további telepítési lehetőségekről a Ruby [telepítési dokumentációjából](https://www.ruby-lang.org/en/documentation/installation/) tájékozódhat.</span><span class="sxs-lookup"><span data-stu-id="bf4cb-132">For more installation options, see the Ruby [installation documentation](https://www.ruby-lang.org/en/documentation/installation/).</span></span>
- <span data-ttu-id="bf4cb-133">Ellenőrizze a Ruby-telepítés sikerességét a telepített verzió megtekintésével: `ruby -v`.</span><span class="sxs-lookup"><span data-stu-id="bf4cb-133">Test the Ruby installation `ruby -v` to see the version installed.</span></span>
- <span data-ttu-id="bf4cb-134">Telepítse a legújabb Gem-frissítéseket a következő parancs futtatásával: `sudo gem update --system`.</span><span class="sxs-lookup"><span data-stu-id="bf4cb-134">Install the latest updates for Gem by running the command `sudo gem update --system`.</span></span>
- <span data-ttu-id="bf4cb-135">Ellenőrizze a Gem-telepítés sikerességét a telepített verzió megtekintésével: `gem -v`.</span><span class="sxs-lookup"><span data-stu-id="bf4cb-135">Test the Gem installation `gem -v` to see the version installed.</span></span>
- <span data-ttu-id="bf4cb-136">A következő parancs futtatásával telepítse a gcc, a make és az egyéb összeállítási eszközöket: `sudo apt-get install build-essential`.</span><span class="sxs-lookup"><span data-stu-id="bf4cb-136">Install the gcc, make, and other build tools by running the command `sudo apt-get install build-essential`.</span></span>
- <span data-ttu-id="bf4cb-137">Telepítése a PostgreSQL-kódtárakat a `sudo apt-get install libpq-dev` parancs futtatásával.</span><span class="sxs-lookup"><span data-stu-id="bf4cb-137">Install the PostgreSQL libraries by running the command `sudo apt-get install libpq-dev`.</span></span>
- <span data-ttu-id="bf4cb-138">Hozza létre a Ruby pg modult a Gem használatával a `sudo gem install pg` parancs futtatásával.</span><span class="sxs-lookup"><span data-stu-id="bf4cb-138">Build the Ruby pg module using Gem by running the command `sudo gem install pg`.</span></span>

## <a name="run-ruby-code"></a><span data-ttu-id="bf4cb-139">Ruby-kód futtatása</span><span class="sxs-lookup"><span data-stu-id="bf4cb-139">Run Ruby code</span></span> 
- <span data-ttu-id="bf4cb-140">Mentse a kódot egy szövegfájlba, és mentse a fájlt egy projektmappába az .rb fájlkiterjesztéssel, például: `C:\rubypostgres\read.rb` vagy `/home/username/rubypostgres/read.rb`</span><span class="sxs-lookup"><span data-stu-id="bf4cb-140">Save the code into a text file, and save the file into a project folder with file extension .rb, such as `C:\rubypostgres\read.rb` or `/home/username/rubypostgres/read.rb`</span></span>
- <span data-ttu-id="bf4cb-141">A kód futtatásához nyissa meg a parancssort vagy a bash rendszerhéjat.</span><span class="sxs-lookup"><span data-stu-id="bf4cb-141">To run the code, launch the command prompt or bash shell.</span></span> <span data-ttu-id="bf4cb-142">Lépjen a projektmappába a `cd rubypostgres` paranccsal, majd írja be a `ruby read.rb` parancsot az alkalmazás futtatásához.</span><span class="sxs-lookup"><span data-stu-id="bf4cb-142">Change directory into your project folder `cd rubypostgres`, then type the command `ruby read.rb` to run the application.</span></span>

## <a name="get-connection-information"></a><span data-ttu-id="bf4cb-143">Kapcsolatadatok lekérése</span><span class="sxs-lookup"><span data-stu-id="bf4cb-143">Get connection information</span></span>
<span data-ttu-id="bf4cb-144">Kérje le a PostgreSQL-hez készült Azure-adatbázishoz való csatlakozáshoz szükséges kapcsolatadatokat.</span><span class="sxs-lookup"><span data-stu-id="bf4cb-144">Get the connection information needed to connect to the Azure Database for PostgreSQL.</span></span> <span data-ttu-id="bf4cb-145">Szüksége lesz a teljes kiszolgálónévre és a bejelentkezési hitelesítő adatokra.</span><span class="sxs-lookup"><span data-stu-id="bf4cb-145">You need the fully qualified server name and login credentials.</span></span>

1. <span data-ttu-id="bf4cb-146">Jelentkezzen be az [Azure portálra](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="bf4cb-146">Log in to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="bf4cb-147">Az Azure Portal bal oldali menüjében kattintson az **Összes erőforrás** lehetőségre, és keressen rá a létrehozott kiszolgálóra (például **mypgserver-20170401**).</span><span class="sxs-lookup"><span data-stu-id="bf4cb-147">From the left-hand menu in Azure portal, click **All resources** and search for the server you have created, such as **mypgserver-20170401**.</span></span>
3. <span data-ttu-id="bf4cb-148">Kattintson a **mypgserver-20170401** kiszolgálónévre.</span><span class="sxs-lookup"><span data-stu-id="bf4cb-148">Click the server name **mypgserver-20170401**.</span></span>
4. <span data-ttu-id="bf4cb-149">Válassza ki a kiszolgáló **Áttekintés** oldalát.</span><span class="sxs-lookup"><span data-stu-id="bf4cb-149">Select the server's **Overview** page.</span></span> <span data-ttu-id="bf4cb-150">Jegyezze fel a **Kiszolgálónevet** és a **Kiszolgáló-rendszergazdai bejelentkezési nevet**.</span><span class="sxs-lookup"><span data-stu-id="bf4cb-150">Make a note of the **Server name** and **Server admin login name**.</span></span>
 <span data-ttu-id="bf4cb-151">![Azure-adatbázis PostgreSQL-hez - Kiszolgáló-rendszergazdai bejelentkezés](./media/connect-ruby/1-connection-string.png)</span><span class="sxs-lookup"><span data-stu-id="bf4cb-151">![Azure Database for PostgreSQL - Server Admin Login](./media/connect-ruby/1-connection-string.png)</span></span>
5. <span data-ttu-id="bf4cb-152">Amennyiben elfelejtette a kiszolgáló bejelentkezési adatait, lépjen az **Overview** (Áttekintés) oldalra, és itt megtudhatja a kiszolgáló rendszergazdájának bejelentkezési nevét.</span><span class="sxs-lookup"><span data-stu-id="bf4cb-152">If you forget your server login information, navigate to the **Overview** page to view the Server admin login name.</span></span> <span data-ttu-id="bf4cb-153">Szükség esetén állítsa alaphelyzetbe a jelszót.</span><span class="sxs-lookup"><span data-stu-id="bf4cb-153">If necessary, reset the password.</span></span>

## <a name="connect-and-create-a-table"></a><span data-ttu-id="bf4cb-154">Csatlakozás és tábla létrehozása</span><span class="sxs-lookup"><span data-stu-id="bf4cb-154">Connect and create a table</span></span>
<span data-ttu-id="bf4cb-155">A következő kód segítségével csatlakozzon, és hozzon létre egy táblát a **CREATE TABLE** SQL-utasítással, majd az **INSERT INTO** SQL-utasítással adjon hozzá sorokat a táblához.</span><span class="sxs-lookup"><span data-stu-id="bf4cb-155">Use the following code to connect and create a table using **CREATE TABLE** SQL statement, followed by **INSERT INTO** SQL statements to add rows into the table.</span></span>

<span data-ttu-id="bf4cb-156">A kód egy [PG::Connection](http://www.rubydoc.info/gems/pg/PG/Connection) objektumot használ a [new()](http://www.rubydoc.info/gems/pg/PG%2FConnection:initialize) konstruktorral a PostgreSQL-hez készült Azure Database-hez való kapcsolódáshoz.</span><span class="sxs-lookup"><span data-stu-id="bf4cb-156">The code uses a  [PG::Connection](http://www.rubydoc.info/gems/pg/PG/Connection) object with constructor [new()](http://www.rubydoc.info/gems/pg/PG%2FConnection:initialize) to connect to Azure Database for PostgreSQL.</span></span> <span data-ttu-id="bf4cb-157">Ezután meghívja az [exec()](http://www.rubydoc.info/gems/pg/PG/Connection#exec-instance_method) metódust a DROP, CREATE TABLE és INSERT INTO parancsok futtatásához.</span><span class="sxs-lookup"><span data-stu-id="bf4cb-157">Then it calls method [exec()](http://www.rubydoc.info/gems/pg/PG/Connection#exec-instance_method) to run the DROP, CREATE TABLE, and INSERT INTO commands.</span></span> <span data-ttu-id="bf4cb-158">A kód a [PG::Error](http://www.rubydoc.info/gems/pg/PG/Error) osztály használatával ellenőrzi a hibákat.</span><span class="sxs-lookup"><span data-stu-id="bf4cb-158">The code checks for errors using the [PG::Error](http://www.rubydoc.info/gems/pg/PG/Error) class.</span></span> <span data-ttu-id="bf4cb-159">Végül pedig a [close()](http://www.rubydoc.info/gems/pg/PG/Connection#lo_close-instance_method) metódus meghívásával bontja a kapcsolatot, mielőtt kilép.</span><span class="sxs-lookup"><span data-stu-id="bf4cb-159">Then it calls method [close()](http://www.rubydoc.info/gems/pg/PG/Connection#lo_close-instance_method) to close the connection before terminating.</span></span>

<span data-ttu-id="bf4cb-160">Cserélje le a `host`, `database`, `user`, és `password` karakterláncokat a saját értékekre.</span><span class="sxs-lookup"><span data-stu-id="bf4cb-160">Replace the `host`, `database`, `user`, and `password` strings with your own values.</span></span> 
```ruby
require 'pg'

begin
    # Initialize connection variables.
    host = String('mypgserver-20170401.postgres.database.azure.com')
    database = String('postgres')
    user = String('mylogin@mypgserver-20170401')
    password = String('<server_admin_password>')

    # Initialize connection object.
    connection = PG::Connection.new(:host => host, :user => user, :dbname => database, :port => '5432', :password => password)
    puts 'Successfully created connection to database'

    # Drop previous table of same name if one exists
    connection.exec('DROP TABLE IF EXISTS inventory;')
    puts 'Finished dropping table (if existed).'

    # Drop previous table of same name if one exists.
    connection.exec('CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);')
    puts 'Finished creating table.'

    # Insert some data into table.
    connection.exec("INSERT INTO inventory VALUES(1, 'banana', 150)")
    connection.exec("INSERT INTO inventory VALUES(2, 'orange', 154)")
    connection.exec("INSERT INTO inventory VALUES(3, 'apple', 100)")
    puts 'Inserted 3 rows of data.'

rescue PG::Error => e
    puts e.message 
    
ensure
    connection.close if connection
end
```

## <a name="read-data"></a><span data-ttu-id="bf4cb-161">Adatok olvasása</span><span class="sxs-lookup"><span data-stu-id="bf4cb-161">Read data</span></span>
<span data-ttu-id="bf4cb-162">Az alábbi kód használatával csatlakozhat és végezheti el az adatok olvasását az adott **SELECT** SQL-utasítás segítségével.</span><span class="sxs-lookup"><span data-stu-id="bf4cb-162">Use the following code to connect and read the data using a **SELECT** SQL statement.</span></span> 

<span data-ttu-id="bf4cb-163">A kód egy [PG::Connection](http://www.rubydoc.info/gems/pg/PG/Connection) objektumot használ a [new()](http://www.rubydoc.info/gems/pg/PG%2FConnection:initialize) konstruktorral a PostgreSQL-hez készült Azure Database-hez való kapcsolódáshoz.</span><span class="sxs-lookup"><span data-stu-id="bf4cb-163">The code uses a  [PG::Connection](http://www.rubydoc.info/gems/pg/PG/Connection) object with constructor [new()](http://www.rubydoc.info/gems/pg/PG%2FConnection:initialize) to connect to Azure Database for PostgreSQL.</span></span> <span data-ttu-id="bf4cb-164">Ezután meghívja az [exec()](http://www.rubydoc.info/gems/pg/PG/Connection#exec-instance_method) metódust a SELECT parancs futtatásához, az eredményeket az eredményhalmazban megőrizve.</span><span class="sxs-lookup"><span data-stu-id="bf4cb-164">Then it calls method [exec()](http://www.rubydoc.info/gems/pg/PG/Connection#exec-instance_method) to run the SELECT command, keeping the results in a result set.</span></span> <span data-ttu-id="bf4cb-165">Az eredményhalmaz gyűjtése többször is végrehajtódik a `resultSet.each do` ciklus használatával, megőrizve az aktuális sor értékeit a `row` változóban.</span><span class="sxs-lookup"><span data-stu-id="bf4cb-165">The result set collection is iterated over using the `resultSet.each do` loop, keeping the current row values in the `row` variable.</span></span> <span data-ttu-id="bf4cb-166">A kód a [PG::Error](http://www.rubydoc.info/gems/pg/PG/Error) osztály használatával ellenőrzi a hibákat.</span><span class="sxs-lookup"><span data-stu-id="bf4cb-166">The code checks for errors using the [PG::Error](http://www.rubydoc.info/gems/pg/PG/Error) class.</span></span> <span data-ttu-id="bf4cb-167">Végül pedig a [close()](http://www.rubydoc.info/gems/pg/PG/Connection#lo_close-instance_method) metódus meghívásával bontja a kapcsolatot, mielőtt kilép.</span><span class="sxs-lookup"><span data-stu-id="bf4cb-167">Then it calls method [close()](http://www.rubydoc.info/gems/pg/PG/Connection#lo_close-instance_method) to close the connection before terminating.</span></span>

<span data-ttu-id="bf4cb-168">Cserélje le a `host`, `database`, `user`, és `password` karakterláncokat a saját értékekre.</span><span class="sxs-lookup"><span data-stu-id="bf4cb-168">Replace the `host`, `database`, `user`, and `password` strings with your own values.</span></span> 

```ruby
require 'pg'

begin
    # Initialize connection variables.
    host = String('mypgserver-20170401.postgres.database.azure.com')
    database = String('postgres')
    user = String('mylogin@mypgserver-20170401')
    password = String('<server_admin_password>')

    # Initialize connection object.
    connection = PG::Connection.new(:host => host, :user => user, :database => dbname, :port => '5432', :password => password)
    puts 'Successfully created connection to database.'

    resultSet = connection.exec('SELECT * from inventory;')
    resultSet.each do |row|
        puts 'Data row = (%s, %s, %s)' % [row['id'], row['name'], row['quantity']]
    end

rescue PG::Error => e
    puts e.message 
    
ensure
    connection.close if connection
end
```

## <a name="update-data"></a><span data-ttu-id="bf4cb-169">Adatok frissítése</span><span class="sxs-lookup"><span data-stu-id="bf4cb-169">Update data</span></span>
<span data-ttu-id="bf4cb-170">Az alábbi kód használatával csatlakozhat és végezheti el az adatok módosítását egy **UPDATE** SQL-utasítás segítségével.</span><span class="sxs-lookup"><span data-stu-id="bf4cb-170">Use the following code to connect and update the data using a **UPDATE** SQL statement.</span></span>

<span data-ttu-id="bf4cb-171">A kód egy [PG::Connection](http://www.rubydoc.info/gems/pg/PG/Connection) objektumot használ a [new()](http://www.rubydoc.info/gems/pg/PG%2FConnection:initialize) konstruktorral a PostgreSQL-hez készült Azure Database-hez való kapcsolódáshoz.</span><span class="sxs-lookup"><span data-stu-id="bf4cb-171">The code uses a  [PG::Connection](http://www.rubydoc.info/gems/pg/PG/Connection) object with constructor [new()](http://www.rubydoc.info/gems/pg/PG%2FConnection:initialize) to connect to Azure Database for PostgreSQL.</span></span> <span data-ttu-id="bf4cb-172">Ezután meghívja az [exec()](http://www.rubydoc.info/gems/pg/PG/Connection#exec-instance_method) metódust az UPDATE parancs futtatásához.</span><span class="sxs-lookup"><span data-stu-id="bf4cb-172">Then it calls method [exec()](http://www.rubydoc.info/gems/pg/PG/Connection#exec-instance_method) to run the UPDATE command.</span></span> <span data-ttu-id="bf4cb-173">A kód a [PG::Error](http://www.rubydoc.info/gems/pg/PG/Error) osztály használatával ellenőrzi a hibákat.</span><span class="sxs-lookup"><span data-stu-id="bf4cb-173">The code checks for errors using the [PG::Error](http://www.rubydoc.info/gems/pg/PG/Error) class.</span></span> <span data-ttu-id="bf4cb-174">Végül pedig a [close()](http://www.rubydoc.info/gems/pg/PG/Connection#lo_close-instance_method) metódus meghívásával bontja a kapcsolatot, mielőtt kilép.</span><span class="sxs-lookup"><span data-stu-id="bf4cb-174">Then it calls method [close()](http://www.rubydoc.info/gems/pg/PG/Connection#lo_close-instance_method) to close the connection before terminating.</span></span>

<span data-ttu-id="bf4cb-175">Cserélje le a `host`, `database`, `user`, és `password` karakterláncokat a saját értékekre.</span><span class="sxs-lookup"><span data-stu-id="bf4cb-175">Replace the `host`, `database`, `user`, and `password` strings with your own values.</span></span> 

```ruby
require 'pg'

begin
    # Initialize connection variables.
    host = String('mypgserver-20170401.postgres.database.azure.com')
    database = String('postgres')
    user = String('mylogin@mypgserver-20170401')
    password = String('<server_admin_password>')

    # Initialize connection object.
    connection = PG::Connection.new(:host => host, :user => user, :dbname => database, :port => '5432', :password => password)
    puts 'Successfully created connection to database.'

    # Modify some data in table.
    connection.exec('UPDATE inventory SET quantity = %d WHERE name = %s;' % [200, '\'banana\''])
    puts 'Updated 1 row of data.'

rescue PG::Error => e
    puts e.message 
    
ensure
    connection.close if connection
end
```


## <a name="delete-data"></a><span data-ttu-id="bf4cb-176">Adat törlése</span><span class="sxs-lookup"><span data-stu-id="bf4cb-176">Delete data</span></span>
<span data-ttu-id="bf4cb-177">Az alábbi kód használatával csatlakozhat és végezheti el az adatok olvasását egy **DELETE** SQL-utasítás segítségével.</span><span class="sxs-lookup"><span data-stu-id="bf4cb-177">Use the following code to connect and read the data using a **DELETE** SQL statement.</span></span> 

<span data-ttu-id="bf4cb-178">A kód egy [PG::Connection](http://www.rubydoc.info/gems/pg/PG/Connection) objektumot használ a [new()](http://www.rubydoc.info/gems/pg/PG%2FConnection:initialize) konstruktorral a PostgreSQL-hez készült Azure Database-hez való kapcsolódáshoz.</span><span class="sxs-lookup"><span data-stu-id="bf4cb-178">The code uses a  [PG::Connection](http://www.rubydoc.info/gems/pg/PG/Connection) object with constructor [new()](http://www.rubydoc.info/gems/pg/PG%2FConnection:initialize) to connect to Azure Database for PostgreSQL.</span></span> <span data-ttu-id="bf4cb-179">Ezután meghívja az [exec()](http://www.rubydoc.info/gems/pg/PG/Connection#exec-instance_method) metódust az UPDATE parancs futtatásához.</span><span class="sxs-lookup"><span data-stu-id="bf4cb-179">Then it calls method [exec()](http://www.rubydoc.info/gems/pg/PG/Connection#exec-instance_method) to run the UPDATE command.</span></span> <span data-ttu-id="bf4cb-180">A kód a [PG::Error](http://www.rubydoc.info/gems/pg/PG/Error) osztály használatával ellenőrzi a hibákat.</span><span class="sxs-lookup"><span data-stu-id="bf4cb-180">The code checks for errors using the [PG::Error](http://www.rubydoc.info/gems/pg/PG/Error) class.</span></span> <span data-ttu-id="bf4cb-181">Végül pedig a [close()](http://www.rubydoc.info/gems/pg/PG/Connection#lo_close-instance_method) metódus meghívásával bontja a kapcsolatot, mielőtt kilép.</span><span class="sxs-lookup"><span data-stu-id="bf4cb-181">Then it calls method [close()](http://www.rubydoc.info/gems/pg/PG/Connection#lo_close-instance_method) to close the connection before terminating.</span></span>

<span data-ttu-id="bf4cb-182">Cserélje le a `host`, `database`, `user`, és `password` karakterláncokat a saját értékekre.</span><span class="sxs-lookup"><span data-stu-id="bf4cb-182">Replace the `host`, `database`, `user`, and `password` strings with your own values.</span></span> 

```ruby
require 'pg'

begin
    # Initialize connection variables.
    host = String('mypgserver-20170401.postgres.database.azure.com')
    database = String('postgres')
    user = String('mylogin@mypgserver-20170401')
    password = String('<server_admin_password>')

    # Initialize connection object.
    connection = PG::Connection.new(:host => host, :user => user, :dbname => database, :port => '5432', :password => password)
    puts 'Successfully created connection to database.'

    # Modify some data in table.
    connection.exec('DELETE FROM inventory WHERE name = %s;' % ['\'orange\''])
    puts 'Deleted 1 row of data.'

rescue PG::Error => e
    puts e.message 
    
ensure
    connection.close if connection
end
```

## <a name="next-steps"></a><span data-ttu-id="bf4cb-183">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="bf4cb-183">Next steps</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="bf4cb-184">Adatbázis migrálása exportálással és importálással</span><span class="sxs-lookup"><span data-stu-id="bf4cb-184">Migrate your database using Export and Import</span></span>](./howto-migrate-using-export-and-import.md)
