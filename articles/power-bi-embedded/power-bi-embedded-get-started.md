---
title: "lépések a Microsoft Power BI Embedded aaaGet"
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
ms.openlocfilehash: ebb550cb4eba761dde3c23e4dd0314fc885817e2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-microsoft-power-bi-embedded"></a><span data-ttu-id="0ed97-103">A Mcirosoft Power BI Embedded bemutatása</span><span class="sxs-lookup"><span data-stu-id="0ed97-103">Get started with Microsoft Power BI Embedded</span></span>

<span data-ttu-id="0ed97-104">**A Power BI Embedded** van az Azure-szolgáltatások, hogy lehetővé teszi, hogy alkalmazás fejlesztők tooadd interaktív Power BI-jelentéseket vehetnek fel saját alkalmazásaikba.</span><span class="sxs-lookup"><span data-stu-id="0ed97-104">**Power BI Embedded** is an Azure service that enables application developers tooadd interactive Power BI reports into their own applications.</span></span> <span data-ttu-id="0ed97-105">**A Power BI Embedded** jelentkezzen be a meglévő alkalmazások újratervezése kellene vagy hello módon módosítás nélkül működik.</span><span class="sxs-lookup"><span data-stu-id="0ed97-105">**Power BI Embedded** works with existing applications without needing redesign or changing hello way users sign in.</span></span>

