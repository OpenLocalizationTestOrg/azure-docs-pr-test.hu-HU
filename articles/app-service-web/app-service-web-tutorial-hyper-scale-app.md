---
title: "aaaBuild egy kapacitású alkalmazást az Azure-ban |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse különböző Azure-szolgáltatások toomaximize hello teljesítmény egy ASP.NET-alkalmazás az Azure-ban."
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
ms.openlocfilehash: 7952647b49a82c286c6a737eb41a7f23a13fd75e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="build-a-hyper-scale-web-app-in-azure"></a>Az Azure-ban kapacitású webalkalmazás létrehozása

Az oktatóanyag bemutatja, hogyan tooscale kimenő ASP.NET webalkalmazás az Azure toomaximize felhasználó kéri.

Az oktatóanyag elindítása előtt győződjön meg arról, hogy [hello Azure parancssori felület telepítése](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) a számítógépen. Továbbá szüksége [Visual Studio](https://www.visualstudio.com/vs/) a helyi számítógép toorun hello mintaalkalmazást.

## <a name="step-1---get-sample-application"></a>1. lépés – Get mintaalkalmazás
Ebben a lépésben beállíthatja hello helyi ASP.NET-projekt.

### <a name="clone-hello-application-repository"></a>Klónozás hello alkalmazás tárház

Nyissa meg hello parancssori terminált az Ön által választott és `CD` tooa munkakönyvtárát. Ezt követően a futtatási hello következő tooclone hello mintaalkalmazás parancsokat. 

```powershell
git clone https://github.com/cephalin/HighScaleApp.git
```

### <a name="run-hello-sample-application-in-visual-studio"></a>A Visual Studio hello mintaalkalmazás futtatása

Nyissa meg a hello megoldást a Visual Studio.

```powershell
cd HighScaleApp
.\HighScaleApp.sln
```

Típus `F5` toorun hello alkalmazás.

Ez a minta ASP.NET webalkalmazás hello alapértelmezett sablon származik, és továbbra is fennáll a felhasználói munkamenetek és felhasználási hello a kimeneti gyorsítótárban. Vessen egy pillantást `HighScaleApp\Controllers\HomeController.cs`. Hello `Index()` metódus ad adatok toohello munkamenet része.

```csharp
Session.Add("visited", "true"); 
```

És hello `About()` és `Contact()` módszerek a kimeneti gyorsítótárban.

```csharp
[OutputCache(Duration = 60)]
```

## <a name="step-2---deploy-tooazure"></a>2. lépés - tooAzure telepítése
Ebben a lépésben egy Azure-webalkalmazás létrehozása és központi telepítése a minta ASP.NET alkalmazás tooit.

### <a name="create-a-resource-group"></a>Hozzon létre egy erőforráscsoportot   
Használjon [az csoport létrehozása](https://docs.microsoft.com/cli/azure/group#create) toocreate egy [erőforráscsoport](../azure-resource-manager/resource-group-overview.md) hello Nyugat-Európában régióban. Erőforráscsoport meg, ahová az összes hello toomanage együtt, kívánt Azure-erőforrások, például a háttér hello web app és az SQL-adatbázis.

```azurecli
az group create --location "West Europe" --name myResourceGroup
```

toosee milyen lehetséges értékei, akkor is használhat `---location`, használja a hello [az App Service lista-helyek](https://docs.microsoft.com/en-us/cli/azure/appservice#list-locations) parancs.

### <a name="create-an-app-service-plan"></a>App Service-csomag létrehozása
Használjon [az App Service-csomagot hozzon létre](https://docs.microsoft.com/en-us/cli/azure/appservice/plan#create) toocreate "B1" [App Service-csomag](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md). 

```azurecli
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku B1
```

Az App Service-csomag a méretezési egység, ami magában foglalhatja az alkalmazások tooscale szeretné, hogy fel, vagy ki együtt over hello azonos tetszőleges számú App Service-infrastruktúra. Minden terv is hozzá van rendelve egy [tarifacsomag](https://azure.microsoft.com/en-us/pricing/details/app-service/). Magasabb szolgáltatásszintek jobb hardver- és további funkciók, például több kibővített példány.

Ebben az oktatóanyagban B1 hello minimális réteghez, amely lehetővé teszi a bővített kapacitású toothree példányok esetén. Áthelyezheti az alkalmazás mindig felfelé vagy lefelé futtatásával később tarifacsomag hello [az App Service-csomag frissítése](https://docs.microsoft.com/cli/azure/appservice/plan#update). 

### <a name="create-a-web-app"></a>Webalkalmazás létrehozása
Használjon [az App Service webalkalmazás létrehozása](https://docs.microsoft.com/en-us/cli/azure/appservice/web#create) toocreate egy egyedi nevet a webalkalmazás a `$appName`.

```azurecli
$appName = "<replace-with-a-unique-name>"
az appservice web create --name $appName --resource-group myResourceGroup --plan myAppServicePlan
```

### <a name="set-deployment-credentials"></a>Telepítési hitelesítő adatok beállítása
Használjon [az App Service web beállítva üzembe helyező felhasználó](https://docs.microsoft.com/en-us/cli/azure/appservice/web/deployment/user#set) a fiók szintű telepítési hitelesítő adatait, az App Service tooset.

```azurecli
az appservice web deployment user set --user-name <letters-numbers> --password <mininum-8-char-captital-lowercase-letters-numbers>
```

### <a name="configure-git-deployment"></a>Git-telepítés konfigurálása
Használjon [az App Service web verziókezelő config-helyi-git](https://docs.microsoft.com/en-us/cli/azure/appservice/web/source-control#config-local-git) tooconfigure helyi Git-telepítés.

```azurecli
az appservice web source-control config-local-git --name $appName --resource-group myResourceGroup
```

Ez a parancs lehetővé teszi, hogy a következőhöz hasonló hello kimenetnek:

```json
{
  "url": "https://user123@myuniqueappname.scm.azurewebsites.net/myuniqueappname.git"
}
```

Használjon hello visszaadott URL-cím tooconfigure a Git távoli. hello következő parancs hello megelőző példa a kimenetre.

```powershell
git remote add azure https://user123@myuniqueappname.scm.azurewebsites.net/myuniqueappname.git
```

### <a name="deploy-hello-sample-application"></a>Hello mintaalkalmazás telepítése
Meg vannak toodeploy most már készen áll a mintaalkalmazáshoz. Futtassa az `git push` parancsot.

```powershell
git push azure master
```

A jelszó megadását kéri, ha a megadott kulcsfájloknak hello-jelszót használja `az appservice web deployment user set`.

### <a name="browse-tooazure-web-app"></a>Keresse meg a tooAzure webalkalmazás
Használjon [az App Service web Tallózás](https://docs.microsoft.com/en-us/cli/azure/appservice/web#browse) toosee az alkalmazás fut élőben az Azure, a parancs futtatásához.

```azurecli
az appservice web browse --name $appName --resource-group myResourceGroup
```

## <a name="step-3---connect-tooredis"></a>3. lépés - Csatlakozás tooRedis
Ebben a lépésben beállított Azure Redis Cache-gyorsítótár külső, közös elhelyezésű tooyour Azure-webalkalmazás. Gyorsan használhatja a Redis toocache a lap kimenete. Továbbá, amikor később kiterjesztése a webalkalmazások, Redis segít megőrizni a felhasználói munkamenetek több példánya megbízhatóan.

### <a name="create-an-azure-redis-cache"></a>Azure Redis Cache létrehozása
Használjon [az redis létrehozása](https://docs.microsoft.com/en-us/cli/azure/redis#create) toocreate az Azure Redis Cache-gyorsítótár és menthet hello JSON kimeneti. Egyedi nevet a `$cacheName`.

```powershell
$cacheName = "<replace-with-a-unique-cache-name>"
$redis = (az redis create --name $cacheName --resource-group myResourceGroup --location "West Europe" --sku-capacity 0 --sku-family C --sku-name Basic | ConvertFrom-Json)
```

### <a name="configure-hello-application-toouse-redis"></a>Hello alkalmazás toouse Redis konfigurálása
A gyorsítótár hello kapcsolati karakterlánc formátuma.

```powershell
$connstring = "$($redis.hostname):$($redis.sslPort),password=$($redis.accessKeys.primaryKey),ssl=True,abortConnect=False"
$connstring 
```

hello második sorban adjon meg, a következőhöz hasonló kimenetnek:

```
mycachename.redis.cache.windows.net:6380,password=/rQP/TLz1mrEPpmh9b/gnfns/t9vBRXqXn3i1RwBjGA=,ssl=True,abortConnect=False
```

A projekt gyökérkönyvtárában nevű webes konfigurációs fájl létrehozása a Visual Studio `redis.config` és a következő kód beillesztése hello bele. A `value`, hello kapcsolati karakterláncnak a következőről hello PowerShell kimeneti használja.

```xml
<appSettings>
  <add key="RedisConnection" value="your-azure-redis-cache-connection-string"/>
</appSettings>
```

Ha megnézzük hello `.gitignore` fájl a Git-tárházban, láthatja, hogy a fájl nem a verziókövetésből. Ezzel a módszerrel a bizalmas információk biztonságos maradjon. 

Nyissa meg `Web.config`. Értesítés hello `<appSettings file="redis.config">` elemet, amely lekérdezi a létrehozott hello beállítás `redis.config`. 

Hello megjegyzésként található, amely tartalmazza a szakasz `<sessionState>` és `<caching>`. Állítsa vissza az ebben a szakaszban.

![](./media/app-service-web-tutorial-hyper-scale-app/redisproviders.png)

Ez a kód keresi hello Redis kapcsolódási karakterlánc ben megadott `RedisConnection`. 

Az alkalmazás most már használja a Redis toomanage munkamenetek és a gyorsítótárazást. Típus `F5` toorun hello alkalmazás. Ha kívánja letöltheti a Redis felügyeleti ügyfél toovisualize hello most mentett adatok toohello gyorsítótár.

### <a name="configure-hello-connection-string-in-azure"></a>Hello kapcsolati karakterlánc konfigurálása az Azure-ban

Az alkalmazás toowork az Azure-ban, meg kell tooconfigure hello ugyanazt a Redis a kapcsolati karakterláncot az Azure web app alkalmazásban. Mivel a `redis.config` nem őrzi meg a verziókövetés, még nincs üzembe helyezett tooAzure Git-telepítés futtatásakor.

Használjon [az App Service web config appsettings frissítése](https://docs.microsoft.com/cli/azure/appservice/web/config/appsettings#update) tooadd hello kapcsolati karakterlánc hello azonos neve (`RedisConnection`).

az App Service web config appsettings frissítése – beállítások "RedisConnection = $connstring"--$appName--myResourceGroup erőforráscsoport név

Ne feledje, hogy `$connstring` formázott hello kapcsolati karakterláncot tartalmaz.

### <a name="redeploy-hello-application-tooazure"></a>Telepítse újra a hello alkalmazás tooAzure
A módosítások tooAzure használja a Git-parancsok toopush

```bash
git add .
git commit -m "now use Redis providers"
git push azure master
```

A jelszó megadását kéri, ha a megadott kulcsfájloknak hello-jelszót használja `az appservice web deployment user set`.

### <a name="browse-toohello-azure-web-app"></a>Keresse meg az Azure-webalkalmazásban toohello
Használjon [az App Service web Tallózás](https://docs.microsoft.com/en-us/cli/azure/appservice/web#browse) toosee hello módosítások élő Azure-ban.

```azurecli
az appservice web browse --name $appName --resource-group myResourceGroup
```

## <a name="step-4---scale-toomultiple-instances"></a>4. lépés: méretezési toomultiple példányok
App Service-csomag hello hello skálázási egység az Azure web Apps. a webalkalmazás kimenő tooscale, méretezhető hello App Service-csomag.

Használjon [az App Service-csomag frissítése](https://docs.microsoft.com/cli/azure/appservice/plan#update) tooscale kimenő hello az alkalmazásszolgáltatási csomag toothree példányok, amely hello hello B1 IP-címek által engedélyezett maximális számát. Ne feledje, hogy B1 hello hello korábban az App Service-csomag létrehozásakor választott tarifacsomag. 

```azurecli
az appservice plan update --name myAppServicePlan --resource-group myResourceGroup --number-of-workers 3 
```

## <a name="step-5---scale-geographically"></a>5 - skálázási földrajzilag lépés
Skálázás földrajzilag, amikor az alkalmazás futtatása az Azure-felhőbe hello több régióba. A telepítő kiegyenlíti az alkalmazás további földrajzi hely alapján, és úgy, hogy az alkalmazás szorosabb tooclient böngészők hello válaszidő csökkenti.

Ebben a lépésben az ASP.NET web app tooa második régióban, méretezhető [Azure Traffic Manager](https://docs.microsoft.com/en-us/azure/traffic-manager/). Hello lépés végén hello hogy egy webalkalmazást, Nyugat-Európában (már létrehozott) futó és a webalkalmazás fut (még nem hozott létre) Délkelet-Ázsiában. Mindkét alkalmazásban szolgáltató hello a Traffic Manager URL-címe megegyezik.

### <a name="scale-up-hello-europe-app-toostandard-tier"></a>Vertikális felskálázás hello Európa app tooStandard réteg
Az App Service-ben az Azure Traffic Manager integrációs hello a Standard tarifacsomag szükséges. Használjon [az App Service-csomag frissítése](https://docs.microsoft.com/cli/azure/appservice/plan#update) tooscale az App Service-csomag tooS1 fel. 

```azurecli
az appservice plan update --name myAppServicePlan --resource-group myResourceGroup --sku S1
```
### <a name="create-a-traffic-manager-profile"></a>Traffic Manager-profil létrehozása 
Használjon [az hálózati forgalmat-manager-profil létrehozása](https://docs.microsoft.com/cli/azure/network/traffic-manager/profile#create) toocreate egy Traffic Manager profilt, és adja hozzá tooyour erőforráscsoportot. Egy egyedi DNS-név $dnsName használja.

```azurecli
$dnsName = "<replace-with-unique-dns-name>"
az network traffic-manager profile create --name myTrafficManagerProfile --resource-group myResourceGroup --routing-method Performance --unique-dns-name $dnsName
```

> [!NOTE]
> `--routing-method Performance`Megadja, hogy ez a profil [irányítja a felhasználói forgalom toohello legközelebbi végpont](../traffic-manager/traffic-manager-routing-methods.md).

### <a name="get-hello-resource-id-of-hello-europe-app"></a>Erőforrás-azonosító hello hello Európa alkalmazás beszerzése
Használjon [az App Service web megjelenítése](https://docs.microsoft.com/en-us/cli/azure/appservice/web#show) tooget hello erőforrás-azonosítója a webes alkalmazást.

```azurecli
$appId = az appservice web show --name $appName --resource-group myResourceGroup --query id --output tsv
```

### <a name="add-a-traffic-manager-endpoint-for-hello-europe-app"></a>A Traffic Manager-végpont hello Európa alkalmazás hozzáadása
Használjon [az hálózati forgalmat-manager-végpont létrehozása](https://docs.microsoft.com/cli/azure/network/traffic-manager/endpoint#create) tooadd egy végpont tooyour Traffic Manager-profil és a használata hello erőforrás-azonosító szerint webalkalmazás hello cél.

```azurecli
az network traffic-manager endpoint create --name myWestEuropeEndpoint --profile-name myTrafficManagerProfile --resource-group myResourceGroup --type azureEndpoints --target-resource-id $appId
```

### <a name="get-hello-traffic-manager-endpoint-url"></a>Hello Traffic Manager-végpont URL-cím beszerzése
A Traffic Manager-profil adott pontok tooyour már meglévő webalkalmazás most már rendelkezik egy végpontot. Használjon [az hálózati forgalmat-kezelő profil megjelenítése](https://docs.microsoft.com/cli/azure/network/traffic-manager/profile#show) tooget az URL-CÍMÉT. 

```azurecli
az network traffic-manager profile show --name myTrafficManagerProfile --resource-group myResourceGroup --query dnsConfig.fqdn --output tsv
```

Hello kimenetének másolása HTML-a böngészőbe. A webalkalmazás ismét meg kell jelennie.

### <a name="create-an-azure-redis-cache-in-asia"></a>Hozzon létre egy Azure Redis Cache Ázsiában
Most az Azure web app toohello Délkelet-Ázsia régióba replikálja. toostart, használjon [az redis létrehozása](https://docs.microsoft.com/en-us/cli/azure/redis#create) toocreate egy második Azure Redis Cache-ben Délkelet-Ázsia. Ez a gyorsítótár Ázsiában alkalmazása közösen elhelyezett toobe kell.

```powershell
$redis = (az redis create --name $cacheName-asia --resource-group myResourceGroup --location "Southeast Asia" --sku-capacity 0 --sku-family C --sku-name Basic | ConvertFrom-Json)
```

`--name $cacheName-asia`által biztosított hello gyorsítótár hello neve hello Nyugat-Európában gyorsítótár, a hello `-asia` utótag.

### <a name="create-an-app-service-plan-in-asia"></a>Az App Service-csomag létrehozása Ázsiában
Használjon [az App Service-csomagot hozzon létre](https://docs.microsoft.com/cli/azure/appservice/plan#create) toocreate egy második App Service-csomag hello Délkelet-Ázsia régióban, azonos S1 réteg hello segítségével hello Nyugat-Európában terv szerint.

```azurecli
az appservice plan create --name myAppServicePlanAsia --resource-group myResourceGroup --location "Southeast Asia" --sku S1
```

### <a name="create-a-web-app-in-asia"></a>A webalkalmazás létrehozása az Ázsia
Használjon [az App Service webalkalmazás létrehozása](https://docs.microsoft.com/en-us/cli/azure/appservice/web#create) toocreate egy másik webes alkalmazást.

```azurecli
az appservice web create --name $appName-asia --resource-group myResourceGroup --plan myAppServicePlanAsia
```

`--name $appName-asia`által biztosított hello hello alkalmazásnév hello Nyugat-Európában alkalmazáshoz, a hello `-asia` utótag.

### <a name="configure-hello-connection-string-for-redis"></a>A Redis hello kapcsolati karakterlánc konfigurálása
Használjon [az App Service web config appsettings frissítése](https://docs.microsoft.com/cli/azure/appservice/web/config/appsettings#update) tooadd toohello web app hello kapcsolati karakterláncot hello Délkelet-Ázsia gyorsítótár.

az App Service web config appsettings frissítése – beállítások "RedisConnection$ ($redis.hostname) =: $($redis.sslPort), a jelszó$ ($redis.accessKeys.primaryKey) ssl = = True, abortConnect = False"--name $appName-Ázsia--erőforráscsoport myResourceGroup

### <a name="configure-git-deployment-for-hello-asia-app"></a>Hello Ázsia alkalmazás Git-KözpontiTelepítés konfigurálása.
Használjon [az App Service web verziókezelő config-helyi-git](https://docs.microsoft.com/en-us/cli/azure/appservice/web/source-control#config-local-git) tooconfigure helyi Git-telepítésének hello második webalkalmazás.

```azurecli
az appservice web source-control config-local-git --name $appName-asia --resource-group myResourceGroup
```

Ez a parancs lehetővé teszi, hogy a következőhöz hasonló hello kimenetnek:

```json
{
  "url": "https://user123@myuniqueappname-asia.scm.azurewebsites.net/myuniqueappname.git"
}
```

Használjon hello visszaadott URL-cím tooconfigure egy második Git a helyi tárház távoli. hello következő parancs hello megelőző példa a kimenetre.

```bash
git remote add azure-asia https://user123@myuniqueappname-asia.scm.azurewebsites.net/myuniqueappname.git
```

### <a name="deploy-your-sample-application"></a>A mintaalkalmazás telepítése
Futtatás `git push` toodeploy a minta alkalmazás toohello második Git távoli. 

```bash
git push azure-asia master
```

A jelszó megadását kéri, ha a megadott kulcsfájloknak hello-jelszót használja `az appservice web deployment user set`.

### <a name="browse-toohello-asia-app"></a>Keresse meg a toohello Ázsia alkalmazás
Használjon [az App Service web Tallózás](https://docs.microsoft.com/en-us/cli/azure/appservice/web#browse) tooverify, hogy az alkalmazást működés közben fut az Azure-ban.

```azurecli
az appservice web browse --name $appName-asia --resource-group myResourceGroup
```

### <a name="get-hello-resource-id-of-hello-asia-app"></a>Erőforrás-azonosító hello hello Ázsia alkalmazás beszerzése
Használjon [az App Service web megjelenítése](https://docs.microsoft.com/en-us/cli/azure/appservice/web#show) tooget hello erőforrás-azonosító Délkelet-Ázsiában található webalkalmazás.

```azurecli
$appIdAsia = az appservice web show --name $appName-asia --resource-group myResourceGroup --query id --output tsv
```

### <a name="add-a-traffic-manager-endpoint-for-hello-asia-app"></a>A Traffic Manager-végpont hello Ázsia alkalmazás hozzáadása
Használjon [az hálózati forgalmat-manager-végpont létrehozása](https://docs.microsoft.com/cli/azure/network/traffic-manager/endpoint#create) tooadd egy második végpont toohello Traffic Manager-profil.

```azurecli
az network traffic-manager endpoint create --name myAsiaEndpoint --profile-name myTrafficManagerProfile --resource-group myResourceGroup --type azureEndpoints --target-resource-id $appIdAsia
```

### <a name="add-region-identifier-tooweb-apps"></a>Régió azonosítója tooweb alkalmazások hozzáadása
Használjon [az App Service web config appsettings frissítése](https://docs.microsoft.com/cli/azure/appservice/web/config/appsettings#update) tooadd egy régióspecifikus környezeti változót.

```azurecli
az appservice web config appsettings update --settings "Region=West Europe" --name $appName --resource-group myResourceGroup
az appservice web config appsettings update --settings "Region=Southeast Asia" --name $appName-asia --resource-group myResourceGroup
```

Az alkalmazás kódjában már használja az alkalmazás-beállítás. Vessen egy pillantást `HighScaleApp\Views\Home\Index.cshtml`.

### <a name="complete"></a>Fejezze be!

Most próbáljon meg a Traffic Manager-profil a különböző földrajzi régióban böngészőkkel történő tooaccess hello URL-CÍMÉT. Ügyfélböngészők Európából megjelennek az "ASP.NET Nyugat-Európa", és az Ázsia ügyfélböngészőnek megjelennek az "ASP.NET Délkelet-Ázsia."

## <a name="more-resources"></a>További erőforrások
