---
title: a Notification Hubs aaaAzure
description: "Ismerje meg, hogyan tooadd leküldéses értesítési képességek az Azure Notification hubs használatával."
author: ysxu
manager: erikre
editor: 
services: notification-hubs
documentationcenter: 
ms.assetid: fcfb0ce8-0e19-4fa8-b777-6b9f9cdda178
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: multiple
ms.devlang: multiple
ms.topic: article
ms.date: 1/17/2017
ms.author: yuaxu
ms.openlocfilehash: 78ce34b6b094b560c8002ab9652f7ba4563c5c74
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-notification-hubs"></a>Azure Notification Hubs
## <a name="overview"></a>Áttekintés
Az Azure Notification Hubs egy egyszerűen használható, többplatformos, kibővített leküldéses alrendszeren adja meg. Egyetlen platformfüggetlen API-hívással használatával egyszerűen küldhet megcélzott és a testreszabott leküldéses értesítések tooany mobilplatformot bármilyen felhőalapú vagy helyszíni háttérrendszerből.

A Notification Hubs esetében működik jól mind vállalati és végfelhasználói célokra. Íme néhány példa az ügyfelek a Notification Hubs használata:

* Kis késleltetésű breaking news értesítések toomillions küldenek.
* Helyalapú kuponok küldése felhasználói szegmenseknek toointerested.
* Esemény kapcsolatos értesítéseket toousers vagy csoportok media/sport/pénzügyi/játékalkalmazások küldése.
* Ez a promóciós leküldéses tooapps tooengage és a piaci toocustomers tartalmát.
* Felhasználók értesítése olyan vállalati eseményekről, mint az új üzeneteket, és a munkaelemek.
* Kódokat a multi-factor authentication küldeni.

## <a name="what-are-push-notifications"></a>Mik azok a leküldéses értesítések?
Leküldéses értesítések formája, amely egy alkalmazás-felhasználói kommunikációs ahol értesítse a felhasználókat a mobilalkalmazások egyes kívánt információk, általában a előugró ablak vagy párbeszédpanel. Felhasználók általában tooview választhat, vagy hagyja figyelmen kívül üdvözlőüzenetére, és hello volt kiválasztásával megnyílik, amely kellett közölt hello értesítési hello mobilalkalmazás.

Leküldéses értesítések létfontosságú a fogyasztói alkalmazásokba az alkalmazással kapcsolatos marketingtevékenységeket és a használat növelése, valamint a vállalati alkalmazások naprakész üzleti adatok kommunikáció során. Azt hello ajánlott alkalmazás-felhasználói kommunikációs, mert energia-hatékonyabbá teszi a mobileszközök kezeléséhez a hello értesítések feladók rugalmas, és elérhető, amíg a megfelelő alkalmazások nem aktívak.

