---
title: "A Notification Hubs használata PHP"
description: "Útmutató az Azure Notification Hubs használata PHP háttér-a."
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
ms.openlocfilehash: c27b6308ff528224a0398e0ff40537db05417bb0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-notification-hubs-from-php"></a><span data-ttu-id="59740-103">Php-ből a Notification Hubs használatával</span><span class="sxs-lookup"><span data-stu-id="59740-103">How to use Notification Hubs from PHP</span></span>
[!INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]

<span data-ttu-id="59740-104">Minden értesítési központok szolgáltatás elérhető egy Java/PHP/Ruby-háttér használata a Notification Hub REST interfész, az MSDN-témakörben leírtak szerint [Notification hub REST API-k](http://msdn.microsoft.com/library/dn223264.aspx).</span><span class="sxs-lookup"><span data-stu-id="59740-104">You can access all Notification Hubs features from a Java/PHP/Ruby back-end using the Notification Hub REST interface as described in the MSDN topic [Notification Hubs REST APIs](http://msdn.microsoft.com/library/dn223264.aspx).</span></span>

<span data-ttu-id="59740-105">Ebben a témakörben megmutatjuk, hogyan:</span><span class="sxs-lookup"><span data-stu-id="59740-105">In this topic we show how to:</span></span>

* <span data-ttu-id="59740-106">A Notification Hubs szolgáltatások PHP; a többi ügyfél létrehozása</span><span class="sxs-lookup"><span data-stu-id="59740-106">Build a REST client for Notification Hubs features in PHP;</span></span>
* <span data-ttu-id="59740-107">Kövesse a [Get bemutató](notification-hubs-ios-apple-push-notification-apns-get-started.md) a mobil választott platformhoz valósít meg a háttér-rész PHP.</span><span class="sxs-lookup"><span data-stu-id="59740-107">Follow the [Get started tutorial](notification-hubs-ios-apple-push-notification-apns-get-started.md) for your mobile platform of choice, implementing the back-end portion in PHP.</span></span>

## <a name="client-interface"></a><span data-ttu-id="59740-108">Ügyféloldali felület</span><span class="sxs-lookup"><span data-stu-id="59740-108">Client interface</span></span>
<span data-ttu-id="59740-109">A fő ügyféloldali felületen elérhető ugyanazokat a módszereket biztosít a [.NET Notification Hubs SDK](http://msdn.microsoft.com/library/jj933431.aspx), ez lehetővé teszi az oktatóanyagok és ezen a helyen aktuálisan elérhető mintákat közvetlenül fordítása, és az interneten a Közösség által közzétett.</span><span class="sxs-lookup"><span data-stu-id="59740-109">The main client interface can provide the same methods that are available in the [.NET Notification Hubs SDK](http://msdn.microsoft.com/library/jj933431.aspx), this will allow you to directly translate all the tutorials and samples currently available on this site, and contributed by the community on the internet.</span></span>

<span data-ttu-id="59740-110">Az összes elérhető kód megtalálhatja a [PHP REST burkoló minta].</span><span class="sxs-lookup"><span data-stu-id="59740-110">You can find all the code available in the [PHP REST wrapper sample].</span></span>

<span data-ttu-id="59740-111">Ha például egy ügyfél létrehozásához:</span><span class="sxs-lookup"><span data-stu-id="59740-111">For example, to create a client:</span></span>

    $hub = new NotificationHub("connection string", "hubname");    

<span data-ttu-id="59740-112">IOS natív értesítést küldeni:</span><span class="sxs-lookup"><span data-stu-id="59740-112">To send an iOS native notification:</span></span>

    $notification = new Notification("apple", '{"aps":{"alert": "Hello!"}}');
    $hub->sendNotification($notification, null);

## <a name="implementation"></a><span data-ttu-id="59740-113">Megvalósítás</span><span class="sxs-lookup"><span data-stu-id="59740-113">Implementation</span></span>
<span data-ttu-id="59740-114">Ha még nem tette, kövesse a [Get bemutató] akár utolsó szakasz esetében a háttér-megvalósításához.</span><span class="sxs-lookup"><span data-stu-id="59740-114">If you did not already, please follow our [Get started tutorial] up to the last section where you have to implement the back-end.</span></span>
<span data-ttu-id="59740-115">Is, ha azt szeretné, használhatja a kódot a [PHP REST burkoló minta] és közvetlenül a [az oktatóanyag befejezése](#complete-tutorial) szakasz.</span><span class="sxs-lookup"><span data-stu-id="59740-115">Also, if you want you can use the code from the [PHP REST wrapper sample] and go directly to the [Complete the tutorial](#complete-tutorial) section.</span></span>

<span data-ttu-id="59740-116">Egy teljes REST-burkoló megvalósításához a részleteit található [MSDN](http://msdn.microsoft.com/library/dn530746.aspx).</span><span class="sxs-lookup"><span data-stu-id="59740-116">All the details to implement a full REST wrapper can be found on [MSDN](http://msdn.microsoft.com/library/dn530746.aspx).</span></span> <span data-ttu-id="59740-117">Ebben a szakaszban azt ismerteti, a PHP megvalósítása a Notification hub REST-végpontok eléréséhez szükséges fő lépésből áll:</span><span class="sxs-lookup"><span data-stu-id="59740-117">In this section we will describe the PHP implementation of the main steps required to access Notification Hubs REST endpoints:</span></span>

1. <span data-ttu-id="59740-118">Kapcsolati karakterlánc elemzése</span><span class="sxs-lookup"><span data-stu-id="59740-118">Parse the connection string</span></span>
2. <span data-ttu-id="59740-119">Az engedélyezési jogkivonat létrehozása</span><span class="sxs-lookup"><span data-stu-id="59740-119">Generate the authorization token</span></span>
3. <span data-ttu-id="59740-120">Hajtsa végre a HTTP-hívás</span><span class="sxs-lookup"><span data-stu-id="59740-120">Perform the HTTP call</span></span>

### <a name="parse-the-connection-string"></a><span data-ttu-id="59740-121">Kapcsolati karakterlánc elemzése</span><span class="sxs-lookup"><span data-stu-id="59740-121">Parse the connection string</span></span>
<span data-ttu-id="59740-122">Az ügyfél, amelynek konstruktort, amely elemzi a kapcsolati karakterlánc végrehajtási fő osztály itt található:</span><span class="sxs-lookup"><span data-stu-id="59740-122">Here is the main class implementing the client, whose constructor that parses the connection string:</span></span>

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


### <a name="create-security-token"></a><span data-ttu-id="59740-123">Biztonsági jogkivonat létrehozása</span><span class="sxs-lookup"><span data-stu-id="59740-123">Create security token</span></span>
<span data-ttu-id="59740-124">A részleteket a biztonsági jogkivonat-létrehozási [Itt](http://msdn.microsoft.com/library/dn495627.aspx).</span><span class="sxs-lookup"><span data-stu-id="59740-124">The details of the security token creation are available [here](http://msdn.microsoft.com/library/dn495627.aspx).</span></span>
<span data-ttu-id="59740-125">A következő metódusnak fel kell venni a **NotificationHub** a jogkivonat létrehozásához osztály alapján a jelenlegi kérelem és a hitelesítő adatokat a kapcsolati karakterlánc kinyert URI.</span><span class="sxs-lookup"><span data-stu-id="59740-125">The following method has to be added to the **NotificationHub** class to create the token based on the URI of the current request and the credentials extracted from the connection string.</span></span>

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

### <a name="send-a-notification"></a><span data-ttu-id="59740-126">Értesítés küldése</span><span class="sxs-lookup"><span data-stu-id="59740-126">Send a notification</span></span>
<span data-ttu-id="59740-127">Először ossza meg velünk határozza meg egy értesítési képviselő osztályt.</span><span class="sxs-lookup"><span data-stu-id="59740-127">First, let us define a class representing a notification.</span></span>

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

<span data-ttu-id="59740-128">Ez az osztály egy olyan tároló, a natív értesítési törzs, vagy egy sablon értesítés esettel tulajdonságait, és a fejléc formátuma (natív platform vagy sablon) és a platform-specifikus tulajdonságok (például az Apple lejárati tulajdonság és WNS fejlécekkel együtt) tartalmazó készlettel.</span><span class="sxs-lookup"><span data-stu-id="59740-128">This class is a container for a native notification body, or a set of properties on case of a template notification, and a set of headers which contains format (native platform or template) and platform-specific properties (like Apple expiration property and WNS headers).</span></span>

<span data-ttu-id="59740-129">Tekintse meg a [Notification hub REST API-k dokumentáció](http://msdn.microsoft.com/library/dn495827.aspx) és az összes rendelkezésre álló beállítások az adott értesítési platformok formázza az adathordozót.</span><span class="sxs-lookup"><span data-stu-id="59740-129">Please refer to the [Notification Hubs REST APIs documentation](http://msdn.microsoft.com/library/dn495827.aspx) and the specific notification platforms' formats for all the options available.</span></span>

<span data-ttu-id="59740-130">Volt képes ez az osztály felvértezve, azt most írhat a send notification módszerek belül a **NotificationHub** osztály.</span><span class="sxs-lookup"><span data-stu-id="59740-130">Armed with this class, we can now write the send notification methods inside of the **NotificationHub** class.</span></span>

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

        // Send the request
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

<span data-ttu-id="59740-131">A fenti módszerek HTTP POST-kérelmet küld a /messages végpont az értesítési központ, és a megfelelő törzs és -fejléceket elküldeni az értesítést.</span><span class="sxs-lookup"><span data-stu-id="59740-131">The above methods send an HTTP POST request to the /messages endpoint of your notification hub, with the correct body and headers to send the notification.</span></span>

## <span data-ttu-id="59740-132"><a name="complete-tutorial"></a>Az oktatóanyag befejezése</span><span class="sxs-lookup"><span data-stu-id="59740-132"><a name="complete-tutorial"></a>Complete the tutorial</span></span>
<span data-ttu-id="59740-133">Az első lépéseket bemutató oktatóanyaghoz most az értesítés küldésével a PHP háttér-hajthatja végre.</span><span class="sxs-lookup"><span data-stu-id="59740-133">Now you can complete the Get Started tutorial by sending the notification from a PHP back-end.</span></span>

<span data-ttu-id="59740-134">A Notification Hubs-ügyfél inicializálása (útmutatását, helyettesítse be a kapcsolati karakterlánc és a központ nevét a [Get bemutató]):</span><span class="sxs-lookup"><span data-stu-id="59740-134">Initialize your Notification Hubs client (substitute the connection string and hub name as instructed in the [Get started tutorial]):</span></span>

    $hub = new NotificationHub("connection string", "hubname");    

<span data-ttu-id="59740-135">Majd adja hozzá a küldési kódot, attól függően, hogy a célként megadott mobilplatformot.</span><span class="sxs-lookup"><span data-stu-id="59740-135">Then add the send code depending on your target mobile platform.</span></span>

### <a name="windows-store-and-windows-phone-81-non-silverlight"></a><span data-ttu-id="59740-136">Windows áruház és Windows Phone 8.1 (nem Silverlight)</span><span class="sxs-lookup"><span data-stu-id="59740-136">Windows Store and Windows Phone 8.1 (non-Silverlight)</span></span>
    $toast = '<toast><visual><binding template="ToastText01"><text id="1">Hello from PHP!</text></binding></visual></toast>';
    $notification = new Notification("windows", $toast);
    $notification->headers[] = 'X-WNS-Type: wns/toast';
    $hub->sendNotification($notification, null);

### <a name="ios"></a><span data-ttu-id="59740-137">iOS</span><span class="sxs-lookup"><span data-stu-id="59740-137">iOS</span></span>
    $alert = '{"aps":{"alert":"Hello from PHP!"}}';
    $notification = new Notification("apple", $alert);
    $hub->sendNotification($notification, null);

### <a name="android"></a><span data-ttu-id="59740-138">Android</span><span class="sxs-lookup"><span data-stu-id="59740-138">Android</span></span>
    $message = '{"data":{"msg":"Hello from PHP!"}}';
    $notification = new Notification("gcm", $message);
    $hub->sendNotification($notification, null);

### <a name="windows-phone-80-and-81-silverlight"></a><span data-ttu-id="59740-139">Windows Phone 8.0-s és 8.1 Silverlight</span><span class="sxs-lookup"><span data-stu-id="59740-139">Windows Phone 8.0 and 8.1 Silverlight</span></span>
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


### <a name="kindle-fire"></a><span data-ttu-id="59740-140">Kindle Fire</span><span class="sxs-lookup"><span data-stu-id="59740-140">Kindle Fire</span></span>
    $message = '{"data":{"msg":"Hello from PHP!"}}';
    $notification = new Notification("adm", $message);
    $hub->sendNotification($notification, null);

<span data-ttu-id="59740-141">A PHP kódja kell előállítania most egy értesítés jelenik meg a céleszközön.</span><span class="sxs-lookup"><span data-stu-id="59740-141">Running your PHP code should produce now a notification appearing on your target device.</span></span>

## <a name="next-steps"></a><span data-ttu-id="59740-142">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="59740-142">Next Steps</span></span>
<span data-ttu-id="59740-143">Ebben a témakörben azt bemutatta, hogyan hozhat létre egy egyszerű Java REST-ügyfél a Notification Hubs.</span><span class="sxs-lookup"><span data-stu-id="59740-143">In this topic we showed how to create a simple Java REST client for Notification Hubs.</span></span> <span data-ttu-id="59740-144">Itt a következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="59740-144">From here you can:</span></span>

* <span data-ttu-id="59740-145">Töltse le a teljes [PHP REST burkoló minta], amely tartalmazza a fenti kódot.</span><span class="sxs-lookup"><span data-stu-id="59740-145">Download the full [PHP REST wrapper sample], which contains all the code above.</span></span>
* <span data-ttu-id="59740-146">Folytathatja az értesítési központok szolgáltatás címkézés [Megtörje hírek oktatóprogram]</span><span class="sxs-lookup"><span data-stu-id="59740-146">Continue learning about Notification Hubs tagging feature in the [Breaking News tutorial]</span></span>
* <span data-ttu-id="59740-147">További tudnivalók az értesítések küldését az egyéni felhasználók számára [felhasználók értesítése oktatóprogram]</span><span class="sxs-lookup"><span data-stu-id="59740-147">Learn about pushing notifications to individual users in [Notify Users tutorial]</span></span>

<span data-ttu-id="59740-148">További információ: a [PHP fejlesztői központ](/develop/php/).</span><span class="sxs-lookup"><span data-stu-id="59740-148">For more information, see also the [PHP Developer Center](/develop/php/).</span></span>

<span data-ttu-id="59740-149">[PHP REST burkoló minta]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/notificationhubs-rest-php</span><span class="sxs-lookup"><span data-stu-id="59740-149">[PHP REST wrapper sample]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/notificationhubs-rest-php</span></span>
<span data-ttu-id="59740-150">[Get bemutató]: http://azure.microsoft.com/documentation/articles/notification-hubs-ios-get-started/</span><span class="sxs-lookup"><span data-stu-id="59740-150">[Get started tutorial]: http://azure.microsoft.com/documentation/articles/notification-hubs-ios-get-started/</span></span>

