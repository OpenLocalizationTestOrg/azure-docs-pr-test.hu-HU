---
title: "Kapcsolat nélküli szinkronizálásának engedélyezése az Azure Mobile alkalmazások (Android)"
description: "App Service Mobile Apps az Android-alkalmazás gyorsítótárába, a szinkronizálási kapcsolat nélküli adatainak használata"
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
ms.openlocfilehash: 304323c1816302e8c1f68f36a029aee55e02c54e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="enable-offline-sync-for-your-android-mobile-app"></a><span data-ttu-id="f2618-103">Androidos mobilalkalmazás kapcsolat nélküli szinkronizálásának engedélyezése</span><span class="sxs-lookup"><span data-stu-id="f2618-103">Enable offline sync for your Android mobile app</span></span>
[!INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a><span data-ttu-id="f2618-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="f2618-104">Overview</span></span>
<span data-ttu-id="f2618-105">Ez az oktatóanyag az Azure Mobile Apps a kapcsolat nélküli szinkronizálás szolgáltatása Android ismerteti.</span><span class="sxs-lookup"><span data-stu-id="f2618-105">This tutorial covers the offline sync feature of Azure Mobile Apps for Android.</span></span> <span data-ttu-id="f2618-106">Kapcsolat nélküli szinkronizálás lehetővé teszi, hogy a végfelhasználók számára a mobilalkalmazás együttműködhet&mdash;megtekintését, hozzáadását és módosítását adatok&mdash;akkor is, ha nincs hálózati kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="f2618-106">Offline sync allows end users to interact with a mobile app&mdash;viewing, adding, or modifying data&mdash;even when there is no network connection.</span></span> <span data-ttu-id="f2618-107">Változások a helyi adatbázisban tárolódnak.</span><span class="sxs-lookup"><span data-stu-id="f2618-107">Changes are stored in a local database.</span></span> <span data-ttu-id="f2618-108">Az eszköz újra online állapotba kerül, ha ezek a változások szinkronizálása megtörtént-e a távoli háttér.</span><span class="sxs-lookup"><span data-stu-id="f2618-108">Once the device is back online, these changes are synced with the remote backend.</span></span>

<span data-ttu-id="f2618-109">Ha ez az Azure Mobile Apps első élményét, először ki az oktatóanyag [Android-alkalmazás létrehozása].</span><span class="sxs-lookup"><span data-stu-id="f2618-109">If this is your first experience with Azure Mobile Apps, you should first complete the tutorial [Create an Android App].</span></span> <span data-ttu-id="f2618-110">Ha nem használja a letöltött gyors üzembe helyezési kiszolgálóprojektet, hozzá kell adnia a hozzáférési adatok bővítménycsomagok a projekthez.</span><span class="sxs-lookup"><span data-stu-id="f2618-110">If you do not use the downloaded quick start server project, you must add the data access extension packages to your project.</span></span> <span data-ttu-id="f2618-111">Kiszolgáló bővítménycsomagok kapcsolatos további információkért lásd: [használható a .NET-háttérrendszer server SDK az Azure Mobile Apps a](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="f2618-111">For more information about server extension packages, see [Work with the .NET backend server SDK for Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).</span></span>

<span data-ttu-id="f2618-112">A kapcsolat nélküli szinkronizálás szolgáltatással kapcsolatos további tudnivalókért lásd a témakör [az Azure Mobile Apps Offline adatszinkronizálás].</span><span class="sxs-lookup"><span data-stu-id="f2618-112">To learn more about the offline sync feature, see the topic [Offline Data Sync in Azure Mobile Apps].</span></span>

## <a name="update-the-app-to-support-offline-sync"></a><span data-ttu-id="f2618-113">Kapcsolat nélküli szinkronizálás támogatásához az alkalmazás frissítésére</span><span class="sxs-lookup"><span data-stu-id="f2618-113">Update the app to support offline sync</span></span>
<span data-ttu-id="f2618-114">Kapcsolat nélküli szinkronizálás, az olvasási és írási egy *szinkronizálási tábla* (használatával a *IMobileServiceSyncTable* interface), amely része egy **SQLite** adatbázis az eszközön.</span><span class="sxs-lookup"><span data-stu-id="f2618-114">With offline sync, you read to and write from a *sync table* (using the *IMobileServiceSyncTable* interface), which is part of a **SQLite** database on your device.</span></span>

<span data-ttu-id="f2618-115">Leküldéses és lekéréses módosítások között az eszköz és az Azure Mobile Services, használja a *szinkronizálási környezetet* (*MobileServiceClient.SyncContext*), amely a helyi adatbázisban tárolják a inicializálása helyi adatok.</span><span class="sxs-lookup"><span data-stu-id="f2618-115">To push and pull changes between the device and Azure Mobile Services, you use a *synchronization context* (*MobileServiceClient.SyncContext*), which you initialize with the local database to store data locally.</span></span>

1. <span data-ttu-id="f2618-116">A `TodoActivity.java`, a meglévő definíciót megjegyzésbe `mToDoTable` és állítsa vissza a szinkronizálási tábla verziója:</span><span class="sxs-lookup"><span data-stu-id="f2618-116">In `TodoActivity.java`, comment out the existing definition of `mToDoTable` and uncomment the sync table version:</span></span>
   
        private MobileServiceSyncTable<ToDoItem> mToDoTable;
2. <span data-ttu-id="f2618-117">Az a `onCreate` metódus, a meglévő inicializálása megjegyzésbe `mToDoTable` és törölje az ehhez a definícióhoz:</span><span class="sxs-lookup"><span data-stu-id="f2618-117">In the `onCreate` method, comment out the existing initialization of `mToDoTable` and uncomment this definition:</span></span>
   
        mToDoTable = mClient.getSyncTable("ToDoItem", ToDoItem.class);
3. <span data-ttu-id="f2618-118">A `refreshItemsFromTable` definíciója megjegyzésbe `results` és törölje az ehhez a definícióhoz:</span><span class="sxs-lookup"><span data-stu-id="f2618-118">In `refreshItemsFromTable` comment out the definition of `results` and uncomment this definition:</span></span>
   
        // Offline Sync
        final List<ToDoItem> results = refreshItemsFromMobileServiceTableSyncTable();
4. <span data-ttu-id="f2618-119">Definíciója megjegyzésbe `refreshItemsFromMobileServiceTable`.</span><span class="sxs-lookup"><span data-stu-id="f2618-119">Comment out the definition of `refreshItemsFromMobileServiceTable`.</span></span>
5. <span data-ttu-id="f2618-120">Állítsa vissza a definíciója `refreshItemsFromMobileServiceTableSyncTable`:</span><span class="sxs-lookup"><span data-stu-id="f2618-120">Uncomment the definition of `refreshItemsFromMobileServiceTableSyncTable`:</span></span>
   
        private List<ToDoItem> refreshItemsFromMobileServiceTableSyncTable() throws ExecutionException, InterruptedException {
            //sync the data
            sync().get();
            Query query = QueryOperations.field("complete").
                    eq(val(false));
            return mToDoTable.read(query).get();
        }
6. <span data-ttu-id="f2618-121">Állítsa vissza a definíciója `sync`:</span><span class="sxs-lookup"><span data-stu-id="f2618-121">Uncomment the definition of `sync`:</span></span>
   
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

## <a name="test-the-app"></a><span data-ttu-id="f2618-122">Az alkalmazás tesztelése</span><span class="sxs-lookup"><span data-stu-id="f2618-122">Test the app</span></span>
<span data-ttu-id="f2618-123">Ebben a szakaszban a működését Wi-Fi tesztelje a, és kapcsolja ki a Wi-Fi egy kapcsolat nélküli forgatókönyv létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="f2618-123">In this section, you test the behavior with WiFi on, and then turn off WiFi to create an offline scenario.</span></span>

<span data-ttu-id="f2618-124">Adatelemek hozzáadásakor használatban a helyi tárolóból. SQLite, de nincs szinkronizálva a mobilszolgáltatást, amíg lenyomja az a **frissítése** gombra.</span><span class="sxs-lookup"><span data-stu-id="f2618-124">When you add data items, they are held in the local SQLite store, but not synced to the mobile service until you press the **Refresh** button.</span></span> <span data-ttu-id="f2618-125">Más alkalmazások előfordulhat, hogy amikor adatokat szinkronizálni kell különböző követelményeit, de bemutató céljára ebben az oktatóanyagban a felhasználónak explicit módon kérheti rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="f2618-125">Other apps may have different requirements regarding when data needs to be synchronized, but for demo purposes this tutorial has the user explicitly request it.</span></span>

<span data-ttu-id="f2618-126">A gomb megnyomásakor új háttérfeladat kezdődik.</span><span class="sxs-lookup"><span data-stu-id="f2618-126">When you press that button, a new background task starts.</span></span> <span data-ttu-id="f2618-127">Először a helyi tárolójába szinkronizálási környezetet, akkor minden ponttá módosult adatokat az Azure-ból a helyi táblába használatával végrehajtott valamennyi módosítást leküldéses értesítések.</span><span class="sxs-lookup"><span data-stu-id="f2618-127">It first pushes all changes made to the local store using synchronization context, then pulls all changed data from Azure to the local table.</span></span>

### <a name="offline-testing"></a><span data-ttu-id="f2618-128">Kapcsolat nélküli tesztelése</span><span class="sxs-lookup"><span data-stu-id="f2618-128">Offline testing</span></span>
1. <span data-ttu-id="f2618-129">Helyezze el az eszköz vagy a szimulátor *repülési üzemmód*.</span><span class="sxs-lookup"><span data-stu-id="f2618-129">Place the device or simulator in *Airplane Mode*.</span></span> <span data-ttu-id="f2618-130">Ezzel létrehoz egy kapcsolat nélküli forgatókönyv.</span><span class="sxs-lookup"><span data-stu-id="f2618-130">This creates an offline scenario.</span></span>
2. <span data-ttu-id="f2618-131">Adja hozzá az egyes *ToDo* elemeket, vagy be van jelölve befejezettként egyes elemek.</span><span class="sxs-lookup"><span data-stu-id="f2618-131">Add some *ToDo* items, or mark some items as complete.</span></span> <span data-ttu-id="f2618-132">Lépjen ki az eszköz vagy szimulátor (vagy a kényszerített zárja be az alkalmazást), és indítsa újra.</span><span class="sxs-lookup"><span data-stu-id="f2618-132">Quit the device or simulator (or forcibly close the app) and restart.</span></span> <span data-ttu-id="f2618-133">Győződjön meg arról, hogy a módosítások maradnak az eszközön mert tárolják őket a helyi SQLite-tárolóban.</span><span class="sxs-lookup"><span data-stu-id="f2618-133">Verify that your changes have been persisted on the device because they are held in the local SQLite store.</span></span>
3. <span data-ttu-id="f2618-134">Az Azure tartalmának megtekintése *TodoItem* például a tábla vagy egy SQL eszközzel *SQL Server Management Studio*, vagy egy REST-ügyfél, például *Fiddler* vagy  *Postman*.</span><span class="sxs-lookup"><span data-stu-id="f2618-134">View the contents of the Azure *TodoItem* table either with a SQL tool such as *SQL Server Management Studio*, or a REST client such as *Fiddler* or *Postman*.</span></span> <span data-ttu-id="f2618-135">Ellenőrizze, hogy rendelkezik-e az új elemek *nem* lett-e szinkronizálva a kiszolgálóhoz</span><span class="sxs-lookup"><span data-stu-id="f2618-135">Verify that the new items have *not* been synced to the server</span></span>
   
       + <span data-ttu-id="f2618-136">Node.js-háttéralkalmazáshoz, látogasson el a [Azure-portálon](https://portal.azure.com/), és a háttérkiszolgáló kattintson a mobilalkalmazás **könnyen táblák** > **TodoItem** a tartalmánakmegtekintéséhez`TodoItem`tábla.</span><span class="sxs-lookup"><span data-stu-id="f2618-136">For a Node.js backend, go to the [Azure portal](https://portal.azure.com/), and in your Mobile App backend click **Easy Tables** > **TodoItem** to view the contents of the `TodoItem` table.</span></span>
       + <span data-ttu-id="f2618-137">A .NET-háttérrendszer, tekintse meg a tábla tartalmát, vagy egy SQL eszközzel, mint *SQL Server Management Studio*, vagy egy REST-ügyfél, például *Fiddler* vagy *Postman*.</span><span class="sxs-lookup"><span data-stu-id="f2618-137">For a .NET backend, view the table contents either with a SQL tool such as *SQL Server Management Studio*, or a REST client such as *Fiddler* or *Postman*.</span></span>
4. <span data-ttu-id="f2618-138">Kapcsolja be a Wi-Fi az eszköz vagy szimulátor.</span><span class="sxs-lookup"><span data-stu-id="f2618-138">Turn on WiFi in the device or simulator.</span></span> <span data-ttu-id="f2618-139">Nyomja meg a **frissítése** gombra.</span><span class="sxs-lookup"><span data-stu-id="f2618-139">Next, press the **Refresh** button.</span></span>
5. <span data-ttu-id="f2618-140">A TodoItem adatainak megtekintéséhez újra az Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="f2618-140">View the TodoItem data again in the Azure portal.</span></span> <span data-ttu-id="f2618-141">Az új és módosított TodoItems ekkor meg kell jelennie.</span><span class="sxs-lookup"><span data-stu-id="f2618-141">The new and changed TodoItems should now appear.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f2618-142">További források</span><span class="sxs-lookup"><span data-stu-id="f2618-142">Additional Resources</span></span>
* <span data-ttu-id="f2618-143">[az Azure Mobile Apps Offline adatszinkronizálás]</span><span class="sxs-lookup"><span data-stu-id="f2618-143">[Offline Data Sync in Azure Mobile Apps]</span></span>
* <span data-ttu-id="f2618-144">[Felhő tartalma: Kapcsolat nélküli szinkronizálás az Azure Mobile Services] \(Megjegyzés: a videó megtalálható-e a Mobile Services, de a kapcsolat nélküli szinkronizálás az Azure Mobile Apps hasonló módon működik\)</span><span class="sxs-lookup"><span data-stu-id="f2618-144">[Cloud Cover: Offline Sync in Azure Mobile Services] \(note: the video is on Mobile Services, but offline sync works in a similar way in Azure Mobile Apps\)</span></span>

<!-- URLs. -->

<span data-ttu-id="f2618-145">[az Azure Mobile Apps Offline adatszinkronizálás]: app-service-mobile-offline-data-sync.md</span><span class="sxs-lookup"><span data-stu-id="f2618-145">[Offline Data Sync in Azure Mobile Apps]: app-service-mobile-offline-data-sync.md</span></span>

<span data-ttu-id="f2618-146">[Android-alkalmazás létrehozása]: app-service-mobile-android-get-started.md</span><span class="sxs-lookup"><span data-stu-id="f2618-146">[Create an Android App]: app-service-mobile-android-get-started.md</span></span>

<span data-ttu-id="f2618-147">[Felhő tartalma: Kapcsolat nélküli szinkronizálás az Azure Mobile Services]: http://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri</span><span class="sxs-lookup"><span data-stu-id="f2618-147">[Cloud Cover: Offline Sync in Azure Mobile Services]: http://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri</span></span>
[Azure Friday: Offline-enabled apps in Azure Mobile Services]: http://azure.microsoft.com/documentation/videos/azure-mobile-services-offline-enabled-apps-with-donna-malayeri/

