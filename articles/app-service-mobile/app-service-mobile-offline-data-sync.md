---
title: "az Azure Mobile Apps adatszinkronizálás aaaOffline |} Microsoft Docs"
description: "Fogalmi referenciája és hello kapcsolat nélküli szinkronizálás funkció az Azure Mobile Apps áttekintése"
documentationcenter: windows
author: ggailey777
manager: syntaxc4
editor: 
services: app-service\mobile
ms.assetid: 982fb683-8884-40da-96e6-77eeca2500e3
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 10/30/2016
ms.author: glenga
ms.openlocfilehash: 58673240ba433651faf1f619ca5da33dd6459d2b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="offline-data-sync-in-azure-mobile-apps"></a>Kapcsolat nélküli adatszinkronizálás az Azure Mobile Apps megoldásban
## <a name="what-is-offline-data-sync"></a>Mi az az offline adatszinkronizálás?
Offline adatszinkronizálás egy ügyfél és kiszolgáló SDK szolgáltatás az Azure Mobile Apps, amely megkönnyíti a fejlesztők toocreate alkalmazásokat, amelyek funkcionális hálózati kapcsolat nélkül.

Ha az alkalmazás offline módban van, is létrehozhatja és módosíthatják az adatokat, amelyek menti a helyi tárolójába tooa. Hello app újra online állapotba kerül, ha azt képes szinkronizálni az Azure Mobile Apps-háttéralkalmazás helyi módosításokkal. hello szolgáltatást is támogatja az ütközések észlelése ugyanazt a rekordot is történtek hello ügyfél hello és háttér hello. Ütközések lehet kezelni, hello kiszolgáló vagy hello ügyfél.

Kapcsolat nélküli szinkronizálás rendelkezik számos előnnyel jár:

* Alkalmazások válaszképességét továbbfejlesztésében helyileg hello eszközökön server adatok gyorsítótárazása
* Továbbra is, ha hálózati problémák hasznos robusztus alkalmazások létrehozása
* Lehetővé teszi a végfelhasználók felhasználók toocreate és módosíthatják az adatokat, akkor is, amikor nincs hálózati hozzáférés, összefüggő kevéssé vagy egyáltalán ne forgatókönyvek támogatása
* Szinkronizálja az adatokat több eszközön, és ütközések észlelés, ha hello azonos rekord módosul a két eszköz
* A nagy késleltetésű vagy forgalmi díjas hálózatokon hálózati használatának korlátozása

a következő oktatóanyagok hello megjelenítése, hogyan tooadd kapcsolat nélküli szinkronizálás tooyour mobil ügyfelek Azure Mobile Apps használata:

* [Android: Kapcsolat nélküli szinkronizálásának engedélyezése]
* [Apache Cordova: Kapcsolat nélküli szinkronizálásának engedélyezése](app-service-mobile-cordova-get-started-offline-data.md)
* [iOS: kapcsolat nélküli szinkronizálásának engedélyezése]
* [Xamarin iOS: kapcsolat nélküli szinkronizálásának engedélyezése]
* [Xamarin Android: Kapcsolat nélküli szinkronizálásának engedélyezése]
* [Xamarin.Forms: Engedélyezése kapcsolat nélküli szinkronizálás](app-service-mobile-xamarin-forms-get-started-offline-data.md)
* [univerzális Windows Platform: kapcsolat nélküli szinkronizálásának engedélyezése]

## <a name="what-is-a-sync-table"></a>Mi az a egy szinkronizálás tábla?
tooaccess hello "/ táblák" végpont, hello Azure Mobile ügyfél SDK-k biztosítanak a felületek, mint `IMobileServiceTable` (.NET SDK-ügyfél) vagy `MSTable` (iOS-ügyfél). Ezen API-k csatlakozzon közvetlenül toohello Azure Mobile Apps-háttéralkalmazás, és sikertelen lesz, ha hello ügyféleszköz nincs hálózati kapcsolat.

