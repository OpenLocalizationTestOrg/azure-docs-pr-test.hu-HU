---
title: a PHP a Notification Hubs aaaHow toouse
description: "Megtudhatja, hogyan toouse Azure Notification Hubs PHP háttér-a."
services: notification-hubs
documentationcenter: 
author: ysxu
manager: erikre
editor: 
ms.assetid: 0156f994-96d0-4878-b07b-49b7be4fd856
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: php
ms.devlang: php
ms.topic: article
ms.date: 06/07/2016
ms.author: yuaxu
ms.openlocfilehash: 6cd426286a684006a07867fcf44a8ff71be7efa8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-notification-hubs-from-php"></a><span data-ttu-id="9d740-103">Hogyan toouse php-ből a Notification Hubs</span><span class="sxs-lookup"><span data-stu-id="9d740-103">How toouse Notification Hubs from PHP</span></span>
[!INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]

<span data-ttu-id="9d740-104">Java/PHP/Ruby háttér-összes értesítési központok szolgáltatás érhető el hello Notification Hub REST interfész használatával hello MSDN-témakörben leírtak szerint [Notification hub REST API-k](http://msdn.microsoft.com/library/dn223264.aspx).</span><span class="sxs-lookup"><span data-stu-id="9d740-104">You can access all Notification Hubs features from a Java/PHP/Ruby back-end using hello Notification Hub REST interface as described in hello MSDN topic [Notification Hubs REST APIs](http://msdn.microsoft.com/library/dn223264.aspx).</span></span>

<span data-ttu-id="9d740-105">Ebben a témakörben megmutatjuk, hogyan:</span><span class="sxs-lookup"><span data-stu-id="9d740-105">In this topic we show how to:</span></span>

* <span data-ttu-id="9d740-106">A Notification Hubs szolgáltatások PHP; a többi ügyfél létrehozása</span><span class="sxs-lookup"><span data-stu-id="9d740-106">Build a REST client for Notification Hubs features in PHP;</span></span>
* <span data-ttu-id="9d740-107">Hajtsa végre a hello [Get bemutató](notification-hubs-ios-apple-push-notification-apns-get-started.md) választott hello háttér-részét valósít meg a PHP a mobil platformra.</span><span class="sxs-lookup"><span data-stu-id="9d740-107">Follow hello [Get started tutorial](notification-hubs-ios-apple-push-notification-apns-get-started.md) for your mobile platform of choice, implementing hello back-end portion in PHP.</span></span>

## <a name="client-interface"></a><span data-ttu-id="9d740-108">Ügyféloldali felület</span><span class="sxs-lookup"><span data-stu-id="9d740-108">Client interface</span></span>
<span data-ttu-id="9d740-109">fő hello felületet biztosít hello ugyanazokat a módszereket, amelyek hello elérhető [.NET Notification Hubs SDK](http://msdn.microsoft.com/library/jj933431.aspx), ez lehetővé teszi toodirectly állomásneveket, az összes hello oktatóanyagok és ezen a helyen aktuálisan elérhető mintákat és hello hello Közösség által közzétett internetes.</span><span class="sxs-lookup"><span data-stu-id="9d740-109">hello main client interface can provide hello same methods that are available in hello [.NET Notification Hubs SDK](http://msdn.microsoft.com/library/jj933431.aspx), this will allow you toodirectly translate all hello tutorials and samples currently available on this site, and contributed by hello community on hello internet.</span></span>

<span data-ttu-id="9d740-110">Az összes elérhető hello hello kód található [PHP REST burkoló minta].</span><span class="sxs-lookup"><span data-stu-id="9d740-110">You can find all hello code available in hello [PHP REST wrapper sample].</span></span>

<span data-ttu-id="9d740-111">Például toocreate ügyfél:</span><span class="sxs-lookup"><span data-stu-id="9d740-111">For example, toocreate a client:</span></span>

    $hub = new NotificationHub("connection string", "hubname");    

<span data-ttu-id="9d740-112">az iOS natív értesítések toosend:</span><span class="sxs-lookup"><span data-stu-id="9d740-112">toosend an iOS native notification:</span></span>

    $notification = new Notification("apple", '{"aps":{"alert": "Hello!"}}');
    $hub->sendNotification($notification, null);

## <a name="implementation"></a><span data-ttu-id="9d740-113">Megvalósítás</span><span class="sxs-lookup"><span data-stu-id="9d740-113">Implementation</span></span>
<span data-ttu-id="9d740-114">Ha még nem tette, kövesse a [Get bemutató] akár toohello utolsó szakasza tooimplement hello háttér-esetében.</span><span class="sxs-lookup"><span data-stu-id="9d740-114">If you did not already, please follow our [Get started tutorial] up toohello last section where you have tooimplement hello back-end.</span></span>
<span data-ttu-id="9d740-115">Is, ha azt szeretné, használhatja a hello hello kód [PHP REST burkoló minta] , és közvetlenül toohello [teljes hello oktatóanyag](#complete-tutorial) szakasz.</span><span class="sxs-lookup"><span data-stu-id="9d740-115">Also, if you want you can use hello code from hello [PHP REST wrapper sample] and go directly toohello [Complete hello tutorial](#complete-tutorial) section.</span></span>

<span data-ttu-id="9d740-116">Összes hello teljes REST-burkoló található Részletek tooimplement [MSDN](http://msdn.microsoft.com/library/dn530746.aspx).</span><span class="sxs-lookup"><span data-stu-id="9d740-116">All hello details tooimplement a full REST wrapper can be found on [MSDN](http://msdn.microsoft.com/library/dn530746.aspx).</span></span> <span data-ttu-id="9d740-117">Ebben a szakaszban azt ismerteti, hello fő lépésből szükséges tooaccess Notification hub REST-végpontok hello PHP végrehajtását:</span><span class="sxs-lookup"><span data-stu-id="9d740-117">In this section we will describe hello PHP implementation of hello main steps required tooaccess Notification Hubs REST endpoints:</span></span>

1. <span data-ttu-id="9d740-118">Hello kapcsolati karakterlánc elemzése</span><span class="sxs-lookup"><span data-stu-id="9d740-118">Parse hello connection string</span></span>
2. <span data-ttu-id="9d740-119">Hello engedélyezési jogkivonat létrehozása</span><span class="sxs-lookup"><span data-stu-id="9d740-119">Generate hello authorization token</span></span>
3. <span data-ttu-id="9d740-120">Hello HTTP hívás végrehajtása</span><span class="sxs-lookup"><span data-stu-id="9d740-120">Perform hello HTTP call</span></span>

### <a name="parse-hello-connection-string"></a><span data-ttu-id="9d740-121">Hello kapcsolati karakterlánc elemzése</span><span class="sxs-lookup"><span data-stu-id="9d740-121">Parse hello connection string</span></span>
<span data-ttu-id="9d740-122">Itt a hello fő osztály végrehajtási hello ügyfél, amelynek konstruktort, amely elemzi a hello kapcsolati karakterlánc:</span><span class="sxs-lookup"><span data-stu-id="9d740-122">Here is hello main class implementing hello client, whose constructor that parses hello connection string:</span></span>

    class NotificationHub {
        const API_VERSION = "?api-version=2013-10";

        private $endpoint;
        private $hubPath;
        private $sasKeyName;
        private $sasKeyValue;

        function __construct($connectionString, $hubPath) {
            $this->hubPath = $hubPath;

            $this->parseConnectionString($connectionString);
        }

        private function parseConnectionString($connectionString) {
            $parts = explode(";", $connectionString);
            if (sizeof($parts) != 3) {
                throw new Exception("Error parsing connection string: " . $connectionString);
            }

            foreach ($parts as $part) {
                if (strpos($part, "Endpoint") === 0) {
                    $this->endpoint = "https" . substr($part, 11);
                } else if (strpos($part, "SharedAccessKeyName") === 0) {
                    $this->sasKeyName = substr($part, 20);
                } else if (strpos($part, "SharedAccessKey") === 0) {
                    $this->sasKeyValue = substr($part, 16);
                }
            }
        }
    }


### <a name="create-security-token"></a><span data-ttu-id="9d740-123">Biztonsági jogkivonat létrehozása</span><span class="sxs-lookup"><span data-stu-id="9d740-123">Create security token</span></span>
<span data-ttu-id="9d740-124">hello biztonsági jogkivonat-létrehozási hello részletei érhetők el [Itt](http://msdn.microsoft.com/library/dn495627.aspx).</span><span class="sxs-lookup"><span data-stu-id="9d740-124">hello details of hello security token creation are available [here](http://msdn.microsoft.com/library/dn495627.aspx).</span></span>
<span data-ttu-id="9d740-125">hello következő metódusnak hozzáadott toobe toohello **NotificationHub** osztály toocreate hello token hello URI hello aktuális kérelem és hello kapcsolati karakterlánc kinyert hello hitelesítő adatok alapján.</span><span class="sxs-lookup"><span data-stu-id="9d740-125">hello following method has toobe added toohello **NotificationHub** class toocreate hello token based on hello URI of hello current request and hello credentials extracted from hello connection string.</span></span>

    private function generateSasToken($uri) {
        $targetUri = strtolower(rawurlencode(strtolower($uri)));

        $expires = time();
        $expiresInMins = 60;
        $expires = $expires + $expiresInMins * 60;
        $toSign = $targetUri . "\n" . $expires;

        $signature = rawurlencode(base64_encode(hash_hmac('sha256', $toSign, $this->sasKeyValue, TRUE)));

        $token = "SharedAccessSignature sr=" . $targetUri . "&sig="
                    . $signature . "&se=" . $expires . "&skn=" . $this->sasKeyName;

        return $token;
    }

### <a name="send-a-notification"></a><span data-ttu-id="9d740-126">Értesítés küldése</span><span class="sxs-lookup"><span data-stu-id="9d740-126">Send a notification</span></span>
<span data-ttu-id="9d740-127">Először ossza meg velünk határozza meg egy értesítési képviselő osztályt.</span><span class="sxs-lookup"><span data-stu-id="9d740-127">First, let us define a class representing a notification.</span></span>

    class Notification {
        public $format;
        public $payload;

        # array with keynames for headers
        # Note: Some headers are mandatory: Windows: X-WNS-Type, WindowsPhone: X-NotificationType
        # Note: For Apple you can set Expiry with header: ServiceBusNotification-ApnsExpiry in W3C DTF, YYYY-MM-DDThh:mmTZD (for example, 1997-07-16T19:20+01:00).
        public $headers;

        function __construct($format, $payload) {
            if (!in_array($format, ["template", "apple", "windows", "gcm", "windowsphone"])) {
                throw new Exception('Invalid format: ' . $format);
            }

            $this->format = $format;
            $this->payload = $payload;
        }
    }

<span data-ttu-id="9d740-128">Ez az osztály egy olyan tároló, a natív értesítési törzs, vagy egy sablon értesítés esettel tulajdonságait, és a fejléc formátuma (natív platform vagy sablon) és a platform-specifikus tulajdonságok (például az Apple lejárati tulajdonság és WNS fejlécekkel együtt) tartalmazó készlettel.</span><span class="sxs-lookup"><span data-stu-id="9d740-128">This class is a container for a native notification body, or a set of properties on case of a template notification, and a set of headers which contains format (native platform or template) and platform-specific properties (like Apple expiration property and WNS headers).</span></span>

<span data-ttu-id="9d740-129">Tekintse meg a toohello [Notification hub REST API-k dokumentáció](http://msdn.microsoft.com/library/dn495827.aspx) és hello adott értesítési platformok formátumok, az összes rendelkezésre álló lehetőségek hello.</span><span class="sxs-lookup"><span data-stu-id="9d740-129">Please refer toohello [Notification Hubs REST APIs documentation](http://msdn.microsoft.com/library/dn495827.aspx) and hello specific notification platforms' formats for all hello options available.</span></span>

<span data-ttu-id="9d740-130">Volt képes ez az osztály felvértezve, azt most írhat hello send notification módszerek hello belül **NotificationHub** osztály.</span><span class="sxs-lookup"><span data-stu-id="9d740-130">Armed with this class, we can now write hello send notification methods inside of hello **NotificationHub** class.</span></span>

    public function sendNotification($notification, $tagsOrTagExpression="") {
        if (is_array($tagsOrTagExpression)) {
            $tagExpression = implode(" || ", $tagsOrTagExpression);
        } else {
            $tagExpression = $tagsOrTagExpression;
        }

        # build uri
        $uri = $this->endpoint . $this->hubPath . "/messages" . NotificationHub::API_VERSION;
        $ch = curl_init($uri);

        if (in_array($notification->format, ["template", "apple", "gcm"])) {
            $contentType = "application/json";
        } else {
            $contentType = "application/xml";
        }

        $token = $this->generateSasToken($uri);

        $headers = [
            'Authorization: '.$token,
            'Content-Type: '.$contentType,
            'ServiceBusNotification-Format: '.$notification->format
        ];

        if ("" !== $tagExpression) {
            $headers[] = 'ServiceBusNotification-Tags: '.$tagExpression;
        }

        # add headers for other platforms
        if (is_array($notification->headers)) {
            $headers = array_merge($headers, $notification->headers);
        }

        curl_setopt_array($ch, array(
            CURLOPT_POST => TRUE,
            CURLOPT_RETURNTRANSFER => TRUE,
            CURLOPT_SSL_VERIFYPEER => FALSE,
            CURLOPT_HTTPHEADER => $headers,
            CURLOPT_POSTFIELDS => $notification->payload
        ));

        // Send hello request
        $response = curl_exec($ch);

        // Check for errors
        if($response === FALSE){
            throw new Exception(curl_error($ch));
        }

        $info = curl_getinfo($ch);

        if ($info['http_code'] <> 201) {
            throw new Exception('Error sending notificaiton: '. $info['http_code'] . ' msg: ' . $response);
        }
    } 

<span data-ttu-id="9d740-131">hello módszerek fent egy HTTP POST kérelem toohello /messages végpontot az értesítési központ hello megfelelő szervezet és a fejlécek toosend hello értesítés küldése.</span><span class="sxs-lookup"><span data-stu-id="9d740-131">hello above methods send an HTTP POST request toohello /messages endpoint of your notification hub, with hello correct body and headers toosend hello notification.</span></span>

## <span data-ttu-id="9d740-132"><a name="complete-tutorial"></a>Teljes hello oktatóanyag</span><span class="sxs-lookup"><span data-stu-id="9d740-132"><a name="complete-tutorial"></a>Complete hello tutorial</span></span>
<span data-ttu-id="9d740-133">Most elvégezhet hello első lépéseket bemutató oktatóanyaghoz PHP háttér-hello értesítési üzenetet küld.</span><span class="sxs-lookup"><span data-stu-id="9d740-133">Now you can complete hello Get Started tutorial by sending hello notification from a PHP back-end.</span></span>

<span data-ttu-id="9d740-134">A Notification Hubs-ügyfél inicializálása (hello kapcsolati karakterláncot és a központ neve helyettesítő hello útmutatását [Get bemutató]):</span><span class="sxs-lookup"><span data-stu-id="9d740-134">Initialize your Notification Hubs client (substitute hello connection string and hub name as instructed in hello [Get started tutorial]):</span></span>

    $hub = new NotificationHub("connection string", "hubname");    

<span data-ttu-id="9d740-135">Majd adja hozzá a hello küldési kódot attól függően, hogy a célként megadott mobilplatformot.</span><span class="sxs-lookup"><span data-stu-id="9d740-135">Then add hello send code depending on your target mobile platform.</span></span>

### <a name="windows-store-and-windows-phone-81-non-silverlight"></a><span data-ttu-id="9d740-136">Windows áruház és Windows Phone 8.1 (nem Silverlight)</span><span class="sxs-lookup"><span data-stu-id="9d740-136">Windows Store and Windows Phone 8.1 (non-Silverlight)</span></span>
    $toast = '<toast><visual><binding template="ToastText01"><text id="1">Hello from PHP!</text></binding></visual></toast>';
    $notification = new Notification("windows", $toast);
    $notification->headers[] = 'X-WNS-Type: wns/toast';
    $hub->sendNotification($notification, null);

### <a name="ios"></a><span data-ttu-id="9d740-137">iOS</span><span class="sxs-lookup"><span data-stu-id="9d740-137">iOS</span></span>
    $alert = '{"aps":{"alert":"Hello from PHP!"}}';
    $notification = new Notification("apple", $alert);
    $hub->sendNotification($notification, null);

### <a name="android"></a><span data-ttu-id="9d740-138">Android</span><span class="sxs-lookup"><span data-stu-id="9d740-138">Android</span></span>
    $message = '{"data":{"msg":"Hello from PHP!"}}';
    $notification = new Notification("gcm", $message);
    $hub->sendNotification($notification, null);

### <a name="windows-phone-80-and-81-silverlight"></a><span data-ttu-id="9d740-139">Windows Phone 8.0-s és 8.1 Silverlight</span><span class="sxs-lookup"><span data-stu-id="9d740-139">Windows Phone 8.0 and 8.1 Silverlight</span></span>
    $toast = '<?xml version="1.0" encoding="utf-8"?>' .
                '<wp:Notification xmlns:wp="WPNotification">' .
                   '<wp:Toast>' .
                        '<wp:Text1>Hello from PHP!</wp:Text1>' .
                   '</wp:Toast> ' .
                '</wp:Notification>';
    $notification = new Notification("windowsphone", $toast);
    $notification->headers[] = 'X-WindowsPhone-Target : toast';
    $notification->headers[] = 'X-NotificationClass : 2';
    $hub->sendNotification($notification, null);


### <a name="kindle-fire"></a><span data-ttu-id="9d740-140">Kindle Fire</span><span class="sxs-lookup"><span data-stu-id="9d740-140">Kindle Fire</span></span>
    $message = '{"data":{"msg":"Hello from PHP!"}}';
    $notification = new Notification("adm", $message);
    $hub->sendNotification($notification, null);

<span data-ttu-id="9d740-141">A PHP kódja kell előállítania most egy értesítés jelenik meg a céleszközön.</span><span class="sxs-lookup"><span data-stu-id="9d740-141">Running your PHP code should produce now a notification appearing on your target device.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9d740-142">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9d740-142">Next Steps</span></span>
<span data-ttu-id="9d740-143">Ebben a témakörben azt bemutatta, hogyan toocreate egy egyszerű Java REST-a Notification Hubs-ügyfél.</span><span class="sxs-lookup"><span data-stu-id="9d740-143">In this topic we showed how toocreate a simple Java REST client for Notification Hubs.</span></span> <span data-ttu-id="9d740-144">Itt a következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="9d740-144">From here you can:</span></span>

* <span data-ttu-id="9d740-145">Teljes hello letöltése [PHP REST burkoló minta], amely tartalmazza a fenti hello kódot.</span><span class="sxs-lookup"><span data-stu-id="9d740-145">Download hello full [PHP REST wrapper sample], which contains all hello code above.</span></span>
* <span data-ttu-id="9d740-146">Folytathatja az értesítési központok szolgáltatás szerinti címkézését hello [Megtörje hírek oktatóanyag]</span><span class="sxs-lookup"><span data-stu-id="9d740-146">Continue learning about Notification Hubs tagging feature in hello [Breaking News tutorial]</span></span>
* <span data-ttu-id="9d740-147">További tudnivalók tooindividual felhasználók értesítések küldését [felhasználók értesítése oktatóprogram]</span><span class="sxs-lookup"><span data-stu-id="9d740-147">Learn about pushing notifications tooindividual users in [Notify Users tutorial]</span></span>

<span data-ttu-id="9d740-148">További információkért lásd még: hello [PHP fejlesztői központ](/develop/php/).</span><span class="sxs-lookup"><span data-stu-id="9d740-148">For more information, see also hello [PHP Developer Center](/develop/php/).</span></span>

[PHP REST burkoló minta]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/notificationhubs-rest-php
[Get bemutató]: http://azure.microsoft.com/documentation/articles/notification-hubs-ios-get-started/

