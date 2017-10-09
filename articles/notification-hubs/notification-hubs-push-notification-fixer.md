---
title: "a Notification Hubs - aaaAzure diagnosztikai irányelvek"
description: "Iránymutatás a hogyan toodiagnose közös állít ki az Azure Notification hubs használatával."
services: notification-hubs
documentationcenter: Mobile
author: ysxu
manager: erikre
editor: 
ms.assetid: b5c89a2a-63b8-46d2-bbed-924f5a4cce61
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: NA
ms.devlang: multiple
ms.topic: article
ms.date: 10/03/2016
ms.author: yuaxu
ms.openlocfilehash: e374278f2bfdfad36ba091e8846059cd184c17ef
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-notification-hubs---diagnosis-guidelines"></a>Az Azure Notification Hubs - diagnosztikai irányelvek
## <a name="overview"></a>Áttekintés
Azure Notification Hubs-ügyfél hozzáadunk hello leggyakoribb kérdések egyike, miért nem látja az alkalmazás háttérrendszerből küldött értesítést toofigure megjelenésének hello ügyféleszköz - értesítések eldobva hol és miért és hogyan toofix ez. Ebben a cikkben azt végighaladnia hello különböző okokból miért kerülhetnek vagy értesítések ne zárja a hello eszközökön. Keresztül, amelyben elemezheti és mérje fel, hello alapvető ok többféleképpen is fog keresni. 

Először is hogyan Azure Notification Hubs leküldéses értesítések kimenő értesítések toohello eszközök kritikus toounderstand.
![][0]

Egy tipikus send notification folyamatában üdvözlőüzenetére küldi hello **alkalmazás háttér** túl**Azure Notification Hub (NH)** amelynek pedig egyes feldolgozási figyelembe véve az összes hello regisztrációs szerepe fiók hello konfigurált címkék & címke kifejezések toodetermine "cél" azaz összes hello regisztrációk tooreceive hello leküldéses értesítés szükséges. A regisztrációk vagy azok egy részének a támogatott platformok -, iOS és Google, a Windows, Windows Phone között is kiterjedhet Kindle és az Android Kína Baidu. Hello célok létrejöttük NH majd leküldéses értesítések értesítéseket,-e osztani több kötegelt regisztrációk, adott toohello eszközplatform **Push Notification szolgáltatás (PNS)** – például az Apple APNS, GCM a Google stb. NH hello megfelelő PNS hello értesítési központ konfigurálása lapon klasszikus Azure portál hello beállítása hello hitelesítő adatok alapján hitelesíti. hello PNS ezután továbbítja a hello értesítések toohello megfelelő **ügyféleszközök**. Ez az ajánlott módszer toodeliver leküldéses értesítéseket, és vegye figyelembe, hogy értesítés kézbesítése utolsó szakasza hello hello platform pns-sel és a hello között történik hello platform. Ezért tudunk négy fő összetevőből - *ügyfél*, *alkalmazás háttér*, *Azure Notification Hubs (NH)* és *leküldéses értesítési szolgáltatások (PNS)*  és a ezek az értesítések megszakad. További részleteket a ebbe az architektúrába érhető el a [Notification Hubs – áttekintés].

Hiba toodeliver értesítések fordulhat elő, kezdeti hello során teszt/átmeneti fázisban, amely konfigurációs hiba lépett fel, vagy akkor fordulhat elő, ha mindegyike vagy némelyike, hello értesítések éles környezetben előfordulhat, hogy lehet megszakad jelző egy mélyebb alkalmazás vagy az üzenetkezelési minta probléma. Hello területen alatt követően áttekintjük közötti közös toohello ritkább jellegű, amelyek azt tapasztalhatja nyilvánvaló és más nem sok különböző eldobott értesítések lehetőségeket. 

