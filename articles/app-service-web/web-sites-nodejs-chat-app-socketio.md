---
title: "Node.js csevegőalkalmazás Socket.IO segítségével az Azure App Service aaaCreate"
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
ms.openlocfilehash: 3bd7867ccc297dc0a21c7a00cc9db06358877f5d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-nodejs-chat-application-with-socketio-in-azure-app-service"></a>Node.js csevegőalkalmazás létrehozása a Socket.IO segítségével az Azure App Service-ben
Socket.IO biztosít a valós idejű kommunikációját a node.js-kiszolgáló és az ügyfél websocket elemeket. Tartalék tooother átvitelek (például hosszú lekérdezési), amely a régebbi böngészők is támogatja. Ez az oktatóanyag végigvezetik egy alapú Socket.IO Csevegés alkalmazásra, amely egy Azure webalkalmazás üzemeltetéséhez, és bemutatják, hogyan tooscale hello használó [Azure Redis Cache]. A Socket.IO további információkért lásd: <http://socket.io/>.

> [!NOTE]
> Ebben a feladatban hello eljárások alkalmazhatók túl[App Service Web Apps]; a Felhőszolgáltatások, lásd: [Socket.IO segítségével egy Node.js Csevegés alkalmazás létrehozása az Azure-Felhőszolgáltatásban].
> 
> 

## <a name="download-hello-chat-example"></a>Töltse le a hello Csevegés – példa
Ebben a projektben használjuk hello Csevegés példa hello [Socket.IO GitHub-tárházban]. Hajtsa végre a következő lépéseket toodownload hello példa hello, majd vegye fel azt a korábban létrehozott toohello projekt.

1. Töltse le a [ZIP- vagy GZ archivált kiadás] hello Socket.IO projekt (a dokumentum a 1.3.5 verziója lett megadva)
2. Bontsa ki a hello archiválási és másolási hello **példák\\Csevegés** directory tooa új helyre. Például  **\\csomópont\\Csevegés**.

## <a name="modify-appjs-and-install-modules"></a>Az App.js fájl módosítása és -modulok telepítése
1. Nevezze át a hello **index.js** fájl túl**app.js**. Ez lehetővé teszi az Azure toodetect, hogy ez a Node.js-alkalmazás.
2. Nyissa meg hello **app.js** fájlt egy szövegszerkesztőben. Változás hello sort tartalmazó `var io = require('../..')(server);` alább látható módon:
   
       var express = require('express');
       var app = express();
       var server = require('http').createServer(app);
       // var io = require('../..')(server);
       // New:
       var io = require('socket.io')(server);
       var port = process.env.PORT || 3000;
3. Nyissa meg hello **package.json** fájlt, és a hivatkozás toosocket.io alatt `dependencies`, az alábbiak szerint:
   
        "dependencies": {
          "express": "3.4.8",
          "socket.io": "1.3.5"
        }
4. Hello parancssori, módosítsa toohello  **\\csomópont\\Csevegés** könyvtárra, és használja npm tooinstall hello modult az alkalmazás számára szükséges:
   
        npm install
   
    Ezzel telepíti hello modulok nevű almappába **node_modules**.

## <a name="create-an-azure-web-app"></a>Azure-webalkalmazás létrehozása
Kövesse ezeket a lépéseket toocreate Azure-webalkalmazás Git-közzététel engedélyezése és majd a hello webalkalmazás WebSocket támogatásának engedélyezéséhez.

> [!NOTE]
> toocomplete ebben az oktatóanyagban egy Azure-fiókra van szüksége. Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot. További információkért lásd: <a href="http://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A7171371E" target="_blank">Ingyenes Azure-fiók létrehozása</a>.
> 
> 

1. Hello Azure parancssori felület (CLI) telepítése, és csatlakozzon az Azure-előfizetés tooyour. Lásd: [telepítése és konfigurálása az Azure parancssori felület hello](../cli-install-nodejs.md).
2. Ha ez az első idő beállítása a tárházat az Azure-ban, toocreate bejelentkezési hitelesítő adatokat kell. Hello Azure CLI adja meg a következő parancs hello:
   
        azure site deployment user set [username] [password]