toosupport kapcsolat nélküli használatra, az alkalmazás inkább használjon hello *szinkronizálási tábla* API-k, például a `IMobileServiceSyncTable` (.NET SDK-ügyfél) vagy `MSSyncTable` (iOS-ügyfél). Szinkronizálási dolgozhat ugyanazon CRUD műveleteihez (létrehozási, olvasási, frissítési, törlési) összes hello tábla API-k, kivéve most azok olvasni vagy tooa írási *helyi tároló*. A szinkronizálási tábla műveletek elvégzése előtt hello helyi tároló inicializálni kell.

## <a name="what-is-a-local-store"></a>Mi az a helyi tárolót?
A helyi tárolójába hello adatmegőrző réteget hello ügyféleszközön. hello Azure Mobile Apps-ügyfél SDK-k, adjon meg egy helyi alapértelmezett megvalósítási tárolja. A Windows, a Xamarin és az Android SQLite alapul. IOS az alapvető adatokon alapul.

toouse hello SQLite-alapú megvalósítás a Windows Phone vagy Windows áruház 8.1-es, meg kell tooinstall egy SQLite-bővítmény. További információkért lásd: [univerzális Windows Platform: kapcsolat nélküli szinkronizálásának engedélyezése]. Android és iOS küldje el az operációs rendszer, így nem szükséges tooreference saját SQLite verziója hello eszköz SQLite verziójával.

A fejlesztők is megvalósíthatja a saját helyi tárolóból. Például ha toostore adatok titkosított formában hello mobil ügyfélen, megadhatja a titkosításhoz SQLCipher használó helyi tároló.

## <a name="what-is-a-sync-context"></a>Mi az a szinkronizálási környezetet?
A *szinkronizálási környezetet* egy mobil ügyfél objektumhoz társított (például `IMobileServiceClient` vagy `MSClient`) és szinkronizáló táblákkal végzett módosításokat követi nyomon. hello szinkronizálási környezetet tart fenn egy *művelet várólista*, amely tartja a rendezett listáját CUD műveleteket (létrehozás, frissítés, Törlés), amely későbbi küldésének toohello kiszolgáló.

Hello szinkronizálási környezetet használja, mint egy inicializálási metódusa társítva a helyi tárolójába `IMobileServicesSyncContext.InitializeAsync(localstore)` a hello [.NET ügyfél SDK].

## <a name="how-sync-works"></a>Hogyan kapcsolat nélküli szinkronizálás használata
Szinkronizálási táblák használata esetén az Ügyfélkód szabályozza, amikor változtatásokat szinkronizálva van-e az Azure Mobile Apps-háttéralkalmazás. Semmi küldött toohello háttér addig, amíg nincs egy hívás túl*leküldéses* helyi módosításokkal. Ehhez hasonlóan hello helyi tároló fel van töltve az új adatokat csak akkor hívása túl*lekéréses* adatokat.

