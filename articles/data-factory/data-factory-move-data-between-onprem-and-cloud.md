---
title: "aaaMove adatok - az adatkezelési átjáró |} Microsoft Docs"
description: "Egy adatok átjáró toomove adatok között a helyszíni és hello beállítása felhő. Használja az adatkezelési átjáró Azure Data Factory toomove adatait."
keywords: "az adatátjáró, Adatintegráció, áthelyezni az adatokat, átjáró hitelesítő adatok"
services: data-factory
documentationcenter: 
author: nabhishek
manager: jhubbard
editor: monicar
ms.assetid: 7bf6d8fd-04b5-499d-bd19-eff217aa4a9c
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: abnarain
ms.openlocfilehash: 314341c142d5260c785b7e82081774f044450e81
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-between-on-premises-sources-and-hello-cloud-with-data-management-gateway"></a><span data-ttu-id="4dab6-105">Adatok áthelyezése a helyszíni adatforrások és az adatkezelési átjáró hello felhő között</span><span class="sxs-lookup"><span data-stu-id="4dab6-105">Move data between on-premises sources and hello cloud with Data Management Gateway</span></span>
<span data-ttu-id="4dab6-106">Ez a cikk áttekintést adatok integrálása a helyszíni adattárolókhoz és a Data Factory használatával felhő adattárolók között.</span><span class="sxs-lookup"><span data-stu-id="4dab6-106">This article provides an overview of data integration between on-premises data stores and cloud data stores using Data Factory.</span></span> <span data-ttu-id="4dab6-107">-Buildekről nyújtanak a hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikk és az egyéb adatok gyári alapvető fogalmak: [adatkészletek](data-factory-create-datasets.md) és [folyamatok](data-factory-create-pipelines.md).</span><span class="sxs-lookup"><span data-stu-id="4dab6-107">It builds on hello [Data Movement Activities](data-factory-data-movement-activities.md) article and other data factory core concepts articles: [datasets](data-factory-create-datasets.md) and [pipelines](data-factory-create-pipelines.md).</span></span>

## <a name="data-management-gateway"></a><span data-ttu-id="4dab6-108">Adatkezelési átjáró</span><span class="sxs-lookup"><span data-stu-id="4dab6-108">Data Management Gateway</span></span>
<span data-ttu-id="4dab6-109">Telepítenie kell az adatkezelési átjáró a helyi gép tooenable adatmozgatás belőle egy helyszíni adattároló.</span><span class="sxs-lookup"><span data-stu-id="4dab6-109">You must install Data Management Gateway on your on-premises machine tooenable moving data to/from an on-premises data store.</span></span> <span data-ttu-id="4dab6-110">hello átjáró ugyanaz a gép hello adattárban vagy egy másik számítógépen, amíg hello-átjáró képes kapcsolódni toohello adattár hello is telepíthető.</span><span class="sxs-lookup"><span data-stu-id="4dab6-110">hello gateway can be installed on hello same machine as hello data store or on a different machine as long as hello gateway can connect toohello data store.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="4dab6-111">Lásd: [az adatkezelési átjáró](data-factory-data-management-gateway.md) szóló cikkben olvashat az adatkezelési átjáró.</span><span class="sxs-lookup"><span data-stu-id="4dab6-111">See [Data Management Gateway](data-factory-data-management-gateway.md) article for details about Data Management Gateway.</span></span> 

<span data-ttu-id="4dab6-112">hello következő forgatókönyv bemutatja, hogyan toocreate egy adat-előállító egy folyamatot, amely egy helyszíni helyezi át az adatokat a **SQL Server** tooan Azure blob-tároló adatbázis.</span><span class="sxs-lookup"><span data-stu-id="4dab6-112">hello following walkthrough shows you how toocreate a data factory with a pipeline that moves data from an on-premises **SQL Server** database tooan Azure blob storage.</span></span> <span data-ttu-id="4dab6-113">Hello forgatókönyv részeként telepítse, és konfigurálja a hello az adatkezelési átjáró a számítógépre.</span><span class="sxs-lookup"><span data-stu-id="4dab6-113">As part of hello walkthrough, you install and configure hello Data Management Gateway on your machine.</span></span>

## <a name="walkthrough-copy-on-premises-data-toocloud"></a><span data-ttu-id="4dab6-114">Forgatókönyv: a helyszíni adatok toocloud másolása</span><span class="sxs-lookup"><span data-stu-id="4dab6-114">Walkthrough: copy on-premises data toocloud</span></span>
<span data-ttu-id="4dab6-115">A bemutatóban szereplő lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="4dab6-115">In this walkthrough you do hello following steps:</span></span> 

1. <span data-ttu-id="4dab6-116">Adat-előállító létrehozása</span><span class="sxs-lookup"><span data-stu-id="4dab6-116">Create a data factory.</span></span>
2. <span data-ttu-id="4dab6-117">Hozzon létre egy az adatkezelési átjáró.</span><span class="sxs-lookup"><span data-stu-id="4dab6-117">Create a data management gateway.</span></span> 
3. <span data-ttu-id="4dab6-118">A forrás és a fogadó adattárolókhoz társított szolgáltatások létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="4dab6-118">Create linked services for source and sink data stores.</span></span>
4. <span data-ttu-id="4dab6-119">Létrehozni adatkészletek toorepresent bemeneti és kimeneti adatai.</span><span class="sxs-lookup"><span data-stu-id="4dab6-119">Create datasets toorepresent input and output data.</span></span>
5. <span data-ttu-id="4dab6-120">Hozzon létre egy folyamatot egy tevékenység toomove hello adatok másolása.</span><span class="sxs-lookup"><span data-stu-id="4dab6-120">Create a pipeline with a copy activity toomove hello data.</span></span>

## <a name="prerequisites-for-hello-tutorial"></a><span data-ttu-id="4dab6-121">Hello oktatóanyag előfeltételei</span><span class="sxs-lookup"><span data-stu-id="4dab6-121">Prerequisites for hello tutorial</span></span>
<span data-ttu-id="4dab6-122">Ez a forgatókönyv megkezdése előtt rendelkeznie kell a következő előfeltételek hello:</span><span class="sxs-lookup"><span data-stu-id="4dab6-122">Before you begin this walkthrough, you must have hello following prerequisites:</span></span>

