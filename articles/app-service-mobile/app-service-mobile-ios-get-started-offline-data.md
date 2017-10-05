---
title: "Az iOS-mobilalkalmazások kapcsolat nélküli szinkronizálás engedélyezése |} Microsoft Docs"
description: "Ismerje meg az iOS-alkalmazások az Azure App Service mobile apps szolgáltatásban való gyorsítótár és a szinkronizálási kapcsolat nélküli használata."
documentationcenter: ios
author: ggailey777
manager: syntaxc4
editor: 
services: app-service\mobile
ms.assetid: eb5b9520-0f39-4a09-940a-dadb6d940db8
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: 44c0d26b2d7d28322d436d4bda319d728c31a635
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="enable-offline-syncing-with-ios-mobile-apps"></a><span data-ttu-id="024ef-103">Az iOS-mobilalkalmazások kapcsolat nélküli szinkronizálás engedélyezése</span><span class="sxs-lookup"><span data-stu-id="024ef-103">Enable offline syncing with iOS mobile apps</span></span>
[!INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a><span data-ttu-id="024ef-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="024ef-104">Overview</span></span>
<span data-ttu-id="024ef-105">Ez az oktatóanyag bemutatja, kapcsolat nélküli szinkronizálás a iOS rendszerhez készült Azure App Service Mobile Apps szolgáltatásának.</span><span class="sxs-lookup"><span data-stu-id="024ef-105">This tutorial covers offline syncing with the Mobile Apps feature of Azure App Service for iOS.</span></span> <span data-ttu-id="024ef-106">A kapcsolat nélküli szinkronizálja végfelhasználókkal kezelheti a mobilalkalmazások megtekintését, vegye fel, és módosíthatják az adatokat, akkor is, ha nincs hálózati kapcsolat van.</span><span class="sxs-lookup"><span data-stu-id="024ef-106">With offline syncing end-users can interact with a mobile app to view, add, or modify data, even when they have no network connection.</span></span> <span data-ttu-id="024ef-107">Változások a helyi adatbázisban tárolódnak.</span><span class="sxs-lookup"><span data-stu-id="024ef-107">Changes are stored in a local database.</span></span> <span data-ttu-id="024ef-108">Miután az eszköz újra online állapotba kerül, a változások szinkronizálása megtörtént-e a távoli háttér.</span><span class="sxs-lookup"><span data-stu-id="024ef-108">After the device is back online, the changes are synced with the remote back end.</span></span>

<span data-ttu-id="024ef-109">Ha ez a Mobile Apps első élményét, először ki az oktatóanyag [iOS-alkalmazás létrehozása].</span><span class="sxs-lookup"><span data-stu-id="024ef-109">If this is your first experience with Mobile Apps, you should first complete the tutorial [Create an iOS App].</span></span> <span data-ttu-id="024ef-110">Ha nem használja a letöltött gyors üzembe helyezési kiszolgálóprojektet, hozzá kell adnia a adatelérési bővítménycsomagok a projekthez.</span><span class="sxs-lookup"><span data-stu-id="024ef-110">If you do not use the downloaded quick-start server project, you must add the data-access extension packages to your project.</span></span> <span data-ttu-id="024ef-111">Kiszolgáló bővítménycsomagok kapcsolatos további információkért lásd: [használható a .NET-háttérrendszer server SDK az Azure Mobile Apps a](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="024ef-111">For more information about server extension packages, see [Work with the .NET backend server SDK for Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).</span></span>

<span data-ttu-id="024ef-112">A kapcsolat nélküli szinkronizálás szolgáltatással kapcsolatos további tudnivalókért lásd: [Mobile Apps az Offline adatszinkronizálás].</span><span class="sxs-lookup"><span data-stu-id="024ef-112">To learn more about the offline sync feature, see [Offline Data Sync in Mobile Apps].</span></span>

## <span data-ttu-id="024ef-113"><a name="review-sync"></a>Tekintse át az Ügyfélkód szinkronizálása</span><span class="sxs-lookup"><span data-stu-id="024ef-113"><a name="review-sync"></a>Review the client sync code</span></span>
<span data-ttu-id="024ef-114">A letöltött ügyfélprojekt a [iOS-alkalmazás létrehozása] oktatóanyag már kódot tartalmaz, amely támogatja a helyi adatok Core-alapú adatbázis kapcsolat nélküli szinkronizálás.</span><span class="sxs-lookup"><span data-stu-id="024ef-114">The client project that you downloaded for the [Create an iOS App] tutorial already contains code that supports offline synchronization using a local Core Data-based database.</span></span> <span data-ttu-id="024ef-115">Ez a szakasz foglalja az oktatóanyag kódot már tartalmát.</span><span class="sxs-lookup"><span data-stu-id="024ef-115">This section summarizes what is already included in the tutorial code.</span></span> <span data-ttu-id="024ef-116">A szolgáltatás elméleti áttekintését lásd: [Mobile Apps az Offline adatszinkronizálás].</span><span class="sxs-lookup"><span data-stu-id="024ef-116">For a conceptual overview of the feature, see [Offline Data Sync in Mobile Apps].</span></span>

<span data-ttu-id="024ef-117">Mobile Apps az offline adatszinkronizálás szolgáltatásával, végfelhasználók használhatják a helyi adatbázis akkor is, ha a hálózat nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="024ef-117">Using the offline data-sync feature of Mobile Apps, end-users can interact with a local database even when the network is inaccessible.</span></span> <span data-ttu-id="024ef-118">Ezeket a szolgáltatásokat az alkalmazás használatához inicializálni a szinkronizálási kontextusában `MSClient` , és a helyi tárolójába hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="024ef-118">To use these features in your app, you initialize the sync context of `MSClient` and reference a local store.</span></span> <span data-ttu-id="024ef-119">Ezt követően a tábla használatával hivatkozik a **MSSyncTable** felületet.</span><span class="sxs-lookup"><span data-stu-id="024ef-119">Then you reference your table through the **MSSyncTable** interface.</span></span>

<span data-ttu-id="024ef-120">A **QSTodoService.m** (Objective-C) vagy **ToDoTableViewController.swift** (Swift), figyelje meg, hogy a tag típusával **syncTable** van **MSSyncTable** .</span><span class="sxs-lookup"><span data-stu-id="024ef-120">In **QSTodoService.m** (Objective-C) or **ToDoTableViewController.swift** (Swift), notice that the type of the member **syncTable** is **MSSyncTable**.</span></span> <span data-ttu-id="024ef-121">A szinkronizálási tábla felületet helyett használja a kapcsolat nélküli szinkronizálás **MSTable**.</span><span class="sxs-lookup"><span data-stu-id="024ef-121">Offline sync uses this sync table interface instead of **MSTable**.</span></span> <span data-ttu-id="024ef-122">Szinkronizálási tábla használata esetén az összes művelet nyissa meg a helyi tárolójába, és csak a távoli háttérrendszerének integrációját explicit eseménylekérési és eseményküldési műveletek szinkronizálva.</span><span class="sxs-lookup"><span data-stu-id="024ef-122">When a sync table is used, all operations go to the local store and are synchronized only with the remote back end with explicit push and pull operations.</span></span>

 <span data-ttu-id="024ef-123">A szinkronizálási hivatkozás beszerzéséhez használja a **syncTableWithName** metódusa `MSClient`.</span><span class="sxs-lookup"><span data-stu-id="024ef-123">To get a reference to a sync table, use the **syncTableWithName** method on `MSClient`.</span></span> <span data-ttu-id="024ef-124">Kapcsolat nélküli szinkronizálásának eltávolításához használja **tableWithName** helyette.</span><span class="sxs-lookup"><span data-stu-id="024ef-124">To remove offline sync functionality, use **tableWithName** instead.</span></span>

<span data-ttu-id="024ef-125">Mielőtt bármely tábla művelet végrehajtható, a helyi tárolójába inicializálni kell.</span><span class="sxs-lookup"><span data-stu-id="024ef-125">Before any table operations can be performed, the local store must be initialized.</span></span> <span data-ttu-id="024ef-126">A megfelelő kód itt látható:</span><span class="sxs-lookup"><span data-stu-id="024ef-126">Here is the relevant code:</span></span>

* <span data-ttu-id="024ef-127">**Objective-C**.</span><span class="sxs-lookup"><span data-stu-id="024ef-127">**Objective-C**.</span></span> <span data-ttu-id="024ef-128">Az a **QSTodoService.init** módszert:</span><span class="sxs-lookup"><span data-stu-id="024ef-128">In the **QSTodoService.init** method:</span></span>

   ```objc
   MSCoreDataStore *store = [[MSCoreDataStore alloc] initWithManagedObjectContext:context];
   self.client.syncContext = [[MSSyncContext alloc] initWithDelegate:nil dataSource:store callback:nil];
   ```    
* <span data-ttu-id="024ef-129">**SWIFT**.</span><span class="sxs-lookup"><span data-stu-id="024ef-129">**Swift**.</span></span> <span data-ttu-id="024ef-130">Az a **ToDoTableViewController.viewDidLoad** módszert:</span><span class="sxs-lookup"><span data-stu-id="024ef-130">In the **ToDoTableViewController.viewDidLoad** method:</span></span>

   ```swift
   let client = MSClient(applicationURLString: "http:// ...") // URI of the Mobile App
   let managedObjectContext = (UIApplication.sharedApplication().delegate as! AppDelegate).managedObjectContext!
   self.store = MSCoreDataStore(managedObjectContext: managedObjectContext)
   client.syncContext = MSSyncContext(delegate: nil, dataSource: self.store, callback: nil)
   ```
   <span data-ttu-id="024ef-131">Ez a módszer használatával hozza létre a helyi tárolójába a `MSCoreDataStore` felület, amely a Mobile Apps SDK biztosít.</span><span class="sxs-lookup"><span data-stu-id="024ef-131">This method creates a local store by using the `MSCoreDataStore` interface, which the Mobile Apps SDK provides.</span></span> <span data-ttu-id="024ef-132">Azt is megteheti, megadhat egy másik helyi tároló alkalmazásával a `MSSyncContextDataSource` protokoll.</span><span class="sxs-lookup"><span data-stu-id="024ef-132">Alternatively, you can provide a different local store by implementing the `MSSyncContextDataSource` protocol.</span></span> <span data-ttu-id="024ef-133">Az is, az első paraméter **MSSyncContext** ütközés kezelő megadására szolgál.</span><span class="sxs-lookup"><span data-stu-id="024ef-133">Also, the first parameter of **MSSyncContext** is used to specify a conflict handler.</span></span> <span data-ttu-id="024ef-134">Mivel jelenleg átadott `nil`, azt lekérése sikertelen ütközés alapértelmezett ütközés kezelő.</span><span class="sxs-lookup"><span data-stu-id="024ef-134">Because we have passed `nil`, we get the default conflict handler, which fails on any conflict.</span></span>

<span data-ttu-id="024ef-135">Most tegyük a tényleges szinkronizálási művelet végrehajtására, és adatokat lekérni a távoli háttér:</span><span class="sxs-lookup"><span data-stu-id="024ef-135">Now, let's perform the actual sync operation, and get data from the remote back end:</span></span>

* <span data-ttu-id="024ef-136">**Objective-C**.</span><span class="sxs-lookup"><span data-stu-id="024ef-136">**Objective-C**.</span></span> <span data-ttu-id="024ef-137">`syncData`először leküldéses értesítések a módosításokat, és ekkor meghívja a **pullData** a távoli háttérből adatok eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="024ef-137">`syncData` first pushes new changes and then calls **pullData** to get data from the remote back end.</span></span> <span data-ttu-id="024ef-138">Viszont a **pullData** metódus lekéri az új lekérdezés adatokat:</span><span class="sxs-lookup"><span data-stu-id="024ef-138">In turn, the **pullData** method gets new data that matches a query:</span></span>

   ```objc
   -(void)syncData:(QSCompletionBlock)completion
   {
       // Push all changes in the sync context, and then pull new data.
       [self.client.syncContext pushWithCompletion:^(NSError *error) {
           [self logErrorIfNotNil:error];
           [self pullData:completion];
       }];
   }

   -(void)pullData:(QSCompletionBlock)completion
   {
       MSQuery *query = [self.syncTable query];

       // Pulls data from the remote server into the local table.
       // We're pulling all items and filtering in the view.
       // Query ID is used for incremental sync.
       [self.syncTable pullWithQuery:query queryId:@"allTodoItems" completion:^(NSError *error) {
           [self logErrorIfNotNil:error];

           // Lets the caller know that we have finished.
           if (completion != nil) {
               dispatch_async(dispatch_get_main_queue(), completion);
           }
       }];
   }
   ```
* <span data-ttu-id="024ef-139">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="024ef-139">**Swift**:</span></span>
   ```swift
   func onRefresh(sender: UIRefreshControl!) {
      UIApplication.sharedApplication().networkActivityIndicatorVisible = true

      self.table!.pullWithQuery(self.table?.query(), queryId: "AllRecords") {
          (error) -> Void in

          UIApplication.sharedApplication().networkActivityIndicatorVisible = false

          if error != nil {
              // A real application would handle various errors like network conditions,
              // server conflicts, etc via the MSSyncContextDelegate
              print("Error: \(error!.description)")

              // We will discard our changes and keep the server's copy for simplicity
              if let opErrors = error!.userInfo[MSErrorPushResultKey] as? Array<MSTableOperationError> {
                  for opError in opErrors {
                      print("Attempted operation to item \(opError.itemId)")
                      if (opError.operation == .Insert || opError.operation == .Delete) {
                          print("Insert/Delete, failed discarding changes")
                          opError.cancelOperationAndDiscardItemWithCompletion(nil)
                      } else {
                          print("Update failed, reverting to server's copy")
                          opError.cancelOperationAndUpdateItem(opError.serverItem!, completion: nil)
                      }
                  }
              }
          }
          self.refreshControl?.endRefreshing()
      }
   }
   ```

<span data-ttu-id="024ef-140">Az Objective-C verzióban a `syncData`, első hívás **pushWithCompletion** sync-környezetében.</span><span class="sxs-lookup"><span data-stu-id="024ef-140">In the Objective-C version, in `syncData`, we first call **pushWithCompletion** on the sync context.</span></span> <span data-ttu-id="024ef-141">Ez a módszer egy tagja `MSSyncContext` (és nem magára szinkronizálási táblázat), mert azt módosítások leküldéses értesítések összes táblák között.</span><span class="sxs-lookup"><span data-stu-id="024ef-141">This method is a member of `MSSyncContext` (and not the sync table itself) because it pushes changes across all tables.</span></span> <span data-ttu-id="024ef-142">Csak azt jelzi, hogy helyi (CUD műveletek) révén valamilyen módon módosítva lett a kiszolgálón kerülnek.</span><span class="sxs-lookup"><span data-stu-id="024ef-142">Only records that have been modified in some way locally (through CUD operations) are sent to the server.</span></span> <span data-ttu-id="024ef-143">Ezután a segítő **pullData** neve, mely hívások **MSSyncTable.pullWithQuery** távoli adatok lekéréséhez és a helyi adatbázisban tárolja.</span><span class="sxs-lookup"><span data-stu-id="024ef-143">Then the helper **pullData** is called, which calls **MSSyncTable.pullWithQuery** to retrieve remote data and store it in the local database.</span></span>

<span data-ttu-id="024ef-144">A Swift verziójában a leküldéses művelet volt nem feltétlenül szükséges, mert nincs nincs hívása **pushWithCompletion**.</span><span class="sxs-lookup"><span data-stu-id="024ef-144">In the Swift version, because the push operation was not strictly necessary, there is no call to **pushWithCompletion**.</span></span> <span data-ttu-id="024ef-145">Ha a szinkronizálás a környezetben a következő táblázatban, amely egy leküldéses művelet függőben lévő módosításokat, lekéréses mindig kibocsát egy leküldéses először.</span><span class="sxs-lookup"><span data-stu-id="024ef-145">If there are any changes pending in the sync context for the table that is doing a push operation, pull always issues a push first.</span></span> <span data-ttu-id="024ef-146">Azonban ha több szinkronizálási tábla, célszerű explicit módon hívja az leküldéses annak biztosításához, hogy minden konzisztens kapcsolódó táblák között.</span><span class="sxs-lookup"><span data-stu-id="024ef-146">However, if you have more than one sync table, it is best to explicitly call push to ensure that everything is consistent across related tables.</span></span>

<span data-ttu-id="024ef-147">Az Objective-C és Swift verziók, használhatja a **pullWithQuery** adjon meg egy lekérdezést a rekordok szűrése a keresett metódust.</span><span class="sxs-lookup"><span data-stu-id="024ef-147">In both the Objective-C and Swift versions, you can use the **pullWithQuery** method to specify a query to filter the records you want to retrieve.</span></span> <span data-ttu-id="024ef-148">Ebben a példában a lekérdezés lekéri a távoli minden rekordot `TodoItem` tábla.</span><span class="sxs-lookup"><span data-stu-id="024ef-148">In this example, the query retrieves all records in the remote `TodoItem` table.</span></span>

<span data-ttu-id="024ef-149">A második paramétere **pullWithQuery** használt lekérdezés azonosító *növekményes szinkronizálás*.</span><span class="sxs-lookup"><span data-stu-id="024ef-149">The second parameter of **pullWithQuery** is a query ID that is used for *incremental sync*.</span></span> <span data-ttu-id="024ef-150">A növekményes szinkronizálás csak azt jelzi, hogy módosultak a rekord, a legutóbbi szinkronizálás óta lekéri `UpdatedAt` időbélyegzője (nevű `updatedAt` a helyi tárolóban levő.) A lekérdezés Azonosítóját, amely egyedi az alkalmazás minden logikai lekérdezés leíró karakterláncnak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="024ef-150">Incremental sync retrieves only records that were modified since the last sync, using the record's `UpdatedAt` time stamp (called `updatedAt` in the local store.) The query ID should be a descriptive string that is unique for each logical query in your app.</span></span> <span data-ttu-id="024ef-151">A növekményes szinkronizálás megadásának lehetőségét, adja át `nil` , a lekérdezés azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="024ef-151">To opt out of incremental sync, pass `nil` as the query ID.</span></span> <span data-ttu-id="024ef-152">Ezt a módszert potenciálisan nem hatékony, lehet, mert az összes rekord lévő összes lekéréses művelet lekérdezi.</span><span class="sxs-lookup"><span data-stu-id="024ef-152">This approach can be potentially inefficient, because it retrieves all records on each pull operation.</span></span>

<span data-ttu-id="024ef-153">Az Objective-C alkalmazás szinkronizálásának módosítása vagy hozzáadása adatokat, amikor egy felhasználó a frissítési kézmozdulat végez, és indítsa el.</span><span class="sxs-lookup"><span data-stu-id="024ef-153">The Objective-C app syncs when you modify or add data, when a user performs the refresh gesture, and on launch.</span></span>

<span data-ttu-id="024ef-154">A Swift app szinkronizálja, amikor a felhasználó hajt végre a frissítési hitelesítési mód, és indítsa el a.</span><span class="sxs-lookup"><span data-stu-id="024ef-154">The Swift app syncs when the user performs the refresh gesture and on launch.</span></span>

<span data-ttu-id="024ef-155">Mivel az alkalmazás szinkronizálások mindig, amikor adatokat módosítva (Objective-C), vagy amikor az alkalmazás indítása (Objective-C és Swift), az alkalmazás azt feltételezi, hogy a felhasználó online.</span><span class="sxs-lookup"><span data-stu-id="024ef-155">Because the app syncs whenever data is modified (Objective-C) or whenever the app starts (Objective-C and Swift), the app assumes that the user is online.</span></span> <span data-ttu-id="024ef-156">Egy későbbi szakasz ismerteti hogy frissíteni fogja az alkalmazást úgy, hogy a felhasználók akkor is, ha a kapcsolat nélküli szerkesztheti.</span><span class="sxs-lookup"><span data-stu-id="024ef-156">In a later section, you will update the app so that users can edit even when they are offline.</span></span>

## <span data-ttu-id="024ef-157"><a name="review-core-data"></a>Tekintse át az alapvető adatokat az adatmodellbe</span><span class="sxs-lookup"><span data-stu-id="024ef-157"><a name="review-core-data"></a>Review the Core Data model</span></span>
<span data-ttu-id="024ef-158">Az alapvető offline adattár használata esetén meg kell adnia adott táblákat és a mezőket az adatmodellben.</span><span class="sxs-lookup"><span data-stu-id="024ef-158">When you use the Core Data offline store, you must define particular tables and fields in your data model.</span></span> <span data-ttu-id="024ef-159">A mintaalkalmazás már tartalmazza a megfelelő formátumban az adatmodellt.</span><span class="sxs-lookup"><span data-stu-id="024ef-159">The sample app already includes a data model with the right format.</span></span> <span data-ttu-id="024ef-160">Ez a szakasz azt bízná ezek a táblázatok használatával megjelenítendő is.</span><span class="sxs-lookup"><span data-stu-id="024ef-160">In this section, we walk through these tables to show how they are used.</span></span>

<span data-ttu-id="024ef-161">Nyissa meg **QSDataModel.xcdatamodeld**.</span><span class="sxs-lookup"><span data-stu-id="024ef-161">Open **QSDataModel.xcdatamodeld**.</span></span> <span data-ttu-id="024ef-162">Négy táblázatokat--három, amelyeket az SDK-t használnak, a másik a tennivaló használt maguk elemei:</span><span class="sxs-lookup"><span data-stu-id="024ef-162">Four tables are defined--three that are used by the SDK and one that's used for the to-do items themselves:</span></span>
  * <span data-ttu-id="024ef-163">MS_TableOperations: Nyomon követi a kiszolgálóval való szinkronizálásra elemeket.</span><span class="sxs-lookup"><span data-stu-id="024ef-163">MS_TableOperations: Tracks the items that need to be synchronized with the server.</span></span>
  * <span data-ttu-id="024ef-164">MS_TableOperationErrors: Követi nyomon, hogy olyan hibákat, amelyek a kapcsolat nélküli szinkronizálás során kerül sor.</span><span class="sxs-lookup"><span data-stu-id="024ef-164">MS_TableOperationErrors: Tracks any errors that happen during offline synchronization.</span></span>
  * <span data-ttu-id="024ef-165">MS_TableConfig: Nyomon követi az utolsó az idő az utolsó szinkronizálás művelet összes lekéréses műveletek frissítése.</span><span class="sxs-lookup"><span data-stu-id="024ef-165">MS_TableConfig: Tracks the last updated time for the last sync operation for all pull operations.</span></span>
  * <span data-ttu-id="024ef-166">TodoItem: Tárolja a Tennivalólista elemein.</span><span class="sxs-lookup"><span data-stu-id="024ef-166">TodoItem: Stores the to-do items.</span></span> <span data-ttu-id="024ef-167">A rendszer oszlopok **createdAt**, **updatedAt**, és **verzió** választható rendszer tulajdonságai vannak.</span><span class="sxs-lookup"><span data-stu-id="024ef-167">The system columns **createdAt**, **updatedAt**, and **version** are optional system properties.</span></span>

> [!NOTE]
> <span data-ttu-id="024ef-168">A Mobile Apps SDK fenntartja az oszlop neve kezdődik "**``**".</span><span class="sxs-lookup"><span data-stu-id="024ef-168">The Mobile Apps SDK reserves column names that begin with "**``**".</span></span> <span data-ttu-id="024ef-169">A rendszer oszlopok csakis ne használja ezt az előtagot.</span><span class="sxs-lookup"><span data-stu-id="024ef-169">Do not use this prefix with anything other than system columns.</span></span> <span data-ttu-id="024ef-170">Ellenkező esetben a nevének módosítva lett a távoli háttér használatakor.</span><span class="sxs-lookup"><span data-stu-id="024ef-170">Otherwise, your column names are modified when you use the remote back end.</span></span>
>
>

<span data-ttu-id="024ef-171">A kapcsolat nélküli szinkronizálás szolgáltatás használatakor a három rendszertáblák és a tábla megadása.</span><span class="sxs-lookup"><span data-stu-id="024ef-171">When you use the offline sync feature, define the three system tables and the data table.</span></span>

### <a name="system-tables"></a><span data-ttu-id="024ef-172">Rendszertáblák</span><span class="sxs-lookup"><span data-stu-id="024ef-172">System tables</span></span>

<span data-ttu-id="024ef-173">**MS_TableOperations**</span><span class="sxs-lookup"><span data-stu-id="024ef-173">**MS_TableOperations**</span></span>  

![MS_TableOperations tábla attribútumai][defining-core-data-tableoperations-entity]

| <span data-ttu-id="024ef-175">Attribútum</span><span class="sxs-lookup"><span data-stu-id="024ef-175">Attribute</span></span> | <span data-ttu-id="024ef-176">Típus</span><span class="sxs-lookup"><span data-stu-id="024ef-176">Type</span></span> |
| --- | --- |
| <span data-ttu-id="024ef-177">id</span><span class="sxs-lookup"><span data-stu-id="024ef-177">id</span></span> | <span data-ttu-id="024ef-178">64 egész szám</span><span class="sxs-lookup"><span data-stu-id="024ef-178">Integer 64</span></span> |
| <span data-ttu-id="024ef-179">itemId</span><span class="sxs-lookup"><span data-stu-id="024ef-179">itemId</span></span> | <span data-ttu-id="024ef-180">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="024ef-180">String</span></span> |
| <span data-ttu-id="024ef-181">properties</span><span class="sxs-lookup"><span data-stu-id="024ef-181">properties</span></span> | <span data-ttu-id="024ef-182">Bináris adatok</span><span class="sxs-lookup"><span data-stu-id="024ef-182">Binary Data</span></span> |
| <span data-ttu-id="024ef-183">Tábla</span><span class="sxs-lookup"><span data-stu-id="024ef-183">table</span></span> | <span data-ttu-id="024ef-184">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="024ef-184">String</span></span> |
| <span data-ttu-id="024ef-185">tableKind</span><span class="sxs-lookup"><span data-stu-id="024ef-185">tableKind</span></span> | <span data-ttu-id="024ef-186">16 egész szám</span><span class="sxs-lookup"><span data-stu-id="024ef-186">Integer 16</span></span> |


<span data-ttu-id="024ef-187">**MS_TableOperationErrors**</span><span class="sxs-lookup"><span data-stu-id="024ef-187">**MS_TableOperationErrors**</span></span>

 ![MS_TableOperationErrors tábla attribútumai][defining-core-data-tableoperationerrors-entity]

| <span data-ttu-id="024ef-189">Attribútum</span><span class="sxs-lookup"><span data-stu-id="024ef-189">Attribute</span></span> | <span data-ttu-id="024ef-190">Típus</span><span class="sxs-lookup"><span data-stu-id="024ef-190">Type</span></span> |
| --- | --- |
| <span data-ttu-id="024ef-191">id</span><span class="sxs-lookup"><span data-stu-id="024ef-191">id</span></span> |<span data-ttu-id="024ef-192">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="024ef-192">String</span></span> |
| <span data-ttu-id="024ef-193">OperationID azonosítójú</span><span class="sxs-lookup"><span data-stu-id="024ef-193">operationId</span></span> |<span data-ttu-id="024ef-194">64 egész szám</span><span class="sxs-lookup"><span data-stu-id="024ef-194">Integer 64</span></span> |
| <span data-ttu-id="024ef-195">properties</span><span class="sxs-lookup"><span data-stu-id="024ef-195">properties</span></span> |<span data-ttu-id="024ef-196">Bináris adatok</span><span class="sxs-lookup"><span data-stu-id="024ef-196">Binary Data</span></span> |
| <span data-ttu-id="024ef-197">tableKind</span><span class="sxs-lookup"><span data-stu-id="024ef-197">tableKind</span></span> |<span data-ttu-id="024ef-198">16 egész szám</span><span class="sxs-lookup"><span data-stu-id="024ef-198">Integer 16</span></span> |

 <span data-ttu-id="024ef-199">**MS_TableConfig**</span><span class="sxs-lookup"><span data-stu-id="024ef-199">**MS_TableConfig**</span></span>

 ![][defining-core-data-tableconfig-entity]

| <span data-ttu-id="024ef-200">Attribútum</span><span class="sxs-lookup"><span data-stu-id="024ef-200">Attribute</span></span> | <span data-ttu-id="024ef-201">Típus</span><span class="sxs-lookup"><span data-stu-id="024ef-201">Type</span></span> |
| --- | --- |
| <span data-ttu-id="024ef-202">id</span><span class="sxs-lookup"><span data-stu-id="024ef-202">id</span></span> |<span data-ttu-id="024ef-203">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="024ef-203">String</span></span> |
| <span data-ttu-id="024ef-204">kulcs</span><span class="sxs-lookup"><span data-stu-id="024ef-204">key</span></span> |<span data-ttu-id="024ef-205">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="024ef-205">String</span></span> |
| <span data-ttu-id="024ef-206">keyType</span><span class="sxs-lookup"><span data-stu-id="024ef-206">keyType</span></span> |<span data-ttu-id="024ef-207">64 egész szám</span><span class="sxs-lookup"><span data-stu-id="024ef-207">Integer 64</span></span> |
| <span data-ttu-id="024ef-208">Tábla</span><span class="sxs-lookup"><span data-stu-id="024ef-208">table</span></span> |<span data-ttu-id="024ef-209">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="024ef-209">String</span></span> |
| <span data-ttu-id="024ef-210">érték</span><span class="sxs-lookup"><span data-stu-id="024ef-210">value</span></span> |<span data-ttu-id="024ef-211">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="024ef-211">String</span></span> |

### <a name="data-table"></a><span data-ttu-id="024ef-212">Adattábla</span><span class="sxs-lookup"><span data-stu-id="024ef-212">Data table</span></span>

<span data-ttu-id="024ef-213">**TodoItem**</span><span class="sxs-lookup"><span data-stu-id="024ef-213">**TodoItem**</span></span>

| <span data-ttu-id="024ef-214">Attribútum</span><span class="sxs-lookup"><span data-stu-id="024ef-214">Attribute</span></span> | <span data-ttu-id="024ef-215">Típus</span><span class="sxs-lookup"><span data-stu-id="024ef-215">Type</span></span> | <span data-ttu-id="024ef-216">Megjegyzés</span><span class="sxs-lookup"><span data-stu-id="024ef-216">Note</span></span> |
| --- | --- | --- |
| <span data-ttu-id="024ef-217">id</span><span class="sxs-lookup"><span data-stu-id="024ef-217">id</span></span> | <span data-ttu-id="024ef-218">Karakterlánc, kötelezőként megjelölt</span><span class="sxs-lookup"><span data-stu-id="024ef-218">String, marked required</span></span> |<span data-ttu-id="024ef-219">elsődleges távoli tárolóban levő kulccsal.</span><span class="sxs-lookup"><span data-stu-id="024ef-219">Primary key in remote store</span></span> |
| <span data-ttu-id="024ef-220">Végezze el</span><span class="sxs-lookup"><span data-stu-id="024ef-220">complete</span></span> | <span data-ttu-id="024ef-221">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="024ef-221">Boolean</span></span> | <span data-ttu-id="024ef-222">Tennivalók elem mező</span><span class="sxs-lookup"><span data-stu-id="024ef-222">To-do item field</span></span> |
| <span data-ttu-id="024ef-223">Szöveg</span><span class="sxs-lookup"><span data-stu-id="024ef-223">text</span></span> |<span data-ttu-id="024ef-224">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="024ef-224">String</span></span> |<span data-ttu-id="024ef-225">Tennivalók elem mező</span><span class="sxs-lookup"><span data-stu-id="024ef-225">To-do item field</span></span> |
| <span data-ttu-id="024ef-226">createdAt</span><span class="sxs-lookup"><span data-stu-id="024ef-226">createdAt</span></span> | <span data-ttu-id="024ef-227">Dátum</span><span class="sxs-lookup"><span data-stu-id="024ef-227">Date</span></span> | <span data-ttu-id="024ef-228">(választható) Leképezve **createdAt** rendszer tulajdonság</span><span class="sxs-lookup"><span data-stu-id="024ef-228">(optional) Maps to **createdAt** system property</span></span> |
| <span data-ttu-id="024ef-229">updatedAt</span><span class="sxs-lookup"><span data-stu-id="024ef-229">updatedAt</span></span> | <span data-ttu-id="024ef-230">Dátum</span><span class="sxs-lookup"><span data-stu-id="024ef-230">Date</span></span> | <span data-ttu-id="024ef-231">(választható) Leképezve **updatedAt** rendszer tulajdonság</span><span class="sxs-lookup"><span data-stu-id="024ef-231">(optional) Maps to **updatedAt** system property</span></span> |
| <span data-ttu-id="024ef-232">Verzió</span><span class="sxs-lookup"><span data-stu-id="024ef-232">version</span></span> | <span data-ttu-id="024ef-233">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="024ef-233">String</span></span> | <span data-ttu-id="024ef-234">(választható) Ütközések, verzióra maps érzékeli</span><span class="sxs-lookup"><span data-stu-id="024ef-234">(optional) Used to detect conflicts, maps to version</span></span> |

## <span data-ttu-id="024ef-235"><a name="setup-sync"></a>Az alkalmazás szinkronizálási viselkedésének módosítása</span><span class="sxs-lookup"><span data-stu-id="024ef-235"><a name="setup-sync"></a>Change the sync behavior of the app</span></span>
<span data-ttu-id="024ef-236">Ebben a szakaszban módosítani az alkalmazásnak, hogy nincs szinkronban az alkalmazás indítása vagy beszúrása, és ha a elemek frissítését.</span><span class="sxs-lookup"><span data-stu-id="024ef-236">In this section, you modify the app so that it does not sync on app start or when you insert and update items.</span></span> <span data-ttu-id="024ef-237">A szinkronizált csak akkor, ha a frissítés kézmozdulat gombra történik.</span><span class="sxs-lookup"><span data-stu-id="024ef-237">It syncs only when the refresh gesture button is performed.</span></span>

<span data-ttu-id="024ef-238">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="024ef-238">**Objective-C**:</span></span>

1. <span data-ttu-id="024ef-239">A **QSTodoListViewController.m**, módosítsa a **viewDidLoad** metódus hívása eltávolítása `[self refresh]` metódus végén.</span><span class="sxs-lookup"><span data-stu-id="024ef-239">In **QSTodoListViewController.m**, change the **viewDidLoad** method to remove the call to `[self refresh]` at the end of the method.</span></span> <span data-ttu-id="024ef-240">Az adatok a kiszolgálóval, az alkalmazás indítása most nincs szinkronizálva.</span><span class="sxs-lookup"><span data-stu-id="024ef-240">Now the data is not synced with the server on app start.</span></span> <span data-ttu-id="024ef-241">Ehelyett azt szinkronizálva van a helyi tárolójába tartalmát.</span><span class="sxs-lookup"><span data-stu-id="024ef-241">Instead, it's synced with the contents of the local store.</span></span>
2. <span data-ttu-id="024ef-242">A **QSTodoService.m**, módosítsa a meghatározása `addItem` , hogy azt nem szinkronizálása után a cikk csatlakoztatva van.</span><span class="sxs-lookup"><span data-stu-id="024ef-242">In **QSTodoService.m**, modify the definition of `addItem` so that it doesn't sync after the item is inserted.</span></span> <span data-ttu-id="024ef-243">Távolítsa el a `self syncData` letiltása, és cserélje le a következő:</span><span class="sxs-lookup"><span data-stu-id="024ef-243">Remove the `self syncData` block and replace it with the following:</span></span>

   ```objc
   if (completion != nil) {
       dispatch_async(dispatch_get_main_queue(), completion);
   }
   ```
3. <span data-ttu-id="024ef-244">Módosítsa a meghatározása `completeItem` amint azt korábban említettük.</span><span class="sxs-lookup"><span data-stu-id="024ef-244">Modify the definition of `completeItem` as mentioned previously.</span></span> <span data-ttu-id="024ef-245">A Tiltás feloldásához a `self syncData` , és cserélje le a következőre:</span><span class="sxs-lookup"><span data-stu-id="024ef-245">Remove the block for `self syncData` and replace it with the following:</span></span>
   ```objc
   if (completion != nil) {
       dispatch_async(dispatch_get_main_queue(), completion);
   }
   ```

<span data-ttu-id="024ef-246">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="024ef-246">**Swift**:</span></span>

<span data-ttu-id="024ef-247">A `viewDidLoad`, a **ToDoTableViewController.swift**, az alkalmazás indítása a szinkronizálás befejeződése látható itt, két sort megjegyzésbe.</span><span class="sxs-lookup"><span data-stu-id="024ef-247">In `viewDidLoad`, in **ToDoTableViewController.swift**, comment out the two lines shown here, to stop syncing on app start.</span></span> <span data-ttu-id="024ef-248">A cikk írásának időpontjában a Swift teendőlista alkalmazás nem frissíti a szolgáltatás valaki ad hozzá, vagy egy elem befejeződött.</span><span class="sxs-lookup"><span data-stu-id="024ef-248">At the time of this writing, the Swift Todo app does not update the service when someone adds or completes an item.</span></span> <span data-ttu-id="024ef-249">Frissíti a szolgáltatást csak az alkalmazás indítása.</span><span class="sxs-lookup"><span data-stu-id="024ef-249">It updates the service only on app start.</span></span>

   ```swift
  self.refreshControl?.beginRefreshing()
  self.onRefresh(self.refreshControl)
```

## <span data-ttu-id="024ef-250"><a name="test-app"></a>Az alkalmazás tesztelése</span><span class="sxs-lookup"><span data-stu-id="024ef-250"><a name="test-app"></a>Test the app</span></span>
<span data-ttu-id="024ef-251">Ebben a szakaszban csatlakozhat egy érvénytelen URL-CÍMÉT egy kapcsolat nélküli forgatókönyv szimulálásához.</span><span class="sxs-lookup"><span data-stu-id="024ef-251">In this section, you connect to an invalid URL to simulate an offline scenario.</span></span> <span data-ttu-id="024ef-252">Amikor adatelemek, azok van használatban a helyi alapvető adatok tárolására, de azok még nincs szinkronizálva a mobilalkalmazás háttérrendszer működésében.</span><span class="sxs-lookup"><span data-stu-id="024ef-252">When you add data items, they're held in the local Core Data store, but they're not synced with the mobile-app back end.</span></span>

1. <span data-ttu-id="024ef-253">Módosítsa a mobilalkalmazás URL-címet a **QSTodoService.m** egy érvénytelen URL-címet, és futtassa ismét az alkalmazást:</span><span class="sxs-lookup"><span data-stu-id="024ef-253">Change the mobile-app URL in **QSTodoService.m** to an invalid URL, and run the app again:</span></span>

   <span data-ttu-id="024ef-254">**Objective-C**.</span><span class="sxs-lookup"><span data-stu-id="024ef-254">**Objective-C**.</span></span> <span data-ttu-id="024ef-255">A QSTodoService.m:</span><span class="sxs-lookup"><span data-stu-id="024ef-255">In QSTodoService.m:</span></span>
   ```objc
   self.client = [MSClient clientWithApplicationURLString:@"https://sitename.azurewebsites.net.fail"];
   ```
   <span data-ttu-id="024ef-256">**SWIFT**.</span><span class="sxs-lookup"><span data-stu-id="024ef-256">**Swift**.</span></span> <span data-ttu-id="024ef-257">A ToDoTableViewController.swift:</span><span class="sxs-lookup"><span data-stu-id="024ef-257">In ToDoTableViewController.swift:</span></span>
   ```swift
   let client = MSClient(applicationURLString: "https://sitename.azurewebsites.net.fail")
   ```
2. <span data-ttu-id="024ef-258">Adja hozzá az egyes Tennivalólista elemein.</span><span class="sxs-lookup"><span data-stu-id="024ef-258">Add some to-do items.</span></span> <span data-ttu-id="024ef-259">Lépjen ki a szimulátor (vagy kényszerített zárja be az alkalmazást), majd indítsa újra.</span><span class="sxs-lookup"><span data-stu-id="024ef-259">Quit the simulator (or forcibly close the app), and then restart it.</span></span> <span data-ttu-id="024ef-260">Győződjön meg arról, hogy a változtatások megmaradnak.</span><span class="sxs-lookup"><span data-stu-id="024ef-260">Verify that your changes persist.</span></span>

3. <span data-ttu-id="024ef-261">A távoli tartalmának megtekintése **TodoItem** tábla:</span><span class="sxs-lookup"><span data-stu-id="024ef-261">View the contents of the remote **TodoItem** table:</span></span>
   * <span data-ttu-id="024ef-262">A Node.js háttérből, látogasson el a [Azure-portálon](https://portal.azure.com/) és kattintson a mobilalkalmazás háttér **könnyen táblák** > **TodoItem**.</span><span class="sxs-lookup"><span data-stu-id="024ef-262">For a Node.js back end, go to the [Azure portal](https://portal.azure.com/) and, in your mobile-app back end, click **Easy Tables** > **TodoItem**.</span></span>  
   * <span data-ttu-id="024ef-263">A .NET vissza az end, egy SQL-eszközt, például SQL Server Management Studio alkalmazást, vagy a többi ügyfél, például a Postman vagy a Fiddler használatával.</span><span class="sxs-lookup"><span data-stu-id="024ef-263">For a .NET back end, use either a SQL tool, such as SQL Server Management Studio, or a REST client, such as Fiddler or Postman.</span></span>  

4. <span data-ttu-id="024ef-264">Ellenőrizze, hogy rendelkezik-e az új elemek *nem* lett-e szinkronizálva a kiszolgálóval.</span><span class="sxs-lookup"><span data-stu-id="024ef-264">Verify that the new items have *not* been synced with the server.</span></span>

5. <span data-ttu-id="024ef-265">Az URL-CÍMÉT állítsa vissza a megfelelőre **QSTodoService.m**, és futtassa újra az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="024ef-265">Change the URL back to the correct one in **QSTodoService.m**, and rerun the app.</span></span>

6. <span data-ttu-id="024ef-266">Hajtsa végre a frissítési kézmozdulat modulba húzza le az elemeket.</span><span class="sxs-lookup"><span data-stu-id="024ef-266">Perform the refresh gesture by pulling down the list of items.</span></span>  
<span data-ttu-id="024ef-267">A folyamatban lévő léptető jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="024ef-267">A progress spinner is displayed.</span></span>

7. <span data-ttu-id="024ef-268">Nézet a **TodoItem** újra adatokat.</span><span class="sxs-lookup"><span data-stu-id="024ef-268">View the **TodoItem** data again.</span></span> <span data-ttu-id="024ef-269">Az új és módosított Tennivalólista elemein meg kell jelennie.</span><span class="sxs-lookup"><span data-stu-id="024ef-269">The new and changed to-do items should now be displayed.</span></span>

## <a name="summary"></a><span data-ttu-id="024ef-270">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="024ef-270">Summary</span></span>
<span data-ttu-id="024ef-271">A kapcsolat nélküli szinkronizálás szolgáltatást támogató használtuk az `MSSyncTable` felületet, és inicializálva `MSClient.syncContext` a helyi tárolójába.</span><span class="sxs-lookup"><span data-stu-id="024ef-271">To support the offline sync feature, we used the `MSSyncTable` interface and initialized `MSClient.syncContext` with a local store.</span></span> <span data-ttu-id="024ef-272">Ebben az esetben a helyi tárolójába lett egy alapvető adatok-alapú adatbázist.</span><span class="sxs-lookup"><span data-stu-id="024ef-272">In this case, the local store was a Core Data-based database.</span></span>

<span data-ttu-id="024ef-273">A Core helyi tárolóban használatakor meg kell adnia a több táblákban a [javítsa ki a rendszer tulajdonságai](#review-core-data).</span><span class="sxs-lookup"><span data-stu-id="024ef-273">When you use a Core Data local store, you must define several tables with the [correct system properties](#review-core-data).</span></span>

<span data-ttu-id="024ef-274">A normál létrehozása, olvasása, frissítése és törlése (CRUD) típusú műveletek mobilalkalmazások együttműködnek a szerint, ha az alkalmazás még a kapcsolat, de a műveletek fordulhat elő, a helyi tárolóban.</span><span class="sxs-lookup"><span data-stu-id="024ef-274">The normal create, read, update, and delete (CRUD) operations for mobile apps work as if the app is still connected, but all the operations occur against the local store.</span></span>

<span data-ttu-id="024ef-275">Ha a helyi tárolójába szinkronizálja azt a kiszolgálóval, használtuk az **MSSyncTable.pullWithQuery** metódust.</span><span class="sxs-lookup"><span data-stu-id="024ef-275">When we synchronized the local store with the server, we used the **MSSyncTable.pullWithQuery** method.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="024ef-276">További források</span><span class="sxs-lookup"><span data-stu-id="024ef-276">Additional resources</span></span>
* <span data-ttu-id="024ef-277">[Mobile Apps az Offline adatszinkronizálás]</span><span class="sxs-lookup"><span data-stu-id="024ef-277">[Offline Data Sync in Mobile Apps]</span></span>
* <span data-ttu-id="024ef-278">[Felhő tartalma: Kapcsolat nélküli szinkronizálás az Azure Mobile Services] \(a videó kapcsolatos Mobile Services, de a Mobile Apps kapcsolat nélküli szinkronizálás működésének hasonló módon.\)</span><span class="sxs-lookup"><span data-stu-id="024ef-278">[Cloud Cover: Offline Sync in Azure Mobile Services] \(The video is about Mobile Services, but Mobile Apps offline sync works in a similar way.\)</span></span>

<!-- URLs. -->


<span data-ttu-id="024ef-279">[iOS-alkalmazás létrehozása]: app-service-mobile-ios-get-started.md</span><span class="sxs-lookup"><span data-stu-id="024ef-279">[Create an iOS App]: app-service-mobile-ios-get-started.md</span></span>
<span data-ttu-id="024ef-280">[Mobile Apps az Offline adatszinkronizálás]: app-service-mobile-offline-data-sync.md</span><span class="sxs-lookup"><span data-stu-id="024ef-280">[Offline Data Sync in Mobile Apps]: app-service-mobile-offline-data-sync.md</span></span>

[defining-core-data-tableoperationerrors-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-tableoperationerrors-entity.png
[defining-core-data-tableoperations-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-tableoperations-entity.png
[defining-core-data-tableconfig-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-tableconfig-entity.png
[defining-core-data-todoitem-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-todoitem-entity.png

<span data-ttu-id="024ef-281">[Felhő tartalma: Kapcsolat nélküli szinkronizálás az Azure Mobile Services]: http://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri</span><span class="sxs-lookup"><span data-stu-id="024ef-281">[Cloud Cover: Offline Sync in Azure Mobile Services]: http://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri</span></span>
[Azure Friday: Offline-enabled apps in Azure Mobile Services]: http://azure.microsoft.com/en-us/documentation/videos/azure-mobile-services-offline-enabled-apps-with-donna-malayeri/
