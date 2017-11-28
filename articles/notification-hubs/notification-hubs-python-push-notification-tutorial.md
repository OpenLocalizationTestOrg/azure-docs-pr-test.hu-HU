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
# <a name="how-toouse-notification-hubs-from-python"></a><span data-ttu-id="9579b-103">Hogyan toouse Notification Hubs Python</span><span class="sxs-lookup"><span data-stu-id="9579b-103">How toouse Notification Hubs from Python</span></span>
[!INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]

<span data-ttu-id="9579b-104">Java/PHP/Python vagy Ruby háttér-összes értesítési központok szolgáltatás érhető el hello Notification Hub REST interfész használatával hello MSDN-témakörben leírtak szerint [Notification hub REST API-k](http://msdn.microsoft.com/library/dn223264.aspx).</span><span class="sxs-lookup"><span data-stu-id="9579b-104">You can access all Notification Hubs features from a Java/PHP/Python/Ruby back-end using hello Notification Hub REST interface as described in hello MSDN topic [Notification Hubs REST APIs](http://msdn.microsoft.com/library/dn223264.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="9579b-105">Ez a minta hivatkozás megvalósítása megvalósításának hello értesítést küld a Python és nem hello hivatalosan értesítések Hub Python SDK támogatott.</span><span class="sxs-lookup"><span data-stu-id="9579b-105">This is a sample reference implementation for implementing hello notification sends in Python and is not hello officially supported Notifications Hub Python SDK.</span></span>
> 
> <span data-ttu-id="9579b-106">Ez a minta a Python 3.4 leírva.</span><span class="sxs-lookup"><span data-stu-id="9579b-106">This sample is written using Python 3.4.</span></span>
> 
> 

<span data-ttu-id="9579b-107">Ebben a témakörben megmutatjuk, hogyan:</span><span class="sxs-lookup"><span data-stu-id="9579b-107">In this topic we show how to:</span></span>

* <span data-ttu-id="9579b-108">A Notification Hubs-szolgáltatások a Python egy REST-ügyfél felépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="9579b-108">Build a REST client for Notification Hubs features in Python.</span></span>
* <span data-ttu-id="9579b-109">Hello Python felület toohello Notification Hub REST API-k használatával értesítések küldéséhez.</span><span class="sxs-lookup"><span data-stu-id="9579b-109">Send notifications using hello Python interface toohello Notification Hub REST APIs.</span></span> 
* <span data-ttu-id="9579b-110">Első hello HTTP REST kérelem-válasz egy kiírást hibakeresés/oktatási célra.</span><span class="sxs-lookup"><span data-stu-id="9579b-110">Get a dump of hello HTTP REST request/response for debugging/educational purpose.</span></span> 

<span data-ttu-id="9579b-111">Hajtsa végre az hello [Get bemutató](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) választott, Python hello háttér-részét valósít meg a mobil platformra.</span><span class="sxs-lookup"><span data-stu-id="9579b-111">You can follow hello [Get started tutorial](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) for your mobile platform of choice, implementing hello back-end portion in Python.</span></span>

> [!NOTE]
> <span data-ttu-id="9579b-112">hello hello minta hatóköre csak korlátozott toosend értesítéseket, és minden olyan regisztrációs felügyeleti nem.</span><span class="sxs-lookup"><span data-stu-id="9579b-112">hello scope of hello sample is only limited toosend notifications and it doesn't do any registration management.</span></span>
> 
> 

## <a name="client-interface"></a><span data-ttu-id="9579b-113">Ügyféloldali felület</span><span class="sxs-lookup"><span data-stu-id="9579b-113">Client interface</span></span>
<span data-ttu-id="9579b-114">fő hello felületet biztosít hello ugyanazokat a módszereket, amelyek hello elérhető [.NET Notification Hubs SDK](http://msdn.microsoft.com/library/jj933431.aspx).</span><span class="sxs-lookup"><span data-stu-id="9579b-114">hello main client interface can provide hello same methods that are available in hello [.NET Notification Hubs SDK](http://msdn.microsoft.com/library/jj933431.aspx).</span></span> <span data-ttu-id="9579b-115">Ez lehetővé teszi toodirectly állomásneveket, az összes hello oktatóanyagok és ezen a helyen aktuálisan elérhető mintákat és hello hello Közösség által közzétett internetes.</span><span class="sxs-lookup"><span data-stu-id="9579b-115">This will allow you toodirectly translate all hello tutorials and samples currently available on this site, and contributed by hello community on hello internet.</span></span>

<span data-ttu-id="9579b-116">Az összes elérhető hello hello kód található [Python REST burkoló minta].</span><span class="sxs-lookup"><span data-stu-id="9579b-116">You can find all hello code available in hello [Python REST wrapper sample].</span></span>

<span data-ttu-id="9579b-117">Például toocreate ügyfél:</span><span class="sxs-lookup"><span data-stu-id="9579b-117">For example, toocreate a client:</span></span>

    isDebug = True
    hub = NotificationHub("myConnectionString", "myNotificationHubName", isDebug)

<span data-ttu-id="9579b-118">a Windows toosend poharad értesítés:</span><span class="sxs-lookup"><span data-stu-id="9579b-118">toosend a Windows toast notification:</span></span>

    wns_payload = """<toast><visual><binding template=\"ToastText01\"><text id=\"1\">Hello world!</text></binding></visual></toast>"""
    hub.send_windows_notification(wns_payload)

## <a name="implementation"></a><span data-ttu-id="9579b-119">Megvalósítás</span><span class="sxs-lookup"><span data-stu-id="9579b-119">Implementation</span></span>
<span data-ttu-id="9579b-120">Ha még nem tette, kövesse a [Get bemutató] akár toohello utolsó szakasza tooimplement hello háttér-esetében.</span><span class="sxs-lookup"><span data-stu-id="9579b-120">If you did not already, please follow our [Get started tutorial] up toohello last section where you have tooimplement hello back-end.</span></span>

<span data-ttu-id="9579b-121">Összes hello teljes REST-burkoló található Részletek tooimplement [MSDN](http://msdn.microsoft.com/library/dn530746.aspx).</span><span class="sxs-lookup"><span data-stu-id="9579b-121">All hello details tooimplement a full REST wrapper can be found on [MSDN](http://msdn.microsoft.com/library/dn530746.aspx).</span></span> <span data-ttu-id="9579b-122">Ez a szakasz a rendszer hello fő lépésből szükséges tooaccess Notification hub REST-végpontok hello Python végrehajtását ismertetik és értesítések küldéséhez</span><span class="sxs-lookup"><span data-stu-id="9579b-122">In this section we will describe hello Python implementation of hello main steps required tooaccess Notification Hubs REST endpoints and send notifications</span></span>

1. <span data-ttu-id="9579b-123">Hello kapcsolati karakterlánc elemzése</span><span class="sxs-lookup"><span data-stu-id="9579b-123">Parse hello connection string</span></span>
2. <span data-ttu-id="9579b-124">Hello engedélyezési jogkivonat létrehozása</span><span class="sxs-lookup"><span data-stu-id="9579b-124">Generate hello authorization token</span></span>
3. <span data-ttu-id="9579b-125">A HTTP REST API használatával értesítést küldeni</span><span class="sxs-lookup"><span data-stu-id="9579b-125">Send a notification using HTTP REST API</span></span>

### <a name="parse-hello-connection-string"></a><span data-ttu-id="9579b-126">Hello kapcsolati karakterlánc elemzése</span><span class="sxs-lookup"><span data-stu-id="9579b-126">Parse hello connection string</span></span>
<span data-ttu-id="9579b-127">Íme hello hello ügyfél, amelynek konstruktor elemez hello kapcsolati karakterlánc végrehajtási fő osztályban:</span><span class="sxs-lookup"><span data-stu-id="9579b-127">Here is hello main class implementing hello client, whose constructor parses hello connection string:</span></span>

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


### <a name="create-security-token"></a><span data-ttu-id="9579b-128">Biztonsági jogkivonat létrehozása</span><span class="sxs-lookup"><span data-stu-id="9579b-128">Create security token</span></span>
<span data-ttu-id="9579b-129">hello biztonsági jogkivonat-létrehozási hello részletei érhetők el [Itt](http://msdn.microsoft.com/library/dn495627.aspx).</span><span class="sxs-lookup"><span data-stu-id="9579b-129">hello details of hello security token creation are available [here](http://msdn.microsoft.com/library/dn495627.aspx).</span></span>
<span data-ttu-id="9579b-130">hello következőkkel rendelkezik hozzáadott toobe toohello **NotificationHub** osztály toocreate hello token hello URI hello aktuális kérelem és hello kapcsolati karakterlánc kinyert hello hitelesítő adatok alapján.</span><span class="sxs-lookup"><span data-stu-id="9579b-130">hello following methods have toobe added toohello **NotificationHub** class toocreate hello token based on hello URI of hello current request and hello credentials extracted from hello connection string.</span></span>

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

### <a name="send-a-notification-using-http-rest-api"></a><span data-ttu-id="9579b-131">A HTTP REST API használatával értesítést küldeni</span><span class="sxs-lookup"><span data-stu-id="9579b-131">Send a notification using HTTP REST API</span></span>
<span data-ttu-id="9579b-132">Először is, hogy használja értesítést képviselő osztályt határozza meg.</span><span class="sxs-lookup"><span data-stu-id="9579b-132">First, let use define a class representing a notification.</span></span>

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

<span data-ttu-id="9579b-133">Ez az osztály egy natív értesítési vagy sablon értesítést, a fejléc formátuma (natív platform vagy sablon) és a platform-specifikus tulajdonságok (például az Apple lejárati tulajdonság és WNS fejlécekkel együtt) tartalmazó készlettel esetén tulajdonságait tárolója .</span><span class="sxs-lookup"><span data-stu-id="9579b-133">This class is a container for a native notification body or a set of properties in case of a template notification, a set of headers which contains format (native platform or template) and platform-specific properties (like Apple expiration property and WNS headers).</span></span>

<span data-ttu-id="9579b-134">Tekintse meg a toohello [Notification hub REST API-k dokumentáció](http://msdn.microsoft.com/library/dn495827.aspx) és hello adott értesítési platformok formátumok, az összes rendelkezésre álló lehetőségek hello.</span><span class="sxs-lookup"><span data-stu-id="9579b-134">Please refer toohello [Notification Hubs REST APIs documentation](http://msdn.microsoft.com/library/dn495827.aspx) and hello specific notification platforms' formats for all hello options available.</span></span>

<span data-ttu-id="9579b-135">Most már ehhez az osztályhoz, azt írhat hello send notification módszerek hello belül **NotificationHub** osztály.</span><span class="sxs-lookup"><span data-stu-id="9579b-135">Now with this class, we can write hello send notification methods inside of hello **NotificationHub** class.</span></span>

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

<span data-ttu-id="9579b-136">hello módszerek fent egy HTTP POST kérelem toohello /messages végpontot az értesítési központ hello megfelelő szervezet és a fejlécek toosend hello értesítés küldése.</span><span class="sxs-lookup"><span data-stu-id="9579b-136">hello above methods send an HTTP POST request toohello /messages endpoint of your notification hub, with hello correct body and headers toosend hello notification.</span></span>

### <a name="using-debug-property-tooenable-detailed-logging"></a><span data-ttu-id="9579b-137">Hibakeresési tulajdonság tooenable használatával részletes naplózás</span><span class="sxs-lookup"><span data-stu-id="9579b-137">Using debug property tooenable detailed logging</span></span>
<span data-ttu-id="9579b-138">Értesítési központ hello inicializálása során hibakeresési tulajdonság engedélyezése fog kiírni, hello HTTP részletes naplózás információt kérés és válasz memóriakép, valamint részletes értesítési üzenetet küldeni az eredménye.</span><span class="sxs-lookup"><span data-stu-id="9579b-138">Enabling debug property while initializing hello Notification Hub will write out detailed logging information about hello HTTP request and response dump as well as detailed Notification message send outcome.</span></span> <span data-ttu-id="9579b-139">Ez a tulajdonság neve nemrégiben hozzáadott [Notification Hubs TestSend tulajdonság](http://msdn.microsoft.com/library/microsoft.servicebus.notifications.notificationhubclient.enabletestsend.aspx) hello értesítés küldése kimenetelét részletes információt ad vissza, amely.</span><span class="sxs-lookup"><span data-stu-id="9579b-139">We recently added this property called [Notification Hubs TestSend property](http://msdn.microsoft.com/library/microsoft.servicebus.notifications.notificationhubclient.enabletestsend.aspx) which returns detailed information about hello notification send outcome.</span></span> <span data-ttu-id="9579b-140">toouse azt - hello alábbi inicializálása:</span><span class="sxs-lookup"><span data-stu-id="9579b-140">toouse it - initialize using hello following:</span></span>

    hub = NotificationHub("myConnectionString", "myNotificationHubName", isDebug)

<span data-ttu-id="9579b-141">hello Notification Hub küldési kérelem HTTP URL-cím beolvasása kiegészül a "test" lekérdezési karakterlánc eredményeképpen.</span><span class="sxs-lookup"><span data-stu-id="9579b-141">hello Notification Hub Send request HTTP URL gets appended with a "test" querystring as a result.</span></span> 

## <span data-ttu-id="9579b-142"><a name="complete-tutorial"></a>Teljes hello oktatóanyag</span><span class="sxs-lookup"><span data-stu-id="9579b-142"><a name="complete-tutorial"></a>Complete hello tutorial</span></span>
<span data-ttu-id="9579b-143">Most már a Python háttér-hello értesítés küldésével hello első lépéseket bemutató oktatóanyaghoz hajthatja végre.</span><span class="sxs-lookup"><span data-stu-id="9579b-143">Now you can complete hello Get Started tutorial by sending hello notification from a Python back-end.</span></span>

<span data-ttu-id="9579b-144">A Notification Hubs-ügyfél inicializálása (hello kapcsolati karakterláncot és a központ neve helyettesítő hello útmutatását [Get bemutató]):</span><span class="sxs-lookup"><span data-stu-id="9579b-144">Initialize your Notification Hubs client (substitute hello connection string and hub name as instructed in hello [Get started tutorial]):</span></span>

    hub = NotificationHub("myConnectionString", "myNotificationHubName")

<span data-ttu-id="9579b-145">Majd adja hozzá a hello küldési kódot attól függően, hogy a célként megadott mobilplatformot.</span><span class="sxs-lookup"><span data-stu-id="9579b-145">Then add hello send code depending on your target mobile platform.</span></span> <span data-ttu-id="9579b-146">Ez a minta is hozzáad a magasabb szintű módszerek tooenable hello platform pl. Windows; send_windows_notification alapján értesítések küldése (az apple) send_apple_notification stb.</span><span class="sxs-lookup"><span data-stu-id="9579b-146">This sample also adds higher level methods tooenable sending notifications based on hello platform e.g. send_windows_notification for windows; send_apple_notification (for apple) etc.</span></span> 

### <a name="windows-store-and-windows-phone-81-non-silverlight"></a><span data-ttu-id="9579b-147">Windows áruház és Windows Phone 8.1 (nem Silverlight)</span><span class="sxs-lookup"><span data-stu-id="9579b-147">Windows Store and Windows Phone 8.1 (non-Silverlight)</span></span>
    wns_payload = """<toast><visual><binding template=\"ToastText01\"><text id=\"1\">Test</text></binding></visual></toast>"""
    hub.send_windows_notification(wns_payload)

### <a name="windows-phone-80-and-81-silverlight"></a><span data-ttu-id="9579b-148">Windows Phone 8.0-s és 8.1 Silverlight</span><span class="sxs-lookup"><span data-stu-id="9579b-148">Windows Phone 8.0 and 8.1 Silverlight</span></span>
    hub.send_mpns_notification(toast)

### <a name="ios"></a><span data-ttu-id="9579b-149">iOS</span><span class="sxs-lookup"><span data-stu-id="9579b-149">iOS</span></span>
    alert_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_apple_notification(alert_payload)

### <a name="android"></a><span data-ttu-id="9579b-150">Android</span><span class="sxs-lookup"><span data-stu-id="9579b-150">Android</span></span>
    gcm_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_gcm_notification(gcm_payload)

### <a name="kindle-fire"></a><span data-ttu-id="9579b-151">Kindle Fire</span><span class="sxs-lookup"><span data-stu-id="9579b-151">Kindle Fire</span></span>
    adm_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_adm_notification(adm_payload)

### <a name="baidu"></a><span data-ttu-id="9579b-152">Baidu</span><span class="sxs-lookup"><span data-stu-id="9579b-152">Baidu</span></span>
    baidu_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_baidu_notification(baidu_payload)

<span data-ttu-id="9579b-153">A Python kódja egy értesítés jelenik meg a céleszközön kell előállítania.</span><span class="sxs-lookup"><span data-stu-id="9579b-153">Running your Python code should produce a notification appearing on your target device.</span></span>

## <a name="examples"></a><span data-ttu-id="9579b-154">Példák:</span><span class="sxs-lookup"><span data-stu-id="9579b-154">Examples:</span></span>
### <a name="enabling-debug-property"></a><span data-ttu-id="9579b-155">Debug tulajdonság engedélyezése</span><span class="sxs-lookup"><span data-stu-id="9579b-155">Enabling debug property</span></span>
<span data-ttu-id="9579b-156">Amikor engedélyezi a hibakeresési jelző hello NotificationHub inicializálásakor, látni fogja, majd részletes HTTP-kérelem-válasz memóriakép, valamint a NotificationOutcome hasonló hello ahol megismerheti, milyen HTTP-fejlécek átadott hello kérelemben és milyen HTTP Értesítési központ hello érkezett válasz:![][1]</span><span class="sxs-lookup"><span data-stu-id="9579b-156">When you enable debug flag while initializing hello NotificationHub then you will see detailed HTTP request and response dump as well as NotificationOutcome like hello following where you can understand what HTTP headers are passed in hello request and what HTTP response was received from hello Notification Hub: ![][1]</span></span>

<span data-ttu-id="9579b-157">Látni fogja, pl. részletes értesítési központ eredménye</span><span class="sxs-lookup"><span data-stu-id="9579b-157">You will see detailed Notification Hub result e.g.</span></span> 

* <span data-ttu-id="9579b-158">Ha hello sikeresen üzenettel toohello leküldéses értesítéseket kezelő szolgáltatásában.</span><span class="sxs-lookup"><span data-stu-id="9579b-158">when hello message is successfully sent toohello Push Notification Service.</span></span> 
  
        <Outcome>hello Notification was successfully sent toohello Push Notification System</Outcome>
* <span data-ttu-id="9579b-159">Ha voltak célok megadva a leküldéses értesítésekhez található, akkor valószínűleg fog toosee hello következő hello választ (amely azt jelzi, hogy történtek-e toodeliver hello értesítési valószínűleg található, mert a hello regisztrációk volt néhány regisztrációt nem megfelelő címkékkel)</span><span class="sxs-lookup"><span data-stu-id="9579b-159">If there were no targets found for any push notification then you are likely going toosee hello following in hello response (which indicates that there were no registrations found toodeliver hello notification probably because hello registrations had some mismatched tags)</span></span>
  
        '<NotificationOutcome xmlns="http://schemas.microsoft.com/netservices/2010/10/servicebus/connect" xmlns:i="http://www.w3.org/2001/XMLSchema-instance"><Success>0</Success><Failure>0</Failure><Results i:nil="true"/></NotificationOutcome>'

### <a name="broadcast-toast-notification-toowindows"></a><span data-ttu-id="9579b-160">Bejelentési értesítés tooWindows szórási</span><span class="sxs-lookup"><span data-stu-id="9579b-160">Broadcast toast notification tooWindows</span></span>
<span data-ttu-id="9579b-161">Figyelje meg hello fejlécek beolvasása kiküldött tooWindows ügyfél szórási bejelentési értesítés küldéséhez.</span><span class="sxs-lookup"><span data-stu-id="9579b-161">Notice hello headers that get sent out when you are sending a broadcast toast notification tooWindows client.</span></span> 

    hub.send_windows_notification(wns_payload)

![][2]

### <a name="send-notification-specifying-a-tag-or-tag-expression"></a><span data-ttu-id="9579b-162">Egy tag (vagy egy címke kifejezés) megadása értesítés küldése</span><span class="sxs-lookup"><span data-stu-id="9579b-162">Send notification specifying a tag (or tag expression)</span></span>
<span data-ttu-id="9579b-163">Értesítés hello címkék HTTP-fejléc lekérdezi hozzáadott toohello HTTP-kérelem (hello az alábbi példában azt küldenek hello értesítési csak tooregistrations "Sport" hasznos)</span><span class="sxs-lookup"><span data-stu-id="9579b-163">Notice hello Tags HTTP header which gets added toohello HTTP request (in hello example below, we are sending hello notification only tooregistrations with 'sports' payload)</span></span>

    hub.send_windows_notification(wns_payload, "sports")

![][3]

### <a name="send-notification-specifying-multiple-tags"></a><span data-ttu-id="9579b-164">Több címkék megadásával értesítés küldése</span><span class="sxs-lookup"><span data-stu-id="9579b-164">Send notification specifying multiple tags</span></span>
<span data-ttu-id="9579b-165">Figyelje meg, hogyan hello címkék HTTP-fejléc változik, ha több címke küld.</span><span class="sxs-lookup"><span data-stu-id="9579b-165">Notice how hello Tags HTTP header changes when multiple tags are sent.</span></span> 

    tags = {'sports', 'politics'}
    hub.send_windows_notification(wns_payload, tags)

![][4]

### <a name="templated-notification"></a><span data-ttu-id="9579b-166">Értesítési sablon</span><span class="sxs-lookup"><span data-stu-id="9579b-166">Templated notification</span></span>
<span data-ttu-id="9579b-167">Figyelje meg, hogy hello formátum HTTP-fejléc módosításokat, és hasznos törzs hello hello HTTP-kérés törzsében részeként zajlik:</span><span class="sxs-lookup"><span data-stu-id="9579b-167">Notice that hello Format HTTP header changes and hello payload body is sent as part of hello HTTP request body:</span></span>

<span data-ttu-id="9579b-168">**Ügyféloldali - regisztrált sablon**</span><span class="sxs-lookup"><span data-stu-id="9579b-168">**Client side - registered template**</span></span>

        var template =
                        @"<toast><visual><binding template=""ToastText01""><text id=""1"">$(greeting_en)</text></binding></visual></toast>";

<span data-ttu-id="9579b-169">**Kiszolgálóoldali - hello tartalom küldése**</span><span class="sxs-lookup"><span data-stu-id="9579b-169">**Server side - sending hello payload**</span></span>

        template_payload = {'greeting_en': 'Hello', 'greeting_fr': 'Salut'}
        hub.send_template_notification(template_payload)

![][5]

## <a name="next-steps"></a><span data-ttu-id="9579b-170">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9579b-170">Next Steps</span></span>
<span data-ttu-id="9579b-171">Ebben a témakörben azt bemutatta, hogyan toocreate egy egyszerű Python REST-a Notification Hubs-ügyfél.</span><span class="sxs-lookup"><span data-stu-id="9579b-171">In this topic we showed how toocreate a simple Python REST client for Notification Hubs.</span></span> <span data-ttu-id="9579b-172">Itt a következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="9579b-172">From here you can:</span></span>

* <span data-ttu-id="9579b-173">Teljes hello letöltése [Python REST burkoló minta], amely tartalmazza a fenti hello kódot.</span><span class="sxs-lookup"><span data-stu-id="9579b-173">Download hello full [Python REST wrapper sample], which contains all hello code above.</span></span>
* <span data-ttu-id="9579b-174">Folytathatja az értesítési központok szolgáltatás hello szerinti címkézését [Megtörje hírek oktatóanyag]</span><span class="sxs-lookup"><span data-stu-id="9579b-174">Continue learning about Notification Hubs tagging feature in hello [Breaking News tutorial]</span></span>
* <span data-ttu-id="9579b-175">Folytathatja az értesítési központok sablonok funkció a hello [azaz hírek oktatóanyag]</span><span class="sxs-lookup"><span data-stu-id="9579b-175">Continue learning about Notification Hubs Templates feature in hello [Localizing News tutorial]</span></span>

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

