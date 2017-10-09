---
title: "aaaGet használatába API Apps és az ASP.NET, az App Service szolgáltatásban |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate, és felhasználhatják az ASP.NET API-alkalmazás az Azure App Service szolgáltatásban a Visual Studio 2015 használatával."
services: app-service\api
documentationcenter: .net
author: alexkarcher-msft
manager: erikre
editor: 
ms.assetid: ddc028b2-cde0-4567-a6ee-32cb264a830a
ms.service: app-service-api
ms.workload: na
ms.tgt_pltfrm: dotnet
ms.devlang: na
ms.topic: hero-article
ms.date: 09/20/2016
ms.author: alkarche
ms.openlocfilehash: d3e90f1585907d183b0435c6cafc5585bc1e29ca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-api-apps-aspnet-and-swagger-in-azure-app-service"></a>Az Azure App Service szolgáltatásban elérhető API Apps, az ASP.NET és a Swagger használatának megismerése
[!INCLUDE [selector](../../includes/app-service-api-get-started-selector.md)]

Ez az hello először egy sorozat része oktatóprogramot kínál, amelyek megjelenítése hogyan toouse részeit, az Azure App Service, amelyek hasznosak a fejlesztéséhez és üzemeltetéséhez a RESTful API-kat.  Ebben az oktatóanyagban Swagger formátumú API-metaadatok használatát vesszük alapul.

Az oktatóanyagból a következőket sajátíthatja el:

* Hogyan toocreate és központi telepítése [API-alkalmazások](app-service-api-apps-why-best-platform.md) az Azure App Service szolgáltatásban a Visual Studio 2015 beépített eszközei segítségével.
* Hogyan tooautomate API felderítési hello a Swashbuckle NuGet csomag toodynamically generálása Swagger API-metaadatok.
* Hogyan a Swagger API-metaadatok tooautomatically toouse generálhat ügyfélkódot az API-alkalmazást.

## <a name="sample-application-overview"></a>Mintaalkalmazás áttekintése
Ebben az oktatóanyagban egy egyszerű tennivalólista típusú mintaalkalmazáson keresztül mutatjuk be a szolgáltatás funkcióit. hello alkalmazás egyoldalas alkalmazások (SPA) előtér egy ASP.NET Web API középső réteg és egy ASP.NET Web API adatréteggel rendelkezik.

![Az API Apps mintaalkalmazás-diagramja](./media/app-service-api-dotnet-get-started/noauthdiagram.png)

