---
title: "Csatlakozás a MySQL-hez készült Azure Database-hez a C# segítségével | Microsoft Docs"
description: "Ez a rövid útmutató egy C# (.NET) mintakódot biztosít, amellyel csatlakozhat a MySQL-hez készült Azure-adatbázishoz, illetve adatokat kérdezhet le róla."
services: MySQL
author: seanli1988
ms.author: seal
manager: janders
editor: jasonwhowell
ms.service: MySQL-database
ms.custom: mvc
ms.devlang: csharp
ms.topic: hero-article
ms.date: 07/10/2017
ms.openlocfilehash: f1488f6b4a240165c71c95f759af73d6b9fd7bfe
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="azure-database-for-mysql-use-net-c-to-connect-and-query-data"></a><span data-ttu-id="e67b1-103">A MySQL-hez készült Azure-adatbázis: Csatlakozás és adatlekérdezés a .Net C# használatával</span><span class="sxs-lookup"><span data-stu-id="e67b1-103">Azure Database for MySQL: Use .NET (C#) to connect and query data</span></span>
<span data-ttu-id="e67b1-104">Ebben a rövid útmutatóban azt szemléltetjük, hogy miként lehet C#-alkalmazás használatával csatlakozni a MySQL-hez készült Azure Database-hez.</span><span class="sxs-lookup"><span data-stu-id="e67b1-104">This quickstart demonstrates how to connect to an Azure Database for MySQL using a C# application.</span></span> <span data-ttu-id="e67b1-105">Azt is bemutatja, hogyan lehet SQL-utasítások használatával adatokat lekérdezni, beszúrni, frissíteni és törölni az adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="e67b1-105">It shows how to use SQL statements to query, insert, update, and delete data in the database.</span></span> <span data-ttu-id="e67b1-106">A cikkben ismertetett lépések feltételezik, hogy Ön rendelkezik fejlesztési tapasztalatokkal a C# használatával kapcsolatban, a MySQL-hez készült Azure Database használatában pedig még járatlan.</span><span class="sxs-lookup"><span data-stu-id="e67b1-106">The steps in this article assume that you are familiar with developing using C#, and that you are new to working with Azure Database for MySQL.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e67b1-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="e67b1-107">Prerequisites</span></span>
<span data-ttu-id="e67b1-108">Ebben a rövid útmutatóban a következő útmutatók valamelyikében létrehozott erőforrásokat használunk kiindulási pontként:</span><span class="sxs-lookup"><span data-stu-id="e67b1-108">This quickstart uses the resources created in either of these guides as a starting point:</span></span>
- [<span data-ttu-id="e67b1-109">Azure-adatbázis létrehozása MySQL-kiszolgálóhoz az Azure Portal használatával</span><span class="sxs-lookup"><span data-stu-id="e67b1-109">Create an Azure Database for MySQL server using Azure portal</span></span>](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [<span data-ttu-id="e67b1-110">Azure-adatbázis létrehozása MySQL-kiszolgálóhoz az Azure CLI használatával</span><span class="sxs-lookup"><span data-stu-id="e67b1-110">Create an Azure Database for MySQL server using Azure CLI</span></span>](./quickstart-create-mysql-server-database-using-azure-cli.md)

<span data-ttu-id="e67b1-111">Emellett a következőket kell elvégezni:</span><span class="sxs-lookup"><span data-stu-id="e67b1-111">You also need to:</span></span>
- <span data-ttu-id="e67b1-112">A [.NET](https://www.microsoft.com/net/download) telepítése.</span><span class="sxs-lookup"><span data-stu-id="e67b1-112">Install [.NET](https://www.microsoft.com/net/download).</span></span> <span data-ttu-id="e67b1-113">A .NET-keretrendszer az adott platformon (Windows, Ubuntu Linux vagy macOS) történő telepítéséhez kövesse a hivatkozott cikk lépéseit.</span><span class="sxs-lookup"><span data-stu-id="e67b1-113">Follow the steps in the linked article to install .NET specifically for your platform (Windows, Ubuntu Linux, or macOS).</span></span> 
- <span data-ttu-id="e67b1-114">A [Visual Studio](https://www.visualstudio.com/downloads/) telepítése.</span><span class="sxs-lookup"><span data-stu-id="e67b1-114">Install [Visual Studio](https://www.visualstudio.com/downloads/).</span></span>
- <span data-ttu-id="e67b1-115">A [MySQL-hez készült ODBC-illesztő](https://dev.mysql.com/downloads/connector/odbc/) telepítése.</span><span class="sxs-lookup"><span data-stu-id="e67b1-115">Install [ODBC Driver for MySQL](https://dev.mysql.com/downloads/connector/odbc/).</span></span>

## <a name="get-connection-information"></a><span data-ttu-id="e67b1-116">Kapcsolatadatok lekérése</span><span class="sxs-lookup"><span data-stu-id="e67b1-116">Get connection information</span></span>
<span data-ttu-id="e67b1-117">Kérje le a MySQL-hez készült Azure Database-hez való csatlakozáshoz szükséges kapcsolatadatokat.</span><span class="sxs-lookup"><span data-stu-id="e67b1-117">Get the connection information needed to connect to the Azure Database for MySQL.</span></span> <span data-ttu-id="e67b1-118">Ehhez szükség lesz a teljes kiszolgálónévre és bejelentkezési hitelesítő adatokra.</span><span class="sxs-lookup"><span data-stu-id="e67b1-118">You need the fully qualified server name and login credentials.</span></span>

1. <span data-ttu-id="e67b1-119">Jelentkezzen be az [Azure portálra](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="e67b1-119">Log in to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="e67b1-120">Az Azure Portal bal oldali menüjében kattintson az **Összes erőforrás** lehetőségre, és keressen rá a létrehozott kiszolgálóra, például **myserver4demo**.</span><span class="sxs-lookup"><span data-stu-id="e67b1-120">From the left-hand menu in Azure portal, click **All resources** and search for the server you have created, such as **myserver4demo**.</span></span>
3. <span data-ttu-id="e67b1-121">Kattintson a kiszolgálónévre.</span><span class="sxs-lookup"><span data-stu-id="e67b1-121">Click the server name.</span></span>
4. <span data-ttu-id="e67b1-122">Válassza a kiszolgáló **tulajdonságlapját**.</span><span class="sxs-lookup"><span data-stu-id="e67b1-122">Select the server's **Properties** page.</span></span> <span data-ttu-id="e67b1-123">Jegyezze fel a **Kiszolgálónevet** és a **Kiszolgáló-rendszergazdai bejelentkezési nevet**.</span><span class="sxs-lookup"><span data-stu-id="e67b1-123">Make a note of the **Server name** and **Server admin login name**.</span></span>
 <span data-ttu-id="e67b1-124">![A MySQL-hez készült Azure Database-kiszolgáló neve](./media/connect-csharp/1_server-properties-name-login.png)</span><span class="sxs-lookup"><span data-stu-id="e67b1-124">![Azure Database for MySQL server name](./media/connect-csharp/1_server-properties-name-login.png)</span></span>
5. <span data-ttu-id="e67b1-125">Amennyiben elfelejtette a kiszolgáló bejelentkezési adatait, lépjen az **Overview** (Áttekintés) oldalra, és itt megtudhatja a kiszolgáló rendszergazdájának bejelentkezési nevét, valamint szükség esetén visszaállíthatja a jelszót.</span><span class="sxs-lookup"><span data-stu-id="e67b1-125">If you forget your server login information, navigate to the **Overview** page to view the Server admin login name and, if necessary, reset the password.</span></span>

## <a name="connect-create-table-and-insert-data"></a><span data-ttu-id="e67b1-126">Csatlakozás, táblák létrehozása és adatok beszúrása</span><span class="sxs-lookup"><span data-stu-id="e67b1-126">Connect, create table, and insert data</span></span>
<span data-ttu-id="e67b1-127">Az alábbi kód használatával csatlakozhat és töltheti be az adatokat a **CREATE TABLE** és az **INSERT INTO** SQL-utasítások segítségével.</span><span class="sxs-lookup"><span data-stu-id="e67b1-127">Use the following code to connect and load the data using **CREATE TABLE** and  **INSERT INTO** SQL statements.</span></span> <span data-ttu-id="e67b1-128">A kód ODBC-osztályt használ az [Open()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbcconnection.open(v=vs.110).aspx) metódussal, hogy kapcsolatot létesítsen a MySQL-lel.</span><span class="sxs-lookup"><span data-stu-id="e67b1-128">The code uses ODBC class with method [Open()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbcconnection.open(v=vs.110).aspx) to establish a connection to MySQL.</span></span> <span data-ttu-id="e67b1-129">A kód ezután a [CreateCommand()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbcconnection.createcommand(v=vs.110).aspx) metódust használja, beállítja a CommandText tulajdonságot, és meghívja az [ExecuteNonQuery()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbccommand.executenonquery(v=vs.110).aspx) metódust az adatbázis-parancsok futtatásához.</span><span class="sxs-lookup"><span data-stu-id="e67b1-129">Then the code uses method [CreateCommand()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbcconnection.createcommand(v=vs.110).aspx), sets the CommandText property, and calls method [ExecuteNonQuery()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbccommand.executenonquery(v=vs.110).aspx) to run the database commands.</span></span> 

<span data-ttu-id="e67b1-130">Cserélje le a gazdagép, az adatbázisnév, a felhasználó és a jelszó paramétereit azokra az értékekre, amelyeket a kiszolgáló és az adatbázis létrehozásakor adott meg.</span><span class="sxs-lookup"><span data-stu-id="e67b1-130">Replace the Host, DBName, User, and Password parameters with the values that you specified when you created the server and database.</span></span> 

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using MySql.Data;
using System.Data.Odbc;

namespace driver
{
    class MySQLCreate
    {
        static void Main(string[] args)
        {
            var conn = new OdbcConnection("DRIVER={MySQL ODBC 5.3 unicode Driver}; Server=myserver4demo.mysql.database.azure.com; Port=3306;" +
            " Database=quickstartdb; Uid=myadmin@myserver4demo; Pwd=server_admin_password; sslverify=0; Option=3;MULTI_STATEMENTS=1");

            Console.Out.WriteLine("Opening connection");
            conn.Open();

            var command = conn.CreateCommand();
            command.CommandText = "DROP TABLE IF EXISTS inventory;";
            command.ExecuteNonQuery();
            Console.Out.WriteLine("Finished dropping table (if existed)");

            command.CommandText = "CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);";
            command.ExecuteNonQuery();
            Console.Out.WriteLine("Finished creating table");

            command.CommandText =
                String.Format(
                    @"INSERT INTO inventory (name, quantity) VALUES ({0}, {1});
                    INSERT INTO inventory (name, quantity) VALUES ({2}, {3});
                    INSERT INTO inventory (name, quantity) VALUES ({4}, {5});",
                    "\'banana\'", 150,
                    "\'orange\'", 154,
                    "\'apple\'", 100
                    );

            int nRows = command.ExecuteNonQuery();
            Console.Out.WriteLine(String.Format("Number of rows inserted={0}", nRows));

            Console.Out.WriteLine("Closing connection");
            conn.Close();

            Console.WriteLine("Press RETURN to exit");
            Console.ReadLine();
        }

    }
}

```

## <a name="read-data"></a><span data-ttu-id="e67b1-131">Adatok olvasása</span><span class="sxs-lookup"><span data-stu-id="e67b1-131">Read data</span></span>

<span data-ttu-id="e67b1-132">A következő kóddal csatlakozhat, és beolvashatja az adatokat a **SELECT** SQL-utasítással.</span><span class="sxs-lookup"><span data-stu-id="e67b1-132">Use the following code to connect and read the data using a **SELECT** SQL statement.</span></span> <span data-ttu-id="e67b1-133">A kód ODBC-osztályt használ az [Open()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbcconnection.open(v=vs.110).aspx) metódussal, hogy kapcsolatot létesítsen a MySQL-lel.</span><span class="sxs-lookup"><span data-stu-id="e67b1-133">The code uses ODBC class with method [Open()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbcconnection.open(v=vs.110).aspx) to establish a connection to MySQL.</span></span> <span data-ttu-id="e67b1-134">A kód ezután a [CreateCommand()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbcconnection.createcommand(v=vs.110).aspx) és az [ExecuteReader()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbccommand.executereader(v=vs.110).aspx) metódust használja az adatbázis-parancsok futtatásához.</span><span class="sxs-lookup"><span data-stu-id="e67b1-134">Then the code uses method [CreateCommand()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbcconnection.createcommand(v=vs.110).aspx) and method [ExecuteReader()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbccommand.executereader(v=vs.110).aspx) to run the database commands.</span></span> <span data-ttu-id="e67b1-135">Ezután a kód a [Read()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbcdatareader.read(v=vs.110).aspx) metódust használja az eredményekben lévő rekordokra lépéshez.</span><span class="sxs-lookup"><span data-stu-id="e67b1-135">Next the code uses [Read()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbcdatareader.read(v=vs.110).aspx) to advance to the records in the results.</span></span> <span data-ttu-id="e67b1-136">Ezután a kód a GetInt32 és a GetString metódussal elemzi a rekord értékeit.</span><span class="sxs-lookup"><span data-stu-id="e67b1-136">Then the code uses GetInt32 and GetString to parse the values in the record.</span></span>

<span data-ttu-id="e67b1-137">Cserélje le a gazdagép, az adatbázisnév, a felhasználó és a jelszó paramétereit azokra az értékekre, amelyeket a kiszolgáló és az adatbázis létrehozásakor adott meg.</span><span class="sxs-lookup"><span data-stu-id="e67b1-137">Replace the Host, DBName, User, and Password parameters with the values that you specified when you created the server and database.</span></span> 

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using MySql.Data;
using System.Data.Odbc;


namespace driver
{
    class MySQLRead
    {

        static void Main(string[] args)
        {
            var conn = new OdbcConnection("DRIVER={MySQL ODBC 5.3 unicode Driver}; Server=myserver4demo.mysql.database.azure.com; Port=3306;" +
            " Database=quickstartdb; Uid=myadmin@myserver4demo; Pwd=server_admin_password; sslverify=0; Option=3;MULTI_STATEMENTS=1");

            Console.Out.WriteLine("Opening connection");
            conn.Open();

            var command = conn.CreateCommand();
            command.CommandText = "SELECT * FROM inventory;";

            var reader = command.ExecuteReader();
            while (reader.Read())
            {
                Console.WriteLine(
                    string.Format(
                        "Reading from table=({0}, {1}, {2})",
                        reader.GetInt32(0).ToString(),
                        reader.GetString(1),
                        reader.GetInt32(2).ToString()
                        )
                    );
            }

            Console.Out.WriteLine("Closing connection");
            conn.Close();

            Console.WriteLine("Press RETURN to exit");
            Console.ReadLine();
        }
    }
}


```

## <a name="update-data"></a><span data-ttu-id="e67b1-138">Adatok frissítése</span><span class="sxs-lookup"><span data-stu-id="e67b1-138">Update data</span></span>
<span data-ttu-id="e67b1-139">Az alábbi kód használatával csatlakozhat és végezheti el az adatok olvasását **UPDATE** SQL-utasítás segítségével.</span><span class="sxs-lookup"><span data-stu-id="e67b1-139">Use the following code to connect and read the data using a **UPDATE** SQL statement.</span></span> <span data-ttu-id="e67b1-140">A kód ODBC-osztályt használ az [Open()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbcconnection.open(v=vs.110).aspx) metódussal, hogy kapcsolatot létesítsen a MySQL-lel.</span><span class="sxs-lookup"><span data-stu-id="e67b1-140">The code uses ODBC class with method [Open()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbcconnection.open(v=vs.110).aspx) to establish a connection to MySQL.</span></span> <span data-ttu-id="e67b1-141">A kód ezután a [CreateCommand()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbcconnection.createcommand(v=vs.110).aspx) metódust használja, beállítja a CommandText tulajdonságot, és meghívja az [ExecuteNonQuery()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbccommand.executenonquery(v=vs.110).aspx) metódust az adatbázis-parancsok futtatásához.</span><span class="sxs-lookup"><span data-stu-id="e67b1-141">Then the code uses method [CreateCommand()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbcconnection.createcommand(v=vs.110).aspx), sets the CommandText property, and calls method [ExecuteNonQuery()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbccommand.executenonquery(v=vs.110).aspx) to run the database commands.</span></span>

<span data-ttu-id="e67b1-142">Cserélje le a gazdagép, az adatbázisnév, a felhasználó és a jelszó paramétereit azokra az értékekre, amelyeket a kiszolgáló és az adatbázis létrehozásakor adott meg.</span><span class="sxs-lookup"><span data-stu-id="e67b1-142">Replace the Host, DBName, User, and Password parameters with the values that you specified when you created the server and database.</span></span> 

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Data.Odbc;

namespace driver
{
    class MySQLUpdate
    {
        static void Main(string[] args)
        {
            var conn = new OdbcConnection("DRIVER={MySQL ODBC 5.3 unicode Driver}; Server=myserver4demo.mysql.database.azure.com; Port=3306;" +
            " Database=quickstartdb; Uid=myadmin@myserver4demo; Pwd=server_admin_password; sslverify=0; Option=3;MULTI_STATEMENTS=1");

            Console.Out.WriteLine("Opening connection");
            conn.Open();

            var command = conn.CreateCommand();
            command.CommandText =
            String.Format("UPDATE inventory SET quantity = {0} WHERE name = {1};",
                200,
                "\'banana\'"
                );

            int nRows = command.ExecuteNonQuery();
            Console.Out.WriteLine(String.Format("Number of rows updated={0}", nRows));

            Console.Out.WriteLine("Closing connection");
            conn.Close();

            Console.WriteLine("Press RETURN to exit");
            Console.ReadLine();
        }
    }
}



```


## <a name="delete-data"></a><span data-ttu-id="e67b1-143">Adat törlése</span><span class="sxs-lookup"><span data-stu-id="e67b1-143">Delete data</span></span>
<span data-ttu-id="e67b1-144">Az alábbi kód használatával csatlakozhat és végezheti el az adatok törlését egy **DELETE** SQL-utasítás segítségével.</span><span class="sxs-lookup"><span data-stu-id="e67b1-144">Use the following code to connect and delete the data using a **DELETE** SQL statement.</span></span> 

<span data-ttu-id="e67b1-145">A kód ODBC-osztályt használ az [Open()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbcconnection.open(v=vs.110).aspx) metódussal, hogy kapcsolatot létesítsen a MySQL-lel.</span><span class="sxs-lookup"><span data-stu-id="e67b1-145">The code uses ODBC class with method [Open()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbcconnection.open(v=vs.110).aspx) to establish a connection to MySQL.</span></span> <span data-ttu-id="e67b1-146">A kód ezután a [CreateCommand()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbcconnection.createcommand(v=vs.110).aspx) metódust használja, beállítja a CommandText tulajdonságot, és meghívja az [ExecuteNonQuery()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbccommand.executenonquery(v=vs.110).aspx) metódust az adatbázis-parancsok futtatásához.</span><span class="sxs-lookup"><span data-stu-id="e67b1-146">Then the code uses method [CreateCommand()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbcconnection.createcommand(v=vs.110).aspx), sets the CommandText property, and calls method [ExecuteNonQuery()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbccommand.executenonquery(v=vs.110).aspx) to run the database commands.</span></span>

<span data-ttu-id="e67b1-147">Cserélje le a gazdagép, az adatbázisnév, a felhasználó és a jelszó paramétereit azokra az értékekre, amelyeket a kiszolgáló és az adatbázis létrehozásakor adott meg.</span><span class="sxs-lookup"><span data-stu-id="e67b1-147">Replace the Host, DBName, User, and Password parameters with the values that you specified when you created the server and database.</span></span> 

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Data.Odbc;

namespace driver
{
    class MySQLDelete
    {
        static void Main(string[] args)
        {
            var conn = new OdbcConnection("DRIVER={MySQL ODBC 5.3 unicode Driver}; Server=myserver4demo.mysql.database.azure.com; Port=3306;" +
            " Database=quickstartdb; Uid=myadmin@myserver4demo; Pwd=server_admin_password; sslverify=0; Option=3;MULTI_STATEMENTS=1");

            Console.Out.WriteLine("Opening connection");
            conn.Open();

            var command = conn.CreateCommand();
            command.CommandText =
                String.Format("DELETE FROM inventory WHERE name = {0};",
                    "\'orange\'");
            int nRows = command.ExecuteNonQuery();
            Console.Out.WriteLine(String.Format("Number of rows deleted={0}", nRows));

            Console.Out.WriteLine("Closing connection");
            conn.Close();

            Console.WriteLine("Press RETURN to exit");
            Console.ReadLine();
        }
    }
}

```

## <a name="next-steps"></a><span data-ttu-id="e67b1-148">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e67b1-148">Next steps</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="e67b1-149">MySQL-adatbázis migrálása a MySQL-hez készült Azure Database-be memóriakép és visszaállítás használatával</span><span class="sxs-lookup"><span data-stu-id="e67b1-149">Migrate your MySQL database to Azure Database for MySQL using dump and restore</span></span>](concepts-migrate-dump-restore.md)
