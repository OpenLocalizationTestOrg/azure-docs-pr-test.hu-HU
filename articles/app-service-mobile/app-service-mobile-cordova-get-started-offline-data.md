---
title: "A Azure Mobile Apps (Cordova) kapcsolat nélküli szinkronizálásának engedélyezése |} Microsoft Docs"
description: "App Service Mobile Apps a Cordova-alkalmazás gyorsítótárába, a szinkronizálási kapcsolat nélküli adatainak használata"
documentationcenter: cordova
author: ggailey777
manager: syntaxc4
editor: 
services: app-service\mobile
ms.assetid: 1a3f685d-f79d-4f8b-ae11-ff96e79e9de9
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-cordova-ios
ms.devlang: dotnet
ms.topic: article
ms.date: 10/30/2016
ms.author: glenga
ms.openlocfilehash: 45e80ca672dfdb6defc6e5c1aac3d29f5479125c
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="enable-offline-sync-for-your-cordova-mobile-app"></a><span data-ttu-id="d7199-103">A mobil Cordova-alkalmazás kapcsolat nélküli szinkronizálásának engedélyezése</span><span class="sxs-lookup"><span data-stu-id="d7199-103">Enable offline sync for your Cordova mobile app</span></span>
[!INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

<span data-ttu-id="d7199-104">Ez az oktatóanyag a kapcsolat nélküli szinkronizálás szolgáltatást, az Azure Mobile Apps a Cordova vezet be.</span><span class="sxs-lookup"><span data-stu-id="d7199-104">This tutorial introduces the offline sync feature of Azure Mobile Apps for Cordova.</span></span> <span data-ttu-id="d7199-105">Kapcsolat nélküli szinkronizálás lehetővé teszi, hogy a végfelhasználók számára a mobilalkalmazás együttműködhet&mdash;megtekintését, hozzáadását és módosítását adatok&mdash;akkor is, ha nincs hálózati kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="d7199-105">Offline sync allows end users to interact with a mobile app&mdash;viewing, adding, or modifying data&mdash;even when there is no network connection.</span></span> <span data-ttu-id="d7199-106">Változások a helyi adatbázisban tárolódnak.</span><span class="sxs-lookup"><span data-stu-id="d7199-106">Changes are stored in a local database.</span></span>  <span data-ttu-id="d7199-107">Az eszköz újra online állapotba kerül, ha ezek a változások szinkronizálása megtörtént-e a távoli szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="d7199-107">Once the device is back online, these changes are synced with the remote service.</span></span>

<span data-ttu-id="d7199-108">Ez az oktatóanyag alapul a Cordova gyors üzembe helyezés megoldást hoz létre, ha az oktatóanyag befejezése Mobile Apps-alkalmazáshoz [Apache Cordova gyors üzembe helyezési].</span><span class="sxs-lookup"><span data-stu-id="d7199-108">This tutorial is based on the Cordova quickstart solution for Mobile Apps that you create when you complete the tutorial [Apache Cordova quick start].</span></span> <span data-ttu-id="d7199-109">Ebben az oktatóanyagban az Azure Mobile Apps offline funkciók hozzáadása a gyors üzembe helyezés megoldás frissítése.</span><span class="sxs-lookup"><span data-stu-id="d7199-109">In this tutorial, you update the quickstart solution to add offline features of Azure Mobile Apps.</span></span>  <span data-ttu-id="d7199-110">Azt is kiemelése a kapcsolat nélküli tartozó kódot az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="d7199-110">We also highlight the offline-specific code in the app.</span></span>

<span data-ttu-id="d7199-111">A kapcsolat nélküli szinkronizálás szolgáltatással kapcsolatos további tudnivalókért lásd a témakör [az Azure Mobile Apps Offline adatszinkronizálás].</span><span class="sxs-lookup"><span data-stu-id="d7199-111">To learn more about the offline sync feature, see the topic [Offline Data Sync in Azure Mobile Apps].</span></span> <span data-ttu-id="d7199-112">API-használati részletekért lásd: a [API-JÁNAK dokumentációja](https://azure.github.io/azure-mobile-apps-js-client).</span><span class="sxs-lookup"><span data-stu-id="d7199-112">For details of API usage, see the [API documentation](https://azure.github.io/azure-mobile-apps-js-client).</span></span>

## <a name="add-offline-sync-to-the-quickstart-solution"></a><span data-ttu-id="d7199-113">Kapcsolat nélküli szinkronizálás hozzáadása a gyors üzembe helyezés megoldáshoz</span><span class="sxs-lookup"><span data-stu-id="d7199-113">Add offline sync to the quickstart solution</span></span>
<span data-ttu-id="d7199-114">A kapcsolat nélküli szinkronizálás kódot kell felvenni az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="d7199-114">The offline sync code must be added to the app.</span></span> <span data-ttu-id="d7199-115">Kapcsolat nélküli szinkronizálás a cordova-sqlite-tároló beépülő modul, amely automatikusan bekerül az alkalmazáshoz, ha a projekt tartalmazza az Azure Mobile Apps beépülő modul szükséges.</span><span class="sxs-lookup"><span data-stu-id="d7199-115">Offline sync requires the cordova-sqlite-storage plugin, which automatically gets added to your app when the Azure Mobile Apps plugin is included in the project.</span></span> <span data-ttu-id="d7199-116">A Gyorsútmutató-projekt tartalmazza a beépülő modulok mindegyikét.</span><span class="sxs-lookup"><span data-stu-id="d7199-116">The Quickstart project includes both of these plugins.</span></span>

1. <span data-ttu-id="d7199-117">A Visual Studio Solution Explorerben nyissa meg a index.js, és cserélje le a következő kódot</span><span class="sxs-lookup"><span data-stu-id="d7199-117">In Visual Studio's Solution Explorer, open index.js and replace the following code</span></span>

        var client,            // Connection to the Azure Mobile App backend
           todoItemTable;      // Reference to a table endpoint on backend

    <span data-ttu-id="d7199-118">Ezzel a kóddal:</span><span class="sxs-lookup"><span data-stu-id="d7199-118">with this code:</span></span>

        var client,            // Connection to the Azure Mobile App backend
           todoItemTable,      // Reference to a table endpoint on backend
           syncContext;        // Reference to offline data sync context

2. <span data-ttu-id="d7199-119">Ezután cserélje le a következő kódot:</span><span class="sxs-lookup"><span data-stu-id="d7199-119">Next, replace the following code:</span></span>

        client = new WindowsAzure.MobileServiceClient('http://yourmobileapp.azurewebsites.net');

    <span data-ttu-id="d7199-120">Ezzel a kóddal:</span><span class="sxs-lookup"><span data-stu-id="d7199-120">with this code:</span></span>

        client = new WindowsAzure.MobileServiceClient('http://yourmobileapp.azurewebsites.net');
        var store = new WindowsAzure.MobileServiceSqliteStore('store.db');

        store.defineTable({
          name: 'todoitem',
          columnDefinitions: {
              id: 'string',
              text: 'string',
              complete: 'boolean',
              version: 'string'
          }
        });

        // Get the sync context from the client
        syncContext = client.getSyncContext();

    <span data-ttu-id="d7199-121">Az előző kód kiegészítésekkel inicializálása a helyi tárolójába, és adjon meg egy helyi táblát, amely az oszlopértékeket használt megfelel az Azure-ban háttér.</span><span class="sxs-lookup"><span data-stu-id="d7199-121">The preceding code additions initialize the local store and define a local table that matches the column values used in your Azure back end.</span></span> <span data-ttu-id="d7199-122">(Nincs szükség minden oszlopértékeket foglalandó ezt a kódot.)  A `version` mező által a mobil-háttéralkalmazást, és ütközésfeloldás közben használható.</span><span class="sxs-lookup"><span data-stu-id="d7199-122">(You don't need to include all column values in this code.)  The `version` field is maintained by the mobile backend and is used for conflict resolution.</span></span>

    <span data-ttu-id="d7199-123">A szinkronizálási környezetet hivatkozást kap meghívásával **getSyncContext**.</span><span class="sxs-lookup"><span data-stu-id="d7199-123">You get a reference to the sync context by calling **getSyncContext**.</span></span> <span data-ttu-id="d7199-124">A szinkronizálási környezetet segíti követésével és kérdez le a módosításokat minden olyan táblát, egy ügyfélalkalmazás módosította, ha a tábla közötti megőrzése érdekében `.push()` nevezik.</span><span class="sxs-lookup"><span data-stu-id="d7199-124">The sync context helps preserve table relationships by tracking and pushing changes in all tables a client app has modified when `.push()` is called.</span></span>

3. <span data-ttu-id="d7199-125">Frissítse az alkalmazás URL-címet a mobilalkalmazás alkalmazás URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="d7199-125">Update the application URL to your Mobile App application URL.</span></span>

4. <span data-ttu-id="d7199-126">Ezután cserélje le ezt a kódot:</span><span class="sxs-lookup"><span data-stu-id="d7199-126">Next, replace this code:</span></span>

        todoItemTable = client.getTable('todoitem'); // todoitem is the table name

    <span data-ttu-id="d7199-127">Ezzel a kóddal:</span><span class="sxs-lookup"><span data-stu-id="d7199-127">with this code:</span></span>

        // Initialize the sync context with the store
        syncContext.initialize(store).then(function () {

        // Get the local table reference.
        todoItemTable = client.getSyncTable('todoitem');

        syncContext.pushHandler = {
            onConflict: function (pushError) {
                // Handle the conflict.
                console.log("Sync conflict! " + pushError.getError().message);
                // Update failed, revert to server's copy.
                pushError.cancelAndDiscard();
              },
              onError: function (pushError) {
                  // Handle the error
                  // In the simulated offline state, you get "Sync error! Unexpected connection failure."
                  console.log("Sync error! " + pushError.getError().message);
              }
        };

        // Call a function to perform the actual sync
        syncBackend();

        // Refresh the todoItems
        refreshDisplay();

        // Wire up the UI Event Handler for the Add Item
        $('#add-item').submit(addItemHandler);
        $('#refresh').on('click', refreshDisplay);

    <span data-ttu-id="d7199-128">Az előzőekben látható kód inicializálja a szinkronizálási környezetet, és ekkor meghívja a getSyncTable (helyett getTable) történő hivatkozás a helyi táblába.</span><span class="sxs-lookup"><span data-stu-id="d7199-128">The preceding code initializes the sync context and then calls getSyncTable (instead of getTable) to get a reference to the local table.</span></span>

    <span data-ttu-id="d7199-129">A kódot használja az összes helyi adatbázis létrehozása, olvasása, frissítése és törlése (CRUD) tábla műveletek.</span><span class="sxs-lookup"><span data-stu-id="d7199-129">This code uses the local database for all create, read, update, and delete (CRUD) table operations.</span></span>

    <span data-ttu-id="d7199-130">Ez a minta egyszerű hibakezelési szinkronizálási ütközések hajt végre.</span><span class="sxs-lookup"><span data-stu-id="d7199-130">This sample performs simple error handling on sync conflicts.</span></span> <span data-ttu-id="d7199-131">Egy valós alkalmazás hálózati feltételek mellett, a kiszolgáló ütközések és mások például a különféle hibák szeretné kezelni.</span><span class="sxs-lookup"><span data-stu-id="d7199-131">A real application would handle the various errors like network conditions, server conflicts, and others.</span></span> <span data-ttu-id="d7199-132">Kód példákért lásd a [kapcsolat nélküli szinkronizálás minta].</span><span class="sxs-lookup"><span data-stu-id="d7199-132">For code examples, see the [offline sync sample].</span></span>

5. <span data-ttu-id="d7199-133">Ezután adja hozzá ezt a funkciót a tényleges szinkronizálás végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="d7199-133">Next, add this function to perform the actual sync.</span></span>

        function syncBackend() {

          // Sync local store to Azure table when app loads, or when login complete.
          syncContext.push().then(function () {
              // Push completed

          });

          // Pull items from the Azure table after syncing to Azure.
          syncContext.pull(new WindowsAzure.Query('todoitem'));
        }

    <span data-ttu-id="d7199-134">Úgy dönt, hogy mikor kell a Mobile Apps-háttéralkalmazás meghívásával változásainak leküldése **syncContext.push()**.</span><span class="sxs-lookup"><span data-stu-id="d7199-134">You decide when to push changes to the Mobile App backend by calling **syncContext.push()**.</span></span> <span data-ttu-id="d7199-135">Például hívhatja **syncBackend** a szinkronizálás gomb kötve egy gomb eseménykezelő.</span><span class="sxs-lookup"><span data-stu-id="d7199-135">For example, you could call **syncBackend** in a button event handler tied to a sync button.</span></span>

## <a name="offline-sync-considerations"></a><span data-ttu-id="d7199-136">Kapcsolat nélküli szinkronizálás kapcsolatos szempontok</span><span class="sxs-lookup"><span data-stu-id="d7199-136">Offline sync considerations</span></span>

<span data-ttu-id="d7199-137">A példában a **leküldéses** metódusában **syncContext** alkalmazás következő indításakor a visszahívási függvény bejelentkezési azonosító csak nevezik.</span><span class="sxs-lookup"><span data-stu-id="d7199-137">In the sample, the **push** method of **syncContext** is only called on app startup in the callback function for login.</span></span>  <span data-ttu-id="d7199-138">Egy valós alkalmazás is teheti a szinkronizálási funkció manuálisan elindítva, vagy ha hálózati állapota megváltozik.</span><span class="sxs-lookup"><span data-stu-id="d7199-138">In a real-world application, you could also make this sync functionality triggered manually or when the network state changes.</span></span>

<span data-ttu-id="d7199-139">Elleni táblának függőben lévő lekérési végrehajtásakor helyi frissítések követi nyomon a környezetben, hogy a pull művelet automatikusan eseményindítók leküldéses.</span><span class="sxs-lookup"><span data-stu-id="d7199-139">When a pull is executed against a table that has pending local updates tracked by the context, that pull operation automatically triggers a push.</span></span> <span data-ttu-id="d7199-140">Frissítéskor, hozzáadását, és ebben a példában szereplő elemek befejezése, akkor kihagyhatja a explicit **leküldéses** hívható, mert elképzelhető, hogy a redundáns.</span><span class="sxs-lookup"><span data-stu-id="d7199-140">When refreshing, adding, and completing items in this sample, you can omit the explicit **push** call, since it may be redundant.</span></span>

<span data-ttu-id="d7199-141">A megadott kód, a rendszer megkérdezi a a távoli todoItem tábla összes adatát, de a rekordok szűrése úgy, hogy a lekérdezés azonosítóját, és a lekérdezési lehetőség arra is **leküldéses**.</span><span class="sxs-lookup"><span data-stu-id="d7199-141">In the provided code, all records in the remote todoItem table are queried, but it is also possible to filter records by passing a query id and query to **push**.</span></span> <span data-ttu-id="d7199-142">További információkért tekintse meg a szakasz *növekményes szinkronizálás* a [az Azure Mobile Apps Offline adatszinkronizálás].</span><span class="sxs-lookup"><span data-stu-id="d7199-142">For more information, see the section *Incremental Sync* in [Offline Data Sync in Azure Mobile Apps].</span></span>

## <a name="optional-disable-authentication"></a><span data-ttu-id="d7199-143">(Választható) Hitelesítés letiltása</span><span class="sxs-lookup"><span data-stu-id="d7199-143">(Optional) Disable authentication</span></span>

<span data-ttu-id="d7199-144">Ha nem szeretné tesztelési kapcsolat nélküli szinkronizálás előtt hitelesítés beállítása, a visszahívási függvény bejelentkezési azonosítóhoz megjegyzéssé, de hagyja meg a kódot a visszahívási függvény uncommented.</span><span class="sxs-lookup"><span data-stu-id="d7199-144">If you don't want to set up authentication before testing offline sync, comment out the callback function for login, but leave the code inside the callback function uncommented.</span></span>  <span data-ttu-id="d7199-145">Után fűzött megjegyzések ki a bejelentkezési sorok, a kódot a következőképpen:</span><span class="sxs-lookup"><span data-stu-id="d7199-145">After commenting out the login lines, the code follows:</span></span>

      // Login to the service.
      // client.login('twitter')
      //    .then(function () {
        syncContext.initialize(store).then(function () {
          // Leave the rest of the code in this callback function  uncommented.
                ...
        });
      // }, handleError);

<span data-ttu-id="d7199-146">Most az alkalmazás szinkronizál az Azure-háttérrendszernek az alkalmazás futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="d7199-146">Now, the app syncs with the Azure backend when you run the app.</span></span>

## <a name="run-the-client-app"></a><span data-ttu-id="d7199-147">Az ügyfél-alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="d7199-147">Run the client app</span></span>
<span data-ttu-id="d7199-148">A kapcsolat nélküli szinkronizálás most engedélyezve van futtathatja az ügyfélalkalmazás legalább egyszer a helyi tárolójába adatbázisának feltöltése minden platformon.</span><span class="sxs-lookup"><span data-stu-id="d7199-148">With offline sync now enabled, you can run the client application at least once on each platform to populate the local store database.</span></span> <span data-ttu-id="d7199-149">Később egy kapcsolat nélküli forgatókönyv szimulálása, és módosíthatja az adatokat, a helyi tárolóban levő, amikor az alkalmazás offline állapotban.</span><span class="sxs-lookup"><span data-stu-id="d7199-149">Later, simulate an offline scenario and modify the data in the local store while the app is offline.</span></span>

## <a name="optional-test-the-sync-behavior"></a><span data-ttu-id="d7199-150">(Választható) A szinkronizálás viselkedés tesztelése</span><span class="sxs-lookup"><span data-stu-id="d7199-150">(Optional) Test the sync behavior</span></span>
<span data-ttu-id="d7199-151">Ebben a szakaszban módosítsa az ügyfél-projektet egy kapcsolat nélküli forgatókönyv szimulálása a háttérkiszolgáló URL-címének érvénytelen az alkalmazás használatával.</span><span class="sxs-lookup"><span data-stu-id="d7199-151">In this section, you modify the client project to simulate an offline scenario by using an invalid application URL for your backend.</span></span> <span data-ttu-id="d7199-152">Amikor hozzá, vagy módosítsa az elemeket, ezeket a módosításokat a helyi tárolóban levő tranzakció kimenetelére várva, de nem lett szinkronizálva a háttérrendszer adattárolóhoz mindaddig, amíg a kapcsolat helyreáll.</span><span class="sxs-lookup"><span data-stu-id="d7199-152">When you add or change data items, these changes are held in the local store, but are not synced to the backend data store until the connection is re-established.</span></span>

1. <span data-ttu-id="d7199-153">A Solution Explorerben nyissa meg a index.js projektfájlt, és módosítsa az alkalmazás URL-címet mutasson az egy URL-cím érvénytelen, például a következő kódot:</span><span class="sxs-lookup"><span data-stu-id="d7199-153">In the Solution Explorer, open the index.js project file and change the application URL to point to  an invalid URL, like the following code:</span></span>

        client = new WindowsAzure.MobileServiceClient('http://yourmobileapp.azurewebsites.net-fail');

2. <span data-ttu-id="d7199-154">Index.html, frissítse a kriptográfiai Szolgáltató `<meta>` az azonos érvénytelen URL-CÍMMEL rendelkező elemet.</span><span class="sxs-lookup"><span data-stu-id="d7199-154">In index.html, update the CSP `<meta>` element with the same invalid URL.</span></span>

        <meta http-equiv="Content-Security-Policy" content="default-src 'self' data: gap: http://yourmobileapp.azurewebsites.net-fail; style-src 'self'; media-src *">

3. <span data-ttu-id="d7199-155">Hozza létre, és futtassa az ügyfél-alkalmazást, és figyelje meg, hogy egy kivételt a konzol kerül, amikor az alkalmazás kísérel meg szinkronizálni a háttéralkalmazással való bejelentkezés után.</span><span class="sxs-lookup"><span data-stu-id="d7199-155">Build and run the client app and notice that an exception is logged in the console when the app attempts to sync with the backend after login.</span></span> <span data-ttu-id="d7199-156">Hozzáadhat új elemek csak a helyi tárolóban levő létezik, csak azokat a mobil-háttéralkalmazást vannak leküldve.</span><span class="sxs-lookup"><span data-stu-id="d7199-156">Any new items you add exist only in the local store until they are pushed to the mobile backend.</span></span> <span data-ttu-id="d7199-157">Az ügyfélalkalmazás úgy viselkedik, mintha a háttérrendszer van csatlakoztatva.</span><span class="sxs-lookup"><span data-stu-id="d7199-157">The client app behaves as if it is connected to the backend.</span></span>

4. <span data-ttu-id="d7199-158">Zárja be az alkalmazást, és indítsa újra, hogy ellenőrizze, hogy a helyi tárolójába őrződnek meg a létrehozott új elemeket.</span><span class="sxs-lookup"><span data-stu-id="d7199-158">Close the app and restart it to verify that the new items you created are persisted to the local store.</span></span>

5. <span data-ttu-id="d7199-159">(Választható) Visual Studio használatával az Azure SQL Database tábla, hogy a háttér-adatbázis adatai nem változott meg.</span><span class="sxs-lookup"><span data-stu-id="d7199-159">(Optional) Use Visual Studio to view your Azure SQL Database table to see that the data in the backend database has not changed.</span></span>

    <span data-ttu-id="d7199-160">A Visual Studióban nyissa meg a **Server Explorer**.</span><span class="sxs-lookup"><span data-stu-id="d7199-160">In Visual Studio, open **Server Explorer**.</span></span> <span data-ttu-id="d7199-161">Keresse meg az adatbázist a **Azure**->**SQL-adatbázisok**.</span><span class="sxs-lookup"><span data-stu-id="d7199-161">Navigate to your database in **Azure**->**SQL Databases**.</span></span> <span data-ttu-id="d7199-162">Kattintson a jobb gombbal az adatbázist, és válassza ki **nyissa meg az SQL Server Object Explorerben**.</span><span class="sxs-lookup"><span data-stu-id="d7199-162">Right-click your database and select **Open in SQL Server Object Explorer**.</span></span> <span data-ttu-id="d7199-163">Most már megkeresheti az SQL-adatbázistáblában szereplő és annak tartalmát.</span><span class="sxs-lookup"><span data-stu-id="d7199-163">Now you can browse to your SQL database table and its contents.</span></span>

## <a name="optional-test-the-reconnection-to-your-mobile-backend"></a><span data-ttu-id="d7199-164">(Választható) Az újracsatlakozás a mobil-háttéralkalmazást történő tesztelése</span><span class="sxs-lookup"><span data-stu-id="d7199-164">(Optional) Test the reconnection to your mobile backend</span></span>

<span data-ttu-id="d7199-165">Ebben a szakaszban az alkalmazásnak, hogy a mobil-háttéralkalmazást, az alkalmazáshoz, és az online állapot vissza hamarosan szimulálja újra.</span><span class="sxs-lookup"><span data-stu-id="d7199-165">In this section, you reconnect the app to the mobile backend, which simulates the app coming back to an online state.</span></span> <span data-ttu-id="d7199-166">Jelentkezzen be, amikor adatok szinkronizálva van a mobil-háttéralkalmazást.</span><span class="sxs-lookup"><span data-stu-id="d7199-166">When you log in, data is synced to your mobile backend.</span></span>

1. <span data-ttu-id="d7199-167">Nyissa meg újra a index.js, és állítsa vissza az alkalmazás URL-címet.</span><span class="sxs-lookup"><span data-stu-id="d7199-167">Reopen index.js and restore the application URL.</span></span>
2. <span data-ttu-id="d7199-168">Nyissa meg újra a index.html, és javítsa ki az alkalmazás URL-címet a kriptográfiai Szolgáltató `<meta>` elemet.</span><span class="sxs-lookup"><span data-stu-id="d7199-168">Reopen index.html and correct the application URL in the CSP `<meta>` element.</span></span>
3. <span data-ttu-id="d7199-169">Építse újra, és futtassa az ügyfél-alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="d7199-169">Rebuild and run the client app.</span></span> <span data-ttu-id="d7199-170">Az alkalmazás megpróbálja szinkronizálni a mobil-háttéralkalmazás bejelentkezés után.</span><span class="sxs-lookup"><span data-stu-id="d7199-170">The app attempts to sync with the mobile app backend after login.</span></span> <span data-ttu-id="d7199-171">Győződjön meg arról, hogy nem tartalmaznak kivételeket a hibakereső konzol vannak bejelentkezve.</span><span class="sxs-lookup"><span data-stu-id="d7199-171">Verify that no exceptions are logged in the debug console.</span></span>
4. <span data-ttu-id="d7199-172">(Választható) Az SQL Server Object Explorer vagy egy REST-eszköz, például Fiddler használatával frissített adatok megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="d7199-172">(Optional) View the updated data using either SQL Server Object Explorer or a REST tool like Fiddler.</span></span> <span data-ttu-id="d7199-173">Figyelje meg, az adatok szinkronizálása a háttérbeli adatbázis és a helyi tárolójába.</span><span class="sxs-lookup"><span data-stu-id="d7199-173">Notice the data has been synchronized between the backend database and the local store.</span></span>

    <span data-ttu-id="d7199-174">Figyelje meg, az adatok az adatbázis és a helyi tárolójába lett szinkronizálva, és az alkalmazás forráshoz hozzáadott elemeket tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="d7199-174">Notice the data has been synchronized between the database and the local store and contains the items you added while your app was disconnected.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d7199-175">További források</span><span class="sxs-lookup"><span data-stu-id="d7199-175">Additional resources</span></span>
* <span data-ttu-id="d7199-176">[az Azure Mobile Apps Offline adatszinkronizálás]</span><span class="sxs-lookup"><span data-stu-id="d7199-176">[Offline Data Sync in Azure Mobile Apps]</span></span>
* <span data-ttu-id="d7199-177">[Visual Studio Tools for Apache Cordova]</span><span class="sxs-lookup"><span data-stu-id="d7199-177">[Visual Studio Tools for Apache Cordova]</span></span>

## <a name="next-steps"></a><span data-ttu-id="d7199-178">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d7199-178">Next steps</span></span>
* <span data-ttu-id="d7199-179">Tekintse át a fejlettebb, kapcsolat nélküli szinkronizálás szolgáltatások, például az ütközések feloldása a [kapcsolat nélküli szinkronizálás minta]</span><span class="sxs-lookup"><span data-stu-id="d7199-179">Review more advanced offline sync features such as conflict resolution in the [offline sync sample]</span></span>
* <span data-ttu-id="d7199-180">Tekintse át a kapcsolat nélküli szinkronizálás API-hivatkozás a [API-JÁNAK dokumentációja](https://azure.github.io/azure-mobile-apps-js-client).</span><span class="sxs-lookup"><span data-stu-id="d7199-180">Review the offline sync API reference in the [API documentation](https://azure.github.io/azure-mobile-apps-js-client).</span></span>

<!-- ##Summary -->

<!-- Images -->

<!-- URLs. -->
<span data-ttu-id="d7199-181">[Apache Cordova gyors üzembe helyezési]: app-service-mobile-cordova-get-started.md</span><span class="sxs-lookup"><span data-stu-id="d7199-181">[Apache Cordova quick start]: app-service-mobile-cordova-get-started.md</span></span>
<span data-ttu-id="d7199-182">[kapcsolat nélküli szinkronizálás minta]: https://github.com/Azure-Samples/app-service-mobile-cordova-client-conflict-handling</span><span class="sxs-lookup"><span data-stu-id="d7199-182">[offline sync sample]: https://github.com/Azure-Samples/app-service-mobile-cordova-client-conflict-handling</span></span>
<span data-ttu-id="d7199-183">[az Azure Mobile Apps Offline adatszinkronizálás]: app-service-mobile-offline-data-sync.md</span><span class="sxs-lookup"><span data-stu-id="d7199-183">[Offline Data Sync in Azure Mobile Apps]: app-service-mobile-offline-data-sync.md</span></span>
[Cloud Cover: Offline Sync in Azure Mobile Services]: http://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri
[Adding Authentication]: app-service-mobile-cordova-get-started-users.md
[authentication]: app-service-mobile-cordova-get-started-users.md
[Work with the .NET backend server SDK for Azure Mobile Apps]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Visual Studio Community 2015]: http://www.visualstudio.com/
<span data-ttu-id="d7199-184">[Visual Studio Tools for Apache Cordova]: https://www.visualstudio.com/en-us/features/cordova-vs.aspx</span><span class="sxs-lookup"><span data-stu-id="d7199-184">[Visual Studio Tools for Apache Cordova]: https://www.visualstudio.com/en-us/features/cordova-vs.aspx</span></span>
[Apache Cordova SDK]: app-service-mobile-cordova-how-to-use-client-library.md
[ASP.NET Server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Node.js Server SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md
