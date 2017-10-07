---
title: "aaaHow toowork hello Node.js háttérrendszer kiszolgálóval a Mobile Apps SDK |} Microsoft Docs"
description: "Ismerje meg, hogyan toowork az Azure App Service Mobile Apps Node.js háttérkiszolgáló SDK hello."
services: app-service\mobile
documentationcenter: 
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: e7d97d3b-356e-4fb3-ba88-38ecbda5ea50
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: node
ms.topic: article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: 2b1ea5fda6f6ca422b92fe29ff8d16bf035018d9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-hello-azure-mobile-apps-nodejs-sdk"></a>Hogyan toouse hello Azure Mobile Apps Node.js SDK
[!INCLUDE [app-service-mobile-selector-server-sdk](../../includes/app-service-mobile-selector-server-sdk.md)]

Ez a cikk ismerteti a részletes útmutatást és példákat megjelenítő hogyan toowork az Azure App Service Mobile Apps a Node.js-háttéralkalmazáshoz.

## <a name="Introduction"></a>Bevezetés
Az Azure App Service Mobile Apps hello funkció mobile optimalizált tooadd adatelérési Web API tooa webalkalmazás biztosít.  az ASP.NET és a Node.js webes alkalmazásokhoz, Azure App Service Mobile Apps SDK hello valósul meg.  hello SDK tartalmazza a következő műveletek hello:

* Az adatelérési tábla műveletek (olvasás, Insert, Update, Delete)
* Egyéni API-műveletek

Mindkét műveletek Azure App Service, beleértve a közösségi Identitásszolgáltatók például a Facebookhoz, Twitter, Google és a Microsoft, valamint Azure Active Directory vállalati identitás által engedélyezett összes Identitásszolgáltatók biztosít a hitelesítéshez.

Minták minden egyes felhasználási eseténél a hello található [minták directory a Githubon].

## <a name="supported-platforms"></a>A támogatott platformok
hello Azure Mobile Apps csomópont SDK aktuális LTS kiadási csomópont, és később hello támogatja.  Től írást, hello legújabb LTS verzió a csomópont v4.5.0.  A csomópont más verzióiban előfordulhat, hogy működik, de nem támogatottak.

Azure Mobile Apps csomópont SDK hello támogatja a két adatbázis illesztőprogram - hello csomópont-mssql illesztőprogram támogatja az SQL Azure és a helyi SQL Server-példányokat.  hello sqlite3 illesztőprogram csak egyetlen példányán SQLite adatbázisokat támogatja.

### <a name="howto-cmdline-basicapp"></a>Hogyan: hozzon létre egy alapszintű Node.js-háttéralkalmazáshoz hello parancssor használatával
Minden Azure App Service Mobile App Node.js-háttéralkalmazáshoz ExpressJS alkalmazásként indul.  ExpressJS hello legnépszerűbb web service keretrendszer Node.js érhető el.  Létrehozhat egy alapszintű [Express] alkalmazás az alábbiak szerint:

1. Egy parancs vagy a PowerShell-ablakot hozzon létre egy könyvtárat a projekthez.

        mkdir basicapp
2. Futtassa az npm init tooinitialize hello csomag struktúra.

        cd basicapp
        npm init

    hello npm init parancs kérdések tooinitialize hello projekt készlete kéri.  Lásd: hello egy példa a kimenetre:

    ![hello npm init kimeneti][0]
3. Telepítse a hello expressz és az azure-mobilalkalmazások szalagtárak adattárból hello npm.

        npm install --save express azure-mobile-apps
4. Az app.js fájl tooimplement hello alapvető mobil kiszolgáló létrehozása.

        var express = require('express'),
            azureMobileApps = require('azure-mobile-apps');

        var app = express(),
            mobile = azureMobileApps();

        // Define a TodoItem table
        mobile.tables.add('TodoItem');

        // Add hello mobile API so it is accessible as a Web API
        app.use(mobile);

        // Start listening on HTTP
        app.listen(process.env.PORT || 3000);

Ez az alkalmazás egy végpontot hoz létre egy mobile optimalizált WebAPI (`/tables/TodoItem`), amely biztosítja, nem hitelesített hozzáférést tooan az alapul szolgáló SQL-adattár egy dinamikus sémáját használja.  Alkalmas az ügyfél szalagtár gyors üzembe helyezések a következő:

* [Android ügyfél gyors üzembe helyezés]
* [Apache Cordova-ügyfél gyors üzembe helyezés]
* [iOS ügyfél gyors üzembe helyezés]
* [A Windows Store ügyfél gyors üzembe helyezés]
* [Xamarin.iOS ügyfél gyors üzembe helyezés]
* [Xamarin.Android ügyfél gyors üzembe helyezés]
* [Xamarin.Forms-ügyfél gyors üzembe helyezés]

Ezen alapszintű hello alkalmazás hello kód található [basicapp mintát a Githubon].

### <a name="howto-vs2015-basicapp"></a>Útmutató: a Visual Studio 2015 csomópont-háttéralkalmazás létrehozása
Visual Studio 2015-öt egy bővítmény toodevelop Node.js-alkalmazások hello IDE belül van szükség.  toostart, telepítés hello [Node.js eszközök 1.1 a Visual Studio].  Hello Node.js Tools for Visual Studio telepítése után hozzon létre egy Express 4.x alkalmazást:

1. Nyissa meg hello **új projekt** párbeszédpanelen (a **fájl** > **új** > **projekt …** ).
2. Bontsa ki a **sablonok** > **JavaScript** > **Node.js**.
3. Jelölje be hello **alapszintű Azure Node.js Express 4 Application**.
4. Töltse ki hello projekt nevét.  Kattintson az *OK* gombra.

    ![Visual Studio 2015-öt új projekt][1]
5. Kattintson a jobb gombbal hello **npm** csomópont, és válassza **telepítése új npm csomagok...** .
6. Szükség lehet toorefresh hello npm katalógus az első Node.js-alkalmazás létrehozása.  Kattintson a **frissítése** szükség esetén.
7. Adja meg *azure-mobilalkalmazások* hello Keresés mezőbe.  Kattintson a hello **azure-mobileszköz-alkalmazások 2.0.0** csomagot, majd kattintson az **csomagtelepítés**.

    ![Új npm-csomagok][2]
8. Kattintson a **Bezárás** gombra.
9. Nyissa meg hello *app.js* tooadd támogatás az Azure Mobile Apps SDK hello.  Sor 6 at hello aljához hello könyvtár utasítás szükséges, adja hozzá a következő kód hello:

        var bodyParser = require('body-parser');
        var azureMobileApps = require('azure-mobile-apps');

    Egyéb app.use utasítások körülbelül sor 27 hello után adja hozzá a hello a következő kódot:

        app.use('/users', users);

        // Azure Mobile Apps Initialization
        var mobile = azureMobileApps();
        mobile.tables.add('TodoItem');
        app.use(mobile);

    Hello fájl mentéséhez.
