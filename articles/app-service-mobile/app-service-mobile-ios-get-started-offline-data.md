---
title: "aaaEnable kapcsolat nélküli szinkronizálás a mobil iOS-alkalmazások |} Microsoft Docs"
description: "Megtudhatja, hogyan toouse Azure App Service mobile apps szolgáltatásban toocache és szinkronizálási offline adatokat iOS-alkalmazások."
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
ms.openlocfilehash: 570ea7cf6694ab7317c977331038929b64508ad3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="enable-offline-syncing-with-ios-mobile-apps"></a><span data-ttu-id="57ba2-103">Az iOS-mobilalkalmazások kapcsolat nélküli szinkronizálás engedélyezése</span><span class="sxs-lookup"><span data-stu-id="57ba2-103">Enable offline syncing with iOS mobile apps</span></span>
[!INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a><span data-ttu-id="57ba2-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="57ba2-104">Overview</span></span>
<span data-ttu-id="57ba2-105">Ez az oktatóanyag ismerteti, kapcsolat nélküli szinkronizálás a hello Mobile Apps szolgáltatás az Azure App Service iOS-hez.</span><span class="sxs-lookup"><span data-stu-id="57ba2-105">This tutorial covers offline syncing with hello Mobile Apps feature of Azure App Service for iOS.</span></span> <span data-ttu-id="57ba2-106">A kapcsolat nélküli szinkronizálja végfelhasználókkal interakciót folytatni a mobilalkalmazás tooview, hozzáadhat és módosíthatják az adatokat, akkor is, ha nincs hálózati kapcsolat van.</span><span class="sxs-lookup"><span data-stu-id="57ba2-106">With offline syncing end-users can interact with a mobile app tooview, add, or modify data, even when they have no network connection.</span></span> <span data-ttu-id="57ba2-107">Változások a helyi adatbázisban tárolódnak.</span><span class="sxs-lookup"><span data-stu-id="57ba2-107">Changes are stored in a local database.</span></span> <span data-ttu-id="57ba2-108">Miután hello eszköz újra online állapotba kerül, hello módosítások szinkronizálása megtörtént-e hello távoli háttérből.</span><span class="sxs-lookup"><span data-stu-id="57ba2-108">After hello device is back online, hello changes are synced with hello remote back end.</span></span>

<span data-ttu-id="57ba2-109">Ha ez a Mobile Apps első élményét, először ki hello oktatóanyag [iOS-alkalmazás létrehozása].</span><span class="sxs-lookup"><span data-stu-id="57ba2-109">If this is your first experience with Mobile Apps, you should first complete hello tutorial [Create an iOS App].</span></span> <span data-ttu-id="57ba2-110">Ha nem használja a letöltött hello gyors üzembe helyezési kiszolgálóprojektet, hozzá kell adnia a hello adatelérési kiegészítő csomagok tooyour projekt.</span><span class="sxs-lookup"><span data-stu-id="57ba2-110">If you do not use hello downloaded quick-start server project, you must add hello data-access extension packages tooyour project.</span></span> <span data-ttu-id="57ba2-111">Kiszolgáló bővítménycsomagok kapcsolatos további információkért lásd: [használható hello .NET háttérkiszolgáló SDK az Azure Mobile Apps a](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="57ba2-111">For more information about server extension packages, see [Work with hello .NET backend server SDK for Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).</span></span>

<span data-ttu-id="57ba2-112">toolearn hello kapcsolat nélküli szinkronizálás szolgáltatást, bővebben lásd: [Mobile Apps az Offline adatszinkronizálás].</span><span class="sxs-lookup"><span data-stu-id="57ba2-112">toolearn more about hello offline sync feature, see [Offline Data Sync in Mobile Apps].</span></span>

## <span data-ttu-id="57ba2-113"><a name="review-sync"></a>Tekintse át a kód a hello ügyfél szinkronizálása</span><span class="sxs-lookup"><span data-stu-id="57ba2-113"><a name="review-sync"></a>Review hello client sync code</span></span>
<span data-ttu-id="57ba2-114">a hello letöltött hello ügyfélprojekt [iOS-alkalmazás létrehozása] oktatóanyag már kódot tartalmaz, amely támogatja a helyi adatok Core-alapú adatbázis kapcsolat nélküli szinkronizálás.</span><span class="sxs-lookup"><span data-stu-id="57ba2-114">hello client project that you downloaded for hello [Create an iOS App] tutorial already contains code that supports offline synchronization using a local Core Data-based database.</span></span> <span data-ttu-id="57ba2-115">Ez a szakasz foglalja hello oktatóanyag kódot már tartalmát.</span><span class="sxs-lookup"><span data-stu-id="57ba2-115">This section summarizes what is already included in hello tutorial code.</span></span> <span data-ttu-id="57ba2-116">Hello szolgáltatás elméleti áttekintését lásd: [Mobile Apps az Offline adatszinkronizálás].</span><span class="sxs-lookup"><span data-stu-id="57ba2-116">For a conceptual overview of hello feature, see [Offline Data Sync in Mobile Apps].</span></span>

<span data-ttu-id="57ba2-117">A szolgáltatással hello offline Adatszinkronizálás a Mobile Apps, végfelhasználók kezelheti egy helyi adatbázist még nem érhető el hello hálózati esetén.</span><span class="sxs-lookup"><span data-stu-id="57ba2-117">Using hello offline data-sync feature of Mobile Apps, end-users can interact with a local database even when hello network is inaccessible.</span></span> <span data-ttu-id="57ba2-118">toouse ezeket a szolgáltatásokat az alkalmazás inicializálása hello szinkronizálási kontextusában `MSClient` , és a helyi tárolójába hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="57ba2-118">toouse these features in your app, you initialize hello sync context of `MSClient` and reference a local store.</span></span> <span data-ttu-id="57ba2-119">Ezt követően a tábla keresztül hello hivatkozik **MSSyncTable** felületet.</span><span class="sxs-lookup"><span data-stu-id="57ba2-119">Then you reference your table through hello **MSSyncTable** interface.</span></span>

<span data-ttu-id="57ba2-120">A **QSTodoService.m** (Objective-C) vagy **ToDoTableViewController.swift** (Swift), figyelje meg, hogy hello tag típusa hello **syncTable** van  **MSSyncTable**.</span><span class="sxs-lookup"><span data-stu-id="57ba2-120">In **QSTodoService.m** (Objective-C) or **ToDoTableViewController.swift** (Swift), notice that hello type of hello member **syncTable** is **MSSyncTable**.</span></span> <span data-ttu-id="57ba2-121">A szinkronizálási tábla felületet helyett használja a kapcsolat nélküli szinkronizálás **MSTable**.</span><span class="sxs-lookup"><span data-stu-id="57ba2-121">Offline sync uses this sync table interface instead of **MSTable**.</span></span> <span data-ttu-id="57ba2-122">A szinkronizálási tábla használatakor minden műveletet nyissa meg a helyi tárolójába toohello és csak hello távoli háttérrendszerének integrációját explicit leküldéses szinkronizálva, és műveletek lekéréses.</span><span class="sxs-lookup"><span data-stu-id="57ba2-122">When a sync table is used, all operations go toohello local store and are synchronized only with hello remote back end with explicit push and pull operations.</span></span>

 <span data-ttu-id="57ba2-123">egy hivatkozási tooa szinkronizálási tábla tooget hello használata **syncTableWithName** metódusa `MSClient`.</span><span class="sxs-lookup"><span data-stu-id="57ba2-123">tooget a reference tooa sync table, use hello **syncTableWithName** method on `MSClient`.</span></span> <span data-ttu-id="57ba2-124">tooremove kapcsolat nélküli szinkronizálás működését, használjon **tableWithName** helyette.</span><span class="sxs-lookup"><span data-stu-id="57ba2-124">tooremove offline sync functionality, use **tableWithName** instead.</span></span>

<span data-ttu-id="57ba2-125">Minden tábla műveletek elvégzése előtt hello helyi tároló inicializálni kell.</span><span class="sxs-lookup"><span data-stu-id="57ba2-125">Before any table operations can be performed, hello local store must be initialized.</span></span> <span data-ttu-id="57ba2-126">Íme hello megfelelő kódot:</span><span class="sxs-lookup"><span data-stu-id="57ba2-126">Here is hello relevant code:</span></span>

* <span data-ttu-id="57ba2-127">**Objective-C**.</span><span class="sxs-lookup"><span data-stu-id="57ba2-127">**Objective-C**.</span></span> <span data-ttu-id="57ba2-128">A hello **QSTodoService.init** módszert:</span><span class="sxs-lookup"><span data-stu-id="57ba2-128">In hello **QSTodoService.init** method:</span></span>

   ```objc
   MSCoreDataStore *store = [[MSCoreDataStore alloc] initWithManagedObjectContext:context];
   self.client.syncContext = [[MSSyncContext alloc] initWithDelegate:nil dataSource:store callback:nil];
   ```    
* <span data-ttu-id="57ba2-129">**SWIFT**.</span><span class="sxs-lookup"><span data-stu-id="57ba2-129">**Swift**.</span></span> <span data-ttu-id="57ba2-130">A hello **ToDoTableViewController.viewDidLoad** módszert:</span><span class="sxs-lookup"><span data-stu-id="57ba2-130">In hello **ToDoTableViewController.viewDidLoad** method:</span></span>

   ```swift
   let client = MSClient(applicationURLString: "http:// ...") // URI of hello Mobile App
   let managedObjectContext = (UIApplication.sharedApplication().delegate as! AppDelegate).managedObjectContext!
   self.store = MSCoreDataStore(managedObjectContext: managedObjectContext)
   client.syncContext = MSSyncContext(delegate: nil, dataSource: self.store, callback: nil)
   ```
   <span data-ttu-id="57ba2-131">Ez a módszer hello használatával hozza létre a helyi tárolójába `MSCoreDataStore` felület, amely a Mobile Apps SDK hello révén.</span><span class="sxs-lookup"><span data-stu-id="57ba2-131">This method creates a local store by using hello `MSCoreDataStore` interface, which hello Mobile Apps SDK provides.</span></span> <span data-ttu-id="57ba2-132">Azt is megteheti, megadhat egy másik helyi tároló hello implementálásával `MSSyncContextDataSource` protokoll.</span><span class="sxs-lookup"><span data-stu-id="57ba2-132">Alternatively, you can provide a different local store by implementing hello `MSSyncContextDataSource` protocol.</span></span> <span data-ttu-id="57ba2-133">Az első paraméter is, hello **MSSyncContext** használt toospecify ütközés kezelő van.</span><span class="sxs-lookup"><span data-stu-id="57ba2-133">Also, hello first parameter of **MSSyncContext** is used toospecify a conflict handler.</span></span> <span data-ttu-id="57ba2-134">Mivel jelenleg átadott `nil`, azt lekérése sikertelen ütközés hello alapértelmezett ütközés kezelő.</span><span class="sxs-lookup"><span data-stu-id="57ba2-134">Because we have passed `nil`, we get hello default conflict handler, which fails on any conflict.</span></span>

<span data-ttu-id="57ba2-135">Most tegyük hello tényleges szinkronizálási műveletet végrehajtani, és az adatok beolvasása hello távoli háttérből:</span><span class="sxs-lookup"><span data-stu-id="57ba2-135">Now, let's perform hello actual sync operation, and get data from hello remote back end:</span></span>

* <span data-ttu-id="57ba2-136">**Objective-C**.</span><span class="sxs-lookup"><span data-stu-id="57ba2-136">**Objective-C**.</span></span> <span data-ttu-id="57ba2-137">`syncData`először leküldéses értesítések a módosításokat, és ekkor meghívja a **pullData** tooget adatok hello távoli háttérből.</span><span class="sxs-lookup"><span data-stu-id="57ba2-137">`syncData` first pushes new changes and then calls **pullData** tooget data from hello remote back end.</span></span> <span data-ttu-id="57ba2-138">Viszont hello **pullData** metódus lekéri az új lekérdezés adatokat:</span><span class="sxs-lookup"><span data-stu-id="57ba2-138">In turn, hello **pullData** method gets new data that matches a query:</span></span>

   ```objc
   -(void)syncData:(QSCompletionBlock)completion
   {
       // Push all changes in hello sync context, and then pull new data.
       [self.client.syncContext pushWithCompletion:^(NSError *error) {
           [self logErrorIfNotNil:error];
           [self pullData:completion];
       }];
   }

   -(void)pullData:(QSCompletionBlock)completion
   {
       MSQuery *query = [self.syncTable query];

       // Pulls data from hello remote server into hello local table.
       // We're pulling all items and filtering in hello view.
       // Query ID is used for incremental sync.
       [self.syncTable pullWithQuery:query queryId:@"allTodoItems" completion:^(NSError *error) {
           [self logErrorIfNotNil:error];

           // Lets hello caller know that we have finished.
           if (completion != nil) {
               dispatch_async(dispatch_get_main_queue(), completion);
           }
       }];
   }
   ```
* <span data-ttu-id="57ba2-139">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="57ba2-139">**Swift**:</span></span>
   ```swift
   func onRefresh(sender: UIRefreshControl!) {
      UIApplication.sharedApplication().networkActivityIndicatorVisible = true

      self.table!.pullWithQuery(self.table?.query(), queryId: "AllRecords") {
          (error) -> Void in

          UIApplication.sharedApplication().networkActivityIndicatorVisible = false

          if error != nil {
              // A real application would handle various errors like network conditions,
              // server conflicts, etc via hello MSSyncContextDelegate
              print("Error: \(error!.description)")

              // We will discard our changes and keep hello server's copy for simplicity
              if let opErrors = error!.userInfo[MSErrorPushResultKey] as? Array<MSTableOperationError> {
                  for opError in opErrors {
                      print("Attempted operation tooitem \(opError.itemId)")
                      if (opError.operation == .Insert || opError.operation == .Delete) {
                          print("Insert/Delete, failed discarding changes")
                          opError.cancelOperationAndDiscardItemWithCompletion(nil)
                      } else {
                          print("Update failed, reverting tooserver's copy")
                          opError.cancelOperationAndUpdateItem(opError.serverItem!, completion: nil)
                      }
                  }
              }
          }
          self.refreshControl?.endRefreshing()
      }
   }
   ```

<span data-ttu-id="57ba2-140">Hello Objective-C verzióban a `syncData`, első hívás **pushWithCompletion** hello szinkronizálási környezetében.</span><span class="sxs-lookup"><span data-stu-id="57ba2-140">In hello Objective-C version, in `syncData`, we first call **pushWithCompletion** on hello sync context.</span></span> <span data-ttu-id="57ba2-141">Ez a módszer egy tagja `MSSyncContext` (és nem hello szinkronizálási maga táblázat), mert azt módosítások leküldéses értesítések összes táblák között.</span><span class="sxs-lookup"><span data-stu-id="57ba2-141">This method is a member of `MSSyncContext` (and not hello sync table itself) because it pushes changes across all tables.</span></span> <span data-ttu-id="57ba2-142">Csak azt jelzi, hogy helyi (CUD műveletek) révén valamilyen módon módosítva lett toohello server küldése.</span><span class="sxs-lookup"><span data-stu-id="57ba2-142">Only records that have been modified in some way locally (through CUD operations) are sent toohello server.</span></span> <span data-ttu-id="57ba2-143">Majd hello segítő **pullData** neve, mely hívások **MSSyncTable.pullWithQuery** tooretrieve távoli adatok és hello helyi adatbázisban tárolja.</span><span class="sxs-lookup"><span data-stu-id="57ba2-143">Then hello helper **pullData** is called, which calls **MSSyncTable.pullWithQuery** tooretrieve remote data and store it in hello local database.</span></span>

<span data-ttu-id="57ba2-144">Hello Swift verziójában, hello leküldéses művelet volt nem feltétlenül szükséges, mert nincs nincs hívás túl**pushWithCompletion**.</span><span class="sxs-lookup"><span data-stu-id="57ba2-144">In hello Swift version, because hello push operation was not strictly necessary, there is no call too**pushWithCompletion**.</span></span> <span data-ttu-id="57ba2-145">Ha nincsenek függőben lévő módosítások hello szinkronizálási környezetben, amely egy leküldéses művelet hello tábla, lekéréses mindig kibocsát egy leküldéses először.</span><span class="sxs-lookup"><span data-stu-id="57ba2-145">If there are any changes pending in hello sync context for hello table that is doing a push operation, pull always issues a push first.</span></span> <span data-ttu-id="57ba2-146">Ha több szinkronizálási tábla, célszerű legjobb tooexplicitly hívást leküldéses tooensure porton nem konzisztens kapcsolódó táblák között.</span><span class="sxs-lookup"><span data-stu-id="57ba2-146">However, if you have more than one sync table, it is best tooexplicitly call push tooensure that everything is consistent across related tables.</span></span>

<span data-ttu-id="57ba2-147">Hello Objective-C, mind a Swift verziók, használhatja a hello **pullWithQuery** metódus toospecify egy lekérdezés toofilter hello tooretrieve kívánt rekordokat.</span><span class="sxs-lookup"><span data-stu-id="57ba2-147">In both hello Objective-C and Swift versions, you can use hello **pullWithQuery** method toospecify a query toofilter hello records you want tooretrieve.</span></span> <span data-ttu-id="57ba2-148">Ebben a példában a hello lekérdezés lekéri a távoli hello minden rekordot `TodoItem` tábla.</span><span class="sxs-lookup"><span data-stu-id="57ba2-148">In this example, hello query retrieves all records in hello remote `TodoItem` table.</span></span>

<span data-ttu-id="57ba2-149">a második paraméter hello **pullWithQuery** használt lekérdezés azonosító *növekményes szinkronizálás*. A növekményes szinkronizálás csak azt jelzi, hogy hello utolsó szinkronizálás hello rekord óta módosultak lekéri `UpdatedAt` időbélyegzője (nevű `updatedAt` hello helyi tárban.) hello lekérdezés Azonosítóját, amely egyedi az egyes logikai lekérdezés leíró karakterláncnak kell lennie. az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="57ba2-149">hello second parameter of **pullWithQuery** is a query ID that is used for *incremental sync*. Incremental sync retrieves only records that were modified since hello last sync, using hello record's `UpdatedAt` time stamp (called `updatedAt` in hello local store.) hello query ID should be a descriptive string that is unique for each logical query in your app.</span></span> <span data-ttu-id="57ba2-150">tooopt kívül a növekményes szinkronizálás átadni `nil` , hello lekérdezés azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="57ba2-150">tooopt out of incremental sync, pass `nil` as hello query ID.</span></span> <span data-ttu-id="57ba2-151">Ezt a módszert potenciálisan nem hatékony, lehet, mert az összes rekord lévő összes lekéréses művelet lekérdezi.</span><span class="sxs-lookup"><span data-stu-id="57ba2-151">This approach can be potentially inefficient, because it retrieves all records on each pull operation.</span></span>

<span data-ttu-id="57ba2-152">hello Objective-C app szinkronizálásának módosítása vagy hozzáadása adatokat, amikor egy felhasználó hello frissítési kézmozdulat végez, és indítsa el a.</span><span class="sxs-lookup"><span data-stu-id="57ba2-152">hello Objective-C app syncs when you modify or add data, when a user performs hello refresh gesture, and on launch.</span></span>

<span data-ttu-id="57ba2-153">hello Swift app szinkronizálja, amikor hello felhasználó hello frissítési kézmozdulat hajt végre, és indítsa el a.</span><span class="sxs-lookup"><span data-stu-id="57ba2-153">hello Swift app syncs when hello user performs hello refresh gesture and on launch.</span></span>

<span data-ttu-id="57ba2-154">Mivel hello app szinkronizálások, amikor az adatok módosítva (Objective-C), vagy amikor hello alkalmazás indítása (Objective-C és Swift), hello app azt feltételezi, hogy ez hello a felhasználó online állapotban.</span><span class="sxs-lookup"><span data-stu-id="57ba2-154">Because hello app syncs whenever data is modified (Objective-C) or whenever hello app starts (Objective-C and Swift), hello app assumes that hello user is online.</span></span> <span data-ttu-id="57ba2-155">Egy későbbi szakasz ismerteti akkor frissíteni fogja hello alkalmazást úgy, hogy a felhasználók akkor is, ha a kapcsolat nélküli szerkesztheti.</span><span class="sxs-lookup"><span data-stu-id="57ba2-155">In a later section, you will update hello app so that users can edit even when they are offline.</span></span>

## <span data-ttu-id="57ba2-156"><a name="review-core-data"></a>Felülvizsgálati hello Core adatmodell</span><span class="sxs-lookup"><span data-stu-id="57ba2-156"><a name="review-core-data"></a>Review hello Core Data model</span></span>
<span data-ttu-id="57ba2-157">Hello Core offline adattár használata esetén meg kell adnia adott táblákat és a mezőket az adatmodellben.</span><span class="sxs-lookup"><span data-stu-id="57ba2-157">When you use hello Core Data offline store, you must define particular tables and fields in your data model.</span></span> <span data-ttu-id="57ba2-158">hello mintaalkalmazás már tartalmazza a megfelelő formátumban hello adatmodellt.</span><span class="sxs-lookup"><span data-stu-id="57ba2-158">hello sample app already includes a data model with hello right format.</span></span> <span data-ttu-id="57ba2-159">Ez a szakasz azt végezze el a táblák tooshow azok használata.</span><span class="sxs-lookup"><span data-stu-id="57ba2-159">In this section, we walk through these tables tooshow how they are used.</span></span>

<span data-ttu-id="57ba2-160">Nyissa meg **QSDataModel.xcdatamodeld**.</span><span class="sxs-lookup"><span data-stu-id="57ba2-160">Open **QSDataModel.xcdatamodeld**.</span></span> <span data-ttu-id="57ba2-161">Négy táblák definiált--három által használt hello SDK, és egy hello tennivaló használt elemet maguk:</span><span class="sxs-lookup"><span data-stu-id="57ba2-161">Four tables are defined--three that are used by hello SDK and one that's used for hello to-do items themselves:</span></span>
  * <span data-ttu-id="57ba2-162">MS_TableOperations: Számok hello hello server szinkronizálva toobe elemeket.</span><span class="sxs-lookup"><span data-stu-id="57ba2-162">MS_TableOperations: Tracks hello items that need toobe synchronized with hello server.</span></span>
  * <span data-ttu-id="57ba2-163">MS_TableOperationErrors: Követi nyomon, hogy olyan hibákat, amelyek a kapcsolat nélküli szinkronizálás során kerül sor.</span><span class="sxs-lookup"><span data-stu-id="57ba2-163">MS_TableOperationErrors: Tracks any errors that happen during offline synchronization.</span></span>
  * <span data-ttu-id="57ba2-164">MS_TableConfig: Nyomon követi az utolsó frissítés időpontja, utolsó szinkronizálási művelet hello összes lekéréses művelet hello.</span><span class="sxs-lookup"><span data-stu-id="57ba2-164">MS_TableConfig: Tracks hello last updated time for hello last sync operation for all pull operations.</span></span>
  * <span data-ttu-id="57ba2-165">TodoItem: Hello Tennivalólista elemein tárolja.</span><span class="sxs-lookup"><span data-stu-id="57ba2-165">TodoItem: Stores hello to-do items.</span></span> <span data-ttu-id="57ba2-166">rendszer oszlopok hello **createdAt**, **updatedAt**, és **verzió** választható rendszer tulajdonságai vannak.</span><span class="sxs-lookup"><span data-stu-id="57ba2-166">hello system columns **createdAt**, **updatedAt**, and **version** are optional system properties.</span></span>

> [!NOTE]
> <span data-ttu-id="57ba2-167">hello Mobile Apps SDK fenntartja az oszlop neve kezdődik "**``**".</span><span class="sxs-lookup"><span data-stu-id="57ba2-167">hello Mobile Apps SDK reserves column names that begin with "**``**".</span></span> <span data-ttu-id="57ba2-168">A rendszer oszlopok csakis ne használja ezt az előtagot.</span><span class="sxs-lookup"><span data-stu-id="57ba2-168">Do not use this prefix with anything other than system columns.</span></span> <span data-ttu-id="57ba2-169">Ellenkező esetben a nevének módosítva lett hello távoli háttér használatakor.</span><span class="sxs-lookup"><span data-stu-id="57ba2-169">Otherwise, your column names are modified when you use hello remote back end.</span></span>
>
>

<span data-ttu-id="57ba2-170">Hello kapcsolat nélküli szinkronizálás funkció használata esetén adja meg a hello három rendszertáblák és hello adattábla.</span><span class="sxs-lookup"><span data-stu-id="57ba2-170">When you use hello offline sync feature, define hello three system tables and hello data table.</span></span>

### <a name="system-tables"></a><span data-ttu-id="57ba2-171">Rendszertáblák</span><span class="sxs-lookup"><span data-stu-id="57ba2-171">System tables</span></span>

<span data-ttu-id="57ba2-172">**MS_TableOperations**</span><span class="sxs-lookup"><span data-stu-id="57ba2-172">**MS_TableOperations**</span></span>  

![MS_TableOperations tábla attribútumai][defining-core-data-tableoperations-entity]

| <span data-ttu-id="57ba2-174">Attribútum</span><span class="sxs-lookup"><span data-stu-id="57ba2-174">Attribute</span></span> | <span data-ttu-id="57ba2-175">Típus</span><span class="sxs-lookup"><span data-stu-id="57ba2-175">Type</span></span> |
| --- | --- |
| <span data-ttu-id="57ba2-176">id</span><span class="sxs-lookup"><span data-stu-id="57ba2-176">id</span></span> | <span data-ttu-id="57ba2-177">64 egész szám</span><span class="sxs-lookup"><span data-stu-id="57ba2-177">Integer 64</span></span> |
| <span data-ttu-id="57ba2-178">itemId</span><span class="sxs-lookup"><span data-stu-id="57ba2-178">itemId</span></span> | <span data-ttu-id="57ba2-179">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="57ba2-179">String</span></span> |
| <span data-ttu-id="57ba2-180">properties</span><span class="sxs-lookup"><span data-stu-id="57ba2-180">properties</span></span> | <span data-ttu-id="57ba2-181">Bináris adatok</span><span class="sxs-lookup"><span data-stu-id="57ba2-181">Binary Data</span></span> |
| <span data-ttu-id="57ba2-182">Tábla</span><span class="sxs-lookup"><span data-stu-id="57ba2-182">table</span></span> | <span data-ttu-id="57ba2-183">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="57ba2-183">String</span></span> |
| <span data-ttu-id="57ba2-184">tableKind</span><span class="sxs-lookup"><span data-stu-id="57ba2-184">tableKind</span></span> | <span data-ttu-id="57ba2-185">16 egész szám</span><span class="sxs-lookup"><span data-stu-id="57ba2-185">Integer 16</span></span> |


<span data-ttu-id="57ba2-186">**MS_TableOperationErrors**</span><span class="sxs-lookup"><span data-stu-id="57ba2-186">**MS_TableOperationErrors**</span></span>

 ![MS_TableOperationErrors tábla attribútumai][defining-core-data-tableoperationerrors-entity]

| <span data-ttu-id="57ba2-188">Attribútum</span><span class="sxs-lookup"><span data-stu-id="57ba2-188">Attribute</span></span> | <span data-ttu-id="57ba2-189">Típus</span><span class="sxs-lookup"><span data-stu-id="57ba2-189">Type</span></span> |
| --- | --- |
| <span data-ttu-id="57ba2-190">id</span><span class="sxs-lookup"><span data-stu-id="57ba2-190">id</span></span> |<span data-ttu-id="57ba2-191">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="57ba2-191">String</span></span> |
| <span data-ttu-id="57ba2-192">OperationID azonosítójú</span><span class="sxs-lookup"><span data-stu-id="57ba2-192">operationId</span></span> |<span data-ttu-id="57ba2-193">64 egész szám</span><span class="sxs-lookup"><span data-stu-id="57ba2-193">Integer 64</span></span> |
| <span data-ttu-id="57ba2-194">properties</span><span class="sxs-lookup"><span data-stu-id="57ba2-194">properties</span></span> |<span data-ttu-id="57ba2-195">Bináris adatok</span><span class="sxs-lookup"><span data-stu-id="57ba2-195">Binary Data</span></span> |
| <span data-ttu-id="57ba2-196">tableKind</span><span class="sxs-lookup"><span data-stu-id="57ba2-196">tableKind</span></span> |<span data-ttu-id="57ba2-197">16 egész szám</span><span class="sxs-lookup"><span data-stu-id="57ba2-197">Integer 16</span></span> |

 <span data-ttu-id="57ba2-198">**MS_TableConfig**</span><span class="sxs-lookup"><span data-stu-id="57ba2-198">**MS_TableConfig**</span></span>

 ![][defining-core-data-tableconfig-entity]

| <span data-ttu-id="57ba2-199">Attribútum</span><span class="sxs-lookup"><span data-stu-id="57ba2-199">Attribute</span></span> | <span data-ttu-id="57ba2-200">Típus</span><span class="sxs-lookup"><span data-stu-id="57ba2-200">Type</span></span> |
| --- | --- |
| <span data-ttu-id="57ba2-201">id</span><span class="sxs-lookup"><span data-stu-id="57ba2-201">id</span></span> |<span data-ttu-id="57ba2-202">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="57ba2-202">String</span></span> |
| <span data-ttu-id="57ba2-203">kulcs</span><span class="sxs-lookup"><span data-stu-id="57ba2-203">key</span></span> |<span data-ttu-id="57ba2-204">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="57ba2-204">String</span></span> |
| <span data-ttu-id="57ba2-205">keyType</span><span class="sxs-lookup"><span data-stu-id="57ba2-205">keyType</span></span> |<span data-ttu-id="57ba2-206">64 egész szám</span><span class="sxs-lookup"><span data-stu-id="57ba2-206">Integer 64</span></span> |
| <span data-ttu-id="57ba2-207">Tábla</span><span class="sxs-lookup"><span data-stu-id="57ba2-207">table</span></span> |<span data-ttu-id="57ba2-208">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="57ba2-208">String</span></span> |
| <span data-ttu-id="57ba2-209">érték</span><span class="sxs-lookup"><span data-stu-id="57ba2-209">value</span></span> |<span data-ttu-id="57ba2-210">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="57ba2-210">String</span></span> |

### <a name="data-table"></a><span data-ttu-id="57ba2-211">Adattábla</span><span class="sxs-lookup"><span data-stu-id="57ba2-211">Data table</span></span>

<span data-ttu-id="57ba2-212">**TodoItem**</span><span class="sxs-lookup"><span data-stu-id="57ba2-212">**TodoItem**</span></span>

| <span data-ttu-id="57ba2-213">Attribútum</span><span class="sxs-lookup"><span data-stu-id="57ba2-213">Attribute</span></span> | <span data-ttu-id="57ba2-214">Típus</span><span class="sxs-lookup"><span data-stu-id="57ba2-214">Type</span></span> | <span data-ttu-id="57ba2-215">Megjegyzés</span><span class="sxs-lookup"><span data-stu-id="57ba2-215">Note</span></span> |
| --- | --- | --- |
| <span data-ttu-id="57ba2-216">id</span><span class="sxs-lookup"><span data-stu-id="57ba2-216">id</span></span> | <span data-ttu-id="57ba2-217">Karakterlánc, kötelezőként megjelölt</span><span class="sxs-lookup"><span data-stu-id="57ba2-217">String, marked required</span></span> |<span data-ttu-id="57ba2-218">elsődleges távoli tárolóban levő kulccsal.</span><span class="sxs-lookup"><span data-stu-id="57ba2-218">Primary key in remote store</span></span> |
| <span data-ttu-id="57ba2-219">Végezze el</span><span class="sxs-lookup"><span data-stu-id="57ba2-219">complete</span></span> | <span data-ttu-id="57ba2-220">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="57ba2-220">Boolean</span></span> | <span data-ttu-id="57ba2-221">Tennivalók elem mező</span><span class="sxs-lookup"><span data-stu-id="57ba2-221">To-do item field</span></span> |
| <span data-ttu-id="57ba2-222">Szöveg</span><span class="sxs-lookup"><span data-stu-id="57ba2-222">text</span></span> |<span data-ttu-id="57ba2-223">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="57ba2-223">String</span></span> |<span data-ttu-id="57ba2-224">Tennivalók elem mező</span><span class="sxs-lookup"><span data-stu-id="57ba2-224">To-do item field</span></span> |
| <span data-ttu-id="57ba2-225">createdAt</span><span class="sxs-lookup"><span data-stu-id="57ba2-225">createdAt</span></span> | <span data-ttu-id="57ba2-226">Dátum</span><span class="sxs-lookup"><span data-stu-id="57ba2-226">Date</span></span> | <span data-ttu-id="57ba2-227">(választható) Túl leképezhető**createdAt** rendszer tulajdonság</span><span class="sxs-lookup"><span data-stu-id="57ba2-227">(optional) Maps too**createdAt** system property</span></span> |
| <span data-ttu-id="57ba2-228">updatedAt</span><span class="sxs-lookup"><span data-stu-id="57ba2-228">updatedAt</span></span> | <span data-ttu-id="57ba2-229">Dátum</span><span class="sxs-lookup"><span data-stu-id="57ba2-229">Date</span></span> | <span data-ttu-id="57ba2-230">(választható) Túl leképezhető**updatedAt** rendszer tulajdonság</span><span class="sxs-lookup"><span data-stu-id="57ba2-230">(optional) Maps too**updatedAt** system property</span></span> |
| <span data-ttu-id="57ba2-231">Verzió</span><span class="sxs-lookup"><span data-stu-id="57ba2-231">version</span></span> | <span data-ttu-id="57ba2-232">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="57ba2-232">String</span></span> | <span data-ttu-id="57ba2-233">(választható) Használt toodetect ütközések, maps tooversion</span><span class="sxs-lookup"><span data-stu-id="57ba2-233">(optional) Used toodetect conflicts, maps tooversion</span></span> |

## <span data-ttu-id="57ba2-234"><a name="setup-sync"></a>Hello szinkronizálási megváltozzon hello alkalmazás</span><span class="sxs-lookup"><span data-stu-id="57ba2-234"><a name="setup-sync"></a>Change hello sync behavior of hello app</span></span>
<span data-ttu-id="57ba2-235">Ebben a szakaszban módosítani hello alkalmazást, hogy nincs szinkronban az alkalmazás indítása vagy beszúrása, és ha a elemek frissítését.</span><span class="sxs-lookup"><span data-stu-id="57ba2-235">In this section, you modify hello app so that it does not sync on app start or when you insert and update items.</span></span> <span data-ttu-id="57ba2-236">A szinkronizált csak hello frissítés kézmozdulat gomb végrehajtásakor.</span><span class="sxs-lookup"><span data-stu-id="57ba2-236">It syncs only when hello refresh gesture button is performed.</span></span>

<span data-ttu-id="57ba2-237">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="57ba2-237">**Objective-C**:</span></span>

1. <span data-ttu-id="57ba2-238">A **QSTodoListViewController.m**, módosítsa a hello **viewDidLoad** metódus tooremove hello hívás túl`[self refresh]` hello metódus hello végén.</span><span class="sxs-lookup"><span data-stu-id="57ba2-238">In **QSTodoListViewController.m**, change hello **viewDidLoad** method tooremove hello call too`[self refresh]` at hello end of hello method.</span></span> <span data-ttu-id="57ba2-239">Hello adatok hello Server alkalmazás indítása most nincs szinkronizálva.</span><span class="sxs-lookup"><span data-stu-id="57ba2-239">Now hello data is not synced with hello server on app start.</span></span> <span data-ttu-id="57ba2-240">Ehelyett azt szinkronizálva van hello helyi tároló hello tartalmát.</span><span class="sxs-lookup"><span data-stu-id="57ba2-240">Instead, it's synced with hello contents of hello local store.</span></span>
2. <span data-ttu-id="57ba2-241">A **QSTodoService.m**, hello definíciójának módosítása `addItem` , hogy azt nem szinkronizálása után hello sort kell beszúrni.</span><span class="sxs-lookup"><span data-stu-id="57ba2-241">In **QSTodoService.m**, modify hello definition of `addItem` so that it doesn't sync after hello item is inserted.</span></span> <span data-ttu-id="57ba2-242">Távolítsa el a hello `self syncData` letiltása, és cserélje le a következő hello:</span><span class="sxs-lookup"><span data-stu-id="57ba2-242">Remove hello `self syncData` block and replace it with hello following:</span></span>

   ```objc
   if (completion != nil) {
       dispatch_async(dispatch_get_main_queue(), completion);
   }
   ```
3. <span data-ttu-id="57ba2-243">Hello definíciójának módosítása `completeItem` amint azt korábban említettük.</span><span class="sxs-lookup"><span data-stu-id="57ba2-243">Modify hello definition of `completeItem` as mentioned previously.</span></span> <span data-ttu-id="57ba2-244">Távolítsa el a hello blokkja `self syncData` és cserélje le a következő hello:</span><span class="sxs-lookup"><span data-stu-id="57ba2-244">Remove hello block for `self syncData` and replace it with hello following:</span></span>
   ```objc
   if (completion != nil) {
       dispatch_async(dispatch_get_main_queue(), completion);
   }
   ```

<span data-ttu-id="57ba2-245">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="57ba2-245">**Swift**:</span></span>

<span data-ttu-id="57ba2-246">A `viewDidLoad`, a **ToDoTableViewController.swift**, hello két sort látható itt, az alkalmazás indítása szinkronizálása toostop megjegyzésbe.</span><span class="sxs-lookup"><span data-stu-id="57ba2-246">In `viewDidLoad`, in **ToDoTableViewController.swift**, comment out hello two lines shown here, toostop syncing on app start.</span></span> <span data-ttu-id="57ba2-247">Írásának hello időpontban hello Swift teendőlista alkalmazás nem frissíthető hello szolgáltatást Ha valaki ad hozzá, vagy egy elem befejeződött.</span><span class="sxs-lookup"><span data-stu-id="57ba2-247">At hello time of this writing, hello Swift Todo app does not update hello service when someone adds or completes an item.</span></span> <span data-ttu-id="57ba2-248">Hello szolgáltatást csak az alkalmazás indítása frissíti.</span><span class="sxs-lookup"><span data-stu-id="57ba2-248">It updates hello service only on app start.</span></span>

   ```swift
  self.refreshControl?.beginRefreshing()
  self.onRefresh(self.refreshControl)
```

## <span data-ttu-id="57ba2-249"><a name="test-app"></a>Teszt hello alkalmazás</span><span class="sxs-lookup"><span data-stu-id="57ba2-249"><a name="test-app"></a>Test hello app</span></span>
<span data-ttu-id="57ba2-250">Ebben a szakaszban tooan érvénytelen URL-cím toosimulate egy kapcsolat nélküli keresztül csatlakozni.</span><span class="sxs-lookup"><span data-stu-id="57ba2-250">In this section, you connect tooan invalid URL toosimulate an offline scenario.</span></span> <span data-ttu-id="57ba2-251">Adatelemek hozzáadásakor azok által birtokolt hello a helyi alapvető adatok tárolására, de azok még nem szinkronizálta hello mobilalkalmazás háttérből.</span><span class="sxs-lookup"><span data-stu-id="57ba2-251">When you add data items, they're held in hello local Core Data store, but they're not synced with hello mobile-app back end.</span></span>

1. <span data-ttu-id="57ba2-252">Módosítsa a hello mobilalkalmazás URL-címet **QSTodoService.m** tooan URL-cím érvénytelen, és újra futtatási hello alkalmazást:</span><span class="sxs-lookup"><span data-stu-id="57ba2-252">Change hello mobile-app URL in **QSTodoService.m** tooan invalid URL, and run hello app again:</span></span>

   <span data-ttu-id="57ba2-253">**Objective-C**.</span><span class="sxs-lookup"><span data-stu-id="57ba2-253">**Objective-C**.</span></span> <span data-ttu-id="57ba2-254">A QSTodoService.m:</span><span class="sxs-lookup"><span data-stu-id="57ba2-254">In QSTodoService.m:</span></span>
   ```objc
   self.client = [MSClient clientWithApplicationURLString:@"https://sitename.azurewebsites.net.fail"];
   ```
   <span data-ttu-id="57ba2-255">**SWIFT**.</span><span class="sxs-lookup"><span data-stu-id="57ba2-255">**Swift**.</span></span> <span data-ttu-id="57ba2-256">A ToDoTableViewController.swift:</span><span class="sxs-lookup"><span data-stu-id="57ba2-256">In ToDoTableViewController.swift:</span></span>
   ```swift
   let client = MSClient(applicationURLString: "https://sitename.azurewebsites.net.fail")
   ```
2. <span data-ttu-id="57ba2-257">Adja hozzá az egyes Tennivalólista elemein.</span><span class="sxs-lookup"><span data-stu-id="57ba2-257">Add some to-do items.</span></span> <span data-ttu-id="57ba2-258">Lépjen ki a hello szimulátor (vagy a kényszerített bezárása hello alkalmazást), és indítsa újra.</span><span class="sxs-lookup"><span data-stu-id="57ba2-258">Quit hello simulator (or forcibly close hello app), and then restart it.</span></span> <span data-ttu-id="57ba2-259">Győződjön meg arról, hogy a változtatások megmaradnak.</span><span class="sxs-lookup"><span data-stu-id="57ba2-259">Verify that your changes persist.</span></span>

3. <span data-ttu-id="57ba2-260">Távoli hello hello tartalmának megtekintése **TodoItem** tábla:</span><span class="sxs-lookup"><span data-stu-id="57ba2-260">View hello contents of hello remote **TodoItem** table:</span></span>
   * <span data-ttu-id="57ba2-261">A Node.js háttérből, nyissa meg a toohello [Azure-portálon](https://portal.azure.com/) és kattintson a mobilalkalmazás háttér **könnyen táblák** > **TodoItem**.</span><span class="sxs-lookup"><span data-stu-id="57ba2-261">For a Node.js back end, go toohello [Azure portal](https://portal.azure.com/) and, in your mobile-app back end, click **Easy Tables** > **TodoItem**.</span></span>  
   * <span data-ttu-id="57ba2-262">A .NET vissza az end, egy SQL-eszközt, például SQL Server Management Studio alkalmazást, vagy a többi ügyfél, például a Postman vagy a Fiddler használatával.</span><span class="sxs-lookup"><span data-stu-id="57ba2-262">For a .NET back end, use either a SQL tool, such as SQL Server Management Studio, or a REST client, such as Fiddler or Postman.</span></span>  

4. <span data-ttu-id="57ba2-263">Ellenőrizze, hogy rendelkezik-e új elemek hello *nem* megtörtént szinkronizálta hello kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="57ba2-263">Verify that hello new items have *not* been synced with hello server.</span></span>

5. <span data-ttu-id="57ba2-264">Változás hello URL-cím hátsó toohello javítsa ki, egy-egy **QSTodoService.m**, és futtassa újra hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="57ba2-264">Change hello URL back toohello correct one in **QSTodoService.m**, and rerun hello app.</span></span>

6. <span data-ttu-id="57ba2-265">Hajtsa végre a hello frissítési kézmozdulat modulba húzza lefelé hello elemek listáját.</span><span class="sxs-lookup"><span data-stu-id="57ba2-265">Perform hello refresh gesture by pulling down hello list of items.</span></span>  
<span data-ttu-id="57ba2-266">A folyamatban lévő léptető jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="57ba2-266">A progress spinner is displayed.</span></span>

7. <span data-ttu-id="57ba2-267">Nézet hello **TodoItem** újra adatokat.</span><span class="sxs-lookup"><span data-stu-id="57ba2-267">View hello **TodoItem** data again.</span></span> <span data-ttu-id="57ba2-268">hello új és módosított Tennivalólista elemein meg kell jelennie.</span><span class="sxs-lookup"><span data-stu-id="57ba2-268">hello new and changed to-do items should now be displayed.</span></span>

## <a name="summary"></a><span data-ttu-id="57ba2-269">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="57ba2-269">Summary</span></span>
<span data-ttu-id="57ba2-270">hello használtuk toosupport hello kapcsolat nélküli szinkronizálás szolgáltatás `MSSyncTable` felületet, és inicializálva `MSClient.syncContext` a helyi tárolójába.</span><span class="sxs-lookup"><span data-stu-id="57ba2-270">toosupport hello offline sync feature, we used hello `MSSyncTable` interface and initialized `MSClient.syncContext` with a local store.</span></span> <span data-ttu-id="57ba2-271">Ebben az esetben hello helyi tároló lett egy alapvető adatok-alapú adatbázist.</span><span class="sxs-lookup"><span data-stu-id="57ba2-271">In this case, hello local store was a Core Data-based database.</span></span>

<span data-ttu-id="57ba2-272">A Core helyi tárolóban használatakor meg kell adnia a hello több táblákban [javítsa ki a rendszer tulajdonságai](#review-core-data).</span><span class="sxs-lookup"><span data-stu-id="57ba2-272">When you use a Core Data local store, you must define several tables with hello [correct system properties](#review-core-data).</span></span>

<span data-ttu-id="57ba2-273">hello normál létrehozása, olvasása, frissítése és törölhetők, (CRUD) típusú műveletek mobilalkalmazások együttműködnek a hello alkalmazás még a kapcsolat, de minden hello műveletet hello helyi tárolóban történik.</span><span class="sxs-lookup"><span data-stu-id="57ba2-273">hello normal create, read, update, and delete (CRUD) operations for mobile apps work as if hello app is still connected, but all hello operations occur against hello local store.</span></span>

<span data-ttu-id="57ba2-274">Ha azt a szinkronizált hello helyi tároló hello kiszolgálóval, használtuk hello **MSSyncTable.pullWithQuery** metódust.</span><span class="sxs-lookup"><span data-stu-id="57ba2-274">When we synchronized hello local store with hello server, we used hello **MSSyncTable.pullWithQuery** method.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="57ba2-275">További források</span><span class="sxs-lookup"><span data-stu-id="57ba2-275">Additional resources</span></span>
* <span data-ttu-id="57ba2-276">[Mobile Apps az Offline adatszinkronizálás]</span><span class="sxs-lookup"><span data-stu-id="57ba2-276">[Offline Data Sync in Mobile Apps]</span></span>
* <span data-ttu-id="57ba2-277">[Felhő tartalma: Kapcsolat nélküli szinkronizálás az Azure Mobile Services] \(videó hello kapcsolatos Mobile Services, de a Mobile Apps kapcsolat nélküli szinkronizálás működésének hasonló módon.\)</span><span class="sxs-lookup"><span data-stu-id="57ba2-277">[Cloud Cover: Offline Sync in Azure Mobile Services] \(hello video is about Mobile Services, but Mobile Apps offline sync works in a similar way.\)</span></span>

<!-- URLs. -->


[iOS-alkalmazás létrehozása]: app-service-mobile-ios-get-started.md
[Mobile Apps az Offline adatszinkronizálás]: app-service-mobile-offline-data-sync.md

[defining-core-data-tableoperationerrors-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-tableoperationerrors-entity.png
[defining-core-data-tableoperations-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-tableoperations-entity.png
[defining-core-data-tableconfig-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-tableconfig-entity.png
[defining-core-data-todoitem-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-todoitem-entity.png

[Felhő tartalma: Kapcsolat nélküli szinkronizálás az Azure Mobile Services]: http://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri
[Azure Friday: Offline-enabled apps in Azure Mobile Services]: http://azure.microsoft.com/en-us/documentation/videos/azure-mobile-services-offline-enabled-apps-with-donna-malayeri/
