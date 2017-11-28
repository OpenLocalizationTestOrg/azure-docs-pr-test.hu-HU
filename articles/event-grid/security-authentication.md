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
# <a name="event-grid-security-and-authentication"></a><span data-ttu-id="116a6-103">Esemény rács biztonsági és hitelesítési</span><span class="sxs-lookup"><span data-stu-id="116a6-103">Event Grid security and authentication</span></span> 

<span data-ttu-id="116a6-104">Az Azure Event rács három típusú hitelesítés van:</span><span class="sxs-lookup"><span data-stu-id="116a6-104">Azure Event Grid has three types of authentication:</span></span>

* <span data-ttu-id="116a6-105">Esemény-előfizetések</span><span class="sxs-lookup"><span data-stu-id="116a6-105">Event subscriptions</span></span>
* <span data-ttu-id="116a6-106">Esemény közzététele</span><span class="sxs-lookup"><span data-stu-id="116a6-106">Event publishing</span></span>
* <span data-ttu-id="116a6-107">WebHook esemény kézbesítés</span><span class="sxs-lookup"><span data-stu-id="116a6-107">WebHook event delivery</span></span>

## <a name="webhook-event-delivery"></a><span data-ttu-id="116a6-108">WebHook esemény kézbesítés</span><span class="sxs-lookup"><span data-stu-id="116a6-108">WebHook Event delivery</span></span>

<span data-ttu-id="116a6-109">Webhook rendszer egyik számos módon tooreceive esemény Azure esemény rácsban valós időben.</span><span class="sxs-lookup"><span data-stu-id="116a6-109">Webhooks are one of many ways tooreceive events in real time from Azure Event Grid.</span></span>

<span data-ttu-id="116a6-110">Minden alkalommal, amikor egy új készen esemény toobe kézbesíteni van, a esemény rács tooyour WebHook hello eseménnyel együtt egy HTTP-kérést küld hello törzsében.</span><span class="sxs-lookup"><span data-stu-id="116a6-110">Every time there is a new event ready toobe delivered, Event Grid sends an HTTP request with tooyour WebHook with hello event in hello body.</span></span>

<span data-ttu-id="116a6-111">Esemény rácshoz is regisztrálhatja a saját WebHook végpont, ismerhetők meg egy POST kérést egy egyszerű érvényesítési kódot a rendelés tooprove végpont tulajdonosa.</span><span class="sxs-lookup"><span data-stu-id="116a6-111">When you register your own WebHook endpoint with Event Grid, it sends you a POST request with a simple validation code in order tooprove endpoint ownership.</span></span> <span data-ttu-id="116a6-112">Az alkalmazás által hátsó hello Ellenőrzőkód echo toorespond kell.</span><span class="sxs-lookup"><span data-stu-id="116a6-112">Your app needs toorespond by echoing back hello validation code.</span></span> <span data-ttu-id="116a6-113">Esemény rács nem továbbítja események tooWebHook végpontok, amelyek még nem telt hello érvényesítése.</span><span class="sxs-lookup"><span data-stu-id="116a6-113">Event Grid will not deliver events tooWebHook endpoints that have not passed hello validation.</span></span>
 
### <a name="validation-details"></a><span data-ttu-id="116a6-114">Ellenőrzési részletek:</span><span class="sxs-lookup"><span data-stu-id="116a6-114">Validation details:</span></span>

