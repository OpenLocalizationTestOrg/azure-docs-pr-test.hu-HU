---
title: "A Notification Hubs használata Python"
description: "Ismerje meg, hogy a Python háttér-Azure Notification Hubs használatával."
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
ms.openlocfilehash: 9ceedb9940759427fc8cec74a1307e42472563a6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-notification-hubs-from-python"></a><span data-ttu-id="16616-103">Notification Hubs Python használatával</span><span class="sxs-lookup"><span data-stu-id="16616-103">How to use Notification Hubs from Python</span></span>
[!INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]

<span data-ttu-id="16616-104">A Java/PHP/Python vagy Ruby-háttér használata a Notification Hub REST interfész, az MSDN-témakörben leírtak szerint minden értesítési központok szolgáltatás érhető el [Notification hub REST API-k](http://msdn.microsoft.com/library/dn223264.aspx).</span><span class="sxs-lookup"><span data-stu-id="16616-104">You can access all Notification Hubs features from a Java/PHP/Python/Ruby back-end using the Notification Hub REST interface as described in the MSDN topic [Notification Hubs REST APIs](http://msdn.microsoft.com/library/dn223264.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="16616-105">Ez a minta hivatkozás megvalósítása megvalósításához a értesítést küld a Python és nem a hivatalosan támogatott értesítések Hub Python SDK.</span><span class="sxs-lookup"><span data-stu-id="16616-105">This is a sample reference implementation for implementing the notification sends in Python and is not the officially supported Notifications Hub Python SDK.</span></span>
> 
> <span data-ttu-id="16616-106">Ez a minta a Python 3.4 leírva.</span><span class="sxs-lookup"><span data-stu-id="16616-106">This sample is written using Python 3.4.</span></span>
> 
> 

<span data-ttu-id="16616-107">Ebben a témakörben megmutatjuk, hogyan:</span><span class="sxs-lookup"><span data-stu-id="16616-107">In this topic we show how to:</span></span>

* <span data-ttu-id="16616-108">A Notification Hubs-szolgáltatások a Python egy REST-ügyfél felépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="16616-108">Build a REST client for Notification Hubs features in Python.</span></span>
* <span data-ttu-id="16616-109">A Python felületén a Notification Hub REST API-k értesítések küldéséhez.</span><span class="sxs-lookup"><span data-stu-id="16616-109">Send notifications using the Python interface to the Notification Hub REST APIs.</span></span> 
* <span data-ttu-id="16616-110">Adja a HTTP REST-kérelem/válasz lekérése hibakeresés/oktatási célra.</span><span class="sxs-lookup"><span data-stu-id="16616-110">Get a dump of the HTTP REST request/response for debugging/educational purpose.</span></span> 

<span data-ttu-id="16616-111">Kövesse a [Get bemutató](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) választás a mobil platform megvalósításához a háttér-rész a Python.</span><span class="sxs-lookup"><span data-stu-id="16616-111">You can follow the [Get started tutorial](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) for your mobile platform of choice, implementing the back-end portion in Python.</span></span>

> [!NOTE]
> <span data-ttu-id="16616-112">A minta a hatókör csak korlátozott mértékben értesítések küldéséhez, és minden olyan regisztrációs felügyeleti nem.</span><span class="sxs-lookup"><span data-stu-id="16616-112">The scope of the sample is only limited to send notifications and it doesn't do any registration management.</span></span>
> 
> 

## <a name="client-interface"></a><span data-ttu-id="16616-113">Ügyféloldali felület</span><span class="sxs-lookup"><span data-stu-id="16616-113">Client interface</span></span>
<span data-ttu-id="16616-114">A fő ügyféloldali felületen elérhető ugyanazokat a módszereket biztosít a [.NET Notification Hubs SDK](http://msdn.microsoft.com/library/jj933431.aspx).</span><span class="sxs-lookup"><span data-stu-id="16616-114">The main client interface can provide the same methods that are available in the [.NET Notification Hubs SDK](http://msdn.microsoft.com/library/jj933431.aspx).</span></span> <span data-ttu-id="16616-115">Ez lehetővé teszi az oktatóanyagok és ezen a helyen aktuálisan elérhető mintákat közvetlenül fordítása, és az interneten a Közösség által közzétett.</span><span class="sxs-lookup"><span data-stu-id="16616-115">This will allow you to directly translate all the tutorials and samples currently available on this site, and contributed by the community on the internet.</span></span>

<span data-ttu-id="16616-116">Az összes elérhető kód megtalálhatja a [Python REST burkoló minta].</span><span class="sxs-lookup"><span data-stu-id="16616-116">You can find all the code available in the [Python REST wrapper sample].</span></span>

<span data-ttu-id="16616-117">Ha például egy ügyfél létrehozásához:</span><span class="sxs-lookup"><span data-stu-id="16616-117">For example, to create a client:</span></span>

    isDebug = True
    hub = NotificationHub("myConnectionString", "myNotificationHubName", isDebug)

<span data-ttu-id="16616-118">Windows bejelentési értesítés küldése:</span><span class="sxs-lookup"><span data-stu-id="16616-118">To send a Windows toast notification:</span></span>

    wns_payload = """<toast><visual><binding template=\"ToastText01\"><text id=\"1\">Hello world!</text></binding></visual></toast>"""
    hub.send_windows_notification(wns_payload)

## <a name="implementation"></a><span data-ttu-id="16616-119">Megvalósítás</span><span class="sxs-lookup"><span data-stu-id="16616-119">Implementation</span></span>
<span data-ttu-id="16616-120">Ha még nem tette, kövesse a [Get bemutató] akár utolsó szakasz esetében a háttér-megvalósításához.</span><span class="sxs-lookup"><span data-stu-id="16616-120">If you did not already, please follow our [Get started tutorial] up to the last section where you have to implement the back-end.</span></span>

<span data-ttu-id="16616-121">Egy teljes REST-burkoló megvalósításához a részleteit található [MSDN](http://msdn.microsoft.com/library/dn530746.aspx).</span><span class="sxs-lookup"><span data-stu-id="16616-121">All the details to implement a full REST wrapper can be found on [MSDN](http://msdn.microsoft.com/library/dn530746.aspx).</span></span> <span data-ttu-id="16616-122">Ebben a szakaszban azt ismerteti, a fő Notification hub REST-végpontok elérését, és értesítések küldéséhez szükséges lépéseket a Python végrehajtása</span><span class="sxs-lookup"><span data-stu-id="16616-122">In this section we will describe the Python implementation of the main steps required to access Notification Hubs REST endpoints and send notifications</span></span>

1. <span data-ttu-id="16616-123">Kapcsolati karakterlánc elemzése</span><span class="sxs-lookup"><span data-stu-id="16616-123">Parse the connection string</span></span>
2. <span data-ttu-id="16616-124">Az engedélyezési jogkivonat létrehozása</span><span class="sxs-lookup"><span data-stu-id="16616-124">Generate the authorization token</span></span>
3. <span data-ttu-id="16616-125">A HTTP REST API használatával értesítést küldeni</span><span class="sxs-lookup"><span data-stu-id="16616-125">Send a notification using HTTP REST API</span></span>

### <a name="parse-the-connection-string"></a><span data-ttu-id="16616-126">Kapcsolati karakterlánc elemzése</span><span class="sxs-lookup"><span data-stu-id="16616-126">Parse the connection string</span></span>
<span data-ttu-id="16616-127">Az ügyfél, amelynek konstruktor elemzi a kapcsolati karakterlánc végrehajtási fő osztály itt található:</span><span class="sxs-lookup"><span data-stu-id="16616-127">Here is the main class implementing the client, whose constructor parses the connection string:</span></span>

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


### <a name="create-security-token"></a><span data-ttu-id="16616-128">Biztonsági jogkivonat létrehozása</span><span class="sxs-lookup"><span data-stu-id="16616-128">Create security token</span></span>
<span data-ttu-id="16616-129">A részleteket a biztonsági jogkivonat-létrehozási [Itt](http://msdn.microsoft.com/library/dn495627.aspx).</span><span class="sxs-lookup"><span data-stu-id="16616-129">The details of the security token creation are available [here](http://msdn.microsoft.com/library/dn495627.aspx).</span></span>
<span data-ttu-id="16616-130">Az alábbi módszerek kell hozzáadni a **NotificationHub** a jogkivonat létrehozásához osztály alapján a jelenlegi kérelem és a hitelesítő adatokat a kapcsolati karakterlánc kinyert URI.</span><span class="sxs-lookup"><span data-stu-id="16616-130">The following methods have to be added to the **NotificationHub** class to create the token based on the URI of the current request and the credentials extracted from the connection string.</span></span>

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

### <a name="send-a-notification-using-http-rest-api"></a><span data-ttu-id="16616-131">A HTTP REST API használatával értesítést küldeni</span><span class="sxs-lookup"><span data-stu-id="16616-131">Send a notification using HTTP REST API</span></span>
<span data-ttu-id="16616-132">Először is, hogy használja értesítést képviselő osztályt határozza meg.</span><span class="sxs-lookup"><span data-stu-id="16616-132">First, let use define a class representing a notification.</span></span>

    class Notification:
        def __init__(self, notification_format=None, payload=None, debug=0):
            valid_formats = ['template', 'apple', 'gcm', 'windows', 'windowsphone', "adm", "baidu"]
            if not any(x in notification_format for x in valid_formats):
                raise Exception(
                    "Invalid Notification format. " +
                    "Must be one of the following - 'template', 'apple', 'gcm', 'windows', 'windowsphone', 'adm', 'baidu'")

            self.format = notification_format
            self.payload = payload

            # array with keynames for headers
            # Note: Some headers are mandatory: Windows: X-WNS-Type, WindowsPhone: X-NotificationType
            # Note: For Apple you can set Expiry with header: ServiceBusNotification-ApnsExpiry
            # in W3C DTF, YYYY-MM-DDThh:mmTZD (for example, 1997-07-16T19:20+01:00).
            self.headers = None

<span data-ttu-id="16616-133">Ez az osztály egy natív értesítési vagy sablon értesítést, a fejléc formátuma (natív platform vagy sablon) és a platform-specifikus tulajdonságok (például az Apple lejárati tulajdonság és WNS fejlécekkel együtt) tartalmazó készlettel esetén tulajdonságait tárolója .</span><span class="sxs-lookup"><span data-stu-id="16616-133">This class is a container for a native notification body or a set of properties in case of a template notification, a set of headers which contains format (native platform or template) and platform-specific properties (like Apple expiration property and WNS headers).</span></span>

<span data-ttu-id="16616-134">Tekintse meg a [Notification hub REST API-k dokumentáció](http://msdn.microsoft.com/library/dn495827.aspx) és az összes rendelkezésre álló beállítások az adott értesítési platformok formázza az adathordozót.</span><span class="sxs-lookup"><span data-stu-id="16616-134">Please refer to the [Notification Hubs REST APIs documentation](http://msdn.microsoft.com/library/dn495827.aspx) and the specific notification platforms' formats for all the options available.</span></span>

<span data-ttu-id="16616-135">Most már ehhez az osztályhoz, azt írhat a send notification módszerek belül a **NotificationHub** osztály.</span><span class="sxs-lookup"><span data-stu-id="16616-135">Now with this class, we can write the send notification methods inside of the **NotificationHub** class.</span></span>

    def make_http_request(self, url, payload, headers):
        parsed_url = urllib.parse.urlparse(url)
        connection = http.client.HTTPSConnection(parsed_url.hostname, parsed_url.port)

        if self.Debug > 0:
            connection.set_debuglevel(self.Debug)
            # adding this querystring parameter gets detailed information about the PNS send notification outcome
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

        # add the tags/tag expressions to the headers collection
        if tag_list != "":
            headers.update({'ServiceBusNotification-Tags': tag_list})

        # add any custom headers to the headers collection that the user may have added
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

<span data-ttu-id="16616-136">A fenti módszerek HTTP POST-kérelmet küld a /messages végpont az értesítési központ, és a megfelelő törzs és -fejléceket elküldeni az értesítést.</span><span class="sxs-lookup"><span data-stu-id="16616-136">The above methods send an HTTP POST request to the /messages endpoint of your notification hub, with the correct body and headers to send the notification.</span></span>

### <a name="using-debug-property-to-enable-detailed-logging"></a><span data-ttu-id="16616-137">Hibakeresési tulajdonság használatával a részletes naplózás engedélyezése</span><span class="sxs-lookup"><span data-stu-id="16616-137">Using debug property to enable detailed logging</span></span>
<span data-ttu-id="16616-138">Debug tulajdonság engedélyezése az értesítési központ inicializálása során lesz naplózási részletes információt a HTTP-kérelem és válasz memóriakép, valamint részletes értesítési üzenet küldése eredménye.</span><span class="sxs-lookup"><span data-stu-id="16616-138">Enabling debug property while initializing the Notification Hub will write out detailed logging information about the HTTP request and response dump as well as detailed Notification message send outcome.</span></span> <span data-ttu-id="16616-139">Ez a tulajdonság neve nemrégiben hozzáadott [Notification Hubs TestSend tulajdonság](http://msdn.microsoft.com/library/microsoft.servicebus.notifications.notificationhubclient.enabletestsend.aspx) az értesítés küldési kimenetelét részletes információt ad vissza, amely.</span><span class="sxs-lookup"><span data-stu-id="16616-139">We recently added this property called [Notification Hubs TestSend property](http://msdn.microsoft.com/library/microsoft.servicebus.notifications.notificationhubclient.enabletestsend.aspx) which returns detailed information about the notification send outcome.</span></span> <span data-ttu-id="16616-140">A használatára - használatával a következő inicializálása:</span><span class="sxs-lookup"><span data-stu-id="16616-140">To use it - initialize using the following:</span></span>

    hub = NotificationHub("myConnectionString", "myNotificationHubName", isDebug)

<span data-ttu-id="16616-141">A Notification Hub kérés küldése HTTP URL-cím beolvasása kiegészül a "test" lekérdezési karakterlánc eredményeképpen.</span><span class="sxs-lookup"><span data-stu-id="16616-141">The Notification Hub Send request HTTP URL gets appended with a "test" querystring as a result.</span></span> 

## <span data-ttu-id="16616-142"><a name="complete-tutorial"></a>Az oktatóanyag befejezése</span><span class="sxs-lookup"><span data-stu-id="16616-142"><a name="complete-tutorial"></a>Complete the tutorial</span></span>
<span data-ttu-id="16616-143">Az első lépéseket bemutató oktatóanyaghoz most az értesítés küldésével a Python háttér-hajthatja végre.</span><span class="sxs-lookup"><span data-stu-id="16616-143">Now you can complete the Get Started tutorial by sending the notification from a Python back-end.</span></span>

<span data-ttu-id="16616-144">A Notification Hubs-ügyfél inicializálása (útmutatását, helyettesítse be a kapcsolati karakterlánc és a központ nevét a [Get bemutató]):</span><span class="sxs-lookup"><span data-stu-id="16616-144">Initialize your Notification Hubs client (substitute the connection string and hub name as instructed in the [Get started tutorial]):</span></span>

    hub = NotificationHub("myConnectionString", "myNotificationHubName")

<span data-ttu-id="16616-145">Majd adja hozzá a küldési kódot, attól függően, hogy a célként megadott mobilplatformot.</span><span class="sxs-lookup"><span data-stu-id="16616-145">Then add the send code depending on your target mobile platform.</span></span> <span data-ttu-id="16616-146">Ez a minta is hozzáad a magasabb szintű módszerek, például Windows; send_windows_notification platformtípus alapján küldő értesítések engedélyezése (az apple) send_apple_notification stb.</span><span class="sxs-lookup"><span data-stu-id="16616-146">This sample also adds higher level methods to enable sending notifications based on the platform e.g. send_windows_notification for windows; send_apple_notification (for apple) etc.</span></span> 

### <a name="windows-store-and-windows-phone-81-non-silverlight"></a><span data-ttu-id="16616-147">Windows áruház és Windows Phone 8.1 (nem Silverlight)</span><span class="sxs-lookup"><span data-stu-id="16616-147">Windows Store and Windows Phone 8.1 (non-Silverlight)</span></span>
    wns_payload = """<toast><visual><binding template=\"ToastText01\"><text id=\"1\">Test</text></binding></visual></toast>"""
    hub.send_windows_notification(wns_payload)

### <a name="windows-phone-80-and-81-silverlight"></a><span data-ttu-id="16616-148">Windows Phone 8.0-s és 8.1 Silverlight</span><span class="sxs-lookup"><span data-stu-id="16616-148">Windows Phone 8.0 and 8.1 Silverlight</span></span>
    hub.send_mpns_notification(toast)

### <a name="ios"></a><span data-ttu-id="16616-149">iOS</span><span class="sxs-lookup"><span data-stu-id="16616-149">iOS</span></span>
    alert_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_apple_notification(alert_payload)

### <a name="android"></a><span data-ttu-id="16616-150">Android</span><span class="sxs-lookup"><span data-stu-id="16616-150">Android</span></span>
    gcm_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_gcm_notification(gcm_payload)

### <a name="kindle-fire"></a><span data-ttu-id="16616-151">Kindle Fire</span><span class="sxs-lookup"><span data-stu-id="16616-151">Kindle Fire</span></span>
    adm_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_adm_notification(adm_payload)

### <a name="baidu"></a><span data-ttu-id="16616-152">Baidu</span><span class="sxs-lookup"><span data-stu-id="16616-152">Baidu</span></span>
    baidu_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_baidu_notification(baidu_payload)

<span data-ttu-id="16616-153">A Python kódja egy értesítés jelenik meg a céleszközön kell előállítania.</span><span class="sxs-lookup"><span data-stu-id="16616-153">Running your Python code should produce a notification appearing on your target device.</span></span>

## <a name="examples"></a><span data-ttu-id="16616-154">Példák:</span><span class="sxs-lookup"><span data-stu-id="16616-154">Examples:</span></span>
### <a name="enabling-debug-property"></a><span data-ttu-id="16616-155">Debug tulajdonság engedélyezése</span><span class="sxs-lookup"><span data-stu-id="16616-155">Enabling debug property</span></span>
<span data-ttu-id="16616-156">Amikor engedélyezi a hibakeresési jelző a NotificationHub inicializálásakor, akkor láthatja részletes HTTP-kérelem és válasz memóriakép, valamint NotificationOutcome a következő ahol megismerheti, milyen HTTP-fejlécek át lettek adva, a kérelem és milyen HTTP-válasz az értesítési központ érkezett:![][1]</span><span class="sxs-lookup"><span data-stu-id="16616-156">When you enable debug flag while initializing the NotificationHub then you will see detailed HTTP request and response dump as well as NotificationOutcome like the following where you can understand what HTTP headers are passed in the request and what HTTP response was received from the Notification Hub: ![][1]</span></span>

<span data-ttu-id="16616-157">Látni fogja, pl. részletes értesítési központ eredménye</span><span class="sxs-lookup"><span data-stu-id="16616-157">You will see detailed Notification Hub result e.g.</span></span> 

* <span data-ttu-id="16616-158">Ha az üzenet sikeresen elküldte a leküldéses értesítési szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="16616-158">when the message is successfully sent to the Push Notification Service.</span></span> 
  
        <Outcome>The Notification was successfully sent to the Push Notification System</Outcome>
* <span data-ttu-id="16616-159">Ha nem voltak célok megadva a leküldéses értesítésekhez található majd valószínűleg kívánja a következő a válasz (amely azt jelzi, hogy történtek-e az értesítési valószínűleg biztosítanak, mert a regisztrációk volt néhány nem megfelelő címke található regisztrációt)</span><span class="sxs-lookup"><span data-stu-id="16616-159">If there were no targets found for any push notification then you are likely going to see the following in the response (which indicates that there were no registrations found to deliver the notification probably because the registrations had some mismatched tags)</span></span>
  
        '<NotificationOutcome xmlns="http://schemas.microsoft.com/netservices/2010/10/servicebus/connect" xmlns:i="http://www.w3.org/2001/XMLSchema-instance"><Success>0</Success><Failure>0</Failure><Results i:nil="true"/></NotificationOutcome>'

### <a name="broadcast-toast-notification-to-windows"></a><span data-ttu-id="16616-160">Szórási Windows bejelentési értesítés</span><span class="sxs-lookup"><span data-stu-id="16616-160">Broadcast toast notification to Windows</span></span>
<span data-ttu-id="16616-161">Figyelje meg a fejlécek beolvasása kiküldött, ha a Windows-ügyfélhez egy szórási bejelentési értesítést küld.</span><span class="sxs-lookup"><span data-stu-id="16616-161">Notice the headers that get sent out when you are sending a broadcast toast notification to Windows client.</span></span> 

    hub.send_windows_notification(wns_payload)

![][2]

### <a name="send-notification-specifying-a-tag-or-tag-expression"></a><span data-ttu-id="16616-162">Egy tag (vagy egy címke kifejezés) megadása értesítés küldése</span><span class="sxs-lookup"><span data-stu-id="16616-162">Send notification specifying a tag (or tag expression)</span></span>
<span data-ttu-id="16616-163">Figyelje meg, a címkék HTTP-fejléc, amely lekérdezi a HTTP-kérelem fel (az alábbi példában azt küldi az értesítés csak a "Sport" tartalom regisztrációk)</span><span class="sxs-lookup"><span data-stu-id="16616-163">Notice the Tags HTTP header which gets added to the HTTP request (in the example below, we are sending the notification only to registrations with 'sports' payload)</span></span>

    hub.send_windows_notification(wns_payload, "sports")

![][3]

### <a name="send-notification-specifying-multiple-tags"></a><span data-ttu-id="16616-164">Több címkék megadásával értesítés küldése</span><span class="sxs-lookup"><span data-stu-id="16616-164">Send notification specifying multiple tags</span></span>
<span data-ttu-id="16616-165">Figyelje meg, hogyan változik a címkék HTTP-fejléc, több címke elküldésekor.</span><span class="sxs-lookup"><span data-stu-id="16616-165">Notice how the Tags HTTP header changes when multiple tags are sent.</span></span> 

    tags = {'sports', 'politics'}
    hub.send_windows_notification(wns_payload, tags)

![][4]

### <a name="templated-notification"></a><span data-ttu-id="16616-166">Értesítési sablon</span><span class="sxs-lookup"><span data-stu-id="16616-166">Templated notification</span></span>
<span data-ttu-id="16616-167">Figyelje meg, hogy a formátum HTTP-fejléc módosítja és a tartalom törzsében van küldi el a HTTP-kérelem törzse:</span><span class="sxs-lookup"><span data-stu-id="16616-167">Notice that the Format HTTP header changes and the payload body is sent as part of the HTTP request body:</span></span>

<span data-ttu-id="16616-168">**Ügyféloldali - regisztrált sablon**</span><span class="sxs-lookup"><span data-stu-id="16616-168">**Client side - registered template**</span></span>

        var template =
                        @"<toast><visual><binding template=""ToastText01""><text id=""1"">$(greeting_en)</text></binding></visual></toast>";

<span data-ttu-id="16616-169">**Kiszolgálóoldali – a tartalom küldése**</span><span class="sxs-lookup"><span data-stu-id="16616-169">**Server side - sending the payload**</span></span>

        template_payload = {'greeting_en': 'Hello', 'greeting_fr': 'Salut'}
        hub.send_template_notification(template_payload)

![][5]

## <a name="next-steps"></a><span data-ttu-id="16616-170">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="16616-170">Next Steps</span></span>
<span data-ttu-id="16616-171">Ebben a témakörben azt bemutatta, hogyan hozhat létre egy egyszerű Python REST a Notification Hubs.</span><span class="sxs-lookup"><span data-stu-id="16616-171">In this topic we showed how to create a simple Python REST client for Notification Hubs.</span></span> <span data-ttu-id="16616-172">Itt a következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="16616-172">From here you can:</span></span>

* <span data-ttu-id="16616-173">Töltse le a teljes [Python REST burkoló minta], amely tartalmazza a fenti kódot.</span><span class="sxs-lookup"><span data-stu-id="16616-173">Download the full [Python REST wrapper sample], which contains all the code above.</span></span>
* <span data-ttu-id="16616-174">Folytathatja az címkézés funkciót a Notification Hubs a [Megtörje hírek oktatóanyag]</span><span class="sxs-lookup"><span data-stu-id="16616-174">Continue learning about Notification Hubs tagging feature in the [Breaking News tutorial]</span></span>
* <span data-ttu-id="16616-175">A Notification Hubs sablonok funkcióival kapcsolatos folytathatja a [azaz híreket az oktatóanyag]</span><span class="sxs-lookup"><span data-stu-id="16616-175">Continue learning about Notification Hubs Templates feature in the [Localizing News tutorial]</span></span>

<!-- URLs -->
<span data-ttu-id="16616-176">[Python REST burkoló minta]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/notificationhubs-rest-python</span><span class="sxs-lookup"><span data-stu-id="16616-176">[Python REST wrapper sample]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/notificationhubs-rest-python</span></span>
<span data-ttu-id="16616-177">[Get bemutató]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-get-started/</span><span class="sxs-lookup"><span data-stu-id="16616-177">[Get started tutorial]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-get-started/</span></span>
<span data-ttu-id="16616-178">[Megtörje hírek oktatóanyag]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-send-breaking-news/</span><span class="sxs-lookup"><span data-stu-id="16616-178">[Breaking News tutorial]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-send-breaking-news/</span></span>
<span data-ttu-id="16616-179">[azaz híreket az oktatóanyag]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-send-localized-breaking-news/</span><span class="sxs-lookup"><span data-stu-id="16616-179">[Localizing News tutorial]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-send-localized-breaking-news/</span></span>

<!-- Images. -->
[1]: ./media/notification-hubs-python-backend-how-to/DetailedLoggingInfo.png
[2]: ./media/notification-hubs-python-backend-how-to/BroadcastScenario.png
[3]: ./media/notification-hubs-python-backend-how-to/SendWithOneTag.png
[4]: ./media/notification-hubs-python-backend-how-to/SendWithMultipleTags.png
[5]: ./media/notification-hubs-python-backend-how-to/TemplatedNotification.png

