---
title: "Flighting deployment (bétatesztelés) az Azure App Service-ben"
description: "Útmutató az alkalmazás új szolgáltatások repülési vagy beta végpont oktatóanyagban a frissítések tesztelése. Összegyűjti az App Service szolgáltatások, mint a folyamatos közzététel, tárhelyek, a forgalom útválasztásához és az Application Insights-integráció."
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: 17953c51-38f8-442d-bb0b-f69c1542f0e9
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/02/2016
ms.author: cephalin
ms.openlocfilehash: 83e3247310461ac148fff3c4ade3aa7216478537
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="flighting-deployment-beta-testing-in-azure-app-service"></a>Flighting deployment (bétatesztelés) az Azure App Service-ben
Az oktatóanyag bemutatja, hogyan kell végrehajtani *flighting központi telepítések* különböző lehetőségeinek integrálásával [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) és [Azure Application Insights](/services/application-insights/).

*Flighting* egy folyamatot, amely egy új szolgáltatás, vagy módosítsa a valódi felhasználók korlátozott számú ellenőrzi, és egy fő tesztelése éles telepítési forgatókönyvhöz. Mintha beta tesztelése, és mint "ellenőrzött teszt repülési" néven is ismert. Egy webes jelenlét sok nagy vállalatok ezt a módszert használja az app frissítéseken a gyakorlatban a korai érvényesítési eléréséhez [gyors fejlesztési](https://en.wikipedia.org/wiki/Agile_software_development). Az Azure App Service lehetővé teszi a teszt termelési környezetben integrálható a folyamatos közzététele és az Application Insights is az azonos DevOps forgatókönyv megvalósításához. Ez a módszer előnyei a következők:

* **Valós visszajelzés szerezhet *előtt* frissítések üzemi** -jobb, mint való visszajelzést, amint felszabadít egyedül van való visszajelzést, engedélyezés előtt. A felhasználó forgalmi és viselkedéshez frissítések korai biztosítják a életciklusa tesztelheti.
* **Javíthatja [folyamatos teszt adatvezérelt fejlesztési (CTDD)](https://en.wikipedia.org/wiki/Continuous_test-driven_development)**  - teszt termelési környezetben és integrálásával folyamatos integrációt és az Application insights szolgáltatással instrumentation felhasználói érvényesítési korai és automatikusan történik, a termék életciklusának. Ezzel csökkenthető manuális tesztelési végrehajtási idő befektetések.
* **Teszt munkafolyamat optimalizálása** -teszt termelési környezetben, a folyamatos figyelési instrumentation automatizálásával potenciálisan elvégezhető a célja, különféle típusú tesztek egyetlen folyamat, például a [integrációs](https://en.wikipedia.org/wiki/Integration_testing), [regressziós](https://en.wikipedia.org/wiki/Regression_testing), [használhatóság](https://en.wikipedia.org/wiki/Usability_testing), kisegítő, honosítási, [teljesítmény](https://en.wikipedia.org/wiki/Software_performance_testing), [biztonsági](https://en.wikipedia.org/wiki/Security_testing), és [elfogadási](https://en.wikipedia.org/wiki/Acceptance_testing).

Egy flighting telepítési szinte nem útválasztási élő forgalmat. Ilyen elrendezés minél gyorsabban áttekintésével betekintést szeretné hogy legyen egy nem várt hiba, a teljesítménycsökkenés, a felhasználói élmény problémák. Ne feledje, hogy valós ügyfelek foglalkoznak. Ezért ne jobb oldali biztosítsa, hogy állította be a flighting telepítés tájékozott döntést, a következő lépéshez szükséges minden adatgyűjtést. Az oktatóanyag bemutatja, hogyan szeretné az adatgyűjtést az Application insights szolgáltatással, de a New Relic vagy egyéb technológiák, amely megfelel az adott esetben is használhatja.

## <a name="what-you-will-do"></a>Mit fog
Ebben az oktatóanyagban megtanulhatja, hogyan kell az App Service-alkalmazás tesztelése éles üzemben segítségével a következő esetekben:

* [Éles forgalmat](app-service-web-test-in-production-get-start.md) a béta-alkalmazásba
* [Az alkalmazás állíthatnak](../application-insights/app-insights-web-track-usage.md) hasznos metrikákat beszerzése
* Folyamatosan a béta-alkalmazás telepítése, és nyomon követheti az élő app metrikák
* Az éles alkalmazások és a béta alkalmazás láthatja, hogyan kódmódosításokat fordítás eredmények között mérőszámok összehasonlítása

## <a name="what-you-will-need"></a>Mit kell
* Az Azure-fiók
* A [GitHub](https://github.com/) fiók
* A Visual Studio 2015 - letöltheti a [Community edition](https://www.visualstudio.com/en-us/products/visual-studio-express-vs.aspx).
* Git rendszerhéj (telepített [GitHub for Windows](https://windows.github.com/)) – Ez lehetővé teszi, hogy a Git és a PowerShell-parancsok futtatása ugyanabban a munkamenetben
* Legújabb [Azure PowerShell](https://github.com/Azure/azure-powershell/releases/download/v0.9.8-September2015/azure-powershell.0.9.8.msi) bits
* A következő alapvető ismeretekkel:
  * [Az Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) sablon-üzembehelyezés (lásd: [kiszámítható módon tudja az Azure-ban összetett alkalmazást központilag](app-service-deploy-complex-application-predictably.md))
  * [Git](http://git-scm.com/documentation)
  * [PowerShell](https://technet.microsoft.com/library/bb978526.aspx)

> [!NOTE]
> A jelen oktatóanyag elvégzéséhez Azure-fiókra van szükség:
>
> * Is [szabad nyissa meg az Azure-fiók](https://azure.microsoft.com/pricing/free-trial/) -kapott kreditek használatával kipróbálhatja a fizetős Azure-szolgáltatásokat, és még azok lejárta után is megtarthatja a fiókot, és használhatja az ingyenes Azure-szolgáltatások, például webes alkalmazásokat.
> * Is [aktiválhatja a Visual Studio előfizetői előnyeit](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) -a Visual Studio-előfizetéssel biztosít Önnek krediteket minden hónapban, fizetős Azure-szolgáltatásokat is használhat.
>
> Ha az Azure App Service-t az Azure-fiók regisztrálása előtt szeretné kipróbálni, ugorjon [Az Azure App Service kipróbálása](https://azure.microsoft.com/try/app-service/) oldalra. Itt azonnal létrehozhat egy ideiglenes, kezdő szintű webalkalmazást az App Service szolgáltatásban. Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.
>
>

## <a name="set-up-your-production-web-app"></a>Az éles webes alkalmazás beállítása
> [!NOTE]
> Ebben az oktatóanyagban használt parancsfájl automatikusan konfigurálja a GitHub-tárház folyamatos közzététel. Ehhez az szükséges, hogy a Githubon tartozó hitelesítő adatok már szerepelnek Azure, ellenkező esetben a parancsprogram-alapú központi telepítés sikertelen lesz a verziókövetési beállítások a webes alkalmazások konfigurálása.
>
> A GitHub hitelesítő adatok tárolását az Azure-ban, a webalkalmazás létrehozása a [Azure Portal](https://portal.azure.com/) és [GitHub-KözpontiTelepítés konfigurálása](app-service-continuous-deployment.md). Csak akkor kell egyszer.
>
>

Egy tipikus DevOps forgatókönyv valamely alkalmazás az élő Azure-ban futó, és szeretné módosítani a folyamatos közzétételen keresztül;. Ebben a forgatókönyvben Ha telepíti az üzemi egy sablon, amelyet kifejlesztett és tesztelt.

1. Hozzon létre a saját elágazás a [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) tárházba. Létrehozásával kapcsolatos információkat az elágazáshoz, tekintse meg a [oszthatja ketté a tárházban](https://help.github.com/articles/fork-a-repo/). Ha létrejött az elágazáshoz, tekintheti meg a böngészőben.

   ![](./media/app-service-agile-software-development/production-1-private-repo.png)
2. Nyisson meg egy Git rendszerhéj-munkamenetet. Ha a Git rendszerhéj még nincs, telepítse [GitHub for Windows](https://windows.github.com/) most.
3. Klónozhatja helyi az elágazáshoz futtassa a következő parancsot:

     git-klón https://github.com/<your_fork>/ToDoApp.git
4. Ha elvégezte a helyi klónozás, navigáljon a  *&lt;repository_root >*\ARMTemplates, és futtatnia kell a deploy.ps1 parancsfájlt egyedi utótagot kapjon, az alább látható módon:

     .\deploy.ps1 – RepoUrl https://github.com/<your_fork>/todoapp.git - ResourceGroupSuffix < your_suffix >
5. Amikor a rendszer kéri, írja be a kívánt felhasználónevet és jelszót az adatbázis eléréséhez. Ne felejtse el az adatbázis hitelesítő adatait, mert kell újból megadnia őket az erőforráscsoport frissítésekor.

   Meg kell jelennie a kiépítési folyamat különböző Azure-erőforrások. Központi telepítés befejezése után a parancsfájl indítani az alkalmazást a böngészőben, és adjon meg egy rövid hangjelzés.
   ![](./media/app-service-web-test-in-production-controlled-test-flight/00.1-app-in-browser.png)
6. Vissza a Git rendszerhéj-munkamenetben futtassa:

     . \swap – name ToDoApp < your_suffix >

   ![](./media/app-service-web-test-in-production-controlled-test-flight/00.2-swap-to-production.png)
7. A parancsfájl befejezése után térjen vissza az előtér-cím megkeresése tallózással (http://ToDoApp*&lt;your_suffix >*.azurewebsites.net/) az éles környezetben futó alkalmazást.
8. Jelentkezzen be a [Azure Portal](https://portal.azure.com/) és vessen egy pillantást mi jön létre.

   Meg kell tudni ugyanazt az erőforráscsoportot, egy-két webes alkalmazások jelennek meg a `Api` utótag nevét. Az erőforráscsoport nézet tekinti meg, ha az SQL-adatbázis és a kiszolgáló, az App Service-csomag és az átmeneti üzembe helyezési ponti a web Apps is láthat. Tallózzon a különböző erőforrások, és hasonlítsa össze azokat a  *&lt;repository_root >*\ARMTemplates\ProdAndStage.json megtekintéséhez, hogy azok miként vannak konfigurálva a sablonban.

   ![](./media/app-service-web-test-in-production-controlled-test-flight/00.3-resource-group-view.png)

Az éles alkalmazás beállítása.  Most tegyük képzelhető el, hogy megjelenik-e visszajelzést, hogy az alkalmazás gyenge-e használhatóság. Ezért úgy dönt, hogy kivizsgálja a Microsofttal. Beállíthatják az alkalmazásnak, hogy visszajelzést fog.

## <a name="investigate-instrument-your-client-app-for-monitoringmetrics"></a>Vizsgálja meg: Állíthatnak be az ügyfél alkalmazás figyelési/metrikák
1. Nyissa meg  *&lt;repository_root >*\src\MultiChannelToDo.sln a Visual Studióban.
2. Kattintson a jobb gombbal a megoldás Nuget-csomagok visszaállítása > **NuGet-csomagok kezelése megoldáshoz** > **visszaállítása**.
3. Kattintson a jobb gombbal **MultiChannelToDo.Web** > **Application Insights Telemetria** > **beállításainak konfigurálása** > Módosítás erőforráscsoportot ToDoApp*&lt;your_suffix >* > **Application Insights hozzáadása a projekthez**.
4. Az Azure portál panel megnyitásához a **MultiChannelToDo.Web** Application Insights-erőforrás. Ezt a a **alkalmazás állapotának** része, kattintson a **további tudnivalók a böngészők lapbetöltési adatainak gyűjtéséről** > kód másolása.
5. Adja hozzá a másolt JS instrumentation kódot  *&lt;repository_root >*\src\MultiChannelToDo.Web\app\Index.cshtml, csak a Bezárás előtt `<heading>` címke. Az Application Insights-erőforrás a instrumentation egyedi kulcsot kell tartalmaznia.

        <script type="text/javascript">
        var appInsights=window.appInsights||function(config){
            function s(config){t[config]=function(){var i=arguments;t.queue.push(function(){t[config].apply(t,i)})}}var t={config:config},r=document,f=window,e="script",o=r.createElement(e),i,u;for(o.src=config.url||"//az416426.vo.msecnd.net/scripts/a/ai.0.js",r.getElementsByTagName(e)[0].parentNode.appendChild(o),t.cookie=r.cookie,t.queue=[],i=["Event","Exception","Metric","PageView","Trace"];i.length;)s("track"+i.pop());return config.disableExceptionTracking||(i="onerror",s("_"+i),u=f[i],f[i]=function(config,r,f,e,o){var s=u&&u(config,r,f,e,o);return s!==!0&&t["_"+i](config,r,f,e,o),s}),t
        }({
            instrumentationKey:"<your_unique_instrumentation_key>"
        });

        window.appInsights=appInsights;
        appInsights.trackPageView();
        </script>
6. Küldése egyéni események az Application Insights az egér gombra kell kattintania törzs alján ad hozzá a következő kódot:

       <script>
           $(document.body).find("*").click(function(event) {

               appInsights.trackEvent(event.target.tagName + ": " + event.target.className);
           });
       </script>

   A JavaScript részlet minden alkalommal, amikor a felhasználó kattint bárhol a web app alkalmazásban küld az Application Insights egyéni esemény.
7. A Git rendszerhéj véglegesíteni, és továbbítsa a módosításokat a Githubon az elágazáshoz. Ezután Várjon, amíg az ügyfelek számára, frissítse a böngészőt.

       git add -A :/
       git commit -m "add AI configuration for client app"
       git push origin master
8. A felcserélendő éles üzembe helyezett alkalmazás változásai:

     . \swap – name ToDoApp < your_suffix >
9. Tallózással keresse meg az Application Insights-erőforrás konfigurált. Kattintson az egyéni események.

   ![](./media/app-service-web-test-in-production-controlled-test-flight/01-custom-events.png)

   Ha nem látja az egyéni események metrikákat, várjon néhány percet, és kattintson **frissítése**.

Tegyük fel, hogy látja, a diagram például alatt:

![](./media/app-service-web-test-in-production-controlled-test-flight/02-custom-events-chart-view.png)

És az esemény rács alá:

![](./media/app-service-web-test-in-production-controlled-test-flight/03-custom-event-grid-view.png)

A ToDoApp alkalmazáskód megfelelően a **GOMB** esemény megfelel-e a Küldés gombra, és a **bemeneti** esemény megfelel-e a szövegmező. Az eddigi dolgot értelme. Azonban úgy tűnik, a megfelelő mennyiségű kattintások és nagyon mindössze néhány kattintással a a Tennivalólista elemein (a **LI** események).

Ezzel, akkor űrlap alapján a alapul, hogy egyes felhasználók nem biztos, mely a felhasználói felület része kattintható, és mivel a cursor stílussal a kijelölt szöveg, ha azt a listaelemek és annak ikonja felett áll.

![](./media/app-service-web-test-in-production-controlled-test-flight/04-to-do-list-item-ui.png)

Ez lehet egy contrived példa. Ettől függetlenül fog az alkalmazás javítása, és hajtsa végre a használhatóság visszajelzés lekérése élő ügyfelek flighting központi telepítés.

### <a name="instrument-your-server-app-for-monitoringmetrics"></a>A kiszolgáló alkalmazás figyelési/metrikáihoz állíthatnak
Egy érintő Ez azért, mert a forgatókönyvet, az oktatóanyag egy csak az ügyfélalkalmazás foglalkozik. Azonban a teljesség beállításához nyújt útmutatást a kiszolgálóoldali alkalmazás.

1. Kattintson a jobb gombbal **MultiChannelToDo** > **Application Insights Telemetria** > **beállításainak konfigurálása** > Módosítás erőforráscsoportot ToDoApp*&lt;your_suffix >* > **Application Insights hozzáadása a projekthez**.
2. A Git rendszerhéj véglegesíteni, és továbbítsa a módosításokat a Githubon az elágazáshoz. Ezután Várjon, amíg az ügyfelek számára, frissítse a böngészőt.

       git add -A :/
       git commit -m "add AI configuration for server app"
       git push origin master
3. A felcserélendő éles üzembe helyezett alkalmazás változásai:

     . \swap – name ToDoApp < your_suffix >

Ennyi az egész!

## <a name="investigate-add-slot-specific-tags-to-your-client-app-metrics"></a>Vizsgálja meg: Tárolóhely-specifikus címkék hozzáadása az ügyfél app metrikák
Ebben a szakaszban konfigurálhatja a különböző üzembe helyezési tárhely vonatkozó telemetriai adatokat küldhet ugyanahhoz az Application Insights-erőforráshoz. Ezzel a módszerrel összehasonlíthatja telemetriai adatokat különböző üzembe helyezési ponti (központi telepítési környezetben) a forgalom között könnyen Észreveheti, annak hatását, hogy az alkalmazások változásairól. Egy időben elkülönítheti a termelési forgalmat a többi, továbbra is figyelheti a termelési alkalmazást igény szerint.

Az ügyfél viselkedése az adatgyűjtést van, akkor lesz [adja hozzá a telemetriai adatok inicializáló a JavaScript-kód](../application-insights/app-insights-api-filtering-sampling.md) a index.cshtml. Ha azt szeretné, kiszolgálóoldali teljesítményének a tesztelésére, például is elvégezheti hasonlóan a kiszolgáló kódjában található (lásd: [Application Insights API egyéni események és metrikák](../application-insights/app-insights-api-custom-events-metrics.md).

1. Első lépésként adja meg a kódot bewteen a két `//` a JavaScript az alábbi megjegyzések blokkolása fel ezeket a `<heading>` korábbi címkét.

        window.appInsights = appInsights;

        // Begin new code
        appInsights.queue.push(function () {
            appInsights.context.addTelemetryInitializer(function (envelope) {
                var telemetryItem = envelope.data.baseData;
                telemetryItem.properties = telemetryItem.properties || {};
                telemetryItem.properties["Environment"] = "@System.Configuration.ConfigurationManager.AppSettings["environment"]";
            });
        });
        // End new code

        appInsights.trackPageView();

    Az inicializáló okozza a `appInsights` objektum hozzáadása az egyéni tulajdonság neve `Environment` minden adat, telemetriai adatokat küld a.
2. Ezután adja hozzá az egyéni tulajdonság egy [beállítás tárolóhely](web-sites-staged-publishing.md#AboutConfiguration) a webalkalmazás az Azure-ban. Ehhez futtassa az alábbi parancsokat a Git rendszerhéj-munkamenetben.

        $app = Get-AzureWebsite -Name todoapp<your_suffix> -Slot production
        $app.AppSettings.Add("environment", "Production")
        $app.SlotStickyAppSettingNames.Add("environment")
        $app | Set-AzureWebsite -Name todoapp<your_suffix> -Slot production

    A Web.config fájl, a projekt már határozza meg a `environment` Alkalmazásbeállítás. Ezzel a beállítással az alkalmazást helyileg, tesztelésekor a metrikákat címkével fog rendelkezni a `VS Debugger`. Azonban ha az Azure-bA továbbítsa a módosításokat, Azure lesz megkeresheti, a `environment` alkalmazás inkább a webalkalmazás konfigurációs beállítás, és a metrikákat címkével rendelkező `Production`.
3. Véglegesítse és küldje le a kód módosításokat az elágazáshoz a Githubon, és majd várja meg a felhasználókat, hogy az új alkalmazás (frissítse a böngészőt kell) használatát. Az Application Insights megjelennek az új tulajdonság körülbelül 15 percet vesz igénybe `MultiChannelToDo.Web` erőforrás.

        git add -A :/
        git commit -m "add environment property to AI events for client app"
        git push origin master
4. Most, lépjen a **egyéni események** újra a panelt és a metrikák szűrést végezni `Environment=Production`. Most kell a szűrőt az összes az új egyéni események az éles ponton látni.

    ![](./media/app-service-web-test-in-production-controlled-test-flight/05-filter-on-production-environment.png)
5. Kattintson a **Kedvencek** gombra kattintva mentse az aktuális Metrikaböngésző beállításait hasonlót **egyéni események: éles**. Egyszerűen válthat között ez és egy központi telepítési tárolóhely nézetben később.

   > [!TIP]
   > Még sokkal hatékonyabb elemzés, érdemes lehet [az Application Insights-erőforrás integrálása a Power BI](../application-insights/app-insights-export-power-bi.md).
   >
   >

### <a name="add-slot-specific-tags-to-your-server-app-metrics"></a>A server app metrikákat, tárhely-specifikus címkék hozzáadása
Ebben az esetben a teljesség beállításához nyújt útmutatást a kiszolgálóoldali alkalmazás. Eltérően az ügyfél-alkalmazást, mert a JavaScript tagolva van a tárhely-specifikus címkék a server app tagolva van a .NET-kódot.

1. Nyissa meg  *&lt;repository_root >*\src\MultiChannelToDo\Global.asax.cs. Adja hozzá a lezáró előtt az alábbi kódrészletet névtér kapcsos zárójelet.

        namespace MultiChannelToDo
        {
                ...

                // Begin new code
            public class ConfigInitializer
            : ITelemetryInitializer
            {
                void ITelemetryInitializer.Initialize(ITelemetry telemetry)
                {
                    telemetry.Context.Properties["Environment"] = System.Configuration.ConfigurationManager.AppSettings["environment"];
                }
            }
                // End new code
        }
2. Javítsa ki a név feloldása hibákat hozzáadásával a `using` utasítás alatt elején a fájl:

        using Microsoft.ApplicationInsights.Channel;
        using Microsoft.ApplicationInsights.Extensibility;
3. Adja hozzá az alábbi kódot a elejéhez a `Application_Start()` módszert:

        TelemetryConfiguration.Active.TelemetryInitializers.Add(new ConfigInitializer());
4. Véglegesítse és küldje le a kód módosításokat az elágazáshoz a Githubon, és majd várja meg a felhasználókat, hogy az új alkalmazás (frissítse a böngészőt kell) használatát. Az Application Insights megjelennek az új tulajdonság körülbelül 15 percet vesz igénybe `MultiChannelToDo` erőforrás.

        git add -A :/
        git commit -m "add environment property to AI events for server app"
        git push origin master

## <a name="update-set-up-your-beta-branch"></a>Frissítés: A béta fiókirodai beállítása
1. Nyissa meg  *&lt;repository_root >*\ARMTemplates\ProdAndStagetest.json és a Keresés a `appsettings` erőforrások (keressen `"name": "appsettings"`). Nincsenek 4 egyet mindegyik tárolóhely őket.
2. Az egyes `appsettings` erőforrás, adja hozzá egy `"environment": "[parameters('slotName')]"` Alkalmazásbeállítás végén a `properties` tömb. Ne feledje, vesszővel válassza el az előző sor végére.

    ![](./media/app-service-web-test-in-production-controlled-test-flight/06-arm-app-setting-with-slot-name.png)

    Hozzáadott a `environment` a sablon összes üzembe helyezési ponti Alkalmazásbeállítás.
3. Ugyanebben a fájlban található a `slotconfignames` erőforrások (keressen `"name": "slotconfignames"`). Nincsenek 2 valamelyiket, egy, az egyes alkalmazásokhoz.
4. Az egyes `slotconfignames` erőforrás hozzáadása `"environment"` végének a `appSettingNames` tömb. Ne feledje, vesszővel válassza el az előző sor végére.

    Van szükség a `environment` memóriás beállítást a megfelelő üzembe helyezési pont mindkét alkalmazások app.  
5. A Git rendszerhéj-munkamenetben futtassa a következő parancsokat a azonos erőforrás csoport utótaggal előtt használt.

        git checkout -b beta
        git push origin beta
        .\deploy.ps1 -RepoUrl https://github.com/<your_fork>/ToDoApp.git -ResourceGroupSuffix <your_suffix> -SlotName beta -Branch beta
6. Amikor a rendszer kéri, adja meg az azonos SQL adatbázis-hitelesítő adatok szerint előtt. Az erőforráscsoport frissítése kérdésnél írja be, `Y`, majd `ENTER`.

    Miután a parancsfájl befejeződik, az eredeti erőforráscsoporthoz tartozik, az erőforrások megmaradnak, de új tárhely "béta" nevű hozza létre azt a "Tesztelés" tárolóhely elején a létrehozott azonos konfigurációjú.

   > [!NOTE]
   > Ez a metódus létrehozásának különböző telepítési környezetekben eltér metódust [gyors szoftverfejlesztői az Azure App Service](app-service-agile-software-development.md). Itt hoz létre központi telepítési környezetben üzembe helyezési, ahol is vannak hoz létre központi telepítési környezetekben az erőforráscsoportokhoz. Telepítési környezetekben kezelése az erőforráscsoportokhoz tesz lehetővé az éles környezetben off-limits a fejlesztők számára, de nincs könnyen éles, amelyek könnyen elvégezhető üzembe helyezési ponti tesztelés végrehajtásához.
   >
   >

Ha kívánja, is létrehozhat egy alfa alkalmazás futtatásával

    git checkout -b alpha
    git push origin alpha
    .\deploy.ps1 -RepoUrl https://github.com/<your_fork>/ToDoApp.git -ResourceGroupSuffix <your_suffix> -SlotName beta -Branch alpha

Ehhez az oktatóanyaghoz akkor csak használja a béta-alkalmazást.

## <a name="update-push-your-updates-to-the-beta-app"></a>Frissítés: A frissítések leküldése a béta-alkalmazás
Vissza a az alkalmazás, amelyet javítása érdekében.

1. Ellenőrizze, hogy van-e már a béta fiókirodai

        git checkout beta
2. A  *&lt;repository_root >*\src\MultiChannelToDo.Web\app\Index.cshtml, keresse a `<li>` címkét, és adja hozzá a `style="cursor:pointer"` attribútumot, a lent látható módon.

    ![](./media/app-service-web-test-in-production-controlled-test-flight/07-change-cursor-style-on-li.png)
3. Véglegesítse és leküldéses az Azure-bA.
4. Győződjön meg arról, hogy a módosítás is most megjelenik a béta tárolóhely http://todoapp útvonalon*&lt;your_suffix >*-beta.azurewebsites.net/. Ha még nem látja a módosítást, frissítse a böngészőt, hogy az új javascript-kód beolvasása.

    ![](./media/app-service-web-test-in-production-controlled-test-flight/08-verify-change-in-beta-site.png)

Most, hogy a módosítás a béta tárolóhelye fut, készen áll a flighting központi telepítés végrehajtásához.

## <a name="validate-route-traffic-to-the-beta-app"></a>Ellenőrzése: Irányíthatja a forgalmat a béta-alkalmazásba
Ebben a szakaszban a béta alkalmazásba irányítja a forgalmat. Bemutató átláthatóság érdekében for sake of fog jelentős részét a felhasználói forgalom átirányítása. A valóságban kívánt útválasztó forgalom mennyisége adott helyzettől függ. Például ha a webhely Microsoft.com léptékű, majd szükség lehet a teljes forgalom kevesebb mint egy százalék a hasznos adatok eléréséhez.

1. A Git rendszerhéj-munkamenetben, a béta tárolóhely-hez irányítandó fele a termelési forgalmat a következő parancsokat:

        $siteName = "ToDoApp<your suffix>"
        $rule = New-Object Microsoft.WindowsAzure.Commands.Utilities.Websites.Services.WebEntities.RampUpRule
        $rule.ActionHostName = "$siteName-beta.azurewebsites.net"
        $rule.ReroutePercentage = 50
        $rule.Name = "beta"
        Set-AzureWebsite $siteName -Slot Production -RoutingRules $rule

   A `ReroutePercentage=50` tulajdonság határozza meg, hogy a rendszer 50 %-a termelési forgalom irányítsa a béta-alkalmazás URL-CÍMÉT (által a `ActionHostName` tulajdonság).
2. Most lépjen http://ToDoApp*&lt;your_suffix >*. azurewebsites.net. 50 %-a forgalmat a most már a rendszer átirányítja a béta-tárolóhely.
3. Az Application Insights-erőforrást, a szűrést a metrikák környezet = "béta".

   > [!NOTE]
   > Ha egy másik kedvencként menti a szűrt nézet, majd egyszerűen váltani a metrika explorer nézetek éles tárhely és a béta nézetek között.
   >
   >

Tegyük fel, hogy az Application Insightsban ehhez a következőhöz hasonló:

![](./media/app-service-web-test-in-production-controlled-test-flight/09-test-beta-site-in-production.png)

Nem csak a rendszer Ez azt jelzi, hogy nincsenek-e számos további gombra kell kattintania a a `<li>` címkék, de úgy tűnik, hogy a kattintással megugrását `<li>` címkék. Majd be, hogy személyek felderített az új `<li>` címkék kattintható, és most vannak törlésével a korábban szerzett feladataival az alkalmazásban.

A központi telepítés flighting adatokon alapulnak, dönthet úgy, hogy az üzemi készen áll-e az új felhasználói felületén.

## <a name="go-live-move-your-new-code-into-production"></a>Nyissa meg az élő: helyezze át az új kódot éles környezetben
Most már készen áll a frissítés áthelyezése éles. Mi az az nagy, hogy most már tudja, hogy a frissítés már érvényesítve lett *előtt* üzemi topológiájával. Most magabiztosan telepítheti azt. Az AngularJS ügyfélalkalmazás végzett frissítés, mert az ügyféloldali kódot csak érvényesítve. Ha módosítja a háttér-webes API-alkalmazás, tudta érvényesíteni a módosításokat, hasonlóképpen és könnyen.

1. A Git rendszerhéj távolítsa el a forgalom-útválasztási szabály a következő parancs futtatásával:

        Set-AzureWebsite $siteName -Slot Production -RoutingRules @()
2. A Git-parancsok futtatásával:

        git checkout master
        git pull origin master
        git merge beta
        git push origin master
3. Várjon néhány percet, amíg az új kódot az előkészítési pont üzembe helyezni, majd indítsa el a http://ToDoApp*&lt;your_suffix >*-ellenőrzése, hogy az új frissítés az átmeneti helyet a tárolóhelyspecifikus staging.azurewebsites.net. Ne feledje, hogy a az elágazáshoz főághoz kapcsolódik az alkalmazás az átmeneti helyet.
4. Most felcserélni az átmeneti éles környezetben

        cd <ROOT>\ToDoApp\ARMTemplates
        .\swap.ps1 -Name todoapp<your_suffix>

## <a name="summary"></a>Összefoglalás
Az Azure App Service megkönnyíti a kis - és közepes méretű vállalkozások számára ügyfélkapcsolati alkalmazások tesztelése éles üzemben, valami által hagyományosan elvégzett a nagyvállalatokhoz. Remélhetőleg Ez az oktatóanyag adott Önnek kell összefogni App Service és az Application Insights, hogy a lehetséges flighting telepítési, és akár egyéb tesztelése az éles forgatókönyvek a DevOps világ Tudásbázis.

## <a name="more-resources"></a>További erőforrások
* [Agilis szoftverfejlesztői az Azure App Service](app-service-agile-software-development.md)
* [Átmeneti környezet az Azure App Service web Apps beállítása](web-sites-staged-publishing.md)
* [Kiszámítható módon tudja az Azure-ban egy összetett alkalmazás központi telepítése](app-service-deploy-complex-application-predictably.md)
* [Az Azure Resource Manager sablonok készítése](../azure-resource-manager/resource-group-authoring-templates.md)
* [JSONLint - a JSON-érvényesítő](http://jsonlint.com/)
* [Git ugorhat – alapvető ugorhat és egyesítése](http://www.git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging)
* [Azure PowerShell](/powershell/azure/overview)
* [Projekt Kudu Wiki](https://github.com/projectkudu/kudu/wiki)
