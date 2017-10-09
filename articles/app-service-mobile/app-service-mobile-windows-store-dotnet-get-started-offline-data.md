---
title: "az univerzális Windows Platform (UWP-) alkalmazás, a Mobile Apps aaaEnable kapcsolat nélküli szinkronizálásának |} Microsoft Docs"
description: "Megtudhatja, hogyan toouse egy Azure Mobile Apps toocache és a szinkronizálási kapcsolat nélküli adatok az univerzális Windows Platform (UWP) alkalmazásban."
documentationcenter: windows
author: ggailey777
manager: syntaxc4
editor: 
services: app-service\mobile
ms.assetid: 8fe51773-90de-4014-8a38-41544446d9b5
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: a9f4ad02e92c2c423f10f07b7f1a4270aafd6c6f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="enable-offline-sync-for-your-windows-app"></a>Windows-alkalmazás kapcsolat nélküli szinkronizálásának engedélyezése
[!INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Áttekintés
Az oktatóanyag bemutatja, hogyan támogatják a kapcsolat nélküli tooadd a tooa univerzális Windows Platform (UWP) alkalmazást egy Azure Mobile Apps-háttéralkalmazás segítségével. Kapcsolat nélküli szinkronizálás lehetővé teszi, hogy a befejező felhasználóknak toointeract mobilalkalmazást--megtekintését, hozzáadását és - adatok módosítását, akkor is, ha nincs hálózati kapcsolat. Változások a helyi adatbázisban tárolódnak. Hello eszköz újra online állapotba kerül, ha ezek a változások szinkronizálása megtörtént-e hello távoli háttér.

Ebben az oktatóanyagban hello UWP-alkalmazásprojektet hello oktatóanyagot frissítése [Windows-alkalmazás létrehozása] toosupport hello Azure Mobile Apps offline funkcióit. Ha nem használ hello letöltése – első lépések, hello data access kiegészítő csomagok tooyour projekt hozzá kell adnia. Kiszolgáló bővítménycsomagok kapcsolatos további információkért lásd: [használható hello .NET háttérkiszolgáló SDK az Azure Mobile Apps a](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

További információ az hello kapcsolat nélküli szinkronizálás szolgáltatást, toolearn hello témakörben találhatók [az Azure Mobile Apps Offline adatszinkronizálás].

## <a name="requirements"></a>Követelmények
Ez az oktatóanyag a következő előfeltételek hello van szükség:

* A Visual Studio 2013 fut, a Windows 8.1 vagy újabb.
* Megvalósításának [Windows-alkalmazás létrehozása][windows-alkalmazás létrehozása].
* [Az Azure Mobile Services SQLite Store][sqlite store nuget]
* [Univerzális Windows Platform fejlesztési SQLite](http://www.sqlite.org/downloads)

## <a name="update-hello-client-app-toosupport-offline-features"></a>Hello ügyfél app toosupport offline funkciók frissítése
Az Azure Mobile Apps offline funkciók lehetővé teszik egy helyi adatbázis toointeract amikor egy offline állapotban lévő. toouse ezeket a szolgáltatásokat az alkalmazásban, inicializálni egy [SyncContext] [ synccontext] tooa helyi tárolóból. Majd hivatkozzon a táblához hello [IMobileServiceSyncTable][IMobileServiceSyncTable] felületet. SQLite hello eszközön hello helyi tárolóként szolgál.

1. Telepítse a hello [SQLite futásidejű az univerzális Windows Platform hello](http://sqlite.org/2016/sqlite-uwp-3120200.vsix).
2. A Visual Studióban nyissa meg a hello NuGet-Csomagkezelő hello UWP alkalmazásprojektet hello elvégzett [Windows-alkalmazás létrehozása] oktatóanyag.
    Keresse meg és telepítse a hello **Microsoft.Azure.Mobile.Client.SQLiteStore** NuGet-csomagot.
3. A Megoldáskezelőben kattintson a jobb gombbal **hivatkozások** > **hivatkozás hozzáadása...** >**Univerzális Windows** > **bővítmények**, engedélyeznie kell a mindkét **univerzális Windows Platform SQLite** és **a Visual C++ 2015 futtatókörnyezete Az univerzális Windows Platform alkalmazások**.

    ![SQLite UWP-hivatkozás hozzáadása][1]
4. Nyissa meg a hello MainPage.xaml.cs fájlban, és állítsa vissza a hello `#define OFFLINE_SYNC_ENABLED` definíciója.
5. A Visual Studio, nyomja le a hello **F5** toorebuild és futtatási hello ügyfélalkalmazást. hello az alkalmazás akkor működik a hello ugyanaz, mint az adta meg a kapcsolat nélküli szinkronizálás engedélyezése előtt. Hello helyi adatbázis azonban most van feltöltve adatokkal egy kapcsolat nélküli forgatókönyvben használható.

## <a name="update-sync"></a>Hello app toodisconnect hello háttérrendszerből frissítése
Ebben a szakaszban bontsa hello kapcsolat tooyour mobilalkalmazás háttér toosimulate egy kapcsolat nélküli helyzet. Amikor az elemeket, a kivételkezelő megtudhatja, hello alkalmazást offline módban van. Ebben az állapotban lévő helyi hello hozzáadott új elemek tárolja, és a mobil-háttéralkalmazás hello leküldéses csatlakoztatott állapotban következő futása során lesznek szinkronizálva.

1. Szerkessze az App.xaml.cs hello megosztott projektben. Hello hello inicializálása megjegyzésbe **MobileServiceClient** , és adja hozzá a sort, amely egy érvénytelen mobilalkalmazás URL-CÍMÉT használja a következő hello:

         public static MobileServiceClient MobileService = new MobileServiceClient("https://your-service.azurewebsites.fail");

    Kapcsolat nélküli viselkedés bemutatják a Wi-Fi és hello eszközön cellás hálózatok letiltásával vagy repülőgép üzemmódban is.
2. Nyomja le az **F5** toobuild, és futtassa hello alkalmazást. Figyelje meg, hogy a frissítéskor sikertelen szinkronizálás amikor hello alkalmazást elindítva.
3. Adja meg az új elemeket, és figyelje meg, hogy leküldéses sikertelen, és egy [CancelledByNetworkError] állapota minden egyes kattint **mentése**. Azonban hello új todo elemeket található hello helyi tárolóban mindaddig, amíg azok toohello mobil-háttéralkalmazás továbbíthatja.  Egy éles környezetben az alkalmazást, ha Ön ne jelenjen meg többé az ilyen kivételek hello ügyfélalkalmazás úgy viselkedik, mintha az még mindig toohello mobil-háttéralkalmazás csatlakoztatva.
4. Hello-alkalmazások bezárása, és indítsa újra azt, hogy hello létrehozott új elemek-e a megőrzött toohello helyi tároló tooverify.
5. (Választható) A Visual Studióban nyissa meg a **Server Explorer**. Keresse meg a tooyour adatbázis **Azure**->**SQL-adatbázisok**. Kattintson a jobb gombbal az adatbázist, és válassza ki **nyissa meg az SQL Server Object Explorerben**. Most már megkeresheti tooyour SQL-adatbázistáblában szereplő és annak tartalmát. Győződjön meg arról, hogy hello háttérbeli adatbázis hello adatai nem módosult.
6. (Választható) Például a Postman vagy a Fiddler tooquery REST eszközök használata a mobil-háttéralkalmazást, GET lekérdezéssel formájában `https://<your-mobile-app-backend-name>.azurewebsites.net/tables/TodoItem`.

## <a name="update-online-app"></a>Hello app tooreconnect a Mobile Apps-háttéralkalmazás frissítése
Ebben a szakaszban hello app toohello mobil-háttéralkalmazás újra. Ezek a változások szimulálása egy hálózati újracsatlakozás hello alkalmazásában.

Hello alkalmazás első futtatásakor hello `OnNavigatedTo` eseménykezelő-hívások `InitLocalStoreAsync`. Ez a metódus meghívja a `SyncAsync` toosync a helyi hello háttérbeli adatbázis tárolja. hello app kísérel meg indításkor toosync.

1. Nyissa meg az App.xaml.cs hello megosztott projektben, és állítsa vissza az előző inicializálása `MobileServiceClient` toouse hello megfelelő hello mobilalkalmazás URL-CÍMÉT.
2. Nyomja le az hello **F5** toorebuild és futtatási hello alkalmazást. hello app szinkronizál a helyi módosítások hello Azure Mobile Apps-háttéralkalmazás segítségével eseménylekérési és eseményküldési műveletek során hello `OnNavigatedTo` eseménykezelő hajt végre.
3. (Választható) Nézet hello a módosított adatokat az SQL Server Object Explorer vagy egy REST-eszköz, például Fiddler használatával. Értesítés hello adatok között hello Azure Mobile Apps-háttéralkalmazás adatbázis és a helyi tárolójába hello lett szinkronizálva.
4. Hello alkalmazásban kattintva ellenőrizheti az hello jelre néhány elemek toocomplete őket hello helyi tárolóban levő.

   `UpdateCheckedTodoItem`hívások `SyncAsync` hello Mobile Apps-háttéralkalmazás elem toosync minden befejeződött. `SyncAsync`meghívja a lekérést és a küldést. Azonban **amikor csak akkor hajtható végre egy táblázaton lekérési hello ügyfél által végrehajtott módosítások, leküldéses a rendszer mindig futtatja automatikusan**. Ez a viselkedés garantálja, hogy együtt kapcsolatok hello a helyi tárolóban levő összes tábla azonban konzisztens marad. Ez a viselkedés egy váratlan leküldéses eredményezhet.  Ez a viselkedés további információkért lásd: [az Azure Mobile Apps Offline adatszinkronizálás].

## <a name="api-summary"></a>API-összefoglalót
hello használtuk toosupport hello offline funkciók a mobile services, [IMobileServiceSyncTable] felületet, és inicializálva [MobileServiceClient.SyncContext] [ synccontext] a helyi SQLite-adatbázis. Ha offline módban vannak, normál CRUD műveletek hello Mobile Apps dolgozhatnak, mintha hello alkalmazás még mindig kapcsolódik, míg az hello műveletek hello helyi tárolóban történik. a következő módszerek hello használt toosynchronize hello helyi tároló hello server:

* **[PushAsync]**  mivel ez a módszer egy tagja [IMobileServicesSyncContext], minden táblák között módosítások vannak leküldött toohello háttér. Csak helyi módosításokkal rekordok toohello server küldése.
* **[PullAsync]**  lekérési elindult a egy [IMobileServiceSyncTable]. Ha észlelt változásokat hello tábla, egy implicit leküldéses fut toomake meg arról, hogy együtt kapcsolatok hello a helyi tárolóban levő összes tábla azonban konzisztens marad. Hello *pushOtherTables* egy implicit leküldéses tolni e más táblák hello környezetben vannak a paraméterrel állítható be. Hello *lekérdezés* paraméter egy [IMobileServiceTableQuery<T> ] [ IMobileServiceTableQuery] vagy OData lekérdezési karakterlánc toofilter hello adatokat adott vissza. Hello *queryId* paraméter toodefine növekményes szinkronizálás. További információkért lásd: [az Azure Mobile Apps Offline adatszinkronizálás](app-service-mobile-offline-data-sync.md#how-sync-works).
* **[PurgeAsync]**  az alkalmazás kell hívásához a metódus toopurge elavult adatokat a hello helyi tárolóból. Használjon hello *kényszerítése* paramétert, ha még nem szinkronizált módosításokat kell toopurge.

Ezek a fogalmak kapcsolatos további információkért lásd: [az Azure Mobile Apps Offline adatszinkronizálás](app-service-mobile-offline-data-sync.md#how-sync-works).

## <a name="more-info"></a>További információ
hello következő témakörök nyújtanak további információt a Mobile Apps hello kapcsolat nélküli szinkronizálás funkciója:

* [az Azure Mobile Apps Offline adatszinkronizálás]
* [Az Azure Mobile Apps .NET SDK útmutató][8]

<!-- Anchors. -->
[Update hello app toosupport offline features]: #enable-offline-app
[Update hello sync behavior of hello app]: #update-sync
[Update hello app tooreconnect your Mobile Apps backend]: #update-online-app
[Next Steps]:#next-steps

<!-- Images -->
[1]: ./media/app-service-mobile-windows-store-dotnet-get-started-offline-data/app-service-mobile-add-reference-sqlite-dialog.png
[11]: ./media/app-service-mobile-windows-store-dotnet-get-started-offline-data/app-service-mobile-add-wp81-reference-sqlite-dialog.png
[13]: ./media/app-service-mobile-windows-store-dotnet-get-started-offline-data/cpu-architecture.png


<!-- URLs. -->
[az Azure Mobile Apps Offline adatszinkronizálás]: app-service-mobile-offline-data-sync.md
[windows-alkalmazás létrehozása]: app-service-mobile-windows-store-dotnet-get-started.md
[SQLite for Windows 8.1]: http://go.microsoft.com/fwlink/?LinkID=716919
[SQLite for Windows Phone 8.1]: http://go.microsoft.com/fwlink/?LinkID=716920
[SQLite for Windows 10]: http://go.microsoft.com/fwlink/?LinkID=716921
[synccontext]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient.synccontext(v=azure.10).aspx
[sqlite store nuget]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client.SQLiteStore/
[IMobileServiceSyncTable]: https://msdn.microsoft.com/library/azure/mt691742(v=azure.10).aspx
[IMobileServiceTableQuery]: https://msdn.microsoft.com/library/azure/dn250631(v=azure.10).aspx
[IMobileServicesSyncContext]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.imobileservicesynccontext(v=azure.10).aspx
[MobileServicePushFailedException]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.mobileservicepushfailedexception(v=azure.10).aspx
[Status]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.mobileservicepushcompletionresult.status(v=azure.10).aspx
[CancelledByNetworkError]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.mobileservicepushstatus(v=azure.10).aspx
[PullAsync]: https://msdn.microsoft.com/library/azure/mt667558(v=azure.10).aspx
[PushAsync]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileservicesynccontextextensions.pushasync(v=azure.10).aspx
[PurgeAsync]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.imobileservicesynctable.purgeasync(v=azure.10).aspx
[8]: app-service-mobile-dotnet-how-to-use-client-library.md
