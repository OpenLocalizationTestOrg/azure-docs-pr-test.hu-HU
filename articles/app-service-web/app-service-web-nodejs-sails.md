---
title: "egy Sails.js webalkalmazás az App Service alkalmazás tooAzure aaaDeploy |} Microsoft Docs"
description: "Megtudhatja, hogyan toodeploy egy Node.js-alkalmazás Azure App Service szolgáltatásban. Az oktatóanyag bemutatja, hogyan toodeploy egy Sails.js webalkalmazás."
services: app-service\web
documentationcenter: nodejs
author: cephalin
manager: erikre
editor: 
ms.assetid: 8877ddc8-1476-45ae-9e7f-3c75917b4564
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 12/16/2016
ms.author: cephalin
ms.openlocfilehash: f5b2518b9c87c040845f7268763862be8c15e83e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-sailsjs-web-app-tooazure-app-service"></a>Sails.js webalkalmazás app tooAzure App Service telepítése
Az oktatóanyag bemutatja, hogyan toodeploy egy Sails.js app tooAzure App Service. Hello folyamatban, néhány általános információkat, hogyan lehet glean tooconfigure a Node.js-alkalmazás toorun az App Service-ben.

Itt megtudhatja, hasznos képességek, például:

* Egy App Service-ben futtassa Sails.js alkalmazás konfigurálása.
* Telepítsen egy alkalmazást tooApp szolgáltatás hello parancssorból.
* Olvassa el a stderr-en és a stdout naplók tootroubleshoot telepítési problémáit.
* Környezeti változók verziókezelő kívül tárolhatja.
* Hozzáférés az Azure környezeti változókat az alkalmazásból.
* Csatlakozás tooa adatbázis (MongoDB).

Sails.js ismeretét kell rendelkeznie. Ebben az oktatóanyagban nincs meg problémákkal kapcsolatos toorunning Sail.js általában tervezett toohelp.

## <a name="cli-versions-toocomplete-hello-task"></a>Parancssori felület verziók toocomplete hello feladat

Hello feladat a következő parancssori felület verziók hello egyikével hajthatja végre:

- [Az Azure CLI 1.0](app-service-web-nodejs-sails-cli-nodejs.md) – hello klasszikus és resource management üzembe helyezési modellek számára a parancssori felület
- [Az Azure CLI 2.0](app-service-web-nodejs-sails.md) -a következő generációs CLI hello erőforrás felügyeleti telepítési modell

