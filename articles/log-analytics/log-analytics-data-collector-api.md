---
title: "aaaLog Analytics HTTP adatait gyűjtője API |} Microsoft Docs"
description: "Hello napló Analytics HTTP adatokat gyűjtő API tooadd POST JSON toohello Naplóelemzési adattárház bármely ügyfél, amely meghívhatja hello REST API-t is használhatja. Ez a cikk ismerteti, hogyan toouse hello API, és példákat a rendelkezik toopublish adatok különböző programnyelveken használatával."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: 
ms.assetid: a831fd90-3f55-423b-8b20-ccbaaac2ca75
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: bwren
ms.openlocfilehash: c2921082831c49da764d946ac9c4fab975a38185
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="send-data-toolog-analytics-with-hello-http-data-collector-api"></a>Adatok tooLog Analytics a hello HTTP adatait gyűjtője API küldése
Ez a cikk bemutatja, hogyan toouse hello HTTP adatait gyűjtője API toosend adatok tooLog Analytics a REST API-ügyfél.  Útmutatás a tooformat adatokat gyűjtött a parancsfájl vagy az alkalmazáshoz, adja hozzá a kérelem, és a van a Naplóelemzési által engedélyezett.  A példák PowerShell, a C# és Python.

## <a name="concepts"></a>Alapelvek
Hello HTTP adatait gyűjtője API toosend adatok tooLog Analytics minden ügyfélről, amely a REST API meghívása is használhatja.  Ez lehet egy runbook az Azure Automationben, hogy a gyűjtő felügyeleti Azure vagy egy másik felhőben vagy adatait, előfordulhat, hogy egy másik felügyeleti rendszer Naplóelemzési tooconsolidate használó és adatok elemzése.

Egy rekord, egy adott rekordtípus hello Naplóelemzési tárházban összes adatot tárolja.  Az adatok toosend toohello HTTP adatait gyűjtője API formázza a több rekordot a JSON-ban.  Hello adatokat küld, ha egy egyéni rekord hello tárházban hello-kérések forgalma lévő minden egyes rekord jön létre.


![HTTP adatgyűjtő áttekintése](media/log-analytics-data-collector-api/overview.png)



## <a name="create-a-request"></a>Kérelem létrehozása
toouse hello HTTP adatgyűjtő API-hoz létre, amely tartalmazza az hello adatok toosend JavaScript Object Notation (JSON) a POST-kérelmet.  hello a következő három táblák listáját hello attribútumok, amelyek szükségesek az egyes kérelmek. Azt írja le, minden egyes attribútum hello cikk későbbi részében részletesebben.

### <a name="request-uri"></a>Kérelem URI-azonosítója
| Attribútum | Tulajdonság |
|:--- |:--- |
| Módszer |POST |
| URI |https://\<CustomerId\>.ods.opinsights.azure.com/api/logs?api-version=2016-04-01 |
| Tartalom típusa |application/json |

### <a name="request-uri-parameters"></a>A kérelem URI-paraméterei
| Paraméter | Leírás |
|:--- |:--- |
| CustomerID |a Microsoft Operations Management Suite-munkaterülettel hello hello egyedi azonosítója. |
| Erőforrás |hello API erőforrásnév: / api/logs. |
| API-verzió |az ehhez a kérelemhez hello API toouse hello verzióját. Ez jelenleg 2016-04-01. |

### <a name="request-headers"></a>Kérelem fejlécei
| Fejléc | Leírás |
|:--- |:--- |
| Engedélyezés |hello engedélyezési aláírás. Hello cikk későbbi részében olvashat hogyan toocreate egy HMAC-SHA256-fejléc. |
| Napló-típusa |Adja meg az Ön küldi hello adatok hello rekord típusát. Hello típussal jelenleg csak alfanumerikus karaktereket tartalmazhat. Nem támogatja írhatók vagy speciális karaktereket. |
| x-ms-dátuma |hello dátum, hogy hello kérelem feldolgozása, RFC 1123 formátumban. |
| idő generált mező |hello hello adatok hello időbélyeg hello adatelem tartalmazó mező nevét. Ha a megadott mező, akkor annak tartalmát használt **TimeGenerated**. Ha ez a mező nincs megadva, az alapértelmezett hello **TimeGenerated** az hello idő adott hello üzenet van okozhatnak. hello tartalmát hello üzenet érdemes követnie hello ISO 8601 formátum éééé-hh-SSz. |

