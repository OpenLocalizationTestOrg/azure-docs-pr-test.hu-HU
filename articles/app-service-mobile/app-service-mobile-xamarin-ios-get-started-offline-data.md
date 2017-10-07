---
title: "kapcsolat nélküli szinkronizálásának aaaEnable az Azure Mobile Apps (Xamarin iOS)"
description: "Megtudhatja, hogyan toouse App Service Mobile Apps toocache és szinkronizálási offline adatokat a Xamarin iOS-alkalmazás"
documentationcenter: xamarin
author: ggailey777
manager: cfowler
editor: 
services: app-service\mobile
ms.assetid: 828a287c-5d58-4540-9527-1309ebb0f32b
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-ios
ms.devlang: dotnet
ms.topic: article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: 5243f2d292377d8de103a40f45d649394f11b97c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="enable-offline-sync-for-your-xamarinios-mobile-app"></a>Tegye lehetővé az offline szinkronizálást Xamarin.iOS-mobilalkalmazásában
[!INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Áttekintés
Ez az oktatóanyag az Azure Mobile Apps hello kapcsolat nélküli szinkronizálás szolgáltatása xamarin.IOS vezet be. Kapcsolat nélküli szinkronizálás lehetővé teszi, hogy a befejező felhasználóknak toointeract mobilalkalmazást--megtekintését, hozzáadását és módosítását adatokat –, akkor is, ha nincs hálózati kapcsolat. Változások a helyi adatbázisban tárolódnak. Hello eszköz újra online állapotba kerül, ha ezek a változások szinkronizálása megtörtént-e távoli hello szolgáltatást.

Ebben az oktatóanyagban frissítése hello Xamarin.iOS app projektet a [Xamarin iOS-alkalmazás létrehozása] toosupport hello Azure Mobile Apps offline funkcióit. Ha nem használ hello letöltése – első lépések, hello data access bővítménycsomagok hozzá kell adnia a projekthez. Kiszolgáló bővítménycsomagok kapcsolatos további információkért lásd: [használható hello .NET háttérkiszolgáló SDK az Azure Mobile Apps a](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

További információ az hello kapcsolat nélküli szinkronizálás szolgáltatást, toolearn hello témakörben találhatók [az Azure Mobile Apps Offline adatszinkronizálás].

## <a name="update-hello-client-app-toosupport-offline-features"></a>Hello ügyfél app toosupport offline funkciók frissítése
Az Azure Mobile Apps offline funkciók lehetővé teszik egy helyi adatbázis toointeract amikor egy offline állapotban lévő. toouse ezeket a szolgáltatásokat az alkalmazásban inicializálni egy [SyncContext] tooa helyi tárolóból. A [IMobileServiceSyncTable] hello felületen táblájára hivatkozhat. SQLite hello eszközön hello helyi tárolóként szolgál.

1. Nyissa meg hello NuGet-Csomagkezelő hello projektben hello elvégzett [Xamarin iOS-alkalmazás létrehozása] oktatóanyag, majd keresse meg és telepítési hello **Microsoft.Azure.Mobile.Client.SQLiteStore** NuGet a csomag.
2. Nyissa meg hello QSTodoService.cs fájlt, és állítsa vissza a hello `#define OFFLINE_SYNC_ENABLED` definíciója.
3. Építse újra és hello ügyfélalkalmazás futtatása. hello az alkalmazás akkor működik a hello ugyanaz, mint az adta meg a kapcsolat nélküli szinkronizálás engedélyezése előtt. Hello helyi adatbázis azonban most van feltöltve adatokkal egy kapcsolat nélküli forgatókönyvben használható.

## <a name="update-sync"></a>Hello app toodisconnect hello háttérrendszerből frissítése
Ebben a szakaszban bontsa hello kapcsolat tooyour mobilalkalmazás háttér toosimulate egy kapcsolat nélküli helyzet. Amikor az elemeket, a kivételkezelő megtudhatja, hello alkalmazást offline módban van. Ebben az állapotban lévő helyi hello hozzáadott új elemek tárolja, és a mobil-háttéralkalmazás hello leküldéses csatlakoztatott állapotban következő futása során lesznek szinkronizálva.

1. Szerkessze a QSToDoService.cs hello megosztott projektben. Változás hello **applicationURL** toopoint tooan URL-cím érvénytelen:

         const string applicationURL = @"https://your-service.azurewebsites.fail";

    Kapcsolat nélküli viselkedés igazolni Wi-Fi és hello eszközön cellás hálózatok letiltása vagy repülőgép üzemmódban.
2. Hozza létre, és futtassa hello alkalmazást. Figyelje meg, hogy a frissítéskor sikertelen szinkronizálás amikor hello alkalmazást elindítva.
3. Adja meg az új elemeket, és figyelje meg, hogy leküldéses sikertelen [CancelledByNetworkError] állapotú gombra kattintáskor **mentése**. Azonban hello új todo elemeket található hello helyi tárolóban mindaddig, amíg azok toohello mobil-háttéralkalmazás továbbíthatja.  Egy éles környezetben az alkalmazást, ha Ön ne jelenjen meg többé az ilyen kivételek hello ügyfélalkalmazás úgy viselkedik, mintha az még mindig toohello mobil-háttéralkalmazás csatlakoztatva.
4. Hello-alkalmazások bezárása, és indítsa újra azt, hogy hello létrehozott új elemek-e a megőrzött toohello helyi tároló tooverify.
5. (Választható) Ha a számítógépen telepített Visual Studio, nyissa meg a **Server Explorer**. Keresse meg a tooyour adatbázis **Azure**-> **SQL-adatbázisok**. Kattintson a jobb gombbal az adatbázist, és válassza ki **nyissa meg az SQL Server Object Explorerben**. Most már megkeresheti tooyour SQL-adatbázistáblában szereplő és annak tartalmát. Győződjön meg arról, hogy hello háttérbeli adatbázis hello adatai nem módosult.
6. (Választható) Például a Postman vagy a Fiddler tooquery REST eszközök használata a mobil-háttéralkalmazást, GET lekérdezéssel formájában `https://<your-mobile-app-backend-name>.azurewebsites.net/tables/TodoItem`.

## <a name="update-online-app"></a>Hello app tooreconnect a Mobile Apps-háttéralkalmazás frissítése
Ebben a szakaszban újracsatlakozás hello app toohello mobil-háttéralkalmazást. Helyezze át az offline állapotban tooan online állapotú a mobil-háttéralkalmazás hello hello app ez szimulálja.   Hello hálózati törés szimulálja kikapcsolása a hálózati kapcsolatot, ha nincs kód módosítására van szükség.
Kapcsolja hello hálózati be újból.  Hello alkalmazás első futtatásakor hello `RefreshDataAsync` metódust. Ezután meghívja `SyncAsync` toosync a helyi hello háttérbeli adatbázis tárolja.

1. Nyissa meg a QSToDoService.cs hello projektet, és állítsa vissza a módosítás a hello **applicationURL** tulajdonság.
2. Építse újra és hello alkalmazás futtatásához. hello app szinkronizál a helyi módosítások hello Azure Mobile Apps-háttéralkalmazás segítségével eseménylekérési és eseményküldési műveletek során hello `OnRefreshItemsSelected` metódus.
3. (Választható) Nézet hello a módosított adatokat az SQL Server Object Explorer vagy egy REST-eszköz, például Fiddler használatával. Értesítés hello adatok között hello Azure Mobile Apps-háttéralkalmazás adatbázis és a helyi tárolójába hello lett szinkronizálva.
4. Hello alkalmazásban kattintva ellenőrizheti az hello jelre néhány elemek toocomplete őket hello helyi tárolóban levő.

   `CompleteItemAsync`hívások `SyncAsync` hello Mobile Apps-háttéralkalmazás elem toosync minden befejeződött. `SyncAsync`meghívja a lekérést és a küldést.
   **Amikor egy lekéréses egy táblázaton végrehajtása hello ügyfél által végrehajtott módosítások, a szinkronizálási ügyfélkontextus hello leküldéses a rendszer mindig futtatja először automatikusan**. hello implicit leküldéses biztosítja, hogy együtt kapcsolatok hello a helyi tárolóban levő összes tábla azonban konzisztens marad. Ez a viselkedés további információkért lásd: [az Azure Mobile Apps Offline adatszinkronizálás].

## <a name="review-hello-client-sync-code"></a>Tekintse át a kód a hello ügyfél szinkronizálása
hello Xamarin ügyfélprojekt hello oktatóanyag befejezésekor letöltött [Xamarin iOS-alkalmazás létrehozása] már tartalmaz egy helyi SQLite-adatbázis használata a kapcsolat nélküli szinkronizálást támogató kódot. Ez már tartalmának hello oktatóanyag kód rövid áttekintést. Hello szolgáltatás elméleti áttekintését lásd: [az Azure Mobile Apps Offline adatszinkronizálás].

* Minden tábla műveletek elvégzése előtt hello helyi tároló inicializálni kell. hello helyi tároló adatbázis inicializálása során `QSTodoListViewController.ViewDidLoad()` végrehajtja a `QSTodoService.InitializeStoreAsync()`. Ez a módszer új adatbázist hoz létre helyi SQLite használatával hello `MobileServiceSQLiteStore` hello Azure Mobile Apps-ügyfél SDK által meghatározott osztály.

    Hello `DefineTable` metódus táblázatot hoz létre, amely megfelel a megadott hello típusban hello mezők hello helyi tárolóban levő `ToDoItem` ebben az esetben. hello típushoz nem tartozik tooinclude hello távoli adatbázisban lévő összes hello-oszlopot. Lehetséges toostore oszlopok egy részét is.

        // QSTodoService.cs

        public async Task InitializeStoreAsync()
        {
            var store = new MobileServiceSQLiteStore(localDbPath);
            store.DefineTable<ToDoItem>();

            // Uses hello default conflict handler, which fails on conflict
            await client.SyncContext.InitializeAsync(store);
        }
* Hello `todoTable` tagja `QSTodoService` a hello `IMobileServiceSyncTable` helyett típusú `IMobileServiceTable`. hello IMobileServiceSyncTable összes létrehozása, olvasása, frissítése és törlése (CRUD) tábla műveletek toohello helyi tároló adatbázis irányítja.

    Úgy dönt, ha ezek a módosítások vannak leküldött toohello Azure Mobile Apps-háttéralkalmazás meghívásával `IMobileServiceSyncContext.PushAsync()`. hello szinkronizálási környezetet segíti követésével és kérdez le a módosításokat minden olyan táblát, egy ügyfélalkalmazás módosította, ha a tábla közötti megőrzése érdekében `PushAsync` nevezik.

    a megadott hello kód hívások `QSTodoService.SyncAsync()` toosync hello todoitem listájának frissítését, és a todoitem hozzá, vagy befejeződött. Az alkalmazás szinkronizálások minden helyi módosítás után. Ha egy lekéréses van függőben lévő helyi frissítést nyomon követett táblákon hello környezetben futtatja, lekéréses művelet automatikusan indít környezetben leküldéses először.

    A megadott hello kódban, az összes távoli hello rögzíti `TodoItem` tábla a rendszer megkérdezi, de azt is lehetséges toofilter rekordok úgy, hogy a lekérdezés azonosítóját és a lekérdezés túl`PushAsync`. További információkért lásd: hello szakasz *növekményes szinkronizálás* a [az Azure Mobile Apps Offline adatszinkronizálás].

        // QSTodoService.cs
        public async Task SyncAsync()
        {
            try
            {
                await client.SyncContext.PushAsync();
                await todoTable.PullAsync("allTodoItems", todoTable.CreateQuery()); // query ID is used for incremental sync
            }

            catch (MobileServiceInvalidOperationException e)
            {
                Console.Error.WriteLine(@"Sync Failed: {0}", e.Message);
            }
        }

## <a name="additional-resources"></a>További források
* [az Azure Mobile Apps Offline adatszinkronizálás]
* [Az Azure Mobile Apps .NET SDK útmutató][8]

<!-- Images -->

<!-- URLs. -->
[Xamarin iOS-alkalmazás létrehozása]: app-service-mobile-xamarin-ios-get-started.md
[az Azure Mobile Apps Offline adatszinkronizálás]: app-service-mobile-offline-data-sync.md
[SyncContext]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient.synccontext(v=azure.10).aspx
[8]: app-service-mobile-dotnet-how-to-use-client-library.md
