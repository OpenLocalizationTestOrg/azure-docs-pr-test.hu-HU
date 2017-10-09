---
title: "aaaSecure az eszközök internetes hálózatát telepítéstől |} Microsoft Docs"
description: "Ez a következő cikket: részletek hogyan toosecure az IoT-telepítés"
services: 
suite: iot-suite
documentationcenter: 
author: YuriDio
manager: timlt
editor: 
ms.assetid: 95c23341-16b0-4954-b3f2-d2e82ab7b367
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: yurid
ms.openlocfilehash: befba8f2009279c2217dcd3496d529139134ec01
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="secure-your-iot-deployment"></a>Az IoT üzemelő példányának védelme
Ez a cikk hello következő szint biztosít hello Azure IoT-alapú az eszközök internetes hálózatát (IoT) infrastruktúra védelmének biztosítása. Tooimplementation szint konfigurálása és telepítése az egyes összetevők részletes hivatkozik. Összehasonlítás és a lehetőségek között a különböző versengő módszer is biztosít.

Hello Azure IoT telepítés biztonságossá tétele a következő három biztonsági területek hello osztható:

* **Eszközbiztonsági**: biztonságossá tétele hello IoT-eszközök a helyettesítő hello telepítésekor.
* **Kapcsolat biztonsági**: hello IoT-eszközök és az IoT-központ között továbbított összes adat biztosítva a bizalmas és hamisíthatatlan.
* **A felhő biztonsági**: azt toosecure adatokat szolgáltató keresztül halad át, és hello felhőben van tárolva.

![Három biztonsági területek][img-overview]

## <a name="secure-device-provisioning-and-authentication"></a>Kiépítés biztonságos eszköz és a hitelesítés
hello Azure IoT Suite az IoT-eszközök a következő két módszer hello biztonságossá tételére:

* Az egyes eszközök ad meg egy egyedi identitáskulcs (biztonsági jogkivonatokat), amelyekkel hello eszköz toocommunicate a hello IoT-központ által.
* A-eszköz segítségével [X.509 tanúsítvány] [ lnk-x509] és titkos kulcsok azt jelenti, hogy tooauthenticate hello eszköz toohello IoT-központot. Ez a hitelesítési módszer biztosítja, hogy hello hello eszközön titkos kulcs nem ismert hello eszköz kívül bármikor, így magasabb szintű biztonságot.

hello biztonsági token módszer minden hívás hello szimmetrikus kulcs tooeach hívás társításával hello eszköz tooIoT központ által végzett hitelesítést nyújt. X.509-alapú hitelesítés lehetővé teszi, hogy az IoT-eszközök hitelesítési hello fizikai rétegben hello TLS kapcsolat létrehozásának részeként. hello biztonsági jogkivonat-alapú módszer hello X.509-hitelesítést, amely egy kevésbé biztonságos minta nélkül használható lesz. hello hello két módszerek közötti választást van elsősorban meghatározni által, hogy mennyire vannak biztonságban hello eszköz hitelesítést kell toobe és biztonságos tárolási hello eszközön rendelkezésre állását (toostore hello titkos kulcs biztonságos).

## <a name="iot-hub-security-tokens"></a>IoT Hub biztonsági tokenek
Az IoT-központ biztonsági jogkivonatokat tooauthenticate eszközöket és szolgáltatásokat tooavoid kulcsok küldése hello hálózati használja. Emellett biztonsági jogkivonatok érvényesség és hatókör korlátozott. Azure IoT SDK-k automatikusan hoz létre jogkivonatokat anélkül, hogy semmiféle speciális beállítást. Bizonyos esetekben azonban hello felhasználói toogenerate igényelnek, és biztonsági jogkivonatokat közvetlenül használható. Ezek közé tartozik a hello hello MQTT, az AMQP vagy a HTTP-felületek közvetlen használatát, vagy hello végrehajtásának hello biztonságijogkivonat-szolgáltatás mintát.

További részleteket a hello biztonsági jogkivonatot, és a használati hello szerkezete hello a következő cikkekben található:

* [Biztonsági jogkivonat szerkezete][lnk-security-tokens]
* [SAS-tokenje eszközként használatával][lnk-sas-tokens]

Minden egyes IoT-központ rendelkezik egy [identitásjegyzékhez] [ lnk-identity-registry] , amely lehet hello szolgáltatás, például egy várólista, amely tartalmazza az üzenetsoroktól felhő eszközre üzenetek és tooallow hozzáférés toohello használt toocreate eszközönkénti erőforrások eszköz felé néző végpontok. az IoT-központ identitásjegyzékhez hello eszköz identitások és a biztonsági kulcsok megoldás biztonságos tárolására szolgál. Személy vagy eszköz identitások csoportok is hozzáadhatók tooan engedélyezése, vagy egy tiltólista engedélyezése a teljes felügyeletet gyakorolhat az eszközök elérést. hello következő cikkekben további részletekkel szolgálnak hello struktúra hello identitásjegyzékhez és a támogatott műveletek.