## <a name="authorization"></a>Engedélyezés
A kérelem toohello napló Analytics HTTP adatokat gyűjtő API tartalmaznia kell egy engedélyezési fejléc. a kérelmet, tooauthenticate hello kérelem hello elsődleges vagy másodlagos kulcsát hello hello munkaterület, amely lehetővé teszi a hello kérelem be kell jelentkeznie. Ezt követően adja át az adott aláírás hello kérelem részeként.   

Hello hello engedélyezési fejléc formátuma a következő:

```
Authorization: SharedKey <WorkspaceID>:<Signature>
```

*WorkspaceID* hello hello Operations Management Suite-munkaterülettel az egyedi azonosító. *Aláírás* van egy [kivonat-alapú üzenethitelesítési kód (HMAC)](https://msdn.microsoft.com/library/system.security.cryptography.hmacsha256.aspx) , hello kérelemből összeállított és hello segítségével majd számított [SHA-256 algoritmus](https://msdn.microsoft.com/library/system.security.cryptography.sha256.aspx). Majd hogy kódolása Base64 kódolással.

A formátum tooencode hello használata **SharedKey** aláírás-karakterlánc:

```
StringToSign = VERB + "\n" +
                  Content-Length + "\n" +
               Content-Type + "\n" +
                  x-ms-date + "\n" +
                  "/api/logs";
```

Íme egy példa látható egy aláírás karakterláncra:

```
POST\n1024\napplication/json\nx-ms-date:Mon, 04 Apr 2016 08:00:00 GMT\n/api/logs
```

Ha rendelkezik hello aláírás karakterlánc kódolása használatával hello hello UTF-8 kódolású karakterlánc a HMAC-SHA-256 algoritmust, és majd a hello eredmény Base64 kódolása. Ezt a formátumot használja:

```
Signature=Base64(HMAC-SHA256(UTF8(StringToSign)))
```

hello minták hello következő szakaszokban lévő minta kód toohelp létrehozhat egy engedélyezési fejléc rendelkezik.

## <a name="request-body"></a>Kérés törzsében
hello üzenet törzsét hello JSON kell lennie. Egy vagy több rekordot hello tulajdonság név-érték párokat kell tartalmaznia a következő formátumban:

```
{
"property1": "value1",
" property 2": "value2"
" property 3": "value3",
" property 4": "value4"
}
```

A köteg egyetlen kérelemmel együtt a több rekordot hello a következő formátum használatával. Minden hello kell hello azonos rekordtípus.

```
{
"property1": "value1",
" property 2": "value2"
" property 3": "value3",
" property 4": "value4"
},
{
"property1": "value1",
" property 2": "value2"
" property 3": "value3",
" property 4": "value4"
}
```

## <a name="record-type-and-properties"></a>Bejegyzéstípus és tulajdonságai
Az egyéni típusát határozza meg, amikor hello napló Analytics HTTP adatokat gyűjtő API használatával adatokat küld. Jelenleg nem írhat adatok tooexisting rekordtípusokhoz létrehozott más adattípusok és megoldásokat. A Naplóelemzési hello bejövő adatokat olvas, és ezután hoz létre, amelyek megfelelnek a megadott értékek hello hello adatok típusú tulajdonságok.

Minden egyes kérelem toohello napló Analytics API tartalmaznia kell egy **hibanapló-típus** hello bejegyzéstípus hello nevű fejléc. hello utótag **_CL** meg más naplóból, meg kell adnia egy egyéni naplót, toodistinguish automatikusan hozzáfűzött toohello neve. Például, ha hello beírása **MyNewRecordType**, Log Analyticshez létrehoz egy rekordot hello típusú **MyNewRecordType_CL**. Ezzel biztosíthatja, hogy nincsenek-e a felhasználó által létrehozott típusnevek és a jelenlegi vagy jövőbeli Microsoft megoldások szállított közötti ütközések.

a Naplóelemzési veszi fel utótag toohello tulajdonság nevét, tooidentify a tulajdonság adattípusát. Ha egy tulajdonság null értéket tartalmaz, hello tulajdonság nem szerepel a rekordban levő. Ez a táblázat felsorolja a hello tulajdonságadat-típus és a megfelelő utótag:

| Tulajdonságadat-típus | Utótag |
|:--- |:--- |
| Karakterlánc |z |
| Logikai érték |_B |
| Dupla |_D |
| Dátum és idő |_Szo |
| GUID |_G |

hello adattípus, amely Naplóelemzési alkalmaz minden egyes tulajdonság függ hello rekordtípus hello új rekord létezik-e.

* Ha hello rekordtípus nem létezik, a Naplóelemzési létrehoz egy új. Naplóelemzési hello JSON típus megállapítás toodetermine hello adattípus alkalmaz minden tulajdonság hello rekordban.
* Ha hello rekordtípus nem létezik, Naplóelemzési megpróbál toocreate egy új rekordot a létező tulajdonságok alapján. Ha hello hello új rekordban a tulajdonság adattípusát nem megfelelő típusú meglévő konvertált toohello, és nem Ha hello rekordot tartalmaz olyan tulajdonságon, amely nem létezik, Log Analyticshez hoz létre új tulajdonság, amely rendelkezik a megfelelő utótag hello.

Például a küldésének bejegyzés hozna létre egy olyan rekordot három tulajdonságokkal **number_d**, **boolean_b**, és **string_s**:

![A minta rekord 1](media/log-analytics-data-collector-api/record-01.png)

Ha ezután a következő bejegyzés formázott karakterláncok értékekkel, hello tulajdonságok nem megváltozna. Ezek az értékek lehetnek konvertált tooexisting adattípusok:

![2. példa rekord](media/log-analytics-data-collector-api/record-02.png)

De ha a következő küldésének majd végrehajtott Naplóelemzési létrehoznia hello új tulajdonságok **boolean_d** és **string_d**. Ezeket az értékeket nem lehet konvertálni:

![A minta rekord 3](media/log-analytics-data-collector-api/record-03.png)

Hello hello rekordtípus létrehozása előtt a következő bejegyzést, majd benyújtott Naplóelemzési volna rekordot kell létrehozni három tulajdonságokkal **sikeresek**, **boolean_s**, és **string_s**. Ez a bejegyzés a hello kezdeti értékekre van formázva karakterláncként:

![4. példa rekord](media/log-analytics-data-collector-api/record-04.png)

## <a name="data-limits"></a>Adatok korlátok
Nincsenek közzétett toohello Log Analytics-adatok gyűjtése API hello adatok körül bizonyos korlátozások.

* Legfeljebb 30 MB post tooLog Analytics adatait gyűjtője API. Ez az egyetlen post méretkorlátot. Ha nagyobb 30 MB egyetlen post hello adatait, kell felosztása hello adatok mentése toosmaller méretű adattömböket, és küldje el egyszerre.
* Legfeljebb 32 KB-os korlát mezők értékét. Ha hello mezőérték 32 KB-nál nagyobb, hello adatok csonkolva lesz.
* Egy adott típusú mezőket ajánlott maximális száma érték az 50. Ez a használhatóság és keresési élményt szempontból gyakorlati korlátozni.  

## <a name="return-codes"></a>A visszatérési kódok
200-as HTTP-állapotkód: hello azt jelenti, hogy adott hello kérelmet kapott a feldolgozáshoz. Ez azt jelzi, hogy hello művelet sikeresen befejeződött.

Ez a táblázat hello szolgáltatás előfordulhat, hogy vissza állapotkódokat teljes készletének hello:

| Kód | status | Hibakód: | Leírás |
|:--- |:--- |:--- |:--- |
| 200 |OKÉ | |hello kérés sikeresen elfogadva. |
| 400 |Helytelen kérelem |InactiveCustomer |hello munkaterület be lett zárva. |
| 400 |Helytelen kérelem |InvalidApiVersion |a megadott hello API-verzió nem ismerte fel hello szolgáltatást. |
| 400 |Helytelen kérelem |InvalidCustomerId |a megadott hello munkaterület azonosítója érvénytelen. |
| 400 |Helytelen kérelem |InvalidDataFormat |Érvénytelen JSON el lett küldve. hello adott válasz törzsének hogyan tooresolve hello hiba további információt tartalmazhat. |
| 400 |Helytelen kérelem |InvalidLogType |hello típussal megadott tartalmazott különleges karaktereket vagy írhatók. |
| 400 |Helytelen kérelem |MissingApiVersion |hello API-verzió nincs megadva. |
| 400 |Helytelen kérelem |MissingContentType |hello tartalomtípus nincs megadva. |
| 400 |Helytelen kérelem |MissingLogType |hello szükséges érték napló típusa nincs megadva. |
| 400 |Helytelen kérelem |UnsupportedContentType |hello tartalomtípus túl nincs beállítva**az application/json**. |
| 403 |Tiltott |InvalidAuthorization |hello szolgáltatás tooauthenticate hello kérelem sikertelen volt. Hello munkaterület azonosítója és a kapcsolat kulcs érvényességének ellenőrzése. |
| 404 |Nem található | | Hello URL-cím helytelen, vagy hello kérelme, mert túl nagy. |
| 429 |Túl sok kérelem | | hello szolgáltatás tapasztal nagyszámú fiókja adatait. Próbálkozzon újra később hello kérelem. |
| 500 |Belső kiszolgálóhiba. |UnspecifiedError |hello szolgáltatás belső hibát észlelt. Próbálja meg újra hello kérelem. |
| 503 |A szolgáltatás nem érhető el |ServiceUnavailable |hello szolgáltatás jelenleg nem érhető el tooreceive kérelmeket. Próbálja megismételni a kérést. |

## <a name="query-data"></a>Adatok lekérdezése
hello napló Analytics HTTP adatokat gyűjtő API, keresse meg a rekordokat által küldött tooquery adatokat **típus** , amely egyenlő toohello **LogType** , amely a megadott érték hozzáíródik **_CL**. Például, ha a használt **MyCustomLog**, majd az összes rekordot alakítanák vissza **típus = MyCustomLog_CL**.

>[!NOTE]
> Ha a munkaterületet frissített toohello [új Log Analytics lekérdezési nyelv](log-analytics-log-search-upgrade.md), majd a fenti lekérdezés hello megváltozna toohello következő.

> `MyCustomLog_CL`

## <a name="sample-requests"></a>A minta-kérelmek
A következő szakaszokban hello látni fogja, hogy hogyan minták toosubmit adatok toohello napló Analytics HTTP adatokat gyűjtő API különböző programnyelveken használatával.

Minden egyes minta tooset hello változók hello engedélyezési fejléc tegye ezeket a lépéseket:

1. A hello Operations Management Suite portálját, válassza ki a hello **beállítások** csempére, majd válassza ki a hello **csatlakoztatott források** fülre.
2. jobbra toohello **munkaterület azonosítója**hello másolás ikon, között, majd illessze be a hello azonosító hello hello értékeként **ügyfél azonosítója** változó.
3. jobbra toohello **elsődleges kulcs**hello másolás ikon, között, majd illessze be a hello azonosító hello hello értékeként **megosztott kulcsos** változó.

Módosíthatja azt is megteheti, hello változók hello napló típusa és a JSON-adatokat.

### <a name="powershell-sample"></a>PowerShell-példa
```
# Replace with your Workspace ID
$CustomerId = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"  

# Replace with your Primary Key
$SharedKey = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"

# Specify hello name of hello record type that you'll be creating
$LogType = "MyRecordType"

# Specify a field with hello created time for hello records
$TimeStampField = "DateValue"


# Create two records with hello same set of properties toocreate
$json = @"
[{  "StringValue": "MyString1",
    "NumberValue": 42,
    "BooleanValue": true,
    "DateValue": "2016-05-12T20:00:00.625Z",
    "GUIDValue": "9909ED01-A74C-4874-8ABF-D2678E3AE23D"
},
{   "StringValue": "MyString2",
    "NumberValue": 43,
    "BooleanValue": false,
    "DateValue": "2016-05-12T20:00:00.625Z",
    "GUIDValue": "8809ED01-A74C-4874-8ABF-D2678E3AE23D"
}]
"@

# Create hello function toocreate hello authorization signature
Function Build-Signature ($customerId, $sharedKey, $date, $contentLength, $method, $contentType, $resource)
{
    $xHeaders = "x-ms-date:" + $date
    $stringToHash = $method + "`n" + $contentLength + "`n" + $contentType + "`n" + $xHeaders + "`n" + $resource

    $bytesToHash = [Text.Encoding]::UTF8.GetBytes($stringToHash)
    $keyBytes = [Convert]::FromBase64String($sharedKey)

    $sha256 = New-Object System.Security.Cryptography.HMACSHA256
    $sha256.Key = $keyBytes
    $calculatedHash = $sha256.ComputeHash($bytesToHash)
    $encodedHash = [Convert]::ToBase64String($calculatedHash)
    $authorization = 'SharedKey {0}:{1}' -f $customerId,$encodedHash
    return $authorization
}


# Create hello function toocreate and post hello request
Function Post-OMSData($customerId, $sharedKey, $body, $logType)
{
    $method = "POST"
    $contentType = "application/json"
    $resource = "/api/logs"
    $rfc1123date = [DateTime]::UtcNow.ToString("r")
    $contentLength = $body.Length
    $signature = Build-Signature `
        -customerId $customerId `
        -sharedKey $sharedKey `
        -date $rfc1123date `
        -contentLength $contentLength `
        -fileName $fileName `
        -method $method `
        -contentType $contentType `
        -resource $resource
    $uri = "https://" + $customerId + ".ods.opinsights.azure.com" + $resource + "?api-version=2016-04-01"

    $headers = @{
        "Authorization" = $signature;
        "Log-Type" = $logType;
        "x-ms-date" = $rfc1123date;
        "time-generated-field" = $TimeStampField;
    }

    $response = Invoke-WebRequest -Uri $uri -Method $method -ContentType $contentType -Headers $headers -Body $body -UseBasicParsing
    return $response.StatusCode

}

# Submit hello data toohello API endpoint
Post-OMSData -customerId $customerId -sharedKey $sharedKey -body ([System.Text.Encoding]::UTF8.GetBytes($json)) -logType $logType  
```

### <a name="c-sample"></a>C#-minta
```
using System;
using System.Net;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Security.Cryptography;
using System.Text;
using System.Threading.Tasks;

namespace OIAPIExample
{
    class ApiExample
    {
        // An example JSON object, with key/value pairs
        static string json = @"[{""DemoField1"":""DemoValue1"",""DemoField2"":""DemoValue2""},{""DemoField3"":""DemoValue3"",""DemoField4"":""DemoValue4""}]";

        // Update customerId tooyour Operations Management Suite workspace ID
        static string customerId = "xxxxxxxx-xxx-xxx-xxx-xxxxxxxxxxxx";

        // For sharedKey, use either hello primary or hello secondary Connected Sources client authentication key   
        static string sharedKey = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx";

        // LogName is name of hello event type that is being submitted tooLog Analytics
        static string LogName = "DemoExample";

        // You can use an optional field toospecify hello timestamp from hello data. If hello time field is not specified, Log Analytics assumes hello time is hello message ingestion time
        static string TimeStampField = "";

        static void Main()
        {
            // Create a hash for hello API signature
            var datestring = DateTime.UtcNow.ToString("r");
            string stringToHash = "POST\n" + json.Length + "\napplication/json\n" + "x-ms-date:" + datestring + "\n/api/logs";
            string hashedString = BuildSignature(stringToHash, sharedKey);
            string signature = "SharedKey " + customerId + ":" + hashedString;

            PostData(signature, datestring, json);
        }

        // Build hello API signature
        public static string BuildSignature(string message, string secret)
        {
            var encoding = new System.Text.ASCIIEncoding();
            byte[] keyByte = Convert.FromBase64String(secret);
            byte[] messageBytes = encoding.GetBytes(message);
            using (var hmacsha256 = new HMACSHA256(keyByte))
            {
                byte[] hash = hmacsha256.ComputeHash(messageBytes);
                return Convert.ToBase64String(hash);
            }
        }

        // Send a request toohello POST API endpoint
        public static void PostData(string signature, string date, string json)
        {
            try
            {
                string url = "https://" + customerId + ".ods.opinsights.azure.com/api/logs?api-version=2016-04-01";

                System.Net.Http.HttpClient client = new System.Net.Http.HttpClient();
                client.DefaultRequestHeaders.Add("Accept", "application/json");
                client.DefaultRequestHeaders.Add("Log-Type", LogName);
                client.DefaultRequestHeaders.Add("Authorization", signature);
                client.DefaultRequestHeaders.Add("x-ms-date", date);
                client.DefaultRequestHeaders.Add("time-generated-field", TimeStampField);

                System.Net.Http.HttpContent httpContent = new StringContent(json, Encoding.UTF8);
                httpContent.Headers.ContentType = new MediaTypeHeaderValue("application/json");
                Task<System.Net.Http.HttpResponseMessage> response = client.PostAsync(new Uri(url), httpContent);

                System.Net.Http.HttpContent responseContent = response.Result.Content;
                string result = responseContent.ReadAsStringAsync().Result;
                Console.WriteLine("Return Result: " + result);
            }
            catch (Exception excep)
            {
                Console.WriteLine("API Post Exception: " + excep.Message);
            }
        }
    }
}

```

### <a name="python-sample"></a>Python-minta
```
import json
import requests
import datetime
import hashlib
import hmac
import base64

# Update hello customer ID tooyour Operations Management Suite workspace ID
customer_id = 'xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'

# For hello shared key, use either hello primary or hello secondary Connected Sources client authentication key   
shared_key = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"

# hello log type is hello name of hello event that is being submitted
log_type = 'WebMonitorTest'

# An example JSON web monitor object
json_data = [{
   "slot_ID": 12345,
    "ID": "5cdad72f-c848-4df0-8aaa-ffe033e75d57",
    "availability_Value": 100,
    "performance_Value": 6.954,
    "measurement_Name": "last_one_hour",
    "duration": 3600,
    "warning_Threshold": 0,
    "critical_Threshold": 0,
    "IsActive": "true"
},
{   
    "slot_ID": 67890,
    "ID": "b6bee458-fb65-492e-996d-61c4d7fbb942",
    "availability_Value": 100,
    "performance_Value": 3.379,
    "measurement_Name": "last_one_hour",
    "duration": 3600,
    "warning_Threshold": 0,
    "critical_Threshold": 0,
    "IsActive": "false"
}]
body = json.dumps(json_data)

#####################
######Functions######  
#####################

# Build hello API signature
def build_signature(customer_id, shared_key, date, content_length, method, content_type, resource):
    x_headers = 'x-ms-date:' + date
    string_to_hash = method + "\n" + str(content_length) + "\n" + content_type + "\n" + x_headers + "\n" + resource
    bytes_to_hash = bytes(string_to_hash).encode('utf-8')  
    decoded_key = base64.b64decode(shared_key)
    encoded_hash = base64.b64encode(hmac.new(decoded_key, bytes_to_hash, digestmod=hashlib.sha256).digest())
    authorization = "SharedKey {}:{}".format(customer_id,encoded_hash)
    return authorization

# Build and send a request toohello POST API
def post_data(customer_id, shared_key, body, log_type):
    method = 'POST'
    content_type = 'application/json'
    resource = '/api/logs'
    rfc1123date = datetime.datetime.utcnow().strftime('%a, %d %b %Y %H:%M:%S GMT')
    content_length = len(body)
    signature = build_signature(customer_id, shared_key, rfc1123date, content_length, method, content_type, resource)
    uri = 'https://' + customer_id + '.ods.opinsights.azure.com' + resource + '?api-version=2016-04-01'

    headers = {
        'content-type': content_type,
        'Authorization': signature,
        'Log-Type': log_type,
        'x-ms-date': rfc1123date
    }

    response = requests.post(uri,data=body, headers=headers)
    if (response.status_code >= 200 and response.status_code <= 299):
        print 'Accepted'
    else:
        print "Response code: {}".format(response.status_code)

post_data(customer_id, shared_key, body, log_type)
```

## <a name="next-steps"></a>Következő lépések
- Használjon hello [napló Search API](log-analytics-log-search-api.md) adattárból hello Naplóelemzési tooretrieve adatokat.