További információ a leküldéses értesítések néhány népszerű platformokhoz:
* [iOS](https://developer.apple.com/notifications/)
* [Android](https://developer.android.com/guide/topics/ui/notifiers/notifications.html)
* [Windows](http://msdn.microsoft.com/library/windows/apps/hh779725.aspx)

## <a name="how-push-notifications-work"></a>A leküldéses értesítések működése
Leküldéses értesítések érkeznek platform-specifikus nevű infrastruktúrákon keresztül *Platform Notification System* (PNSes). Barebone leküldéses funkciók toodelivery üzenet tooa eszköz egy megadott kezeli, és nincs közös felületük kínálnak. értesítés toosend tooall ügyfelek hello iOS, Android és Windows alkalmazás verzióiban hello fejlesztői együttműködve APNS (Apple Push Notification szolgáltatás), a FCM (Firebase Cloud Messaging) és a WNS (a Windows értesítési szolgáltatás), közben kötegelés hello küld.

Magas szinten itt van leküldéses működése:

1. hello ügyfélalkalmazás úgy dönt, hogy szeretnének tooreceive leküldéses értesítések ezért PNS tooretrieve megfelelő ügyfelek hello egyedi és ideiglenes leküldéses leíróját. hello leíró típusa (pl. WNS rendelkezik URI-azonosítók míg APNS jogkivonatok) hello rendszertől függ.
2. hello ügyfélalkalmazás tárolja Ez a leíró hello app háttér- vagy a szolgáltató.
3. leküldéses értesítés toosend, hello app kapcsolatba lép hello PNS hello leíró tootarget alkalmazással egy adott ügyfél.
4. hello PNS hello leíró által megadott hello toohello eszközt továbbítja.

![][0]

## <a name="hello-challenges-of-push-notifications"></a>hello a leküldéses értesítések kihívásai
Amíg PNSes nem hatékony, mennyi munkaelem toohello alkalmazás fejlesztőjének rendelés tooimplement még gyakori leküldéses értesítési forgatókönyvek, például a szórásos küldés leküldéses értesítések toosegmented felhasználók vagy azok hagyja.

Leküldéses egyike hello leginkább kért mobil felhőszolgáltatásokat, a szolgáltatások, mert a munkát igényel összetett infrastruktúra, amelyek nem kapcsolódó toohello alkalmazás fő üzleti logikájához. Hello infrastrukturális kihívást vannak:

* **Platformfüggőség**: 

  * hello háttér PNSes vannak nem egységes módon meg kell toohave összetett és rögzített karbantartása platformfüggő logika toosend értesítések toodevices a különböző platformokon.
* **Skála**:

  * PNS-irányelvek eszközök tokenjeit minden alkalmazás indítás után frissíteni kell. Ez azt jelenti, hogy hello háttér foglalkozó forgalom nagy mennyiségű, és az adatbázis hozzáférési csak tookeep hello tokenek naprakészen. Eszközök hello száma nő, toohundreds és több millió ezer, létrehozása és fenntartása az infrastruktúra hello költségét esetén nagy.
  * A legtöbb PNSes szórási toomultiple eszközök nem támogatják. Ez azt jelenti, hogy egy egyszerű szórásos tooa egy millió hívások toohello PNSes millió eszközök eredményez. A forgalom mennyisége méretezést, minimális késés nem magától értetődő.
* **Útválasztás**:
  
  * PNSes adjon meg egy módon toosend üzenetek toodevices, bár a legtöbb alkalmazások értesítéseket felhasználóknak vagy érdeklődési köröknek célzott. Ez azt jelenti, hogy hello háttér kell fenntartani egy beállításjegyzék-tooassociate eszközök érdeklődési körök alapján, a felhasználók, a tulajdonságok, stb. Ez a terhelés hozzáadódik toohello idő toomarket és fenntartási költségeihez egy alkalmazást.

## <a name="why-use-notification-hubs"></a>Miért érdemes a Notification Hubsot használni?
A Notification Hubs megszünteti az összes olyan összetett szolgáltatásokkal, társított engedélyezése leküldéses saját. A többplatformos, kibővített leküldéses értesítési infrastruktúrát leküldéses kapcsolatos kódok csökkenti, és egyszerűbbé teszi a kiszolgáló. A Notification hubs használatával eszközök felelősek csupán a PNS-leírók regisztrálásáért regisztrálása egy központi, amíg hello háttér küld üzenetek toousers vagy érdeklődési köröknek, ahogy az ábra a következő hello:

![][1]

A Notification hubs a következő előnyök hello használatra kész leküldéses motor:

* **Kereszt-platformok**

  * Minden, iOS, Android, Windows, és Kindle és Baidu fő leküldéses platformokat is támogat.
  * Egy közös felület toopush tooall platformok nincs platform-specifikus feladata a platform-specifikus vagy platformfüggetlen formátumban.
  * Eszköz az egyik helyen kezelheti.
* **Kereszt-háttérkiszolgálókon**
  
  * Felhőalapú vagy helyszíni
  * .NET, Node.js, Java, stb.
* **Sokféle kézbesítési minta**:

  * *Szórási tooone vagy több platform*: azonnal elküldheti toomillions eszközök különböző platformokon egyetlen API-hívással.
  * *Leküldéses toodevice*: értesítések tooindividual eszközök célba.
  * *Leküldéses toouser*: címkék és a sablonok funkció segítségével egy felhasználó összes platformfüggetlen-eszközök eléréséhez.
  * *Dinamikus címkékkel toosegment leküldéses*: címkék szolgáltatás segítségével szegmenseket, eszközök és leküldéses toothem tooyour igényeinek megfelelően, hogy küldendő tooone szegmens vagy szegmensek (pl. active és életét Budapest nem új felhasználók) kifejezés. Korlátozott toopub-sub nem frissítheti az eszközt címkék bárhonnan és bármikor.
  * *Honosított leküldéses*: funkciójával sablonok honosítási elérése a háttéralkalmazás kódjának módosítása nélkül.
  * *Csendes leküldéses*: hello leküldéses mintát lehetővé is csendes értesítések toodevices küldésével, és indítására, toocomplete bizonyos ponttá vagy a műveletek.
  * *Ütemezett leküldéses*: értesítés toosend bármikor is ütemezheti.
  * *Leküldéses közvetlen*: a szolgáltatás, és közvetlenül kötegelt leküldéses tooa listája eszköz leírók regisztrálásakor eszközök kihagyhatja.
  * *Személyre szabott leküldéses*: eszköz leküldéses változók segítségével küld az eszközre vonatkozó testreszabott leküldéses értesítések az egyéni kulcs-érték párokat.
* **Részletes telemetria**
  
  * Általános leküldéses, eszköz, hiba és művelet telemetriai érhető el hello Azure-portálon és szoftveresen.
  * Egy üzenet Telemetriai nyomon követi az eredeti kérelem hívás tooour szolgáltatás sikeresen kötegelés hello minden leküldéses leküldéses értesítések.
  * Platform Notification System visszajelzés platformmegbízhatósági értesítési rendszere tooassist megoldani az összes visszajelzései kommunikál.
* **Méretezhetőség** 
  
  * Gyors üzenetek toomillions nélkül újratervezni vagy az eszköz horizontális eszközök küldése.
* **Biztonság**

  * Közös hozzáférésű Jogosultságkód (SAS) vagy összevont hitelesítés.

## <a name="integration-with-app-service-mobile-apps"></a>App Service Mobile Apps-integráció
toofacilitate Egységesítő és zökkenőmentes felhasználói élmény különböző Azure, [App Service Mobile Apps] a Notification Hubs használata leküldéses értesítések beépített támogatását. [App Service Mobile Apps] vállalati fejlesztők és rendszerintegrátorok, amely csökkenti a lehetőségek széles skáláját toomobile fejlesztők egy jól skálázható, világszerte elérhető mobilalkalmazás-fejlesztő platform kínál.

Mobile Apps-fejlesztők használhatják a Notification Hubs a következő munkafolyamat hello:

1. Eszköz PNS-leírójának lekérése
2. Eszköz regisztrálása a Notification Hubs kényelmes Mobile Apps-ügyfél SDK regisztrációs API-n keresztül
   * Vegye figyelembe, hogy a Mobile Apps biztonsági okokból eltávolítja az összes regisztrációs címkét. Használhatja a Notification Hubst a háttérrendszerből közvetlen tooassociate címkéket eszközökkel.
3. Értesítések küldése az alkalmazás háttérrendszeréből a Notification Hubs használatával

Az alábbiakban néhány kényelmi szolgáltatásokat, ez az integráció toodevelopers állapotba:

* **Mobile Apps ügyfél SDK-k**: A többplatformos SDK-k egyszerű API-k biztosítanak a regisztrációhoz, és toohello kapcsolódó értesítési központtal való hello mobilalkalmazás automatikusan kommunikál. A fejlesztők nem kell a Notification Hubs hitelesítő toodig és további szolgáltatás használata.

  * *Leküldéses toouser*: hello SDK-k automatikusan felcímkézik az adott eszköz és Mobile Apps hitelesített felhasználói azonosító tooenable leküldéses toouser forgatókönyv hello.
  * *Leküldéses toodevice*: hello SDK-k hello Mobile Apps telepítési Azonosítót automatikusan GUID tooregister a Notification hubs használatával, így a fejlesztőknek hello nem kell több szolgáltatásbeli GUID azonosítót fenntartaniuk használni.
* **Telepítési modell**: Mobile Apps a Notification Hubs legújabb leküldési modell toorepresent összes leküldéses tulajdonságait egy JSON-telepítésben, amely igazodik a leküldéses értesítési szolgáltatásokhoz, és könnyen toouse eszköz.
* **Rugalmasság**: a fejlesztők mindig választhatják toowork Notification hubs közvetlen hello integrálása mellett is érvényben.
* **Integrált felhasználói felület az [Azure-portálon]**: a leküldési funkciót egy olyan képességet a Mobile Apps vizuálisan jelzi, és a fejlesztők könnyedén használhatják hello kapcsolódó értesítési központon keresztül Mobile Apps.

## <a name="next-steps"></a>Következő lépések
A Notification Hubsról a következő témakörökben talál további információt:

* **[Hogyan használják az ügyfelek a Notification Hubs szolgáltatást]**
* **[A Notification Hubs szolgáltatással kapcsolatos oktatóanyagok és útmutatók]**
* **Notification Hubs használatának első lépéseit oktatóanyagok**: [iOS], [Android], [univerzális Windows-], [Windows Phone], [Kindle], [Xamarin.iOS], [Xamarin.Android]

[0]: ./media/notification-hubs-overview/registration-diagram.png
[1]: ./media/notification-hubs-overview/notification-hub-diagram.png
[Hogyan használják az ügyfelek a Notification Hubs szolgáltatást]: http://azure.microsoft.com/services/notification-hubs
[A Notification Hubs szolgáltatással kapcsolatos oktatóanyagok és útmutatók]: http://azure.microsoft.com/documentation/services/notification-hubs
[iOS]: http://azure.microsoft.com/documentation/articles/notification-hubs-ios-get-started
[Android]: http://azure.microsoft.com/documentation/articles/notification-hubs-android-get-started
[Windows Universal]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-get-started
[Windows Phone]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-phone-get-started
[Kindle]: http://azure.microsoft.com/documentation/articles/notification-hubs-kindle-get-started
[Xamarin.iOS]: http://azure.microsoft.com/documentation/articles/partner-xamarin-notification-hubs-ios-get-started
[Xamarin.Android]: http://azure.microsoft.com/documentation/articles/partner-xamarin-notification-hubs-android-get-started
[Microsoft.WindowsAzure.Messaging.NotificationHub]: http://msdn.microsoft.com/library/microsoft.windowsazure.messaging.notificationhub.aspx
[Microsoft.ServiceBus.Notifications]: http://msdn.microsoft.com/library/microsoft.servicebus.notifications.aspx
[App Service Mobile Apps]: https://azure.microsoft.com/en-us/documentation/articles/app-service-mobile-value-prop/
[templates]: notification-hubs-templates-cross-platform-push-messages.md
[Azure Portal]: https://portal.azure.com
[tags]: (http://msdn.microsoft.com/library/azure/dn530749.aspx)
