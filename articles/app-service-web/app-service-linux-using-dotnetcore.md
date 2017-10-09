---
title: a Web App Linux .NET Core aaaUse |} Microsoft Docs
description: "Használja a .NET Core Linux Web App alkalmazásban."
keywords: "az Azure app service, webalkalmazás, dotnet, mag, linux, oss"
services: app-service
documentationCenter: 
authors: michimune, rachelappel
manager: erikre
editor: 
ms.assetid: c02959e6-7220-496a-a417-9b2147638e2e
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: aelnably;wesmc;mikono;rachelap
ms.openlocfilehash: 9b7fb7185dff2c99ed88e7937d455177504937b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-net-core-in-an-azure-web-app-on-linux"></a>.NET Core használja az Azure-webalkalmazás Linux rendszeren #

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]

[Webalkalmazás](https://docs.microsoft.com/azure/app-service-web/app-service-linux-intro) Linux biztosít egy jól skálázható, önálló javítási üzemeltetési webszolgáltatás hello Linux operációs rendszert használ. Ez az oktatóanyag bemutató részletes útmutatást tartalmaz hogyan toocreate egy [.NET Core](https://docs.microsoft.com/aspnet/core/) alkalmazást az Azure-webalkalmazásban Linux rendszeren. 

![Webappok Linuxon][10]

A lépésekkel hello Mac, a Windows vagy Linux rendszerű gépek használatának alatt.

## <a name="prerequisites"></a>Előfeltételek ##

toocomplete Ez az oktatóanyag: 

* Telepítse a hello [.NET Core SDK](https://www.microsoft.com/net/download/core).
* Telepítés [Git](https://git-scm.com/downloads).

[!INCLUDE [Free trial note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-local-net-core-application"></a>Helyi .NET Core-alkalmazás létrehozása ##

Egy új terminál-munkamenet elindításához. Hozzon létre egy könyvtárat nevű `hellodotnetcore`, és módosítsa a hello aktuális directory tooit. Írja be a következő hello: 

```
dotnet new web
``` 

  Ezzel a paranccsal létrejön három fájlok (*hellodotnetcore.csproj*, *Program.cs*, és *Startup.cs*) és egy üres mappa (*wwwroot /*) hello aktuális könyvtárban. a tartalom hello `.csproj` fájl hello hasonlóan kell kinéznie: 

```xml
  <!-- Empty lines are omitted. -->

  <Project Sdk="Microsoft.NET.Sdk.Web">
        <PropertyGroup>
        <TargetFramework>netcoreapp1.1</TargetFramework>
        </PropertyGroup>
        <ItemGroup>
        <Folder Include="wwwroot\" />
        </ItemGroup>
        <ItemGroup>
        <PackageReference Include="Microsoft.AspNetCore" Version="1.1.2" />
        </ItemGroup>
  </Project>
```

Mivel ez az alkalmazás egy webes alkalmazás, egy hivatkozás tooan ASP.NET Core csomag volt automatikusan hozzáadott toohello *hellodotnetcore.csproj* fájlt. hello csomag verziószáma hello toohello keretrendszer választása szerint van beállítva. Ebben a példában az ASP.NET Core 1.1.2 verzió hivatkozik, mert .NET Core 1.1 használatos.

## <a name="build-and-test-hello-application-locally"></a>És hello alkalmazás helyi teszteléséhez ##

Létrehozhatja és a .NET Core-alkalmazás futtatása az hello `dotnet restore` parancsot, majd hello `dotnet run` parancsban, ahogy az itt látható:

```
dotnet restore
dotnet run
```


Hello alkalmazás indulásakor hello alkalmazás figyeli-e a port tooincoming kérelmek utaló üzenetet jelenít meg. 

```bash
Hosting environment: Production
Content root path: C:\hellodotnetcore
Now listening on: http://localhost:5000
Application started. Press Ctrl+C tooshut down.
```

Tallózással túl kipróbálja`http://localhost:5000/` a böngészőben. Ha minden remekül működik, megjelenik a "Hello World!" hello eredményt szövegként.

![A böngésző tesztelése][7]

## <a name="create-a-net-core-app-in-hello-azure-portal"></a>A .NET Core-alkalmazás létrehozása az Azure Portal hello ##

Először egy üres webalkalmazás toocreate van szüksége. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/) és hozzon létre egy új [Linux webalkalmazás](https://portal.azure.com/#create/Microsoft.AppSvcLinux).

![A webes alkalmazás létrehozása][1]

Ha hello **létrehozása** lap megnyitásakor, a webalkalmazás részleteit tartalmazzák:

![A .NET Core runtime verem kiválasztása][2]

Használjon hello következő táblázatban egy útmutató toofill hello ki, **létrehozása** lapon, majd válassza ki **OK** és **létrehozása** toocreate hello alkalmazás.

| Beállítás      | Ajánlott érték  | Leírás                                        |
| ------------ | ---------------- | -------------------------------------------------- |
| Alkalmazás neve | hellodotnetcore  | az alkalmazás hello neve. Ez a név nem egyedi. |
| Előfizetés | Válasszon egy meglévő előfizetéshez | hello Azure-előfizetés. |
| Erőforráscsoport | myResourceGroup |  hello Azure-erőforrás csoport toocontain hello webalkalmazás. |
| App Service-csomag | Meglévő App Service-csomag neve |  hello App Service-csomag.  |
| Tároló konfigurálása | A .NET core 1.1 | a webalkalmazás tároló típusú hello: beépített, Docker vagy titkos beállításjegyzék. |
| Képek forrása  | Beépített  |  hello kép hello forrását. |
| Futásidejű verem  | A .NET core 1.1  | hello futásidejű verem és verziót.  |

## <a name="deploy-your-application-via-git"></a>A Git segítségével alkalmazás központi telepítése ##

Git toodeploy hello .NET Core alkalmazás tooAzure App Service Web App használata Linux rendszeren.

hello új Azure-webalkalmazás már konfigurált Git-telepítés. Navigáljon a következő URL-címet a webalkalmazás nevének behelyezése után toohello található hello Git telepítési URL-cím:

```https://{your web app name}.scm.azurewebsites.net/api/scm/info```

hello Git URL-cím van a webes alkalmazás neve alapján a következő hello:

```https://{your web app name}.scm.azurewebsites.net/{your web app name}.git```

Futtassa a következő parancsok toodeploy hello helyi alkalmazás tooyour Azure-webalkalmazásban hello: 
 
```bash
git init
git remote add azure <Git deployment URL from above>
git add *.csproj *.cs
git commit -m "Initial deployment commit"
git push azure master
```

Nem kell toopush található összes fájl *bin /* vagy *obj /* könyvtárak, mert az alkalmazás hello felhő van építve amikor hello alkalmazás forrásfájljait tároló tooAzure elküldte azokat. Hello felépítési folyamat befejezése után bináris fájlokat másol hello alkalmazás könyvtár: */otthoni/hely/wwwroot/*.

Győződjön meg arról, hogy a hello távoli üzembe helyezési műveleteket sikeres jelentést. Leküldéses műveletek előfordulhat, hogy óta csomag feloldási időigényes és hello felhőben futtatható folyamat létrehozása. Több állapotüzenetek, beleértve azokat, amely meghatározza, hogy a fájl másolása jelenik meg. hello kimeneti alábbihoz hasonló toohello következő:

```bash
/* some output has been removed for brevity */
remote: Copying file: 'System.Net.Websockets.dll' 
remote: Copying file: 'System.Runtime.CompilerServices.Unsafe.dll' 
remote: Copying file: 'System.Runtime.Serialization.Primitives.dll' 
remote: Copying file: 'System.Text.Encodings.Web.dll' 
remote: Copying file: 'hellodotnetcore.deps.json' 
remote: Copying file: 'hellodotnetcore.dll' 
remote: Omitting next output lines...
remote: Finished successfully.
remote: Running post deployment commands...
remote: Deployment successful.
toohttps://hellodotnetcore.scm.azurewebsites.net/
 * [new branch]           master -> master

```

Hello központi telepítés befejezése után indítsa újra a webalkalmazása hello telepítési tootake hatást. toodo, nyissa meg az Azure portál toohello, és keresse meg a toohello **áttekintése** a webes alkalmazás lapján. Jelölje be hello **indítsa újra a** hello lap gombjára. Amikor megjelenik egy előugró ablak, válassza ki a **Igen** tooconfirm. A webalkalmazás, majd keresse meg, ahogy az itt látható:

![Keresse meg a .NET Core alkalmazás telepítve tooAzure Linux App Service][10]

[!INCLUDE [Clean-up section](../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a>Következő lépések
* [Az Azure App Service webalkalmazásba Linux – gyakori kérdések](./app-service-linux-faq.md)

[1]: ./media/app-service-linux-using-dotnetcore/top-level-create.png
[2]: ./media/app-service-linux-using-dotnetcore/dotnet-new-webapp.png
[7]: ./media/app-service-linux-using-dotnetcore/dotnet-browse-local.png
[10]: ./media/app-service-linux-using-dotnetcore/dotnet-browse-azure.png
