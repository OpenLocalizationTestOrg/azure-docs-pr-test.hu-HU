---
title: "aaaCreate az ASP.NET webalkalmazás az Azure-ban |} Microsoft Docs"
description: "Ismerje meg, hogyan toorun webalkalmazásokat az Azure App Service telepítésével hello alapértelmezett ASP.NET webalkalmazás."
services: app-service\web
documentationcenter: 
author: cephalin
manager: wpickett
editor: 
ms.assetid: b1e6bd58-48d1-4007-9d6c-53fd6db061e3
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 06/14/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: eec916b3c32b6c8b68083177938c5c822a9782b8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-aspnet-web-app-in-azure"></a>ASP.NET-webalkalmazás létrehozása

Az [Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) egy hatékonyan méretezhető, önjavító webes üzemeltetési szolgáltatás.  A gyors üzembe helyezés bemutatja, hogyan toodeploy az első ASP.NET web app tooAzure webalkalmazások. Az oktatóanyag végére egy olyan erőforráscsoport lesz elérhető, amely egy App Service-csomagból és egy üzembe helyezett webalkalmazással rendelkező Azure webalkalmazásból áll.

Bemutató videó toosee hello a gyors üzembe helyezés, a művelet, és hajtsa végre hello lépések saját kezűleg toopublish az első .NET-alkalmazás az Azure-on.