* <span data-ttu-id="116a6-115">Esemény előfizetés létrehozása/frissítése a hello időpontjában esemény rács visszaküldés "SubscriptionValidationEvent" esemény toohello cél-végponthoz.</span><span class="sxs-lookup"><span data-stu-id="116a6-115">At hello time of event subscription creation/update, Event Grid posts a “SubscriptionValidationEvent” event toohello target endpoint.</span></span>
* <span data-ttu-id="116a6-116">hello esemény tartalmazza az "Esemény-típus: érvényesítése" fejléc értéke.</span><span class="sxs-lookup"><span data-stu-id="116a6-116">hello event contains a header value “Event-Type: Validation”.</span></span>
* <span data-ttu-id="116a6-117">hello esemény törzsében van hello azonos sémából más esemény rács eseményként is rögzíti.</span><span class="sxs-lookup"><span data-stu-id="116a6-117">hello event body has hello same schema as other Event Grid events.</span></span>
* <span data-ttu-id="116a6-118">hello eseményadatok pl. tartalmaz egy "ValidationCode" tulajdonság egy véletlenszerűen generált karakterlánccal "ValidationCode: acb13...".</span><span class="sxs-lookup"><span data-stu-id="116a6-118">hello event data includes a “ValidationCode” property with a randomly generated string e.g. “ValidationCode: acb13…”.</span></span>

<span data-ttu-id="116a6-119">Rendelés tooprove végpont tulajdonosa az echo hátsó hello érvényesítési kód például "ValidationResponse: acb13...".</span><span class="sxs-lookup"><span data-stu-id="116a6-119">In order tooprove endpoint ownership, echo back hello validation code e.g “ValidationResponse: acb13…”.</span></span>

<span data-ttu-id="116a6-120">Végezetül fontos, hogy a Azure esemény rács csak támogatja-e a HTTPS-végpontnak webhook toonote.</span><span class="sxs-lookup"><span data-stu-id="116a6-120">Finally, it is important toonote that Azure Event Grid only supports HTTPS webhook endpoints.</span></span>
## <a name="event-subscription"></a><span data-ttu-id="116a6-121">Az esemény-előfizetés</span><span class="sxs-lookup"><span data-stu-id="116a6-121">Event subscription</span></span>

<span data-ttu-id="116a6-122">toosubscribe tooan esemény, rendelkeznie kell hello **Microsoft.EventGrid/EventSubscriptions/Write** hello engedély szükséges erőforrás.</span><span class="sxs-lookup"><span data-stu-id="116a6-122">toosubscribe tooan event, you must have hello **Microsoft.EventGrid/EventSubscriptions/Write** permission on hello required resource.</span></span> <span data-ttu-id="116a6-123">Meg kell ezt az engedélyt, mivel hello erőforrás hello hatókörben egy új előfizetés írása.</span><span class="sxs-lookup"><span data-stu-id="116a6-123">You need this permission because you are writing a new subscription at hello scope of hello resource.</span></span> <span data-ttu-id="116a6-124">hello szükséges erőforrás eltér e előfizetés egyéni vagy tooa rendszer témakört.</span><span class="sxs-lookup"><span data-stu-id="116a6-124">hello required resource differs based on whether you are subscribing tooa system topic or custom topic.</span></span> <span data-ttu-id="116a6-125">Ez a szakasz ismerteti a kétféle típusú.</span><span class="sxs-lookup"><span data-stu-id="116a6-125">Both types are described in this section.</span></span>

### <a name="system-topics-azure-service-publishers"></a><span data-ttu-id="116a6-126">Rendszer témakörök (az Azure szolgáltatáshoz kiadók)</span><span class="sxs-lookup"><span data-stu-id="116a6-126">System topics (Azure service publishers)</span></span>

<span data-ttu-id="116a6-127">A rendszer témakörök kell engedély toowrite egy új Eseményelőfizetés hello hatókörből hello erőforrás közzétételi hello esemény.</span><span class="sxs-lookup"><span data-stu-id="116a6-127">For system topics, you need permission toowrite a new event subscription at hello scope of hello resource publishing hello event.</span></span> <span data-ttu-id="116a6-128">hello hello erőforrás formátuma:`/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/{resource-provider}/{resource-type}/{resource-name}`</span><span class="sxs-lookup"><span data-stu-id="116a6-128">hello format of hello resource is: `/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/{resource-provider}/{resource-type}/{resource-name}`</span></span>

