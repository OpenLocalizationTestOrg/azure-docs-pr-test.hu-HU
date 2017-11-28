---
title: "A Mcirosoft Power BI Embedded bemutatása"
description: "Power BI Embedded, interaktív Power BI-jelentések felvétele üzletiintelligencia-alkalmazásokba"
services: power-bi-embedded
documentationcenter: 
author: guyinacube
manager: erikre
editor: 
tags: 
ms.assetid: 4787cf44-5d1c-4bc3-b3fd-bf396e5c1176
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 03/11/2017
ms.author: asaxton
ms.openlocfilehash: 4afa8d2c7f8ec1942521ba5fa131967dfd581c91
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-microsoft-power-bi-embedded"></a><span data-ttu-id="909a6-103">A Mcirosoft Power BI Embedded bemutatása</span><span class="sxs-lookup"><span data-stu-id="909a6-103">Get started with Microsoft Power BI Embedded</span></span>

<span data-ttu-id="909a6-104">Az Azure-szolgáltatások körébe tartozó **Power BI Embedded** segítségével az alkalmazásfejlesztők interaktív Power BI-jelentéseket vehetnek fel saját alkalmazásaikba.</span><span class="sxs-lookup"><span data-stu-id="909a6-104">**Power BI Embedded** is an Azure service that enables application developers to add interactive Power BI reports into their own applications.</span></span> <span data-ttu-id="909a6-105">A **Power BI Embedded** úgy működik együtt a meglévő alkalmazásokkal, hogy nem kell újratervezni vagy módosítani a felhasználók bejelentkezésének módját.</span><span class="sxs-lookup"><span data-stu-id="909a6-105">**Power BI Embedded** works with existing applications without needing redesign or changing the way users sign in.</span></span>

