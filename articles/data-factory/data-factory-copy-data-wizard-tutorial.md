---
title: "Oktatóanyag: Folyamat létrehozása a Másolás varázsló használatával | Microsoft Docs"
description: "Ebben az oktatóanyagban hoz létre egy Azure Data Factory-folyamat a másolási tevékenység hello adat-előállító által támogatott másolása varázsló használatával"
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: b87afb8e-53b7-4e1b-905b-0343dd096198
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: 567b89e7a54c245c134cd0674690e6f3499b46d3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-create-a-pipeline-with-copy-activity-using-data-factory-copy-wizard"></a><span data-ttu-id="7f51c-103">Oktatóanyag: Másolási tevékenységgel rendelkező folyamat létrehozása a Data Factory Másolás varázslója használatával</span><span class="sxs-lookup"><span data-stu-id="7f51c-103">Tutorial: Create a pipeline with Copy Activity using Data Factory Copy Wizard</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="7f51c-104">Áttekintés és előfeltételek</span><span class="sxs-lookup"><span data-stu-id="7f51c-104">Overview and prerequisites</span></span>](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [<span data-ttu-id="7f51c-105">Másolás varázsló</span><span class="sxs-lookup"><span data-stu-id="7f51c-105">Copy Wizard</span></span>](data-factory-copy-data-wizard-tutorial.md)
> * [<span data-ttu-id="7f51c-106">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="7f51c-106">Azure portal</span></span>](data-factory-copy-activity-tutorial-using-azure-portal.md)
> * [<span data-ttu-id="7f51c-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7f51c-107">Visual Studio</span></span>](data-factory-copy-activity-tutorial-using-visual-studio.md)
> * [<span data-ttu-id="7f51c-108">PowerShell</span><span class="sxs-lookup"><span data-stu-id="7f51c-108">PowerShell</span></span>](data-factory-copy-activity-tutorial-using-powershell.md)
> * [<span data-ttu-id="7f51c-109">Azure Resource Manager-sablon</span><span class="sxs-lookup"><span data-stu-id="7f51c-109">Azure Resource Manager template</span></span>](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
> * [<span data-ttu-id="7f51c-110">REST API</span><span class="sxs-lookup"><span data-stu-id="7f51c-110">REST API</span></span>](data-factory-copy-activity-tutorial-using-rest-api.md)
> * [<span data-ttu-id="7f51c-111">.NET API</span><span class="sxs-lookup"><span data-stu-id="7f51c-111">.NET API</span></span>](data-factory-copy-activity-tutorial-using-dotnet-api.md)

<span data-ttu-id="7f51c-112">Az oktatóanyag bemutatja, hogyan toouse hello **másolása varázsló** toocopy Azure blob storage tooan Azure SQL-adatbázis adatait.</span><span class="sxs-lookup"><span data-stu-id="7f51c-112">This tutorial shows you how toouse hello **Copy Wizard** toocopy data from an Azure blob storage tooan Azure SQL database.</span></span> 

<span data-ttu-id="7f51c-113">Azure Data Factory hello **másolása varázsló** lehetővé teszi a tooquickly hozzon létre egy adatok folyamatot, amely egy támogatott forráshierarchiából adatokat tároló támogatott tooa cél adattárból másolja az adatokat.</span><span class="sxs-lookup"><span data-stu-id="7f51c-113">hello Azure Data Factory **Copy Wizard** allows you tooquickly create a data pipeline that copies data from a supported source data store tooa supported destination data store.</span></span> <span data-ttu-id="7f51c-114">Ezért azt javasoljuk, hogy hello varázslót használja, az első lépés toocreate egy minta folyamatot a mozgás forgatókönyvet.</span><span class="sxs-lookup"><span data-stu-id="7f51c-114">Therefore, we recommend that you use hello wizard as a first step toocreate a sample pipeline for your data movement scenario.</span></span> <span data-ttu-id="7f51c-115">A forrásként és célként támogatott adattárak listáját a [támogatott adattárakról](data-factory-data-movement-activities.md#supported-data-stores-and-formats) szóló témakörben találja.</span><span class="sxs-lookup"><span data-stu-id="7f51c-115">For a list of data stores supported as sources and as destinations, see [supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span></span>  

<span data-ttu-id="7f51c-116">Az oktatóanyag bemutatja, hogyan toocreate egy az Azure data factory indítási hello másolása varázsló halad át lépéseket tooprovide adatait az adatfeldolgozást/adatátviteli forgatókönyvet sorozata.</span><span class="sxs-lookup"><span data-stu-id="7f51c-116">This tutorial shows you how toocreate an Azure data factory, launch hello Copy Wizard, go through a series of steps tooprovide details about your data ingestion/movement scenario.</span></span> <span data-ttu-id="7f51c-117">Hello varázsló befejezése után hello varázsló automatikusan hoz létre egy folyamatot a másolási tevékenység toocopy adatait az Azure blob storage tooan Azure SQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="7f51c-117">When you finish steps in hello wizard, hello wizard automatically creates a pipeline with a Copy Activity toocopy data from an Azure blob storage tooan Azure SQL database.</span></span> <span data-ttu-id="7f51c-118">A másolási tevékenységről további információk az [adattovábbítási tevékenységekről](data-factory-data-movement-activities.md) szóló cikkben találhatók.</span><span class="sxs-lookup"><span data-stu-id="7f51c-118">For more information about Copy Activity, see [data movement activities](data-factory-data-movement-activities.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7f51c-119">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="7f51c-119">Prerequisites</span></span>
<span data-ttu-id="7f51c-120">Végezze el a témakörben ismertetett előfeltételeknek: hello [oktatóanyag – áttekintés](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) cikk Ez az oktatóanyag végrehajtása előtt.</span><span class="sxs-lookup"><span data-stu-id="7f51c-120">Complete prerequisites listed in hello [Tutorial Overview](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) article before performing this tutorial.</span></span>

## <a name="create-data-factory"></a><span data-ttu-id="7f51c-121">Data factory létrehozása</span><span class="sxs-lookup"><span data-stu-id="7f51c-121">Create data factory</span></span>
<span data-ttu-id="7f51c-122">Ebben a lépésben használhatja az Azure portál toocreate egy az Azure data factory nevű hello **ADFTutorialDataFactory**.</span><span class="sxs-lookup"><span data-stu-id="7f51c-122">In this step, you use hello Azure portal toocreate an Azure data factory named **ADFTutorialDataFactory**.</span></span>

1. <span data-ttu-id="7f51c-123">Jelentkezzen be túl[Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="7f51c-123">Log in too[Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="7f51c-124">Kattintson a **+ új** hello bal felső sarokban, kattintson **adatok + analitika**, és kattintson a **adat-előállító**.</span><span class="sxs-lookup"><span data-stu-id="7f51c-124">Click **+ NEW** from hello top-left corner, click **Data + analytics**, and click **Data Factory**.</span></span> 
   
   ![New (Új)->DataFactory](./media/data-factory-copy-data-wizard-tutorial/new-data-factory-menu.png)
2. <span data-ttu-id="7f51c-126">A hello **új adat-előállító** panel:</span><span class="sxs-lookup"><span data-stu-id="7f51c-126">In hello **New data factory** blade:</span></span>
   
   1. <span data-ttu-id="7f51c-127">Adja meg **ADFTutorialDataFactory** a hello **neve**.</span><span class="sxs-lookup"><span data-stu-id="7f51c-127">Enter **ADFTutorialDataFactory** for hello **name**.</span></span>
       <span data-ttu-id="7f51c-128">az Azure data factory hello hello nevének globálisan egyedi kell lennie.</span><span class="sxs-lookup"><span data-stu-id="7f51c-128">hello name of hello Azure data factory must be globally unique.</span></span> <span data-ttu-id="7f51c-129">Ha hello hibaüzenetet kapja: `Data factory name “ADFTutorialDataFactory” is not available`, hello adat-előállítóban (például yournameADFTutorialDataFactoryYYYYMMDD) hello nevének módosítása, és próbálja meg újra létrehozni.</span><span class="sxs-lookup"><span data-stu-id="7f51c-129">If you receive hello error: `Data factory name “ADFTutorialDataFactory” is not available`, change hello name of hello data factory (for example, yournameADFTutorialDataFactoryYYYYMMDD) and try creating again.</span></span> <span data-ttu-id="7f51c-130">A Data Factory-összetevők elnevezési szabályait a [Data Factory - Naming Rules](data-factory-naming-rules.md) (Data Factory – Elnevezési szabályok) című témakörben találhatja.</span><span class="sxs-lookup"><span data-stu-id="7f51c-130">See [Data Factory - Naming Rules](data-factory-naming-rules.md) topic for naming rules for Data Factory artifacts.</span></span>  
      
       ![A Data Factory name not available (A data factory neve nem érhető el) üzenet](./media/data-factory-copy-data-wizard-tutorial/getstarted-data-factory-not-available.png)    
   2. <span data-ttu-id="7f51c-132">Jelölje ki az Azure-**előfizetést**.</span><span class="sxs-lookup"><span data-stu-id="7f51c-132">Select your Azure **subscription**.</span></span>
   3. <span data-ttu-id="7f51c-133">Az erőforráscsoport hajtsa végre a lépéseket követve hello egyikét:</span><span class="sxs-lookup"><span data-stu-id="7f51c-133">For Resource Group, do one of hello following steps:</span></span> 
      
      - <span data-ttu-id="7f51c-134">Válassza ki **meglévő** tooselect egy meglévő erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="7f51c-134">Select **Use existing** tooselect an existing resource group.</span></span>
      - <span data-ttu-id="7f51c-135">Válassza ki **hozzon létre új** tooenter erőforráscsoport nevét.</span><span class="sxs-lookup"><span data-stu-id="7f51c-135">Select **Create new** tooenter a name for a resource group.</span></span>
          
        <span data-ttu-id="7f51c-136">Egyes ebben az oktatóanyagban hello lépések azt feltételezik, hogy, hogy hello nevét használja: **ADFTutorialResourceGroup** hello erőforráscsoport.</span><span class="sxs-lookup"><span data-stu-id="7f51c-136">Some of hello steps in this tutorial assume that you use hello name: **ADFTutorialResourceGroup** for hello resource group.</span></span> <span data-ttu-id="7f51c-137">tudnivalók az erőforráscsoportokról, toolearn lásd [erőforrás segítségével csoportosítja toomanage az Azure-erőforrások](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="7f51c-137">toolearn about resource groups, see [Using resource groups toomanage your Azure resources](../azure-resource-manager/resource-group-overview.md).</span></span>
   4. <span data-ttu-id="7f51c-138">Válassza ki a **hely** hello adat-előállító esetében.</span><span class="sxs-lookup"><span data-stu-id="7f51c-138">Select a **location** for hello data factory.</span></span>
   5. <span data-ttu-id="7f51c-139">Válassza ki **PIN-kód toodashboard** hello hello panel alsó részén jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="7f51c-139">Select **Pin toodashboard** check box at hello bottom of hello blade.</span></span>  
   6. <span data-ttu-id="7f51c-140">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="7f51c-140">Click **Create**.</span></span>
      
       ![A New data factory (Új data factory) panel](media/data-factory-copy-data-wizard-tutorial/new-data-factory-blade.png)            
3. <span data-ttu-id="7f51c-142">Hello létrehozásának befejezése után megjelenik a hello **adat-előállító** panelen látható hello kép a következő módon:</span><span class="sxs-lookup"><span data-stu-id="7f51c-142">After hello creation is complete, you see hello **Data Factory** blade as shown in hello following image:</span></span>
   
   ![Data factory kezdőlap](./media/data-factory-copy-data-wizard-tutorial/getstarted-data-factory-home-page.png)

## <a name="launch-copy-wizard"></a><span data-ttu-id="7f51c-144">A Másolás varázsló indítása</span><span class="sxs-lookup"><span data-stu-id="7f51c-144">Launch Copy Wizard</span></span>
1. <span data-ttu-id="7f51c-145">Hello adat-előállító paneljén kattintson **[előzetes verzió] adatok másolása** toolaunch hello **másolása varázsló**.</span><span class="sxs-lookup"><span data-stu-id="7f51c-145">On hello Data Factory blade, click **Copy data [PREVIEW]** toolaunch hello **Copy Wizard**.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="7f51c-146">Ha azt látja, hogy hello webböngésző akadt-e a "Engedélyező...", tiltsa le vagy törölje a jelet **külső cookie-k blokkolását, és a helyadatok** beállítása a böngészőbeállítások hello (vagy) során is garantálják az engedélyezve van, és hozzon létre egy kivételt  **login.microsoftonline.com** , és ezután próbálja meg újból elindítani a hello varázsló.</span><span class="sxs-lookup"><span data-stu-id="7f51c-146">If you see that hello web browser is stuck at "Authorizing...", disable/uncheck **Block third-party cookies and site data** setting in hello browser settings (or) keep it enabled and create an exception for **login.microsoftonline.com** and then try launching hello wizard again.</span></span>
2. <span data-ttu-id="7f51c-147">A hello **tulajdonságok** lap:</span><span class="sxs-lookup"><span data-stu-id="7f51c-147">In hello **Properties** page:</span></span>
   
   1. <span data-ttu-id="7f51c-148">Írja be a **CopyFromBlobToAzureSql** kifejezést a **Task name** (Feladat neve) mezőbe.</span><span class="sxs-lookup"><span data-stu-id="7f51c-148">Enter **CopyFromBlobToAzureSql** for **Task name**</span></span>
   2. <span data-ttu-id="7f51c-149">Adjon meg **leírást** (opcionális).</span><span class="sxs-lookup"><span data-stu-id="7f51c-149">Enter **description** (optional).</span></span>
   3. <span data-ttu-id="7f51c-150">Változás hello **kezdő dátum/idő** és hello **záró dátum és idő** úgy, hogy hello záró dátum tootoday be és indítsa el a korábbi dátumot toofive nap.</span><span class="sxs-lookup"><span data-stu-id="7f51c-150">Change hello **Start date time** and hello **End date time** so that hello end date is set tootoday and start date toofive days earlier.</span></span>  
   4. <span data-ttu-id="7f51c-151">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="7f51c-151">Click **Next**.</span></span>  
      
      ![Copy (Másolás) eszköz – Properties (Tulajdonságok) oldal](./media/data-factory-copy-data-wizard-tutorial/copy-tool-properties-page.png) 
3. <span data-ttu-id="7f51c-153">A hello **forrás adattár** kattintson **Azure Blob Storage** csempére.</span><span class="sxs-lookup"><span data-stu-id="7f51c-153">On hello **Source data store** page, click **Azure Blob Storage** tile.</span></span> <span data-ttu-id="7f51c-154">Az oldal toospecify hello forrás adattároló hello másolási feladathoz kell használnia.</span><span class="sxs-lookup"><span data-stu-id="7f51c-154">You use this page toospecify hello source data store for hello copy task.</span></span> 
   
    ![Copy (Másolás) eszköz – Source data store (Forrásadattár) oldal](./media/data-factory-copy-data-wizard-tutorial/copy-tool-source-data-store-page.png)
4. <span data-ttu-id="7f51c-156">A hello **hello Azure Blob storage-fiók megadása** lap:</span><span class="sxs-lookup"><span data-stu-id="7f51c-156">On hello **Specify hello Azure Blob storage account** page:</span></span>
   
   1. <span data-ttu-id="7f51c-157">Adja meg az **AzureStorageLinkedService** nevet a **Linked service name** (Társított szolgáltatás neve) mezőben.</span><span class="sxs-lookup"><span data-stu-id="7f51c-157">Enter **AzureStorageLinkedService** for **Linked service name**.</span></span>
   2. <span data-ttu-id="7f51c-158">Győződjön meg arról, hogy az **Account selection method** (Fiókválasztási módszer) mezőben a **From Azure subscriptions** (Azure-előfizetésekből) lehetőség van kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="7f51c-158">Confirm that **From Azure subscriptions** option is selected for **Account selection method**.</span></span>
   3. <span data-ttu-id="7f51c-159">Jelölje ki az Azure-**előfizetést**.</span><span class="sxs-lookup"><span data-stu-id="7f51c-159">Select your Azure **subscription**.</span></span>  
   4. <span data-ttu-id="7f51c-160">Válasszon egy **Azure storage-fiók** hello a hello kiválasztott előfizetésben elérhető fiókok az Azure storage listája.</span><span class="sxs-lookup"><span data-stu-id="7f51c-160">Select an **Azure storage account** from hello list of Azure storage accounts available in hello selected subscription.</span></span> <span data-ttu-id="7f51c-161">Másik lehetőségként tooenter tárolási fiók beállításait manuálisan kiválasztásával **adja meg manuálisan** hello beállítása **kijelöléséről fiók**, és kattintson a **tovább**.</span><span class="sxs-lookup"><span data-stu-id="7f51c-161">You can also choose tooenter storage account settings manually by selecting **Enter manually** option for hello **Account selection method**, and then click **Next**.</span></span> 
      
      ![Másolja az eszköz – hello Azure Blob storage-fiók megadása](./media/data-factory-copy-data-wizard-tutorial/copy-tool-specify-azure-blob-storage-account.png)
5. <span data-ttu-id="7f51c-163">A **válasszon hello bemeneti fájl vagy mappa** lap:</span><span class="sxs-lookup"><span data-stu-id="7f51c-163">On **Choose hello input file or folder** page:</span></span>
   
   1. <span data-ttu-id="7f51c-164">Kattintson duplán az **adftutorial** mappára.</span><span class="sxs-lookup"><span data-stu-id="7f51c-164">Double-click **adftutorial** (folder).</span></span>
   2. <span data-ttu-id="7f51c-165">Jelölje ki az **emp.txt** fájlt, és kattintson a **Choose** (Kiválasztás) lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="7f51c-165">Select **emp.txt**, and click **Choose**</span></span>
      
      ![Másolja az eszköz - a hello bemeneti fájl vagy mappa](./media/data-factory-copy-data-wizard-tutorial/copy-tool-choose-input-file-or-folder.png)
6. <span data-ttu-id="7f51c-167">A hello **válasszon hello bemeneti fájl vagy mappa** kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="7f51c-167">On hello **Choose hello input file or folder** page, click **Next**.</span></span> <span data-ttu-id="7f51c-168">Ne jelölje be a **Binary copy** (Bináris másolat) beállítást.</span><span class="sxs-lookup"><span data-stu-id="7f51c-168">Do not select **Binary copy**.</span></span> 
   
    ![Másolja az eszköz - a hello bemeneti fájl vagy mappa](./media/data-factory-copy-data-wizard-tutorial/chose-input-file-folder.png) 
7. <span data-ttu-id="7f51c-170">A hello **fájl formázási beállítások** lapon látni hello határolójelek és hello sémát, amelyhez hello varázsló automatikusan észleli hello-fájl elemzése.</span><span class="sxs-lookup"><span data-stu-id="7f51c-170">On hello **File format settings** page, you see hello delimiters and hello schema that is auto-detected by hello wizard by parsing hello file.</span></span> <span data-ttu-id="7f51c-171">Hello határolójelek manuálisan is beírhatja hello másolása varázsló toostop automatikus-észlelése vagy toooverride.</span><span class="sxs-lookup"><span data-stu-id="7f51c-171">You can also enter hello delimiters manually for hello copy wizard toostop auto-detecting or toooverride.</span></span> <span data-ttu-id="7f51c-172">Kattintson a **következő** után tekintse át a hello elválasztó karaktert, és tekintse meg adatok.</span><span class="sxs-lookup"><span data-stu-id="7f51c-172">Click **Next** after you review hello delimiters and preview data.</span></span> 
   
    ![Copy (Másolás eszköz) – File format settings (Fájlformátum beállításai) oldal](./media/data-factory-copy-data-wizard-tutorial/copy-tool-file-format-settings.png)  
8. <span data-ttu-id="7f51c-174">Hello cél adatokon tárolja a lapra, válassza ki **Azure SQL Database**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="7f51c-174">On hello Destination data store page, select **Azure SQL Database**, and click **Next**.</span></span>
   
    ![Copy (Másolás) eszköz – Choose destination store (Céltár kiválasztása) oldal](./media/data-factory-copy-data-wizard-tutorial/choose-destination-store.png)
9. <span data-ttu-id="7f51c-176">A **megadása hello Azure SQL adatbázis** lap:</span><span class="sxs-lookup"><span data-stu-id="7f51c-176">On **Specify hello Azure SQL database** page:</span></span>
   
   1. <span data-ttu-id="7f51c-177">Adja meg **AzureSqlLinkedService** a hello **kapcsolatnév** mező.</span><span class="sxs-lookup"><span data-stu-id="7f51c-177">Enter **AzureSqlLinkedService** for hello **Connection name** field.</span></span>
   2. <span data-ttu-id="7f51c-178">Győződjön meg arról, hogy a **Server/database selection method** (Kiszolgáló-/adatbázis-kiválasztási módszer) mezőben a **From Azure subscriptions** (Azure-előfizetésekből) lehetőség van kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="7f51c-178">Confirm that **From Azure subscriptions** option is selected for **Server / database selection method**.</span></span>
   3. <span data-ttu-id="7f51c-179">Jelölje ki az Azure-**előfizetést**.</span><span class="sxs-lookup"><span data-stu-id="7f51c-179">Select your Azure **subscription**.</span></span>  
   4. <span data-ttu-id="7f51c-180">Válassza ki a **Server name** (Kiszolgálónév) és a **Database** (Adatbázis) mezők értékeit.</span><span class="sxs-lookup"><span data-stu-id="7f51c-180">Select **Server name** and **Database**.</span></span>
   5. <span data-ttu-id="7f51c-181">Adja meg a **felhasználónevet** és a **jelszót**.</span><span class="sxs-lookup"><span data-stu-id="7f51c-181">Enter **User name** and **Password**.</span></span>
   6. <span data-ttu-id="7f51c-182">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="7f51c-182">Click **Next**.</span></span>  
      
      ![Copy (Másolás) eszköz – specify Azure SQL database (Az Azure SQL Database megadása) oldal](./media/data-factory-copy-data-wizard-tutorial/specify-azure-sql-database.png)
10. <span data-ttu-id="7f51c-184">A hello **táblaleképezés** lapon jelölje be **üres** a hello **cél** hello legördülő listából, majd kattintson **lefelé mutató nyíl** (nem kötelező) toosee hello séma- és toopreview hello adatokat.</span><span class="sxs-lookup"><span data-stu-id="7f51c-184">On hello **Table mapping** page, select **emp** for hello **Destination** field from hello drop-down list, click **down arrow** (optional) toosee hello schema and toopreview hello data.</span></span>
    
     ![Copy (Másolás) eszköz – Table mapping (Tábla-hozzárendelés) oldal](./media/data-factory-copy-data-wizard-tutorial/copy-tool-table-mapping-page.png) 
11. <span data-ttu-id="7f51c-186">A hello **séma-hozzárendelése** kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="7f51c-186">On hello **Schema mapping** page, click **Next**.</span></span>
    
    ![Copy (Másolás) eszköz – schema mapping (Séma-hozzárendelés) oldal](./media/data-factory-copy-data-wizard-tutorial/schema-mapping-page.png)
12. <span data-ttu-id="7f51c-188">A hello **Teljesítménybeállítások** kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="7f51c-188">On hello **Performance settings** page, click **Next**.</span></span> 
    
    ![Copy (Másolás) eszköz – performance settings (Teljesítménybeállítások) oldal](./media/data-factory-copy-data-wizard-tutorial/performance-settings.png)
13. <span data-ttu-id="7f51c-190">Tekintse át a hello **összegzés** lapon, majd kattintson **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="7f51c-190">Review information in hello **Summary** page, and click **Finish**.</span></span> <span data-ttu-id="7f51c-191">hello varázsló létrehoz két társított szolgáltatások, két adatkészletet (bemeneti és kimeneti) és egy folyamat (amelyen Ön indítani hello másolása varázsló) az adat-előállító hello.</span><span class="sxs-lookup"><span data-stu-id="7f51c-191">hello wizard creates two linked services, two datasets (input and output), and one pipeline in hello data factory (from where you launched hello Copy Wizard).</span></span> 
    
    ![Copy (Másolás) eszköz – performance settings (Teljesítménybeállítások) oldal](./media/data-factory-copy-data-wizard-tutorial/summary-page.png)

## <a name="launch-monitor-and-manage-application"></a><span data-ttu-id="7f51c-193">A Monitor and Manage alkalmazás elindítása</span><span class="sxs-lookup"><span data-stu-id="7f51c-193">Launch Monitor and Manage application</span></span>
1. <span data-ttu-id="7f51c-194">A hello **telepítési** lapján kattintson a hello hivatkozás: `Click here toomonitor copy pipeline`.</span><span class="sxs-lookup"><span data-stu-id="7f51c-194">On hello **Deployment** page, click hello link: `Click here toomonitor copy pipeline`.</span></span>
   
   ![Copy (Másolás) eszköz – Deployment succeeded (Sikeres üzembe helyezés) oldal](./media/data-factory-copy-data-wizard-tutorial/copy-tool-deployment-succeeded.png)  
2. <span data-ttu-id="7f51c-196">a webböngészőben külön lapon alkalmazás hello indul el.</span><span class="sxs-lookup"><span data-stu-id="7f51c-196">hello monitoring application is launched in a separate tab in your web browser.</span></span>   
   
   ![Monitoring App](./media/data-factory-copy-data-wizard-tutorial/monitoring-app.png)   
3. <span data-ttu-id="7f51c-198">toosee hello legfrissebb állapotát óránkénti szeletek kattintson **frissítése** hello gombjára **tevékenység WINDOWS** lista hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="7f51c-198">toosee hello latest status of hourly slices, click **Refresh** button in hello **ACTIVITY WINDOWS** list at hello bottom.</span></span> <span data-ttu-id="7f51c-199">Öt tevékenység windows hello adatcsatorna kezdő és záró időpont között öt napig láthatja.</span><span class="sxs-lookup"><span data-stu-id="7f51c-199">You see five activity windows for five days between start and end times for hello pipeline.</span></span> <span data-ttu-id="7f51c-200">hello lista nem frissül automatikusan, így Ön kell tooclick frissítse néhány alkalommal megjelenik az összes hello tevékenység windows hello üzemkész állapotban.</span><span class="sxs-lookup"><span data-stu-id="7f51c-200">hello list is not automatically refreshed, so you may need tooclick Refresh a couple of times before you see all hello activity windows in hello Ready state.</span></span> 
4. <span data-ttu-id="7f51c-201">Jelöljön ki egy tevékenységet ablak hello listán.</span><span class="sxs-lookup"><span data-stu-id="7f51c-201">Select an activity window in hello list.</span></span> <span data-ttu-id="7f51c-202">Hello vonatkozó részletes azt a hello **tevékenység ablak Explorer** a jobb oldali hello.</span><span class="sxs-lookup"><span data-stu-id="7f51c-202">See hello details about it in hello **Activity Window Explorer** on hello right.</span></span>

    ![Tevékenységablakok részletei](media/data-factory-copy-data-wizard-tutorial/activity-window-details.png)    

    <span data-ttu-id="7f51c-204">Figyelje meg, hogy hello dátumok 11, 12, 13, 14 vagy 15 zöld színnel történik, ami azt jelenti, hogy ebben az időszakban hello napi kimeneti szeletek már előállított legyenek.</span><span class="sxs-lookup"><span data-stu-id="7f51c-204">Notice that hello dates 11, 12, 13, 14, and 15 are in green color, which means that hello daily output slices for these dates have already been produced.</span></span> <span data-ttu-id="7f51c-205">Is tekintse meg a színkódolás a hello folyamat, és hello kimeneti adatkészlet hello diagram nézetben.</span><span class="sxs-lookup"><span data-stu-id="7f51c-205">You also see this color coding on hello pipeline and hello output dataset in hello diagram view.</span></span> <span data-ttu-id="7f51c-206">Hello előző lépésben figyelje meg, hogy két szeletek már előállított, egy szelet folyamatban van, és a hello más két végrehajtásra feldolgozott toobe (hello színkódolás alapján).</span><span class="sxs-lookup"><span data-stu-id="7f51c-206">In hello previous step, notice that two slices have already been produced, one slice is currently being processed, and hello other two are waiting toobe processed (based on hello color coding).</span></span> 

    <span data-ttu-id="7f51c-207">Az alkalmazás használatáról további tudnivalókat talál a [Monitor and manage pipeline using Monitoring App](data-factory-monitor-manage-app.md) (Folyamat figyelése és felügyelete a Monitoring App használatával) című cikkben.</span><span class="sxs-lookup"><span data-stu-id="7f51c-207">For more information on using this application, see [Monitor and manage pipeline using Monitoring App](data-factory-monitor-manage-app.md) article.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7f51c-208">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7f51c-208">Next steps</span></span>
<span data-ttu-id="7f51c-209">Ez az oktatóanyag egy olyan másolási műveletet mutatott be, amelynek a forrásadattára egy Azure Blob Storage-tár, a céladattára pedig egy Azure SQL-adatbázis volt.</span><span class="sxs-lookup"><span data-stu-id="7f51c-209">In this tutorial, you used Azure blob storage as a source data store and an Azure SQL database as a destination data store in a copy operation.</span></span> <span data-ttu-id="7f51c-210">hello következő táblázat felsorolja az adatforrások és a célhelyek hello másolási tevékenység által támogatott adattárolókhoz:</span><span class="sxs-lookup"><span data-stu-id="7f51c-210">hello following table provides a list of data stores supported as sources and destinations by hello copy activity:</span></span> 

[!INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

<span data-ttu-id="7f51c-211">Mezők és tulajdonságok számára egy adattárból hello másolása varázsló látható információt rendszerben hivatkozásra hello hello adattároló hello táblában.</span><span class="sxs-lookup"><span data-stu-id="7f51c-211">For details about fields/properties that you see in hello copy wizard for a data store, click hello link for hello data store in hello table.</span></span> 