<span data-ttu-id="116a6-129">Például toosubscribe tooan esemény nevű tárfiók a **fióknév**, a hello Microsoft.EventGrid/EventSubscriptions/Write engedélyre van szüksége:`/subscriptions/####/resourceGroups/testrg/providers/Microsoft.Storage/storageAccounts/myacct`</span><span class="sxs-lookup"><span data-stu-id="116a6-129">For example, toosubscribe tooan event on a storage account named **myacct**, you need hello Microsoft.EventGrid/EventSubscriptions/Write permission on: `/subscriptions/####/resourceGroups/testrg/providers/Microsoft.Storage/storageAccounts/myacct`</span></span>

### <a name="custom-topics"></a><span data-ttu-id="116a6-130">Egyéni kapcsolatos témakörök</span><span class="sxs-lookup"><span data-stu-id="116a6-130">Custom topics</span></span>

<span data-ttu-id="116a6-131">Egyéni témakörök kell engedély toowrite egy új Eseményelőfizetés hello hatókörből hello esemény rács témakör.</span><span class="sxs-lookup"><span data-stu-id="116a6-131">For custom topics, you need permission toowrite a new event subscription at hello scope of hello Event Grid topic.</span></span> <span data-ttu-id="116a6-132">hello hello erőforrás formátuma:`/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.EventGrid/topics/{topic-name}`</span><span class="sxs-lookup"><span data-stu-id="116a6-132">hello format of hello resource is: `/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.EventGrid/topics/{topic-name}`</span></span>

<span data-ttu-id="116a6-133">Például toosubscribe tooa egyéni nevű üzenettémakör **mytopic**, a hello Microsoft.EventGrid/EventSubscriptions/Write engedélyre van szüksége:`/subscriptions/####/resourceGroups/testrg/providers/Microsoft.EventGrid/topics/mytopic`</span><span class="sxs-lookup"><span data-stu-id="116a6-133">For example, toosubscribe tooa custom topic named **mytopic**, you need hello Microsoft.EventGrid/EventSubscriptions/Write permission on: `/subscriptions/####/resourceGroups/testrg/providers/Microsoft.EventGrid/topics/mytopic`</span></span>

## <a name="topic-publishing"></a><span data-ttu-id="116a6-134">A témakör a közzététel</span><span class="sxs-lookup"><span data-stu-id="116a6-134">Topic publishing</span></span>

<span data-ttu-id="116a6-135">Témakörök használja, vagy a közös hozzáférésű Jogosultságkód (SAS), vagy a hitelesítés.</span><span class="sxs-lookup"><span data-stu-id="116a6-135">Topics use either Shared Access Signature (SAS) or key authentication.</span></span> <span data-ttu-id="116a6-136">Azt javasoljuk, hogy SAS, de hitelesítés egyszerű programozási biztosít, és sok meglévő webhook közzétevők kompatibilis.</span><span class="sxs-lookup"><span data-stu-id="116a6-136">We recommend SAS, but key authentication provides simple programming, and is compatible with many existing webhook publishers.</span></span> 

<span data-ttu-id="116a6-137">Hello HTTP-fejléc hello hitelesítési érték szerepel.</span><span class="sxs-lookup"><span data-stu-id="116a6-137">You include hello authentication value in hello HTTP header.</span></span> <span data-ttu-id="116a6-138">SAS-használata **AEG ügyet-sas-jogkivonat** a hello állomásfejléc-érték.</span><span class="sxs-lookup"><span data-stu-id="116a6-138">For SAS, use **aeg-sas-token** for hello header value.</span></span> <span data-ttu-id="116a6-139">A hitelesítés, használjon **AEG ügyet-sas-kulcs** a hello állomásfejléc-érték.</span><span class="sxs-lookup"><span data-stu-id="116a6-139">For key authentication, use **aeg-sas-key** for hello header value.</span></span>

### <a name="key-authentication"></a><span data-ttu-id="116a6-140">Hitelesítés</span><span class="sxs-lookup"><span data-stu-id="116a6-140">Key authentication</span></span>