10. Futtassa helyileg hello alkalmazást (hello API kiszolgált a http://localhost: 3000), vagy tooAzure közzététele.

### <a name="create-node-backend-portal"></a>Útmutató: Azure-portálon hello használata Node.js-háttéralkalmazás létrehozása
Létrehozhat egy mobilalkalmazás-háttérrendszer jobb hello [Azure-portálon]. Következő lépések, vagy hozzon létre egy ügyfél és kiszolgáló együtt a következő hello hello vagy követésével [mobilalkalmazás létrehozása](app-service-mobile-ios-get-started.md) oktatóanyag. hello oktatóanyag tartalmazza ezeket az utasításokat egy egyszerűsített verziója és a projektek a koncepció igazolása.

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service-classic](../../includes/app-service-mobile-dotnet-backend-create-new-service-classic.md)]

Vissza a hello *Ismerkedés* panelen, a **tábla API létrehozása**, válassza a **Node.js** , a **háttéralkalmazás-nyelv**.
Hello jelölőnégyzetet, a "**tudomásul veszem, hogy ezzel a művelettel felülírja az összes hely tartalmának.**", majd kattintson a **TodoItem tábla létrehozása**.

### <a name="download-quickstart"></a>Hogyan: letöltési hello Node.js háttérrendszer gyors üzembe helyezés kód projekt Git használatával
Ha Node.js Mobile Apps-háttéralkalmazás létrehozása hello portál használatával **gyors üzembe helyezési** panelen, egy Node.js-projektet hoz létre, és központilag telepített tooyour hely. Adja hozzá a táblák és API-kat, és Node.js-háttéralkalmazáshoz hello hello portálon tartozó kódfájlok szerkesztése. Különböző központi telepítési eszközök toodownload hello háttéralkalmazás-projekt, hogy hozzáadása vagy módosítása a táblák és API-k, majd közzé hello projekt is használható. További információkért lásd: a [Azure App Service telepítési útmutató]. hello következő eljárás használja a Git tárház toodownload hello gyors üzembe helyezési projekt kódját.

