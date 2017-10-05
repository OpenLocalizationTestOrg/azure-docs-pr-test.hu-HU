---
title: "Ismerkedés egy minta segítségével"
description: "A Power BI Embedded, interaktív Power BI-jelentések hozzáadása az üzleti intelligenciát biztosító alkalmazásba SDK segítségével"
services: power-bi-embedded
documentationcenter: 
author: guyinacube
manager: erikre
editor: 
tags: 
ms.assetid: d8a9ef78-ad4e-4bc7-9711-89172dc5c548
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 03/02/2017
ms.author: asaxton
ms.openlocfilehash: c3cb1763f807220a4a829f410d7eb77974b25776
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-power-bi-embedded-sample"></a><span data-ttu-id="bfef5-103">Bevezetés a Power BI Embedded minta használatába</span><span class="sxs-lookup"><span data-stu-id="bfef5-103">Get started with Power BI Embedded sample</span></span>

<span data-ttu-id="bfef5-104">A **Microsoft Power BI Embedded**, akkor integrálható a Power BI-jelentéseket jobbra a webhelyen vagy az alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="bfef5-104">With **Microsoft Power BI Embedded**, you can integrate Power BI reports right into your web or mobile applications.</span></span> <span data-ttu-id="bfef5-105">Ebben a cikkben lesz bemutatása után, hogy a **Power BI Embedded** get elindított minta.</span><span class="sxs-lookup"><span data-stu-id="bfef5-105">In this article, we'll introduce you to the **Power BI Embedded** get started sample.</span></span>

<span data-ttu-id="bfef5-106">Ahhoz, hogy nyissa meg a további, valószínűleg érdemes a következő erőforrások mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="bfef5-106">Before we go any further, you'll probably want to save the following resources.</span></span> <span data-ttu-id="bfef5-107">Akkor lesz hasznos, ha integrálása a Power BI-jelentéseket a minta alkalmazást és a saját túl.</span><span class="sxs-lookup"><span data-stu-id="bfef5-107">They'll help you when integrating Power BI reports into the sample app and your own apps too.</span></span>