## <a name="azure-notifications-hub-mis-configuration"></a>Az Azure értesítési központ nem megfelelő konfiguráció
Az Azure Notification Hubs kell tooauthenticate magát a hello környezetben hello fejlesztői alkalmazás toobe képes toosuccessfully küldési értesítések toohello a megfelelő PNS. Ennek köszönhetően lehetséges hello fejlesztői fejlesztői fiók létrehozása hello megfelelő platform (Google, Apple Windows stb.), és regisztrálja, ahol azok beszerezni a hitelesítő adatokat, konfigurálva az értesítések hello portálon toobe igénylő alkalmazását Hubok konfigurációs szakasz. Ha értesítés első lépéseként keresztül kell, hogy hello megfelelő hitelesítő adatokkal konfigurálva vannak-e az értesítési központ egyeztetése hello alkalmazással hello tooensure létrehozni a platform adott fejlesztői fiókban. Látni fogja a [első lépéseket bemutató Oktatóanyagainkat] hasznos toogo részletes módon a folyamat. Az alábbiakban néhány gyakori hibás konfigurációk:

1. **Általános**
   
    egy.) Győződjön meg arról, hogy az értesítési központ nevére (nélkül gépelési) azonos hello:
   
   * Ha regisztrál hello ügyfélről 
   * Ahol küldendő értesítések hello háttérrendszerből  
   * Ha konfigurálta a hello PNS hitelesítő adatokat és 
   * A van konfigurálva, amelynek SAS hitelesítő adatait az ügyfél és a hello háttér hello. 
     
     b.) Győződjön meg arról, hogy a használt hello megfelelő SAS konfigurációs karakterláncok hello ügyfélre és hello alkalmazás háttér. A szokásos megoldás, mint kell használnia hello **DefaultListenSharedAccessSignature** hello ügyfélen és **DefaultFullSharedAccessSignature** hello alkalmazás háttérkiszolgálón (amely engedély toobe képes toosend értesítési toohello NH)
2. **Apple Push Notification (APN) szolgáltatás konfigurálása**
   
    Két, különböző hubs – egy termelési környezetben kell fenntartani, és egy másik tesztelési célból. Ez azt jelenti, hogy a védőfal környezet tooa külön hub és éles tooa külön központban toouse fog hello tanúsítvány toouse fog feltölteni hello. Ne próbálkozzon tanúsítványok toohello különböző típusú tooupload ugyanabban a központban, mert okozhat értesítési hibák hello sor le. Ha egy helyen, ahol véletlenül feltöltött tanúsítvány toohello különböző típusú ugyanabban a központban, ajánlott toodelete hello hub és kezdő friss. Ha valamilyen okból, akkor nem tudja toodelete hello hub hello, majd nagyon legalább, törölnie kell minden hello meglévő regisztrációk hello központ. 
3. **Google Cloud Messaging (GCM) konfigurálása** 
   
    egy.) Győződjön meg arról, hogy engedélyezi a "Google Cloud Messaging az Android" a felhő projekt alatt. 
   
    ![][2]
   
    b) ellenőrizze, hogy a "Server"kulcsot hoz létre, amíg a szükséges hello hitelesítő adatokat, mely NH használandó tooauthenticate GCM-mel. 
   
    ![][3]
   
    c.) Győződjön meg arról, hogy a konfigurált "Projekt ID" hello ügyfél, amely szerezhet be hello irányítópult a teljes mértékben numerikus entitás:
   
    ![][1]

## <a name="application-issues"></a>Alkalmazással kapcsolatos problémák
1) **Címkék / kifejezések címke**

Címkék vagy használ címke kifejezések toosegment a célközönséget, esetén mindig lehetséges hello értesítést küld, amikor nincs nincs target alatt található meg a send hívásban hello címkék/címke kifejezések alapján. Legjobb tooreview a regisztrációk tooensure, amelyek nincsenek címkék mely egyezés értesítést küldeni, és ellenőrizze a visszaigazoláshoz hello csak hello ügyfelekről a regisztrációkat. Például Ha az összes NH az a regisztráció kész volt mondja ki a címke "Politika", és küldünk egy értesítés a címke "Sport", akkor nem lesz elküldve tooany eszköz. Összetett eset az alábbiak lehetnek címke kifejezések, ahol csak regisztrált "Címke A" vagy "Címke B", de közben értesítések küldését, "Címke A & & címke B" céloz meg. A hello önálló diagnosztizálása tippek az alábbi szakasz, amelyben áttekintheti a regisztrációk hello címkékkel rendelkeznek együtt módja van. 

