---
title: "Java használatával PostgreSQL-tooAzure adatbázis kapcsolati |} Microsoft Docs"
description: "A gyors üzembe helyezés biztosít a Java kódminta PostgreSQL lekérdezése a Azure-adatbázis adatait és tooconnect használhatja."
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.custom: mvc
ms.devlang: java
ms.topic: quickstart
ms.date: 06/23/2017
ms.openlocfilehash: 8f6e0a47a0d6dfebf29eb56c31ccccabd7c2b670
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-database-for-postgresql-use-java-tooconnect-and-query-data"></a><span data-ttu-id="38b54-103">Azure PostgreSQL-adatbázishoz: használata Java tooconnect és lekérdezési adatok</span><span class="sxs-lookup"><span data-stu-id="38b54-103">Azure Database for PostgreSQL: Use Java tooconnect and query data</span></span>
<span data-ttu-id="38b54-104">A gyors üzembe helyezés bemutatja, hogyan tooconnect tooan Azure adatbázis a Java-alkalmazások használatával PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="38b54-104">This quickstart demonstrates how tooconnect tooan Azure Database for PostgreSQL using a Java application.</span></span> <span data-ttu-id="38b54-105">Azt illusztrálja, hogyan toouse SQL utasítás tooquery beszúrási, frissítési és törlési hello adatbázis adatait.</span><span class="sxs-lookup"><span data-stu-id="38b54-105">It shows how toouse SQL statements tooquery, insert, update, and delete data in hello database.</span></span> <span data-ttu-id="38b54-106">hello cikkben leírt lépések azt feltételezik, hogy ismeri a Java használatával történő fejlesztéséhez, és, hogy-e új tooworking PostgreSQL az Azure-adatbázissal.</span><span class="sxs-lookup"><span data-stu-id="38b54-106">hello steps in this article assume that you are familiar with developing using Java, and that you are new tooworking with Azure Database for PostgreSQL.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="38b54-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="38b54-107">Prerequisites</span></span>
<span data-ttu-id="38b54-108">A gyors üzembe helyezés kiindulási pontként ezek az útmutatók valamelyikével létrehozott hello erőforrást használ:</span><span class="sxs-lookup"><span data-stu-id="38b54-108">This quickstart uses hello resources created in either of these guides as a starting point:</span></span>
- [<span data-ttu-id="38b54-109">DB létrehozása – portál</span><span class="sxs-lookup"><span data-stu-id="38b54-109">Create DB - Portal</span></span>](quickstart-create-server-database-portal.md)
- [<span data-ttu-id="38b54-110">DB létrehozása – Azure CLI</span><span class="sxs-lookup"><span data-stu-id="38b54-110">Create DB - Azure CLI</span></span>](quickstart-create-server-database-azure-cli.md)

