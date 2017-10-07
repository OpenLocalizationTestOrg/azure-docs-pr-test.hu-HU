---
title: "Azure SQL Database és az ASP.NET alkalmazás aaaBuild |} Microsoft Docs"
description: "Megtudhatja, hogyan tooget egy ASP.NET alkalmazás használata az Azure SQL-adatbázis kapcsolati tooa."
services: app-service\web
documentationcenter: nodejs
author: cephalin
manager: erikre
editor: 
ms.assetid: 03c584f1-a93c-4e3d-ac1b-c82b50c75d3e
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: csharp
ms.topic: tutorial
ms.date: 06/09/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: d21c2bc404bfe038608c17e5a94d96847153002c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="build-an-aspnet-app-in-azure-with-sql-database"></a>Azure SQL Database és az ASP.NET alkalmazás létrehozása

Az [Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) egy hatékonyan méretezhető, önjavító webes üzemeltetési szolgáltatás. Az oktatóanyag bemutatja, hogyan toodeploy adatvezérelt ASP.NET webalkalmazás az Azure-ban, és csatlakoztassa túl[Azure SQL Database](../sql-database/sql-database-technical-overview.md). Amikor végzett, hogy egy ASP.NET-alkalmazás fusson az [Azure App Service](../app-service/app-service-value-prop-what-is.md) és tooSQL adatbázis csatlakoztatva.

![Közzétett ASP.NET-alkalmazást az Azure-webalkalmazásban](./media/app-service-web-tutorial-dotnet-sqldatabase/azure-app-in-browser.png)

Eben az oktatóanyagban az alábbiakkal fog megismerkedni:

> [!div class="checklist"]
> * SQL-adatbázis létrehozása az Azure-ban
> * Csatlakozás egy ASP.NET alkalmazás tooSQL adatbázis
> * Hello app tooAzure telepítése
> * Hello adatmodell frissítése, és telepítse újra hello alkalmazást
> * Stream naplók az Azure tooyour terminál
> * Az Azure-portálon hello hello alkalmazás kezelése

## <a name="prerequisites"></a>Előfeltételek

toocomplete Ez az oktatóanyag:

