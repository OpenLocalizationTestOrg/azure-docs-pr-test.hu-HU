---
title: "A Node.js Ismerkedés az Azure CDN SDK-val |} Microsoft Docs"
description: "Node.js-alkalmazások kezelése az Azure CDN írásának ismertetése."
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
ms.openlocfilehash: 46ae8cd9775432d126cbde856c1fb06ea319297e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-azure-cdn-development"></a><span data-ttu-id="ecabd-103">Ismerkedés az Azure CDN-fejlesztéssel</span><span class="sxs-lookup"><span data-stu-id="ecabd-103">Get started with Azure CDN development</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="ecabd-104">Node.js</span><span class="sxs-lookup"><span data-stu-id="ecabd-104">Node.js</span></span>](cdn-app-dev-node.md)
> * [<span data-ttu-id="ecabd-105">.NET</span><span class="sxs-lookup"><span data-stu-id="ecabd-105">.NET</span></span>](cdn-app-dev-net.md)
> 
> 

<span data-ttu-id="ecabd-106">Használhatja a [Azure CDN SDK for Node.js](https://www.npmjs.com/package/azure-arm-cdn) létrehozása és a CDN-profil és a végpontok automatizálására.</span><span class="sxs-lookup"><span data-stu-id="ecabd-106">You can use the [Azure CDN SDK for Node.js](https://www.npmjs.com/package/azure-arm-cdn) to automate creation and management of CDN profiles and endpoints.</span></span>  <span data-ttu-id="ecabd-107">Ez az oktatóanyag bemutatja, hogyan kell létrehozni egy egyszerű Node.js-Konzolalkalmazás, azt mutatja be a rendelkezésre álló műveletek számos.</span><span class="sxs-lookup"><span data-stu-id="ecabd-107">This tutorial walks through the creation of a simple Node.js console application that demonstrates several of the available operations.</span></span>  <span data-ttu-id="ecabd-108">Ez az oktatóanyag nem célja, hogy a Node.js, részletesen leírja az Azure CDN SDK minden szempontját.</span><span class="sxs-lookup"><span data-stu-id="ecabd-108">This tutorial is not intended to describe all aspects of the Azure CDN SDK for Node.js in detail.</span></span>

<span data-ttu-id="ecabd-109">Az oktatóanyag elvégzéséhez, akkor már rendelkezik [Node.js](http://www.nodejs.org) **4.x.x** vagy újabb rendszerre telepített és konfigurált.</span><span class="sxs-lookup"><span data-stu-id="ecabd-109">To complete this tutorial, you should already have [Node.js](http://www.nodejs.org) **4.x.x** or higher installed and configured.</span></span>  <span data-ttu-id="ecabd-110">Használhat bármilyen szövegszerkesztővel, a Node.js-alkalmazás létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="ecabd-110">You can use any text editor you want to create your Node.js application.</span></span>  <span data-ttu-id="ecabd-111">Ez az oktatóanyag írni használt [Visual Studio Code](https://code.visualstudio.com).</span><span class="sxs-lookup"><span data-stu-id="ecabd-111">To write this tutorial, I used [Visual Studio Code](https://code.visualstudio.com).</span></span>  

> [!TIP]
> <span data-ttu-id="ecabd-112">A [az oktatóanyagot befejezett projekt](https://code.msdn.microsoft.com/Azure-CDN-SDK-for-Nodejs-c712bc74) letölthető az MSDN Webhelyén.</span><span class="sxs-lookup"><span data-stu-id="ecabd-112">The [completed project from this tutorial](https://code.msdn.microsoft.com/Azure-CDN-SDK-for-Nodejs-c712bc74) is available for download on MSDN.</span></span>
> 
> 

[!INCLUDE [cdn-app-dev-prep](../../includes/cdn-app-dev-prep.md)]

## <a name="create-your-project-and-add-npm-dependencies"></a><span data-ttu-id="ecabd-113">A projekt létrehozása és hozzáadása NPM függőségek</span><span class="sxs-lookup"><span data-stu-id="ecabd-113">Create your project and add NPM dependencies</span></span>
<span data-ttu-id="ecabd-114">Most, hogy előre létrehozott erőforráscsoport a CDN-profil és a CDN-profil és a csoporton belüli végpontok kezelése az Azure AD-alkalmazás engedélyt kap, nem lehet elindítani, az alkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="ecabd-114">Now that we've created a resource group for our CDN profiles and given our Azure AD application permission to manage CDN profiles and endpoints within that group, we can start creating our application.</span></span>

<span data-ttu-id="ecabd-115">Hozzon létre egy mappát az alkalmazás tárolásához.</span><span class="sxs-lookup"><span data-stu-id="ecabd-115">Create a folder to store your application.</span></span>  <span data-ttu-id="ecabd-116">Az aktuális elérési úthoz, a Node.js eszközökkel konzolon az aktuális hely beállítása az új mappába, és a projekt inicializálása a következő futtatásával:</span><span class="sxs-lookup"><span data-stu-id="ecabd-116">From a console with the Node.js tools in your current path, set your current location to this new folder and initialize your project by executing:</span></span>

    npm init

<span data-ttu-id="ecabd-117">Majd választhat a projekt inicializálása kérdések sorát teszi fel.</span><span class="sxs-lookup"><span data-stu-id="ecabd-117">You will then be presented a series of questions to initialize your project.</span></span>  <span data-ttu-id="ecabd-118">A **belépési pont**, ez az oktatóanyag használja *app.js*.</span><span class="sxs-lookup"><span data-stu-id="ecabd-118">For **entry point**, this tutorial uses *app.js*.</span></span>  <span data-ttu-id="ecabd-119">Az egyéb lehetőségek az alábbi példában látható.</span><span class="sxs-lookup"><span data-stu-id="ecabd-119">You can see my other choices in the following example.</span></span>

![NPM init kimeneti](./media/cdn-app-dev-node/cdn-npm-init.png)

<span data-ttu-id="ecabd-121">A projekt már inicializálva van egy *packages.json* fájlt.</span><span class="sxs-lookup"><span data-stu-id="ecabd-121">Our project is now initialized with a *packages.json* file.</span></span>  <span data-ttu-id="ecabd-122">A projekt üzemeltetéséhez kívánja használni az NPM csomagban foglalt Azure könyvtárak.</span><span class="sxs-lookup"><span data-stu-id="ecabd-122">Our project is going to use some Azure libraries contained in NPM packages.</span></span>  <span data-ttu-id="ecabd-123">Az Azure-ügyfél futásidejű a Node.js (ms-rest-azure) és az Azure CDN ügyféloldali kódtára a Node.js (azure-arm-cd) fogjuk használni.</span><span class="sxs-lookup"><span data-stu-id="ecabd-123">We'll use the Azure Client Runtime for Node.js (ms-rest-azure) and the Azure CDN Client Library for Node.js (azure-arm-cd).</span></span>  <span data-ttu-id="ecabd-124">Szerint függőségeinek adjuk hozzá azokat a projekthez.</span><span class="sxs-lookup"><span data-stu-id="ecabd-124">Let's add those to the project as dependencies.</span></span>

    npm install --save ms-rest-azure
    npm install --save azure-arm-cdn

<span data-ttu-id="ecabd-125">Miután végzett a csomagok telepítése, a *package.json* fájl ebben a példában (verzió: számok eltérőek lehetnek) hasonlóan kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="ecabd-125">After the packages are done installing, the *package.json* file should look similar to this example (version numbers may differ):</span></span>

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

<span data-ttu-id="ecabd-126">Végezetül a szövegszerkesztővel, hozzon létre egy üres szöveges fájlt, és mentse a projekt mappában gyökérmappájában *app.js*.</span><span class="sxs-lookup"><span data-stu-id="ecabd-126">Finally, using your text editor, create a blank text file and save it in the root of our project folder as *app.js*.</span></span>  <span data-ttu-id="ecabd-127">Most még készen kód írása.</span><span class="sxs-lookup"><span data-stu-id="ecabd-127">We're now ready to begin writing code.</span></span>

## <a name="requires-constants-authentication-and-structure"></a><span data-ttu-id="ecabd-128">Szükséges, állandók, hitelesítési és szerkezete</span><span class="sxs-lookup"><span data-stu-id="ecabd-128">Requires, constants, authentication, and structure</span></span>
<span data-ttu-id="ecabd-129">A *app.js* nyissa meg a szerkesztőben, folytassuk a program írása alapvető szerkezete.</span><span class="sxs-lookup"><span data-stu-id="ecabd-129">With *app.js* open in our editor, let's get the basic structure of our program written.</span></span>

1. <span data-ttu-id="ecabd-130">Vegye fel a "szükséges" az NPM-csomagok tetején a következő:</span><span class="sxs-lookup"><span data-stu-id="ecabd-130">Add the "requires" for our NPM packages at the top with the following:</span></span>
   
    ``` javascript
    var msRestAzure = require('ms-rest-azure');
    var cdnManagementClient = require('azure-arm-cdn');
    ```
2. <span data-ttu-id="ecabd-131">Adja meg az egyes állandók a módszerek fogja használni kell.</span><span class="sxs-lookup"><span data-stu-id="ecabd-131">We need to define some constants our methods will use.</span></span>  <span data-ttu-id="ecabd-132">Adja hozzá a következő.</span><span class="sxs-lookup"><span data-stu-id="ecabd-132">Add the following.</span></span>  <span data-ttu-id="ecabd-133">Ügyeljen arra, hogy cserélje le a helyőrzőket, beleértve a  **&lt;csúcsos zárójelek&gt;**, igény szerint a saját értékekkel.</span><span class="sxs-lookup"><span data-stu-id="ecabd-133">Be sure to replace the placeholders, including the **&lt;angle brackets&gt;**, with your own values as needed.</span></span>
   
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
3. <span data-ttu-id="ecabd-134">Lesz ezután azt példányt létrehozni a CDN-felügyeleti ügyfél, és adjon neki a hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="ecabd-134">Next, we'll instantiate the CDN management client and give it our credentials.</span></span>
   
    ``` javascript
    var credentials = new msRestAzure.ApplicationTokenCredentials(clientId, tenantId, clientSecret);
    var cdnClient = new cdnManagementClient(credentials, subscriptionId);
    ```
   
    <span data-ttu-id="ecabd-135">Ha egyéni felhasználói hitelesítést használ, két sort némileg eltérő fog kinézni.</span><span class="sxs-lookup"><span data-stu-id="ecabd-135">If you are using individual user authentication, these two lines will look slightly different.</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="ecabd-136">Ha használja a kódmintában a felhasználói hitelesítés helyett egy egyszerű szolgáltatás kiválasztása.</span><span class="sxs-lookup"><span data-stu-id="ecabd-136">Only use this code sample if you are choosing to have individual user authentication instead of a service principal.</span></span>  <span data-ttu-id="ecabd-137">Ügyeljen rá, és tartsa titokban az egyéni felhasználói hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="ecabd-137">Be careful to guard your individual user credentials and keep them secret.</span></span>
   > 
   > 
   
    ``` javascript
    var credentials = new msRestAzure.UserTokenCredentials(clientId, 
        tenantId, '<username>', '<password>', '<redirect URI>');
    var cdnClient = new cdnManagementClient(credentials, subscriptionId);
    ```
   
    <span data-ttu-id="ecabd-138">Ügyeljen arra, hogy az elemek cseréje  **&lt;csúcsos zárójelek&gt;**  a megfelelő információkkal.</span><span class="sxs-lookup"><span data-stu-id="ecabd-138">Be sure to replace the items in **&lt;angle brackets&gt;** with the correct information.</span></span>  <span data-ttu-id="ecabd-139">A `<redirect URI>`, használja az átirányítási URI-t az Azure ad-ben az alkalmazás regisztrálásakor megadott.</span><span class="sxs-lookup"><span data-stu-id="ecabd-139">For `<redirect URI>`, use the redirect URI you entered when you registered the application in Azure AD.</span></span>
4. <span data-ttu-id="ecabd-140">A Node.js-Konzolalkalmazás lesz e parancssori paraméterek.</span><span class="sxs-lookup"><span data-stu-id="ecabd-140">Our Node.js console application is going to take some command-line parameters.</span></span>  <span data-ttu-id="ecabd-141">Most ellenőrzi, hogy legalább egy paramétert.</span><span class="sxs-lookup"><span data-stu-id="ecabd-141">Let's validate that at least one parameter was passed.</span></span>
   
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
5. <span data-ttu-id="ecabd-142">Amely számos lehetőséget kínál, a program, ahol azt fiókirodai más funkciók alapján milyen paraméterek lettek átadva a fő részére.</span><span class="sxs-lookup"><span data-stu-id="ecabd-142">That brings us to the main part of our program, where we branch off to other functions based on what parameters were passed.</span></span>
   
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
6. <span data-ttu-id="ecabd-143">A program több helyen igazolnia kell győződjön meg arról, hogy a megfelelő számú paraméter lett átadva, és néhány Súgó megjelenítése, ha megfelelő nem megfelelő.</span><span class="sxs-lookup"><span data-stu-id="ecabd-143">At several places in our program, we'll need to make sure the right number of parameters were passed in and display some help if they don't look correct.</span></span>  <span data-ttu-id="ecabd-144">Hozzon létre, amely függvényt.</span><span class="sxs-lookup"><span data-stu-id="ecabd-144">Let's create functions to do that.</span></span>
   
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
7. <span data-ttu-id="ecabd-145">Végezetül a fogjuk használni a CDN-felügyeleti ügyfél függvényei aszinkron, egy metódust kell meghívni, amikor kész van szükségük.</span><span class="sxs-lookup"><span data-stu-id="ecabd-145">Finally, the functions we'll be using on the CDN management client are asynchronous, so they need a method to call back when they're done.</span></span>  <span data-ttu-id="ecabd-146">Ellenőrizze egy, a kimenet a CDN-felügyeleti ügyfél (ha van ilyen), és a szabályosan kilép a programból.</span><span class="sxs-lookup"><span data-stu-id="ecabd-146">Let's make one that can display the output from the CDN management client (if any) and exit the program gracefully.</span></span>
   
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

<span data-ttu-id="ecabd-147">Most, hogy a program alapvető szerkezete írása, azt a függvény hívása a paraméterek alapján kell létrehoznia.</span><span class="sxs-lookup"><span data-stu-id="ecabd-147">Now that the basic structure of our program is written, we should create the functions called based on our parameters.</span></span>

## <a name="list-cdn-profiles-and-endpoints"></a><span data-ttu-id="ecabd-148">Lista CDN-profil és -végpontok</span><span class="sxs-lookup"><span data-stu-id="ecabd-148">List CDN profiles and endpoints</span></span>
<span data-ttu-id="ecabd-149">Kezdjük kód a meglévő profilok és a végpontok listáját.</span><span class="sxs-lookup"><span data-stu-id="ecabd-149">Let's start with code to list our existing profiles and endpoints.</span></span>  <span data-ttu-id="ecabd-150">A kód megjegyzéseket adja meg a várt szintaxist, hogy tudjuk, ahol az egyes paramétereket kerül.</span><span class="sxs-lookup"><span data-stu-id="ecabd-150">My code comments provide the expected syntax so we know where each parameter goes.</span></span>

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

## <a name="create-cdn-profiles-and-endpoints"></a><span data-ttu-id="ecabd-151">CDN-profil és a végpontok létrehozása</span><span class="sxs-lookup"><span data-stu-id="ecabd-151">Create CDN profiles and endpoints</span></span>
<span data-ttu-id="ecabd-152">A Funkciók, profilok és a végpontok létrehozása a következő fog írni azt.</span><span class="sxs-lookup"><span data-stu-id="ecabd-152">Next, we'll write the functions to create profiles and endpoints.</span></span>

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

## <a name="purge-an-endpoint"></a><span data-ttu-id="ecabd-153">A végpont törlése</span><span class="sxs-lookup"><span data-stu-id="ecabd-153">Purge an endpoint</span></span>
<span data-ttu-id="ecabd-154">Feltéve, hogy a végpont létrehozását, azt szeretnénk, előfordulhat, hogy a program végrehajtásához egy közös tevékenység a végpont tartalmának van kiürítése.</span><span class="sxs-lookup"><span data-stu-id="ecabd-154">Assuming the endpoint has been created, one common task that we might want to perform in our program is purging content in our endpoint.</span></span>

```javascript
// purge <profile name> <endpoint name> <path>
function cdnPurge() {
    requireParms(4);
    console.log("Purging endpoint...");
    var purgeContentPaths = [ parms[3] ];
    cdnClient.endpoints.purgeContent(parms[2], parms[1], resourceGroupName, purgeContentPaths, callback);
}
```

## <a name="delete-cdn-profiles-and-endpoints"></a><span data-ttu-id="ecabd-155">CDN-profil és a végpontok törlése</span><span class="sxs-lookup"><span data-stu-id="ecabd-155">Delete CDN profiles and endpoints</span></span>
<span data-ttu-id="ecabd-156">Az utolsó függvény is végpontok és a profilok törlése.</span><span class="sxs-lookup"><span data-stu-id="ecabd-156">The last function we will include deletes endpoints and profiles.</span></span>

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

## <a name="running-the-program"></a><span data-ttu-id="ecabd-157">A program futtatása</span><span class="sxs-lookup"><span data-stu-id="ecabd-157">Running the program</span></span>
<span data-ttu-id="ecabd-158">Most már a Node.js-program használatával a kedvenc hibakereső végezhetünk vagy a konzolon.</span><span class="sxs-lookup"><span data-stu-id="ecabd-158">We can now execute our Node.js program using our favorite debugger or at the console.</span></span>

> [!TIP]
> <span data-ttu-id="ecabd-159">Visual Studio Code az hibakereső használata, ha szüksége a környezet kialakítása felelt meg a parancssori paraméterek.</span><span class="sxs-lookup"><span data-stu-id="ecabd-159">If you're using Visual Studio Code as your debugger, you'll need to set up your environment to pass in the command-line parameters.</span></span>  <span data-ttu-id="ecabd-160">A Visual Studio Code nem ez a **lanuch.json** fájlt.</span><span class="sxs-lookup"><span data-stu-id="ecabd-160">Visual Studio Code does this in the **lanuch.json** file.</span></span>  <span data-ttu-id="ecabd-161">Keresse meg nevű tulajdonság **argumentum** , és adja hozzá a karakterlánc-értékeket a paraméterek tömbje úgy, hogy hasonló: `"args": ["list", "profiles"]`.</span><span class="sxs-lookup"><span data-stu-id="ecabd-161">Look for a property named **args** and add an array of string values for your parameters, so that it looks similar to this:  `"args": ["list", "profiles"]`.</span></span>
> 
> 

<span data-ttu-id="ecabd-162">Először a profilok listázása.</span><span class="sxs-lookup"><span data-stu-id="ecabd-162">Let's start by listing our profiles.</span></span>

![Lista profilok](./media/cdn-app-dev-node/cdn-list-profiles.png)

<span data-ttu-id="ecabd-164">Azt a kapott vissza üres tömb.</span><span class="sxs-lookup"><span data-stu-id="ecabd-164">We got back an empty array.</span></span>  <span data-ttu-id="ecabd-165">Jelenleg nincs a profil az erőforráscsoportban, mivel a várt érték, amely.</span><span class="sxs-lookup"><span data-stu-id="ecabd-165">Since we don't have any profiles in our resource group, that's expected.</span></span>  <span data-ttu-id="ecabd-166">Most hozzon létre most egy profilt.</span><span class="sxs-lookup"><span data-stu-id="ecabd-166">Let's create a profile now.</span></span>

![Profil létrehozása](./media/cdn-app-dev-node/cdn-create-profile.png)

<span data-ttu-id="ecabd-168">Most adjuk hozzá egy végpontot.</span><span class="sxs-lookup"><span data-stu-id="ecabd-168">Now, let's add an endpoint.</span></span>

![-Végpont létrehozása](./media/cdn-app-dev-node/cdn-create-endpoint.png)

<span data-ttu-id="ecabd-170">Végezetül most törli a profilt.</span><span class="sxs-lookup"><span data-stu-id="ecabd-170">Finally, let's delete our profile.</span></span>

![Profil törlése](./media/cdn-app-dev-node/cdn-delete-profile.png)

## <a name="next-steps"></a><span data-ttu-id="ecabd-172">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ecabd-172">Next Steps</span></span>
<span data-ttu-id="ecabd-173">Ez a forgatókönyv a befejezett projekt megjelenítéséhez [a minta letöltéséhez](https://code.msdn.microsoft.com/Azure-CDN-SDK-for-Nodejs-c712bc74).</span><span class="sxs-lookup"><span data-stu-id="ecabd-173">To see the completed project from this walkthrough, [download the sample](https://code.msdn.microsoft.com/Azure-CDN-SDK-for-Nodejs-c712bc74).</span></span>

<span data-ttu-id="ecabd-174">Tekintse meg a referencia az Azure CDN SDK-ban a Node.js, tekintse meg a [hivatkozás](http://azure.github.io/azure-sdk-for-node/azure-arm-cdn/latest/).</span><span class="sxs-lookup"><span data-stu-id="ecabd-174">To see the reference for the Azure CDN SDK for Node.js, view the [reference](http://azure.github.io/azure-sdk-for-node/azure-arm-cdn/latest/).</span></span>

<span data-ttu-id="ecabd-175">További dokumentációjában talál az Azure SDK-val Node.js, tekintse meg a [hivatkozás teljes](http://azure.github.io/azure-sdk-for-node/).</span><span class="sxs-lookup"><span data-stu-id="ecabd-175">To find additional documentation on the Azure SDK for Node.js, view the [full reference](http://azure.github.io/azure-sdk-for-node/).</span></span>

<span data-ttu-id="ecabd-176">A CDN erőforrások kezelése [PowerShell](cdn-manage-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="ecabd-176">Manage your CDN resources with [PowerShell](cdn-manage-powershell.md).</span></span>