Íme egy képernyőkép hello [AngularJS](https://angularjs.org/) előtér.

![API-alkalmazások minta toodo Alkalmazáslista](./media/app-service-api-dotnet-get-started/todospa.png)

hello Visual Studio megoldás három projektet tartalmaz:

![](./media/app-service-api-dotnet-get-started/projectsinse.png)

* **ToDoListAngular** -hello előtér: az AngularJS SPA, amely behívja a középső réteg hello.
* **ToDoListAPI** -hello középső réteg: egy ASP.NET Web API-projektet, amely behívja hello adatrétegbeli tooperform CRUD-műveleteknek a Tennivalólista elemein.
* **ToDoListDataAPI** -hello adatréteg: egy ASP.NET Web API-projektet, amely elvégzi a CRUD-műveleteknek a Tennivalólista elemein.

hello háromrétegű architektúra csupán egy azok közül, az API Apps segítségével Megvalósíthat, és itt csak bemutató célokra szolgál. az egyes rétegekbe hello kód más dolga, mint lehetséges toodemonstrate API Apps funkcióinak; például hello adatrétegbeli használ adatbázis helyett a kiszolgáló memória adatmegőrzési mechanizmusként.

Az oktatóanyag elvégzését hello két Web API- projektje fog az App Service API apps hello felhőben futó kell.

hello hello sorozat következő oktatóanyaga hello SPA előtér toohello felhő telepíti.

## <a name="prerequisites"></a>Előfeltételek
* Az ASP.NET Web API - hello oktatóanyagban szereplő utasítások során feltételezzük, hogy a vonatkozó általános ismeretekre ASP.NET toowork [Web API 2](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) a Visual Studióban.
* Azure-fiók – [Nyisson ingyenes Azure-fiókot](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) vagy [használja ki a Visual Studio-előfizetők számára elérhető előnyöket](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).
  
    Ha azt szeretné, hogy az az Azure-fiók regisztrálása előtt az Azure App Service lépések tooget, nyissa meg túl[App Service kipróbálása](https://azure.microsoft.com/try/app-service/). Itt azonnal létrehozhat egy ideiglenes, kezdő szintű alkalmazást az App Service szolgáltatásban – kötelezettségek vállalása, és **bankkártyaadatok megadása nélkül**.
* A Visual Studio 2015 a következővel hello [Azure SDK for .NET](https://azure.microsoft.com/downloads/archive-net-downloads/) -hello SDK telepíti a Visual Studio 2015 automatikusan még nincs.
  
  * A Visual Studióban kattintson a Súgó -> A Microsoft Visual Studio névjegye elemre, és győződjön meg arról, hogy az Azure App Service Tools 2.9.1-es vagy újabb verziója van telepítve.
    
    ![Azure App Tools-verzió](./media/app-service-api-dotnet-get-started/apiversion.png)
    
    > [!NOTE]
    > Attól függően, hogy hány SDK-függőség hello már van a számítógépen a telepítése hello SDK tooa mint fél óráig vagy tovább néhány perctől akár hosszú ideig eltarthat.
    > 
    > 

## <a name="download-hello-sample-application"></a>Hello mintaalkalmazás letöltése
1. Töltse le a hello [Azure-Samples/app-service-api-dotnet-to-do-list](https://github.com/Azure-Samples/app-service-api-dotnet-todo-list) tárházba.
   
    Kattinthat a hello **töltse le a ZIP-** hello gombra vagy a klónozott tárház a helyi számítógépen.
2. Nyissa meg a hello ToDoList megoldást a Visual Studio 2015-öt vagy 2013.
   
   1. Szüksége lesz tootrust egyes megoldások.
         ![Biztonsági figyelmeztetés](./media/app-service-api-dotnet-get-started/securitywarning.png)
3. Build hello (CTRL + SHIFT + B) megoldás toorestore hello NuGet-csomagok.
   
    Ha szeretné, a művelet toosee hello alkalmazás telepítése előtt, helyileg is futtathatja. Győződjön meg arról, hogy ToDoListDataAPI a kezdőprojekt és futtatási hello megoldás. A böngészőben számíthat toosee egy HTTP 403-as hiba.

## <a name="use-swagger-api-metadata-and-ui"></a>A Swagger API-metaadatok és felhasználói felület használata
Az Azure App Service beépített támogatást tartalmaz a [Swagger](http://swagger.io/) 2.0 API-metaadatokhoz. Minden API-alkalmazás megadhat egy URL-végpontot, amely Swagger JSON formátumban ad vissza hello API metaadatait. Hello, hogy a végpont által visszaadott metaadatok lehet toogenerate ügyfél használja.

Egy ASP.NET Web API-projekt lehet dinamikusan létrehozni a Swagger-metaadatok hello segítségével [Swashbuckle](https://www.nuget.org/packages/Swashbuckle) NuGet-csomagot. hello Swashbuckle NuGet-csomag már telepítve van a hello ToDoListDataAPI és ToDoListAPI projektek letöltött.

Ebben a szakaszban hello oktatóanyag hello generált Swagger 2.0 metaadatok tekinti meg, és próbálkozzon egy próba felhasználói felület hello Swagger-metaadatok alapján.

1. Állítsa be a hello ToDoListDataAPI projektet (**nem** hello ToDoListAPI projektet) hello indítási projektként.
   
    ![A ToDoDataAPI beállítása kezdőprojektként](./media/app-service-api-dotnet-get-started/startupproject.png)
2. Nyomja le az F5 billentyűt, vagy kattintson a **Debug > Start Debugging** toorun hello projekt hibakeresési módban.
   
    hello böngészőben megnyílik, és HTTP 403 Hibaoldal hello mutatja.
3. A böngésző címsorában adja hozzá `swagger/docs/v1` hello sort, és nyomja le az ENTER toohello végét. (URL-cím hello `http://localhost:45914/swagger/docs/v1`.)
   
    Ez a hello alapértelmezett URL-cím hello API Swashbuckle tooreturn Swagger 2.0 JSON-metaadatok által használt.
   
    Ha az Internet Explorer használata esetén hello böngésző felszólítja toodownload egy *v1.json* fájlt.
   
    ![JSON-metaadatok letöltése az Internet Explorerben](./media/app-service-api-dotnet-get-started/iev1json.png)
   
    Ha Chrome, Firefox vagy Microsoft Edge használata esetén hello böngésző hello JSON hello böngészőablakban jeleníti meg. A különböző böngészők eltérően kezelik a JSON és a böngészőablakot nem hasonlíthat pontosan hello példa.
   
    ![JSON-metaadatok a Chrome-ban](./media/app-service-api-dotnet-get-started/chromev1json.png)
   
    hello következő példa bemutatja hello Swagger-metaadatokat az API-t hello első szakasza hello hello hello-definícióval Get metódus. A metaadatok milyen meghajtók hello Swagger felhasználói Felületet, hogy a lépéseket követve hello segítségével, és egy későbbi szakasz ismerteti a hello oktatóanyag tooautomatically használhatja az Ügyfélkód generálásához.
   
        {
          "swagger": "2.0",
          "info": {
            "version": "v1",
            "title": "ToDoListDataAPI"
          },
          "host": "localhost:45914",
          "schemes": [ "http" ],
          "paths": {
            "/api/ToDoList": {
              "get": {
                "tags": [ "ToDoList" ],
                "operationId": "ToDoList_GetByOwner",
                "consumes": [ ],
                "produces": [ "application/json", "text/json", "application/xml", "text/xml" ],
                "parameters": [
                  {
                    "name": "owner",
                    "in": "query",
                    "required": true,
                    "type": "string"
                  }
                ],
                "responses": {
                  "200": {
                    "description": "OK",
                    "schema": {
                      "type": "array",
                      "items": { "$ref": "#/definitions/ToDoItem" }
                    }
                  }
                },
                "deprecated": false
              },
4. Zárja be hello böngészőt, és állítsa le a Visual Studio hibakeresési módját.
5. A projekt hello ToDoListDataAPI **Megoldáskezelőben**, nyissa meg hello *App_Start\SwaggerConfig.cs* fájlt, majd görgessen lefelé tooline 174, és állítsa vissza a következő kód hello.
   
        /*
            })
        .EnableSwaggerUi(c =>
            {
        */
   
    Hello *SwaggerConfig.cs* fájl jön létre a projektben hello Swashbuckle csomag telepítésekor. hello fájl számos módon tooconfigure Swashbuckle.
   
    hello kódrész lehetővé teszi, hogy hello Swagger felhasználói felület, a következő hello használó lépések. Amikor hello API-alkalmazás projektsablon használatával hoz létre Web API-projektet, ez a kód megjegyzésbe van alapértelmezés szerint biztonsági intézkedésként.
6. Futtassa újra a hello projektet.
7. A böngésző címsorában adja hozzá `swagger` hello sort, és nyomja le az ENTER toohello végét. (URL-cím hello `http://localhost:45914/swagger`.)
8. Ha hello Swagger felhasználói felületben kattintson **ToDoList** toosee hello módszer áll rendelkezésre.
   
    ![Swagger felhasználói felület, elérhető metódusok](./media/app-service-api-dotnet-get-started/methods.png)
9. Először kattintson hello **beolvasása** hello listában gombra.
10. A hello **paraméterek** területen írjon be csillag hello hello értékeként `owner` paraméter, és kattintson **próbálja ki**.
    
    Hitelesítés a későbbi részei hozzáadásakor a hello középső réteg biztosítani fogja hello tényleges felhasználói Azonosítót toohello adatrétegbeli. Most ezért minden feladatnál csillag helyettesítő a Tulajdonosazonosítót közben hello alkalmazás hitelesítés nélkül fut.
    
    ![Swagger felhasználói felület, kipróbálás funkció](./media/app-service-api-dotnet-get-started/gettryitout1.png)
    
    Swagger felhasználói felület hello hello ToDoList Get metódust hívja, és hello válaszkódot és a JSON-eredményeket jeleníti meg.
    
    ![Swagger felhasználói felület, kipróbálás, eredmények](./media/app-service-api-dotnet-get-started/gettryitout.png)
11. Kattintson a **Post**, és kattintson a hello csoportban **Modellsémát**.
    
    Kattintson a hello modell séma prefills hello beviteli mezőt hello paraméter értéke Post metódussal hello megadására. (Ha ez sem működik, az Internet Explorerben, próbálkozzon másik böngészővel, vagy manuálisan adja meg hello paraméter értéke hello következő lépésben.)  
    
    ![Swagger felhasználói felület, kipróbálás funkció, Post metódus](./media/app-service-api-dotnet-get-started/post.png)
12. Változás hello JSON a hello `todo` paraméter beviteli mezőjében úgy, hogy a következő példa hello úgy tűnik, vagy helyettesítse a saját leíró szöveg:
    
        {
          "ID": 2,
          "Description": "buy hello dog a toy",
          "Owner": "*"
        }
13. Kattintson a **Try it out** (Kipróbálás) elemre.
    
    hello ToDoList API sikerességét jelző HTTP 204-es válasz kódot ad vissza.
14. Először kattintson hello **beolvasása** gombra, majd hello oldal ezen részében hello **próbálja ki** gombra.
    
    hello Get metódusra adott válasz mostantól tartalmazza a hello új toodo elemet.
15. Nem kötelező: Is hello Put, Delete, próbálja meg, és azonosító módszerekkel beolvasása.
16. Zárja be hello böngészőt, és állítsa le a Visual Studio hibakeresési módját.

A Swashbuckle bármelyik ASP.NET Web API-projekttel működik. Ha tooadd Swagger metaadatainak generálása tooan meglévő projektet, egyszerűen telepítse hello Swashbuckle csomagot.

> [!NOTE]
> A Swagger-metaadatokban minden API-művelet saját egyedi azonosítót kap. Alapértelmezés szerint előfordulhat, hogy a Swashbuckle duplikált Swagger-műveleti azonosítókat hoz létre a Web API-vezérlő metódusokhoz. Ez akkor fordul elő, ha a vezérlőben túlterhelt HTTP-metódusok találhatók, például: `Get()` vagy `Get(id)`. Hogyan toohandle túlterhelések kapcsolatos információkért lásd: [testreszabása Swashbuckle által generált API-definíciók](app-service-api-dotnet-swashbuckle-customize.md). Ha Web API-projektet a Visual Studio hello Azure API App sablon segítségével hoz létre, egyedi műveleti azonosítókat generáló kódot automatikusan hozzáadott toohello *SwaggerConfig.cs* fájlt.  
> 
> 

## <a id="createapiapp"></a>API-alkalmazás létrehozása az Azure-ban, és a kód tooit telepítése
Ebben a szakaszban használhatja az Azure-eszközök Visual Studio hello integrált **webhely közzététele** varázsló toocreate egy olyan új API alkalmazást az Azure-ban. Ezután hello ToDoListDataAPI projektet toohello új API alkalmazást, és hello API hívása hello Swagger felhasználói felület futtatásával.

1. A **Megoldáskezelőben**, kattintson a jobb gombbal a hello ToDoListDataAPI projektet, és kattintson a **közzététel**.
   
    ![A Publish (Közzététel) elem a Visual Studióban](./media/app-service-api-dotnet-get-started/pubinmenu.png)
2. A hello **profil** hello lépését **webhely közzététele** varázsló, kattintson a **Microsoft Azure App Service**.
   
   ![Az Azure App Service elem kiválasztása a Publish Web (Weboldal közzététele) varázslóban](./media/app-service-api-dotnet-get-started/selectappservice.png)
3. Jelentkezzen be Azure-fiók tooyour, ha még nem tette meg, vagy frissítse a hitelesítő adatokat, ha kijelentkeztette.
4. Az App Service párbeszédpanelen hello, válassza a hello Azure **előfizetés** toouse szeretne, és kattintson a **új**.
   
    ![A New (Új) elem kiválasztása az App Service párbeszédpanelen](./media/app-service-api-dotnet-get-started/clicknew.png)
   
    Hello **üzemeltetési** hello lapján **létrehozása az App Service** párbeszédpanel jelenik meg.
   
    Mivel, amelyhez telepített Swashbuckle tartozik Web API-projektet telepít, a Visual Studio feltételezi, hogy toocreate API-alkalmazást. Ez jelzi hello **API App Name** title és által hello tényt, hogy hello **típusának módosítása** legördülő lista túl van-e állítva**API-alkalmazás**.
   
    ![Alkalmazás típusának megadása az App Service párbeszédpanelen](./media/app-service-api-dotnet-get-started/apptype.png)
5. Adjon meg egy **API App Name** a hello egyedi *azurewebsites.net* tartomány. Elfogadhatja a Visual Studio által ajánlott hello alapértelmezett nevet.
   
    Ha megad egy nevet, hogy valaki más már használatban van, megjelenik egy piros felkiáltójel toohello jobbra.
   
    hello hello API-alkalmazás URL-CÍMÉT lesz `{API app name}.azurewebsites.net`.
6. A hello **erőforráscsoport** legördülő menüben kattintson **új**, majd adja meg "ToDoListGroup" nevet, vagy ha szeretné.
   
    Az erőforráscsoportok Azure-erőforrások (például API Apps, adatbázisok, virtuális gépek stb.) gyűjteményei.    Ebben az oktatóanyagban esetén ajánlott toocreate egy új erőforráscsoportot mivel így később könnyen toodelete összes hello Azure-erőforrások hello az oktatóanyaghoz létrehozott egy lépésben.
   
    Ebben a mezőben kiválaszthat egy meglévő [erőforráscsoportot](../azure-resource-manager/resource-group-overview.md), vagy újat is létrehozhat. Új csoport létrehozásához írjon be egy olyan nevet, amely eltér az előfizetéshez tartozó többi erőforráscsoport nevétől.
7. Kattintson a hello **új** gomb következő toohello **App Service-csomag** legördülő listán.
   
    hello a képernyőfelvételen látható tartalmaz **API App Name**, **előfizetés**, és **erőforráscsoport** --a eltérőek lesznek.
   
    ![A Create App Service (App Service létrehozása) párbeszédpanel](./media/app-service-api-dotnet-get-started/createas.png)
   
    Az alábbi lépésekkel hello hello új erőforráscsoporthoz tartozó App Service-csomagot hoz létre. Az App Service-csomag határozza meg, amely az API-alkalmazás fut hello számítási erőforrásokat. Például ha hello ingyenes csomagot választja, az API-alkalmazás fut a megosztott virtuális gépeken, míg egyes fizetett szinteken dedikált virtuális gépeken. Az App Service-csomagokkal kapcsolatban további információkért lásd a következő cikket: [App Service plans overview](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) (App Service-csomagok áttekintése).
8. A hello **konfigurálása App Service-csomag** párbeszédpanelen adja meg "ToDoListPlan", vagy egy másik nevet, ha szeretné.
9. A hello **hely** legördülő menüben válassza ki a legközelebbi tooyou hello helyre.
   
    Ez a beállítás azt határozza meg, hogy melyik Azure-adatközpontban fog futni az alkalmazás. Válasszon egy helyet, zárja be tooyou toominimize [késés](http://www.bing.com/search?q=web%20latency%20introduction&qs=n&form=QBRE&pq=web%20latency%20introduction&sc=1-24&sp=-1&sk=&cvid=eefff99dfc864d25a75a83740f1e0090).
10. A hello **mérete** legördülő menüben kattintson **szabad**.
    
    Ebben az oktatóanyagban hello szabad IP-címek is megfelelő teljesítményt nyújt.
11. A hello **konfigurálása App Service-csomag** párbeszédpanel, kattintson a **OK**.
    
    ![Az OK gombra kattintás a Configure App Service Plan (App Service-csomag konfigurálása) párbeszédpanelen](./media/app-service-api-dotnet-get-started/configasp.png)
12. A hello **létrehozása az App Service** párbeszédpanel, kattintson a **létrehozása**.
    
    ![A Create (Létrehozás) gombra kattintás a Create App Service (App Service létrehozása) párbeszédpanelen](./media/app-service-api-dotnet-get-started/clickcreate.png)
    
    A Visual Studio létrehozza a hello API-alkalmazást és egy közzétételi profilt, amely rendelkezik az összes szükséges hello beállítások hello API-alkalmazás. Ezt követően megnyitja hello **webhely közzététele** varázslót, amely azt ismertetjük toodeploy hello projekt.
    
    Hello **webhely közzététele** megnyílik a varázsló hello **kapcsolat** (lásd alább) fülre.
    
    A hello **kapcsolat** lap hello **Server** és **helynév** beállítások pont tooyour API-alkalmazásba. Hello **felhasználónév** és **jelszó** által Azure létrehozott telepítési hitelesítő adatokat. A központi telepítést követően megnyílik a Visual Studio egy böngésző toohello **URL-címre** (célja hello csak a **cél URL-címe**).  
13. Kattintson a **Tovább** gombra.
    
    ![Kattintás a Next (Tovább) gombra a Publish Web (Weboldal közzététele) varázsló Connection (Kapcsolat) lapján](./media/app-service-api-dotnet-get-started/connnext.png)
    
    hello következő lap hello **beállítások** (lásd alább) fülre. Itt módosíthatja hello build konfigurációs lapon toodeploy hibakeresési buildet [távoli hibakeresés](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug). hello lapon is biztosít, több **Fájlközzétételi beállítás**:
    
    * Remove additional files at destination (További fájlok eltávolítása a célhelyen)
    * Precompile during publishing (Előfordítás a közzététel során)
    * Fájlok kizárása hello App_Data mappájában
    
    Ehhez az oktatóanyagokhoz ezek egyikét sem kell használnia. Ezek használatáról a következő cikkben talál részletes leírást: [How to: Deploy a Web Project Using One-Click Publish in Visual Studio](https://msdn.microsoft.com/library/dd465337.aspx) (Útmutató: Webes projekt telepítése a Visual Studio Közzététel egyetlen kattintással funkciójával).
14. Kattintson a **Tovább** gombra.
    
    ![Kattintás a Next (Tovább) gombra a Publish Web (Weboldal közzététele) varázsló Settings (Beállítások) lapján](./media/app-service-api-dotnet-get-started/settingsnext.png)
    
    Ezután a hello van **előzetes** lapon (lásd alább), amely lehetővé teszi az olyan lehetőség toosee, mely fájlokat fogja másolni a projekt toohello API-alkalmazás toobe. Ha egy már telepített tooearlier projekt tooan API-alkalmazást telepít, csak a módosult fájlokat másolja. Ha azt szeretné, hogy toosee másolandó listáját, kattintson a hello **Start Preview** gombra.
15. Kattintson a **Publish** (Közzététel) gombra.
    
    ![Kattintás a Publish (Közzététel) gombra a Publish Web (Webes közzététel) varázsló Preview (Előnézet) lapján](./media/app-service-api-dotnet-get-started/clickpublish.png)
    
    Visual Studio hello ToDoListDataAPI projektet toohello új API alkalmazást telepíti. Hello **kimeneti** ablak naplózza a sikeres telepítést, és a "sikeres létrehozásról" tájékoztató oldal jelenik meg a böngészőben megnyílik toohello URL-címében hello API-alkalmazás.
    
    ![Output (Kimenet) ablak, sikeres telepítés](./media/app-service-api-dotnet-get-started/deploymentoutput.png)
    
    ![Az új API-alkalmazás sikeres létrehozásáról tájékoztató oldal](./media/app-service-api-dotnet-get-started/appcreated.png)
16. Hello böngésző címsorában adja hozzá "swagger" toohello URL-címet, majd nyomja le az ENTER billentyűt. (URL-cím hello `http://{apiappname}.azurewebsites.net/swagger`.)
    
    hello böngésző Swagger felhasználói felület, amely a korábban, de most fut. hello felhő hello jeleníti meg. Próbálja ki hello Get metódust, és láthatja, hogy Ön hátsó toohello 2 alapértelmezett tennivaló. hello elvégzett módosításokat hello helyi gép memóriájába mentette.
17. Nyissa meg hello [Azure-portálon](https://portal.azure.com/).
    
    hello Azure portál egy webes felület, például az API-alkalmazások kezelése az Azure erőforrások.
18. Kattintson a **További szolgáltatások > App Services** elemre.
    
    ![Tallózás az Alkalmazásszolgáltatások között](./media/app-service-api-dotnet-get-started/browseas.png)
19. A hello **alkalmazásszolgáltatások** panelen keresse meg és kattintson az új API-alkalmazás. (Hello Azure-portálon, a megnyíló ablakokat toohello jobb neve *paneleken*.)
    
    ![Az App Services (Alkalmazásszolgáltatások) panel](./media/app-service-api-dotnet-get-started/choosenewapiappinportal.png)
    
    Két panel nyílik meg. Az egyik panelen hello API-alkalmazás áttekintése, pedig egy megtekinthetik és módosíthatják a beállításokat listája túl hosszú.
20. A hello **beállítások** panelen, a keresés hello **API** szakaszt, és kattintson **API-definíció**.
    
    ![Az API Definition (API-definíció) elem a Settings (Beállítások) panelen](./media/app-service-api-dotnet-get-started/apidefinsettings.png)
    
    Hello **API-definíció** panelen adhatja meg a hello URL-címet, amely Swagger 2.0-metaadatokat adja vissza JSON formátumban. Visual Studio létrehozza a hello API-alkalmazást, amikor hello API definition URL-cím toohello alapértelmezett értéket állítja a a Swashbuckle által generált metaadatokban, amely a korábban, amely hello API-alkalmazás csomagazonosítóját alap URL-cím plusz `/swagger/docs/v1`.
    
    ![Az API-definíció URL-címe](./media/app-service-api-dotnet-get-started/apidefurl.png)
    
    Ez az API app toogenerate Ügyfélkód választva, a Visual Studio lekéri a hello metaadatait a URL-címet.

## <a id="codegen"></a>Hello adatrétegbeli Ügyfélkód generálása
A Swagger integrálása az Azure API apps előnyeit hello egyik kódok automatikus létrehozása. A generált révén könnyebben toowrite kódot, amely hívja az API-alkalmazást.

hello a ToDoListAPI projekt már hello generált ügyfélkódot, de a lépéseket követve hello bemutatjuk törölje azt, hogyan toodo hello kód generálása a toosee ismét.

1. A Visual Studio **Megoldáskezelőben**, a hello ToDoListAPI projektre, törölje a hello *ToDoListDataAPI* mappa. **Figyelmeztetés: Csak hello mappa törléséhez hello ToDoListDataAPI projektet.**
   
    ![Generált ügyfélkód törlése](./media/app-service-api-dotnet-get-started/deletecodegen.png)
   
    Ez a mappa, amely körülbelül toogo keresztül hello kódgenerálási folyamat segítségével hozta létre.
2. Kattintson a jobb gombbal a hello ToDoListAPI projektet, és kattintson **Hozzáadás > REST API-ügyfél**.
   
    ![REST API-ügyfél hozzáadása a Visual Studióban](./media/app-service-api-dotnet-get-started/codegenmenu.png)
3. A hello **REST API-ügyfél hozzáadása** párbeszédpanel, kattintson a **Swagger URL**, és kattintson a **Azure eszköz kiválasztása**.
   
    ![Azure-objektum kiválasztása](./media/app-service-api-dotnet-get-started/codegenbrowse.png)
4. A hello **App Service** párbeszédpanelen bontsa ki a jelen oktatóanyag használata hello erőforráscsoportot, és válassza ki az API-alkalmazás, és kattintson **OK**.
   
    ![API-alkalmazás kiválasztása a kódgeneráláshoz](./media/app-service-api-dotnet-get-started/codegenselect.png)
   
    Figyelje meg, hogy amikor visszatér toohello **REST API-ügyfél hozzáadása** párbeszédpanelen hello szövegmező rendszer kitöltötte hello API definition hello portálon korábban látott URL-címével.
   
    ![API-definíció URL-címe](./media/app-service-api-dotnet-get-started/codegenurlplugged.png)
   
   > [!TIP]
   > Egy másik módja tooget kód generálására metaadatai tooenter hello URL-címet közvetlenül tett lépéseket megkerülve hello tallózási párbeszédpanelre. Vagy ha toogenerate Ügyfélkód tooAzure telepítése előtt, sikerült hello Web API-projekt helyi futtatásra, nyissa meg hello Swagger JSON-fájlt mentse hello fájlt biztosító toohello URL-címet, és hello használata **válasszon egy meglévő Swagger-metaadatfájl**lehetőséget.
   > 
   > 
5. A hello **REST API-ügyfél hozzáadása** párbeszédpanel, kattintson a **OK**.
   
    A Visual Studio létrehoz egy után hello API app nevű mappát, és elvégzi az ügyfélosztályok generálását.
   
    ![A generált ügyfélhez tartozó kódfájlok](./media/app-service-api-dotnet-get-started/codegenfiles.png)
6. Hello ToDoListAPI projektben nyissa meg *Controllers\ToDoListController.cs* toosee hello kód sor 40 hello API hello generált ügyfél segítségével behívó.
   
    a következő kódrészletet hello bemutatja, hogyan hello kód példányosítja hello objektumot, és hívások hello Get metódus.
   
        private static ToDoListDataAPI NewDataAPIClient()
        {
            var client = new ToDoListDataAPI(new Uri(ConfigurationManager.AppSettings["toDoListDataAPIURL"]));
            return client;
        }
   
        public async Task<IEnumerable<ToDoItem>> Get()
        {
            using (var client = NewDataAPIClient())
            {
                var results = await client.ToDoList.GetByOwnerAsync(owner);
                return results.Select(m => new ToDoItem
                {
                    Description = m.Description,
                    ID = (int)m.ID,
                    Owner = m.Owner
                });
            }
        }
   
    hello konstruktorparamétert hello végponti URL-cím lekérése hello `toDoListDataAPIURL` Alkalmazásbeállítás. Hello Web.config fájlban, hogy értéke set toohello helyi IIS Express URL-címe hello API projektre, hogy hello alkalmazás helyileg futtathat. Ha hello konstruktor paraméter nincs megadva, hello alapértelmezett végpont: hello kódot generálta hello URL-cím.
7. Ön egy eltérő nevű az API-alkalmazás nevének; alapján jön létre hello kód módosítható *Controllers\ToDoListController.cs* , hogy hello típusnév generáltnak a projektben. Ha például az API-alkalmazás neve ToDoListDataAPI071316, a következő kódot:
   
        private static ToDoListDataAPI NewDataAPIClient()
        {
            var client = new ToDoListDataAPI(new Uri(ConfigurationManager.AppSettings["toDoListDataAPIURL"]));

toothis:

        private static ToDoListDataAPI071316 NewDataAPIClient()
        {
            var client = new ToDoListDataAPI071316(new Uri(ConfigurationManager.AppSettings["toDoListDataAPIURL"]));


## <a name="create-an-api-app-toohost-hello-middle-tier"></a>Az API app toohost hello középső réteg létrehozása
Korábban, [hello API-alkalmazás adatrétegét létrehozott és telepített kód tooit](#createapiapp).  Hajtsa végre a hello most ugyanezt az eljárást hello középső rétegbeli API-alkalmazást.

1. A **Megoldáskezelőben**, kattintson a jobb gombbal hello középső rétegbeli ToDoListAPI projektre (nem hello adatrétegbeli ToDoListDataAPI), és kattintson a **közzététel**.
   
    ![A Publish (Közzététel) elem a Visual Studióban](./media/app-service-api-dotnet-get-started/pubinmenu2.png)
2. A hello **profil** hello lapján **webhely közzététele** varázsló, kattintson a **Microsoft Azure App Service**.
3. A hello **App Service** párbeszédpanel, kattintson a **új**.
4. A hello **üzemeltetési** hello lapján **létrehozása az App Service** párbeszédpanel mezőben fogadja el az alapértelmezett hello **API App Name** vagy adjon meg egy nevet, amely egyedi a hello  *azurewebsites.NET* tartomány.
5. Válassza ki a hello Azure **előfizetés** használja.
6. A hello **erőforráscsoport** legördülő listából válassza ki a hello ugyanazt az a korábban létrehozott erőforráscsoportot.
7. A hello **App Service-csomag** legördülő listából válassza ki a hello korábban létrehozott csomagot. Az toothat értéke alapértelmezés szerint.
8. Kattintson a **Create** (Létrehozás) gombra.
   
    A Visual Studio létrehozza hello API-alkalmazást, a közzétételi profilt hozza létre és hello megjeleníti **kapcsolat** hello lépését **webhely közzététele** varázsló.
9. A hello **kapcsolat** hello lépését **webhely közzététele** varázsló, kattintson a **közzététel**.
   
   Visual Studio telepíti a hello ToDoListAPI projekt toohello új API-alkalmazást, és megnyitja a böngésző toohello URL-t hello API-alkalmazásba. "sikeresen létrejött" Hello lap jelenik meg.

## <a name="configure-hello-middle-tier-toocall-hello-data-tier"></a>Hello középső réteg toocall hello adatszint konfigurálása
Ha most nevű hello középső rétegbeli API-alkalmazást, azt próbálná toocall hello adatrétegbeli hello localhost URL-cím használatával, amely még hello Web.config fájlban. Ebben a szakaszban a hello adatok réteg API alkalmazás URL-CÍMÉT a hello középső réteg API-alkalmazásának egyik környezeti beállításánál meg. Hello hello középső rétegbeli API-alkalmazás kódja lekéri hello adatok adatréteg URL-beállításait, amikor a hello környezeti beállítás felülírja a hello Web.config fájlt.

1. Nyissa meg toohello [Azure-portálon](https://portal.azure.com/), és navigáljon a toohello **API-alkalmazás** toohost hello TodoListAPI (középső réteg) projekt létrehozott hello API-alkalmazás paneljén.
2. Az API-alkalmazás hello **beállítások** panelen kattintson a **Alkalmazásbeállítások**.
3. Az API-alkalmazás hello **Alkalmazásbeállítások** panelen görgessen lefelé toohello **Alkalmazásbeállítások** szakaszt, és adja hozzá a következő hello kulcs-érték. hello értéket fogja hello hello első API App ebben az oktatóanyagban közzétett URL-CÍMÉT.
   
   | **Kulcs** | toDoListDataAPIURL |
   | --- | --- |
   | **Érték** |https://{az adatréteghez tartozó API-alkalmazás neve}.azurewebsites.net |
   | **Példa** |https://todolistdataapi.azurewebsites.net |
4. Kattintson a **Save** (Mentés) gombra.
   
    ![Kattintás a Mentés gombra az Alkalmazásbeállítások menüben](./media/app-service-api-dotnet-get-started/asinportal.png)
   
    Amikor hello kód lefut az Azure-ban, ez az érték most felülírják hello localhost URL-cím hello Web.config fájlban.

## <a name="test"></a>Tesztelés
1. Egy böngészőablakban keresse meg a hello új középső rétegbeli API-alkalmazás, amelyet most hozott létre a ToDoListAPI toohello URL-CÍMÉT. Ehhez úgy juthat el hello hello portál fő paneljén hello API-alkalmazás URL-CÍMRE kattintva.
2. Hello böngésző címsorában adja hozzá "swagger" toohello URL-címet, majd nyomja le az ENTER billentyűt. (URL-cím hello `http://{apiappname}.azurewebsites.net/swagger`.)
   
    hello böngésző megjeleníti hello Swagger felhasználói felület, amely korábban már látta a ToDoListDataAPI, de most `owner` megadása nem kötelező hello Get művelethez, mert hello középső rétegbeli API-alkalmazást, hogy érték toohello API-alkalmazás adatrétegét küld Önnek. (Ha a hitelesítési oktatóanyagok hello, hello középső réteg valós felhasználói azonosítókat a hello fog küldeni `owner` paraméter; most azt fix kódolású csillag.)
3. Próbálja ki a Get metódust hello és hello egyéb módszerek toovalidate, amely hello középső rétegbeli API-alkalmazás sikeresen behívja hello adatréteg API-alkalmazás.
   
    ![Swagger felhasználói felület, Get metódus](./media/app-service-api-dotnet-get-started/midtierget.png)

## <a name="troubleshooting"></a>Hibakeresés
Ha az oktatóanyag lépéseinek elvégzése közben hibákba ütközne, olvassa el az alábbi hibakeresési tippeket:

* Győződjön meg arról, hogy hello hello legújabb verzióját használja [Azure SDK for .NET](http://go.microsoft.com/fwlink/?linkid=518003).
* Hello projekt neve kettő hasonlít egymáshoz (ToDoListAPI, ToDoListDataAPI). Ha dolgot nem utasításokban leírtak hello utasításokat egy dolgozva, ellenőrizze, hogy hello megfelelő projektet nyitotta meg.
* Ha egy vállalati hálózat és próbál toodeploy tooAzure App Service a tűzfalon keresztül, győződjön meg arról, hogy 443-as és 8172-es port nyitva a Web Deploy keretrendszert. Ha nem tudja megnyitni ezeket a portokat, használjon más telepítési módszert.  Lásd: [telepítheti az alkalmazást tooAzure App Service](../app-service-web/web-sites-deploy.md).
* "Útvonalneveknek egyedieknek kell lenniük" hibák sikerült kap ezek Ha véletlenül a hello megfelelő projektet tooan API app telepíti, később központilag terjesztheti hello helyes-e egy tooit. toocorrect a, helyezze üzembe újra hello megfelelő projektet toohello API app hello a **beállítások** hello lapján **webhely közzététele** varázsló select **célhelyentovábbifájlokeltávolítása**.

Miután az ASP.NET API-alkalmazás futtatása az Azure App Service-ben, érdemes lehet további információk a Visual Studio hibaelhárítást egyszerűsítő szolgáltatásairól toolearn. A naplózással, a távoli hibakereséssel és a többi funkcióval kapcsolatban további információkért lásd: [Troubleshooting Azure App Service apps in Visual Studio](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md) (Azure App Service alkalmazások hibakeresése a Visual Studióban).

## <a name="next-steps"></a>Következő lépések
Megtudhatta, hogyan toodeploy meglévő Web API projektek tooAPI alkalmazások, generálhat ügyfélkódot az API apps, és a .NET-ügyfelekről az API-alkalmazásokat. a sorozat következő oktatóanyaga hello bemutatja, hogyan túl[használhatja a CORS tooconsume API alkalmazásokat JavaScript-ügyfelekből](app-service-api-cors-consume-javascript.md).

Ügyfélkód generálásával kapcsolatban további információkért lásd: hello [Azure/AutoRest](https://github.com/azure/autorest) github.com tárházba. Hello generált ügyfél segítségével problémák kapcsolatban nyisson meg egy [hello AutoRest tárházban probléma](https://github.com/azure/autorest/issues).

Ha azt szeretné, hogy toocreate új API app projektet teljesen új, használja a hello **Azure API App** sablont.

![API App sablon a Visual Studióban](./media/app-service-api-dotnet-get-started/apiapptemplate.png)

Hello **Azure API App** projektsablon egyenértékű toochoosing hello **üres** ASP.NET 4.5.2 sablont, kattintással hello jelölőnégyzetet tooadd Web API támogatását, és telepíti a Swashbuckle NuGet csomagot hello. Ezenkívül hello sablon hozzáadása némi Swashbuckle konfigurációs kódot is, amely tooprevent hello létrehozása duplikált Swagger műveleti azonosítók. Ha létrehozta az API App projektet, ezután telepítheti azt tooan API app hello ebben az oktatóanyagban megtudhatta azonos módon.

