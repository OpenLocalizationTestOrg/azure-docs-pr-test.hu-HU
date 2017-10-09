---
title: "a minta használatába aaaGet"
description: "A Power BI Embedded, interaktív Power BI-jelentéseket az üzleti intelligenciát biztosító alkalmazásba SDK tooadd használata"
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
ms.openlocfilehash: 1fef9dd8e0f734b748b930d3f85ad4b517d9661e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-power-bi-embedded-sample"></a><span data-ttu-id="99a22-103">Bevezetés a Power BI Embedded minta használatába</span><span class="sxs-lookup"><span data-stu-id="99a22-103">Get started with Power BI Embedded sample</span></span>

<span data-ttu-id="99a22-104">A **Microsoft Power BI Embedded**, akkor integrálható a Power BI-jelentéseket jobbra a webhelyen vagy az alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="99a22-104">With **Microsoft Power BI Embedded**, you can integrate Power BI reports right into your web or mobile applications.</span></span> <span data-ttu-id="99a22-105">Ebben a cikkben azt is bemutatják a toohello **Power BI Embedded** get elindított minta.</span><span class="sxs-lookup"><span data-stu-id="99a22-105">In this article, we'll introduce you toohello **Power BI Embedded** get started sample.</span></span>

<span data-ttu-id="99a22-106">Ahhoz, hogy nyissa meg a további, érdemes a következő erőforrások toosave hello.</span><span class="sxs-lookup"><span data-stu-id="99a22-106">Before we go any further, you'll probably want toosave hello following resources.</span></span> <span data-ttu-id="99a22-107">Akkor lesz hasznos, ha integrálása a Power BI-jelentéseket hello mintaalkalmazás és a saját alkalmazásai túl.</span><span class="sxs-lookup"><span data-stu-id="99a22-107">They'll help you when integrating Power BI reports into hello sample app and your own apps too.</span></span>