2) **Sablon problémák**

Ha sablonokat használ, akkor győződjön meg arról, hogy a következő részben ismertetett hello irányelvek [sablon útmutatást]. 

3) **Érvénytelen a regisztrációk**

Feltéve, hogy helyesen eredményezve hello keresés toowhich hello értesítések kell küldött toobe érvényes célok használt értesítési központ konfigurálva lett hello és címkék/címke kifejezéseket, NH akkor következik be, több feldolgozási kötegek párhuzamosan - egy köteg kikapcsolása üzenetek küldése regisztrációk tooa csoportjának. 

> [!NOTE]
> A Microsoft hello párhuzamos feldolgozása, mert azt nem garantálja mely hello az értesítések kézbesítése történik hello sorrendjét. 
> 
> 

Azure Értesítésközpontról most egy "a legtöbb egyszer" üzenet kézbesítési modell van optimalizálva. Ez azt jelenti, hogy azt kísérlet deaktiválás ismétlődést, így értesítés egynél többször tooa eszköz érkeznek. tooensure azt nézze át a hello regisztrációk, és győződjön meg arról, hogy csak egy üzenetet küldött előtt ténylegesen küldő hello üzenet toohello PNS eszköz azonosítója. Mint minden kötegelt küldött toohello pns-sel, amely viszont fogad és hello regisztráció ellenőrzése, lehetséges, hogy hello PNS egy vagy több hello regisztrációk hibát észlel a kötegben, egy hiba tooAzure NH adja vissza, és leállítja a feldolgozást, ezáltal elvetését a Batch-teljesen. Ez különösen igaz az APN szolgáltatás, amely adatfolyam TCP protokollt használ. Bár azt vannak optimalizálva, a szélső egyszer kézbesítési, ebben az esetben eltávolítása hello hibás regisztrációt az adatbázist, és a újrapróbálkozási lekérdezésértesítés-kézbesítés hello rest hello eszközök adott kötegben.