1. Ha még nem tette, telepítse a Git esetében. hello lépéseket szükséges tooinstall Git eltérő operációs rendszerek között. Lásd: [telepítése Git](http://git-scm.com/book/en/Getting-Started-Installing-Git) az operációsrendszer-specifikus disztribúcióiról, valamint a telepítési útmutatót.
2. Hello kövesse [engedélyezése hello App Service alkalmazás tárház](../app-service-web/app-service-deploy-local-git.md#Step3) tooenable hello Git-tárház háttér webhelyét, és jegyezze fel hello telepítési felhasználónevet és jelszót.
3. A mobilalkalmazás háttérrendszerének hello panelen, jegyezze fel a hello **Git-klón URL-cím** beállítást.
4. Hello végrehajtása `git clone` parancs használatával hello Git klón URL-címet írja be a jelszót, ha szükséges, az alábbi példában látható módon:

        $ git clone https://username@todolist.scm.azurewebsites.net:443/todolist.git
5. Tallózás toolocal könyvtár, amely a fenti példa hello /todolist, és figyelje meg, hogy a projektfájlok le vannak töltve. Keresse meg a hello `todoitem.json` hello fájlban `/tables` könyvtár.  Ez a fájl engedélyeit a tábla határozza meg.  Is megkeresi a hello `todoitem.js` hello fájlt azonos könyvtárban, amely meghatározza, hogy a CRUD művelet parancsfájlok hello tábla.
6. Módosításokat végzett tooproject fájlokat, miután hajtható végre a következő parancsok tooadd, véglegesítése, majd töltse fel a változások toohello hely hello:

        $ git commit -m "updated hello table script"
        $ git push origin master

    Amikor új fájlok toohello projekt, először tooexecute hello `git add .` parancsot.

hello webhely minden alkalommal, amikor véglegesíti az új készlet fejlesztőre toohello webhely ismételt közzététele.

### <a name="howto-publish-to-azure"></a>Útmutató: a Node.js háttérrendszer tooAzure közzététele
Microsoft Azure közzététel az Azure App Service Mobile Apps Node.js-háttéralkalmazását az Azure szolgáltatás hello számos mechanizmust biztosít.  Ezek közé tartozik a központi telepítési eszközöket a Visual Studio integrált, parancssori eszközök és verziókövetési alapuló folyamatos üzembe helyezés beállítások használatával.  A témakörrel kapcsolatos további információkért lásd: a [Azure App Service telepítési útmutató].

Az Azure App Service a Node.js-alkalmazás üzembe helyezése előtt tekintse át a konkrét útmutatásért rendelkezik:

* Hogyan túl[hello csomópont verzió megadása]
* Hogyan túl[csomópont modulok használata]

### <a name="howto-enable-homepage"></a>Útmutató: az alkalmazás kezdőlapját engedélyezése
Számos alkalmazás webes és mobilalkalmazások és hello ExpressJS keretrendszer lehetővé teszi a két toocombine értékkorlátozást.  Egyes esetekben azonban Kezdésként tooonly alkalmazzon egy mobil felületet.  Egy követően lap tooensure hello alkalmazásszolgáltatás megfelelően működik, és hasznos tooprovide.  Adja meg a saját kezdőlapján, vagy egy ideiglenes kezdőlap engedélyezése.  egy ideiglenes kezdőlapján tooenable hello tooinstantiate Azure Mobile Apps a következő használja:

    var mobile = azureMobileApps({ homePage: true });

Ha csak ez a beállítás érhető el helyi fejlesztésekor, ez a beállítás tooyour adhat hozzá `azureMobile.js` fájlt.

## <a name="TableOperations"></a>Tábla műveletek
hello azure-mobilalkalmazások Node.js Server SDK mechanizmusok tooexpose adatokat biztosít az Azure SQL Database, a WebAPI tárolja.  Öt műveleti rendelkezésre állnak.

| Művelet | Leírás |
| --- | --- |
| GET /tables/*táblanév* |Az összes rekord hello tábla beolvasása |
| GET /tables/*tablename*/:id |Egy megadott rekord hello tábla beolvasása |
| POST /tables/*táblanév* |Hozzon létre egy bejegyzést a hello tábla |
| JAVÍTÁS /tables/*tablename*/:id |Módosítani egy rekordot hello tábla |
| TÖRLÉS /tables/*tablename*/:id |Hello tábla rekord törlése |

Támogatja a WebAPI [OData] és kiterjesztik az hello tábla séma toosupport [offline adatszinkronizálás].

### <a name="howto-dynamicschema"></a>Hogyan: Adja meg a táblák egy dinamikus séma használata
Egy tábla használt, meg kell határozni.  Táblák meghatározása (ahol hello fejlesztői meghatározása hello oszlopok hello sémáján belül) statikus séma vagy dinamikusan (ahol hello SDK szabályozza a bejövő kérések alapján hello séma). Ezenkívül hello fejlesztői irányítani tudja adott aspektusait hello WebAPI Javascript-kód toohello definition hozzáadásával.

Ajánlott eljárásként akkor kell mindegyik tábla hello táblák könyvtárban Javascript-fájlt ad meg, majd a tables.import() metódus tooimport hello táblázatot használnak.  Hello basic alkalmazáson belüli kiterjesztése esetén hello app.js fájl ki kell igazítani:

    var express = require('express'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Define hello database schema that is exposed
    mobile.tables.import('./tables');

    // Provide initialization of any tables that are statically defined
    mobile.tables.initialize().then(function () {
        // Add hello mobile API so it is accessible as a Web API
        app.use(mobile);

        // Start listening on HTTP
        app.listen(process.env.PORT || 3000);
    });

Adja meg a hello tábla. vagy tables/TodoItem.js:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Additional configuration for hello table goes here

    module.exports = table;

Táblák alapértelmezés szerint dinamikus séma használja.  dinamikus séma ki tooturn globálisan, állítsa be a hello Alkalmazásbeállítás **MS_DynamicSchema** toofalse hello Azure-portálon belül.

Teljes példa található hello [todo mintát a Githubon].

### <a name="howto-staticschema"></a>Hogyan: Adja meg a táblák egy statikus séma használata
Explicit módon meghatározhatja hello oszlopok tooexpose hello WebAPI keresztül.  hello azure-mobilalkalmazások Node.js SDK automatikusan hozzáadja a kapcsolat nélküli szinkronizálás toohello lista, amely megadja a szükséges további oszlopokat.  Például a gyors üzembe helyezés ügyfélalkalmazások szükséges két oszlopokkal rendelkező táblát: text (szöveg) (logikai) hajtsa végre.  
hello tábla hello tábla definícióját JavaScript-fájlt (hello táblák könyvtárában található) az alábbiak szerint lehet meghatározni:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define hello columns within hello table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off dynamic schema
    table.dynamicSchema = false;

    module.exports = table;

Ha statikusan határozza meg a táblák, majd kell is meghívja a hello tables.initialize() metódus toocreate hello adatbázisséma indításakor.  hello tables.initialize() metódus visszaadja a [ígéret] , hogy hello webszolgáltatás nem teljesíti a kérelmeket hello adatbázis inicializálása előtt.

### <a name="howto-sqlexpress-setup"></a>Hogyan: használja az SQL Express fejlesztési adattárként a helyi számítógépen
hello Azure Mobile Apps hello AzureMobile alkalmazások csomópont SDK három lehetőséget kínál a kiszolgáló hello kezdő verzióról: SDK hello kezdő verzióról kiszolgáló három lehetőséget biztosít:

* Használjon hello **memória** illesztőprogramtárába egy nem állandó tooprovide – példa
* Használjon hello **mssql** illesztőprogram tooprovide fejlesztési SQL Express adattárat
* Használjon hello **mssql** illesztőprogram tooprovide egy Azure SQL Database adattároló termelési környezetben

hello Azure Mobile Apps Node.js SDK-t használ a hello [mssql Node.js csomag] tooestablish és az SQL Express kapcsolat tooboth és SQL-adatbázis használata.  A csomag igényli-e, hogy az SQL Express-példány TCP-kapcsolatok engedélyezése.

> [!TIP]
> hello memória illesztőprogram nem biztosít tesztelési létesítményekben teljes készletét.  Ha a tootest a háttéralkalmazást helyileg, azt egy SQL Express adattárból hello használatát javasoljuk, és hello mssql illesztőprogram.
>
>

1. Töltse le és telepítse [Microsoft SQL Server 2014 Express].  Ügyeljen arra, SQL Server 2014 Express hello eszközök kiadásával telepítse.  Ha 64 bites támogatás kifejezetten szüksége hello 32 bites verziója kevesebb memóriát igényel, futtatásakor.
2. Futtassa az SQL Server 2014 Configuration Manager hello.

   1. Bontsa ki a hello **SQL Server hálózati konfigurációja** hello bal oldali fában menü csomópontja.
   2. Kattintson a **SQLEXPRESS protokollok**.
   3. Kattintson a jobb gombbal **TCP/IP** válassza **engedélyezése**.  Kattintson a **OK** hello előugró párbeszédpanelen.
   4. Kattintson a jobb gombbal **TCP/IP** válassza **tulajdonságok**.
   5. Kattintson a hello **IP-címek** fülre.
   6. Hello található **IPAll** csomópont.  A hello **TCP-Port** mezőbe írja be **1433**.

          ![Configure SQL Express for TCP/IP][3]
   7. Kattintson az **OK** gombra.  Kattintson a **OK** hello előugró párbeszédpanelen.
   8. Kattintson a **SQL Server Services** hello bal oldali fában menüben.
   9. Kattintson a jobb gombbal **SQL Server (SQLEXPRESS)** válassza **újraindítása**
   10. Zárja be az SQL Server 2014 Configuration Manager hello.
3. Futtassa az SQL Server 2014 Management Studio hello, és csatlakozzon a helyi SQL Express példány tooyour

   1. Kattintson a jobb gombbal az Object Explorer hello a példányát, és válassza ki **tulajdonságai**
   2. Jelölje be hello **biztonsági** lap.
   3. Győződjön meg arról hello **SQL Server és a Windows-hitelesítés üzemmód** van kijelölve
   4. Kattintson az **OK** gombra

          ![Configure SQL Express Authentication][4]
   5. Bontsa ki a **biztonsági** > **bejelentkezések** az Object Explorer hello
   6. Kattintson a jobb gombbal **bejelentkezések** válassza **új bejelentkezés...**
   7. Adja meg a bejelentkezési nevet.  Kattintson az **SQL Server-hitelesítés** lehetőségre.  Adjon meg egy jelszót, majd adja meg a hello ugyanezt a jelszót **jelszó megerősítése**.  hello jelszónak meg kell felelnie a Windows bonyolultsági követelményeknek.
   8. Kattintson az **OK** gombra

          ![Add a new user tooSQL Express][5]
   9. Kattintson a jobb gombbal az új bejelentkezési adatait, és válassza ki **tulajdonságai**
   10. Jelölje be hello **kiszolgálói szerepkörök** lap
   11. Ellenőrizze hello mezőben következő toohello **dbcreator** kiszolgálói szerepkör
   12. Kattintson az **OK** gombra
   13. Zárja be az SQL Server 2015 Management Studio hello

Győződjön meg arról, hogy rekord hello felhasználónév és jelszó választotta.  A megadott adatbázis követelményeitől függően esetleg tooassign további kiszolgálói szerepkörök vagy engedélyek.

Node.js-alkalmazás hello beolvassa hello **SQLCONNSTR_MS_TableConnectionString** környezeti változó hello kapcsolati karakterláncot kell megadnia ehhez az adatbázishoz.  A változó beállított a környezetben.  Például használhatja PowerShell tooset e környezeti változó:

    $env:SQLCONNSTR_MS_TableConnectionString = "Server=127.0.0.1; Database=mytestdatabase; User Id=azuremobile; Password=T3stPa55word;"

TCP/IP-kapcsolaton keresztül érik el hello adatbázist, és adjon meg egy felhasználónevet és jelszót hello kapcsolat.

### <a name="howto-config-localdev"></a>Útmutató: a helyi fejlesztési projekt konfigurálása
Az Azure Mobile Apps beolvassa a JavaScript-fájl neve *azureMobile.js* hello helyi FileSystem.  Nem használja a fájl tooconfigure hello Azure Mobile Apps SDK üzemi - Alkalmazásbeállítások használható hello [Azure-portálon] helyette.  Hello *azureMobile.js* fájlba exportálja a konfigurációs objektum.  hello leggyakrabban használt beállítások a következők:

* Adatbázis-beállítások
* Diagnosztikai naplózás beállításait
* Alternatív CORS-beállítások

Példa *azureMobile.js* fájl végrehajtási hello megelőző adatbázis beállítások követi:

    module.exports = {
        cors: {
            origins: [ 'localhost' ]
        },
        data: {
            provider: 'mssql',
            server: '127.0.0.1',
            database: 'mytestdatabase',
            user: 'azuremobile',
            password: 'T3stPa55word'
        },
        logging: {
            level: 'verbose'
        }
    };

Javasoljuk, hogy adja hozzá *azureMobile.js* tooyour *.gitignore* fájlt (vagy más verziókövetési figyelmen kívül hagyja a fájlt) hello felhőben tárolt tooprevent jelszavakat.  Mindig adja meg az üzemi beállításait Alkalmazásbeállítások belül hello [Azure-portálon].

### <a name="howto-appsettings"></a>Útmutató: A mobilalkalmazás-beállításainak konfigurálása
A legtöbb beállításai a hello *azureMobile.js* fájl rendelkezik egyenértékű Alkalmazásbeállítás hello [Azure-portálon].  A következő lista tooconfigure hello az alkalmazás használata beállítást:

| Alkalmazás beállítása | *azureMobile.js* beállítás | Leírás | Érvényes értékek |
|:--- |:--- |:--- |:--- |
| **MS_MobileAppName** |név |hello hello alkalmazás nevét |Karakterlánc |
| **MS_MobileLoggingLevel** |Logging.level |Az üzenetek toolog minimális naplózási szint |Hiba, figyelmeztetés, információ, részletes, hibakeresési silly |
| **MS_DebugMode** |Hibakeresési |Engedélyezés vagy letiltás hibakeresési mód |IGAZ, hamis |
| **MS_TableSchema** |Data.Schema |SQL-táblák alapértelmezett séma neve |karakterlánc (alapértelmezett: dbo) |
| **MS_DynamicSchema** |data.dynamicSchema |Engedélyezés vagy letiltás hibakeresési mód |IGAZ, hamis |
| **MS_DisableVersionHeader** |verzió (set tooundefined) |Letiltja az X-ZUMO-kiszolgálóverzió hello fejléc |IGAZ, hamis |
| **MS_SkipVersionCheck** |skipversioncheck |Letiltja a hello ügyfél API-verziójának ellenőrzése |IGAZ, hamis |

egy Alkalmazásbeállítás tooset:

1. Jelentkezzen be toohello [Azure-portálon].
2. Válassza ki **összes erőforrás** vagy **alkalmazásszolgáltatások** kattintson a mobilalkalmazás hello nevére.
3. Alapértelmezés szerint hello-beállítások panel nyílik meg. Ha nem, kattintson a gombra **beállítások**.
4. Kattintson a **Alkalmazásbeállítások** hello általános menüben.
5. Görgessen toohello App Settings szakasz.
6. Ha az alkalmazás, beállítás már létezik, kattintson a hello app tooedit hello beállításérték hello értékét.
7. Ha az alkalmazás nem létezik, adja meg a hello Alkalmazásbeállítás hello kulcs és hello érték hello érték mezőbe.
8. Ha befejeződött, kattintson a **mentése**.

A legtöbb alkalmazás beállításainak módosítása a szolgáltatás újraindítását igényli.

### <a name="howto-use-sqlazure"></a>Útmutató: az üzemi adattároló SQL-adatbázis használata
<!--- ALTERNATE INCLUDE - we can't use ../includes/app-service-mobile-dotnet-backend-create-new-service.md - slightly different semantics -->

Az Azure SQL Database adattárként használata azonos Azure App Service-alkalmazás összes típusa. Ha még nem tette meg, kövesse ezeket a lépéseket toocreate a Mobile Apps-háttéralkalmazás.

1. Jelentkezzen be toohello [Azure-portálon].
2. A hello bal felső részén hello ablak, kattintson a hello **+ új** gomb > **Web + mobil** > **mobilalkalmazás**, majd adjon meg egy nevet a mobilalkalmazás háttérrendszerének.
3. A hello **erőforráscsoport** mezőbe írja be a hello ugyanazt a nevet az alkalmazáshoz.
4. hello alapértelmezett App Service-csomag van kiválasztva.  Ha toochange App Service-csomag, azt is megteheti hello App Service-csomag kattintva > **+ új**.  Adjon meg egy nevet az új App Service-csomag hello, és válasszon egy megfelelő helyet.  Kattintson a hello tarifacsomag, és válasszon egy megfelelő tarifacsomagot hello szolgáltatáshoz. Válassza ki **összes** több, mint az beállítások árképzési tooview **szabad** és **megosztott**.  Miután kiválasztotta a tarifacsomagot, kattintson a hello **válasszon** gombra.  Vissza a hello **App Service-csomag** panelen kattintson a **OK**.
5. Kattintson a **Create** (Létrehozás) gombra. A Mobile Apps-háttéralkalmazás kiépítés néhány percet is igénybe vehet.  Hello Mobile Apps-háttéralkalmazás kiépítését, követően hello portál megnyitása hello **beállítások** hello Mobile Apps-háttéralkalmazás paneljét.

Hello Mobile Apps-háttéralkalmazás létrehozása után dönthet úgy, tooeither csatlakozzon egy létező SQL adatbázis tooyour mobil-háttéralkalmazást, vagy hozzon létre egy új SQL-adatbázis.  Ebben a szakaszban a Microsoft SQL-adatbázis létrehozása.

> [!NOTE]
> Ha már rendelkezik adatbázissal hello hello mobil-háttéralkalmazás és ugyanazon a helyen, választhatja **meglévő adatbázis használata** , és válassza ki, hogy az adatbázis. egy adatbázist egy másik helyen lévő hello használata nem ajánlott magasabb késések miatt.
>
>

1. Hello új mobil-háttéralkalmazást, kattintson **beállítások** > **mobilalkalmazás** > **adatok** > **+ Hozzáadás**.
2. A hello **adatkapcsolat hozzáadása** panelen kattintson a **SQL adatbázis - kötelező beállítások konfigurálása** > **hozzon létre egy új adatbázist**.  Adja meg az új adatbázis hello hello nevét a hello **neve** mező.
3. Kattintson a **Server**.  A hello **új kiszolgáló** panelen adjon meg egy egyedi kiszolgálónevet a hello **kiszolgálónév** mezőben, majd adjon meg egy megfelelő **kiszolgáló-rendszergazdai bejelentkezés** és **jelszó**.  Győződjön meg arról **engedélyezése az azure szolgáltatások tooaccess server** be van jelölve.  Kattintson az **OK** gombra.

    ![Egy Azure SQL-adatbázis létrehozása][6]
4. A hello **új adatbázis** panelen kattintson a **OK**.
5. Vissza a hello **adatkapcsolat hozzáadása** panelen válassza **kapcsolati karakterlánc**, adja meg hello bejelentkezési és hello adatbázis létrehozásakor megadott jelszót.  Ha a meglévő adatbázis használata hello bejelentkezési hitelesítő adatok megadása, hogy az adatbázis.  Ha a megadott, kattintson **OK**.
6. Vissza a hello **adatkapcsolat hozzáadása** újra, kattintson a panel **OK** toocreate hello adatbázis.

<!--- END OF ALTERNATE INCLUDE -->

Hello adatbázisának létrehozása eltarthat néhány percig.  Használjon hello **értesítések** terület toomonitor hello hello központi telepítés végrehajtási állapotát.  Nem folytatódni mindaddig, amíg hello adatbázis sikeresen telepítve lett.  Amennyiben sikeresen telepített, hello SQL Database-példányt a mobil-háttéralkalmazást alkalmazás beállításaiban szereplő kapcsolati karakterláncot jön létre.  Láthatja, hogy ez a hello Alkalmazásbeállítás **beállítások** > **Alkalmazásbeállítások** > **kapcsolati karakterláncok**.

### <a name="howto-tables-auth"></a>Útmutató: hitelesítés megkövetelése a hozzáférési tootables
Ha a toouse App Service hitelesítés hello táblák végponttal, konfigurálnia kell App Service hitelesítés hello [Azure-portálon] első.  A hitelesítés beállítása az Azure App Service szolgáltatásban kapcsolatos további részletekért tekintse át a konfigurációs útmutató hello hello identitásszolgáltató az toouse szeretné:

* [Hogyan tooconfigure Azure Active Directory-hitelesítés]
* [Hogyan tooconfigure Facebook-hitelesítés]
* [Hogyan tooconfigure Google-hitelesítés]
* [Hogyan tooconfigure Microsoft Authentication]
* [Hogyan tooconfigure Twitter hitelesítés]

Minden tábla tulajdonsága hozzáférés használt toocontrol access toohello tábla használható.  a következő minta hello hitelesítés szükséges a statikusan megadott táblát tartalmazza.

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define hello columns within hello table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off dynamic schema
    table.dynamicSchema = false;

    // Require authentication tooaccess hello table
    table.access = 'authenticated';

    module.exports = table;

hello hozzáférés tulajdonságot is igénybe vehet, értékek egyike

* *Névtelen* azt jelzi, hogy hello ügyfélalkalmazás tooread adatok hitelesítés nélkül
* *hitelesített* azt jelzi, hogy hello ügyfélalkalmazás kell küldenie egy érvényes hitelesítési jogkivonat hello kérelemhez
* *letiltott* azt jelzi, hogy ez a táblázat jelenleg le van tiltva

Ha a hello access tulajdonság nincs meghatározva, nem hitelesített elérését.

### <a name="howto-tables-getidentity"></a>Útmutató: hitelesítés jogcímek a táblák használata
Beállíthat különböző hitelesítési beállításakor igényelt jogcímeket.  Ezeket a jogcímeket nem általában keresztül érhetők hello `context.user` objektum.  Azonban azokat lekérheti hello segítségével `context.user.getIdentity()` metódust.  Hello `getIdentity()` metódus, amely tooan objektum ígéret ad vissza.  hello objektum kulccsal van ellátva, a hitelesítési módszerrel (facebook, google, twitter, microsoftaccount vagy aad-ben).

Például beállította a Microsoft Account hitelesítésének és kérelem hello e-mail cím jogcímet, ha adhat hozzá hello e-mail cím toohello rekord, a következő táblázat a tartományvezérlő hello:

    var azureMobileApps = require('azure-mobile-apps');

    // Create a new table definition
    var table = azureMobileApps.table();

    table.columns = {
        "emailAddress": "string",
        "text": "string",
        "complete": "boolean"
    };
    table.dynamicSchema = false;
    table.access = 'authenticated';

    /**
    * Limit hello context query toothose records with hello authenticated user email address
    * @param {Context} context hello operation context
    * @returns {Promise} context execution Promise
    */
    function queryContextForEmail(context) {
        return context.user.getIdentity().then((data) => {
            context.query.where({ emailAddress: data.microsoftaccount.claims.emailaddress });
            return context.execute();
        });
    }

    /**
    * Adds hello email address from hello claims toohello context item - used for
    * insert operations
    * @param {Context} context hello operation context
    * @returns {Promise} context execution Promise
    */
    function addEmailToContext(context) {
        return context.user.getIdentity().then((data) => {
            context.item.emailAddress = data.microsoftaccount.claims.emailaddress;
            return context.execute();
        });
    }

    // Configure specific code when hello client does a request
    // READ - only return records belonging toohello authenticated user
    table.read(queryContextForEmail);

    // CREATE - add or overwrite hello userId based on hello authenticated user
    table.insert(addEmailToContext);

    // UPDATE - only allow updating of record belong toohello authenticated user
    table.update(queryContextForEmail);

    // DELETE - only allow deletion of records belong toohello authenticated uer
    table.delete(queryContextForEmail);

    module.exports = table;

toosee milyen jogcímek elérhető, hogy egy webes böngésző tooview hello használata `/.auth/me` végpont a webhely.

### <a name="howto-tables-disabled"></a>Hogyan: toospecific tábla műveletek letiltása
Továbbá tooappearing hello táblán, a hello hozzáférés tulajdonság használt toocontrol műveletenként is lehet.  Nincsenek négy műveletet:

* *olvassa el* hello hello tábla RESTful BEOLVASNI művelet
* *Helyezze be* hello RESTful POST művelet hello tábla
* *frissítés* hello tábla hello RESTful javítás művelet
* *Törlés* hello RESTful DELETE művelet hello táblán van

Például előfordulhat, hogy kívánja tooprovide egy csak olvasható nem hitelesített táblázatot:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Read-Only table - only allow READ operations
    table.read.access = 'anonymous';
    table.insert.access = 'disabled';
    table.update.access = 'disabled';
    table.delete.access = 'disabled';

    module.exports = table;

### <a name="howto-tables-query"></a>Útmutató: a tábla műveletekkel használható hello lekérdezés beállítása
Kötelező megadni a tábla műveletek tooprovide hello adatok korlátozott nézete.  Megadhat például hello címkéje tábla hitelesített felhasználói Azonosítóját, hogy csak olvasható vagy a saját rekordok frissítése.  a következő tábla definíciójában hello ezt a szolgáltatást biztosítja:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define a static schema for hello table
    table.columns = {
        "userId": "string",
        "text": "string",
        "complete": "boolean"
    };
    table.dynamicSchema = false;

    // Require authentication for this table
    table.access = 'authenticated';

    // Ensure that only records for hello authenticated user are retrieved
    table.read(function (context) {
        context.query.where({ userId: context.user.id });
        return context.execute();
    });

    // When adding records, add or overwrite hello userId with hello authenticated user
    table.insert(function (context) {
        context.item.userId = context.user.id;
        return context.execute();
    });

    module.exports = table;

Műveletek általában a lekérdezés végrehajtása rendelkezik egy lekérdezés tulajdonságot, amelynek módosíthatja, a where záradékban. hello lekérdezés tulajdonság egy [QueryJS] objektum, amely egy OData-lekérdezés toosomething, amely az adatok háttérbeli hello használt tooconvert tud feldolgozni.  Egyszerű egyenlőség esetekben (például egy megelőző hello) térképre használhatók. SQL záradékokat is hozzáadhat:

    context.query.where('myfield eq ?', 'value');

### <a name="howto-tables-softdelete"></a>Útmutató: egy helyreállítható törlésre konfigurálása
Helyreállítható törlésre ténylegesen nem törli a rekordok.  Ehelyett azt jelöli meg azokat úgy, hogy törölte hello oszlop tootrue hello adatbázis törlése.  hello Azure Mobile Apps SDK automatikusan eltávolítja helyreállíthatóan törölt rekordok eredmények, kivéve, ha hello Mobile ügyfél SDK-t használ IncludeDeleted().  az ideiglenes tábla törléséhez tooconfigure hello beállítása `softDelete` tulajdonság hello tábla szolgáltatásdefiníciós fájlban:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define hello columns within hello table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off dynamic schema
    table.dynamicSchema = false;

    // Turn on Soft Delete
    table.softDelete = true;

    // Require authentication tooaccess hello table
    table.access = 'authenticated';

    module.exports = table;

Létre kell hoznia egy olyan mechanizmus kiürítése rekordok - vagy a ügyfélalkalmazás keresztül webjobs-feladat, Azure-függvény, vagy egy egyéni API-n keresztül.

### <a name="howto-tables-seeding"></a>Útmutató: az adatbázis adatokkal magtípusú leképezést
Amikor egy új alkalmazást hoz létre, Kezdésként tooseed adatokat tartalmazó táblát.  Ezt megteheti belül hello tábla definícióját JavaScript-fájlt az alábbiak szerint:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define hello columns within hello table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };
    table.seed = [
        { text: 'Example 1', complete: false },
        { text: 'Example 2', complete: true }
    ];

    // Turn off dynamic schema
    table.dynamicSchema = false;

    // Require authentication tooaccess hello table
    table.access = 'authenticated';

    module.exports = table;

Az adatok összehangolása csak történik hello Azure Mobile Apps SDK hello tábla létrehozásakor.  Hello tábla már létezik hello adatbázison belül, ha a program hello táblába beszúrta nincsenek adatok.  Ha dinamikus séma be van kapcsolva, akkor a séma táblanévhez rendezés hello adatokból.

Azt javasoljuk, hogy explicit módon hívjon hello `tables.initialize()` metódus toocreate hello tábla hello szolgáltatás elindul.

### <a name="Swagger"></a>Útmutató: a Swagger-támogatás engedélyezése
Az Azure App Service Mobile Apps mellékelt beépített [Swagger] támogatja.  tooenable Swagger-támogatást, először telepítenie hello swagger-ui a függőség beállításához:

    npm install --save swagger-ui

A telepítést követően engedélyezheti a Swagger támogatását hello Azure Mobile Apps konstruktorban:

    var mobile = azureMobileApps({ swagger: true });

Ön valószínűleg csak kívánt tooenable Swagger támogatási fejlesztési kiadásában.  Ehhez felügyelniük a `NODE_ENV` Alkalmazásbeállítás:

    var mobile = azureMobileApps({ swagger: process.env.NODE_ENV !== 'production' });

hello swagger-végpont itt található: http://*yoursite*.azurewebsites.net/swagger.  Swagger felhasználói felület hello hello keresztül érheti el `/swagger/ui` végpont.  Ha a teljes alkalmazás közötti toorequire hitelesítés, a Swagger hiba hoz létre.  A legjobb eredmények elérése érdekében válassza ki a hitelesítés nélküli tooallow kérelmeket keresztül hello Azure App Service Authentication / engedélyezési beállításait, majd vezérlőt hitelesítéssel hello `table.access` tulajdonság.

Azt is megteheti hello Swagger beállítás tooyour `azureMobile.js` fájlt, ha azt szeretné csak Swagger támogatási helyileg fejlesztése során.

## <a name="a-namepushpush-notifications"></a><a name="push">Leküldéses értesítések
Mobile Apps integrálódik az Azure Notification Hubs tooenable, toosend célzott leküldéses értesítések toomillions eszközök minden fő platformon. A Notification Hubs használatával küldhet leküldéses értesítések tooiOS, Android és Windows rendszerű eszközökhöz. További információk az akkor lehet elvégezni a segítségével toolearn Notification hubs használatával, lásd: [Notification Hubs – áttekintés](../notification-hubs/notification-hubs-push-notification-overview.md).

### </a><a name="send-push"></a>Hogyan: leküldéses értesítések küldése
a következő kód hello látható toouse hello leküldéses objektum toosend közvetítés a leküldéses értesítési tooregistered iOS-eszközök:

    // Create an APNS payload.
    var payload = '{"aps": {"alert": "This is an APNS payload."}}';

    // Only do hello push if configured
    if (context.push) {
        // Send a push notification using APNS.
        context.push.apns.send(null, payload, function (error) {
            if (error) {
                // Do something or log hello error.
            }
        });
    }

Hozzon létre egy sablon leküldéses regisztrációs hello ügyfélről, ehelyett küldhet egy sablon leküldéses üzenet toodevices az összes támogatott platformon. a következő kód bemutatja hogyan hello toosend sablon értesítést:

    // Define hello template payload.
    var payload = '{"messageParam": "This is a template payload."}';

    // Only do hello push if configured
    if (context.push) {
        // Send a template notification.
        context.push.send(null, payload, function (error) {
            if (error) {
                // Do something or log hello error.
            }
        });
    }


### <a name="push-user"></a>Hogyan: küldés leküldéses értesítések tooan hitelesített felhasználó címkék használatával
Amikor a hitelesített felhasználó regisztrálja a leküldéses értesítések, a felhasználói azonosító címke automatikusan kerül toohello regisztrációs. Ezt a címkét használatával küldhet leküldéses értesítések tooall eszközök egy adott felhasználó regisztrálja. hello alábbira lekérdezi hello hello kérelmező felhasználó biztonsági azonosítója és a hozzájuk a sablon leküldéses értesítés tooevery eszköz regisztrálása, hogy a felhasználó:

    // Only do hello push if configured
    if (context.push) {
        // Send a notification toohello current user.
        context.push.send(context.user.id, payload, function (error) {
            if (error) {
                // Do something or log hello error.
            }
        });
    }

Amikor regisztrál egy hitelesített ügyfél leküldéses értesítésekhez, győződjön meg arról, hogy a hitelesítési teljes regisztrációs megkísérlése előtt.

## <a name="CustomAPI"></a>Egyéni API-k
### <a name="howto-customapi-basic"></a>Hogyan: Adja meg egy egyéni API
Ezenkívül toohello adatokat érhetnek el hello /tables végpont API, Azure Mobile Apps is biztosít egyéni API.  Egyéni API-k egy hasonló módon toohello definíciói vannak definiálva, és elérheti hello ugyanazokat a lehetőségeket, például a hitelesítés.

Ha a toouse App Service hitelesítés egy egyéni API-t, konfigurálnia kell App Service hitelesítés hello [Azure-portálon] első.  A hitelesítés beállítása az Azure App Service szolgáltatásban kapcsolatos további részletekért tekintse át a konfigurációs útmutató hello hello identitásszolgáltató az toouse szeretné:

* [Hogyan tooconfigure Azure Active Directory-hitelesítés]
* [Hogyan tooconfigure Facebook-hitelesítés]
* [Hogyan tooconfigure Google-hitelesítés]
* [Hogyan tooconfigure Microsoft Authentication]
* [Hogyan tooconfigure Twitter hitelesítés]

Egyéni API-k meghatározott nagy hello azonos módon hello táblák API.

1. Hozzon létre egy **api** könyvtár
2. Hozzon létre egy API definition JavaScript fájlt hello **api** könyvtár.
3. Használjon hello importálás módszer tooimport hello **api** könyvtár.

Itt található hello prototípus api definition korábbi használtuk hello basic alkalmazáson belüli minta alapján.

    var express = require('express'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Import hello Custom API
    mobile.api.import('./api');

    // Add hello mobile API so it is accessible as a Web API
    app.use(mobile);

    // Start listening on HTTP
    app.listen(process.env.PORT || 3000);

Vegyünk egy példát hello server dátumot hello visszaadó API *Date.now()* metódust.  Hello api/date.js fájl itt található:

    var api = {
        get: function (req, res, next) {
            var date = { currentTime: Date.now() };
            res.status(200).type('application/json').send(date);
        });
    };

    module.exports = api;

Minden paraméter hello szabványos RESTful műveletek egyike - GET, POST, javítását vagy törlése.  hello metódus szabványos [ExpressJS köztes] függvény által küldött szükséges hello kimeneti.

### <a name="howto-customapi-auth"></a>Útmutató: hitelesítés szükséges hozzáférési tooa egyéni API-hoz.
Az Azure Mobile Apps SDK megvalósítja a hitelesítési hello a megszokott módon hello táblák végpont és egyéni API-k.  Hitelesítési toohello API hello előző szakaszban létrehozott felvételéhez adja hozzá egy **hozzáférés** tulajdonság:

    var api = {
        get: function (req, res, next) {
            var date = { currentTime: Date.now() };
            res.status(200).type('application/json').send(date);
        });
    };
    // All methods must be authenticated.
    api.access = 'authenticated';

    module.exports = api;

A konkrét műveletek is hitelesítési adhatja meg:

    var api = {
        get: function (req, res, next) {
            var date = { currentTime: Date.now() };
            res.status(200).type('application/json').send(date);
        }
    };
    // hello GET methods must be authenticated.
    api.get.access = 'authenticated';

    module.exports = api;

hello ugyanezt a tokent hello táblák végpont használt kell használni a hitelesítést igénylő, egyéni API-k.

### <a name="howto-customapi-auth"></a>Hogyan: nagy fájlfeltöltéseket kezelése
Az Azure Mobile Apps SDK-t használ a hello [törzs-elemző köztes](https://github.com/expressjs/body-parser) tooaccept és dekódolási törzs tartalma a jelentkezést.  Törzs-elemző tooaccept nagyobb fájlfeltöltéseket előre konfigurálhatja:

    var express = require('express'),
        bodyParser = require('body-parser'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Set up large body content handling
    app.use(bodyParser.json({ limit: '50mb' }));
    app.use(bodyParser.urlencoded({ limit: '50mb', extended: true }));

    // Import hello Custom API
    mobile.api.import('./api');

    // Add hello mobile API so it is accessible as a Web API
    app.use(mobile);

    // Start listening on HTTP
    app.listen(process.env.PORT || 3000);

hello fájl base-64 kódolású továbbítás előtt.  Ez növeli a hello tényleges feltöltés hello méretét (és figyelembe kell venni mérete ezért hello).

### <a name="howto-customapi-sql"></a>Útmutató: egyéni SQL-utasítások végrehajtása
hello Azure Mobile Apps SDK lehetővé teszi, hogy a hozzáférés toohello teljes környezetben keresztül hello request objektumon, így tooexecute paraméteres SQL utasítás toohello meghatározott adatszolgáltató könnyen:

    var api = {
        get: function (request, response, next) {
            // Check for parameters - if not there, pass on tooa later API call
            if (typeof request.params.completed === 'undefined')
                return next();

            // Define hello query - anything that can be handled by hello mssql
            // driver is allowed.
            var query = {
                sql: 'UPDATE TodoItem SET complete=@completed',
                parameters: [{
                    completed: request.params.completed
                }]
            };

            // Execute hello query.  hello context for Azure Mobile Apps is available through
            // request.azureMobile - hello data object contains hello configured data provider.
            request.azureMobile.data.execute(query)
            .then(function (results) {
                response.json(results);
            });
        }
    };

    api.get.access = 'authenticated';
    module.exports = api;

## <a name="Debugging"></a>Hibakeresés, könnyen táblák és egyszerű API-k
### <a name="howto-diagnostic-logs"></a>Hogyan: hibakeresési, diagnosztizálása és elhárítása az Azure Mobile apps
hello Azure App Service számos Hibakeresés és hibaelhárítási módszerekről a Node.js-alkalmazások biztosít.
Tekintse meg a következő lépések során a Node.js mobil-háttéralkalmazás cikkek tooget toohello:

* [Az Azure App Service figyelése]
* [Az Azure App Service diagnosztikai naplózás engedélyezése]
* [A Visual Studio egy Azure App Service hibaelhárítása]

NODE.js-alkalmazások hozzáférhetnek tooa diagnosztikai naplófájl eszközök széles skáláját.  Belsőleg, az Azure Mobile Apps Node.js SDK-t használ hello [Winston] diagnosztikai naplózás.  Automatikusan naplózását hibakeresési mód engedélyezése vagy hello beállítása **MS_DebugMode** hello az alkalmazás-beállítás tootrue [Azure-portálon]. Létrehozott naplók jelennek meg a hello diagnosztikai naplók hello [Azure-portálon].

### <a name="in-portal-editing"></a><a name="work-easy-tables"></a>Útmutató: egyszerű táblák munkához hello Azure-portálon
Hello portálon könnyen táblák lehetővé teszik, hogy létrehozása és használata táblákat közvetlenül hello portálon. Tábla műveletekbe hello App Service-szerkesztő akár szerkesztheti is.

Amikor rákattint **könnyen táblák** a háttér-webhely beállításai hozzáadása, módosítása és törlése egy tábla. Hello tábla is megtekinthető.

![A könnyen táblázatok használata](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-easy-tables.png)

hello következő parancsok is elérhetők hello parancssávon táblázat:

* **Engedélyek módosítása** - olvasási engedély hello módosítás, Beszúrás, frissítése és törlési műveletek hello táblán.
  Beállítások a következők tooallow névtelen hozzáférés, toorequire hitelesítési vagy toodisable minden hozzáférési toohello műveletet.
* **Parancsfájl szerkesztése** -hello parancsfájl hello tábla az App Service-szerkesztő hello van megnyitva.
* **Séma kezelése** - hozzáadása vagy oszlopok törlése vagy hello tábla index módosítása.
* **Törölje a jelet tábla** -csonkolja a meglévő tábla kell az összes adat sorok törléséhez, de hello séma hagyja változatlanul.
* **Sorok törlése** -egyedi sornyi adatot törli.
* **A folyamatos átviteli naplók megtekintése** -összeköttetést toohello adatfolyam-naplózási szolgáltatás a helyhez.

### <a name="work-easy-apis"></a>Útmutató: egyszerű API-k munkához hello Azure-portálon
Hello portal egyszerű API-k lehetővé teszik, hogy létrehozása és használata egyéni API-k közvetlenül hello portálon. API-parancsfájlok hello App Service-szerkesztő használatával módosíthatja.

Amikor rákattint **egyszerű API-k** a háttér-webhely beállításai hozzáadása, módosítása és egy egyéni API-végpont törlése.

![Egyszerű API-khoz](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-easy-apis.png)

Hello portálon egy adott HTTP-művelet hello hozzáférési engedélyeinek módosítása, hello API parancsfájlt a App Service-szerkesztőben szerkesztheti vagy hello folyamatos átviteli naplók megtekintése.

### <a name="online-editor"></a>Hogyan: hello App Service-szerkesztő kód szerkesztése
hello Azure-portál lehetővé teszi a Node.js háttérrendszer parancsfájlok hello App Service-szerkesztő szerkesztése hello projekt tooyour helyi számítógép letöltése nélkül. a parancsfájlok tooedit hello online szerkesztőben:

1. A mobilalkalmazás-háttérrendszer paneljén kattintson **összes beállítás** > vagy **könnyen táblák** vagy **egyszerű API-k**, egy táblázatot vagy API-t, majd **parancsfájl szerkesztése**. hello parancsfájlt az App Service-szerkesztő hello van megnyitva.

    ![App Service-szerkesztő](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-visual-studio-editor.png)
2. Végezze el a módosításokat toohello kódfájl hello online szerkesztő. Változások automatikusan mentése közben.

<!-- Images -->
[0]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/npm-init.png
[1]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/vs2015-new-project.png
[2]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/vs2015-install-npm.png
[3]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-config.png
[4]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-authconfig.png
[5]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-newuser-1.png
[6]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/dotnet-backend-create-db.png

<!-- URLs -->
[Android ügyfél gyors üzembe helyezés]: app-service-mobile-android-get-started.md
[Apache Cordova-ügyfél gyors üzembe helyezés]: app-service-mobile-cordova-get-started.md
[iOS ügyfél gyors üzembe helyezés]: app-service-mobile-ios-get-started.md
[Xamarin.iOS ügyfél gyors üzembe helyezés]: app-service-mobile-xamarin-ios-get-started.md
[Xamarin.Android ügyfél gyors üzembe helyezés]: app-service-mobile-xamarin-android-get-started.md
[Xamarin.Forms-ügyfél gyors üzembe helyezés]: app-service-mobile-xamarin-forms-get-started.md
[A Windows Store ügyfél gyors üzembe helyezés]: app-service-mobile-windows-store-dotnet-get-started.md
[HTML/Javascript Client QuickStart]: app-service-html-get-started.md
[offline adatszinkronizálás]: app-service-mobile-offline-data-sync.md
[Hogyan tooconfigure Azure Active Directory-hitelesítés]: app-service-mobile-how-to-configure-active-directory-authentication.md
[Hogyan tooconfigure Facebook-hitelesítés]: app-service-mobile-how-to-configure-facebook-authentication.md
[Hogyan tooconfigure Google-hitelesítés]: app-service-mobile-how-to-configure-google-authentication.md
[Hogyan tooconfigure Microsoft Authentication]: app-service-mobile-how-to-configure-microsoft-authentication.md
[Hogyan tooconfigure Twitter hitelesítés]: app-service-mobile-how-to-configure-twitter-authentication.md
[Azure App Service telepítési útmutató]: ../app-service-web/web-sites-deploy.md
[Az Azure App Service figyelése]: ../app-service-web/web-sites-monitor.md
[Az Azure App Service diagnosztikai naplózás engedélyezése]: ../app-service-web/web-sites-enable-diagnostic-log.md
[A Visual Studio egy Azure App Service hibaelhárítása]: ../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md
[hello csomópont verzió megadása]: ../nodejs-specify-node-version-azure-apps.md
[csomópont modulok használata]: ../nodejs-use-node-modules-azure-apps.md
[Create a new Azure App Service]: ../app-service-web/
[azure-mobile-apps]: https://www.npmjs.com/package/azure-mobile-apps
[Express]: http://expressjs.com/
[Swagger]: http://swagger.io/

[Azure-portálon]: https://portal.azure.com/
[OData]: http://www.odata.org
[ígéret]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise
[basicapp mintát a Githubon]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/basic-app
[todo mintát a Githubon]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/todo
[minták directory a Githubon]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples
[static-schema sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/static-schema
[QueryJS]: https://github.com/Azure/queryjs
[Node.js eszközök 1.1 a Visual Studio]: https://github.com/Microsoft/nodejstools/releases/tag/v1.1-RC.2.1
[mssql Node.js csomag]: https://www.npmjs.com/package/mssql
[Microsoft SQL Server 2014 Express]: http://www.microsoft.com/en-us/server-cloud/Products/sql-server-editions/sql-server-express.aspx
[ExpressJS köztes]: http://expressjs.com/guide/using-middleware.html
[Winston]: https://github.com/winstonjs/winston