* [<span data-ttu-id="bfef5-108">Munkaterület webes mintaalkalmazás</span><span class="sxs-lookup"><span data-stu-id="bfef5-108">Sample workspace web app</span></span>](http://go.microsoft.com/fwlink/?LinkId=761493)
* [<span data-ttu-id="bfef5-109">A Power BI Embedded API-referencia</span><span class="sxs-lookup"><span data-stu-id="bfef5-109">Power BI Embedded API reference</span></span>](https://msdn.microsoft.com/en-US/library/azure/mt711507.aspx)
* <span data-ttu-id="bfef5-110">[A Power BI Embedded .NET SDK ](http://go.microsoft.com/fwlink/?LinkId=746472) (NuGet keresztül érhető el)</span><span class="sxs-lookup"><span data-stu-id="bfef5-110">[Power BI Embedded .NET SDK ](http://go.microsoft.com/fwlink/?LinkId=746472) (available via NuGet)</span></span>
* [<span data-ttu-id="bfef5-111">JavaScript-jelentés beágyazása minta</span><span class="sxs-lookup"><span data-stu-id="bfef5-111">JavaScript Report Embed Sample</span></span>](https://microsoft.github.io/PowerBI-JavaScript/demo)

> [!NOTE] 
> <span data-ttu-id="bfef5-112">Mielőtt is beállíthat, és futtassa a Power BI Embedded első lépések minta, legalább egy létrehozásához szükséges **munkaterület-csoport** az Azure-előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="bfef5-112">Before you can configure and run the Power BI Embedded get started sample, you need to create at least one **Workspace Collection** in your Azure subscription.</span></span> <span data-ttu-id="bfef5-113">Hogyan hozzon létre egy **munkaterület-csoportok** az Azure portál itt talál: [Ismerkedés a Power BI Embedded](power-bi-embedded-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="bfef5-113">To learn how to create a **Workspace Collection** in the Azure Portal see [Getting Started with Power BI Embedded](power-bi-embedded-get-started.md).</span></span>

## <a name="configure-the-sample-app"></a><span data-ttu-id="bfef5-114">A mintaalkalmazás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="bfef5-114">Configure the sample app</span></span>

<span data-ttu-id="bfef5-115">Bemutatjuk, keresztül érik el a mintaalkalmazás futtatásához szükséges összetevőket a Visual Studio fejlesztési környezet beállítása.</span><span class="sxs-lookup"><span data-stu-id="bfef5-115">Let's walk through setting up your Visual Studio development environment to access the  components needed to run the sample app.</span></span>

1. <span data-ttu-id="bfef5-116">Töltse le és csomagolja ki a [Power BI Embedded - jelentés integrálásához webalkalmazás](http://go.microsoft.com/fwlink/?LinkId=761493) mintát a Githubon.</span><span class="sxs-lookup"><span data-stu-id="bfef5-116">Download and unzip the [Power BI Embedded - Integrate a report into a web app](http://go.microsoft.com/fwlink/?LinkId=761493) sample on GitHub.</span></span>
2. <span data-ttu-id="bfef5-117">Nyissa meg **Power bi-embedded.sln** a Visual Studióban.</span><span class="sxs-lookup"><span data-stu-id="bfef5-117">Open **PowerBI-embedded.sln** in Visual Studio.</span></span> <span data-ttu-id="bfef5-118">Szükség lehet végrehajtani a **-csomag** a NuGET-Csomagkezelő konzol frissítéséhez a csomagokat a megoldásban használt parancsot.</span><span class="sxs-lookup"><span data-stu-id="bfef5-118">You may need to execute the **Update-Package** command in the NuGET Package Manager Console in order to update the packages used in this solution.</span></span>
3. <span data-ttu-id="bfef5-119">A megoldás felépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="bfef5-119">Build the solution.</span></span>
4. <span data-ttu-id="bfef5-120">Futtassa a **ProvisionSample** konzolalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="bfef5-120">Run the **ProvisionSample** console app.</span></span> <span data-ttu-id="bfef5-121">A minta-Konzolalkalmazás munkaterület kiépíteni, és PBIX-fájl importálása.</span><span class="sxs-lookup"><span data-stu-id="bfef5-121">In the sample console app, you provision a workspace and import a PBIX file.</span></span>
5. <span data-ttu-id="bfef5-122">Kiépítéséhez egy új **munkaterület**, válassza az 1. lehetőség – **gyűjtési felügyeleti**, és választhatja meg a 6., **új munkaterület kiépítése**</span><span class="sxs-lookup"><span data-stu-id="bfef5-122">To provision a new **Workspace**, select option 1, **Collection management**, and then select option 6, **Provision a new Workspace**</span></span>
6. <span data-ttu-id="bfef5-123">Importálhatja egy új **jelentés**, válassza ki a 2. lehetőség – **jelentést a felügyeleti**, majd válassza ki a 3. lehetőség – **pbix-fájlt asztali importálása fájlból egy munkaterület**.</span><span class="sxs-lookup"><span data-stu-id="bfef5-123">To import a new **Report**, select option 2, **Report management**, and then select option 3, **Import PBIX Desktop file into a workspace**.</span></span>

7. <span data-ttu-id="bfef5-124">Adja meg a **munkaterület-csoportok** nevét, és **hozzáférési kulcs**.</span><span class="sxs-lookup"><span data-stu-id="bfef5-124">Enter your **Workspace Collection** name, and **Access Key**.</span></span> <span data-ttu-id="bfef5-125">Ezek kaphat a **Azure Portal**.</span><span class="sxs-lookup"><span data-stu-id="bfef5-125">You can get these in the **Azure Portal**.</span></span> <span data-ttu-id="bfef5-126">További információt az beszerzése a **a hozzáférési kulcsot**, lásd: [Power BI API elérési kulcsainak megtekintése](power-bi-embedded-get-started.md#view-power-bi-api-access-keys) a Ismerkedés a Microsoft Power BI Embedded.</span><span class="sxs-lookup"><span data-stu-id="bfef5-126">To learn more about how to get your **Access Key**, see [View Power BI API Access Keys](power-bi-embedded-get-started.md#view-power-bi-api-access-keys) in Get started with Microsoft Power BI Embedded.</span></span>

    ![](media/powerbi-embedded-get-started-sample/azure-portal.png)
8. <span data-ttu-id="bfef5-127">Másolja ki és mentse az újonnan létrehozott **munkaterület azonosítója** használatára a cikk későbbi részében.</span><span class="sxs-lookup"><span data-stu-id="bfef5-127">Copy and save the newly created **Workspace ID** to use later in this article.</span></span> <span data-ttu-id="bfef5-128">Után a **munkaterület azonosítója** van létrehozva, megtalálja a **Azure Portal**.</span><span class="sxs-lookup"><span data-stu-id="bfef5-128">After the **Workspace ID** is created, you can find it the **Azure Portal**.</span></span>

    ![](media/powerbi-embedded-get-started-sample/workspace-id.png)
9. <span data-ttu-id="bfef5-129">A PBIX-fájl importálása a **munkaterület**, a beállításnak a **6. Egy meglévő munkaterületre pbix-fájlt asztali importfájl**.</span><span class="sxs-lookup"><span data-stu-id="bfef5-129">To import a PBIX file into your **Workspace**, select option **6. Import PBIX Desktop file into an existing workspace**.</span></span> <span data-ttu-id="bfef5-130">Ha még nem rendelkezik a PBIX-fájl lesz szüksége, letöltheti a [kiskereskedelmi elemzési minta pbix-fájlt](http://go.microsoft.com/fwlink/?LinkID=780547).</span><span class="sxs-lookup"><span data-stu-id="bfef5-130">If you don't have a PBIX file handy, you can download the [Retail Analysis Sample PBIX](http://go.microsoft.com/fwlink/?LinkID=780547).</span></span>
10. <span data-ttu-id="bfef5-131">Ha a rendszer kéri, adjon meg egy rövid nevet a **Dataset**.</span><span class="sxs-lookup"><span data-stu-id="bfef5-131">If prompted, enter a friendly name for your **Dataset**.</span></span>

<span data-ttu-id="bfef5-132">A következőhöz hasonló választ kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="bfef5-132">You should see a response like:</span></span>

```
Checking import state... Publishing
Checking import state... Succeeded
```

> [!NOTE]
> <span data-ttu-id="bfef5-133">A PBIX-fájl tartalmazza a közvetlen lekérdezés kapcsolatokat, ha futtatja a kapcsolót 7 a kapcsolati karakterláncok frissítésére.</span><span class="sxs-lookup"><span data-stu-id="bfef5-133">If your PBIX file contains any direct query connections, run option 7 to update the connection strings.</span></span>

<span data-ttu-id="bfef5-134">Ezen a ponton rendelkezik egy Power BI pbix-fájlt a jelentés importálni a **munkaterület**.</span><span class="sxs-lookup"><span data-stu-id="bfef5-134">At this point, you have a Power BI PBIX report imported into your **Workspace**.</span></span> <span data-ttu-id="bfef5-135">Most hogyan futtathat vizsgáljuk meg a **Power BI Embedded** elindított minta webalkalmazás beolvasása.</span><span class="sxs-lookup"><span data-stu-id="bfef5-135">Now, let's look at how to run the **Power BI Embedded** get started sample web app.</span></span>

## <a name="run-the-sample-web-app"></a><span data-ttu-id="bfef5-136">A webes mintaalkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="bfef5-136">Run the sample web app</span></span>
<span data-ttu-id="bfef5-137">A webes alkalmazás minta egy jeleníti meg az importált jelentések mintaalkalmazás a **munkaterület**.</span><span class="sxs-lookup"><span data-stu-id="bfef5-137">The web app sample is a sample application that renders reports imported into your **Workspace**.</span></span> <span data-ttu-id="bfef5-138">Ez a webes alkalmazás minta konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="bfef5-138">Here's how to configure the web app sample.</span></span>

1. <span data-ttu-id="bfef5-139">Az a **Power bi embedded** Visual Studio megoldás, kattintson a jobb gombbal a **EmbedSample** webalkalmazást, és válassza a **beállítás kezdőprojektként**.</span><span class="sxs-lookup"><span data-stu-id="bfef5-139">In the **PowerBI-embedded** Visual Studio solution, right click the **EmbedSample** web application, and choose **Set as StartUp project**.</span></span>
2. <span data-ttu-id="bfef5-140">A **web.config**, a a **EmbedSample** webalkalmazást, módosítsa a **appSettings**: **AccessKey**,  **WorkspaceCollection** nevét, és **WorkspaceId**.</span><span class="sxs-lookup"><span data-stu-id="bfef5-140">In **web.config**, in the **EmbedSample** web application, edit the **appSettings**: **AccessKey**, **WorkspaceCollection** name, and **WorkspaceId**.</span></span>

    ```
    <appSettings>
        <add key="powerbi:AccessKey" value="" />
        <add key="powerbi:ApiUrl" value="https://api.powerbi.com" />
        <add key="powerbi:WorkspaceCollection" value="" />
        <add key="powerbi:WorkspaceId" value="" />
    </appSettings>
    ```
3. <span data-ttu-id="bfef5-141">Futtassa a **EmbedSample** webalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="bfef5-141">Run the **EmbedSample** web application.</span></span>

<span data-ttu-id="bfef5-142">Miután lefuttatta a **EmbedSample** webalkalmazás, a bal oldali navigációs panelen tartalmaznia kell egy **jelentések** menü.</span><span class="sxs-lookup"><span data-stu-id="bfef5-142">Once you run the **EmbedSample** web application, the left navigation panel should contain a **Reports** menu.</span></span> <span data-ttu-id="bfef5-143">Az importált jelentés megtekintéséhez bontsa ki a **jelentések**, és kattintson a jelentés.</span><span class="sxs-lookup"><span data-stu-id="bfef5-143">To view the report you imported, expand **Reports**, and click a report.</span></span> <span data-ttu-id="bfef5-144">Ha importálta a [kiskereskedelmi elemzési minta pbix-fájlt](http://go.microsoft.com/fwlink/?LinkID=780547), a minta webalkalmazás néz ki:</span><span class="sxs-lookup"><span data-stu-id="bfef5-144">If you imported the [Retail Analysis Sample PBIX](http://go.microsoft.com/fwlink/?LinkID=780547), the sample web app would look like this:</span></span>

![](media/powerbi-embedded-get-started-sample/power-bi-embedded-sample-left-nav.png)

<span data-ttu-id="bfef5-145">Miután rákattintott egy jelentést, a **EmbedSample** webalkalmazás kell kinéznie ezt:</span><span class="sxs-lookup"><span data-stu-id="bfef5-145">After you click a report, the **EmbedSample** web application should look something this:</span></span>

![](media/powerbi-embedded-get-started-sample/sample-web-app.png)

## <a name="explore-the-sample-code"></a><span data-ttu-id="bfef5-146">A mintakód felfedezés</span><span class="sxs-lookup"><span data-stu-id="bfef5-146">Explore the sample code</span></span>

<span data-ttu-id="bfef5-147">A **Microsoft Power BI Embedded** minta egy példa-webalkalmazást, amely bemutatja, hogyan integrálható **Power BI** jelentések az alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="bfef5-147">The **Microsoft Power BI Embedded** sample is an example web app that shows you how to integrate **Power BI** reports into your app.</span></span> <span data-ttu-id="bfef5-148">Egy Model-View-Controller (MVC) tervezési mintát használ, ajánlott eljárások bemutatásához.</span><span class="sxs-lookup"><span data-stu-id="bfef5-148">It uses a Model-View-Controller (MVC) design pattern to demonstrate best practices.</span></span> <span data-ttu-id="bfef5-149">Ez a szakasz a mintakódot, amely belül részei kiemeli a **Power bi embedded** web app megoldás.</span><span class="sxs-lookup"><span data-stu-id="bfef5-149">This section highlights parts of the sample code that you can explore within the **PowerBI-embedded** web app solution.</span></span> <span data-ttu-id="bfef5-150">A Model-View-Controller (MVC) mintát elválasztja a tartományhoz, a megjelenítési és a műveletek a felhasználói bevitel három különálló osztályokba alapján modellezési: modell, megtekintése és vezérlő.</span><span class="sxs-lookup"><span data-stu-id="bfef5-150">The Model-View-Controller (MVC) pattern separates the modeling of the domain, the presentation, and the actions based on user input into three separate classes: Model, View, and Control.</span></span> <span data-ttu-id="bfef5-151">MVC kapcsolatos további információkért lásd: [ismerje meg, az ASP.NET kapcsolatos](http://www.asp.net/mvc).</span><span class="sxs-lookup"><span data-stu-id="bfef5-151">To learn more about MVC, see [Learn About ASP.NET](http://www.asp.net/mvc).</span></span>

<span data-ttu-id="bfef5-152">A **Microsoft Power BI Embedded** mintakód választja el az alábbiak szerint.</span><span class="sxs-lookup"><span data-stu-id="bfef5-152">The **Microsoft Power BI Embedded** sample code is separated as follows.</span></span> <span data-ttu-id="bfef5-153">Az egyes szakaszokon a fájl nevét, hogy könnyedén megtalálhatja a kód a minta a Power bi-embedded.sln megoldás tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="bfef5-153">Each section includes the file name in the PowerBI-embedded.sln solution so that you can easily find the code in the sample.</span></span>

> [!NOTE]
> <span data-ttu-id="bfef5-154">Ez a szakasz a mintakód bemutatja, hogyan bejegyzésre kerültek-e a kód összegzését.</span><span class="sxs-lookup"><span data-stu-id="bfef5-154">This section is a summary of the sample code that shows how the code was written.</span></span> <span data-ttu-id="bfef5-155">A teljes minta megtekintéséhez töltse be a Power bi-embedded.sln megoldást a Visual Studióban.</span><span class="sxs-lookup"><span data-stu-id="bfef5-155">To view the complete sample, please load the PowerBI-embedded.sln solution in Visual Studio.</span></span>

### <a name="model"></a><span data-ttu-id="bfef5-156">Modell</span><span class="sxs-lookup"><span data-stu-id="bfef5-156">Model</span></span>

<span data-ttu-id="bfef5-157">A minta egy **ReportsViewModel** és **ReportViewModel**.</span><span class="sxs-lookup"><span data-stu-id="bfef5-157">The sample has a **ReportsViewModel** and **ReportViewModel**.</span></span>

<span data-ttu-id="bfef5-158">**ReportsViewModel.cs**: jelenti. a Power BI-jelentéseket.</span><span class="sxs-lookup"><span data-stu-id="bfef5-158">**ReportsViewModel.cs**: Represents Power BI Reports.</span></span>

    public class ReportsViewModel
    {
        public List<Report> Reports { get; set; }
    }

<span data-ttu-id="bfef5-159">**ReportViewModel.cs**: a Power BI-jelentés jelöli.</span><span class="sxs-lookup"><span data-stu-id="bfef5-159">**ReportViewModel.cs**: Represents a Power BI Report.</span></span>

    public classReportViewModel
    {
        public IReport Report { get; set; }

        public string AccessToken { get; set; }
    }

### <a name="connection-string"></a><span data-ttu-id="bfef5-160">Kapcsolati karakterlánc</span><span class="sxs-lookup"><span data-stu-id="bfef5-160">Connection string</span></span>

<span data-ttu-id="bfef5-161">A kapcsolati karakterláncnak a következő formátumúnak kell lennie:</span><span class="sxs-lookup"><span data-stu-id="bfef5-161">The connection string must be in the following format:</span></span>

```
Data Source=tcp:MyServer.database.windows.net,1433;Initial Catalog=MyDatabase
```

<span data-ttu-id="bfef5-162">Gyakori kiszolgáló és az adatbázis attribútumok használata sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="bfef5-162">Using common server and database attributes will fail.</span></span> <span data-ttu-id="bfef5-163">Például: Server=tcp:MyServer.database.windows.net,1433;Database=MyDatabase,</span><span class="sxs-lookup"><span data-stu-id="bfef5-163">For example: Server=tcp:MyServer.database.windows.net,1433;Database=MyDatabase,</span></span>

### <a name="view"></a><span data-ttu-id="bfef5-164">Nézet</span><span class="sxs-lookup"><span data-stu-id="bfef5-164">View</span></span>

<span data-ttu-id="bfef5-165">A **nézet** kezeli a képernyőt a Power bi **jelentések** és a Power BI **jelentés**.</span><span class="sxs-lookup"><span data-stu-id="bfef5-165">The **View** manages the display of Power BI **Reports** and a Power BI **Report**.</span></span>

<span data-ttu-id="bfef5-166">**Reports.cshtml**: ismétlés **Model.Reports** létrehozásához egy **ActionLink**.</span><span class="sxs-lookup"><span data-stu-id="bfef5-166">**Reports.cshtml**: Iterate over **Model.Reports** to create an **ActionLink**.</span></span> <span data-ttu-id="bfef5-167">A **ActionLink** össze a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="bfef5-167">The **ActionLink** is composed as follows:</span></span>

| <span data-ttu-id="bfef5-168">Rész</span><span class="sxs-lookup"><span data-stu-id="bfef5-168">Part</span></span> | <span data-ttu-id="bfef5-169">Leírás</span><span class="sxs-lookup"><span data-stu-id="bfef5-169">Description</span></span> |
| --- | --- |
| <span data-ttu-id="bfef5-170">Cím</span><span class="sxs-lookup"><span data-stu-id="bfef5-170">Title</span></span> |<span data-ttu-id="bfef5-171">A jelentés nevét.</span><span class="sxs-lookup"><span data-stu-id="bfef5-171">Name of the Report.</span></span> |
| <span data-ttu-id="bfef5-172">Lekérdezési karakterlánc</span><span class="sxs-lookup"><span data-stu-id="bfef5-172">QueryString</span></span> |<span data-ttu-id="bfef5-173">A jelentés azonosítója mutató hivatkozás</span><span class="sxs-lookup"><span data-stu-id="bfef5-173">A link to the Report ID.</span></span> |

    <div id="reports-nav" class="panel-collapse collapse">
        <div class="panel-body">
            <ul class="nav navbar-nav">
                @foreach (var report in Model.Reports)
                {
                    var reportClass = Request.QueryString["reportId"] == report.Id ? "active" : "";
                    <li class="@reportClass">
                        @Html.ActionLink(report.Name, "Report", new { reportId = report.Id })
                    </li>
                }
            </ul>
        </div>
    </div>

<span data-ttu-id="bfef5-174">Report.cshtml: Állítsa be a **Model.AccessToken**, és a Lambda kifejezést **PowerBIReportFor**.</span><span class="sxs-lookup"><span data-stu-id="bfef5-174">Report.cshtml: Set the **Model.AccessToken**, and the Lambda expression for **PowerBIReportFor**.</span></span>

    @model ReportViewModel

    ...

    <div class="side-body padding-top">
        @Html.PowerBIAccessToken(Model.AccessToken)
        @Html.PowerBIReportFor(m => m.Report, new { style = "height:85vh" })
    </div>

### <a name="controller"></a><span data-ttu-id="bfef5-175">Tartományvezérlő</span><span class="sxs-lookup"><span data-stu-id="bfef5-175">Controller</span></span>

<span data-ttu-id="bfef5-176">**DashboardController.cs**: egy PowerBIClient sikeres létrehoz egy **alkalmazási jogkivonatának**.</span><span class="sxs-lookup"><span data-stu-id="bfef5-176">**DashboardController.cs**: Creates a PowerBIClient passing an **app token**.</span></span> <span data-ttu-id="bfef5-177">A JSON webes jogkivonat (JWT) jön létre a **aláírási kulcs** lekérni a **hitelesítő adatok**.</span><span class="sxs-lookup"><span data-stu-id="bfef5-177">A JSON Web Token (JWT) is generated from the **Signing Key** to get the **Credentials**.</span></span> <span data-ttu-id="bfef5-178">A **hitelesítő adatok** példányának létrehozásához használt **PowerBIClient**.</span><span class="sxs-lookup"><span data-stu-id="bfef5-178">The **Credentials** are used to create an instance of **PowerBIClient**.</span></span> <span data-ttu-id="bfef5-179">Ha egy példánya már **PowerBIClient**, GetReports() és GetReportsAsync() hívása.</span><span class="sxs-lookup"><span data-stu-id="bfef5-179">Once you have an instance of **PowerBIClient**, you can call GetReports() and GetReportsAsync().</span></span>

<span data-ttu-id="bfef5-180">CreatePowerBIClient()</span><span class="sxs-lookup"><span data-stu-id="bfef5-180">CreatePowerBIClient()</span></span>

    private IPowerBIClient CreatePowerBIClient()
    {
        var credentials = new TokenCredentials(accessKey, "AppKey");
        var client = new PowerBIClient(credentials)
        {
            BaseUri = new Uri(apiUrl)
        };

        return client;
    }

<span data-ttu-id="bfef5-181">ActionResult Reports()</span><span class="sxs-lookup"><span data-stu-id="bfef5-181">ActionResult Reports()</span></span>

    public ActionResult Reports()
    {
        using (var client = this.CreatePowerBIClient())
        {
            var reportsResponse = client.Reports.GetReports(this.workspaceCollection, this.workspaceId);

            var viewModel = new ReportsViewModel
            {
                Reports = reportsResponse.Value.ToList()
            };

            return PartialView(viewModel);
        }
    }


<span data-ttu-id="bfef5-182">A feladat<ActionResult> jelentés (karakterlánc reportId)</span><span class="sxs-lookup"><span data-stu-id="bfef5-182">Task<ActionResult> Report(string reportId)</span></span>

    public async Task<ActionResult> Report(string reportId)
    {
        using (var client = this.CreatePowerBIClient())
        {
            var reportsResponse = await client.Reports.GetReportsAsync(this.workspaceCollection, this.workspaceId);
            var report = reportsResponse.Value.FirstOrDefault(r => r.Id == reportId);
            var embedToken = PowerBIToken.CreateReportEmbedToken(this.workspaceCollection, this.workspaceId, report.Id);

            var viewModel = new ReportViewModel
            {
                Report = report,
                AccessToken = embedToken.Generate(this.accessKey)
            };

            return View(viewModel);
        }
    }

### <a name="integrate-a-report-into-your-app"></a><span data-ttu-id="bfef5-183">A jelentések integrálásához az alkalmazásba</span><span class="sxs-lookup"><span data-stu-id="bfef5-183">Integrate a report into your app</span></span>

<span data-ttu-id="bfef5-184">Ha már van egy **jelentés**, használhat egy **IFrame** beágyazása a Power BI **jelentés**.</span><span class="sxs-lookup"><span data-stu-id="bfef5-184">Once you have a **Report**, you use an **IFrame** to embed the Power BI **Report**.</span></span> <span data-ttu-id="bfef5-185">Íme egy kódrészletet a a powerbi.js a **Microsoft Power BI Embedded** minta.</span><span class="sxs-lookup"><span data-stu-id="bfef5-185">Here is a code snippet from  powerbi.js in the **Microsoft Power BI Embedded** sample.</span></span>

![](media/powerbi-embedded-get-started-sample/power-bi-embedded-iframe-code.png)

## <a name="filter-reports-embedded-in-your-application"></a><span data-ttu-id="bfef5-186">Az alkalmazásba beágyazandó szűrő jelentések</span><span class="sxs-lookup"><span data-stu-id="bfef5-186">Filter reports embedded in your application</span></span>

<span data-ttu-id="bfef5-187">A beágyazott jelentések szűrését elvégezheti egy URL-cím szintaxisával.</span><span class="sxs-lookup"><span data-stu-id="bfef5-187">You can filter an embedded report using a URL syntax.</span></span> <span data-ttu-id="bfef5-188">Ehhez az szükséges, hozzáadhat egy **$filter** lekérdezési karakterlánc paraméter egy **eq** az iFrame src URL-cím meghatározott szűrőt tartalmazó operátort.</span><span class="sxs-lookup"><span data-stu-id="bfef5-188">To do this, you add a **$filter** query string parameter with an **eq** operator to your iFrame src url with the filter specified.</span></span> <span data-ttu-id="bfef5-189">A szűrő lekérdezés szintaxisa a következő:</span><span class="sxs-lookup"><span data-stu-id="bfef5-189">Here is the filter query syntax:</span></span>

```
https://app.powerbi.com/reportEmbed
?reportId=d2a0ea38-...-9673-ee9655d54a4a&
$filter={tableName/fieldName}%20eq%20'{fieldValue}'
```

> [!NOTE]
> <span data-ttu-id="bfef5-190">A {tableName/fieldName} nem tartalmazhat szóközt és speciális karaktereket.</span><span class="sxs-lookup"><span data-stu-id="bfef5-190">{tableName/fieldName} cannot include spaces or special characters.</span></span> <span data-ttu-id="bfef5-191">A {fieldValue} egyetlen kategorikus értéket enged.</span><span class="sxs-lookup"><span data-stu-id="bfef5-191">The {fieldValue} accepts a single categorical value.</span></span>  

## <a name="see-also"></a><span data-ttu-id="bfef5-192">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="bfef5-192">See also</span></span>

[<span data-ttu-id="bfef5-193">Microsoft Power BI Embedded gyakori helyzetek</span><span class="sxs-lookup"><span data-stu-id="bfef5-193">Common Microsoft Power BI Embedded scenarios</span></span>](power-bi-embedded-scenarios.md)  
[<span data-ttu-id="bfef5-194">Hitelesítés és engedélyezés a Power BI Embedded használatával</span><span class="sxs-lookup"><span data-stu-id="bfef5-194">Authenticating and authorizing in Power BI Embedded</span></span>](power-bi-embedded-app-token-flow.md)  
[<span data-ttu-id="bfef5-195">Jelentés beágyazása</span><span class="sxs-lookup"><span data-stu-id="bfef5-195">Embed a report</span></span>](power-bi-embedded-embed-report.md)  
[<span data-ttu-id="bfef5-196">Új jelentés létrehozása adatkészletből</span><span class="sxs-lookup"><span data-stu-id="bfef5-196">Create a new report from a dataset</span></span>](power-bi-embedded-create-report-from-dataset.md)  
[<span data-ttu-id="bfef5-197">Power BI Desktop</span><span class="sxs-lookup"><span data-stu-id="bfef5-197">Power BI Desktop</span></span>](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)  
[<span data-ttu-id="bfef5-198">JavaScript beágyazási minta</span><span class="sxs-lookup"><span data-stu-id="bfef5-198">JavaScript Embed Sample</span></span>](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
<span data-ttu-id="bfef5-199">További kérdései vannak?</span><span class="sxs-lookup"><span data-stu-id="bfef5-199">More questions?</span></span> [<span data-ttu-id="bfef5-200">Tegye próbára a Power BI közösségét</span><span class="sxs-lookup"><span data-stu-id="bfef5-200">Try the Power BI Community</span></span>](http://community.powerbi.com/)