Kaphat a hello sikertelen kézbesítési kísérlet egy regisztrációs hello Azure Notification hub REST API-k használata elleni hiba információi: [üzenet Telemetriai: első értesítési üzenet Telemetriai](https://msdn.microsoft.com/library/azure/mt608135.aspx) és [PNS visszajelzés](https://msdn.microsoft.com/library/azure/mt705560.aspx). Lásd: hello [SendRESTExample](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/SendRestExample) például kód.

## <a name="pns-issues"></a>Pns-sel kapcsolatos problémák
Miután hello értesítési üzenetet kapott hello megfelelő PNS, akkor célszerű a felelősség toodeliver hello toohello eszközt. Az Azure Notification Hubs hello kép itt kívül esik, és rendelkezik nem szabályozza, amikor, vagy ha hello értesítési toobe toohello eszköz kézbesítve lesz. Mivel hello platform értesítési szolgáltatások közérthető robusztus, értesítéseket a hello PNS általában tooreach hello eszközök néhány másodpercen belül. Ha hello PNS azonban a szabályozás Azure Notification Hubs alkalmazhatja az exponenciális biztonsági stratégia ki, és hogy hello PNS nem érhető el, 30 percig marad, majd azt a szabályzat tooexpire helyezze, és véglegesen dobja el azokat az üzeneteket. 

Ha egy PNS toodeliver értesítést kísérleteket, de hello eszköz offline állapotban, hello értesítési hello PNS egy korlátozott ideig tárolja, és kézbesíteni toohello eszköz, amikor elérhetővé válik. Egy adott alkalmazás csak egy legutóbbi értesítési tárolja. Ha több értesítések küldése közben hello eszköz offline állapotban minden új értesítés hatására hello előzetesen toobe vetve. Ez a viselkedés csak hello legújabb értesítési legyenek hivatkozott tooas értesítést küld a APNS egyesítése és a GCM (összecsukás kulcsot használó) bezárásával. Ha hello eszköz hosszú ideig offline állapotban marad, a rendszer elveti a tárolt volt folyamatban az értesítések. Forrás - [APNS útmutatást] & [GCM útmutató]

Az Azure Notification Hubs - via HTTP-fejléc hello általános használatával is át összevonási kulcs `SendNotification` API (pl. a .NET SDK – `SendNotificationAsync`), amely is veszi át lettek adva, mint a HTTP-fejlécek toohello megfelelő PNS. 

## <a name="self-diagnose-tips"></a>Önálló diagnosztizálása tippek
Itt azt megvizsgálja hello különböző rendszere toodiagnose és a legfelső szintű okozhat az értesítési központ probléma merül fel:

### <a name="verify-credentials"></a>Hitelesítő adatok ellenőrzése
1. **PNS fejlesztői portálján**
   
    Hello megfelelő PNS fejlesztői portal (APNS, GCM, WNS stb.) az ellenőrizze ezeket a [első lépéseket bemutató Oktatóanyagainkat].
2. **Klasszikus Azure portál**
   
    Nyissa meg toohello konfigurálása lapon tooreview, és megfelelő hello hitelesítő adatokat a hello PNS fejlesztői portálján. 
   
    ![][4]

### <a name="verify-registrations"></a>Regisztráció ellenőrzése
1. **Visual Studio**
   
    Visual Studio fejlesztési használatakor Ön tooMicrosoft Azure és a nézet és álló, lemezcsoport típusú többek között az értesítési központ a "Server Explorer" Azure-szolgáltatások kezelése. Ez elsősorban fontos fejlesztési és tesztelési célú környezetnek. 
   
    ![][9]
   
    Megtekintheti és kezelheti a központban minden olyan hello regisztrációk amely platform, natív vagy sablon regisztrációs, bármely címkék, a PNS-azonosító, a regisztrációs azonosítót és hello lejárati dátum szépen kategóriába sorolni. A regisztráció hello parancsprogramok - akkor hasznos, mondja ki, ha azt szeretné tooedit címkéket szerkesztheti is. 
   
    ![][8]
   
   > [!NOTE]
   > A Visual Studio funkcióit tooedit regisztráció csak akkor használható, fejlesztési és tesztelési célú korlátozott számú regisztráció során. Ha hiba merül fel a szükséges toofix egyszerre több, a regisztráció-érdemes funkciójával hello exportálási/importálási regisztrációs leírt ide - [exportálási/importálási regisztrációk](https://msdn.microsoft.com/library/dn790624.aspx)
   > 
   > 
2. **A Service Bus explorer**
   
    Számos ügyfél használja a Szolgáltatásbusz explorer leírt ide - [Szolgáltatásbusz Explorer] megtekintéséhez és kezeléséhez az értesítési központban. Nyílt forráskódú projektként érhető el code.microsoft.com - érdemes [Szolgáltatásbusz Explorer kódot]

### <a name="verify-message-notifications"></a>Ellenőrizze az értesítő üzenetek
1. **Klasszikus Azure portál**
   
    Megnyithatja a toohello "Debug" lapon toosend teszt értesítések tooyour ügyfelek bármely szolgáltatás háttér be kellene és futtatása nélkül. 
   
    ![][7]
2. **Visual Studio**
   
    A Visual Studio hello comforts is küldhet tesztértesítést:
   
    ![][10]
   
    Több on hello Visual Studio Notification Hub Azure explorer funkciót ide - érheti el 
   
   * [Visual STUDIO Server Explorer áttekintése]
   * [Visual STUDIO Server Explorer blogbejegyzés - 1]
   * [Visual STUDIO Server Explorer blogbejegyzés - 2]

### <a name="debug-failed-notifications-review-notification-outcome"></a>Sikertelen értesítések Debug / tekintse át az értesítés kimenetelét
**EnableTestSend tulajdonság**

Notification Hubs keresztül küldi el, amikor először azt csak lekérdezi sorba az összes cél kimenő toofigure feldolgozása NH toodo, és végül NH visszaküldi az toohello PNS. Ez azt jelenti, hogy REST API vagy bármelyik hello ügyfél SDK használatakor hello sikeres térjen vissza a küldési hívás csak azt jelenti, hogy üzenet hello sikeresen sorba lett értesítési központban. Mi történt, amikor NH végül kapott toosend hello üzenet tooPNS betekintést azt nem ad. Ha az értesítés nem bejövő hello ügyféleszköz, van esély arra, hogy ha NH próbált toodeliver hello üzenet tooPNS, hibás pl. hello terhelés méretének túllépte hello PNS hello megengedett vagy hello NH konfigurált hitelesítő adatok Érvénytelen stb tooget hello PNS hibák betekintést egy, a Microsoft vezettek be tulajdonságot, [EnableTestSend szolgáltatás]. Ez a tulajdonság automatikusan engedélyezett teszt hello portál vagy a Visual Studio ügyfél érkező üzenetek, és így lehetővé teszi a részletes toosee küldésekor hibakeresési információ. Ezzel keresztül véve hello példa hello ahol érhető el most .NET SDK API-k és lesz hozzáadva tooall ügyfél SDK-k felé. toouse ezt hello REST-hívást, egyszerűen hozzáfűzése egy lekérdezési karakterlánc paraméter "teszt" hello végén a küldési hívás pl. 

    https://mynamespace.servicebus.windows.net/mynotificationhub/messages?api-version=2013-10&test

*Példa (.NET SDK-val)*

Tegyük fel, hogy a .NET SDK toosend natív bejelentési értesítést használ:

    NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(connString, hubName);
    var result = await hub.SendWindowsNativeNotificationAsync(toast);
    Console.WriteLine(result.State);

`result.State`rendszer egyszerűen állapot `Enqueued` hello végén lévő hello végrehajtása nélkül bármely betekintést, mi történt tooyour leküldéses. Mostantól a hello `EnableTestSend` hello inicializálása közben logikai tulajdonság `NotificationHubClient` és kérhet hello PNS hibákat tapasztalt, miközben hello értesítési kapcsolatos részletesebb állapotinformációit. hello küldési hívás Itt további időt tooreturn lépnek, mert csak adja vissza után NH rendelkezik kézbesíteni hello értesítés tooPNS toodetermine hello kimenetelét. 

    bool enableTestSend = true;
    NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(connString, hubName, enableTestSend);

    var outcome = await hub.SendWindowsNativeNotificationAsync(toast);
    Console.WriteLine(outcome.State);

    foreach (RegistrationResult result in outcome.Results)
    {
        Console.WriteLine(result.ApplicationPlatform + "\n" + result.RegistrationId + "\n" + result.Outcome);
    }

*Minta kimenet*

    DetailedStateAvailable
    windows
    7619785862101227384-7840974832647865618-3
    hello Token obtained from hello Token Provider is wrong

Ez az üzenet azt jelzi, hello értesítési központ érvénytelen hitelesítő adatok vannak konfigurálva, vagy hello regisztrációnak hello hub és hello problémát kell toodelete a regisztrációs és lehetővé teszik a hello ügyfél hello elküldése előtt hozza létre újra kellene ajánlott üzenet. 

> [!NOTE]
> Vegye figyelembe, hogy a tulajdonság használatával hello fokozottan folyamatban van, és ezért csak akkor kell használnia a fejlesztési/tesztelési környezetben a regisztrációk korlátozott számú. Jelenleg csak hibakeresési értesítések küldéséhez too10 eszközök. Azt is legyen a hibakeresési küld toobe 10 percenként feldolgozási korlátja. 
> 
> 

### <a name="review-telemetry"></a>Telemetria áttekintése
1. **Használja a klasszikus Azure portálon**
   
    hello portál lehetővé teszi az értesítési központ összes hello tevékenységhez gyors áttekintést tooget. 
   
    a) a hello "irányítópult" lapon megtekintheti a hello regisztrációk, értesítések, valamint minden egyes platformhoz hibák összesített nézete. 
   
    ![][5]
   
    b) is is hozzáadhat sok más platform adott mérőszámok hello "Figyelés" lapon tootake különösen amikor NH toosend hello értesítési toohello PNS visszaadott PNS konkrét hibáit a mélyebb betekintést. 
   
    ![][6]
   
    c) meg kell kezdődnie, tekintse át a hello **bejövő üzenetek**, **regisztrációs műveletek**, **sikeres értesítések** és folytassa a tooper platform lapon tooreview hello PNS bizonyos hibákat. 
   
    d) Ha van hello értesítési központ konfigurálva hello hitelesítési beállításokkal majd meg PNS hitelesítési hiba jelenik meg. Ez az egy jól jelzi toocheck hello PNS hitelesítő adatokat. 