[Az IoT-központ például MQTT AMQP vagy HTTP protokollt támogat][lnk-protocols]. Biztonsági jogkivonatokat a hello IoT eszköz tooIoT Hub másképp egyes ezeket a protokollokat használja:

* AMQP: SASL egyszerű és AMQP jogcímalapú biztonsági ({házirendnév}@sas.root. { iothubName} hello esetben az IoT hub-szintű tokenek; {deviceId} eszköz hatókörű jogkivonatok esetén).
* MQTT: {DeviceId} csomagot használ Csatlakozás másként hello {ClientId}, {IoThubhostname} / {deviceId} hello a **felhasználónév** mező és SAS-kód a token hello **jelszó** mező.
* HTTP: Érvénytelen lexikális elem hello engedélyezési kérelem fejlécében.

Az IoT-központ identitásjegyzékhez használt tooconfigure eszközönkénti hitelesítő adatokat kell, és hozzáférés-vezérlést. Azonban ha egy IoT-megoldás már jelentős befektetési egy [egyéni eszköz identitása beállításjegyzék és/vagy hitelesítési séma][lnk-custom-auth], ezért integrálható az IoT hubbal meglévő infrastruktúra hozzon létre egy jogkivonat-szolgáltatás.

### <a name="x509-certificate-based-device-authentication"></a>X.509 tanúsítvány alapú eszközhitelesítés
hello használata egy [eszközalapú X.509 tanúsítvány] [ lnk-protocols] és a társított titkos és nyilvános kulcsból álló kulcspárt lehetővé teszi, hogy a további hitelesítési hello fizikai rétegben. hello titkos kulcs lesz biztonságosan tárolva hello eszközt, és nincs felderíthető hello eszköz kívül. hello X.509 tanúsítvány hello eszköz, eszköz-Azonosítóját, például információ és az egyéb szervezeti adatait tartalmazza. Egy hello tanúsítvány aláírása generálja hello titkos kulccsal.

Magas szintű eszköz üzembe helyezési folyamata:

* Egy azonosító tooa fizikai eszköz társítása – eszközidentitást és/vagy X.509 tanúsítvány társított toohello eszköz eszköz gyártási vagy üzembe helyezés során.
* Hozzon létre egy megfelelő identitás IoT Hub – eszközidentitást és az IoT-központ identitásjegyzékhez hello társított eszközadatokat.
* Biztonságosan tárolja az IoT-központ identitásjegyzékhez X.509 tanúsítvány ujjlenyomata.

### <a name="root-certificate-on-device"></a>Legfelső szintű tanúsítványt az eszközre
Az IoT hubbal biztonságos TLS kapcsolat létrehozásakor hello IoT-eszközök hello eszköz SDK részét képező legfelső szintű tanúsítványt használ az IoT-központ hitelesíti magát. Hello C ügyfél SDK hello tanúsítvány hello mappában található "\\c\\Tanúsítványos" hello tárház hello gyökere alatt. Bár a legfelső szintű tanúsítványok hosszú élettartamú, továbbra is lehetséges, hogy lejáratukig vagy vonható vissza. Ha nincs mód frissítésére a hello tanúsítvány hello eszközön hello eszköz nem lehet toosubsequently csatlakoztassa az IoT-központ toohello (vagy bármely más felhőalapú szolgáltatás). Azt jelenti, hogy tooupdate hello legfelső szintű tanúsítvánnyal rendelkező hello IoT-eszközök telepítése után fog hatékonyan a kockázat csökkentése érdekében.

## <a name="securing-hello-connection"></a>Hello kapcsolat biztonságossá tétele
Internet kapcsolat hello IoT-eszközök és az IoT-központ között hello Transport Layer Security (TLS) szabvány használatával lett biztonságossá. Az Azure IoT támogatja [TLS 1.2][lnk-tls12], TLS 1.1 és TLS 1.0, az itt megadott sorrendben. A TLS 1.0 támogatását a csak a visszamenőleges kompatibilitás érdekében. Mivel a lehető legnagyobb biztonságot nyújt hello toouse TLS 1.2-es ajánlott.

Az Azure IoT Suite titkosító csomag sorrendje a következő hello támogatja.

