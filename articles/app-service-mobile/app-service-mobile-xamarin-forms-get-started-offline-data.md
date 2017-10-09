---
title: "az Azure Mobile Apps (Xamarin.Forms) kapcsolat nélküli szinkronizálásának aaaEnable |} Microsoft Docs"
description: "Megtudhatja, hogyan toouse App Service Mobile Apps toocache és a szinkronizálási kapcsolat nélküli adatok a Xamarin.Forms-alkalmazás"
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
ms.openlocfilehash: 4b41404fc9507e82068fdf8aa612d57cbeb366bf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="enable-offline-sync-for-your-xamarinforms-mobile-app"></a><span data-ttu-id="b02e5-103">Xamarin.Forms-mobilalkalmazás offline szinkronizálásának engedélyezése</span><span class="sxs-lookup"><span data-stu-id="b02e5-103">Enable offline sync for your Xamarin.Forms mobile app</span></span>
[!INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a><span data-ttu-id="b02e5-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="b02e5-104">Overview</span></span>
<span data-ttu-id="b02e5-105">Ez az oktatóanyag hello kapcsolat nélküli szinkronizálás szolgáltatást az Azure Mobile Apps xamarin.Forms vezet be.</span><span class="sxs-lookup"><span data-stu-id="b02e5-105">This tutorial introduces hello offline sync feature of Azure Mobile Apps for Xamarin.Forms.</span></span> <span data-ttu-id="b02e5-106">Kapcsolat nélküli szinkronizálás lehetővé teszi, hogy a végfelhasználók számára, amellyel kommunikálni tud a mobilalkalmazások--megtekintését, hozzáadását és módosítását adatokat –, akkor is, ha nincs hálózati kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="b02e5-106">Offline sync allows end users to interact with a mobile app--viewing, adding, or modifying data--even when there is no network connection.</span></span> <span data-ttu-id="b02e5-107">Változások a helyi adatbázisban tárolódnak.</span><span class="sxs-lookup"><span data-stu-id="b02e5-107">Changes are stored in a local database.</span></span> <span data-ttu-id="b02e5-108">Hello eszköz újra online állapotba kerül, ha ezek a változások szinkronizálása megtörtént-e távoli hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="b02e5-108">Once hello device is back online, these changes are synced with hello remote service.</span></span>

<span data-ttu-id="b02e5-109">Ez az oktatóanyag hello gyors üzembe helyezés Xamarin.Forms-megoldás alapján hoz létre, amikor befejezte az oktatóanyag [Xamarin iOS-alkalmazás létrehozása] Mobile Apps-alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="b02e5-109">This tutorial is based on hello Xamarin.Forms quickstart solution for Mobile Apps that you create when you complete the tutorial [Create a Xamarin iOS app].</span></span> <span data-ttu-id="b02e5-110">hello gyors üzembe helyezés megoldás xamarin.Forms hello kód toosupport kapcsolat nélküli szinkronizálás, amely csak az engedélyezett toobe kell tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="b02e5-110">hello quickstart solution for Xamarin.Forms contains hello code toosupport offline sync, which just needs toobe enabled.</span></span> <span data-ttu-id="b02e5-111">Ebben az oktatóanyagban hello gyors üzembe helyezés megoldás tooturn hello offline szolgáltatások az Azure Mobile Apps frissíti.</span><span class="sxs-lookup"><span data-stu-id="b02e5-111">In this tutorial, you update hello quickstart solution tooturn on hello offline features of Azure Mobile Apps.</span></span> <span data-ttu-id="b02e5-112">A Microsoft hello offline tartozó kódot hello alkalmazásban is jelöljön ki.</span><span class="sxs-lookup"><span data-stu-id="b02e5-112">We also highlight hello offline-specific code in hello app.</span></span> <span data-ttu-id="b02e5-113">Ha nem használja a letöltött hello gyors üzembe helyezés megoldás, hozzá kell adnia a hello data access kiegészítő csomagok tooyour projekt.</span><span class="sxs-lookup"><span data-stu-id="b02e5-113">If you do not use hello downloaded quickstart solution, you must add hello data access extension packages tooyour project.</span></span> <span data-ttu-id="b02e5-114">Kiszolgáló bővítménycsomagok kapcsolatos további információkért lásd: [használható hello .NET háttérkiszolgáló SDK az Azure Mobile Apps a][1].</span><span class="sxs-lookup"><span data-stu-id="b02e5-114">For more information about server extension packages, see [Work with hello .NET backend server SDK for Azure Mobile Apps][1].</span></span>

<span data-ttu-id="b02e5-115">További információ az hello kapcsolat nélküli szinkronizálás szolgáltatást, toolearn hello témakörben találhatók [az Azure Mobile Apps Offline adatszinkronizálás][2].</span><span class="sxs-lookup"><span data-stu-id="b02e5-115">toolearn more about hello offline sync feature, see hello topic [Offline Data Sync in Azure Mobile Apps][2].</span></span>

## <a name="enable-offline-sync-functionality-in-hello-quickstart-solution"></a><span data-ttu-id="b02e5-116">Hello gyors üzembe helyezés megoldásban kapcsolat nélküli szinkronizálásának engedélyezése</span><span class="sxs-lookup"><span data-stu-id="b02e5-116">Enable offline sync functionality in hello quickstart solution</span></span>
<span data-ttu-id="b02e5-117">hello kapcsolat nélküli szinkronizálás kód hello projekt C# előfeldolgozó irányelvek segítségével tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="b02e5-117">hello offline sync code is included in hello project by using C# preprocessor directives.</span></span> <span data-ttu-id="b02e5-118">Ha hello **OFFLINE\_SZINKRONIZÁLÁSI\_engedélyezve** szimbólum van megadva, a kód elérési utak hello build szerepelnek.</span><span class="sxs-lookup"><span data-stu-id="b02e5-118">When hello **OFFLINE\_SYNC\_ENABLED** symbol is defined, these code paths are included in hello build.</span></span> <span data-ttu-id="b02e5-119">Windows-alkalmazások esetén is telepíteni kell a hello SQLite platform.</span><span class="sxs-lookup"><span data-stu-id="b02e5-119">For Windows apps, you must also install hello SQLite platform.</span></span>

1. <span data-ttu-id="b02e5-120">A Visual Studióban, kattintson a jobb gombbal hello megoldás > **NuGet-csomagok kezelése megoldáshoz...** , majd keresse meg, és telepítse a **Microsoft.Azure.Mobile.Client.SQLiteStore** projektek hello megoldás NuGet-csomagot.</span><span class="sxs-lookup"><span data-stu-id="b02e5-120">In Visual Studio, right-click hello solution > **Manage NuGet Packages for Solution...**, then search for and install the **Microsoft.Azure.Mobile.Client.SQLiteStore** NuGet package for all projects in hello solution.</span></span>
2. <span data-ttu-id="b02e5-121">A Solution Explorer hello, nyissa meg az hello TodoItemManager.cs fájlt hello projektből az **hordozható** hello neve, amely hordozható Class Library projektet, majd állítsa vissza a következő előfeldolgozó irányelv hello:</span><span class="sxs-lookup"><span data-stu-id="b02e5-121">In hello Solution Explorer, open hello TodoItemManager.cs file from hello project with **Portable** in hello name, which is Portable Class Library project, then uncomment hello following preprocessor directive:</span></span>

        #define OFFLINE_SYNC_ENABLED
3. <span data-ttu-id="b02e5-122">(Választható) toosupport Windows-eszközök esetében egy SQLite futtatókörnyezetek következő hello telepíthető:</span><span class="sxs-lookup"><span data-stu-id="b02e5-122">(Optional) toosupport Windows devices, install one of hello following SQLite runtime packages:</span></span>

   * <span data-ttu-id="b02e5-123">**Windows 8.1 futásidejű:** telepítése [a Windows 8.1 SQLite][3].</span><span class="sxs-lookup"><span data-stu-id="b02e5-123">**Windows 8.1 Runtime:** Install [SQLite for Windows 8.1][3].</span></span>
   * <span data-ttu-id="b02e5-124">**Windows Phone 8.1:** telepítése [a Windows Phone 8.1 SQLite][4].</span><span class="sxs-lookup"><span data-stu-id="b02e5-124">**Windows Phone 8.1:** Install [SQLite for Windows Phone 8.1][4].</span></span>
   * <span data-ttu-id="b02e5-125">**Univerzális Windows Platform** telepítése [az univerzális Windows Universal hello SQLite][5].</span><span class="sxs-lookup"><span data-stu-id="b02e5-125">**Universal Windows Platform** Install [SQLite for hello Universal Windows Universal][5].</span></span>

     <span data-ttu-id="b02e5-126">Bár a hello gyors üzembe helyezés nem tartalmaz egy univerzális Windows-projektjét, Xamarin Forms hello univerzális Windows platform használata támogatott.</span><span class="sxs-lookup"><span data-stu-id="b02e5-126">Although hello quickstart does not contain a Universal Windows project, hello Universal Windows platform is supported with Xamarin Forms.</span></span>
4. <span data-ttu-id="b02e5-127">(Választható) Minden Windows-alkalmazás projekt, kattintson a jobb egérgombbal **hivatkozások** > **hivatkozás hozzáadása...** , bontsa ki a hello **Windows** mappa > **bővítmények**.</span><span class="sxs-lookup"><span data-stu-id="b02e5-127">(Optional) In each Windows app project, right-click **References** > **Add Reference...**, expand hello **Windows** folder > **Extensions**.</span></span>
    <span data-ttu-id="b02e5-128">Engedélyezze a megfelelő hello **SQLite for Windows** együtt hello SDK **Visual C++ 2013 futásidejű for Windows** SDK.</span><span class="sxs-lookup"><span data-stu-id="b02e5-128">Enable hello appropriate **SQLite for Windows** SDK along with hello **Visual C++ 2013 Runtime for Windows** SDK.</span></span>
    <span data-ttu-id="b02e5-129">hello SQLite SDK nevek függően eltérőek az egyes Windows-platformra.</span><span class="sxs-lookup"><span data-stu-id="b02e5-129">hello SQLite SDK names vary slightly with each Windows platform.</span></span>

## <a name="review-hello-client-sync-code"></a><span data-ttu-id="b02e5-130">Tekintse át a kód a hello ügyfél szinkronizálása</span><span class="sxs-lookup"><span data-stu-id="b02e5-130">Review hello client sync code</span></span>
<span data-ttu-id="b02e5-131">Ez már tartalmának hello oktatóanyag kódot hello rövid áttekintést `#if OFFLINE_SYNC_ENABLED` irányelvek.</span><span class="sxs-lookup"><span data-stu-id="b02e5-131">Here is a brief overview of what is already included in hello tutorial code inside hello `#if OFFLINE_SYNC_ENABLED` directives.</span></span> <span data-ttu-id="b02e5-132">A kapcsolat nélküli szinkronizálásának hello hello hordozható osztály Hordozhatóosztálytár-projektjének TodoItemManager.cs projektfájlt van.</span><span class="sxs-lookup"><span data-stu-id="b02e5-132">The offline sync functionality is in hello TodoItemManager.cs project file in hello Portable Class Library project.</span></span> <span data-ttu-id="b02e5-133">Hello szolgáltatás elméleti áttekintését lásd: [az Azure Mobile Apps Offline adatszinkronizálás][2].</span><span class="sxs-lookup"><span data-stu-id="b02e5-133">For a conceptual overview of hello feature, see [Offline Data Sync in Azure Mobile Apps][2].</span></span>

* <span data-ttu-id="b02e5-134">Minden tábla műveletek elvégzése előtt hello helyi tároló inicializálni kell.</span><span class="sxs-lookup"><span data-stu-id="b02e5-134">Before any table operations can be performed, hello local store must be initialized.</span></span> <span data-ttu-id="b02e5-135">hello helyi tároló adatbázis inicializálva van a hello **TodoItemManager** osztálykonstruktor hello kód a következő használatával:</span><span class="sxs-lookup"><span data-stu-id="b02e5-135">hello local store database is initialized in hello **TodoItemManager** class constructor by using hello following code:</span></span>

        var store = new MobileServiceSQLiteStore(OfflineDbPath);
        store.DefineTable<TodoItem>();

        //Initializes hello SyncContext using hello default IMobileServiceSyncHandler.
        this.client.SyncContext.InitializeAsync(store);

        this.todoTable = client.GetSyncTable<TodoItem>();

    <span data-ttu-id="b02e5-136">Ez a kód egy új adatbázist hoz létre helyi SQLite használatával hello **MobileServiceSQLiteStore** osztály.</span><span class="sxs-lookup"><span data-stu-id="b02e5-136">This code creates a new local SQLite database using hello **MobileServiceSQLiteStore** class.</span></span>

    <span data-ttu-id="b02e5-137">Hello **DefineTable** metódus táblázatot hoz létre, amely megfelel a megadott hello típus hello mezői hello helyi tárolóban levő.</span><span class="sxs-lookup"><span data-stu-id="b02e5-137">hello **DefineTable** method creates a table in hello local store that matches hello fields in hello provided type.</span></span>  <span data-ttu-id="b02e5-138">hello típushoz nem tartozik tooinclude hello távoli adatbázisban lévő összes hello-oszlopot.</span><span class="sxs-lookup"><span data-stu-id="b02e5-138">hello type doesn't have tooinclude all hello columns that are in hello remote database.</span></span> <span data-ttu-id="b02e5-139">Lehetséges toostore az oszlopok egy részét is.</span><span class="sxs-lookup"><span data-stu-id="b02e5-139">It is possible toostore a subset of columns.</span></span>
* <span data-ttu-id="b02e5-140">Hello **todoTable** mezőjét **TodoItemManager** van egy **IMobileServiceSyncTable** helyett típusú **IMobileServiceTable**.</span><span class="sxs-lookup"><span data-stu-id="b02e5-140">hello **todoTable** field in **TodoItemManager** is an **IMobileServiceSyncTable** type instead of **IMobileServiceTable**.</span></span> <span data-ttu-id="b02e5-141">Ez az osztály által használt hello helyi adatbázis összes létrehozása, olvasása, frissítése és Törlés (CRUD) tábla műveletek.</span><span class="sxs-lookup"><span data-stu-id="b02e5-141">This class uses hello local database for all create, read, update, and delete (CRUD) table operations.</span></span> <span data-ttu-id="b02e5-142">Úgy dönt, ha ezek a módosítások vannak leküldött toohello Mobile Apps-háttéralkalmazás meghívásával **PushAsync** a hello **IMobileServiceSyncContext**.</span><span class="sxs-lookup"><span data-stu-id="b02e5-142">You decide when those changes are pushed toohello Mobile App backend by calling **PushAsync** on hello **IMobileServiceSyncContext**.</span></span> <span data-ttu-id="b02e5-143">hello szinkronizálási környezetet segíti követésével és kérdez le a módosításokat minden olyan táblát, egy ügyfélalkalmazás módosította, ha a tábla közötti megőrzése érdekében **PushAsync** nevezik.</span><span class="sxs-lookup"><span data-stu-id="b02e5-143">hello sync context helps preserve table relationships by tracking and pushing changes in all tables a client app has modified when **PushAsync** is called.</span></span>

    <span data-ttu-id="b02e5-144">hello következő **SyncAsync** metódus lehívásra kerül a Mobile Apps-háttéralkalmazás hello toosync:</span><span class="sxs-lookup"><span data-stu-id="b02e5-144">hello following **SyncAsync** method is called toosync with hello Mobile App backend:</span></span>

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
                        //Update failed, reverting tooserver's copy.
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

    <span data-ttu-id="b02e5-145">Ez a minta hello alapértelmezett szinkronizálási leírójú egyszerű hibakezelési használja.</span><span class="sxs-lookup"><span data-stu-id="b02e5-145">This sample uses simple error handling with hello default sync handler.</span></span> <span data-ttu-id="b02e5-146">Egy valós alkalmazás hello például hálózati feltételek és a kiszolgáló különböző hibák ütközik egy egyéni használatával szeretné kezelni **IMobileServiceSyncHandler** végrehajtására.</span><span class="sxs-lookup"><span data-stu-id="b02e5-146">A real application would handle hello various errors like network conditions and server conflicts by using a custom **IMobileServiceSyncHandler** implementation.</span></span>

## <a name="offline-sync-considerations"></a><span data-ttu-id="b02e5-147">Kapcsolat nélküli szinkronizálás kapcsolatos szempontok</span><span class="sxs-lookup"><span data-stu-id="b02e5-147">Offline sync considerations</span></span>
<span data-ttu-id="b02e5-148">Hello mintában hello **SyncAsync** metódus csak meghívta az indítási és amikor a szinkronizálás van szükség.</span><span class="sxs-lookup"><span data-stu-id="b02e5-148">In hello sample, hello **SyncAsync** method is only called on start-up and when a sync is requested.</span></span>  <span data-ttu-id="b02e5-149">Android vagy iOS alkalmazások, lekéréses le hello elemek listája; a szinkronizálási tooinitiate a Windows hello használata **szinkronizálási** gombra.</span><span class="sxs-lookup"><span data-stu-id="b02e5-149">tooinitiate a sync in an Android or iOS app, pull down on hello items list; for Windows, use hello **Sync** button.</span></span> <span data-ttu-id="b02e5-150">Egy valós alkalmazás is meg sikerült hello szinkronizálási eseményindító, ha hello hálózati állapota megváltozik.</span><span class="sxs-lookup"><span data-stu-id="b02e5-150">In a real-world application, you could also make hello sync trigger when hello network state changes.</span></span>

<span data-ttu-id="b02e5-151">Elleni táblának függőben lévő lekérési végrehajtásakor helyi frissítések követi nyomon, hogy a pull művelet automatikusan eseményindítók hello összefüggésben egy előző környezetben leküldéses.</span><span class="sxs-lookup"><span data-stu-id="b02e5-151">When a pull is executed against a table that has pending local updates tracked by hello context, that pull operation automatically triggers a preceding context push.</span></span> <span data-ttu-id="b02e5-152">Ha frissíteni, hozzáadása, és ez a példa elemek befejezése, akkor kihagyhatja hello explicit **PushAsync** hívható meg.</span><span class="sxs-lookup"><span data-stu-id="b02e5-152">When refreshing, adding, and completing items in this sample, you can omit hello explicit **PushAsync** call.</span></span>

<span data-ttu-id="b02e5-153">A megadott hello kódban, a rendszer megkérdezi a hello távoli TodoItem tábla összes adatát, de azt is lehetséges toofilter rekordok úgy, hogy a lekérdezés azonosítóját és a lekérdezés túl**PushAsync**.</span><span class="sxs-lookup"><span data-stu-id="b02e5-153">In hello provided code, all records in hello remote TodoItem table are queried, but it is also possible toofilter records by passing a query id and query too**PushAsync**.</span></span> <span data-ttu-id="b02e5-154">További információkért lásd: hello szakasz *növekményes szinkronizálás* a [az Azure Mobile Apps Offline adatszinkronizálás][2].</span><span class="sxs-lookup"><span data-stu-id="b02e5-154">For more information, see hello section *Incremental Sync* in [Offline Data Sync in Azure Mobile Apps][2].</span></span>

## <a name="run-hello-client-app"></a><span data-ttu-id="b02e5-155">Hello ügyfél-alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="b02e5-155">Run hello client app</span></span>
<span data-ttu-id="b02e5-156">Kapcsolat nélküli szinkronizálás most engedélyezve van futtatja a hello ügyfélalkalmazás legalább egyszer minden egyes platform toopopulate hello helyi tároló adatbázis.</span><span class="sxs-lookup"><span data-stu-id="b02e5-156">With offline sync now enabled, run hello client application at least once on each platform toopopulate hello local store database.</span></span> <span data-ttu-id="b02e5-157">Később egy kapcsolat nélküli forgatókönyv szimulálása és hello adatok hello helyi tárolóban levő módosítható, amíg az alkalmazás hello offline állapotban.</span><span class="sxs-lookup"><span data-stu-id="b02e5-157">Later, simulate an offline scenario and modify hello data in hello local store while hello app is offline.</span></span>

## <a name="update-hello-sync-behavior-of-hello-client-app"></a><span data-ttu-id="b02e5-158">Hello szinkronizálási viselkedését hello ügyfélalkalmazás frissítése</span><span class="sxs-lookup"><span data-stu-id="b02e5-158">Update hello sync behavior of hello client app</span></span>
<span data-ttu-id="b02e5-159">Ebben a szakaszban módosítsa hello ügyfél projekt toosimulate egy kapcsolat nélküli forgatókönyv a háttérkiszolgáló URL-címének érvénytelen az alkalmazás használatával.</span><span class="sxs-lookup"><span data-stu-id="b02e5-159">In this section, modify hello client project toosimulate an offline scenario by using an invalid application URL for your backend.</span></span> <span data-ttu-id="b02e5-160">Alternatív megoldásként kikapcsolhatja hálózati kapcsolatok túl át az eszköz "Repülőgép üzemmódban."</span><span class="sxs-lookup"><span data-stu-id="b02e5-160">Alternatively, you can turn off network connections by moving your device too"Airplane mode."</span></span>  <span data-ttu-id="b02e5-161">Hozzáadásakor, vagy módosítsa az elemeket, ezek a változások hello helyi tárolóban levő tartani, de nem szinkronizált toohello háttérrendszeri adatok tárolására, amíg hello kapcsolat helyreáll.</span><span class="sxs-lookup"><span data-stu-id="b02e5-161">When you add or change data items, these changes are held in hello local store, but not synced toohello backend data store until hello connection is re-established.</span></span>

1. <span data-ttu-id="b02e5-162">A Solution Explorer hello, nyissa meg az hello Constants.cs projektfájlt a hello **hordozható** projektre, és hello értékének módosítása `ApplicationURL` toopoint tooan URL-cím érvénytelen:</span><span class="sxs-lookup"><span data-stu-id="b02e5-162">In hello Solution Explorer, open hello Constants.cs project file from hello **Portable** project and change hello value of `ApplicationURL` toopoint tooan invalid URL:</span></span>

        public static string ApplicationURL = @"https://your-service.azurewebsites.net/";
2. <span data-ttu-id="b02e5-163">Megnyitás hello TodoItemManager.cs fájlt hello **hordozható** projektre, majd adja hozzá a **catch** az alapszintű hello **kivétel** osztály toohello **próbálja... catch** letiltása **SyncAsync**.</span><span class="sxs-lookup"><span data-stu-id="b02e5-163">Open hello TodoItemManager.cs file from hello **Portable** project, then add a **catch** for hello base **Exception** class toohello **try...catch** block in **SyncAsync**.</span></span> <span data-ttu-id="b02e5-164">Ez **catch** blokk ír hello kivétel üzenet toohello konzol, az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="b02e5-164">This **catch** block writes hello exception message toohello console, as follows:</span></span>

            catch (Exception ex)
            {
                Console.Error.WriteLine(@"Exception: {0}", ex.Message);
            }
3. <span data-ttu-id="b02e5-165">Hozza létre és hello ügyfélalkalmazás futtatása.</span><span class="sxs-lookup"><span data-stu-id="b02e5-165">Build and run hello client app.</span></span>  <span data-ttu-id="b02e5-166">Vegyen fel egy új elemeket.</span><span class="sxs-lookup"><span data-stu-id="b02e5-166">Add some new items.</span></span> <span data-ttu-id="b02e5-167">Figyelje meg, hogy kivételt hello konzol minden kísérlet toosync hello háttérrendszerrel van bejelentkezve.</span><span class="sxs-lookup"><span data-stu-id="b02e5-167">Notice that an exception is logged in hello console for each attempt toosync with hello backend.</span></span> <span data-ttu-id="b02e5-168">Ezek az új elemek található csak hello helyi tárolóban mindaddig, amíg azok toohello mobil-háttéralkalmazást továbbíthatja.</span><span class="sxs-lookup"><span data-stu-id="b02e5-168">These new items exist only in hello local store until they can be pushed toohello mobile backend.</span></span> <span data-ttu-id="b02e5-169">hello ügyfélalkalmazás úgy viselkedik, mintha már csatlakoztatott toohello háttér, támogató, az összes létrehozása, olvasása, frissítés, Törlés (CRUD) műveleteket.</span><span class="sxs-lookup"><span data-stu-id="b02e5-169">hello client app behaves as if it is connected toohello backend, supporting all create, read, update, delete (CRUD) operations.</span></span>
4. <span data-ttu-id="b02e5-170">Hello-alkalmazások bezárása, és indítsa újra azt, hogy hello létrehozott új elemek-e a megőrzött toohello helyi tároló tooverify.</span><span class="sxs-lookup"><span data-stu-id="b02e5-170">Close hello app and restart it tooverify that hello new items you created are persisted toohello local store.</span></span>
5. <span data-ttu-id="b02e5-171">(Választható) Visual Studio tooview használja az Azure SQL Database tábla toosee hello adatok hello háttérbeli adatbázis nem módosított.</span><span class="sxs-lookup"><span data-stu-id="b02e5-171">(Optional) Use Visual Studio tooview your Azure SQL Database table toosee that hello data in hello backend database has not changed.</span></span>

    <span data-ttu-id="b02e5-172">A Visual Studióban nyissa meg a **Server Explorer**.</span><span class="sxs-lookup"><span data-stu-id="b02e5-172">In Visual Studio, open **Server Explorer**.</span></span> <span data-ttu-id="b02e5-173">Keresse meg a tooyour adatbázis **Azure**->**SQL-adatbázisok**.</span><span class="sxs-lookup"><span data-stu-id="b02e5-173">Navigate tooyour database in **Azure**->**SQL Databases**.</span></span> <span data-ttu-id="b02e5-174">Kattintson a jobb gombbal az adatbázist, és válassza ki **nyissa meg az SQL Server Object Explorerben**.</span><span class="sxs-lookup"><span data-stu-id="b02e5-174">Right-click your database and select **Open in SQL Server Object Explorer**.</span></span> <span data-ttu-id="b02e5-175">Most már megkeresheti tooyour SQL-adatbázistáblában szereplő és annak tartalmát.</span><span class="sxs-lookup"><span data-stu-id="b02e5-175">Now you can browse tooyour SQL database table and its contents.</span></span>

## <a name="update-hello-client-app-tooreconnect-your-mobile-backend"></a><span data-ttu-id="b02e5-176">Hello ügyfél app tooreconnect a mobil-háttéralkalmazást frissítése</span><span class="sxs-lookup"><span data-stu-id="b02e5-176">Update hello client app tooreconnect your mobile backend</span></span>
<span data-ttu-id="b02e5-177">Ebben a szakaszban újracsatlakozás hello toohello mobil háttéralkalmazás, szimulálja hello alkalmazáshoz hamarosan vissza tooan online állapotban.</span><span class="sxs-lookup"><span data-stu-id="b02e5-177">In this section, reconnect hello app toohello mobile backend, which simulates hello app coming back tooan online state.</span></span> <span data-ttu-id="b02e5-178">Hello frissítési kézmozdulat végrehajtásakor a adata szinkronizált tooyour mobil-háttéralkalmazást.</span><span class="sxs-lookup"><span data-stu-id="b02e5-178">When you perform hello refresh gesture, data is synced tooyour mobile backend.</span></span>

1. <span data-ttu-id="b02e5-179">Nyissa meg újra a Constants.cs.</span><span class="sxs-lookup"><span data-stu-id="b02e5-179">Reopen Constants.cs.</span></span> <span data-ttu-id="b02e5-180">Megfelelő hello `applicationURL` toopoint toohello javítsa ki az URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="b02e5-180">Correct hello `applicationURL` toopoint toohello correct URL.</span></span>
2. <span data-ttu-id="b02e5-181">Építse újra és hello ügyfélalkalmazás futtatása.</span><span class="sxs-lookup"><span data-stu-id="b02e5-181">Rebuild and run hello client app.</span></span> <span data-ttu-id="b02e5-182">hello alkalmazás indítását követően próbálja meg a mobil-háttéralkalmazás hello toosync.</span><span class="sxs-lookup"><span data-stu-id="b02e5-182">hello app attempts toosync with hello mobile app backend after launching.</span></span> <span data-ttu-id="b02e5-183">Győződjön meg arról, hogy nem tartalmaznak kivételeket hello hibakereső konzol vannak bejelentkezve.</span><span class="sxs-lookup"><span data-stu-id="b02e5-183">Verify that no exceptions are logged in hello debug console.</span></span>
3. <span data-ttu-id="b02e5-184">(Választható) Nézet hello a módosított adatokat az SQL Server Object Explorer vagy egy REST-eszköz, például Fiddler használatával vagy [Postman][6].</span><span class="sxs-lookup"><span data-stu-id="b02e5-184">(Optional) View hello updated data using either SQL Server Object Explorer or a REST tool like Fiddler or [Postman][6].</span></span> <span data-ttu-id="b02e5-185">Értesítés hello adatok hello háttér adatbázis és a helyi tárolójába hello lett szinkronizálva.</span><span class="sxs-lookup"><span data-stu-id="b02e5-185">Notice hello data has been synchronized between hello backend database and hello local store.</span></span>

    <span data-ttu-id="b02e5-186">Figyelje meg hello adatok hello adatbázis és a helyi tárolójába hello lett szinkronizálva, és az alkalmazás forráshoz hozzáadott hello elemet tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="b02e5-186">Notice hello data has been synchronized between hello database and hello local store and contains hello items you added while your app was disconnected.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b02e5-187">További források</span><span class="sxs-lookup"><span data-stu-id="b02e5-187">Additional Resources</span></span>
* <span data-ttu-id="b02e5-188">[Az Azure Mobile Apps az offline adatszinkronizálás][2]</span><span class="sxs-lookup"><span data-stu-id="b02e5-188">[Offline Data Sync in Azure Mobile Apps][2]</span></span>
* <span data-ttu-id="b02e5-189">[Az Azure Mobile Apps .NET SDK útmutató][8]</span><span class="sxs-lookup"><span data-stu-id="b02e5-189">[Azure Mobile Apps .NET SDK HOWTO][8]</span></span>

<!-- URLs. -->
[1]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[2]: app-service-mobile-offline-data-sync.md
[3]: http://go.microsoft.com/fwlink/p/?LinkID=716919
[4]: http://go.microsoft.com/fwlink/p/?LinkID=716920
[5]: http://sqlite.org/2016/sqlite-uwp-3120200.vsix
[6]: https://www.getpostman.com/
[7]: http://www.telerik.com/fiddler
[8]: app-service-mobile-dotnet-how-to-use-client-library.md
