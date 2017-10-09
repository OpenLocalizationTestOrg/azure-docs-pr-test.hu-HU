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
# <a name="get-started-with-azure-cdn-development"></a><span data-ttu-id="e9ef8-103">Ismerkedés az Azure CDN-fejlesztéssel</span><span class="sxs-lookup"><span data-stu-id="e9ef8-103">Get started with Azure CDN development</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="e9ef8-104">Node.js</span><span class="sxs-lookup"><span data-stu-id="e9ef8-104">Node.js</span></span>](cdn-app-dev-node.md)
> * [<span data-ttu-id="e9ef8-105">.NET</span><span class="sxs-lookup"><span data-stu-id="e9ef8-105">.NET</span></span>](cdn-app-dev-net.md)
> 
> 

<span data-ttu-id="e9ef8-106">Használhatja a hello [Azure CDN SDK for Node.js](https://www.npmjs.com/package/azure-arm-cdn) tooautomate létrehozását és a CDN-profil és a végpontok.</span><span class="sxs-lookup"><span data-stu-id="e9ef8-106">You can use hello [Azure CDN SDK for Node.js](https://www.npmjs.com/package/azure-arm-cdn) tooautomate creation and management of CDN profiles and endpoints.</span></span>  <span data-ttu-id="e9ef8-107">Ez az oktatóanyag végigvezeti egy egyszerű Node.js-Konzolalkalmazás, azt mutatja be, hogy több hello elérhető műveletek hello létrehozását.</span><span class="sxs-lookup"><span data-stu-id="e9ef8-107">This tutorial walks through hello creation of a simple Node.js console application that demonstrates several of hello available operations.</span></span>  <span data-ttu-id="e9ef8-108">Ez az oktatóanyag van nem tervezett toodescribe minden szempontját hello Azure CDN SDK for Node.js részletesen.</span><span class="sxs-lookup"><span data-stu-id="e9ef8-108">This tutorial is not intended toodescribe all aspects of hello Azure CDN SDK for Node.js in detail.</span></span>

<span data-ttu-id="e9ef8-109">toocomplete ebben az oktatóanyagban érdemes már [Node.js](http://www.nodejs.org) **4.x.x** vagy újabb rendszerre telepített és konfigurált.</span><span class="sxs-lookup"><span data-stu-id="e9ef8-109">toocomplete this tutorial, you should already have [Node.js](http://www.nodejs.org) **4.x.x** or higher installed and configured.</span></span>  <span data-ttu-id="e9ef8-110">Használhatja a Node.js-alkalmazás toocreate kívánt szövegszerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="e9ef8-110">You can use any text editor you want toocreate your Node.js application.</span></span>  <span data-ttu-id="e9ef8-111">toowrite ebben az oktatóanyagban használt [Visual Studio Code](https://code.visualstudio.com).</span><span class="sxs-lookup"><span data-stu-id="e9ef8-111">toowrite this tutorial, I used [Visual Studio Code](https://code.visualstudio.com).</span></span>  

> [!TIP]
> <span data-ttu-id="e9ef8-112">Hello [az oktatóanyagot befejezett projekt](https://code.msdn.microsoft.com/Azure-CDN-SDK-for-Nodejs-c712bc74) letölthető az MSDN Webhelyén.</span><span class="sxs-lookup"><span data-stu-id="e9ef8-112">hello [completed project from this tutorial](https://code.msdn.microsoft.com/Azure-CDN-SDK-for-Nodejs-c712bc74) is available for download on MSDN.</span></span>
> 
> 

[!INCLUDE [cdn-app-dev-prep](../../includes/cdn-app-dev-prep.md)]

## <a name="create-your-project-and-add-npm-dependencies"></a><span data-ttu-id="e9ef8-113">A projekt létrehozása és hozzáadása NPM függőségek</span><span class="sxs-lookup"><span data-stu-id="e9ef8-113">Create your project and add NPM dependencies</span></span>
<span data-ttu-id="e9ef8-114">Most, hogy előre létrehozott erőforráscsoport a CDN-profilok és az Azure AD alkalmazás engedély toomanage CDN-profil és a csoporton belüli végpontok, nem lehet elindítani, az alkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="e9ef8-114">Now that we've created a resource group for our CDN profiles and given our Azure AD application permission toomanage CDN profiles and endpoints within that group, we can start creating our application.</span></span>

<span data-ttu-id="e9ef8-115">Az alkalmazás egy mappában toostore létrehozása.</span><span class="sxs-lookup"><span data-stu-id="e9ef8-115">Create a folder toostore your application.</span></span>  <span data-ttu-id="e9ef8-116">Az aktuális elérési úthoz hello Node.js eszközökkel konzolról állítsa be az aktuális hely toothis új mappát, és a projekt inicializálása a következő futtatásával:</span><span class="sxs-lookup"><span data-stu-id="e9ef8-116">From a console with hello Node.js tools in your current path, set your current location toothis new folder and initialize your project by executing:</span></span>

    npm init

<span data-ttu-id="e9ef8-117">Ezután jelenik meg a kérdéseket tooinitialize sorozata a projekthez.</span><span class="sxs-lookup"><span data-stu-id="e9ef8-117">You will then be presented a series of questions tooinitialize your project.</span></span>  <span data-ttu-id="e9ef8-118">A **belépési pont**, ez az oktatóanyag használja *app.js*.</span><span class="sxs-lookup"><span data-stu-id="e9ef8-118">For **entry point**, this tutorial uses *app.js*.</span></span>  <span data-ttu-id="e9ef8-119">A következő példa hello a más lehetőségei tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="e9ef8-119">You can see my other choices in hello following example.</span></span>

![NPM init kimeneti](./media/cdn-app-dev-node/cdn-npm-init.png)

<span data-ttu-id="e9ef8-121">A projekt már inicializálva van egy *packages.json* fájlt.</span><span class="sxs-lookup"><span data-stu-id="e9ef8-121">Our project is now initialized with a *packages.json* file.</span></span>  <span data-ttu-id="e9ef8-122">A projekt NPM csomagok szereplő Azure könyvtárak toouse lesz.</span><span class="sxs-lookup"><span data-stu-id="e9ef8-122">Our project is going toouse some Azure libraries contained in NPM packages.</span></span>  <span data-ttu-id="e9ef8-123">Hello Azure ügyfél futásidejű a Node.js (ms-rest-azure) és hello Azure CDN ügyféloldali kódtára a Node.js (azure-arm-cd) fogjuk használni.</span><span class="sxs-lookup"><span data-stu-id="e9ef8-123">We'll use hello Azure Client Runtime for Node.js (ms-rest-azure) and hello Azure CDN Client Library for Node.js (azure-arm-cd).</span></span>  <span data-ttu-id="e9ef8-124">Most adja hozzá ezek toohello projekt függőségek.</span><span class="sxs-lookup"><span data-stu-id="e9ef8-124">Let's add those toohello project as dependencies.</span></span>

    npm install --save ms-rest-azure
    npm install --save azure-arm-cdn

<span data-ttu-id="e9ef8-125">Hello csomagok befejezése után telepíti, hello *package.json* fájl alábbihoz hasonló toothis példa (számok eltérő verzió):</span><span class="sxs-lookup"><span data-stu-id="e9ef8-125">After hello packages are done installing, hello *package.json* file should look similar toothis example (version numbers may differ):</span></span>

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

<span data-ttu-id="e9ef8-126">Végezetül a szövegszerkesztővel, hozzon létre egy üres szöveges fájlt, és mentse a projekt mappában hello gyökérmappájában *app.js*.</span><span class="sxs-lookup"><span data-stu-id="e9ef8-126">Finally, using your text editor, create a blank text file and save it in hello root of our project folder as *app.js*.</span></span>  <span data-ttu-id="e9ef8-127">A rendszer most már készen áll a kód írása toobegin.</span><span class="sxs-lookup"><span data-stu-id="e9ef8-127">We're now ready toobegin writing code.</span></span>

## <a name="requires-constants-authentication-and-structure"></a><span data-ttu-id="e9ef8-128">Szükséges, állandók, hitelesítési és szerkezete</span><span class="sxs-lookup"><span data-stu-id="e9ef8-128">Requires, constants, authentication, and structure</span></span>
<span data-ttu-id="e9ef8-129">A *app.js* nyissa meg a szerkesztőben, folytassuk hello alapszintű struktúrát a program írása.</span><span class="sxs-lookup"><span data-stu-id="e9ef8-129">With *app.js* open in our editor, let's get hello basic structure of our program written.</span></span>

1. <span data-ttu-id="e9ef8-130">Adja hozzá a hello "szükséges" az NPM-csomagok hello felső hello alábbira:</span><span class="sxs-lookup"><span data-stu-id="e9ef8-130">Add hello "requires" for our NPM packages at hello top with hello following:</span></span>
   
    ``` javascript
    var msRestAzure = require('ms-rest-azure');
    var cdnManagementClient = require('azure-arm-cdn');
    ```
2. <span data-ttu-id="e9ef8-131">Toodefine kell néhány állandók a módszerek fogja használni.</span><span class="sxs-lookup"><span data-stu-id="e9ef8-131">We need toodefine some constants our methods will use.</span></span>  <span data-ttu-id="e9ef8-132">Adja hozzá a hello következő.</span><span class="sxs-lookup"><span data-stu-id="e9ef8-132">Add hello following.</span></span>  <span data-ttu-id="e9ef8-133">Lehet, hogy tooreplace hello helyőrzők, beleértve a hello  **&lt;csúcsos zárójelek&gt;**, igény szerint a saját értékekkel.</span><span class="sxs-lookup"><span data-stu-id="e9ef8-133">Be sure tooreplace hello placeholders, including hello **&lt;angle brackets&gt;**, with your own values as needed.</span></span>
   
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
3. <span data-ttu-id="e9ef8-134">Lesz a következő azt példányosítható hello CDN felügyeleti ügyfél, és adjon neki a hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="e9ef8-134">Next, we'll instantiate hello CDN management client and give it our credentials.</span></span>
   
    ``` javascript
    var credentials = new msRestAzure.ApplicationTokenCredentials(clientId, tenantId, clientSecret);
    var cdnClient = new cdnManagementClient(credentials, subscriptionId);
    ```
   
    <span data-ttu-id="e9ef8-135">Ha egyéni felhasználói hitelesítést használ, két sort némileg eltérő fog kinézni.</span><span class="sxs-lookup"><span data-stu-id="e9ef8-135">If you are using individual user authentication, these two lines will look slightly different.</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="e9ef8-136">Ha használja a kódmintában toohave egyes felhasználói hitelesítés helyett egy egyszerű szolgáltatás kiválasztása.</span><span class="sxs-lookup"><span data-stu-id="e9ef8-136">Only use this code sample if you are choosing toohave individual user authentication instead of a service principal.</span></span>  <span data-ttu-id="e9ef8-137">Gondos tooguard kell az egyéni felhasználói hitelesítő adatait, és tartsa titokban.</span><span class="sxs-lookup"><span data-stu-id="e9ef8-137">Be careful tooguard your individual user credentials and keep them secret.</span></span>
   > 
   > 
   
    ``` javascript
    var credentials = new msRestAzure.UserTokenCredentials(clientId, 
        tenantId, '<username>', '<password>', '<redirect URI>');
    var cdnClient = new cdnManagementClient(credentials, subscriptionId);
    ```
   
    <span data-ttu-id="e9ef8-138">Lehet, hogy tooreplace hello elemeinek  **&lt;csúcsos zárójelek&gt;**  hello megfelelő információkat.</span><span class="sxs-lookup"><span data-stu-id="e9ef8-138">Be sure tooreplace hello items in **&lt;angle brackets&gt;** with hello correct information.</span></span>  <span data-ttu-id="e9ef8-139">A `<redirect URI>`, hello átirányítási URI-t az Azure AD hello alkalmazás regisztrálásakor megadott használja.</span><span class="sxs-lookup"><span data-stu-id="e9ef8-139">For `<redirect URI>`, use hello redirect URI you entered when you registered hello application in Azure AD.</span></span>
4. <span data-ttu-id="e9ef8-140">A Node.js-Konzolalkalmazás érintetlen tootake egyes parancssori paraméterek.</span><span class="sxs-lookup"><span data-stu-id="e9ef8-140">Our Node.js console application is going tootake some command-line parameters.</span></span>  <span data-ttu-id="e9ef8-141">Most ellenőrzi, hogy legalább egy paramétert.</span><span class="sxs-lookup"><span data-stu-id="e9ef8-141">Let's validate that at least one parameter was passed.</span></span>
   
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
5. <span data-ttu-id="e9ef8-142">Amely számos lehetőséget kínál, a program, ahol azt fiókirodai, ki milyen paraméterek lettek átadva alapján tooother funkciók toohello fő részét.</span><span class="sxs-lookup"><span data-stu-id="e9ef8-142">That brings us toohello main part of our program, where we branch off tooother functions based on what parameters were passed.</span></span>
   
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
6. <span data-ttu-id="e9ef8-143">A program több helyen kell, hogy hello megfelelő számú paraméter lett átadva, és néhány Súgó megjelenítése, nem fogják látni megfelelő toomake.</span><span class="sxs-lookup"><span data-stu-id="e9ef8-143">At several places in our program, we'll need toomake sure hello right number of parameters were passed in and display some help if they don't look correct.</span></span>  <span data-ttu-id="e9ef8-144">Hozzon létre funkciók toodo, amely.</span><span class="sxs-lookup"><span data-stu-id="e9ef8-144">Let's create functions toodo that.</span></span>
   
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
7. <span data-ttu-id="e9ef8-145">Végezetül használni fogjuk hello CDN felügyeleti ügyfél hello funkciók aszinkron jellegűek, egy módszer toocall vissza, ha kész van szükségük.</span><span class="sxs-lookup"><span data-stu-id="e9ef8-145">Finally, hello functions we'll be using on hello CDN management client are asynchronous, so they need a method toocall back when they're done.</span></span>  <span data-ttu-id="e9ef8-146">Most Meggyőződünk tartalmazó hello kimeneti hello CDN felügyeleti ügyfél (ha van ilyen), és a hello program kilépésre.</span><span class="sxs-lookup"><span data-stu-id="e9ef8-146">Let's make one that can display hello output from hello CDN management client (if any) and exit hello program gracefully.</span></span>
   
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

<span data-ttu-id="e9ef8-147">Most, hogy a program alapvető szerkezete hello írása, a Microsoft hello függvény hívása a paraméterek alapján kell létrehoznia.</span><span class="sxs-lookup"><span data-stu-id="e9ef8-147">Now that hello basic structure of our program is written, we should create hello functions called based on our parameters.</span></span>

## <a name="list-cdn-profiles-and-endpoints"></a><span data-ttu-id="e9ef8-148">Lista CDN-profil és -végpontok</span><span class="sxs-lookup"><span data-stu-id="e9ef8-148">List CDN profiles and endpoints</span></span>
<span data-ttu-id="e9ef8-149">Kezdjük kód toolist a meglévő profilok és a végpontok.</span><span class="sxs-lookup"><span data-stu-id="e9ef8-149">Let's start with code toolist our existing profiles and endpoints.</span></span>  <span data-ttu-id="e9ef8-150">A kód megjegyzéseket, hogy tudjuk, ahol az egyes paramétereket kerül adja meg a várt hello szintaxist.</span><span class="sxs-lookup"><span data-stu-id="e9ef8-150">My code comments provide hello expected syntax so we know where each parameter goes.</span></span>

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

## <a name="create-cdn-profiles-and-endpoints"></a><span data-ttu-id="e9ef8-151">CDN-profil és a végpontok létrehozása</span><span class="sxs-lookup"><span data-stu-id="e9ef8-151">Create CDN profiles and endpoints</span></span>
<span data-ttu-id="e9ef8-152">A következő azt fog írni hello funkciók toocreate profilok és a végpontok.</span><span class="sxs-lookup"><span data-stu-id="e9ef8-152">Next, we'll write hello functions toocreate profiles and endpoints.</span></span>

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

## <a name="purge-an-endpoint"></a><span data-ttu-id="e9ef8-153">A végpont törlése</span><span class="sxs-lookup"><span data-stu-id="e9ef8-153">Purge an endpoint</span></span>
<span data-ttu-id="e9ef8-154">Ha hello végpont létrejött, egy közös feladat, hogy a program célszerű lehet tooperform van végleges törlése a végpont tartalma.</span><span class="sxs-lookup"><span data-stu-id="e9ef8-154">Assuming hello endpoint has been created, one common task that we might want tooperform in our program is purging content in our endpoint.</span></span>

```javascript
// purge <profile name> <endpoint name> <path>
function cdnPurge() {
    requireParms(4);
    console.log("Purging endpoint...");
    var purgeContentPaths = [ parms[3] ];
    cdnClient.endpoints.purgeContent(parms[2], parms[1], resourceGroupName, purgeContentPaths, callback);
}
```

## <a name="delete-cdn-profiles-and-endpoints"></a><span data-ttu-id="e9ef8-155">CDN-profil és a végpontok törlése</span><span class="sxs-lookup"><span data-stu-id="e9ef8-155">Delete CDN profiles and endpoints</span></span>
<span data-ttu-id="e9ef8-156">hello utolsó függvény is végpontok és a profilok törlése.</span><span class="sxs-lookup"><span data-stu-id="e9ef8-156">hello last function we will include deletes endpoints and profiles.</span></span>

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

## <a name="running-hello-program"></a><span data-ttu-id="e9ef8-157">Hello programot futtat</span><span class="sxs-lookup"><span data-stu-id="e9ef8-157">Running hello program</span></span>
<span data-ttu-id="e9ef8-158">Most már a Node.js-program használatával a kedvenc hibakereső végezhetünk vagy hello konzolján.</span><span class="sxs-lookup"><span data-stu-id="e9ef8-158">We can now execute our Node.js program using our favorite debugger or at hello console.</span></span>

> [!TIP]
> <span data-ttu-id="e9ef8-159">Visual Studio Code az hibakereső használata, tooset lesz szüksége a környezet toopass hello parancssori paraméterek mentése.</span><span class="sxs-lookup"><span data-stu-id="e9ef8-159">If you're using Visual Studio Code as your debugger, you'll need tooset up your environment toopass in hello command-line parameters.</span></span>  <span data-ttu-id="e9ef8-160">A Visual Studio Code nem ez hello **lanuch.json** fájlt.</span><span class="sxs-lookup"><span data-stu-id="e9ef8-160">Visual Studio Code does this in hello **lanuch.json** file.</span></span>  <span data-ttu-id="e9ef8-161">Keresse meg nevű tulajdonság **argumentum** és karakterlánc-értékeket a paraméterek tömbjét adja hozzá, úgy, hogy hasonló toothis: `"args": ["list", "profiles"]`.</span><span class="sxs-lookup"><span data-stu-id="e9ef8-161">Look for a property named **args** and add an array of string values for your parameters, so that it looks similar toothis:  `"args": ["list", "profiles"]`.</span></span>
> 
> 

<span data-ttu-id="e9ef8-162">Először a profilok listázása.</span><span class="sxs-lookup"><span data-stu-id="e9ef8-162">Let's start by listing our profiles.</span></span>

![Lista profilok](./media/cdn-app-dev-node/cdn-list-profiles.png)

<span data-ttu-id="e9ef8-164">Azt a kapott vissza üres tömb.</span><span class="sxs-lookup"><span data-stu-id="e9ef8-164">We got back an empty array.</span></span>  <span data-ttu-id="e9ef8-165">Jelenleg nincs a profil az erőforráscsoportban, mivel a várt érték, amely.</span><span class="sxs-lookup"><span data-stu-id="e9ef8-165">Since we don't have any profiles in our resource group, that's expected.</span></span>  <span data-ttu-id="e9ef8-166">Most hozzon létre most egy profilt.</span><span class="sxs-lookup"><span data-stu-id="e9ef8-166">Let's create a profile now.</span></span>

![Profil létrehozása](./media/cdn-app-dev-node/cdn-create-profile.png)

<span data-ttu-id="e9ef8-168">Most adjuk hozzá egy végpontot.</span><span class="sxs-lookup"><span data-stu-id="e9ef8-168">Now, let's add an endpoint.</span></span>

![-Végpont létrehozása](./media/cdn-app-dev-node/cdn-create-endpoint.png)

<span data-ttu-id="e9ef8-170">Végezetül most törli a profilt.</span><span class="sxs-lookup"><span data-stu-id="e9ef8-170">Finally, let's delete our profile.</span></span>

![Profil törlése](./media/cdn-app-dev-node/cdn-delete-profile.png)

## <a name="next-steps"></a><span data-ttu-id="e9ef8-172">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e9ef8-172">Next Steps</span></span>
<span data-ttu-id="e9ef8-173">toosee befejeződött hello projektet ebben a forgatókönyvben a [hello minta letöltése](https://code.msdn.microsoft.com/Azure-CDN-SDK-for-Nodejs-c712bc74).</span><span class="sxs-lookup"><span data-stu-id="e9ef8-173">toosee hello completed project from this walkthrough, [download hello sample](https://code.msdn.microsoft.com/Azure-CDN-SDK-for-Nodejs-c712bc74).</span></span>

<span data-ttu-id="e9ef8-174">hello Azure CDN SDK for Node.js, a nézet hello toosee hello referencia [hivatkozás](http://azure.github.io/azure-sdk-for-node/azure-arm-cdn/latest/).</span><span class="sxs-lookup"><span data-stu-id="e9ef8-174">toosee hello reference for hello Azure CDN SDK for Node.js, view hello [reference](http://azure.github.io/azure-sdk-for-node/azure-arm-cdn/latest/).</span></span>

<span data-ttu-id="e9ef8-175">toofind további dokumentációiért hello Azure SDK for Node.js, a nézet hello [hivatkozás teljes](http://azure.github.io/azure-sdk-for-node/).</span><span class="sxs-lookup"><span data-stu-id="e9ef8-175">toofind additional documentation on hello Azure SDK for Node.js, view hello [full reference](http://azure.github.io/azure-sdk-for-node/).</span></span>

<span data-ttu-id="e9ef8-176">A CDN erőforrások kezelése [PowerShell](cdn-manage-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="e9ef8-176">Manage your CDN resources with [PowerShell](cdn-manage-powershell.md).</span></span>

