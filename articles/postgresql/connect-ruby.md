---
title: "Ruby használatával PostgreSQL adatbázis aaaConnect tooAzure |} Microsoft Docs"
description: "A gyors üzembe helyezés biztosít egy Ruby kódminta PostgreSQL lekérdezése a Azure-adatbázis adatait és tooconnect használhatja."
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
ms.openlocfilehash: 7a0c8c92023452b40ca19d76fa659744f3e9a236
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-database-for-postgresql-use-ruby-tooconnect-and-query-data"></a><span data-ttu-id="116ad-103">Azure PostgreSQL-adatbázishoz: használata Ruby tooconnect és lekérdezési adatok</span><span class="sxs-lookup"><span data-stu-id="116ad-103">Azure Database for PostgreSQL: Use Ruby tooconnect and query data</span></span>
<span data-ttu-id="116ad-104">A gyors üzembe helyezés bemutatja, hogyan tooconnect tooan Azure PostgreSQL az adatbázis egy [Ruby](https://www.ruby-lang.org) alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="116ad-104">This quickstart demonstrates how tooconnect tooan Azure Database for PostgreSQL using a [Ruby](https://www.ruby-lang.org) application.</span></span> <span data-ttu-id="116ad-105">Azt illusztrálja, hogyan toouse SQL utasítás tooquery beszúrási, frissítési és törlési hello adatbázis adatait.</span><span class="sxs-lookup"><span data-stu-id="116ad-105">It shows how toouse SQL statements tooquery, insert, update, and delete data in hello database.</span></span> <span data-ttu-id="116ad-106">Ez a cikk feltételezi, hogy jártas használatával Ruby, azonban, hogy új tooworking PostgreSQL az Azure-adatbázissal.</span><span class="sxs-lookup"><span data-stu-id="116ad-106">This article assumes you are familiar with development using Ruby, but that you are new tooworking with Azure Database for PostgreSQL.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="116ad-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="116ad-107">Prerequisites</span></span>
<span data-ttu-id="116ad-108">A gyors üzembe helyezés kiindulási pontként ezek az útmutatók valamelyikével létrehozott hello erőforrást használ:</span><span class="sxs-lookup"><span data-stu-id="116ad-108">This quickstart uses hello resources created in either of these guides as a starting point:</span></span>
- [<span data-ttu-id="116ad-109">DB létrehozása – portál</span><span class="sxs-lookup"><span data-stu-id="116ad-109">Create DB - Portal</span></span>](quickstart-create-server-database-portal.md)
- [<span data-ttu-id="116ad-110">DB létrehozása – Azure CLI</span><span class="sxs-lookup"><span data-stu-id="116ad-110">Create DB - Azure CLI</span></span>](quickstart-create-server-database-azure-cli.md)

## <a name="install-ruby"></a><span data-ttu-id="116ad-111">A Ruby telepítése</span><span class="sxs-lookup"><span data-stu-id="116ad-111">Install Ruby</span></span>
<span data-ttu-id="116ad-112">Telepítse a Rubyt saját számítógépén.</span><span class="sxs-lookup"><span data-stu-id="116ad-112">Install Ruby on your own machine.</span></span> 

### <a name="windows"></a><span data-ttu-id="116ad-113">Windows</span><span class="sxs-lookup"><span data-stu-id="116ad-113">Windows</span></span>
- <span data-ttu-id="116ad-114">A letöltés és telepítés hello legújabb verziójának [Ruby](http://rubyinstaller.org/downloads/).</span><span class="sxs-lookup"><span data-stu-id="116ad-114">Download and Install hello latest version of [Ruby](http://rubyinstaller.org/downloads/).</span></span>
- <span data-ttu-id="116ad-115">Hello a Befejezés gombra a képernyő hello MSI-telepítő, jelölőnégyzetet hello, amely szerint a "Futtatás"ridk telepítése"tooinstall MSYS2 és fejlesztési toolchain."</span><span class="sxs-lookup"><span data-stu-id="116ad-115">On hello finish screen of hello MSI installer, check hello box that says "Run 'ridk install' tooinstall MSYS2 and development toolchain."</span></span> <span data-ttu-id="116ad-116">Kattintson a **Befejezés** toolaunch hello következő telepítő.</span><span class="sxs-lookup"><span data-stu-id="116ad-116">Then click **Finish** toolaunch hello next installer.</span></span>
- <span data-ttu-id="116ad-117">hello RubyInstaller2 a Windows telepítő elindítja.</span><span class="sxs-lookup"><span data-stu-id="116ad-117">hello RubyInstaller2 for Windows installer launches.</span></span> <span data-ttu-id="116ad-118">Adjon meg 2 tooinstall hello MSYS2 lemezképtár frissítése.</span><span class="sxs-lookup"><span data-stu-id="116ad-118">Type 2 tooinstall hello MSYS2 repository update.</span></span> <span data-ttu-id="116ad-119">Miután befejeződik, és toohello telepítési kérdés adja vissza, hello parancs ablak bezárásához.</span><span class="sxs-lookup"><span data-stu-id="116ad-119">After it finishes and returns toohello installation prompt, close hello command window.</span></span>
- <span data-ttu-id="116ad-120">Nyissa meg egy új parancssort (cmd) hello Start menüből.</span><span class="sxs-lookup"><span data-stu-id="116ad-120">Launch a new command prompt (cmd) from hello Start menu.</span></span>
- <span data-ttu-id="116ad-121">Teszt hello Ruby telepítési `ruby -v` toosee hello verziója van telepítve.</span><span class="sxs-lookup"><span data-stu-id="116ad-121">Test hello Ruby installation `ruby -v` toosee hello version installed.</span></span>
- <span data-ttu-id="116ad-122">Hello Gem telepítés sikerességének ellenőrzése `gem -v` toosee hello verziója van telepítve.</span><span class="sxs-lookup"><span data-stu-id="116ad-122">Test hello Gem installation `gem -v` toosee hello version installed.</span></span>
- <span data-ttu-id="116ad-123">Hello PostgreSQL modul építése Gem használatával hello parancs futtatásával Ruby `gem install pg`.</span><span class="sxs-lookup"><span data-stu-id="116ad-123">Build hello PostgreSQL module for Ruby using Gem by running hello command `gem install pg`.</span></span>

### <a name="macos"></a><span data-ttu-id="116ad-124">MacOS</span><span class="sxs-lookup"><span data-stu-id="116ad-124">MacOS</span></span>
- <span data-ttu-id="116ad-125">Ruby Homebrew használatával hello parancs futtatásával telepítse `brew install ruby`.</span><span class="sxs-lookup"><span data-stu-id="116ad-125">Install Ruby using Homebrew by running hello command `brew install ruby`.</span></span> <span data-ttu-id="116ad-126">További telepítési lehetőségekről, tekintse meg a hello Ruby [dokumentáció](https://www.ruby-lang.org/en/documentation/installation/#homebrew)</span><span class="sxs-lookup"><span data-stu-id="116ad-126">For more installation options, see hello Ruby [installation documentation](https://www.ruby-lang.org/en/documentation/installation/#homebrew)</span></span>
- <span data-ttu-id="116ad-127">Teszt hello Ruby telepítési `ruby -v` toosee hello verziója van telepítve.</span><span class="sxs-lookup"><span data-stu-id="116ad-127">Test hello Ruby installation `ruby -v` toosee hello version installed.</span></span>
- <span data-ttu-id="116ad-128">Hello Gem telepítés sikerességének ellenőrzése `gem -v` toosee hello verziója van telepítve.</span><span class="sxs-lookup"><span data-stu-id="116ad-128">Test hello Gem installation `gem -v` toosee hello version installed.</span></span>
- <span data-ttu-id="116ad-129">Hello PostgreSQL modul építése Gem használatával hello parancs futtatásával Ruby `gem install pg`.</span><span class="sxs-lookup"><span data-stu-id="116ad-129">Build hello PostgreSQL module for Ruby using Gem by running hello command `gem install pg`.</span></span>

### <a name="linux-ubuntu"></a><span data-ttu-id="116ad-130">Linux (Ubuntu)</span><span class="sxs-lookup"><span data-stu-id="116ad-130">Linux (Ubuntu)</span></span>
- <span data-ttu-id="116ad-131">Telepítse a Ruby hello parancs futtatásával `sudo apt-get install ruby-full`.</span><span class="sxs-lookup"><span data-stu-id="116ad-131">Install Ruby by running hello command `sudo apt-get install ruby-full`.</span></span> <span data-ttu-id="116ad-132">További telepítési lehetőségekről, tekintse meg a hello Ruby [dokumentáció](https://www.ruby-lang.org/en/documentation/installation/).</span><span class="sxs-lookup"><span data-stu-id="116ad-132">For more installation options, see hello Ruby [installation documentation](https://www.ruby-lang.org/en/documentation/installation/).</span></span>
- <span data-ttu-id="116ad-133">Teszt hello Ruby telepítési `ruby -v` toosee hello verziója van telepítve.</span><span class="sxs-lookup"><span data-stu-id="116ad-133">Test hello Ruby installation `ruby -v` toosee hello version installed.</span></span>
- <span data-ttu-id="116ad-134">Hello legújabb frissítéseinek telepítése Gem hello parancs futtatásával `sudo gem update --system`.</span><span class="sxs-lookup"><span data-stu-id="116ad-134">Install hello latest updates for Gem by running hello command `sudo gem update --system`.</span></span>
- <span data-ttu-id="116ad-135">Hello Gem telepítés sikerességének ellenőrzése `gem -v` toosee hello verziója van telepítve.</span><span class="sxs-lookup"><span data-stu-id="116ad-135">Test hello Gem installation `gem -v` toosee hello version installed.</span></span>
- <span data-ttu-id="116ad-136">Telepítse a hello ÖET, ellenőrizze és egyéb build tools hello parancs futtatásával `sudo apt-get install build-essential`.</span><span class="sxs-lookup"><span data-stu-id="116ad-136">Install hello gcc, make, and other build tools by running hello command `sudo apt-get install build-essential`.</span></span>
- <span data-ttu-id="116ad-137">Telepítse a hello PostgreSQL szalagtárak hello parancs futtatásával `sudo apt-get install libpq-dev`.</span><span class="sxs-lookup"><span data-stu-id="116ad-137">Install hello PostgreSQL libraries by running hello command `sudo apt-get install libpq-dev`.</span></span>
- <span data-ttu-id="116ad-138">Build hello Ruby pg modul használatával Gem hello parancs futtatásával `sudo gem install pg`.</span><span class="sxs-lookup"><span data-stu-id="116ad-138">Build hello Ruby pg module using Gem by running hello command `sudo gem install pg`.</span></span>

## <a name="run-ruby-code"></a><span data-ttu-id="116ad-139">Ruby-kód futtatása</span><span class="sxs-lookup"><span data-stu-id="116ad-139">Run Ruby code</span></span> 
- <span data-ttu-id="116ad-140">Hello kód egy szövegfájlba, és hello fájlt például a fájl kiterjesztése .rb, a projekt mappába Mentés `C:\rubypostgres\read.rb` vagy`/home/username/rubypostgres/read.rb`</span><span class="sxs-lookup"><span data-stu-id="116ad-140">Save hello code into a text file, and save hello file into a project folder with file extension .rb, such as `C:\rubypostgres\read.rb` or `/home/username/rubypostgres/read.rb`</span></span>
- <span data-ttu-id="116ad-141">toorun hello kódot, indítsa el a hello parancssor, vagy a bash rendszerhéjat.</span><span class="sxs-lookup"><span data-stu-id="116ad-141">toorun hello code, launch hello command prompt or bash shell.</span></span> <span data-ttu-id="116ad-142">A projekt mappába könyvtárváltás `cd rubypostgres`, majd írja be a hello parancsot `ruby read.rb` toorun hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="116ad-142">Change directory into your project folder `cd rubypostgres`, then type hello command `ruby read.rb` toorun hello application.</span></span>

## <a name="get-connection-information"></a><span data-ttu-id="116ad-143">Kapcsolatadatok lekérése</span><span class="sxs-lookup"><span data-stu-id="116ad-143">Get connection information</span></span>
<span data-ttu-id="116ad-144">Hello kapcsolat szükséges információkat tooconnect toohello Azure adatbázis beolvasása PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="116ad-144">Get hello connection information needed tooconnect toohello Azure Database for PostgreSQL.</span></span> <span data-ttu-id="116ad-145">Teljesen minősített kiszolgáló nevét és a bejelentkezési hitelesítő adatokat hello van szüksége.</span><span class="sxs-lookup"><span data-stu-id="116ad-145">You need hello fully qualified server name and login credentials.</span></span>

1. <span data-ttu-id="116ad-146">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="116ad-146">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="116ad-147">A hello Azure-portálon a bal oldali menüből, kattintson az **összes erőforrás** , és keressen a létrehozott, például a hello server **mypgserver-20170401**.</span><span class="sxs-lookup"><span data-stu-id="116ad-147">From hello left-hand menu in Azure portal, click **All resources** and search for hello server you have created, such as **mypgserver-20170401**.</span></span>
3. <span data-ttu-id="116ad-148">Hello kiszolgáló nevére kattint **mypgserver-20170401**.</span><span class="sxs-lookup"><span data-stu-id="116ad-148">Click hello server name **mypgserver-20170401**.</span></span>
4. <span data-ttu-id="116ad-149">Jelölje be hello server **áttekintése** lap.</span><span class="sxs-lookup"><span data-stu-id="116ad-149">Select hello server's **Overview** page.</span></span> <span data-ttu-id="116ad-150">Jegyezze fel a hello **kiszolgálónév** és **kiszolgálói rendszergazda bejelentkezési név**.</span><span class="sxs-lookup"><span data-stu-id="116ad-150">Make a note of hello **Server name** and **Server admin login name**.</span></span>
 <span data-ttu-id="116ad-151">![PostgreSQL-hez készült Azure-adatbázis – Kiszolgáló-rendszergazdai bejelentkezés](./media/connect-ruby/1-connection-string.png)</span><span class="sxs-lookup"><span data-stu-id="116ad-151">![Azure Database for PostgreSQL - Server Admin Login](./media/connect-ruby/1-connection-string.png)</span></span>
5. <span data-ttu-id="116ad-152">Ha elfelejti a kiszolgálói bejelentkezési adatok, keresse meg a toohello **áttekintése** lap tooview hello rendszergazda bejelentkezési nevet.</span><span class="sxs-lookup"><span data-stu-id="116ad-152">If you forget your server login information, navigate toohello **Overview** page tooview hello Server admin login name.</span></span> <span data-ttu-id="116ad-153">Ha szükséges, jelszó-átállítási hello.</span><span class="sxs-lookup"><span data-stu-id="116ad-153">If necessary, reset hello password.</span></span>

## <a name="connect-and-create-a-table"></a><span data-ttu-id="116ad-154">Csatlakozás és tábla létrehozása</span><span class="sxs-lookup"><span data-stu-id="116ad-154">Connect and create a table</span></span>
<span data-ttu-id="116ad-155">Használjon hello alábbi code tooconnect, és hozzon létre egy táblát az **CREATE TABLE** SQL-utasítást, és **INSERT INTO** SQL utasítás tooadd sorok hello táblába.</span><span class="sxs-lookup"><span data-stu-id="116ad-155">Use hello following code tooconnect and create a table using **CREATE TABLE** SQL statement, followed by **INSERT INTO** SQL statements tooadd rows into hello table.</span></span>

<span data-ttu-id="116ad-156">hello kódot használja a [PG::Connection](http://www.rubydoc.info/gems/pg/PG/Connection) konstruktorral objektum [hívás](http://www.rubydoc.info/gems/pg/PG%2FConnection:initialize) tooconnect tooAzure PostgreSQL-adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="116ad-156">hello code uses a  [PG::Connection](http://www.rubydoc.info/gems/pg/PG/Connection) object with constructor [new()](http://www.rubydoc.info/gems/pg/PG%2FConnection:initialize) tooconnect tooAzure Database for PostgreSQL.</span></span> <span data-ttu-id="116ad-157">Ezután a metódus meghívja [exec()](http://www.rubydoc.info/gems/pg/PG/Connection#exec-instance_method) toorun hello dobja el, CREATE TABLE és INSERT INTO parancsok.</span><span class="sxs-lookup"><span data-stu-id="116ad-157">Then it calls method [exec()](http://www.rubydoc.info/gems/pg/PG/Connection#exec-instance_method) toorun hello DROP, CREATE TABLE, and INSERT INTO commands.</span></span> <span data-ttu-id="116ad-158">hello kód ellenőrzi a hello segítségével hibákat [PG::Error](http://www.rubydoc.info/gems/pg/PG/Error) osztály.</span><span class="sxs-lookup"><span data-stu-id="116ad-158">hello code checks for errors using hello [PG::Error](http://www.rubydoc.info/gems/pg/PG/Error) class.</span></span> <span data-ttu-id="116ad-159">Ezután a metódus meghívja [close()](http://www.rubydoc.info/gems/pg/PG/Connection#lo_close-instance_method) tooclose hello kapcsolat megszakítása előtt.</span><span class="sxs-lookup"><span data-stu-id="116ad-159">Then it calls method [close()](http://www.rubydoc.info/gems/pg/PG/Connection#lo_close-instance_method) tooclose hello connection before terminating.</span></span>

<span data-ttu-id="116ad-160">Cserélje le a hello `host`, `database`, `user`, és `password` karakterláncok saját értékekkel.</span><span class="sxs-lookup"><span data-stu-id="116ad-160">Replace hello `host`, `database`, `user`, and `password` strings with your own values.</span></span> 
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
    puts 'Successfully created connection toodatabase'

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

## <a name="read-data"></a><span data-ttu-id="116ad-161">Adatok olvasása</span><span class="sxs-lookup"><span data-stu-id="116ad-161">Read data</span></span>
<span data-ttu-id="116ad-162">Használjon hello alábbi code tooconnect, és hello adatok segítségével olvassa a **kiválasztása** SQL-utasításban.</span><span class="sxs-lookup"><span data-stu-id="116ad-162">Use hello following code tooconnect and read hello data using a **SELECT** SQL statement.</span></span> 

<span data-ttu-id="116ad-163">hello kódot használja a [PG::Connection](http://www.rubydoc.info/gems/pg/PG/Connection) konstruktorral objektum [hívás](http://www.rubydoc.info/gems/pg/PG%2FConnection:initialize) tooconnect tooAzure PostgreSQL-adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="116ad-163">hello code uses a  [PG::Connection](http://www.rubydoc.info/gems/pg/PG/Connection) object with constructor [new()](http://www.rubydoc.info/gems/pg/PG%2FConnection:initialize) tooconnect tooAzure Database for PostgreSQL.</span></span> <span data-ttu-id="116ad-164">Ezután a metódus meghívja [exec()](http://www.rubydoc.info/gems/pg/PG/Connection#exec-instance_method) toorun hello kijelölési parancs, hello eredmények tartása egy eredményhalmazban szerepel.</span><span class="sxs-lookup"><span data-stu-id="116ad-164">Then it calls method [exec()](http://www.rubydoc.info/gems/pg/PG/Connection#exec-instance_method) toorun hello SELECT command, keeping hello results in a result set.</span></span> <span data-ttu-id="116ad-165">hello eredmény készlet gyűjteményéhez van többször is hello használatához képest `resultSet.each do` hurok hello aktuális sorérték megőrzi található hello `row` változó.</span><span class="sxs-lookup"><span data-stu-id="116ad-165">hello result set collection is iterated over using hello `resultSet.each do` loop, keeping hello current row values in hello `row` variable.</span></span> <span data-ttu-id="116ad-166">hello kód ellenőrzi a hello segítségével hibákat [PG::Error](http://www.rubydoc.info/gems/pg/PG/Error) osztály.</span><span class="sxs-lookup"><span data-stu-id="116ad-166">hello code checks for errors using hello [PG::Error](http://www.rubydoc.info/gems/pg/PG/Error) class.</span></span> <span data-ttu-id="116ad-167">Ezután a metódus meghívja [close()](http://www.rubydoc.info/gems/pg/PG/Connection#lo_close-instance_method) tooclose hello kapcsolat megszakítása előtt.</span><span class="sxs-lookup"><span data-stu-id="116ad-167">Then it calls method [close()](http://www.rubydoc.info/gems/pg/PG/Connection#lo_close-instance_method) tooclose hello connection before terminating.</span></span>

<span data-ttu-id="116ad-168">Cserélje le a hello `host`, `database`, `user`, és `password` karakterláncok saját értékekkel.</span><span class="sxs-lookup"><span data-stu-id="116ad-168">Replace hello `host`, `database`, `user`, and `password` strings with your own values.</span></span> 

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
    puts 'Successfully created connection toodatabase.'

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

## <a name="update-data"></a><span data-ttu-id="116ad-169">Adatok frissítése</span><span class="sxs-lookup"><span data-stu-id="116ad-169">Update data</span></span>
<span data-ttu-id="116ad-170">Használjon hello következő code tooconnect, és frissítse a hello adatok segítségével egy **frissítése** SQL-utasításban.</span><span class="sxs-lookup"><span data-stu-id="116ad-170">Use hello following code tooconnect and update hello data using a **UPDATE** SQL statement.</span></span>

<span data-ttu-id="116ad-171">hello kódot használja a [PG::Connection](http://www.rubydoc.info/gems/pg/PG/Connection) konstruktorral objektum [hívás](http://www.rubydoc.info/gems/pg/PG%2FConnection:initialize) tooconnect tooAzure PostgreSQL-adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="116ad-171">hello code uses a  [PG::Connection](http://www.rubydoc.info/gems/pg/PG/Connection) object with constructor [new()](http://www.rubydoc.info/gems/pg/PG%2FConnection:initialize) tooconnect tooAzure Database for PostgreSQL.</span></span> <span data-ttu-id="116ad-172">Ezután a metódus meghívja [exec()](http://www.rubydoc.info/gems/pg/PG/Connection#exec-instance_method) toorun hello frissítés parancsot.</span><span class="sxs-lookup"><span data-stu-id="116ad-172">Then it calls method [exec()](http://www.rubydoc.info/gems/pg/PG/Connection#exec-instance_method) toorun hello UPDATE command.</span></span> <span data-ttu-id="116ad-173">hello kód ellenőrzi a hello segítségével hibákat [PG::Error](http://www.rubydoc.info/gems/pg/PG/Error) osztály.</span><span class="sxs-lookup"><span data-stu-id="116ad-173">hello code checks for errors using hello [PG::Error](http://www.rubydoc.info/gems/pg/PG/Error) class.</span></span> <span data-ttu-id="116ad-174">Ezután a metódus meghívja [close()](http://www.rubydoc.info/gems/pg/PG/Connection#lo_close-instance_method) tooclose hello kapcsolat megszakítása előtt.</span><span class="sxs-lookup"><span data-stu-id="116ad-174">Then it calls method [close()](http://www.rubydoc.info/gems/pg/PG/Connection#lo_close-instance_method) tooclose hello connection before terminating.</span></span>

<span data-ttu-id="116ad-175">Cserélje le a hello `host`, `database`, `user`, és `password` karakterláncok saját értékekkel.</span><span class="sxs-lookup"><span data-stu-id="116ad-175">Replace hello `host`, `database`, `user`, and `password` strings with your own values.</span></span> 

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
    puts 'Successfully created connection toodatabase.'

    # Modify some data in table.
    connection.exec('UPDATE inventory SET quantity = %d WHERE name = %s;' % [200, '\'banana\''])
    puts 'Updated 1 row of data.'

rescue PG::Error => e
    puts e.message 
    
ensure
    connection.close if connection
end
```


## <a name="delete-data"></a><span data-ttu-id="116ad-176">Adat törlése</span><span class="sxs-lookup"><span data-stu-id="116ad-176">Delete data</span></span>
<span data-ttu-id="116ad-177">Használjon hello alábbi code tooconnect, és olvasott hello adatok egy **törlése** SQL-utasításban.</span><span class="sxs-lookup"><span data-stu-id="116ad-177">Use hello following code tooconnect and read hello data using a **DELETE** SQL statement.</span></span> 

<span data-ttu-id="116ad-178">hello kódot használja a [PG::Connection](http://www.rubydoc.info/gems/pg/PG/Connection) konstruktorral objektum [hívás](http://www.rubydoc.info/gems/pg/PG%2FConnection:initialize) tooconnect tooAzure PostgreSQL-adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="116ad-178">hello code uses a  [PG::Connection](http://www.rubydoc.info/gems/pg/PG/Connection) object with constructor [new()](http://www.rubydoc.info/gems/pg/PG%2FConnection:initialize) tooconnect tooAzure Database for PostgreSQL.</span></span> <span data-ttu-id="116ad-179">Ezután a metódus meghívja [exec()](http://www.rubydoc.info/gems/pg/PG/Connection#exec-instance_method) toorun hello frissítés parancsot.</span><span class="sxs-lookup"><span data-stu-id="116ad-179">Then it calls method [exec()](http://www.rubydoc.info/gems/pg/PG/Connection#exec-instance_method) toorun hello UPDATE command.</span></span> <span data-ttu-id="116ad-180">hello kód ellenőrzi a hello segítségével hibákat [PG::Error](http://www.rubydoc.info/gems/pg/PG/Error) osztály.</span><span class="sxs-lookup"><span data-stu-id="116ad-180">hello code checks for errors using hello [PG::Error](http://www.rubydoc.info/gems/pg/PG/Error) class.</span></span> <span data-ttu-id="116ad-181">Ezután a metódus meghívja [close()](http://www.rubydoc.info/gems/pg/PG/Connection#lo_close-instance_method) tooclose hello kapcsolat megszakítása előtt.</span><span class="sxs-lookup"><span data-stu-id="116ad-181">Then it calls method [close()](http://www.rubydoc.info/gems/pg/PG/Connection#lo_close-instance_method) tooclose hello connection before terminating.</span></span>

<span data-ttu-id="116ad-182">Cserélje le a hello `host`, `database`, `user`, és `password` karakterláncok saját értékekkel.</span><span class="sxs-lookup"><span data-stu-id="116ad-182">Replace hello `host`, `database`, `user`, and `password` strings with your own values.</span></span> 

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
    puts 'Successfully created connection toodatabase.'

    # Modify some data in table.
    connection.exec('DELETE FROM inventory WHERE name = %s;' % ['\'orange\''])
    puts 'Deleted 1 row of data.'

rescue PG::Error => e
    puts e.message 
    
ensure
    connection.close if connection
end
```

## <a name="next-steps"></a><span data-ttu-id="116ad-183">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="116ad-183">Next steps</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="116ad-184">Adatbázis migrálása exportálással és importálással</span><span class="sxs-lookup"><span data-stu-id="116ad-184">Migrate your database using Export and Import</span></span>](./howto-migrate-using-export-and-import.md)