<span data-ttu-id="0ed97-106">Az erőforrások **Microsoft Power BI Embedded** törlődnek az hello [Azure ARM API-k](https://msdn.microsoft.com/library/mt712306.aspx).</span><span class="sxs-lookup"><span data-stu-id="0ed97-106">Resources for **Microsoft Power BI Embedded** are provisioned through hello [Azure ARM APIs](https://msdn.microsoft.com/library/mt712306.aspx).</span></span> <span data-ttu-id="0ed97-107">Ebben az esetben kiépítése hello erőforrás van egy **Power BI-Munkaterületcsoport**.</span><span class="sxs-lookup"><span data-stu-id="0ed97-107">In this case, hello resource you provision is a **Power BI Workspace Collection**.</span></span>

![](media/power-bi-embedded-get-started/introduction.png)

## <a name="create-a-workspace-collection"></a><span data-ttu-id="0ed97-108">Munkaterület-csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="0ed97-108">Create a workspace collection</span></span>

<span data-ttu-id="0ed97-109">A **munkaterület-csoportok** hello legfelső szintű Azure-erőforrás és a rendszer beágyazza az alkalmazás hello tartalom tárolója.</span><span class="sxs-lookup"><span data-stu-id="0ed97-109">A **Workspace Collection** is hello top-level Azure resource and a container for hello content that will be embedded in your application.</span></span> <span data-ttu-id="0ed97-110">A **munkaterület-csoportok** kétféleképpen hozhatók létre:</span><span class="sxs-lookup"><span data-stu-id="0ed97-110">A **Workspace Collection** can be created in two ways::</span></span>

* <span data-ttu-id="0ed97-111">Hello Azure portál segítségével manuálisan</span><span class="sxs-lookup"><span data-stu-id="0ed97-111">Manually using hello Azure Portal</span></span>
* <span data-ttu-id="0ed97-112">Programozott módon hello Azure Resource Manager (ARM) API-k</span><span class="sxs-lookup"><span data-stu-id="0ed97-112">Programmatically using hello Azure Resource Manager(ARM) APIs</span></span>

<span data-ttu-id="0ed97-113">Hello lépéseket toobuild keresztül bemutatjuk a **munkaterület-csoportok** használatával hello Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="0ed97-113">Let's walk through hello steps toobuild a **Workspace Collection** using hello Azure Portal.</span></span>

1. <span data-ttu-id="0ed97-114">Nyissa meg az **Azure Portalt** és jelentkezzen be: [http://portal.azure.com](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="0ed97-114">Open and sign into **Azure Portal**: [http://portal.azure.com](http://portal.azure.com).</span></span>
2. <span data-ttu-id="0ed97-115">Kattintson a **+ új** a hello felső panelje.</span><span class="sxs-lookup"><span data-stu-id="0ed97-115">Click **+ New** on hello top panel.</span></span>
   
   ![](media/power-bi-embedded-get-started/create-workspace-1.png)
3. <span data-ttu-id="0ed97-116">Az **Adatok + analitika** területen kattintson a **Power BI Embedded** elemre.</span><span class="sxs-lookup"><span data-stu-id="0ed97-116">Under **Data + Analytics** click **Power BI Embedded**.</span></span>
4. <span data-ttu-id="0ed97-117">A hello **munkaterület-csoport panel**, adja meg a hello szükséges adatokat.</span><span class="sxs-lookup"><span data-stu-id="0ed97-117">On hello **Workspace Collection Blade**, enter hello required information.</span></span> <span data-ttu-id="0ed97-118">A **Díjszabásról** további információt [A Power BI Embedded díjszabása](http://go.microsoft.com/fwlink/?LinkID=760527) című témakörben talál.</span><span class="sxs-lookup"><span data-stu-id="0ed97-118">For **Pricing**, see [Power BI Embedded pricing](http://go.microsoft.com/fwlink/?LinkID=760527).</span></span>
   
   ![](media/power-bi-embedded-get-started/create-workspace-2.png)
5. <span data-ttu-id="0ed97-119">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="0ed97-119">Click **Create**.</span></span>

<span data-ttu-id="0ed97-120">Hello **munkaterület-csoportok** eltarthat néhány másodpercig tooprovision.</span><span class="sxs-lookup"><span data-stu-id="0ed97-120">hello **Workspace Collection** will take a few moments tooprovision.</span></span> <span data-ttu-id="0ed97-121">Amikor elkészült, akkor megnyílik toohello **munkaterület-csoport panel**.</span><span class="sxs-lookup"><span data-stu-id="0ed97-121">When completed, you'll be taken toohello **Workspace Collection Blade**.</span></span>

   ![](media/power-bi-embedded-get-started/create-workspace-3.png)

<span data-ttu-id="0ed97-122">Hello **létrehozás panelen** toocall hello API-t a munkaterületek létrehozásáért és a tartalom toothem szükséges hello információkat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="0ed97-122">hello **Creation Blade** contains hello information you need toocall hello APIs that create workspaces and deploy content toothem.</span></span>

<a name="view-access-keys"/>

## <a name="view-power-bi-api-access-keys"></a><span data-ttu-id="0ed97-123">A Power BI API elérési kulcsainak megtekintése</span><span class="sxs-lookup"><span data-stu-id="0ed97-123">View Power BI API access keys</span></span>

<span data-ttu-id="0ed97-124">Hello legfontosabb szükséges információkat toocall kódrészletek hello Power BI REST API-k egyike hello **hívóbetűk**.</span><span class="sxs-lookup"><span data-stu-id="0ed97-124">One of hello most important pieces of information needed toocall hello Power BI REST APIs are hello **Access Keys**.</span></span> <span data-ttu-id="0ed97-125">Ezek a használt toogenerate hello **alkalmazási jogkivonatok** , amelyek használt tooauthenticate az API-kérelmek.</span><span class="sxs-lookup"><span data-stu-id="0ed97-125">These are used toogenerate hello **app tokens** that are used tooauthenticate your API requests.</span></span> <span data-ttu-id="0ed97-126">tooview a **hívóbetűk**, kattintson a **hívóbetűk** a hello **beállítások panel**.</span><span class="sxs-lookup"><span data-stu-id="0ed97-126">tooview your **Access Keys**, click **Access Keys** on hello **Settings Blade**.</span></span> <span data-ttu-id="0ed97-127">További információk az **alkalmazás-jogkivonatokról**: [Authenticating and authorizing with Power BI Embedded](power-bi-embedded-app-token-flow.md) (Hitelesítés és engedélyezés a Power BI Embedded használatával).</span><span class="sxs-lookup"><span data-stu-id="0ed97-127">For more about **app tokens**, see [Authenticating and authorizing with Power BI Embedded](power-bi-embedded-app-token-flow.md).</span></span>

   ![](media/power-bi-embedded-get-started/access-keys.png)

<span data-ttu-id="0ed97-128">Láthatja, hogy két kulccsal rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="0ed97-128">You'll'notice that you have two keys.</span></span>

   ![](media/power-bi-embedded-get-started/access-keys-2.png)

<span data-ttu-id="0ed97-129">Készítsen másolatot a kulcsokról, és tárolja őket biztonságos helyen az alkalmazáson belül.</span><span class="sxs-lookup"><span data-stu-id="0ed97-129">Copy these keys and store them securely in your application.</span></span> <span data-ttu-id="0ed97-130">Nagyon fontos, hogy úgy kezelje, ezek a kulcsok jelszavát, ugyanúgy, mert hozzáférési tooall hello tartalma lesz biztosítanak a **munkaterület-csoportok**.</span><span class="sxs-lookup"><span data-stu-id="0ed97-130">It's very important that you treat these keys as you would a password, because they'll provide access tooall hello content in your **Workspace Collection**.</span></span>

<span data-ttu-id="0ed97-131">Jóllehet két kulcs van listázva, egyszerre azonban mindig csak az egyikre lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="0ed97-131">While two keys are listed, only one key is needed at a particular time.</span></span> <span data-ttu-id="0ed97-132">hello második kulcsra azért van, akkor a kulcsok rendszeres újragenerálása toohello szolgáltatás megszakítása nélkül.</span><span class="sxs-lookup"><span data-stu-id="0ed97-132">hello second key is provided so you can periodically regenerate keys without interrupting access toohello service.</span></span>

<span data-ttu-id="0ed97-133">Most, hogy rendelkezik **elérési kulcsokkal** és az alkalmazáshoz való Power BI-példánnyal, jelentést importálhat az alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="0ed97-133">Now that you have an instance of Power BI for your application, and **Access Keys**, you can import a report into your own app.</span></span> <span data-ttu-id="0ed97-134">Mielőtt megtudhatja, hogyan tooimport egy jelentést, hello a következő szakasz ismerteti az alkalmazásba létrehozása a Power BI-adatkészletek és jelentések tooembed.</span><span class="sxs-lookup"><span data-stu-id="0ed97-134">Before you learn how tooimport a report, hello next section describes creating Power BI datasets and reports tooembed into an app.</span></span>

## <a name="working-with-workspaces"></a><span data-ttu-id="0ed97-135">A munkaterületek használata</span><span class="sxs-lookup"><span data-stu-id="0ed97-135">Working with workspaces</span></span>

<span data-ttu-id="0ed97-136">A munkaterület-csoport létrehozása után kell, hogy a rendszer mellett a jelentések és adatkészletek munkaterület toocreate.</span><span class="sxs-lookup"><span data-stu-id="0ed97-136">After you have created your workspace collection, you will need toocreate a workspace that will house your reports and datasets.</span></span> <span data-ttu-id="0ed97-137">a munkaterület toocreate, szüksége lesz toouse hello [Post Worksapce REST API](https://msdn.microsoft.com/library/azure/mt711503.aspx).</span><span class="sxs-lookup"><span data-stu-id="0ed97-137">toocreate a workspace, you will need toouse hello [Post Worksapce REST API](https://msdn.microsoft.com/library/azure/mt711503.aspx).</span></span>

## <a name="create-power-bi-datasets-and-reports-tooembed-into-an-app-using-power-bi-desktop"></a><span data-ttu-id="0ed97-138">A Power BI-adatkészletek és jelentések tooembed létrehozása az alkalmazásba a Power BI Desktop használatával</span><span class="sxs-lookup"><span data-stu-id="0ed97-138">Create Power BI datasets and reports tooembed into an app using Power BI Desktop</span></span>

<span data-ttu-id="0ed97-139">Most, hogy a Power BI példányának hozott létre az alkalmazáshoz, és rendelkezik **hívóbetűk**, toocreate hello Power BI-adatkészletek és jelentések, amelyet az tooembed lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="0ed97-139">Now that you have created an instance of Power BI for your application, and have **Access Keys**, you will need toocreate hello Power BI datasets and reports that you want tooembed.</span></span> <span data-ttu-id="0ed97-140">Az adatkészletek és a jelentések a **Power BI Desktop** segítségével hozhatók létre.</span><span class="sxs-lookup"><span data-stu-id="0ed97-140">Datasets and reports can be created by using **Power BI Desktop**.</span></span> <span data-ttu-id="0ed97-141">A [Power BI Desktop ingyenesen](https://go.microsoft.com/fwlink/?LinkId=521662) letölthető.</span><span class="sxs-lookup"><span data-stu-id="0ed97-141">You can download [Power BI Desktop for free](https://go.microsoft.com/fwlink/?LinkId=521662).</span></span> <span data-ttu-id="0ed97-142">Vagy, tooquickly első lépései, letöltheti a hello [kiskereskedelmi elemzési minta pbix-fájlt](http://go.microsoft.com/fwlink/?LinkID=780547).</span><span class="sxs-lookup"><span data-stu-id="0ed97-142">Or, tooquickly get started, you can download hello [Retail Analysis Sample PBIX](http://go.microsoft.com/fwlink/?LinkID=780547).</span></span>

> [!NOTE]
> <span data-ttu-id="0ed97-143">További kapcsolatos toolearn toouse **Power BI Desktop**, lásd: [első lépések a Power BI Desktop](https://powerbi.microsoft.com/guided-learning/powerbi-learning-0-2-get-started-power-bi-desktop).</span><span class="sxs-lookup"><span data-stu-id="0ed97-143">toolearn more about how toouse **Power BI Desktop**, see [Getting Started with Power BI Desktop](https://powerbi.microsoft.com/guided-learning/powerbi-learning-0-2-get-started-power-bi-desktop).</span></span>

<span data-ttu-id="0ed97-144">A **Power BI Desktop**, csatlakozás tooyour adatforrás hello adatok másolatát importálásával **Power BI Desktop** vagy az összekötő közvetlenül toohello adatok használatával **DirectQuery**.</span><span class="sxs-lookup"><span data-stu-id="0ed97-144">With **Power BI Desktop**, you connect tooyour data source by importing a copy of hello data into **Power BI Desktop** or connecting directly toohello data source using **DirectQuery**.</span></span>

<span data-ttu-id="0ed97-145">Az alábbiakban közti különbségeket hello **importálási** és **DirectQuery**.</span><span class="sxs-lookup"><span data-stu-id="0ed97-145">Here are hello differences between using **Import** and **DirectQuery**.</span></span>

| <span data-ttu-id="0ed97-146">Importálás</span><span class="sxs-lookup"><span data-stu-id="0ed97-146">Import</span></span> | <span data-ttu-id="0ed97-147">DirectQuery</span><span class="sxs-lookup"><span data-stu-id="0ed97-147">DirectQuery</span></span> |
| --- | --- |
| <span data-ttu-id="0ed97-148">Ezzel a módszerrel a táblákat, oszlopokat *és adatokat* is a **Power BI Desktopba** importálhatja vagy másolja.</span><span class="sxs-lookup"><span data-stu-id="0ed97-148">Tables, columns, *and data* are imported or copied into **Power BI Desktop**.</span></span> <span data-ttu-id="0ed97-149">A megjelenítésekkel végzett munka **Power BI Desktop** lekérdezi hello adatok másolatát.</span><span class="sxs-lookup"><span data-stu-id="0ed97-149">As you work with visualizations, **Power BI Desktop** queries a copy of hello data.</span></span> <span data-ttu-id="0ed97-150">toosee történt toohello alapjául szolgáló adatok módosítása, frissítse, vagy importálja, a teljes aktuális adatkészletet újra.</span><span class="sxs-lookup"><span data-stu-id="0ed97-150">toosee any changes that occurred toohello underlying data, you must refresh, or import, a complete, current dataset again.</span></span> |<span data-ttu-id="0ed97-151">Ezzel a módszerrel csak *a táblákat és oszlopokat* importálhatja vagy másolhatja a **Power BI Desktopba**.</span><span class="sxs-lookup"><span data-stu-id="0ed97-151">Only *tables and columns* are imported or copied into **Power BI Desktop**.</span></span> <span data-ttu-id="0ed97-152">A megjelenítésekkel végzett munka **Power BI Desktop** lekérdezések hello alapul szolgáló adatforrásban, ami azt jelenti, hogy mindig aktuális adatokat megtekintése.</span><span class="sxs-lookup"><span data-stu-id="0ed97-152">As you work with visualizations, **Power BI Desktop** queries hello underlying data source, which means you're always viewing current data.</span></span> |

<span data-ttu-id="0ed97-153">Csatlakozás tooa adatforrás kapcsolatban bővebben lásd: [Connect tooa adatforrás](power-bi-embedded-connect-datasource.md).</span><span class="sxs-lookup"><span data-stu-id="0ed97-153">For more about connecting tooa data source, see [Connect tooa data source](power-bi-embedded-connect-datasource.md).</span></span>

<span data-ttu-id="0ed97-154">A **Power BI Desktopban** végzett munka mentésekor létrejön egy PBIX-fájl.</span><span class="sxs-lookup"><span data-stu-id="0ed97-154">After you save your work in **Power BI Desktop**, a PBIX file is created.</span></span> <span data-ttu-id="0ed97-155">Ez tartalmazza a jelentést.</span><span class="sxs-lookup"><span data-stu-id="0ed97-155">This file contains your report.</span></span> <span data-ttu-id="0ed97-156">Emellett, ha importál adatokat hello pbix-fájlt tartalmaz hello teljes adatkészletet, vagy ha **DirectQuery**, hello pbix-fájlt tartalmaz, csak az adatkészlet sémája.</span><span class="sxs-lookup"><span data-stu-id="0ed97-156">In addition, if you import data hello PBIX contains hello complete dataset, or if you use **DirectQuery**, hello PBIX contains just a dataset schema.</span></span> <span data-ttu-id="0ed97-157">Programozott módon telepítheti az hello pbix-fájlt a munkaterületre hello segítségével [Power BI importálási API](https://msdn.microsoft.com/library/mt711504.aspx).</span><span class="sxs-lookup"><span data-stu-id="0ed97-157">You programmatically deploy hello PBIX into your workspace using hello [Power BI Import API](https://msdn.microsoft.com/library/mt711504.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="0ed97-158">**A Power BI Embedded** tartalmaz további API-k toochange hello kiszolgálót és adatbázist, hogy a DataSet adatkészlet mutat tooand beállítása a szolgáltatási fiók hitelesítő adatait, amely a hello dataset tooconnect tooyour adatbázist fogja használni.</span><span class="sxs-lookup"><span data-stu-id="0ed97-158">**Power BI Embedded** has additional APIs toochange hello server and database that your dataset is pointing tooand set a service account credential that hello dataset will use tooconnect tooyour database.</span></span> <span data-ttu-id="0ed97-159">További információ: [Post SetAllConnections](https://msdn.microsoft.com/library/mt711505.aspx) és [Patch Gateway Datasource](https://msdn.microsoft.com/library/mt711498.aspx).</span><span class="sxs-lookup"><span data-stu-id="0ed97-159">See [Post SetAllConnections](https://msdn.microsoft.com/library/mt711505.aspx) and [Patch Gateway Datasource](https://msdn.microsoft.com/library/mt711498.aspx).</span></span>

## <a name="create-power-bi-datasets-and-reports-using-apis"></a><span data-ttu-id="0ed97-160">Power BI-adatkészletek és -jelentések létrehozása API-k használatával</span><span class="sxs-lookup"><span data-stu-id="0ed97-160">Create Power BI datasets and reports using APIs</span></span>

### <a name="datsets"></a><span data-ttu-id="0ed97-161">Adatkészletek</span><span class="sxs-lookup"><span data-stu-id="0ed97-161">Datsets</span></span>

<span data-ttu-id="0ed97-162">Adatkészletek belül a Power BI Embedded hello REST API használatával hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="0ed97-162">You can create datasets within Power BI Embedded using hello REST API.</span></span> <span data-ttu-id="0ed97-163">Ezután elküldheti adatait saját adatkészletébe.</span><span class="sxs-lookup"><span data-stu-id="0ed97-163">You can then push data into your dataset.</span></span> <span data-ttu-id="0ed97-164">Ez lehetővé teszi a Power BI Desktop hello igénye nélkül adatokkal toowork.</span><span class="sxs-lookup"><span data-stu-id="0ed97-164">This allows you toowork with data without hello need of Power BI Desktop.</span></span> <span data-ttu-id="0ed97-165">További információkat az [adatkészletek közzétételét](https://msdn.microsoft.com/library/azure/mt778875.aspx) ismertető részben talál.</span><span class="sxs-lookup"><span data-stu-id="0ed97-165">For more information, see [Post Datasets](https://msdn.microsoft.com/library/azure/mt778875.aspx).</span></span>

### <a name="reports"></a><span data-ttu-id="0ed97-166">Jelentések</span><span class="sxs-lookup"><span data-stu-id="0ed97-166">Reports</span></span>

<span data-ttu-id="0ed97-167">Létrehozhat egy jelentést egy adatkészletből közvetlenül a az alkalmazás hello JavaScript API használatával.</span><span class="sxs-lookup"><span data-stu-id="0ed97-167">You can create a report from a dataset directly in your application using hello JavaScript API.</span></span> <span data-ttu-id="0ed97-168">További információkért lásd az [Új jelentés létrehozása adatkészletből a Power BI Embedded használatával](power-bi-embedded-create-report-from-dataset.md) részt.</span><span class="sxs-lookup"><span data-stu-id="0ed97-168">For more information, see [Create a new report from a dataset in Power BI Embedded](power-bi-embedded-create-report-from-dataset.md).</span></span>

## <a name="see-also"></a><span data-ttu-id="0ed97-169">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="0ed97-169">See Also</span></span>

[<span data-ttu-id="0ed97-170">Bevezetés a minta használatába</span><span class="sxs-lookup"><span data-stu-id="0ed97-170">Get started with sample</span></span>](power-bi-embedded-get-started-sample.md)  
[<span data-ttu-id="0ed97-171">Hitelesítés és engedélyezés a Power BI Embedded használatával</span><span class="sxs-lookup"><span data-stu-id="0ed97-171">Authenticating and authorizing in Power BI Embedded</span></span>](power-bi-embedded-app-token-flow.md)  
[<span data-ttu-id="0ed97-172">Jelentés beágyazása</span><span class="sxs-lookup"><span data-stu-id="0ed97-172">Embed a report</span></span>](power-bi-embedded-embed-report.md)  
<span data-ttu-id="0ed97-173">[Új jelentés létrehozása adatkészletből a Power BI Embedded használatával](power-bi-embedded-create-report-from-dataset.md)
[Jelentések mentése](power-bi-embedded-save-reports.md)</span><span class="sxs-lookup"><span data-stu-id="0ed97-173">[Create a new report from a dataset in Power BI Embedded](power-bi-embedded-create-report-from-dataset.md)
[Save reports](power-bi-embedded-save-reports.md)</span></span>  
[<span data-ttu-id="0ed97-174">Power BI Desktop</span><span class="sxs-lookup"><span data-stu-id="0ed97-174">Power BI Desktop</span></span>](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)  
[<span data-ttu-id="0ed97-175">JavaScript beágyazási minta</span><span class="sxs-lookup"><span data-stu-id="0ed97-175">JavaScript Embed Sample</span></span>](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
<span data-ttu-id="0ed97-176">További kérdései vannak?</span><span class="sxs-lookup"><span data-stu-id="0ed97-176">More questions?</span></span> [<span data-ttu-id="0ed97-177">Próbálja meg a Power BI-Közösség hello</span><span class="sxs-lookup"><span data-stu-id="0ed97-177">Try hello Power BI Community</span></span>](http://community.powerbi.com/)

