---
<span data-ttu-id="afa07-101">cím: aaa "Azure Analysis Services-oktatóanyag lecke 13: telepítése |} Microsoft Docs"Leírás: ismerteti, hogyan toodeploy hello oktatóanyag projekt tooAzure Analysis Services.</span><span class="sxs-lookup"><span data-stu-id="afa07-101">title: aaa"Azure Analysis Services tutorial lesson 13: Deploy | Microsoft Docs" description:  Describes how toodeploy hello tutorial project tooAzure Analysis Services.</span></span>
<span data-ttu-id="afa07-102">szolgáltatások: analysis-szolgáltatások documentationcenter: "Szerző: minewiskan manager: erikre szerkesztőben:" címkék: "</span><span class="sxs-lookup"><span data-stu-id="afa07-102">services: analysis-services documentationcenter: '' author: minewiskan manager: erikre editor: '' tags: ''</span></span>

<span data-ttu-id="afa07-103">MS.AssetId: ms.service: analysis-szolgáltatások ms.devlang: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 07/17/2017 ms.author: owend</span><span class="sxs-lookup"><span data-stu-id="afa07-103">ms.assetid: ms.service: analysis-services ms.devlang: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 07/17/2017 ms.author: owend</span></span>
---
# <a name="lesson-13-deploy"></a><span data-ttu-id="afa07-104">13. lecke: Üzembe helyezés</span><span class="sxs-lookup"><span data-stu-id="afa07-104">Lesson 13: Deploy</span></span>

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

