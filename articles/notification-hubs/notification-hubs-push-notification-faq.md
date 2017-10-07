---
title: "Az Azure Notification hubs használatával: Gyakori kérdések (GYIK) |} Microsoft Docs"
description: "A Notification Hubs megoldások designing/végrehajtási – gyakori kérdések"
services: notification-hubs
documentationcenter: mobile
author: ysxu
manager: erikre
keywords: "leküldéses értesítések, leküldéses értesítések, leküldéses értesítések iOS, android leküldéses értesítések, ios leküldéses, android leküldéses"
editor: 
ms.assetid: 7b385713-ef3b-4f01-8b1f-ffe3690bbd40
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: multiple
ms.topic: article
ms.date: 01/19/2017
ms.author: yuaxu
ms.openlocfilehash: a70efa7fc5954966847d5a173e9b10accf4b737e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="push-notifications-with-azure-notification-hubs-frequently-asked-questions"></a>Leküldéses értesítések az Azure Notification hubs használatával: gyakori kérdések
## <a name="general"></a>Általános kérdések
### <a name="what-is-hello-resource-structure-of-notification-hubs"></a>Mi az erőforrás-szerkezet hello értesítési központját?

Az Azure Notification Hubs két erőforrás olyan szintje van: hubok és névtereket. A központ hello platformok közötti leküldéses adatokat egy alkalmazás tárolására képes egyetlen leküldéses erőforrás. A névtér egy régióban hubok gyűjteménye.

Ajánlott leképezési megegyezik egy névtér egy alkalmazással. Egy adott névtéren belül is egy üzemi hub, amely a termelési alkalmazással együttműködve, egy tesztelési hub, amely a tesztelési alkalmazást, és így tovább működik.

### <a name="what-is-hello-price-model-for-notification-hubs"></a>Mi az a Notification Hubs hello ár modell?
hello legújabb díjszabása található hello [Notification Hubs-díjszabás] lap. A Notification Hubs számlázása hello névtér szintjén történik. (Hello meghatározása névtér: "Mi az erőforrás-szerkezet hello értesítési központját?") A Notification Hubs három réteg kínálja:

* **Szabad**: Ebben a rétegben az jó kiindulási pont felderítését lehetővé tevő leküldéses képességeit. Termelési alkalmazások esetében nem ajánlott. 500 eszközök kap, és nincs szolgáltatás szolgáltatásiszint-szerződés (SLA) garantált száma havonta, névtér része kimenő 1 millió.
* **Alapszintű**: Ez a réteg (vagy hello Standard csomagra) javasolt kisebb termelési alkalmazások esetében. 200 000 eszköz kap, és 10 millió leküldéses értesítések száma havonta kiindulópontként névtér része. Kvóta növekedési beállítások tartoznak.
* **Standard**: Ebben a rétegben közepes toolarge termelési alkalmazások esetében ajánlott. 10 millió eszközök kap, és 10 millió leküldéses értesítések száma havonta kiindulópontként névtér része. Kvóta növelését beállítások és a gazdag telemetriai képességek érhetők el.

Standard szint funkciói:
* **Részletes telemetria**: használhatja a Notification Hubs egy üzenet Telemetriai tootrack bármely leküldéses kérelmek és Platform Notification System visszajelzés a hibakereséshez.
* **Több vállalat kiszolgálása**: a névtér szinten Platform Notification System hitelesítő adatok használhatók. Ez a beállítás lehetővé teszi, hogy Ön tooeasily bérlők felosztása hubok hello belül ugyanazt a névteret.
* **Ütemezett leküldéses**: bármikor küldött értesítések toobe is ütemezheti.

