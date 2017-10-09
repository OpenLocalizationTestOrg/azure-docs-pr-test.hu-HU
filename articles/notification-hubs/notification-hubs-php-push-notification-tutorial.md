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
# <a name="how-toouse-notification-hubs-from-php"></a>Hogyan toouse php-ből a Notification Hubs
[!INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]

Java/PHP/Ruby háttér-összes értesítési központok szolgáltatás érhető el hello Notification Hub REST interfész használatával hello MSDN-témakörben leírtak szerint [Notification hub REST API-k](http://msdn.microsoft.com/library/dn223264.aspx).

Ebben a témakörben megmutatjuk, hogyan:

* A Notification Hubs szolgáltatások PHP; a többi ügyfél létrehozása
* Hajtsa végre a hello [Get bemutató](notification-hubs-ios-apple-push-notification-apns-get-started.md) választott hello háttér-részét valósít meg a PHP a mobil platformra.

## <a name="client-interface"></a>Ügyféloldali felület
fő hello felületet biztosít hello ugyanazokat a módszereket, amelyek hello elérhető [.NET Notification Hubs SDK](http://msdn.microsoft.com/library/jj933431.aspx), ez lehetővé teszi toodirectly állomásneveket, az összes hello oktatóanyagok és ezen a helyen aktuálisan elérhető mintákat és hello hello Közösség által közzétett internetes.

Az összes elérhető hello hello kód található [PHP REST burkoló minta].

Például toocreate ügyfél:

    $hub = new NotificationHub("connection string", "hubname");    

az iOS natív értesítések toosend:

    $notification = new Notification("apple", '{"aps":{"alert": "Hello!"}}');
    $hub->sendNotification($notification, null);

## <a name="implementation"></a>Megvalósítás
Ha még nem tette, kövesse a [Get bemutató] akár toohello utolsó szakasza tooimplement hello háttér-esetében.
Is, ha azt szeretné, használhatja a hello hello kód [PHP REST burkoló minta] , és közvetlenül toohello [teljes hello oktatóanyag](#complete-tutorial) szakasz.

Összes hello teljes REST-burkoló található Részletek tooimplement [MSDN](http://msdn.microsoft.com/library/dn530746.aspx). Ebben a szakaszban azt ismerteti, hello fő lépésből szükséges tooaccess Notification hub REST-végpontok hello PHP végrehajtását:

1. Hello kapcsolati karakterlánc elemzése
2. Hello engedélyezési jogkivonat létrehozása
3. Hello HTTP hívás végrehajtása

### <a name="parse-hello-connection-string"></a>Hello kapcsolati karakterlánc elemzése
Itt a hello fő osztály végrehajtási hello ügyfél, amelynek konstruktort, amely elemzi a hello kapcsolati karakterlánc:

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


### <a name="create-security-token"></a>Biztonsági jogkivonat létrehozása
hello biztonsági jogkivonat-létrehozási hello részletei érhetők el [Itt](http://msdn.microsoft.com/library/dn495627.aspx).
hello következő metódusnak hozzáadott toobe toohello **NotificationHub** osztály toocreate hello token hello URI hello aktuális kérelem és hello kapcsolati karakterlánc kinyert hello hitelesítő adatok alapján.

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

### <a name="send-a-notification"></a>Értesítés küldése
Először ossza meg velünk határozza meg egy értesítési képviselő osztályt.

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

Ez az osztály egy olyan tároló, a natív értesítési törzs, vagy egy sablon értesítés esettel tulajdonságait, és a fejléc formátuma (natív platform vagy sablon) és a platform-specifikus tulajdonságok (például az Apple lejárati tulajdonság és WNS fejlécekkel együtt) tartalmazó készlettel.

Tekintse meg a toohello [Notification hub REST API-k dokumentáció](http://msdn.microsoft.com/library/dn495827.aspx) és hello adott értesítési platformok formátumok, az összes rendelkezésre álló lehetőségek hello.

Volt képes ez az osztály felvértezve, azt most írhat hello send notification módszerek hello belül **NotificationHub** osztály.

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

hello módszerek fent egy HTTP POST kérelem toohello /messages végpontot az értesítési központ hello megfelelő szervezet és a fejlécek toosend hello értesítés küldése.

## <a name="complete-tutorial"></a>Teljes hello oktatóanyag
Most elvégezhet hello első lépéseket bemutató oktatóanyaghoz PHP háttér-hello értesítési üzenetet küld.

A Notification Hubs-ügyfél inicializálása (hello kapcsolati karakterláncot és a központ neve helyettesítő hello útmutatását [Get bemutató]):

    $hub = new NotificationHub("connection string", "hubname");    

Majd adja hozzá a hello küldési kódot attól függően, hogy a célként megadott mobilplatformot.

### <a name="windows-store-and-windows-phone-81-non-silverlight"></a>Windows áruház és Windows Phone 8.1 (nem Silverlight)
    $toast = '<toast><visual><binding template="ToastText01"><text id="1">Hello from PHP!</text></binding></visual></toast>';
    $notification = new Notification("windows", $toast);
    $notification->headers[] = 'X-WNS-Type: wns/toast';
    $hub->sendNotification($notification, null);

### <a name="ios"></a>iOS
    $alert = '{"aps":{"alert":"Hello from PHP!"}}';
    $notification = new Notification("apple", $alert);
    $hub->sendNotification($notification, null);

### <a name="android"></a>Android
    $message = '{"data":{"msg":"Hello from PHP!"}}';
    $notification = new Notification("gcm", $message);
    $hub->sendNotification($notification, null);

### <a name="windows-phone-80-and-81-silverlight"></a>Windows Phone 8.0-s és 8.1 Silverlight
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


### <a name="kindle-fire"></a>Kindle Fire
    $message = '{"data":{"msg":"Hello from PHP!"}}';
    $notification = new Notification("adm", $message);
    $hub->sendNotification($notification, null);

A PHP kódja kell előállítania most egy értesítés jelenik meg a céleszközön.

## <a name="next-steps"></a>Következő lépések
Ebben a témakörben azt bemutatta, hogyan toocreate egy egyszerű Java REST-a Notification Hubs-ügyfél. Itt a következőket teheti:

* Teljes hello letöltése [PHP REST burkoló minta], amely tartalmazza a fenti hello kódot.
* Folytathatja az értesítési központok szolgáltatás szerinti címkézését hello [Megtörje hírek oktatóanyag]
* További tudnivalók tooindividual felhasználók értesítések küldését [felhasználók értesítése oktatóprogram]

További információkért lásd még: hello [PHP fejlesztői központ](/develop/php/).

[PHP REST burkoló minta]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/notificationhubs-rest-php
[Get bemutató]: http://azure.microsoft.com/documentation/articles/notification-hubs-ios-get-started/

