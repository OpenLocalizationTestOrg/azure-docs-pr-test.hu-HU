---
title: "aaaBest eljárásokat és hibaelhárítási útmutatójában csomópont alkalmazások Azure-webalkalmazásokban"
description: "Ismerje meg, hello ajánlott eljárásokról és a csomópont-alkalmazások az Azure Web Apps hibaelhárítási lépéseket."
services: app-service\web
documentationcenter: nodejs
author: ranjithr
manager: wadeh
editor: 
ms.assetid: 387ea217-7910-4468-8987-9a1022a99bef
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 06/06/2016
ms.author: ranjithr
ms.openlocfilehash: 975898142a224f14df1091a46d16e9074d9e2831
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="best-practices-and-troubleshooting-guide-for-node-applications-on-azure-web-apps"></a>Ajánlott eljárások és hibaelhárítási útmutatójában csomópont alkalmazások Azure-webalkalmazásokban
[!INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

Ebből a cikkből megtudhatja, hello ajánlott eljárásokról és a hibaelhárítási lépéseket [csomópont alkalmazások](app-service-web-get-started-nodejs.md) Azure webalkalmazás fut (a [iisnode](https://github.com/azure/iisnode)).

> [!WARNING]
> Körültekintően járjon el, ha a hibaelhárítási lépéseket a munkakörnyezeti helyet. Javasoljuk, tootroubleshoot rendszeren nem éles telepítési például az átmeneti helyet, és hello a probléma fennáll, amikor felcserélni az átmeneti és az éles webalkalmazásra.
> 
> 

## <a name="iisnode-configuration"></a>Az IISNODE konfigurálása
Ez [sémafájl](https://github.com/Azure/iisnode/blob/master/src/config/iisnode_schema_x64.xml) összes hello beállításait, amelyek képesek az iisnode-hoz. Az alkalmazás hasznos hello-beállítások a következők:

* nodeProcessCountPerApplication
  
    Ez a beállítás IIS alkalmazásonként működését csomópont folyamatok hello számát szabályozza. Alapértelmezett értéke 1. A virtuális gép core darabszámként annyi node.exe úgy, hogy a too0 indíthatja el. Javasolt érték 0 a legtöbb alkalmazás, használhatja az összes hello magok a számítógépen. NODE.exe az egyetlen, egy node.exe fogyaszt legfeljebb 1 core és tooget maximális teljesítmény kívül a csomópont alkalmazás egyszálas érdemes tooutilize összes mag.
* nodeProcessCommandLine
  
    Ez a beállítás hello elérési toohello node.exe szabályozza. Az érték toopoint tooyour node.exe verzió állíthatja be.
* maxConcurrentRequestsPerProcess
  
    Ez a beállítás hello iisnode tooeach node.exe által küldött egyidejű kérelmek maximális számát szabályozza. Azure webalkalmazás hello alapértelmezett érték a végtelen. Ezzel a beállítással kapcsolatban tooworry nem lesz. Az azure webalkalmazás kívül a hello alapértelmezett érték: 1024. Érdemes tooconfigure Ez attól függően, hogy hány kér az alkalmazás lekérdezi, és hogy milyen gyorsan az alkalmazás minden kérést dolgoz fel.
* maxNamedPipeConnectionRetry
  
    Ez a beállítás szabályozza hello maximálisan megengedett számú iisnode visszaállítja a cső toosend hello kérelem nevű toonode.exe keresztül hello élő kapcsolatot. Ez a beállítás namedPipeConnectionRetryDelay együtt az összes kérelem belül iisnode időtúllépés teljes hello határozza meg. Alapértelmezett érték 200 található Azure webalkalmazás. Időtúllépés másodpercben teljes = (maxNamedPipeConnectionRetry \* namedPipeConnectionRetryDelay) / 1000
* namedPipeConnectionRetryDelay
  
    A beállítás szabályozza hello ennyi ideje (ms) iisnode közötti nevesített cső hello keresztül minden újrapróbálkozási toosend kérelem toonode.exe várakozik. Alapértelmezett érték: 250ms.
    Időtúllépés másodpercben teljes = (maxNamedPipeConnectionRetry \* namedPipeConnectionRetryDelay) / 1000
  
    Alapértelmezés szerint a teljes időtúllépés hello azure webalkalmazás az iisnode 200 \* 250ms = 50 másodperc.
* logDirectory
  
    Ez a beállítás szabályozza hello könyvtár, ahol a iisnode stdout/stderr naplózza. Alapértelmezett érték: iisnode, amely relatív toohello fő parancsfájlba könyvtár (könyvtárat, amelyben fő server.js jelen)
* debuggerExtensionDll
  
    Ez a beállítás határozza meg, melyik verzióját a node-inspector iisnode fogja használni, amikor a csomópont-alkalmazásban. Az iisnode-inspector-0.7.3.dll és az iisnode-inspector.dll jelenleg ez a beállítás érvényes értékei hello csak 2. Alapértelmezett érték: az iisnode-inspector-0.7.3.dll. az iisnode-inspector-0.7.3.dll verzió csomópont-inspector-0.7.3 használ, és websocket elemeket, használ, ezért szüksége lesz tooenable szoftvercsatornák használatával a azure webalkalmazás toouse a jelen verziójában. Lásd: <http://www.ranjithr.com/?p=98> hogyan tooconfigure iisnode toouse hello új a node-inspector olvashat.
* flushResponse
  
    hello alapértelmezett IIS működése, hogy az érkezett válasz adatait too4MB mentése puffereli kiürítése előtt, vagy végéig hello hello választ, amelyik előbb következik be. az iisnode kínál konfigurációs beállítás toooverride ezt a viselkedést: tooflush hello választörzs entitás egy kódrészletet, amint az iisnode kap az node.exe, tooset hello kell iisnode/@flushResponse attribútumot a web.config too'true ":
  
    ```
    <configuration>    
        <system.webServer>    
            <!-- ... -->    
            <iisnode flushResponse="true" />    
        </system.webServer>    
    </configuration>
    ```
  
    Engedélyezésével hello válasz entitástörzsében minden kódrészletet a kiürítési, amely csökkenti a hello átviteli hello rendszer (az v0.1.13) ~ 5 %-kal teljesítményigény, ezért ajánlott tooscope ezt a beállítást csak tooendpoints válasz streaming (pl. használatával hello igénylő <location> hello web.config eleme)
  
    Továbbá toothis, az alkalmazások, kell tooalso set responseBufferLimit az iisnode kezelő too0 a.
  
    ```
    <handlers>    
        <add name="iisnode" path="app.js" verb="\*" modules="iisnode" responseBufferLimit="0"/>    
    </handlers>
    ```
* watchedFiles
  
    Ez az fog kell figyelt módosítások fájlok egy pontosvesszővel elválasztott listája. A módosítás tooa fájl hello alkalmazás toorecycle hatására. Mindegyik bejegyzés egy választható könyvtár nevét és a szükséges fájl nevét, amely relatív toohello könyvtárra hello fő belépési pontja áll. Hello fájl neve funkciója csak helyettesítő karakterek megengedettek. Alapértelmezett érték "\*. js;web.config"
* recycleSignalEnabled
  
    Alapértelmezett értéke hamis. Ha engedélyezve van, a csomópont alkalmazás csatlakozni tud-e nevesített cső tooa (környezeti változót az IISNODE\_vezérlő\_CSŐ) és "újrahasznosítást" üzenet. Ennek következtében hello w3wp toorecycle szabályosan.
* idlePageOutTimePeriod
  
    Alapértelmezett érték 0, ami azt jelenti, hogy ez a funkció le van tiltva. Ha a beállított toosome értéke nagyobb, mint 0, az iisnode lapon fog ki az összes alárendelt dolgozza fel a minden "idlePageOutTimePeriod" ezredmásodperc. Mi azt jelenti, kimenő lapon toounderstand tekintse meg a toothis [dokumentáció](https://msdn.microsoft.com/library/windows/desktop/ms682606.aspx). Ez a beállítás akkor lehet hasznos, az alkalmazásokat, amelyek nagy mennyiségű memóriát használnak, és szeretné, hogy toopageout memória toodisk alkalmanként fel néhány RAM toofree.

> [!WARNING]
> Körültekintően járjon el, ha engedélyezve van a következő konfigurációs beállításokat az üzemi környezetben működő alkalmazásokhoz hello. Javasoljuk, toonot lehetővé teszi az élő éles környezetben.
> 
> 

* debugHeaderEnabled
  
    hello alapértelmezett értéke hamis. Ha tootrue, állítsa be az iisnode egy HTTP-válasz fejléce az iisnode-debug tooevery HTTP válasz küldi hello az iisnode-debug fejléc értéke egy URL-címet ad hozzá. Egyes adatra diagnosztikai adatokat is hello URL-cím töredék megtekintésével, de sok jobb képi megjelenítés hello böngészőben hello URL-cím megnyitásával érhető el.
* loggingEnabled
  
    Ez a beállítás az stdout és az stderr hello naplózását szabályozza, iisnode által. Az Iisnode stdout/stderr elindítja azt csomópont folyamatok rögzítése, és a hello "logDirectory" beállításban megadott toohello directory írása. A beállítás engedélyezése, az alkalmazás a naplók toohello fájlrendszer írása lesz, és attól függően, hogy hello alkalmazás által végzett naplózás hello mennyisége, lehetnek teljesítményre gyakorolt hatása.
* devErrorsEnabled
  
    Alapértelmezett értéke hamis. Ha beállítása tootrue, iisnode megjeleníti hello HTTP állapotkód és a Win32-hibakód: a böngésző. hello win32 kódban megoldani a problémákat bizonyos típusú hasznos lesz.
* debuggingEnabled (ne engedélyezze az éles hely)
  
    Ez a beállítás szabályozza a hibakeresési funkciója. Az Iisnode integrálva van a node-Inspector használatával. Ha engedélyezi ezt a beállítást, engedélyezi a csomópont alkalmazás hibakeresést. Ha engedélyezi ezt a beállítást, az iisnode fog elrendezés hello szükséges a node-inspector könyvtárban található fájlok "debuggerVirtualDir" hello első debug kérelem tooyour csomópont alkalmazástól. A kérelem toohttp://yoursite/server.js/debug elküldésével hello a node-inspector be tudják tölteni. Hello hibakeresési URL-szegmenseket "debuggerPathSegment" beállítással szabályozható. Alapértelmezett debuggerPathSegment által = "debug". A GUID tooa például beállíthatja úgy, hogy mások által felderített nehezebb toobe.
  
    Ennek ellenőrzéséhez [hivatkozás](https://tomasz.janczuk.org/2011/11/debug-nodejs-applications-on-windows.html) kapcsolatban további részleteket a hibakeresést.

## <a name="scenarios-and-recommendationstroubleshooting"></a>Forgatókönyvek és javaslatok/hibaelhárítása
### <a name="my-node-application-is-making-too-many-outbound-calls"></a>A csomópont-alkalmazás, hogy így túl sok kimenő hívásokat.
Számos alkalmazás szeretnének toomake kimenő kapcsolatok a rendszeres művelet részeként. Például kérelem érkezik, amikor a csomópont-alkalmazást ehhez szeretné, hogy a REST API máshol toocontact és néhány információt tooprocess hello kérelmek. Toouse megtartása életben ügynök kívánt http vagy https hívása esetén. Például használhatja hello agentkeepalive modul a keep-alive megbízottként ezek kimenő hívása esetén. Ezzel biztosíthatja, hogy hello sockets újra felhasználja a rendszer a virtuális gép azure webalkalmazás és csökkentése hello terhek új sockets minden kimenő kérelem létrehozása. Emellett ez biztosítja, hogy kevesebb sok kimenő kérelmek, és ezért nem haladhatja meg a virtuális gépenként kiosztott hello maxsocket sockets toomake használt. Azure webalkalmazás ajánlást tooset hello agentKeepAlive maxsocket virtuális gépenként 160 sockets tooa összesen érték lehet. Ez azt jelenti, hogy ha 4 node.exe hello virtuális gép futó, célszerű tooset hello agentKeepAlive maxsocket too40 másodpercenkénti teljes virtuális gépenként 160 egyben node.exe.

Példa [agentKeepALive](https://www.npmjs.com/package/agentkeepalive) konfiguráció:

```
var keepaliveAgent = new Agent({    
    maxSockets: 40,    
    maxFreeSockets: 10,    
    timeout: 60000,    
    keepAliveTimeout: 300000    
});
```

Ez a példa feltételezi, hogy rendelkezik-e a virtuális gépen 4 node.exe. Ha fut a virtuális gép hello node.exe eltérő számú, hogy toomodify hello maxsocket beállítást ennek megfelelően.

### <a name="my-node-application-is-consuming-too-much-cpu"></a>A csomópont-alkalmazás által felhasznált túl sok CPU.
A portál magas cpu-felhasználás kapcsolatos az Azure-webalkalmazás valószínűleg kap egy javaslatot. Is telepítő figyelők toowatch egyes [metrikák](web-sites-monitor.md). Hello CPU-használat a hello ellenőrzésekor [Azure Portal irányítópult](../application-insights/app-insights-web-monitor-performance.md), tekintse meg hello maximális értékek CPU, hogy nem hagy ki ki hello maximális értékeket.
Azokban az esetekben, ha úgy gondolja, hogy az alkalmazás által felhasznált túl sok CPU, és miért nem ismertetik szüksége lesz tooprofile a node.js-alkalmazásokban.

### 
#### <a name="profiling-your-node-application-on-azure-webapps-with-v8-profiler"></a>A csomópont alkalmazás V8-Profilkészítő rendelkező azure webalkalmazás profilkészítés
Például lehet mondja ki a hello world alkalmazás, amelyet tooprofile alább látható módon:

```
var http = require('http');    
function WriteConsoleLog() {    
    for(var i=0;i<99999;++i) {    
        console.log('hello world');    
    }    
}

function HandleRequest() {    
    WriteConsoleLog();    
}

http.createServer(function (req, res) {    
    res.writeHead(200, {'Content-Type': 'text/html'});    
    HandleRequest();    
    res.end('Hello world!');    
}).listen(process.env.PORT);
```

Nyissa meg tooyour scm hely https://yoursite.scm.azurewebsites.net/DebugConsole

Alább látható módon jelenik meg egy parancssort. Nyissa meg a hely/wwwroot könyvtárba

![](./media/app-service-web-nodejs-best-practices-and-troubleshoot-guide/scm_install_v8.png)

A "npm telepítés v8-Profilkészítő" hello parancs futtatása

Ez telepítse v8-Profilkészítő csomópont alatt\_modulok directory és az összes függősége.
A server.js tooprofile, szerkessze az alkalmazást.

```
var http = require('http');    
var profiler = require('v8-profiler');    
var fs = require('fs');

function WriteConsoleLog() {    
    for(var i=0;i<99999;++i) {    
        console.log('hello world');    
    }    
}

function HandleRequest() {    
    profiler.startProfiling('HandleRequest');    
    WriteConsoleLog();    
    fs.writeFileSync('profile.cpuprofile', JSON.stringify(profiler.stopProfiling('HandleRequest')));    
}

http.createServer(function (req, res) {    
    res.writeHead(200, {'Content-Type': 'text/html'});    
    HandleRequest();    
    res.end('Hello world!');    
}).listen(process.env.PORT);
```

hello fent a változtatásokat fog hello WriteConsoleLog függvény profil, és jegyezze hello-profil kimeneti too'profile.cpuprofile "a hely wwwroot fájlt. Egy kérelem tooyour kérelem küldése. A hely wwwroot alapján létrehozott "profile.cpuprofile" fájl jelenik meg.

![](./media/app-service-web-nodejs-best-practices-and-troubleshoot-guide/scm_profile.cpuprofile.png)

Töltse le a fájlt, és kell tooopen a fájl a Chrome F12 eszközökkel. A Látványelem elérte a F12, majd kattintson a "Profilt" hello. Kattintson a "Load" gombra. Válassza ki az imént letöltött profile.cpuprofile fájlt. Kattintson az épp hello-profil.

![](./media/app-service-web-nodejs-best-practices-and-troubleshoot-guide/chrome_tools_view.png)

Látni fogja, hogy hello idő 95 % felhasznált WriteConsoleLog függvény alább látható módon. Ez azt is bemutatja, hello pontos sorok számát és a forrásfájlok hello probléma okozhatja.

### <a name="my-node-application-is-consuming-too-much-memory"></a>A csomópont-alkalmazás által felhasznált túl sok memóriát.
A portál kapcsolatos leterheli a rendszermemóriát az Azure-webalkalmazás valószínűleg kap egy javaslatot. Is telepítő figyelők toowatch egyes [metrikák](web-sites-monitor.md). Hello memóriahasználat hello a ellenőrzésekor [Azure Portal irányítópult](../application-insights/app-insights-web-monitor-performance.md), tekintse meg az hello maximális értékek a memória, hogy nem hagy ki ki hello maximális értékeket.

#### <a name="leak-detection-and-heap-diffing-for-nodejs"></a>A node.js felderítését és a halommemória Diffing szivárgás lépett fel
Használhat [csomópont-memwatch](https://github.com/lloyd/node-memwatch) toohelp azonosíthatja a memória-szivárgást.
Az alkalmazás telepítése memwatch hasonlóan v8-Profilkészítő és a kód toocapture és a különbözeti halommemória tooidentify hello memóriavesztések szerkesztése.

### <a name="my-nodeexes-are-getting-killed-randomly"></a>A node.exe vannak első véletlenszerűen leállítása
Néhány oka miért Ez történhet:

1. Az alkalmazás szűrész nem kezelt kivételek – kérjük ellenőrzés d:\\otthoni\\naplófájlok\\alkalmazás\\hello kivétel lépett fel az hello részletes naplózás-errors.txt fájlt. Ez a fájl rendelkezik hello Veremkivonat, így ezt úgy javíthatja ki az alkalmazás ennek alapján.
2. Az alkalmazás által felhasznált túl sok memóriát, amely érinti a bevezetés más folyamatokkal. Ha hello összes virtuális gép memória Bezárás too100 %, a node.exe megállítása sikerült által hello folyamat manager toolet más folyamatok végezheti egy alkalommal toodo néhány. toofix, vagy győződjön meg arról, hogy az alkalmazás nem memóriaszivárgást, vagy ha akkor alkalmazás valóban toouse nagy mennyiségű memóriát igényel, adjon vertikális felskálázás tooa sokkal több RAM Memóriát a nagyobb méretű.

### <a name="my-node-application-does-not-start"></a>A csomópont alkalmazás nem indul el
Az alkalmazás indításkor 500 hibát ad vissza, ha néhány oka lehet:

1. NODE.exe nincs jelen hello megfelelő helyen. Jelölje be nodeProcessCommandLine beállítást.
2. Fő parancsfájl nincs jelen hello megfelelő helyen. Ellenőrizze a Web.config fájlban, és győződjön meg arról, hogy hello fő parancsfájl hello nevét hello kezelők szakasz egyezések hello fő parancsfájlba.
3. Web.config konfigurációra nincs megfelelő – hello beállítások neveket és-értékek ellenőrzése.
4. Hidegindítás – az alkalmazás toostartup túl sokáig tart. Ha az alkalmazás tovább tart, mint (maxNamedPipeConnectionRetry \* namedPipeConnectionRetryDelay) / 1000 másodperc, az iisnode 500 hibát ad vissza. Ezek az alkalmazás idő tooprevent iisnode indítása az időtúllépés miatt, és vissza hello 500-as hiba beállítások toomatch hello értékének növelése.

### <a name="my-node-application-crashed"></a>A csomópont alkalmazás összeomlott
Az alkalmazás szűrész nem kezelt kivételek – kérjük ellenőrzés d:\\otthoni\\naplófájlok\\alkalmazás\\hello kivétel lépett fel az hello részletes naplózás-errors.txt fájlt. Ez a fájl rendelkezik hello Veremkivonat, így ezt úgy javíthatja ki az alkalmazás ennek alapján.

### <a name="my-node-application-takes-too-much-time-toostartup-cold-start"></a>A csomópont-alkalmazás tart túl sok idő toostartup (Cold indítás)
Ennek leggyakoribb oka az, hogy rendelkezik-e nagy mennyiségű hello csomópontban fájlok hello alkalmazás\_modulok és hello alkalmazás záma tooload nagy része a rendszerindítás során ezeket a fájlokat. Alapértelmezés szerint a fájlok találhatók hello hálózati megosztáson lévő Azure webalkalmazás, mivel betöltése sok fájlok némi időbe telhet.
Néhány megoldások toomake Ez gyorsabb a következők:

1. Ellenőrizze, hogy az egyszerű függőségi struktúra és a nem ismétlődő függőség npm3 tooinstall a modulok használatával.
2. Próbálja toolazy betöltése a csomópont\_modulok, és nem indításkor hello modulok mindegyikének történik. Ez azt jelenti, hogy hello hívás toorequire('module') kell hajtható végre, ha ténylegesen szükséges hello függvényen belül toouse hello modul meg.
3. Az Azure webalkalmazás kínál a helyi gyorsítótár nevezett szolgáltatással. Ez a funkció a tartalom hello hálózati megosztás toohello helyi lemez a virtuális gép hello másolja át. Mivel hello fájlok helyi, hello betöltési ideje csomópont\_modulok sokkal gyorsabb. -A [dokumentáció](../app-service/app-service-local-cache.md) azt ismerteti, hogyan toouse a helyi gyorsítótár több részletesen.

## <a name="iisnode-http-status-and-substatus"></a>Az IISNODE http-állapot és részállapot
Ez [forrásfájl](https://github.com/Azure/iisnode/blob/master/src/iisnode/cnodeconstants.h) hiba esetén lépjen vissza az összes hello lehetséges állapota/részállapot kombinációja iisnode listája.

Az alkalmazás toosee hello win32 hibakód FREB engedélyezése (Ellenőrizze, hogy csak a nem éles helyeken a teljesítményre vonatkozó megfontolásból FREB engedélyezi).

| HTTP-állapot | HTTP-SubStatus | Lehetséges ok? |
| --- | --- | --- |
| 500 |1000 |Hiba történt az egyes hello kérelem tooIISNODE terjesztéséhez probléma – ellenőrizze, hogy node.exe indult-e. Indításkor node.exe sikerült összeomlott. Ellenőrizze a web.config konfigurációs hibák. |
| 500 |1001 |-Win32Error 0x2 - alkalmazás toohello URL-cím nem válaszol. Jelölőnégyzet URL-újraíró, szabályok, vagy ha az express alkalmazást rendelkezik-e definiálva hello helyes útvonalak. -Win32Error 0x6d – nevesített cső elfoglalva, – Node.exe nem fogad el kérelmek mert hello cső foglalt. Ellenőrizze a nagy cpu-használat. -Más hibák – ellenőrizze, hogy ha node.exe összeomlott. |
| 500 |1002 |NODE.exe összeomlott – d: Ellenőrizze\\otthoni\\naplófájlok\\naplózási-errors.txt Veremkivonat a. |
| 500 |1003 |Az adatcsatorna konfigurációs probléma – soha nem kell megjelennie a, de ha így tesz, hello nevű pipe-konfiguráció nem megfelelő. |
| 500 |1004-1018 |Néhány hiba történt hello kérelem vagy feldolgozási hello válasz belőle node.exe küldése során. Ellenőrizze, hogy node.exe összeomlott. Ellenőrizze a d:\\otthoni\\naplófájlok\\naplózási-errors.txt Veremkivonat a. |
| 503 |1000 |Nincs elég memória tooallocate több nevű cső kapcsolat. Ellenőrizze, hogy miért az alkalmazás nem használ a sok memóriát. Ellenőrizze a maxConcurrentRequestsPerProcess beállítás értékét. Ha nem végtelen, és rendelkezik a nagy mennyiségű kérést, növelje a érték tooprevent ezt a hibát. |
| 503 |1001 |Kérelem nem sikerült elküldött toonode.exe, mert hello alkalmazás újrahasznosítása van. Miután hello alkalmazás rendelkezik felhasználását, kérelmek szokásos módon fel kell dolgozni. |
| 503 |1002 |Jelölőnégyzet win32 hibakód tényleges okból – kérést nem lehetett elküldött tooa node.exe. |
| 503 |1003 |Nevesített cső jelenleg túl elfoglalt – ellenőrizze, hogy ha a csomópont nem használ-e nagy mennyiségű Processzor |

Egy beállítás belül nevű csomópont NODE.exe\_FÜGGŐBEN\_CSŐ\_példányok. Azure webalkalmazás kívül alapértelmezés szerint ez az érték 4. Ez azt jelenti, hogy node.exe 4 kérés törléshez a nevesített cső hello egyszerre. Az Azure-webalkalmazás az alapérték too5000, és ez az érték elég jó azure webalkalmazás futó legtöbb csomópont alkalmazások kell lennie. Nem megjelenik 503.1003 azure webalkalmazás mert hello csomópont a magas értéket kell\_FÜGGŐBEN\_CSŐ\_példányok.  |

## <a name="more-resources"></a>További erőforrások
Kövesse a további információk a node.js-alkalmazások hivatkozások toolearn Azure App Service.

* [Ismerkedés a Node.js webalkalmazásokkal az Azure App Service-ben](app-service-web-get-started-nodejs.md)
* [Hogyan toodebug egy Node.js webalkalmazás az Azure App Service-ben](web-sites-nodejs-debug.md)
* [A Node.js modulok használata az Azure alkalmazásokkal](../nodejs-use-node-modules-azure-apps.md)
* [Azure App Service Web Apps: Node.js](https://blogs.msdn.microsoft.com/silverlining/2012/06/14/windows-azure-websites-node-js/)
* [Node.js fejlesztői központ](../nodejs-use-node-modules-azure-apps.md)
* [Hello Super titkos Kudu hibakereső konzol felfedezése](https://azure.microsoft.com/documentation/videos/super-secret-kudu-debug-console-for-azure-web-sites/)