<span data-ttu-id="116a6-141">Hitelesítés a hitelesítési hello legegyszerűbb formája, amely.</span><span class="sxs-lookup"><span data-stu-id="116a6-141">Key authentication is hello simplest form of authentication.</span></span> <span data-ttu-id="116a6-142">Hello formátumot használja:`aeg-sas-key: <your key>`</span><span class="sxs-lookup"><span data-stu-id="116a6-142">Use hello format: `aeg-sas-key: <your key>`</span></span>

<span data-ttu-id="116a6-143">Például adja meg egy kulcs:</span><span class="sxs-lookup"><span data-stu-id="116a6-143">For example, you pass a key with:</span></span> 

```
aeg-sas-key: VXbGWce53249Mt8wuotr0GPmyJ/nDT4hgdEj9DpBeRr38arnnm5OFg==
```

### <a name="sas-tokens"></a><span data-ttu-id="116a6-144">SAS-tokenek</span><span class="sxs-lookup"><span data-stu-id="116a6-144">SAS tokens</span></span>

<span data-ttu-id="116a6-145">SAS-tokenje esemény rács hello erőforrás lejárati időt és aláírás tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="116a6-145">SAS tokens for Event Grid include hello resource, an expiration time, and a signature.</span></span> <span data-ttu-id="116a6-146">hello hello SAS-jogkivonat formátuma: `r={resource}&e={expiration}&s={signature}`.</span><span class="sxs-lookup"><span data-stu-id="116a6-146">hello format of hello SAS token is: `r={resource}&e={expiration}&s={signature}`.</span></span>

<span data-ttu-id="116a6-147">hello erőforrás hello elérési útja a hello témakör toowhich küldött események.</span><span class="sxs-lookup"><span data-stu-id="116a6-147">hello resource is hello path for hello topic toowhich you are sending events.</span></span> <span data-ttu-id="116a6-148">Például egy érvényes erőforrás elérési útja:`https://<yourtopic>.<region>.eventgrid.azure.net/eventGrid/api/events`</span><span class="sxs-lookup"><span data-stu-id="116a6-148">For example, a valid resource path is: `https://<yourtopic>.<region>.eventgrid.azure.net/eventGrid/api/events`</span></span>

<span data-ttu-id="116a6-149">Hello aláírás generálása egy kulcsot.</span><span class="sxs-lookup"><span data-stu-id="116a6-149">You generate hello signature from a key.</span></span>

<span data-ttu-id="116a6-150">Például egy érvényes **AEG ügyet-sas-jogkivonat** érték:</span><span class="sxs-lookup"><span data-stu-id="116a6-150">For example, a valid **aeg-sas-token** value is:</span></span>

```http
aeg-sas-token: r=https%3a%2f%2fmytopic.eventgrid.azure.net%2feventGrid%2fapi%2fevent&e=6%2f15%2f2017+6%3a20%3a15+PM&s=a4oNHpRZygINC%2fBPjdDLOrc6THPy3tDcGHw1zP4OajQ%3d
``` 

<span data-ttu-id="116a6-151">a következő példa hello egy SAS-jogkivonat használata esemény rácshoz hozza létre:</span><span class="sxs-lookup"><span data-stu-id="116a6-151">hello following example creates a SAS token for use with Event Grid:</span></span>

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

## <a name="management-access-control"></a><span data-ttu-id="116a6-152">Felügyeleti hozzáférés-vezérlés</span><span class="sxs-lookup"><span data-stu-id="116a6-152">Management Access Control</span></span>

<span data-ttu-id="116a6-153">Azure esemény rács lehetővé teszi, hogy Ön toocontrol hello szintű hozzáférés toodifferent felhasználók toodo különböző felügyeleti műveleteket, például az esemény-előfizetések listája, hozzon létre újakat, és a kulcsok létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="116a6-153">Azure Event Grid allows you toocontrol hello level of access given toodifferent users toodo various management operations such as list event subscriptions, create new ones, and generate keys.</span></span> <span data-ttu-id="116a6-154">Esemény rács ehhez használja az Azure szerepköralapú hozzáférés ellenőrzése (RBAC).</span><span class="sxs-lookup"><span data-stu-id="116a6-154">Event Grid utilizes Azure's Role Based Access Check (RBAC) for this.</span></span>

