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
# <a name="enable-offline-sync-for-your-xamarinforms-mobile-app"></a>Xamarin.Forms-mobilalkalmazás offline szinkronizálásának engedélyezése
[!INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Áttekintés
Ez az oktatóanyag hello kapcsolat nélküli szinkronizálás szolgáltatást az Azure Mobile Apps xamarin.Forms vezet be. Kapcsolat nélküli szinkronizálás lehetővé teszi, hogy a végfelhasználók számára, amellyel kommunikálni tud a mobilalkalmazások--megtekintését, hozzáadását és módosítását adatokat –, akkor is, ha nincs hálózati kapcsolat. Változások a helyi adatbázisban tárolódnak. Hello eszköz újra online állapotba kerül, ha ezek a változások szinkronizálása megtörtént-e távoli hello szolgáltatást.

Ez az oktatóanyag hello gyors üzembe helyezés Xamarin.Forms-megoldás alapján hoz létre, amikor befejezte az oktatóanyag [Xamarin iOS-alkalmazás létrehozása] Mobile Apps-alkalmazáshoz. hello gyors üzembe helyezés megoldás xamarin.Forms hello kód toosupport kapcsolat nélküli szinkronizálás, amely csak az engedélyezett toobe kell tartalmaz. Ebben az oktatóanyagban hello gyors üzembe helyezés megoldás tooturn hello offline szolgáltatások az Azure Mobile Apps frissíti. A Microsoft hello offline tartozó kódot hello alkalmazásban is jelöljön ki. Ha nem használja a letöltött hello gyors üzembe helyezés megoldás, hozzá kell adnia a hello data access kiegészítő csomagok tooyour projekt. Kiszolgáló bővítménycsomagok kapcsolatos további információkért lásd: [használható hello .NET háttérkiszolgáló SDK az Azure Mobile Apps a][1].

További információ az hello kapcsolat nélküli szinkronizálás szolgáltatást, toolearn hello témakörben találhatók [az Azure Mobile Apps Offline adatszinkronizálás][2].

## <a name="enable-offline-sync-functionality-in-hello-quickstart-solution"></a>Hello gyors üzembe helyezés megoldásban kapcsolat nélküli szinkronizálásának engedélyezése
hello kapcsolat nélküli szinkronizálás kód hello projekt C# előfeldolgozó irányelvek segítségével tartalmazza. Ha hello **OFFLINE\_SZINKRONIZÁLÁSI\_engedélyezve** szimbólum van megadva, a kód elérési utak hello build szerepelnek. Windows-alkalmazások esetén is telepíteni kell a hello SQLite platform.

1. A Visual Studióban, kattintson a jobb gombbal hello megoldás > **NuGet-csomagok kezelése megoldáshoz...** , majd keresse meg, és telepítse a **Microsoft.Azure.Mobile.Client.SQLiteStore** projektek hello megoldás NuGet-csomagot.
2. A Solution Explorer hello, nyissa meg az hello TodoItemManager.cs fájlt hello projektből az **hordozható** hello neve, amely hordozható Class Library projektet, majd állítsa vissza a következő előfeldolgozó irányelv hello:

        #define OFFLINE_SYNC_ENABLED
3. (Választható) toosupport Windows-eszközök esetében egy SQLite futtatókörnyezetek következő hello telepíthető:

   * **Windows 8.1 futásidejű:** telepítése [a Windows 8.1 SQLite][3].
   * **Windows Phone 8.1:** telepítése [a Windows Phone 8.1 SQLite][4].
   * **Univerzális Windows Platform** telepítése [az univerzális Windows Universal hello SQLite][5].

     Bár a hello gyors üzembe helyezés nem tartalmaz egy univerzális Windows-projektjét, Xamarin Forms hello univerzális Windows platform használata támogatott.
4. (Választható) Minden Windows-alkalmazás projekt, kattintson a jobb egérgombbal **hivatkozások** > **hivatkozás hozzáadása...** , bontsa ki a hello **Windows** mappa > **bővítmények**.
    Engedélyezze a megfelelő hello **SQLite for Windows** együtt hello SDK **Visual C++ 2013 futásidejű for Windows** SDK.
    hello SQLite SDK nevek függően eltérőek az egyes Windows-platformra.

## <a name="review-hello-client-sync-code"></a>Tekintse át a kód a hello ügyfél szinkronizálása
Ez már tartalmának hello oktatóanyag kódot hello rövid áttekintést `#if OFFLINE_SYNC_ENABLED` irányelvek. A kapcsolat nélküli szinkronizálásának hello hello hordozható osztály Hordozhatóosztálytár-projektjének TodoItemManager.cs projektfájlt van. Hello szolgáltatás elméleti áttekintését lásd: [az Azure Mobile Apps Offline adatszinkronizálás][2].

* Minden tábla műveletek elvégzése előtt hello helyi tároló inicializálni kell. hello helyi tároló adatbázis inicializálva van a hello **TodoItemManager** osztálykonstruktor hello kód a következő használatával:

        var store = new MobileServiceSQLiteStore(OfflineDbPath);
        store.DefineTable<TodoItem>();

        //Initializes hello SyncContext using hello default IMobileServiceSyncHandler.
        this.client.SyncContext.InitializeAsync(store);

        this.todoTable = client.GetSyncTable<TodoItem>();

    Ez a kód egy új adatbázist hoz létre helyi SQLite használatával hello **MobileServiceSQLiteStore** osztály.

    Hello **DefineTable** metódus táblázatot hoz létre, amely megfelel a megadott hello típus hello mezői hello helyi tárolóban levő.  hello típushoz nem tartozik tooinclude hello távoli adatbázisban lévő összes hello-oszlopot. Lehetséges toostore az oszlopok egy részét is.
* Hello **todoTable** mezőjét **TodoItemManager** van egy **IMobileServiceSyncTable** helyett típusú **IMobileServiceTable**. Ez az osztály által használt hello helyi adatbázis összes létrehozása, olvasása, frissítése és Törlés (CRUD) tábla műveletek. Úgy dönt, ha ezek a módosítások vannak leküldött toohello Mobile Apps-háttéralkalmazás meghívásával **PushAsync** a hello **IMobileServiceSyncContext**. hello szinkronizálási környezetet segíti követésével és kérdez le a módosításokat minden olyan táblát, egy ügyfélalkalmazás módosította, ha a tábla közötti megőrzése érdekében **PushAsync** nevezik.

    hello következő **SyncAsync** metódus lehívásra kerül a Mobile Apps-háttéralkalmazás hello toosync:

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

    Ez a minta hello alapértelmezett szinkronizálási leírójú egyszerű hibakezelési használja. Egy valós alkalmazás hello például hálózati feltételek és a kiszolgáló különböző hibák ütközik egy egyéni használatával szeretné kezelni **IMobileServiceSyncHandler** végrehajtására.

## <a name="offline-sync-considerations"></a>Kapcsolat nélküli szinkronizálás kapcsolatos szempontok
Hello mintában hello **SyncAsync** metódus csak meghívta az indítási és amikor a szinkronizálás van szükség.  Android vagy iOS alkalmazások, lekéréses le hello elemek listája; a szinkronizálási tooinitiate a Windows hello használata **szinkronizálási** gombra. Egy valós alkalmazás is meg sikerült hello szinkronizálási eseményindító, ha hello hálózati állapota megváltozik.

Elleni táblának függőben lévő lekérési végrehajtásakor helyi frissítések követi nyomon, hogy a pull művelet automatikusan eseményindítók hello összefüggésben egy előző környezetben leküldéses. Ha frissíteni, hozzáadása, és ez a példa elemek befejezése, akkor kihagyhatja hello explicit **PushAsync** hívható meg.

A megadott hello kódban, a rendszer megkérdezi a hello távoli TodoItem tábla összes adatát, de azt is lehetséges toofilter rekordok úgy, hogy a lekérdezés azonosítóját és a lekérdezés túl**PushAsync**. További információkért lásd: hello szakasz *növekményes szinkronizálás* a [az Azure Mobile Apps Offline adatszinkronizálás][2].

## <a name="run-hello-client-app"></a>Hello ügyfél-alkalmazás futtatása
Kapcsolat nélküli szinkronizálás most engedélyezve van futtatja a hello ügyfélalkalmazás legalább egyszer minden egyes platform toopopulate hello helyi tároló adatbázis. Később egy kapcsolat nélküli forgatókönyv szimulálása és hello adatok hello helyi tárolóban levő módosítható, amíg az alkalmazás hello offline állapotban.

## <a name="update-hello-sync-behavior-of-hello-client-app"></a>Hello szinkronizálási viselkedését hello ügyfélalkalmazás frissítése
Ebben a szakaszban módosítsa hello ügyfél projekt toosimulate egy kapcsolat nélküli forgatókönyv a háttérkiszolgáló URL-címének érvénytelen az alkalmazás használatával. Alternatív megoldásként kikapcsolhatja hálózati kapcsolatok túl át az eszköz "Repülőgép üzemmódban."  Hozzáadásakor, vagy módosítsa az elemeket, ezek a változások hello helyi tárolóban levő tartani, de nem szinkronizált toohello háttérrendszeri adatok tárolására, amíg hello kapcsolat helyreáll.

1. A Solution Explorer hello, nyissa meg az hello Constants.cs projektfájlt a hello **hordozható** projektre, és hello értékének módosítása `ApplicationURL` toopoint tooan URL-cím érvénytelen:

        public static string ApplicationURL = @"https://your-service.azurewebsites.net/";
2. Megnyitás hello TodoItemManager.cs fájlt hello **hordozható** projektre, majd adja hozzá a **catch** az alapszintű hello **kivétel** osztály toohello **próbálja... catch** letiltása **SyncAsync**. Ez **catch** blokk ír hello kivétel üzenet toohello konzol, az alábbiak szerint:

            catch (Exception ex)
            {
                Console.Error.WriteLine(@"Exception: {0}", ex.Message);
            }
3. Hozza létre és hello ügyfélalkalmazás futtatása.  Vegyen fel egy új elemeket. Figyelje meg, hogy kivételt hello konzol minden kísérlet toosync hello háttérrendszerrel van bejelentkezve. Ezek az új elemek található csak hello helyi tárolóban mindaddig, amíg azok toohello mobil-háttéralkalmazást továbbíthatja. hello ügyfélalkalmazás úgy viselkedik, mintha már csatlakoztatott toohello háttér, támogató, az összes létrehozása, olvasása, frissítés, Törlés (CRUD) műveleteket.
4. Hello-alkalmazások bezárása, és indítsa újra azt, hogy hello létrehozott új elemek-e a megőrzött toohello helyi tároló tooverify.
5. (Választható) Visual Studio tooview használja az Azure SQL Database tábla toosee hello adatok hello háttérbeli adatbázis nem módosított.

    A Visual Studióban nyissa meg a **Server Explorer**. Keresse meg a tooyour adatbázis **Azure**->**SQL-adatbázisok**. Kattintson a jobb gombbal az adatbázist, és válassza ki **nyissa meg az SQL Server Object Explorerben**. Most már megkeresheti tooyour SQL-adatbázistáblában szereplő és annak tartalmát.

## <a name="update-hello-client-app-tooreconnect-your-mobile-backend"></a>Hello ügyfél app tooreconnect a mobil-háttéralkalmazást frissítése
Ebben a szakaszban újracsatlakozás hello toohello mobil háttéralkalmazás, szimulálja hello alkalmazáshoz hamarosan vissza tooan online állapotban. Hello frissítési kézmozdulat végrehajtásakor a adata szinkronizált tooyour mobil-háttéralkalmazást.

1. Nyissa meg újra a Constants.cs. Megfelelő hello `applicationURL` toopoint toohello javítsa ki az URL-CÍMÉT.
2. Építse újra és hello ügyfélalkalmazás futtatása. hello alkalmazás indítását követően próbálja meg a mobil-háttéralkalmazás hello toosync. Győződjön meg arról, hogy nem tartalmaznak kivételeket hello hibakereső konzol vannak bejelentkezve.
3. (Választható) Nézet hello a módosított adatokat az SQL Server Object Explorer vagy egy REST-eszköz, például Fiddler használatával vagy [Postman][6]. Értesítés hello adatok hello háttér adatbázis és a helyi tárolójába hello lett szinkronizálva.

    Figyelje meg hello adatok hello adatbázis és a helyi tárolójába hello lett szinkronizálva, és az alkalmazás forráshoz hozzáadott hello elemet tartalmaz.

## <a name="additional-resources"></a>További források
* [Az Azure Mobile Apps az offline adatszinkronizálás][2]
* [Az Azure Mobile Apps .NET SDK útmutató][8]

<!-- URLs. -->
[1]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[2]: app-service-mobile-offline-data-sync.md
[3]: http://go.microsoft.com/fwlink/p/?LinkID=716919
[4]: http://go.microsoft.com/fwlink/p/?LinkID=716920
[5]: http://sqlite.org/2016/sqlite-uwp-3120200.vsix
[6]: https://www.getpostman.com/
[7]: http://www.telerik.com/fiddler
[8]: app-service-mobile-dotnet-how-to-use-client-library.md
