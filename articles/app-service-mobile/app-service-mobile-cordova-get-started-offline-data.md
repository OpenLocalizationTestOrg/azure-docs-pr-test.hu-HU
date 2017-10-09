---
title: "az Azure Mobile Apps (Cordova) kapcsolat nélküli szinkronizálásának aaaEnable |} Microsoft Docs"
description: "Megtudhatja, hogyan toouse App Service Mobile Apps toocache és a szinkronizálási kapcsolat nélküli adatok a Cordova-alkalmazás"
documentationcenter: cordova
author: ggailey777
manager: syntaxc4
editor: 
services: app-service\mobile
ms.assetid: 1a3f685d-f79d-4f8b-ae11-ff96e79e9de9
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-cordova-ios
ms.devlang: dotnet
ms.topic: article
ms.date: 10/30/2016
ms.author: glenga
ms.openlocfilehash: 4e6ae96c3d96dac8ebb3749354b83a04686831b7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="enable-offline-sync-for-your-cordova-mobile-app"></a>A mobil Cordova-alkalmazás kapcsolat nélküli szinkronizálásának engedélyezése
[!INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

Ez az oktatóanyag az Azure Mobile Apps hello kapcsolat nélküli szinkronizálás szolgáltatása a Cordova vezet be. Kapcsolat nélküli szinkronizálás lehetővé teszi, hogy a befejező felhasználóknak toointeract mobilalkalmazást&mdash;megtekintését, hozzáadását és módosítását adatok&mdash;akkor is, ha nincs hálózati kapcsolat. Változások a helyi adatbázisban tárolódnak.  Hello eszköz újra online állapotba kerül, ha ezek a változások szinkronizálása megtörtént-e távoli hello szolgáltatást.

Ez az oktatóanyag alapul hello Cordova gyors üzembe helyezés megoldás a Mobile Apps befejezésekor létrehozandó hello oktatóanyag [Apache Cordova gyors üzembe helyezési]. Ebben az oktatóanyagban hello gyors üzembe helyezés megoldás tooadd offline funkciók az Azure Mobile Apps frissíti.  A Microsoft hello offline tartozó kódot hello alkalmazásban is jelöljön ki.

További információ az hello kapcsolat nélküli szinkronizálás szolgáltatást, toolearn hello témakörben találhatók [az Azure Mobile Apps Offline adatszinkronizálás]. API-használati részletekért lásd: hello [API-JÁNAK dokumentációja](https://azure.github.io/azure-mobile-apps-js-client).

## <a name="add-offline-sync-toohello-quickstart-solution"></a>Kapcsolat nélküli szinkronizálás toohello gyors üzembe helyezés megoldás hozzáadása
hello kapcsolat nélküli szinkronizálás kód toohello alkalmazás hozzá kell adni. Kapcsolat nélküli szinkronizálás szükséges hello cordova-sqlite-tároló beépülő modul, amely automatikusan bekerül tooyour app amikor hello Azure Mobile Apps beépülő modul hello projekt tartalmazza. hello Gyorsútmutató-projekt tartalmazza a beépülő modulok mindegyikét.

1. A Visual Studio Solution Explorerben nyissa meg a index.js, és cserélje le a következő kód hello

        var client,            // Connection toohello Azure Mobile App backend
           todoItemTable;      // Reference tooa table endpoint on backend

    Ezzel a kóddal:

        var client,            // Connection toohello Azure Mobile App backend
           todoItemTable,      // Reference tooa table endpoint on backend
           syncContext;        // Reference toooffline data sync context

2. Ezután cserélje le a következő kód hello:

        client = new WindowsAzure.MobileServiceClient('http://yourmobileapp.azurewebsites.net');

    Ezzel a kóddal:

        client = new WindowsAzure.MobileServiceClient('http://yourmobileapp.azurewebsites.net');
        var store = new WindowsAzure.MobileServiceSqliteStore('store.db');

        store.defineTable({
          name: 'todoitem',
          columnDefinitions: {
              id: 'string',
              text: 'string',
              complete: 'boolean',
              version: 'string'
          }
        });

        // Get hello sync context from hello client
        syncContext = client.getSyncContext();

    hello előző kód kiegészítéseket hello helyi tároló inicializálása és adjon meg egy helyi táblát, amely megfelel a hello oszlop értékeit használja az Azure-ban háttér. (Nincs szükség tooinclude összes oszlop értékének ezt a kódot.)  Hello `version` mező hello mobil-háttéralkalmazást tartja fenn, és ütközésfeloldás közben használható.

    Egy hivatkozási toohello szinkronizálási környezetet meghívásával kap **getSyncContext**. hello szinkronizálási környezetet segíti követésével és kérdez le a módosításokat minden olyan táblát, egy ügyfélalkalmazás módosította, ha a tábla közötti megőrzése érdekében `.push()` nevezik.

3. Frissítés hello alkalmazás URL-cím tooyour mobilalkalmazás alkalmazás URL-címe.

4. Ezután cserélje le ezt a kódot:

        todoItemTable = client.getTable('todoitem'); // todoitem is hello table name

    Ezzel a kóddal:

        // Initialize hello sync context with hello store
        syncContext.initialize(store).then(function () {

        // Get hello local table reference.
        todoItemTable = client.getSyncTable('todoitem');

        syncContext.pushHandler = {
            onConflict: function (pushError) {
                // Handle hello conflict.
                console.log("Sync conflict! " + pushError.getError().message);
                // Update failed, revert tooserver's copy.
                pushError.cancelAndDiscard();
              },
              onError: function (pushError) {
                  // Handle hello error
                  // In hello simulated offline state, you get "Sync error! Unexpected connection failure."
                  console.log("Sync error! " + pushError.getError().message);
              }
        };

        // Call a function tooperform hello actual sync
        syncBackend();

        // Refresh hello todoItems
        refreshDisplay();

        // Wire up hello UI Event Handler for hello Add Item
        $('#add-item').submit(addItemHandler);
        $('#refresh').on('click', refreshDisplay);

    hello előző kód inicializálja hello szinkronizálási környezetet és az majd (helyett getTable) getSyncTable tooget hivatkozási toohello helyi táblában.

    A kód által használt hello helyi adatbázis összes létrehozása, olvasása, frissítése és Törlés (CRUD) tábla műveletek.

    Ez a minta egyszerű hibakezelési szinkronizálási ütközések hajt végre. Egy valós alkalmazás kezelni kellene hello hálózati feltételek mellett, server ütközések és mások számára például különböző hibákat. Kód, tekintse meg a hello [kapcsolat nélküli szinkronizálás minta].

5. Ezután adja hozzá a függvény tooperform hello tényleges szinkronizálás.

        function syncBackend() {

          // Sync local store tooAzure table when app loads, or when login complete.
          syncContext.push().then(function () {
              // Push completed

          });

          // Pull items from hello Azure table after syncing tooAzure.
          syncContext.pull(new WindowsAzure.Query('todoitem'));
        }

    Úgy dönt, amikor toopush változik meghívásával toohello Mobile Apps-háttéralkalmazás **syncContext.push()**. Például hívhatja **syncBackend** a gomb esemény kezelő kapcsolt tooa szinkronizálás gomb.

## <a name="offline-sync-considerations"></a>Kapcsolat nélküli szinkronizálás kapcsolatos szempontok

Hello mintában hello **leküldéses** metódusában **syncContext** alkalmazás következő indításakor hello visszahívási függvény bejelentkezési azonosító csak nevezik.  Egy valós alkalmazás is teheti a szinkronizálási funkció manuálisan elindítva, vagy ha hello hálózati állapota megváltozik..

Elleni táblának függőben lévő lekérési végrehajtásakor helyi frissítések követi nyomon, hogy a pull művelet automatikusan eseményindítók hello összefüggésben leküldéses. Ha frissíteni, hozzáadása, és ez a példa elemek befejezése, akkor kihagyhatja hello explicit **leküldéses** hívható, mert elképzelhető, hogy a redundáns.

A megadott hello kódban, a rendszer megkérdezi a hello távoli todoItem tábla összes adatát, de azt is lehetséges toofilter rekordok úgy, hogy a lekérdezés azonosítóját és a lekérdezés túl**leküldéses**. További információkért lásd: hello szakasz *növekményes szinkronizálás* a [az Azure Mobile Apps Offline adatszinkronizálás].

## <a name="optional-disable-authentication"></a>(Választható) Hitelesítés letiltása

Ha nem szeretné, hogy kapcsolat nélküli szinkronizálás tesztelése előtt hitelesítés tooset, megjegyzéssé hello visszahívási függvény a bejelentkezéshez, de hello kódot hagyja uncommented hello visszahívási függvény.  Megjegyzések hello bejelentkezési sorok el, miután hello kódot a következőképpen:

      // Login toohello service.
      // client.login('twitter')
      //    .then(function () {
        syncContext.initialize(store).then(function () {
          // Leave hello rest of hello code in this callback function  uncommented.
                ...
        });
      // }, handleError);

Most hello app szinkronizál hello Azure-háttérrendszernek hello alkalmazás futtatásakor.

## <a name="run-hello-client-app"></a>Hello ügyfél-alkalmazás futtatása
A kapcsolat nélküli szinkronizálás most engedélyezve van futtathatja hello ügyfélalkalmazás legalább egyszer hello helyi tároló adatbázisának feltöltése minden platformon. Később egy kapcsolat nélküli forgatókönyv szimulálása és hello adatok hello helyi tárolóban levő módosítható, amíg az alkalmazás hello offline állapotban.

## <a name="optional-test-hello-sync-behavior"></a>(Választható) Teszt hello szinkronizálási viselkedése
Ebben a szakaszban hello ügyfél projekt toosimulate egy kapcsolat nélküli forgatókönyv egy érvénytelen az alkalmazás URL-CÍMÉT a háttérkiszolgáló használatával módosíthatja. Hozzáadásakor, vagy módosítsa az elemeket, ezeket a módosításokat a helyi tárolóban levő tartanak, de nem szinkronizált toohello háttér adattár amíg hello kapcsolat helyreáll.

1. A Solution Explorer hello nyissa meg hello index.js projekt fájlt, és módosítsa az URL-cím érvénytelen, például a következő kód hello hello alkalmazás URL-cím toopoint:

        client = new WindowsAzure.MobileServiceClient('http://yourmobileapp.azurewebsites.net-fail');

2. A index.html, frissítse a hello CSP `<meta>` elem hello azonos URL-cím érvénytelen.

        <meta http-equiv="Content-Security-Policy" content="default-src 'self' data: gap: http://yourmobileapp.azurewebsites.net-fail; style-src 'self'; media-src *">

3. Hozza létre és hello ügyfélalkalmazás futtatása, és figyelje meg, hogy kivételt hello konzol kerül, amikor hello app kísérli meg a bejelentkezés után hello háttér szinkronizálás. Hozzáadhat új elemek található csak hello helyi tárolóban csak akkor vannak leküldött toohello mobil-háttéralkalmazást. hello ügyfélalkalmazás úgy viselkedik, mintha a csatlakoztatott toohello háttér.

4. Hello-alkalmazások bezárása, és indítsa újra azt, hogy hello létrehozott új elemek-e a megőrzött toohello helyi tároló tooverify.

5. (Választható) Visual Studio tooview használja az Azure SQL Database tábla toosee hello adatok hello háttérbeli adatbázis nem módosított.

    A Visual Studióban nyissa meg a **Server Explorer**. Keresse meg a tooyour adatbázis **Azure**->**SQL-adatbázisok**. Kattintson a jobb gombbal az adatbázist, és válassza ki **nyissa meg az SQL Server Object Explorerben**. Most már megkeresheti tooyour SQL-adatbázistáblában szereplő és annak tartalmát.

## <a name="optional-test-hello-reconnection-tooyour-mobile-backend"></a>(Választható) Hello újracsatlakozás tooyour mobil-háttéralkalmazást tesztelése

Ebben a szakaszban újra hello toohello mobil háttéralkalmazás, szimulálja hello alkalmazás hamarosan ismét online állapotba. Jelentkezzen be, amikor adatokat szinkronizált tooyour mobil-háttéralkalmazást.

1. Nyissa meg újra a index.js, és állítsa vissza a hello alkalmazás URL-CÍMÉT.
2. Nyissa meg újra a index.html, és kijavíthatja az hello alkalmazás URL-címet hello CSP `<meta>` elemet.
3. Építse újra és hello ügyfélalkalmazás futtatása. hello app bejelentkezés után próbálja meg a mobil-háttéralkalmazás hello toosync. Győződjön meg arról, hogy nem tartalmaznak kivételeket hello hibakereső konzol vannak bejelentkezve.
4. (Választható) Nézet hello a módosított adatokat az SQL Server Object Explorer vagy egy REST-eszköz, például Fiddler használatával. Értesítés hello adatok hello háttér adatbázis és a helyi tárolójába hello lett szinkronizálva.

    Figyelje meg hello adatok hello adatbázis és a helyi tárolójába hello lett szinkronizálva, és az alkalmazás forráshoz hozzáadott hello elemet tartalmaz.

## <a name="additional-resources"></a>További források
* [az Azure Mobile Apps Offline adatszinkronizálás]
* [Visual Studio Tools for Apache Cordova]

## <a name="next-steps"></a>Következő lépések
* Tekintse át a fejlettebb, kapcsolat nélküli szinkronizálás szolgáltatások, például a hello ütközésének feloldása [kapcsolat nélküli szinkronizálás minta]
* Tekintse át a hello kapcsolat nélküli szinkronizálás hello API-hivatkozás [API-JÁNAK dokumentációja](https://azure.github.io/azure-mobile-apps-js-client).

<!-- ##Summary -->

<!-- Images -->

<!-- URLs. -->
[Apache Cordova gyors üzembe helyezési]: app-service-mobile-cordova-get-started.md
[kapcsolat nélküli szinkronizálás minta]: https://github.com/Azure-Samples/app-service-mobile-cordova-client-conflict-handling
[az Azure Mobile Apps Offline adatszinkronizálás]: app-service-mobile-offline-data-sync.md
[Cloud Cover: Offline Sync in Azure Mobile Services]: http://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri
[Adding Authentication]: app-service-mobile-cordova-get-started-users.md
[authentication]: app-service-mobile-cordova-get-started-users.md
[Work with hello .NET backend server SDK for Azure Mobile Apps]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Visual Studio Community 2015]: http://www.visualstudio.com/
[Visual Studio Tools for Apache Cordova]: https://www.visualstudio.com/en-us/features/cordova-vs.aspx
[Apache Cordova SDK]: app-service-mobile-cordova-how-to-use-client-library.md
[ASP.NET Server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Node.js Server SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md