### <a name="operation-types"></a><span data-ttu-id="116a6-155">Művelet típusa</span><span class="sxs-lookup"><span data-stu-id="116a6-155">Operation types</span></span>

<span data-ttu-id="116a6-156">Az Azure event rács hello a következő műveleteket támogatja:</span><span class="sxs-lookup"><span data-stu-id="116a6-156">Azure event grid supports hello following actions:</span></span>

* <span data-ttu-id="116a6-157">Microsoft.EventGrid/*/read</span><span class="sxs-lookup"><span data-stu-id="116a6-157">Microsoft.EventGrid/*/read</span></span> 
* <span data-ttu-id="116a6-158">Microsoft.EventGrid/*/write</span><span class="sxs-lookup"><span data-stu-id="116a6-158">Microsoft.EventGrid/*/write</span></span> 
* <span data-ttu-id="116a6-159">Microsoft.EventGrid/*/delete</span><span class="sxs-lookup"><span data-stu-id="116a6-159">Microsoft.EventGrid/*/delete</span></span> 
* <span data-ttu-id="116a6-160">Microsoft.EventGrid/eventSubscriptions/getFullUrl/action</span><span class="sxs-lookup"><span data-stu-id="116a6-160">Microsoft.EventGrid/eventSubscriptions/getFullUrl/action</span></span> 
* <span data-ttu-id="116a6-161">Microsoft.EventGrid/topics/listKeys/action</span><span class="sxs-lookup"><span data-stu-id="116a6-161">Microsoft.EventGrid/topics/listKeys/action</span></span> 
* <span data-ttu-id="116a6-162">Microsoft.EventGrid/topics/regenerateKey/action</span><span class="sxs-lookup"><span data-stu-id="116a6-162">Microsoft.EventGrid/topics/regenerateKey/action</span></span>

<span data-ttu-id="116a6-163">hello utolsó három műveletek visszatérési potenciálisan titkos információk, amelyek normál olvasási műveletek lekérdezi szűrve.</span><span class="sxs-lookup"><span data-stu-id="116a6-163">hello last three operations return potentially secret information which gets filtered out of normal read operations.</span></span> <span data-ttu-id="116a6-164">Ajánlott eljárás az Ön toorestrict toothese műveletek is.</span><span class="sxs-lookup"><span data-stu-id="116a6-164">It is best practice for you toorestrict access toothese operations.</span></span> <span data-ttu-id="116a6-165">Egyéni szerepkörök segítségével hozhatók létre [Azure PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md), [Azure parancssori felület (CLI)](../active-directory/role-based-access-control-manage-access-azure-cli.md), és hello [REST API](../active-directory/role-based-access-control-manage-access-rest.md).</span><span class="sxs-lookup"><span data-stu-id="116a6-165">Custom roles can be created using [Azure PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md), [Azure Command-Line Interface (CLI)](../active-directory/role-based-access-control-manage-access-azure-cli.md), and hello [REST API](../active-directory/role-based-access-control-manage-access-rest.md).</span></span>

### <a name="enforcing-role-based-access-check-rbac"></a><span data-ttu-id="116a6-166">Érvényesítési szerepkör alapú hozzáférés-ellenőrzést (RBAC)</span><span class="sxs-lookup"><span data-stu-id="116a6-166">Enforcing Role Based Access Check (RBAC)</span></span>

<span data-ttu-id="116a6-167">A következő lépéseket tooenforce RBAC a különböző felhasználók számára hello használata:</span><span class="sxs-lookup"><span data-stu-id="116a6-167">Use hello following steps tooenforce RBAC for different users:</span></span>

#### <a name="create-a-custom-role-definition-file-json"></a><span data-ttu-id="116a6-168">Hozzon létre egy egyéni szerepkör-definíció fájlt (.JSON kiterjesztésű)</span><span class="sxs-lookup"><span data-stu-id="116a6-168">Create a custom role definition file (.json)</span></span>

