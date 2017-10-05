---
title: "Azure-adatbázis csatlakoztatni a MySQL a MySQL munkaterület |} Microsoft Docs"
description: "A gyors üzembe helyezés lépései MySQL munkaterület való kapcsolódás és lekérdezés az Azure-adatbázis adatait a MySQL használatára."
services: mysql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: seanli1988
ms.service: mysql-database
ms.custom: mvc
ms.topic: article
ms.date: 08/23/2017
ms.openlocfilehash: 20a1f31ce42abb924504c4008f85420fc49aec89
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="azure-database-for-mysql-use-mysql-workbench-to-connect-and-query-data"></a><span data-ttu-id="1a3a9-103">MySQL az Azure-adatbázishoz: MySQL-munkaterület használata kapcsolódási és lekérdezési adatok</span><span class="sxs-lookup"><span data-stu-id="1a3a9-103">Azure Database for MySQL: Use MySQL Workbench to connect and query data</span></span>
<span data-ttu-id="1a3a9-104">A gyors üzembe helyezés mutatja be a MySQL munkaterület alkalmazással MySQL egy Azure-adatbázishoz való kapcsolódáshoz.</span><span class="sxs-lookup"><span data-stu-id="1a3a9-104">This quickstart demonstrates how to connect to an Azure Database for MySQL using the MySQL Workbench application.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="1a3a9-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="1a3a9-105">Prerequisites</span></span>
<span data-ttu-id="1a3a9-106">Ebben a rövid útmutatóban a következő útmutatók valamelyikében létrehozott erőforrásokat használunk kiindulási pontként:</span><span class="sxs-lookup"><span data-stu-id="1a3a9-106">This quickstart uses the resources created in either of these guides as a starting point:</span></span>
- [<span data-ttu-id="1a3a9-107">Azure-adatbázis létrehozása MySQL-kiszolgálóhoz az Azure Portal használatával</span><span class="sxs-lookup"><span data-stu-id="1a3a9-107">Create an Azure Database for MySQL server using Azure portal</span></span>](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [<span data-ttu-id="1a3a9-108">Azure-adatbázis létrehozása MySQL-kiszolgálóhoz az Azure CLI használatával</span><span class="sxs-lookup"><span data-stu-id="1a3a9-108">Create an Azure Database for MySQL server using Azure CLI</span></span>](./quickstart-create-mysql-server-database-using-azure-cli.md)

## <a name="install-mysql-workbench"></a><span data-ttu-id="1a3a9-109">Telepítse a MySQL munkaterület</span><span class="sxs-lookup"><span data-stu-id="1a3a9-109">Install MySQL Workbench</span></span>
<span data-ttu-id="1a3a9-110">Töltse le és telepítse a MySQL munkaterület a számítógépen található [a MySQL webhely](https://dev.mysql.com/downloads/workbench/).</span><span class="sxs-lookup"><span data-stu-id="1a3a9-110">Download and install MySQL Workbench on your computer from [the MySQL website](https://dev.mysql.com/downloads/workbench/).</span></span>

## <a name="get-connection-information"></a><span data-ttu-id="1a3a9-111">Kapcsolatadatok lekérése</span><span class="sxs-lookup"><span data-stu-id="1a3a9-111">Get connection information</span></span>
<span data-ttu-id="1a3a9-112">Kérje le a MySQL-hez készült Azure Database-hez való csatlakozáshoz szükséges kapcsolatadatokat.</span><span class="sxs-lookup"><span data-stu-id="1a3a9-112">Get the connection information needed to connect to the Azure Database for MySQL.</span></span> <span data-ttu-id="1a3a9-113">Ehhez szükség lesz a teljes kiszolgálónévre és bejelentkezési hitelesítő adatokra.</span><span class="sxs-lookup"><span data-stu-id="1a3a9-113">You need the fully qualified server name and login credentials.</span></span>

1. <span data-ttu-id="1a3a9-114">Jelentkezzen be az [Azure portálra](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="1a3a9-114">Log in to the [Azure portal](https://portal.azure.com/).</span></span>

2. <span data-ttu-id="1a3a9-115">Az Azure Portal bal oldali menüjében kattintson az **Összes erőforrás** lehetőségre, és keressen rá a létrehozott kiszolgálóra, például **myserver4demo**.</span><span class="sxs-lookup"><span data-stu-id="1a3a9-115">From the left-hand menu in Azure portal, click **All resources** and search for the server you have created, such as **myserver4demo**.</span></span>

3. <span data-ttu-id="1a3a9-116">Kattintson a kiszolgálónévre.</span><span class="sxs-lookup"><span data-stu-id="1a3a9-116">Click the server name.</span></span>

4. <span data-ttu-id="1a3a9-117">Válassza a kiszolgáló **tulajdonságlapját**.</span><span class="sxs-lookup"><span data-stu-id="1a3a9-117">Select the server's **Properties** page.</span></span> <span data-ttu-id="1a3a9-118">Jegyezze fel a **Kiszolgálónevet** és a **Kiszolgáló-rendszergazdai bejelentkezési nevet**.</span><span class="sxs-lookup"><span data-stu-id="1a3a9-118">Make a note of the **Server name** and **Server admin login name**.</span></span>

 ![Azure-adatbázis MySQL kiszolgálónév](./media/connect-workbench/1-server-properties-name-login.png)
 
5. <span data-ttu-id="1a3a9-120">Amennyiben elfelejtette a kiszolgálója bejelentkezési adatait, lépjen az **Áttekintés** oldalra, ahol kikeresheti a kiszolgáló-rendszergazda bejelentkezési nevét, valamint szükség esetén új jelszót kérhet.</span><span class="sxs-lookup"><span data-stu-id="1a3a9-120">If you forget your server login information, navigate to the **Overview** page to view the Server admin login name and, if necessary, reset the password.</span></span>

## <a name="connect-to-the-server-using-mysql-workbench"></a><span data-ttu-id="1a3a9-121">Csatlakozzon a kiszolgálóhoz, MySQL munkaterület használatával</span><span class="sxs-lookup"><span data-stu-id="1a3a9-121">Connect to the server using MySQL Workbench</span></span> 
<span data-ttu-id="1a3a9-122">Kapcsolódás az Azure MySQL-kiszolgálóhoz a MySQL Workbench GUI eszköz használatával:</span><span class="sxs-lookup"><span data-stu-id="1a3a9-122">To connect to Azure MySQL server using the GUI tool MySQL Workbench:</span></span>

1.  <span data-ttu-id="1a3a9-123">A MySQL munkaterület lkalmazás a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="1a3a9-123">Launch the MySQL Workbench application on your computer.</span></span> 

2.  <span data-ttu-id="1a3a9-124">A **új kapcsolat beállítása** párbeszédpanelen adja meg a következő adatokat a **paraméterek** lapon:</span><span class="sxs-lookup"><span data-stu-id="1a3a9-124">In **Setup New Connection** dialog box, enter the following information on the **Parameters** tab:</span></span>

    ![új kapcsolat beállítása](./media/connect-workbench/2-setup-new-connection.png)

    | <span data-ttu-id="1a3a9-126">**Beállítás**</span><span class="sxs-lookup"><span data-stu-id="1a3a9-126">**Setting**</span></span> | <span data-ttu-id="1a3a9-127">**Ajánlott érték**</span><span class="sxs-lookup"><span data-stu-id="1a3a9-127">**Suggested value**</span></span> | <span data-ttu-id="1a3a9-128">**Mező leírása**</span><span class="sxs-lookup"><span data-stu-id="1a3a9-128">**Field description**</span></span> |
    |---|---|---|
    |   <span data-ttu-id="1a3a9-129">Kapcsolat neve</span><span class="sxs-lookup"><span data-stu-id="1a3a9-129">Connection Name</span></span> | <span data-ttu-id="1a3a9-130">Bemutató kapcsolat</span><span class="sxs-lookup"><span data-stu-id="1a3a9-130">Demo Connection</span></span> | <span data-ttu-id="1a3a9-131">Adjon meg egy címkét a kapcsolathoz.</span><span class="sxs-lookup"><span data-stu-id="1a3a9-131">Specify a label for this connection.</span></span> |
    | <span data-ttu-id="1a3a9-132">Kapcsolati módszer</span><span class="sxs-lookup"><span data-stu-id="1a3a9-132">Connection Method</span></span> | <span data-ttu-id="1a3a9-133">Standard (TCP/IP)</span><span class="sxs-lookup"><span data-stu-id="1a3a9-133">Standard (TCP/IP)</span></span> | <span data-ttu-id="1a3a9-134">A Standard (TCP/IP) elégséges.</span><span class="sxs-lookup"><span data-stu-id="1a3a9-134">Standard (TCP/IP) is sufficient.</span></span> |
    | <span data-ttu-id="1a3a9-135">Gazdanév</span><span class="sxs-lookup"><span data-stu-id="1a3a9-135">Hostname</span></span> | <span data-ttu-id="1a3a9-136">*kiszolgáló neve*</span><span class="sxs-lookup"><span data-stu-id="1a3a9-136">*server name*</span></span> | <span data-ttu-id="1a3a9-137">Adja meg azt a kiszolgálónevet, amelyet korábban a MySQL-hez készült Azure-adatbázis létrehozásakor használt.</span><span class="sxs-lookup"><span data-stu-id="1a3a9-137">Specify the server name value that was used when you created the Azure Database for MySQL earlier.</span></span> <span data-ttu-id="1a3a9-138">Az itt látható példakiszolgáló a myserver4demo.mysql.database.azure.com. Használja a teljes tartománynevet (\*.mysql.database.azure.com), ahogyan az a példában látható.</span><span class="sxs-lookup"><span data-stu-id="1a3a9-138">Our example server shown is myserver4demo.mysql.database.azure.com. Use the fully qualified domain name (\*.mysql.database.azure.com) as shown in the example.</span></span> <span data-ttu-id="1a3a9-139">Ha nem emlékszik a kiszolgáló nevére, a kapcsolati adatok lekéréséhez kövesse az előző szakasz lépéseit.</span><span class="sxs-lookup"><span data-stu-id="1a3a9-139">Follow the steps in the previous section to get the connection information if you do not remember your server name.</span></span>  |
    | <span data-ttu-id="1a3a9-140">Port</span><span class="sxs-lookup"><span data-stu-id="1a3a9-140">Port</span></span> | <span data-ttu-id="1a3a9-141">3306</span><span class="sxs-lookup"><span data-stu-id="1a3a9-141">3306</span></span> | <span data-ttu-id="1a3a9-142">A MySQL-hez készült Azure-adatbázishoz való csatlakozáskor mindig a 3306-os portot használja.</span><span class="sxs-lookup"><span data-stu-id="1a3a9-142">Always use port 3306 when connecting to Azure Database for MySQL.</span></span> |
    | <span data-ttu-id="1a3a9-143">Felhasználónév</span><span class="sxs-lookup"><span data-stu-id="1a3a9-143">Username</span></span> |  <span data-ttu-id="1a3a9-144">*kiszolgáló-rendszergazdai bejelentkezési név*</span><span class="sxs-lookup"><span data-stu-id="1a3a9-144">*server admin login name*</span></span> | <span data-ttu-id="1a3a9-145">Írja be a kiszolgáló-rendszergazdai bejelentkezési felhasználónevet, amelyet korábban a MySQL-hez készült Azure-adatbázis létrehozásakor adott meg.</span><span class="sxs-lookup"><span data-stu-id="1a3a9-145">Type in the server admin login username supplied when you created the Azure Database for MySQL earlier.</span></span> <span data-ttu-id="1a3a9-146">A példában szereplő felhasználónév a következő: myadmin@myserver4demo.</span><span class="sxs-lookup"><span data-stu-id="1a3a9-146">Our example username is myadmin@myserver4demo.</span></span> <span data-ttu-id="1a3a9-147">Ha nem emlékszik a felhasználónévre, a kapcsolati adatok lekéréséhez kövesse az előző szakasz lépéseit.</span><span class="sxs-lookup"><span data-stu-id="1a3a9-147">Follow the steps in the previous section to get the connection information if you do not remember the username.</span></span> <span data-ttu-id="1a3a9-148">A formátum *username@servername*.</span><span class="sxs-lookup"><span data-stu-id="1a3a9-148">The format is *username@servername*.</span></span>
    | <span data-ttu-id="1a3a9-149">Jelszó</span><span class="sxs-lookup"><span data-stu-id="1a3a9-149">Password</span></span> | <span data-ttu-id="1a3a9-150">az ön jelszava</span><span class="sxs-lookup"><span data-stu-id="1a3a9-150">your password</span></span> | <span data-ttu-id="1a3a9-151">Kattintson a **Store tárolóban...**  gombra kattintva mentse a jelszót.</span><span class="sxs-lookup"><span data-stu-id="1a3a9-151">Click **Store in Vault...** button to save the password.</span></span> |

3.   <span data-ttu-id="1a3a9-152">Kattintson a **Kapcsolat tesztelése** lehetőségre, hogy tesztelje, minden paraméter helyesen lett-e konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="1a3a9-152">Click **Test Connection** to test if all parameters are correctly configured.</span></span> 

4.   <span data-ttu-id="1a3a9-153">Kattintson a **OK** a kapcsolat mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="1a3a9-153">Click **OK** to save the connection.</span></span> 

5.   <span data-ttu-id="1a3a9-154">A lista tartalmazza a **MySQL kapcsolatok**, kattintson a csempére a kiszolgáló megfelelő, és várja meg a-kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="1a3a9-154">In the listing of **MySQL Connections**, click the tile corresponding to your server and wait for the connection to be established.</span></span>

6.   <span data-ttu-id="1a3a9-155">Új SQL lapon nyílik meg egy üres szerkesztő, ahová beírhatja a lekérdezéseket.</span><span class="sxs-lookup"><span data-stu-id="1a3a9-155">A new SQL tab opens with a blank editor where you can type your queries.</span></span>

    > [!NOTE]
    > <span data-ttu-id="1a3a9-156">Alapértelmezés szerint az SSL-kapcsolat biztonsági szükséges, és a MySQL-kiszolgálóhoz tartozó Azure-adatbázis érvényes.</span><span class="sxs-lookup"><span data-stu-id="1a3a9-156">By default, SSL connection security is required and enforced on your Azure Database for MySQL server.</span></span> <span data-ttu-id="1a3a9-157">Általában nem SSL-tanúsítványokkal nincs szükség további konfigurációra a MySQL-munkaterület a kiszolgálóhoz való csatlakozás.</span><span class="sxs-lookup"><span data-stu-id="1a3a9-157">Typically no additional configuration with SSL certificates is required for MySQL Workbench to connect to your server.</span></span> <span data-ttu-id="1a3a9-158">Az SSL további információkért lásd: [konfigurálása az SSL-kapcsolatot az alkalmazásokhoz való biztonságos kapcsolódás Azure-adatbázis a MySQL](./howto-configure-ssl.md).</span><span class="sxs-lookup"><span data-stu-id="1a3a9-158">For more information on SSL, see [Configure SSL connectivity in your application to securely connect to Azure Database for MySQL](./howto-configure-ssl.md).</span></span>  <span data-ttu-id="1a3a9-159">Ha le kell tiltania az SSL, látogasson el az Azure-portálon, és a kapcsolat biztonsági lap a kényszerítéséhez SSL-kapcsolat váltógomb letiltásához kattintson.</span><span class="sxs-lookup"><span data-stu-id="1a3a9-159">If you need to disable SSL, visit the Azure portal and click the Connection security page to disable the Enforce SSL connection toggle button.</span></span>

## <a name="create-a-table-insert-data-read-data-update-data-delete-data"></a><span data-ttu-id="1a3a9-160">Hozzon létre egy táblát, adatokat beszúrni, olvassa el az adatok, adatainak frissítése, adatok törlése</span><span class="sxs-lookup"><span data-stu-id="1a3a9-160">Create a table, insert data, read data, update data, delete data</span></span>
1. <span data-ttu-id="1a3a9-161">Másolja, és a mintakódot az SQL mutatja be a mintaadatokat egy üres SQL fülre.</span><span class="sxs-lookup"><span data-stu-id="1a3a9-161">Copy and paste the sample SQL code into a blank SQL tab to illustrate some sample data.</span></span>

    <span data-ttu-id="1a3a9-162">Ez a kód egy quickstartdb nevű üres adatbázist hoz létre, és majd a készlet nevű minta táblázat létrehozása.</span><span class="sxs-lookup"><span data-stu-id="1a3a9-162">This code creates an empty database named quickstartdb, and then creates a sample table named inventory.</span></span> <span data-ttu-id="1a3a9-163">Ezután néhány sor beilleszti beolvassa a sorokat.</span><span class="sxs-lookup"><span data-stu-id="1a3a9-163">It inserts some rows, then reads the rows.</span></span> <span data-ttu-id="1a3a9-164">Módosítja az adatokat az update utasításban a, és olvassa be újra a sorokat.</span><span class="sxs-lookup"><span data-stu-id="1a3a9-164">It changes the data with an update statement, and reads the rows again.</span></span> <span data-ttu-id="1a3a9-165">Végül azt töröl egy sort, és olvassa be újra a sorokat.</span><span class="sxs-lookup"><span data-stu-id="1a3a9-165">Finally it deletes a row, and reads the rows again.</span></span>
    
    ```sql
    -- Create a database
    -- DROP DATABASE IF EXISTS quickstartdb;
    CREATE DATABASE quickstartdb;
    USE quickstartdb;
    
    -- Create a table and insert rows
    DROP TABLE IF EXISTS inventory;
    CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);
    INSERT INTO inventory (name, quantity) VALUES ('banana', 150);
    INSERT INTO inventory (name, quantity) VALUES ('orange', 154);
    INSERT INTO inventory (name, quantity) VALUES ('apple', 100);
    
    -- Read
    SELECT * FROM inventory;
    
    -- Update
    UPDATE inventory SET quantity = 200 WHERE id = 1;
    SELECT * FROM inventory;
    
    -- Delete
    DELETE FROM inventory WHERE id = 2;
    SELECT * FROM inventory;
    ```

    <span data-ttu-id="1a3a9-166">A képernyőfelvételen látható az SQL-kódot SQL munkaterület és a kimeneti után futott.</span><span class="sxs-lookup"><span data-stu-id="1a3a9-166">The screenshot shows an example of the SQL code in SQL Workbench and the output after it has been run.</span></span>
    
    ![MySQL munkaterület SQL lapon SQL példakód futtatása](media/connect-workbench/3-workbench-sql-tab.png)

2. <span data-ttu-id="1a3a9-168">Mintát szeretné futtatni az SQL-kódot, kattintson az eszköztár lightening bolt ikonra a **SQL fájl** fülre.</span><span class="sxs-lookup"><span data-stu-id="1a3a9-168">To run the sample SQL Code, click the lightening bolt icon in the toolbar of the **SQL File** tab.</span></span>
3. <span data-ttu-id="1a3a9-169">Figyelje meg, a három lapokra eredményez a **eredmény rács** szakasz az oldal közepén.</span><span class="sxs-lookup"><span data-stu-id="1a3a9-169">Notice the three tabbed results in the **Result Grid** section in the middle of the page.</span></span> 
4. <span data-ttu-id="1a3a9-170">Figyelje meg a **kimeneti** lista az oldal alján.</span><span class="sxs-lookup"><span data-stu-id="1a3a9-170">Notice the **Output** list at the bottom of the page.</span></span> <span data-ttu-id="1a3a9-171">Minden parancs állapota látható.</span><span class="sxs-lookup"><span data-stu-id="1a3a9-171">The status of each command is shown.</span></span> 

<span data-ttu-id="1a3a9-172">Most hogy MySQL MySQL munkaterület használata az Azure-adatbázis csatlakozott, és lekérdezte az adatokat az SQL-nyelv használatával.</span><span class="sxs-lookup"><span data-stu-id="1a3a9-172">Now, you have connected to Azure Database for MySQL using MySQL Workbench, and have queried data using the SQL language.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1a3a9-173">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1a3a9-173">Next steps</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="1a3a9-174">Adatbázis migrálása exportálással és importálással</span><span class="sxs-lookup"><span data-stu-id="1a3a9-174">Migrate your database using Export and Import</span></span>](./concepts-migrate-import-export.md)