## <a name="prerequisites"></a>Előfeltételek
* [Node.js](https://nodejs.org/)
* [Sails.js](http://sailsjs.org/get-started)
* [Git](http://www.git-scm.com/downloads)
* [Azure CLI 2.0](/cli/azure/install-az-cli2)
* Egy Microsoft Azure-fiók. Ha nincs fiókja, [regisztráljon egy ingyenes próbaverzióra](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F), vagy [aktiválhatja a Visual Studio előfizetői előnyeit](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

> [!NOTE]
> Az [App Service kipróbálása](https://azure.microsoft.com/try/app-service/) Azure-fiók nélkül is lehetséges. Hozzon létre egy alapszintű alkalmazást, és nincs szükség bankkártyára, nem jár kötelezettségekkel tooan óra--be azt a lejátszásához.
> 
> 

## <a name="step-1-create-and-configure-a-sailsjs-app-locally"></a>1. lépés: Hozzon létre és konfigurálhat egy Sails.js alkalmazást helyileg
Először gyorsan létrehozhat egy alapértelmezett Sails.js alkalmazást a fejlesztési környezetben az alábbiak szerint:

1. Nyissa meg hello parancssori terminált az Ön által választott és `CD` tooa munkakönyvtárát.
2. Hozzon létre egy Sails.js alkalmazást, és futtassa:

        sails new <app_name>
        cd <app_name>
        sails lift

    Ellenőrizze, hogy toohello alapértelmezett kezdőlapja a http://localhost:1377 léphet.

1. Következő lépésként engedélyezze az Azure-naplózás. A gyökérkönyvtár, hozzon létre egy nevű fájlt `iisnode.yml` , és adja hozzá az alábbi két hello:

        loggingEnabled: true
        logDirectory: iisnode

    Naplózás engedélyezve van a hello [iisnode](https://github.com/tjanczuk/iisnode) , hogy az Azure App Service használja toorun Node.js alkalmazások kiszolgálói. 
    Ennek működéséről további információkért lásd: [hogyan toodebug egy Node.js webalkalmazás az Azure App Service](web-sites-nodejs-debug.md).

2. Ezután konfigurálja a hello Sails.js app toouse Azure környezeti változókat. Nyissa meg a config/env/production.js tooconfigure az éles környezetben, és állítsa be `port` és `hookTimeout`:

        module.exports = {

            // Use process.env.port toohandle web requests toohello default HTTP port
            port: process.env.port,
            // Increase hooks timout too30 seconds
            // This avoids hello Sails.js error documented at https://github.com/balderdashy/sails/issues/2691
            hookTimeout: 30000,

            ...
        };

    Ezeket a konfigurációs beállításokat a dokumentációjában találja a [Sails.js dokumentáció](http://sailsjs.org/documentation/reference/configuration/sails-config).

4. Ezt követően kódba foglalni hello Node.js verziót, amelybe toouse. A Package.JSON kódjában, adja hozzá a hello következő `engines` tulajdonság tooset hello Node.js verzió tooone, amely azt szeretnénk, ha.

        "engines": {
            "node": "6.9.1"
        },

5. Végezetül a Git-tárház inicializálása, és véglegesítse a fájlokat. Hello alkalmazás gyökérkönyvtárában (ahol a package.json azt), a következő Git-parancsok futtatása hello:

        git init
        git add .
        git commit -m "<your commit message>"

A kód készen toobe telepítve. 

## <a name="step-2-create-an-azure-app-and-deploy-sailsjs"></a>2. lépés: Az Azure-alkalmazás létrehozása és központi telepítése Sails.js

A következő hello App Service-erőforrás létrehozása az Azure-ban, és a Sails.js app tooit telepítése.

1. bejelentkezés tooAzure, például így:

        az login

    Hajtsa végre a hello Rákérdezés toocontinue hello bejelentkezési egy böngészőben az Azure-előfizetéssel rendelkező Microsoft-fiókkal.

3. App Service hello központi felhasználói be. A kód üzembe helyezését később fogja elvégezni ezen hitelesítő adatok használatával.
   
        az appservice web deployment user set --user-name <username> --password <password>

3. Hozzon létre egy [erőforráscsoport](../azure-resource-manager/resource-group-overview.md) néven. A jelen Node.js oktatóanyag esetében nem valóban szükséges tooknow mi.

        az group create --location "<location>" --name my-sailsjs-app-group

    toosee milyen lehetséges értékei, akkor is használhat `<location>`, használja a hello `az appservice list-locations` CLI parancsot.

3. Hozzon létre egy "ingyenes" [App Service-csomag](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) néven. A jelen Node.js oktatóanyag esetében csak tudja, hogy azt nem kell felszámítani a web Apps a terv.

        az appservice plan create --name my-sailsjs-appservice-plan --resource-group my-sailsjs-app-group --sku FREE

4. Hozzon létre egy új, egyéni névvel rendelkező webappot az `<app_name>` paraméterben.

        az appservice web create --name <app_name> --resource-group my-sailsjs-app-group --plan my-sailsjs-appservice-plan

## <a name="step-3-configure-and-deploy-your-sailsjs-app"></a>3. lépés: Konfigurálja, és az Sails.js alkalmazás üzembe helyezése

1. Az új webes alkalmazás a helyi Git-telepítés a következő parancs hello konfigurálása:

        az appservice web source-control config-local-git --name <app_name> --resource-group my-sailsjs-app-group

    A JSON-kimenetét ilyen, ami azt jelenti, hogy hello távoli Git-tárház beállítása jelenik meg:

        {
        "url": "https://<deployment_user>@<app_name>.scm.azurewebsites.net/<app_name>.git"
        }

6. Hello URL-cím hozzáadása a hello JSON a helyi tárház távoli Git mappaként (nevű `azure` az egyszerűség érdekében).

        git remote add azure https://<deployment_user>@<app_name>.scm.azurewebsites.net/<app_name>.git
   
7. A minta kód toohello telepítése `azure` Git távoli. Amikor a rendszer kéri, használja a korábban konfigurált hello üzembe helyezési hitelesítő adatokat.

        git push azure master

7. Végezetül csak indítsa el az élő Azure alkalmazást hello böngészőben:

        az appservice web browse --name <app_name> --resource-group my-sailsjs-app-group

    Most látnia kell hello azonos Sails.js kezdőlapján.

    ![](./media/app-service-web-nodejs-sails/sails-in-azure.png)

## <a name="troubleshoot-your-deployment"></a>Hibaelhárítás
A Sails.js alkalmazás az App Service valamilyen okból nem sikerül, ha található hello stderr naplók toohelp hibaelhárítás érdekében.
További információkért lásd: [hogyan toodebug egy Node.js webalkalmazás az Azure App Service](web-sites-nodejs-debug.md).
Ha hello app sikeresen elindult, hello stdout napló meg kell jelennie, ismerős üdvözlőüzenetére:

                   .-..-.
    
       Sails              <|    .-..-.
       v0.12.11            |\
                          /|.\
                         / || \
                       ,'  |'  \
                    .-'.-==|/_--'
                    `--'-------' 
       __---___--___---___--___---___--___
     ____---___--___---___--___---___--___-__

    Server lifted in `D:\home\site\wwwroot`
    toosee your app, visit http://localhost:\\.\pipe\c775303c-0ebc-4854-8ddd-2e280aabccac
    tooshut down Sails, press <CTRL> + C at any time.

Megadhatja a lépésköz legyen hello stdout-naplók hello [config/log.js](http://sailsjs.org/#!/documentation/concepts/Logging) fájlt.

## <a name="connect-tooa-database-in-azure"></a>Csatlakozás az Azure-ban tooa adatbázis
tooconnect tooa adatbázis az Azure az Ön által választott hello-adatbázis létrehozása az Azure, például az Azure SQL Database, a MySQL, a MongoDB, a (Redis) Azure Cache, stb., és használja a megfelelő hello [datastore adapter](https://github.com/balderdashy/sails#compatibility) tooconnect tooit. hello lépésekből ebben a szakaszban megtudhatja, hogyan tooconnect tooMongoDB használatával egy [Azure Cosmos DB](../documentdb/documentdb-protocol-mongodb.md) adatbázis, amely támogatja a MongoDB-ügyfélkapcsolatokat.

1. [Hozzon létre egy Cosmos-DB-fiók MongoDB-protokolltámogatással rendelkező](../documentdb/documentdb-create-mongodb-account.md).
2. [Hozzon létre egy Cosmos DB gyűjtemény és az adatbázis](../documentdb/documentdb-create-collection.md). hello gyűjtemény hello nevét nem számít, de ha Sails.js kell hello hello adatbázis nevét.
3. [A Cosmos-adatbázis adatbázis-kapcsolódási információt hello található](../cosmos-db/connect-mongodb-account.md#GetCustomConnection).
2. A parancssori terminálról hello MongoDB-adapter telepítése:

        npm install sails-mongo --save

3. Nyissa meg a config/connections.js, és adja hozzá a következő objektum toohello kapcsolatlista hello:

        docDbMongo: {
            adapter: 'sails-mongo',
            user: process.env.dbuser,
            password: process.env.dbpassword,
            host: process.env.dbhost,
            port: process.env.dbport,
            database: process.env.dbname,
            ssl: true
        },

    > [!NOTE] 
    > Hello `ssl: true` beállítás fontos, mert [Cosmos DB írja elő](../cosmos-db/connect-mongodb-account.md#connection-string-requirements). 
    >
    >

4. Környezeti változók (`process.env.*`), tooset van szüksége az App Service-ben. toodo a, futtatási hello parancsok követően a terminálon. A Cosmos DB hello kapcsolati adatokat használjon.

        az appservice web config appsettings update --settings dbuser="<database user>" --name <app_name> --resource-group my-sailsjs-app-group
        az appservice web config appsettings update --settings dbpassword="<database password>" --name <app_name> --resource-group my-sailsjs-app-group
        az appservice web config appsettings update --settings dbhost="<database hostname>" --name <app_name> --resource-group my-sailsjs-app-group
        az appservice web config appsettings update --settings dbport="<database port>" --name <app_name> --resource-group my-sailsjs-app-group
        az appservice web config appsettings update --settings dbname="<database name>" --name <app_name> --resource-group my-sailsjs-app-group

    A beállítások üzembe az Azure alkalmazás beállításaiban tartja a bizalmas adatokat a verziókövetési rendszerrel (Git). Ezután konfigurálja a fejlesztési környezet toouse hello kapcsolat ugyanazokat az információkat.
5. Nyissa meg a config/local.js, és adja hozzá a következő kapcsolatok objektum hello:

        connections: {
            docDbMongo: {
                user: "<database user>",
                password: "<database password>",
                host: "<database hostname>",
                database: "<database name>",
                ssl: true
            },
        },

    Ez a konfiguráció felülbírálja a config/connections.js fájl hello helyi környezet hello beállításai. Így nem kerül a Git hello alapértelmezett .gitignore a projekt ki van zárva a be ezt a fájlt. Most képes tooconnect tooyour Cosmos DB (MongoDB) adatbázis, mind az Azure web app és a helyi fejlesztési környezetet.
6. Nyissa meg a config/env/production.js tooconfigure az éles környezetben, és adja hozzá a következő hello `models` objektum:

        models: {
            connection: 'docDbMongo',
            migrate: 'safe'
        },
7. Nyissa meg a fejlesztési környezetet az config/env/development.js tooconfigure, és adja hozzá a következő hello `models` objektum:

        models: {
            connection: 'docDbMongo',
            migrate: 'alter'
        },

    `migrate: 'alter'`lehetővé teszi használja adatbázis áttelepítési funkciók toocreate és adatbázis-gyűjteményeket, illetve olyan táblázatok könnyen. Azonban `migrate: 'safe'` használatos az Azure (éles) környezetben, mert Sails.js nem teszi lehetővé a toouse `migrate: 'alter'` éles környezetben (lásd: [Sails.js dokumentáció](http://sailsjs.org/documentation/concepts/models-and-orm/model-settings)).
8. A Terminálszolgáltatások, hello [készítése](http://sailsjs.org/documentation/reference/command-line-interface/sails-generate) egy Sails.js [tervezetének API](http://sailsjs.org/documentation/concepts/blueprints) például általában, majd futtassa `sails lift` Sails.js adatbázis áttelepítési hello adatbázis létrehozásához. Példa:

         sails generate api mywidget
         sails lift

    Hello `mywidget` Ez a parancs által létrehozott modell üres, de használhatjuk tooshow, hogy rendelkezik-e az adatbázis-kapcsolat.
    Amikor futtatja `sails lift`, hello hiányzik gyűjteményt hoz létre, és hello táblák modellek az alkalmazás használja.
9. Hozzáférés a most létrehozott hello böngészőben hello tervezetének API. Példa:

        http://localhost:1337/mywidget/create

    hello API létrehozott hello bejegyzés hátsó tooyou hello böngészőablakban, ami azt jelenti, hogy a gyűjtemény sikeresen létrejött-e vissza.

        {"id":1,"createdAt":"2016-09-23T13:32:00.000Z","updatedAt":"2016-09-23T13:32:00.000Z"}
10. Most a módosítások tooAzure leküldéses, és keresse meg a tooyour app toomake meg arról, hogy továbbra is működik.

         git add .
         git commit -m "<your commit message>"
         git push azure master
         az appservice web browse --name <app_name> --resource-group my-sailsjs-app-group

11. Hozzáférés hello tervezetének API Azure webalkalmazás. Példa:

         http://<appname>.azurewebsites.net/mywidget/create

     Ha hello API adja vissza egy másik új bejegyzést, majd az Azure-webalkalmazásban van van szó tooyour Cosmos DB (MongoDB) adatbázis.

## <a name="more-resources"></a>További erőforrások
* [Ismerkedés a Node.js webalkalmazásokkal az Azure App Service-ben](app-service-web-get-started-nodejs.md)
* [A Node.js modulok használata az Azure alkalmazásokkal](../nodejs-use-node-modules-azure-apps.md)
