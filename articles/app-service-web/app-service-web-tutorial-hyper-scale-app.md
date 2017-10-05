---
title: "Hozza létre a kapacitású alkalmazását az Azure-ban |} Microsoft Docs"
description: "Ismerje meg, hogy egy ASP.NET-alkalmazás az Azure-ban a teljesítmény maximalizálásához különböző Azure-szolgáltatások használatával."
services: app-service\web
documentationcenter: dotnet
author: cephalin
manager: erikre
editor: 
ms.assetid: a4d49ac7-0f97-4997-84c5-cdb9c4465757
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 03/23/2017
ms.author: cephalin
ms.openlocfilehash: eac9c5b0d8d0f7802d88e6f4f27d9d23c406e025
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="build-a-hyper-scale-web-app-in-azure"></a>Az Azure-ban kapacitású webalkalmazás létrehozása

Az oktatóanyag bemutatja, hogyan horizontális felskálázás az Azure-felhasználói kéréseinek maximális ASP.NET webalkalmazás.

Az oktatóanyag elindítása előtt győződjön meg arról, hogy [az Azure parancssori felület telepítése](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) a számítógépen. Továbbá szüksége [Visual Studio](https://www.visualstudio.com/vs/) a helyi számítógépen futtassa a mintaalkalmazást.

## <a name="step-1---get-sample-application"></a>1. lépés – Get mintaalkalmazás
Ebben a lépésben beállíthatja a helyi ASP.NET-projekt.

### <a name="clone-the-application-repository"></a>Az alkalmazás tárház klónozása

Nyissa meg a kívánt parancssori terminált, az Ön által választott és `CD` egy működő könyvtárba. Ezután futtassa a következő parancsok futtatásával klónozza a mintaalkalmazást. 

```powershell
git clone https://github.com/cephalin/HighScaleApp.git
```

### <a name="run-the-sample-application-in-visual-studio"></a>Futtassa a mintaalkalmazást a Visual Studióban

Nyissa meg a megoldást a Visual Studióban.

```powershell
cd HighScaleApp
.\HighScaleApp.sln
```

Típus `F5` az alkalmazás futtatásához.

Ez a minta ASP.NET webalkalmazásként való kezelése az alapértelmezett sablon származik, és továbbra is fennáll a felhasználói munkamenetek és a kimeneti gyorsítótár használja. Vessen egy pillantást `HighScaleApp\Controllers\HomeController.cs`. A `Index()` módszer az adatok a munkamenethez ad hozzá.

```csharp
Session.Add("visited", "true"); 
```

És a `About()` és `Contact()` módszerek a kimeneti gyorsítótárban.

```csharp
[OutputCache(Duration = 60)]
```

## <a name="step-2---deploy-to-azure"></a>– 2. lépés: telepítse az Azure
Ebben a lépésben Azure-webalkalmazás létrehozása és központi telepítése a mintaalkalmazást ASP.NET hozzá.

### <a name="create-a-resource-group"></a>Hozzon létre egy erőforráscsoportot   
Használjon [az csoport létrehozása](https://docs.microsoft.com/cli/azure/group#create) létrehozásához egy [erőforráscsoport](../azure-resource-manager/resource-group-overview.md) Nyugat-Európában régióban. Erőforráscsoport az összes Azure-erőforrást, amely a kezelni kívánt együtt, mint például a web app és az SQL-adatbázis háttér ahová.

```azurecli
az group create --location "West Europe" --name myResourceGroup
```

Milyen lehetséges értékei, megjelenítéséhez használható `---location`, használja a [az App Service lista-helyek](https://docs.microsoft.com/en-us/cli/azure/appservice#list-locations) parancsot.

### <a name="create-an-app-service-plan"></a>App Service-csomag létrehozása
Használjon [az App Service-csomagot hozzon létre](https://docs.microsoft.com/en-us/cli/azure/appservice/plan#create) létrehozása a "B1" [App Service-csomag](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md). 

```azurecli
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku B1
```

Az App Service-csomag a méretezési egység, ami magában foglalhatja tetszőleges számú vertikális vagy horizontális együtt ugyanazon az App Service-infrastruktúrán keresztül kívánt alkalmazásokat. Minden terv is hozzá van rendelve egy [tarifacsomag](https://azure.microsoft.com/en-us/pricing/details/app-service/). Magasabb szolgáltatásszintek jobb hardver- és további funkciók, például több kibővített példány.

Ebben az oktatóanyagban B1 esetén a minimális rétegben, amely lehetővé teszi a bővített kapacitású három alkalmazáspéldányra. Mindig áthelyezheti az alkalmazás felfelé vagy lefelé a tarifacsomag később futtatásával [az App Service-csomag frissítése](https://docs.microsoft.com/cli/azure/appservice/plan#update). 

### <a name="create-a-web-app"></a>Webalkalmazás létrehozása
Használjon [az App Service webalkalmazás létrehozása](https://docs.microsoft.com/en-us/cli/azure/appservice/web#create) egy egyedi nevet a webalkalmazás létrehozásához `$appName`.

```azurecli
$appName = "<replace-with-a-unique-name>"
az appservice web create --name $appName --resource-group myResourceGroup --plan myAppServicePlan
```

### <a name="set-deployment-credentials"></a>Telepítési hitelesítő adatok beállítása
Használjon [az App Service web beállítva üzembe helyező felhasználó](https://docs.microsoft.com/en-us/cli/azure/appservice/web/deployment/user#set) az App Service a fiók szintű üzembe helyezési hitelesítő adatok beállításához.

```azurecli
az appservice web deployment user set --user-name <letters-numbers> --password <mininum-8-char-captital-lowercase-letters-numbers>
```

### <a name="configure-git-deployment"></a>Git-telepítés konfigurálása
Használjon [az App Service web verziókezelő config-helyi-git](https://docs.microsoft.com/en-us/cli/azure/appservice/web/source-control#config-local-git) helyi Git-telepítés konfigurálása.

```azurecli
az appservice web source-control config-local-git --name $appName --resource-group myResourceGroup
```

Ez a parancs, amely a következőhöz hasonló kimenetnek biztosítja:

```json
{
  "url": "https://user123@myuniqueappname.scm.azurewebsites.net/myuniqueappname.git"
}
```

A visszaadott URL-cím segítségével konfigurálhatja a Git távoli. A következő parancs az előző példa konzolkimenetben.

```powershell
git remote add azure https://user123@myuniqueappname.scm.azurewebsites.net/myuniqueappname.git
```

### <a name="deploy-the-sample-application"></a>A mintaalkalmazás telepítése
Most már készen áll a mintaalkalmazás telepítése. Futtassa az `git push` parancsot.

```powershell
git push azure master
```

Ha a jelszó megadását kéri, használja a jelszót, amely a megadott névvel `az appservice web deployment user set`.

### <a name="browse-to-azure-web-app"></a>Keresse meg az Azure web apphoz
Használjon [az App Service web Tallózás](https://docs.microsoft.com/en-us/cli/azure/appservice/web#browse) fut élőben az Azure-ban az alkalmazás megtekintéséhez futtassa a parancsot.

```azurecli
az appservice web browse --name $appName --resource-group myResourceGroup
```

## <a name="step-3---connect-to-redis"></a>Lépés a 3 - való csatlakozáshoz
Ebben a lépésben beállított Azure Redis Cache egy külső, közös elhelyezésű gyorsítótár az Azure-webalkalmazásban. A lap kimenete gyorsítótárazásához Redis gyorsan használhatja. Továbbá, amikor később kiterjesztése a webalkalmazások, Redis segít megőrizni a felhasználói munkamenetek több példánya megbízhatóan.

### <a name="create-an-azure-redis-cache"></a>Azure Redis Cache létrehozása
Használjon [az redis létrehozása](https://docs.microsoft.com/en-us/cli/azure/redis#create) Azure Redis Cache létrehozása és mentése a JSON kimeneti. Egyedi nevet a `$cacheName`.

```powershell
$cacheName = "<replace-with-a-unique-cache-name>"
$redis = (az redis create --name $cacheName --resource-group myResourceGroup --location "West Europe" --sku-capacity 0 --sku-family C --sku-name Basic | ConvertFrom-Json)
```

### <a name="configure-the-application-to-use-redis"></a>Az alkalmazás használata a Redis beállítása
A gyorsítótár a kapcsolati karakterlánc formátuma.

```powershell
$connstring = "$($redis.hostname):$($redis.sslPort),password=$($redis.accessKeys.primaryKey),ssl=True,abortConnect=False"
$connstring 
```

A második sorban adjon meg, a következőhöz hasonló kimenetnek:

```
mycachename.redis.cache.windows.net:6380,password=/rQP/TLz1mrEPpmh9b/gnfns/t9vBRXqXn3i1RwBjGA=,ssl=True,abortConnect=False
```

A projekt gyökérkönyvtárában nevű webes konfigurációs fájl létrehozása a Visual Studio `redis.config` és az alábbi kód beillesztése. A `value`, használja a kapcsolati karakterláncot a PowerShell-kimenetből.

```xml
<appSettings>
  <add key="RedisConnection" value="your-azure-redis-cache-connection-string"/>
</appSettings>
```

Ha megnézi a `.gitignore` fájl a Git-tárházban, láthatja, hogy a fájl nem a verziókövetésből. Ezzel a módszerrel a bizalmas információk biztonságos maradjon. 

Nyissa meg `Web.config`. Figyelje meg a `<appSettings file="redis.config">` elemet, amely lekérdezi a létrehozott beállítást `redis.config`. 

Keresse meg a megjegyzésekkel szakaszt, amely tartalmazza az `<sessionState>` és `<caching>`. Állítsa vissza az ebben a szakaszban.

![](./media/app-service-web-tutorial-hyper-scale-app/redisproviders.png)

Ez a kód keresi a Redis kapcsolódási karakterlánc ben megadott `RedisConnection`. 

Az alkalmazás most Redis munkamenetek és a gyorsítótár kezelésére használ. Típus `F5` az alkalmazás futtatásához. Ha kívánja letöltheti a Redis-kezelési ügynök, a gyorsítótár most mentett adatok megjelenítéséhez.

### <a name="configure-the-connection-string-in-azure"></a>A kapcsolati karakterlánc konfigurálása az Azure-ban

Az alkalmazás működéséhez az Azure-ban az azonos Redis-kapcsolati karakterlánc konfigurálása az Azure web app alkalmazásban kell. Mivel a `redis.config` nem őrzi meg a verziókövetés, még nincs telepítve az Azure Git-telepítés futtatásakor.

Használjon [az App Service web config appsettings frissítése](https://docs.microsoft.com/cli/azure/appservice/web/config/appsettings#update) neve a kapcsolati karakterlánc hozzáadása (`RedisConnection`).

az App Service web config appsettings frissítése – beállítások "RedisConnection = $connstring"--$appName--myResourceGroup erőforráscsoport név

Ne feledje, hogy `$connstring` a formázott kapcsolati karakterláncot tartalmaz.

### <a name="redeploy-the-application-to-azure"></a>Telepítse újra az alkalmazást az Azure-bA
Továbbítsa a módosításokat az Azure Git-parancsok segítségével

```bash
git add .
git commit -m "now use Redis providers"
git push azure master
```

Ha a jelszó megadását kéri, használja a jelszót, amely a megadott névvel `az appservice web deployment user set`.

### <a name="browse-to-the-azure-web-app"></a>Keresse meg az Azure-webalkalmazásban
Használjon [az App Service web Tallózás](https://docs.microsoft.com/en-us/cli/azure/appservice/web#browse) a változtatások élő Azure-ban.

```azurecli
az appservice web browse --name $appName --resource-group myResourceGroup
```

## <a name="step-4---scale-to-multiple-instances"></a>4. lépés: több példány méret
Az App Service-csomag a skálázási egység az Azure web Apps. A webes alkalmazás horizontális, az App Service-csomag vertikális.

Használjon [az App Service-csomag frissítése](https://docs.microsoft.com/cli/azure/appservice/plan#update) B1 tarifacsomag engedélyezett kiterjesztése az App Service-csomag három alkalmazáspéldányra, amely a maximális szám. Ne feledje, hogy B1 az árképzési szint, amikor az App Service-csomag korábban létrehozott választott-e. 

```azurecli
az appservice plan update --name myAppServicePlan --resource-group myResourceGroup --number-of-workers 3 
```

## <a name="step-5---scale-geographically"></a>5 - skálázási földrajzilag lépés
Skálázás földrajzilag, amikor az alkalmazás futtatása az Azure-felhő több régióba. A telepítő kiegyenlíti az alkalmazás további földrajzi hely alapján, és csökkenti a válaszidő úgy, hogy az alkalmazás közelebb ügyfélböngészők.

Ebben a lépésben egy második terület az ASP.NET webalkalmazás skálázása [Azure Traffic Manager](https://docs.microsoft.com/en-us/azure/traffic-manager/). A lépés végén kell egy webalkalmazást, Nyugat-Európában (már létrehozott) futó és egy webalkalmazást, Délkelet-Ázsia (még nem hozott létre) futtatja. Mindkét alkalmazásban a Traffic Manager URL-CÍMÉRE a szolgáltató.

### <a name="scale-up-the-europe-app-to-standard-tier"></a>A Standard csomag Európa alkalmazás méretezése
Az App Service-ben integráció az Azure Traffic Manager csak a Standard tarifacsomagban használható. Használjon [az App Service-csomag frissítése](https://docs.microsoft.com/cli/azure/appservice/plan#update) méretezést kívánó S1 az App Service-csomag. 

```azurecli
az appservice plan update --name myAppServicePlan --resource-group myResourceGroup --sku S1
```
### <a name="create-a-traffic-manager-profile"></a>Traffic Manager-profil létrehozása 
Használjon [az hálózati forgalmat-manager-profil létrehozása](https://docs.microsoft.com/cli/azure/network/traffic-manager/profile#create) Traffic Manager-profil létrehozásához, és adja hozzá az erőforráscsoport. Egy egyedi DNS-név $dnsName használja.

```azurecli
$dnsName = "<replace-with-unique-dns-name>"
az network traffic-manager profile create --name myTrafficManagerProfile --resource-group myResourceGroup --routing-method Performance --unique-dns-name $dnsName
```

> [!NOTE]
> `--routing-method Performance`Megadja, hogy ez a profil [továbbítja a felhasználói forgalomnak a legközelebbi végpont](../traffic-manager/traffic-manager-routing-methods.md).

### <a name="get-the-resource-id-of-the-europe-app"></a>Az erőforrás-azonosítója a Európa alkalmazás beszerzése
Használjon [az App Service web megjelenítése](https://docs.microsoft.com/en-us/cli/azure/appservice/web#show) az erőforrás-azonosítója a webes alkalmazás eléréséhez.

```azurecli
$appId = az appservice web show --name $appName --resource-group myResourceGroup --query id --output tsv
```

### <a name="add-a-traffic-manager-endpoint-for-the-europe-app"></a>A Traffic Manager-végpont a Európa alkalmazás hozzáadása
Használjon [az hálózati forgalmat-manager-végpont létrehozása](https://docs.microsoft.com/cli/azure/network/traffic-manager/endpoint#create) végpont felvétele a Traffic Manager-profil és a webes alkalmazás erőforrás-azonosítója a célként használni.

```azurecli
az network traffic-manager endpoint create --name myWestEuropeEndpoint --profile-name myTrafficManagerProfile --resource-group myResourceGroup --type azureEndpoints --target-resource-id $appId
```

### <a name="get-the-traffic-manager-endpoint-url"></a>A Traffic Manager-végpont URL-cím lekérése
A Traffic Manager-profil most már olyan végponttal, amely a már meglévő webalkalmazás mutat. Használjon [az hálózati forgalmat-kezelő profil megjelenítése](https://docs.microsoft.com/cli/azure/network/traffic-manager/profile#show) lekérni az URL-CÍMÉT. 

```azurecli
az network traffic-manager profile show --name myTrafficManagerProfile --resource-group myResourceGroup --query dnsConfig.fqdn --output tsv
```

Másolja át a kimenetet a böngészőbe. A webalkalmazás ismét meg kell jelennie.

### <a name="create-an-azure-redis-cache-in-asia"></a>Hozzon létre egy Azure Redis Cache Ázsiában
Az Azure-webalkalmazásban, Délkelet-Ázsia régió replikálni. Indításához használja [az redis létrehozása](https://docs.microsoft.com/en-us/cli/azure/redis#create) egy második Azure Redis Cache létrehozása a Délkelet-Ázsia. Ez a gyorsítótár kell lennie a közös elhelyezésű Ázsiában alkalmazása.

```powershell
$redis = (az redis create --name $cacheName-asia --resource-group myResourceGroup --location "Southeast Asia" --sku-capacity 0 --sku-family C --sku-name Basic | ConvertFrom-Json)
```

`--name $cacheName-asia`Nyugat-Európában gyorsítótár nevét a gyorsítótár biztosít a a `-asia` utótag.

### <a name="create-an-app-service-plan-in-asia"></a>Az App Service-csomag létrehozása Ázsiában
Használjon [az App Service-csomagot hozzon létre](https://docs.microsoft.com/cli/azure/appservice/plan#create) egy második App Service-csomagot a régióban Délkelet-Ázsiában, az azonos S1 tartozó használja, mint a Nyugat-Európában terv létrehozásához.

```azurecli
az appservice plan create --name myAppServicePlanAsia --resource-group myResourceGroup --location "Southeast Asia" --sku S1
```

### <a name="create-a-web-app-in-asia"></a>A webalkalmazás létrehozása az Ázsia
Használjon [az App Service webalkalmazás létrehozása](https://docs.microsoft.com/en-us/cli/azure/appservice/web#create) hozhat létre egy második webalkalmazást.

```azurecli
az appservice web create --name $appName-asia --resource-group myResourceGroup --plan myAppServicePlanAsia
```

`--name $appName-asia`Nyugat-Európában alkalmazás nevét az alkalmazás biztosítja a a `-asia` utótag.

### <a name="configure-the-connection-string-for-redis"></a>A Redis a kapcsolati karakterlánc konfigurálása
Használjon [az App Service web config appsettings frissítése](https://docs.microsoft.com/cli/azure/appservice/web/config/appsettings#update) hozzáadása a webes alkalmazás a kapcsolati karakterláncot a Délkelet-Ázsia gyorsítótár.

az App Service web config appsettings frissítése – beállítások "RedisConnection$ ($redis.hostname) =: $($redis.sslPort), a jelszó$ ($redis.accessKeys.primaryKey) ssl = = True, abortConnect = False"--name $appName-Ázsia--erőforráscsoport myResourceGroup

### <a name="configure-git-deployment-for-the-asia-app"></a>Az Ázsia alkalmazás Git-KözpontiTelepítés konfigurálása.
Használjon [az App Service web verziókezelő config-helyi-git](https://docs.microsoft.com/en-us/cli/azure/appservice/web/source-control#config-local-git) helyi Git-telepítésének a második webalkalmazás konfigurálása.

```azurecli
az appservice web source-control config-local-git --name $appName-asia --resource-group myResourceGroup
```

Ez a parancs, amely a következőhöz hasonló kimenetnek biztosítja:

```json
{
  "url": "https://user123@myuniqueappname-asia.scm.azurewebsites.net/myuniqueappname.git"
}
```

A visszaadott URL-cím segítségével konfigurálhatja a helyi tárház távoli Git-egy második. A következő parancs az előző példa konzolkimenetben.

```bash
git remote add azure-asia https://user123@myuniqueappname-asia.scm.azurewebsites.net/myuniqueappname.git
```

### <a name="deploy-your-sample-application"></a>A mintaalkalmazás telepítése
Futtatás `git push` a mintaalkalmazást a második távoli Git való telepítéséhez. 

```bash
git push azure-asia master
```

Ha a jelszó megadását kéri, használja a jelszót, amely a megadott névvel `az appservice web deployment user set`.

### <a name="browse-to-the-asia-app"></a>Keresse meg az Ázsia alkalmazás
Használjon [az App Service web Tallózás](https://docs.microsoft.com/en-us/cli/azure/appservice/web#browse) ellenőrizze, hogy az alkalmazás az Azure-ban élő fut-e.

```azurecli
az appservice web browse --name $appName-asia --resource-group myResourceGroup
```

### <a name="get-the-resource-id-of-the-asia-app"></a>Az erőforrás-azonosítója az Ázsia alkalmazás beszerzése
Használjon [az App Service web megjelenítése](https://docs.microsoft.com/en-us/cli/azure/appservice/web#show) beolvasása a erőforrás-azonosítója a webalkalmazás, Délkelet-Ázsia.

```azurecli
$appIdAsia = az appservice web show --name $appName-asia --resource-group myResourceGroup --query id --output tsv
```

### <a name="add-a-traffic-manager-endpoint-for-the-asia-app"></a>A Traffic Manager-végpontot az Ázsia alkalmazás hozzáadása
Használjon [az hálózati forgalmat-manager-végpont létrehozása](https://docs.microsoft.com/cli/azure/network/traffic-manager/endpoint#create) egy másik végpont hozzáadása a Traffic Manager-profil.

```azurecli
az network traffic-manager endpoint create --name myAsiaEndpoint --profile-name myTrafficManagerProfile --resource-group myResourceGroup --type azureEndpoints --target-resource-id $appIdAsia
```

### <a name="add-region-identifier-to-web-apps"></a>A webalkalmazások régióazonosító hozzáadása
Használjon [az App Service web config appsettings frissítése](https://docs.microsoft.com/cli/azure/appservice/web/config/appsettings#update) hozzáadása egy régióspecifikus környezeti változót.

```azurecli
az appservice web config appsettings update --settings "Region=West Europe" --name $appName --resource-group myResourceGroup
az appservice web config appsettings update --settings "Region=Southeast Asia" --name $appName-asia --resource-group myResourceGroup
```

Az alkalmazás kódjában már használja az alkalmazás-beállítás. Vessen egy pillantást `HighScaleApp\Views\Home\Index.cshtml`.

### <a name="complete"></a>Fejezze be!

Most próbáljon meg a Traffic Manager-profil URL-CÍMÉT a különböző földrajzi régióban böngészőkkel történő eléréséhez. Ügyfélböngészők Európából megjelennek az "ASP.NET Nyugat-Európa", és az Ázsia ügyfélböngészőnek megjelennek az "ASP.NET Délkelet-Ázsia."

## <a name="more-resources"></a>További erőforrások