| Titkosítási csomagok | Hossza |
| --- | --- |
| A TLS\_ECDHE\_RSA\_WITH\_AES\_256\_CBC\_SHA384 (0xc028) ECDH secp384r1 (eq. FS 7680 bits RSA) |256 |
| A TLS\_ECDHE\_RSA\_WITH\_AES\_128\_CBC\_SHA-256 (0xc027) ECDH secp256r1 (eq. FS 3072 bits RSA) |128 |
| A TLS\_ECDHE\_RSA\_WITH\_AES\_256\_CBC\_SHA (0xc014) ECDH secp384r1 (eq. FS 7680 bits RSA) |256 |
| A TLS\_ECDHE\_RSA\_WITH\_AES\_128\_CBC\_SHA (0xc013) ECDH secp256r1 (eq. FS 3072 bits RSA) |128 |
| A TLS\_RSA\_WITH\_AES\_256\_GCM\_SHA384 (0x9d) |256 |
| A TLS\_RSA\_WITH\_AES\_128\_GCM\_SHA-256 (0x9c) |128 |
| A TLS\_RSA\_WITH\_AES\_256\_CBC\_SHA-256 (0x3d) |256 |
| A TLS\_RSA\_WITH\_AES\_128\_CBC\_SHA-256 (0x3c) |128 |
| A TLS\_RSA\_WITH\_AES\_256\_CBC\_SHA (0x35 hiba) |256 |
| A TLS\_RSA\_WITH\_AES\_128\_CBC\_SHA (0x2f) |128 |
| A TLS\_RSA\_WITH\_3DES\_EDE\_CBC\_SHA (0xa) |112 |

## <a name="securing-hello-cloud"></a>Hello felhő biztonságossá tétele
Az Azure IoT-központ lehetővé teszi, hogy a definíciója [hozzáférés-vezérlési házirendeket] [ lnk-protocols] minden egyes biztonsági kulcshoz. A következő engedélyek toogrant hozzáférés tooeach az IoT-központok végpontjai készlete hello használ. Engedélyek korlátozása hello hozzáférés tooan IoT-központ funkciókon alapulnak.

* **RegistryRead**. Olvasási hozzáférés toohello identitásjegyzékhez biztosít. További információkért lásd: [identitásjegyzékhez][lnk-identity-registry].
* **RegistryReadWrite**. Biztosít olvasási és írási hozzáférés toohello identitásjegyzékhez. További információkért lásd: [identitásjegyzékhez][lnk-identity-registry].
* **ServiceConnect**. A hozzáférési toocloud szolgáltatás irányuló kommunikációt és a végpontok ellenőrzésére. Például ad engedélyt tooback-a befejezési felhőalapú szolgáltatások tooreceive eszközről a felhőbe üzeneteket, és a felhő-eszközre küldött üzenetek küldése hello megfelelő kézbesítési visszaigazolások beolvasása.
* **DeviceConnect**. A hozzáférési toodevice felé néző végpontok. Például engedélyt ad toosend eszközről a felhőbe üzeneteket, és felhő-eszközre küldött üzenetek fogadására. Ez az engedély eszközöket használják.

Nincsenek a két módon tooobtain **DeviceConnect** IoT hubot engedélyeket [biztonsági jogkivonatokat][lnk-sas-tokens]: egy eszköz identitáskulcs, vagy egy megosztott elérési kulcsot. Ezenkívül fontos toonote eszközökhöz elérhető összes funkciót előtaggal rendelkező végpontokon szándékosan elérhetővé `/devices/{deviceId}`.

[Szolgáltatás-összetevők csak hozhat létre a biztonsági jogkivonatokat] [ lnk-service-tokens] használatával megosztott hozzáférési házirendek hello megfelelő jogosultságokat.

Azure IoT Hub és egyéb szolgáltatásokat, amelyek hello megoldás részét képezhetik engedélyezése a felhasználók a hello Azure Active Directory felügyeleti.

Azure IoT-központ által okozhatnak adatokat képes használni a számos olyan szolgáltatásokat, például Azure Stream Analytics és az Azure blob Storage tárolóban. Ezek a szolgáltatások felügyeleti hozzá lehessen férni. További tájékoztatást talál a szolgáltatások és a rendelkezésre álló lehetőségeket az alábbi:

* [Az Azure Cosmos DB][lnk-docdb]: egy méretezhető, teljes mértékben indexelt dokumentumadatbázis-szolgáltatás, amely kezeli a metaadatok hello eszközök félig strukturált adatok ellátásához, például az attribútumokat, a konfiguráció és a biztonsági tulajdonságait. Cosmos DB nagy teljesítményű és nagy átviteli feldolgozás, a séma-független indexelő adatokat, és egy részletes SQL-lekérdezési felületet kínál.
* [Az Azure Stream Analytics][lnk-asa]: valós idejű streamfeldolgozó hello felhőben, amely lehetővé teszi, hogy toorapidly fejlesztéséhez és központi telepítése egy alacsony költségű analytics megoldás toouncover valós idejű elemzése eszközök, érzékelőket, infrastruktúra, és az alkalmazások. a teljes körűen felügyelt szolgáltatás hello adatait is méretezhető tooany kötet megőrzése mellett nagy átviteli sebességet, alacsony késéssel és rugalmasság.
* [Az Azure App Services][lnk-appservices]: egy felhőalapú platform toobuild hatékony webes és mobilalkalmazások bárhol; toodata hello felhőalapú vagy helyszíni. Vonzó alkalmazások készítése iOS, Android és Windows rendszerre. Integrálható a szoftver szolgáltatott szoftverként (SaaS) és vállalati alkalmazások a felhő alapú szolgáltatások out-of-az-box kapcsolat toodozens és vállalati alkalmazások. A kedvenc nyelvi és IDE az (.NET, Node.js, PHP, Python vagy Java) toobuild webes alkalmazásokat és API-k minden eddiginél gyorsabban.
* [A Logic Apps][lnk-logicapps]: hello Logic Apps szolgáltatás az Azure App Service segítségével rendszerintegráció IoT megoldás tooyour meglévő üzleti és munkafolyamat-folyamatok automatizálása. A Logic Apps lehetővé teszi, hogy a fejlesztők toodesign munkafolyamatok, amelyek egy és majd végrehajtanak bizonyos lépéseket – szabályok és a műveletek, amelyeket az üzleti folyamatok hatékony összekötők toointegrate használhat. A Logic Apps kínál out-of-az-box kapcsolat tooa túlnyomó ökoszisztémájának alkalmazásával az SaaS, a felhő alapú, és a helyszíni alkalmazások.
* [Az Azure blob storage][lnk-blob]: hello adatok, az eszközök küldjön toohello felhő gazdaságos, megbízható felhőalapú tárolása.

## <a name="conclusion"></a>Összegzés
Ez a cikk áttekintést nyújt az megvalósítási tervezése és telepítése az IoT-infrastruktúrát használó Azure IoT szintű részleteit. Minden biztonságos összetevő toobe konfigurálása van a kulcs védelméhez hello általános IoT-infrastruktúra. hello tervezési döntések ütköznek azokkal elérhető Azure IoT biztosít bizonyos fokú rugalmasságot és a választott; minden kiválasztott azonban biztonsági hatásai lehetnek. Javasoljuk, hogy ezek mindegyikének lehet értékelni a kockázat/költsége értékelése.

## <a name="see-also"></a>Lásd még:
Akkor is is felfedezheti hello más szolgáltatásokat és képességeket hello előre konfigurált IoT Suite megoldások:

* [Előre konfigurált prediktív karbantartási megoldás áttekintése][lnk-predictive-overview]
* [Gyakran ismételt kérdések az IoT Suite-ról][lnk-faq]

Az IoT-központ biztonsági olvashat [vezérlő hozzáférés tooIoT Hub] [ lnk-devguide-security] az IoT Hub fejlesztői útmutató hello.


[img-overview]: media/iot-suite-security-deployment/overview.png

[lnk-security-tokens]: ../iot-hub/iot-hub-devguide-security.md#security-token-structure
[lnk-sas-tokens]: ../iot-hub/iot-hub-devguide-security.md#use-sas-tokens-in-a-device-app
[lnk-identity-registry]: ../iot-hub/iot-hub-devguide-identity-registry.md
[lnk-protocols]: ../iot-hub/iot-hub-devguide-security.md
[lnk-custom-auth]: ../iot-hub/iot-hub-devguide-security.md#custom-device-authentication
[lnk-x509]: http://www.itu.int/rec/T-REC-X.509-201210-I/en
[lnk-tls12]: https://tools.ietf.org/html/rfc5246
[lnk-service-tokens]: ../iot-hub/iot-hub-devguide-security.md#use-security-tokens-from-service-components
[lnk-docdb]: https://azure.microsoft.com/services/documentdb/
[lnk-asa]: https://azure.microsoft.com/services/stream-analytics/
[lnk-appservices]: https://azure.microsoft.com/services/app-service/
[lnk-logicapps]: https://azure.microsoft.com/services/app-service/logic/
[lnk-blob]: https://azure.microsoft.com/services/storage/

[lnk-predictive-overview]: iot-suite-predictive-overview.md
[lnk-faq]: iot-suite-faq.md
[lnk-devguide-security]: ../iot-hub/iot-hub-devguide-security.md
