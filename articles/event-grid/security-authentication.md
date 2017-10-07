---
title: "aaaAzure esemény rács biztonsági és hitelesítési"
description: "Azure Event rács és a fogalmakat ismerteti."
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 08/14/2017
ms.author: babanisa
ms.openlocfilehash: 74f692c7e3a30856c3a80cbbfe82a26bf4c1c9b5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="event-grid-security-and-authentication"></a>Esemény rács biztonsági és hitelesítési 

Az Azure Event rács három típusú hitelesítés van:

* Esemény-előfizetések
* Esemény közzététele
* WebHook esemény kézbesítés

## <a name="webhook-event-delivery"></a>WebHook esemény kézbesítés

Webhook rendszer egyik számos módon tooreceive esemény Azure esemény rácsban valós időben.

Minden alkalommal, amikor egy új készen esemény toobe kézbesíteni van, a esemény rács tooyour WebHook hello eseménnyel együtt egy HTTP-kérést küld hello törzsében.

Esemény rácshoz is regisztrálhatja a saját WebHook végpont, ismerhetők meg egy POST kérést egy egyszerű érvényesítési kódot a rendelés tooprove végpont tulajdonosa. Az alkalmazás által hátsó hello Ellenőrzőkód echo toorespond kell. Esemény rács nem továbbítja események tooWebHook végpontok, amelyek még nem telt hello érvényesítése.
 
### <a name="validation-details"></a>Ellenőrzési részletek:

* Esemény előfizetés létrehozása/frissítése a hello időpontjában esemény rács visszaküldés "SubscriptionValidationEvent" esemény toohello cél-végponthoz.
* hello esemény tartalmazza az "Esemény-típus: érvényesítése" fejléc értéke.
* hello esemény törzsében van hello azonos sémából más esemény rács eseményként is rögzíti.
* hello eseményadatok pl. tartalmaz egy "ValidationCode" tulajdonság egy véletlenszerűen generált karakterlánccal "ValidationCode: acb13...".

Rendelés tooprove végpont tulajdonosa az echo hátsó hello érvényesítési kód például "ValidationResponse: acb13...".

Végezetül fontos, hogy a Azure esemény rács csak támogatja-e a HTTPS-végpontnak webhook toonote.
## <a name="event-subscription"></a>Az esemény-előfizetés

toosubscribe tooan esemény, rendelkeznie kell hello **Microsoft.EventGrid/EventSubscriptions/Write** hello engedély szükséges erőforrás. Meg kell ezt az engedélyt, mivel hello erőforrás hello hatókörben egy új előfizetés írása. hello szükséges erőforrás eltér e előfizetés egyéni vagy tooa rendszer témakört. Ez a szakasz ismerteti a kétféle típusú.

### <a name="system-topics-azure-service-publishers"></a>Rendszer témakörök (az Azure szolgáltatáshoz kiadók)

A rendszer témakörök kell engedély toowrite egy új Eseményelőfizetés hello hatókörből hello erőforrás közzétételi hello esemény. hello hello erőforrás formátuma:`/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/{resource-provider}/{resource-type}/{resource-name}`

Például toosubscribe tooan esemény nevű tárfiók a **fióknév**, a hello Microsoft.EventGrid/EventSubscriptions/Write engedélyre van szüksége:`/subscriptions/####/resourceGroups/testrg/providers/Microsoft.Storage/storageAccounts/myacct`

### <a name="custom-topics"></a>Egyéni kapcsolatos témakörök

Egyéni témakörök kell engedély toowrite egy új Eseményelőfizetés hello hatókörből hello esemény rács témakör. hello hello erőforrás formátuma:`/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.EventGrid/topics/{topic-name}`

Például toosubscribe tooa egyéni nevű üzenettémakör **mytopic**, a hello Microsoft.EventGrid/EventSubscriptions/Write engedélyre van szüksége:`/subscriptions/####/resourceGroups/testrg/providers/Microsoft.EventGrid/topics/mytopic`

## <a name="topic-publishing"></a>A témakör a közzététel