### <a name="what-is-hello-notification-hubs-sla"></a>Mi az a Notification Hubs SLA hello?
Basic és standard szintű Notification Hubs rétegek, a megfelelően konfigurált alkalmazások leküldéses értesítéseket küldeni, vagy legalább 99,9 % hello idő regisztrációs felügyeleti műveleteket végezhet. További információk hello SLA-t, lépjen toohello toolearn [Notification Hubs SLA](https://azure.microsoft.com/support/legal/sla/notification-hubs/) lap.

> [!NOTE]
> Leküldéses értesítések függ külső Platform Notification System (például az Apple APNS és Google FCM), mert nincs egyetlen üzenetek hello kézbesítésre (SLA) garantált. Után a Notification hubs használatával küldi hello kötegek tooPlatform Notification System (garantált SLA), akkor hello Platform Notification System toodeliver hello hello felelőssége leküldéses értesítések (nem garantált SLA).

### <a name="which-customers-are-using-notification-hubs"></a>Mely felhasználók, akik használják a Notification Hubs?
Sok ügyfél a Notification Hubs használatával. Az itt felsorolt néhány fontos néhányat a meglévők közül:

* Sochi 2014: Több száz érdeklődési körök alapján, 3 + millió eszközöket, és 150 + millió értesítések elküldése két hétben. [Esettanulmány: Sochi]
* Skanska: [esettanulmány: Skanska]
* Budapest alkalommal: [esettanulmány: Budapest alkalommal]
* Mural.LY: [esettanulmány: Mural.ly]
* 7Digital: [esettanulmány: 7Digital]
* Bing-alkalmazásokat: Eszközök tízezer értesítésküldést 3 millió / nap.

### <a name="how-do-i-upgrade-or-downgrade-my-hub-or-namespace-tooa-different-tier"></a>Hogyan frissítést vagy a központ vagy az névtér tooa másik réteghez használni?
Nyissa meg toohello  **[Azure-portálon]** > **Notification Hubs névterek** vagy **Notification Hubs**. Válassza ki a kívánt tooupdate, és nyissa meg túl hello erőforrás**Tarifacsomagot**. Megjegyzés: hello követelményeknek:

* hello frissített tarifacsomag vonatkozik túl*összes* hubok dolgozunk hello névtérben.
* Ha az eszköz száma túllépi ezt hello hello réteg, akkor még a visszaminősítése, toodelete eszközök előtt kell meg használni.


## <a name="design-and-development"></a>Tervezési és fejlesztési
### <a name="which-server-side-platforms-do-you-support"></a>Mely kiszolgálóoldali platformokon támogatott?
A .NET, Java, Node.js, PHP és Python Server SDK-k érhetők el. Notification Hubs API-k REST felületek, alapulnak, így közvetlenül a REST API-k közben használható, ha nem szeretné, további függőségi vagy használjuk a különböző platformokon. További információért tekintse meg a toohello [Notification hub REST API-k] lap.

### <a name="which-client-platforms-do-you-support"></a>Milyen ügyfélplatformokat támogatja?
Leküldéses értesítések támogatottak [iOS](notification-hubs-ios-apple-push-notification-apns-get-started.md), [Android](notification-hubs-android-push-notification-google-gcm-get-started.md), [univerzális Windows-](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md), [Windows Phone](notification-hubs-windows-mobile-push-notifications-mpns.md), [Kindle](notification-hubs-kindle-amazon-adm-push-notification.md), [(Keresztül Baidu) android Kína](notification-hubs-baidu-china-android-notifications-get-started.md), Xamarin ([iOS](xamarin-notification-hubs-ios-push-notification-apns-get-started.md) és [Android](xamarin-notification-hubs-push-notifications-android-gcm.md)), [Chrome-alkalmazások](notification-hubs-chrome-push-notifications-get-started.md), és [Safari](https://github.com/Azure/azure-notificationhubs-samples/tree/master/PushToSafari). További információért tekintse meg a toohello [Notification Hubs használatának első lépéseit oktatóanyagok] lap.

### <a name="do-you-support-text-message-email-or-web-notifications"></a>Támogatott az SMS-üzenet, e-mailek vagy webes értesítések?
A Notification Hubs elsősorban tervezett toosend értesítések toomobile alkalmazások. Azt nem adja meg e-mailben vagy szöveges üzenetek kezelésére képes. Azonban külső rendszerek, amelyek ezeket a képességeket biztosítanak integrálható a Notification Hubs toosend natív leküldéses értesítések segítségével [Mobile Apps].

A Notification Hubs is nem biztosít egy böngészőn belüli leküldéses értesítési kézbesítési szolgáltatás hello kezdő verzióról. Az ügyfelek ezt a funkciót fölött hello támogatott kiszolgálóoldali platformok SignalR használatával is létrehozható. Ha azt szeretné, hogy toosend értesítések toobrowser alkalmazások hello Chrome védőfal, tekintse meg a hello [Chrome-alkalmazások oktatóanyag].

### <a name="how-are-mobile-apps-and-azure-notification-hubs-related-and-when-do-i-use-them"></a>Hogyan történik a Mobile Apps és az Azure Notification hubs használatával kapcsolatos és mikor használni őket?
Ha egy meglévő mobile app vissza end és meg tooadd csak hello funkció toosend leküldéses értesítések, használhatja az Azure Notification hubs használatával. Ha a mobilalkalmazás háttér teljesen új akarja tooset, érdemes lehet hello Azure App Service Mobile Apps szolgáltatása. A mobilalkalmazások automatikusan egy értesítési központot látja el, hogy könnyen küldhet leküldéses értesítést hello mobilalkalmazás háttérből. A Mobile Apps árképzési magában foglalja a hello alap díjat egy értesítési központ. Ha több mint szereplő hello leküldéses értesítések csak kell fizetnie. További részletekért a költségek, lépjen a toohello [App Service szolgáltatás díjszabása] lap.

### <a name="how-many-devices-can-i-support-if-i-send-push-notifications-via-notification-hubs"></a>Eszközök számáról is támogatásnak Ha küldhető a leküldéses értesítések a Notification Hubs használatával?
Tekintse meg a toohello [Notification Hubs-díjszabás] hello támogatott eszközök száma a Részletek lap.

Ha több mint 10 millió regisztrált eszközök esetében segítségre [, lépjen velünk kapcsolatba](https://azure.microsoft.com/overview/contact-us/) közvetlenül és segíteni fogunk a megoldás méretezése.

### <a name="how-many-push-notifications-can-i-send-out"></a>Hány leküldéses értesítéseket is küldhető?
Attól függően, hogy hello kijelölt réteg Azure Notification Hubs automatikusan rendkívül hello rendszer átfutó értesítések hello száma alapján.

> [!NOTE]
> hello általános felhasználás költsége növelheti a leküldéses értesítések kiszolgált hello száma alapján. Gondoskodjon arról, hogy tisztában legyen a hello vázolt hello réteg korlátok [Notification Hubs-díjszabás] lap.
> 
> 

A felhasználók naponta használja a Notification Hubs toosend több millió leküldéses értesítést. Nincs toodo semmilyen különleges tooscale hello mindaddig, amíg az Azure Notification Hubs használata leküldéses értesítést elérni.

### <a name="how-long-does-it-take-for-sent-push-notifications-tooreach-my-device"></a>Mennyi ideig minderre hajtsa végre a megfelelő számára küldött leküldéses értesítések tooreach az eszköz?
Normál-használata esetén, ahol hello bejövő terhelés konzisztens, és akkor is, Azure Notification hubs használatával tud feldolgozni legalább *1 millió leküldéses értesítést küld egy percet*. Ez a számláló hello lehet több címkét, hello jellegét hello bejövő küld, és más külső tényezőktől függően változhat.

Becsült hello szállítási idő alatt hello szolgáltatást hello célok platformonként számítja ki, és továbbítja a üzenetek toohello Push Notification szolgáltatás (PNS) regisztrált hello címkék vagy címke kifejezések alapján. Feladata hello hello PNS toosend értesítések toohello eszköz.

hello PNS nem garantálható, hogy bármely SLA értesítések kézbesítéséhez. Azonban a legtöbb leküldéses értesítések érkeznek tootarget eszközök (általában legalább 10 perccel) néhány percen belül hello óta tooNotification hubok elküldi őket. Néhány értesítések több időt vehet igénybe.

> [!NOTE]
> Az Azure Notification Hubs egy szabályzattal rendelkezik a hely toodrop toohello PNS nem kézbesíteni 30 percen belül leküldéses értesítések. Ez a késleltetés akkor fordulhat elő, a számos oka, de a legtöbb gyakran, mert az alkalmazás a hello PNS szabályozás.
> 
> 

### <a name="is-there-any-latency-guarantee"></a>Van bármilyen késés garancia?
(Azok érkeznek által egy külső, a platform-specifikus PNS) leküldéses értesítések hello jellege, mert nincs késés garancia. Általában a leküldéses értesítések hello többsége néhány percen belül érkeznek.

### <a name="what-do-i-need-tooconsider-when-designing-a-solution-with-namespaces-and-notification-hubs"></a>Mire van szükségem az tooconsider a névtereket és a notification hubs megoldás tervezése során?
#### <a name="mobile-appenvironment"></a>Mobile app-környezet
* Használjon egy értesítési központ mobilalkalmazás környezet másodpercenkénti száma.
* Több-bérlős-konfigurációban mindegyik bérlő külön hubot kell rendelkeznie.
* Soha ne megosztás hello termelési és tesztelési környezetek egy értesítési központban. Ez az eljárás problémákat okozhat, értesítések küldéséhez. (Apple védőfal és a termelési leküldéses végpont, külön-külön hitelesítő adatokat biztosít.)
* Alapértelmezés szerint küldjön értesítéseket regisztrált tooyour teszteszközök hello Azure-portálon keresztül, vagy a Visual Studio Azure integrált összetevő hello. hello küszöbértéke too10 eszközöket, véletlenszerűen kiválasztott hello regisztrációs készletből.

> [!NOTE]
> Ha a központ volt konfigurálva, az Apple védőfal tanúsítványt, és újra konfigurált toouse egy Apple éles tanúsítvány majd lett, hello eredeti tokenek érvénytelenek. Érvénytelen jogkivonatok leküldéses értesítések toofail miatt. A termelési és tesztkörnyezetek külön, és különböző hubok használni a különböző környezetekhez.
> 
> 

#### <a name="pns-credentials"></a>PNS hitelesítő adatok
Ha a mobilalkalmazás regisztrálva lett a platform fejlesztői portálján (például az Apple vagy a Google), az alkalmazás azonosítóját és a biztonsági jogkivonatok küldése. hello app háttér biztosítják ezeket a jogkivonatokat toohello platform pns-sel, hogy a leküldéses értesítések küldhetők toodevices. Biztonsági jogkivonatok hello formában tanúsítványok (például Apple iOS- vagy Windows Phone) vagy a biztonsági kulcsok (például a Google Android vagy Windows) lehet. Azok a notification hubs használatával kell konfigurálni. Konfigurációs általában hello értesítésiközpont szintjén történik, de azt is megteheti egy több-bérlős forgatókönyvben hello névtér szinten.

#### <a name="namespaces"></a>Névterek
Névterek telepítési csoportosításához használható. Is el használt toorepresent összes notification hubs használatával az összes bérlői hello több-bérlős esetén ugyanazt az alkalmazást.

#### <a name="geo-distribution"></a>Földrajzi terjesztési
Földrajzi terjesztési fontosságú nem mindig a leküldéses értesítési forgatókönyvek. Különböző PNSes (például APNS vagy GCM), hogy a leküldéses értesítések toodevices nem egyenletesen.

Ha egy alkalmazás, amelynek globálisan, létrehozhat hubok különböző névterekben különböző Azure-régiók körül hello world hello értesítési központok szolgáltatás használatával.

> [!NOTE]
> Ezzel az elrendezéssel nem ajánlott, mert növeli a felügyeleti költségek, különösen a regisztrációhoz. El kell végezni, csak akkor, ha explicit szükség van.
> 
> 

### <a name="should-i-do-registrations-from-hello-app-back-end-or-directly-through-client-devices"></a>Teendő regisztrációk hello app háttérből, vagy közvetlenül az ügyfél eszközöket?
Regisztráció hello app háttérből hasznosak, ha rendelkezik tooauthenticate hello regisztrációs létrehozása előtt. Fontosságúak is hasznos, ha kell létrehozni vagy módosítani hello app háttér app logikán alapuló címkéket. További információért tekintse meg a toohello [háttér regisztrációs útmutatót] és [háttér regisztrációs útmutatót 2] lapok.

### <a name="what-is-hello-push-notification-delivery-security-model"></a>Mi az a leküldéses értesítési hello biztonsági modell?
Az Azure Notification Hubs használ egy [közös hozzáférésű jogosultságkódot](../storage/common/storage-dotnet-shared-access-signature-part-1.md)-alapú biztonsági modellt. Hello megosztott hozzáférési aláírást jogkivonatok gyökérszinten hello névtér vagy hello részletes notification hub szinten használhatja. Megosztott hozzáférési aláírást jogkivonatok toofollow különböző engedélyezési szabályokat, például toosend üzenet engedélyeket vagy az értesítési engedélyek toolisten lehet. További információkért lásd: hello [Notification Hubs biztonsági modell] dokumentum.

### <a name="how-should-i-handle-sensitive-payload-in-push-notifications"></a>Hogyan kezelje a leküldéses értesítések bizalmas tartalom?
Az értesítések érkeznek tootarget eszközök hello platform PNS által. Értesítést küldött tooAzure Notification Hubs, feldolgozott és toohello átadott megfelelő PNS.

Az összes kapcsolat hello küldő toohello Azure Notification Hubs toohello PNS, a HTTPS használatára.

> [!NOTE]
> Az Azure Notification Hubs nem naplózza az bármely módon üzenetek hello hasznos.
> 
> 

toosend bizalmas Payload van jelen, ajánlott biztonságos leküldéses minta használatával. hello küldő ping értesítést nyújt, egy üzenet azonosítója toohello eszközzel hello bizalmas tartalom nélkül. Hello hasznos hello alkalmazást hello eszközön fogadásakor hello app biztonságos API meghívja a közvetlen toofetch hello üzenet adatai. Útmutató, hogyan tooimplement ez mintát, nyissa meg a toohello [Notification Hubs biztonságos leküldéses oktatóanyag] lap.

## <a name="operations"></a>Műveletek
### <a name="what-support-is-provided-for-disaster-recovery"></a>Milyen támogatja-e a vész-helyreállítási?
Metaadatok vész-helyreállítási érvényességének nyújtunk a End (hello Notification Hubs nevét, hello kapcsolati karakterlánc és más kritikus fontosságú adatokat). Vész-helyreállítási forgatókönyv kiváltásakor a regisztrációs adatok-e a hello *csak szegmentálni* hello Notification Hubs infrastruktúra, amely elvész. Az új központi a helyreállítás után azokat a megoldás toorepopulate tooimplement ezeket az adatokat lesz szüksége:

1. Hozzon létre egy másodlagos értesítésközpontról ugyanabban az adatközpontban. Javasoljuk, hogy hozzon létre egyet a hello kezdete tooshield, a vész helyreállítási eseményeket, amelyek hatással lehetnek a felügyeleti képességek. Is létrehozhat egy hello vész-helyreállítási esemény hello idején.

2. Az elsődleges értesítési központ a hello regisztrációk a hello másodlagos értesítési központ feltöltéséhez. Jelenleg nem javasoljuk, próbáljon mindkét hubok toomaintain regisztrációnak és szinkronban maradjanak, a regisztráció térjen. Ez az eljárás hello PNS ügyféloldali jól hello rejlő hogy regisztrációk tooexpire miatt nem működik. A Notification Hubs a szükségtelenné vált őket, lejárt vagy érvénytelen a regisztrációk PNS visszajelzést kap.  

Alkalmazás hátsó végpontok két javaslatok vezetünk be:

* Egy alkalmazás háttér végén regisztrációk egy adott csoportjának által használható. A tömeges beszúrás hello másodlagos értesítési elosztóhoz majd ellátásához.

* Egy alkalmazás háttér, amely egy rendszeres biztonsági másolat regisztrációk lekérése hello elsődleges értesítési központ biztonsági mentéséhez használja. A tömeges beszúrás hello másodlagos értesítési elosztóhoz majd ellátásához.

> [!NOTE]
> Regisztráció Exportálás/importálás funkció érhető el, normál rétegben hello hello ismertetett [regisztrációk exportálási/importálási] dokumentum.
> 
> 

Ha a háttérből nincs céleszközökön meg hello alkalmazás indításakor, akkor hajtsa végre egy új regisztrációs hello másodlagos értesítési központ. Végül hello másodlagos értesítési központ lesz minden hello aktív eszköz regisztrálva.

Egy adott időszakban, amikor a megnyitott alkalmazásokat futtató eszközökről nem értesítéseket lesz.

### <a name="is-there-audit-log-capability"></a>Ellenőrzési napló lehetőségek van?
A Notification Hubs összes felügyeleti művelet lépjen toooperation naplókat, amelyek ki vannak téve a hello [a klasszikus Azure portálon].

## <a name="monitoring-and-troubleshooting"></a>Figyelés és hibaelhárítás
### <a name="what-troubleshooting-capabilities-are-available"></a>Milyen hibaelhárítási lehetőségek érhetők el?
Az Azure Notification Hubs hibaelhárításhoz, különösen a leggyakoribb forgatókönyvet hello eldobott értesítések több funkciót biztosít. További információkért lásd: hello [Notification Hubs hibaelhárítási] találhatók meg.

### <a name="what-telemetry-features-are-available"></a>Milyen telemetriai funkciók érhetők el?
Az Azure Notification Hubs lehetővé teszi, hogy telemetriai adatok megtekintése az hello [a klasszikus Azure portálon]. Hello metrikák részletei érhetők el a hello [Notification Hubs metrikák] lap.

> [!NOTE]
> Sikeres értesítések egyszerűen jelenti, hogy leküldéses értesítéseket lesz küldve toohello külső pns-sel (például az Apple APNS) vagy a Google GCM. Feladata hello hello PNS toodeliver hello értesítések tootarget eszközök. Általában hello PNS nem fed fel kézbesítési metrikák toothird felek.  
> 
> 

Is biztosítunk hello funkció tooexport hello telemetriai adatokat programozott módon (a hello Standard szint). További információkért lásd: hello [Notification Hubs metrikák minta].

[a klasszikus Azure portálon]: https://manage.windowsazure.com
[Notification Hubs-díjszabás]: http://azure.microsoft.com/pricing/details/notification-hubs/
[Notification Hubs SLA]: http://azure.microsoft.com/support/legal/sla/
[Esettanulmány: Sochi]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=7942
[Esettanulmány: Skanska]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=5847
[Esettanulmány: Budapest alkalommal]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=8354
[Esettanulmány: Mural.ly]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=11592
[Esettanulmány: 7Digital]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=3684
[Notification hub REST API-k]: https://msdn.microsoft.com/library/azure/dn530746.aspx
[Notification Hubs használatának első lépéseit oktatóanyagok]: http://azure.microsoft.com/documentation/articles/notification-hubs-ios-get-started/
[Chrome-alkalmazások oktatóanyag]: http://azure.microsoft.com/documentation/articles/notification-hubs-chrome-get-started/
[Mobile Services Pricing]: http://azure.microsoft.com/pricing/details/mobile-services/
[háttér regisztrációs útmutatót]: https://msdn.microsoft.com/library/azure/dn743807.aspx
[háttér regisztrációs útmutatót 2]: https://msdn.microsoft.com/library/azure/dn530747.aspx
[Notification Hubs biztonsági modell]: https://msdn.microsoft.com/library/azure/dn495373.aspx
[Notification Hubs biztonságos leküldéses oktatóanyag]: http://azure.microsoft.com/documentation/articles/notification-hubs-aspnet-backend-ios-secure-push/
[Notification Hubs hibaelhárítási]: http://azure.microsoft.com/documentation/articles/notification-hubs-diagnosing/
[Notification Hubs metrikák]: https://msdn.microsoft.com/library/dn458822.aspx
[Notification Hubs metrikák minta]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/FetchNHTelemetryInExcel
[regisztrációk exportálási/importálási]: https://msdn.microsoft.com/library/dn790624.aspx
[Azure-portálon]: https://portal.azure.com
[complete samples]: https://github.com/Azure/azure-notificationhubs-samples
[Mobile Apps]: https://azure.microsoft.com/services/app-service/mobile/
[App Service szolgáltatás díjszabása]: https://azure.microsoft.com/pricing/details/app-service/
