---
title: "A Azure Mobile Apps (Xamarin.Forms) kapcsolat nélküli szinkronizálásának engedélyezése |} Microsoft Docs"
description: "App Service Mobile Apps a Xamarin.Forms-alkalmazás gyorsítótárába, a szinkronizálási kapcsolat nélküli adatainak használata"
documentationcenter: xamarin
author: ggailey777
manager: yochayk
editor: 
services: app-service\mobile
ms.assetid: acf0f874-3ea5-4410-bd22-b0e72140f3b5
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-ios
ms.devlang: dotnet
ms.topic: article
ms.date: 10/04/2016
ms.author: glenga
ms.openlocfilehash: f2bed0a7124517319cc82405c4ab6b4d79aacfe1
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="enable-offline-sync-for-your-xamarinforms-mobile-app"></a><span data-ttu-id="01dda-103">Xamarin.Forms-mobilalkalmazás offline szinkronizálásának engedélyezése</span><span class="sxs-lookup"><span data-stu-id="01dda-103">Enable offline sync for your Xamarin.Forms mobile app</span></span>
[!INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a><span data-ttu-id="01dda-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="01dda-104">Overview</span></span>
<span data-ttu-id="01dda-105">Ez az oktatóanyag a kapcsolat nélküli szinkronizálás szolgáltatást, az Azure Mobile Apps xamarin.Forms vezet be.</span><span class="sxs-lookup"><span data-stu-id="01dda-105">This tutorial introduces the offline sync feature of Azure Mobile Apps for Xamarin.Forms.</span></span> <span data-ttu-id="01dda-106">Kapcsolat nélküli szinkronizálás lehetővé teszi, hogy a végfelhasználók számára, amellyel kommunikálni tud a mobilalkalmazások--megtekintését, hozzáadását és módosítását adatokat –, akkor is, ha nincs hálózati kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="01dda-106">Offline sync allows end users to interact with a mobile app--viewing, adding, or modifying data--even when there is no network connection.</span></span> <span data-ttu-id="01dda-107">Változások a helyi adatbázisban tárolódnak.</span><span class="sxs-lookup"><span data-stu-id="01dda-107">Changes are stored in a local database.</span></span> <span data-ttu-id="01dda-108">Az eszköz újra online állapotba kerül, ha ezek a változások szinkronizálása megtörtént-e a távoli szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="01dda-108">Once the device is back online, these changes are synced with the remote service.</span></span>

<span data-ttu-id="01dda-109">Ez az oktatóanyag a Xamarin.Forms-gyors üzembe helyezés megoldás alapján hoz létre, amikor befejezte az oktatóanyag [Xamarin iOS-alkalmazás létrehozása] Mobile Apps-alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="01dda-109">This tutorial is based on the Xamarin.Forms quickstart solution for Mobile Apps that you create when you complete the tutorial [Create a Xamarin iOS app].</span></span> <span data-ttu-id="01dda-110">A gyors üzembe helyezés megoldást a Xamarin.Forms kapcsolat nélküli szinkronizálás, amelyen engedélyezhető, hogy támogatásához kódját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="01dda-110">The quickstart solution for Xamarin.Forms contains the code to support offline sync, which just needs to be enabled.</span></span> <span data-ttu-id="01dda-111">Ebben az oktatóanyagban az Azure Mobile Apps offline funkciók bekapcsolása a gyors üzembe helyezés megoldás frissítése.</span><span class="sxs-lookup"><span data-stu-id="01dda-111">In this tutorial, you update the quickstart solution to turn on the offline features of Azure Mobile Apps.</span></span> <span data-ttu-id="01dda-112">Azt is kiemelése a kapcsolat nélküli tartozó kódot az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="01dda-112">We also highlight the offline-specific code in the app.</span></span> <span data-ttu-id="01dda-113">Ha nem használja a letöltött gyorsútmutató-megoldás, hozzá kell adnia a hozzáférési adatok bővítménycsomagok a projekthez.</span><span class="sxs-lookup"><span data-stu-id="01dda-113">If you do not use the downloaded quickstart solution, you must add the data access extension packages to your project.</span></span> <span data-ttu-id="01dda-114">Kiszolgáló bővítménycsomagok kapcsolatos további információkért lásd: [használható a .NET-háttérrendszer server SDK az Azure Mobile Apps a][1].</span><span class="sxs-lookup"><span data-stu-id="01dda-114">For more information about server extension packages, see [Work with the .NET backend server SDK for Azure Mobile Apps][1].</span></span>

<span data-ttu-id="01dda-115">A kapcsolat nélküli szinkronizálás szolgáltatással kapcsolatos további tudnivalókért lásd a témakör [az Azure Mobile Apps Offline adatszinkronizálás][2].</span><span class="sxs-lookup"><span data-stu-id="01dda-115">To learn more about the offline sync feature, see the topic [Offline Data Sync in Azure Mobile Apps][2].</span></span>

## <a name="enable-offline-sync-functionality-in-the-quickstart-solution"></a><span data-ttu-id="01dda-116">A gyors üzembe helyezés megoldásban kapcsolat nélküli szinkronizálásának engedélyezése</span><span class="sxs-lookup"><span data-stu-id="01dda-116">Enable offline sync functionality in the quickstart solution</span></span>
<span data-ttu-id="01dda-117">A kapcsolat nélküli szinkronizálás kód szerepel a projekt C# előfeldolgozó irányelvek segítségével.</span><span class="sxs-lookup"><span data-stu-id="01dda-117">The offline sync code is included in the project by using C# preprocessor directives.</span></span> <span data-ttu-id="01dda-118">Ha a **OFFLINE\_SZINKRONIZÁLÁSI\_engedélyezve** szimbólum van megadva, a kód elérési utak a build szerepelnek.</span><span class="sxs-lookup"><span data-stu-id="01dda-118">When the **OFFLINE\_SYNC\_ENABLED** symbol is defined, these code paths are included in the build.</span></span> <span data-ttu-id="01dda-119">Windows-alkalmazások esetén is telepítenie kell az SQLite platform.</span><span class="sxs-lookup"><span data-stu-id="01dda-119">For Windows apps, you must also install the SQLite platform.</span></span>

1. <span data-ttu-id="01dda-120">A Visual Studióban, kattintson a jobb gombbal a megoldás > **NuGet-csomagok kezelése megoldáshoz...** , majd keresse meg, és telepítse a **Microsoft.Azure.Mobile.Client.SQLiteStore** NuGet-csomagot az összes projektet a megoldásban.</span><span class="sxs-lookup"><span data-stu-id="01dda-120">In Visual Studio, right-click the solution > **Manage NuGet Packages for Solution...**, then search for and install the **Microsoft.Azure.Mobile.Client.SQLiteStore** NuGet package for all projects in the solution.</span></span>
2. <span data-ttu-id="01dda-121">A Solution Explorerben nyissa meg a TodoItemManager.cs fájlt a projektből az **hordozható** a neve, amely hordozható Class Library projektet, majd állítsa vissza a következő előfeldolgozó irányelv:</span><span class="sxs-lookup"><span data-stu-id="01dda-121">In the Solution Explorer, open the TodoItemManager.cs file from the project with **Portable** in the name, which is Portable Class Library project, then uncomment the following preprocessor directive:</span></span>

        #define OFFLINE_SYNC_ENABLED
3. <span data-ttu-id="01dda-122">(Választható) Windows-eszközök támogatása érdekében telepítse a következő SQLite futtatókörnyezetek egyike:</span><span class="sxs-lookup"><span data-stu-id="01dda-122">(Optional) To support Windows devices, install one of the following SQLite runtime packages:</span></span>

   * <span data-ttu-id="01dda-123">**Windows 8.1 futásidejű:** telepítése [a Windows 8.1 SQLite][3].</span><span class="sxs-lookup"><span data-stu-id="01dda-123">**Windows 8.1 Runtime:** Install [SQLite for Windows 8.1][3].</span></span>
   * <span data-ttu-id="01dda-124">**Windows Phone 8.1:** telepítése [a Windows Phone 8.1 SQLite][4].</span><span class="sxs-lookup"><span data-stu-id="01dda-124">**Windows Phone 8.1:** Install [SQLite for Windows Phone 8.1][4].</span></span>
   * <span data-ttu-id="01dda-125">**Az univerzális Windows Platform** telepítése [az univerzális Windows Universal az SQLite][5].</span><span class="sxs-lookup"><span data-stu-id="01dda-125">**Universal Windows Platform** Install [SQLite for the Universal Windows Universal][5].</span></span>

     <span data-ttu-id="01dda-126">Bár a gyors üzembe helyezés nem tartalmaz egy univerzális Windows-projektet, a univerzális Windows platform Xamarin Forms használata támogatott.</span><span class="sxs-lookup"><span data-stu-id="01dda-126">Although the quickstart does not contain a Universal Windows project, the Universal Windows platform is supported with Xamarin Forms.</span></span>
4. <span data-ttu-id="01dda-127">(Választható) Minden Windows-alkalmazás projekt, kattintson a jobb egérgombbal **hivatkozások** > **hivatkozás hozzáadása...** , bontsa ki a **Windows** mappa > **bővítmények**.</span><span class="sxs-lookup"><span data-stu-id="01dda-127">(Optional) In each Windows app project, right-click **References** > **Add Reference...**, expand the **Windows** folder > **Extensions**.</span></span>
    <span data-ttu-id="01dda-128">Engedélyezze a megfelelő **SQLite for Windows** SDK, valamint a **Visual C++ 2013 futásidejű Windows** SDK.</span><span class="sxs-lookup"><span data-stu-id="01dda-128">Enable the appropriate **SQLite for Windows** SDK along with the **Visual C++ 2013 Runtime for Windows** SDK.</span></span>
    <span data-ttu-id="01dda-129">Az SQLite SDK nevek függően eltérőek az egyes Windows-platformra.</span><span class="sxs-lookup"><span data-stu-id="01dda-129">The SQLite SDK names vary slightly with each Windows platform.</span></span>

## <a name="review-the-client-sync-code"></a><span data-ttu-id="01dda-130">Tekintse át az Ügyfélkód szinkronizálása</span><span class="sxs-lookup"><span data-stu-id="01dda-130">Review the client sync code</span></span>
<span data-ttu-id="01dda-131">Ez az oktatóanyag kódot már tartalmát rövid áttekintést a `#if OFFLINE_SYNC_ENABLED` irányelvek.</span><span class="sxs-lookup"><span data-stu-id="01dda-131">Here is a brief overview of what is already included in the tutorial code inside the `#if OFFLINE_SYNC_ENABLED` directives.</span></span> <span data-ttu-id="01dda-132">A kapcsolat nélküli szinkronizálásának a hordozható osztály Hordozhatóosztálytár-projektjének TodoItemManager.cs projekt fájlban van.</span><span class="sxs-lookup"><span data-stu-id="01dda-132">The offline sync functionality is in the TodoItemManager.cs project file in the Portable Class Library project.</span></span> <span data-ttu-id="01dda-133">A szolgáltatás elméleti áttekintését lásd: [az Azure Mobile Apps Offline adatszinkronizálás][2].</span><span class="sxs-lookup"><span data-stu-id="01dda-133">For a conceptual overview of the feature, see [Offline Data Sync in Azure Mobile Apps][2].</span></span>

* <span data-ttu-id="01dda-134">Mielőtt bármely tábla művelet végrehajtható, a helyi tárolójába inicializálni kell.</span><span class="sxs-lookup"><span data-stu-id="01dda-134">Before any table operations can be performed, the local store must be initialized.</span></span> <span data-ttu-id="01dda-135">A helyi adatbázis a inicializálva van a **TodoItemManager** osztálykonstruktor az alábbi kód használatával:</span><span class="sxs-lookup"><span data-stu-id="01dda-135">The local store database is initialized in the **TodoItemManager** class constructor by using the following code:</span></span>

        var store = new MobileServiceSQLiteStore(OfflineDbPath);
        store.DefineTable<TodoItem>();

        //Initializes the SyncContext using the default IMobileServiceSyncHandler.
        this.client.SyncContext.InitializeAsync(store);

        this.todoTable = client.GetSyncTable<TodoItem>();

    <span data-ttu-id="01dda-136">Ez a kód létrehoz egy új helyi SQLite adatbázis használata a **MobileServiceSQLiteStore** osztály.</span><span class="sxs-lookup"><span data-stu-id="01dda-136">This code creates a new local SQLite database using the **MobileServiceSQLiteStore** class.</span></span>

    <span data-ttu-id="01dda-137">A **DefineTable** metódus táblázatot hoz létre a helyi tárolóban, amely megfelel a megadott típusú mezőket.</span><span class="sxs-lookup"><span data-stu-id="01dda-137">The **DefineTable** method creates a table in the local store that matches the fields in the provided type.</span></span>  <span data-ttu-id="01dda-138">A típus nem kell megadnia a távoli adatbázisban lévő összes oszlopot.</span><span class="sxs-lookup"><span data-stu-id="01dda-138">The type doesn't have to include all the columns that are in the remote database.</span></span> <span data-ttu-id="01dda-139">Akkor lehet az oszlopok olyan részhalmazán tárolásához.</span><span class="sxs-lookup"><span data-stu-id="01dda-139">It is possible to store a subset of columns.</span></span>
* <span data-ttu-id="01dda-140">A **todoTable** mezőjét **TodoItemManager** van egy **IMobileServiceSyncTable** helyett típusú **IMobileServiceTable**.</span><span class="sxs-lookup"><span data-stu-id="01dda-140">The **todoTable** field in **TodoItemManager** is an **IMobileServiceSyncTable** type instead of **IMobileServiceTable**.</span></span> <span data-ttu-id="01dda-141">A osztály használja az összes helyi adatbázis létrehozása, olvasása, frissítése és törlése (CRUD) tábla műveletek.</span><span class="sxs-lookup"><span data-stu-id="01dda-141">This class uses the local database for all create, read, update, and delete (CRUD) table operations.</span></span> <span data-ttu-id="01dda-142">Úgy dönt, ha ezeket a módosításokat a Mobile Apps-háttéralkalmazás meghívásával leküldve vannak **PushAsync** a a **IMobileServiceSyncContext**.</span><span class="sxs-lookup"><span data-stu-id="01dda-142">You decide when those changes are pushed to the Mobile App backend by calling **PushAsync** on the **IMobileServiceSyncContext**.</span></span> <span data-ttu-id="01dda-143">A szinkronizálási környezetet segíti követésével és kérdez le a módosításokat minden olyan táblát, egy ügyfélalkalmazás módosította, ha a tábla közötti megőrzése érdekében **PushAsync** nevezik.</span><span class="sxs-lookup"><span data-stu-id="01dda-143">The sync context helps preserve table relationships by tracking and pushing changes in all tables a client app has modified when **PushAsync** is called.</span></span>

    <span data-ttu-id="01dda-144">A következő **SyncAsync** metódust a Mobile Apps-háttéralkalmazás szinkronizálni:</span><span class="sxs-lookup"><span data-stu-id="01dda-144">The following **SyncAsync** method is called to sync with the Mobile App backend:</span></span>

        public async Task SyncAsync()
        {
            ReadOnlyCollection<MobileServiceTableOperationError> syncErrors = null;

            try
            {
                await this.client.SyncContext.PushAsync();

                await this.todoTable.PullAsync(
                    "allTodoItems",
                    this.todoTable.CreateQuery());
            }
            catch (MobileServicePushFailedException exc)
            {
                if (exc.PushResult != null)
                {
                    syncErrors = exc.PushResult.Errors;
                }
            }

            // Simple error/conflict handling.
            if (syncErrors != null)
            {
                foreach (var error in syncErrors)
                {
                    if (error.OperationKind == MobileServiceTableOperationKind.Update && error.Result != null)
                    {
                        //Update failed, reverting to server's copy.
                        await error.CancelAndUpdateItemAsync(error.Result);
                    }
                    else
                    {
                        // Discard local change.
                        await error.CancelAndDiscardItemAsync();
                    }

                    Debug.WriteLine(@"Error executing sync operation. Item: {0} ({1}). Operation discarded.",
                        error.TableName, error.Item["id"]);
                }
            }
        }

    <span data-ttu-id="01dda-145">A példa az alapértelmezett szinkronizálási leírójú egyszerű hibakezelési használja.</span><span class="sxs-lookup"><span data-stu-id="01dda-145">This sample uses simple error handling with the default sync handler.</span></span> <span data-ttu-id="01dda-146">Egy valós alkalmazás egyéni használatával szeretné kezelni a különféle hibák például hálózati feltételek és a kiszolgáló ütközések **IMobileServiceSyncHandler** végrehajtására.</span><span class="sxs-lookup"><span data-stu-id="01dda-146">A real application would handle the various errors like network conditions and server conflicts by using a custom **IMobileServiceSyncHandler** implementation.</span></span>

## <a name="offline-sync-considerations"></a><span data-ttu-id="01dda-147">Kapcsolat nélküli szinkronizálás kapcsolatos szempontok</span><span class="sxs-lookup"><span data-stu-id="01dda-147">Offline sync considerations</span></span>
<span data-ttu-id="01dda-148">A példában a **SyncAsync** metódus csak meghívta az indítási és amikor a szinkronizálás van szükség.</span><span class="sxs-lookup"><span data-stu-id="01dda-148">In the sample, the **SyncAsync** method is only called on start-up and when a sync is requested.</span></span>  <span data-ttu-id="01dda-149">Az Android vagy iOS alkalmazások a szinkronizálást kezdeményezni, húzza le a elemek listájában; a Windows, használja a **szinkronizálási** gombra.</span><span class="sxs-lookup"><span data-stu-id="01dda-149">To initiate a sync in an Android or iOS app, pull down on the items list; for Windows, use the **Sync** button.</span></span> <span data-ttu-id="01dda-150">Egy valós alkalmazás is, sikerült a szinkronizálási eseményindító, ha a hálózati állapota megváltozik.</span><span class="sxs-lookup"><span data-stu-id="01dda-150">In a real-world application, you could also make the sync trigger when the network state changes.</span></span>

<span data-ttu-id="01dda-151">Elleni táblának függőben lévő lekérési végrehajtásakor helyi frissítések követik kapcsolatban, hogy a pull művelet automatikusan eseményindítók egy előző környezetben leküldéses.</span><span class="sxs-lookup"><span data-stu-id="01dda-151">When a pull is executed against a table that has pending local updates tracked by the context, that pull operation automatically triggers a preceding context push.</span></span> <span data-ttu-id="01dda-152">Frissítéskor, hozzáadását, és ebben a példában szereplő elemek befejezése, akkor kihagyhatja a explicit **PushAsync** hívható meg.</span><span class="sxs-lookup"><span data-stu-id="01dda-152">When refreshing, adding, and completing items in this sample, you can omit the explicit **PushAsync** call.</span></span>

<span data-ttu-id="01dda-153">A megadott kód a távoli TodoItem tábla minden rekordot a rendszer megkérdezi, de a rekordok szűrése úgy, hogy a lekérdezés azonosítóját, és a lekérdezési lehetőség arra is **PushAsync**.</span><span class="sxs-lookup"><span data-stu-id="01dda-153">In the provided code, all records in the remote TodoItem table are queried, but it is also possible to filter records by passing a query id and query to **PushAsync**.</span></span> <span data-ttu-id="01dda-154">További információkért tekintse meg a szakasz *növekményes szinkronizálás* a [az Azure Mobile Apps Offline adatszinkronizálás][2].</span><span class="sxs-lookup"><span data-stu-id="01dda-154">For more information, see the section *Incremental Sync* in [Offline Data Sync in Azure Mobile Apps][2].</span></span>

## <a name="run-the-client-app"></a><span data-ttu-id="01dda-155">Az ügyfél-alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="01dda-155">Run the client app</span></span>
<span data-ttu-id="01dda-156">A kapcsolat nélküli szinkronizálás most engedélyezve van, az ügyfélalkalmazás legalább egyszer futtatni a helyi tárolójába adatbázisának feltöltése minden platformon.</span><span class="sxs-lookup"><span data-stu-id="01dda-156">With offline sync now enabled, run the client application at least once on each platform to populate the local store database.</span></span> <span data-ttu-id="01dda-157">Később egy kapcsolat nélküli forgatókönyv szimulálása, és módosíthatja az adatokat, a helyi tárolóban levő, amikor az alkalmazás offline állapotban.</span><span class="sxs-lookup"><span data-stu-id="01dda-157">Later, simulate an offline scenario and modify the data in the local store while the app is offline.</span></span>

## <a name="update-the-sync-behavior-of-the-client-app"></a><span data-ttu-id="01dda-158">Frissítés az ügyfélalkalmazás szinkronizálási viselkedése</span><span class="sxs-lookup"><span data-stu-id="01dda-158">Update the sync behavior of the client app</span></span>
<span data-ttu-id="01dda-159">Ebben a szakaszban módosítsa az ügyfél-projektet egy kapcsolat nélküli forgatókönyv szimulálása a háttérkiszolgáló URL-címének érvénytelen az alkalmazás használatával.</span><span class="sxs-lookup"><span data-stu-id="01dda-159">In this section, modify the client project to simulate an offline scenario by using an invalid application URL for your backend.</span></span> <span data-ttu-id="01dda-160">Azt is megteheti de ki is kapcsolhatja hálózati kapcsolatok az eszköz áthelyezésével "Repülőgép üzemmódban."</span><span class="sxs-lookup"><span data-stu-id="01dda-160">Alternatively, you can turn off network connections by moving your device to "Airplane mode."</span></span>  <span data-ttu-id="01dda-161">Hozzáadásakor, vagy módosítsa az elemeket, ezeket a módosításokat a helyi tárolóban levő tartani, de nincs szinkronizálva a háttér-tárolót, amíg a kapcsolat helyreáll.</span><span class="sxs-lookup"><span data-stu-id="01dda-161">When you add or change data items, these changes are held in the local store, but not synced to the backend data store until the connection is re-established.</span></span>

1. <span data-ttu-id="01dda-162">A Solution Explorerben nyissa meg a Constants.cs projektfájlt a a **hordozható** projektre, és módosítsa az értéket a `ApplicationURL` mutasson az egy URL-cím érvénytelen:</span><span class="sxs-lookup"><span data-stu-id="01dda-162">In the Solution Explorer, open the Constants.cs project file from the **Portable** project and change the value of `ApplicationURL` to point to an invalid URL:</span></span>

        public static string ApplicationURL = @"https://your-service.azurewebsites.net/";
2. <span data-ttu-id="01dda-163">Nyissa meg a TodoItemManager.cs a fájl a **hordozható** projektre, majd adja hozzá egy **catch** az alap **kivétel** az osztály a **próbálja... catch** letiltása **SyncAsync**.</span><span class="sxs-lookup"><span data-stu-id="01dda-163">Open the TodoItemManager.cs file from the **Portable** project, then add a **catch** for the base **Exception** class to the **try...catch** block in **SyncAsync**.</span></span> <span data-ttu-id="01dda-164">Ez **catch** blokk ír a kivételre vonatkozó üzenet a konzolt, az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="01dda-164">This **catch** block writes the exception message to the console, as follows:</span></span>

            catch (Exception ex)
            {
                Console.Error.WriteLine(@"Exception: {0}", ex.Message);
            }
3. <span data-ttu-id="01dda-165">Hozza létre, és futtassa az ügyfél-alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="01dda-165">Build and run the client app.</span></span>  <span data-ttu-id="01dda-166">Vegyen fel egy új elemeket.</span><span class="sxs-lookup"><span data-stu-id="01dda-166">Add some new items.</span></span> <span data-ttu-id="01dda-167">Figyelje meg, hogy egy kivételt a konzol minden kísérlet a háttér-vel való szinkronizálásának van bejelentkezve.</span><span class="sxs-lookup"><span data-stu-id="01dda-167">Notice that an exception is logged in the console for each attempt to sync with the backend.</span></span> <span data-ttu-id="01dda-168">Ezek az új elemek csak a helyi tárolóban levő létezik, csak azokat a mobil-háttéralkalmazást továbbíthatja.</span><span class="sxs-lookup"><span data-stu-id="01dda-168">These new items exist only in the local store until they can be pushed to the mobile backend.</span></span> <span data-ttu-id="01dda-169">Az ügyfélalkalmazás úgy viselkedik, mintha a háttér, támogató, az összes létrehozása, olvasása, frissítés, Törlés (CRUD) operations van csatlakoztatva.</span><span class="sxs-lookup"><span data-stu-id="01dda-169">The client app behaves as if it is connected to the backend, supporting all create, read, update, delete (CRUD) operations.</span></span>
4. <span data-ttu-id="01dda-170">Zárja be az alkalmazást, és indítsa újra, hogy ellenőrizze, hogy a helyi tárolójába őrződnek meg a létrehozott új elemeket.</span><span class="sxs-lookup"><span data-stu-id="01dda-170">Close the app and restart it to verify that the new items you created are persisted to the local store.</span></span>
5. <span data-ttu-id="01dda-171">(Választható) Visual Studio használatával az Azure SQL Database tábla, hogy a háttér-adatbázis adatai nem változott meg.</span><span class="sxs-lookup"><span data-stu-id="01dda-171">(Optional) Use Visual Studio to view your Azure SQL Database table to see that the data in the backend database has not changed.</span></span>

    <span data-ttu-id="01dda-172">A Visual Studióban nyissa meg a **Server Explorer**.</span><span class="sxs-lookup"><span data-stu-id="01dda-172">In Visual Studio, open **Server Explorer**.</span></span> <span data-ttu-id="01dda-173">Keresse meg az adatbázist a **Azure**->**SQL-adatbázisok**.</span><span class="sxs-lookup"><span data-stu-id="01dda-173">Navigate to your database in **Azure**->**SQL Databases**.</span></span> <span data-ttu-id="01dda-174">Kattintson a jobb gombbal az adatbázist, és válassza ki **nyissa meg az SQL Server Object Explorerben**.</span><span class="sxs-lookup"><span data-stu-id="01dda-174">Right-click your database and select **Open in SQL Server Object Explorer**.</span></span> <span data-ttu-id="01dda-175">Most már megkeresheti az SQL-adatbázistáblában szereplő és annak tartalmát.</span><span class="sxs-lookup"><span data-stu-id="01dda-175">Now you can browse to your SQL database table and its contents.</span></span>

## <a name="update-the-client-app-to-reconnect-your-mobile-backend"></a><span data-ttu-id="01dda-176">Ha újból csatlakozni a mobil-háttéralkalmazást ügyfél alkalmazás frissítése</span><span class="sxs-lookup"><span data-stu-id="01dda-176">Update the client app to reconnect your mobile backend</span></span>
<span data-ttu-id="01dda-177">Ebben a szakaszban csatlakozzon újra az alkalmazásnak, hogy a mobil-háttéralkalmazást, az alkalmazáshoz, és az online állapot vissza hamarosan szimulálja.</span><span class="sxs-lookup"><span data-stu-id="01dda-177">In this section, reconnect the app to the mobile backend, which simulates the app coming back to an online state.</span></span> <span data-ttu-id="01dda-178">A frissítési kézmozdulat végrehajtásakor adatok szinkronizálva van a mobil-háttéralkalmazást.</span><span class="sxs-lookup"><span data-stu-id="01dda-178">When you perform the refresh gesture, data is synced to your mobile backend.</span></span>

1. <span data-ttu-id="01dda-179">Nyissa meg újra a Constants.cs.</span><span class="sxs-lookup"><span data-stu-id="01dda-179">Reopen Constants.cs.</span></span> <span data-ttu-id="01dda-180">Javítsa ki a `applicationURL` úgy, hogy a helyes URL-címet mutasson.</span><span class="sxs-lookup"><span data-stu-id="01dda-180">Correct the `applicationURL` to point to the correct URL.</span></span>
2. <span data-ttu-id="01dda-181">Építse újra, és futtassa az ügyfél-alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="01dda-181">Rebuild and run the client app.</span></span> <span data-ttu-id="01dda-182">Az alkalmazás megpróbálja szinkronizálni a mobil-háttéralkalmazás indítását követően.</span><span class="sxs-lookup"><span data-stu-id="01dda-182">The app attempts to sync with the mobile app backend after launching.</span></span> <span data-ttu-id="01dda-183">Győződjön meg arról, hogy nem tartalmaznak kivételeket a hibakereső konzol vannak bejelentkezve.</span><span class="sxs-lookup"><span data-stu-id="01dda-183">Verify that no exceptions are logged in the debug console.</span></span>
3. <span data-ttu-id="01dda-184">(Választható) Az SQL Server Object Explorer vagy egy REST-eszköz, például Fiddler használatával frissített adatok megtekintéséhez vagy [Postman][6].</span><span class="sxs-lookup"><span data-stu-id="01dda-184">(Optional) View the updated data using either SQL Server Object Explorer or a REST tool like Fiddler or [Postman][6].</span></span> <span data-ttu-id="01dda-185">Figyelje meg, az adatok szinkronizálása a háttérbeli adatbázis és a helyi tárolójába.</span><span class="sxs-lookup"><span data-stu-id="01dda-185">Notice the data has been synchronized between the backend database and the local store.</span></span>

    <span data-ttu-id="01dda-186">Figyelje meg, az adatok az adatbázis és a helyi tárolójába lett szinkronizálva, és az alkalmazás forráshoz hozzáadott elemeket tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="01dda-186">Notice the data has been synchronized between the database and the local store and contains the items you added while your app was disconnected.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="01dda-187">További források</span><span class="sxs-lookup"><span data-stu-id="01dda-187">Additional Resources</span></span>
* <span data-ttu-id="01dda-188">[Az Azure Mobile Apps az offline adatszinkronizálás][2]</span><span class="sxs-lookup"><span data-stu-id="01dda-188">[Offline Data Sync in Azure Mobile Apps][2]</span></span>
* <span data-ttu-id="01dda-189">[Az Azure Mobile Apps .NET SDK útmutató][8]</span><span class="sxs-lookup"><span data-stu-id="01dda-189">[Azure Mobile Apps .NET SDK HOWTO][8]</span></span>

<!-- URLs. -->
[1]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[2]: app-service-mobile-offline-data-sync.md
[3]: http://go.microsoft.com/fwlink/p/?LinkID=716919
[4]: http://go.microsoft.com/fwlink/p/?LinkID=716920
[5]: http://sqlite.org/2016/sqlite-uwp-3120200.vsix
[6]: https://www.getpostman.com/
[7]: http://www.telerik.com/fiddler
[8]: app-service-mobile-dotnet-how-to-use-client-library.md