> [!VIDEO https://channel9.msdn.com/Shows/Azure-for-NET-Developers/Create-a-NET-app-in-Azure-Quickstart/player]

## <a name="prerequisites"></a>Előfeltételek

toocomplete Ez az oktatóanyag:

* Telepítés [Visual Studio 2017](https://www.visualstudio.com/downloads/) az alábbi munkaterhelések hello:
    - **ASP.NET és webfejlesztés**
    - **Azure-fejlesztés**

    ![ASP.NET és webfejlesztés és Azure-fejlesztés (Web és felhőszolgáltatások alatt)](media/app-service-web-tutorial-dotnet-sqldatabase/workloads.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-an-aspnet-web-app"></a>ASP.NET-webapp létrehozása

Hozzon létre egy projektet a Visual Studióban a **File > New > Project** (Fájl > Új > Projekt) lehetőség kiválasztásával. 

A hello **új projekt** párbeszédablakban válassza **Visual C# > Web > ASP.NET Web Application (.NET-keretrendszer)**.

Hello alkalmazás neve _myFirstAzureWebApp_, majd válassza ki **OK**.
   
![A New Project (Új projekt) párbeszédpanel](./media/app-service-web-get-started-dotnet/new-project.png)

Az ASP.NET web app tooAzure bármilyen típusú telepítése. A gyors üzembe helyezés, válassza a hello **MVC** sablont, és ellenőrizze, hogy a hitelesítés beállítása túl**nem hitelesítési**.
      
Kattintson az **OK** gombra.

![A New ASP.NET Project (Új ASP.NET-projekt) párbeszédpanel](./media/app-service-web-get-started-dotnet/select-mvc-template.png)

Hello menüben válassza ki **hibakeresés > hibakeresés nélkül** toorun hello webalkalmazást helyileg.

![Az alkalmazás futtatása helyileg](./media/app-service-web-get-started-dotnet/local-web-app.png)

## <a name="publish-tooazure"></a>TooAzure közzététele

A hello **Megoldáskezelőben**, kattintson a jobb gombbal hello **myFirstAzureWebApp** projektre, és válassza ki **közzététel**.

![Közzététel a Megoldáskezelőből](./media/app-service-web-get-started-dotnet/solution-explorer-publish.png)

Győződjön meg arról, hogy a **Microsoft Azure App Service** van kiválasztva, és kattintson a **Publish** (Közzététel) elemre.

![Közzététel a projekt áttekintő oldaláról](./media/app-service-web-get-started-dotnet/publish-to-app-service.png)

Ekkor megnyílik a hello **létrehozása az App Service** párbeszédpanel, amely segítséget nyújt az összes hello szükséges Azure-erőforrások toorun hello ASP.NET-webalkalmazás létrehozása az Azure-ban.

## <a name="sign-in-tooazure"></a>Jelentkezzen be tooAzure

A hello **létrehozása az App Service** párbeszédablakban válassza **vegyen fel egy fiókot**, és jelentkezzen be Azure-előfizetés tooyour. Ha már bejelentkezett ezen, hello tartalmazó select hello fiók kívánt előfizetést hello legördülő menüből.

> [!NOTE]
> Ha már be van jelentkezve, akkor még ne válassza a **Create** (Létrehozás) lehetőséget.
>
>
   
![Jelentkezzen be tooAzure](./media/app-service-web-get-started-dotnet/sign-in-azure.png)

## <a name="create-a-resource-group"></a>Hozzon létre egy erőforráscsoportot

[!INCLUDE [resource group intro text](../../includes/resource-group.md)]

Következő túl**erőforráscsoport**, jelölje be **új**.

Hello erőforráscsoport neve **myResourceGroup** válassza **OK**.

## <a name="create-an-app-service-plan"></a>App Service-csomag létrehozása

[!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

Következő túl**App Service-csomag**, jelölje be **új**. 

A hello **konfigurálása App Service-csomag** párbeszédpanelen hello tábla a következő hello képernyőfelvétel a hello beállítások használata.

![App Service-csomag létrehozása](./media/app-service-web-get-started-dotnet/configure-app-service-plan.png)

| Beállítás | Ajánlott érték | Leírás |
|-|-|-|
|App Service-csomag| myAppServicePlan | Hello App Service-csomag neve. |
| Hely | Nyugat-Európa | hello adatközpont, amelyen hello webalkalmazás található. |
| Méret | Ingyenes | A [tarifacsomag](https://azure.microsoft.com/pricing/details/app-service/) meghatározza az üzemeltetési funkciókat. |

Kattintson az **OK** gombra.

## <a name="create-and-publish-hello-web-app"></a>Hozzon létre és hello webes alkalmazás közzététele

A **webalkalmazásnév**, írja be egy egyedi alkalmazásnévvel (érvényes karakterek: `a-z`, `0-9`, és `-`), vagy fogadja el a hello automatikusan hozott létre egyedi neve. hello webalkalmazás URL-címe hello `http://<app_name>.azurewebsites.net`, ahol `<app_name>` a webes alkalmazás neve.

Válassza ki **létrehozása** toostart hello Azure-erőforrások létrehozása.

![A webapp nevének konfigurálása](./media/app-service-web-get-started-dotnet/web-app-name.png)

Hello varázsló befejezése után hello ASP.NET web app tooAzure teszi közzé, és ezután elindítja hello app hello alapértelmezett böngészőben.

![Közzétett ASP.NET-webapp az Azure-ban](./media/app-service-web-get-started-dotnet/published-azure-web-app.png)

webalkalmazás neve hello hello megadott [létrehozása és közzététele lépés](#create-and-publish-the-web-app) használatos az URL-előtagját: hello formátumban hello `http://<app_name>.azurewebsites.net`.

Gratulálunk, az ASP.NET-webalkalmazás immáron elérhető az Azure App Service-ben.

## <a name="update-hello-app-and-redeploy"></a>Frissítés hello alkalmazást, és helyezze üzembe újra

A hello **Megoldáskezelőben**, nyissa meg _Views\Home\Index.cshtml_.

Hello található `<div class="jumbotron">` HTML-címke közelében hello felső, és hello teljes elemet cserélje le a következő kód hello:

```HTML
<div class="jumbotron">
    <h1>ASP.NET in Azure!</h1>
    <p class="lead">This is a simple app that we’ve built that demonstrates how toodeploy a .NET app tooAzure App Service.</p>
</div>
```

tooredeploy tooAzure, kattintson a jobb gombbal hello **myFirstAzureWebApp** a projekt **Megoldáskezelőben** válassza **közzététel**.

A hello közzéteszi a lapot, jelölje be **közzététel**.

Közzététel befejezését követően a Visual Studio elindítja egy böngésző toohello webalkalmazás URL-címe hello.

![Frissített ASP.NET-webapp az Azure-ban](./media/app-service-web-get-started-dotnet/updated-azure-web-app.png)

## <a name="manage-hello-azure-web-app"></a>Hello Azure web apphoz kezelése

Nyissa meg toohello <a href="https://portal.azure.com" target="_blank">Azure-portálon</a> toomanage hello webalkalmazás.

Hello bal oldali menüben válassza ki a **alkalmazásszolgáltatások**, majd válassza ki az Azure-webalkalmazásban hello nevét.

![Portálnavigációjával tooAzure webalkalmazás](./media/app-service-web-get-started-dotnet/access-portal.png)

Megtekintheti a webalkalmazás Áttekintés oldalát. Itt elvégezhet olyan alapszintű felügyeleti feladatokat, mint a tallózás, leállítás, elindítás, újraindítás és törlés. 

![Az App Service panel az Azure Portalon](./media/app-service-web-get-started-dotnet/web-app-blade.png)

hello bal oldali menü különböző oldalain biztosít az alkalmazás konfigurálását. 

[!INCLUDE [Clean-up section](../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a>Következő lépések

> [!div class="nextstepaction"]
> [ASP.NET-alkalmazás és SQL Database](app-service-web-tutorial-dotnet-sqldatabase.md)
