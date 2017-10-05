---
title: "Csatlakozás a MySQL-hez készült Azure Database-hez a C++ segítségével | Microsoft Docs"
description: "Ez a rövid útmutató egy C++-mintakódot biztosít, amellyel csatlakozhat a MySQL-hez készült Azure Database-hez, illetve adatokat kérdezhet le róla."
services: mysql
author: seanli1988
ms.author: seal
manager: janders
editor: jasonwhowell
ms.service: mysql-database
ms.custom: mvc
ms.devlang: C++
ms.topic: hero-article
ms.date: 08/03/2017
ms.openlocfilehash: 63388b83b913d95136140fa4c56af0dbebbdad81
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="azure-database-for-mysql-use-connectorc-to-connect-and-query-data"></a><span data-ttu-id="c1b98-103">A MySQL-hez készült Azure Database: Csatlakozás és adatlekérdezés összekötő/C++ használatával</span><span class="sxs-lookup"><span data-stu-id="c1b98-103">Azure Database for MySQL: Use Connector/C++ to connect and query data</span></span>
<span data-ttu-id="c1b98-104">Ebben a rövid útmutatóban azt szemléltetjük, hogy miként lehet C++-alkalmazás használatával csatlakozni a MySQL-hez készült Azure Database-hez.</span><span class="sxs-lookup"><span data-stu-id="c1b98-104">This quickstart demonstrates how to connect to an Azure Database for MySQL using a C++ application.</span></span> <span data-ttu-id="c1b98-105">Azt is bemutatja, hogyan lehet SQL-utasítások használatával adatokat lekérdezni, beszúrni, frissíteni és törölni az adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="c1b98-105">It shows how to use SQL statements to query, insert, update, and delete data in the database.</span></span> <span data-ttu-id="c1b98-106">A cikkben ismertetett lépések feltételezik, hogy Ön rendelkezik fejlesztési tapasztalatokkal a C++ használatával kapcsolatban, a MySQL-hez készült Azure Database használatában pedig még járatlan.</span><span class="sxs-lookup"><span data-stu-id="c1b98-106">The steps in this article assume that you are familiar with developing using C++, and that you are new to working with Azure Database for MySQL.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c1b98-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="c1b98-107">Prerequisites</span></span>
<span data-ttu-id="c1b98-108">Ebben a rövid útmutatóban a következő útmutatók valamelyikében létrehozott erőforrásokat használunk kiindulási pontként:</span><span class="sxs-lookup"><span data-stu-id="c1b98-108">This quickstart uses the resources created in either of these guides as a starting point:</span></span>
- [<span data-ttu-id="c1b98-109">Azure-adatbázis létrehozása MySQL-kiszolgálóhoz az Azure Portal használatával</span><span class="sxs-lookup"><span data-stu-id="c1b98-109">Create an Azure Database for MySQL server using Azure portal</span></span>](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [<span data-ttu-id="c1b98-110">Azure-adatbázis létrehozása MySQL-kiszolgálóhoz az Azure CLI használatával</span><span class="sxs-lookup"><span data-stu-id="c1b98-110">Create an Azure Database for MySQL server using Azure CLI</span></span>](./quickstart-create-mysql-server-database-using-azure-cli.md)

<span data-ttu-id="c1b98-111">Emellett a következőket kell elvégezni:</span><span class="sxs-lookup"><span data-stu-id="c1b98-111">You also need to:</span></span>
- <span data-ttu-id="c1b98-112">A [.NET-keretrendszer](https://www.microsoft.com/net/download) telepítése</span><span class="sxs-lookup"><span data-stu-id="c1b98-112">Install [.NET Framework](https://www.microsoft.com/net/download)</span></span>
- <span data-ttu-id="c1b98-113">A [Visual Studio](https://www.visualstudio.com/downloads/) telepítése</span><span class="sxs-lookup"><span data-stu-id="c1b98-113">Install [Visual Studio](https://www.visualstudio.com/downloads/)</span></span>
- <span data-ttu-id="c1b98-114">A [MySQL-összekötő/C++](https://dev.mysql.com/downloads/connector/cpp/) telepítése</span><span class="sxs-lookup"><span data-stu-id="c1b98-114">Install [MySQL Connector/C++](https://dev.mysql.com/downloads/connector/cpp/)</span></span> 
- <span data-ttu-id="c1b98-115">A [Boost](http://www.boost.org/) telepítése</span><span class="sxs-lookup"><span data-stu-id="c1b98-115">Install [Boost](http://www.boost.org/)</span></span>

## <a name="install-visual-studio-and-net"></a><span data-ttu-id="c1b98-116">A Visual Studio és a .NET telepítése</span><span class="sxs-lookup"><span data-stu-id="c1b98-116">Install Visual Studio and .NET</span></span>
<span data-ttu-id="c1b98-117">A jelen szakaszban ismertetett lépések feltételezik, hogy Ön rendelkezik .NET-fejlesztési tapasztalatokkal.</span><span class="sxs-lookup"><span data-stu-id="c1b98-117">The steps in this section assume that you are familiar with developing using .NET.</span></span>

### <a name="windows"></a><span data-ttu-id="c1b98-118">**Windows**</span><span class="sxs-lookup"><span data-stu-id="c1b98-118">**Windows**</span></span>
1. <span data-ttu-id="c1b98-119">Telepítse a Visual Studio 2017 Communityt, amely egy minden funkcióval ellátott, bővíthető, ingyenes IDE, és amellyel korszerű alkalmazásokat hozhat létre Android, iOS és Windows operációs rendszerre, valamint webes illetve adatbázis-alkalmazásokhoz és felhőszolgáltatásokhoz.</span><span class="sxs-lookup"><span data-stu-id="c1b98-119">Install Visual Studio 2017 Community, which is a full featured, extensible, free IDE for creating modern applications for Android, iOS, Windows, as well as web & database applications and cloud services.</span></span> <span data-ttu-id="c1b98-120">Telepítheti a teljes .NET-keretrendszert vagy csak a .NET Core-t.</span><span class="sxs-lookup"><span data-stu-id="c1b98-120">You can install either the full .NET Framework or just .NET Core.</span></span> <span data-ttu-id="c1b98-121">A rövid útmutatóban foglalt kódrészletek mindkettővel működnek.</span><span class="sxs-lookup"><span data-stu-id="c1b98-121">The code snippets in the Quickstart work with either.</span></span> <span data-ttu-id="c1b98-122">Ha a Visual Studio már telepítve van a számítógépén, a következő két lépés kihagyható.</span><span class="sxs-lookup"><span data-stu-id="c1b98-122">If you already have Visual Studio installed on your machine, skip the next two steps.</span></span>
   - <span data-ttu-id="c1b98-123">Töltse le a [Visual Studio 2017 telepítőt](https://www.visualstudio.com/thank-you-downloading-visual-studio/?sku=Community&rel=15).</span><span class="sxs-lookup"><span data-stu-id="c1b98-123">Download the [Visual Studio 2017 installer](https://www.visualstudio.com/thank-you-downloading-visual-studio/?sku=Community&rel=15).</span></span> 
   - <span data-ttu-id="c1b98-124">Futtassa a telepítőt, és kövesse a telepítési utasításokat a telepítés befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="c1b98-124">Run the installer and follow the installation prompts to complete the installation.</span></span>

### <a name="configure-visual-studio"></a><span data-ttu-id="c1b98-125">**A Visual Studio konfigurálása**</span><span class="sxs-lookup"><span data-stu-id="c1b98-125">**Configure Visual Studio**</span></span>
1. <span data-ttu-id="c1b98-126">A Visual Studióban a project property (projekttulajdonság) > configuration properties (konfigurációs tulajdonságok) > C/C++ > linker (kapcsolószerkesztő) > general (általános) > additional library directories (további kódtárkönyvtárak) menüpontban adja hozzá a c++-összekötő lib\opt könyvtárát (pl.: C:\Program Files (x86)\MySQL\MySQL Connector C++ 1.1.9\lib\opt).</span><span class="sxs-lookup"><span data-stu-id="c1b98-126">From Visual Studio, project property > configuration properties > C/C++ > linker > general > additional library directories, add the lib\opt directory (i.e.: C:\Program Files (x86)\MySQL\MySQL Connector C++ 1.1.9\lib\opt) of the c++ connector.</span></span>
2. <span data-ttu-id="c1b98-127">A Visual Studióban a project property (projekttulajdonság) > configuration properties (konfigurációs tulajdonságok) > C/C++ > general (általános) > additional include directories (további belefoglalt könyvtárak) menüpontban</span><span class="sxs-lookup"><span data-stu-id="c1b98-127">From Visual Studio, project property > configuration properties > C/C++ > general > additional include directories</span></span>
   - <span data-ttu-id="c1b98-128">Adja hozzá a c++-összekötő \include könyvtárát (pl.: C:\Program Files (x86)\MySQL\MySQL Connector C++ 1.1.9\include\)</span><span class="sxs-lookup"><span data-stu-id="c1b98-128">Add include/ directory of c++ connector (i.e.: C:\Program Files (x86)\MySQL\MySQL Connector C++ 1.1.9\include\)</span></span>
   - <span data-ttu-id="c1b98-129">Adja hozzá a Boost kódtár gyökérkönyvtárát (pl.: C:\boost_1_64_0\)</span><span class="sxs-lookup"><span data-stu-id="c1b98-129">Add Boost library's root directory (i.e.: C:\boost_1_64_0\)</span></span>
3. <span data-ttu-id="c1b98-130">A Visual Studióban a project property (projekttulajdonság) > configuration properties (konfigurációs tulajdonságok) > C/C++ > linker (kapcsolószerkesztő) > Input (Bemenet) > Additional Dependencies (További függőségek) menüpontban adja hozzá a mysqlcppconn.lib elemet a szövegmezőhöz</span><span class="sxs-lookup"><span data-stu-id="c1b98-130">From Visual Studio, project property > configuration properties > C/C++ > linker > Input > Additional Dependencies, add mysqlcppconn.lib into the text field</span></span>
4. <span data-ttu-id="c1b98-131">Másolja át a mysqlcppconn.dll fájlt a 3. lépésben szereplő c++-összekötő könyvtárából az alkalmazás futtatható fájlját tartalmazó könyvtárba, vagy adja hozzá környezeti változóként, hogy az alkalmazás megtalálhassa azt.</span><span class="sxs-lookup"><span data-stu-id="c1b98-131">Either copy mysqlcppconn.dll from the c++ connector library folder in step 3 to the same directory as the application executable or add it to the environment variable so your application can find it.</span></span>

## <a name="get-connection-information"></a><span data-ttu-id="c1b98-132">Kapcsolatadatok lekérése</span><span class="sxs-lookup"><span data-stu-id="c1b98-132">Get connection information</span></span>
<span data-ttu-id="c1b98-133">Kérje le a MySQL-hez készült Azure Database-hez való csatlakozáshoz szükséges kapcsolatadatokat.</span><span class="sxs-lookup"><span data-stu-id="c1b98-133">Get the connection information needed to connect to the Azure Database for MySQL.</span></span> <span data-ttu-id="c1b98-134">Ehhez szükség lesz a teljes kiszolgálónévre és bejelentkezési hitelesítő adatokra.</span><span class="sxs-lookup"><span data-stu-id="c1b98-134">You need the fully qualified server name and login credentials.</span></span>

1. <span data-ttu-id="c1b98-135">Jelentkezzen be az [Azure portálra](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="c1b98-135">Log in to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="c1b98-136">Az Azure Portal bal oldali menüjében kattintson az **Összes erőforrás** lehetőségre, és keressen rá a létrehozott kiszolgálóra, például **myserver4demo**.</span><span class="sxs-lookup"><span data-stu-id="c1b98-136">From the left-hand menu in Azure portal, click **All resources** and search for the server you have created, such as **myserver4demo**.</span></span>
3. <span data-ttu-id="c1b98-137">Kattintson a kiszolgálónévre.</span><span class="sxs-lookup"><span data-stu-id="c1b98-137">Click the server name.</span></span>
4. <span data-ttu-id="c1b98-138">Válassza a kiszolgáló **tulajdonságlapját**.</span><span class="sxs-lookup"><span data-stu-id="c1b98-138">Select the server's **Properties** page.</span></span> <span data-ttu-id="c1b98-139">Jegyezze fel a **Kiszolgálónevet** és a **Kiszolgáló-rendszergazdai bejelentkezési nevet**.</span><span class="sxs-lookup"><span data-stu-id="c1b98-139">Make a note of the **Server name** and **Server admin login name**.</span></span>
 <span data-ttu-id="c1b98-140">![A MySQL-hez készült Azure Database-kiszolgáló neve](./media/connect-cpp/1_server-properties-name-login.png)</span><span class="sxs-lookup"><span data-stu-id="c1b98-140">![Azure Database for MySQL server name](./media/connect-cpp/1_server-properties-name-login.png)</span></span>
5. <span data-ttu-id="c1b98-141">Amennyiben elfelejtette a kiszolgáló bejelentkezési adatait, lépjen az **Overview** (Áttekintés) oldalra, és itt megtudhatja a kiszolgáló rendszergazdájának bejelentkezési nevét, valamint szükség esetén visszaállíthatja a jelszót.</span><span class="sxs-lookup"><span data-stu-id="c1b98-141">If you forget your server login information, navigate to the **Overview** page to view the Server admin login name and, if necessary, reset the password.</span></span>

## <a name="connect-create-table-and-insert-data"></a><span data-ttu-id="c1b98-142">Csatlakozás, táblák létrehozása és adatok beszúrása</span><span class="sxs-lookup"><span data-stu-id="c1b98-142">Connect, create table, and insert data</span></span>
<span data-ttu-id="c1b98-143">Az alábbi kód használatával csatlakozhat és töltheti be az adatokat a **CREATE TABLE** és az **INSERT INTO** SQL-utasítások segítségével.</span><span class="sxs-lookup"><span data-stu-id="c1b98-143">Use the following code to connect and load the data using **CREATE TABLE** and  **INSERT INTO** SQL statements.</span></span> <span data-ttu-id="c1b98-144">A kód az sql::Driver osztályt használja a connect() metódussal a MySQL-lel létesített kapcsolat létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="c1b98-144">The code uses sql::Driver class with the connect() method to establish a connection to MySQL.</span></span> <span data-ttu-id="c1b98-145">A kód ezután a createStatement() és az execute() metódust használja az adatbázis-parancsok futtatásához.</span><span class="sxs-lookup"><span data-stu-id="c1b98-145">Then the code uses method createStatement() and execute() to run the database commands.</span></span> 

<span data-ttu-id="c1b98-146">Cserélje le a gazdagép, az adatbázisnév, a felhasználó és a jelszó paramétereit azokra az értékekre, amelyeket a kiszolgáló és az adatbázis létrehozásakor adott meg.</span><span class="sxs-lookup"><span data-stu-id="c1b98-146">Replace the Host, DBName, User, and Password parameters with the values that you specified when you created the server and database.</span></span> 

```c++
#include <stdlib.h>
#include <iostream>
#include "stdafx.h"

#include "mysql_connection.h"
#include <cppconn/driver.h>
#include <cppconn/exception.h>
#include <cppconn/prepared_statement.h>
using namespace std;

int main()
{
    sql::Driver *driver;
    sql::Connection *con;
    sql::Statement *stmt;
    sql::PreparedStatement *pstmt;

    try
    {
        driver = get_driver_instance();
        //for demonstration only. never save password in the code!
        con = driver>connect("tcp://myserver4demo.mysql.database.azure.com:3306/quickstartdb", "myadmin@myserver4demo", "server_admin_password");
    }
    catch (sql::SQLException e)
    {
        cout << "Could not connect to database. Error message: " << e.what() << endl;
        system("pause");
        exit(1);
    }

    stmt = con>createStatement();
    stmt>execute("DROP TABLE IF EXISTS inventory");
    cout << "Finished dropping table (if existed)" << endl;
    stmt>execute("CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);");
    cout << "Finished creating table" << endl;
    delete stmt;

    pstmt = con>prepareStatement("INSERT INTO inventory(name, quantity) VALUES(?,?)");
    pstmt>setString(1, "banana");
    pstmt>setInt(2, 150);
    pstmt>execute();
    cout << "One row inserted." << endl;

    pstmt>setString(1, "orange");
    pstmt>setInt(2, 154);
    pstmt>execute();
    cout << "One row inserted." << endl;

    pstmt>setString(1, "apple");
    pstmt>setInt(2, 100);
    pstmt>execute();
    cout << "One row inserted." << endl;
    
    delete pstmt;   
    delete con;
    system("pause");
    return 0;

```

## <a name="read-data"></a><span data-ttu-id="c1b98-147">Adatok olvasása</span><span class="sxs-lookup"><span data-stu-id="c1b98-147">Read data</span></span>

<span data-ttu-id="c1b98-148">A következő kóddal csatlakozhat, és beolvashatja az adatokat a **SELECT** SQL-utasítással.</span><span class="sxs-lookup"><span data-stu-id="c1b98-148">Use the following code to connect and read the data using a **SELECT** SQL statement.</span></span> <span data-ttu-id="c1b98-149">A kód az sql::Driver osztályt használja a connect() metódussal a MySQL-lel létesített kapcsolat létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="c1b98-149">The code uses sql::Driver class with the connect() method to establish a connection to MySQL.</span></span> <span data-ttu-id="c1b98-150">A kód ezután a prepareStatement() és az executeQuery() metódust használja a select-parancsok futtatásához.</span><span class="sxs-lookup"><span data-stu-id="c1b98-150">Then the code uses method prepareStatement() and executeQuery() to run the select commands.</span></span> <span data-ttu-id="c1b98-151">Végül a kód a next() metódust használja az eredményekben lévő rekordokra lépéshez.</span><span class="sxs-lookup"><span data-stu-id="c1b98-151">Finally the code uses next() to advance to the records in the results.</span></span> <span data-ttu-id="c1b98-152">Ezután a kód a getInt() és a getString() metódussal elemzi a rekord értékeit.</span><span class="sxs-lookup"><span data-stu-id="c1b98-152">Then the code uses getInt() and getString() to parse the values in the record.</span></span>

<span data-ttu-id="c1b98-153">Cserélje le a gazdagép, az adatbázisnév, a felhasználó és a jelszó paramétereit azokra az értékekre, amelyeket a kiszolgáló és az adatbázis létrehozásakor adott meg.</span><span class="sxs-lookup"><span data-stu-id="c1b98-153">Replace the Host, DBName, User, and Password parameters with the values that you specified when you created the server and database.</span></span> 

```csharp
#include <stdlib.h>
#include <iostream>
#include "stdafx.h"

#include "mysql_connection.h"
#include <cppconn/driver.h>
#include <cppconn/exception.h>
#include <cppconn/resultset.h>
#include <cppconn/prepared_statement.h>
using namespace std;

int main()
{
    sql::Driver *driver;
    sql::Connection *con;
    sql::PreparedStatement *pstmt;
    sql::ResultSet *result;

    try
    {
        driver = get_driver_instance();
        //for demonstration only. never save password in the code!
        con = driver>connect("tcp://myserver4demo.mysql.database.azure.com:3306/quickstartdb", "myadmin@myserver4demo", "server_admin_password");
    }
    catch (sql::SQLException e)
    {
        cout << "Could not connect to database. Error message: " << e.what() << endl;
        system("pause");
        exit(1);
    }   

//  select  
    pstmt = con>prepareStatement("SELECT * FROM inventory;");
    result = pstmt>executeQuery();  
    
    while (result>next())
        printf("Reading from table=(%d, %s, %d)\n", result>getInt(1), result>getString(2).c_str(), result>getInt(3));   
    
    delete result;
    delete pstmt;   
    delete con;
    system("pause");
    return 0;
}
```

## <a name="update-data"></a><span data-ttu-id="c1b98-154">Adatok frissítése</span><span class="sxs-lookup"><span data-stu-id="c1b98-154">Update data</span></span>
<span data-ttu-id="c1b98-155">Az alábbi kód használatával csatlakozhat és végezheti el az adatok olvasását **UPDATE** SQL-utasítás segítségével.</span><span class="sxs-lookup"><span data-stu-id="c1b98-155">Use the following code to connect and read the data using a **UPDATE** SQL statement.</span></span> <span data-ttu-id="c1b98-156">A kód az sql::Driver osztályt használja a connect() metódussal a MySQL-lel létesített kapcsolat létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="c1b98-156">The code uses sql::Driver class with the connect() method to establish a connection to MySQL.</span></span> <span data-ttu-id="c1b98-157">A kód ezután a prepareStatement() és az executeQuery() metódust használja az update-parancsok futtatásához.</span><span class="sxs-lookup"><span data-stu-id="c1b98-157">Then the code uses method prepareStatement() and executeQuery() to run the update commands.</span></span> 

<span data-ttu-id="c1b98-158">Cserélje le a gazdagép, az adatbázisnév, a felhasználó és a jelszó paramétereit azokra az értékekre, amelyeket a kiszolgáló és az adatbázis létrehozásakor adott meg.</span><span class="sxs-lookup"><span data-stu-id="c1b98-158">Replace the Host, DBName, User, and Password parameters with the values that you specified when you created the server and database.</span></span> 

```csharp
#include <stdlib.h>
#include <iostream>
#include "stdafx.h"

#include "mysql_connection.h"
#include <cppconn/driver.h>
#include <cppconn/exception.h>
#include <cppconn/prepared_statement.h>
using namespace std;

int main()
{
    sql::Driver *driver;
    sql::Connection *con;
    sql::PreparedStatement *pstmt;

    try
    {
        driver = get_driver_instance();
        //for demonstration only. never save password in the code!
        con = driver>connect("tcp://myserver4demo.mysql.database.azure.com:3306/quickstartdb", "myadmin@myserver4demo", "server_admin_password");
    }
    catch (sql::SQLException e)
    {
        cout << "Could not connect to database. Error message: " << e.what() << endl;
        system("pause");
        exit(1);
    }   

    //update
    pstmt = con>prepareStatement("UPDATE inventory SET quantity = ? WHERE name = ?");
    pstmt>setInt(1, 200);
    pstmt>setString(2, "banana");
    pstmt>executeQuery();
    printf("Row updated\n");
    
    delete con;
    delete pstmt;
    system("pause");
    return 0;
}
```


## <a name="delete-data"></a><span data-ttu-id="c1b98-159">Adat törlése</span><span class="sxs-lookup"><span data-stu-id="c1b98-159">Delete data</span></span>
<span data-ttu-id="c1b98-160">A következő kód használatával csatlakozhat, és beolvashatja az adatokat a **DELETE** SQL-utasítással.</span><span class="sxs-lookup"><span data-stu-id="c1b98-160">Use the following code to connect and read the data using a **DELETE** SQL statement.</span></span> <span data-ttu-id="c1b98-161">A kód az sql::Driver osztályt használja a connect() metódussal a MySQL-lel létesített kapcsolat létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="c1b98-161">The code uses sql::Driver class with the connect() method to establish a connection to MySQL.</span></span> <span data-ttu-id="c1b98-162">A kód ezután a prepareStatement() és az executeQuery() metódust használja a delete-parancsok futtatásához.</span><span class="sxs-lookup"><span data-stu-id="c1b98-162">Then the code uses method prepareStatement() and executeQuery() to run the delete commands.</span></span>

<span data-ttu-id="c1b98-163">Cserélje le a gazdagép, az adatbázisnév, a felhasználó és a jelszó paramétereit azokra az értékekre, amelyeket a kiszolgáló és az adatbázis létrehozásakor adott meg.</span><span class="sxs-lookup"><span data-stu-id="c1b98-163">Replace the Host, DBName, User, and Password parameters with the values that you specified when you created the server and database.</span></span> 

```csharp
#include <stdlib.h>
#include <iostream>
#include "stdafx.h"

#include "mysql_connection.h"
#include <cppconn/driver.h>
#include <cppconn/exception.h>
#include <cppconn/resultset.h>
#include <cppconn/prepared_statement.h>
using namespace std;

int main()
{
    sql::Driver *driver;
    sql::Connection *con;
    sql::PreparedStatement *pstmt;
    sql::ResultSet *result;

    try
    {
        driver = get_driver_instance();
        //for demonstration only. never save password in the code!
        con = driver>connect("tcp://myserver4demo.mysql.database.azure.com:3306/quickstartdb", "myadmin@myserver4demo", "server_admin_password");
    }
    catch (sql::SQLException e)
    {
        cout << "Could not connect to database. Error message: " << e.what() << endl;
        system("pause");
        exit(1);
    }
        
    //delete
    pstmt = con>prepareStatement("DELETE FROM inventory WHERE name = ?");
    pstmt>setString(1, "orange");
    result = pstmt>executeQuery();
    printf("Row deleted\n");    
    
    delete pstmt;
    delete con;
    delete result;
    system("pause");
    return 0;
}
```

## <a name="next-steps"></a><span data-ttu-id="c1b98-164">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c1b98-164">Next steps</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="c1b98-165">MySQL-adatbázis migrálása a MySQL-hez készült Azure Database-be memóriakép és visszaállítás használatával</span><span class="sxs-lookup"><span data-stu-id="c1b98-165">Migrate your MySQL database to Azure Database for MySQL using dump and restore</span></span>](concepts-migrate-dump-restore.md)