<span data-ttu-id="116a6-169">Az alábbiakban hello minta esemény rács szerepkör-definíciók, amelyen a felhasználók más-más részhalmazához tooperform műveleteket.</span><span class="sxs-lookup"><span data-stu-id="116a6-169">hello following are sample Event Grid role definitions which allow users tooperform different sets of actions.</span></span>

<span data-ttu-id="116a6-170">**EventGridReadOnlyRole.json**: csak a csak olvasási műveletek engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="116a6-170">**EventGridReadOnlyRole.json**: Only allow read only operations.</span></span>
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

<span data-ttu-id="116a6-171">**EventGridNoDeleteListKeysRole.json**: engedélyezze a korlátozott utáni műveletek azonban engedélyezi a törlési műveletek naplóját.</span><span class="sxs-lookup"><span data-stu-id="116a6-171">**EventGridNoDeleteListKeysRole.json**: Allow restricted post actions but disallow delete actions.</span></span>
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
 
<span data-ttu-id="116a6-172">**EventGridContributorRole.json**: lehetővé teszi, hogy az összes esemény rács műveletek.</span><span class="sxs-lookup"><span data-stu-id="116a6-172">**EventGridContributorRole.json**: Allows all event grid actions.</span></span>  
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

#### <a name="install-and-login-tooazure-cli"></a><span data-ttu-id="116a6-173">Telepítés és a bejelentkezés tooAzure parancssori felület</span><span class="sxs-lookup"><span data-stu-id="116a6-173">Install and login tooAzure CLI</span></span>

* <span data-ttu-id="116a6-174">Az Azure CLI 0.8.8 verzió vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="116a6-174">Azure CLI version 0.8.8 or later.</span></span> <span data-ttu-id="116a6-175">tooinstall hello legújabb verzióját, és rendelje azt az Azure-előfizetéshez, lásd: [telepítése és konfigurálása az Azure parancssori felület hello](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="116a6-175">tooinstall hello latest version and associate it with your Azure subscription, see [Install and Configure hello Azure CLI](../cli-install-nodejs.md).</span></span>
* <span data-ttu-id="116a6-176">Az Azure Resource Manager az Azure parancssori felület.</span><span class="sxs-lookup"><span data-stu-id="116a6-176">Azure Resource Manager in Azure CLI.</span></span> <span data-ttu-id="116a6-177">Nyissa meg túl[Using hello Azure CLI az erőforrás-kezelő hello](../xplat-cli-azure-resource-manager.md) további részleteket.</span><span class="sxs-lookup"><span data-stu-id="116a6-177">Go too[Using hello Azure CLI with hello Resource Manager](../xplat-cli-azure-resource-manager.md) for more details.</span></span>

#### <a name="create-a-custom-role"></a><span data-ttu-id="116a6-178">Egyéni szerepkör létrehozása</span><span class="sxs-lookup"><span data-stu-id="116a6-178">Create a custom role</span></span>

<span data-ttu-id="116a6-179">egy egyéni biztonsági szerepkört, toocreate használja:</span><span class="sxs-lookup"><span data-stu-id="116a6-179">toocreate a custom role, use:</span></span>

    azure role create --inputfile <file path>

#### <a name="assign-hello-role-tooa-user"></a><span data-ttu-id="116a6-180">Hello szerepkör tooa felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="116a6-180">Assign hello role tooa user</span></span>


    azure role assignment create --signInName  <user email address> --roleName "<name of role>" --resourceGroup <resource group name>


## <a name="next-steps"></a><span data-ttu-id="116a6-181">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="116a6-181">Next steps</span></span>

* <span data-ttu-id="116a6-182">Egy bevezető tooEvent rács, lásd: [esemény rács](overview.md)</span><span class="sxs-lookup"><span data-stu-id="116a6-182">For an introduction tooEvent Grid, see [About Event Grid](overview.md)</span></span>