2) **Programozott hozzáférés**

További részletek itt- 

* [Telemetria programozott hozzáférés]
* [A minta API-k hozzáférésének telemetriai adat] 

> [!NOTE]
> Több telemetriával kapcsolatos funkciók, például **exportálási/importálási regisztrációk**, **Telemetriai API-k hozzáférésének** stb csak érhetők el a Standard csomagra. Ha toouse kísérli meg a ezeket a szolgáltatásokat, ha a szabad vagy az alapszintű csomag majd meg kap a kivétel üzenet toothis hatás hello SDK és egy HTTP 403 (tiltott) használatakor azokat közvetlenül a REST API-k hello használatakor. Győződjön meg arról, hogy Ön rendelkezik magasabbra állítani tooStandard réteg klasszikus Azure portálon keresztül.  
> 
> 

<!-- IMAGES -->
[0]: ./media/notification-hubs-diagnosing/Architecture.png
[1]: ./media/notification-hubs-diagnosing/GCMConfigure.png
[2]: ./media/notification-hubs-diagnosing/GCMEnable.png
[3]: ./media/notification-hubs-diagnosing/GCMServerKey.png
[4]: ./media/notification-hubs-diagnosing/PortalConfigure.png
[5]: ./media/notification-hubs-diagnosing/PortalDashboard.png
[6]: ./media/notification-hubs-diagnosing/PortalMonitoring.png
[7]: ./media/notification-hubs-diagnosing/PortalTestNotification.png
[8]: ./media/notification-hubs-diagnosing/VSRegistrations.png
[9]: ./media/notification-hubs-diagnosing/VSServerExplorer.png
[10]: ./media/notification-hubs-diagnosing/VSTestNotification.png