3. Módosítsa a toohello  **\\node\chat** használata hello követően és a directory parancs toocreate egy új Azure web app és egy helyi Git-tárházat. Ez a parancs is létrehoz egy Git távoli elnevezett "azure".
   
        azure site create mysitename --git
   
    A webalkalmazás egyedi nevű, "mysitename" le kell cserélnie.
4. Véglegesítse hello meglévő fájlok toohello helyi tárház hello a következő parancsok használatával:
   
        git add .
        git commit -m "Initial commit"
5. Leküldéses hello fájlok toohello Azure Web Apps tárház a hello a következő parancsot:
   
        git push azure master
   
    Amikor a rendszer kéri, adja meg a hitelesítő adatokat a 2. lépésben. Állapotüzenetek kap, a rendszer importálja a hello kiszolgálón. Ez a folyamat befejezése után hello alkalmazás fogja üzemeltetni az Azure-webalkalmazásban.
   
   > [!NOTE]
   > A modul a telepítés során azt tapasztalhatja hibák, amelyek "hello importált projekt … nem található". Ezek biztonságosan figyelmen kívül hagyható.
   > 
   > 
6. Socket.IO használ a websocket elemek, amelyek az Azure-on alapértelmezés szerint nem engedélyezettek. webes szoftvercsatornák tooenable, hello a következő parancsot használja:
   
        azure site set -w
   
    Ha a rendszer kéri, adja meg a webalkalmazás hello hello nevét.
   
   > [!NOTE]
   > "az azure site set -w" parancs lesz csak verziójával 0.7.4 munkahelyi vagy az Azure parancssori felület hello magasabb hello. Engedélyezheti a WebSocket-támogatás hello használatával [Azure Portal](https://portal.azure.com).
   > 
   > tooenable websocket elemek használata hello Azure portálon kattintson a hello webalkalmazás hello webalkalmazások panelen, kattintson **összes beállítás** > **Alkalmazásbeállítások**. A **webes szoftvercsatornák**, kattintson a **a**. Ezután kattintson a **Save** (Mentés) gombra.
   > 
   > 
7. tooview hello webalkalmazást az Azure, használjon hello következő toolaunch parancsot a webböngészőt, és keresse meg a toohello futtatott webalkalmazás:
   
        azure site browse

Az alkalmazás mostantól az Azure-on fut, és továbbíthat csevegési üzeneteket a Socket.IO segítségével különböző ügyfelek között.

## <a name="scale-out"></a>Horizontális felskálázás
Socket.IO alkalmazások kiterjeszthető használatával egy **adapter** toodistribute üzenetek és események között több alkalmazáspéldányt. Nincsenek elérhető több adapterek, amíg hello [socket.io-redis] adapter hello Azure Redis Cache szolgáltatással könnyen használható.

> [!NOTE]
> Egy további követelmény a Socket.IO megoldás kiterjesztése a kapcsolódó munkamenetek támogatása. A kapcsolódó munkamenetek alapértelmezés szerint engedélyezve vannak a Azure Web Apps használatával Azure alkalmazáskérelmek irányítására. További információkért lásd: [példány kapcsolat az Azure-webhelyek].
> 
> 

### <a name="create-a-redis-cache"></a>A Redis gyorsítótár létrehozása
A hello lépésekkel [a gyorsítótár létrehozása az Azure Redis Cache] toocreate új gyorsítótárat.

> [!NOTE]
> Mentés hello **állomásnév** és **elsődleges kulcs** a gyorsítótárhoz, mivel ezek lesz szükség a hello lépéseket.
> 
> 

### <a name="add-hello-redis-and-socketio-redis-modules"></a>Hello redis és socket.io-redis-modulok hozzáadása
1. A parancssorból, módosítsa a toohello  **\\csomópont\\Csevegés** parancs a következő könyvtárra, és használja hello.
   
        npm install socket.io-redis@0.1.4 redis@0.12.1 --save
   
   > [!NOTE]
   > Ebben a parancsban megadott hello verziók Ez a cikk tesztelése során használt hello verziója.
   > 
   > 
2. Módosítsa a hello **app.js** fájl tooadd hello következő sorok után azonnal`var io = require('socket.io')(server);`
   
        var pub = require('redis').createClient(6379,'redishostname', {auth_pass: 'rediskey', return_buffers: true});
        var sub = require('redis').createClient(6379,'redishostname', {auth_pass: 'rediskey', return_buffers: true});
   
        var redis = require('socket.io-redis');
        io.adapter(redis({pubClient: pub, subClient: sub}));
   
    Cserélje le **redishostname** és **rediskey** hello állomásnév és a Redis gyorsítótár kulccsal.
   
    A hozzon létre egy közzétételi és előfizetési ügyfél toohello korábban létrehozott Redis gyorsítótár. hello ügyfelek majd használt hello adapter tooconfigure Socket.IO toouse hello Redis gyorsítótár az üzenetek és események továbbításához az alkalmazás példányai között
   
   > [!NOTE]
   > Hello közben **socket.io-redis** adapter közvetlenül tooRedis kommunikálhatnak, hello aktuális verziója nem támogatja az Azure Redis cache-által igényelt hello hitelesítés. Így hello hello kezdeti kapcsolat létre **redis** modult, majd hello ügyfél átadása toohello **socket.io-redis** adapter.
   > 
   > Azure Redis Cache-példányokhoz biztonságos 6380 portot használ, miközben ebben a példában használt hello modulok nem támogatja a biztonságos kapcsolatokat 7/14/2014-től. kód fent hello hello alapértelmezés szerint nem biztonságos portja 6379 használja.
   > 
   > 
3. Mentés hello módosított **app.js**

### <a name="commit-changes-and-redeploy"></a>Véglegesítse a módosításokat, és telepítse újra
A parancssori a hello hello  **\\csomópont\\Csevegés** directory, a következő parancsok toocommit módosítások hello használja, és telepítse újra a hello alkalmazás.

    git add .
    git commit -m "implementing scale out"
    git push azure master

Miután hello módosítások toohello server értesítését, méretezhető a webhely több példánya között hello a következő parancs használatával.

    azure site scale instances --instances #

Ha  **#**  példányok toocreate hello száma.

Több böngészők vagy számítógépek tooverify küldött üzenetek megfelelően tooall ügyfelek tooyour webalkalmazás lehet csatlakoztatni.

## <a name="troubleshooting"></a>Hibaelhárítás
### <a name="connection-limits"></a>Kapcsolat korlátai
Az Azure Web Apps több SKU, hello erőforrások elérhető tooyour hely meghatározó érhető el. Ez magában foglalja a hello engedélyezett WebSocket-kapcsolatok száma. További információkért lásd: hello [Web Apps árképzést ismertető oldalra].

### <a name="messages-arent-being-sent-using-websockets"></a>Üzenetek nem küldött websocket elemek használatával
Ügyfélböngészők tartsa visszaváltás toolong lekérdezési websocket elemek használata helyett, ha miatt hello a következők egyike lehet.

* **Próbálja a hello átviteli toojust websocket elemek korlátozása**
  
    Ahhoz, hogy Socket.IO toouse, átviteli üzenetküldési hello websocket elemeket hello kiszolgáló és az ügyfél támogatnia kell websocket elemeket. Ha egy vagy többi hello nem, a Socket.IO egyezteti egy másik átviteltől, pl. hosszú lekérdezési. átvitelek Socket.IO által használt alapértelmezett listája hello ` websocket, htmlfile, xhr-polling, jsonp-polling`. Kényszerítheti tooonly használata szoftvercsatornák használatával a következő kód toohello hello hozzáadásával **app.js** fájl után hello sort tartalmazó `, nicknames = {};`.
  
        io.configure(function() {
          io.set('transports', ['websocket']);
        });
  
  > [!NOTE]
  > Megjegyzés: régebbi websocket elemek nem támogató böngészők nem fog tudni tooconnect toohello hely pedig kód fent hello aktív, csak a kommunikációs tooWebSockets korlátozza.
  > 
  > 
* **SSL használata**
  
    Néhány kisebb használt HTTP-fejlécek, például a hello támaszkodik websocket elemek **frissítése** fejléc. Néhány, a közbenső hálózati eszközök, például webes proxykat, ezek a fejlécek távolíthatja el. tooavoid probléma létrehozhat hello WebSocket-kapcsolat SSL-en keresztül.
  
    Egy egyszerű módot tooaccomplish Ez az tooconfigure Socket.IO túl`match origin protocol`. Ez arra utasítja a Socket.IO toosecure websocket elemek kommunikációs hello ugyanaz, mint a HTTP/HTTPS-címekről hello kérelem hello weblap. Ha egy böngészőt használ egy HTTPS URL-cím toovisit a webhelyet, a további WebSocket kommunikáció Socket.IO segítségével megfelelően történik SSL-en keresztül.
  
    toomodify a példa tooenable ebben a konfigurációban adja hozzá a következő kód toohello hello **app.js** után hello sor tartalmazó fájl `, nicknames = {};`.
  
        io.configure(function() {
          io.set('match origin protocol', true);
        });
* **Ellenőrizze a web.config beállításait**
  
    Node.js-alkalmazásokat az Azure web apps használata hello **web.config** fájl tooroute bejövő kérelmek toohello Node.js-alkalmazás. Node.js-alkalmazások és helyesen toofunction websocket elemeket, hello **web.config** bejegyzés a következő hello tartalmaznia kell.
  
        <webSocket enabled="false"/>
  
    Ez letiltja az hello IIS websocket elemek modult, amely magában foglalja a saját végrehajtásának websocket elemek és a Node.js adott WebSocket modulok például Socket.IO ütközik. Ha a sor nem észlelhető, vagy túl van beállítva`true`, ennek az lehet, hogy az alkalmazás nem működik a hello WebSocket átviteli hello OK.
  
    Általában a Node.js-alkalmazások nem tartalmaznak egy **web.config** fájlt, így Azure Websitesra automatikusan létrehoz egy Node.js-alkalmazások a rendszer telepítésekor. Ez a fájl automatikusan létrehozott hello kiszolgálón, mert kell használnia hello FTP vagy FTPS URL-CÍMÉT a webhely tooview az ezt a fájlt. Hello FTP és FTPS URL-címek a hely az hello klasszikus portálon található; ehhez válassza a webes alkalmazás, és majd hello **irányítópult** hivatkozásra. hello URL-címek jelennek meg hello **gyors áttekintő** szakasz.
  
  > [!NOTE]
  > Hello **web.config** csak létre fájlt Azure Websitesra Ha az alkalmazás nem biztosít. Ha megad egy **web.config** fájl hello legfelső szintű a projektet, azt által használt Azure Web Apps.
  > 
  > 
  
    Ha hello bejegyzése nincs jelen, vagy tooa értékének beállítása `true`, akkor létre kell hoznia egy **web.config** a hello a Node.js-alkalmazás gyökérkönyvtárában, és adjon meg egy értéket a `false`.  Összehasonlításul, az alábbi hello egy alapértelmezett **web.config** az alkalmazás által használt **app.js** hello belépési pontként.
  
        <?xml version="1.0" encoding="utf-8"?>
        <!--
             This configuration file is required if iisnode is used toorun node processes behind
             IIS or IIS Express.  For more information, visit:
  
             https://github.com/tjanczuk/iisnode/blob/master/src/samples/configuration/web.config
        -->
  
        <configuration>
          <system.webServer>
            <!-- Visit http://blogs.msdn.com/b/windowsazure/archive/2013/11/14/introduction-to-websockets-on-windows-azure-web-sites.aspx for more information on WebSocket support -->
            <webSocket enabled="false" />
            <handlers>
              <!-- Indicates that hello server.js file is a node.js web app toobe handled by hello iisnode module -->
              <add name="iisnode" path="app.js" verb="*" modules="iisnode"/>
            </handlers>
            <rewrite>
              <rules>
                <!-- Do not interfere with requests for node-inspector debugging -->
                <rule name="NodeInspector" patternSyntax="ECMAScript" stopProcessing="true">
                  <match url="^app.js\/debug[\/]?" />
                </rule>
  
                <!-- First we consider whether hello incoming URL matches a physical file in hello /public folder -->
                <rule name="StaticContent">
                  <action type="Rewrite" url="public{REQUEST_URI}"/>
                </rule>
  
                <!-- All other URLs are mapped toohello node.js web app entry point -->
                <rule name="DynamicContent">
                  <conditions>
                    <add input="{REQUEST_FILENAME}" matchType="IsFile" negate="True"/>
                  </conditions>
                  <action type="Rewrite" url="app.js"/>
                </rule>
              </rules>
            </rewrite>
            <!--
              You can control how Node is hosted within IIS using hello following options:
                * watchedFiles: semi-colon separated list of files that will be watched for changes toorestart hello server
                * node_env: will be propagated toonode as NODE_ENV environment variable
                * debuggingEnabled - controls whether hello built-in debugger is enabled
  
              See https://github.com/tjanczuk/iisnode/blob/master/src/samples/configuration/web.config for a full list of options
            -->
            <!--<iisnode watchedFiles="web.config;*.js"/>-->
          </system.webServer>
        </configuration>
  
    Ha az alkalmazás nem használja a belépési pont **app.js**, le kell cserélnie összes előfordulását **app.js** hello megfelelő belépési pont. Például cseréje **app.js** rendelkező **server.js**.

> [!NOTE]
> Ha azt szeretné, hogy az az Azure-fiók regisztrálása előtt az Azure App Service lépései tooget, nyissa meg túl[App Service kipróbálása], ahol azonnal létrehozhat egy rövid élettartamú alapszintű webalkalmazást az App Service-ben. Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.
> 
> 

## <a name="next-steps"></a>Következő lépések
Ebben az oktatóanyagban megtanulta, hogyan toocreate egy csevegőalkalmazás tárolt Azure-webalkalmazás. Az alkalmazás Azure-Felhőszolgáltatásban is tárolhatja. Útmutatást a tooaccomplish a, lásd: [Socket.IO segítségével egy Node.js Csevegés alkalmazás létrehozása az Azure-Felhőszolgáltatásban].

További információkért lásd még: hello [Node.js fejlesztői központ].

## <a name="whats-changed"></a>A változások
* Egy útmutató toohello webhelyek tooApp szolgáltatás változás lásd: [Azure App Service és a hatása a meglévő Azure-szolgáltatások].

<!-- URL List -->

[Azure Redis Cache]: /documentation/services/redis-cache/
[App Service Web Apps]: http://go.microsoft.com/fwlink/?LinkId=529714
[Web Apps árképzést ismertető oldalra]: http://go.microsoft.com/fwlink/?LinkId=511643
[Socket.IO segítségével egy Node.js Csevegés alkalmazás létrehozása az Azure-Felhőszolgáltatásban]: ../cloud-services/cloud-services-nodejs-chat-app-socketio.md
[Install and Configure hello Azure CLI]: ../cli-install-nodejs.md
[Azure App Service és a hatása a meglévő Azure-szolgáltatások]: http://go.microsoft.com/fwlink/?LinkId=529714
[Node.js fejlesztői központ]: /develop/nodejs/
[App Service kipróbálása]: https://azure.microsoft.com/try/app-service/
[példány kapcsolat az Azure-webhelyek]: https://azure.microsoft.com/blog/2013/11/18/disabling-arrs-instance-affinity-in-windows-azure-web-sites/
[a gyorsítótár létrehozása az Azure Redis Cache]: ../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md

[socket.io-redis]: https://github.com/socketio/socket.io-redis
[Socket.IO GitHub-tárházban]: https://github.com/socketio/socket.io
[ZIP- vagy GZ archivált kiadás]: https://github.com/socketio/socket.io/releases

<!-- IMG List -->

[chat-example-view]: ./media/web-sites-nodejs-chat-app-socketio/socketio-2.png
[npm-output]: ./media/web-sites-nodejs-chat-app-socketio/socketio-7.png
[completed-app]: ./media/web-sites-nodejs-chat-app-socketio/websitesocketcomplete.png
