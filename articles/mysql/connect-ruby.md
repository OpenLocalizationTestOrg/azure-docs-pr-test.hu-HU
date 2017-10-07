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
# <a name="azure-database-for-mysql-use-ruby-tooconnect-and-query-data"></a>MySQL az Azure-adatbázishoz: használata Ruby tooconnect és lekérdezési adatok
A gyors üzembe helyezés bemutatja, hogyan tooconnect tooan Azure adatbázis a MySQL használata egy [Ruby](https://www.ruby-lang.org) alkalmazás és hello [mysql2](https://rubygems.org/gems/mysql2) a Windows, Ubuntu Linux és Mac rendszerek gem. Azt illusztrálja, hogyan toouse SQL utasítás tooquery beszúrási, frissítési és törlési hello adatbázis adatait. Ez a cikk feltételezi, hogy jártas használatával Ruby, azonban, hogy új tooworking MySQL az Azure-adatbázissal.

## <a name="prerequisites"></a>Előfeltételek
A gyors üzembe helyezés kiindulási pontként ezek az útmutatók valamelyikével létrehozott hello erőforrást használ:
- [Azure-adatbázis létrehozása MySQL-kiszolgálóhoz az Azure Portal használatával](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [Azure-adatbázis létrehozása MySQL-kiszolgálóhoz az Azure CLI használatával](./quickstart-create-mysql-server-database-using-azure-cli.md)

## <a name="install-ruby"></a>A Ruby telepítése
Telepítse a saját gép Ruby, Gem és hello MySQL2 könyvtár. 

### <a name="windows"></a>Windows
1. A letöltés és telepítés hello 2.3 verziójának [Ruby](http://rubyinstaller.org/downloads/).
2. Nyissa meg egy új parancssort (cmd) hello Start menüből.
3. Módosítsa a könyvtárat hello verziójához 2.3 Ruby directory be. `cd c:\Ruby23-x64\bin`
4. Teszt hello hello parancs futtatásával Ruby telepítési `ruby -v` toosee hello verziója van telepítve.
5. Hello Gem telepítés sikerességének ellenőrzése hello parancs futtatásával `gem -v` toosee hello verziója van telepítve.
6. Hello Mysql2 modul építése Gem használatával hello parancs futtatásával Ruby `gem install mysql2`.

### <a name="macos"></a>MacOS
1. Ruby Homebrew használatával hello parancs futtatásával telepítse `brew install ruby`. További telepítési lehetőségekről, tekintse meg a hello Ruby [dokumentáció](https://www.ruby-lang.org/en/documentation/installation/#homebrew).
2. Teszt hello hello parancs futtatásával Ruby telepítési `ruby -v` toosee hello verziója van telepítve.
3. Hello Gem telepítés sikerességének ellenőrzése hello parancs futtatásával `gem -v` toosee hello verziója van telepítve.
4. Hello Mysql2 modul építése Gem használatával hello parancs futtatásával Ruby `gem install mysql2`.

### <a name="linux-ubuntu"></a>Linux (Ubuntu)
1. Telepítse a Ruby hello parancs futtatásával `sudo apt-get install ruby-full`. További telepítési lehetőségekről, tekintse meg a hello Ruby [dokumentáció](https://www.ruby-lang.org/en/documentation/installation/).
2. Teszt hello hello parancs futtatásával Ruby telepítési `ruby -v` toosee hello verziója van telepítve.
3. Hello legújabb frissítéseinek telepítése Gem hello parancs futtatásával `sudo gem update --system`.
4. Hello Gem telepítés sikerességének ellenőrzése hello parancs futtatásával `gem -v` toosee hello verziója van telepítve.
5. Telepítse a hello ÖET, ellenőrizze és egyéb build tools hello parancs futtatásával `sudo apt-get install build-essential`.
6. Telepítse a hello MySQL klienskódtárak fejlesztői hello parancs futtatásával `sudo apt-get install libmysqlclient-dev`.
7. Hello mysql2 modul építése Gem használatával hello parancs futtatásával Ruby `sudo gem install mysql2`.

## <a name="get-connection-information"></a>Kapcsolatadatok lekérése
MySQL hello kapcsolat szükséges információkat tooconnect toohello Azure adatbázis beolvasása. Teljesen minősített kiszolgáló nevét és a bejelentkezési hitelesítő adatokat hello van szüksége.

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).
2. A hello Azure-portálon a bal oldali menüből, kattintson az **összes erőforrás** , és keresse meg rendelkezik gyűrött, például a hello kiszolgáló **myserver4demo**.
3. Hello kiszolgáló nevére kattint **myserver4demo**.
4. Jelölje be hello server **tulajdonságok** lap. Jegyezze fel a hello **kiszolgálónév** és **kiszolgálói rendszergazda bejelentkezési név**.
 ![MySQL-hez készült Azure-adatbázis – Kiszolgáló-rendszergazdai bejelentkezés](./media/connect-ruby/1_server-properties-name-login.png)
5. Ha elfelejti a kiszolgálói bejelentkezési adatok, keresse meg a toohello **áttekintése** tooview hello kiszolgálói rendszergazda bejelentkezési név lapon, és ha szükséges, állítsa vissza a hello jelszót.

## <a name="run-ruby-code"></a>Ruby-kód futtatása 
1. Hello hello szakaszokat Ruby kód beillesztése szövegfájlok, fájljait és mentse el hello egy projekt mappába, a fájl kiterjesztése .rb, például a `C:\rubymysql\createtable.rb` vagy `/home/username/rubymysql/createtable.rb`.
2. toorun hello kódot, indítsa el a hello parancssor, vagy a bash rendszerhéjat. Lépjen a projektmappára: `cd rubymysql`
3. Írja be a hello ruby parancsot, majd hello fájl nevét, például a `ruby createtable.rb` toorun hello alkalmazás.
4. A Windows operációs rendszer hello Ha hello ruby alkalmazás nem szerepel a path környezeti változóba, szükség lehet toouse hello teljes elérési útja toolaunch hello csomópont alkalmazás, például a`"c:\Ruby23-x64\bin\ruby.exe" createtable.rb`

## <a name="connect-and-create-a-table"></a>Csatlakozás és tábla létrehozása
Használjon hello alábbi code tooconnect, és hozzon létre egy táblát az **CREATE TABLE** SQL-utasítást, és **INSERT INTO** SQL utasítás tooadd sorok hello táblába.

hello kódot használja a [mysql2::client](http://www.rubydoc.info/gems/mysql2/0.4.8) MySQL .new() metódus tooconnect tooAzure adatbázis osztályt. Ezután a metódus meghívja [query()](http://www.rubydoc.info/gems/mysql2/0.4.8#Usage) többször toorun hello dobja el, CREATE TABLE és INSERT INTO parancsok. Ezután a metódus meghívja [close()](http://www.rubydoc.info/gems/mysql2/0.4.8/Mysql2/Client#close-instance_method) tooclose hello kapcsolat megszakítása előtt.

Cserélje le a hello `host`, `database`, `username`, és `password` karakterláncok saját értékekkel. 
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

## <a name="read-data"></a>Adatok olvasása
Használjon hello alábbi code tooconnect, és hello adatok segítségével olvassa a **kiválasztása** SQL-utasításban. 

hello kódot használja a [mysql2::client](http://www.rubydoc.info/gems/mysql2/0.4.8) MySQL .new() metódus tooconnect tooAzure adatbázis osztályt. Ezután a metódus meghívja [query()](http://www.rubydoc.info/gems/mysql2/0.4.8#Usage) toorun hello VÁLASSZA parancsok. Ezután a metódus meghívja [close()](http://www.rubydoc.info/gems/mysql2/0.4.8/Mysql2/Client#close-instance_method) tooclose hello kapcsolat megszakítása előtt.

Cserélje le a hello `host`, `database`, `username`, és `password` karakterláncok saját értékekkel. 

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

## <a name="update-data"></a>Adatok frissítése
Használjon hello következő code tooconnect, és frissítse a hello adatok segítségével egy **frissítése** SQL-utasításban.

hello kódot használja a [mysql2::client](http://www.rubydoc.info/gems/mysql2/0.4.8) MySQL .new() metódus tooconnect tooAzure adatbázis osztályt. Ezután a metódus meghívja [query()](http://www.rubydoc.info/gems/mysql2/0.4.8#Usage) toorun hello frissítés parancsok. Ezután a metódus meghívja [close()](http://www.rubydoc.info/gems/mysql2/0.4.8/Mysql2/Client#close-instance_method) tooclose hello kapcsolat megszakítása előtt.

Cserélje le a hello `host`, `database`, `username`, és `password` karakterláncok saját értékekkel. 

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


## <a name="delete-data"></a>Adat törlése
Használjon hello alábbi code tooconnect, és olvasott hello adatok egy **törlése** SQL-utasításban. 

hello kódot használja a [mysql2::client](http://www.rubydoc.info/gems/mysql2/0.4.8) MySQL .new() metódus tooconnect tooAzure adatbázis osztályt. Ezután a metódus meghívja [query()](http://www.rubydoc.info/gems/mysql2/0.4.8#Usage) toorun hello törlési parancsok. Ezután a metódus meghívja [close()](http://www.rubydoc.info/gems/mysql2/0.4.8/Mysql2/Client#close-instance_method) tooclose hello kapcsolat megszakítása előtt.

Cserélje le a hello `host`, `database`, `username`, és `password` karakterláncok saját értékekkel. 

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

## <a name="next-steps"></a>Következő lépések
> [!div class="nextstepaction"]
> [Adatbázis migrálása exportálással és importálással](./concepts-migrate-import-export.md)
