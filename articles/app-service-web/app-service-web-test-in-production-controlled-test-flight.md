---
title: "aaaFlighting deployment (bétatesztelés) az Azure App Service-ben"
description: "Ismerje meg, hogyan tooflight új szolgáltatásokat az alkalmazásban és a béta a frissítések tesztelése az végpont oktatóanyag. Összegyűjti az App Service szolgáltatások, mint a folyamatos közzététel, tárhelyek, a forgalom útválasztásához és az Application Insights-integráció."
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
ms.openlocfilehash: e83477b1fe46be09e5baa7bc2bd239b840b05cf7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="flighting-deployment-beta-testing-in-azure-app-service"></a>Flighting deployment (bétatesztelés) az Azure App Service-ben
Az oktatóanyag bemutatja, hogyan toodo *flighting központi telepítések* integrálásával hello különböző lehetőségeinek [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) és [Azure Application Insights](/services/application-insights/).

*Flighting* egy folyamatot, amely egy új szolgáltatás, vagy módosítsa a valódi felhasználók korlátozott számú ellenőrzi, és egy fő tesztelése éles telepítési forgatókönyvhöz. Akin toobeta tesztelése, és mint "ellenőrzött teszt repülési" néven is ismert. Egy webes jelenlét sok nagy vállalatok ezt a módszert használja az app frissítéseken a gyakorlatban a korai érvényesítési eléréséhez [gyors fejlesztési](https://en.wikipedia.org/wiki/Agile_software_development). Az Azure App Service lehetővé teszi toointegrate teszt termelési környezetben, a folyamatos közzététel, és az Application Insights tooimplement hello azonos DevOps-forgatókönyv. Ez a módszer előnyei a következők:

* **Valós visszajelzés szerezhet *előtt* frissítései kiadott tooproduction** -hello csak jobb, mint való visszajelzést, amint felszabadít dolog van való visszajelzést, engedélyezés előtt. A felhasználó forgalmi és viselkedéshez frissítések korai biztosítják a hello életciklusa tesztelheti.
* **Javíthatja [folyamatos teszt adatvezérelt fejlesztési (CTDD)](https://en.wikipedia.org/wiki/Continuous_test-driven_development)**  - teszt termelési környezetben és integrálásával folyamatos integrációt és az Application insights szolgáltatással instrumentation felhasználói érvényesítési korai és automatikusan történik, a termék életciklusának. Ezzel csökkenthető manuális tesztelési végrehajtási idő befektetések.
* **Teszt munkafolyamat optimalizálása** -teszt termelési környezetben, a folyamatos figyelési instrumentation automatizálásával potenciálisan végrehajtható tesztek egyetlen folyamatban, a különféle hello céljai például [integrációs](https://en.wikipedia.org/wiki/Integration_testing), [regressziós](https://en.wikipedia.org/wiki/Regression_testing), [használhatóság](https://en.wikipedia.org/wiki/Usability_testing), kisegítő, honosítási, [teljesítmény](https://en.wikipedia.org/wiki/Software_performance_testing), [biztonsági](https://en.wikipedia.org/wiki/Security_testing), és [ elfogadási](https://en.wikipedia.org/wiki/Acceptance_testing).

Egy flighting telepítési szinte nem útválasztási élő forgalmat. Ilyen elrendezés kívánt toogain betekintést lehető leggyorsabban tegye, hogy egy nem várt hiba, a teljesítménycsökkenés, a felhasználói élmény problémák. Ne feledje, hogy valós ügyfelek foglalkoznak. Igen toodo, jobbra, meg kell győződnie arról, hogy meg van adva a flighting telepítési toogather összes hello adatokat meg kell a rendezés toomake tájékozott döntést a következő lépéshez. Ez az oktatóanyag toocollect adatok az Application Insights, de új New Relic vagy egyéb technológiák a forgatókönyvéhez leginkább megfelelő használatát.

## <a name="what-you-will-do"></a>Mit fog
Ebből az oktatóanyagból megtudhatja, hogyan toobring App Service-alkalmazás, éles környezetben a következő forgatókönyvek együtt tootest hello:

* [Éles forgalmat](app-service-web-test-in-production-get-start.md) tooyour beta alkalmazás
* [Az alkalmazás állíthatnak](../application-insights/app-insights-web-track-usage.md) tooobtain hasznos metrikákat
* Folyamatosan a béta-alkalmazás telepítése, és nyomon követheti az élő app metrikák
* Hasonlítsa össze metrikák hello éles alkalmazások és hello beta app toosee hogyan változik a kód között tooresults fordítása

## <a name="what-you-will-need"></a>Mit kell
* Az Azure-fiók
* A [GitHub](https://github.com/) fiók
* A Visual Studio 2015 - letöltheti a hello [Community edition](https://www.visualstudio.com/en-us/products/visual-studio-express-vs.aspx).
* Git rendszerhéj (telepített [GitHub for Windows](https://windows.github.com/)) – Ez lehetővé teszi, hogy Ön toorun hello mindkét hello a Git szoftver, PowerShell parancsok ugyanabban a munkamenetben
* Legújabb [Azure PowerShell](https://github.com/Azure/azure-powershell/releases/download/v0.9.8-September2015/azure-powershell.0.9.8.msi) bits
* Hello következő alapvető ismeretekkel:
  * [Az Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) sablon-üzembehelyezés (lásd: [kiszámítható módon tudja az Azure-ban összetett alkalmazást központilag](app-service-deploy-complex-application-predictably.md))
  * [Git](http://git-scm.com/documentation)
  * [PowerShell](https://technet.microsoft.com/library/bb978526.aspx)

> [!NOTE]
> Ez az oktatóanyag kell egy Azure-fiók toocomplete:
>
> * Is [szabad nyissa meg az Azure-fiók](https://azure.microsoft.com/pricing/free-trial/) -jóváírásokat kap használhatja ki tootry fizetős Azure-szolgáltatásokkal, és még azok lejárta után hálózati adaptere esetében megtarthatja hello fiókot és használhatja az ingyenes Azure-szolgáltatások, például a Web Apps.
> * Is [aktiválhatja a Visual Studio előfizetői előnyeit](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) -a Visual Studio-előfizetéssel biztosít Önnek krediteket minden hónapban, fizetős Azure-szolgáltatásokat is használhat.
>
> Ha azt szeretné, hogy az az Azure-fiók regisztrálása előtt az Azure App Service lépései tooget, nyissa meg túl[App Service kipróbálása](https://azure.microsoft.com/try/app-service/), ahol azonnal létrehozhat egy rövid élettartamú alapszintű webalkalmazást az App Service-ben. Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.
>
>

## <a name="set-up-your-production-web-app"></a>Az éles webes alkalmazás beállítása
> [!NOTE]
> Ebben az oktatóanyagban használt hello parancsfájl automatikusan konfigurálja a GitHub-tárház folyamatos közzététel. Ehhez az szükséges, hogy a Githubon tartozó hitelesítő adatok már szerepelnek Azure, ellenkező esetben a hello parancsprogram-telepítés sikertelen lesz-e tooconfigure verziókövetési beállítások hello web Apps megkísérlése során.
>
> a GitHub-felhasználó hitelesítő adatait az Azure toostore egy webalkalmazás létrehozása az hello [Azure Portal](https://portal.azure.com/) és [GitHub-KözpontiTelepítés konfigurálása](app-service-continuous-deployment.md). Csak akkor kell toodo ezen egyszer.
>
>

Egy tipikus DevOps forgatókönyv valamely alkalmazás az élő Azure-ban futó, és azt szeretné, hogy toomake módosítások tooit folyamatos közzétételen keresztül;. Ebben a forgatókönyvben a tooproduction egy sablon, amelyet kifejlesztett és tesztelt telepíti.

1. Hozzon létre saját elágazás a hello [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) tárházba. Létrehozásával kapcsolatos információkat az elágazáshoz, tekintse meg a [oszthatja ketté a tárházban](https://help.github.com/articles/fork-a-repo/). Ha létrejött az elágazáshoz, tekintheti meg a böngészőben.

   ![](./media/app-service-agile-software-development/production-1-private-repo.png)
2. Nyisson meg egy Git rendszerhéj-munkamenetet. Ha a Git rendszerhéj még nincs, telepítse [GitHub for Windows](https://windows.github.com/) most.
3. Az elágazáshoz, helyi másolat létrehozása a következő hello a következő parancs futtatásával:

     git-klón https://github.com/<your_fork>/ToDoApp.git
4. Ha elvégezte a helyi klónozás, keresse meg túl*&lt;repository_root >*\ARMTemplates és futtatási hello deploy.ps1 egyedi utótagot kapjon, ahogy az alábbi parancsfájlt:

     .\deploy.ps1 – RepoUrl https://github.com/<your_fork>/todoapp.git - ResourceGroupSuffix < your_suffix >
5. Amikor a rendszer kéri, írja be hello szükséges felhasználónevet és jelszót az adatbázis eléréséhez. Ne feledje, az adatbázis hitelesítő adatait, mert toospecify kell őket ismét amikor frissítése hello erőforráscsoportot.

   Meg kell jelennie a jogosultságkiosztás folyamata különböző Azure-erőforrások hello. Központi telepítés befejezése után hello parancsfájl hello böngészőben hello alkalmazást, és adjon meg egy rövid hangjelzés.
   ![](./media/app-service-web-test-in-production-controlled-test-flight/00.1-app-in-browser.png)
6. Vissza a Git rendszerhéj-munkamenetben futtassa:

     . \swap – name ToDoApp < your_suffix >

   ![](./media/app-service-web-test-in-production-controlled-test-flight/00.2-swap-to-production.png)
7. Hello parancsfájl befejezése után térjen vissza toobrowse toohello előtér-cím (http://ToDoApp*&lt;your_suffix >*.azurewebsites.net/) toosee hello alkalmazást éles környezetben.
8. Jelentkezzen be a hello [Azure Portal](https://portal.azure.com/) és vessen egy pillantást mi jön létre.

   Meg kell tudni toosee két webes alkalmazások hello ugyanabban az erőforráscsoportban, amelyen a hello `Api` utótag hello nevében. Hello erőforráscsoport nézet tekinti meg, ha hello SQL-adatbázis és a kiszolgáló, a hello App Service-csomag és a hello átmeneti üzembe helyezési ponti hello web Apps is láthat. Tallózzon a hello különböző erőforrások, és hasonlítsa össze azokat a  *&lt;repository_root >*\ARMTemplates\ProdAndStage.json toosee konfigurációjuktól hello sablonban.

   ![](./media/app-service-web-test-in-production-controlled-test-flight/00.3-resource-group-view.png)

Hello éles alkalmazások beállítása.  Most tegyük képzelhető el, hogy megjelenik-e visszajelzést, hogy gyenge hello alkalmazáshoz-e használhatóság. Ezért úgy dönt, hogy tooinvestigate. Fog tooinstrument az alkalmazás toogive Ön visszajelzését.

## <a name="investigate-instrument-your-client-app-for-monitoringmetrics"></a>Vizsgálja meg: Állíthatnak be az ügyfél alkalmazás figyelési/metrikák
1. Nyissa meg  *&lt;repository_root >*\src\MultiChannelToDo.sln a Visual Studióban.
2. Kattintson a jobb gombbal a megoldás Nuget-csomagok visszaállítása > **NuGet-csomagok kezelése megoldáshoz** > **visszaállítása**.
3. Kattintson a jobb gombbal **MultiChannelToDo.Web** > **Application Insights Telemetria** > **beállításainak konfigurálása** > erőforrás-csoport módosítása tooToDoApp*&lt;your_suffix >* > **Application Insights hozzáadása tooProject**.
4. Hello Azure portál, nyissa meg hello hello paneljén **MultiChannelToDo.Web** Application Insights-erőforrás. Ezt a hello **alkalmazás állapotának** része, kattintson a **megtudhatja, hogyan toocollect böngésző betöltési adatok** > kód másolása.
5. Adja hozzá a hello másolta JS instrumentation túl*&lt;repository_root >*\src\MultiChannelToDo.Web\app\Index.cshtml hello bezárása előtt `<heading>` címke. Az Application Insights-erőforrás hello instrumentation egyedi kulcsot kell tartalmaznia.

        <script type="text/javascript">
        var appInsights=window.appInsights||function(config){
            function s(config){t[config]=function(){var i=arguments;t.queue.push(function(){t[config].apply(t,i)})}}var t={config:config},r=document,f=window,e="script",o=r.createElement(e),i,u;for(o.src=config.url||"//az416426.vo.msecnd.net/scripts/a/ai.0.js",r.getElementsByTagName(e)[0].parentNode.appendChild(o),t.cookie=r.cookie,t.queue=[],i=["Event","Exception","Metric","PageView","Trace"];i.length;)s("track"+i.pop());return config.disableExceptionTracking||(i="onerror",s("_"+i),u=f[i],f[i]=function(config,r,f,e,o){var s=u&&u(config,r,f,e,o);return s!==!0&&t["_"+i](config,r,f,e,o),s}),t
        }({
            instrumentationKey:"<your_unique_instrumentation_key>"
        });

        window.appInsights=appInsights;
        appInsights.trackPageView();
        </script>
6. A kattintások tooApplication Insights az egyéni események küldése adja hozzá a következő kód toohello alsó törzse hello:

       <script>
           $(document.body).find("*").click(function(event) {

               appInsights.trackEvent(event.target.tagName + ": " + event.target.className);
           });
       </script>

   A JavaScript részlet küld egy egyéni esemény tooApplication Insights minden alkalommal, amikor a felhasználó kattint bárhol hello web app alkalmazásban.
7. Git rendszerhéj véglegesítse, és küldje le a változások tooyour elágazás a Githubon. Várja meg, majd az ügyfelek toorefresh böngésző.

       git add -A :/
       git commit -m "add AI configuration for client app"
       git push origin master
8. Lapozófájl-kapacitás hello telepített alkalmazás módosítások tooproduction:

     . \swap – name ToDoApp < your_suffix >
9. Keresse meg a konfigurált toohello Application Insights-erőforrást. Kattintson az egyéni események.

   ![](./media/app-service-web-test-in-production-controlled-test-flight/01-custom-events.png)

   Ha nem látja az egyéni események metrikákat, várjon néhány percet, és kattintson **frissítése**.

Tegyük fel, hogy látja, a diagram például alatt:

![](./media/app-service-web-test-in-production-controlled-test-flight/02-custom-events-chart-view.png)

És hello esemény rács alá:

![](./media/app-service-web-test-in-production-controlled-test-flight/03-custom-event-grid-view.png)

Hello tooyour ToDoApp alkalmazáskód szerint, **GOMB** esemény megfelel toohello Küldés gomb és hello **bemeneti** esemény toohello szövegmező felel meg. Az eddigi dolgot értelme. Azonban úgy tűnik, a megfelelő mennyiségű kattintások és nagyon mindössze néhány kattintással a Tennivalólista elemein hello (hello **LI** események).

Alapú az űrlapon, akkor a alapul, hogy egyes felhasználók nem biztos, mely része hello felhasználói felület kattintható és mivel az a kijelölt szöveg stílussal hello kurzor, amikor az hello listaelemek és annak ikonja felett áll.

![](./media/app-service-web-test-in-production-controlled-test-flight/04-to-do-list-item-ui.png)

Ez lehet egy contrived példa. Ettől függetlenül folyamatos toomake fokozása tooyour alkalmazást, és végezze el a központi telepítési tooget flighting használhatóság visszajelzés élő ügyfelek.

### <a name="instrument-your-server-app-for-monitoringmetrics"></a>A kiszolgáló alkalmazás figyelési/metrikáihoz állíthatnak
Egy érintő Ez azért, mert az oktatóanyag egy hello forgatókönyv csak hello ügyfélalkalmazás foglalkozik. Azonban a teljesség beállíthat hello kiszolgálóoldali alkalmazás.

1. Kattintson a jobb gombbal **MultiChannelToDo** > **Application Insights Telemetria** > **beállításainak konfigurálása** > erőforrás-csoport módosítása tooToDoApp*&lt;your_suffix >* > **Application Insights hozzáadása tooProject**.
2. Git rendszerhéj véglegesítse, és küldje le a változások tooyour elágazás a Githubon. Várja meg, majd az ügyfelek toorefresh böngésző.

       git add -A :/
       git commit -m "add AI configuration for server app"
       git push origin master
3. Lapozófájl-kapacitás hello telepített alkalmazás módosítások tooproduction:

     . \swap – name ToDoApp < your_suffix >

Ennyi az egész!

## <a name="investigate-add-slot-specific-tags-tooyour-client-app-metrics"></a>Vizsgálata: Tárolóhely-specifikus címkék tooyour ügyfél app metrikák hozzáadása
Ebben a szakaszban hello különböző telepítési üzembe helyezési ponti toosend tárolóhely-specifikus telemetriai toohello konfigurálja ugyanazon Application Insights-erőforrást. Ezzel a módszerrel összehasonlíthatja a telemetriai adatok között különböző üzembe helyezési ponti (telepítési környezetekben) tooeasily forgalmát, lásd: hello hatását, hogy az alkalmazások változásairól. At hello azonos időben, elkülönítheti hello éles forgalom a hello rest így is toomonitor az éles alkalmazás igény szerint.

Az ügyfél viselkedése az adatgyűjtést van, akkor lesz [adja hozzá a telemetriai adatok inicializáló tooyour JavaScript-kód](../application-insights/app-insights-api-filtering-sampling.md) a index.cshtml. Ha azt szeretné, hogy tootest kiszolgálóoldali teljesítményét, például is elvégezheti hasonlóan a kiszolgáló kódjában található (lásd: [Application Insights API egyéni események és metrikák](../application-insights/app-insights-api-custom-events-metrics.md).

1. Először adja hozzá a hello kód bewteen hello két `//` Megjegyzések alatt hello JavaScript blokkban toohello hozzáadásának `<heading>` korábbi címkét.

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

    Az inicializáló hello okozza `appInsights` objektum tooadd hello nevű egyéni tulajdonságot `Environment` tooevery adat, telemetriai adatokat küldi el.
2. Ezután adja hozzá az egyéni tulajdonság egy [beállítás tárolóhely](web-sites-staged-publishing.md#AboutConfiguration) a webalkalmazás az Azure-ban. toodo, a parancsok a Git rendszerhéj-munkamenetben a következő futtatási hello.

        $app = Get-AzureWebsite -Name todoapp<your_suffix> -Slot production
        $app.AppSettings.Add("environment", "Production")
        $app.SlotStickyAppSettingNames.Add("environment")
        $app | Set-AzureWebsite -Name todoapp<your_suffix> -Slot production

    a projekt Web.config hello már meghatározása hello `environment` Alkalmazásbeállítás. Ezzel a beállítással hello alkalmazást helyileg, tesztelésekor a metrikákat címkével fog rendelkezni a `VS Debugger`. Azonban, amikor a módosítások tooAzure, Azure lesz megkeresheti, hello `environment` alkalmazás hello web app konfigurációban beállítást használja, és a metrikákat címkével rendelkező `Production`.
3. Véglegesítse és a kód módosítások tooyour elágazás leküldéses a Githubon, és majd várja meg a felhasználók toouse hello új app (kell toorefresh hello böngésző). Ez körülbelül 15 percet vesz igénybe hello új tulajdonság tooshow az Application insightsban `MultiChannelToDo.Web` erőforrás.

        git add -A :/
        git commit -m "add environment property tooAI events for client app"
        git push origin master
4. Most, lépjen a toohello **egyéni események** újra a panelt és szűrő hello metrikákat a `Environment=Production`. Minden hello új egyéni események hello éles környezetben ez a szűrő a tárolóhely képes toosee kell.

    ![](./media/app-service-web-test-in-production-controlled-test-flight/05-filter-on-production-environment.png)
5. Kattintson a hello **Kedvencek** gomb toosave hello aktuális Metrikaböngésző beállításait toosomething, például **egyéni események: éles**. Egyszerűen válthat között ez és egy központi telepítési tárolóhely nézetben később.

   > [!TIP]
   > Még sokkal hatékonyabb elemzés, érdemes lehet [az Application Insights-erőforrás integrálása a Power BI](../application-insights/app-insights-export-power-bi.md).
   >
   >

### <a name="add-slot-specific-tags-tooyour-server-app-metrics"></a>Tárolóhely-specifikus címkék tooyour kiszolgálói alkalmazás metrikák hozzáadása
Ebben az esetben a teljesség beállíthat hello kiszolgálóoldali alkalmazás. Hello ügyfélalkalmazást, amely a JavaScript van tagolva, eltérően hello server alkalmazás tárolóhely-specifikus címkék .NET kóddal van tagolva.

1. Nyissa meg  *&lt;repository_root >*\src\MultiChannelToDo\Global.asax.cs. Adja hozzá a hello kódblokkot az alábbi hello névtér kapcsos zárójelet bezárása előtt.

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
2. Javítsa ki a hello név feloldása hibákat hello hozzáadásával `using` hello fájl alábbi toohello kezdete utasításokat:

        using Microsoft.ApplicationInsights.Channel;
        using Microsoft.ApplicationInsights.Extensibility;
3. Adja hozzá hello kódot alább hello toohello elejére `Application_Start()` módszert:

        TelemetryConfiguration.Active.TelemetryInitializers.Add(new ConfigInitializer());
4. Véglegesítse és a kód módosítások tooyour elágazás leküldéses a Githubon, és majd várja meg a felhasználók toouse hello új app (kell toorefresh hello böngésző). Ez körülbelül 15 percet vesz igénybe hello új tulajdonság tooshow az Application insightsban `MultiChannelToDo` erőforrás.

        git add -A :/
        git commit -m "add environment property tooAI events for server app"
        git push origin master

## <a name="update-set-up-your-beta-branch"></a>Frissítés: A béta fiókirodai beállítása
1. Nyissa meg  *&lt;repository_root >*\ARMTemplates\ProdAndStagetest.json és a keresés hello `appsettings` erőforrások (keressen `"name": "appsettings"`). Nincsenek 4 egyet mindegyik tárolóhely őket.
2. Az egyes `appsettings` erőforrás hozzáadása egy `"environment": "[parameters('slotName')]"` app beállítás toohello vége hello `properties` tömb. Ne feledje tooend hello előző sor vesszővel válassza el.

    ![](./media/app-service-web-test-in-production-controlled-test-flight/06-arm-app-setting-with-slot-name.png)

    Hello hozzáadott `environment` hello üzembe helyezési ponti tooall beállítása hello sablon az alkalmazást.
3. A hello ugyanazt a fájlt, megkeresi hello `slotconfignames` erőforrások (keressen `"name": "slotconfignames"`). Nincsenek 2 valamelyiket, egy, az egyes alkalmazásokhoz.
4. Az egyes `slotconfignames` erőforrás hozzáadása `"environment"` hello toohello végét `appSettingNames` tömb. Ne feledje tooend hello előző sor vesszővel válassza el.

    Van szükség hello `environment` app beállítás memóriás tooits megfelelő üzembe helyezési pont mindkét alkalmazásokhoz.  
5. A Git rendszerhéj-munkamenetben futtassa hello következő parancsok hello azonos erőforrás csoport utótag előtt használt.

        git checkout -b beta
        git push origin beta
        .\deploy.ps1 -RepoUrl https://github.com/<your_fork>/ToDoApp.git -ResourceGroupSuffix <your_suffix> -SlotName beta -Branch beta
6. Amikor a rendszer kéri, adja meg, mint korábban hello azonos SQL-adatbázis hitelesítő adatait. Ezt követően amikor ismételt tooupdate hello erőforráscsoport, típus `Y`, majd `ENTER`.

    Miután hello parancsfájl befejeződik, hello eredeti erőforráscsoporthoz tartozik, az erőforrások megmaradnak, de "béta" nevű új tárhely hozza létre azt hello azonos hello "Tesztelés" tárolóhely hello elején a létrehozott konfigurációt.

   > [!NOTE]
   > Ez a metódus létrehozásának különböző telepítési környezetekben eltér a hello metódus [gyors szoftverfejlesztői az Azure App Service](app-service-agile-software-development.md). Itt hoz létre központi telepítési környezetben üzembe helyezési, ahol is vannak hoz létre központi telepítési környezetekben az erőforráscsoportokhoz. Telepítési környezetekben kezelése az erőforráscsoportok lehetővé teszi tookeep hello éles környezetben off-limits toodevelopers, de nincs tesztelése éles, amelyek könnyen elvégezhető tárolóhelyek könnyen toodo.
   >
   >

Ha kívánja, is létrehozhat egy alfa alkalmazás futtatásával

    git checkout -b alpha
    git push origin alpha
    .\deploy.ps1 -RepoUrl https://github.com/<your_fork>/ToDoApp.git -ResourceGroupSuffix <your_suffix> -SlotName beta -Branch alpha

Ehhez az oktatóanyaghoz akkor csak használja a béta-alkalmazást.

## <a name="update-push-your-updates-toohello-beta-app"></a>Frissítés: A frissítések toohello beta app leküldéses
Biztonsági másolatot, amelyet az tooimprove tooyour alkalmazást.

1. Ellenőrizze, hogy van-e már a béta fiókirodai

        git checkout beta
2. A  *&lt;repository_root >*\src\MultiChannelToDo.Web\app\Index.cshtml, a keresés hello `<li>` címkét, és adja hozzá a hello `style="cursor:pointer"` attribútum, a lent látható módon.

    ![](./media/app-service-web-test-in-production-controlled-test-flight/07-change-cursor-style-on-li.png)
3. Véglegesítse és leküldéses tooAzure.
4. Győződjön meg arról, hogy hello módosítás is most megjelenik hello beta tárolóhely toohttp://todoapp navigálva*&lt;your_suffix >*-beta.azurewebsites.net/. Ha nem lát hello módosítása, de a böngésző tooget hello új javascript-kód frissítése.

    ![](./media/app-service-web-test-in-production-controlled-test-flight/08-verify-change-in-beta-site.png)

Most, hogy a módosítás hello beta tárolóhelye fut, készen áll a tooperform flighting telepítési áll.

## <a name="validate-route-traffic-toohello-beta-app"></a>Ellenőrzése: Útvonal forgalom toohello beta app
Ebben a szakaszban a forgalom toohello beta app irányítja. Bemutató átláthatóság érdekében for sake of fog tooroute hello felhasználói forgalom tooit jelentős részét. A valóságban hello kívánt tooroute függ az adott helyzethez forgalom mennyisége. Például ha a webhely Microsoft.com hello léptékű, majd szükség lehet a teljes forgalom rendelés toogain hasznos adataiban kevesebb mint egy százalék.

1. A Git rendszerhéj-munkamenetben futtassa a következő parancsok tooroute hello hello éles forgalom toohello beta tárolóhelyre felének:

        $siteName = "ToDoApp<your suffix>"
        $rule = New-Object Microsoft.WindowsAzure.Commands.Utilities.Websites.Services.WebEntities.RampUpRule
        $rule.ActionHostName = "$siteName-beta.azurewebsites.net"
        $rule.ReroutePercentage = 50
        $rule.Name = "beta"
        Set-AzureWebsite $siteName -Slot Production -RoutingRules $rule

   Hello `ReroutePercentage=50` tulajdonság határozza meg, hogy hello éles forgalom 50 %-át lesznek-e irányított toohello beta alkalmazás URL-CÍMÉT (hello által megadott `ActionHostName` tulajdonság).
2. Most lépjen toohttp://ToDoApp*&lt;your_suffix >*. azurewebsites.net. hello forgalom 50 %-át kell átirányított toohello beta tárolóhely.
3. Az Application Insights-erőforrást, a szűrést hello metrikák környezet = "béta".

   > [!NOTE]
   > Ha egy másik kedvencként menti a szűrt nézet, majd egyszerűen váltani hello metrika explorer nézetek éles tárhely és a béta nézetek között.
   >
   >

Tegyük fel, hogy az Application Insightsban valami hasonló toohello következő látható:

![](./media/app-service-web-test-in-production-controlled-test-flight/09-test-beta-site-in-production.png)

Nem csak a rendszer Ez azt jelzi, hogy vannak-e a hello számos további kattintással `<li>` címkék, de úgy tűnik, toobe kattintással megugrását a `<li>` címkék. Majd be, hogy személyek hello új felderített `<li>` címkék kattintható, és most vannak törlésével a korábban szerzett feladataival hello alkalmazásban.

A központi telepítés flighting hello adatok alapján, dönthet úgy, hogy az üzemi készen áll-e az új felhasználói felületén.

## <a name="go-live-move-your-new-code-into-production"></a>Nyissa meg az élő: helyezze át az új kódot éles környezetben
Ön éppen most már készen áll a toomove a frissítés tooproduction. Mi az az nagy, hogy most már tudja, hogy a frissítés már érvényesítve lett *előtt* tooproduction topológiájával. Most magabiztosan telepítheti azt. Végrehajtott frissítés toohello AngularJS ügyfelet alkalmazást, mert csak érvényesítve hello ügyféloldali kódot. Ha toomake módosítások toohello háttér-webes API-alkalmazás, érvényesíthető a módosításokat, hasonlóképpen és könnyen.

1. Git rendszerhéj távolítsa el a hello forgalom-útválasztási szabály hello a következő parancs futtatásával:

        Set-AzureWebsite $siteName -Slot Production -RoutingRules @()
2. Hello Git-parancsok futtatásával:

        git checkout master
        git pull origin master
        git merge beta
        git push origin master
3. Várjon néhány percig, hello új tárhely átmeneti telepített toobe toohello code, majd indítsa el a http://ToDoApp*&lt;your_suffix >*-staging.azurewebsites.net tooverify, amely az új frissítés hello hello átmenetiként tárolóhelyspecifikus tárolóhely. Ne feledje, hogy az elágazáshoz fő ág hello kapcsolódó toohello átmeneti tárolóhely az alkalmazás.
4. Most a tárhely átmeneti éles környezetben hello felcserélése

        cd <ROOT>\ToDoApp\ARMTemplates
        .\swap.ps1 -Name todoapp<your_suffix>

## <a name="summary"></a>Összefoglalás
Az Azure App Service megkönnyíti toomedium-a kisméretű vállalkozások tootest ügyfélkapcsolati alkalmazások éles, valamit a nagyvállalatokhoz hagyományosan elvégzett. Remélhetőleg, ez az oktatóanyag adott Tudásbázis toobring együtt az App Service és az Application Insights toomake lehetséges flighting telepítési, és akár egyéb tesztelése az éles forgatókönyvek szüksége a DevOps world hello.

## <a name="more-resources"></a>További erőforrások
* [Agilis szoftverfejlesztői az Azure App Service](app-service-agile-software-development.md)
* [Átmeneti környezet az Azure App Service web Apps beállítása](web-sites-staged-publishing.md)
* [Kiszámítható módon tudja az Azure-ban egy összetett alkalmazás központi telepítése](app-service-deploy-complex-application-predictably.md)
* [Az Azure Resource Manager sablonok készítése](../azure-resource-manager/resource-group-authoring-templates.md)
* [JSONLint - hello JSON-érvényesítő](http://jsonlint.com/)
* [Git ugorhat – alapvető ugorhat és egyesítése](http://www.git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging)
* [Azure PowerShell](/powershell/azure/overview)
* [Projekt Kudu Wiki](https://github.com/projectkudu/kudu/wiki)
