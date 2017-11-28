---
title: "kapcsolat nélküli szinkronizálásának aaaEnable az Azure Mobile Apps (Android)"
description: "Megtudhatja, hogyan toouse App Service Mobile Apps toocache és szinkronizálási offline adatokat az Android-alkalmazás"
documentationcenter: android
author: ggailey777
manager: syntaxc4
services: app-service\mobile
ms.assetid: 32a8a079-9b3c-4faf-8588-ccff02097224
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: 34508c7394610cf9127e1753637940826b8fd06a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="enable-offline-sync-for-your-android-mobile-app"></a><span data-ttu-id="12399-103">Androidos mobilalkalmazás kapcsolat nélküli szinkronizálásának engedélyezése</span><span class="sxs-lookup"><span data-stu-id="12399-103">Enable offline sync for your Android mobile app</span></span>
[!INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a><span data-ttu-id="12399-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="12399-104">Overview</span></span>
<span data-ttu-id="12399-105">Ez az oktatóanyag az Azure Mobile Apps hello kapcsolat nélküli szinkronizálás szolgáltatása Android ismerteti.</span><span class="sxs-lookup"><span data-stu-id="12399-105">This tutorial covers hello offline sync feature of Azure Mobile Apps for Android.</span></span> <span data-ttu-id="12399-106">Kapcsolat nélküli szinkronizálás lehetővé teszi, hogy a befejező felhasználóknak toointeract mobilalkalmazást&mdash;megtekintését, hozzáadását és módosítását adatok&mdash;akkor is, ha nincs hálózati kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="12399-106">Offline sync allows end users toointeract with a mobile app&mdash;viewing, adding, or modifying data&mdash;even when there is no network connection.</span></span> <span data-ttu-id="12399-107">Változások a helyi adatbázisban tárolódnak.</span><span class="sxs-lookup"><span data-stu-id="12399-107">Changes are stored in a local database.</span></span> <span data-ttu-id="12399-108">Hello eszköz újra online állapotba kerül, ha ezek a változások szinkronizálása megtörtént-e hello távoli háttér.</span><span class="sxs-lookup"><span data-stu-id="12399-108">Once hello device is back online, these changes are synced with hello remote backend.</span></span>

<span data-ttu-id="12399-109">Ha ez az Azure Mobile Apps első élményét, először ki hello oktatóanyag [Android-alkalmazás létrehozása].</span><span class="sxs-lookup"><span data-stu-id="12399-109">If this is your first experience with Azure Mobile Apps, you should first complete hello tutorial [Create an Android App].</span></span> <span data-ttu-id="12399-110">Ha nem használ hello letöltése – első lépések, hello data access kiegészítő csomagok tooyour projekt hozzá kell adnia.</span><span class="sxs-lookup"><span data-stu-id="12399-110">If you do not use hello downloaded quick start server project, you must add hello data access extension packages tooyour project.</span></span> <span data-ttu-id="12399-111">Kiszolgáló bővítménycsomagok kapcsolatos további információkért lásd: [használható hello .NET háttérkiszolgáló SDK az Azure Mobile Apps a](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="12399-111">For more information about server extension packages, see [Work with hello .NET backend server SDK for Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).</span></span>

<span data-ttu-id="12399-112">További információ az hello kapcsolat nélküli szinkronizálás szolgáltatást, toolearn hello témakörben találhatók [az Azure Mobile Apps Offline adatszinkronizálás].</span><span class="sxs-lookup"><span data-stu-id="12399-112">toolearn more about hello offline sync feature, see hello topic [Offline Data Sync in Azure Mobile Apps].</span></span>

## <a name="update-hello-app-toosupport-offline-sync"></a><span data-ttu-id="12399-113">Hello app toosupport kapcsolat nélküli szinkronizálás frissítése</span><span class="sxs-lookup"><span data-stu-id="12399-113">Update hello app toosupport offline sync</span></span>
<span data-ttu-id="12399-114">A kapcsolat nélküli szinkronizálás, olvassa el a tooand írási egy *szinkronizálási tábla* (hello segítségével *IMobileServiceSyncTable* felület), amely része egy **SQLite** adatbázis az eszközön.</span><span class="sxs-lookup"><span data-stu-id="12399-114">With offline sync, you read tooand write from a *sync table* (using hello *IMobileServiceSyncTable* interface), which is part of a **SQLite** database on your device.</span></span>

<span data-ttu-id="12399-115">toopush és lekéréses módosítások hello eszköz és az Azure Mobile Services között, használhatja a *szinkronizálási környezetet* (*MobileServiceClient.SyncContext*), amely a helyi adatbázis hello inicializálása helyben toostore az adatokat.</span><span class="sxs-lookup"><span data-stu-id="12399-115">toopush and pull changes between hello device and Azure Mobile Services, you use a *synchronization context* (*MobileServiceClient.SyncContext*), which you initialize with hello local database toostore data locally.</span></span>

1. <span data-ttu-id="12399-116">A `TodoActivity.java`, meglévő definíciója hello megjegyzésbe `mToDoTable` hello szinkronizálási tábla verziója kódrészig, és:</span><span class="sxs-lookup"><span data-stu-id="12399-116">In `TodoActivity.java`, comment out hello existing definition of `mToDoTable` and uncomment hello sync table version:</span></span>
   
        private MobileServiceSyncTable<ToDoItem> mToDoTable;
2. <span data-ttu-id="12399-117">A hello `onCreate` módszer, meglévő inicializálása hello megjegyzésbe `mToDoTable` és törölje az ehhez a definícióhoz:</span><span class="sxs-lookup"><span data-stu-id="12399-117">In hello `onCreate` method, comment out hello existing initialization of `mToDoTable` and uncomment this definition:</span></span>
   
        mToDoTable = mClient.getSyncTable("ToDoItem", ToDoItem.class);
3. <span data-ttu-id="12399-118">A `refreshItemsFromTable` hello definíciója megjegyzésbe `results` és törölje az ehhez a definícióhoz:</span><span class="sxs-lookup"><span data-stu-id="12399-118">In `refreshItemsFromTable` comment out hello definition of `results` and uncomment this definition:</span></span>
   
        // Offline Sync
        final List<ToDoItem> results = refreshItemsFromMobileServiceTableSyncTable();
4. <span data-ttu-id="12399-119">Hello definíciója megjegyzésbe `refreshItemsFromMobileServiceTable`.</span><span class="sxs-lookup"><span data-stu-id="12399-119">Comment out hello definition of `refreshItemsFromMobileServiceTable`.</span></span>
5. <span data-ttu-id="12399-120">Állítsa vissza a hello definíciója `refreshItemsFromMobileServiceTableSyncTable`:</span><span class="sxs-lookup"><span data-stu-id="12399-120">Uncomment hello definition of `refreshItemsFromMobileServiceTableSyncTable`:</span></span>
   
        private List<ToDoItem> refreshItemsFromMobileServiceTableSyncTable() throws ExecutionException, InterruptedException {
            //sync hello data
            sync().get();
            Query query = QueryOperations.field("complete").
                    eq(val(false));
            return mToDoTable.read(query).get();
        }
6. <span data-ttu-id="12399-121">Állítsa vissza a hello definíciója `sync`:</span><span class="sxs-lookup"><span data-stu-id="12399-121">Uncomment hello definition of `sync`:</span></span>
   
        private AsyncTask<Void, Void, Void> sync() {
            AsyncTask<Void, Void, Void> task = new AsyncTask<Void, Void, Void>(){
                @Override
                protected Void doInBackground(Void... params) {
                    try {
                        MobileServiceSyncContext syncContext = mClient.getSyncContext();
                        syncContext.push().get();
                        mToDoTable.pull(null).get();
                    } catch (final Exception e) {
                        createAndShowDialogFromTask(e, "Error");
                    }
                    return null;
                }
            };
            return runAsyncTask(task);
        }

## <a name="test-hello-app"></a><span data-ttu-id="12399-122">Teszt hello alkalmazás</span><span class="sxs-lookup"><span data-stu-id="12399-122">Test hello app</span></span>
<span data-ttu-id="12399-123">Ebben a szakaszban hello működését Wi-Fi tesztelje a, és kapcsolja ki a Wi-Fi toocreate egy kapcsolat nélküli forgatókönyv.</span><span class="sxs-lookup"><span data-stu-id="12399-123">In this section, you test hello behavior with WiFi on, and then turn off WiFi toocreate an offline scenario.</span></span>

<span data-ttu-id="12399-124">Adatelemek hozzáadásakor tárolják őket hello helyi SQLite tárolóban, de nem szinkronizált toohello mobilszolgáltatás amíg hello nyomja le az ENTER **frissítése** gombra.</span><span class="sxs-lookup"><span data-stu-id="12399-124">When you add data items, they are held in hello local SQLite store, but not synced toohello mobile service until you press hello **Refresh** button.</span></span> <span data-ttu-id="12399-125">Más alkalmazások előfordulhat, hogy mikor adatoknak kell szinkronizálva toobe kapcsolatos eltérő követelmények vonatkoznak, de bemutató céljára ebben az oktatóanyagban rendelkezik explicit módon kérheti hello felhasználó.</span><span class="sxs-lookup"><span data-stu-id="12399-125">Other apps may have different requirements regarding when data needs toobe synchronized, but for demo purposes this tutorial has hello user explicitly request it.</span></span>

<span data-ttu-id="12399-126">A gomb megnyomásakor új háttérfeladat kezdődik.</span><span class="sxs-lookup"><span data-stu-id="12399-126">When you press that button, a new background task starts.</span></span> <span data-ttu-id="12399-127">Először az összes végrehajtott módosítások toohello helyi állapottárolójához szinkronizálási környezetet, majd Azure toohello helyi tábla összes ponttá megváltozott adatait leküldéses értesítések.</span><span class="sxs-lookup"><span data-stu-id="12399-127">It first pushes all changes made toohello local store using synchronization context, then pulls all changed data from Azure toohello local table.</span></span>

### <a name="offline-testing"></a><span data-ttu-id="12399-128">Kapcsolat nélküli tesztelése</span><span class="sxs-lookup"><span data-stu-id="12399-128">Offline testing</span></span>
1. <span data-ttu-id="12399-129">Hely hello eszköz vagy a szimulátor *repülési üzemmód*.</span><span class="sxs-lookup"><span data-stu-id="12399-129">Place hello device or simulator in *Airplane Mode*.</span></span> <span data-ttu-id="12399-130">Ezzel létrehoz egy kapcsolat nélküli forgatókönyv.</span><span class="sxs-lookup"><span data-stu-id="12399-130">This creates an offline scenario.</span></span>
2. <span data-ttu-id="12399-131">Adja hozzá az egyes *ToDo* elemeket, vagy be van jelölve befejezettként egyes elemek.</span><span class="sxs-lookup"><span data-stu-id="12399-131">Add some *ToDo* items, or mark some items as complete.</span></span> <span data-ttu-id="12399-132">Lépjen a hello eszköz vagy szimulátor (vagy a kényszerített bezárása hello app), és indítsa újra.</span><span class="sxs-lookup"><span data-stu-id="12399-132">Quit hello device or simulator (or forcibly close hello app) and restart.</span></span> <span data-ttu-id="12399-133">Győződjön meg arról, hogy a módosítások maradnak hello eszközön tárolják őket mert hello helyi SQLite tárolójában.</span><span class="sxs-lookup"><span data-stu-id="12399-133">Verify that your changes have been persisted on hello device because they are held in hello local SQLite store.</span></span>
3. <span data-ttu-id="12399-134">Hello Azure hello tartalmának megtekintése *TodoItem* például a tábla vagy egy SQL eszközzel *SQL Server Management Studio*, vagy egy REST-ügyfél, például *Fiddler* vagy  *Postman*.</span><span class="sxs-lookup"><span data-stu-id="12399-134">View hello contents of hello Azure *TodoItem* table either with a SQL tool such as *SQL Server Management Studio*, or a REST client such as *Fiddler* or *Postman*.</span></span> <span data-ttu-id="12399-135">Ellenőrizze, hogy rendelkezik-e új elemek hello *nem* lett szinkronizált toohello kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="12399-135">Verify that hello new items have *not* been synced toohello server</span></span>
   
       + <span data-ttu-id="12399-136">Node.js-háttéralkalmazáshoz, lépjen a toohello [Azure-portálon](https://portal.azure.com/), és a háttérkiszolgáló kattintson a mobilalkalmazás **könnyen táblák** > **TodoItem** hello tooviewhellotartalmát`TodoItem` tábla.</span><span class="sxs-lookup"><span data-stu-id="12399-136">For a Node.js backend, go toohello [Azure portal](https://portal.azure.com/), and in your Mobile App backend click **Easy Tables** > **TodoItem** tooview hello contents of hello `TodoItem` table.</span></span>
       + <span data-ttu-id="12399-137">A .NET-háttérrendszer, nézet hello tábla tartalmát vagy egy SQL eszközzel például *SQL Server Management Studio*, vagy egy REST-ügyfél, például *Fiddler* vagy *Postman*.</span><span class="sxs-lookup"><span data-stu-id="12399-137">For a .NET backend, view hello table contents either with a SQL tool such as *SQL Server Management Studio*, or a REST client such as *Fiddler* or *Postman*.</span></span>
4. <span data-ttu-id="12399-138">Kapcsolja be a Wi-Fi hello eszköz vagy szimulátor.</span><span class="sxs-lookup"><span data-stu-id="12399-138">Turn on WiFi in hello device or simulator.</span></span> <span data-ttu-id="12399-139">Nyomja meg az hello **frissítése** gombra.</span><span class="sxs-lookup"><span data-stu-id="12399-139">Next, press hello **Refresh** button.</span></span>
5. <span data-ttu-id="12399-140">Hello TodoItem adatainak megtekintéséhez újra hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="12399-140">View hello TodoItem data again in hello Azure portal.</span></span> <span data-ttu-id="12399-141">hello új és módosított TodoItems ekkor meg kell jelennie.</span><span class="sxs-lookup"><span data-stu-id="12399-141">hello new and changed TodoItems should now appear.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="12399-142">További források</span><span class="sxs-lookup"><span data-stu-id="12399-142">Additional Resources</span></span>
* <span data-ttu-id="12399-143">[az Azure Mobile Apps Offline adatszinkronizálás]</span><span class="sxs-lookup"><span data-stu-id="12399-143">[Offline Data Sync in Azure Mobile Apps]</span></span>
* <span data-ttu-id="12399-144">[Felhő tartalma: Kapcsolat nélküli szinkronizálás az Azure Mobile Services] \(Megjegyzés: a Mobile Services hello video van, de a kapcsolat nélküli szinkronizálás az Azure Mobile Apps hasonló módon működik\)</span><span class="sxs-lookup"><span data-stu-id="12399-144">[Cloud Cover: Offline Sync in Azure Mobile Services] \(note: hello video is on Mobile Services, but offline sync works in a similar way in Azure Mobile Apps\)</span></span>

<!-- URLs. -->

[az Azure Mobile Apps Offline adatszinkronizálás]: app-service-mobile-offline-data-sync.md

[Android-alkalmazás létrehozása]: app-service-mobile-android-get-started.md

[Felhő tartalma: Kapcsolat nélküli szinkronizálás az Azure Mobile Services]: http://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri
[Azure Friday: Offline-enabled apps in Azure Mobile Services]: http://azure.microsoft.com/documentation/videos/azure-mobile-services-offline-enabled-apps-with-donna-malayeri/

