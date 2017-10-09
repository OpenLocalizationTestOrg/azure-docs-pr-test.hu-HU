---
title: "a Node.js hello Azure CDN SDK használatába aaaGet |} Microsoft Docs"
description: "Megtudhatja, hogyan toowrite Node.js alkalmazások toomanage Azure CDN szolgáltatás használata."
services: cdn
documentationcenter: nodejs
author: zhangmanling
manager: erikre
editor: 
ms.assetid: c4bb6a61-de3d-4f0c-9dca-202554c43dfa
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 6c805e5fb8e0b471e8b248cb2f4b29efd6c85940
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-cdn-development"></a>Ismerkedés az Azure CDN-fejlesztéssel
> [!div class="op_single_selector"]
> * [Node.js](cdn-app-dev-node.md)
> * [.NET](cdn-app-dev-net.md)
> 
> 

Használhatja a hello [Azure CDN SDK for Node.js](https://www.npmjs.com/package/azure-arm-cdn) tooautomate létrehozását és a CDN-profil és a végpontok.  Ez az oktatóanyag végigvezeti egy egyszerű Node.js-Konzolalkalmazás, azt mutatja be, hogy több hello elérhető műveletek hello létrehozását.  Ez az oktatóanyag van nem tervezett toodescribe minden szempontját hello Azure CDN SDK for Node.js részletesen.

toocomplete ebben az oktatóanyagban érdemes már [Node.js](http://www.nodejs.org) **4.x.x** vagy újabb rendszerre telepített és konfigurált.  Használhatja a Node.js-alkalmazás toocreate kívánt szövegszerkesztőben.  toowrite ebben az oktatóanyagban használt [Visual Studio Code](https://code.visualstudio.com).  

> [!TIP]
> Hello [az oktatóanyagot befejezett projekt](https://code.msdn.microsoft.com/Azure-CDN-SDK-for-Nodejs-c712bc74) letölthető az MSDN Webhelyén.
> 
> 

[!INCLUDE [cdn-app-dev-prep](../../includes/cdn-app-dev-prep.md)]

## <a name="create-your-project-and-add-npm-dependencies"></a>A projekt létrehozása és hozzáadása NPM függőségek
Most, hogy előre létrehozott erőforráscsoport a CDN-profilok és az Azure AD alkalmazás engedély toomanage CDN-profil és a csoporton belüli végpontok, nem lehet elindítani, az alkalmazás létrehozása.

Az alkalmazás egy mappában toostore létrehozása.  Az aktuális elérési úthoz hello Node.js eszközökkel konzolról állítsa be az aktuális hely toothis új mappát, és a projekt inicializálása a következő futtatásával:

    npm init

Ezután jelenik meg a kérdéseket tooinitialize sorozata a projekthez.  A **belépési pont**, ez az oktatóanyag használja *app.js*.  A következő példa hello a más lehetőségei tekintheti meg.

![NPM init kimeneti](./media/cdn-app-dev-node/cdn-npm-init.png)

A projekt már inicializálva van egy *packages.json* fájlt.  A projekt NPM csomagok szereplő Azure könyvtárak toouse lesz.  Hello Azure ügyfél futásidejű a Node.js (ms-rest-azure) és hello Azure CDN ügyféloldali kódtára a Node.js (azure-arm-cd) fogjuk használni.  Most adja hozzá ezek toohello projekt függőségek.

    npm install --save ms-rest-azure
    npm install --save azure-arm-cdn

Hello csomagok befejezése után telepíti, hello *package.json* fájl alábbihoz hasonló toothis példa (számok eltérő verzió):

``` json
{
  "name": "cdn_node",
  "version": "1.0.0",
  "description": "Azure CDN Node.js tutorial project",
  "main": "app.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "Cam Soper",
  "license": "MIT",
  "dependencies": {
    "azure-arm-cdn": "^0.2.1",
    "ms-rest-azure": "^1.14.4"
  }
}
```

Végezetül a szövegszerkesztővel, hozzon létre egy üres szöveges fájlt, és mentse a projekt mappában hello gyökérmappájában *app.js*.  A rendszer most már készen áll a kód írása toobegin.

## <a name="requires-constants-authentication-and-structure"></a>Szükséges, állandók, hitelesítési és szerkezete
A *app.js* nyissa meg a szerkesztőben, folytassuk hello alapszintű struktúrát a program írása.

1. Adja hozzá a hello "szükséges" az NPM-csomagok hello felső hello alábbira:
   
    ``` javascript
    var msRestAzure = require('ms-rest-azure');
    var cdnManagementClient = require('azure-arm-cdn');
    ```
2. Toodefine kell néhány állandók a módszerek fogja használni.  Adja hozzá a hello következő.  Lehet, hogy tooreplace hello helyőrzők, beleértve a hello  **&lt;csúcsos zárójelek&gt;**, igény szerint a saját értékekkel.
   
    ``` javascript
    //Tenant app constants
    const clientId = "<YOUR CLIENT ID>";
    const clientSecret = "<YOUR CLIENT AUTHENTICATION KEY>"; //Only for service principals
    const tenantId = "<YOUR TENANT ID>";
   
    //Application constants
    const subscriptionId = "<YOUR SUBSCRIPTION ID>";
    const resourceGroupName = "CdnConsoleTutorial";
    const resourceLocation = "<YOUR PREFERRED AZURE LOCATION, SUCH AS Central US>";
    ```
3. Lesz a következő azt példányosítható hello CDN felügyeleti ügyfél, és adjon neki a hitelesítő adatokat.
   
    ``` javascript
    var credentials = new msRestAzure.ApplicationTokenCredentials(clientId, tenantId, clientSecret);
    var cdnClient = new cdnManagementClient(credentials, subscriptionId);
    ```
   
    Ha egyéni felhasználói hitelesítést használ, két sort némileg eltérő fog kinézni.
   
   > [!IMPORTANT]
   > Ha használja a kódmintában toohave egyes felhasználói hitelesítés helyett egy egyszerű szolgáltatás kiválasztása.  Gondos tooguard kell az egyéni felhasználói hitelesítő adatait, és tartsa titokban.
   > 
   > 
   
    ``` javascript
    var credentials = new msRestAzure.UserTokenCredentials(clientId, 
        tenantId, '<username>', '<password>', '<redirect URI>');
    var cdnClient = new cdnManagementClient(credentials, subscriptionId);
    ```
   
    Lehet, hogy tooreplace hello elemeinek  **&lt;csúcsos zárójelek&gt;**  hello megfelelő információkat.  A `<redirect URI>`, hello átirányítási URI-t az Azure AD hello alkalmazás regisztrálásakor megadott használja.
4. A Node.js-Konzolalkalmazás érintetlen tootake egyes parancssori paraméterek.  Most ellenőrzi, hogy legalább egy paramétert.
   
   ```javascript
   //Collect command-line parameters
   var parms = process.argv.slice(2);
   
   //Do we have parameters?
   if(parms == null || parms.length == 0)
   {
       console.log("Not enough parameters!");
       console.log("Valid commands are list, delete, create, and purge.");
       process.exit(1);
   }
   ```
5. Amely számos lehetőséget kínál, a program, ahol azt fiókirodai, ki milyen paraméterek lettek átadva alapján tooother funkciók toohello fő részét.
   
    ```javascript
    switch(parms[0].toLowerCase())
    {
        case "list":
            cdnList();
            break;
   
        case "create":
            cdnCreate();
            break;
   
        case "delete":
            cdnDelete();
            break;
   
        case "purge":
            cdnPurge();
            break;
   
        default:
            console.log("Valid commands are list, delete, create, and purge.");
            process.exit(1);
    }
    ```
6. A program több helyen kell, hogy hello megfelelő számú paraméter lett átadva, és néhány Súgó megjelenítése, nem fogják látni megfelelő toomake.  Hozzon létre funkciók toodo, amely.
   
   ```javascript
   function requireParms(parmCount) {
       if(parms.length < parmCount) {
           usageHelp(parms[0].toLowerCase());
           process.exit(1);
       }
   }
   
   function usageHelp(cmd) {
       console.log("Usage for " + cmd + ":");
       switch(cmd)
       {
           case "list":
               console.log("list profiles");
               console.log("list endpoints <profile name>");
               break;
   
           case "create":
               console.log("create profile <profile name>");
               console.log("create endpoint <profile name> <endpoint name> <origin hostname>");
               break;
   
           case "delete":
               console.log("delete profile <profile name>");
               console.log("delete endpoint <profile name> <endpoint name>");
               break;
   
           case "purge":
               console.log("purge <profile name> <endpoint name> <path>");
               break;
   
           default:
               console.log("Invalid command.");
       }
   }
   ```
7. Végezetül használni fogjuk hello CDN felügyeleti ügyfél hello funkciók aszinkron jellegűek, egy módszer toocall vissza, ha kész van szükségük.  Most Meggyőződünk tartalmazó hello kimeneti hello CDN felügyeleti ügyfél (ha van ilyen), és a hello program kilépésre.
   
    ```javascript
    function callback(err, result, request, response) {
        if (err) {
            console.log(err);
            process.exit(1);
        } else {
            console.log((result == null) ? "Done!" : result);
            process.exit(0);
        }
    }
    ```

Most, hogy a program alapvető szerkezete hello írása, a Microsoft hello függvény hívása a paraméterek alapján kell létrehoznia.

## <a name="list-cdn-profiles-and-endpoints"></a>Lista CDN-profil és -végpontok
Kezdjük kód toolist a meglévő profilok és a végpontok.  A kód megjegyzéseket, hogy tudjuk, ahol az egyes paramétereket kerül adja meg a várt hello szintaxist.

```javascript
// list profiles
// list endpoints <profile name>
function cdnList(){
    requireParms(2);
    switch(parms[1].toLowerCase())
    {
        case "profiles":
            console.log("Listing profiles...");
            cdnClient.profiles.listByResourceGroup(resourceGroupName, callback);
            break;

        case "endpoints":
            requireParms(3);
            console.log("Listing endpoints...");
            cdnClient.endpoints.listByProfile(parms[2], resourceGroupName, callback);
            break;

        default:
            console.log("Invalid parameter.");
            process.exit(1);
    }
}
```

## <a name="create-cdn-profiles-and-endpoints"></a>CDN-profil és a végpontok létrehozása
A következő azt fog írni hello funkciók toocreate profilok és a végpontok.

```javascript
function cdnCreate() {
    requireParms(2);
    switch(parms[1].toLowerCase())
    {
        case "profile":
            cdnCreateProfile();
            break;

        case "endpoint":
            cdnCreateEndpoint();
            break;

        default:
            console.log("Invalid parameter.");
            process.exit(1);
    }
}

// create profile <profile name>
function cdnCreateProfile() {
    requireParms(3);
    console.log("Creating profile...");
    var standardCreateParameters = {
        location: resourceLocation,
        sku: {
            name: 'Standard_Verizon'
        }
    };

    cdnClient.profiles.create(parms[2], standardCreateParameters, resourceGroupName, callback);
}

// create endpoint <profile name> <endpoint name> <origin hostname>        
function cdnCreateEndpoint() {
    requireParms(5);
    console.log("Creating endpoint...");
    var endpointProperties = {
        location: resourceLocation,
        origins: [{
            name: parms[4],
            hostName: parms[4]
        }]
    };

    cdnClient.endpoints.create(parms[3], endpointProperties, parms[2], resourceGroupName, callback);
}
```

## <a name="purge-an-endpoint"></a>A végpont törlése
Ha hello végpont létrejött, egy közös feladat, hogy a program célszerű lehet tooperform van végleges törlése a végpont tartalma.

```javascript
// purge <profile name> <endpoint name> <path>
function cdnPurge() {
    requireParms(4);
    console.log("Purging endpoint...");
    var purgeContentPaths = [ parms[3] ];
    cdnClient.endpoints.purgeContent(parms[2], parms[1], resourceGroupName, purgeContentPaths, callback);
}
```

## <a name="delete-cdn-profiles-and-endpoints"></a>CDN-profil és a végpontok törlése
hello utolsó függvény is végpontok és a profilok törlése.

```javascript
function cdnDelete() {
    requireParms(2);
    switch(parms[1].toLowerCase())
    {
        // delete profile <profile name>
        case "profile":
            requireParms(3);
            console.log("Deleting profile...");
            cdnClient.profiles.deleteIfExists(parms[2], resourceGroupName, callback);
            break;

        // delete endpoint <profile name> <endpoint name>
        case "endpoint":
            requireParms(4);
            console.log("Deleting endpoint...");
            cdnClient.endpoints.deleteIfExists(parms[3], parms[2], resourceGroupName, callback);
            break;

        default:
            console.log("Invalid parameter.");
            process.exit(1);
    }
}
```

## <a name="running-hello-program"></a>Hello programot futtat
Most már a Node.js-program használatával a kedvenc hibakereső végezhetünk vagy hello konzolján.

> [!TIP]
> Visual Studio Code az hibakereső használata, tooset lesz szüksége a környezet toopass hello parancssori paraméterek mentése.  A Visual Studio Code nem ez hello **lanuch.json** fájlt.  Keresse meg nevű tulajdonság **argumentum** és karakterlánc-értékeket a paraméterek tömbjét adja hozzá, úgy, hogy hasonló toothis: `"args": ["list", "profiles"]`.
> 
> 

Először a profilok listázása.

![Lista profilok](./media/cdn-app-dev-node/cdn-list-profiles.png)

Azt a kapott vissza üres tömb.  Jelenleg nincs a profil az erőforráscsoportban, mivel a várt érték, amely.  Most hozzon létre most egy profilt.

![Profil létrehozása](./media/cdn-app-dev-node/cdn-create-profile.png)

Most adjuk hozzá egy végpontot.

![-Végpont létrehozása](./media/cdn-app-dev-node/cdn-create-endpoint.png)

Végezetül most törli a profilt.

![Profil törlése](./media/cdn-app-dev-node/cdn-delete-profile.png)

## <a name="next-steps"></a>Következő lépések
toosee befejeződött hello projektet ebben a forgatókönyvben a [hello minta letöltése](https://code.msdn.microsoft.com/Azure-CDN-SDK-for-Nodejs-c712bc74).

hello Azure CDN SDK for Node.js, a nézet hello toosee hello referencia [hivatkozás](http://azure.github.io/azure-sdk-for-node/azure-arm-cdn/latest/).

toofind további dokumentációiért hello Azure SDK for Node.js, a nézet hello [hivatkozás teljes](http://azure.github.io/azure-sdk-for-node/).

A CDN erőforrások kezelése [PowerShell](cdn-manage-powershell.md).

