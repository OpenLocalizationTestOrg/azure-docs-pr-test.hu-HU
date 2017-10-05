---
title: "Csatlakozás a MySQL-hez készült Azure Database-hez a Java használatával | Microsoft Docs"
description: "Az alábbi gyors útmutatóban egy olyan Java-kódminta található, amely a MySQL-hez készült Azure Database csatlakoztatására és adatlekérdezésre használható."
services: mysql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.custom: mvc
ms.topic: hero-article
ms.devlang: java
ms.date: 06/20/2017
ms.openlocfilehash: 6ffcf3b38a3d868dfa10ea2e2a9d097441387d4f
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="azure-database-for-mysql-use-java-to-connect-and-query-data"></a><span data-ttu-id="e5448-103">A MySQL-hez készült Azure Database: Csatlakozás és adatlekérdezés a Java használatával</span><span class="sxs-lookup"><span data-stu-id="e5448-103">Azure Database for MySQL: Use Java to connect and query data</span></span>
<span data-ttu-id="e5448-104">Ebben a gyors útmutatóban azt szemléltetjük, hogy miként lehet Java-alkalmazás használatával csatlakozni a MySQL-hez készült Azure Database-hez.</span><span class="sxs-lookup"><span data-stu-id="e5448-104">This quickstart demonstrates how to connect to an Azure Database for MySQL using a Java application.</span></span> <span data-ttu-id="e5448-105">Bemutatjuk, hogy az SQL-utasítások használatával hogyan kérdezhetők le, illeszthetők be, frissíthetők és törölhetők az adatok az adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="e5448-105">It shows how to use SQL statements to query, insert, update, and delete data in the database.</span></span> <span data-ttu-id="e5448-106">A cikkben ismertetett lépések feltételezik, hogy Ön rendelkezik fejlesztési tapasztalatokkal a Java használatával kapcsolatban, a MySQL-hez készült Azure Database használatában pedig még járatlan.</span><span class="sxs-lookup"><span data-stu-id="e5448-106">The steps in this article assume that you are familiar with developing using Java, and that you are new to working with Azure Database for MySQL.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e5448-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="e5448-107">Prerequisites</span></span>
<span data-ttu-id="e5448-108">Ebben a rövid útmutatóban a következő útmutatók valamelyikében létrehozott erőforrásokat használunk kiindulási pontként:</span><span class="sxs-lookup"><span data-stu-id="e5448-108">This quickstart uses the resources created in either of these guides as a starting point:</span></span>
- [<span data-ttu-id="e5448-109">Azure-adatbázis létrehozása MySQL-kiszolgálóhoz az Azure Portal használatával</span><span class="sxs-lookup"><span data-stu-id="e5448-109">Create an Azure Database for MySQL server using Azure portal</span></span>](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [<span data-ttu-id="e5448-110">Azure-adatbázis létrehozása MySQL-kiszolgálóhoz az Azure CLI használatával</span><span class="sxs-lookup"><span data-stu-id="e5448-110">Create an Azure Database for MySQL server using Azure CLI</span></span>](./quickstart-create-mysql-server-database-using-azure-cli.md)

<span data-ttu-id="e5448-111">Emellett a következőket kell elvégezni:</span><span class="sxs-lookup"><span data-stu-id="e5448-111">You also need to:</span></span>
- <span data-ttu-id="e5448-112">Töltse le a [MySQL Connector/J](https://dev.mysql.com/downloads/connector/j/) JDBC-illesztőprogramot</span><span class="sxs-lookup"><span data-stu-id="e5448-112">Download the JDBC driver [MySQL Connector/J](https://dev.mysql.com/downloads/connector/j/)</span></span>
- <span data-ttu-id="e5448-113">Illessze be a JDBC jar-fájlját (például mysql-connector-java-5.1.42-bin.jar) az alkalmazás osztályútvonalába.</span><span class="sxs-lookup"><span data-stu-id="e5448-113">Include the JDBC jar file (for example mysql-connector-java-5.1.42-bin.jar) into your application classpath.</span></span> <span data-ttu-id="e5448-114">Ha problémát tapasztal az itt leírtakkal kapcsolatban, tekintse meg környezete dokumentációját az osztályok elérési útvonalával kapcsolatban (például: [Apache Tomcat](https://tomcat.apache.org/tomcat-7.0-doc/class-loader-howto.html) vagy [Java SE](http://docs.oracle.com/javase/7/docs/technotes/tools/windows/classpath.html)).</span><span class="sxs-lookup"><span data-stu-id="e5448-114">If you have trouble with this, please consult your environment's documentation for class path specifics, such as [Apache Tomcat](https://tomcat.apache.org/tomcat-7.0-doc/class-loader-howto.html) or [Java SE](http://docs.oracle.com/javase/7/docs/technotes/tools/windows/classpath.html)</span></span>
- <span data-ttu-id="e5448-115">A MySQL-hez készült Azure Database-kapcsolat biztonsága megnyitott tűzfallal van konfigurálva és az SSL-beállítások úgy vannak megadva, hogy csatlakozni tudjon az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="e5448-115">Ensure your Azure Database for MySQL connection security is configured with the firewall opened and SSL settings adjusted for your application to connect successfully.</span></span>

## <a name="get-connection-information"></a><span data-ttu-id="e5448-116">Kapcsolatadatok lekérése</span><span class="sxs-lookup"><span data-stu-id="e5448-116">Get connection information</span></span>
<span data-ttu-id="e5448-117">Kérje le a MySQL-hez készült Azure Database-hez való csatlakozáshoz szükséges kapcsolatadatokat.</span><span class="sxs-lookup"><span data-stu-id="e5448-117">Get the connection information needed to connect to the Azure Database for MySQL.</span></span> <span data-ttu-id="e5448-118">Ehhez szükség lesz a teljes kiszolgálónévre és bejelentkezési hitelesítő adatokra.</span><span class="sxs-lookup"><span data-stu-id="e5448-118">You need the fully qualified server name and login credentials.</span></span>

1. <span data-ttu-id="e5448-119">Jelentkezzen be az [Azure portálra](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="e5448-119">Log in to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="e5448-120">A bal oldali ablaktáblán kattintson a **Minden erőforrás** lehetőségre, és keressen rá a létrehozott kiszolgálóra (például: **myserver4demo**).</span><span class="sxs-lookup"><span data-stu-id="e5448-120">In the left pane, click **All resources**, and then search for the server you have created (for example, **myserver4demo**).</span></span>
3. <span data-ttu-id="e5448-121">Kattintson a kiszolgálónévre.</span><span class="sxs-lookup"><span data-stu-id="e5448-121">Click the server name.</span></span>
4. <span data-ttu-id="e5448-122">Válassza a kiszolgáló **tulajdonságlapját**.</span><span class="sxs-lookup"><span data-stu-id="e5448-122">Select the server's **Properties** page.</span></span> <span data-ttu-id="e5448-123">Jegyezze fel a **Kiszolgálónevet** és a **Kiszolgáló-rendszergazdai bejelentkezési nevet**.</span><span class="sxs-lookup"><span data-stu-id="e5448-123">Make a note of the **Server name** and **Server admin login name**.</span></span>
 <span data-ttu-id="e5448-124">![A MySQL-hez készült Azure Database-kiszolgáló neve](./media/connect-java/1_server-properties-name-login.png)</span><span class="sxs-lookup"><span data-stu-id="e5448-124">![Azure Database for MySQL server name](./media/connect-java/1_server-properties-name-login.png)</span></span>
5. <span data-ttu-id="e5448-125">Amennyiben elfelejtette a kiszolgáló bejelentkezési adatait, lépjen az **Overview** (Áttekintés) oldalra, és itt megtudhatja a kiszolgáló rendszergazdájának bejelentkezési nevét, valamint szükség esetén visszaállíthatja a jelszót.</span><span class="sxs-lookup"><span data-stu-id="e5448-125">If you forget your server login information, navigate to the **Overview** page to view the Server admin login name and, if necessary, reset the password.</span></span>

## <a name="connect-create-table-and-insert-data"></a><span data-ttu-id="e5448-126">Csatlakozás, táblák létrehozása és adatok beszúrása</span><span class="sxs-lookup"><span data-stu-id="e5448-126">Connect, create table, and insert data</span></span>
<span data-ttu-id="e5448-127">Az alábbi kód használatával csatlakozhat és tölthet be adatokat az **INSERT SQL-utasítással** használt függvény segítségével.</span><span class="sxs-lookup"><span data-stu-id="e5448-127">Use the following code to connect and load the data using the function with an **INSERT** SQL statement.</span></span> <span data-ttu-id="e5448-128">A [getConnection()](https://dev.mysql.com/doc/connector-j/5.1/en/connector-j-usagenotes-connect-drivermanager.html) metódus a MySQL-hez való kapcsolódásra szolgál.</span><span class="sxs-lookup"><span data-stu-id="e5448-128">The [getConnection()](https://dev.mysql.com/doc/connector-j/5.1/en/connector-j-usagenotes-connect-drivermanager.html) method is used to connect to MySQL.</span></span> <span data-ttu-id="e5448-129">A [createStatement()](https://dev.mysql.com/doc/connector-j/5.1/en/connector-j-usagenotes-statements.html) és az execute() metódusok a tábla létrehozásához, illetve törléséhez használatosak.</span><span class="sxs-lookup"><span data-stu-id="e5448-129">Methods [createStatement()](https://dev.mysql.com/doc/connector-j/5.1/en/connector-j-usagenotes-statements.html) and execute() are used to drop and create the table.</span></span> <span data-ttu-id="e5448-130">A prepareStatement objektummal hozhatja létre a beszúrási parancsokat, valamint a setString() és a setInt() metódusokkal végezheti el a paraméterértékek kötését.</span><span class="sxs-lookup"><span data-stu-id="e5448-130">The prepareStatement object is used to build the insert commands, with setString() and setInt() to bind the parameter values.</span></span> <span data-ttu-id="e5448-131">Az executeUpdate() metódussal futtathatja az egyes paraméterkészletekhez tartozó értékek beszúrására szolgáló parancsot.</span><span class="sxs-lookup"><span data-stu-id="e5448-131">Method executeUpdate() runs the command for each set of parameters to insert the values.</span></span> 

<span data-ttu-id="e5448-132">Cserélje le a gazdagép, az adatbázis, a felhasználó és a jelszó paramétereit azokra az értékekre, amelyeket a saját kiszolgáló és adatbázis létrehozásakor adott meg.</span><span class="sxs-lookup"><span data-stu-id="e5448-132">Replace the host, database, user, and password parameters with the values that you specified when you created your own server and database.</span></span>

```java
import java.sql.*;
import java.util.Properties;

public class CreateTableInsertRows {

    public static void main (String[] args)  throws Exception
    {
        // Initialize connection variables. 
        String host = "myserver4demo.mysql.database.azure.com";
        String database = "quickstartdb";
        String user = "myadmin@myserver4demo";
        String password = "<server_admin_password>";

        // check that the driver is installed
        try
        {
            Class.forName("com.mysql.jdbc.Driver");
        }
        catch (ClassNotFoundException e)
        {
            throw new ClassNotFoundException("MySQL JDBC driver NOT detected in library path.", e);
        }

        System.out.println("MySQL JDBC driver detected in library path.");

        Connection connection = null;

        // Initialize connection object
        try
        {
            String url = String.format("jdbc:mysql://%s/%s", host, database);

            // Set connection properties.
            Properties properties = new Properties();
            properties.setProperty("user", user);
            properties.setProperty("password", password);
            properties.setProperty("useSSL", "true");
            properties.setProperty("verifyServerCertificate", "true");
            properties.setProperty("requireSSL", "false");

            // get connection
            connection = DriverManager.getConnection(url, properties);
        }
        catch (SQLException e)
        {
            throw new SQLException("Failed to create connection to database.", e);
        }
        if (connection != null) 
        { 
            System.out.println("Successfully created connection to database.");
        
            // Perform some SQL queries over the connection.
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
    
                // NOTE No need to commit all changes to database, as auto-commit is enabled by default.
    
            }
            catch (SQLException e)
            {
                throw new SQLException("Encountered an error when executing given sql statement.", e);
            }       
        }
        else {
            System.out.println("Failed to create connection to database.");
        }
        System.out.println("Execution finished.");
    }
}

```

## <a name="read-data"></a><span data-ttu-id="e5448-133">Adatok olvasása</span><span class="sxs-lookup"><span data-stu-id="e5448-133">Read data</span></span>
<span data-ttu-id="e5448-134">Az alábbi kód használatával végezheti el az adatok olvasását a **SELECT** SQL-utasítás segítségével.</span><span class="sxs-lookup"><span data-stu-id="e5448-134">Use the following code to read the data with a **SELECT** SQL statement.</span></span> <span data-ttu-id="e5448-135">A [getConnection()](https://dev.mysql.com/doc/connector-j/5.1/en/connector-j-usagenotes-connect-drivermanager.html) metódus a MySQL-hez való kapcsolódásra szolgál.</span><span class="sxs-lookup"><span data-stu-id="e5448-135">The [getConnection()](https://dev.mysql.com/doc/connector-j/5.1/en/connector-j-usagenotes-connect-drivermanager.html) method is used to connect to MySQL.</span></span> <span data-ttu-id="e5448-136">A [createStatement()](https://dev.mysql.com/doc/connector-j/5.1/en/connector-j-usagenotes-statements.html) és az executeQuery() metódusok csatlakoztatásra és a SELECT-utasítás futtatására szolgálnak.</span><span class="sxs-lookup"><span data-stu-id="e5448-136">The methods [createStatement()](https://dev.mysql.com/doc/connector-j/5.1/en/connector-j-usagenotes-statements.html) and executeQuery() are used to connect and run the select statement.</span></span> <span data-ttu-id="e5448-137">Az eredmények feldolgozása a [ResultSet](https://docs.oracle.com/javase/tutorial/jdbc/basics/retrieving.html) objektum használatával történik.</span><span class="sxs-lookup"><span data-stu-id="e5448-137">The results are processed using a [ResultSet](https://docs.oracle.com/javase/tutorial/jdbc/basics/retrieving.html) object.</span></span> 

<span data-ttu-id="e5448-138">Cserélje le a gazdagép, az adatbázis, a felhasználó és a jelszó paramétereit azokra az értékekre, amelyeket a saját kiszolgáló és adatbázis létrehozásakor adott meg.</span><span class="sxs-lookup"><span data-stu-id="e5448-138">Replace the host, database, user, and password parameters with the values that you specified when you created your own server and database.</span></span>

```java
import java.sql.*;
import java.util.Properties;

public class ReadTable {

    public static void main (String[] args)  throws Exception
    {
        // Initialize connection variables.
        String host = "myserver4demo.mysql.database.azure.com";
        String database = "quickstartdb";
        String user = "myadmin@myserver4demo";
        String password = "<server_admin_password>";

        // check that the driver is installed
        try
        {
            Class.forName("com.mysql.jdbc.Driver");
        }
        catch (ClassNotFoundException e)
        {
            throw new ClassNotFoundException("MySQL JDBC driver NOT detected in library path.", e);
        }

        System.out.println("MySQL JDBC driver detected in library path.");

        Connection connection = null;

        // Initialize connection object
        try
        {
            String url = String.format("jdbc:mysql://%s/%s", host, database);

            // Set connection properties.
            Properties properties = new Properties();
            properties.setProperty("user", user);
            properties.setProperty("password", password);
            properties.setProperty("useSSL", "true");
            properties.setProperty("verifyServerCertificate", "true");
            properties.setProperty("requireSSL", "false");
            
            // get connection
            connection = DriverManager.getConnection(url, properties);
        }
        catch (SQLException e)
        {
            throw new SQLException("Failed to create connection to database", e);
        }
        if (connection != null) 
        { 
            System.out.println("Successfully created connection to database.");
        
            // Perform some SQL queries over the connection.
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
                throw new SQLException("Encountered an error when executing given sql statement", e);
            }       
        }
        else {
            System.out.println("Failed to create connection to database."); 
        }
        System.out.println("Execution finished.");
    }
}
```

## <a name="update-data"></a><span data-ttu-id="e5448-139">Adatok frissítése</span><span class="sxs-lookup"><span data-stu-id="e5448-139">Update data</span></span>
<span data-ttu-id="e5448-140">Az alábbi kód használatával végezheti el az adatok módosítását az **UPDATE** SQL-utasítás segítségével.</span><span class="sxs-lookup"><span data-stu-id="e5448-140">Use the following code to change the data with an **UPDATE** SQL statement.</span></span> <span data-ttu-id="e5448-141">A [getConnection()](https://dev.mysql.com/doc/connector-j/5.1/en/connector-j-usagenotes-connect-drivermanager.html) metódus a MySQL-hez való kapcsolódásra szolgál.</span><span class="sxs-lookup"><span data-stu-id="e5448-141">The [getConnection()](https://dev.mysql.com/doc/connector-j/5.1/en/connector-j-usagenotes-connect-drivermanager.html) method is used to connect to MySQL.</span></span> <span data-ttu-id="e5448-142">A [prepareStatement()](http://docs.oracle.com/javase/tutorial/jdbc/basics/prepared.html) és az executeUpdate() metódusok előkészítésre, valamint az UPDATE-utasítás futtatására szolgálnak.</span><span class="sxs-lookup"><span data-stu-id="e5448-142">The methods [prepareStatement()](http://docs.oracle.com/javase/tutorial/jdbc/basics/prepared.html) and executeUpdate() are used to prepare and run the update statement.</span></span> 

<span data-ttu-id="e5448-143">Cserélje le a gazdagép, az adatbázis, a felhasználó és a jelszó paramétereit azokra az értékekre, amelyeket a saját kiszolgáló és adatbázis létrehozásakor adott meg.</span><span class="sxs-lookup"><span data-stu-id="e5448-143">Replace the host, database, user, and password parameters with the values that you specified when you created your own server and database.</span></span>

```java
import java.sql.*;
import java.util.Properties;

public class UpdateTable {
    public static void main (String[] args)  throws Exception
    {
        // Initialize connection variables. 
        String host = "myserver4demo.mysql.database.azure.com";
        String database = "quickstartdb";
        String user = "myadmin@myserver4demo";
        String password = "<server_admin_password>";

        // check that the driver is installed
        try
        {
            Class.forName("com.mysql.jdbc.Driver");
        }
        catch (ClassNotFoundException e)
        {
            throw new ClassNotFoundException("MySQL JDBC driver NOT detected in library path.", e);
        }
        System.out.println("MySQL JDBC driver detected in library path.");

        Connection connection = null;

        // Initialize connection object
        try
        {
            String url = String.format("jdbc:mysql://%s/%s", host, database);
            
            // set up the connection properties
            Properties properties = new Properties();
            properties.setProperty("user", user);
            properties.setProperty("password", password);
            properties.setProperty("useSSL", "true");
            properties.setProperty("verifyServerCertificate", "true");
            properties.setProperty("requireSSL", "false");

            // get connection
            connection = DriverManager.getConnection(url, properties);
        }
        catch (SQLException e)
        {
            throw new SQLException("Failed to create connection to database.", e);
        }
        if (connection != null) 
        { 
            System.out.println("Successfully created connection to database.");
        
            // Perform some SQL queries over the connection.
            try
            {
                // Modify some data in table.
                int nRowsUpdated = 0;
                PreparedStatement preparedStatement = connection.prepareStatement("UPDATE inventory SET quantity = ? WHERE name = ?;");
                preparedStatement.setInt(1, 200);
                preparedStatement.setString(2, "banana");
                nRowsUpdated += preparedStatement.executeUpdate();
                System.out.println(String.format("Updated %d row(s) of data.", nRowsUpdated));
    
                // NOTE No need to commit all changes to database, as auto-commit is enabled by default.
            }
            catch (SQLException e)
            {
                throw new SQLException("Encountered an error when executing given sql statement.", e);
            }       
        }
        else {
            System.out.println("Failed to create connection to database.");
        }
        System.out.println("Execution finished.");
    }
}
```

## <a name="delete-data"></a><span data-ttu-id="e5448-144">Adat törlése</span><span class="sxs-lookup"><span data-stu-id="e5448-144">Delete data</span></span>
<span data-ttu-id="e5448-145">Az alábbi kód használatával végezheti el az adatok eltávolítását a **DELETE** SQL-utasítás segítségével.</span><span class="sxs-lookup"><span data-stu-id="e5448-145">Use the following code to remove data with a **DELETE** SQL statement.</span></span> <span data-ttu-id="e5448-146">A [getConnection()](https://dev.mysql.com/doc/connector-j/5.1/en/connector-j-usagenotes-connect-drivermanager.html) metódus a MySQL-hez való kapcsolódásra szolgál.</span><span class="sxs-lookup"><span data-stu-id="e5448-146">The [getConnection()](https://dev.mysql.com/doc/connector-j/5.1/en/connector-j-usagenotes-connect-drivermanager.html) method is used to connect to MySQL.</span></span>  <span data-ttu-id="e5448-147">A [prepareStatement()](http://docs.oracle.com/javase/tutorial/jdbc/basics/prepared.html) és az executeUpdate() metódusok előkészítésre, valamint az UPDATE-utasítás futtatására szolgálnak.</span><span class="sxs-lookup"><span data-stu-id="e5448-147">The methods [prepareStatement()](http://docs.oracle.com/javase/tutorial/jdbc/basics/prepared.html) and executeUpdate() are used to prepare and run the update statement.</span></span> 

<span data-ttu-id="e5448-148">Cserélje le a gazdagép, az adatbázis, a felhasználó és a jelszó paramétereit azokra az értékekre, amelyeket a saját kiszolgáló és adatbázis létrehozásakor adott meg.</span><span class="sxs-lookup"><span data-stu-id="e5448-148">Replace the host, database, user, and password parameters with the values that you specified when you created your own server and database.</span></span>

```java
import java.sql.*;
import java.util.Properties;

public class DeleteTable {
    public static void main (String[] args)  throws Exception
    {
        // Initialize connection variables.
        String host = "myserver4demo.mysql.database.azure.com";
        String database = "quickstartdb";
        String user = "myadmin@myserver4demo";
        String password = "<server_admin_password>";
        
        // check that the driver is installed
        try
        {
            Class.forName("com.mysql.jdbc.Driver");
        }
        catch (ClassNotFoundException e)
        {
            throw new ClassNotFoundException("MySQL JDBC driver NOT detected in library path.", e);
        }

        System.out.println("MySQL JDBC driver detected in library path.");

        Connection connection = null;

        // Initialize connection object
        try
        {
            String url = String.format("jdbc:mysql://%s/%s", host, database);
            
            // set up the connection properties
            Properties properties = new Properties();
            properties.setProperty("user", user);
            properties.setProperty("password", password);
            properties.setProperty("useSSL", "true");
            properties.setProperty("verifyServerCertificate", "true");
            properties.setProperty("requireSSL", "false");
            
            // get connection
            connection = DriverManager.getConnection(url, properties);
        }
        catch (SQLException e)
        {
            throw new SQLException("Failed to create connection to database", e);
        }
        if (connection != null) 
        { 
            System.out.println("Successfully created connection to database.");
        
            // Perform some SQL queries over the connection.
            try
            {
                // Delete some data from table.
                int nRowsDeleted = 0;
                PreparedStatement preparedStatement = connection.prepareStatement("DELETE FROM inventory WHERE name = ?;");
                preparedStatement.setString(1, "orange");
                nRowsDeleted += preparedStatement.executeUpdate();
                System.out.println(String.format("Deleted %d row(s) of data.", nRowsDeleted));
    
                // NOTE No need to commit all changes to database, as auto-commit is enabled by default.
            }
            catch (SQLException e)
            {
                throw new SQLException("Encountered an error when executing given sql statement.", e);
            }       
        }
        else {
            System.out.println("Failed to create connection to database.");
        }
        System.out.println("Execution finished.");
    }
}
```

## <a name="next-steps"></a><span data-ttu-id="e5448-149">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e5448-149">Next steps</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="e5448-150">MySQL-adatbázis migrálása a MySQL-hez készült Azure Database-be memóriakép és visszaállítás használatával</span><span class="sxs-lookup"><span data-stu-id="e5448-150">Migrate your MySQL database to Azure Database for MySQL using dump and restore</span></span>](concepts-migrate-dump-restore.md)
