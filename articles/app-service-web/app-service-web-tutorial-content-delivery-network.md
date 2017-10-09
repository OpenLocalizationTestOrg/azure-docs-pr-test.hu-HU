---
title: a CDN tooan Azure App Service aaaAdd |} Microsoft Docs
description: "Adja hozzá a Content Delivery Network (CDN) tooan Azure App Service toocache, és Bezárás tooyour ügyfelek hello világ biztosításához a statikus fájlok a kiszolgálókról."
services: app-service\web
author: syntaxc4
ms.author: cfowler
ms.date: 05/31/2017
ms.topic: tutorial
ms.service: app-service-web
manager: erikre
ms.workload: web
ms.custom: mvc
ms.openlocfilehash: 88b7fd884517279064472b804a6d1dc2921cbd24
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-a-content-delivery-network-cdn-tooan-azure-app-service"></a>Adja hozzá a Content Delivery Network (CDN) tooan Azure App Service

[Az Azure Content Delivery Network (CDN)](../cdn/cdn-overview.md) gyorsítótárazza a statikus webtartalmakat stratégiailag kiválasztott helyeken tooprovide maximális átviteli sebesség tartalom toousers kézbesítéséhez. hello CDN is csökkenti a kiszolgáló terhelését a webalkalmazásban. Ez az oktatóanyag bemutatja, hogyan tooadd Azure CDN tooa [az Azure App Service webalkalmazás](app-service-web-overview.md). 

Íme hello kezdőlap hello minta statikus HTML hely fog dolgozni:

![Mintaalkalmazás kezdőlapja](media/app-service-web-tutorial-content-delivery-network/sample-app-home-page.png)

Ismertetett témák:

> [!div class="checklist"]
> * CDN-végpont létrehozása.
> * Gyorsítótárazott objektumok frissítése.
> * Használjon lekérdezési karakterláncok gyorsítótárazott toocontrol verziók.
> * Használja az egyéni tartománynév hello CDN-végponthoz.

## <a name="prerequisites"></a>Előfeltételek

toocomplete Ez az oktatóanyag:

- [A Git telepítése](https://git-scm.com/)
- [Az Azure CLI 2.0 telepítése](https://docs.microsoft.com/cli/azure/install-azure-cli)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-hello-web-app"></a>Hello webalkalmazás létrehozása

toocreate hello webes alkalmazás is együttműködik, hajtsa végre hello [statikus HTML gyors üzembe helyezés](app-service-web-get-started-html.md) keresztül hello **Tallózás toohello app** lépés.

### <a name="have-a-custom-domain-ready"></a>Rendelkezésre álló egyéni tartománynév

toocomplete hello egyéni tartománynév lépéssel ebben az oktatóanyagban a tooown az egyéni tartománynév kell, és hozzáférést tooyour DNS beállításjegyzék rendelkezik a tartomány-szolgáltató (például a GoDaddy). Például tooadd DNS-bejegyzéseket `contoso.com` és `www.contoso.com`, rendelkeznie kell hozzáféréssel tooconfigure hello DNS-beállításainak hello `contoso.com` gyökértartomány.

Ha még nem rendelkezik egy tartomány nevét, fontolja meg a következő hello [App Service tartomány oktatóanyag](custom-dns-web-site-buydomains-web-app.md) egy tartomány használatával toopurchase hello Azure-portálon. 

## <a name="log-in-toohello-azure-portal"></a>Jelentkezzen be toohello Azure-portálon

Nyisson meg egy böngészőt, és keresse meg a toohello [Azure-portálon](https://portal.azure.com).

## <a name="create-a-cdn-profile-and-endpoint"></a>CDN-profil és -végpont létrehozása

A bal oldali navigációs hello, válassza ki **alkalmazásszolgáltatások**, majd válassza ki a hello hello alkalmazást [statikus HTML gyors üzembe helyezés](app-service-web-get-started-html.md).

![Válassza ki az App Service alkalmazás hello portálon](media/app-service-web-tutorial-content-delivery-network/portal-select-app-services.png)

A hello **App Service** lap hello **beállítások** szakaszban jelölje be **hálózati > konfigurálása Azure CDN szolgáltatás használata az alkalmazás**.

![Válassza ki a CDN hello portálon](media/app-service-web-tutorial-content-delivery-network/portal-select-cdn.png)

A hello **Azure Content Delivery Network** lapján adja meg a hello **új végpont** beállításokat hello táblázatban megadottak szerint.

![Hello portálon profil és -végpont létrehozása](media/app-service-web-tutorial-content-delivery-network/portal-new-endpoint.png)

| Beállítás | Ajánlott érték | Leírás |
| ------- | --------------- | ----------- |
| **CDN-profil** | myCDNProfile | Válassza ki **hozzon létre új** toocreate a CDN-profil. A CDN-profil hello a CDN-végpontok gyűjteménye azonos IP-címek. |
| **Tarifacsomag** | Akamai Standard | Hello [tarifacsomag](../cdn/cdn-overview.md#azure-cdn-features) hello szolgáltató és elérhető funkciókkal határozza meg. Ebben az oktatóanyagban Standard Akamai használunk. |
| **CDN-végpont neve** | Hello azureedge.net tartomány egyedi nevek | A gyorsítótárazott erőforrások hello tartományban  *\<endpointname >. azureedge.net*.

Kattintson a **Létrehozás** gombra.

Azure hello-profil és -végpont létrehozása hello új végpont megjelenik hello **végpontok** listájából hello ugyanazon az oldalon, és ha ki van építve hello állapota **futtató**.

![Új végpont a listában](media/app-service-web-tutorial-content-delivery-network/portal-new-endpoint-in-list.png)

### <a name="test-hello-cdn-endpoint"></a>Teszt hello CDN-végpont

A Verizon tarifacsomag kiválasztása esetén a végpontok propagálása körülbelül 90 percet vesz igénybe. Az Akamai esetében a terjesztés csak néhány percig tart

hello mintaalkalmazás rendelkezik egy `index.html` fájl és *css*, *img*, és *js* egyéb statikus eszközök tartalmazó mappákat. a tartalom elérési az alábbi fájlokat hello hello CDN-végpont hello azonos. Például mindkét következő URL-címek hello hozzáférési hello *bootstrap.css* hello fájlban *css* mappába:

```
http://<appname>.azurewebsites.net/css/bootstrap.css
```

```
http://<endpointname>.azureedge.net/css/bootstrap.css
```

Keresse meg a böngésző toohello a következő URL-címe:

```
http://<endpointname>.azureedge.net/index.html
```

![A mintaalkalmazás CDN által szolgáltatott kezdőlapja](media/app-service-web-tutorial-content-delivery-network/sample-app-home-page-cdn.png)

 Láthatja, hogy futtatta-e korábban az Azure-webalkalmazás lapon azonos hello. Az Azure CDN fogadta hello származási web app eszközök, és van szolgál a hello CDN-végpont

hogy ezen a lapon tárolja a CDN, frissítési hello lap hello tooensure. Két kéri a hello azonos eszköz néha szükség hello CDN toocache hello lekért tartalom.

Az Azure CDN-profilok és -végpontok létrehozásával kapcsolatos további információkért lásd: [Az Azure CDN használatának első lépései](../cdn/cdn-create-new-endpoint.md).

## <a name="purge-hello-cdn"></a>Hello CDN kiürítése

hello CDN hello származási webalkalmazás hello--élettartama (TTL) konfigurációja alapján erőforrását rendszeresen frissülnek. hello alapértelmezett élettartam érték hét nap.

Néha szükség lehet toorefresh hello CDN előtt hello TTL lejárata – például frissített tartalom toohello webes alkalmazás központi telepítésekor. tootrigger frissítését, manuálisan törölheti hello CDN erőforrásokat. 

Ebben a szakaszban hello oktatóanyag módosítása toohello webalkalmazás üzembe helyezése, és hello CDN tootrigger hello CDN toorefresh a gyorsítótár kiürítése.

### <a name="deploy-a-change-toohello-web-app"></a>Változás toohello webalkalmazás üzembe helyezése

Nyissa meg hello `index.html` fájlt, és "-V2" toohello H1 címsor, ahogy az alábbi példa hello: 

```
<h1>Azure App Service - Sample Static HTML Site - V2</h1>
```

A módosítás véglegesítése, és telepítse azt toohello webalkalmazás.

```bash
git commit -am "version 2"
git push azure master
```

Központi telepítés befejezése után a Tallózás toohello webes alkalmazás URL-CÍMÉT, és tekintse meg a hello módosítása.

```
http://<appname>.azurewebsites.net/index.html
```

![V2 a webapp címében](media/app-service-web-tutorial-content-delivery-network/v2-in-web-app-title.png)

Keresse meg a toohello CDN-végpont URL-címe hello kezdőlapjára, és nem jelenik meg a hello módosítani, mert a gyorsítótárazott verzió hello hello CDN még a még nem járt le. 

```
http://<endpointname>.azureedge.net/index.html
```

![A V2 nem jelenik meg a CDN-beli címben](media/app-service-web-tutorial-content-delivery-network/no-v2-in-cdn-title.png)

### <a name="purge-hello-cdn-in-hello-portal"></a>Hello CDN hello portálon kiürítése

tootrigger hello CDN tooupdate a gyorsítótárazott verzió, hello CDN kiürítése.

Válassza ki a portál bal oldali navigációs hello, **erőforráscsoportok**, majd válassza ki a webalkalmazás (contoso.com) létrehozott erőforráscsoport hello.

![Erőforráscsoport kiválasztása](media/app-service-web-tutorial-content-delivery-network/portal-select-group.png)

Az erőforrások listájához hello válassza ki a CDN-végpontot.

![Végpont kiválasztása](media/app-service-web-tutorial-content-delivery-network/portal-select-endpoint.png)

Hello hello tetején **végpont** kattintson **kiürítése**.

![Végleges törlés kiválasztva](media/app-service-web-tutorial-content-delivery-network/portal-select-purge.png)

Adja meg a tartalomelérési út hello toopurge kívánja. A fájl teljes elérési útja toopurge át egyetlen fájl vagy egy elérési utat szegmens toopurge, és frissítse egy mappát teljes tartalmával. Mivel módosította `index.html`, győződjön meg arról, hogy a hello elérési utak egyike.

Hello hello lap alsó részén, jelölje ki a **kiürítése**.

![Oldal végleges törlése](media/app-service-web-tutorial-content-delivery-network/app-service-web-purge-cdn.png)

### <a name="verify-that-hello-cdn-is-updated"></a>Győződjön meg arról, hogy hello CDN frissül.

Várjon, amíg hello kiürítési kérés befejezze a feldolgozást, általában néhány percig. toosee hello aktuális állapotát, jelölje be hello harang ikonra hello oldal hello tetején. 

![Törlési értesítés](media/app-service-web-tutorial-content-delivery-network/portal-purge-notification.png)

Keresse meg a toohello CDN végponti URL-Címének `index.html`, és ekkor megjelenik a hello toohello címét hozzáadta hello kezdőlapján V2. Ez azt jelenti, hogy hello CDN gyorsítótár frissítése.

```
http://<endpointname>.azureedge.net/index.html
```

![V2 a CDN-beli címben](media/app-service-web-tutorial-content-delivery-network/v2-in-cdn-title.png)

További információkért lásd az [Azure CDN-végpontok végleges törléséről](../cdn/cdn-purge-endpoint.md) szóló cikket. 

## <a name="use-query-strings-tooversion-content"></a>Lekérdezési karakterláncok tooversion tartalom használata

hello Azure CDN alábbi gyorsítótárazási viselkedés beállítások hello kínálja:

* Lekérdezési karakterláncok figyelmen kívül hagyása
* Lekérdezési karakterláncok gyorsítótárazásának megkerülése
* Minden egyedi URL gyorsítótárazása 

először hello ezek hello alapértelmezett, ami azt jelenti, hogy egy eszköz függetlenül hello lekérdezési karakterlánc hello URL-cím csak egy gyorsítótárazott verzió van. 

Ebben a szakaszban hello oktatóanyag módosítja hello viselkedés toocache minden egyedi URL-cím gyorsítótárazása.

### <a name="change-hello-cache-behavior"></a>Gyorsítótár-viselkedést hello módosítása

Az Azure-portálon hello **CDN-végpont** lapon jelölje be **gyorsítótár**.

Válassza ki **minden egyedi URL-cím gyorsítótárazása** a hello **lekérdezési sztringek gyorsítótárazásának** legördülő listából.

Kattintson a **Mentés** gombra.

![Lekérdezési karakterláncok gyorsítótárazási működésének kiválasztása](media/app-service-web-tutorial-content-delivery-network/portal-select-caching-behavior.png)

### <a name="verify-that-unique-urls-are-cached-separately"></a>Az egyedi URL-címek külön gyorsítótárazásának ellenőrzése

Egy böngészőben nyissa meg a kezdőlapján toohello hello CDN-végpont, de közé tartozik a lekérdezési karakterlánc: 

```
http://<endpointname>.azureedge.net/index.html?q=1
```

hello CDN hello aktuális webes alkalmazás tartalmát, amelyet "2" hello fejléc tartalmazza adja vissza. 

hogy ezen a lapon tárolja a CDN, frissítési hello lap hello tooensure. 

Nyissa meg `index.html` túl módosítsa a "2" és "V3", valamint hello. 

```bash
git commit -am "version 3"
git push azure master
```

Egy böngészőben nyissa meg toohello CDN végponti URL-cím új lekérdezési karakterláncot, mint `q=2`. hello CDN lekérdezi hello aktuális `index.html` fájlt, és megjeleníti a "V3".  De ha manuálisan lép toohello CDN-végpont a hello `q=1` lekérdezési karakterlánc, lásd a "2".

```
http://<endpointname>.azureedge.net/index.html?q=2
```

![V3 a CDN-beli címben, 2. lekérdezési karakterlánc](media/app-service-web-tutorial-content-delivery-network/v3-in-cdn-title-qs2.png)

```
http://<endpointname>.azureedge.net/index.html?q=1
```

![V2 a CDN-beli címben, 1. lekérdezési karakterlánc](media/app-service-web-tutorial-content-delivery-network/v2-in-cdn-title-qs1.png)

A kimeneti jeleníti meg, hogy minden egyes lekérdezési karakterlánc eltérően kell-e kezelni:

* q = 1 használt előtt, így a gyorsítótárazott tartalom (V2) ad vissza.
* q = 2 egy új, így hello legújabb web app tartalom letöltésének és (V3) adott vissza.

További információkért lásd: [Az Azure CDN gyorsítótárazási viselkedésének vezérlése lekérdezési karakterláncokkal](../cdn/cdn-query-string.md).

## <a name="map-a-custom-domain-tooa-cdn-endpoint"></a>Az egyéni tartomány tooa CDN-végpont leképezése

Az egyéni tartomány tooyour CDN-végpont hozzon létre egy CNAME rekordot kell rendelni. Egy olyan CNAME rekordot a DNS szolgáltatás, amely leképezi a forrás tartományi tooa cél tartomány. Például, hogy le lehet képezni `cdn.contoso.com` vagy `static.contoso.com` túl`contoso.azureedge.net`.

Ha egyéni tartományt nem rendelkezik, fontolja meg hello [App Service tartomány oktatóanyag](custom-dns-web-site-buydomains-web-app.md) egy tartomány használatával toopurchase hello Azure-portálon. 

### <a name="find-hello-hostname-toouse-with-hello-cname"></a>A hello CNAME hello állomásnév toouse keresése

A hello Azure-portálon **végpont** lapon, győződjön meg arról, hogy **áttekintése** van kiválasztva a bal oldali navigációs, és jelölje ki hello hello **+ egyéni tartomány** hello oldal hello tetején gombra.

![Egyéni tartomány hozzáadása kiválasztva](media/app-service-web-tutorial-content-delivery-network/portal-select-add-domain.png)

A hello **hozzá egyéni tartományt** lapon látni hello végpont állomás neve toouse a CNAME rekord létrehozásával. hello állomásnevet a CDN-végpont URL-címe van származtatva:  **&lt;EndpointName >. azureedge.net**. 

![Tartomány hozzáadása lap](media/app-service-web-tutorial-content-delivery-network/portal-add-domain.png)

### <a name="configure-hello-cname-with-your-domain-registrar"></a>Hello CNAME REKORDOT a tartományregisztrálónál konfigurálása

Keresse meg a tooyour tartomány regisztráló webhelyén, és keresse meg a DNS-rekordok létrehozásához hello szakaszt. Ezt a **Tartománynév**, **DNS**, **Névkiszolgáló kezelése** vagy hasonló területen találja.

Hello szakaszban található CNAME kezeléséhez. Valószínűleg toogo tooan speciális beállításai oldal és CNAME, az Alias vagy a altartományok hello szavakat keresi.

Hozzon létre egy CNAME rekordot, amely leképezhető a kiválasztott altartomány (például **statikus** vagy **cdn**) toohello **végpont állomásnév** korábbi hello portal látható. 

### <a name="enter-hello-custom-domain-in-azure"></a>Adja meg az egyéni tartomány hello az Azure-ban

Térjen vissza a toohello **hozzá egyéni tartományt** lapon, és adja meg az egyéni tartomány, hello altartomány, beleértve a hello párbeszédpanelen. Adja meg például a következőt: `cdn.contoso.com`.   
   
Azure ellenőrzi, hogy létezik-e hello CNAME rekord megadott hello tartománynév. Ha hello CNAME helyességéről, az egyéni tartomány érvényességét.

Hello CNAME rekord toopropagate tooname kiszolgálók hello Internet időt is igénybe vehet. Ha a tartomány nincs érvényesítve azonnal, várjon néhány percet, és próbálkozzon újra.

### <a name="test-hello-custom-domain"></a>Teszt hello egyéni tartományt

Egy böngészőben nyissa meg a toohello `index.html` fájlt az egyéni tartomány (például `cdn.contoso.com/index.html`), amely hello eredmény tooverify hello azonos, amikor módba közvetlenül túl`<endpointname>azureedge.net/index.html`.

![Mintaalkalmazás egyéni tartományi URL-címet használó kezdőlapja](media/app-service-web-tutorial-content-delivery-network/home-page-custom-domain.png)

További információkért lásd: [tartalom tooa egyéni térkép Azure CDN-tartomány](../cdn/cdn-map-content-to-custom-domain.md).

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Következő lépések

Hogy mit tudott:

> [!div class="checklist"]
> * CDN-végpont létrehozása.
> * Gyorsítótárazott objektumok frissítése.
> * Használjon lekérdezési karakterláncok gyorsítótárazott toocontrol verziók.
> * Használja az egyéni tartománynév hello CDN-végponthoz.

Megtudhatja, hogyan toooptimize CDN teljesítmény hello a következő cikkek:

> [!div class="nextstepaction"]
> [A teljesítmény javítása a fájlok tömörítésével az Azure CDN-ben](../cdn/cdn-improve-performance.md)

> [!div class="nextstepaction"]
> [Eszközök előzetes betöltése Azure CDN-végponton](../cdn/cdn-preload-endpoint.md)
