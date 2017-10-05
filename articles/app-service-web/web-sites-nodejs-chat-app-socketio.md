---
title: "Node.js csevegőalkalmazás létrehozása a Socket.IO segítségével az Azure App Service-ben"
description: "Ez az oktatóanyag bemutatja a socket.io segítségével az Azure-platformon futó node.js webalkalmazás."
services: app-service\web
documentationcenter: nodejs
author: TomArcher
manager: routlaw
editor: 
ms.assetid: c4c4af36-3ecf-4619-b586-ca90d53ce35b
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 08/17/2016
ms.author: tarcher
ms.openlocfilehash: e1aa539e1134884261ea7464bfda6d14815618d4
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="create-a-nodejs-chat-application-with-socketio-in-azure-app-service"></a>Node.js csevegőalkalmazás létrehozása a Socket.IO segítségével az Azure App Service-ben
Socket.IO biztosít a valós idejű kommunikációját a node.js-kiszolgáló és az ügyfél websocket elemeket. Visszalépnek más átvitelek (például hosszú lekérdezési), amely a régebbi böngészők is támogatja. Ez az oktatóanyag végigvezetik egy alapú Socket.IO Csevegés alkalmazásra, amely egy Azure webalkalmazás üzemeltetéséhez, és bemutatják a használó méretezni [Azure Redis Cache]. A Socket.IO további információkért lásd: <http://socket.io/>.

> [!NOTE]
> Ebben a feladatban eljárásokat alkalmazni [App Service Web Apps]; Felhőszolgáltatásai számára, lásd: [Socket.IO segítségével egy Node.js Csevegés alkalmazás létrehozása az Azure-Felhőszolgáltatásban].
> 
> 

## <a name="download-the-chat-example"></a>Töltse le a Csevegés – példa
Ebben a projektben a Csevegés példa fogjuk használni a [Socket.IO GitHub-tárházban]. A következő lépésekkel töltse le a példában, és adja hozzá a projekthez, korábban létrehozott.

1. Töltse le a [ZIP- vagy GZ archivált kiadás] a Socket.IO projekt (a dokumentum a 1.3.5 verziója lett megadva)
2. Bontsa ki a az archiválás és másolja a **példák\\Csevegés** könyvtár, egy új helyre. Például  **\\csomópont\\Csevegés**.

## <a name="modify-appjs-and-install-modules"></a>Az App.js fájl módosítása és -modulok telepítése
1. Nevezze át a **index.js** fájl **app.js**. Ez lehetővé teszi az Azure észleli, hogy ez a Node.js-alkalmazás.
2. Nyissa meg a **app.js** fájlt egy szövegszerkesztőben. Módosítsa a sor tartalmazó `var io = require('../..')(server);` alább látható módon:
   
       var express = require('express');
       var app = express();
       var server = require('http').createServer(app);
       // var io = require('../..')(server);
       // New:
       var io = require('socket.io')(server);
       var port = process.env.PORT || 3000;
3. Nyissa meg a **package.json** fájlt, és adjon hozzá egy hivatkozást, a socket.io `dependencies`, az alábbiak szerint:
   
        "dependencies": {
          "express": "3.4.8",
          "socket.io": "1.3.5"
        }
4. A parancssorból, módosítsa a  **\\csomópont\\Csevegés** könyvtárra, és használja a jelen alkalmazás által igényelt moduljainak telepítése npm:
   
        npm install
   
    Ezzel telepíti a modulok nevű almappába **node_modules**.

## <a name="create-an-azure-web-app"></a>Azure-webalkalmazás létrehozása
A lépések végrehajtásával hozzon létre egy Azure webalkalmazás Git-közzététel engedélyezése és engedélyeznie kell a webalkalmazás WebSocket támogatása.

> [!NOTE]
> Az oktatóanyag elvégzéséhez egy Azure-fiókra lesz szüksége. Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot. További információkért lásd: <a href="http://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A7171371E" target="_blank">Ingyenes Azure-fiók létrehozása</a>.
> 
> 