Témakörök használja, vagy a közös hozzáférésű Jogosultságkód (SAS), vagy a hitelesítés. Azt javasoljuk, hogy SAS, de hitelesítés egyszerű programozási biztosít, és sok meglévő webhook közzétevők kompatibilis. 

Hello HTTP-fejléc hello hitelesítési érték szerepel. SAS-használata **AEG ügyet-sas-jogkivonat** a hello állomásfejléc-érték. A hitelesítés, használjon **AEG ügyet-sas-kulcs** a hello állomásfejléc-érték.

### <a name="key-authentication"></a>Hitelesítés

Hitelesítés a hitelesítési hello legegyszerűbb formája, amely. Hello formátumot használja:`aeg-sas-key: <your key>`

Például adja meg egy kulcs: 

```
aeg-sas-key: VXbGWce53249Mt8wuotr0GPmyJ/nDT4hgdEj9DpBeRr38arnnm5OFg==
```

### <a name="sas-tokens"></a>SAS-tokenek

SAS-tokenje esemény rács hello erőforrás lejárati időt és aláírás tartalmazza. hello hello SAS-jogkivonat formátuma: `r={resource}&e={expiration}&s={signature}`.

hello erőforrás hello elérési útja a hello témakör toowhich küldött események. Például egy érvényes erőforrás elérési útja:`https://<yourtopic>.<region>.eventgrid.azure.net/eventGrid/api/events`

Hello aláírás generálása egy kulcsot.

Például egy érvényes **AEG ügyet-sas-jogkivonat** érték:

```http
aeg-sas-token: r=https%3a%2f%2fmytopic.eventgrid.azure.net%2feventGrid%2fapi%2fevent&e=6%2f15%2f2017+6%3a20%3a15+PM&s=a4oNHpRZygINC%2fBPjdDLOrc6THPy3tDcGHw1zP4OajQ%3d
``` 

a következő példa hello egy SAS-jogkivonat használata esemény rácshoz hozza létre:

```cs
static string BuildSharedAccessSignature(string resource, DateTime expirationUtc, string key)
{
    const char Resource = 'r';
    const char Expiration = 'e';
    const char Signature = 's';

    string encodedResource = HttpUtility.UrlEncode(resource);
    string encodedExpirationUtc = HttpUtility.UrlEncode(expirationUtc.ToString());

    string unsignedSas = $"{Resource}={encodedResource}&{Expiration}={encodedExpirationUtc}";
    using (var hmac = new HMACSHA256(Convert.FromBase64String(key)))
    {
        string signature = Convert.ToBase64String(hmac.ComputeHash(Encoding.UTF8.GetBytes(unsignedSas)));
        string encodedSignature = HttpUtility.UrlEncode(signature);
        string signedSas = $"{unsignedSas}&{Signature}={encodedSignature}";

        return signedSas;
    }
}
```

## <a name="management-access-control"></a>Felügyeleti hozzáférés-vezérlés

Azure esemény rács lehetővé teszi, hogy Ön toocontrol hello szintű hozzáférés toodifferent felhasználók toodo különböző felügyeleti műveleteket, például az esemény-előfizetések listája, hozzon létre újakat, és a kulcsok létrehozásához. Esemény rács ehhez használja az Azure szerepköralapú hozzáférés ellenőrzése (RBAC).

### <a name="operation-types"></a>Művelet típusa

Az Azure event rács hello a következő műveleteket támogatja:

* Microsoft.EventGrid/*/read 
* Microsoft.EventGrid/*/write 
* Microsoft.EventGrid/*/delete 
* Microsoft.EventGrid/eventSubscriptions/getFullUrl/action 
* Microsoft.EventGrid/topics/listKeys/action 
* Microsoft.EventGrid/topics/regenerateKey/action

hello utolsó három műveletek visszatérési potenciálisan titkos információk, amelyek normál olvasási műveletek lekérdezi szűrve. Ajánlott eljárás az Ön toorestrict toothese műveletek is. Egyéni szerepkörök segítségével hozhatók létre [Azure PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md), [Azure parancssori felület (CLI)](../active-directory/role-based-access-control-manage-access-azure-cli.md), és hello [REST API](../active-directory/role-based-access-control-manage-access-rest.md).

### <a name="enforcing-role-based-access-check-rbac"></a>Érvényesítési szerepkör alapú hozzáférés-ellenőrzést (RBAC)

A következő lépéseket tooenforce RBAC a különböző felhasználók számára hello használata:

#### <a name="create-a-custom-role-definition-file-json"></a>Hozzon létre egy egyéni szerepkör-definíció fájlt (.JSON kiterjesztésű)

Az alábbiakban hello minta esemény rács szerepkör-definíciók, amelyen a felhasználók más-más részhalmazához tooperform műveleteket.

**EventGridReadOnlyRole.json**: csak a csak olvasási műveletek engedélyezése.
```json
{ 
  "Name": "Event grid read only role", 
  "Id": "7C0B6B59-A278-4B62-BA19-411B70753856", 
  "IsCustom": true, 
  "Description": "Event grid read only role", 
  "Actions": [ 
    "Microsoft.EventGrid/*/read" 
  ], 
  "NotActions": [ 
  ], 
  "AssignableScopes": [ 
    "/subscriptions/<Subscription Id>" 
  ] 
}
```

**EventGridNoDeleteListKeysRole.json**: engedélyezze a korlátozott utáni műveletek azonban engedélyezi a törlési műveletek naplóját.
```json
{ 
  "Name": "Event grid No Delete Listkeys role", 
  "Id": "B9170838-5F9D-4103-A1DE-60496F7C9174", 
  "IsCustom": true, 
  "Description": "Event grid No Delete Listkeys role", 
  "Actions": [     
    "Microsoft.EventGrid/*/write", 
    "Microsoft.EventGrid/eventSubscriptions/getFullUrl/action" 
    "Microsoft.EventGrid/topics/listkeys/action", 
    "Microsoft.EventGrid/topics/regenerateKey/action" 
  ], 
  "NotActions": [ 
    "Microsoft.EventGrid/*/delete" 
  ], 
  "AssignableScopes": [ 
    "/subscriptions/<Subscription id>" 
  ] 
}
```
 
**EventGridContributorRole.json**: lehetővé teszi, hogy az összes esemény rács műveletek.  
```json
{ 
  "Name": "Event grid contributor role", 
  "Id": "4BA6FB33-2955-491B-A74F-53C9126C9514", 
  "IsCustom": true, 
  "Description": "Event grid contributor role", 
  "Actions": [ 
    "Microsoft.EventGrid/*/write", 
    "Microsoft.EventGrid/*/delete", 
    "Microsoft.EventGrid/topics/listkeys/action", 
    "Microsoft.EventGrid/topics/regenerateKey/action", 
    "Microsoft.EventGrid/eventSubscriptions/getFullUrl/action" 
  ], 
  "NotActions": [], 
  "AssignableScopes": [ 
    "/subscriptions/d48566a8-2428-4a6c-8347-9675d09fb851" 
  ] 
} 
```

#### <a name="install-and-login-tooazure-cli"></a>Telepítés és a bejelentkezés tooAzure parancssori felület

* Az Azure CLI 0.8.8 verzió vagy újabb. tooinstall hello legújabb verzióját, és rendelje azt az Azure-előfizetéshez, lásd: [telepítése és konfigurálása az Azure parancssori felület hello](../cli-install-nodejs.md).
* Az Azure Resource Manager az Azure parancssori felület. Nyissa meg túl[Using hello Azure CLI az erőforrás-kezelő hello](../xplat-cli-azure-resource-manager.md) további részleteket.

#### <a name="create-a-custom-role"></a>Egyéni szerepkör létrehozása

egy egyéni biztonsági szerepkört, toocreate használja:

    azure role create --inputfile <file path>

#### <a name="assign-hello-role-tooa-user"></a>Hello szerepkör tooa felhasználó hozzárendelése


    azure role assignment create --signInName  <user email address> --roleName "<name of role>" --resourceGroup <resource group name>


## <a name="next-steps"></a>Következő lépések

* Egy bevezető tooEvent rács, lásd: [esemény rács](overview.md)