* <span data-ttu-id="4dab6-123">**Azure-előfizetés**.</span><span class="sxs-lookup"><span data-stu-id="4dab6-123">**Azure subscription**.</span></span>  <span data-ttu-id="4dab6-124">Ha nem rendelkezik előfizetéssel, mindössze néhány perc alatt létrehozhat egy ingyenes próbafiókot.</span><span class="sxs-lookup"><span data-stu-id="4dab6-124">If you don't have a subscription, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="4dab6-125">Lásd: hello [ingyenes](http://azure.microsoft.com/pricing/free-trial/) cikkben alább.</span><span class="sxs-lookup"><span data-stu-id="4dab6-125">See hello [Free Trial](http://azure.microsoft.com/pricing/free-trial/) article for details.</span></span>
* <span data-ttu-id="4dab6-126">**Azure Storage-fiók**.</span><span class="sxs-lookup"><span data-stu-id="4dab6-126">**Azure Storage Account**.</span></span> <span data-ttu-id="4dab6-127">Hello blob Storage tárolót használja a **cél/fogadó** ebben az oktatóanyagban az adattároló.</span><span class="sxs-lookup"><span data-stu-id="4dab6-127">You use hello blob storage as a **destination/sink** data store in this tutorial.</span></span> <span data-ttu-id="4dab6-128">Ha egy Azure storage-fiók nem rendelkezik, tekintse meg a hello [hozzon létre egy tárfiókot](../storage/common/storage-create-storage-account.md#create-a-storage-account) lépéseket toocreate egyet a cikkben találhat.</span><span class="sxs-lookup"><span data-stu-id="4dab6-128">if you don't have an Azure storage account, see hello [Create a storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account) article for steps toocreate one.</span></span>
* <span data-ttu-id="4dab6-129">**Egy SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="4dab6-129">**SQL Server**.</span></span> <span data-ttu-id="4dab6-130">Ebben az oktatóanyagban egy helyszíni SQL Server-adatbázist használunk **forrásadattárként**.</span><span class="sxs-lookup"><span data-stu-id="4dab6-130">You use an on-premises SQL Server database as a **source** data store in this tutorial.</span></span> 

## <a name="create-data-factory"></a><span data-ttu-id="4dab6-131">Data factory létrehozása</span><span class="sxs-lookup"><span data-stu-id="4dab6-131">Create data factory</span></span>
<span data-ttu-id="4dab6-132">Ebben a lépésben használhatja az Azure Data Factory-példány neve az Azure portál toocreate hello **ADFTutorialOnPremDF**.</span><span class="sxs-lookup"><span data-stu-id="4dab6-132">In this step, you use hello Azure portal toocreate an Azure Data Factory instance named **ADFTutorialOnPremDF**.</span></span>

1. <span data-ttu-id="4dab6-133">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="4dab6-133">Log in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="4dab6-134">Kattintson a **+ új**, kattintson a **Eszközintelligencia + analitika**, és kattintson a **adat-előállító**.</span><span class="sxs-lookup"><span data-stu-id="4dab6-134">Click **+ NEW**, click **Intelligence + analytics**, and click **Data Factory**.</span></span>

   ![New (Új)->DataFactory](./media/data-factory-move-data-between-onprem-and-cloud/NewDataFactoryMenu.png)  
3. <span data-ttu-id="4dab6-136">A hello **új adat-előállító** lapján adja meg **ADFTutorialOnPremDF** a hello nevét.</span><span class="sxs-lookup"><span data-stu-id="4dab6-136">In hello **New data factory** page, enter **ADFTutorialOnPremDF** for hello Name.</span></span>

    ![TooStartboard hozzáadása](./media/data-factory-move-data-between-onprem-and-cloud/OnPremNewDataFactoryAddToStartboard.png)

   > [!IMPORTANT]
   > <span data-ttu-id="4dab6-138">az Azure data factory hello hello nevének globálisan egyedi kell lennie.</span><span class="sxs-lookup"><span data-stu-id="4dab6-138">hello name of hello Azure data factory must be globally unique.</span></span> <span data-ttu-id="4dab6-139">Ha hello hibaüzenetet kapja: **nem érhető el adat-előállító "ADFTutorialOnPremDF"**, hello adat-előállítóban (például yournameADFTutorialOnPremDF) hello nevének módosítása, és próbálja meg újra létrehozni.</span><span class="sxs-lookup"><span data-stu-id="4dab6-139">If you receive hello error: **Data factory name “ADFTutorialOnPremDF” is not available**, change hello name of hello data factory (for example, yournameADFTutorialOnPremDF) and try creating again.</span></span> <span data-ttu-id="4dab6-140">Használja ezt a nevet ADFTutorialOnPremDF helyett fennmaradó lépéseit az oktatóanyag végrehajtása közben.</span><span class="sxs-lookup"><span data-stu-id="4dab6-140">Use this name in place of ADFTutorialOnPremDF while performing remaining steps in this tutorial.</span></span>
   >
   > <span data-ttu-id="4dab6-141">hello adat-előállító nevét hello regisztrálva előfordulhat, hogy legyen egy **DNS** hello jövőben nevet, és ezért a nyilvánosan láthatóvá válnak.</span><span class="sxs-lookup"><span data-stu-id="4dab6-141">hello name of hello data factory may be registered as a **DNS** name in hello future and hence become publically visible.</span></span>
   >
   >
4. <span data-ttu-id="4dab6-142">Jelölje be hello **Azure-előfizetés** hello data factory toobe létrehozni kívánt.</span><span class="sxs-lookup"><span data-stu-id="4dab6-142">Select hello **Azure subscription** where you want hello data factory toobe created.</span></span>
5. <span data-ttu-id="4dab6-143">Jelöljön ki egy meglévő **erőforráscsoportot**, vagy hozzon létre egyet.</span><span class="sxs-lookup"><span data-stu-id="4dab6-143">Select existing **resource group** or create a resource group.</span></span> <span data-ttu-id="4dab6-144">Az oktatóanyagban hello nevű erőforráscsoport létrehozása: **ADFTutorialResourceGroup**.</span><span class="sxs-lookup"><span data-stu-id="4dab6-144">For hello tutorial, create a resource group named: **ADFTutorialResourceGroup**.</span></span>
6. <span data-ttu-id="4dab6-145">Kattintson a **létrehozása** a hello **új adat-előállító** lap.</span><span class="sxs-lookup"><span data-stu-id="4dab6-145">Click **Create** on hello **New data factory** page.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="4dab6-146">toocreate adat-előállító példányok hello tagjának kell lennie [Data Factory közreműködői](../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) szerepkör hello előfizetés-erőforráscsoport szintjén.</span><span class="sxs-lookup"><span data-stu-id="4dab6-146">toocreate Data Factory instances, you must be a member of hello [Data Factory Contributor](../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) role at hello subscription/resource group level.</span></span>
   >
   >
7. <span data-ttu-id="4dab6-147">Létrehozásának befejezése után megjelenik a hello **adat-előállító** lapon látható hello kép a következő módon:</span><span class="sxs-lookup"><span data-stu-id="4dab6-147">After creation is complete, you see hello **Data Factory** page as shown in hello following image:</span></span>

   ![Data Factory kezdőlap](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDataFactoryHomePage.png)

## <a name="create-gateway"></a><span data-ttu-id="4dab6-149">Átjáró létrehozása</span><span class="sxs-lookup"><span data-stu-id="4dab6-149">Create gateway</span></span>
1. <span data-ttu-id="4dab6-150">A hello **adat-előállító** kattintson **Szerző és központi telepítése** csempe toolaunch hello **szerkesztő** az hello data factory.</span><span class="sxs-lookup"><span data-stu-id="4dab6-150">In hello **Data Factory** page, click **Author and deploy** tile toolaunch hello **Editor** for hello data factory.</span></span>

    ![Az Author and deploy (Fejlesztés és üzembe helyezés) csempe](./media/data-factory-move-data-between-onprem-and-cloud/author-deploy-tile.png)
2. <span data-ttu-id="4dab6-152">A Data Factory Editor hello, kattintson **... További** az eszköztár hello, és kattintson a **az új adatátjáró**.</span><span class="sxs-lookup"><span data-stu-id="4dab6-152">In hello Data Factory Editor, click **... More** on hello toolbar and then click **New data gateway**.</span></span> <span data-ttu-id="4dab6-153">Másik lehetőségként kattintson a jobb egérgombbal **Data Gateways** hello fanézetben, és kattintson a **az új adatátjáró**.</span><span class="sxs-lookup"><span data-stu-id="4dab6-153">Alternatively, you can right-click **Data Gateways** in hello tree view, and click **New data gateway**.</span></span>

   ![Az új adatátjáró eszköztár](./media/data-factory-move-data-between-onprem-and-cloud/NewDataGateway.png)
3. <span data-ttu-id="4dab6-155">A hello **létrehozása** lapján adja meg **adftutorialgateway** a hello **neve**, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="4dab6-155">In hello **Create** page, enter **adftutorialgateway** for hello **name**, and click **OK**.</span></span>     

    ![Átjáró lap létrehozása](./media/data-factory-move-data-between-onprem-and-cloud/OnPremCreateGatewayBlade.png)

    > [!NOTE]
    > <span data-ttu-id="4dab6-157">A forgatókönyv akkor hozzon létre hello logikai csak egy csomópont (a helyi Windows-számítógépen).</span><span class="sxs-lookup"><span data-stu-id="4dab6-157">In this walkthrough, you create hello logical gateway with only one node (on-premises Windows machine).</span></span> <span data-ttu-id="4dab6-158">Az adatkezelési átjáró kiterjesztése hello átjáró több helyszíni gépet társít.</span><span class="sxs-lookup"><span data-stu-id="4dab6-158">You can scale out a data management gateway by associating multiple on-premises machines with hello gateway.</span></span> <span data-ttu-id="4dab6-159">Méretezheti növelésével csomóponton egyidejűleg futtatható az adatátviteli feladatok száma legfeljebb.</span><span class="sxs-lookup"><span data-stu-id="4dab6-159">You can scale up by increasing number of data movement jobs that can run concurrently on a node.</span></span> <span data-ttu-id="4dab6-160">Ez a szolgáltatás egy logikai átjárójának egyetlen csomópont is érhető el.</span><span class="sxs-lookup"><span data-stu-id="4dab6-160">This feature is also available for a logical gateway with a single node.</span></span> <span data-ttu-id="4dab6-161">Lásd: [méretezés az adatkezelési átjáró az Azure Data Factory](data-factory-data-management-gateway-high-availability-scalability.md) cikkben alább.</span><span class="sxs-lookup"><span data-stu-id="4dab6-161">See [Scaling data management gateway in Azure Data Factory](data-factory-data-management-gateway-high-availability-scalability.md) article for details.</span></span>  
4. <span data-ttu-id="4dab6-162">A hello **konfigurálása** kattintson **telepíthető közvetlenül a számítógépen**.</span><span class="sxs-lookup"><span data-stu-id="4dab6-162">In hello **Configure** page, click **Install directly on this computer**.</span></span> <span data-ttu-id="4dab6-163">Ez a művelet hello átjáró hello telepítési csomagot tölti le, telepíti, konfigurálja, és regisztrálja a hello átjáró hello számítógépen.</span><span class="sxs-lookup"><span data-stu-id="4dab6-163">This action downloads hello installation package for hello gateway, installs, configures, and registers hello gateway on hello computer.</span></span>  

   > [!NOTE]
   > <span data-ttu-id="4dab6-164">Az Internet Explorer vagy a Microsoft ClickOnce kompatibilis böngésző használata.</span><span class="sxs-lookup"><span data-stu-id="4dab6-164">Use Internet Explorer or a Microsoft ClickOnce compatible web browser.</span></span>
   >
   > <span data-ttu-id="4dab6-165">Ha a Chrome használ, lépjen a toohello [Chrome webes tároló](https://chrome.google.com/webstore/), keressen a "ClickOnce" kulcsszóval, válasszon egyet a hello ClickOnce kiterjesztések, és telepítse.</span><span class="sxs-lookup"><span data-stu-id="4dab6-165">If you are using Chrome, go toohello [Chrome web store](https://chrome.google.com/webstore/), search with "ClickOnce" keyword, choose one of hello ClickOnce extensions, and install it.</span></span>
   >
   > <span data-ttu-id="4dab6-166">Hello azonos Firefox (install-bővítmény).</span><span class="sxs-lookup"><span data-stu-id="4dab6-166">Do hello same for Firefox (install add-in).</span></span> <span data-ttu-id="4dab6-167">Kattintson a **menü megnyitása** hello gombjára (**három vízszintes vonal** hello jobb felső sarkában), kattintson a **bővítmények**, keressen a "ClickOnce" kulcsszóval, válasszon egyet a hello ClickOnce-bővítményeket, és telepítse azt.</span><span class="sxs-lookup"><span data-stu-id="4dab6-167">Click **Open Menu** button on hello toolbar (**three horizontal lines** in hello top-right corner), click **Add-ons**, search with "ClickOnce" keyword, choose one of hello ClickOnce extensions, and install it.</span></span>    
   >
   >

    ![Átjáró - lapjának konfigurálása](./media/data-factory-move-data-between-onprem-and-cloud/OnPremGatewayConfigureBlade.png)

    <span data-ttu-id="4dab6-169">Ily módon hello legegyszerűbb módja (egykattintásos) toodownload, telepítése, konfigurálása, és regisztrálja a hello átjárót egy egyetlen lépésben.</span><span class="sxs-lookup"><span data-stu-id="4dab6-169">This way is hello easiest way (one-click) toodownload, install, configure, and register hello gateway in one single step.</span></span> <span data-ttu-id="4dab6-170">Megtekintheti a hello **Microsoft adatkezelési átjáró konfigurációkezelőjének** alkalmazás telepítve van a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="4dab6-170">You can see hello **Microsoft Data Management Gateway Configuration Manager** application is installed on your computer.</span></span> <span data-ttu-id="4dab6-171">Megtalálhat hello végrehajtható **ConfigManager.exe** hello mappában: **C:\Program Files\Microsoft Data felügyeleti Gateway\2.0\Shared**.</span><span class="sxs-lookup"><span data-stu-id="4dab6-171">You can also find hello executable **ConfigManager.exe** in hello folder: **C:\Program Files\Microsoft Data Management Gateway\2.0\Shared**.</span></span>

    <span data-ttu-id="4dab6-172">Is letöltheti és ezen a lapon hello hivatkozások használatával manuálisan telepíteni az átjárót és hello látható hello kulccsal regisztrálható **új kulcs** szövegmezőben.</span><span class="sxs-lookup"><span data-stu-id="4dab6-172">You can also download and install gateway manually by using hello links in this page and register it using hello key shown in hello **NEW KEY** text box.</span></span>

    <span data-ttu-id="4dab6-173">Lásd: [az adatkezelési átjáró](data-factory-data-management-gateway.md) a következő cikket: az összes hello hello átjáró adatait.</span><span class="sxs-lookup"><span data-stu-id="4dab6-173">See [Data Management Gateway](data-factory-data-management-gateway.md) article for all hello details about hello gateway.</span></span>

   > [!NOTE]
   > <span data-ttu-id="4dab6-174">A helyi számítógép tooinstall hello rendszergazdai és hello az adatkezelési átjáró sikeresen konfigurálnia kell.</span><span class="sxs-lookup"><span data-stu-id="4dab6-174">You must be an administrator on hello local computer tooinstall and configure hello Data Management Gateway successfully.</span></span> <span data-ttu-id="4dab6-175">Hozzáadhat további felhasználók toohello **adatkezelési átjáró felhasználói** helyi Windows-csoport.</span><span class="sxs-lookup"><span data-stu-id="4dab6-175">You can add additional users toohello **Data Management Gateway Users** local Windows group.</span></span> <span data-ttu-id="4dab6-176">a csoport tagjai hello hello az adatkezelési átjáró konfigurációkezelőjének eszköz tooconfigure hello gateway használható.</span><span class="sxs-lookup"><span data-stu-id="4dab6-176">hello members of this group can use hello Data Management Gateway Configuration Manager tool tooconfigure hello gateway.</span></span>
   >
   >
5. <span data-ttu-id="4dab6-177">Várjon néhány percet, vagy várjon, amíg megjelenik a következő értesítési üzenet hello:</span><span class="sxs-lookup"><span data-stu-id="4dab6-177">Wait for a couple of minutes or wait until you see hello following notification message:</span></span>

    ![Átjáró telepítése sikeres](./media/data-factory-move-data-between-onprem-and-cloud/gateway-install-success.png)
6. <span data-ttu-id="4dab6-179">Indítsa el **az adatkezelési átjáró konfigurációkezelőjének** alkalmazás a számítógépre.</span><span class="sxs-lookup"><span data-stu-id="4dab6-179">Launch **Data Management Gateway Configuration Manager** application on your computer.</span></span> <span data-ttu-id="4dab6-180">A hello **keresési** ablakot, írja be **az adatkezelési átjáró** tooaccess ilyen eszközt.</span><span class="sxs-lookup"><span data-stu-id="4dab6-180">In hello **Search** window, type **Data Management Gateway** tooaccess this utility.</span></span> <span data-ttu-id="4dab6-181">Megtalálhat hello végrehajtható **ConfigManager.exe** hello mappában: **C:\Program Files\Microsoft Data felügyeleti Gateway\2.0\Shared**</span><span class="sxs-lookup"><span data-stu-id="4dab6-181">You can also find hello executable **ConfigManager.exe** in hello folder: **C:\Program Files\Microsoft Data Management Gateway\2.0\Shared**</span></span>

    ![Átjáró konfigurációkezelője](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDMGConfigurationManager.png)
7. <span data-ttu-id="4dab6-183">Ellenőrizze, hogy látható `adftutorialgateway is connected toohello cloud service` üzenet.</span><span class="sxs-lookup"><span data-stu-id="4dab6-183">Confirm that you see `adftutorialgateway is connected toohello cloud service` message.</span></span> <span data-ttu-id="4dab6-184">hello alsó megjeleníti állapotsorban hello **toohello felhőszolgáltatás csatlakoztatott** együtt egy **zöld pipa**.</span><span class="sxs-lookup"><span data-stu-id="4dab6-184">hello status bar hello bottom displays **Connected toohello cloud service** along with a **green check mark**.</span></span>

    <span data-ttu-id="4dab6-185">A hello **Home** lapon is elvégezhető műveletek következő hello:</span><span class="sxs-lookup"><span data-stu-id="4dab6-185">On hello **Home** tab, you can also do hello following operations:</span></span>

   * <span data-ttu-id="4dab6-186">**Regisztrálni** egy átjárót kulccsal hello Azure-portálon hello Register gomb használatával.</span><span class="sxs-lookup"><span data-stu-id="4dab6-186">**Register** a gateway with a key from hello Azure portal by using hello Register button.</span></span>
   * <span data-ttu-id="4dab6-187">**Állítsa le** az adatkezelési átjáró gazdaszolgáltatása az átjáró gépen futó hello.</span><span class="sxs-lookup"><span data-stu-id="4dab6-187">**Stop** hello Data Management Gateway Host Service running on your gateway machine.</span></span>
   * <span data-ttu-id="4dab6-188">**Frissítések ütemezése** toobe hello nap adott időpontban telepítve.</span><span class="sxs-lookup"><span data-stu-id="4dab6-188">**Schedule updates** toobe installed at a specific time of hello day.</span></span>
   * <span data-ttu-id="4dab6-189">Megtekinthet, amikor a hello átjáró lett **utolsó frissítés**.</span><span class="sxs-lookup"><span data-stu-id="4dab6-189">View when hello gateway was **last updated**.</span></span>
   * <span data-ttu-id="4dab6-190">Adjon meg egy frissítési toohello átjárót telepítheti időpontot.</span><span class="sxs-lookup"><span data-stu-id="4dab6-190">Specify time at which an update toohello gateway can be installed.</span></span>
8. <span data-ttu-id="4dab6-191">Váltás toohello **beállítások** hello megadott külön-külön hello tanúsítvány **tanúsítvány** szakaszban használt tooencrypt/visszafejtési hello helyszíni adattár hello portal meg hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="4dab6-191">Switch toohello **Settings** tab. hello certificate specified in hello **Certificate** section is used tooencrypt/decrypt credentials for hello on-premises data store that you specify on hello portal.</span></span> <span data-ttu-id="4dab6-192">(választható) Kattintson a **módosítás** toouse saját tanúsítvány helyette.</span><span class="sxs-lookup"><span data-stu-id="4dab6-192">(optional) Click **Change** toouse your own certificate instead.</span></span> <span data-ttu-id="4dab6-193">Alapértelmezés szerint hello átjáró hello tanúsítványt használ, amelyet van a Data Factory szolgáltatásnak hello hozta létre automatikusan.</span><span class="sxs-lookup"><span data-stu-id="4dab6-193">By default, hello gateway uses hello certificate that is auto-generated by hello Data Factory service.</span></span>

    ![Átjáró tanúsítvány konfigurálása](./media/data-factory-move-data-between-onprem-and-cloud/gateway-certificate.png)

    <span data-ttu-id="4dab6-195">Azt is megteheti hello az alábbi műveleteket a hello **beállítások** lapon:</span><span class="sxs-lookup"><span data-stu-id="4dab6-195">You can also do hello following actions on hello **Settings** tab:</span></span>

   * <span data-ttu-id="4dab6-196">Megtekintése vagy hello hello átjáró által használt tanúsítvány exportálása.</span><span class="sxs-lookup"><span data-stu-id="4dab6-196">View or export hello certificate being used by hello gateway.</span></span>
   * <span data-ttu-id="4dab6-197">Módosítsa a hello átjáró által használt hello HTTPS-végponton.</span><span class="sxs-lookup"><span data-stu-id="4dab6-197">Change hello HTTPS endpoint used by hello gateway.</span></span>    
   * <span data-ttu-id="4dab6-198">Egy hello átjáró által használt HTTP-proxy toobe beállítása.</span><span class="sxs-lookup"><span data-stu-id="4dab6-198">Set an HTTP proxy toobe used by hello gateway.</span></span>     
9. <span data-ttu-id="4dab6-199">(választható) Váltás toohello **diagnosztika** fület, ellenőrizze a hello **részletes naplózás engedélyezése** lehetőséget, ha azt szeretné, tooenable részletes naplózás használható tootroubleshoot probléma merül fel hello átjáróval.</span><span class="sxs-lookup"><span data-stu-id="4dab6-199">(optional) Switch toohello **Diagnostics** tab, check hello **Enable verbose logging** option if you want tooenable verbose logging that you can use tootroubleshoot any issues with hello gateway.</span></span> <span data-ttu-id="4dab6-200">hello naplóinformációk található **Eseménynapló** alatt **alkalmazási és Szolgáltatásnaplójában** -> **az adatkezelési átjáró** csomópont.</span><span class="sxs-lookup"><span data-stu-id="4dab6-200">hello logging information can be found in **Event Viewer** under **Applications and Services Logs** -> **Data Management Gateway** node.</span></span>

    ![Diagnosztika lap](./media/data-factory-move-data-between-onprem-and-cloud/diagnostics-tab.png)

    <span data-ttu-id="4dab6-202">A következő műveletek a hello hello is végrehajthat **diagnosztika** lapon:</span><span class="sxs-lookup"><span data-stu-id="4dab6-202">You can also perform hello following actions in hello **Diagnostics** tab:</span></span>

   * <span data-ttu-id="4dab6-203">Használjon **kapcsolat tesztelése** szakasz tooan helyszíni adatforrás hello átjáró használatával.</span><span class="sxs-lookup"><span data-stu-id="4dab6-203">Use **Test Connection** section tooan on-premises data source using hello gateway.</span></span>
   * <span data-ttu-id="4dab6-204">Kattintson a **naplók megtekintése** toosee hello az adatkezelési átjáró jelentkezzen be egy Eseménynapló-ablakban.</span><span class="sxs-lookup"><span data-stu-id="4dab6-204">Click **View Logs** toosee hello Data Management Gateway log in an Event Viewer window.</span></span>
   * <span data-ttu-id="4dab6-205">Kattintson a **naplók küldése** tooupload egy zip-fájlt az elmúlt hét napban tooMicrosoft toofacilitate hibaelhárításához a problémák rögzít.</span><span class="sxs-lookup"><span data-stu-id="4dab6-205">Click **Send Logs** tooupload a zip file with logs of last seven days tooMicrosoft toofacilitate troubleshooting of your issues.</span></span>
10. <span data-ttu-id="4dab6-206">A hello **diagnosztika** lap hello **kapcsolat tesztelése** szakaszban jelölje be **SqlServer** hello adattípushoz hello tárolja, adjon meg hello hello adatbázis-kiszolgáló, neve adatbázis hello, adja meg a hitelesítés típusát, írja be a felhasználónevet és jelszót, majd kattintson **teszt** tootest e hello-átjáró képes kapcsolódni a toohello adatbázis.</span><span class="sxs-lookup"><span data-stu-id="4dab6-206">On hello **Diagnostics** tab, in hello **Test Connection** section, select **SqlServer** for hello type of hello data store, enter hello name of hello database server, name of hello database, specify authentication type, enter user name, and password, and click **Test** tootest whether hello gateway can connect toohello database.</span></span>
11. <span data-ttu-id="4dab6-207">Kapcsoló toohello webböngésző, és a hello **Azure-portálon**, kattintson a **OK** a hello **konfigurálása** lap majd a hello **az új adatátjáró** lap.</span><span class="sxs-lookup"><span data-stu-id="4dab6-207">Switch toohello web browser, and in hello **Azure portal**, click **OK** on hello **Configure** page and then on hello **New data gateway** page.</span></span>
12. <span data-ttu-id="4dab6-208">Megtekintheti az **adftutorialgateway** alatt **Data Gateways** a hello bal oldali fanézetben hello.</span><span class="sxs-lookup"><span data-stu-id="4dab6-208">You should see **adftutorialgateway** under **Data Gateways** in hello tree view on hello left.</span></span>  <span data-ttu-id="4dab6-209">A hivatkozásra, megtekintheti az hello kapcsolódó JSON.</span><span class="sxs-lookup"><span data-stu-id="4dab6-209">If you click it, you should see hello associated JSON.</span></span>

## <a name="create-linked-services"></a><span data-ttu-id="4dab6-210">Társított szolgáltatások létrehozása</span><span class="sxs-lookup"><span data-stu-id="4dab6-210">Create linked services</span></span>
<span data-ttu-id="4dab6-211">Ebben a lépésben két társított szolgáltatások létrehozásához: **AzureStorageLinkedService** és **SqlServerLinkedService**.</span><span class="sxs-lookup"><span data-stu-id="4dab6-211">In this step, you create two linked services: **AzureStorageLinkedService** and **SqlServerLinkedService**.</span></span> <span data-ttu-id="4dab6-212">Hello **SqlServerLinkedService** kapcsolat egy helyi SQL Server-adatbázis és a hello **AzureStorageLinkedService** társított szolgáltatás az Azure blob-tároló toohello adat-előállító hivatkozásokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="4dab6-212">hello **SqlServerLinkedService** links an on-premises SQL Server database and hello **AzureStorageLinkedService** linked service links an Azure blob store toohello data factory.</span></span> <span data-ttu-id="4dab6-213">Ez a forgatókönyv másol adatokat hello a helyszíni SQL Server adatbázis toohello Azure blob-tároló részében hoz létre egy folyamatot.</span><span class="sxs-lookup"><span data-stu-id="4dab6-213">You create a pipeline later in this walkthrough that copies data from hello on-premises SQL Server database toohello Azure blob store.</span></span>

#### <a name="add-a-linked-service-tooan-on-premises-sql-server-database"></a><span data-ttu-id="4dab6-214">Adja hozzá a társított szolgáltatás tooan a helyszíni SQL Server-adatbázis</span><span class="sxs-lookup"><span data-stu-id="4dab6-214">Add a linked service tooan on-premises SQL Server database</span></span>
1. <span data-ttu-id="4dab6-215">A hello **Data Factory Editor**, kattintson a **az új adattároló** hello eszköztár, és válassza a **SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="4dab6-215">In hello **Data Factory Editor**, click **New data store** on hello toolbar and select **SQL Server**.</span></span>

   ![Új kapcsolódó SQL Server szolgáltatás](./media/data-factory-move-data-between-onprem-and-cloud/NewSQLServer.png)
2. <span data-ttu-id="4dab6-217">A hello **JSON-szerkesztő** a jobb oldali hello hello lépéseket követve:</span><span class="sxs-lookup"><span data-stu-id="4dab6-217">In hello **JSON editor** on hello right, do hello following steps:</span></span>

   1. <span data-ttu-id="4dab6-218">A hello **gatewayName**, adja meg **adftutorialgateway**.</span><span class="sxs-lookup"><span data-stu-id="4dab6-218">For hello **gatewayName**, specify **adftutorialgateway**.</span></span>    
   2. <span data-ttu-id="4dab6-219">A hello **connectionString**, hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="4dab6-219">In hello **connectionString**, do hello following steps:</span></span>    

      1. <span data-ttu-id="4dab6-220">A **kiszolgálónév**, adja meg, amelyen az SQL Server-adatbázis hello hello kiszolgáló hello nevét.</span><span class="sxs-lookup"><span data-stu-id="4dab6-220">For **servername**, enter hello name of hello server that hosts hello SQL Server database.</span></span>
      2. <span data-ttu-id="4dab6-221">A **databasename**, adja meg a hello hello adatbázis nevét.</span><span class="sxs-lookup"><span data-stu-id="4dab6-221">For **databasename**, enter hello name of hello database.</span></span>
      3. <span data-ttu-id="4dab6-222">Kattintson a **titkosítása** hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="4dab6-222">Click **Encrypt** button on hello toolbar.</span></span> <span data-ttu-id="4dab6-223">Hello hitelesítőadat-kezelője alkalmazás láthatja.</span><span class="sxs-lookup"><span data-stu-id="4dab6-223">You see hello Credentials Manager application.</span></span>

         ![Hitelesítő adatok Manager alkalmazásban](./media/data-factory-move-data-between-onprem-and-cloud/credentials-manager-application.png)
      4. <span data-ttu-id="4dab6-225">A hello **hitelesítő adatok beállítása a** párbeszédpanelen adja meg a hitelesítés típusa, a felhasználónevet és jelszót, majd kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="4dab6-225">In hello **Setting Credentials** dialog box, specify authentication type, user name, and password, and click **OK**.</span></span> <span data-ttu-id="4dab6-226">Sikeres hello kapcsolat esetén hello JSON hello titkosított hitelesítő adatokat tárolja, és hello párbeszédpanel bezárul.</span><span class="sxs-lookup"><span data-stu-id="4dab6-226">If hello connection is successful, hello encrypted credentials are stored in hello JSON and hello dialog box closes.</span></span>
      5. <span data-ttu-id="4dab6-227">Zárja be, amelyek alkalmazhatók hello párbeszédpanel, ha nem automatikusan lezáródik és hello Azure-portálon toohello lap segítségnyújtáshoz hello üres böngészőlapon.</span><span class="sxs-lookup"><span data-stu-id="4dab6-227">Close hello empty browser tab that launched hello dialog box if it is not automatically closed and get back toohello tab with hello Azure portal.</span></span>

         <span data-ttu-id="4dab6-228">Hello-átjáró számítógépén, ezek a hitelesítő adatok vannak **titkosított** tanúsítvánnyal, amely a Data Factory hello szolgáltatás tulajdonosa.</span><span class="sxs-lookup"><span data-stu-id="4dab6-228">On hello gateway machine, these credentials are **encrypted** by using a certificate that hello Data Factory service owns.</span></span> <span data-ttu-id="4dab6-229">Ha azt szeretné, hogy toouse hello tanúsítványt az adatkezelési átjáró hello társított helyett, lásd: [biztonságosan használandó hitelesítő adatok beállításához](#set-credentials-and-security).</span><span class="sxs-lookup"><span data-stu-id="4dab6-229">If you want toouse hello certificate that is associated with hello Data Management Gateway instead, see [Set credentials securely](#set-credentials-and-security).</span></span>    
   3. <span data-ttu-id="4dab6-230">Kattintson a **telepítés** a hello parancssávon toodeploy hello kapcsolódó SQL Server szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="4dab6-230">Click **Deploy** on hello command bar toodeploy hello SQL Server linked service.</span></span> <span data-ttu-id="4dab6-231">Kapcsolódó hello szolgáltatás hello faszerkezetes nézetben kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="4dab6-231">You should see hello linked service in hello tree view.</span></span>

      ![Kapcsolódó SQL Server szolgáltatás hello faszerkezetes nézetben](./media/data-factory-move-data-between-onprem-and-cloud/sql-linked-service-in-tree-view.png)    

#### <a name="add-a-linked-service-for-an-azure-storage-account"></a><span data-ttu-id="4dab6-233">A társított szolgáltatás az Azure-tárfiók hozzáadása</span><span class="sxs-lookup"><span data-stu-id="4dab6-233">Add a linked service for an Azure storage account</span></span>
1. <span data-ttu-id="4dab6-234">A hello **Data Factory Editor**, kattintson a **az új adattároló** a hello parancssávon, és kattintson a **az Azure storage**.</span><span class="sxs-lookup"><span data-stu-id="4dab6-234">In hello **Data Factory Editor**, click **New data store** on hello command bar and click **Azure storage**.</span></span>
2. <span data-ttu-id="4dab6-235">Adja meg az Azure storage-fiókjának nevét hello hello **fióknév**.</span><span class="sxs-lookup"><span data-stu-id="4dab6-235">Enter hello name of your Azure storage account for hello **Account name**.</span></span>
3. <span data-ttu-id="4dab6-236">Adja meg az Azure storage-fiók kulcsát hello hello **fiókkulcs**.</span><span class="sxs-lookup"><span data-stu-id="4dab6-236">Enter hello key for your Azure storage account for hello **Account key**.</span></span>
4. <span data-ttu-id="4dab6-237">Kattintson a **telepítés** toodeploy hello **AzureStorageLinkedService**.</span><span class="sxs-lookup"><span data-stu-id="4dab6-237">Click **Deploy** toodeploy hello **AzureStorageLinkedService**.</span></span>

## <a name="create-datasets"></a><span data-ttu-id="4dab6-238">Adatkészletek létrehozása</span><span class="sxs-lookup"><span data-stu-id="4dab6-238">Create datasets</span></span>
<span data-ttu-id="4dab6-239">Ebben a lépésben létrehozni bemeneti és kimeneti adatkészletek hello másolási művelet bemeneti és kimeneti adatokat megjelenítő (a helyszíni SQL Server-adatbázis = > az Azure blob-tároló).</span><span class="sxs-lookup"><span data-stu-id="4dab6-239">In this step, you create input and output datasets that represent input and output data for hello copy operation (On-premises SQL Server database => Azure blob storage).</span></span> <span data-ttu-id="4dab6-240">Az hello adatkészletek létrehozása, előtt a következő lépéseket (részletes lépéseit követi hello lista):</span><span class="sxs-lookup"><span data-stu-id="4dab6-240">Before creating datasets, do hello following steps (detailed steps follows hello list):</span></span>

* <span data-ttu-id="4dab6-241">Hozzon létre egy táblát nevű **üres** az SQL Server-adatbázis egy kapcsolódó szolgáltatás toohello adat-előállító hozzáadott hello, majd szúrja be a minta bejegyzések hello táblába néhány.</span><span class="sxs-lookup"><span data-stu-id="4dab6-241">Create a table named **emp** in hello SQL Server Database you added as a linked service toohello data factory and insert a couple of sample entries into hello table.</span></span>
* <span data-ttu-id="4dab6-242">Hozzon létre egy blob-tároló nevű **adftutorial** a hello Azure blob storage-fiók egy kapcsolódó szolgáltatás toohello adat-előállító hozzáadott.</span><span class="sxs-lookup"><span data-stu-id="4dab6-242">Create a blob container named **adftutorial** in hello Azure blob storage account you added as a linked service toohello data factory.</span></span>

### <a name="prepare-on-premises-sql-server-for-hello-tutorial"></a><span data-ttu-id="4dab6-243">A helyszíni SQL Server hello az oktatóanyag előkészítéséhez</span><span class="sxs-lookup"><span data-stu-id="4dab6-243">Prepare On-premises SQL Server for hello tutorial</span></span>
1. <span data-ttu-id="4dab6-244">A megadott hello a helyszíni SQL Server hello adatbázis társított szolgáltatás (**SqlServerLinkedService**), használja a következő SQL-parancsfájl toocreate hello hello **üres** hello adatbázis táblájában.</span><span class="sxs-lookup"><span data-stu-id="4dab6-244">In hello database you specified for hello on-premises SQL Server linked service (**SqlServerLinkedService**), use hello following SQL script toocreate hello **emp** table in hello database.</span></span>

    ```SQL   
    CREATE TABLE dbo.emp
    (
        ID int IDENTITY(1,1) NOT NULL,
        FirstName varchar(50),
        LastName varchar(50),
        CONSTRAINT PK_emp PRIMARY KEY (ID)
    )
    GO
    ```
2. <span data-ttu-id="4dab6-245">Néhány példa beszúrása hello táblába:</span><span class="sxs-lookup"><span data-stu-id="4dab6-245">Insert some sample into hello table:</span></span>

    ```SQL
    INSERT INTO emp VALUES ('John', 'Doe')
    INSERT INTO emp VALUES ('Jane', 'Doe')
    ```

### <a name="create-input-dataset"></a><span data-ttu-id="4dab6-246">Bemeneti adatkészlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="4dab6-246">Create input dataset</span></span>

1. <span data-ttu-id="4dab6-247">A hello **Data Factory Editor**, kattintson a **... További**, kattintson a **új adatkészlet** hello parancssávon, és kattintson a **SQL Server tábla**.</span><span class="sxs-lookup"><span data-stu-id="4dab6-247">In hello **Data Factory Editor**, click **... More**, click **New dataset** on hello command bar, and click **SQL Server table**.</span></span>
2. <span data-ttu-id="4dab6-248">Cserélje le a következő szöveg hello hello JSON hello jobb oldali ablaktáblában:</span><span class="sxs-lookup"><span data-stu-id="4dab6-248">Replace hello JSON in hello right pane with hello following text:</span></span>

    ```JSON   
    {        
        "name": "EmpOnPremSQLTable",
        "properties": {
            "type": "SqlServerTable",
            "linkedServiceName": "SqlServerLinkedService",
            "typeProperties": {
                "tableName": "emp"
            },
            "external": true,
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "externalData": {
                    "retryInterval": "00:01:00",
                    "retryTimeout": "00:10:00",
                    "maximumRetry": 3
                }
            }
        }
    }     
    ```     
   <span data-ttu-id="4dab6-249">Vegye figyelembe a következő pontok hello:</span><span class="sxs-lookup"><span data-stu-id="4dab6-249">Note hello following points:</span></span>

   * <span data-ttu-id="4dab6-250">**típus** értéke túl**SqlServerTable**.</span><span class="sxs-lookup"><span data-stu-id="4dab6-250">**type** is set too**SqlServerTable**.</span></span>
   * <span data-ttu-id="4dab6-251">**tableName** értéke túl**üres**.</span><span class="sxs-lookup"><span data-stu-id="4dab6-251">**tableName** is set too**emp**.</span></span>
   * <span data-ttu-id="4dab6-252">**linkedServiceName** értéke túl**SqlServerLinkedService** (korábban létrehozott szolgáltatásnak a forgatókönyv során korábban küldje el.).</span><span class="sxs-lookup"><span data-stu-id="4dab6-252">**linkedServiceName** is set too**SqlServerLinkedService** (you had created this linked service earlier in this walkthrough.).</span></span>
   * <span data-ttu-id="4dab6-253">Egy bemeneti adatkészlet nem az Azure Data Factory egy másik folyamat által generált, be kell **külső** túl**igaz**.</span><span class="sxs-lookup"><span data-stu-id="4dab6-253">For an input dataset that is not generated by another pipeline in Azure Data Factory, you must set **external** too**true**.</span></span> <span data-ttu-id="4dab6-254">Ez azt jelzi, bemeneti adatokat hello előállított külső toohello Azure Data Factory szolgáltatásnak.</span><span class="sxs-lookup"><span data-stu-id="4dab6-254">It denotes hello input data is produced external toohello Azure Data Factory service.</span></span> <span data-ttu-id="4dab6-255">Opcionálisan megadhat bármilyen külső adatok házirendekkel hello **externalData** hello elemének **házirend** szakasz.</span><span class="sxs-lookup"><span data-stu-id="4dab6-255">You can optionally specify any external data policies using hello **externalData** element in hello **Policy** section.</span></span>    

   <span data-ttu-id="4dab6-256">Lásd: [helyezi át az adatokat az SQL Server](data-factory-sqlserver-connector.md) JSON-tulajdonságok vonatkozó további információért.</span><span class="sxs-lookup"><span data-stu-id="4dab6-256">See [Move data to/from SQL Server](data-factory-sqlserver-connector.md) for details about JSON properties.</span></span>
3. <span data-ttu-id="4dab6-257">Kattintson a **telepítés** a toodeploy hello dataset hello parancssávon.</span><span class="sxs-lookup"><span data-stu-id="4dab6-257">Click **Deploy** on hello command bar toodeploy hello dataset.</span></span>  

### <a name="create-output-dataset"></a><span data-ttu-id="4dab6-258">Kimeneti adatkészlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="4dab6-258">Create output dataset</span></span>

1. <span data-ttu-id="4dab6-259">A hello **Data Factory Editor**, kattintson a **új adatkészlet** hello parancssávon, és kattintson a **Azure Blob Storage tárolóban**.</span><span class="sxs-lookup"><span data-stu-id="4dab6-259">In hello **Data Factory Editor**, click **New dataset** on hello command bar, and click **Azure Blob storage**.</span></span>
2. <span data-ttu-id="4dab6-260">Cserélje le a következő szöveg hello hello JSON hello jobb oldali ablaktáblában:</span><span class="sxs-lookup"><span data-stu-id="4dab6-260">Replace hello JSON in hello right pane with hello following text:</span></span>

    ```JSON   
    {
        "name": "OutputBlobTable",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "adftutorial/outfromonpremdf",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
     }
    ```   
   <span data-ttu-id="4dab6-261">Vegye figyelembe a következő pontok hello:</span><span class="sxs-lookup"><span data-stu-id="4dab6-261">Note hello following points:</span></span>

   * <span data-ttu-id="4dab6-262">**típus** értéke túl**AzureBlob**.</span><span class="sxs-lookup"><span data-stu-id="4dab6-262">**type** is set too**AzureBlob**.</span></span>
   * <span data-ttu-id="4dab6-263">**linkedServiceName** értéke túl**AzureStorageLinkedService** (hozna létre szolgáltatásnak a 2. lépés).</span><span class="sxs-lookup"><span data-stu-id="4dab6-263">**linkedServiceName** is set too**AzureStorageLinkedService** (you had created this linked service in Step 2).</span></span>
   * <span data-ttu-id="4dab6-264">**folderPath** értéke túl**adftutorial/outfromonpremdf** outfromonpremdf esetén hello mappa hello adftutorial tárolóban.</span><span class="sxs-lookup"><span data-stu-id="4dab6-264">**folderPath** is set too**adftutorial/outfromonpremdf** where outfromonpremdf is hello folder in hello adftutorial container.</span></span> <span data-ttu-id="4dab6-265">Hozzon létre hello **adftutorial** tárolót, ha még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="4dab6-265">Create hello **adftutorial** container if it does not already exist.</span></span>
   * <span data-ttu-id="4dab6-266">Hello **rendelkezésre állási** értéke túl**óránkénti** (**gyakoriság** túl beállítása**óra** és **időköz** túl beállítása **1**).</span><span class="sxs-lookup"><span data-stu-id="4dab6-266">hello **availability** is set too**hourly** (**frequency** set too**hour** and **interval** set too**1**).</span></span>  <span data-ttu-id="4dab6-267">hello Data Factory szolgáltatásnak hoz létre egy kimeneti adatszelet óránként hello **üres** hello Azure SQL adatbázis táblájában.</span><span class="sxs-lookup"><span data-stu-id="4dab6-267">hello Data Factory service generates an output data slice every hour in hello **emp** table in hello Azure SQL Database.</span></span>

   <span data-ttu-id="4dab6-268">Ha nincs megadva egy **Fájlnév** a egy **eredménytábla**, hello hello létrehozott fájlokat **folderPath** formátuma a következő hello elnevezése: adatok.<Guid>. txt (például:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.).</span><span class="sxs-lookup"><span data-stu-id="4dab6-268">If you do not specify a **fileName** for an **output table**, hello generated files in hello **folderPath** are named in hello following format: Data.<Guid>.txt (for example: : Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.).</span></span>

   <span data-ttu-id="4dab6-269">tooset **folderPath** és **Fájlnév** dinamikusan alapján hello **SliceStart** idő, hello partitionedBy tulajdonsággal.</span><span class="sxs-lookup"><span data-stu-id="4dab6-269">tooset **folderPath** and **fileName** dynamically based on hello **SliceStart** time, use hello partitionedBy property.</span></span> <span data-ttu-id="4dab6-270">A következő példa hello folderPath év, hónap és nap származó hello SliceStart (kezdő időpontjának hello szelet feldolgozása folyamatban) használja, és a fájlnév óra használ a hello SliceStart.</span><span class="sxs-lookup"><span data-stu-id="4dab6-270">In hello following example, folderPath uses Year, Month, and Day from hello SliceStart (start time of hello slice being processed) and fileName uses Hour from hello SliceStart.</span></span> <span data-ttu-id="4dab6-271">Például, ha a rendszer hozott létre a szelet 2014-10-20T08:00:00, hello mappanév toowikidatagateway/wikisampledataout/2014/10/20 hello fájlnév van beállítva és too08.csv.</span><span class="sxs-lookup"><span data-stu-id="4dab6-271">For example, if a slice is being produced for 2014-10-20T08:00:00, hello folderName is set toowikidatagateway/wikisampledataout/2014/10/20 and hello fileName is set too08.csv.</span></span>

    ```JSON
    "folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
    "fileName": "{Hour}.csv",
    "partitionedBy":
    [

        { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
        { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
        { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } },
        { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } }
    ],
    ```

    <span data-ttu-id="4dab6-272">Lásd: [helyezi át az adatokat az Azure Blob Storage](data-factory-azure-blob-connector.md) JSON-tulajdonságok vonatkozó további információért.</span><span class="sxs-lookup"><span data-stu-id="4dab6-272">See [Move data to/from Azure Blob Storage](data-factory-azure-blob-connector.md) for details about JSON properties.</span></span>
3. <span data-ttu-id="4dab6-273">Kattintson a **telepítés** a toodeploy hello dataset hello parancssávon.</span><span class="sxs-lookup"><span data-stu-id="4dab6-273">Click **Deploy** on hello command bar toodeploy hello dataset.</span></span> <span data-ttu-id="4dab6-274">Győződjön meg arról, hogy megjelenik-e mindkét hello adatkészletek hello faszerkezetes nézetben.</span><span class="sxs-lookup"><span data-stu-id="4dab6-274">Confirm that you see both hello datasets in hello tree view.</span></span>  

## <a name="create-pipeline"></a><span data-ttu-id="4dab6-275">Folyamat létrehozása</span><span class="sxs-lookup"><span data-stu-id="4dab6-275">Create pipeline</span></span>
<span data-ttu-id="4dab6-276">Ebben a lépésben létrehoz egy **csővezeték** egy **másolási tevékenység** használó **EmpOnPremSQLTable** bemenetként és **OutputBlobTable** output típusúként.</span><span class="sxs-lookup"><span data-stu-id="4dab6-276">In this step, you create a **pipeline** with one **Copy Activity** that uses **EmpOnPremSQLTable** as input and **OutputBlobTable** as output.</span></span>

1. <span data-ttu-id="4dab6-277">Kattintson a Data Factory Editor **... Továbbiak**, majd az **Új adatcsatorna** elemre.</span><span class="sxs-lookup"><span data-stu-id="4dab6-277">In Data Factory Editor, click **... More**, and click **New pipeline**.</span></span>
2. <span data-ttu-id="4dab6-278">Cserélje le a következő szöveg hello hello JSON hello jobb oldali ablaktáblában:</span><span class="sxs-lookup"><span data-stu-id="4dab6-278">Replace hello JSON in hello right pane with hello following text:</span></span>    

    ```JSON   
     {
         "name": "ADFTutorialPipelineOnPrem",
         "properties": {
         "description": "This pipeline has one Copy activity that copies data from an on-prem SQL tooAzure blob",
         "activities": [
           {
             "name": "CopyFromSQLtoBlob",
             "description": "Copy data from on-prem SQL server tooblob",
             "type": "Copy",
             "inputs": [
               {
                 "name": "EmpOnPremSQLTable"
               }
             ],
             "outputs": [
               {
                 "name": "OutputBlobTable"
               }
             ],
             "typeProperties": {
               "source": {
                 "type": "SqlSource",
                 "sqlReaderQuery": "select * from emp"
               },
               "sink": {
                 "type": "BlobSink"
               }
             },
             "Policy": {
               "concurrency": 1,
               "executionPriorityOrder": "NewestFirst",
               "style": "StartOfInterval",
               "retry": 0,
               "timeout": "01:00:00"
             }
           }
         ],
         "start": "2016-07-05T00:00:00Z",
         "end": "2016-07-06T00:00:00Z",
         "isPaused": false
       }
     }
    ```   
   > [!IMPORTANT]
   > <span data-ttu-id="4dab6-279">Cserélje le a hello hello értékének **start** tulajdonságát az aktuális nap hello és **end** hello másnap értéket.</span><span class="sxs-lookup"><span data-stu-id="4dab6-279">Replace hello value of hello **start** property with hello current day and **end** value with hello next day.</span></span>
   >
   >

   <span data-ttu-id="4dab6-280">Vegye figyelembe a következő pontok hello:</span><span class="sxs-lookup"><span data-stu-id="4dab6-280">Note hello following points:</span></span>

   * <span data-ttu-id="4dab6-281">Hello tevékenységek szakaszban csak tevékenység nincs amelynek **típus** értéke túl**másolási**.</span><span class="sxs-lookup"><span data-stu-id="4dab6-281">In hello activities section, there is only activity whose **type** is set too**Copy**.</span></span>
   * <span data-ttu-id="4dab6-282">**Bemeneti** hello tevékenység túl van-e állítva a**EmpOnPremSQLTable** és **kimeneti** hello tevékenység túl van-e állítva a**OutputBlobTable**.</span><span class="sxs-lookup"><span data-stu-id="4dab6-282">**Input** for hello activity is set too**EmpOnPremSQLTable** and **output** for hello activity is set too**OutputBlobTable**.</span></span>
   * <span data-ttu-id="4dab6-283">A hello **typeProperties** szakaszban **SqlSource** hello van megadva **adatforrástípust** és ** BlobSink ** hello megadott **típusgyűjtése**.</span><span class="sxs-lookup"><span data-stu-id="4dab6-283">In hello **typeProperties** section, **SqlSource** is specified as hello **source type** and **BlobSink **is specified as hello **sink type**.</span></span>
   * <span data-ttu-id="4dab6-284">SQL-lekérdezés `select * from emp` hello megadott **sqlReaderQuery** tulajdonsága **SqlSource**.</span><span class="sxs-lookup"><span data-stu-id="4dab6-284">SQL query `select * from emp` is specified for hello **sqlReaderQuery** property of **SqlSource**.</span></span>

   <span data-ttu-id="4dab6-285">Mind a kezdő, mind a befejező dátum-időpont értéket [ISO formátumban](http://en.wikipedia.org/wiki/ISO_8601) kell megadni.</span><span class="sxs-lookup"><span data-stu-id="4dab6-285">Both start and end datetimes must be in [ISO format](http://en.wikipedia.org/wiki/ISO_8601).</span></span> <span data-ttu-id="4dab6-286">Például: 2014-10-14T16:32:41Z.</span><span class="sxs-lookup"><span data-stu-id="4dab6-286">For example: 2014-10-14T16:32:41Z.</span></span> <span data-ttu-id="4dab6-287">Hello **end** idő megadása nem kötelező, de ebben az oktatóanyagban használjuk.</span><span class="sxs-lookup"><span data-stu-id="4dab6-287">hello **end** time is optional, but we use it in this tutorial.</span></span>

   <span data-ttu-id="4dab6-288">Ha nem ad meg értéket a hello **end** tulajdonságot, akkor a program "**kezdés + 48 óra**".</span><span class="sxs-lookup"><span data-stu-id="4dab6-288">If you do not specify value for hello **end** property, it is calculated as "**start + 48 hours**".</span></span> <span data-ttu-id="4dab6-289">toorun hello folyamat határozatlan ideig, adja meg **9 9/9999** hello hello értékként **end** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="4dab6-289">toorun hello pipeline indefinitely, specify **9/9/9999** as hello value for hello **end** property.</span></span>

   <span data-ttu-id="4dab6-290">Meghatározásakor hello hello alapján mely hello adatszeletek feldolgozása akkor történik meg időtartamig **rendelkezésre állási** minden Azure Data Factory adatkészlet definiált tulajdonságaihoz.</span><span class="sxs-lookup"><span data-stu-id="4dab6-290">You are defining hello time duration in which hello data slices are processed based on hello **Availability** properties that were defined for each Azure Data Factory dataset.</span></span>

   <span data-ttu-id="4dab6-291">Hello példában amelyeket 24 adatszeletek minden adatszelet rendszer óránként készít adatszeletet.</span><span class="sxs-lookup"><span data-stu-id="4dab6-291">In hello example, there are 24 data slices as each data slice is produced hourly.</span></span>        
3. <span data-ttu-id="4dab6-292">Kattintson a **telepítés** a toodeploy hello dataset hello parancssávon (a tábla téglalap alakú dataset).</span><span class="sxs-lookup"><span data-stu-id="4dab6-292">Click **Deploy** on hello command bar toodeploy hello dataset (table is a rectangular dataset).</span></span> <span data-ttu-id="4dab6-293">Győződjön meg róla, hello faszerkezetes nézetben alapján jeleníti meg, hogy hello folyamat **folyamatok** csomópont.</span><span class="sxs-lookup"><span data-stu-id="4dab6-293">Confirm that hello pipeline shows up in hello tree view under **Pipelines** node.</span></span>  
4. <span data-ttu-id="4dab6-294">Most kattintson **X** kétszer tooclose hello lap tooget hátsó toohello **adat-előállító** hello lapján **ADFTutorialOnPremDF**.</span><span class="sxs-lookup"><span data-stu-id="4dab6-294">Now, click **X** twice tooclose hello page tooget back toohello **Data Factory** page for hello **ADFTutorialOnPremDF**.</span></span>

<span data-ttu-id="4dab6-295">**Gratulálunk!**</span><span class="sxs-lookup"><span data-stu-id="4dab6-295">**Congratulations!**</span></span> <span data-ttu-id="4dab6-296">Sikeresen létrehozott egy az Azure data factory, társított szolgáltatások, adathalmazt és egy folyamat és ütemezett hello folyamat.</span><span class="sxs-lookup"><span data-stu-id="4dab6-296">You have successfully created an Azure data factory, linked services, datasets, and a pipeline and scheduled hello pipeline.</span></span>

#### <a name="view-hello-data-factory-in-a-diagram-view"></a><span data-ttu-id="4dab6-297">A Diagram nézet nézet hello adat-előállító</span><span class="sxs-lookup"><span data-stu-id="4dab6-297">View hello data factory in a Diagram View</span></span>
1. <span data-ttu-id="4dab6-298">A hello **Azure-portálon**, kattintson a **Diagram** hello kezdőlap hello csempe **ADFTutorialOnPremDF** adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="4dab6-298">In hello **Azure portal**, click **Diagram** tile on hello home page for hello **ADFTutorialOnPremDF** data factory.</span></span> <span data-ttu-id="4dab6-299">:</span><span class="sxs-lookup"><span data-stu-id="4dab6-299">:</span></span>

    ![Diagram-hivatkozás](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDiagramLink.png)
2. <span data-ttu-id="4dab6-301">Hello diagram hasonló toohello kép a következő kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="4dab6-301">You should see hello diagram similar toohello following image:</span></span>

    ![Diagramnézet](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDiagramView.png)

    <span data-ttu-id="4dab6-303">Nagyítás, Kicsinyítés, nagyítás too100 %, nagyítás toofit, automatikusan pozíció folyamatok és adatkészleteket és Leszármaztatás adatok megjelenítése (kiemeli a kijelölt elemek felsőbb és alsóbb rétegbeli elemeket).</span><span class="sxs-lookup"><span data-stu-id="4dab6-303">You can zoom in, zoom out, zoom too100%, zoom toofit, automatically position pipelines and datasets, and show lineage information (highlights upstream and downstream items of selected items).</span></span>  <span data-ttu-id="4dab6-304">Egy objektum (bemeneti/kimeneti adatkészlet vagy csővezeték) toosee tulajdonságok az duplán kattintva.</span><span class="sxs-lookup"><span data-stu-id="4dab6-304">You can double-click an object (input/output dataset or pipeline) toosee properties for it.</span></span>

## <a name="monitor-pipeline"></a><span data-ttu-id="4dab6-305">Folyamat figyelése</span><span class="sxs-lookup"><span data-stu-id="4dab6-305">Monitor pipeline</span></span>
<span data-ttu-id="4dab6-306">Ebben a lépésben az Azure data factory lesz az Azure portál toomonitor hello használja.</span><span class="sxs-lookup"><span data-stu-id="4dab6-306">In this step, you use hello Azure portal toomonitor what’s going on in an Azure data factory.</span></span> <span data-ttu-id="4dab6-307">PowerShell-parancsmagok toomonitor adathalmazok és adatcsatornák is használható.</span><span class="sxs-lookup"><span data-stu-id="4dab6-307">You can also use PowerShell cmdlets toomonitor datasets and pipelines.</span></span> <span data-ttu-id="4dab6-308">Figyelésével kapcsolatos részletekért lásd: [figyelő és folyamatok kezelése](data-factory-monitor-manage-pipelines.md).</span><span class="sxs-lookup"><span data-stu-id="4dab6-308">For details about monitoring, see [Monitor and Manage Pipelines](data-factory-monitor-manage-pipelines.md).</span></span>

1. <span data-ttu-id="4dab6-309">Hello diagramon kattintson duplán **EmpOnPremSQLTable**.</span><span class="sxs-lookup"><span data-stu-id="4dab6-309">In hello diagram, double-click **EmpOnPremSQLTable**.</span></span>  

    ![EmpOnPremSQLTable szeletek](./media/data-factory-move-data-between-onprem-and-cloud/OnPremSQLTableSlicesBlade.png)
2. <span data-ttu-id="4dab6-311">Figyelje meg, hogy minden hello adatszelet fel vannak **készen** állapot, mert a korábbi hello hello folyamat időtartama (kezdési idő tooend idő).</span><span class="sxs-lookup"><span data-stu-id="4dab6-311">Notice that all hello data slices up are in **Ready** state because hello pipeline duration (start time tooend time) is in hello past.</span></span> <span data-ttu-id="4dab6-312">Akkor is mert hello adatok beszúrt hello SQL Server-adatbázisban, és nincs minden hello idő.</span><span class="sxs-lookup"><span data-stu-id="4dab6-312">It is also because you have inserted hello data in hello SQL Server database and it is there all hello time.</span></span> <span data-ttu-id="4dab6-313">Győződjön meg arról, hogy nincs szeletek megjelenni hello **probléma szeletek** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="4dab6-313">Confirm that no slices show up in hello **Problem slices** section at hello bottom.</span></span> <span data-ttu-id="4dab6-314">tooview összes hello szeletek, kattintson a **lásd: több** szeletek hello listája hello alján.</span><span class="sxs-lookup"><span data-stu-id="4dab6-314">tooview all hello slices, click **See More** at hello bottom of hello list of slices.</span></span>
3. <span data-ttu-id="4dab6-315">Most, a hello **adatkészletek** kattintson **OutputBlobTable**.</span><span class="sxs-lookup"><span data-stu-id="4dab6-315">Now, In hello **Datasets** page, click **OutputBlobTable**.</span></span>

    ![OputputBlobTable szeletek](./media/data-factory-move-data-between-onprem-and-cloud/OutputBlobTableSlicesBlade.png)
4. <span data-ttu-id="4dab6-317">Bármely adatszelet hello listából kattintson, és megtekintheti az hello **Adatszelet** lap.</span><span class="sxs-lookup"><span data-stu-id="4dab6-317">Click any data slice from hello list and you should see hello **Data Slice** page.</span></span> <span data-ttu-id="4dab6-318">Megjelenik a tevékenység futtatása hello szelet.</span><span class="sxs-lookup"><span data-stu-id="4dab6-318">You see activity runs for hello slice.</span></span> <span data-ttu-id="4dab6-319">Csak egy tevékenységfuttatási általában láthatja.</span><span class="sxs-lookup"><span data-stu-id="4dab6-319">You see only one activity run usually.</span></span>  

    ![Adatok szelet panel](./media/data-factory-move-data-between-onprem-and-cloud/DataSlice.png)

    <span data-ttu-id="4dab6-321">Ha hello szelet nincs hello **készen** állapot, hello felfelé irányuló szeletek nem készen áll, és blokkol hello aktuális szelet hello végrehajtás alatt látható **felfelé irányuló szeletek nem áll készen** listája.</span><span class="sxs-lookup"><span data-stu-id="4dab6-321">If hello slice is not in hello **Ready** state, you can see hello upstream slices that are not Ready and are blocking hello current slice from executing in hello **Upstream slices that are not ready** list.</span></span>
5. <span data-ttu-id="4dab6-322">Kattintson a hello **tevékenységfuttatási** el hello alsó toosee hello listáról **tevékenység fut részletek**.</span><span class="sxs-lookup"><span data-stu-id="4dab6-322">Click hello **activity run** from hello list at hello bottom toosee **activity run details**.</span></span>

   ![Tevékenység futtatása részleteit megjelenítő oldalra](./media/data-factory-move-data-between-onprem-and-cloud/ActivityRunDetailsBlade.png)

   <span data-ttu-id="4dab6-324">Például az átviteli sebesség időtartama és hello átjáró használt tootransfer hello adatok információkat mutatunk be.</span><span class="sxs-lookup"><span data-stu-id="4dab6-324">You would see information such as throughput, duration, and hello gateway used tootransfer hello data.</span></span>
6. <span data-ttu-id="4dab6-325">Kattintson a **X** tooclose összes hello lapokat, amíg ki nem</span><span class="sxs-lookup"><span data-stu-id="4dab6-325">Click **X** tooclose all hello pages until you</span></span>
7. <span data-ttu-id="4dab6-326">vissza a toohello kezdőlapja a hello **ADFTutorialOnPremDF**.</span><span class="sxs-lookup"><span data-stu-id="4dab6-326">get back toohello home page for hello **ADFTutorialOnPremDF**.</span></span>
8. <span data-ttu-id="4dab6-327">(választható) Kattintson a **folyamatok**, kattintson a **ADFTutorialOnPremDF**, és a bemeneti tábla áthatoló részletezést (**felhasznált**) vagy a kimeneti adathalmazokat (**Produced**).</span><span class="sxs-lookup"><span data-stu-id="4dab6-327">(optional) Click **Pipelines**, click **ADFTutorialOnPremDF**, and drill through input tables (**Consumed**) or output datasets (**Produced**).</span></span>
9. <span data-ttu-id="4dab6-328">Használjon például az eszközök [Microsoft Tártallózó](http://storageexplorer.com/) tooverify, hogy minden órában létrejön egy blob vagy fájl.</span><span class="sxs-lookup"><span data-stu-id="4dab6-328">Use tools such as [Microsoft Storage Explorer](http://storageexplorer.com/) tooverify that a blob/file is created for each hour.</span></span>

   ![Azure Storage Explorer](./media/data-factory-move-data-between-onprem-and-cloud/OnPremAzureStorageExplorer.png)

## <a name="next-steps"></a><span data-ttu-id="4dab6-330">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4dab6-330">Next steps</span></span>
* <span data-ttu-id="4dab6-331">Lásd: [az adatkezelési átjáró](data-factory-data-management-gateway.md) a következő cikket: az összes hello hello az adatkezelési átjáró adatait.</span><span class="sxs-lookup"><span data-stu-id="4dab6-331">See [Data Management Gateway](data-factory-data-management-gateway.md) article for all hello details about hello Data Management Gateway.</span></span>
* <span data-ttu-id="4dab6-332">Lásd: [adatok másolása az Azure Blob tooAzure SQL](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) toolearn hogyan toouse másolási tevékenység toomove a forrásadatok adattároló tooa fogadó adattár kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="4dab6-332">See [Copy data from Azure Blob tooAzure SQL](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) toolearn about how toouse Copy Activity toomove data from a source data store tooa sink data store.</span></span>