1. Az Azure parancssori felület (CLI) telepítése, és csatlakozzon az Azure-előfizetéshez. Lásd: [telepítése és konfigurálása az Azure parancssori felület](../cli-install-nodejs.md).
2. Ha ez az első idő beállítása a tárházat az Azure-ban, a bejelentkezési hitelesítő adatok létrehozásához szüksége. Az Azure parancssori felületen adja meg a következő parancsot:
   
        azure site deployment user set [username] [password]
3. Módosítsa a  **\\node\chat** könyvtárra, és használja a következő paranccsal hozhat létre egy új Azure web app és egy helyi Git-tárházat. Ez a parancs is létrehoz egy Git távoli elnevezett "azure".
   
        azure site create mysitename --git
   
    A webalkalmazás egyedi nevű, "mysitename" le kell cserélnie.
4. Véglegesítse a meglévő fájlokat a helyi tárházba a következő parancsokkal:
   
        git add .
        git commit -m "Initial commit"
5. A fájlok az Azure Web Apps tárház leküldése a következő paranccsal:
   
        git push azure master
   
    Amikor a rendszer kéri, adja meg a hitelesítő adatokat a 2. lépésben. Állapotüzenetek fog kapni, mivel a rendszer importálja a kiszolgálón. Ez a folyamat befejezése után az alkalmazás fogja üzemeltetni az Azure-webalkalmazásban.
   
   > [!NOTE]
   > Modul a telepítés során azt tapasztalhatja, hibák, amelyek "... az importált projekt nem található". Ezek biztonságosan figyelmen kívül hagyható.
   > 
   > 
