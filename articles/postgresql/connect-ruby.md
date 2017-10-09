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
# <a name="azure-database-for-postgresql-use-ruby-tooconnect-and-query-data"></a>Azure PostgreSQL-adatbázishoz: használata Ruby tooconnect és lekérdezési adatok
A gyors üzembe helyezés bemutatja, hogyan tooconnect tooan Azure PostgreSQL az adatbázis egy [Ruby](https://www.ruby-lang.org) alkalmazás. Azt illusztrálja, hogyan toouse SQL utasítás tooquery beszúrási, frissítési és törlési hello adatbázis adatait. Ez a cikk feltételezi, hogy jártas használatával Ruby, azonban, hogy új tooworking PostgreSQL az Azure-adatbázissal.

## <a name="prerequisites"></a>Előfeltételek
A gyors üzembe helyezés kiindulási pontként ezek az útmutatók valamelyikével létrehozott hello erőforrást használ:
- [DB létrehozása – portál](quickstart-create-server-database-portal.md)
- [DB létrehozása – Azure CLI](quickstart-create-server-database-azure-cli.md)

## <a name="install-ruby"></a>A Ruby telepítése
Telepítse a Rubyt saját számítógépén. 

### <a name="windows"></a>Windows
- A letöltés és telepítés hello legújabb verziójának [Ruby](http://rubyinstaller.org/downloads/).
- Hello a Befejezés gombra a képernyő hello MSI-telepítő, jelölőnégyzetet hello, amely szerint a "Futtatás"ridk telepítése"tooinstall MSYS2 és fejlesztési toolchain." Kattintson a **Befejezés** toolaunch hello következő telepítő.
- hello RubyInstaller2 a Windows telepítő elindítja. Adjon meg 2 tooinstall hello MSYS2 lemezképtár frissítése. Miután befejeződik, és toohello telepítési kérdés adja vissza, hello parancs ablak bezárásához.
- Nyissa meg egy új parancssort (cmd) hello Start menüből.
- Teszt hello Ruby telepítési `ruby -v` toosee hello verziója van telepítve.
- Hello Gem telepítés sikerességének ellenőrzése `gem -v` toosee hello verziója van telepítve.
- Hello PostgreSQL modul építése Gem használatával hello parancs futtatásával Ruby `gem install pg`.

### <a name="macos"></a>MacOS
- Ruby Homebrew használatával hello parancs futtatásával telepítse `brew install ruby`. További telepítési lehetőségekről, tekintse meg a hello Ruby [dokumentáció](https://www.ruby-lang.org/en/documentation/installation/#homebrew)
- Teszt hello Ruby telepítési `ruby -v` toosee hello verziója van telepítve.
- Hello Gem telepítés sikerességének ellenőrzése `gem -v` toosee hello verziója van telepítve.
- Hello PostgreSQL modul építése Gem használatával hello parancs futtatásával Ruby `gem install pg`.

### <a name="linux-ubuntu"></a>Linux (Ubuntu)
- Telepítse a Ruby hello parancs futtatásával `sudo apt-get install ruby-full`. További telepítési lehetőségekről, tekintse meg a hello Ruby [dokumentáció](https://www.ruby-lang.org/en/documentation/installation/).
- Teszt hello Ruby telepítési `ruby -v` toosee hello verziója van telepítve.
- Hello legújabb frissítéseinek telepítése Gem hello parancs futtatásával `sudo gem update --system`.
- Hello Gem telepítés sikerességének ellenőrzése `gem -v` toosee hello verziója van telepítve.
- Telepítse a hello ÖET, ellenőrizze és egyéb build tools hello parancs futtatásával `sudo apt-get install build-essential`.
- Telepítse a hello PostgreSQL szalagtárak hello parancs futtatásával `sudo apt-get install libpq-dev`.
- Build hello Ruby pg modul használatával Gem hello parancs futtatásával `sudo gem install pg`.

## <a name="run-ruby-code"></a>Ruby-kód futtatása 
- Hello kód egy szövegfájlba, és hello fájlt például a fájl kiterjesztése .rb, a projekt mappába Mentés `C:\rubypostgres\read.rb` vagy`/home/username/rubypostgres/read.rb`
- toorun hello kódot, indítsa el a hello parancssor, vagy a bash rendszerhéjat. A projekt mappába könyvtárváltás `cd rubypostgres`, majd írja be a hello parancsot `ruby read.rb` toorun hello alkalmazás.

## <a name="get-connection-information"></a>Kapcsolatadatok lekérése
Hello kapcsolat szükséges információkat tooconnect toohello Azure adatbázis beolvasása PostgreSQL. Teljesen minősített kiszolgáló nevét és a bejelentkezési hitelesítő adatokat hello van szüksége.

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).
2. A hello Azure-portálon a bal oldali menüből, kattintson az **összes erőforrás** , és keressen a létrehozott, például a hello server **mypgserver-20170401**.
3. Hello kiszolgáló nevére kattint **mypgserver-20170401**.
4. Jelölje be hello server **áttekintése** lap. Jegyezze fel a hello **kiszolgálónév** és **kiszolgálói rendszergazda bejelentkezési név**.
 ![PostgreSQL-hez készült Azure-adatbázis – Kiszolgáló-rendszergazdai bejelentkezés](./media/connect-ruby/1-connection-string.png)
5. Ha elfelejti a kiszolgálói bejelentkezési adatok, keresse meg a toohello **áttekintése** lap tooview hello rendszergazda bejelentkezési nevet. Ha szükséges, jelszó-átállítási hello.

## <a name="connect-and-create-a-table"></a>Csatlakozás és tábla létrehozása
Használjon hello alábbi code tooconnect, és hozzon létre egy táblát az **CREATE TABLE** SQL-utasítást, és **INSERT INTO** SQL utasítás tooadd sorok hello táblába.

hello kódot használja a [PG::Connection](http://www.rubydoc.info/gems/pg/PG/Connection) konstruktorral objektum [hívás](http://www.rubydoc.info/gems/pg/PG%2FConnection:initialize) tooconnect tooAzure PostgreSQL-adatbázisban. Ezután a metódus meghívja [exec()](http://www.rubydoc.info/gems/pg/PG/Connection#exec-instance_method) toorun hello dobja el, CREATE TABLE és INSERT INTO parancsok. hello kód ellenőrzi a hello segítségével hibákat [PG::Error](http://www.rubydoc.info/gems/pg/PG/Error) osztály. Ezután a metódus meghívja [close()](http://www.rubydoc.info/gems/pg/PG/Connection#lo_close-instance_method) tooclose hello kapcsolat megszakítása előtt.

Cserélje le a hello `host`, `database`, `user`, és `password` karakterláncok saját értékekkel. 
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

## <a name="read-data"></a>Adatok olvasása
Használjon hello alábbi code tooconnect, és hello adatok segítségével olvassa a **kiválasztása** SQL-utasításban. 

hello kódot használja a [PG::Connection](http://www.rubydoc.info/gems/pg/PG/Connection) konstruktorral objektum [hívás](http://www.rubydoc.info/gems/pg/PG%2FConnection:initialize) tooconnect tooAzure PostgreSQL-adatbázisban. Ezután a metódus meghívja [exec()](http://www.rubydoc.info/gems/pg/PG/Connection#exec-instance_method) toorun hello kijelölési parancs, hello eredmények tartása egy eredményhalmazban szerepel. hello eredmény készlet gyűjteményéhez van többször is hello használatához képest `resultSet.each do` hurok hello aktuális sorérték megőrzi található hello `row` változó. hello kód ellenőrzi a hello segítségével hibákat [PG::Error](http://www.rubydoc.info/gems/pg/PG/Error) osztály. Ezután a metódus meghívja [close()](http://www.rubydoc.info/gems/pg/PG/Connection#lo_close-instance_method) tooclose hello kapcsolat megszakítása előtt.

Cserélje le a hello `host`, `database`, `user`, és `password` karakterláncok saját értékekkel. 

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

## <a name="update-data"></a>Adatok frissítése
Használjon hello következő code tooconnect, és frissítse a hello adatok segítségével egy **frissítése** SQL-utasításban.

hello kódot használja a [PG::Connection](http://www.rubydoc.info/gems/pg/PG/Connection) konstruktorral objektum [hívás](http://www.rubydoc.info/gems/pg/PG%2FConnection:initialize) tooconnect tooAzure PostgreSQL-adatbázisban. Ezután a metódus meghívja [exec()](http://www.rubydoc.info/gems/pg/PG/Connection#exec-instance_method) toorun hello frissítés parancsot. hello kód ellenőrzi a hello segítségével hibákat [PG::Error](http://www.rubydoc.info/gems/pg/PG/Error) osztály. Ezután a metódus meghívja [close()](http://www.rubydoc.info/gems/pg/PG/Connection#lo_close-instance_method) tooclose hello kapcsolat megszakítása előtt.

Cserélje le a hello `host`, `database`, `user`, és `password` karakterláncok saját értékekkel. 

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


## <a name="delete-data"></a>Adat törlése
Használjon hello alábbi code tooconnect, és olvasott hello adatok egy **törlése** SQL-utasításban. 

hello kódot használja a [PG::Connection](http://www.rubydoc.info/gems/pg/PG/Connection) konstruktorral objektum [hívás](http://www.rubydoc.info/gems/pg/PG%2FConnection:initialize) tooconnect tooAzure PostgreSQL-adatbázisban. Ezután a metódus meghívja [exec()](http://www.rubydoc.info/gems/pg/PG/Connection#exec-instance_method) toorun hello frissítés parancsot. hello kód ellenőrzi a hello segítségével hibákat [PG::Error](http://www.rubydoc.info/gems/pg/PG/Error) osztály. Ezután a metódus meghívja [close()](http://www.rubydoc.info/gems/pg/PG/Connection#lo_close-instance_method) tooclose hello kapcsolat megszakítása előtt.

Cserélje le a hello `host`, `database`, `user`, és `password` karakterláncok saját értékekkel. 

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

## <a name="next-steps"></a>Következő lépések
> [!div class="nextstepaction"]
> [Adatbázis migrálása exportálással és importálással](./howto-migrate-using-export-and-import.md)
