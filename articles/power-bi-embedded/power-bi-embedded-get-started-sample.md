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
# <a name="get-started-with-power-bi-embedded-sample"></a>Bevezetés a Power BI Embedded minta használatába

A **Microsoft Power BI Embedded**, akkor integrálható a Power BI-jelentéseket jobbra a webhelyen vagy az alkalmazásokat. Ebben a cikkben azt is bemutatják a toohello **Power BI Embedded** get elindított minta.

Ahhoz, hogy nyissa meg a további, érdemes a következő erőforrások toosave hello. Akkor lesz hasznos, ha integrálása a Power BI-jelentéseket hello mintaalkalmazás és a saját alkalmazásai túl.

* [Munkaterület webes mintaalkalmazás](http://go.microsoft.com/fwlink/?LinkId=761493)
* [A Power BI Embedded API-referencia](https://msdn.microsoft.com/en-US/library/azure/mt711507.aspx)
* [A Power BI Embedded .NET SDK ](http://go.microsoft.com/fwlink/?LinkId=746472) (NuGet keresztül érhető el)
* [JavaScript-jelentés beágyazása minta](https://microsoft.github.io/PowerBI-JavaScript/demo)

> [!NOTE] 
> Konfigurálhatja és futtatási hello Power BI Embedded első lépések minta, kell legalább egy toocreate **munkaterület-csoport** az Azure-előfizetésben. toolearn hogyan toocreate egy **munkaterület-csoportok** hello Azure Portal látható [Ismerkedés a Power BI Embedded](power-bi-embedded-get-started.md).

## <a name="configure-hello-sample-app"></a>Hello mintaalkalmazás konfigurálása

Bemutatjuk, a Visual Studio fejlesztői környezetben tooaccess hello összetevők szükséges toorun hello mintaalkalmazás beállításának lépésein.

1. Töltse le és csomagolja ki a hello [Power BI Embedded - jelentés integrálásához webalkalmazás](http://go.microsoft.com/fwlink/?LinkId=761493) mintát a Githubon.
2. Nyissa meg **Power bi-embedded.sln** a Visual Studióban. Előfordulhat, hogy tooexecute hello **-csomag** hello NuGET Package Manager Console rendelés tooupdate hello csomagok ebben a megoldásban használt parancsot.
3. Hello megoldás felépítéséhez.
4. Futtassa a hello **ProvisionSample** konzolalkalmazást. A konzol hello mintaalkalmazást munkaterület kiépíteni, és PBIX-fájl importálása.
5. új tooprovision **munkaterület**, válassza az 1. lehetőség – **gyűjtési felügyeleti**, és választhatja meg a 6., **új munkaterület kiépítése**
6. új tooimport **jelentés**, válassza ki a 2. lehetőség – **felügyeleti jelentést**, majd válassza ki a 3. lehetőség – **importálási pbix-fájlt asztali fájlt a munkaterületre**.

7. Adja meg a **munkaterület-csoportok** nevét, és **hozzáférési kulcs**. Ezeket a hello kaphat **Azure Portal**. toolearn kapcsolatos további tooget a **hozzáférési kulcs**, lásd: [Power BI API elérési kulcsainak megtekintése](power-bi-embedded-get-started.md#view-power-bi-api-access-keys) a Ismerkedés a Microsoft Power BI Embedded.

    ![](media/powerbi-embedded-get-started-sample/azure-portal.png)
8. Másolja ki és mentse az újonnan létrehozott hello **munkaterület azonosítója** toouse a cikk későbbi részében. Hello után **munkaterület azonosítója** van létrehozva, megtalálhatja hello **Azure Portal**.

    ![](media/powerbi-embedded-get-started-sample/workspace-id.png)
9. a pbix-fájlt tooimport fájlt a **munkaterület**, a beállításnak a **6. Egy meglévő munkaterületre pbix-fájlt asztali importfájl**. Ha még nem rendelkezik a PBIX-fájl lesz szüksége, érdemes letöltenie hello [kiskereskedelmi elemzési minta pbix-fájlt](http://go.microsoft.com/fwlink/?LinkID=780547).
10. Ha a rendszer kéri, adjon meg egy rövid nevet a **Dataset**.

A következőhöz hasonló választ kell megjelennie:

```
Checking import state... Publishing
Checking import state... Succeeded
```

> [!NOTE]
> Ha a PBIX-fájl tartalmazza a közvetlen lekérdezés kapcsolatokat, futtassa a beállítás 7 tooupdate hello kapcsolati karakterláncokat.

Ezen a ponton rendelkezik egy Power BI pbix-fájlt a jelentés importálni a **munkaterület**. Most hogyan nézzük toorun hello **Power BI Embedded** elindított minta webalkalmazás beolvasása.

## <a name="run-hello-sample-web-app"></a>Hello webes mintaalkalmazás futtatása
hello web app minta egy jeleníti meg az importált jelentések mintaalkalmazás a **munkaterület**. Ez hogyan tooconfigure hello web app minta.

1. A hello **Power bi embedded** Visual Studio megoldás, a jobb oldali kattintson hello **EmbedSample** webalkalmazást, és válassza a **beállítás kezdőprojektként**.
2. A **web.config**, a hello **EmbedSample** webalkalmazást, hello szerkesztése **appSettings**: **AccessKey**,  **WorkspaceCollection** nevét, és **WorkspaceId**.

    ```
    <appSettings>
        <add key="powerbi:AccessKey" value="" />
        <add key="powerbi:ApiUrl" value="https://api.powerbi.com" />
        <add key="powerbi:WorkspaceCollection" value="" />
        <add key="powerbi:WorkspaceId" value="" />
    </appSettings>
    ```
3. Futtassa a hello **EmbedSample** webalkalmazást.

Miután lefuttatta a hello **EmbedSample** webalkalmazás, hello bal oldali navigációs panelen tartalmaznia kell egy **jelentések** menü. Bontsa ki a tooview hello jelentés importált, **jelentések**, és kattintson a jelentés. Ha az importált hello [kiskereskedelmi elemzési minta pbix-fájlt](http://go.microsoft.com/fwlink/?LinkID=780547), hello minta webalkalmazás néz ki:

![](media/powerbi-embedded-get-started-sample/power-bi-embedded-sample-left-nav.png)

A jelentés gombra kattintva hello **EmbedSample** webalkalmazás kell kinéznie ezt:

![](media/powerbi-embedded-get-started-sample/sample-web-app.png)

## <a name="explore-hello-sample-code"></a>Hello mintakód felfedezés

Hello **Microsoft Power BI Embedded** minta egy példa-webalkalmazást, amely bemutatja, hogyan toointegrate **Power BI** jelentések az alkalmazásba. Akkor használja, a Model-View-Controller (MVC) tervezési minta toodemonstrate ajánlott eljárások. Ez a szakasz kiemeli a hello mintakód, amely belül hello részei **Power bi embedded** web app megoldás. hello Model-View-Controller (MVC) mintát elválasztja hello modellezési hello tartomány, a hello bemutató és a felhasználói bevitel három különálló osztályokba alapuló hello műveletek: modell, megtekintése és vezérlő. toolearn MVC, kapcsolatos további információkért lásd: [ismerje meg, az ASP.NET kapcsolatos](http://www.asp.net/mvc).

Hello **Microsoft Power BI Embedded** mintakód választja el az alábbiak szerint. Az egyes szakaszokon hello fájlnév, hogy könnyedén megtalálhatja a hello kód hello minta a Power bi-embedded.sln megoldás hello tartalmazza.

> [!NOTE]
> Ez a szakasz az hello mintakód bemutatja, hogyan hello kód írása történt összegzését. tooview hello teljes mintát, töltse be a Power bi-embedded.sln hello megoldást a Visual Studióban.

### <a name="model"></a>Modell

hello a minta egy **ReportsViewModel** és **ReportViewModel**.

**ReportsViewModel.cs**: jelenti. a Power BI-jelentéseket.

    public class ReportsViewModel
    {
        public List<Report> Reports { get; set; }
    }

**ReportViewModel.cs**: a Power BI-jelentés jelöli.

    public classReportViewModel
    {
        public IReport Report { get; set; }

        public string AccessToken { get; set; }
    }

### <a name="connection-string"></a>Kapcsolati karakterlánc

hello kapcsolati karakterlánc formátuma a következő hello kell lennie:

```
Data Source=tcp:MyServer.database.windows.net,1433;Initial Catalog=MyDatabase
```

Gyakori kiszolgáló és az adatbázis attribútumok használata sikertelen lesz. Például: Server=tcp:MyServer.database.windows.net,1433;Database=MyDatabase,

### <a name="view"></a>Nézet

Hello **nézet** kezeli a Power bi hello megjelenítési **jelentések** és a Power BI **jelentés**.

**Reports.cshtml**: ismétlés **Model.Reports** toocreate egy **ActionLink**. Hello **ActionLink** össze a következőképpen történik:

| Rész | Leírás |
| --- | --- |
| Cím |Hello jelentés neve. |
| Lekérdezési karakterlánc |A hivatkozás toohello jelentés azonosítóját. |

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

Report.cshtml: Hello beállítása **Model.AccessToken**, és a Lambda kifejezés hello **PowerBIReportFor**.

    @model ReportViewModel

    ...

    <div class="side-body padding-top">
        @Html.PowerBIAccessToken(Model.AccessToken)
        @Html.PowerBIReportFor(m => m.Report, new { style = "height:85vh" })
    </div>

### <a name="controller"></a>Tartományvezérlő

**DashboardController.cs**: egy PowerBIClient sikeres létrehoz egy **alkalmazási jogkivonatának**. A JSON webes jogkivonat (JWT) jönnek létre hello **aláírási kulcs** tooget hello **hitelesítő adatok**. Hello **hitelesítő adatok** vannak használt toocreate példányának **PowerBIClient**. Ha egy példánya már **PowerBIClient**, GetReports() és GetReportsAsync() hívása.

CreatePowerBIClient()

    private IPowerBIClient CreatePowerBIClient()
    {
        var credentials = new TokenCredentials(accessKey, "AppKey");
        var client = new PowerBIClient(credentials)
        {
            BaseUri = new Uri(apiUrl)
        };

        return client;
    }

ActionResult Reports()

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


A feladat<ActionResult> jelentés (karakterlánc reportId)

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

### <a name="integrate-a-report-into-your-app"></a>A jelentések integrálásához az alkalmazásba

Ha már van egy **jelentés**, használhat egy **IFrame** tooembed hello Power BI **jelentés**. Íme egy kódrészletet a hello powerbi.js a **Microsoft Power BI Embedded** minta.

![](media/powerbi-embedded-get-started-sample/power-bi-embedded-iframe-code.png)

## <a name="filter-reports-embedded-in-your-application"></a>Az alkalmazásba beágyazandó szűrő jelentések

A beágyazott jelentések szűrését elvégezheti egy URL-cím szintaxisával. toodo, hozzáadhat egy **$filter** lekérdezési karakterlánc paraméter egy **eq** operátor tooyour iFrame src url megadott hello szűrő. Hello szűrő lekérdezés szintaxisa a következő:

```
https://app.powerbi.com/reportEmbed
?reportId=d2a0ea38-...-9673-ee9655d54a4a&
$filter={tableName/fieldName}%20eq%20'{fieldValue}'
```

> [!NOTE]
> A {tableName/fieldName} nem tartalmazhat szóközt és speciális karaktereket. hello {fieldValue} egyetlen kategorikus értéket fogad el.  

## <a name="see-also"></a>Lásd még:

[Microsoft Power BI Embedded gyakori helyzetek](power-bi-embedded-scenarios.md)  
[Hitelesítés és engedélyezés a Power BI Embedded használatával](power-bi-embedded-app-token-flow.md)  
[Jelentés beágyazása](power-bi-embedded-embed-report.md)  
[Új jelentés létrehozása adatkészletből](power-bi-embedded-create-report-from-dataset.md)  
[Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)  
[JavaScript beágyazási minta](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
További kérdései vannak? [Próbálja meg a Power BI-Közösség hello](http://community.powerbi.com/)