<span data-ttu-id="909a6-106">A **Microsoft Power BI Embedded** erőforrásai az [Azure ARM API-k](https://msdn.microsoft.com/library/mt712306.aspx) használatával hozhatók létre.</span><span class="sxs-lookup"><span data-stu-id="909a6-106">Resources for **Microsoft Power BI Embedded** are provisioned through the [Azure ARM APIs](https://msdn.microsoft.com/library/mt712306.aspx).</span></span> <span data-ttu-id="909a6-107">Ebben az esetben a létrehozott erőforrás egy **Power BI munkaterület-csoport**.</span><span class="sxs-lookup"><span data-stu-id="909a6-107">In this case, the resource you provision is a **Power BI Workspace Collection**.</span></span>

![](media/power-bi-embedded-get-started/introduction.png)

## <a name="create-a-workspace-collection"></a><span data-ttu-id="909a6-108">Munkaterület-csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="909a6-108">Create a workspace collection</span></span>

<span data-ttu-id="909a6-109">A **munkaterület-csoportok** a legfelsőbb szintű Azure-erőforrások, melyek az alkalmazásba beágyazandó tartalmat tárolják.</span><span class="sxs-lookup"><span data-stu-id="909a6-109">A **Workspace Collection** is the top-level Azure resource and a container for the content that will be embedded in your application.</span></span> <span data-ttu-id="909a6-110">A **munkaterület-csoportok** kétféleképpen hozhatók létre:</span><span class="sxs-lookup"><span data-stu-id="909a6-110">A **Workspace Collection** can be created in two ways::</span></span>

* <span data-ttu-id="909a6-111">Manuálisan, az Azure Portal segítségével</span><span class="sxs-lookup"><span data-stu-id="909a6-111">Manually using the Azure Portal</span></span>
* <span data-ttu-id="909a6-112">Programozott módon, Azure Resource Manager (ARM) API-k segítségével</span><span class="sxs-lookup"><span data-stu-id="909a6-112">Programmatically using the Azure Resource Manager(ARM) APIs</span></span>

<span data-ttu-id="909a6-113">Az alábbiakban lépésről lépésre bemutatjuk, hogyan hozhat létre egy **munkaterület-csoportot** az Azure Portal segítségével.</span><span class="sxs-lookup"><span data-stu-id="909a6-113">Let's walk through the steps to build a **Workspace Collection** using the Azure Portal.</span></span>

1. <span data-ttu-id="909a6-114">Nyissa meg az **Azure Portalt** és jelentkezzen be: [http://portal.azure.com](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="909a6-114">Open and sign into **Azure Portal**: [http://portal.azure.com](http://portal.azure.com).</span></span>
2. <span data-ttu-id="909a6-115">Kattintson a **+Új** lehetőségre az ablak tetején.</span><span class="sxs-lookup"><span data-stu-id="909a6-115">Click **+ New** on the top panel.</span></span>
   
   ![](media/power-bi-embedded-get-started/create-workspace-1.png)
3. <span data-ttu-id="909a6-116">Az **Adatok + analitika** területen kattintson a **Power BI Embedded** elemre.</span><span class="sxs-lookup"><span data-stu-id="909a6-116">Under **Data + Analytics** click **Power BI Embedded**.</span></span>
4. <span data-ttu-id="909a6-117">A **Munkaterület-csoport panelen** adja meg a szükséges információkat.</span><span class="sxs-lookup"><span data-stu-id="909a6-117">On the **Workspace Collection Blade**, enter the required information.</span></span> <span data-ttu-id="909a6-118">A **Díjszabásról** további információt [A Power BI Embedded díjszabása](http://go.microsoft.com/fwlink/?LinkID=760527) című témakörben talál.</span><span class="sxs-lookup"><span data-stu-id="909a6-118">For **Pricing**, see [Power BI Embedded pricing](http://go.microsoft.com/fwlink/?LinkID=760527).</span></span>
   
   ![](media/power-bi-embedded-get-started/create-workspace-2.png)
5. <span data-ttu-id="909a6-119">Kattintson a **Létrehozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="909a6-119">Click **Create**.</span></span>

<span data-ttu-id="909a6-120">A **munkaterület-csoport** pár percen belül létrejön.</span><span class="sxs-lookup"><span data-stu-id="909a6-120">The **Workspace Collection** will take a few moments to provision.</span></span> <span data-ttu-id="909a6-121">Ha ez elkészült, megnyílik a **Munkaterület-csoport panel**.</span><span class="sxs-lookup"><span data-stu-id="909a6-121">When completed, you'll be taken to the **Workspace Collection Blade**.</span></span>

   ![](media/power-bi-embedded-get-started/create-workspace-3.png)

<span data-ttu-id="909a6-122">A **Létrehozás panelen** találhatók a munkaterületek létrehozásáért és a tartalom telepítéséért felelős API-k meghívásához szükséges adatok.</span><span class="sxs-lookup"><span data-stu-id="909a6-122">The **Creation Blade** contains the information you need to call the APIs that create workspaces and deploy content to them.</span></span>

<a name="view-access-keys"/>

## <a name="view-power-bi-api-access-keys"></a><span data-ttu-id="909a6-123">A Power BI API elérési kulcsainak megtekintése</span><span class="sxs-lookup"><span data-stu-id="909a6-123">View Power BI API access keys</span></span>

<span data-ttu-id="909a6-124">A Power BI REST API-k meghívásához szükséges legfontosabb információt az **elérési kulcsok** jelentik.</span><span class="sxs-lookup"><span data-stu-id="909a6-124">One of the most important pieces of information needed to call the Power BI REST APIs are the **Access Keys**.</span></span> <span data-ttu-id="909a6-125">Segítségükkel lehetséges ugyanis az API-kérelmek hitelesítéséhez szükséges **alkalmazási jogkivonatok** létrehozása.</span><span class="sxs-lookup"><span data-stu-id="909a6-125">These are used to generate the **app tokens** that are used to authenticate your API requests.</span></span> <span data-ttu-id="909a6-126">Az **elérési kulcsok** megtekintéséhez a **Beállítások panelen** kattintson az **Elérési kulcsok** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="909a6-126">To view your **Access Keys**, click **Access Keys** on the **Settings Blade**.</span></span> <span data-ttu-id="909a6-127">További információk az **alkalmazás-jogkivonatokról**: [Authenticating and authorizing with Power BI Embedded](power-bi-embedded-app-token-flow.md) (Hitelesítés és engedélyezés a Power BI Embedded használatával).</span><span class="sxs-lookup"><span data-stu-id="909a6-127">For more about **app tokens**, see [Authenticating and authorizing with Power BI Embedded](power-bi-embedded-app-token-flow.md).</span></span>

   ![](media/power-bi-embedded-get-started/access-keys.png)

<span data-ttu-id="909a6-128">Láthatja, hogy két kulccsal rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="909a6-128">You'll'notice that you have two keys.</span></span>

   ![](media/power-bi-embedded-get-started/access-keys-2.png)

<span data-ttu-id="909a6-129">Készítsen másolatot a kulcsokról, és tárolja őket biztonságos helyen az alkalmazáson belül.</span><span class="sxs-lookup"><span data-stu-id="909a6-129">Copy these keys and store them securely in your application.</span></span> <span data-ttu-id="909a6-130">Nagyon fontos, hogy ezeket a kulcsokat úgy kezelje, mintha jelszavak volnának, mivel a segítségükkel a **munkaterület-csoport** minden tartalmához hozzá lehet férni.</span><span class="sxs-lookup"><span data-stu-id="909a6-130">It's very important that you treat these keys as you would a password, because they'll provide access to all the content in your **Workspace Collection**.</span></span>

<span data-ttu-id="909a6-131">Jóllehet két kulcs van listázva, egyszerre azonban mindig csak az egyikre lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="909a6-131">While two keys are listed, only one key is needed at a particular time.</span></span> <span data-ttu-id="909a6-132">A második kulcsra azért van szükség, hogy a kulcsok rendszeres újragenerálása ne zavarja meg a szolgáltatáshoz való hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="909a6-132">The second key is provided so you can periodically regenerate keys without interrupting access to the service.</span></span>

<span data-ttu-id="909a6-133">Most, hogy rendelkezik **elérési kulcsokkal** és az alkalmazáshoz való Power BI-példánnyal, jelentést importálhat az alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="909a6-133">Now that you have an instance of Power BI for your application, and **Access Keys**, you can import a report into your own app.</span></span> <span data-ttu-id="909a6-134">Mielőtt megismertetnénk a jelentések importálásának módjával, a következő szakasz azt mutatja be, hogyan hozhatók létre az alkalmazásba beágyazandó Power BI-adatkészletek és jelentések.</span><span class="sxs-lookup"><span data-stu-id="909a6-134">Before you learn how to import a report, the next section describes creating Power BI datasets and reports to embed into an app.</span></span>

## <a name="working-with-workspaces"></a><span data-ttu-id="909a6-135">A munkaterületek használata</span><span class="sxs-lookup"><span data-stu-id="909a6-135">Working with workspaces</span></span>

<span data-ttu-id="909a6-136">Munkaterület-csoport létrehozása után létre kell hoznia egy olyan munkaterületet, amely helyet biztosít a jelentéseknek és az adatkészleteknek.</span><span class="sxs-lookup"><span data-stu-id="909a6-136">After you have created your workspace collection, you will need to create a workspace that will house your reports and datasets.</span></span> <span data-ttu-id="909a6-137">Munkaterület létrehozásához szüksége lesz a [Munkaterület küldése REST API](https://msdn.microsoft.com/library/azure/mt711503.aspx)-ra.</span><span class="sxs-lookup"><span data-stu-id="909a6-137">To create a workspace, you will need to use the [Post Worksapce REST API](https://msdn.microsoft.com/library/azure/mt711503.aspx).</span></span>

## <a name="create-power-bi-datasets-and-reports-to-embed-into-an-app-using-power-bi-desktop"></a><span data-ttu-id="909a6-138">Power BI-adatkészletek és -jelentések létrehozása és beágyazása egy Power BI Desktopot használó alkalmazásba</span><span class="sxs-lookup"><span data-stu-id="909a6-138">Create Power BI datasets and reports to embed into an app using Power BI Desktop</span></span>

<span data-ttu-id="909a6-139">Most, hogy rendelkezik **elérési kulcsokkal**, és létrehozta az alkalmazáshoz való Power BI-példányt, el kell készítenie a beágyazni kívánt Power BI-adatkészleteket és jelentéseket.</span><span class="sxs-lookup"><span data-stu-id="909a6-139">Now that you have created an instance of Power BI for your application, and have **Access Keys**, you will need to create the Power BI datasets and reports that you want to embed.</span></span> <span data-ttu-id="909a6-140">Az adatkészletek és a jelentések a **Power BI Desktop** segítségével hozhatók létre.</span><span class="sxs-lookup"><span data-stu-id="909a6-140">Datasets and reports can be created by using **Power BI Desktop**.</span></span> <span data-ttu-id="909a6-141">A [Power BI Desktop ingyenesen](https://go.microsoft.com/fwlink/?LinkId=521662) letölthető.</span><span class="sxs-lookup"><span data-stu-id="909a6-141">You can download [Power BI Desktop for free](https://go.microsoft.com/fwlink/?LinkId=521662).</span></span> <span data-ttu-id="909a6-142">Vagy, a gyorsabb kezdéshez, letöltheti a [Kiskereskedelmi elemzési minta PBIX-fájlt](http://go.microsoft.com/fwlink/?LinkID=780547).</span><span class="sxs-lookup"><span data-stu-id="909a6-142">Or, to quickly get started, you can download the [Retail Analysis Sample PBIX](http://go.microsoft.com/fwlink/?LinkID=780547).</span></span>

> [!NOTE]
> <span data-ttu-id="909a6-143">A **Power BI Desktop** működéséről további információt a [Bevezetés a Power BI Desktop használatába](https://powerbi.microsoft.com/guided-learning/powerbi-learning-0-2-get-started-power-bi-desktop) című témakörben talál.</span><span class="sxs-lookup"><span data-stu-id="909a6-143">To learn more about how to use **Power BI Desktop**, see [Getting Started with Power BI Desktop](https://powerbi.microsoft.com/guided-learning/powerbi-learning-0-2-get-started-power-bi-desktop).</span></span>

<span data-ttu-id="909a6-144">A **Power BI Desktop** segítségével úgy csatlakozhat az adatforráshoz, hogy a **Power BI Desktopba** importálja az adatok másolatát, de a **DirectQueryt** is használhatja ugyanerre a célra.</span><span class="sxs-lookup"><span data-stu-id="909a6-144">With **Power BI Desktop**, you connect to your data source by importing a copy of the data into **Power BI Desktop** or connecting directly to the data source using **DirectQuery**.</span></span>

<span data-ttu-id="909a6-145">Az alábbiakban a két módszer (**importálás** és **DirectQuery**) közti különbségeket láthatja.</span><span class="sxs-lookup"><span data-stu-id="909a6-145">Here are the differences between using **Import** and **DirectQuery**.</span></span>

| <span data-ttu-id="909a6-146">Importálás</span><span class="sxs-lookup"><span data-stu-id="909a6-146">Import</span></span> | <span data-ttu-id="909a6-147">DirectQuery</span><span class="sxs-lookup"><span data-stu-id="909a6-147">DirectQuery</span></span> |
| --- | --- |
| <span data-ttu-id="909a6-148">Ezzel a módszerrel a táblákat, oszlopokat *és adatokat* is a **Power BI Desktopba** importálhatja vagy másolja.</span><span class="sxs-lookup"><span data-stu-id="909a6-148">Tables, columns, *and data* are imported or copied into **Power BI Desktop**.</span></span> <span data-ttu-id="909a6-149">A megjelenítésekkel végzett munka során a **Power BI Desktop** lekéri az adatok másolatát.</span><span class="sxs-lookup"><span data-stu-id="909a6-149">As you work with visualizations, **Power BI Desktop** queries a copy of the data.</span></span> <span data-ttu-id="909a6-150">A mögöttes adatok módosulásainak megtekintéséhez újból frissítenie vagy importálnia kell a teljes aktuális adatkészletet.</span><span class="sxs-lookup"><span data-stu-id="909a6-150">To see any changes that occurred to the underlying data, you must refresh, or import, a complete, current dataset again.</span></span> |<span data-ttu-id="909a6-151">Ezzel a módszerrel csak *a táblákat és oszlopokat* importálhatja vagy másolhatja a **Power BI Desktopba**.</span><span class="sxs-lookup"><span data-stu-id="909a6-151">Only *tables and columns* are imported or copied into **Power BI Desktop**.</span></span> <span data-ttu-id="909a6-152">A megjelenítésekkel végzett munka során a **Power BI Desktop** lekéri a mögöttes adatokat, azaz Önnek mindig az aktuális adatok állnak a rendelkezésére.</span><span class="sxs-lookup"><span data-stu-id="909a6-152">As you work with visualizations, **Power BI Desktop** queries the underlying data source, which means you're always viewing current data.</span></span> |

<span data-ttu-id="909a6-153">Az adatforráshoz való csatlakozásról további információt a [Csatlakozás az adatforráshoz](power-bi-embedded-connect-datasource.md) című témakörben talál.</span><span class="sxs-lookup"><span data-stu-id="909a6-153">For more about connecting to a data source, see [Connect to a data source](power-bi-embedded-connect-datasource.md).</span></span>

<span data-ttu-id="909a6-154">A **Power BI Desktopban** végzett munka mentésekor létrejön egy PBIX-fájl.</span><span class="sxs-lookup"><span data-stu-id="909a6-154">After you save your work in **Power BI Desktop**, a PBIX file is created.</span></span> <span data-ttu-id="909a6-155">Ez tartalmazza a jelentést.</span><span class="sxs-lookup"><span data-stu-id="909a6-155">This file contains your report.</span></span> <span data-ttu-id="909a6-156">Ha importálja az adatokat, a PBIX-fájl a teljes adatkészletet tartalmazza, ha azonban a **DirectQueryt** használja, a PBIX-fájlban csak az adatkészlet sémája található.</span><span class="sxs-lookup"><span data-stu-id="909a6-156">In addition, if you import data the PBIX contains the complete dataset, or if you use **DirectQuery**, the PBIX contains just a dataset schema.</span></span> <span data-ttu-id="909a6-157">A [Power BI importálási API-jával](https://msdn.microsoft.com/library/mt711504.aspx) programozott módon telepítheti a PBIX-fájlt a munkaterületre.</span><span class="sxs-lookup"><span data-stu-id="909a6-157">You programmatically deploy the PBIX into your workspace using the [Power BI Import API](https://msdn.microsoft.com/library/mt711504.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="909a6-158">A **Power BI Embedded** által kínált további API-k segítségével módosíthatja, hogy az adatkészlet mely kiszolgálóra vagy adatbázisra mutasson, illetve beállíthatja a szolgáltatásfiók hitelesítő adatait, melyek segítségével az adatkészlet majd csatlakozhat az adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="909a6-158">**Power BI Embedded** has additional APIs to change the server and database that your dataset is pointing to and set a service account credential that the dataset will use to connect to your database.</span></span> <span data-ttu-id="909a6-159">További információ: [Post SetAllConnections](https://msdn.microsoft.com/library/mt711505.aspx) és [Patch Gateway Datasource](https://msdn.microsoft.com/library/mt711498.aspx).</span><span class="sxs-lookup"><span data-stu-id="909a6-159">See [Post SetAllConnections](https://msdn.microsoft.com/library/mt711505.aspx) and [Patch Gateway Datasource](https://msdn.microsoft.com/library/mt711498.aspx).</span></span>

## <a name="create-power-bi-datasets-and-reports-using-apis"></a><span data-ttu-id="909a6-160">Power BI-adatkészletek és -jelentések létrehozása API-k használatával</span><span class="sxs-lookup"><span data-stu-id="909a6-160">Create Power BI datasets and reports using APIs</span></span>

### <a name="datsets"></a><span data-ttu-id="909a6-161">Adatkészletek</span><span class="sxs-lookup"><span data-stu-id="909a6-161">Datsets</span></span>

<span data-ttu-id="909a6-162">A REST API segítségével létrehozhat adatkészleteket a Power BI Embeddedben.</span><span class="sxs-lookup"><span data-stu-id="909a6-162">You can create datasets within Power BI Embedded using the REST API.</span></span> <span data-ttu-id="909a6-163">Ezután elküldheti adatait saját adatkészletébe.</span><span class="sxs-lookup"><span data-stu-id="909a6-163">You can then push data into your dataset.</span></span> <span data-ttu-id="909a6-164">Ez lehetővé teszi, hogy dolgozhasson az adattal a Power BI Desktop használata nélkül is.</span><span class="sxs-lookup"><span data-stu-id="909a6-164">This allows you to work with data without the need of Power BI Desktop.</span></span> <span data-ttu-id="909a6-165">További információkat az [adatkészletek közzétételét](https://msdn.microsoft.com/library/azure/mt778875.aspx) ismertető részben talál.</span><span class="sxs-lookup"><span data-stu-id="909a6-165">For more information, see [Post Datasets](https://msdn.microsoft.com/library/azure/mt778875.aspx).</span></span>

### <a name="reports"></a><span data-ttu-id="909a6-166">Jelentések</span><span class="sxs-lookup"><span data-stu-id="909a6-166">Reports</span></span>

<span data-ttu-id="909a6-167">A JavaScript API használatával saját alkalmazásában készíthet jelentést egy adatkészletből.</span><span class="sxs-lookup"><span data-stu-id="909a6-167">You can create a report from a dataset directly in your application using the JavaScript API.</span></span> <span data-ttu-id="909a6-168">További információkért lásd az [Új jelentés létrehozása adatkészletből a Power BI Embedded használatával](power-bi-embedded-create-report-from-dataset.md) részt.</span><span class="sxs-lookup"><span data-stu-id="909a6-168">For more information, see [Create a new report from a dataset in Power BI Embedded](power-bi-embedded-create-report-from-dataset.md).</span></span>

## <a name="see-also"></a><span data-ttu-id="909a6-169">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="909a6-169">See Also</span></span>

[<span data-ttu-id="909a6-170">Bevezetés a minta használatába</span><span class="sxs-lookup"><span data-stu-id="909a6-170">Get started with sample</span></span>](power-bi-embedded-get-started-sample.md)  
[<span data-ttu-id="909a6-171">Hitelesítés és engedélyezés a Power BI Embedded használatával</span><span class="sxs-lookup"><span data-stu-id="909a6-171">Authenticating and authorizing in Power BI Embedded</span></span>](power-bi-embedded-app-token-flow.md)  
[<span data-ttu-id="909a6-172">Jelentés beágyazása</span><span class="sxs-lookup"><span data-stu-id="909a6-172">Embed a report</span></span>](power-bi-embedded-embed-report.md)  
<span data-ttu-id="909a6-173">[Új jelentés létrehozása adatkészletből a Power BI Embedded használatával](power-bi-embedded-create-report-from-dataset.md)
[Jelentések mentése](power-bi-embedded-save-reports.md)</span><span class="sxs-lookup"><span data-stu-id="909a6-173">[Create a new report from a dataset in Power BI Embedded](power-bi-embedded-create-report-from-dataset.md)
[Save reports](power-bi-embedded-save-reports.md)</span></span>  
[<span data-ttu-id="909a6-174">Power BI Desktop</span><span class="sxs-lookup"><span data-stu-id="909a6-174">Power BI Desktop</span></span>](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)  
[<span data-ttu-id="909a6-175">JavaScript beágyazási minta</span><span class="sxs-lookup"><span data-stu-id="909a6-175">JavaScript Embed Sample</span></span>](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
<span data-ttu-id="909a6-176">További kérdései vannak?</span><span class="sxs-lookup"><span data-stu-id="909a6-176">More questions?</span></span> [<span data-ttu-id="909a6-177">Tegye próbára a Power BI közösségét</span><span class="sxs-lookup"><span data-stu-id="909a6-177">Try the Power BI Community</span></span>](http://community.powerbi.com/)

