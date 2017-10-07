---
title: "Azure IoT Hub biztonsági aaaUnderstand |} Microsoft Docs"
description: "Fejlesztői útmutató - hogyan férhetnek hozzá a toocontrol tooIoT Hub eszköz alkalmazások és a háttér-alkalmazásokat. Biztonsági jogkivonatok, és támogatja az x.509 szabványú tanúsítványokban kapcsolatos adatokat tartalmaz."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 45631e70-865b-4e06-bb1d-aae1175a52ba
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: dobett
ms.openlocfilehash: 717726328a6bb5c5c334a123d0abfed711b2c3b1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="control-access-tooiot-hub"></a>Központi hozzáférési tooIoT szabályozása

Ez a cikk ismerteti az IoT hub biztonságához hello-beállítások. Az IoT-központ használ *engedélyek* toogrant hozzáférés tooeach IoT-központ végpontjának. Engedélyek korlátozása hello hozzáférés tooan IoT hub funkciókon alapulnak.

Ez a cikk ismerteti:

* hello különböző engedélyeket, hogy is meg lehet adni tooa eszköz vagy a háttér-alkalmazás tooaccess az IoT hub.
* hello tooverify engedélyeket használ a hitelesítési folyamat és hello tokenek.
* Hogyan tooscope hitelesítő adatok toolimit toospecific-erőforrások eléréséhez.
* IoT Hub-támogatás az x.509 szabványú tanúsítványokban.
* Egyéni hitelesítési mechanizmusok, amelyek használják a meglévő eszköz identitása nyilvántartó vagy hitelesítési sémák.

### <a name="when-toouse"></a>Ha toouse

Szükséges megfelelő engedélyek tooaccess bármelyik hello IoT-központok végpontjai. Például egy eszköz tartalmaznia kell egy jogkivonatot tartalmazó biztonsági hitelesítő adatokat, és minden üzenetet küld tooIoT Hub.

## <a name="access-control-and-permissions"></a>Hozzáférés-vezérlés és engedélyek

