---
title: "az Azure Mobile Apps (Cordova) kapcsolat nélküli szinkronizálásának aaaEnable |} Microsoft Docs"
description: "Megtudhatja, hogyan toouse App Service Mobile Apps toocache és a szinkronizálási kapcsolat nélküli adatok a Cordova-alkalmazás"
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
ms.openlocfilehash: 4e6ae96c3d96dac8ebb3749354b83a04686831b7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="enable-offline-sync-for-your-cordova-mobile-app"></a><span data-ttu-id="f69d0-103">A mobil Cordova-alkalmazás kapcsolat nélküli szinkronizálásának engedélyezése</span><span class="sxs-lookup"><span data-stu-id="f69d0-103">Enable offline sync for your Cordova mobile app</span></span>
[!INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

<span data-ttu-id="f69d0-104">Ez az oktatóanyag az Azure Mobile Apps hello kapcsolat nélküli szinkronizálás szolgáltatása a Cordova vezet be.</span><span class="sxs-lookup"><span data-stu-id="f69d0-104">This tutorial introduces hello offline sync feature of Azure Mobile Apps for Cordova.</span></span> <span data-ttu-id="f69d0-105">Kapcsolat nélküli szinkronizálás lehetővé teszi, hogy a befejező felhasználóknak toointeract mobilalkalmazást&mdash;megtekintését, hozzáadását és módosítását adatok&mdash;akkor is, ha nincs hálózati kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="f69d0-105">Offline sync allows end users toointeract with a mobile app&mdash;viewing, adding, or modifying data&mdash;even when there is no network connection.</span></span> <span data-ttu-id="f69d0-106">Változások a helyi adatbázisban tárolódnak.</span><span class="sxs-lookup"><span data-stu-id="f69d0-106">Changes are stored in a local database.</span></span>  <span data-ttu-id="f69d0-107">Hello eszköz újra online állapotba kerül, ha ezek a változások szinkronizálása megtörtént-e távoli hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="f69d0-107">Once hello device is back online, these changes are synced with hello remote service.</span></span>

<span data-ttu-id="f69d0-108">Ez az oktatóanyag alapul hello Cordova gyors üzembe helyezés megoldás a Mobile Apps befejezésekor létrehozandó hello oktatóanyag [Apache Cordova gyors üzembe helyezési].</span><span class="sxs-lookup"><span data-stu-id="f69d0-108">This tutorial is based on hello Cordova quickstart solution for Mobile Apps that you create when you complete hello tutorial [Apache Cordova quick start].</span></span> <span data-ttu-id="f69d0-109">Ebben az oktatóanyagban hello gyors üzembe helyezés megoldás tooadd offline funkciók az Azure Mobile Apps frissíti.</span><span class="sxs-lookup"><span data-stu-id="f69d0-109">In this tutorial, you update hello quickstart solution tooadd offline features of Azure Mobile Apps.</span></span>  <span data-ttu-id="f69d0-110">A Microsoft hello offline tartozó kódot hello alkalmazásban is jelöljön ki.</span><span class="sxs-lookup"><span data-stu-id="f69d0-110">We also highlight hello offline-specific code in hello app.</span></span>

<span data-ttu-id="f69d0-111">További információ az hello kapcsolat nélküli szinkronizálás szolgáltatást, toolearn hello témakörben találhatók [az Azure Mobile Apps Offline adatszinkronizálás].</span><span class="sxs-lookup"><span data-stu-id="f69d0-111">toolearn more about hello offline sync feature, see hello topic [Offline Data Sync in Azure Mobile Apps].</span></span> <span data-ttu-id="f69d0-112">API-használati részletekért lásd: hello [API-JÁNAK dokumentációja](https://azure.github.io/azure-mobile-apps-js-client).</span><span class="sxs-lookup"><span data-stu-id="f69d0-112">For details of API usage, see hello [API documentation](https://azure.github.io/azure-mobile-apps-js-client).</span></span>

## <a name="add-offline-sync-toohello-quickstart-solution"></a><span data-ttu-id="f69d0-113">Kapcsolat nélküli szinkronizálás toohello gyors üzembe helyezés megoldás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="f69d0-113">Add offline sync toohello quickstart solution</span></span>
<span data-ttu-id="f69d0-114">hello kapcsolat nélküli szinkronizálás kód toohello alkalmazás hozzá kell adni.</span><span class="sxs-lookup"><span data-stu-id="f69d0-114">hello offline sync code must be added toohello app.</span></span> <span data-ttu-id="f69d0-115">Kapcsolat nélküli szinkronizálás szükséges hello cordova-sqlite-tároló beépülő modul, amely automatikusan bekerül tooyour app amikor hello Azure Mobile Apps beépülő modul hello projekt tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="f69d0-115">Offline sync requires hello cordova-sqlite-storage plugin, which automatically gets added tooyour app when hello Azure Mobile Apps plugin is included in hello project.</span></span> <span data-ttu-id="f69d0-116">hello Gyorsútmutató-projekt tartalmazza a beépülő modulok mindegyikét.</span><span class="sxs-lookup"><span data-stu-id="f69d0-116">hello Quickstart project includes both of these plugins.</span></span>

1. <span data-ttu-id="f69d0-117">A Visual Studio Solution Explorerben nyissa meg a index.js, és cserélje le a következő kód hello</span><span class="sxs-lookup"><span data-stu-id="f69d0-117">In Visual Studio's Solution Explorer, open index.js and replace hello following code</span></span>

        var client,            // Connection toohello Azure Mobile App backend
           todoItemTable;      // Reference tooa table endpoint on backend

    <span data-ttu-id="f69d0-118">Ezzel a kóddal:</span><span class="sxs-lookup"><span data-stu-id="f69d0-118">with this code:</span></span>

        var client,            // Connection toohello Azure Mobile App backend
           todoItemTable,      // Reference tooa table endpoint on backend
           syncContext;        // Reference toooffline data sync context

2. <span data-ttu-id="f69d0-119">Ezután cserélje le a következő kód hello:</span><span class="sxs-lookup"><span data-stu-id="f69d0-119">Next, replace hello following code:</span></span>

        client = new WindowsAzure.MobileServiceClient('http://yourmobileapp.azurewebsites.net');

    <span data-ttu-id="f69d0-120">Ezzel a kóddal:</span><span class="sxs-lookup"><span data-stu-id="f69d0-120">with this code:</span></span>

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

        // Get hello sync context from hello client
        syncContext = client.getSyncContext();

    <span data-ttu-id="f69d0-121">hello előző kód kiegészítéseket hello helyi tároló inicializálása és adjon meg egy helyi táblát, amely megfelel a hello oszlop értékeit használja az Azure-ban háttér.</span><span class="sxs-lookup"><span data-stu-id="f69d0-121">hello preceding code additions initialize hello local store and define a local table that matches hello column values used in your Azure back end.</span></span> <span data-ttu-id="f69d0-122">(Nincs szükség tooinclude összes oszlop értékének ezt a kódot.)  Hello `version` mező hello mobil-háttéralkalmazást tartja fenn, és ütközésfeloldás közben használható.</span><span class="sxs-lookup"><span data-stu-id="f69d0-122">(You don't need tooinclude all column values in this code.)  hello `version` field is maintained by hello mobile backend and is used for conflict resolution.</span></span>

    <span data-ttu-id="f69d0-123">Egy hivatkozási toohello szinkronizálási környezetet meghívásával kap **getSyncContext**.</span><span class="sxs-lookup"><span data-stu-id="f69d0-123">You get a reference toohello sync context by calling **getSyncContext**.</span></span> <span data-ttu-id="f69d0-124">hello szinkronizálási környezetet segíti követésével és kérdez le a módosításokat minden olyan táblát, egy ügyfélalkalmazás módosította, ha a tábla közötti megőrzése érdekében `.push()` nevezik.</span><span class="sxs-lookup"><span data-stu-id="f69d0-124">hello sync context helps preserve table relationships by tracking and pushing changes in all tables a client app has modified when `.push()` is called.</span></span>

3. <span data-ttu-id="f69d0-125">Frissítés hello alkalmazás URL-cím tooyour mobilalkalmazás alkalmazás URL-címe.</span><span class="sxs-lookup"><span data-stu-id="f69d0-125">Update hello application URL tooyour Mobile App application URL.</span></span>

4. <span data-ttu-id="f69d0-126">Ezután cserélje le ezt a kódot:</span><span class="sxs-lookup"><span data-stu-id="f69d0-126">Next, replace this code:</span></span>

        todoItemTable = client.getTable('todoitem'); // todoitem is hello table name

    <span data-ttu-id="f69d0-127">Ezzel a kóddal:</span><span class="sxs-lookup"><span data-stu-id="f69d0-127">with this code:</span></span>

        // Initialize hello sync context with hello store
        syncContext.initialize(store).then(function () {

        // Get hello local table reference.
        todoItemTable = client.getSyncTable('todoitem');

        syncContext.pushHandler = {
            onConflict: function (pushError) {
                // Handle hello conflict.
                console.log("Sync conflict! " + pushError.getError().message);
                // Update failed, revert tooserver's copy.
                pushError.cancelAndDiscard();
              },
              onError: function (pushError) {
                  // Handle hello error
                  // In hello simulated offline state, you get "Sync error! Unexpected connection failure."
                  console.log("Sync error! " + pushError.getError().message);
              }
        };

        // Call a function tooperform hello actual sync
        syncBackend();

        // Refresh hello todoItems
        refreshDisplay();

        // Wire up hello UI Event Handler for hello Add Item
        $('#add-item').submit(addItemHandler);
        $('#refresh').on('click', refreshDisplay);

    <span data-ttu-id="f69d0-128">hello előző kód inicializálja hello szinkronizálási környezetet és az majd (helyett getTable) getSyncTable tooget hivatkozási toohello helyi táblában.</span><span class="sxs-lookup"><span data-stu-id="f69d0-128">hello preceding code initializes hello sync context and then calls getSyncTable (instead of getTable) tooget a reference toohello local table.</span></span>

    <span data-ttu-id="f69d0-129">A kód által használt hello helyi adatbázis összes létrehozása, olvasása, frissítése és Törlés (CRUD) tábla műveletek.</span><span class="sxs-lookup"><span data-stu-id="f69d0-129">This code uses hello local database for all create, read, update, and delete (CRUD) table operations.</span></span>

    <span data-ttu-id="f69d0-130">Ez a minta egyszerű hibakezelési szinkronizálási ütközések hajt végre.</span><span class="sxs-lookup"><span data-stu-id="f69d0-130">This sample performs simple error handling on sync conflicts.</span></span> <span data-ttu-id="f69d0-131">Egy valós alkalmazás kezelni kellene hello hálózati feltételek mellett, server ütközések és mások számára például különböző hibákat.</span><span class="sxs-lookup"><span data-stu-id="f69d0-131">A real application would handle hello various errors like network conditions, server conflicts, and others.</span></span> <span data-ttu-id="f69d0-132">Kód, tekintse meg a hello [kapcsolat nélküli szinkronizálás minta].</span><span class="sxs-lookup"><span data-stu-id="f69d0-132">For code examples, see hello [offline sync sample].</span></span>

5. <span data-ttu-id="f69d0-133">Ezután adja hozzá a függvény tooperform hello tényleges szinkronizálás.</span><span class="sxs-lookup"><span data-stu-id="f69d0-133">Next, add this function tooperform hello actual sync.</span></span>

        function syncBackend() {

          // Sync local store tooAzure table when app loads, or when login complete.
          syncContext.push().then(function () {
              // Push completed

          });

          // Pull items from hello Azure table after syncing tooAzure.
          syncContext.pull(new WindowsAzure.Query('todoitem'));
        }

    <span data-ttu-id="f69d0-134">Úgy dönt, amikor toopush változik meghívásával toohello Mobile Apps-háttéralkalmazás **syncContext.push()**.</span><span class="sxs-lookup"><span data-stu-id="f69d0-134">You decide when toopush changes toohello Mobile App backend by calling **syncContext.push()**.</span></span> <span data-ttu-id="f69d0-135">Például hívhatja **syncBackend** a gomb esemény kezelő kapcsolt tooa szinkronizálás gomb.</span><span class="sxs-lookup"><span data-stu-id="f69d0-135">For example, you could call **syncBackend** in a button event handler tied tooa sync button.</span></span>

## <a name="offline-sync-considerations"></a><span data-ttu-id="f69d0-136">Kapcsolat nélküli szinkronizálás kapcsolatos szempontok</span><span class="sxs-lookup"><span data-stu-id="f69d0-136">Offline sync considerations</span></span>

<span data-ttu-id="f69d0-137">Hello mintában hello **leküldéses** metódusában **syncContext** alkalmazás következő indításakor hello visszahívási függvény bejelentkezési azonosító csak nevezik.</span><span class="sxs-lookup"><span data-stu-id="f69d0-137">In hello sample, hello **push** method of **syncContext** is only called on app startup in hello callback function for login.</span></span>  <span data-ttu-id="f69d0-138">Egy valós alkalmazás is teheti a szinkronizálási funkció manuálisan elindítva, vagy ha hello hálózati állapota megváltozik..</span><span class="sxs-lookup"><span data-stu-id="f69d0-138">In a real-world application, you could also make this sync functionality triggered manually or when hello network state changes.</span></span>

<span data-ttu-id="f69d0-139">Elleni táblának függőben lévő lekérési végrehajtásakor helyi frissítések követi nyomon, hogy a pull művelet automatikusan eseményindítók hello összefüggésben leküldéses.</span><span class="sxs-lookup"><span data-stu-id="f69d0-139">When a pull is executed against a table that has pending local updates tracked by hello context, that pull operation automatically triggers a push.</span></span> <span data-ttu-id="f69d0-140">Ha frissíteni, hozzáadása, és ez a példa elemek befejezése, akkor kihagyhatja hello explicit **leküldéses** hívható, mert elképzelhető, hogy a redundáns.</span><span class="sxs-lookup"><span data-stu-id="f69d0-140">When refreshing, adding, and completing items in this sample, you can omit hello explicit **push** call, since it may be redundant.</span></span>

<span data-ttu-id="f69d0-141">A megadott hello kódban, a rendszer megkérdezi a hello távoli todoItem tábla összes adatát, de azt is lehetséges toofilter rekordok úgy, hogy a lekérdezés azonosítóját és a lekérdezés túl**leküldéses**.</span><span class="sxs-lookup"><span data-stu-id="f69d0-141">In hello provided code, all records in hello remote todoItem table are queried, but it is also possible toofilter records by passing a query id and query too**push**.</span></span> <span data-ttu-id="f69d0-142">További információkért lásd: hello szakasz *növekményes szinkronizálás* a [az Azure Mobile Apps Offline adatszinkronizálás].</span><span class="sxs-lookup"><span data-stu-id="f69d0-142">For more information, see hello section *Incremental Sync* in [Offline Data Sync in Azure Mobile Apps].</span></span>

## <a name="optional-disable-authentication"></a><span data-ttu-id="f69d0-143">(Választható) Hitelesítés letiltása</span><span class="sxs-lookup"><span data-stu-id="f69d0-143">(Optional) Disable authentication</span></span>

<span data-ttu-id="f69d0-144">Ha nem szeretné, hogy kapcsolat nélküli szinkronizálás tesztelése előtt hitelesítés tooset, megjegyzéssé hello visszahívási függvény a bejelentkezéshez, de hello kódot hagyja uncommented hello visszahívási függvény.</span><span class="sxs-lookup"><span data-stu-id="f69d0-144">If you don't want tooset up authentication before testing offline sync, comment out hello callback function for login, but leave hello code inside hello callback function uncommented.</span></span>  <span data-ttu-id="f69d0-145">Megjegyzések hello bejelentkezési sorok el, miután hello kódot a következőképpen:</span><span class="sxs-lookup"><span data-stu-id="f69d0-145">After commenting out hello login lines, hello code follows:</span></span>

      // Login toohello service.
      // client.login('twitter')
      //    .then(function () {
        syncContext.initialize(store).then(function () {
          // Leave hello rest of hello code in this callback function  uncommented.
                ...
        });
      // }, handleError);

<span data-ttu-id="f69d0-146">Most hello app szinkronizál hello Azure-háttérrendszernek hello alkalmazás futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="f69d0-146">Now, hello app syncs with hello Azure backend when you run hello app.</span></span>

## <a name="run-hello-client-app"></a><span data-ttu-id="f69d0-147">Hello ügyfél-alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="f69d0-147">Run hello client app</span></span>
<span data-ttu-id="f69d0-148">A kapcsolat nélküli szinkronizálás most engedélyezve van futtathatja hello ügyfélalkalmazás legalább egyszer hello helyi tároló adatbázisának feltöltése minden platformon.</span><span class="sxs-lookup"><span data-stu-id="f69d0-148">With offline sync now enabled, you can run hello client application at least once on each platform to populate hello local store database.</span></span> <span data-ttu-id="f69d0-149">Később egy kapcsolat nélküli forgatókönyv szimulálása és hello adatok hello helyi tárolóban levő módosítható, amíg az alkalmazás hello offline állapotban.</span><span class="sxs-lookup"><span data-stu-id="f69d0-149">Later, simulate an offline scenario and modify hello data in hello local store while hello app is offline.</span></span>

## <a name="optional-test-hello-sync-behavior"></a><span data-ttu-id="f69d0-150">(Választható) Teszt hello szinkronizálási viselkedése</span><span class="sxs-lookup"><span data-stu-id="f69d0-150">(Optional) Test hello sync behavior</span></span>
<span data-ttu-id="f69d0-151">Ebben a szakaszban hello ügyfél projekt toosimulate egy kapcsolat nélküli forgatókönyv egy érvénytelen az alkalmazás URL-CÍMÉT a háttérkiszolgáló használatával módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="f69d0-151">In this section, you modify hello client project toosimulate an offline scenario by using an invalid application URL for your backend.</span></span> <span data-ttu-id="f69d0-152">Hozzáadásakor, vagy módosítsa az elemeket, ezeket a módosításokat a helyi tárolóban levő tartanak, de nem szinkronizált toohello háttér adattár amíg hello kapcsolat helyreáll.</span><span class="sxs-lookup"><span data-stu-id="f69d0-152">When you add or change data items, these changes are held in the local store, but are not synced toohello backend data store until hello connection is re-established.</span></span>

1. <span data-ttu-id="f69d0-153">A Solution Explorer hello nyissa meg hello index.js projekt fájlt, és módosítsa az URL-cím érvénytelen, például a következő kód hello hello alkalmazás URL-cím toopoint:</span><span class="sxs-lookup"><span data-stu-id="f69d0-153">In hello Solution Explorer, open hello index.js project file and change hello application URL toopoint to  an invalid URL, like hello following code:</span></span>

        client = new WindowsAzure.MobileServiceClient('http://yourmobileapp.azurewebsites.net-fail');

2. <span data-ttu-id="f69d0-154">A index.html, frissítse a hello CSP `<meta>` elem hello azonos URL-cím érvénytelen.</span><span class="sxs-lookup"><span data-stu-id="f69d0-154">In index.html, update hello CSP `<meta>` element with hello same invalid URL.</span></span>

        <meta http-equiv="Content-Security-Policy" content="default-src 'self' data: gap: http://yourmobileapp.azurewebsites.net-fail; style-src 'self'; media-src *">

3. <span data-ttu-id="f69d0-155">Hozza létre és hello ügyfélalkalmazás futtatása, és figyelje meg, hogy kivételt hello konzol kerül, amikor hello app kísérli meg a bejelentkezés után hello háttér szinkronizálás.</span><span class="sxs-lookup"><span data-stu-id="f69d0-155">Build and run hello client app and notice that an exception is logged in hello console when hello app attempts to sync with hello backend after login.</span></span> <span data-ttu-id="f69d0-156">Hozzáadhat új elemek található csak hello helyi tárolóban csak akkor vannak leküldött toohello mobil-háttéralkalmazást.</span><span class="sxs-lookup"><span data-stu-id="f69d0-156">Any new items you add exist only in hello local store until they are pushed toohello mobile backend.</span></span> <span data-ttu-id="f69d0-157">hello ügyfélalkalmazás úgy viselkedik, mintha a csatlakoztatott toohello háttér.</span><span class="sxs-lookup"><span data-stu-id="f69d0-157">hello client app behaves as if it is connected toohello backend.</span></span>

4. <span data-ttu-id="f69d0-158">Hello-alkalmazások bezárása, és indítsa újra azt, hogy hello létrehozott új elemek-e a megőrzött toohello helyi tároló tooverify.</span><span class="sxs-lookup"><span data-stu-id="f69d0-158">Close hello app and restart it tooverify that hello new items you created are persisted toohello local store.</span></span>

5. <span data-ttu-id="f69d0-159">(Választható) Visual Studio tooview használja az Azure SQL Database tábla toosee hello adatok hello háttérbeli adatbázis nem módosított.</span><span class="sxs-lookup"><span data-stu-id="f69d0-159">(Optional) Use Visual Studio tooview your Azure SQL Database table toosee that hello data in hello backend database has not changed.</span></span>

    <span data-ttu-id="f69d0-160">A Visual Studióban nyissa meg a **Server Explorer**.</span><span class="sxs-lookup"><span data-stu-id="f69d0-160">In Visual Studio, open **Server Explorer**.</span></span> <span data-ttu-id="f69d0-161">Keresse meg a tooyour adatbázis **Azure**->**SQL-adatbázisok**.</span><span class="sxs-lookup"><span data-stu-id="f69d0-161">Navigate tooyour database in **Azure**->**SQL Databases**.</span></span> <span data-ttu-id="f69d0-162">Kattintson a jobb gombbal az adatbázist, és válassza ki **nyissa meg az SQL Server Object Explorerben**.</span><span class="sxs-lookup"><span data-stu-id="f69d0-162">Right-click your database and select **Open in SQL Server Object Explorer**.</span></span> <span data-ttu-id="f69d0-163">Most már megkeresheti tooyour SQL-adatbázistáblában szereplő és annak tartalmát.</span><span class="sxs-lookup"><span data-stu-id="f69d0-163">Now you can browse tooyour SQL database table and its contents.</span></span>

## <a name="optional-test-hello-reconnection-tooyour-mobile-backend"></a><span data-ttu-id="f69d0-164">(Választható) Hello újracsatlakozás tooyour mobil-háttéralkalmazást tesztelése</span><span class="sxs-lookup"><span data-stu-id="f69d0-164">(Optional) Test hello reconnection tooyour mobile backend</span></span>

<span data-ttu-id="f69d0-165">Ebben a szakaszban újra hello toohello mobil háttéralkalmazás, szimulálja hello alkalmazás hamarosan ismét online állapotba.</span><span class="sxs-lookup"><span data-stu-id="f69d0-165">In this section, you reconnect hello app toohello mobile backend, which simulates hello app coming back to an online state.</span></span> <span data-ttu-id="f69d0-166">Jelentkezzen be, amikor adatokat szinkronizált tooyour mobil-háttéralkalmazást.</span><span class="sxs-lookup"><span data-stu-id="f69d0-166">When you log in, data is synced tooyour mobile backend.</span></span>

1. <span data-ttu-id="f69d0-167">Nyissa meg újra a index.js, és állítsa vissza a hello alkalmazás URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="f69d0-167">Reopen index.js and restore hello application URL.</span></span>
2. <span data-ttu-id="f69d0-168">Nyissa meg újra a index.html, és kijavíthatja az hello alkalmazás URL-címet hello CSP `<meta>` elemet.</span><span class="sxs-lookup"><span data-stu-id="f69d0-168">Reopen index.html and correct hello application URL in hello CSP `<meta>` element.</span></span>
3. <span data-ttu-id="f69d0-169">Építse újra és hello ügyfélalkalmazás futtatása.</span><span class="sxs-lookup"><span data-stu-id="f69d0-169">Rebuild and run hello client app.</span></span> <span data-ttu-id="f69d0-170">hello app bejelentkezés után próbálja meg a mobil-háttéralkalmazás hello toosync.</span><span class="sxs-lookup"><span data-stu-id="f69d0-170">hello app attempts toosync with hello mobile app backend after login.</span></span> <span data-ttu-id="f69d0-171">Győződjön meg arról, hogy nem tartalmaznak kivételeket hello hibakereső konzol vannak bejelentkezve.</span><span class="sxs-lookup"><span data-stu-id="f69d0-171">Verify that no exceptions are logged in hello debug console.</span></span>
4. <span data-ttu-id="f69d0-172">(Választható) Nézet hello a módosított adatokat az SQL Server Object Explorer vagy egy REST-eszköz, például Fiddler használatával.</span><span class="sxs-lookup"><span data-stu-id="f69d0-172">(Optional) View hello updated data using either SQL Server Object Explorer or a REST tool like Fiddler.</span></span> <span data-ttu-id="f69d0-173">Értesítés hello adatok hello háttér adatbázis és a helyi tárolójába hello lett szinkronizálva.</span><span class="sxs-lookup"><span data-stu-id="f69d0-173">Notice hello data has been synchronized between hello backend database and hello local store.</span></span>

    <span data-ttu-id="f69d0-174">Figyelje meg hello adatok hello adatbázis és a helyi tárolójába hello lett szinkronizálva, és az alkalmazás forráshoz hozzáadott hello elemet tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="f69d0-174">Notice hello data has been synchronized between hello database and hello local store and contains hello items you added while your app was disconnected.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f69d0-175">További források</span><span class="sxs-lookup"><span data-stu-id="f69d0-175">Additional resources</span></span>
* <span data-ttu-id="f69d0-176">[az Azure Mobile Apps Offline adatszinkronizálás]</span><span class="sxs-lookup"><span data-stu-id="f69d0-176">[Offline Data Sync in Azure Mobile Apps]</span></span>
* <span data-ttu-id="f69d0-177">[Visual Studio Tools for Apache Cordova]</span><span class="sxs-lookup"><span data-stu-id="f69d0-177">[Visual Studio Tools for Apache Cordova]</span></span>

## <a name="next-steps"></a><span data-ttu-id="f69d0-178">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f69d0-178">Next steps</span></span>
* <span data-ttu-id="f69d0-179">Tekintse át a fejlettebb, kapcsolat nélküli szinkronizálás szolgáltatások, például a hello ütközésének feloldása [kapcsolat nélküli szinkronizálás minta]</span><span class="sxs-lookup"><span data-stu-id="f69d0-179">Review more advanced offline sync features such as conflict resolution in hello [offline sync sample]</span></span>
* <span data-ttu-id="f69d0-180">Tekintse át a hello kapcsolat nélküli szinkronizálás hello API-hivatkozás [API-JÁNAK dokumentációja](https://azure.github.io/azure-mobile-apps-js-client).</span><span class="sxs-lookup"><span data-stu-id="f69d0-180">Review hello offline sync API reference in hello [API documentation](https://azure.github.io/azure-mobile-apps-js-client).</span></span>

<!-- ##Summary -->

<!-- Images -->

<!-- URLs. -->
[Apache Cordova gyors üzembe helyezési]: app-service-mobile-cordova-get-started.md
[kapcsolat nélküli szinkronizálás minta]: https://github.com/Azure-Samples/app-service-mobile-cordova-client-conflict-handling
[az Azure Mobile Apps Offline adatszinkronizálás]: app-service-mobile-offline-data-sync.md
[Cloud Cover: Offline Sync in Azure Mobile Services]: http://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri
[Adding Authentication]: app-service-mobile-cordova-get-started-users.md
[authentication]: app-service-mobile-cordova-get-started-users.md
[Work with hello .NET backend server SDK for Azure Mobile Apps]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Visual Studio Community 2015]: http://www.visualstudio.com/
[Visual Studio Tools for Apache Cordova]: https://www.visualstudio.com/en-us/features/cordova-vs.aspx
[Apache Cordova SDK]: app-service-mobile-cordova-how-to-use-client-library.md
[ASP.NET Server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Node.js Server SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md
