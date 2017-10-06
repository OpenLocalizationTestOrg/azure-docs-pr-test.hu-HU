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
# <a name="enable-offline-sync-for-your-android-mobile-app"></a>Androidos mobilalkalmazás kapcsolat nélküli szinkronizálásának engedélyezése
[!INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Áttekintés
Ez az oktatóanyag az Azure Mobile Apps hello kapcsolat nélküli szinkronizálás szolgáltatása Android ismerteti. Kapcsolat nélküli szinkronizálás lehetővé teszi, hogy a befejező felhasználóknak toointeract mobilalkalmazást&mdash;megtekintését, hozzáadását és módosítását adatok&mdash;akkor is, ha nincs hálózati kapcsolat. Változások a helyi adatbázisban tárolódnak. Hello eszköz újra online állapotba kerül, ha ezek a változások szinkronizálása megtörtént-e hello távoli háttér.

Ha ez az Azure Mobile Apps első élményét, először ki hello oktatóanyag [Android-alkalmazás létrehozása]. Ha nem használ hello letöltése – első lépések, hello data access kiegészítő csomagok tooyour projekt hozzá kell adnia. Kiszolgáló bővítménycsomagok kapcsolatos további információkért lásd: [használható hello .NET háttérkiszolgáló SDK az Azure Mobile Apps a](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

További információ az hello kapcsolat nélküli szinkronizálás szolgáltatást, toolearn hello témakörben találhatók [az Azure Mobile Apps Offline adatszinkronizálás].

## <a name="update-hello-app-toosupport-offline-sync"></a>Hello app toosupport kapcsolat nélküli szinkronizálás frissítése
A kapcsolat nélküli szinkronizálás, olvassa el a tooand írási egy *szinkronizálási tábla* (hello segítségével *IMobileServiceSyncTable* felület), amely része egy **SQLite** adatbázis az eszközön.

toopush és lekéréses módosítások hello eszköz és az Azure Mobile Services között, használhatja a *szinkronizálási környezetet* (*MobileServiceClient.SyncContext*), amely a helyi adatbázis hello inicializálása helyben toostore az adatokat.

1. A `TodoActivity.java`, meglévő definíciója hello megjegyzésbe `mToDoTable` hello szinkronizálási tábla verziója kódrészig, és:
   
        private MobileServiceSyncTable<ToDoItem> mToDoTable;
2. A hello `onCreate` módszer, meglévő inicializálása hello megjegyzésbe `mToDoTable` és törölje az ehhez a definícióhoz:
   
        mToDoTable = mClient.getSyncTable("ToDoItem", ToDoItem.class);
3. A `refreshItemsFromTable` hello definíciója megjegyzésbe `results` és törölje az ehhez a definícióhoz:
   
        // Offline Sync
        final List<ToDoItem> results = refreshItemsFromMobileServiceTableSyncTable();
4. Hello definíciója megjegyzésbe `refreshItemsFromMobileServiceTable`.
5. Állítsa vissza a hello definíciója `refreshItemsFromMobileServiceTableSyncTable`:
   
        private List<ToDoItem> refreshItemsFromMobileServiceTableSyncTable() throws ExecutionException, InterruptedException {
            //sync hello data
            sync().get();
            Query query = QueryOperations.field("complete").
                    eq(val(false));
            return mToDoTable.read(query).get();
        }
6. Állítsa vissza a hello definíciója `sync`:
   
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

## <a name="test-hello-app"></a>Teszt hello alkalmazás
Ebben a szakaszban hello működését Wi-Fi tesztelje a, és kapcsolja ki a Wi-Fi toocreate egy kapcsolat nélküli forgatókönyv.

Adatelemek hozzáadásakor tárolják őket hello helyi SQLite tárolóban, de nem szinkronizált toohello mobilszolgáltatás amíg hello nyomja le az ENTER **frissítése** gombra. Más alkalmazások előfordulhat, hogy mikor adatoknak kell szinkronizálva toobe kapcsolatos eltérő követelmények vonatkoznak, de bemutató céljára ebben az oktatóanyagban rendelkezik explicit módon kérheti hello felhasználó.

A gomb megnyomásakor új háttérfeladat kezdődik. Először az összes végrehajtott módosítások toohello helyi állapottárolójához szinkronizálási környezetet, majd Azure toohello helyi tábla összes ponttá megváltozott adatait leküldéses értesítések.

### <a name="offline-testing"></a>Kapcsolat nélküli tesztelése
1. Hely hello eszköz vagy a szimulátor *repülési üzemmód*. Ezzel létrehoz egy kapcsolat nélküli forgatókönyv.
2. Adja hozzá az egyes *ToDo* elemeket, vagy be van jelölve befejezettként egyes elemek. Lépjen a hello eszköz vagy szimulátor (vagy a kényszerített bezárása hello app), és indítsa újra. Győződjön meg arról, hogy a módosítások maradnak hello eszközön tárolják őket mert hello helyi SQLite tárolójában.
3. Hello Azure hello tartalmának megtekintése *TodoItem* például a tábla vagy egy SQL eszközzel *SQL Server Management Studio*, vagy egy REST-ügyfél, például *Fiddler* vagy  *Postman*. Ellenőrizze, hogy rendelkezik-e új elemek hello *nem* lett szinkronizált toohello kiszolgáló
   
       + Node.js-háttéralkalmazáshoz, lépjen a toohello [Azure-portálon](https://portal.azure.com/), és a háttérkiszolgáló kattintson a mobilalkalmazás **könnyen táblák** > **TodoItem** hello tooviewhellotartalmát`TodoItem` tábla.
       + A .NET-háttérrendszer, nézet hello tábla tartalmát vagy egy SQL eszközzel például *SQL Server Management Studio*, vagy egy REST-ügyfél, például *Fiddler* vagy *Postman*.
4. Kapcsolja be a Wi-Fi hello eszköz vagy szimulátor. Nyomja meg az hello **frissítése** gombra.
5. Hello TodoItem adatainak megtekintéséhez újra hello Azure-portálon. hello új és módosított TodoItems ekkor meg kell jelennie.

## <a name="additional-resources"></a>További források
* [az Azure Mobile Apps Offline adatszinkronizálás]
* [Felhő tartalma: Kapcsolat nélküli szinkronizálás az Azure Mobile Services] \(Megjegyzés: a Mobile Services hello video van, de a kapcsolat nélküli szinkronizálás az Azure Mobile Apps hasonló módon működik\)

<!-- URLs. -->

[az Azure Mobile Apps Offline adatszinkronizálás]: app-service-mobile-offline-data-sync.md

[Android-alkalmazás létrehozása]: app-service-mobile-android-get-started.md

[Felhő tartalma: Kapcsolat nélküli szinkronizálás az Azure Mobile Services]: http://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri
[Azure Friday: Offline-enabled apps in Azure Mobile Services]: http://azure.microsoft.com/documentation/videos/azure-mobile-services-offline-enabled-apps-with-donna-malayeri/

