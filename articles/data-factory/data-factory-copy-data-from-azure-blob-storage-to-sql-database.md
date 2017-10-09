---
title: "a Blob Storage tooSQL adatbázis - Azure aaaCopy adatait |} Microsoft Docs"
description: "Az oktatóanyag bemutatja, hogyan toouse másolási tevékenység során az Azure Data Factory a csővezeték-e a Blob storage tooSQL adatbázis toocopy adatait."
keywords: "a BLOB sql, a blob storage adatok másolása"
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: e4035060-93bf-4e8d-bf35-35e2d15c51e0
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/19/2017
ms.author: spelluru
ms.openlocfilehash: a2c3fb8a4ddd63b0b6b3e75903b7a7eaf188fda4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-copy-data-from-blob-storage-toosql-database-using-data-factory"></a><span data-ttu-id="6e942-104">Oktatóanyag: Adatok másolása az Blob Storage tooSQL adatbázist Data Factory használatával</span><span class="sxs-lookup"><span data-stu-id="6e942-104">Tutorial: Copy data from Blob Storage tooSQL Database using Data Factory</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="6e942-105">Áttekintés és előfeltételek</span><span class="sxs-lookup"><span data-stu-id="6e942-105">Overview and prerequisites</span></span>](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [<span data-ttu-id="6e942-106">Másolás varázsló</span><span class="sxs-lookup"><span data-stu-id="6e942-106">Copy Wizard</span></span>](data-factory-copy-data-wizard-tutorial.md)
> * [<span data-ttu-id="6e942-107">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="6e942-107">Azure portal</span></span>](data-factory-copy-activity-tutorial-using-azure-portal.md)
> * [<span data-ttu-id="6e942-108">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6e942-108">Visual Studio</span></span>](data-factory-copy-activity-tutorial-using-visual-studio.md)
> * [<span data-ttu-id="6e942-109">PowerShell</span><span class="sxs-lookup"><span data-stu-id="6e942-109">PowerShell</span></span>](data-factory-copy-activity-tutorial-using-powershell.md)
> * [<span data-ttu-id="6e942-110">Azure Resource Manager-sablon</span><span class="sxs-lookup"><span data-stu-id="6e942-110">Azure Resource Manager template</span></span>](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
> * [<span data-ttu-id="6e942-111">REST API</span><span class="sxs-lookup"><span data-stu-id="6e942-111">REST API</span></span>](data-factory-copy-activity-tutorial-using-rest-api.md)
> * [<span data-ttu-id="6e942-112">.NET API</span><span class="sxs-lookup"><span data-stu-id="6e942-112">.NET API</span></span>](data-factory-copy-activity-tutorial-using-dotnet-api.md)

<span data-ttu-id="6e942-113">Ebben az oktatóanyagban létrehoz egy adat-előállító egy Blob storage tooSQL adatbázisból adatcsatorna toocopy adatokkal.</span><span class="sxs-lookup"><span data-stu-id="6e942-113">In this tutorial, you create a data factory with a pipeline toocopy data from Blob storage tooSQL database.</span></span>

<span data-ttu-id="6e942-114">hello másolási tevékenység hello adatmozgás Azure Data Factory hajt végre.</span><span class="sxs-lookup"><span data-stu-id="6e942-114">hello Copy Activity performs hello data movement in Azure Data Factory.</span></span> <span data-ttu-id="6e942-115">Egy olyan, globálisan elérhető szolgáltatás működteti, amely biztonságos, megbízható és méretezhető módon másolja át az adatokat a különböző adattárak között.</span><span class="sxs-lookup"><span data-stu-id="6e942-115">It is powered by a globally available service that can copy data between various data stores in a secure, reliable, and scalable way.</span></span> <span data-ttu-id="6e942-116">Lásd: [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) szóló cikkben olvashat hello másolási tevékenység.</span><span class="sxs-lookup"><span data-stu-id="6e942-116">See [Data Movement Activities](data-factory-data-movement-activities.md) article for details about hello Copy Activity.</span></span>  

> [!NOTE]
> <span data-ttu-id="6e942-117">A Data Factory szolgáltatásnak hello részletes áttekintése, lásd: hello [Data Factory bemutatása tooAzure](data-factory-introduction.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="6e942-117">For a detailed overview of hello Data Factory service, see hello [Introduction tooAzure Data Factory](data-factory-introduction.md) article.</span></span>
>
>

## <a name="prerequisites-for-hello-tutorial"></a><span data-ttu-id="6e942-118">Hello oktatóanyag előfeltételei</span><span class="sxs-lookup"><span data-stu-id="6e942-118">Prerequisites for hello tutorial</span></span>
<span data-ttu-id="6e942-119">Ez az oktatóanyag elkezdéséhez a következő előfeltételek hello kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="6e942-119">Before you begin this tutorial, you must have hello following prerequisites:</span></span>

* <span data-ttu-id="6e942-120">**Azure-előfizetés**.</span><span class="sxs-lookup"><span data-stu-id="6e942-120">**Azure subscription**.</span></span>  <span data-ttu-id="6e942-121">Ha nem rendelkezik előfizetéssel, mindössze néhány perc alatt létrehozhat egy ingyenes próbafiókot.</span><span class="sxs-lookup"><span data-stu-id="6e942-121">If you don't have a subscription, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="6e942-122">Lásd: hello [ingyenes](http://azure.microsoft.com/pricing/free-trial/) cikkben alább.</span><span class="sxs-lookup"><span data-stu-id="6e942-122">See hello [Free Trial](http://azure.microsoft.com/pricing/free-trial/) article for details.</span></span>
* <span data-ttu-id="6e942-123">**Azure Storage-fiók**.</span><span class="sxs-lookup"><span data-stu-id="6e942-123">**Azure Storage Account**.</span></span> <span data-ttu-id="6e942-124">Hello blob Storage tárolót használja a **forrás** ebben az oktatóanyagban az adattároló.</span><span class="sxs-lookup"><span data-stu-id="6e942-124">You use hello blob storage as a **source** data store in this tutorial.</span></span> <span data-ttu-id="6e942-125">Ha egy Azure storage-fiók nem rendelkezik, tekintse meg a hello [hozzon létre egy tárfiókot](../storage/common/storage-create-storage-account.md#create-a-storage-account) lépéseket toocreate egyet a cikkben találhat.</span><span class="sxs-lookup"><span data-stu-id="6e942-125">if you don't have an Azure storage account, see hello [Create a storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account) article for steps toocreate one.</span></span>
* <span data-ttu-id="6e942-126">**Azure SQL Database**</span><span class="sxs-lookup"><span data-stu-id="6e942-126">**Azure SQL Database**.</span></span> <span data-ttu-id="6e942-127">Azure SQL-adatbázis, használja a **cél** ebben az oktatóanyagban az adattároló.</span><span class="sxs-lookup"><span data-stu-id="6e942-127">You use an Azure SQL database as a **destination** data store in this tutorial.</span></span> <span data-ttu-id="6e942-128">Ha még nem rendelkezik, amelyek segítségével is hello oktatóanyag, lásd: Azure SQL-adatbázis [hogyan toocreate és konfigurálása az Azure SQL Database](../sql-database/sql-database-get-started.md) toocreate egyet.</span><span class="sxs-lookup"><span data-stu-id="6e942-128">If you don't have an Azure SQL database that you can use in hello tutorial, See [How toocreate and configure an Azure SQL Database](../sql-database/sql-database-get-started.md) toocreate one.</span></span>
* <span data-ttu-id="6e942-129">**Az SQL Server 2012 vagy 2014 vagy a Visual Studio 2013**.</span><span class="sxs-lookup"><span data-stu-id="6e942-129">**SQL Server 2012/2014 or Visual Studio 2013**.</span></span> <span data-ttu-id="6e942-130">SQL Server Management Studio vagy Visual Studio toocreate mintaadatbázis és tooview hello eredményadatokat hello adatbázis használható.</span><span class="sxs-lookup"><span data-stu-id="6e942-130">You use SQL Server Management Studio or Visual Studio toocreate a sample database and tooview hello result data in hello database.</span></span>  

## <a name="collect-blob-storage-account-name-and-key"></a><span data-ttu-id="6e942-131">A blob storage fióknevet és kulcsot gyűjtése</span><span class="sxs-lookup"><span data-stu-id="6e942-131">Collect blob storage account name and key</span></span>
<span data-ttu-id="6e942-132">Név és fiókkulcs kulcsát az Azure storage-fiók toodo ebben az oktatóanyagban hello fiók szükséges.</span><span class="sxs-lookup"><span data-stu-id="6e942-132">You need hello account name and account key of your Azure storage account toodo this tutorial.</span></span> <span data-ttu-id="6e942-133">Jegyezze fel **fióknév** és **fiókkulcs** az Azure-tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="6e942-133">Note down **account name** and **account key** for your Azure storage account.</span></span>

1. <span data-ttu-id="6e942-134">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="6e942-134">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="6e942-135">Kattintson a **további szolgáltatások** hello bal oldali menüben, és válassza ki a **Tárfiókok**.</span><span class="sxs-lookup"><span data-stu-id="6e942-135">Click **More services** on hello left menu and select **Storage Accounts**.</span></span>

    ![Tallózás - Storage-fiókok](media/data-factory-copy-data-from-azure-blob-storage-to-sql-database/browse-storage-accounts.png)
3. <span data-ttu-id="6e942-137">A hello **Tárfiókok** panelen, jelölje be hello **Azure storage-fiók** megjeleníteni kívánt toouse ebben az oktatóanyagban.</span><span class="sxs-lookup"><span data-stu-id="6e942-137">In hello **Storage Accounts** blade, select hello **Azure storage account** that you want toouse in this tutorial.</span></span>
4. <span data-ttu-id="6e942-138">Válassza ki **hívóbetűk** hivatkozásra **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="6e942-138">Select **Access keys** link under **SETTINGS**.</span></span>
5. <span data-ttu-id="6e942-139">Kattintson a **másolási** (kép) gomb melletti túl**tárfióknév** szöveg mezőbe, a Mentés és illessze be azt valahol (például: a fájlt).</span><span class="sxs-lookup"><span data-stu-id="6e942-139">Click **copy** (image) button next too**Storage account name** text box and save/paste it somewhere (for example: in a text file).</span></span>
6. <span data-ttu-id="6e942-140">Ismételje meg a hello előző lépés toocopy vagy jegyezze fel a hello **key1**.</span><span class="sxs-lookup"><span data-stu-id="6e942-140">Repeat hello previous step toocopy or note down hello **key1**.</span></span>

    ![Tárelérési kulcs](media/data-factory-copy-data-from-azure-blob-storage-to-sql-database/storage-access-key.png)
7. <span data-ttu-id="6e942-142">Zárja be az összes hello paneleken kattintva **X**.</span><span class="sxs-lookup"><span data-stu-id="6e942-142">Close all hello blades by clicking **X**.</span></span>

## <a name="collect-sql-server-database-user-names"></a><span data-ttu-id="6e942-143">SQL server, az adatbázis, a felhasználói neveket gyűjtése</span><span class="sxs-lookup"><span data-stu-id="6e942-143">Collect SQL server, database, user names</span></span>
<span data-ttu-id="6e942-144">Ez az oktatóanyag szüksége van Azure SQL server, az adatbázis és a felhasználó toodo hello nevét.</span><span class="sxs-lookup"><span data-stu-id="6e942-144">You need hello names of Azure SQL server, database, and user toodo this tutorial.</span></span> <span data-ttu-id="6e942-145">Jegyezze fel a neveket **server**, **adatbázis**, és **felhasználói** az Azure SQL adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="6e942-145">Note down names of **server**, **database**, and **user** for your Azure SQL database.</span></span>

1. <span data-ttu-id="6e942-146">A hello **Azure-portálon**, kattintson a **további szolgáltatások** hello bal, és válassza a **SQL-adatbázisok**.</span><span class="sxs-lookup"><span data-stu-id="6e942-146">In hello **Azure portal**, click **More services** on hello left and select **SQL databases**.</span></span>
2. <span data-ttu-id="6e942-147">A hello **SQL adatbázisok panel**, jelölje be hello **adatbázis** megjeleníteni kívánt toouse ebben az oktatóanyagban.</span><span class="sxs-lookup"><span data-stu-id="6e942-147">In hello **SQL databases blade**, select hello **database** that you want toouse in this tutorial.</span></span> <span data-ttu-id="6e942-148">Jegyezze fel a hello **adatbázisnév**.</span><span class="sxs-lookup"><span data-stu-id="6e942-148">Note down hello **database name**.</span></span>  
3. <span data-ttu-id="6e942-149">A hello **SQL-adatbázis** panelen kattintson a **tulajdonságok** alatt **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="6e942-149">In hello **SQL database** blade, click **Properties** under **SETTINGS**.</span></span>
4. <span data-ttu-id="6e942-150">Jegyezze fel a hello értékek **kiszolgálónév** és **kiszolgáló-rendszergazdai bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="6e942-150">Note down hello values for **SERVER NAME** and **SERVER ADMIN LOGIN**.</span></span>
5. <span data-ttu-id="6e942-151">Zárja be az összes hello paneleken kattintva **X**.</span><span class="sxs-lookup"><span data-stu-id="6e942-151">Close all hello blades by clicking **X**.</span></span>

## <a name="allow-azure-services-tooaccess-sql-server"></a><span data-ttu-id="6e942-152">Azure-szolgáltatások tooaccess SQL server engedélyezése</span><span class="sxs-lookup"><span data-stu-id="6e942-152">Allow Azure services tooaccess SQL server</span></span>
<span data-ttu-id="6e942-153">Győződjön meg arról, hogy **hozzáférést tooAzure szolgáltatások** beállítás kikapcsolt **ON** az Azure SQL-kiszolgáló, a Data Factory szolgáltatásnak hello férhetnek hozzá az Azure SQL server.</span><span class="sxs-lookup"><span data-stu-id="6e942-153">Ensure that **Allow access tooAzure services** setting turned **ON** for your Azure SQL server so that hello Data Factory service can access your Azure SQL server.</span></span> <span data-ttu-id="6e942-154">tooverify, és kapcsolja be ezt a beállítást, a következő lépéseket hello:</span><span class="sxs-lookup"><span data-stu-id="6e942-154">tooverify and turn on this setting, do hello following steps:</span></span>

1. <span data-ttu-id="6e942-155">Kattintson a **további szolgáltatások** elemre, majd hello balra hub **SQL Server-kiszolgálók**.</span><span class="sxs-lookup"><span data-stu-id="6e942-155">Click **More services** hub on hello left and click **SQL servers**.</span></span>
2. <span data-ttu-id="6e942-156">Válassza ki a kiszolgálót, és kattintson a **BEÁLLÍTÁSOK** területen a **Tűzfal** elemre.</span><span class="sxs-lookup"><span data-stu-id="6e942-156">Select your server, and click **Firewall** under **SETTINGS**.</span></span>
3. <span data-ttu-id="6e942-157">A hello **tűzfalbeállítások** panelen kattintson a **ON** a **hozzáférést tooAzure szolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="6e942-157">In hello **Firewall settings** blade, click **ON** for **Allow access tooAzure services**.</span></span>
4. <span data-ttu-id="6e942-158">Zárja be az összes hello paneleken kattintva **X**.</span><span class="sxs-lookup"><span data-stu-id="6e942-158">Close all hello blades by clicking **X**.</span></span>

## <a name="prepare-blob-storage-and-sql-database"></a><span data-ttu-id="6e942-159">A Blob Storage és az SQL-adatbázis előkészítése</span><span class="sxs-lookup"><span data-stu-id="6e942-159">Prepare Blob Storage and SQL Database</span></span>
<span data-ttu-id="6e942-160">Most, az Azure blob storage és előkészítése Azure SQL adatbázis hello oktatóanyag hello lépések végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="6e942-160">Now, prepare your Azure blob storage and Azure SQL database for hello tutorial by performing hello following steps:</span></span>  

1. <span data-ttu-id="6e942-161">Indítsa el a Jegyzettömböt.</span><span class="sxs-lookup"><span data-stu-id="6e942-161">Launch Notepad.</span></span> <span data-ttu-id="6e942-162">A következő szöveg hello másolja, majd mentse a fájt **emp.txt** túl**C:\ADFGetStarted** mappa a merevlemezen.</span><span class="sxs-lookup"><span data-stu-id="6e942-162">Copy hello following text and save it as **emp.txt** too**C:\ADFGetStarted** folder on your hard drive.</span></span>

    ```
    John, Doe
    Jane, Doe
    ```
2. <span data-ttu-id="6e942-163">Használjon például az eszközök [Azure Tártallózó](http://storageexplorer.com/) toocreate hello **adftutorial** tárolóhoz és annak tooupload hello **emp.txt** fájl toohello tárolót.</span><span class="sxs-lookup"><span data-stu-id="6e942-163">Use tools such as [Azure Storage Explorer](http://storageexplorer.com/) toocreate hello **adftutorial** container and tooupload hello **emp.txt** file toohello container.</span></span>

    ![Az Azure Storage Explorer.](./media/data-factory-copy-data-from-azure-blob-storage-to-sql-database/getstarted-storage-explorer.png)
3. <span data-ttu-id="6e942-166">A következő SQL-parancsfájl toocreate hello használata hello **üres** az Azure SQL-adatbázis táblájában.</span><span class="sxs-lookup"><span data-stu-id="6e942-166">Use hello following SQL script toocreate hello **emp** table in your Azure SQL Database.</span></span>  

    ```SQL
    CREATE TABLE dbo.emp
    (
        ID int IDENTITY(1,1) NOT NULL,
        FirstName varchar(50),
        LastName varchar(50),
    )
    GO

    CREATE CLUSTERED INDEX IX_emp_ID ON dbo.emp (ID);
    ```

    <span data-ttu-id="6e942-167">**Ha az SQL Server 2012 vagy 2014 a számítógépen telepítve van:** kövesse az utasításokat [kezelése az Azure SQL Database SQL Server Management Studio használatával](../sql-database/sql-database-manage-azure-ssms.md) tooconnect tooyour Azure SQL server és a Futtatás hello SQL-parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="6e942-167">**If you have SQL Server 2012/2014 installed on your computer:** follow instructions from [Managing Azure SQL Database using SQL Server Management Studio](../sql-database/sql-database-manage-azure-ssms.md) tooconnect tooyour Azure SQL server and run hello SQL script.</span></span> <span data-ttu-id="6e942-168">Ebben a cikkben az hello [klasszikus Azure portálon](http://manage.windowsazure.com), nem hello [új Azure-portálon](https://portal.azure.com), tooconfigure tűzfal Azure SQL-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="6e942-168">This article uses hello [classic Azure portal](http://manage.windowsazure.com), not hello [new Azure portal](https://portal.azure.com), tooconfigure firewall for an Azure SQL server.</span></span>

    <span data-ttu-id="6e942-169">Ha az ügyfél számára nem engedélyezett tooaccess hello Azure SQL-kiszolgálót, meg kell tooconfigure tűzfal az Azure SQL server tooallow hozzáférés a számítógépről (IP-cím).</span><span class="sxs-lookup"><span data-stu-id="6e942-169">If your client is not allowed tooaccess hello Azure SQL server, you need tooconfigure firewall for your Azure SQL server tooallow access from your machine (IP Address).</span></span> <span data-ttu-id="6e942-170">Lásd: [Ez a cikk](../sql-database/sql-database-configure-firewall-settings.md) lépéseket tooconfigure hello tűzfal az Azure SQL-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="6e942-170">See [this article](../sql-database/sql-database-configure-firewall-settings.md) for steps tooconfigure hello firewall for your Azure SQL server.</span></span>

## <a name="create-a-data-factory"></a><span data-ttu-id="6e942-171">Data factory létrehozása</span><span class="sxs-lookup"><span data-stu-id="6e942-171">Create a data factory</span></span>
<span data-ttu-id="6e942-172">Hello Előfeltételek befejeződött.</span><span class="sxs-lookup"><span data-stu-id="6e942-172">You have completed hello prerequisites.</span></span> <span data-ttu-id="6e942-173">Egy adat-előállító valamelyik a következő módokon hello segítségével hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="6e942-173">You can create a data factory using one of hello following ways.</span></span> <span data-ttu-id="6e942-174">Kattintson a hello legördülő lista hello tetejéhez vagy a következő hivatkozások tooperform hello oktatóanyag hello hello lehetőségek közül.</span><span class="sxs-lookup"><span data-stu-id="6e942-174">Click one of hello options in hello drop-down list at hello top or hello following links tooperform hello tutorial.</span></span>     

* [<span data-ttu-id="6e942-175">Másolás varázsló</span><span class="sxs-lookup"><span data-stu-id="6e942-175">Copy Wizard</span></span>](data-factory-copy-data-wizard-tutorial.md)
* [<span data-ttu-id="6e942-176">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="6e942-176">Azure portal</span></span>](data-factory-copy-activity-tutorial-using-azure-portal.md)
* [<span data-ttu-id="6e942-177">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6e942-177">Visual Studio</span></span>](data-factory-copy-activity-tutorial-using-visual-studio.md)
* [<span data-ttu-id="6e942-178">PowerShell</span><span class="sxs-lookup"><span data-stu-id="6e942-178">PowerShell</span></span>](data-factory-copy-activity-tutorial-using-powershell.md)
* [<span data-ttu-id="6e942-179">Azure Resource Manager-sablon</span><span class="sxs-lookup"><span data-stu-id="6e942-179">Azure Resource Manager template</span></span>](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
* [<span data-ttu-id="6e942-180">REST API</span><span class="sxs-lookup"><span data-stu-id="6e942-180">REST API</span></span>](data-factory-copy-activity-tutorial-using-rest-api.md)
* [<span data-ttu-id="6e942-181">.NET API</span><span class="sxs-lookup"><span data-stu-id="6e942-181">.NET API</span></span>](data-factory-copy-activity-tutorial-using-dotnet-api.md)

> [!NOTE]
> <span data-ttu-id="6e942-182">hello adatok feldolgozási sor az oktatóanyag a forrás adatokat tároló tooa cél adatokat tároló másol adatokat.</span><span class="sxs-lookup"><span data-stu-id="6e942-182">hello data pipeline in this tutorial copies data from a source data store tooa destination data store.</span></span> <span data-ttu-id="6e942-183">Nem alakítja át a bemeneti adatok tooproduce kimeneti adatokat.</span><span class="sxs-lookup"><span data-stu-id="6e942-183">It does not transform input data tooproduce output data.</span></span> <span data-ttu-id="6e942-184">Hogyan oktatóanyagért tootransform adatok Azure Data Factory használatával, lásd: [oktatóanyag: az első adatcsatorna tootransform adatok Hadoop-fürt létrehozása](data-factory-build-your-first-pipeline.md).</span><span class="sxs-lookup"><span data-stu-id="6e942-184">For a tutorial on how tootransform data using Azure Data Factory, see [Tutorial: Build your first pipeline tootransform data using Hadoop cluster](data-factory-build-your-first-pipeline.md).</span></span>
> 
> <span data-ttu-id="6e942-185">Hello kimeneti adatkészlet egy tevékenység beállítását hello bemeneti hello az adatkészlet többi tevékenység által láncolt lehessen két tevékenység (egy tevékenység egymás után).</span><span class="sxs-lookup"><span data-stu-id="6e942-185">You can chain two activities (run one activity after another) by setting hello output dataset of one activity as hello input dataset of hello other activity.</span></span> <span data-ttu-id="6e942-186">Lásd [a Data Factorybeli ütemezést és végrehajtást](data-factory-scheduling-and-execution.md) ismertető cikket.</span><span class="sxs-lookup"><span data-stu-id="6e942-186">See [Scheduling and execution in Data Factory](data-factory-scheduling-and-execution.md) for detailed information.</span></span> 
