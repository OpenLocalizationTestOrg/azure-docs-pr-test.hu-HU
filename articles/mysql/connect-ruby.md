---
title: "Ruby használatával MySQL-adatbázis tooAzure kapcsolati |} Microsoft Docs"
description: "A gyors üzembe helyezés biztosít több Ruby mintakódok tooconnect használhat és MySQL az Azure-adatbázis adatait lekérdezése."
services: mysql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.custom: mvc
ms.devlang: ruby
ms.topic: hero-article
ms.date: 07/13/2017
ms.openlocfilehash: ff0880dcc24e96f467c9092bc663ce3dc4c2637a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-database-for-mysql-use-ruby-tooconnect-and-query-data"></a><span data-ttu-id="25295-103">MySQL az Azure-adatbázishoz: használata Ruby tooconnect és lekérdezési adatok</span><span class="sxs-lookup"><span data-stu-id="25295-103">Azure Database for MySQL: Use Ruby tooconnect and query data</span></span>
<span data-ttu-id="25295-104">A gyors üzembe helyezés bemutatja, hogyan tooconnect tooan Azure adatbázis a MySQL használata egy [Ruby](https://www.ruby-lang.org) alkalmazás és hello [mysql2](https://rubygems.org/gems/mysql2) a Windows, Ubuntu Linux és Mac rendszerek gem.</span><span class="sxs-lookup"><span data-stu-id="25295-104">This quickstart demonstrates how tooconnect tooan Azure Database for MySQL using a [Ruby](https://www.ruby-lang.org) application and hello [mysql2](https://rubygems.org/gems/mysql2) gem from Windows, Ubuntu Linux, and Mac platforms.</span></span> <span data-ttu-id="25295-105">Azt illusztrálja, hogyan toouse SQL utasítás tooquery beszúrási, frissítési és törlési hello adatbázis adatait.</span><span class="sxs-lookup"><span data-stu-id="25295-105">It shows how toouse SQL statements tooquery, insert, update, and delete data in hello database.</span></span> <span data-ttu-id="25295-106">Ez a cikk feltételezi, hogy jártas használatával Ruby, azonban, hogy új tooworking MySQL az Azure-adatbázissal.</span><span class="sxs-lookup"><span data-stu-id="25295-106">This article assumes you are familiar with development using Ruby, but that you are new tooworking with Azure Database for MySQL.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="25295-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="25295-107">Prerequisites</span></span>
<span data-ttu-id="25295-108">A gyors üzembe helyezés kiindulási pontként ezek az útmutatók valamelyikével létrehozott hello erőforrást használ:</span><span class="sxs-lookup"><span data-stu-id="25295-108">This quickstart uses hello resources created in either of these guides as a starting point:</span></span>
- [<span data-ttu-id="25295-109">Azure-adatbázis létrehozása MySQL-kiszolgálóhoz az Azure Portal használatával</span><span class="sxs-lookup"><span data-stu-id="25295-109">Create an Azure Database for MySQL server using Azure portal</span></span>](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [<span data-ttu-id="25295-110">Azure-adatbázis létrehozása MySQL-kiszolgálóhoz az Azure CLI használatával</span><span class="sxs-lookup"><span data-stu-id="25295-110">Create an Azure Database for MySQL server using Azure CLI</span></span>](./quickstart-create-mysql-server-database-using-azure-cli.md)

## <a name="install-ruby"></a><span data-ttu-id="25295-111">A Ruby telepítése</span><span class="sxs-lookup"><span data-stu-id="25295-111">Install Ruby</span></span>
<span data-ttu-id="25295-112">Telepítse a saját gép Ruby, Gem és hello MySQL2 könyvtár.</span><span class="sxs-lookup"><span data-stu-id="25295-112">Install Ruby, Gem, and hello MySQL2 library on your own machine.</span></span> 

### <a name="windows"></a><span data-ttu-id="25295-113">Windows</span><span class="sxs-lookup"><span data-stu-id="25295-113">Windows</span></span>
1. <span data-ttu-id="25295-114">A letöltés és telepítés hello 2.3 verziójának [Ruby](http://rubyinstaller.org/downloads/).</span><span class="sxs-lookup"><span data-stu-id="25295-114">Download and Install hello 2.3 version of [Ruby](http://rubyinstaller.org/downloads/).</span></span>
2. <span data-ttu-id="25295-115">Nyissa meg egy új parancssort (cmd) hello Start menüből.</span><span class="sxs-lookup"><span data-stu-id="25295-115">Launch a new command prompt (cmd) from hello Start menu.</span></span>
3. <span data-ttu-id="25295-116">Módosítsa a könyvtárat hello verziójához 2.3 Ruby directory be.</span><span class="sxs-lookup"><span data-stu-id="25295-116">Change directory into hello Ruby directory for version 2.3.</span></span> `cd c:\Ruby23-x64\bin`
4. <span data-ttu-id="25295-117">Teszt hello hello parancs futtatásával Ruby telepítési `ruby -v` toosee hello verziója van telepítve.</span><span class="sxs-lookup"><span data-stu-id="25295-117">Test hello Ruby installation by running hello command `ruby -v` toosee hello version installed.</span></span>
5. <span data-ttu-id="25295-118">Hello Gem telepítés sikerességének ellenőrzése hello parancs futtatásával `gem -v` toosee hello verziója van telepítve.</span><span class="sxs-lookup"><span data-stu-id="25295-118">Test hello Gem installation by running hello command `gem -v` toosee hello version installed.</span></span>
6. <span data-ttu-id="25295-119">Hello Mysql2 modul építése Gem használatával hello parancs futtatásával Ruby `gem install mysql2`.</span><span class="sxs-lookup"><span data-stu-id="25295-119">Build hello Mysql2 module for Ruby using Gem by running hello command `gem install mysql2`.</span></span>

### <a name="macos"></a><span data-ttu-id="25295-120">MacOS</span><span class="sxs-lookup"><span data-stu-id="25295-120">MacOS</span></span>
1. <span data-ttu-id="25295-121">Ruby Homebrew használatával hello parancs futtatásával telepítse `brew install ruby`.</span><span class="sxs-lookup"><span data-stu-id="25295-121">Install Ruby using Homebrew by running hello command `brew install ruby`.</span></span> <span data-ttu-id="25295-122">További telepítési lehetőségekről, tekintse meg a hello Ruby [dokumentáció](https://www.ruby-lang.org/en/documentation/installation/#homebrew).</span><span class="sxs-lookup"><span data-stu-id="25295-122">For more installation options, see hello Ruby [installation documentation](https://www.ruby-lang.org/en/documentation/installation/#homebrew).</span></span>
2. <span data-ttu-id="25295-123">Teszt hello hello parancs futtatásával Ruby telepítési `ruby -v` toosee hello verziója van telepítve.</span><span class="sxs-lookup"><span data-stu-id="25295-123">Test hello Ruby installation by running hello command `ruby -v` toosee hello version installed.</span></span>
3. <span data-ttu-id="25295-124">Hello Gem telepítés sikerességének ellenőrzése hello parancs futtatásával `gem -v` toosee hello verziója van telepítve.</span><span class="sxs-lookup"><span data-stu-id="25295-124">Test hello Gem installation by running hello command `gem -v` toosee hello version installed.</span></span>
4. <span data-ttu-id="25295-125">Hello Mysql2 modul építése Gem használatával hello parancs futtatásával Ruby `gem install mysql2`.</span><span class="sxs-lookup"><span data-stu-id="25295-125">Build hello Mysql2 module for Ruby using Gem by running hello command `gem install mysql2`.</span></span>

### <a name="linux-ubuntu"></a><span data-ttu-id="25295-126">Linux (Ubuntu)</span><span class="sxs-lookup"><span data-stu-id="25295-126">Linux (Ubuntu)</span></span>
1. <span data-ttu-id="25295-127">Telepítse a Ruby hello parancs futtatásával `sudo apt-get install ruby-full`.</span><span class="sxs-lookup"><span data-stu-id="25295-127">Install Ruby by running hello command `sudo apt-get install ruby-full`.</span></span> <span data-ttu-id="25295-128">További telepítési lehetőségekről, tekintse meg a hello Ruby [dokumentáció](https://www.ruby-lang.org/en/documentation/installation/).</span><span class="sxs-lookup"><span data-stu-id="25295-128">For more installation options, see hello Ruby [installation documentation](https://www.ruby-lang.org/en/documentation/installation/).</span></span>
2. <span data-ttu-id="25295-129">Teszt hello hello parancs futtatásával Ruby telepítési `ruby -v` toosee hello verziója van telepítve.</span><span class="sxs-lookup"><span data-stu-id="25295-129">Test hello Ruby installation by running hello command `ruby -v` toosee hello version installed.</span></span>
3. <span data-ttu-id="25295-130">Hello legújabb frissítéseinek telepítése Gem hello parancs futtatásával `sudo gem update --system`.</span><span class="sxs-lookup"><span data-stu-id="25295-130">Install hello latest updates for Gem by running hello command `sudo gem update --system`.</span></span>
4. <span data-ttu-id="25295-131">Hello Gem telepítés sikerességének ellenőrzése hello parancs futtatásával `gem -v` toosee hello verziója van telepítve.</span><span class="sxs-lookup"><span data-stu-id="25295-131">Test hello Gem installation by running hello command `gem -v` toosee hello version installed.</span></span>
5. <span data-ttu-id="25295-132">Telepítse a hello ÖET, ellenőrizze és egyéb build tools hello parancs futtatásával `sudo apt-get install build-essential`.</span><span class="sxs-lookup"><span data-stu-id="25295-132">Install hello gcc, make, and other build tools by running hello command `sudo apt-get install build-essential`.</span></span>
6. <span data-ttu-id="25295-133">Telepítse a hello MySQL klienskódtárak fejlesztői hello parancs futtatásával `sudo apt-get install libmysqlclient-dev`.</span><span class="sxs-lookup"><span data-stu-id="25295-133">Install hello MySQL client developer libraries by running hello command `sudo apt-get install libmysqlclient-dev`.</span></span>
7. <span data-ttu-id="25295-134">Hello mysql2 modul építése Gem használatával hello parancs futtatásával Ruby `sudo gem install mysql2`.</span><span class="sxs-lookup"><span data-stu-id="25295-134">Build hello mysql2 module for Ruby using Gem by running hello command `sudo gem install mysql2`.</span></span>

## <a name="get-connection-information"></a><span data-ttu-id="25295-135">Kapcsolatadatok lekérése</span><span class="sxs-lookup"><span data-stu-id="25295-135">Get connection information</span></span>
<span data-ttu-id="25295-136">MySQL hello kapcsolat szükséges információkat tooconnect toohello Azure adatbázis beolvasása.</span><span class="sxs-lookup"><span data-stu-id="25295-136">Get hello connection information needed tooconnect toohello Azure Database for MySQL.</span></span> <span data-ttu-id="25295-137">Teljesen minősített kiszolgáló nevét és a bejelentkezési hitelesítő adatokat hello van szüksége.</span><span class="sxs-lookup"><span data-stu-id="25295-137">You need hello fully qualified server name and login credentials.</span></span>

1. <span data-ttu-id="25295-138">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="25295-138">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="25295-139">A hello Azure-portálon a bal oldali menüből, kattintson az **összes erőforrás** , és keresse meg rendelkezik gyűrött, például a hello kiszolgáló **myserver4demo**.</span><span class="sxs-lookup"><span data-stu-id="25295-139">From hello left-hand menu in Azure portal, click **All resources** and search for hello server you have creased, such as **myserver4demo**.</span></span>
3. <span data-ttu-id="25295-140">Hello kiszolgáló nevére kattint **myserver4demo**.</span><span class="sxs-lookup"><span data-stu-id="25295-140">Click hello server name **myserver4demo**.</span></span>
4. <span data-ttu-id="25295-141">Jelölje be hello server **tulajdonságok** lap.</span><span class="sxs-lookup"><span data-stu-id="25295-141">Select hello server's **Properties** page.</span></span> <span data-ttu-id="25295-142">Jegyezze fel a hello **kiszolgálónév** és **kiszolgálói rendszergazda bejelentkezési név**.</span><span class="sxs-lookup"><span data-stu-id="25295-142">Make a note of hello **Server name** and **Server admin login name**.</span></span>
 <span data-ttu-id="25295-143">![MySQL-hez készült Azure-adatbázis – Kiszolgáló-rendszergazdai bejelentkezés](./media/connect-ruby/1_server-properties-name-login.png)</span><span class="sxs-lookup"><span data-stu-id="25295-143">![Azure Database for MySQL - Server Admin Login](./media/connect-ruby/1_server-properties-name-login.png)</span></span>
5. <span data-ttu-id="25295-144">Ha elfelejti a kiszolgálói bejelentkezési adatok, keresse meg a toohello **áttekintése** tooview hello kiszolgálói rendszergazda bejelentkezési név lapon, és ha szükséges, állítsa vissza a hello jelszót.</span><span class="sxs-lookup"><span data-stu-id="25295-144">If you forget your server login information, navigate toohello **Overview** page tooview hello Server admin login name and, if necessary, reset hello password.</span></span>

## <a name="run-ruby-code"></a><span data-ttu-id="25295-145">Ruby-kód futtatása</span><span class="sxs-lookup"><span data-stu-id="25295-145">Run Ruby code</span></span> 
1. <span data-ttu-id="25295-146">Hello hello szakaszokat Ruby kód beillesztése szövegfájlok, fájljait és mentse el hello egy projekt mappába, a fájl kiterjesztése .rb, például a `C:\rubymysql\createtable.rb` vagy `/home/username/rubymysql/createtable.rb`.</span><span class="sxs-lookup"><span data-stu-id="25295-146">Paste hello Ruby code from hello sections below into text files, and save hello files into a project folder with file extension .rb, such as `C:\rubymysql\createtable.rb` or `/home/username/rubymysql/createtable.rb`.</span></span>
2. <span data-ttu-id="25295-147">toorun hello kódot, indítsa el a hello parancssor, vagy a bash rendszerhéjat.</span><span class="sxs-lookup"><span data-stu-id="25295-147">toorun hello code, launch hello command prompt or bash shell.</span></span> <span data-ttu-id="25295-148">Lépjen a projektmappára: `cd rubymysql`</span><span class="sxs-lookup"><span data-stu-id="25295-148">Change directory into your project folder `cd rubymysql`</span></span>
3. <span data-ttu-id="25295-149">Írja be a hello ruby parancsot, majd hello fájl nevét, például a `ruby createtable.rb` toorun hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="25295-149">Then type hello ruby command followed by hello file name, such as `ruby createtable.rb` toorun hello application.</span></span>
4. <span data-ttu-id="25295-150">A Windows operációs rendszer hello Ha hello ruby alkalmazás nem szerepel a path környezeti változóba, szükség lehet toouse hello teljes elérési útja toolaunch hello csomópont alkalmazás, például a`"c:\Ruby23-x64\bin\ruby.exe" createtable.rb`</span><span class="sxs-lookup"><span data-stu-id="25295-150">On hello Windows OS, if hello ruby application is not in your path environment variable, you may need toouse hello full path toolaunch hello node application, such as `"c:\Ruby23-x64\bin\ruby.exe" createtable.rb`</span></span>

## <a name="connect-and-create-a-table"></a><span data-ttu-id="25295-151">Csatlakozás és tábla létrehozása</span><span class="sxs-lookup"><span data-stu-id="25295-151">Connect and create a table</span></span>
<span data-ttu-id="25295-152">Használjon hello alábbi code tooconnect, és hozzon létre egy táblát az **CREATE TABLE** SQL-utasítást, és **INSERT INTO** SQL utasítás tooadd sorok hello táblába.</span><span class="sxs-lookup"><span data-stu-id="25295-152">Use hello following code tooconnect and create a table using **CREATE TABLE** SQL statement, followed by **INSERT INTO** SQL statements tooadd rows into hello table.</span></span>

<span data-ttu-id="25295-153">hello kódot használja a [mysql2::client](http://www.rubydoc.info/gems/mysql2/0.4.8) MySQL .new() metódus tooconnect tooAzure adatbázis osztályt.</span><span class="sxs-lookup"><span data-stu-id="25295-153">hello code uses a [mysql2::client](http://www.rubydoc.info/gems/mysql2/0.4.8) class .new() method tooconnect tooAzure Database for MySQL.</span></span> <span data-ttu-id="25295-154">Ezután a metódus meghívja [query()](http://www.rubydoc.info/gems/mysql2/0.4.8#Usage) többször toorun hello dobja el, CREATE TABLE és INSERT INTO parancsok.</span><span class="sxs-lookup"><span data-stu-id="25295-154">Then it calls method [query()](http://www.rubydoc.info/gems/mysql2/0.4.8#Usage) several times toorun hello DROP, CREATE TABLE, and INSERT INTO commands.</span></span> <span data-ttu-id="25295-155">Ezután a metódus meghívja [close()](http://www.rubydoc.info/gems/mysql2/0.4.8/Mysql2/Client#close-instance_method) tooclose hello kapcsolat megszakítása előtt.</span><span class="sxs-lookup"><span data-stu-id="25295-155">Then it calls method [close()](http://www.rubydoc.info/gems/mysql2/0.4.8/Mysql2/Client#close-instance_method) tooclose hello connection before terminating.</span></span>

<span data-ttu-id="25295-156">Cserélje le a hello `host`, `database`, `username`, és `password` karakterláncok saját értékekkel.</span><span class="sxs-lookup"><span data-stu-id="25295-156">Replace hello `host`, `database`, `username`, and `password` strings with your own values.</span></span> 
```ruby
require 'mysql2'

begin
    # Initialize connection variables.
    host = String('myserver4demo.mysql.database.azure.com')
    database = String('quickstartdb')
    username = String('myadmin@myserver4demo')
    password = String('yourpassword')

    # Initialize connection object.
    client = Mysql2::Client.new(:host => host, :username => username, :database => database, :password => password)
    puts 'Successfully created connection toodatabase.'

    # Drop previous table of same name if one exists
    client.query('DROP TABLE IF EXISTS inventory;')
    puts 'Finished dropping table (if existed).'

    # Drop previous table of same name if one exists.
    client.query('CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);')
    puts 'Finished creating table.'

    # Insert some data into table.
    client.query("INSERT INTO inventory VALUES(1, 'banana', 150)")
    client.query("INSERT INTO inventory VALUES(2, 'orange', 154)")
    client.query("INSERT INTO inventory VALUES(3, 'apple', 100)")
    puts 'Inserted 3 rows of data.'

# Error handling
rescue Exception => e
    puts e.message

# Cleanup
ensure
    client.close if client
    puts 'Done.'
end
```

## <a name="read-data"></a><span data-ttu-id="25295-157">Adatok olvasása</span><span class="sxs-lookup"><span data-stu-id="25295-157">Read data</span></span>
<span data-ttu-id="25295-158">Használjon hello alábbi code tooconnect, és hello adatok segítségével olvassa a **kiválasztása** SQL-utasításban.</span><span class="sxs-lookup"><span data-stu-id="25295-158">Use hello following code tooconnect and read hello data using a **SELECT** SQL statement.</span></span> 

<span data-ttu-id="25295-159">hello kódot használja a [mysql2::client](http://www.rubydoc.info/gems/mysql2/0.4.8) MySQL .new() metódus tooconnect tooAzure adatbázis osztályt.</span><span class="sxs-lookup"><span data-stu-id="25295-159">hello code uses a [mysql2::client](http://www.rubydoc.info/gems/mysql2/0.4.8) class .new() method tooconnect tooAzure Database for MySQL.</span></span> <span data-ttu-id="25295-160">Ezután a metódus meghívja [query()](http://www.rubydoc.info/gems/mysql2/0.4.8#Usage) toorun hello VÁLASSZA parancsok.</span><span class="sxs-lookup"><span data-stu-id="25295-160">Then it calls method [query()](http://www.rubydoc.info/gems/mysql2/0.4.8#Usage) toorun hello SELECT commands.</span></span> <span data-ttu-id="25295-161">Ezután a metódus meghívja [close()](http://www.rubydoc.info/gems/mysql2/0.4.8/Mysql2/Client#close-instance_method) tooclose hello kapcsolat megszakítása előtt.</span><span class="sxs-lookup"><span data-stu-id="25295-161">Then it calls method [close()](http://www.rubydoc.info/gems/mysql2/0.4.8/Mysql2/Client#close-instance_method) tooclose hello connection before terminating.</span></span>

<span data-ttu-id="25295-162">Cserélje le a hello `host`, `database`, `username`, és `password` karakterláncok saját értékekkel.</span><span class="sxs-lookup"><span data-stu-id="25295-162">Replace hello `host`, `database`, `username`, and `password` strings with your own values.</span></span> 

```ruby
require 'mysql2'

begin
    # Initialize connection variables.
    host = String('myserver4demo.mysql.database.azure.com')
    database = String('quickstartdb')
    username = String('myadmin@myserver4demo')
    password = String('yourpassword')

    # Initialize connection object.
    client = Mysql2::Client.new(:host => host, :username => username, :database => database, :password => password)
    puts 'Successfully created connection toodatabase.'

    # Read data
    resultSet = client.query('SELECT * from inventory;')
    resultSet.each do |row|
        puts 'Data row = (%s, %s, %s)' % [row['id'], row['name'], row['quantity']]
    end
    puts 'Read ' + resultSet.count.to_s + ' row(s).'

# Error handling
rescue Exception => e
    puts e.message

# Cleanup
ensure
    client.close if client
    puts 'Done.'
end
```

## <a name="update-data"></a><span data-ttu-id="25295-163">Adatok frissítése</span><span class="sxs-lookup"><span data-stu-id="25295-163">Update data</span></span>
<span data-ttu-id="25295-164">Használjon hello következő code tooconnect, és frissítse a hello adatok segítségével egy **frissítése** SQL-utasításban.</span><span class="sxs-lookup"><span data-stu-id="25295-164">Use hello following code tooconnect and update hello data using a **UPDATE** SQL statement.</span></span>

<span data-ttu-id="25295-165">hello kódot használja a [mysql2::client](http://www.rubydoc.info/gems/mysql2/0.4.8) MySQL .new() metódus tooconnect tooAzure adatbázis osztályt.</span><span class="sxs-lookup"><span data-stu-id="25295-165">hello code uses a [mysql2::client](http://www.rubydoc.info/gems/mysql2/0.4.8) class .new() method tooconnect tooAzure Database for MySQL.</span></span> <span data-ttu-id="25295-166">Ezután a metódus meghívja [query()](http://www.rubydoc.info/gems/mysql2/0.4.8#Usage) toorun hello frissítés parancsok.</span><span class="sxs-lookup"><span data-stu-id="25295-166">Then it calls method [query()](http://www.rubydoc.info/gems/mysql2/0.4.8#Usage) toorun hello UPDATE commands.</span></span> <span data-ttu-id="25295-167">Ezután a metódus meghívja [close()](http://www.rubydoc.info/gems/mysql2/0.4.8/Mysql2/Client#close-instance_method) tooclose hello kapcsolat megszakítása előtt.</span><span class="sxs-lookup"><span data-stu-id="25295-167">Then it calls method [close()](http://www.rubydoc.info/gems/mysql2/0.4.8/Mysql2/Client#close-instance_method) tooclose hello connection before terminating.</span></span>

<span data-ttu-id="25295-168">Cserélje le a hello `host`, `database`, `username`, és `password` karakterláncok saját értékekkel.</span><span class="sxs-lookup"><span data-stu-id="25295-168">Replace hello `host`, `database`, `username`, and `password` strings with your own values.</span></span> 

```ruby
require 'mysql2'

begin
    # Initialize connection variables.
    host = String('myserver4demo.mysql.database.azure.com')
    database = String('quickstartdb')
    username = String('myadmin@myserver4demo')
    password = String('yourpassword')

    # Initialize connection object.
    client = Mysql2::Client.new(:host => host, :username => username, :database => database, :password => password)
    puts 'Successfully created connection toodatabase.'

    # Update data
   client.query('UPDATE inventory SET quantity = %d WHERE name = %s;' % [200, '\'banana\''])
   puts 'Updated 1 row of data.'

# Error handling
rescue Exception => e
    puts e.message

# Cleanup
ensure
    client.close if client
    puts 'Done.'
end
```


## <a name="delete-data"></a><span data-ttu-id="25295-169">Adat törlése</span><span class="sxs-lookup"><span data-stu-id="25295-169">Delete data</span></span>
<span data-ttu-id="25295-170">Használjon hello alábbi code tooconnect, és olvasott hello adatok egy **törlése** SQL-utasításban.</span><span class="sxs-lookup"><span data-stu-id="25295-170">Use hello following code tooconnect and read hello data using a **DELETE** SQL statement.</span></span> 

<span data-ttu-id="25295-171">hello kódot használja a [mysql2::client](http://www.rubydoc.info/gems/mysql2/0.4.8) MySQL .new() metódus tooconnect tooAzure adatbázis osztályt.</span><span class="sxs-lookup"><span data-stu-id="25295-171">hello code uses a [mysql2::client](http://www.rubydoc.info/gems/mysql2/0.4.8) class .new() method tooconnect tooAzure Database for MySQL.</span></span> <span data-ttu-id="25295-172">Ezután a metódus meghívja [query()](http://www.rubydoc.info/gems/mysql2/0.4.8#Usage) toorun hello törlési parancsok.</span><span class="sxs-lookup"><span data-stu-id="25295-172">Then it calls method [query()](http://www.rubydoc.info/gems/mysql2/0.4.8#Usage) toorun hello DELETE commands.</span></span> <span data-ttu-id="25295-173">Ezután a metódus meghívja [close()](http://www.rubydoc.info/gems/mysql2/0.4.8/Mysql2/Client#close-instance_method) tooclose hello kapcsolat megszakítása előtt.</span><span class="sxs-lookup"><span data-stu-id="25295-173">Then it calls method [close()](http://www.rubydoc.info/gems/mysql2/0.4.8/Mysql2/Client#close-instance_method) tooclose hello connection before terminating.</span></span>

<span data-ttu-id="25295-174">Cserélje le a hello `host`, `database`, `username`, és `password` karakterláncok saját értékekkel.</span><span class="sxs-lookup"><span data-stu-id="25295-174">Replace hello `host`, `database`, `username`, and `password` strings with your own values.</span></span> 

```ruby
require 'mysql2'

begin
    # Initialize connection variables.
    host = String('myserver4demo.mysql.database.azure.com')
    database = String('quickstartdb')
    username = String('myadmin@myserver4demo')
    password = String('yourpassword')

    # Initialize connection object.
    client = Mysql2::Client.new(:host => host, :username => username, :database => database, :password => password)
    puts 'Successfully created connection toodatabase.'

    # Delete data
    resultSet = client.query('DELETE FROM inventory WHERE name = %s;' % ['\'orange\''])
    puts 'Deleted 1 row.'

# Error handling
rescue Exception => e
    puts e.message

# Cleanup
ensure
    client.close if client
    puts 'Done.'
end
```

## <a name="next-steps"></a><span data-ttu-id="25295-175">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="25295-175">Next steps</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="25295-176">Adatbázis migrálása exportálással és importálással</span><span class="sxs-lookup"><span data-stu-id="25295-176">Migrate your database using Export and Import</span></span>](./concepts-migrate-import-export.md)