* **Leküldéses**: leküldéses hello szinkronizálási környezeten művelet, és minden CUD módosításokat küldi hello utolsó leküldéses óta. Vegye figyelembe, hogy az informatikai van a csak egy egyedi tábla módosítása nem lehetséges toosend, mert ellenkező esetben műveletek sikerült elküldeni nem megfelelő sorrendben. Leküldéses végrehajtja a többi hívások tooyour Azure Mobile Apps-háttéralkalmazás, amely pedig módosítja a server-adatbázis sorozata.
* **Lekéréses**: lekéréses tábla alapon történik, és a lekérdezés tooretrieve testre hello kiszolgálói adatok csak egy részét. hello Azure Mobile ügyfél SDK-k, majd szúrja be a kapott adatokban hello hello helyi tárolóhoz.
* **Implicit leküldéses értesítések**: lekérési egy táblázaton, amelyeknek helyi frissítések végrehajtása, ha hello lekéréses először hajt végre egy `push()` hello szinkronizálási környezetében. A leküldéses csökkentheti módosításokat, amelyek már sorban áll, hello kiszolgálóról közötti ütközések.
* **A növekményes szinkronizálás**: hello első paraméter toohello lekéréses művelet egy *lekérdezésnév* , amelyek az ügyfélszámítógépeken csak hello. Egy null értékű lekérdezésnév használatakor hello Azure Mobile SDK hajt végre egy *növekményes szinkronizálás*. Minden alkalommal, amikor egy lekéréses művelet adja vissza, amely eredmény elérése érdekében hello legújabb `updatedAt` adott eredményhalmazából időbélyeg hello SDK helyi rendszertáblák van tárolva. További lekéréses műveletek után az időbélyeg csak rekordok beolvasása.

  a növekményes szinkronizálás toouse, a kiszolgáló kell visszaadnia jelentéssel bíró `updatedAt` értéket, majd is támogatnia kell ezt a mezőt szerint rendezve. Azonban hello SDK saját rendezési hello updatedAt mező ad hozzá, mivel nem használhat saját lekéréses lekérdezés `orderBy` záradékban.

  hello lekérdezésnév választja karakterlánc lehet, de az alkalmazás minden logikai lekérdezés egyedinek kell lennie.
  Ellenkező esetben a különböző lekéréses műveletek felülírhatja hello ugyanazt a növekményes szinkronizálás időbélyeg és a lekérdezések helytelen eredményeket adhat vissza.

  Hello lekérdezési paraméter tartozik, ha egyirányú toocreate egy egyedi lekérdezés neve tooincorporate hello paraméter értékét.
  Például ha szűrt felhasználói azonosítóját, a lekérdezés neve lehet, az alábbiak szerint (a C#):

        await todoTable.PullAsync("todoItems" + userid,
            syncTable.Where(u => u.UserId == userid));

  Ha azt szeretné, hogy a növekményes szinkronizálás kívül tooopt, `null` , hello lekérdezés azonosítóját. Ebben az esetben minden rekordot a rendszer beolvassa a minden hívás túl`PullAsync`, amely nem potenciálisan hatékony.
* **Kiürítése**: hello tartalmát hello helyi tároló használatával törölheti `IMobileServiceSyncTable.PurgeAsync`.
  Kiürítése akkor lehet szükség, ha elavult adatokat hello ügyfél adatbázisban, vagy ha toodiscard a függőben lévő módosítások.

  A kiürítési törli a helyi tárolóból hello egy tábla. Ha nincsenek műveletek várnak szinkronizálási hello kiszolgáló adatbázis-kezelő hello kiürítése jelez kivétel, kivéve, ha hello *kiürítése kényszerítése* paraméter értéke.

  Az elavult adatok hello ügyfélen például tegyük fel, hogy hello "teendőlista" példában Device1 csak kéri le. elemek nem fejeződtek be. A todoitem "Megvásárlása tej" jelölésű hello kiszolgálón egy másik eszköz befejeződött. Azonban Device1 még hello "Megvásárlása tej" todoitem helyi tárolóban levő, mert csak akkor van húzza elemek, amelyek nincsenek megjelölt befejeződött. A kiürítési törli az elavult elem.

## <a name="next-steps"></a>Következő lépések
* [iOS: kapcsolat nélküli szinkronizálásának engedélyezése]
* [Xamarin iOS: kapcsolat nélküli szinkronizálásának engedélyezése]
* [Xamarin Android: Kapcsolat nélküli szinkronizálásának engedélyezése]
* [univerzális Windows Platform: kapcsolat nélküli szinkronizálásának engedélyezése]

<!-- Links -->
[.NET ügyfél SDK]: app-service-mobile-dotnet-how-to-use-client-library.md
[Android: Kapcsolat nélküli szinkronizálásának engedélyezése]: app-service-mobile-android-get-started-offline-data.md
[iOS: kapcsolat nélküli szinkronizálásának engedélyezése]: app-service-mobile-ios-get-started-offline-data.md
[Xamarin iOS: kapcsolat nélküli szinkronizálásának engedélyezése]: app-service-mobile-xamarin-ios-get-started-offline-data.md
[Xamarin Android: Kapcsolat nélküli szinkronizálásának engedélyezése]: app-service-mobile-xamarin-android-get-started-offline-data.md
[univerzális Windows Platform: kapcsolat nélküli szinkronizálásának engedélyezése]: app-service-mobile-windows-store-dotnet-get-started-offline-data.md