<span data-ttu-id="38b54-111">Emellett a következőket kell elvégezni:</span><span class="sxs-lookup"><span data-stu-id="38b54-111">You also need to:</span></span>
- <span data-ttu-id="38b54-112">Töltse le a hello [PostgreSQL JDBC-illesztőt](https://jdbc.postgresql.org/download.html) Java és hello Java fejlesztői készlet verziójának megfelelő.</span><span class="sxs-lookup"><span data-stu-id="38b54-112">Download hello [PostgreSQL JDBC Driver](https://jdbc.postgresql.org/download.html) matching your version of Java and hello Java Development Kit.</span></span>
- <span data-ttu-id="38b54-113">Vegye fel az alkalmazás classpath hello PostgreSQL JDBC jar-fájlra (például postgresql-42.1.1.jar).</span><span class="sxs-lookup"><span data-stu-id="38b54-113">Include hello PostgreSQL JDBC jar file (for example postgresql-42.1.1.jar) in your application classpath.</span></span> <span data-ttu-id="38b54-114">További információért lásd: [Az osztályútvonal részletei](https://jdbc.postgresql.org/documentation/head/classpath.html).</span><span class="sxs-lookup"><span data-stu-id="38b54-114">For more information, see [classpath details](https://jdbc.postgresql.org/documentation/head/classpath.html).</span></span>

## <a name="get-connection-information"></a><span data-ttu-id="38b54-115">Kapcsolatadatok lekérése</span><span class="sxs-lookup"><span data-stu-id="38b54-115">Get connection information</span></span>
<span data-ttu-id="38b54-116">Hello kapcsolat szükséges információkat tooconnect toohello Azure adatbázis beolvasása PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="38b54-116">Get hello connection information needed tooconnect toohello Azure Database for PostgreSQL.</span></span> <span data-ttu-id="38b54-117">Teljesen minősített kiszolgáló nevét és a bejelentkezési hitelesítő adatokat hello van szüksége.</span><span class="sxs-lookup"><span data-stu-id="38b54-117">You need hello fully qualified server name and login credentials.</span></span>

1. <span data-ttu-id="38b54-118">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="38b54-118">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="38b54-119">A hello Azure-portálon a bal oldali menüből, kattintson az **összes erőforrás** , és keressen a létrehozott, például a hello server **mypgserver-20170401**.</span><span class="sxs-lookup"><span data-stu-id="38b54-119">From hello left-hand menu in Azure portal, click **All resources** and search for hello server you have created, such as **mypgserver-20170401**.</span></span>
3. <span data-ttu-id="38b54-120">Hello kiszolgáló nevére kattint **mypgserver-20170401**.</span><span class="sxs-lookup"><span data-stu-id="38b54-120">Click hello server name **mypgserver-20170401**.</span></span>
4. <span data-ttu-id="38b54-121">Jelölje be hello server **áttekintése** lap.</span><span class="sxs-lookup"><span data-stu-id="38b54-121">Select hello server's **Overview** page.</span></span> <span data-ttu-id="38b54-122">Jegyezze fel a hello **kiszolgálónév** és **kiszolgálói rendszergazda bejelentkezési név**.</span><span class="sxs-lookup"><span data-stu-id="38b54-122">Make a note of hello **Server name** and **Server admin login name**.</span></span>
 <span data-ttu-id="38b54-123">![PostgreSQL-hez készült Azure-adatbázis – Kiszolgáló-rendszergazdai bejelentkezés](./media/connect-java/1-connection-string.png)</span><span class="sxs-lookup"><span data-stu-id="38b54-123">![Azure Database for PostgreSQL - Server Admin Login](./media/connect-java/1-connection-string.png)</span></span>
5. <span data-ttu-id="38b54-124">Ha elfelejti a kiszolgálói bejelentkezési adatok, keresse meg a toohello **áttekintése** tooview hello kiszolgálói rendszergazda bejelentkezési név lapon, és ha szükséges, állítsa vissza a hello jelszót.</span><span class="sxs-lookup"><span data-stu-id="38b54-124">If you forget your server login information, navigate toohello **Overview** page tooview hello Server admin login name and, if necessary, reset hello password.</span></span>

## <a name="connect-create-table-and-insert-data"></a><span data-ttu-id="38b54-125">Csatlakozás, táblák létrehozása és adatok beszúrása</span><span class="sxs-lookup"><span data-stu-id="38b54-125">Connect, create table, and insert data</span></span>
<span data-ttu-id="38b54-126">A következő kód tooconnect és hello az adatok betöltése a hello funkcióval használata hello egy **BESZÚRÁSA** SQL-utasításban.</span><span class="sxs-lookup"><span data-stu-id="38b54-126">Use hello following code tooconnect and load hello data using hello function with an **INSERT** SQL statement.</span></span> <span data-ttu-id="38b54-127">módszerek hello [getConnection()](https://www.postgresql.org/docs/7.4/static/jdbc-use.html), [createStatement()](https://jdbc.postgresql.org/documentation/head/query.html), és [executeQuery()](https://jdbc.postgresql.org/documentation/head/query.html) használt tooconnect, amelyek dobja el, és hozzon létre hello táblát.</span><span class="sxs-lookup"><span data-stu-id="38b54-127">hello methods [getConnection()](https://www.postgresql.org/docs/7.4/static/jdbc-use.html), [createStatement()](https://jdbc.postgresql.org/documentation/head/query.html), and [executeQuery()](https://jdbc.postgresql.org/documentation/head/query.html) are used tooconnect, drop, and create hello table.</span></span> <span data-ttu-id="38b54-128">Hello [prepareStatement](https://jdbc.postgresql.org/documentation/head/query.html) objektum használt toobuild hello insert utasítással, setString() és setInt() toobind hello paraméterértékeket.</span><span class="sxs-lookup"><span data-stu-id="38b54-128">hello [prepareStatement](https://jdbc.postgresql.org/documentation/head/query.html) object is used toobuild hello insert commands, with setString() and setInt() toobind hello parameter values.</span></span> <span data-ttu-id="38b54-129">Módszer [executeUpdate()](https://jdbc.postgresql.org/documentation/head/update.html) futtatása hello parancsot az egyes paramétereket.</span><span class="sxs-lookup"><span data-stu-id="38b54-129">Method [executeUpdate()](https://jdbc.postgresql.org/documentation/head/update.html) runs hello command for each set of parameters.</span></span> 

<span data-ttu-id="38b54-130">Cserélje le a saját kiszolgáló és az adatbázis létrehozása után a megadott hello értékek hello állomás, az adatbázis, a felhasználói és a password paraméter.</span><span class="sxs-lookup"><span data-stu-id="38b54-130">Replace hello host, database, user, and password parameters with hello values that you specified when you created your own server and database.</span></span>

```java
import java.sql.*;
import java.util.Properties;

public class CreateTableInsertRows {

    public static void main (String[] args)  throws Exception
    {

        // Initialize connection variables.
        String host = "mypgserver-20170401.postgres.database.azure.com";
        String database = "mypgsqldb";
        String user = "mylogin@mypgserver-20170401";
        String password = "<server_admin_password>";

        // check that hello driver is installed
        try
        {
            Class.forName("org.postgresql.Driver");
        }
        catch (ClassNotFoundException e)
        {
            throw new ClassNotFoundException("PostgreSQL JDBC driver NOT detected in library path.", e);
        }

        System.out.println("PostgreSQL JDBC driver detected in library path.");

        Connection connection = null;

        // Initialize connection object
        try
        {
            String url = String.format("jdbc:postgresql://%s/%s", host, database);
            
            // set up hello connection properties
            Properties properties = new Properties();
            properties.setProperty("user", user);
            properties.setProperty("password", password);
            properties.setProperty("ssl", "true");

            // get connection
            connection = DriverManager.getConnection(url, properties);
        }
        catch (SQLException e)
        {
            throw new SQLException("Failed toocreate connection toodatabase.", e);
        }
        if (connection != null) 
        { 
            System.out.println("Successfully created connection toodatabase.");
        
            // Perform some SQL queries over hello connection.
            try
            {
                // Drop previous table of same name if one exists.
                Statement statement = connection.createStatement();
                statement.execute("DROP TABLE IF EXISTS inventory;");
                System.out.println("Finished dropping table (if existed).");
    
                // Create table.
                statement.execute("CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);");
                System.out.println("Created table.");
    
                // Insert some data into table.
                int nRowsInserted = 0;
                PreparedStatement preparedStatement = connection.prepareStatement("INSERT INTO inventory (name, quantity) VALUES (?, ?);");
                preparedStatement.setString(1, "banana");
                preparedStatement.setInt(2, 150);
                nRowsInserted += preparedStatement.executeUpdate();

                preparedStatement.setString(1, "orange");
                preparedStatement.setInt(2, 154);
                nRowsInserted += preparedStatement.executeUpdate();

                preparedStatement.setString(1, "apple");
                preparedStatement.setInt(2, 100);
                nRowsInserted += preparedStatement.executeUpdate();
                System.out.println(String.format("Inserted %d row(s) of data.", nRowsInserted));
    
                // NOTE No need toocommit all changes toodatabase, as auto-commit is enabled by default.
    
            }
            catch (SQLException e)
            {
                throw new SQLException("Encountered an error when executing given sql statement.", e);
            }       
        }
        else {
            System.out.println("Failed toocreate connection toodatabase.");
        }
        System.out.println("Execution finished.");
    }
}
```

## <a name="read-data"></a><span data-ttu-id="38b54-131">Adatok olvasása</span><span class="sxs-lookup"><span data-stu-id="38b54-131">Read data</span></span>
<span data-ttu-id="38b54-132">Használjon hello következő tooread hello adatok kódot egy **válasszon** SQL-utasításban.</span><span class="sxs-lookup"><span data-stu-id="38b54-132">Use hello following code tooread hello data with a **SELECT** SQL statement.</span></span> <span data-ttu-id="38b54-133">módszerek hello [getConnection()](https://www.postgresql.org/docs/7.4/static/jdbc-use.html), [createStatement()](https://jdbc.postgresql.org/documentation/head/query.html), és [executeQuery()](https://jdbc.postgresql.org/documentation/head/query.html) használt tooconnect, amelyek létrehozása, és hello select utasítás futtatása.</span><span class="sxs-lookup"><span data-stu-id="38b54-133">hello methods [getConnection()](https://www.postgresql.org/docs/7.4/static/jdbc-use.html), [createStatement()](https://jdbc.postgresql.org/documentation/head/query.html), and [executeQuery()](https://jdbc.postgresql.org/documentation/head/query.html) are used tooconnect, create, and run hello select statement.</span></span> <span data-ttu-id="38b54-134">hello eredmények használatával dolgozzák fel a [eredményhalmazt](https://www.postgresql.org/docs/7.4/static/jdbc-query.html) objektum.</span><span class="sxs-lookup"><span data-stu-id="38b54-134">hello results are processed using a [ResultSet](https://www.postgresql.org/docs/7.4/static/jdbc-query.html) object.</span></span> 

<span data-ttu-id="38b54-135">Cserélje le a saját kiszolgáló és az adatbázis létrehozása után a megadott hello értékek hello állomás, az adatbázis, a felhasználói és a password paraméter.</span><span class="sxs-lookup"><span data-stu-id="38b54-135">Replace hello host, database, user, and password parameters with hello values that you specified when you created your own server and database.</span></span>

```java
import java.sql.*;
import java.util.Properties;

public class ReadTable {

    public static void main (String[] args)  throws Exception
    {

        // Initialize connection variables.
        String host = "mypgserver-20170401.postgres.database.azure.com";
        String database = "mypgsqldb";
        String user = "mylogin@mypgserver-20170401";
        String password = "<server_admin_password>";

        // check that hello driver is installed
        try
        {
            Class.forName("org.postgresql.Driver");
        }
        catch (ClassNotFoundException e)
        {
            throw new ClassNotFoundException("PostgreSQL JDBC driver NOT detected in library path.", e);
        }

        System.out.println("PostgreSQL JDBC driver detected in library path.");

        Connection connection = null;

        // Initialize connection object
        try
        {
            String url = String.format("jdbc:postgresql://%s/%s", host, database);
            
            // set up hello connection properties
            Properties properties = new Properties();
            properties.setProperty("user", user);
            properties.setProperty("password", password);
            properties.setProperty("ssl", "true");

            // get connection
            connection = DriverManager.getConnection(url, properties);
        }
        catch (SQLException e)
        {
            throw new SQLException("Failed toocreate connection toodatabase.", e);
        }
        if (connection != null) 
        { 
            System.out.println("Successfully created connection toodatabase.");
        
            // Perform some SQL queries over hello connection.
            try
            {
    
                Statement statement = connection.createStatement();
                ResultSet results = statement.executeQuery("SELECT * from inventory;");
                while (results.next())
                {
                    String outputString = 
                        String.format(
                            "Data row = (%s, %s, %s)",
                            results.getString(1),
                            results.getString(2),
                            results.getString(3));
                    System.out.println(outputString);
                }
            }
            catch (SQLException e)
            {
                throw new SQLException("Encountered an error when executing given sql statement.", e);
            }       
        }
        else {
            System.out.println("Failed toocreate connection toodatabase.");
        }
        System.out.println("Execution finished.");
    }
}

```

## <a name="update-data"></a><span data-ttu-id="38b54-136">Adatok frissítése</span><span class="sxs-lookup"><span data-stu-id="38b54-136">Update data</span></span>
<span data-ttu-id="38b54-137">Használjon hello következő toochange hello adatok kódot egy **frissítés** SQL-utasításban.</span><span class="sxs-lookup"><span data-stu-id="38b54-137">Use hello following code toochange hello data with an **UPDATE** SQL statement.</span></span> <span data-ttu-id="38b54-138">módszerek hello [getConnection()](https://www.postgresql.org/docs/7.4/static/jdbc-use.html), [prepareStatement()](https://jdbc.postgresql.org/documentation/head/query.html), és [executeUpdate()](https://jdbc.postgresql.org/documentation/head/update.html) használt tooconnect, amelyek előkészítése, és hello update utasítás futtatása.</span><span class="sxs-lookup"><span data-stu-id="38b54-138">hello methods [getConnection()](https://www.postgresql.org/docs/7.4/static/jdbc-use.html), [prepareStatement()](https://jdbc.postgresql.org/documentation/head/query.html), and [executeUpdate()](https://jdbc.postgresql.org/documentation/head/update.html) are used tooconnect, prepare, and run hello update statement.</span></span> 

<span data-ttu-id="38b54-139">Cserélje le a saját kiszolgáló és az adatbázis létrehozása után a megadott hello értékek hello állomás, az adatbázis, a felhasználói és a password paraméter.</span><span class="sxs-lookup"><span data-stu-id="38b54-139">Replace hello host, database, user, and password parameters with hello values that you specified when you created your own server and database.</span></span>

```java
import java.sql.*;
import java.util.Properties;

public class UpdateTable {
    public static void main (String[] args)  throws Exception
    {

        // Initialize connection variables.
        String host = "mypgserver-20170401.postgres.database.azure.com";
        String database = "mypgsqldb";
        String user = "mylogin@mypgserver-20170401";
        String password = "<server_admin_password>";

        // check that hello driver is installed
        try
        {
            Class.forName("org.postgresql.Driver");
        }
        catch (ClassNotFoundException e)
        {
            throw new ClassNotFoundException("PostgreSQL JDBC driver NOT detected in library path.", e);
        }

        System.out.println("PostgreSQL JDBC driver detected in library path.");

        Connection connection = null;

        // Initialize connection object
        try
        {
            String url = String.format("jdbc:postgresql://%s/%s", host, database);
            
            // set up hello connection properties
            Properties properties = new Properties();
            properties.setProperty("user", user);
            properties.setProperty("password", password);
            properties.setProperty("ssl", "true");

            // get connection
            connection = DriverManager.getConnection(url, properties);
        }
        catch (SQLException e)
        {
            throw new SQLException("Failed toocreate connection toodatabase.", e);
        }
        if (connection != null) 
        { 
            System.out.println("Successfully created connection toodatabase.");
        
            // Perform some SQL queries over hello connection.
            try
            {
                // Modify some data in table.
                int nRowsUpdated = 0;
                PreparedStatement preparedStatement = connection.prepareStatement("UPDATE inventory SET quantity = ? WHERE name = ?;");
                preparedStatement.setInt(1, 200);
                preparedStatement.setString(2, "banana");
                nRowsUpdated += preparedStatement.executeUpdate();
                System.out.println(String.format("Updated %d row(s) of data.", nRowsUpdated));
    
                // NOTE No need toocommit all changes toodatabase, as auto-commit is enabled by default.
            }
            catch (SQLException e)
            {
                throw new SQLException("Encountered an error when executing given sql statement.", e);
            }       
        }
        else {
            System.out.println("Failed toocreate connection toodatabase.");
        }
        System.out.println("Execution finished.");
    }
}
```
## <a name="delete-data"></a><span data-ttu-id="38b54-140">Adat törlése</span><span class="sxs-lookup"><span data-stu-id="38b54-140">Delete data</span></span>
<span data-ttu-id="38b54-141">Használjon hello következő tooremove adatok kódot egy **törlése** SQL-utasításban.</span><span class="sxs-lookup"><span data-stu-id="38b54-141">Use hello following code tooremove data with a **DELETE** SQL statement.</span></span> <span data-ttu-id="38b54-142">módszerek hello [getConnection()](https://www.postgresql.org/docs/7.4/static/jdbc-use.html), [prepareStatement()](https://jdbc.postgresql.org/documentation/head/query.html), és [executeUpdate()](https://jdbc.postgresql.org/documentation/head/update.html) használt tooconnect, amelyek előkészítéséhez, majd futtassa a hello delete utasításban.</span><span class="sxs-lookup"><span data-stu-id="38b54-142">hello methods [getConnection()](https://www.postgresql.org/docs/7.4/static/jdbc-use.html), [prepareStatement()](https://jdbc.postgresql.org/documentation/head/query.html), and [executeUpdate()](https://jdbc.postgresql.org/documentation/head/update.html) are used tooconnect, prepare, and run hello delete statement.</span></span> 

<span data-ttu-id="38b54-143">Cserélje le a saját kiszolgáló és az adatbázis létrehozása után a megadott hello értékek hello állomás, az adatbázis, a felhasználói és a password paraméter.</span><span class="sxs-lookup"><span data-stu-id="38b54-143">Replace hello host, database, user, and password parameters with hello values that you specified when you created your own server and database.</span></span>

```java
import java.sql.*;
import java.util.Properties;

public class DeleteTable {
    public static void main (String[] args)  throws Exception
    {

        // Initialize connection variables.
        String host = "mypgserver-20170401.postgres.database.azure.com";
        String database = "mypgsqldb";
        String user = "mylogin@mypgserver-20170401";
        String password = "<server_admin_password>";

        // check that hello driver is installed
        try
        {
            Class.forName("org.postgresql.Driver");
        }
        catch (ClassNotFoundException e)
        {
            throw new ClassNotFoundException("PostgreSQL JDBC driver NOT detected in library path.", e);
        }

        System.out.println("PostgreSQL JDBC driver detected in library path.");

        Connection connection = null;

        // Initialize connection object
        try
        {
            String url = String.format("jdbc:postgresql://%s/%s", host, database);
            
            // set up hello connection properties
            Properties properties = new Properties();
            properties.setProperty("user", user);
            properties.setProperty("password", password);
            properties.setProperty("ssl", "true");

            // get connection
            connection = DriverManager.getConnection(url, properties);
        }
        catch (SQLException e)
        {
            throw new SQLException("Failed toocreate connection toodatabase.", e);
        }
        if (connection != null) 
        { 
            System.out.println("Successfully created connection toodatabase.");
        
            // Perform some SQL queries over hello connection.
            try
            {
                // Delete some data from table.
                int nRowsDeleted = 0;
                PreparedStatement preparedStatement = connection.prepareStatement("DELETE FROM inventory WHERE name = ?;");
                preparedStatement.setString(1, "orange");
                nRowsDeleted += preparedStatement.executeUpdate();
                System.out.println(String.format("Deleted %d row(s) of data.", nRowsDeleted));
    
                // NOTE No need toocommit all changes toodatabase, as auto-commit is enabled by default.
            }
            catch (SQLException e)
            {
                throw new SQLException("Encountered an error when executing given sql statement.", e);
            }       
        }
        else {
            System.out.println("Failed toocreate connection toodatabase.");
        }
        System.out.println("Execution finished.");
    }
}
```

## <a name="next-steps"></a><span data-ttu-id="38b54-144">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="38b54-144">Next steps</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="38b54-145">Adatbázis migrálása exportálással és importálással</span><span class="sxs-lookup"><span data-stu-id="38b54-145">Migrate your database using Export and Import</span></span>](./howto-migrate-using-export-and-import.md)