* [<span data-ttu-id="99a22-108">Munkaterület webes mintaalkalmazás</span><span class="sxs-lookup"><span data-stu-id="99a22-108">Sample workspace web app</span></span>](http://go.microsoft.com/fwlink/?LinkId=761493)
* [<span data-ttu-id="99a22-109">A Power BI Embedded API-referencia</span><span class="sxs-lookup"><span data-stu-id="99a22-109">Power BI Embedded API reference</span></span>](https://msdn.microsoft.com/en-US/library/azure/mt711507.aspx)
* <span data-ttu-id="99a22-110">[A Power BI Embedded .NET SDK ](http://go.microsoft.com/fwlink/?LinkId=746472) (NuGet keresztül érhető el)</span><span class="sxs-lookup"><span data-stu-id="99a22-110">[Power BI Embedded .NET SDK ](http://go.microsoft.com/fwlink/?LinkId=746472) (available via NuGet)</span></span>
* [<span data-ttu-id="99a22-111">JavaScript-jelentés beágyazása minta</span><span class="sxs-lookup"><span data-stu-id="99a22-111">JavaScript Report Embed Sample</span></span>](https://microsoft.github.io/PowerBI-JavaScript/demo)

> [!NOTE] 
> <span data-ttu-id="99a22-112">Konfigurálhatja és futtatási hello Power BI Embedded első lépések minta, kell legalább egy toocreate **munkaterület-csoport** az Azure-előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="99a22-112">Before you can configure and run hello Power BI Embedded get started sample, you need toocreate at least one **Workspace Collection** in your Azure subscription.</span></span> <span data-ttu-id="99a22-113">toolearn hogyan toocreate egy **munkaterület-csoportok** hello Azure Portal látható [Ismerkedés a Power BI Embedded](power-bi-embedded-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="99a22-113">toolearn how toocreate a **Workspace Collection** in hello Azure Portal see [Getting Started with Power BI Embedded](power-bi-embedded-get-started.md).</span></span>

## <a name="configure-hello-sample-app"></a><span data-ttu-id="99a22-114">Hello mintaalkalmazás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="99a22-114">Configure hello sample app</span></span>

<span data-ttu-id="99a22-115">Bemutatjuk, a Visual Studio fejlesztői környezetben tooaccess hello összetevők szükséges toorun hello mintaalkalmazás beállításának lépésein.</span><span class="sxs-lookup"><span data-stu-id="99a22-115">Let's walk through setting up your Visual Studio development environment tooaccess hello  components needed toorun hello sample app.</span></span>

1. <span data-ttu-id="99a22-116">Töltse le és csomagolja ki a hello [Power BI Embedded - jelentés integrálásához webalkalmazás](http://go.microsoft.com/fwlink/?LinkId=761493) mintát a Githubon.</span><span class="sxs-lookup"><span data-stu-id="99a22-116">Download and unzip hello [Power BI Embedded - Integrate a report into a web app](http://go.microsoft.com/fwlink/?LinkId=761493) sample on GitHub.</span></span>
2. <span data-ttu-id="99a22-117">Nyissa meg **Power bi-embedded.sln** a Visual Studióban.</span><span class="sxs-lookup"><span data-stu-id="99a22-117">Open **PowerBI-embedded.sln** in Visual Studio.</span></span> <span data-ttu-id="99a22-118">Előfordulhat, hogy tooexecute hello **-csomag** hello NuGET Package Manager Console rendelés tooupdate hello csomagok ebben a megoldásban használt parancsot.</span><span class="sxs-lookup"><span data-stu-id="99a22-118">You may need tooexecute hello **Update-Package** command in hello NuGET Package Manager Console in order tooupdate hello packages used in this solution.</span></span>
3. <span data-ttu-id="99a22-119">Hello megoldás felépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="99a22-119">Build hello solution.</span></span>
4. <span data-ttu-id="99a22-120">Futtassa a hello **ProvisionSample** konzolalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="99a22-120">Run hello **ProvisionSample** console app.</span></span> <span data-ttu-id="99a22-121">A konzol hello mintaalkalmazást munkaterület kiépíteni, és PBIX-fájl importálása.</span><span class="sxs-lookup"><span data-stu-id="99a22-121">In hello sample console app, you provision a workspace and import a PBIX file.</span></span>
5. <span data-ttu-id="99a22-122">új tooprovision **munkaterület**, válassza az 1. lehetőség – **gyűjtési felügyeleti**, és választhatja meg a 6., **új munkaterület kiépítése**</span><span class="sxs-lookup"><span data-stu-id="99a22-122">tooprovision a new **Workspace**, select option 1, **Collection management**, and then select option 6, **Provision a new Workspace**</span></span>
6. <span data-ttu-id="99a22-123">új tooimport **jelentés**, válassza ki a 2. lehetőség – **felügyeleti jelentést**, majd válassza ki a 3. lehetőség – **importálási pbix-fájlt asztali fájlt a munkaterületre**.</span><span class="sxs-lookup"><span data-stu-id="99a22-123">tooimport a new **Report**, select option 2, **Report management**, and then select option 3, **Import PBIX Desktop file into a workspace**.</span></span>

7. <span data-ttu-id="99a22-124">Adja meg a **munkaterület-csoportok** nevét, és **hozzáférési kulcs**.</span><span class="sxs-lookup"><span data-stu-id="99a22-124">Enter your **Workspace Collection** name, and **Access Key**.</span></span> <span data-ttu-id="99a22-125">Ezeket a hello kaphat **Azure Portal**.</span><span class="sxs-lookup"><span data-stu-id="99a22-125">You can get these in hello **Azure Portal**.</span></span> <span data-ttu-id="99a22-126">toolearn kapcsolatos további tooget a **hozzáférési kulcs**, lásd: [Power BI API elérési kulcsainak megtekintése](power-bi-embedded-get-started.md#view-power-bi-api-access-keys) a Ismerkedés a Microsoft Power BI Embedded.</span><span class="sxs-lookup"><span data-stu-id="99a22-126">toolearn more about how tooget your **Access Key**, see [View Power BI API Access Keys](power-bi-embedded-get-started.md#view-power-bi-api-access-keys) in Get started with Microsoft Power BI Embedded.</span></span>

    ![](media/powerbi-embedded-get-started-sample/azure-portal.png)
8. <span data-ttu-id="99a22-127">Másolja ki és mentse az újonnan létrehozott hello **munkaterület azonosítója** toouse a cikk későbbi részében.</span><span class="sxs-lookup"><span data-stu-id="99a22-127">Copy and save hello newly created **Workspace ID** toouse later in this article.</span></span> <span data-ttu-id="99a22-128">Hello után **munkaterület azonosítója** van létrehozva, megtalálhatja hello **Azure Portal**.</span><span class="sxs-lookup"><span data-stu-id="99a22-128">After hello **Workspace ID** is created, you can find it hello **Azure Portal**.</span></span>

    ![](media/powerbi-embedded-get-started-sample/workspace-id.png)
9. <span data-ttu-id="99a22-129">a pbix-fájlt tooimport fájlt a **munkaterület**, a beállításnak a **6. Egy meglévő munkaterületre pbix-fájlt asztali importfájl**.</span><span class="sxs-lookup"><span data-stu-id="99a22-129">tooimport a PBIX file into your **Workspace**, select option **6. Import PBIX Desktop file into an existing workspace**.</span></span> <span data-ttu-id="99a22-130">Ha még nem rendelkezik a PBIX-fájl lesz szüksége, érdemes letöltenie hello [kiskereskedelmi elemzési minta pbix-fájlt](http://go.microsoft.com/fwlink/?LinkID=780547).</span><span class="sxs-lookup"><span data-stu-id="99a22-130">If you don't have a PBIX file handy, you can download hello [Retail Analysis Sample PBIX](http://go.microsoft.com/fwlink/?LinkID=780547).</span></span>
10. <span data-ttu-id="99a22-131">Ha a rendszer kéri, adjon meg egy rövid nevet a **Dataset**.</span><span class="sxs-lookup"><span data-stu-id="99a22-131">If prompted, enter a friendly name for your **Dataset**.</span></span>

<span data-ttu-id="99a22-132">A következőhöz hasonló választ kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="99a22-132">You should see a response like:</span></span>

```
Checking import state... Publishing
Checking import state... Succeeded
```

> [!NOTE]
> <span data-ttu-id="99a22-133">Ha a PBIX-fájl tartalmazza a közvetlen lekérdezés kapcsolatokat, futtassa a beállítás 7 tooupdate hello kapcsolati karakterláncokat.</span><span class="sxs-lookup"><span data-stu-id="99a22-133">If your PBIX file contains any direct query connections, run option 7 tooupdate hello connection strings.</span></span>

<span data-ttu-id="99a22-134">Ezen a ponton rendelkezik egy Power BI pbix-fájlt a jelentés importálni a **munkaterület**.</span><span class="sxs-lookup"><span data-stu-id="99a22-134">At this point, you have a Power BI PBIX report imported into your **Workspace**.</span></span> <span data-ttu-id="99a22-135">Most hogyan nézzük toorun hello **Power BI Embedded** elindított minta webalkalmazás beolvasása.</span><span class="sxs-lookup"><span data-stu-id="99a22-135">Now, let's look at how toorun hello **Power BI Embedded** get started sample web app.</span></span>

## <a name="run-hello-sample-web-app"></a><span data-ttu-id="99a22-136">Hello webes mintaalkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="99a22-136">Run hello sample web app</span></span>
<span data-ttu-id="99a22-137">hello web app minta egy jeleníti meg az importált jelentések mintaalkalmazás a **munkaterület**.</span><span class="sxs-lookup"><span data-stu-id="99a22-137">hello web app sample is a sample application that renders reports imported into your **Workspace**.</span></span> <span data-ttu-id="99a22-138">Ez hogyan tooconfigure hello web app minta.</span><span class="sxs-lookup"><span data-stu-id="99a22-138">Here's how tooconfigure hello web app sample.</span></span>

1. <span data-ttu-id="99a22-139">A hello **Power bi embedded** Visual Studio megoldás, a jobb oldali kattintson hello **EmbedSample** webalkalmazást, és válassza a **beállítás kezdőprojektként**.</span><span class="sxs-lookup"><span data-stu-id="99a22-139">In hello **PowerBI-embedded** Visual Studio solution, right click hello **EmbedSample** web application, and choose **Set as StartUp project**.</span></span>
2. <span data-ttu-id="99a22-140">A **web.config**, a hello **EmbedSample** webalkalmazást, hello szerkesztése **appSettings**: **AccessKey**,  **WorkspaceCollection** nevét, és **WorkspaceId**.</span><span class="sxs-lookup"><span data-stu-id="99a22-140">In **web.config**, in hello **EmbedSample** web application, edit hello **appSettings**: **AccessKey**, **WorkspaceCollection** name, and **WorkspaceId**.</span></span>

    ```
    <appSettings>
        <add key="powerbi:AccessKey" value="" />
        <add key="powerbi:ApiUrl" value="https://api.powerbi.com" />
        <add key="powerbi:WorkspaceCollection" value="" />
        <add key="powerbi:WorkspaceId" value="" />
    </appSettings>
    ```
3. <span data-ttu-id="99a22-141">Futtassa a hello **EmbedSample** webalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="99a22-141">Run hello **EmbedSample** web application.</span></span>

<span data-ttu-id="99a22-142">Miután lefuttatta a hello **EmbedSample** webalkalmazás, hello bal oldali navigációs panelen tartalmaznia kell egy **jelentések** menü.</span><span class="sxs-lookup"><span data-stu-id="99a22-142">Once you run hello **EmbedSample** web application, hello left navigation panel should contain a **Reports** menu.</span></span> <span data-ttu-id="99a22-143">Bontsa ki a tooview hello jelentés importált, **jelentések**, és kattintson a jelentés.</span><span class="sxs-lookup"><span data-stu-id="99a22-143">tooview hello report you imported, expand **Reports**, and click a report.</span></span> <span data-ttu-id="99a22-144">Ha az importált hello [kiskereskedelmi elemzési minta pbix-fájlt](http://go.microsoft.com/fwlink/?LinkID=780547), hello minta webalkalmazás néz ki:</span><span class="sxs-lookup"><span data-stu-id="99a22-144">If you imported hello [Retail Analysis Sample PBIX](http://go.microsoft.com/fwlink/?LinkID=780547), hello sample web app would look like this:</span></span>

![](media/powerbi-embedded-get-started-sample/power-bi-embedded-sample-left-nav.png)

<span data-ttu-id="99a22-145">A jelentés gombra kattintva hello **EmbedSample** webalkalmazás kell kinéznie ezt:</span><span class="sxs-lookup"><span data-stu-id="99a22-145">After you click a report, hello **EmbedSample** web application should look something this:</span></span>

![](media/powerbi-embedded-get-started-sample/sample-web-app.png)

## <a name="explore-hello-sample-code"></a><span data-ttu-id="99a22-146">Hello mintakód felfedezés</span><span class="sxs-lookup"><span data-stu-id="99a22-146">Explore hello sample code</span></span>

<span data-ttu-id="99a22-147">Hello **Microsoft Power BI Embedded** minta egy példa-webalkalmazást, amely bemutatja, hogyan toointegrate **Power BI** jelentések az alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="99a22-147">hello **Microsoft Power BI Embedded** sample is an example web app that shows you how toointegrate **Power BI** reports into your app.</span></span> <span data-ttu-id="99a22-148">Akkor használja, a Model-View-Controller (MVC) tervezési minta toodemonstrate ajánlott eljárások.</span><span class="sxs-lookup"><span data-stu-id="99a22-148">It uses a Model-View-Controller (MVC) design pattern toodemonstrate best practices.</span></span> <span data-ttu-id="99a22-149">Ez a szakasz kiemeli a hello mintakód, amely belül hello részei **Power bi embedded** web app megoldás.</span><span class="sxs-lookup"><span data-stu-id="99a22-149">This section highlights parts of hello sample code that you can explore within hello **PowerBI-embedded** web app solution.</span></span> <span data-ttu-id="99a22-150">hello Model-View-Controller (MVC) mintát elválasztja hello modellezési hello tartomány, a hello bemutató és a felhasználói bevitel három különálló osztályokba alapuló hello műveletek: modell, megtekintése és vezérlő.</span><span class="sxs-lookup"><span data-stu-id="99a22-150">hello Model-View-Controller (MVC) pattern separates hello modeling of hello domain, hello presentation, and hello actions based on user input into three separate classes: Model, View, and Control.</span></span> <span data-ttu-id="99a22-151">toolearn MVC, kapcsolatos további információkért lásd: [ismerje meg, az ASP.NET kapcsolatos](http://www.asp.net/mvc).</span><span class="sxs-lookup"><span data-stu-id="99a22-151">toolearn more about MVC, see [Learn About ASP.NET](http://www.asp.net/mvc).</span></span>

<span data-ttu-id="99a22-152">Hello **Microsoft Power BI Embedded** mintakód választja el az alábbiak szerint.</span><span class="sxs-lookup"><span data-stu-id="99a22-152">hello **Microsoft Power BI Embedded** sample code is separated as follows.</span></span> <span data-ttu-id="99a22-153">Az egyes szakaszokon hello fájlnév, hogy könnyedén megtalálhatja a hello kód hello minta a Power bi-embedded.sln megoldás hello tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="99a22-153">Each section includes hello file name in hello PowerBI-embedded.sln solution so that you can easily find hello code in hello sample.</span></span>

> [!NOTE]
> <span data-ttu-id="99a22-154">Ez a szakasz az hello mintakód bemutatja, hogyan hello kód írása történt összegzését.</span><span class="sxs-lookup"><span data-stu-id="99a22-154">This section is a summary of hello sample code that shows how hello code was written.</span></span> <span data-ttu-id="99a22-155">tooview hello teljes mintát, töltse be a Power bi-embedded.sln hello megoldást a Visual Studióban.</span><span class="sxs-lookup"><span data-stu-id="99a22-155">tooview hello complete sample, please load hello PowerBI-embedded.sln solution in Visual Studio.</span></span>

### <a name="model"></a><span data-ttu-id="99a22-156">Modell</span><span class="sxs-lookup"><span data-stu-id="99a22-156">Model</span></span>

<span data-ttu-id="99a22-157">hello a minta egy **ReportsViewModel** és **ReportViewModel**.</span><span class="sxs-lookup"><span data-stu-id="99a22-157">hello sample has a **ReportsViewModel** and **ReportViewModel**.</span></span>

<span data-ttu-id="99a22-158">**ReportsViewModel.cs**: jelenti. a Power BI-jelentéseket.</span><span class="sxs-lookup"><span data-stu-id="99a22-158">**ReportsViewModel.cs**: Represents Power BI Reports.</span></span>

    public class ReportsViewModel
    {
        public List<Report> Reports { get; set; }
    }

<span data-ttu-id="99a22-159">**ReportViewModel.cs**: a Power BI-jelentés jelöli.</span><span class="sxs-lookup"><span data-stu-id="99a22-159">**ReportViewModel.cs**: Represents a Power BI Report.</span></span>

    public classReportViewModel
    {
        public IReport Report { get; set; }

        public string AccessToken { get; set; }
    }

### <a name="connection-string"></a><span data-ttu-id="99a22-160">Kapcsolati karakterlánc</span><span class="sxs-lookup"><span data-stu-id="99a22-160">Connection string</span></span>

<span data-ttu-id="99a22-161">hello kapcsolati karakterlánc formátuma a következő hello kell lennie:</span><span class="sxs-lookup"><span data-stu-id="99a22-161">hello connection string must be in hello following format:</span></span>

```
Data Source=tcp:MyServer.database.windows.net,1433;Initial Catalog=MyDatabase
```

<span data-ttu-id="99a22-162">Gyakori kiszolgáló és az adatbázis attribútumok használata sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="99a22-162">Using common server and database attributes will fail.</span></span> <span data-ttu-id="99a22-163">Például: Server=tcp:MyServer.database.windows.net,1433;Database=MyDatabase,</span><span class="sxs-lookup"><span data-stu-id="99a22-163">For example: Server=tcp:MyServer.database.windows.net,1433;Database=MyDatabase,</span></span>

### <a name="view"></a><span data-ttu-id="99a22-164">Nézet</span><span class="sxs-lookup"><span data-stu-id="99a22-164">View</span></span>

<span data-ttu-id="99a22-165">Hello **nézet** kezeli a Power bi hello megjelenítési **jelentések** és a Power BI **jelentés**.</span><span class="sxs-lookup"><span data-stu-id="99a22-165">hello **View** manages hello display of Power BI **Reports** and a Power BI **Report**.</span></span>

<span data-ttu-id="99a22-166">**Reports.cshtml**: ismétlés **Model.Reports** toocreate egy **ActionLink**.</span><span class="sxs-lookup"><span data-stu-id="99a22-166">**Reports.cshtml**: Iterate over **Model.Reports** toocreate an **ActionLink**.</span></span> <span data-ttu-id="99a22-167">Hello **ActionLink** össze a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="99a22-167">hello **ActionLink** is composed as follows:</span></span>

| <span data-ttu-id="99a22-168">Rész</span><span class="sxs-lookup"><span data-stu-id="99a22-168">Part</span></span> | <span data-ttu-id="99a22-169">Leírás</span><span class="sxs-lookup"><span data-stu-id="99a22-169">Description</span></span> |
| --- | --- |
| <span data-ttu-id="99a22-170">Cím</span><span class="sxs-lookup"><span data-stu-id="99a22-170">Title</span></span> |<span data-ttu-id="99a22-171">Hello jelentés neve.</span><span class="sxs-lookup"><span data-stu-id="99a22-171">Name of hello Report.</span></span> |
| <span data-ttu-id="99a22-172">Lekérdezési karakterlánc</span><span class="sxs-lookup"><span data-stu-id="99a22-172">QueryString</span></span> |<span data-ttu-id="99a22-173">A hivatkozás toohello jelentés azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="99a22-173">A link toohello Report ID.</span></span> |

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

<span data-ttu-id="99a22-174">Report.cshtml: Hello beállítása **Model.AccessToken**, és a Lambda kifejezés hello **PowerBIReportFor**.</span><span class="sxs-lookup"><span data-stu-id="99a22-174">Report.cshtml: Set hello **Model.AccessToken**, and hello Lambda expression for **PowerBIReportFor**.</span></span>

    @model ReportViewModel

    ...

    <div class="side-body padding-top">
        @Html.PowerBIAccessToken(Model.AccessToken)
        @Html.PowerBIReportFor(m => m.Report, new { style = "height:85vh" })
    </div>

### <a name="controller"></a><span data-ttu-id="99a22-175">Tartományvezérlő</span><span class="sxs-lookup"><span data-stu-id="99a22-175">Controller</span></span>

<span data-ttu-id="99a22-176">**DashboardController.cs**: egy PowerBIClient sikeres létrehoz egy **alkalmazási jogkivonatának**.</span><span class="sxs-lookup"><span data-stu-id="99a22-176">**DashboardController.cs**: Creates a PowerBIClient passing an **app token**.</span></span> <span data-ttu-id="99a22-177">A JSON webes jogkivonat (JWT) jönnek létre hello **aláírási kulcs** tooget hello **hitelesítő adatok**.</span><span class="sxs-lookup"><span data-stu-id="99a22-177">A JSON Web Token (JWT) is generated from hello **Signing Key** tooget hello **Credentials**.</span></span> <span data-ttu-id="99a22-178">Hello **hitelesítő adatok** vannak használt toocreate példányának **PowerBIClient**.</span><span class="sxs-lookup"><span data-stu-id="99a22-178">hello **Credentials** are used toocreate an instance of **PowerBIClient**.</span></span> <span data-ttu-id="99a22-179">Ha egy példánya már **PowerBIClient**, GetReports() és GetReportsAsync() hívása.</span><span class="sxs-lookup"><span data-stu-id="99a22-179">Once you have an instance of **PowerBIClient**, you can call GetReports() and GetReportsAsync().</span></span>

<span data-ttu-id="99a22-180">CreatePowerBIClient()</span><span class="sxs-lookup"><span data-stu-id="99a22-180">CreatePowerBIClient()</span></span>

    private IPowerBIClient CreatePowerBIClient()
    {
        var credentials = new TokenCredentials(accessKey, "AppKey");
        var client = new PowerBIClient(credentials)
        {
            BaseUri = new Uri(apiUrl)
        };

        return client;
    }

<span data-ttu-id="99a22-181">ActionResult Reports()</span><span class="sxs-lookup"><span data-stu-id="99a22-181">ActionResult Reports()</span></span>

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


<span data-ttu-id="99a22-182">A feladat<ActionResult> jelentés (karakterlánc reportId)</span><span class="sxs-lookup"><span data-stu-id="99a22-182">Task<ActionResult> Report(string reportId)</span></span>

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

### <a name="integrate-a-report-into-your-app"></a><span data-ttu-id="99a22-183">A jelentések integrálásához az alkalmazásba</span><span class="sxs-lookup"><span data-stu-id="99a22-183">Integrate a report into your app</span></span>

<span data-ttu-id="99a22-184">Ha már van egy **jelentés**, használhat egy **IFrame** tooembed hello Power BI **jelentés**.</span><span class="sxs-lookup"><span data-stu-id="99a22-184">Once you have a **Report**, you use an **IFrame** tooembed hello Power BI **Report**.</span></span> <span data-ttu-id="99a22-185">Íme egy kódrészletet a hello powerbi.js a **Microsoft Power BI Embedded** minta.</span><span class="sxs-lookup"><span data-stu-id="99a22-185">Here is a code snippet from  powerbi.js in hello **Microsoft Power BI Embedded** sample.</span></span>

![](media/powerbi-embedded-get-started-sample/power-bi-embedded-iframe-code.png)

## <a name="filter-reports-embedded-in-your-application"></a><span data-ttu-id="99a22-186">Az alkalmazásba beágyazandó szűrő jelentések</span><span class="sxs-lookup"><span data-stu-id="99a22-186">Filter reports embedded in your application</span></span>

<span data-ttu-id="99a22-187">A beágyazott jelentések szűrését elvégezheti egy URL-cím szintaxisával.</span><span class="sxs-lookup"><span data-stu-id="99a22-187">You can filter an embedded report using a URL syntax.</span></span> <span data-ttu-id="99a22-188">toodo, hozzáadhat egy **$filter** lekérdezési karakterlánc paraméter egy **eq** operátor tooyour iFrame src url megadott hello szűrő.</span><span class="sxs-lookup"><span data-stu-id="99a22-188">toodo this, you add a **$filter** query string parameter with an **eq** operator tooyour iFrame src url with hello filter specified.</span></span> <span data-ttu-id="99a22-189">Hello szűrő lekérdezés szintaxisa a következő:</span><span class="sxs-lookup"><span data-stu-id="99a22-189">Here is hello filter query syntax:</span></span>

```
https://app.powerbi.com/reportEmbed
?reportId=d2a0ea38-...-9673-ee9655d54a4a&
$filter={tableName/fieldName}%20eq%20'{fieldValue}'
```

> [!NOTE]
> <span data-ttu-id="99a22-190">A {tableName/fieldName} nem tartalmazhat szóközt és speciális karaktereket.</span><span class="sxs-lookup"><span data-stu-id="99a22-190">{tableName/fieldName} cannot include spaces or special characters.</span></span> <span data-ttu-id="99a22-191">hello {fieldValue} egyetlen kategorikus értéket fogad el.</span><span class="sxs-lookup"><span data-stu-id="99a22-191">hello {fieldValue} accepts a single categorical value.</span></span>  

## <a name="see-also"></a><span data-ttu-id="99a22-192">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="99a22-192">See also</span></span>

[<span data-ttu-id="99a22-193">Microsoft Power BI Embedded gyakori helyzetek</span><span class="sxs-lookup"><span data-stu-id="99a22-193">Common Microsoft Power BI Embedded scenarios</span></span>](power-bi-embedded-scenarios.md)  
[<span data-ttu-id="99a22-194">Hitelesítés és engedélyezés a Power BI Embedded használatával</span><span class="sxs-lookup"><span data-stu-id="99a22-194">Authenticating and authorizing in Power BI Embedded</span></span>](power-bi-embedded-app-token-flow.md)  
[<span data-ttu-id="99a22-195">Jelentés beágyazása</span><span class="sxs-lookup"><span data-stu-id="99a22-195">Embed a report</span></span>](power-bi-embedded-embed-report.md)  
[<span data-ttu-id="99a22-196">Új jelentés létrehozása adatkészletből</span><span class="sxs-lookup"><span data-stu-id="99a22-196">Create a new report from a dataset</span></span>](power-bi-embedded-create-report-from-dataset.md)  
[<span data-ttu-id="99a22-197">Power BI Desktop</span><span class="sxs-lookup"><span data-stu-id="99a22-197">Power BI Desktop</span></span>](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)  
[<span data-ttu-id="99a22-198">JavaScript beágyazási minta</span><span class="sxs-lookup"><span data-stu-id="99a22-198">JavaScript Embed Sample</span></span>](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
<span data-ttu-id="99a22-199">További kérdései vannak?</span><span class="sxs-lookup"><span data-stu-id="99a22-199">More questions?</span></span> [<span data-ttu-id="99a22-200">Próbálja meg a Power BI-Közösség hello</span><span class="sxs-lookup"><span data-stu-id="99a22-200">Try hello Power BI Community</span></span>](http://community.powerbi.com/)
