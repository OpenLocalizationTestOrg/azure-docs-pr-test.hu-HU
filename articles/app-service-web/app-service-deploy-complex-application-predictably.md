---
title: "aaaProvision és kiszámítható módon tudja az Azure-ban mikroszolgáltatások telepítése"
description: "Ismerje meg, hogyan toodeploy az alkalmazás összetevői mikroszolgáltatások létrehozására az Azure App Service egy egységként és kiszámítható módon JSON erőforrás csoport sablonok és a PowerShell-parancsfájlok használatával."
services: app-service
documentationcenter: 
author: cephalin
manager: erikre
editor: jimbe
ms.assetid: bb51e565-e462-4c60-929a-2ff90121f41d
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/06/2016
ms.author: cephalin
ms.openlocfilehash: d32c2fc82a8b09e89224ec437e5819b65b2e9e78
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="provision-and-deploy-microservices-predictably-in-azure"></a>Kiosztását és telepítését mikroszolgáltatások kiszámítható módon tudja az Azure-ban
Ez az oktatóanyag bemutatja, hogyan tooprovision és telepítsen egy alkalmazást álló [mikroszolgáltatások](https://en.wikipedia.org/wiki/Microservices) a [Azure App Service](/services/app-service/) egyetlen egységként és JSON-erőforrás csoport sablonokkal kiszámítható módon és PowerShell-parancsprogramok. 

Kiépítés, és jelentősen méretezhető alkalmazások épülnek, a magas leválasztott mikroszolgáltatások létrehozására, ismételhetőség és kiszámíthatóságot kritikus fontosságú toosuccess. [Az Azure App Service](/services/app-service/) lehetővé teszi a webalkalmazások, mobilalkalmazások, az API apps és a logic apps tartalmazó toocreate mikroszolgáltatások létrehozására. [Az Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) lehetővé teszi, hogy Ön toomanage összes hello mikroszolgáltatások egységként, erőforrás-függőségek, például az adatbázis és a forrás-vezérlési beállításokkal együtt. Most ilyen alkalmazás JSON sablonok és egyszerű PowerShell-parancsfájlok használatával is telepítheti. 

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="what-you-will-do"></a>Mit fog
A hello oktatóanyagban fogja központilag telepíteni az alkalmazást, amely tartalmazza:

* Két webes alkalmazások (pl. két mikroszolgáltatások)
* A háttérrendszer SQL-adatbázis
* Alkalmazásbeállítások kapcsolati karakterláncok és adatforrás-vezérlő
* Az Application insights, riasztások, automatikus skálázás beállításai

## <a name="tools-you-will-use"></a>Eszközök fogja használni
Ebben az oktatóanyagban a következő eszközök hello fogja használni. Mivel ez nem az eszközök átfogó leírást, I toostick toohello végpont forgatókönyv kattintok, és csak akkor adjon egy rövid bevezető tooeach és hol találhatók további információk. 

### <a name="azure-resource-manager-templates-json"></a>Az Azure Resource Manager-sablonokat (JSON)
Minden alkalommal, amikor a webalkalmazás létrehozása az Azure App Service-ben, például Azure Resource Manager használ a JSON sablon toocreate hello teljes erőforráscsoport hello összetevő-erőforrások. Egy összetett sablon hello [Azure piactér](/marketplace) például hello [méretezhető WordPress](/marketplace/partners/wordpress/scalablewordpress/) app lehetnek hello MySQL-adatbázis, a storage-fiókok, a hello App Service-csomag, a hello webalkalmazás magát, a riasztási szabályok, az alkalmazás automatikus skálázási beállításokat, és további, és az összes ezen sablonok elérhető tooyou Powershellen keresztül. Hogyan toodownload és használja ezeket a sablonokat: kapcsolatos [az Azure PowerShell használata Azure Resource Managerrel](../powershell-azure-resource-manager.md).

Hello Azure Resource Manager-sablonok további információkért lásd: [Azure Resource Manager sablonok készítése](../azure-resource-manager/resource-group-authoring-templates.md)

### <a name="azure-sdk-26-for-visual-studio"></a>Az Azure SDK 2.6 Visual Studio
hello legújabb SDK tartalmaz fejlesztéseket toohello Resource Manager sablon támogatási hello JSON-szerkesztőben. Használhatja a tooquickly egy erőforrás-sablon létrehozása új vagy nyissa meg a meglévő JSON-sablon (például egy gyűjtemény letöltött sablon) módosításra, hello paraméterek fájl feltöltéséhez, és még akkor is telepíteni hello erőforráscsoportot az Azure-ről Erőforráscsoport megoldás.

További információkért lásd: [Azure SDK 2.6 a Visual Studio](https://azure.microsoft.com/blog/2015/04/29/announcing-the-azure-sdk-2-6-for-net/).

### <a name="azure-powershell-080-or-later"></a>Az Azure PowerShell 0.8.0 vagy újabb verzió
0.8.0 verziójától kezdve hello Azure PowerShell telepítése tartalmazza hello Azure Resource Manager modul hozzáadása toohello Azure modul. Ez a modul lehetővé teszi az erőforráscsoportok tooscript hello központi telepítés.

További információkért lásd: [az Azure PowerShell használata Azure Resource Managerrel](../powershell-azure-resource-manager.md)

### <a name="azure-resource-explorer"></a>Azure Resource Explorer
Ez [preview eszköz](https://resources.azure.com) lehetővé teszi az előfizetés és hello egyes erőforrások összes hello erőforráscsoport tooexplore hello JSON definícióit. Hello eszközében hello JSON-definíciók erőforrás szerkesztése, az egész hierarchiát erőforrások törlése, és hozzon létre új erőforrások.  hello információt azonnal elérhetők legyenek az eszköz nagyon hasznos sablon létrehozásához, mert jelzi, hogy mi szüksége tooset erőforrás használata, ha egy bizonyos típusú hello tulajdonságok javítsa ki az értékeket, stb. Az erőforráscsoport is létrehozhat a hello [Azure Portal](https://portal.azure.com/), majd vizsgálja meg a JSON-definíciói a hello explorer eszköz toohelp templatize hello erőforráscsoport.

### <a name="deploy-tooazure-button"></a>TooAzure gomb telepítése
Ha GitHub a verziókövetési rendszerrel használja, akkor lehet helyezni egy [telepítés tooAzure gomb](https://azure.microsoft.com/blog/2014/11/13/deploy-to-azure-button-for-azure-websites-2/) azokat a fontos. MD, amely lehetővé teszi egy kulcsrakész központi telepítés felhasználói felület tooAzure. Ehhez minden egyszerű webalkalmazás, amíg ez egy teljes erőforráscsoport telepítése tegyen egy azuredeploy.json fájlt az adattár gyökérkönyvtárában hello tooenable bővítheti. A JSON-fájl, amely tartalmazza a hello erőforrás csoport sablon, hello telepítés tooAzure gomb toocreate hello erőforráscsoport használják. Egy vonatkozó példáért lásd: hello [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) mintát, amely ebben az oktatóanyagban használhatja.

## <a name="get-hello-sample-resource-group-template"></a>Hello minta erőforrás csoport sablon
Most folytassuk a megfelelő tooit.

1. Keresse meg a toohello [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) App Service-mintát.
2. A readme.md, kattintson **tooAzure telepítése**.
3. Ön által végrehajtott toohello [telepítése-a-azure](https://deploy.azure.com) hely és ismételt tooinput üzembe helyezéshez megadott paraméterek. Figyelje meg, hogy hello mezők meg hello tárház nevét és az egyes véletlenszerű karakterláncokat feltöltött. Ha azt szeretné, de hello csak dolgot tooenter rendelkezik hello SQL Server-rendszergazdai bejelentkezés és hello jelszót, majd kattintson az összes hello mezőben módosíthatja **következő**.
   
   ![](./media/app-service-deploy-complex-application-predictably/gettemplate-1-deploybuttonui.png)
4. Ezután kattintson **telepítés** toostart hello telepítési folyamat. Ha hello folyamat toocompletion fut, kattintson a hello http://todoapp*XXXX*. azurewebsites.net hivatkozás toobrowse hello telepített alkalmazás. 
   
   ![](./media/app-service-deploy-complex-application-predictably/gettemplate-2-deployprogress.png)
   
   felhasználói felület hello okozna kissé lassú, mert hello alkalmazásokat csak indítása, de meggyőzni a felhasználót saját kezűleg, hogy a rendszer egy teljesen működőképes alkalmazás először Tallózás tooit.
5. Vissza a hello központi telepítése lap, kattintson a hello **kezelése** toosee hello új alkalmazás hello Azure portálon lévő hivatkozásra.
6. A hello **Essentials** legördülő menüben hello erőforrás csoportjának hivatkozásra. Vegye figyelembe, hogy is, hogy hello webalkalmazás már csatlakozott a GitHub-tárházban toohello **külső projekt**. 
   
   ![](./media/app-service-deploy-complex-application-predictably/gettemplate-3-portalresourcegroup.png)
7. Hello erőforráscsoport panel jegyezze fel, hogy nincsenek már két webes alkalmazások és egy SQL-adatbázis hello erőforráscsoportban.
   
   ![](./media/app-service-deploy-complex-application-predictably/gettemplate-4-portalresourcegroupclicked.png)

Minden, ami az imént látott néhány rövid percben egy teljes körűen rendszerbe állított két-mikroszolgáltatási alkalmazást, az összes hello összetevők, függőségek, beállítások, adatbázis, és folyamatos közzétételét, a beállítási egy automatizált vezénylési az Azure Resource Manager. Összes Ez két dolgot végezhető el:

* hello telepítés tooAzure gomb
* azuredeploy.JSON hello tárház gyökérkönyvtárában

Ez az alkalmazás telepítése több tíz, száz vagy ezer hányszor és hello minden alkalommal pontos azonos konfigurációval rendelkezik. hello ismételhetőség és hello előreláthatóságának növelését ezt a módszert használja az egyszerűség és abban, hogy lehetővé teszi toodeploy jelentősen méretezhető alkalmazások.

## <a name="examine-or-edit-azuredeployjson"></a>Vizsgálja meg (vagy szerkesztése) AZUREDEPLOY. JSON-BAN
Most nézzük hogyan hello GitHub-tárházban be lett állítva. Az Azure .NET SDK-t, hello hello JSON-szerkesztő használ, ha még nem telepítette a [Azure .NET SDK 2.6](/downloads/), most megtenni.

1. Klónozás hello [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) tárházat a kedvenc git eszköz használatával. Hello a képernyőfelvételen látható az alábbi I vagyok ezzel a hello Team Explorert a Visual Studio 2013.
   
   ![](./media/app-service-deploy-complex-application-predictably/examinejson-1-vsclone.png)
2. Hello adattár gyökérkönyvtárában nyissa meg a Visual Studio azuredeploy.json. Ha nem látható hello JSON-vázlat ablak, tooinstall Azure .NET SDK-t kell.
   
   ![](./media/app-service-deploy-complex-application-predictably/examinejson-2-vsjsoneditor.png)

Tudom vagyok nem folyamatos toodescribe hello JSON formátumban minden részletét, de hello [több erőforrások](#resources) szakasz hello erőforrás csoport sablon nyelvi tanulási mutató hivatkozásokat tartalmaz. Itt csak fogom tooshow meg hello érdekes funkciókat, amelyek segítenek érdekében, hogy saját egyéni sablon az alkalmazások telepítésének első lépései.

### <a name="parameters"></a>Paraméterek
Tekintsen meg hello paraméterek szakaszban toosee, hogy-e a következő paraméterek legtöbb milyen hello **tooAzure telepítése** gomb tooinput kéri. hello hely mögött hello **tooAzure telepítése** gomb tölti fel a felhasználói felület hello bemeneti azuredeploy.json hello paraméterek használatával. Ezek a paraméterek kifejezés egyaránt használatban hello erőforrás-definíciókban, például erőforrásnevek, tulajdonságértékek stb.

### <a name="resources"></a>Erőforrások
Hello erőforrások csomópontjában tekintheti meg, hogy 4 legfelső szintű erőforrások vannak megadva, például egy SQL Server-példányt, az App Service-csomag és két webalkalmazások. 

#### <a name="app-service-plan"></a>App Service-csomag
Kezdjük a hello JSON egyszerű gyökér szintű erőforrás. A JSON-vázlat hello, kattintson a hello App Service-csomag nevű **[hostingPlanName]** toohighlight hello megfelelő JSON-kódot. 

![](./media/app-service-deploy-complex-application-predictably/examinejson-3-appserviceplan.png)

Vegye figyelembe, hogy hello `type` elem meghatározza hello karakterlánc egy App Service-csomagra (meghívta a kiszolgálófarm egy hosszú, hosszú idő telt el), és más elemek és tulajdonságok kitölti hello paraméterek hello JSON-fájl használatával, és ehhez az erőforráshoz nem rendelkezik a beágyazott erőforrások.

> [!NOTE]
> Azt is fontos megjegyezni, hogy hello értékének `apiVersion` közli az Azure melyik verzióját hello REST API toouse hello JSON erőforrás-definíció, és hatással lehet a hogyan hello erőforrás kell formázni belül hello `{}`. 
> 
> 

#### <a name="sql-server"></a>SQL Server
Ezután kattintson a hello SQL Server-erőforrást nevű **SQLServer** a JSON-vázlat hello.

![](./media/app-service-deploy-complex-application-predictably/examinejson-4-sqlserver.png)

A következő hello kapcsolatos megjegyzés hello kiemeli a JSON-kódot:

* paraméterek hello használata biztosítja a létrehozott hello erőforrások nevű és konfigurálva, hogy egymással konzisztenssé teszi őket.
* hello SQLServer erőforrás két beágyazott erőforrások, akkor mindegyik rendelkezik-e meg más értéket `type`.
* hello beágyazott erőforráshoz, `“resources”: […]`, ahol definiálva vannak hello adatbázis és hello tűzfalszabályok, rendelkezik egy `dependsOn` elem hello gyökér szintű SQLServer erőforrás hello erőforrás-azonosító. Ez alapján Azure Resource Manager "ehhez az erőforráshoz, amely már léteznie kell, más erőforrás; létrehozása előtt Ha más erőforrás hello sablonban van definiálva, és először hozza létre, hogy egy".
  
  > [!NOTE]
  > Részletes információt toouse hello `resourceId()` működik, lásd: [Azure Resource Manager Sablonfüggvényei](../azure-resource-manager/resource-group-template-functions-resource.md#resourceid).
  > 
  > 
* hello hatásának hello `dependsOn` eleme, hogy Azure Resource Manager tudhatja, mely erőforrásokat párhuzamosan lehet létrehozni és erőforrások egymás után létre kell hozni. 

#### <a name="web-app"></a>Webalkalmazás
Most tegyük áthelyezése toohello tényleges webalkalmazásokban, amelyek bonyolultabb. Kattintson a webalkalmazás hello [variables('apiSiteName')] hello JSON-vázlat toohighlight a JSON-kódot. Láthatja, hogy dolgot első sokkal ennél is érdekesebb megoldást. Erre a célra I lesz beszélgetés egyenként hello szolgáltatásairól:

##### <a name="root-resource"></a>Legfelső szintű erőforrás
hello webalkalmazás két különböző erőforrások függ. Ez azt jelenti, hogy Azure Resource Manager mindkét hello App Service-csomag és hello SQL Server-példány létrehozása után csak hello webes alkalmazást hoz létre.

![](./media/app-service-deploy-complex-application-predictably/examinejson-5-webapproot.png)

##### <a name="app-settings"></a>Alkalmazásbeállítások
hello Alkalmazásbeállítások egy beágyazott erőforrást is vannak meghatározva.

![](./media/app-service-deploy-complex-application-predictably/examinejson-6-webappsettings.png)

A hello `properties` eleme `config/appsettings`, két Alkalmazásbeállítások hello formátumban van `“<name>” : “<value>”`.

* `PROJECT`van egy [KUDU beállítás](https://github.com/projectkudu/kudu/wiki/Customizing-deployments) , amely közli az Azure-telepítés mely projekt toouse több projektet a Visual Studio-megoldásban. I bemutatja, hogyan később verziókezelő van konfigurálva, de mivel hello ToDoApp kód több projektet a Visual Studio-megoldásban, igazolnia kell-e ezt a beállítást.
* `clientUrl`rendszer egyszerűen egy adott hello alkalmazáskód beállítása alkalmazás használ.

##### <a name="connection-strings"></a>Kapcsolati karakterláncok
hello kapcsolati karakterláncok egy beágyazott erőforrást is vannak meghatározva.

![](./media/app-service-deploy-complex-application-predictably/examinejson-7-webappconnstr.png)

A hello `properties` eleme `config/connectionstrings`, minden egyes kapcsolati karakterláncot is nevezünk név: érték pár, az adott formátuma hello `“<name>” : {“value”: “…”, “type”: “…”}`. A hello `type` elem, a lehetséges értékek: `MySql`, `SQLServer`, `SQLAzure`, és `Custom`.

> [!TIP]
> Hello karakterláncot kapcsolattípusokat végleges listájának megtekintéséhez futtassa a következő parancs az Azure PowerShell hello: \[Enum]::GetNames("Microsoft.WindowsAzure.Commands.Utilities.Websites.Services.WebEntities.DatabaseType")
> 
> 

##### <a name="source-control"></a>A verziókövetési rendszerrel
hello verziókövetési beállítások egy beágyazott erőforrást is vannak meghatározva. Az Azure Resource Manager használ a erőforrás tooconfigure folyamatos közzétételi (szerint tekintse meg `IsManualIntegration` később) és is tookick ki hello telepítését alkalmazáskód automatikusan hello hello JSON-fájl feldolgozása során.

![](./media/app-service-deploy-complex-application-predictably/examinejson-8-webappsourcecontrol.png)

`RepoUrl`és `branch` közérthető intuitív kell lennie, és toohello Git tárház hello nevét és a hello fiókirodai toopublish kell mutatnia. Ebben az esetben ezek határozzák meg a bemeneti paraméterek. 

Megjegyzés: a hello `dependsOn` elemhez tartozó, továbbá toohello a webes alkalmazás-erőforrást, `sourcecontrols/web` is függ, `config/appsettings` és `config/connectionstrings`. Ennek az az oka után `sourcecontrols/web` van konfigurálva, hello Azure telepítési folyamat automatikusan megpróbálja toodeploy, felépítéséhez, és hello alkalmazáskód elindításához. Ezért beszúrni a függőségi segítségével, győződjön meg arról, hogy hello alkalmazásnak hozzáférés szükséges toohello Alkalmazásbeállítások és kapcsolati karakterláncok hello alkalmazáskód futtatása előtt. 

> [!NOTE]
> Azt is fontos megjegyezni, hogy `IsManualIntegration` értéke túl`true`. Ez a tulajdonság oka szükség ebben az oktatóanyagban ténylegesen nem saját hello GitHub-tárházban, és így nem ténylegesen adjon engedélye tooAzure tooconfigure folyamatos közzététel [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) (pl. leküldéses automatikus tárház frissítések tooAzure). Hello alapértelmezett érték használható `false` hello megadott tárház csak akkor, ha hello tulajdonos GitHub hitelesítő adatait a hello konfigurált [Azure-portálon](https://portal.azure.com/) előtt. Ez azt jelenti Ha állította be forrás vezérlő tooGitHub vagy BitBucket hello bármely alkalmazás [Azure Portal](https://portal.azure.com/) korábban, a felhasználó hitelesítő adatait, akkor Azure lesz hello hitelesítő adatok megjegyzése és használhatja őket, amikor a bármely alkalmazás központi telepítése GitHub vagy bitbucket szolgáltatásokkal a jövőbeli hello. Azonban ha ezt még nem végezte, hello JSON-sablon telepítése sikertelen lesz Azure Resource Manager tooconfigure hello web app verziókövetési beállítások megpróbál, mert az nem lehet bejelentkezni a GitHub vagy BitBucket hello tárház tulajdonosát hitelesítő adatokkal.
> 
> 

## <a name="compare-hello-json-template-with-deployed-resource-group"></a>Hasonlítsa össze a telepített erőforráscsoporttal hello JSON-sablon
Itt, lépjen a hello összes hello webes alkalmazás paneleken keresztül [Azure Portal](https://portal.azure.com/), de nincs egy másik eszköz, amely nincs csak, akkor hasznos, ha több. Nyissa meg toohello [Azure erőforrás-kezelő](https://resources.azure.com) preview eszközt, amely lehetővé teszi az összes hello erőforráscsoport JSON-ábrázolását előfizetése, a ténylegesen hello Azure-háttérrendszernek léteznek. Hogyan hello erőforráscsoport JSON hierarchia az Azure-ban megfelel a hello hierarchia által használt toocreate hello sablonfájl azt is láthatja.

Például ha haladhatok toohello tovább [Azure erőforrás-kezelő](https://resources.azure.com) eszköz és bontsa ki a hello explorer hello csomópontokat, láthatom valahol hello erőforráscsoport és a megfelelő erőforrástípusok szerint gyűjtött hello gyökér szintű erőforrások.

![](./media/app-service-deploy-complex-application-predictably/ARM-1-treeview.png)

Tooa webes alkalmazás részletező, ha el tudja toosee web app konfigurációs részletek hasonló toohello alábbi képernyőfelvételen:

![](./media/app-service-deploy-complex-application-predictably/ARM-2-jsonview.png)

Ebben az esetben hello beágyazott erőforrások kell lennie a hierarchia nagyon hasonló toothose a JSON-sablon fájlban, és hello Alkalmazásbeállítások, kapcsolati karakterláncok, stb., megfelelően hello JSON ablaktáblán megjelennek kell megjelennie. hello hiányában beállítások itt is utalhatnak egy JSON-fájlt, és is segítséget nyújthatnak a JSON-sablon fájl.

## <a name="deploy-hello-resource-group-template-yourself"></a>Hello erőforrás csoport sablon magát üzembe helyezése
Hello **tooAzure telepítése** gomb nagy, de lehetővé teszi toodeploy hello erőforrás csoport sablon az azuredeploy.json csak akkor, ha Ön már rendelkezik leküldött azuredeploy.json tooGitHub. hello Azure .NET SDK hello olyan eszközöket is biztosít az Ön toodeploy bármely JSON-sablon fájl közvetlenül a helyi számítógépről. toodo ezt, hajtsa végre hello az alábbi lépéseket:

1. A Visual Studióban kattintson **fájl** > **új** > **projekt**.
2. Kattintson a **Visual C#** > **felhő** > **Azure erőforráscsoport**, majd kattintson a **OK**.
   
   ![](./media/app-service-deploy-complex-application-predictably/deploy-1-vsproject.png)
3. A **Azure-sablon kiválasztása**, jelölje be **üres sablont** kattintson **OK**.
4. Azuredeploy.json húzza hello **sablon** az új projekt mappájából.
   
   ![](./media/app-service-deploy-complex-application-predictably/deploy-2-copyjson.png)
5. A Solution Explorerben nyissa meg a másolt hello azuredeploy.json.
6. Csak a hello szakét hello bemutató, adjuk hozzá néhány szokásos Application Insights erőforrások tooour JSON-fájl, kattintva **erőforrás hozzáadása**. Ha csak szeretné hello JSON-fájl központi telepítése, hagyja ki toohello telepítés lépéseit.
   
   ![](./media/app-service-deploy-complex-application-predictably/deploy-3-newresource.png)
7. Válassza ki **Application Insights Web Apps**, majd ellenőrizze, hogy egy meglévő App Service csomag és a webes alkalmazás van kiválasztva, és kattintson a **Hozzáadás**.
   
   ![](./media/app-service-deploy-complex-application-predictably/deploy-4-newappinsight.png)
   
   Most lesz képes toosee több lehet, hogy hello erőforrás és működése függ, függőségi viszonyban vannak vagy hello App Service-csomag vagy hello webalkalmazás. Ezeket az erőforrásokat a meglévő definíciót szerint nincsenek engedélyezve, és toochange fog, amely.
   
   ![](./media/app-service-deploy-complex-application-predictably/deploy-5-appinsightresources.png)
8. A JSON-vázlat hello, kattintson **appinsights által biztosított automatikus skálázás** toohighlight a JSON-kódot. Ez a beállítás az App Service-csomag skálázás hello.
9. A hello kiemelt JSON-kódot, keresse meg a hello `location` és `enabled` tulajdonságok és -e meg őket alább látható módon.
   
   ![](./media/app-service-deploy-complex-application-predictably/deploy-6-autoscalesettings.png)
10. A JSON-vázlat hello, kattintson **CPUHigh appinsights által biztosított** toohighlight a JSON-kódot. Ez az értesítés.
11. Keresse meg a hello `location` és `isEnabled` tulajdonságok és -e meg őket alább látható módon. Hello azonos hello egyéb három értesítések (lila hagymák).
    
    ![](./media/app-service-deploy-complex-application-predictably/deploy-7-alerts.png)
12. Most már készen áll a toodeploy. Kattintson a jobb gombbal a hello projektet, és válassza ki **telepítés** > **új központi telepítési**.
    
    ![](./media/app-service-deploy-complex-application-predictably/deploy-8-newdeployment.png)
13. Ha még nem tette meg, jelentkezzen be az Azure-fiókjával.
14. Jelöljön ki egy meglévő erőforráscsoportot az előfizetés, vagy hozzon létre egy új egy, jelöljön ki **azuredeploy.json**, és kattintson a **paraméterek szerkesztése**.
    
    ![](./media/app-service-deploy-complex-application-predictably/deploy-9-deployconfig.png)
    
    Most már tudja tooedit összes hello paraméter töltött tábla hello sablon fájlban meghatározott lesz. Alapértelmezett értékeket meghatározó paraméterek már rendelkezik az alapértelmezett értékekre, és a paraméterek, melyek meghatározzák az engedélyezett értékek listájának jelennek meg a legördülő lista.
    
    ![](./media/app-service-deploy-complex-application-predictably/deploy-10-parametereditor.png)
15. Töltse ki az összes hello üres paraméterek, és hello [ToDoApp GitHub-tárház címe](https://github.com/azure-appservice-samples/ToDoApp.git) a **repoUrl**. Kattintson a **mentése**.
    
    ![](./media/app-service-deploy-complex-application-predictably/deploy-11-parametereditorfilled.png)
    
    > [!NOTE]
    > Automatikus skálázás egy olyan funkció érhető el **szabványos** réteg vagy magasabb és a terv szintű riasztások a nyújtott szolgáltatások **alapvető** szintjüket, vagy újabb rendszerre, szüksége lesz tooset hello **sku** paraméter túl**szabványos** vagy **prémium** a rendezés toosee összes az új App Insights erőforrások könnyű fel.
    > 
    > 
16. Kattintson a **telepítése**. Ha a kiválasztott **menthetik a jelszavakat**, hello jelszót a rendszer menti a hello paraméterfájl **egyszerű szöveges**. Ellenkező esetben meg kell adnia tooinput hello adatbázisjelszót.%n hello telepítési folyamat során.

Készen is van. Most már egyszerűen toogo toohello [Azure Portal](https://portal.azure.com/) és hello [Azure erőforrás-kezelő](https://resources.azure.com) eszköz toosee hello új riasztások és a hozzáadott automatikus skálázási beállítás tooyour JSON telepített alkalmazás.

A jelen szakaszban szereplő lépéseket főként hello következő valósítható meg:

1. Előkészített hello sablonfájl
2. A paraméter fájl toogo létre hello sablonfájl
3. Telepített hello sablonfájl hello paraméter fájllal

hello utolsó lépés egyszerű PowerShell parancsmag végezhető el. toosee Mi a Visual Studio az az alkalmazás, nyissa meg Scripts\Deploy-AzureResourceGroup.ps1 telepítésekor volt. A kódot van, de csak fogom toohighlight összes hello lényeges szükséges kód toodeploy hello sablonfájl hello paraméter fájllal.

![](./media/app-service-deploy-complex-application-predictably/deploy-12-powershellsnippet.png)

utolsó parancsmag hello `New-AzureResourceGroup`, egy hello művelet végrehajtja hello van. Ez bizonyítania kell, hogy, tooling hello segítségével, a rendszer viszonylag egyszerű toodeploy tooyou a cloud kiszámítható módon tudja az alkalmazás. A hello hello parancsmagot kell futtatnia minden alkalommal ugyanazt a sablont a hello azonos paraméterfájl, fog tooget hello ugyanazt az eredményt.

## <a name="summary"></a>Összefoglalás
DevOps ismételhetőség és kiszámíthatóságot kulcsok tooany sikeres alkalmazás központi telepítését egy nagy méretű álló mikroszolgáltatások létrehozására. Ebben az oktatóanyagban telepítette a két-mikroszolgáltatási alkalmazás tooAzure hello Azure Resource Manager-sablon használatával egyetlen erőforráscsoportként működnek. Remélhetőleg, az adott hello Tudásbázis van szükség az alkalmazás az Azure-ban konvertálása a sablonba rendelés toostart és kioszthatja és kiszámítható telepítheti azt. 

## <a name="next-steps"></a>Következő lépések
Megtudhatja, hogyan túl[gyors módszereit, amelyek érvényesek, és folyamatosan tegye közzé a mikroszolgáltatások alkalmazást kihívásokra](app-service-agile-software-development.md) és a központi telepítési módszerek, például a speciális [flighting telepítési](app-service-web-test-in-production-controlled-test-flight.md) könnyen.

<a name="resources"></a>

## <a name="more-resources"></a>További erőforrások
* [Az Azure Resource Manager sablon nyelve](../azure-resource-manager/resource-group-authoring-templates.md)
* [Az Azure Resource Manager sablonok készítése](../azure-resource-manager/resource-group-authoring-templates.md)
* [Az Azure Resource Manager Sablonfüggvényei](../azure-resource-manager/resource-group-template-functions.md)
* [Alkalmazás üzembe helyezése az Azure Resource Manager-sablon](../azure-resource-manager/resource-group-template-deploy.md)
* [Az Azure PowerShell használata az Azure Resource Managerrel](../azure-resource-manager/powershell-azure-resource-manager.md)
* [Az Azure erőforráscsoport-telepítések hibaelhárítása](../azure-resource-manager/resource-manager-common-deployment-errors.md)