<span data-ttu-id="afa07-105">Ez a lecke konfigurál központi telepítési tulajdonságok; Adja meg az Azure Analysis Services kiszolgáló toodeploy tooand hello modell nevét.</span><span class="sxs-lookup"><span data-stu-id="afa07-105">In this lesson, you configure deployment properties; specifying an Azure Analysis Services server toodeploy tooand a name for hello model.</span></span> <span data-ttu-id="afa07-106">Ezután hello modell toothat példányát telepíti.</span><span class="sxs-lookup"><span data-stu-id="afa07-106">You then deploy hello model toothat instance.</span></span> <span data-ttu-id="afa07-107">A minta telepítése után a felhasználók kapcsolódhatnak tooit egy jelentéskészítő ügyfél alkalmazás használatával.</span><span class="sxs-lookup"><span data-stu-id="afa07-107">After your model is deployed, users can connect tooit by using a reporting client application.</span></span> <span data-ttu-id="afa07-108">több, lásd: toolearn [tooAzure Analysis Services telepítése](https://docs.microsoft.com/azure/analysis-services/analysis-services-deploy).</span><span class="sxs-lookup"><span data-stu-id="afa07-108">toolearn more, see [Deploy tooAzure Analysis Services](https://docs.microsoft.com/azure/analysis-services/analysis-services-deploy).</span></span>  
  
<span data-ttu-id="afa07-109">Becsült idő toocomplete Ez a lecke: **5 perc**</span><span class="sxs-lookup"><span data-stu-id="afa07-109">Estimated time toocomplete this lesson: **5 minutes**</span></span>  
  
## <a name="prerequisites"></a><span data-ttu-id="afa07-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="afa07-110">Prerequisites</span></span>  
<span data-ttu-id="afa07-111">Ez a témakör a táblázatos modellezéssel foglalkozó oktatóanyag részét képezi, amelyet a megfelelő sorrendben kell elvégezni.</span><span class="sxs-lookup"><span data-stu-id="afa07-111">This topic is part of a tabular modeling tutorial, which should be completed in order.</span></span> <span data-ttu-id="afa07-112">Ez a lecke hello feladatok elvégzése előtt kell befejeződött hello előző lecke: [lecke 12: elemzés az Excelben](../tutorials/aas-lesson-12-analyze-in-excel.md).</span><span class="sxs-lookup"><span data-stu-id="afa07-112">Before performing hello tasks in this lesson, you should have completed hello previous lesson: [Lesson 12: Analyze in Excel](../tutorials/aas-lesson-12-analyze-in-excel.md).</span></span>  

> [!IMPORTANT]  
> <span data-ttu-id="afa07-113">Rendelkeznie kell [rendszergazdai jogosultságokkal](../analysis-services-server-admins.md) on hello távoli Analysis Services kiszolgáló sorrendben toodeploy tooit.</span><span class="sxs-lookup"><span data-stu-id="afa07-113">You must have [Administrator permissions](../analysis-services-server-admins.md) on hello remote Analysis Services server in-order toodeploy tooit.</span></span>  

> [!IMPORTANT]  
> <span data-ttu-id="afa07-114">Ha telepítette a hello AdventureWorksDW2014 mintaadatbázis egy helyi SQL-kiszolgálón, és telepítése esetén a modell tooan Azure Analysis Services-kiszolgáló, egy [helyszíni adatátjáró](../analysis-services-gateway.md) szükséges.</span><span class="sxs-lookup"><span data-stu-id="afa07-114">If you installed hello AdventureWorksDW2014 sample database on an on-premises SQL Server, and you're deploying your model tooan Azure Analysis Services server, an [On-premises data gateway](../analysis-services-gateway.md) is required.</span></span>
  
## <a name="deploy-hello-model"></a><span data-ttu-id="afa07-115">Hello modell rendszerbe állítása</span><span class="sxs-lookup"><span data-stu-id="afa07-115">Deploy hello model</span></span>  
  
#### <a name="tooconfigure-deployment-properties"></a><span data-ttu-id="afa07-116">tooconfigure központi telepítési tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="afa07-116">tooconfigure deployment properties</span></span>  

  
1.  <span data-ttu-id="afa07-117">A **Megoldáskezelőben**, kattintson a jobb gombbal hello **AW Internet értékesítési** projektre, és kattintson a **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="afa07-117">In **Solution Explorer**, right-click hello **AW Internet Sales** project, and then click **Properties**.</span></span>  
  
2.  <span data-ttu-id="afa07-118">A hello **AW Internet értékesítési tulajdonságlapjain** párbeszédpanel **rendszerbe állítási kiszolgáló**, a hello **Server** tulajdonság, adja meg a teljes kiszolgáló hello.</span><span class="sxs-lookup"><span data-stu-id="afa07-118">In hello **AW Internet Sales Property Pages** dialog box, under **Deployment Server**, in hello **Server** property, enter hello full server.</span></span>  

    ![aas-lesson13-deploy-property](../tutorials/media/aas-lesson13-deploy-property.png)
  
3.  <span data-ttu-id="afa07-120">A hello **adatbázis** tulajdonság, írja be **Adventure Works Internet értékesítési**.</span><span class="sxs-lookup"><span data-stu-id="afa07-120">In hello **Database** property, type **Adventure Works Internet Sales**.</span></span>  
  
4.  <span data-ttu-id="afa07-121">A hello **modellnév** tulajdonság, írja be **Adventure Works internetes értékesítési modell**.</span><span class="sxs-lookup"><span data-stu-id="afa07-121">In hello **Model Name** property, type **Adventure Works Internet Sales Model**.</span></span>  
  
5.  <span data-ttu-id="afa07-122">Ellenőrizze a beállításokat, majd kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="afa07-122">Verify your selections and then click **OK**.</span></span>  
  
#### <a name="toodeploy-hello-adventure-works-internet-sales"></a><span data-ttu-id="afa07-123">az Adventure Works Internet értékesítési toodeploy hello</span><span class="sxs-lookup"><span data-stu-id="afa07-123">toodeploy hello Adventure Works Internet Sales</span></span>
  
1.  <span data-ttu-id="afa07-124">A **Megoldáskezelőben**, kattintson a jobb gombbal hello **AW Internet értékesítési** project > **Build**.</span><span class="sxs-lookup"><span data-stu-id="afa07-124">In **Solution Explorer**, right-click hello **AW Internet Sales** project > **Build**.</span></span>  

2.  <span data-ttu-id="afa07-125">Kattintson a jobb gombbal hello **AW Internet értékesítési** project > **telepítés**.</span><span class="sxs-lookup"><span data-stu-id="afa07-125">Right-click hello **AW Internet Sales** project > **Deploy**.</span></span>

    <span data-ttu-id="afa07-126">TooAzure Analysis Services való telepítésekor, akkor előfordulhat, hogy a fióknak kell lennie a kért tooenter.</span><span class="sxs-lookup"><span data-stu-id="afa07-126">When deploying tooAzure Analysis Services, you may be prompted tooenter your account.</span></span> <span data-ttu-id="afa07-127">Adja meg céges fiókját és jelszavát, például: nancy@adventureworks.com. Ennek a fióknak kell lennie a rendszergazdák hello kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="afa07-127">Enter your organizational account and password, for example nancy@adventureworks.com. This account must be in Admins on hello server.</span></span>
  
    <span data-ttu-id="afa07-128">hello telepítés párbeszédpanel jelenik meg, és hello metaadatok hello központi telepítési állapotának és hello modellben szereplő mindegyik tábla jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="afa07-128">hello Deploy dialog box appears and displays hello deployment status of hello metadata and each table included in hello model.</span></span>  
    
    ![aas-lesson13-deploy-status](../tutorials/media/aas-lesson13-deploy-status.png)
  
3. <span data-ttu-id="afa07-130">Miután az üzembe helyezés sikeresen befejeződött, nyugodtan kattintson a **Bezárás** gombra.</span><span class="sxs-lookup"><span data-stu-id="afa07-130">When deployment successfully completes, go ahead and click **Close**.</span></span>  
  
## <a name="conclusion"></a><span data-ttu-id="afa07-131">Összegzés</span><span class="sxs-lookup"><span data-stu-id="afa07-131">Conclusion</span></span>  
<span data-ttu-id="afa07-132">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="afa07-132">Congratulations!</span></span> <span data-ttu-id="afa07-133">Végzett az első Analysis Services-beli táblázatos modellje elkészítésével és üzembe helyezésével.</span><span class="sxs-lookup"><span data-stu-id="afa07-133">You're finished authoring and deploying your first Analysis Services Tabular model.</span></span> <span data-ttu-id="afa07-134">Ez az oktatóanyag hozzájárult, amelyek hello leggyakoribb feladatokat egy táblázatos modell létrehozásának befejezése.</span><span class="sxs-lookup"><span data-stu-id="afa07-134">This tutorial has helped guide you through completing hello most common tasks in creating a tabular model.</span></span> <span data-ttu-id="afa07-135">Most, hogy az Adventure Works Internet értékesítési modellje telepít, a rendszer csak akkor használhatja az SQL Server Management Studio toomanage hello; folyamat parancsfájlok és biztonsági mentési terv létrehozása.</span><span class="sxs-lookup"><span data-stu-id="afa07-135">Now that your Adventure Works Internet Sales model is deployed, you can use SQL Server Management Studio toomanage hello model; create process scripts and a backup plan.</span></span> <span data-ttu-id="afa07-136">Felhasználók mostantól is kapcsolódhatnak toohello modell jelentéskészítési ügyfél alkalmazások, például a Microsoft Excel alkalmazást vagy a Power BI segítségével.</span><span class="sxs-lookup"><span data-stu-id="afa07-136">Users can also now connect toohello model using a reporting client application such as Microsoft Excel or Power BI.</span></span>  

![aas-lesson13-ssms](../tutorials/media/aas-lesson13-ssms.png)
  
  
  
## <a name="whats-next"></a><span data-ttu-id="afa07-138">A következő lépések</span><span class="sxs-lookup"><span data-stu-id="afa07-138">What's next?</span></span>
<span data-ttu-id="afa07-139">[Kapcsolódás Power BI Desktoppal](../analysis-services-connect-pbi.md) </span><span class="sxs-lookup"><span data-stu-id="afa07-139">[Connect with Power BI Desktop](../analysis-services-connect-pbi.md) </span></span>  
<span data-ttu-id="afa07-140">[Kiegészítő lecke – Dinamikus biztonság](../tutorials/aas-supplemental-lesson-dynamic-security.md) </span><span class="sxs-lookup"><span data-stu-id="afa07-140">[Supplemental Lesson - Dynamic security](../tutorials/aas-supplemental-lesson-dynamic-security.md) </span></span>  
<span data-ttu-id="afa07-141">[Kiegészítő lecke – Részletsorok](../tutorials/aas-supplemental-lesson-detail-rows.md) </span><span class="sxs-lookup"><span data-stu-id="afa07-141">[Supplemental Lesson - Detail rows](../tutorials/aas-supplemental-lesson-detail-rows.md) </span></span>  
[<span data-ttu-id="afa07-142">Kiegészítő lecke – Hézagos hierarchiák</span><span class="sxs-lookup"><span data-stu-id="afa07-142">Supplemental Lesson - Ragged hierarchies</span></span>](../tutorials/aas-supplemental-lesson-ragged-hierarchies.md)   