* Telepítés [Visual Studio 2017](https://www.visualstudio.com/downloads/) az alábbi munkaterhelések hello:
  - **ASP.NET és webfejlesztés**
  - **Azure-fejlesztés**

  ![ASP.NET és webfejlesztés és Azure-fejlesztés (Web és felhőszolgáltatások alatt)](media/app-service-web-tutorial-dotnet-sqldatabase/workloads.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="download-hello-sample"></a>Hello minta letöltése

[Töltse le a hello mintaprojektet](https://github.com/Azure-Samples/dotnet-sqldb-tutorial/archive/master.zip).

Extract (csomagolja ki) hello *dotnet-sqldb-oktatóanyag – master.zip* fájlt.

minta projektben egy alapszintű hello [ASP.NET MVC](https://www.asp.net/mvc) CRUD (hozzon létre-olvasás-Módosítás-Törlés) alkalmazás használatával [Entity Framework Code First](/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application).

### <a name="run-hello-app"></a>Hello alkalmazás futtatása

Nyissa meg hello *dotnet-sqldb-oktatóanyag – master/DotNetAppSqlDb.sln* fájlt a Visual Studióban. 

Típus `Ctrl+F5` toorun hello app hibakeresés nélkül. hello alkalmazás az alapértelmezett böngésző jelenik meg. Jelölje be hello **hozzon létre új** hivatkozásra, és hozzon létre több *tennivaló* elemeket. 

![A New ASP.NET Project (Új ASP.NET-projekt) párbeszédpanel](media/app-service-web-tutorial-dotnet-sqldatabase/local-app-in-browser.png)

Teszt hello **szerkesztése**, **részletek**, és **törlése** hivatkozásokat.

hello app hello adatbázis egy adatbázis-környezet tooconnect használ. Ez a példa hello adatbázis-környezet nevű kapcsolati karakterláncot használ `MyDbConnection`. hello kapcsolati karakterlánc megadása hello *Web.config* fájl- és a hivatkozott hello *Models/MyDatabaseContext.cs* fájlt. hello kapcsolati karakterlánc nevét később hello oktatóanyag tooconnect hello Azure web app tooan Azure SQL Database használatos. 

## <a name="publish-tooazure-with-sql-database"></a>Az SQL Database tooAzure közzététele

A hello **Megoldáskezelőben**, kattintson a jobb gombbal a **DotNetAppSqlDb** projektre, és válassza ki **közzététel**.

![Közzététel a Megoldáskezelőből](./media/app-service-web-tutorial-dotnet-sqldatabase/solution-explorer-publish.png)

Győződjön meg arról, hogy a **Microsoft Azure App Service** van kiválasztva, és kattintson a **Publish** (Közzététel) elemre.

![Közzététel a projekt áttekintő oldaláról](./media/app-service-web-tutorial-dotnet-sqldatabase/publish-to-app-service.png)

A közzététel megnyitja hello **létrehozása az App Service** párbeszédpanel, amely segít hoz létre minden hello Azure-erőforrások toorun az ASP.NET webalkalmazás az Azure-ban van szüksége.

### <a name="sign-in-tooazure"></a>Jelentkezzen be tooAzure

A hello **létrehozása az App Service** párbeszédpanel, kattintson a **vegyen fel egy fiókot**, majd jelentkezzen be Azure-előfizetés tooyour. Ha már be van jelentkezve egy Microsoft-fiókba, győződjön meg arról, hogy abban a fiókban található az előfizetése. Ha hello bejelentkezett Microsoft-fiók nem rendelkezik Azure-előfizetése, kattintson rá tooadd hello megfelelő fiókot.
   
![Jelentkezzen be tooAzure](./media/app-service-web-tutorial-dotnet-sqldatabase/sign-in-azure.png)

Miután bejelentkezett, most készen áll a toocreate összes hello ezen a párbeszédpanelen az Azure webalkalmazás számára szükséges erőforrások.

### <a name="configure-hello-web-app-name"></a>Hello webalkalmazás nevének konfigurálása

Megtartja-hello létrehozott webes alkalmazás neve, vagy módosítsa azt tooanother egyedi nevét (érvényes karakterek: `a-z`, `0-9`, és `-`). az alkalmazás hello alapértelmezett URL-cím részeként használatos hello webes alkalmazás neve (`<app_name>.azurewebsites.net`, ahol `<app_name>` a webes alkalmazás neve). hello webalkalmazásnév kell toobe egyedi az Azure-ban minden alkalmazások között. 

![Hozzon létre az app service párbeszédpanelen](media/app-service-web-tutorial-dotnet-sqldatabase/wan.png)

### <a name="create-a-resource-group"></a>Hozzon létre egy erőforráscsoportot

[!INCLUDE [resource-group](../../includes/resource-group.md)]

Következő túl**erőforráscsoport**, kattintson a **új**.

![Következő tooResource csoport, az új gombra.](media/app-service-web-tutorial-dotnet-sqldatabase/new_rg2.png)

Hello erőforráscsoport neve **myResourceGroup**.

> [!NOTE]
> Ne kattintson **létrehozása**. Először egy SQL-adatbázis egy későbbi lépésben tooset.

### <a name="create-an-app-service-plan"></a>App Service-csomag létrehozása

[!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

Következő túl**App Service-csomag**, kattintson a **új**. 

A hello **App Service-csomag konfigurálása** párbeszédpanelen a következő beállítások hello hello új App Service-csomag konfigurálása:

![App Service-csomag létrehozása](./media/app-service-web-tutorial-dotnet-sqldatabase/configure-app-service-plan.png)

| Beállítás  | Ajánlott érték | További információ |
| ----------------- | ------------ | ----|
|**App Service-csomag**| myAppServicePlan | [App Service-csomagok](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) |
|**Hely**| Nyugat-Európa | [Azure-régiók](https://azure.microsoft.com/regions/) |
|**Méret**| Ingyenes | [Tarifacsomagok](https://azure.microsoft.com/pricing/details/app-service/)|

### <a name="create-a-sql-server-instance"></a>SQL Server-példány létrehozása

Adatbázis létrehozása előtt kell egy [Azure SQL Database logikai kiszolgáló](../sql-database/sql-database-features.md). A logikai kiszolgálók adatbázisok egy csoportját tartalmazzák, amelyeket a rendszer egy csoportként kezel.

Válassza ki **további Azure-szolgáltatások felfedezés**.

![A webapp nevének konfigurálása](media/app-service-web-tutorial-dotnet-sqldatabase/web-app-name.png)

A hello **szolgáltatások** lapra, majd hello  **+**  ikon következő túl**SQL-adatbázis**. 

![A hello szolgáltatások lapon kattintson a hello + ikon következő tooSQL adatbázis.](media/app-service-web-tutorial-dotnet-sqldatabase/sql.png)

A hello **SQL-adatbázis beállítása** párbeszédpanel, kattintson a **új** következő túl**SQL Server**. 

Létrejön egy egyedi kiszolgálónevet. Ez a név részeként hello alapértelmezett URL-cím a logikai kiszolgálóhoz használt `<server_name>.database.windows.net`. Egyedinek kell lennie az Azure-ban minden logikai kiszolgáló példányára. Módosítsa hello kiszolgáló nevét, de ebben az oktatóanyagban tartsa meg a létrehozott hello értéket.

Adja hozzá egy rendszergazdai jogosultságú felhasználónevet és jelszót, majd válassza ki **OK**. Összetettségi követelményeknek, lásd: [jelszóházirend](/sql/relational-databases/security/password-policy).

Ne felejtse el ezt a felhasználónevet és jelszót. Szüksége van rá toomanage hello logikai kiszolgáló újabb példányt.

![SQL Server-példány létrehozása](media/app-service-web-tutorial-dotnet-sqldatabase/configure-sql-database-server.png)

### <a name="create-a-sql-database"></a>SQL-adatbázis létrehozása

A hello **SQL-adatbázis beállítása** párbeszédpanel: 

* Tartsa hello alapértelmezés szerint generált **adatbázisnév**.
* A **kapcsolati karakterlánc nevét**, típus *MyDbConnection*. Ezt a nevet meg kell egyeznie a hivatkozott hello kapcsolati karakterlánc *Models/MyDatabaseContext.cs*.
* Kattintson az **OK** gombra.

![SQL-adatbázis konfigurálása](media/app-service-web-tutorial-dotnet-sqldatabase/configure-sql-database.png)

Hello **létrehozása az App Service** párbeszédpanelen látható hello erőforrások létrehozott. Kattintson a **Create** (Létrehozás) gombra. 

![létrehozott hello erőforrások](media/app-service-web-tutorial-dotnet-sqldatabase/app_svc_plan_done.png)

Miután hello varázsló végzett a létrehozással hello Azure-erőforrások, az ASP.NET alkalmazás tooAzure tesz közzé. Az alapértelmezett böngésző hello URL-cím toohello telepített alkalmazáshoz nincs elindítva. 

Adjon hozzá néhány Tennivalólista elemein.

![Közzétett ASP.NET-alkalmazást az Azure-webalkalmazásban](./media/app-service-web-tutorial-dotnet-sqldatabase/azure-app-in-browser.png)

Gratulálunk! Az adatvezérelt ASP.NET-alkalmazás fut élő az Azure App Service-ben.

## <a name="access-hello-sql-database-locally"></a>SQL-adatbázis Access-hello helyileg

A Visual Studio lehetővé teszi, hogy vizsgálatát, és az új SQL-adatbázis egyszerűen a hello kezelése **SQL Server Object Explorer**.

### <a name="create-a-database-connection"></a>Adatbázis-kapcsolat létrehozása

A hello **nézet** menü **SQL Server Object Explorer**.

Hello felső **SQL Server Object Explorer**, kattintson a hello **SQL-kiszolgáló hozzáadása** gombra.

### <a name="configure-hello-database-connection"></a>Hello adatbázis-kapcsolat konfigurálása

A hello **Connect** párbeszédpanelen bontsa ki a hello **Azure** csomópont. Az SQL-adatbázis példányainak az Azure-ban az itt felsorolt.

Jelölje be hello `DotNetAppSqlDb` SQL-adatbázis. a korábban létrehozott hello kapcsolatot a rendszer automatikusan kitölti hello lap alján.

Írja be a korábban létrehozott hello adatbázis rendszergazdai jelszót, és kattintson a **Connect**.

![A Visual Studio adatbázis-kapcsolat konfigurálása](./media/app-service-web-tutorial-dotnet-sqldatabase/connect-to-sql-database.png)

### <a name="allow-client-connection-from-your-computer"></a>A számítógép az ügyfél-kapcsolat engedélyezése

Hello **hozzon létre egy új tűzfalszabályt** párbeszédpanel már meg van nyitva. Alapértelmezés szerint az SQL Database-példányt csak Azure-szolgáltatások, például az Azure-webalkalmazásban kapcsolatokat engedélyez. tooconnect tooyour adatbázisra, és hozzon létre egy tűzfalszabályt hello SQL-adatbázispéldány. hello tűzfalszabály lehetővé teszi, hogy a helyi számítógép hello nyilvános IP-címét.

hello párbeszédpanel már ki van töltve, a számítógép nyilvános IP-címmel.

Győződjön meg arról, hogy **a ügyfél IP-cím hozzáadása** van kiválasztva, és kattintson a **OK**. 

![SQL Database-példányt a tűzfal beállítása](./media/app-service-web-tutorial-dotnet-sqldatabase/sql-set-firewall.png)

Visual Studio végzett a létrehozással hello tűzfal beállítása a SQL Database-példányt, ha a kapcsolat megjelennek **SQL Server Object Explorer**.

Itt végezheti el hello leggyakoribb adatbázis műveletek, például a lekérdezések futtatása hozhat létre, nézetek és tárolt eljárások, és több. 

Kattintson a jobb gombbal a hello `Todoes` tábla, és válassza ki **adatok megtekintéséhez**. 

![Fedezze fel az SQL adatbázis-objektumok](./media/app-service-web-tutorial-dotnet-sqldatabase/explore-sql-database.png)

## <a name="update-app-with-code-first-migrations"></a>Frissítse app Code First áttelepítést

Használhatja a Visual Studio tooupdate hello a jól ismert eszközökkel az adatbázis és a webes alkalmazás az Azure-ban. Ebben a lépésben az Entity Framework toomake módosítás tooyour adatbázisséma Code First áttelepítést használni, és tegye közzé azt tooAzure.

Entity Framework Code First áttelepítést használatával kapcsolatos további információkért lásd: [Ismerkedés az Entity Framework 6 Code First MVC 5-öt használó](https://docs.microsoft.com/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application).

### <a name="update-your-data-model"></a>Az adatmodell frissítése

Nyissa meg _Models\Todo.cs_ hello kód szerkesztőben. Adja hozzá a következő tulajdonság toohello hello `ToDo` osztály:

```csharp
public bool Done { get; set; }
```

### <a name="run-code-first-migrations-locally"></a>Code First áttelepítést helyileg történő futtatása

Néhány parancsok toomake frissítések tooyour helyi adatbázis futtatásához. 

A hello **eszközök** menüben kattintson a **NuGet-Csomagkezelő** > **Csomagkezelő konzol**.

A Package Manager Console ablakban hello engedélyezze a Code First áttelepítést:

```PowerShell
Enable-Migrations
```

Vegye fel az áttelepítés:

```PowerShell
Add-Migration AddProperty
```

Hello helyi adatbázis frissítése:

```PowerShell
Update-Database
```

Típus `Ctrl+F5` toorun hello alkalmazást. Teszt hello adatok szerkesztéséhez, és hivatkozások létrehozása.

Hello alkalmazás betölt nem jelenik meg hibaüzenet, ha Code First áttelepítést sikeresen befejeződött. Azonban a lap továbbra is keres hello ugyanaz, mert az alkalmazás logikáját még nem használja ezt az új tulajdonságot. 

### <a name="use-hello-new-property"></a>Hello új tulajdonsággal

Módosítja a kód toouse hello `Done` tulajdonság. Az egyszerűség kedvéért ebben az oktatóanyagban, csak fog toochange hello `Index` és `Create` toosee hello tulajdonság megtekinti a művelet.

Nyissa meg _Controllers\TodosController.cs_.

Hello található `Create()` metódust, és adja hozzá `Done` hello tulajdonságok listája toohello `Bind` attribútum. Amikor elkészült, a `Create()` metódus-aláírás néz hello a következő kódot:

```csharp
public ActionResult Create([Bind(Include = "id,Description,CreatedDate,Done")] Todo todo)
```

Nyissa meg _Views\Todos\Create.cshtml_.

A hello Razor kódot, megjelenik egy `<div class="form-group">` elem által használt `model.Description`, és majd egy másik `<div class="form-group">` elem által használt `model.CreatedDate`. Ez a két elem követő hozzáadása egy másik `<div class="form-group">` elem által használt `model.Done`:

```csharp
<div class="form-group">
    @Html.LabelFor(model => model.Done, htmlAttributes: new { @class = "control-label col-md-2" })
    <div class="col-md-10">
        <div class="checkbox">
            @Html.EditorFor(model => model.Done)
            @Html.ValidationMessageFor(model => model.Done, "", new { @class = "text-danger" })
        </div>
    </div>
</div>
```

Nyissa meg _Views\Todos\Index.cshtml_.

Keresse meg az üres hello `<th></th>` elemet. Ez az elem felett Razor kód a következő hello hozzáadása:

```csharp
<th>
    @Html.DisplayNameFor(model => model.Done)
</th>
```

Hello található `<td>` elem, amely tartalmazza a hello `Html.ActionLink()` segédmódszereket. Ez az elem felett Razor kód a következő hello hozzáadása:

```csharp
<td>
    @Html.DisplayFor(modelItem => item.Done)
</td>
```

Ez minden toosee hello változásai hello kell `Index` és `Create` nézetek. 

Típus `Ctrl+F5` toorun hello alkalmazást.

Ezután egy teendő hozzáadása és ellenőrzése **végzett**. Ezután azt kell jelennek meg az Ön újragondolt befejezett elemként. Ne feledje, hogy hello `Edit` nézet nem jeleníti meg hello `Done` mezőben, már nem módosíthatók hello `Edit` nézet.

### <a name="enable-code-first-migrations-in-azure"></a>Engedélyezze a Code First áttelepítést az Azure-ban

Most, hogy a kód módosítása működéséről, beleértve az adatbázis az áttelepítés akkor közzéteszi az Azure-webalkalmazásban tooyour, és frissítse az SQL-adatbázis Code First áttelepítést túl.

Fentiekhez hasonló, kattintson jobb gombbal a projektre, és válassza ki **közzététel**.

Kattintson a **beállítások** tooopen hello közzététele varázsló.

![Nyissa meg a közzétételi beállítások](./media/app-service-web-tutorial-dotnet-sqldatabase/publish-settings.png)

Hello varázslóban kattintson **következő**.

Ellenőrizze, hogy hello kapcsolati karakterlánc az SQL-adatbázis nem üres **MyDatabaseContext (MyDbConnection)**. Előfordulhat, hogy tooselect hello **myToDoAppDb** adatbázis hello legördülő menüből. 

Válassza ki **hajtható végre Code First áttelepítést (fut az alkalmazás indítása)**, majd kattintson a **mentése**.

![Az Azure web app alkalmazásban Code First áttelepítést engedélyezése](./media/app-service-web-tutorial-dotnet-sqldatabase/enable-migrations.png)

### <a name="publish-your-changes"></a>A változtatások közzététele

Most, hogy Code First áttelepítést az Azure web app alkalmazásban engedélyezve van, a kód változtatások közzététele.

Hello közzéteszi a lapot, majd kattintson **közzététel**.

Próbálja meg újból hozzáadni a Tennivalólista elemein, és válassza ki **végzett**, és Ön újragondolt befejezett elemként jelenniük.

![Azure-webalkalmazásban kód első áttelepítés után](./media/app-service-web-tutorial-dotnet-sqldatabase/this-one-is-done.png)

A meglévő Tennivalólista elemein továbbra is megjelennek. Ha az ASP.NET-alkalmazás közzétételéhez meglévő az SQL-adatbázis adatai nem elveszett. Emellett Code First áttelepítést csak akkor változik meg hello adatséma és a meglévő adatok sértetlenek.


## <a name="stream-application-logs"></a>Az adatfolyam alkalmazásnaplók

Az Azure web app tooVisual Studio közvetlenül a nyomkövetési üzeneteket is adatfolyam.

Nyissa meg _Controllers\TodosController.cs_.

Minden egyes művelethez kezdődik-e a `Trace.WriteLine()` metódust. Ezzel a kóddal egészül ki tooshow, hogyan tooadd nyomkövetési üzenetek tooyour Azure-webalkalmazásban.

### <a name="open-server-explorer"></a>Nyissa meg a Server Explorer

A hello **nézet** menü **Server Explorer**. Naplózás konfigurálása az Azure-webalkalmazásban a **Server Explorer**. 

### <a name="enable-log-streaming"></a>Adatfolyamként-napló

A **Server Explorer**, bontsa ki a **Azure** > **App Service**.

Bontsa ki a hello **myResourceGroup** erőforráscsoportot, a létrehozott Azure-webalkalmazásban hello kezdeti létrehozásakor.

Kattintson a jobb gombbal az Azure-webalkalmazásban, és válassza ki **folyamatos átviteli naplók megtekintése**.

![Adatfolyamként-napló](./media/app-service-web-tutorial-dotnet-sqldatabase/stream-logs.png)

hello naplók mostantól átvitt be hello **kimeneti** ablak. 

![A kimeneti ablakban adatfolyam-napló](./media/app-service-web-tutorial-dotnet-sqldatabase/log-streaming-pane.png)

Azonban nem jelennek meg a nyomkövetési köszönőüzenetei még. Meg mert először kiválasztásakor **folyamatos átviteli naplók megtekintése**, az Azure-webalkalmazásban túl hello nyomkövetési szint beállítása`Error`, amely csak naplózza hibaesemények (a hello `Trace.TraceError()` metódus).

### <a name="change-trace-levels"></a>Nyomkövetési szintek módosítása

toochange hello nyomkövetési szintek toooutput többi nyomkövetési üzenet, lépjen vissza túl**Server Explorer**.

Az Azure-webalkalmazás ismét gombbal és válassza ki **beállítások**.

A hello **Alkalmazásnaplózást (fájlrendszer)** legördülő menüből válassza **részletes**. Kattintson a **Save** (Mentés) gombra.

![A nyomkövetési szint tooVerbose módosítása](./media/app-service-web-tutorial-dotnet-sqldatabase/trace-level-verbose.png)

> [!TIP]
> Kísérletezhet különböző nyomkövetési szintek toosee egyes szinteken milyen típusú üzenetek jelennek meg. Például hello **információk** szint továbbít minden által létrehozott naplók `Trace.TraceInformation()`, `Trace.TraceWarning()`, és `Trace.TraceError()`, de nem által létrehozott naplók `Trace.WriteLine()`.
>
>

A böngészőben kattintson körül hello tennivalók listája alkalmazás az Azure-ban próbálja. nyomkövetési köszönőüzenetei most átvitt toohello **kimeneti** ablak a Visual Studióban.

```
Application: 2017-04-06T23:30:41  PID[8132] Verbose     GET /Todos/Index
Application: 2017-04-06T23:30:43  PID[8132] Verbose     GET /Todos/Create
Application: 2017-04-06T23:30:53  PID[8132] Verbose     POST /Todos/Create
Application: 2017-04-06T23:30:54  PID[8132] Verbose     GET /Todos/Index
```



### <a name="stop-log-streaming"></a>Állítsa le az adatfolyam-napló

toostop hello napló-adatfolyam-szolgáltatás, kattintson a hello **figyelés leállításának** hello gombjára **kimeneti** ablak.

![Állítsa le az adatfolyam-napló](./media/app-service-web-tutorial-dotnet-sqldatabase/stop-streaming.png)

## <a name="manage-your-azure-web-app"></a>Az Azure-webalkalmazásban kezelése

Nyissa meg toohello [Azure-portálon](https://portal.azure.com) toosee hello létrehozott webalkalmazás. 



Hello bal oldali menüben kattintson **App Service**, majd kattintson az Azure-webalkalmazásban hello nevére.

![Portálnavigációjával tooAzure webalkalmazás](./media/app-service-web-tutorial-dotnet-sqldatabase/access-portal.png)

A webalkalmazás lapon rendelkezik ki. 

Alapértelmezés szerint hello portal mutatja hello **áttekintése** lap. Ezen az oldalon megtekintheti az alkalmazás állapotát. Itt elvégezhet olyan alapszintű felügyeleti feladatokat is, mint a böngészés, leállítás, elindítás, újraindítás és törlés. hello lapok hello bal oldalán található hello hello különböző konfigurációs lapok megnyithatja megjelenítése. 

![Az App Service lap az Azure Portalon](./media/app-service-web-tutorial-dotnet-sqldatabase/web-app-blade.png)

[!INCLUDE [Clean up section](../../includes/clean-up-section-portal-web-app.md)]

<a name="next"></a>

## <a name="next-steps"></a>Következő lépések

Ez az oktatóanyag bemutatta, hogyan végezheti el az alábbi műveleteket:

> [!div class="checklist"]
> * SQL-adatbázis létrehozása az Azure-ban
> * Csatlakozás egy ASP.NET alkalmazás tooSQL adatbázis
> * Hello app tooAzure telepítése
> * Hello adatmodell frissítése, és telepítse újra hello alkalmazást
> * Stream naplók az Azure tooyour terminál
> * Az Azure-portálon hello hello alkalmazás kezelése

Előzetes toohello következő útmutató toolearn hogyan toomap egy egyéni DNS name toohello webalkalmazás.

> [!div class="nextstepaction"]
> [Egy meglévő egyéni DNS nevét tooAzure webalkalmazások leképezése](app-service-web-tutorial-custom-domain.md)