6. Socket.IO használ a websocket elemek, amelyek az Azure-on alapértelmezés szerint nem engedélyezettek. Webes szoftvercsatornák engedélyezéséhez használja a következő parancsot:
   
        azure site set -w
   
    Ha a rendszer kéri, adja meg a webalkalmazás nevét.
   
   > [!NOTE]
   > A "az azure site set -w" parancs fog csak verziójával 0.7.4 munkahelyi vagy az Azure parancssori felület magasabb. WebSocket-támogatás segítségével is engedélyezheti a [Azure Portal](https://portal.azure.com).
   > 
   > Ahhoz, hogy a websocket elemek az Azure portál használatával, kattintson a webalkalmazás a Web Apps paneljéről, kattintson **összes beállítás** > **Alkalmazásbeállítások**. A **webes szoftvercsatornák**, kattintson a **a**. Ezután kattintson a **Save** (Mentés) gombra.
   > 
   > 
7. A webalkalmazás Azure megtekintéséhez a következő paranccsal indítsa el a webböngészőt, és keresse meg a futtatott webalkalmazás:
   
        azure site browse

Az alkalmazás mostantól az Azure-on fut, és továbbíthat csevegési üzeneteket a Socket.IO segítségével különböző ügyfelek között.

## <a name="scale-out"></a>Horizontális felskálázás
Socket.IO alkalmazások kiterjeszthető használatával egy **adapter** üzenetek és több alkalmazáspéldányt események terjesztését. Több adapterek nincsenek elérhető, amíg a [socket.io-redis] adapter könnyen használható az Azure Redis Cache szolgáltatással.

> [!NOTE]
> Egy további követelmény a Socket.IO megoldás kiterjesztése a kapcsolódó munkamenetek támogatása. A kapcsolódó munkamenetek alapértelmezés szerint engedélyezve vannak a Azure Web Apps használatával Azure alkalmazáskérelmek irányítására. További információkért lásd: [példány kapcsolat az Azure-webhelyek].
> 
> 

### <a name="create-a-redis-cache"></a>A Redis gyorsítótár létrehozása
Hajtsa végre a lépéseket [a gyorsítótár létrehozása az Azure Redis Cache] új gyorsítótárat létrehozni.

> [!NOTE]
> Mentse a **állomásnév** és **elsődleges kulcs** a gyorsítótárhoz, ezek szükség lesz a következő lépésben.
> 
> 

### <a name="add-the-redis-and-socketio-redis-modules"></a>Adja hozzá a redis és socket.io-redis-modulok
1. A parancssorból, módosítsa a  **\\csomópont\\Csevegés** könyvtárra, és használja a következő parancsot.
   
        npm install socket.io-redis@0.1.4 redis@0.12.1 --save
   
   > [!NOTE]
   > Ez a parancs-ben megadott verzió a-e ez a cikk tesztelése során használt.
   > 
   > 
2. Módosítsa a **app.js** fájlt adja hozzá a következő sor után azonnal`var io = require('socket.io')(server);`
   
        var pub = require('redis').createClient(6379,'redishostname', {auth_pass: 'rediskey', return_buffers: true});
        var sub = require('redis').createClient(6379,'redishostname', {auth_pass: 'rediskey', return_buffers: true});
   
        var redis = require('socket.io-redis');
        io.adapter(redis({pubClient: pub, subClient: sub}));
   
    Cserélje le **redishostname** és **rediskey** az állomásnév és a Redis gyorsítótár kulccsal.
   
    Ez a publish készít, és ügyfél számára a korábban létrehozott Redis gyorsítótárt fizessen elő. Az ügyfelek majd a adapterrel konfigurálására szolgálnak az üzenetek és események továbbításához az alkalmazás példányai között a Redis gyorsítótárt használandó Socket.IO
   
   > [!NOTE]
   > Amíg a **socket.io-redis** adapter kommunikálhatnak közvetlenül a Redis, hogy a jelenlegi verziója nem támogatja az Azure Redis cache-szükséges hitelesítés. Ezért a kezdeti kapcsolat létrehozása a **redis** modul, akkor az ügyfél átadott a **socket.io-redis** adapter.
   > 
   > Azure Redis Cache-példányokhoz biztonságos 6380 portot használ, miközben ebben a példában használt modulok nem támogatja a biztonságos kapcsolatokat 7/14/2014-től. A fenti kódot használja az alapértelmezett, 6379 nem biztonságos portja.
   > 
   > 
3. Mentse a módosított **app.js**

### <a name="commit-changes-and-redeploy"></a>Véglegesítse a módosításokat, és telepítse újra
Parancsot a parancssorból a  **\\csomópont\\Csevegés** directory, a következő parancsok segítségével a változtatások véglegesítése a határidő, és telepítse újra az alkalmazást.

    git add .
    git commit -m "implementing scale out"
    git push azure master

Miután a változások értesítését a kiszolgálóra, a következő paranccsal a webhely méretezheti több példánya között.

    azure site scale instances --instances #

Ha  **#**  létrehozásához példányok száma.

A webalkalmazás több böngészők vagy számítógépek számára, győződjön meg arról, hogy megfelelően küldi el az összes ügyfél csatlakozhat.

## <a name="troubleshooting"></a>Hibaelhárítás
### <a name="connection-limits"></a>Kapcsolat korlátai
Az Azure Web Apps több SKU, amelyek megadják a webhely számára elérhető erőforrások érhető el. Ez magában foglalja az engedélyezett WebSocket-kapcsolatok száma. További információkért lásd: a [Web Apps árképzést ismertető oldalra].

### <a name="messages-arent-being-sent-using-websockets"></a>Üzenetek nem küldött websocket elemek használatával
Ha az ügyfelek böngészőin tartsa visszaváltás websocket elemek használata helyett hosszú jelenség, lehet a következők valamelyike miatt.

* **Próbálja a átvitel, csak a websocket elemek korlátozása**
  
    Ahhoz, hogy az üzenetkezelési átvitel websocket elemek használandó Socket.IO a kiszolgáló és az ügyfél támogatnia kell websocket elemeket. Ha az egyiket nem, a Socket.IO egyezteti egy másik átviteltől, pl. hosszú lekérdezési. Az átviteli módokat Socket.IO által használt alapértelmezett listája ` websocket, htmlfile, xhr-polling, jsonp-polling`. Beállíthatja úgy, hogy csak használjon szoftvercsatornák használatával adja hozzá az alábbi kódot a **app.js** fájl, a sor tartalmazó után `, nicknames = {};`.
  
        io.configure(function() {
          io.set('transports', ['websocket']);
        });
  
  > [!NOTE]
  > Figyelje meg, hogy a régebbi websocket elemek nem támogató böngészők nem tudnak kapcsolódni a helyhez, amíg a fenti kódot aktív, websocket elemek csak kommunikációt korlátozza.
  > 
  > 
* **SSL használata**
  
    Websocket elemek, mint támaszkodik néhány kisebb használt HTTP-fejléceket, a **frissítése** fejléc. Néhány, a közbenső hálózati eszközök, például webes proxykat, ezek a fejlécek távolíthatja el. Ez a probléma elkerülése érdekében létrehozhat a WebSocket-kapcsolat SSL-en keresztül.
  
    Ehhez egyszerűen, hogy konfigurálja a Socket.IO `match origin protocol`. Ez arra utasítja a Socket.IO ugyanaz, mint az eredeti HTTP/HTTPS-kérést a weblap websocket elemek kommunikáció biztonságossá tételére. Ha egy böngésző és látogasson el a webhely egy HTTPS URL-címet használ, a további WebSocket kommunikáció Socket.IO segítségével megfelelően történik SSL-en keresztül.
  
    Ebben a példában ahhoz, hogy ez a konfiguráció módosításához adja hozzá a következő kódot a **app.js** fájlt, miután a sor tartalmazó `, nicknames = {};`.
  
        io.configure(function() {
          io.set('match origin protocol', true);
        });
* **Ellenőrizze a web.config beállításait**
  
    Azure-webalkalmazásokban üzemeltető Node.js alkalmazások használatát a **web.config** továbbítani a beérkező kéréseket a Node.js-alkalmazás a fájlt. A Node.js-alkalmazások, és helyesen függvény szoftvercsatornák használatával a **web.config** kell a következő bejegyzés szerepelhet.
  
        <webSocket enabled="false"/>
  
    Ez letiltja az IIS websocket elemek modult, amely magában foglalja a saját végrehajtásának websocket elemek és a Node.js adott WebSocket modulok például Socket.IO ütközik. Ha a sor nem észlelhető, vagy értékre van állítva `true`, ennek az lehet az oka, hogy a WebSocket-átvitel nem működik az alkalmazáshoz.
  
    Általában a Node.js-alkalmazások nem tartalmaznak egy **web.config** fájlt, így Azure Websitesra automatikusan létrehoz egy Node.js-alkalmazások a rendszer telepítésekor. Ez a fájl automatikusan létrejön a kiszolgálón, mert a fájl megtekintéséhez az FTP- vagy FTPS URL-CÍMÉT a webhely kell használnia. Találhat meg az FTP és FTPS URL-címek a klasszikus portál webhelyét a webalkalmazás kiválasztásával, majd a **irányítópult** hivatkozásra. Az URL-címek jelennek meg a **gyors áttekintő** szakasz.
  
  > [!NOTE]
  > A **web.config** csak létre fájlt Azure Websitesra Ha az alkalmazás nem biztosít. Ha megad egy **web.config** fájlt a projekt gyökérkönyvtárában azt által használt Azure Web Apps.
  > 
  > 
  
    Ha a bejegyzés nincs jelen, vagy értékre van állítva `true`, akkor létre kell hoznia egy **web.config** a Node.js-alkalmazás gyökérkönyvtárában, és adjon meg egy értéket a `false`.  Referenciaként az alábbiakban található alapértelmezett **web.config** az alkalmazás által használt **app.js** belépési pontként.
  
        <?xml version="1.0" encoding="utf-8"?>
        <!--
             This configuration file is required if iisnode is used to run node processes behind
             IIS or IIS Express.  For more information, visit:
  
             https://github.com/tjanczuk/iisnode/blob/master/src/samples/configuration/web.config
        -->
  
        <configuration>
          <system.webServer>
            <!-- Visit http://blogs.msdn.com/b/windowsazure/archive/2013/11/14/introduction-to-websockets-on-windows-azure-web-sites.aspx for more information on WebSocket support -->
            <webSocket enabled="false" />
            <handlers>
              <!-- Indicates that the server.js file is a node.js web app to be handled by the iisnode module -->
              <add name="iisnode" path="app.js" verb="*" modules="iisnode"/>
            </handlers>
            <rewrite>
              <rules>
                <!-- Do not interfere with requests for node-inspector debugging -->
                <rule name="NodeInspector" patternSyntax="ECMAScript" stopProcessing="true">
                  <match url="^app.js\/debug[\/]?" />
                </rule>
  
                <!-- First we consider whether the incoming URL matches a physical file in the /public folder -->
                <rule name="StaticContent">
                  <action type="Rewrite" url="public{REQUEST_URI}"/>
                </rule>
  
                <!-- All other URLs are mapped to the node.js web app entry point -->
                <rule name="DynamicContent">
                  <conditions>
                    <add input="{REQUEST_FILENAME}" matchType="IsFile" negate="True"/>
                  </conditions>
                  <action type="Rewrite" url="app.js"/>
                </rule>
              </rules>
            </rewrite>
            <!--
              You can control how Node is hosted within IIS using the following options:
                * watchedFiles: semi-colon separated list of files that will be watched for changes to restart the server
                * node_env: will be propagated to node as NODE_ENV environment variable
                * debuggingEnabled - controls whether the built-in debugger is enabled
  
              See https://github.com/tjanczuk/iisnode/blob/master/src/samples/configuration/web.config for a full list of options
            -->
            <!--<iisnode watchedFiles="web.config;*.js"/>-->
          </system.webServer>
        </configuration>
  
    Ha az alkalmazás nem használja a belépési pont **app.js**, le kell cserélnie összes előfordulását **app.js** a megfelelő belépési ponthoz. Például cseréje **app.js** rendelkező **server.js**.

> [!NOTE]
> Ha az Azure App Service-t az Azure-fiók regisztrálása előtt szeretné kipróbálni, ugorjon [Az Azure App Service kipróbálása] oldalra. Itt azonnal létrehozhat egy ideiglenes, kezdő szintű webalkalmazást az App Service szolgáltatásban. Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.
> 
> 

## <a name="next-steps"></a>Következő lépések
Ez az oktatóanyag megtanulta, hogyan hozhat létre egy Azure-webalkalmazás található Csevegés alkalmazást. Az alkalmazás Azure-Felhőszolgáltatásban is tárolhatja. Ehhez útmutatást a, lásd: [Socket.IO segítségével egy Node.js Csevegés alkalmazás létrehozása az Azure-Felhőszolgáltatásban].

További információ: a [Node.js fejlesztői központ].

## <a name="whats-changed"></a>A változások
* A módosítás a websites szolgáltatásról az App Service-re váltásról: [Azure App Service és a hatása a meglévő Azure-szolgáltatások].

<!-- URL List -->

[Azure Redis Cache]: /documentation/services/redis-cache/
[App Service Web Apps]: http://go.microsoft.com/fwlink/?LinkId=529714
[Web Apps árképzést ismertető oldalra]: http://go.microsoft.com/fwlink/?LinkId=511643
[Socket.IO segítségével egy Node.js Csevegés alkalmazás létrehozása az Azure-Felhőszolgáltatásban]: ../cloud-services/cloud-services-nodejs-chat-app-socketio.md
[Install and Configure the Azure CLI]: ../cli-install-nodejs.md
[Azure App Service és a hatása a meglévő Azure-szolgáltatások]: http://go.microsoft.com/fwlink/?LinkId=529714
[Node.js fejlesztői központ]: /develop/nodejs/
[Az Azure App Service kipróbálása]: https://azure.microsoft.com/try/app-service/
[példány kapcsolat az Azure-webhelyek]: https://azure.microsoft.com/blog/2013/11/18/disabling-arrs-instance-affinity-in-windows-azure-web-sites/
[a gyorsítótár létrehozása az Azure Redis Cache]: ../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md

[socket.io-redis]: https://github.com/socketio/socket.io-redis
[Socket.IO GitHub-tárházban]: https://github.com/socketio/socket.io
[ZIP- vagy GZ archivált kiadás]: https://github.com/socketio/socket.io/releases

<!-- IMG List -->

[chat-example-view]: ./media/web-sites-nodejs-chat-app-socketio/socketio-2.png
[npm-output]: ./media/web-sites-nodejs-chat-app-socketio/socketio-7.png
[completed-app]: ./media/web-sites-nodejs-chat-app-socketio/websitesocketcomplete.png