Meg lehet adni [engedélyek](#iot-hub-permissions) a következő módokon hello:

* **Az IoT hub-szintű megosztott elérési házirendeket**. Megosztott elérési házirendeket is biztosítani bármilyen kombinációját [engedélyek](#iot-hub-permissions). Házirendek megadására a hello [Azure-portálon][lnk-management-portal], vagy programozott módon hello segítségével [IoT-központ erőforrás-szolgáltató REST API-k][lnk-resource-provider-apis]. Egy újonnan létrehozott IoT-központ rendelkezik a következő alapértelmezett házirendek hello:

  * **iothubowner**: házirend az összes engedélyt.
  * **szolgáltatás**: a házirend **ServiceConnect** engedéllyel.
  * **eszköz**: a házirend **DeviceConnect** engedéllyel.
  * **registryRead**: a házirend **RegistryRead** engedéllyel.
  * **registryReadWrite**: a házirend **RegistryRead** és RegistryWrite engedélyeket.
  * **Eszköz hitelesítő adatokat**. Minden egyes IoT-központ tartalmaz egy [identitásjegyzékhez][lnk-identity-registry]. A identitásjegyzékhez az eszközökhöz, konfigurálhatja a hitelesítő adatokat, amelyek biztosítani **DeviceConnect** engedélyek hatókörű toohello megfelelő eszköz végpontok.

Például a egy tipikus IoT-megoldás:

* hello eszköz felügyeleti összetevő használja hello *registryReadWrite* házirend.
* hello esemény feldolgozó összetevő használja hello *szolgáltatás* házirend.
* hello futásidejű eszköz üzleti logika egyik összetevője hello *szolgáltatás* házirend.
* Egyes eszközök hello IoT hub identitásjegyzékhez tárolt hitelesítő adatok keresztül csatlakoznak.

> [!NOTE]
> Lásd: [engedélyek](#iot-hub-permissions) részletes információkat.

## <a name="authentication"></a>Authentication

Azure IoT Hub hozzáférés tooendpoints ellenőrzésével egy token hello megosztott hozzáférési házirendek és identitás beállításjegyzék hitelesítő adatokat biztosít.

Biztonsági hitelesítő adatok, például a szimmetrikus kulcsokat, soha nem kerülnek hello hálózaton keresztül.

> [!NOTE]
> hello Azure IoT Hub erőforrás-szolgáltató védett keresztül az Azure-előfizetéshez, mert hello lévő összes szolgáltatónál [Azure Resource Manager][lnk-azure-resource-manager].

További információ tooconstruct és -felhasználási biztonsági jogkivonatokat, lásd: [IoT-központ biztonsági jogkivonatokat][lnk-sas-tokens].

### <a name="protocol-specifics"></a>Protokoll rögzítésen

Minden támogatott protokollok, például MQTT AMQP vagy HTTP, jogkivonatok szállításokkal különböző módon.

MQTT használatakor hello CONNECT csomag megegyezik hello deviceId hello ClientId, {iothubhostname} / {deviceId} hello felhasználónév mezőbe, és egy SAS-jogkivonat hello jelszó mezőben. {iothubhostname} hello IoT hub (például contoso.azure-devices.net) teljes CName hello kell lennie.

Használata esetén [AMQP][lnk-amqp], IoT-Központ támogatja [SASL egyszerű] [ lnk-sasl-plain] és [AMQP jogcím-alapú-biztonsági][lnk-cbs].

Ha AMQP jogcím-alapú-biztonsági használja, szabványos hello határozza meg hogy tootransmit ezeket a jogkivonatokat.

SASL egyszerű hello **felhasználónév** lehet:

* `{policyName}@sas.root.{iothubName}`Ha az IoT hub-szintű jogkivonatok használatával.
* `{deviceId}@sas.{iothubname}`Ha az eszköz hatókörű jogkivonatok használatával.

Mindkét esetben a jelszó mező hello hello jogkivonatot tartalmaz, a [IoT-központ biztonsági jogkivonatokat][lnk-sas-tokens].

HTTP megvalósítja a hitelesítési belefoglalja egy érvényes tokent hello **engedélyezési** kérelemfejlécet.

#### <a name="example"></a>Példa

Felhasználónév (DeviceId a kis-és nagybetűket):`iothubname.azure-devices.net/DeviceId`

Jelszó (készítése SAS-jogkivonatot az hello [eszköz explorer] [ lnk-device-explorer] eszköz):`SharedAccessSignature sr=iothubname.azure-devices.net%2fdevices%2fDeviceId&sig=kPszxZZZZZZZZZZZZZZZZZAhLT%2bV7o%3d&se=1487709501`

> [!NOTE]
> Hello [Azure IoT SDK-k] [ lnk-sdks] automatikusan hoz létre jogkivonatokat toohello szolgáltatáshoz való kapcsolódáskor. Bizonyos esetekben hello Azure IoT SDK-k nem támogatják az hello protokollok vagy hello hitelesítési módszerek mindegyikéhez.

### <a name="special-considerations-for-sasl-plain"></a>Különleges szempontok SASL egyszerű

Az AMQP SASL egyszerű használatakor ügyfél csatlakozik az IoT-központ tooan egyetlen jogkivonat a TCP-kapcsolatokat használhatja. Amikor hello-token érvényessége lejár, a hello TCP-kapcsolatot bontja a hello szolgáltatást, és újracsatlakozás elindítja. Ez a viselkedés, amíg nem jelent problémát, egy háttér-alkalmazás van káros hello a következő okok miatt az eszköz alkalmazások:

* Átjárók általában sok eszközök nevében csatlakozni. SASL egyszerű használatakor rendelkeznek toocreate az egyes eszközök csatlakoztatása az IoT-központ tooan különböző TCP-kapcsolatot. Ebben a forgatókönyvben jelentősen növeli a teljesítmény és a hálózati erőforrások fogyasztásának hello, és növeli a hello késés minden eszköz kapcsolat.
* Erőforrás-korlátozott eszközök vannak hátrányosan erőforrások tooreconnect nőtt hello használata minden egyes jogkivonat lejárata után.

## <a name="scope-iot-hub-level-credentials"></a>Hatókör IoT hub-szintű hitelesítő adatokat

Az IoT hub-szintű biztonsági házirendek hatókörét megadhatja a tokenek egy korlátozott erőforrás URI létrehozásával. Például a végpont toosend eszközről a felhőbe köszönőüzenetei egy eszközről van **/devices/ {deviceId} / üzenetek/események**. Használhatja az IoT hub-szintű megosztott elérési házirend **DeviceConnect** engedélyek toosign egy jogkivonatot, amelynek resourceURI van **/devices/ {deviceId}**. Ez a megközelítés létrehoz egy jogkivonatot, amely csak akkor használható toosend üzenetek eszköz nevében **deviceId**.

Ez a módszer használata hasonló toohello [Event Hubs közzétevői házirend][lnk-event-hubs-publisher-policy], és lehetővé teszi a tooimplement egyéni hitelesítési módszerek.

## <a name="security-tokens"></a>Biztonsági jogkivonatok kiosztva

Az IoT-központ által használt biztonsági jogkivonatok tooauthenticate eszközöket, és a szolgáltatások tooavoid kulcsok küldése hello keresztülhaladnak a hálózaton. Emellett biztonsági jogkivonatok érvényesség és hatókör korlátozott. [Azure IoT SDK-k] [ lnk-sdks] automatikusan hoz létre jogkivonatokat anélkül, hogy semmiféle speciális beállítást. Bizonyos esetekben kell toogenerate és biztonsági jogkivonatokat közvetlenül használható. Ilyen forgatókönyvek például a következők:

* hello hello MQTT, az AMQP vagy a HTTP-felületek közvetlen használatát.
* hello végrehajtásának hello biztonságijogkivonat-szolgáltatás a mintában a [egyéni eszközhitelesítés][lnk-custom-auth].

Az IoT-központ is lehetővé teszi, hogy eszközök tooauthenticate az IoT-központ [X.509-tanúsítványokat][lnk-x509].

### <a name="security-token-structure"></a>Biztonsági jogkivonat szerkezete

Biztonsági jogkivonatok toogrant idő határolt hozzáférés toodevices és a szolgáltatások toospecific a funkcióival IoT-központot. tooget engedélyezési tooconnect tooIoT Hub, eszközöket és szolgáltatásokat kell küldenie egy közös hozzáférésű vagy a szimmetrikus kulcs aláírva biztonsági jogkivonatokat. Ezek a kulcsok egy eszközidentitás hello identitás beállításjegyzék tárolja.

A token egy megosztott elérési kulcs biztosít hozzáférést tooall hello funkció hello megosztott hozzáférési házirend engedélyek társított aláírva. Egy eszközidentitás szimmetrikus kulcs csak biztosít hello aláírt jogkivonat **DeviceConnect** hello engedély társított eszközidentitás.

hello biztonsági jogkivonatának hello a következő formátumban:

`SharedAccessSignature sig={signature-string}&se={expiry}&skn={policyName}&sr={URL-encoded-resourceURI}`

Az alábbiakban hello várt értékek:

| Érték | Leírás |
| --- | --- |
| {aláírás} |Hello űrlap aláírás HMAC-SHA256 karakterláncra: `{URL-encoded-resourceURI} + "\n" + expiry`. **Fontos**: hello kulcsot dekódolni a Base64 kódolású anyag és kulcs tooperform hello HMAC-SHA256 számítási használja. |
| {resourceURI} |URI-előtag (által szegmens) hello végpontok a jogkivonatok hello IoT-központ (protokoll) állomásneve kezdődően elérhető. Például: `myHub.azure-devices.net/devices/device1` |
| {a lejárati} |UTF8 karakterláncok hello epoch 00:00:00 UTC 1970. január 1. a másodpercek száma. |
| {URL-kódolású-resourceURI} |Alacsonyabb eset az URL-kódolást hello kisbetű erőforrás URI |
| {Házirendnév} |Ez a token hivatkozik hello megosztott hozzáférési házirend toowhich hello neve. Hiányzik, ha hello token hivatkozik toodevice-beállításjegyzék hitelesítő adatokat. |

**Megjegyzés: előtag alapján**: hello URI-előtag számított szegmens és nem karakter. Például `/a/b` előtagja, az `/a/b/c` nem `/a/bc`.

hello Node.js alábbi kódrészletben láthatja a hívott függvény **generateSasToken** , hogy a hello bemenetei token kiszámítja hello `resourceUri, signingKey, policyName, expiresInMins`. hello következő részek ismertetik, hogyan tooinitialize hello különböző bemenetek hello különböző jogkivonat használati esetekben.

```nodejs
var generateSasToken = function(resourceUri, signingKey, policyName, expiresInMins) {
    resourceUri = encodeURIComponent(resourceUri);

    // Set expiration in seconds
    var expires = (Date.now() / 1000) + expiresInMins * 60;
    expires = Math.ceil(expires);
    var toSign = resourceUri + '\n' + expires;

    // Use crypto
    var hmac = crypto.createHmac('sha256', new Buffer(signingKey, 'base64'));
    hmac.update(toSign);
    var base64UriEncoded = encodeURIComponent(hmac.digest('base64'));

    // Construct autorization string
    var token = "SharedAccessSignature sr=" + resourceUri + "&sig="
    + base64UriEncoded + "&se=" + expires;
    if (policyName) token += "&skn="+policyName;
    return token;
};
```

Összehasonlítása, mint a biztonsági jogkivonat egyenértékű Python kódját toogenerate hello:

```python
from base64 import b64encode, b64decode
from hashlib import sha256
from time import time
from urllib import quote_plus, urlencode
from hmac import HMAC

def generate_sas_token(uri, key, policy_name, expiry=3600):
    ttl = time() + expiry
    sign_key = "%s\n%d" % ((quote_plus(uri)), int(ttl))
    print sign_key
    signature = b64encode(HMAC(b64decode(key), sign_key, sha256).digest())

    rawtoken = {
        'sr' :  uri,
        'sig': signature,
        'se' : str(int(ttl))
    }

    if policy_name is not None:
        rawtoken['skn'] = policy_name

    return 'SharedAccessSignature ' + urlencode(rawtoken)
```

> [!NOTE]
> Hello érvényesség hello jogkivonat IoT-központ gépeken van hitelesítve, mivel hello óra hello gép hello token generáló hello eltéréseket minimális kell lennie.

### <a name="use-sas-tokens-in-a-device-app"></a>Használja a SAS-tokenje eszköz alkalmazásban

Nincsenek két módon tooobtain **DeviceConnect** IoT-központ engedélyeket a biztonsági jogkivonatokat: használja egy [eszköz szimmetrikus kulcsot az hello identitásjegyzékhez](#use-a-symmetric-key-in-the-identity-registry), vagy használjon egy [megosztottelérésikulcsot](#use-a-shared-access-policy).

Ne feledje, hogy eszközökhöz elérhető összes funkciót tesz elérhetővé előtaggal rendelkező végpontokon tervezési `/devices/{deviceId}`.

> [!IMPORTANT]
> hello csak úgy, hogy az IoT-központ végez hitelesítést egy adott eszköz hello eszköz identitása szimmetrikus kulcsot használ. Azokban az esetekben, amikor egy megosztott elérési házirendet használt tooaccess eszköz funkciót, hello megoldás figyelembe kell vennie megbízható részletes hello biztonsági jogkivonat kiállító hello összetevő.

hello eszköz felé néző végpontok (függetlenül hello protocol) a következők:

| Végpont | Funkció |
| --- | --- |
| `{iot hub host name}/devices/{deviceId}/messages/events` |Eszköz-felhő üzeneteket küldeni. |
| `{iot hub host name}/devices/{deviceId}/devicebound` |Felhő eszközre üzeneteket fogadni. |

### <a name="use-a-symmetric-key-in-hello-identity-registry"></a>Hello identitásjegyzékhez a szimmetrikus kulcs használata

Ha egy eszközidentitás szimmetrikus kulcs toogenerate jogkivonatot, hello házirendnév (`skn`) elem hello jogkivonat nincs megadva.

Például, egy token létre tooaccess összes eszköz funkciót kell rendelkeznie hello a következő paramétereket:

* erőforrás URI: `{IoT hub name}.azure-devices.net/devices/{device id}`,
* aláírási kulcs: hello bármely szimmetrikus kulcs `{device id}` identitása
* Nincs házirend neve
* a lejárati idő.

Node.js-függvény megelőző hello segítségével például a következő lesz:

```nodejs
var endpoint ="myhub.azure-devices.net/devices/device1";
var deviceKey ="...";

var token = generateSasToken(endpoint, deviceKey, null, 60);
```

hello eredményt – amely device1 hozzáférés tooall funkciókat biztosít, a következő lesz:

`SharedAccessSignature sr=myhub.azure-devices.net%2fdevices%2fdevice1&sig=13y8ejUk2z7PLmvtwR5RqlGBOVwiq7rQR3WZ5xZX3N4%3D&se=1456971697`

> [!NOTE]
> Azt lehetséges toogenerate van egy SAS-jogkivonat használatával hello .NET [eszköz explorer] [ lnk-device-explorer] eszköz vagy platformok, csomópont-alapú hello [IOT hubbal-explorer] [ lnk-iothub-explorer] parancssori segédprogram.

### <a name="use-a-shared-access-policy"></a>Egy megosztott elérési házirendet használja

Amikor egy jogkivonatot hoz létre egy megosztott elérési házirendet, állítsa be a hello `skn` mező toohello hello házirend nevét. Ez a házirend biztosítania kell a hello **DeviceConnect** engedéllyel.

hello kétféle módon történhet a megosztott elérési házirendek tooaccess eszköz segítségével a következők:

* [a felhő protokoll átjárók][lnk-endpoints],
* [a token szolgáltatások] [ lnk-custom-auth] tooimplement egyéni hitelesítési sémák használt.

Hello óta megosztott elérési házirend potenciálisan biztosíthat hozzáférést tooconnect bármely eszközre, mint az fontos toouse hello helyes erőforrás URI biztonsági jogkivonatokat létrehozásakor. Ez a beállítás különösen fontos a token szolgáltatásokat, amelyek tooscope hello token tooa adott eszköz használatával hello erőforrás URI azonosítója. Ezt a pontot fontos kevesebb protokoll átjárók mivel azokat már van közvetítő forgalmat az összes eszköz.

Tegyük fel, előre létrehozott hello jogkivonat szolgáltatás megosztott hozzáférési házirend nevű **eszköz** jogkivonat hozna létre a következő paraméterek hello:

* erőforrás URI: `{IoT hub name}.azure-devices.net/devices/{device id}`,
* aláírási kulcs: hello hello kulcsokkal egyik `device` házirend,
* házirend neve: `device`,
* a lejárati idő.

Node.js-függvény megelőző hello segítségével például a következő lesz:

```nodejs
var endpoint ="myhub.azure-devices.net/devices/device1";
var policyName = 'device';
var policyKey = '...';

var token = generateSasToken(endpoint, policyKey, policyName, 60);
```

hello eredményt – amely device1 hozzáférés tooall funkciókat biztosít, a következő lesz:

`SharedAccessSignature sr=myhub.azure-devices.net%2fdevices%2fdevice1&sig=13y8ejUk2z7PLmvtwR5RqlGBOVwiq7rQR3WZ5xZX3N4%3D&se=1456971697&skn=device`

Protokoll-átjáró használhatja ugyanazt a token az összes eszköz egyszerűen hello erőforrás URI azonosítója túl hello`myhub.azure-devices.net/devices`.

### <a name="use-security-tokens-from-service-components"></a>Használja a biztonsági jogkivonatokat a szolgáltatás-összetevők

Szolgáltatás-összetevők csak hozhat létre a biztonsági jogkivonatokat hello megfelelő engedélyek megadását, amint azt korábban megosztott elérési házirendek segítségével.

Íme hello végpontokon kitett hello funkciói:

| Végpont | Funkció |
| --- | --- |
| `{iot hub host name}/devices` |Létrehozása, frissítése, lekérése és törlése eszköz identitások. |
| `{iot hub host name}/messages/events` |Eszköz-felhő üzeneteket fogadni. |
| `{iot hub host name}/servicebound/feedback` |A felhő-eszközre küldött üzenetek visszajelzést kap. |
| `{iot hub host name}/devicebound` |Felhő-eszközre küldött üzenetek küldése. |

Például egy előre létrehozott hello segítségével generálása szolgáltatás megosztott hozzáférési házirend nevű **registryRead** jogkivonat hozna létre a következő paraméterek hello:

* erőforrás URI: `{IoT hub name}.azure-devices.net/devices`,
* aláírási kulcs: hello hello kulcsokkal egyik `registryRead` házirend,
* házirend neve: `registryRead`,
* a lejárati idő.

```nodejs
var endpoint ="myhub.azure-devices.net/devices";
var policyName = 'device';
var policyKey = '...';

var token = generateSasToken(endpoint, policyKey, policyName, 60);
```

hello eredményt – amely biztosítanak hozzáférést tooread minden eszköz identitások, a következő lesz:

`SharedAccessSignature sr=myhub.azure-devices.net%2fdevices&sig=JdyscqTpXdEJs49elIUCcohw2DlFDR3zfH5KqGJo4r4%3D&se=1456973447&skn=registryRead`

## <a name="supported-x509-certificates"></a>Támogatott X.509-tanúsítványokat

Az IoT-központ bármely X.509 tanúsítvány tooauthenticate eszköz használható. A tanúsítványok tartalmaznak:

* **Meglévő X.509 tanúsítvány**. Előfordulhat, hogy egy eszköz már társítva X.509 tanúsítvány. hello eszköz használatakor a tanúsítvány tooauthenticate IoT-központ.
* **A önálló jön létre, és önaláírt tanúsítvány X-509**. A gyártó vagy a belső fejlesztésű deployer ezeket a tanúsítványokat létrehozni és és tárolására is hello megfelelő titkos kulcsnak (tanúsítvány) hello eszközön. Használhatja például a [OpenSSL] [ lnk-openssl] és [Windows SelfSignedCertificate] [ lnk-selfsigned] segédprogram erre a célra.
* **X.509 tanúsítvány hitelesítésszolgáltató által aláírt**. egy eszköz tooidentify és a hitelesítést az IoT-központ, egy X.509 tanúsítvány létrehozott és aláírt által hitelesítésszolgáltató (CA) is használható. Az IoT-központ csak ellenőrzi, hogy hello ujjlenyomat jelenik meg a konfigurált hello ujjlenyomatnak megfelelő. IOT hubbal nem felel meg a tanúsítványlánc hello.

Egy eszköz vagy használhat egy X.509 tanúsítvány vagy egy biztonsági jogkivonatot a hitelesítés, de soha sem egyszerre mindkettőre.

### <a name="register-an-x509-certificate-for-a-device"></a>Egy eszköz X.509 tanúsítvány regisztrálása

Hello [Azure IoT szolgáltatás SDK for C#] [ lnk-service-sdk] (verzió 1.0.8+) támogatja az olyan eszköz, amely a hitelesítéshez használja egy X.509 tanúsítvány regisztrálása. Például az importálási/exportálási az eszközök más API-k X.509-tanúsítványokat is támogatja.

### <a name="c-support"></a>C\# támogatása

Hello **RegistryManager** osztály biztosít egy programozott módon tooregister eszköz. Különösen hello **AddDeviceAsync** és **UpdateDeviceAsync** módszerek lehetővé teszik a tooregister, és frissítse egy eszközt az IoT-központ identitásjegyzékhez hello. Ez a két módszer igénybe vehet egy **eszköz** példány bemeneti adatként. Hello **eszköz** osztály tartalmaz egy **hitelesítési** , amely lehetővé teszi toospecify elsődleges és másodlagos X.509 tanúsítvány-ujjlenyomatok tulajdonság. hello ujjlenyomat hello X.509 tanúsítvány (kódolással bináris DER tárolása) egy SHA-1 kivonatoló jelöli. Lehetősége van hello egy elsődleges ujjlenyomatot vagy egy másodlagos ujjlenyomatot, vagy mindkettő megadására. Elsődleges és másodlagos ujjlenyomatok támogatott toohandle tanúsítvány helyettesítő forgatókönyv áll rendelkezésre.

> [!NOTE]
> Az IoT-központ nem kér és hello teljes X.509 tanúsítvány, csak hello ujjlenyomat tárolja.

Íme egy minta C\# code részlet tooregister egy eszközt egy X.509 tanúsítvány:

```csharp
var device = new Device(deviceId)
{
  Authentication = new AuthenticationMechanism()
  {
    X509Thumbprint = new X509Thumbprint()
    {
      PrimaryThumbprint = "921BC9694ADEB8929D4F7FE4B9A3A6DE58B0790B"
    }
  }
};
RegistryManager registryManager = RegistryManager.CreateFromConnectionString(deviceGatewayConnectionString);
await registryManager.AddDeviceAsync(device);
```

### <a name="use-an-x509-certificate-during-run-time-operations"></a>Egy X.509 tanúsítvány közbeni futásidejű műveletek

Hello [Azure IoT-eszközök SDK for .NET] [ lnk-client-sdk] (verzió 1.0.11+) támogatja hello X.509-tanúsítványokat.

### <a name="c-support"></a>C\# támogatása

osztály hello **DeviceAuthenticationWithX509Certificate** támogatja hello létrehozása **DeviceClient** példányok X.509 tanúsítvánnyal. hello X.509 tanúsítvány, amely tartalmazza a titkos kulcs hello hello (más néven PKCS #12) PFX formátumban kell lennie.

Íme egy minta kódrészletet:

```csharp
var authMethod = new DeviceAuthenticationWithX509Certificate("<device id>", x509Certificate);

var deviceClient = DeviceClient.Create("<IotHub DNS HostName>", authMethod);
```

## <a name="custom-device-authentication"></a>Egyéni eszközhitelesítés

Használhatja az IoT-központ hello [identitásjegyzékhez] [ lnk-identity-registry] tooconfigure eszközönkénti hitelesítő adatokat és a hozzáférés szabályozásához használatával [jogkivonatok] [ lnk-sas-tokens] . Ha egy IoT-megoldás már van egy egyéni identitás beállításjegyzék és/vagy hitelesítési séma, érdemes létrehozni egy *token service* toointegrate az infrastruktúra és az IoT-központ. Ily módon használhatja más IoT-szolgáltatásokat a megoldásban.

A jogkivonat-szolgáltatás egy olyan egyéni felhőalapú szolgáltatás. Az IoT-központ használ *megosztott hozzáférési házirend* rendelkező **DeviceConnect** engedélyek toocreate *eszköz hatókörű* jogkivonatokat. Ezeket a jogkivonatokat engedélyezése az eszköz tooconnect tooyour IoT-központot.

![Hello biztonságijogkivonat-szolgáltatás minta lépései][img-tokenservice]

Az alábbiakban hello fő lépésből hello biztonságijogkivonat-szolgáltatás minta:

1. Az IoT-központot, megosztott hozzáférési házirend létrehozása **DeviceConnect** az IoT hub engedélyeit. Ezt a házirendet hozhat létre a hello [Azure-portálon] [ lnk-management-portal] vagy programon keresztül. hello biztonságijogkivonat-szolgáltatás a házirend toosign hello jogkivonatokat hoz létre használ.
1. Ha egy eszköz tooaccess az IoT hub, aláírt jogkivonat kér a jogkivonat-szolgáltatás. hello eszközt, hogy a hello biztonságijogkivonat-szolgáltatás toocreate hello jogkivonatot használja az egyéni identitás rendszerleíró adatbázis-hitelesítési séma toodetermine hello eszköz identitással hitelesítheti.
1. hello biztonságijogkivonat-szolgáltatás egy jogkivonatot ad vissza. hello jogkivonat segítségével hozhatók létre `/devices/{deviceId}` , `resourceURI`, a `deviceId` hitelesített hello eszközként. hello biztonságijogkivonat-szolgáltatás hello megosztott hozzáférési házirend tooconstruct hello jogkivonatot használja.
1. hello eszköz hello token közvetlenül az IoT-központ hello használja.

> [!NOTE]
> Használhatja a .NET-osztályt hello [SharedAccessSignatureBuilder] [ lnk-dotnet-sas] vagy Java-osztály hello [IotHubServiceSasToken] [ lnk-java-sas] toocreate egy tokent a token szolgáltatásban.

hello biztonságijogkivonat-szolgáltatás hello jogkivonat lejáratáról állíthatja be, a kívánt módon működjenek. Amikor hello-token érvényessége lejár, a hello IoT-központ kiszolgálója hello eszköz kapcsolat. Ezt követően hello eszköz egy új jogkivonatot kell kérniük hello biztonságijogkivonat-szolgáltatás. Egy rövid lejárati időt hello eszköz és a hello biztonságijogkivonat-szolgáltatás hello terhelését növeli.

Eszköz tooconnect tooyour hub, továbbra is hozzá kell azt az IoT-központ identitásjegyzékhez toohello – annak ellenére, hogy a jogkivonatot, és nem egy eszköz fő tooconnect hello eszköz használja. Ezért, folytathatja a toouse eszköz hozzáférés-vezérlés engedélyezésével vagy letiltásával eszköz identitásokat a hello [identitásjegyzékhez][lnk-identity-registry]. Ez a megközelítés azzal csökkenti az hello kockázatok a tokenek használatára hosszú lejárati idő feltüntetésével.

### <a name="comparison-with-a-custom-gateway"></a>Egy egyéni átjáró összehasonlítása

hello biztonságijogkivonat-szolgáltatás a mintája, hello ajánlott módja tooimplement IoT-központ egyéni identitás rendszerleíró adatbázis-hitelesítési sémát. Ez a minta használata ajánlott, mivel az IoT-központ továbbra is toohandle legtöbb hello megoldás forgalom. Azonban ha hello egyéni hitelesítési séma így egymásba kapcsolódik, hello protokollal, szükség lehet egy *egyéni átjáró* tooprocess összes hello forgalmat. Ilyen eset például használ[Transport Layer Security (TLS) és előmegosztott kulccsal (PSKs)][lnk-tls-psk]. További információkért lásd: hello [protokoll-átjáró] [ lnk-protocols] témakör.

## <a name="reference-topics"></a>Referencia-témaköreit:

hello következő referencia-témakörökre biztosít ellenőrző hozzáférés tooyour IoT-központ további információt.

## <a name="iot-hub-permissions"></a>Az IoT-központ engedélyek

hello következő táblázatban hello engedélyek toocontrol hozzáférés tooyour IoT-központ is használhatja.

| Engedély | Megjegyzések |
| --- | --- |
| **RegistryRead** |Olvasási hozzáférés toohello identitásjegyzékhez biztosít. További információkért lásd: [identitásjegyzékhez][lnk-identity-registry]. <br/>Háttér-felhőszolgáltatások használja ezt az engedélyt. |
| **RegistryReadWrite** |Biztosít olvasási és írási hozzáférés toohello identitásjegyzékhez. További információkért lásd: [identitásjegyzékhez][lnk-identity-registry]. <br/>Háttér-felhőszolgáltatások használja ezt az engedélyt. |
| **ServiceConnect** |A hozzáférési toocloud szolgáltatás irányuló kommunikációt és a végpontok ellenőrzésére. <br/>Felhő-eszközre küldött üzenetek és a megfelelő kézbesítési visszaigazolások lekérése hello biztosít engedély tooreceive eszközről a felhőbe üzenetek küldése <br/>Biztosít engedély tooretrieve kézbesítési nyugtázás a fájl feltöltése. <br/>Biztosít engedély tooaccess eszköz twins tooupdate címkék kívánt tulajdonságokkal jelentett tulajdonságainak beolvasása és lekérdezéseket futtathat. <br/>Háttér-felhőszolgáltatások használja ezt az engedélyt. |
| **DeviceConnect** |A hozzáférési toodevice felé néző végpontok. <br/>Biztosít engedély toosend eszközről-a-felhőbe üzeneteket, és a felhő-eszközre küldött üzenetek fogadására. <br/>Biztosít engedély tooperform fájl feltöltése az eszközről. <br/>Biztosít engedély tooreceive eszköz szükséges iker tulajdonság értesítések és a frissítési eszköz iker jelentett tulajdonságok. <br/>Biztosít engedély tooperform fájlfeltöltések. <br/>Ez az engedély eszközöket használják. |

## <a name="additional-reference-material"></a>További referenciaanyag

Más hello IoT Hub fejlesztői útmutató hivatkozási témaköröket tartalmazza:

* [IoT-központok végpontjai] [ lnk-endpoints] ismerteti, hogy minden egyes IoT-központ elérhetővé teszi a futásidejű és felügyeleti műveletek különböző végpontok hello.
* [Sávszélesség-szabályozási és kvóták] [ lnk-quotas] hello kvóták ismerteti, és a szabályozás viselkedéseket, amelyek az IoT-központ szolgáltatás toohello alkalmazni.
* [Az Azure IoT eszköz és a szolgáltatás SDK-k] [ lnk-sdks] listák hello különböző nyelvi használhatja az eszköz és a szolgáltatás alkalmazások gondoskodnak az IoT hubbal fejlesztésekor SDK-k.
* [Az IoT-központ lekérdezési nyelv] [ lnk-query] hello lekérdezési nyelv használhatja tooretrieve IoT Hub-ből származó adataival a eszköz twins és a feladatokat ismerteti.
* [Az IoT Hub MQTT támogatási] [ lnk-devguide-mqtt] hello MQTT protokoll IoT-központ támogatásával kapcsolatos további információkat biztosít.

## <a name="next-steps"></a>Következő lépések

Most megtanulta, hogyan férnek hozzá a toocontrol az IoT-központot, esetleg érdekli hello IoT Hub fejlesztői útmutató témakörei a következő:

* [Az eszköz twins toosynchronize állapotát és konfigurációkat használják][lnk-devguide-device-twins]
* [Az eszközön közvetlen metódus][lnk-devguide-directmethods]
* [Több eszközön feladatok ütemezése][lnk-devguide-jobs]

Ha szeretné tootry meg néhány ebben a cikkben ismertetett hello fogalmakat, esetleg az IoT-központ oktatóanyagok következő hello iránt érdeklődik:

* [Ismerkedés az Azure IoT Hub][lnk-getstarted-tutorial]
* [Hogyan toosend felhőből eszközre küldött üzenetek IoT-központ][lnk-c2d-tutorial]
* [Hogyan tooprocess IoT-központ eszközről a felhőbe üzenetek][lnk-d2c-tutorial]

<!-- links and images -->

[img-tokenservice]: ./media/iot-hub-devguide-security/tokenservice.png
[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-openssl]: https://www.openssl.org/
[lnk-selfsigned]: https://technet.microsoft.com/library/hh848633

[lnk-resource-provider-apis]: https://docs.microsoft.com/rest/api/iothub/iothubresource
[lnk-sas-tokens]: iot-hub-devguide-security.md#security-tokens
[lnk-amqp]: https://www.amqp.org/
[lnk-azure-resource-manager]: ../azure-resource-manager/resource-group-overview.md
[lnk-cbs]: https://www.oasis-open.org/committees/download.php/50506/amqp-cbs-v1%200-wd02%202013-08-12.doc
[lnk-event-hubs-publisher-policy]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-99ce67ab
[lnk-management-portal]: https://portal.azure.com
[lnk-sasl-plain]: http://tools.ietf.org/html/rfc4616
[lnk-identity-registry]: iot-hub-devguide-identity-registry.md
[lnk-dotnet-sas]: https://msdn.microsoft.com/library/microsoft.azure.devices.common.security.sharedaccesssignaturebuilder.aspx
[lnk-java-sas]: https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.service.auth._iot_hub_service_sas_token
[lnk-tls-psk]: https://tools.ietf.org/html/rfc4279
[lnk-protocols]: iot-hub-protocol-gateway.md
[lnk-custom-auth]: iot-hub-devguide-security.md#custom-device-authentication
[lnk-x509]: iot-hub-devguide-security.md#supported-x509-certificates
[lnk-devguide-device-twins]: iot-hub-devguide-device-twins.md
[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-service-sdk]: https://github.com/Azure/azure-iot-sdk-csharp/tree/master/service
[lnk-client-sdk]: https://github.com/Azure/azure-iot-sdk-csharp/tree/master/device
[lnk-device-explorer]: https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer
[lnk-iothub-explorer]: https://github.com/azure/iothub-explorer

[lnk-getstarted-tutorial]: iot-hub-csharp-csharp-getstarted.md
[lnk-c2d-tutorial]: iot-hub-csharp-csharp-c2d.md
[lnk-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md
