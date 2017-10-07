---
title: a Notification Hubs Python aaaHow toouse
description: "Megtudhatja, hogyan toouse Azure Notification Hubs Python háttér-a."
services: notification-hubs
documentationcenter: 
author: ysxu
manager: erikre
editor: 
ms.assetid: 5640dd4a-a91e-4aa0-a833-93615bde49b4
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: python
ms.devlang: php
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: 21d5aaf7fc24c9936fac8e0a8de640c66c51ab0a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-notification-hubs-from-python"></a>Hogyan toouse Notification Hubs Python
[!INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]

Java/PHP/Python vagy Ruby háttér-összes értesítési központok szolgáltatás érhető el hello Notification Hub REST interfész használatával hello MSDN-témakörben leírtak szerint [Notification hub REST API-k](http://msdn.microsoft.com/library/dn223264.aspx).

> [!NOTE]
> Ez a minta hivatkozás megvalósítása megvalósításának hello értesítést küld a Python és nem hello hivatalosan értesítések Hub Python SDK támogatott.
> 
> Ez a minta a Python 3.4 leírva.
> 
> 

Ebben a témakörben megmutatjuk, hogyan:

* A Notification Hubs-szolgáltatások a Python egy REST-ügyfél felépítéséhez.
* Hello Python felület toohello Notification Hub REST API-k használatával értesítések küldéséhez. 
* Első hello HTTP REST kérelem-válasz egy kiírást hibakeresés/oktatási célra. 

Hajtsa végre az hello [Get bemutató](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) választott, Python hello háttér-részét valósít meg a mobil platformra.

> [!NOTE]
> hello hello minta hatóköre csak korlátozott toosend értesítéseket, és minden olyan regisztrációs felügyeleti nem.
> 
> 

## <a name="client-interface"></a>Ügyféloldali felület
fő hello felületet biztosít hello ugyanazokat a módszereket, amelyek hello elérhető [.NET Notification Hubs SDK](http://msdn.microsoft.com/library/jj933431.aspx). Ez lehetővé teszi toodirectly állomásneveket, az összes hello oktatóanyagok és ezen a helyen aktuálisan elérhető mintákat és hello hello Közösség által közzétett internetes.

Az összes elérhető hello hello kód található [Python REST burkoló minta].

Például toocreate ügyfél:

    isDebug = True
    hub = NotificationHub("myConnectionString", "myNotificationHubName", isDebug)

a Windows toosend poharad értesítés:

    wns_payload = """<toast><visual><binding template=\"ToastText01\"><text id=\"1\">Hello world!</text></binding></visual></toast>"""
    hub.send_windows_notification(wns_payload)

## <a name="implementation"></a>Megvalósítás
Ha még nem tette, kövesse a [Get bemutató] akár toohello utolsó szakasza tooimplement hello háttér-esetében.

Összes hello teljes REST-burkoló található Részletek tooimplement [MSDN](http://msdn.microsoft.com/library/dn530746.aspx). Ez a szakasz a rendszer hello fő lépésből szükséges tooaccess Notification hub REST-végpontok hello Python végrehajtását ismertetik és értesítések küldéséhez

1. Hello kapcsolati karakterlánc elemzése
2. Hello engedélyezési jogkivonat létrehozása
3. A HTTP REST API használatával értesítést küldeni

### <a name="parse-hello-connection-string"></a>Hello kapcsolati karakterlánc elemzése
Íme hello hello ügyfél, amelynek konstruktor elemez hello kapcsolati karakterlánc végrehajtási fő osztályban:

    class NotificationHub:
        API_VERSION = "?api-version=2013-10"
        DEBUG_SEND = "&test"

        def __init__(self, connection_string=None, hub_name=None, debug=0):
            self.HubName = hub_name
            self.Debug = debug

            # Parse connection string
            parts = connection_string.split(';')
            if len(parts) != 3:
                raise Exception("Invalid ConnectionString.")

            for part in parts:
                if part.startswith('Endpoint'):
                    self.Endpoint = 'https' + part[11:]
                if part.startswith('SharedAccessKeyName'):
                    self.SasKeyName = part[20:]
                if part.startswith('SharedAccessKey'):
                    self.SasKeyValue = part[16:]


### <a name="create-security-token"></a>Biztonsági jogkivonat létrehozása
hello biztonsági jogkivonat-létrehozási hello részletei érhetők el [Itt](http://msdn.microsoft.com/library/dn495627.aspx).
hello következőkkel rendelkezik hozzáadott toobe toohello **NotificationHub** osztály toocreate hello token hello URI hello aktuális kérelem és hello kapcsolati karakterlánc kinyert hello hitelesítő adatok alapján.

    @staticmethod
    def get_expiry():
        # By default returns an expiration of 5 minutes (=300 seconds) from now
        return int(round(time.time() + 300))

    @staticmethod
    def encode_base64(data):
        return base64.b64encode(data)

    def sign_string(self, to_sign):
        key = self.SasKeyValue.encode('utf-8')
        to_sign = to_sign.encode('utf-8')
        signed_hmac_sha256 = hmac.HMAC(key, to_sign, hashlib.sha256)
        digest = signed_hmac_sha256.digest()
        encoded_digest = self.encode_base64(digest)
        return encoded_digest

    def generate_sas_token(self):
        target_uri = self.Endpoint + self.HubName
        my_uri = urllib.parse.quote(target_uri, '').lower()
        expiry = str(self.get_expiry())
        to_sign = my_uri + '\n' + expiry
        signature = urllib.parse.quote(self.sign_string(to_sign))
        auth_format = 'SharedAccessSignature sig={0}&se={1}&skn={2}&sr={3}'
        sas_token = auth_format.format(signature, expiry, self.SasKeyName, my_uri)
        return sas_token

### <a name="send-a-notification-using-http-rest-api"></a>A HTTP REST API használatával értesítést küldeni
Először is, hogy használja értesítést képviselő osztályt határozza meg.

    class Notification:
        def __init__(self, notification_format=None, payload=None, debug=0):
            valid_formats = ['template', 'apple', 'gcm', 'windows', 'windowsphone', "adm", "baidu"]
            if not any(x in notification_format for x in valid_formats):
                raise Exception(
                    "Invalid Notification format. " +
                    "Must be one of hello following - 'template', 'apple', 'gcm', 'windows', 'windowsphone', 'adm', 'baidu'")

            self.format = notification_format
            self.payload = payload

            # array with keynames for headers
            # Note: Some headers are mandatory: Windows: X-WNS-Type, WindowsPhone: X-NotificationType
            # Note: For Apple you can set Expiry with header: ServiceBusNotification-ApnsExpiry
            # in W3C DTF, YYYY-MM-DDThh:mmTZD (for example, 1997-07-16T19:20+01:00).
            self.headers = None

Ez az osztály egy natív értesítési vagy sablon értesítést, a fejléc formátuma (natív platform vagy sablon) és a platform-specifikus tulajdonságok (például az Apple lejárati tulajdonság és WNS fejlécekkel együtt) tartalmazó készlettel esetén tulajdonságait tárolója .

Tekintse meg a toohello [Notification hub REST API-k dokumentáció](http://msdn.microsoft.com/library/dn495827.aspx) és hello adott értesítési platformok formátumok, az összes rendelkezésre álló lehetőségek hello.

Most már ehhez az osztályhoz, azt írhat hello send notification módszerek hello belül **NotificationHub** osztály.

    def make_http_request(self, url, payload, headers):
        parsed_url = urllib.parse.urlparse(url)
        connection = http.client.HTTPSConnection(parsed_url.hostname, parsed_url.port)

        if self.Debug > 0:
            connection.set_debuglevel(self.Debug)
            # adding this querystring parameter gets detailed information about hello PNS send notification outcome
            url += self.DEBUG_SEND
            print("--- REQUEST ---")
            print("URI: " + url)
            print("Headers: " + json.dumps(headers, sort_keys=True, indent=4, separators=(' ', ': ')))
            print("--- END REQUEST ---\n")

        connection.request('POST', url, payload, headers)
        response = connection.getresponse()

        if self.Debug > 0:
            # print out detailed response information for debugging purpose
            print("\n\n--- RESPONSE ---")
            print(str(response.status) + " " + response.reason)
            print(response.msg)
            print(response.read())
            print("--- END RESPONSE ---")

        elif response.status != 201:
            # Successful outcome of send message is HTTP 201 - Created
            raise Exception(
                "Error sending notification. Received HTTP code " + str(response.status) + " " + response.reason)

        connection.close()

    def send_notification(self, notification, tag_or_tag_expression=None):
        url = self.Endpoint + self.HubName + '/messages' + self.API_VERSION

        json_platforms = ['template', 'apple', 'gcm', 'adm', 'baidu']

        if any(x in notification.format for x in json_platforms):
            content_type = "application/json"
            payload_to_send = json.dumps(notification.payload)
        else:
            content_type = "application/xml"
            payload_to_send = notification.payload

        headers = {
            'Content-type': content_type,
            'Authorization': self.generate_sas_token(),
            'ServiceBusNotification-Format': notification.format
        }

        if isinstance(tag_or_tag_expression, set):
            tag_list = ' || '.join(tag_or_tag_expression)
        else:
            tag_list = tag_or_tag_expression

        # add hello tags/tag expressions toohello headers collection
        if tag_list != "":
            headers.update({'ServiceBusNotification-Tags': tag_list})

        # add any custom headers toohello headers collection that hello user may have added
        if notification.headers is not None:
            headers.update(notification.headers)

        self.make_http_request(url, payload_to_send, headers)

    def send_apple_notification(self, payload, tags=""):
        nh = Notification("apple", payload)
        self.send_notification(nh, tags)

    def send_gcm_notification(self, payload, tags=""):
        nh = Notification("gcm", payload)
        self.send_notification(nh, tags)

    def send_adm_notification(self, payload, tags=""):
        nh = Notification("adm", payload)
        self.send_notification(nh, tags)

    def send_baidu_notification(self, payload, tags=""):
        nh = Notification("baidu", payload)
        self.send_notification(nh, tags)

    def send_mpns_notification(self, payload, tags=""):
        nh = Notification("windowsphone", payload)

        if "<wp:Toast>" in payload:
            nh.headers = {'X-WindowsPhone-Target': 'toast', 'X-NotificationClass': '2'}
        elif "<wp:Tile>" in payload:
            nh.headers = {'X-WindowsPhone-Target': 'tile', 'X-NotificationClass': '1'}

        self.send_notification(nh, tags)

    def send_windows_notification(self, payload, tags=""):
        nh = Notification("windows", payload)

        if "<toast>" in payload:
            nh.headers = {'X-WNS-Type': 'wns/toast'}
        elif "<tile>" in payload:
            nh.headers = {'X-WNS-Type': 'wns/tile'}
        elif "<badge>" in payload:
            nh.headers = {'X-WNS-Type': 'wns/badge'}

        self.send_notification(nh, tags)

    def send_template_notification(self, properties, tags=""):
        nh = Notification("template", properties)
        self.send_notification(nh, tags)

hello módszerek fent egy HTTP POST kérelem toohello /messages végpontot az értesítési központ hello megfelelő szervezet és a fejlécek toosend hello értesítés küldése.

### <a name="using-debug-property-tooenable-detailed-logging"></a>Hibakeresési tulajdonság tooenable használatával részletes naplózás
Értesítési központ hello inicializálása során hibakeresési tulajdonság engedélyezése fog kiírni, hello HTTP részletes naplózás információt kérés és válasz memóriakép, valamint részletes értesítési üzenetet küldeni az eredménye. Ez a tulajdonság neve nemrégiben hozzáadott [Notification Hubs TestSend tulajdonság](http://msdn.microsoft.com/library/microsoft.servicebus.notifications.notificationhubclient.enabletestsend.aspx) hello értesítés küldése kimenetelét részletes információt ad vissza, amely. toouse azt - hello alábbi inicializálása:

    hub = NotificationHub("myConnectionString", "myNotificationHubName", isDebug)

hello Notification Hub küldési kérelem HTTP URL-cím beolvasása kiegészül a "test" lekérdezési karakterlánc eredményeképpen. 

## <a name="complete-tutorial"></a>Teljes hello oktatóanyag
Most már a Python háttér-hello értesítés küldésével hello első lépéseket bemutató oktatóanyaghoz hajthatja végre.

A Notification Hubs-ügyfél inicializálása (hello kapcsolati karakterláncot és a központ neve helyettesítő hello útmutatását [Get bemutató]):

    hub = NotificationHub("myConnectionString", "myNotificationHubName")

Majd adja hozzá a hello küldési kódot attól függően, hogy a célként megadott mobilplatformot. Ez a minta is hozzáad a magasabb szintű módszerek tooenable hello platform pl. Windows; send_windows_notification alapján értesítések küldése (az apple) send_apple_notification stb. 

### <a name="windows-store-and-windows-phone-81-non-silverlight"></a>Windows áruház és Windows Phone 8.1 (nem Silverlight)
    wns_payload = """<toast><visual><binding template=\"ToastText01\"><text id=\"1\">Test</text></binding></visual></toast>"""
    hub.send_windows_notification(wns_payload)

### <a name="windows-phone-80-and-81-silverlight"></a>Windows Phone 8.0-s és 8.1 Silverlight
    hub.send_mpns_notification(toast)

### <a name="ios"></a>iOS
    alert_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_apple_notification(alert_payload)

### <a name="android"></a>Android
    gcm_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_gcm_notification(gcm_payload)

### <a name="kindle-fire"></a>Kindle Fire
    adm_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_adm_notification(adm_payload)

### <a name="baidu"></a>Baidu
    baidu_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_baidu_notification(baidu_payload)

A Python kódja egy értesítés jelenik meg a céleszközön kell előállítania.

## <a name="examples"></a>Példák:
### <a name="enabling-debug-property"></a>Debug tulajdonság engedélyezése
Amikor engedélyezi a hibakeresési jelző hello NotificationHub inicializálásakor, látni fogja, majd részletes HTTP-kérelem-válasz memóriakép, valamint a NotificationOutcome hasonló hello ahol megismerheti, milyen HTTP-fejlécek átadott hello kérelemben és milyen HTTP Értesítési központ hello érkezett válasz:![][1]

Látni fogja, pl. részletes értesítési központ eredménye 

* Ha hello sikeresen üzenettel toohello leküldéses értesítéseket kezelő szolgáltatásában. 
  
        <Outcome>hello Notification was successfully sent toohello Push Notification System</Outcome>
* Ha voltak célok megadva a leküldéses értesítésekhez található, akkor valószínűleg fog toosee hello következő hello választ (amely azt jelzi, hogy történtek-e toodeliver hello értesítési valószínűleg található, mert a hello regisztrációk volt néhány regisztrációt nem megfelelő címkékkel)
  
        '<NotificationOutcome xmlns="http://schemas.microsoft.com/netservices/2010/10/servicebus/connect" xmlns:i="http://www.w3.org/2001/XMLSchema-instance"><Success>0</Success><Failure>0</Failure><Results i:nil="true"/></NotificationOutcome>'

### <a name="broadcast-toast-notification-toowindows"></a>Bejelentési értesítés tooWindows szórási
Figyelje meg hello fejlécek beolvasása kiküldött tooWindows ügyfél szórási bejelentési értesítés küldéséhez. 

    hub.send_windows_notification(wns_payload)

![][2]

### <a name="send-notification-specifying-a-tag-or-tag-expression"></a>Egy tag (vagy egy címke kifejezés) megadása értesítés küldése
Értesítés hello címkék HTTP-fejléc lekérdezi hozzáadott toohello HTTP-kérelem (hello az alábbi példában azt küldenek hello értesítési csak tooregistrations "Sport" hasznos)

    hub.send_windows_notification(wns_payload, "sports")

![][3]

### <a name="send-notification-specifying-multiple-tags"></a>Több címkék megadásával értesítés küldése
Figyelje meg, hogyan hello címkék HTTP-fejléc változik, ha több címke küld. 

    tags = {'sports', 'politics'}
    hub.send_windows_notification(wns_payload, tags)

![][4]

### <a name="templated-notification"></a>Értesítési sablon
Figyelje meg, hogy hello formátum HTTP-fejléc módosításokat, és hasznos törzs hello hello HTTP-kérés törzsében részeként zajlik:

**Ügyféloldali - regisztrált sablon**

        var template =
                        @"<toast><visual><binding template=""ToastText01""><text id=""1"">$(greeting_en)</text></binding></visual></toast>";

**Kiszolgálóoldali - hello tartalom küldése**

        template_payload = {'greeting_en': 'Hello', 'greeting_fr': 'Salut'}
        hub.send_template_notification(template_payload)

![][5]

## <a name="next-steps"></a>Következő lépések
Ebben a témakörben azt bemutatta, hogyan toocreate egy egyszerű Python REST-a Notification Hubs-ügyfél. Itt a következőket teheti:

* Teljes hello letöltése [Python REST burkoló minta], amely tartalmazza a fenti hello kódot.
* Folytathatja az értesítési központok szolgáltatás hello szerinti címkézését [Megtörje hírek oktatóanyag]
* Folytathatja az értesítési központok sablonok funkció a hello [azaz hírek oktatóanyag]

<!-- URLs -->
[Python REST burkoló minta]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/notificationhubs-rest-python
[Get bemutató]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-get-started/
[Megtörje hírek oktatóanyag]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-send-breaking-news/
[azaz hírek oktatóanyag]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-send-localized-breaking-news/

<!-- Images. -->
[1]: ./media/notification-hubs-python-backend-how-to/DetailedLoggingInfo.png
[2]: ./media/notification-hubs-python-backend-how-to/BroadcastScenario.png
[3]: ./media/notification-hubs-python-backend-how-to/SendWithOneTag.png
[4]: ./media/notification-hubs-python-backend-how-to/SendWithMultipleTags.png
[5]: ./media/notification-hubs-python-backend-how-to/TemplatedNotification.png

