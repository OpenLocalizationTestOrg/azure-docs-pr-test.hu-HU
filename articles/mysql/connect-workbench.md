---
title: "A MySQL munkaterület MySQL-adatbázis tooAzure kapcsolati |} Microsoft Docs"
description: "A gyors üzembe helyezés biztosít hello lépéseket toouse MySQL munkaterület Azure-adatbázis tooconnect és lekérdezés adatait MySQL."
services: mysql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: seanli1988
ms.service: mysql-database
ms.custom: mvc
ms.topic: article
ms.date: 08/23/2017
ms.openlocfilehash: c64fcb9bb99ba06aa3a95eec420d5d5ef4a31d14
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-database-for-mysql-use-mysql-workbench-tooconnect-and-query-data"></a><span data-ttu-id="a6737-103">MySQL az Azure-adatbázishoz: használata MySQL munkaterület tooconnect és lekérdezési adatok</span><span class="sxs-lookup"><span data-stu-id="a6737-103">Azure Database for MySQL: Use MySQL Workbench tooconnect and query data</span></span>
<span data-ttu-id="a6737-104">A gyors üzembe helyezés bemutatja, hogyan tooconnect tooan Azure adatbázis MySQL használatára vonatkozó hello MySQL munkaterület alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="a6737-104">This quickstart demonstrates how tooconnect tooan Azure Database for MySQL using hello MySQL Workbench application.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="a6737-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="a6737-105">Prerequisites</span></span>
<span data-ttu-id="a6737-106">A gyors üzembe helyezés kiindulási pontként ezek az útmutatók valamelyikével létrehozott hello erőforrást használ:</span><span class="sxs-lookup"><span data-stu-id="a6737-106">This quickstart uses hello resources created in either of these guides as a starting point:</span></span>
- [<span data-ttu-id="a6737-107">Azure-adatbázis létrehozása MySQL-kiszolgálóhoz az Azure Portal használatával</span><span class="sxs-lookup"><span data-stu-id="a6737-107">Create an Azure Database for MySQL server using Azure portal</span></span>](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [<span data-ttu-id="a6737-108">Azure-adatbázis létrehozása MySQL-kiszolgálóhoz az Azure CLI használatával</span><span class="sxs-lookup"><span data-stu-id="a6737-108">Create an Azure Database for MySQL server using Azure CLI</span></span>](./quickstart-create-mysql-server-database-using-azure-cli.md)

## <a name="install-mysql-workbench"></a><span data-ttu-id="a6737-109">Telepítse a MySQL munkaterület</span><span class="sxs-lookup"><span data-stu-id="a6737-109">Install MySQL Workbench</span></span>
<span data-ttu-id="a6737-110">Töltse le és telepítse a MySQL munkaterület a számítógépen található [hello MySQL webhely](https://dev.mysql.com/downloads/workbench/).</span><span class="sxs-lookup"><span data-stu-id="a6737-110">Download and install MySQL Workbench on your computer from [hello MySQL website](https://dev.mysql.com/downloads/workbench/).</span></span>

## <a name="get-connection-information"></a><span data-ttu-id="a6737-111">Kapcsolatadatok lekérése</span><span class="sxs-lookup"><span data-stu-id="a6737-111">Get connection information</span></span>
<span data-ttu-id="a6737-112">MySQL hello kapcsolat szükséges információkat tooconnect toohello Azure adatbázis beolvasása.</span><span class="sxs-lookup"><span data-stu-id="a6737-112">Get hello connection information needed tooconnect toohello Azure Database for MySQL.</span></span> <span data-ttu-id="a6737-113">Teljesen minősített kiszolgáló nevét és a bejelentkezési hitelesítő adatokat hello van szüksége.</span><span class="sxs-lookup"><span data-stu-id="a6737-113">You need hello fully qualified server name and login credentials.</span></span>

1. <span data-ttu-id="a6737-114">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="a6737-114">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>

2. <span data-ttu-id="a6737-115">A hello Azure-portálon a bal oldali menüből, kattintson az **összes erőforrás** , és keressen a létrehozott, például a hello server **myserver4demo**.</span><span class="sxs-lookup"><span data-stu-id="a6737-115">From hello left-hand menu in Azure portal, click **All resources** and search for hello server you have created, such as **myserver4demo**.</span></span>

3. <span data-ttu-id="a6737-116">Hello kiszolgáló nevére kattint.</span><span class="sxs-lookup"><span data-stu-id="a6737-116">Click hello server name.</span></span>

4. <span data-ttu-id="a6737-117">Jelölje be hello server **tulajdonságok** lap.</span><span class="sxs-lookup"><span data-stu-id="a6737-117">Select hello server's **Properties** page.</span></span> <span data-ttu-id="a6737-118">Jegyezze fel a hello **kiszolgálónév** és **kiszolgálói rendszergazda bejelentkezési név**.</span><span class="sxs-lookup"><span data-stu-id="a6737-118">Make a note of hello **Server name** and **Server admin login name**.</span></span>

 ![Azure-adatbázis MySQL kiszolgálónév](./media/connect-workbench/1-server-properties-name-login.png)
 
5. <span data-ttu-id="a6737-120">Ha elfelejti a kiszolgálói bejelentkezési adatok, keresse meg a toohello **áttekintése** tooview hello kiszolgálói rendszergazda bejelentkezési név lapon, és ha szükséges, állítsa vissza a hello jelszót.</span><span class="sxs-lookup"><span data-stu-id="a6737-120">If you forget your server login information, navigate toohello **Overview** page tooview hello Server admin login name and, if necessary, reset hello password.</span></span>

## <a name="connect-toohello-server-using-mysql-workbench"></a><span data-ttu-id="a6737-121">Csatlakoztassa a kiszolgálót MySQL munkaterület használatával toohello</span><span class="sxs-lookup"><span data-stu-id="a6737-121">Connect toohello server using MySQL Workbench</span></span> 
<span data-ttu-id="a6737-122">tooconnect tooAzure MySQL kiszolgáló grafikus felhasználói Felülettel hello eszközzel MySQL-munkaterületet:</span><span class="sxs-lookup"><span data-stu-id="a6737-122">tooconnect tooAzure MySQL server using hello GUI tool MySQL Workbench:</span></span>

1.  <span data-ttu-id="a6737-123">Indítsa el a hello MySQL munkaterület alkalmazás a számítógépre.</span><span class="sxs-lookup"><span data-stu-id="a6737-123">Launch hello MySQL Workbench application on your computer.</span></span> 

2.  <span data-ttu-id="a6737-124">A **új kapcsolat beállítása** párbeszédpanelen adja meg a következő információ a hello hello **paraméterek** lapon:</span><span class="sxs-lookup"><span data-stu-id="a6737-124">In **Setup New Connection** dialog box, enter hello following information on hello **Parameters** tab:</span></span>

    ![új kapcsolat beállítása](./media/connect-workbench/2-setup-new-connection.png)

    | <span data-ttu-id="a6737-126">**Beállítás**</span><span class="sxs-lookup"><span data-stu-id="a6737-126">**Setting**</span></span> | <span data-ttu-id="a6737-127">**Ajánlott érték**</span><span class="sxs-lookup"><span data-stu-id="a6737-127">**Suggested value**</span></span> | <span data-ttu-id="a6737-128">**Mező leírása**</span><span class="sxs-lookup"><span data-stu-id="a6737-128">**Field description**</span></span> |
    |---|---|---|
    |   <span data-ttu-id="a6737-129">Kapcsolat neve</span><span class="sxs-lookup"><span data-stu-id="a6737-129">Connection Name</span></span> | <span data-ttu-id="a6737-130">Bemutató kapcsolat</span><span class="sxs-lookup"><span data-stu-id="a6737-130">Demo Connection</span></span> | <span data-ttu-id="a6737-131">Adjon meg egy címkét a kapcsolathoz.</span><span class="sxs-lookup"><span data-stu-id="a6737-131">Specify a label for this connection.</span></span> |
    | <span data-ttu-id="a6737-132">Kapcsolati módszer</span><span class="sxs-lookup"><span data-stu-id="a6737-132">Connection Method</span></span> | <span data-ttu-id="a6737-133">Standard (TCP/IP)</span><span class="sxs-lookup"><span data-stu-id="a6737-133">Standard (TCP/IP)</span></span> | <span data-ttu-id="a6737-134">A Standard (TCP/IP) elégséges.</span><span class="sxs-lookup"><span data-stu-id="a6737-134">Standard (TCP/IP) is sufficient.</span></span> |
    | <span data-ttu-id="a6737-135">Gazdanév</span><span class="sxs-lookup"><span data-stu-id="a6737-135">Hostname</span></span> | <span data-ttu-id="a6737-136">*kiszolgáló neve*</span><span class="sxs-lookup"><span data-stu-id="a6737-136">*server name*</span></span> | <span data-ttu-id="a6737-137">Adja meg a hello kiszolgálónév hello Azure adatbázis MySQL a korábban létrehozott használt.</span><span class="sxs-lookup"><span data-stu-id="a6737-137">Specify hello server name value that was used when you created hello Azure Database for MySQL earlier.</span></span> <span data-ttu-id="a6737-138">Az itt látható példakiszolgáló a myserver4demo.mysql.database.azure.com. Hello teljesen minősített tartománynevét használja (\*. mysql.database.azure.com) hello példában látható módon.</span><span class="sxs-lookup"><span data-stu-id="a6737-138">Our example server shown is myserver4demo.mysql.database.azure.com. Use hello fully qualified domain name (\*.mysql.database.azure.com) as shown in hello example.</span></span> <span data-ttu-id="a6737-139">Kövesse hello hello előző szakasz tooget hello kapcsolati adatokat, ha nem emlékszik a kiszolgáló nevét.</span><span class="sxs-lookup"><span data-stu-id="a6737-139">Follow hello steps in hello previous section tooget hello connection information if you do not remember your server name.</span></span>  |
    | <span data-ttu-id="a6737-140">Port</span><span class="sxs-lookup"><span data-stu-id="a6737-140">Port</span></span> | <span data-ttu-id="a6737-141">3306</span><span class="sxs-lookup"><span data-stu-id="a6737-141">3306</span></span> | <span data-ttu-id="a6737-142">Mindig használjon port 3306 MySQL adatbázis tooAzure kapcsolódáskor.</span><span class="sxs-lookup"><span data-stu-id="a6737-142">Always use port 3306 when connecting tooAzure Database for MySQL.</span></span> |
    | <span data-ttu-id="a6737-143">Felhasználónév</span><span class="sxs-lookup"><span data-stu-id="a6737-143">Username</span></span> |  <span data-ttu-id="a6737-144">*kiszolgáló-rendszergazdai bejelentkezési név*</span><span class="sxs-lookup"><span data-stu-id="a6737-144">*server admin login name*</span></span> | <span data-ttu-id="a6737-145">Írja be a hello server admin bejelentkezési felhasználónevének megadni, ha az Azure-adatbázis hello MySQL a korábban létrehozott.</span><span class="sxs-lookup"><span data-stu-id="a6737-145">Type in hello server admin login username supplied when you created hello Azure Database for MySQL earlier.</span></span> <span data-ttu-id="a6737-146">A példában szereplő felhasználónév a következő: myadmin@myserver4demo.</span><span class="sxs-lookup"><span data-stu-id="a6737-146">Our example username is myadmin@myserver4demo.</span></span> <span data-ttu-id="a6737-147">Ha nem emlékszik hello felhasználónév, kövesse a hello előző szakasz tooget hello kapcsolatadatok hello lépéseit.</span><span class="sxs-lookup"><span data-stu-id="a6737-147">Follow hello steps in hello previous section tooget hello connection information if you do not remember hello username.</span></span> <span data-ttu-id="a6737-148">hello formátuma  *username@servername* .</span><span class="sxs-lookup"><span data-stu-id="a6737-148">hello format is *username@servername*.</span></span>
    | <span data-ttu-id="a6737-149">Jelszó</span><span class="sxs-lookup"><span data-stu-id="a6737-149">Password</span></span> | <span data-ttu-id="a6737-150">az ön jelszava</span><span class="sxs-lookup"><span data-stu-id="a6737-150">your password</span></span> | <span data-ttu-id="a6737-151">Kattintson a **Store tárolóban...**  gomb toosave hello jelszót.</span><span class="sxs-lookup"><span data-stu-id="a6737-151">Click **Store in Vault...** button toosave hello password.</span></span> |

3.   <span data-ttu-id="a6737-152">Kattintson a **kapcsolat tesztelése** tootest, ha az összes paraméter megfelelően vannak konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="a6737-152">Click **Test Connection** tootest if all parameters are correctly configured.</span></span> 

4.   <span data-ttu-id="a6737-153">Kattintson a **OK** toosave hello kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="a6737-153">Click **OK** toosave hello connection.</span></span> 

5.   <span data-ttu-id="a6737-154">A hello listája **MySQL kapcsolatok**, hello megfelelő tooyour mozaikkiszolgálóról kattintson, majd várja meg a létrehozott hello kapcsolat toobe.</span><span class="sxs-lookup"><span data-stu-id="a6737-154">In hello listing of **MySQL Connections**, click hello tile corresponding tooyour server and wait for hello connection toobe established.</span></span>

6.   <span data-ttu-id="a6737-155">Új SQL lapon nyílik meg egy üres szerkesztő, ahová beírhatja a lekérdezéseket.</span><span class="sxs-lookup"><span data-stu-id="a6737-155">A new SQL tab opens with a blank editor where you can type your queries.</span></span>

    > [!NOTE]
    > <span data-ttu-id="a6737-156">Alapértelmezés szerint az SSL-kapcsolat biztonsági szükséges, és a MySQL-kiszolgálóhoz tartozó Azure-adatbázis érvényes.</span><span class="sxs-lookup"><span data-stu-id="a6737-156">By default, SSL connection security is required and enforced on your Azure Database for MySQL server.</span></span> <span data-ttu-id="a6737-157">Általában nem SSL-tanúsítványokkal nincs szükség további konfigurációra MySQL munkaterület tooconnect tooyour kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="a6737-157">Typically no additional configuration with SSL certificates is required for MySQL Workbench tooconnect tooyour server.</span></span> <span data-ttu-id="a6737-158">Az SSL további információkért lásd: [konfigurálása az SSL-kapcsolat az alkalmazás toosecurely a MySQL adatbázis tooAzure kapcsolati](./howto-configure-ssl.md).</span><span class="sxs-lookup"><span data-stu-id="a6737-158">For more information on SSL, see [Configure SSL connectivity in your application toosecurely connect tooAzure Database for MySQL](./howto-configure-ssl.md).</span></span>  <span data-ttu-id="a6737-159">Ha toodisable SSL van szüksége, látogasson el a hello Azure-portálon, és kattintson hello kapcsolat biztonsági lap toodisable hello kényszerítése SSL kapcsolat váltása gombra.</span><span class="sxs-lookup"><span data-stu-id="a6737-159">If you need toodisable SSL, visit hello Azure portal and click hello Connection security page toodisable hello Enforce SSL connection toggle button.</span></span>

## <a name="create-a-table-insert-data-read-data-update-data-delete-data"></a><span data-ttu-id="a6737-160">Hozzon létre egy táblát, adatokat beszúrni, olvassa el az adatok, adatainak frissítése, adatok törlése</span><span class="sxs-lookup"><span data-stu-id="a6737-160">Create a table, insert data, read data, update data, delete data</span></span>
1. <span data-ttu-id="a6737-161">Másolja és mintakódot hello SQL egy üres SQL lapon tooillustrate néhány adatot.</span><span class="sxs-lookup"><span data-stu-id="a6737-161">Copy and paste hello sample SQL code into a blank SQL tab tooillustrate some sample data.</span></span>

    <span data-ttu-id="a6737-162">Ez a kód egy quickstartdb nevű üres adatbázist hoz létre, és majd a készlet nevű minta táblázat létrehozása.</span><span class="sxs-lookup"><span data-stu-id="a6737-162">This code creates an empty database named quickstartdb, and then creates a sample table named inventory.</span></span> <span data-ttu-id="a6737-163">Néhány sor beszúrása, majd hello sorok beolvasása.</span><span class="sxs-lookup"><span data-stu-id="a6737-163">It inserts some rows, then reads hello rows.</span></span> <span data-ttu-id="a6737-164">Módosítja az update utasításban hello adatok, és olvasási műveletek hello sorok újra.</span><span class="sxs-lookup"><span data-stu-id="a6737-164">It changes hello data with an update statement, and reads hello rows again.</span></span> <span data-ttu-id="a6737-165">Végül azt töröl egy sort, és olvassa be újra hello sorokat.</span><span class="sxs-lookup"><span data-stu-id="a6737-165">Finally it deletes a row, and reads hello rows again.</span></span>
    
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

    <span data-ttu-id="a6737-166">képernyőfelvétel a hello szemléltet hello SQL-kódot SQL munkaterület és hello kimenet után futott.</span><span class="sxs-lookup"><span data-stu-id="a6737-166">hello screenshot shows an example of hello SQL code in SQL Workbench and hello output after it has been run.</span></span>
    
    ![MySQL munkaterület SQL lapon toorun SQL mintakód](media/connect-workbench/3-workbench-sql-tab.png)

2. <span data-ttu-id="a6737-168">toorun hello minta SQL-kódot, kattintson a hello hello eszköztárán bolt ikonra könnyítve hello **SQL fájl** fülre.</span><span class="sxs-lookup"><span data-stu-id="a6737-168">toorun hello sample SQL Code, click hello lightening bolt icon in hello toolbar of hello **SQL File** tab.</span></span>
3. <span data-ttu-id="a6737-169">Figyelje meg, hello három lapokra eredményez hello **eredmény rács** hello lap hello középső részében.</span><span class="sxs-lookup"><span data-stu-id="a6737-169">Notice hello three tabbed results in hello **Result Grid** section in hello middle of hello page.</span></span> 
4. <span data-ttu-id="a6737-170">Értesítés hello **kimeneti** lista alján hello hello.</span><span class="sxs-lookup"><span data-stu-id="a6737-170">Notice hello **Output** list at hello bottom of hello page.</span></span> <span data-ttu-id="a6737-171">az egyes parancsok hello állapota jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="a6737-171">hello status of each command is shown.</span></span> 

<span data-ttu-id="a6737-172">Most akkor tooAzure adatbázis csatlakozott a MySQL MySQL munkaterület használatával, és felhasználói adatok hello SQL-nyelv használatával.</span><span class="sxs-lookup"><span data-stu-id="a6737-172">Now, you have connected tooAzure Database for MySQL using MySQL Workbench, and have queried data using hello SQL language.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a6737-173">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a6737-173">Next steps</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="a6737-174">Adatbázis migrálása exportálással és importálással</span><span class="sxs-lookup"><span data-stu-id="a6737-174">Migrate your database using Export and Import</span></span>](./concepts-migrate-import-export.md)
