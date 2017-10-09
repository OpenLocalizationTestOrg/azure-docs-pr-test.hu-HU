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
# <a name="enable-offline-syncing-with-ios-mobile-apps"></a>Az iOS-mobilalkalmazások kapcsolat nélküli szinkronizálás engedélyezése
[!INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Áttekintés
Ez az oktatóanyag ismerteti, kapcsolat nélküli szinkronizálás a hello Mobile Apps szolgáltatás az Azure App Service iOS-hez. A kapcsolat nélküli szinkronizálja végfelhasználókkal interakciót folytatni a mobilalkalmazás tooview, hozzáadhat és módosíthatják az adatokat, akkor is, ha nincs hálózati kapcsolat van. Változások a helyi adatbázisban tárolódnak. Miután hello eszköz újra online állapotba kerül, hello módosítások szinkronizálása megtörtént-e hello távoli háttérből.

Ha ez a Mobile Apps első élményét, először ki hello oktatóanyag [iOS-alkalmazás létrehozása]. Ha nem használja a letöltött hello gyors üzembe helyezési kiszolgálóprojektet, hozzá kell adnia a hello adatelérési kiegészítő csomagok tooyour projekt. Kiszolgáló bővítménycsomagok kapcsolatos további információkért lásd: [használható hello .NET háttérkiszolgáló SDK az Azure Mobile Apps a](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

toolearn hello kapcsolat nélküli szinkronizálás szolgáltatást, bővebben lásd: [Mobile Apps az Offline adatszinkronizálás].

## <a name="review-sync"></a>Tekintse át a kód a hello ügyfél szinkronizálása
a hello letöltött hello ügyfélprojekt [iOS-alkalmazás létrehozása] oktatóanyag már kódot tartalmaz, amely támogatja a helyi adatok Core-alapú adatbázis kapcsolat nélküli szinkronizálás. Ez a szakasz foglalja hello oktatóanyag kódot már tartalmát. Hello szolgáltatás elméleti áttekintését lásd: [Mobile Apps az Offline adatszinkronizálás].

A szolgáltatással hello offline Adatszinkronizálás a Mobile Apps, végfelhasználók kezelheti egy helyi adatbázist még nem érhető el hello hálózati esetén. toouse ezeket a szolgáltatásokat az alkalmazás inicializálása hello szinkronizálási kontextusában `MSClient` , és a helyi tárolójába hivatkozik. Ezt követően a tábla keresztül hello hivatkozik **MSSyncTable** felületet.

A **QSTodoService.m** (Objective-C) vagy **ToDoTableViewController.swift** (Swift), figyelje meg, hogy hello tag típusa hello **syncTable** van  **MSSyncTable**. A szinkronizálási tábla felületet helyett használja a kapcsolat nélküli szinkronizálás **MSTable**. A szinkronizálási tábla használatakor minden műveletet nyissa meg a helyi tárolójába toohello és csak hello távoli háttérrendszerének integrációját explicit leküldéses szinkronizálva, és műveletek lekéréses.

 egy hivatkozási tooa szinkronizálási tábla tooget hello használata **syncTableWithName** metódusa `MSClient`. tooremove kapcsolat nélküli szinkronizálás működését, használjon **tableWithName** helyette.

Minden tábla műveletek elvégzése előtt hello helyi tároló inicializálni kell. Íme hello megfelelő kódot:

* **Objective-C**. A hello **QSTodoService.init** módszert:

   ```objc
   MSCoreDataStore *store = [[MSCoreDataStore alloc] initWithManagedObjectContext:context];
   self.client.syncContext = [[MSSyncContext alloc] initWithDelegate:nil dataSource:store callback:nil];
   ```    
* **SWIFT**. A hello **ToDoTableViewController.viewDidLoad** módszert:

   ```swift
   let client = MSClient(applicationURLString: "http:// ...") // URI of hello Mobile App
   let managedObjectContext = (UIApplication.sharedApplication().delegate as! AppDelegate).managedObjectContext!
   self.store = MSCoreDataStore(managedObjectContext: managedObjectContext)
   client.syncContext = MSSyncContext(delegate: nil, dataSource: self.store, callback: nil)
   ```
   Ez a módszer hello használatával hozza létre a helyi tárolójába `MSCoreDataStore` felület, amely a Mobile Apps SDK hello révén. Azt is megteheti, megadhat egy másik helyi tároló hello implementálásával `MSSyncContextDataSource` protokoll. Az első paraméter is, hello **MSSyncContext** használt toospecify ütközés kezelő van. Mivel jelenleg átadott `nil`, azt lekérése sikertelen ütközés hello alapértelmezett ütközés kezelő.

Most tegyük hello tényleges szinkronizálási műveletet végrehajtani, és az adatok beolvasása hello távoli háttérből:

* **Objective-C**. `syncData`először leküldéses értesítések a módosításokat, és ekkor meghívja a **pullData** tooget adatok hello távoli háttérből. Viszont hello **pullData** metódus lekéri az új lekérdezés adatokat:

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
* **SWIFT**:
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

Hello Objective-C verzióban a `syncData`, első hívás **pushWithCompletion** hello szinkronizálási környezetében. Ez a módszer egy tagja `MSSyncContext` (és nem hello szinkronizálási maga táblázat), mert azt módosítások leküldéses értesítések összes táblák között. Csak azt jelzi, hogy helyi (CUD műveletek) révén valamilyen módon módosítva lett toohello server küldése. Majd hello segítő **pullData** neve, mely hívások **MSSyncTable.pullWithQuery** tooretrieve távoli adatok és hello helyi adatbázisban tárolja.

Hello Swift verziójában, hello leküldéses művelet volt nem feltétlenül szükséges, mert nincs nincs hívás túl**pushWithCompletion**. Ha nincsenek függőben lévő módosítások hello szinkronizálási környezetben, amely egy leküldéses művelet hello tábla, lekéréses mindig kibocsát egy leküldéses először. Ha több szinkronizálási tábla, célszerű legjobb tooexplicitly hívást leküldéses tooensure porton nem konzisztens kapcsolódó táblák között.

Hello Objective-C, mind a Swift verziók, használhatja a hello **pullWithQuery** metódus toospecify egy lekérdezés toofilter hello tooretrieve kívánt rekordokat. Ebben a példában a hello lekérdezés lekéri a távoli hello minden rekordot `TodoItem` tábla.

a második paraméter hello **pullWithQuery** használt lekérdezés azonosító *növekményes szinkronizálás*. A növekményes szinkronizálás csak azt jelzi, hogy hello utolsó szinkronizálás hello rekord óta módosultak lekéri `UpdatedAt` időbélyegzője (nevű `updatedAt` hello helyi tárban.) hello lekérdezés Azonosítóját, amely egyedi az egyes logikai lekérdezés leíró karakterláncnak kell lennie. az alkalmazás. tooopt kívül a növekményes szinkronizálás átadni `nil` , hello lekérdezés azonosítóját. Ezt a módszert potenciálisan nem hatékony, lehet, mert az összes rekord lévő összes lekéréses művelet lekérdezi.

hello Objective-C app szinkronizálásának módosítása vagy hozzáadása adatokat, amikor egy felhasználó hello frissítési kézmozdulat végez, és indítsa el a.

hello Swift app szinkronizálja, amikor hello felhasználó hello frissítési kézmozdulat hajt végre, és indítsa el a.

Mivel hello app szinkronizálások, amikor az adatok módosítva (Objective-C), vagy amikor hello alkalmazás indítása (Objective-C és Swift), hello app azt feltételezi, hogy ez hello a felhasználó online állapotban. Egy későbbi szakasz ismerteti akkor frissíteni fogja hello alkalmazást úgy, hogy a felhasználók akkor is, ha a kapcsolat nélküli szerkesztheti.

## <a name="review-core-data"></a>Felülvizsgálati hello Core adatmodell
Hello Core offline adattár használata esetén meg kell adnia adott táblákat és a mezőket az adatmodellben. hello mintaalkalmazás már tartalmazza a megfelelő formátumban hello adatmodellt. Ez a szakasz azt végezze el a táblák tooshow azok használata.

Nyissa meg **QSDataModel.xcdatamodeld**. Négy táblák definiált--három által használt hello SDK, és egy hello tennivaló használt elemet maguk:
  * MS_TableOperations: Számok hello hello server szinkronizálva toobe elemeket.
  * MS_TableOperationErrors: Követi nyomon, hogy olyan hibákat, amelyek a kapcsolat nélküli szinkronizálás során kerül sor.
  * MS_TableConfig: Nyomon követi az utolsó frissítés időpontja, utolsó szinkronizálási művelet hello összes lekéréses művelet hello.
  * TodoItem: Hello Tennivalólista elemein tárolja. rendszer oszlopok hello **createdAt**, **updatedAt**, és **verzió** választható rendszer tulajdonságai vannak.

> [!NOTE]
> hello Mobile Apps SDK fenntartja az oszlop neve kezdődik "**``**". A rendszer oszlopok csakis ne használja ezt az előtagot. Ellenkező esetben a nevének módosítva lett hello távoli háttér használatakor.
>
>

Hello kapcsolat nélküli szinkronizálás funkció használata esetén adja meg a hello három rendszertáblák és hello adattábla.

### <a name="system-tables"></a>Rendszertáblák

**MS_TableOperations**  

![MS_TableOperations tábla attribútumai][defining-core-data-tableoperations-entity]

| Attribútum | Típus |
| --- | --- |
| id | 64 egész szám |
| itemId | Karakterlánc |
| properties | Bináris adatok |
| Tábla | Karakterlánc |
| tableKind | 16 egész szám |


**MS_TableOperationErrors**

 ![MS_TableOperationErrors tábla attribútumai][defining-core-data-tableoperationerrors-entity]

| Attribútum | Típus |
| --- | --- |
| id |Karakterlánc |
| OperationID azonosítójú |64 egész szám |
| properties |Bináris adatok |
| tableKind |16 egész szám |

 **MS_TableConfig**

 ![][defining-core-data-tableconfig-entity]

| Attribútum | Típus |
| --- | --- |
| id |Karakterlánc |
| kulcs |Karakterlánc |
| keyType |64 egész szám |
| Tábla |Karakterlánc |
| érték |Karakterlánc |

### <a name="data-table"></a>Adattábla

**TodoItem**

| Attribútum | Típus | Megjegyzés |
| --- | --- | --- |
| id | Karakterlánc, kötelezőként megjelölt |elsődleges távoli tárolóban levő kulccsal. |
| Végezze el | Logikai érték | Tennivalók elem mező |
| Szöveg |Karakterlánc |Tennivalók elem mező |
| createdAt | Dátum | (választható) Túl leképezhető**createdAt** rendszer tulajdonság |
| updatedAt | Dátum | (választható) Túl leképezhető**updatedAt** rendszer tulajdonság |
| Verzió | Karakterlánc | (választható) Használt toodetect ütközések, maps tooversion |

## <a name="setup-sync"></a>Hello szinkronizálási megváltozzon hello alkalmazás
Ebben a szakaszban módosítani hello alkalmazást, hogy nincs szinkronban az alkalmazás indítása vagy beszúrása, és ha a elemek frissítését. A szinkronizált csak hello frissítés kézmozdulat gomb végrehajtásakor.

**Objective-C**:

1. A **QSTodoListViewController.m**, módosítsa a hello **viewDidLoad** metódus tooremove hello hívás túl`[self refresh]` hello metódus hello végén. Hello adatok hello Server alkalmazás indítása most nincs szinkronizálva. Ehelyett azt szinkronizálva van hello helyi tároló hello tartalmát.
2. A **QSTodoService.m**, hello definíciójának módosítása `addItem` , hogy azt nem szinkronizálása után hello sort kell beszúrni. Távolítsa el a hello `self syncData` letiltása, és cserélje le a következő hello:

   ```objc
   if (completion != nil) {
       dispatch_async(dispatch_get_main_queue(), completion);
   }
   ```
3. Hello definíciójának módosítása `completeItem` amint azt korábban említettük. Távolítsa el a hello blokkja `self syncData` és cserélje le a következő hello:
   ```objc
   if (completion != nil) {
       dispatch_async(dispatch_get_main_queue(), completion);
   }
   ```

**SWIFT**:

A `viewDidLoad`, a **ToDoTableViewController.swift**, hello két sort látható itt, az alkalmazás indítása szinkronizálása toostop megjegyzésbe. Írásának hello időpontban hello Swift teendőlista alkalmazás nem frissíthető hello szolgáltatást Ha valaki ad hozzá, vagy egy elem befejeződött. Hello szolgáltatást csak az alkalmazás indítása frissíti.

   ```swift
  self.refreshControl?.beginRefreshing()
  self.onRefresh(self.refreshControl)
```

## <a name="test-app"></a>Teszt hello alkalmazás
Ebben a szakaszban tooan érvénytelen URL-cím toosimulate egy kapcsolat nélküli keresztül csatlakozni. Adatelemek hozzáadásakor azok által birtokolt hello a helyi alapvető adatok tárolására, de azok még nem szinkronizálta hello mobilalkalmazás háttérből.

1. Módosítsa a hello mobilalkalmazás URL-címet **QSTodoService.m** tooan URL-cím érvénytelen, és újra futtatási hello alkalmazást:

   **Objective-C**. A QSTodoService.m:
   ```objc
   self.client = [MSClient clientWithApplicationURLString:@"https://sitename.azurewebsites.net.fail"];
   ```
   **SWIFT**. A ToDoTableViewController.swift:
   ```swift
   let client = MSClient(applicationURLString: "https://sitename.azurewebsites.net.fail")
   ```
2. Adja hozzá az egyes Tennivalólista elemein. Lépjen ki a hello szimulátor (vagy a kényszerített bezárása hello alkalmazást), és indítsa újra. Győződjön meg arról, hogy a változtatások megmaradnak.

3. Távoli hello hello tartalmának megtekintése **TodoItem** tábla:
   * A Node.js háttérből, nyissa meg a toohello [Azure-portálon](https://portal.azure.com/) és kattintson a mobilalkalmazás háttér **könnyen táblák** > **TodoItem**.  
   * A .NET vissza az end, egy SQL-eszközt, például SQL Server Management Studio alkalmazást, vagy a többi ügyfél, például a Postman vagy a Fiddler használatával.  

4. Ellenőrizze, hogy rendelkezik-e új elemek hello *nem* megtörtént szinkronizálta hello kiszolgáló.

5. Változás hello URL-cím hátsó toohello javítsa ki, egy-egy **QSTodoService.m**, és futtassa újra hello alkalmazást.

6. Hajtsa végre a hello frissítési kézmozdulat modulba húzza lefelé hello elemek listáját.  
A folyamatban lévő léptető jelenik meg.

7. Nézet hello **TodoItem** újra adatokat. hello új és módosított Tennivalólista elemein meg kell jelennie.

## <a name="summary"></a>Összefoglalás
hello használtuk toosupport hello kapcsolat nélküli szinkronizálás szolgáltatás `MSSyncTable` felületet, és inicializálva `MSClient.syncContext` a helyi tárolójába. Ebben az esetben hello helyi tároló lett egy alapvető adatok-alapú adatbázist.

A Core helyi tárolóban használatakor meg kell adnia a hello több táblákban [javítsa ki a rendszer tulajdonságai](#review-core-data).

hello normál létrehozása, olvasása, frissítése és törölhetők, (CRUD) típusú műveletek mobilalkalmazások együttműködnek a hello alkalmazás még a kapcsolat, de minden hello műveletet hello helyi tárolóban történik.

Ha azt a szinkronizált hello helyi tároló hello kiszolgálóval, használtuk hello **MSSyncTable.pullWithQuery** metódust.

## <a name="additional-resources"></a>További források
* [Mobile Apps az Offline adatszinkronizálás]
* [Felhő tartalma: Kapcsolat nélküli szinkronizálás az Azure Mobile Services] \(videó hello kapcsolatos Mobile Services, de a Mobile Apps kapcsolat nélküli szinkronizálás működésének hasonló módon.\)

<!-- URLs. -->


[iOS-alkalmazás létrehozása]: app-service-mobile-ios-get-started.md
[Mobile Apps az Offline adatszinkronizálás]: app-service-mobile-offline-data-sync.md

[defining-core-data-tableoperationerrors-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-tableoperationerrors-entity.png
[defining-core-data-tableoperations-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-tableoperations-entity.png
[defining-core-data-tableconfig-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-tableconfig-entity.png
[defining-core-data-todoitem-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-todoitem-entity.png

[Felhő tartalma: Kapcsolat nélküli szinkronizálás az Azure Mobile Services]: http://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri
[Azure Friday: Offline-enabled apps in Azure Mobile Services]: http://azure.microsoft.com/en-us/documentation/videos/azure-mobile-services-offline-enabled-apps-with-donna-malayeri/