<!-- LINKS -->
[Notification Hubs – áttekintés]: notification-hubs-push-notification-overview.md
[első lépéseket bemutató Oktatóanyagainkat]: notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md
[sablon útmutatást]: https://msdn.microsoft.com/library/dn530748.aspx 
[APNS útmutatást]: https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW4
[GCM útmutató]: http://developer.android.com/google/gcm/adv.html
[Export/Import Registrations]: http://msdn.microsoft.com/library/dn790624.aspx
[Szolgáltatásbusz Explorer]: http://msdn.microsoft.com/library/dn530751.aspx
[Szolgáltatásbusz Explorer kódot]: https://code.msdn.microsoft.com/windowsazure/Service-Bus-Explorer-f2abca5a
[Visual STUDIO Server Explorer áttekintése]: http://msdn.microsoft.com/library/windows/apps/xaml/dn792122.aspx 
[Visual STUDIO Server Explorer blogbejegyzés - 1]: http://azure.microsoft.com/blog/2014/04/09/deep-dive-visual-studio-2013-update-2-rc-and-azure-sdk-2-3/#NotificationHubs 
[Visual STUDIO Server Explorer blogbejegyzés - 2]: http://azure.microsoft.com/blog/2014/08/04/announcing-release-of-visual-studio-2013-update-3-and-azure-sdk-2-4/ 
[EnableTestSend szolgáltatás]: http://msdn.microsoft.com/library/microsoft.servicebus.notifications.notificationhubclient.enabletestsend.aspx
[Telemetria programozott hozzáférés]: http://msdn.microsoft.com/library/azure/dn458823.aspx
[A minta API-k hozzáférésének telemetriai adat]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/FetchNHTelemetryInExcel

