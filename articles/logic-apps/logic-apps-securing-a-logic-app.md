---
title: "aaaSecure hozzáférési tooAzure Logic Apps |} Microsoft Docs"
description: "Adja hozzá a biztonsági hozzáférési tootriggers, bemeneti adatokat és kimenetek, művelet paramétereinek és a munkafolyamatok az Azure Logic Apps szolgáltatás védelmére."
services: logic-apps
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: anneta
editor: 
ms.assetid: 9fab1050-cfbc-4a8b-b1b3-5531bee92856
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 11/22/2016
ms.author: LADocs; jehollan
ms.openlocfilehash: abda2179e4cc2d2295cd8332ec017c848a456264
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="secure-access-tooyour-logic-apps"></a><span data-ttu-id="d3a3e-103">Biztonságos hozzáférés tooyour logic Apps alkalmazások</span><span class="sxs-lookup"><span data-stu-id="d3a3e-103">Secure access tooyour logic apps</span></span>

<span data-ttu-id="d3a3e-104">Nincsenek sok eszközök elérhető toohelp mindig lássa a logikai alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="d3a3e-104">There are many tools available toohelp you secure your logic app.</span></span>

* <span data-ttu-id="d3a3e-105">Biztonságossá tétele a hozzáférés tootrigger logikai alkalmazás (HTTP kérelem eseményindító)</span><span class="sxs-lookup"><span data-stu-id="d3a3e-105">Securing access tootrigger a logic app (HTTP Request Trigger)</span></span>
* <span data-ttu-id="d3a3e-106">Biztonságossá tétele a hozzáférés toomanage, szerkesztése, vagy olvassa el a logikai alkalmazás</span><span class="sxs-lookup"><span data-stu-id="d3a3e-106">Securing access toomanage, edit, or read a logic app</span></span>
* <span data-ttu-id="d3a3e-107">Hozzáférés toocontents bemeneti és kimeneti futtató biztonságossá tétele</span><span class="sxs-lookup"><span data-stu-id="d3a3e-107">Securing access toocontents of inputs and outputs for a run</span></span>
* <span data-ttu-id="d3a3e-108">Paraméterek vagy a műveletek a munkafolyamat belül biztonságossá tétele</span><span class="sxs-lookup"><span data-stu-id="d3a3e-108">Securing parameters or inputs within actions in a workflow</span></span>
* <span data-ttu-id="d3a3e-109">Biztonságossá tétele a hozzáférés tooservices, amely kéréseket fogadni munkafolyamat</span><span class="sxs-lookup"><span data-stu-id="d3a3e-109">Securing access tooservices that receive requests from a workflow</span></span>

## <a name="secure-access-tootrigger"></a><span data-ttu-id="d3a3e-110">Biztonságos hozzáférés tootrigger</span><span class="sxs-lookup"><span data-stu-id="d3a3e-110">Secure access tootrigger</span></span>

<span data-ttu-id="d3a3e-111">Használata esetén a logikai alkalmazás, amely akkor következik be, a HTTP-kérelem ([kérelem](../connectors/connectors-native-reqres.md) vagy [Webhook](../connectors/connectors-native-webhook.md)), korlátozhatja a hozzáférést, hogy csak az arra jogosult ügyfelek is érvényesítést hello logikai alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="d3a3e-111">When you work with a logic app that fires on an HTTP Request ([Request](../connectors/connectors-native-reqres.md) or [Webhook](../connectors/connectors-native-webhook.md)), you can restrict access so that only authorized clients can fire hello logic app.</span></span> <span data-ttu-id="d3a3e-112">Logikai alkalmazás az összes kérelem titkosítja és védi SSL.</span><span class="sxs-lookup"><span data-stu-id="d3a3e-112">All requests into a logic app are encrypted and secured via SSL.</span></span>

### <a name="shared-access-signature"></a><span data-ttu-id="d3a3e-113">Közös hozzáférésű Jogosultságkódot</span><span class="sxs-lookup"><span data-stu-id="d3a3e-113">Shared Access Signature</span></span>

<span data-ttu-id="d3a3e-114">Minden logikai alkalmazás kérés végpontja tartalmaz egy [közös hozzáférésű Jogosultságkód (SAS)](../storage/common/storage-dotnet-shared-access-signature-part-1.md) hello URL-cím részeként.</span><span class="sxs-lookup"><span data-stu-id="d3a3e-114">Every request endpoint for a logic app includes a [Shared Access Signature (SAS)](../storage/common/storage-dotnet-shared-access-signature-part-1.md) as part of hello URL.</span></span> <span data-ttu-id="d3a3e-115">Minden egyes URL-cím tartalmaz egy `sp`, `sv`, és `sig` lekérdezési paraméter.</span><span class="sxs-lookup"><span data-stu-id="d3a3e-115">Each URL contains a `sp`, `sv`, and `sig` query parameter.</span></span> <span data-ttu-id="d3a3e-116">Engedélyek vannak megadva `sp`, és meg tooHTTP módszer engedélyezett, `sv` van hello használt verzió toogenerate, és `sig` használt tooauthenticate hozzáférés tootrigger van.</span><span class="sxs-lookup"><span data-stu-id="d3a3e-116">Permissions are specified by `sp`, and correspond tooHTTP methods allowed, `sv` is hello version used toogenerate, and `sig` is used tooauthenticate access tootrigger.</span></span> <span data-ttu-id="d3a3e-117">hello aláírás generálta hello SHA-256 algoritmus hello URL-cím elérési út és a tulajdonságok a titkos kulccsal.</span><span class="sxs-lookup"><span data-stu-id="d3a3e-117">hello signature is generated using hello SHA256 algorithm with a secret key on all hello URL paths and properties.</span></span> <span data-ttu-id="d3a3e-118">hello titkos kulcs soha nem közzétett és közzé, és a titkosított és hello logikai alkalmazás tárolt.</span><span class="sxs-lookup"><span data-stu-id="d3a3e-118">hello secret key is never exposed and published, and is kept encrypted and stored as part of hello logic app.</span></span> <span data-ttu-id="d3a3e-119">A Logic Apps alkalmazást csak hello titkos kulccsal létrehozott érvényes aláírást tartalmazó eseményindítók engedélyezi.</span><span class="sxs-lookup"><span data-stu-id="d3a3e-119">Your logic app only authorizes triggers that contain a valid signature created with hello secret key.</span></span>

#### <a name="regenerate-access-keys"></a><span data-ttu-id="d3a3e-120">Tárelérési kulcsok újragenerálása</span><span class="sxs-lookup"><span data-stu-id="d3a3e-120">Regenerate access keys</span></span>

<span data-ttu-id="d3a3e-121">Új biztonsági kulcsot következő bármikor hello REST API-t vagy az Azure portálon keresztül is generálja újra.</span><span class="sxs-lookup"><span data-stu-id="d3a3e-121">You can regenerate a new secure key at anytime through hello REST API or Azure portal.</span></span> <span data-ttu-id="d3a3e-122">Az aktuális URL-címet korábban kulccsal hello régi létrehozott érvénytelenített és már nem jogosult toofire hello logikai alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="d3a3e-122">All current URLs that were generated previously using hello old key are invalidated and no longer authorized toofire hello logic app.</span></span>

1. <span data-ttu-id="d3a3e-123">Hello Azure-portálon nyissa meg azt szeretné, hogy a kulcs tooregenerate hello logikai alkalmazás</span><span class="sxs-lookup"><span data-stu-id="d3a3e-123">In hello Azure portal, open hello logic app you want tooregenerate a key</span></span>
1. <span data-ttu-id="d3a3e-124">Kattintson a hello **hívóbetűk** menüpont alatt **beállítások**</span><span class="sxs-lookup"><span data-stu-id="d3a3e-124">Click hello **Access Keys** menu item under **Settings**</span></span>
1. <span data-ttu-id="d3a3e-125">Válassza ki a kulcs tooregenerate hello és teljes hello folyamat</span><span class="sxs-lookup"><span data-stu-id="d3a3e-125">Choose hello key tooregenerate and complete hello process</span></span>

<span data-ttu-id="d3a3e-126">URL-címek lekérése után hello új elérési kulcs újragenerálása aláírt esetén.</span><span class="sxs-lookup"><span data-stu-id="d3a3e-126">URLs you retrieve after regeneration are signed with hello new access key.</span></span>

#### <a name="creating-callback-urls-with-an-expiration-date"></a><span data-ttu-id="d3a3e-127">A lejárati dátum visszahívási URL-címek létrehozása</span><span class="sxs-lookup"><span data-stu-id="d3a3e-127">Creating callback URLs with an expiration date</span></span>

<span data-ttu-id="d3a3e-128">Hello URL-címet oszt meg más felek, ha a URL-címek meghatározott kulcsokkal és a lejárati dátumok igény szerint is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="d3a3e-128">If you are sharing hello URL with other parties, you can generate URLs with specific keys and expiration dates as needed.</span></span> <span data-ttu-id="d3a3e-129">Ezután zökkenőmentesen kulcsok visszaállítani, vagy hozzáférési toofire biztosítása az alkalmazások korlátozott tooa bizonyos timespan van.</span><span class="sxs-lookup"><span data-stu-id="d3a3e-129">You can then seamlessly roll keys, or ensure access toofire an app is restricted tooa certain timespan.</span></span> <span data-ttu-id="d3a3e-130">Adott URL esetén hello keresztül is megadhat a lejárati dátumot [REST API-t a logic apps](https://docs.microsoft.com/rest/api/logic/workflowtriggers):</span><span class="sxs-lookup"><span data-stu-id="d3a3e-130">You can specify an expiration date for a URL through hello [logic apps REST API](https://docs.microsoft.com/rest/api/logic/workflowtriggers):</span></span>

``` http
POST 
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Logic/workflows/{workflowName}/triggers/{triggerName}/listCallbackUrl?api-version=2016-06-01
```

<span data-ttu-id="d3a3e-131">Hello törzsében, tartalmazza a hello tulajdonságot `NotAfter` karakterláncként JSON dátum, amely adja vissza egy visszahívási URL-címet, amely csak akkor érvényes, amíg hello `NotAfter` dátuma és időpontja.</span><span class="sxs-lookup"><span data-stu-id="d3a3e-131">In hello body, include hello property `NotAfter` as a JSON date string, which returns a callback URL that is only valid until hello `NotAfter` date and time.</span></span>

#### <a name="creating-urls-with-primary-or-secondary-secret-key"></a><span data-ttu-id="d3a3e-132">Elsődleges vagy másodlagos titkos kulccsal URL-címek létrehozása</span><span class="sxs-lookup"><span data-stu-id="d3a3e-132">Creating URLs with primary or secondary secret key</span></span>

<span data-ttu-id="d3a3e-133">Létrehozni, vagy a visszahívási kérelem-alapú eseményindítók URL-listában, mely kulcs toouse toosign hello URL-cím is megadható.</span><span class="sxs-lookup"><span data-stu-id="d3a3e-133">When you generate or list callback URLs for request-based triggers, you can also specify which key toouse toosign hello URL.</span></span>  <span data-ttu-id="d3a3e-134">Létrehozhat egy URL-cím, egy adott kulcs keresztül hello által aláírt [REST API-t a logic apps](https://docs.microsoft.com/rest/api/logic/workflowtriggers) az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="d3a3e-134">You can generate a URL signed by a specific key through hello [logic apps REST API](https://docs.microsoft.com/rest/api/logic/workflowtriggers) as follows:</span></span>

``` http
POST 
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Logic/workflows/{workflowName}/triggers/{triggerName}/listCallbackUrl?api-version=2016-06-01
```

<span data-ttu-id="d3a3e-135">Hello törzsében, tartalmazza a hello tulajdonságot `KeyType` , vagyis `Primary` vagy `Secondary`.</span><span class="sxs-lookup"><span data-stu-id="d3a3e-135">In hello body, include hello property `KeyType` as either `Primary` or `Secondary`.</span></span>  <span data-ttu-id="d3a3e-136">Ez visszaadja a megadott hello biztonságos kulcs által aláírt URL-címet.</span><span class="sxs-lookup"><span data-stu-id="d3a3e-136">This returns a URL signed by hello secure key specified.</span></span>

### <a name="restrict-incoming-ip-addresses"></a><span data-ttu-id="d3a3e-137">Bejövő IP-címek korlátozása</span><span class="sxs-lookup"><span data-stu-id="d3a3e-137">Restrict incoming IP addresses</span></span>

<span data-ttu-id="d3a3e-138">Továbbá a közös hozzáférésű Jogosultságkód toohello, érdemes toorestrict logikai alkalmazás hívása csak az adott ügyfelektől.</span><span class="sxs-lookup"><span data-stu-id="d3a3e-138">In addition toohello Shared Access Signature, you may wish toorestrict calling a logic app only from specific clients.</span></span>  <span data-ttu-id="d3a3e-139">Például ha Ön kezeli a végpont Azure API Management keresztül, korlátozhatja a hello logic app tooonly hello kérés elfogadása, amikor hello kérelem hello API Management példány IP-cím származik.</span><span class="sxs-lookup"><span data-stu-id="d3a3e-139">For example, if you manage your endpoint through Azure API Management, you can restrict hello logic app tooonly accept hello request when hello request comes from hello API Management instance IP address.</span></span>

<span data-ttu-id="d3a3e-140">Ez a beállítás konfigurálható hello logic app beállítások:</span><span class="sxs-lookup"><span data-stu-id="d3a3e-140">This setting can be configured within hello logic app settings:</span></span>

1. <span data-ttu-id="d3a3e-141">A hello Azure-portálon nyissa meg a kívánt tooadd IP-címkorlátozások hello logikai alkalmazás</span><span class="sxs-lookup"><span data-stu-id="d3a3e-141">In hello Azure portal, open hello logic app you want tooadd IP address restrictions</span></span>
1. <span data-ttu-id="d3a3e-142">Kattintson a hello **hozzáférés-vezérlési konfiguráció** menüpont alatt **beállítások**</span><span class="sxs-lookup"><span data-stu-id="d3a3e-142">Click hello **Access control configuration** menu item under **Settings**</span></span>
1. <span data-ttu-id="d3a3e-143">Adja meg az IP cím tartományokhoz toobe hello eseményindító által elfogadott hello listája</span><span class="sxs-lookup"><span data-stu-id="d3a3e-143">Specify hello list of IP address ranges toobe accepted by hello trigger</span></span>

<span data-ttu-id="d3a3e-144">Egy érvényes IP-címtartomány hello formátumban veszi `192.168.1.1/255`.</span><span class="sxs-lookup"><span data-stu-id="d3a3e-144">A valid IP range takes hello format `192.168.1.1/255`.</span></span> <span data-ttu-id="d3a3e-145">Ha hello logic app tooonly tűz beágyazott logika alkalmazásként, jelölje be a hello **csak más logic apps** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="d3a3e-145">If you want hello logic app tooonly fire as a nested logic app, select hello **Only other logic apps** option.</span></span> <span data-ttu-id="d3a3e-146">Ez a beállítás egy üres tömb toohello erőforrás, ami azt jelenti, csak hívást hello szolgáltatás magát (szülő logic apps) tűz sikeresen ír.</span><span class="sxs-lookup"><span data-stu-id="d3a3e-146">This option writes an empty array toohello resource, meaning only calls from hello service itself (parent logic apps) fire successfully.</span></span>

> [!NOTE]
> <span data-ttu-id="d3a3e-147">Továbbra is futtatható a logikai alkalmazást az kérelem eseményindító hello REST API-n keresztül / felügyeleti `/triggers/{triggerName}/run` IP függetlenül.</span><span class="sxs-lookup"><span data-stu-id="d3a3e-147">You can still run a logic app with a request trigger through hello REST API / Management `/triggers/{triggerName}/run` regardless of IP.</span></span> <span data-ttu-id="d3a3e-148">Ebben a forgatókönyvben hello Azure REST API hitelesítést igényel, és az összes esemény hello Azure napló jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="d3a3e-148">This scenario requires authentication against hello Azure REST API, and all events would appear in hello Azure Audit Log.</span></span> <span data-ttu-id="d3a3e-149">Ennek megfelelően beállított hozzáférés-vezérlési házirendek.</span><span class="sxs-lookup"><span data-stu-id="d3a3e-149">Set access control policies accordingly.</span></span>

#### <a name="setting-ip-ranges-on-hello-resource-definition"></a><span data-ttu-id="d3a3e-150">IP-címtartományok hello erőforrás-definíció beállítása</span><span class="sxs-lookup"><span data-stu-id="d3a3e-150">Setting IP ranges on hello resource definition</span></span>

<span data-ttu-id="d3a3e-151">Ha használ egy [központi telepítési sablont](logic-apps-create-deploy-template.md) a központi telepítések, hello IP-címtartomány beállításokat lehet megadni a hello erőforrás sablon tooautomate.</span><span class="sxs-lookup"><span data-stu-id="d3a3e-151">If you are using a [deployment template](logic-apps-create-deploy-template.md) tooautomate your deployments, hello IP range settings can be configured on hello resource template.</span></span>  

``` json
{
    "properties": {
        "definition": {
        },
        "parameters": {},
        "accessControl": {
            "triggers": {
                "allowedCallerIpAddresses": [
                    {
                        "addressRange": "192.168.12.0/23"
                    },
                    {
                        "addressRange": "2001:0db8::/64"
                    }
                ]
            }
        }
    },
    "type": "Microsoft.Logic/workflows"
}

```

### <a name="adding-azure-active-directory-oauth-or-other-security"></a><span data-ttu-id="d3a3e-152">Azure Active Directory, az OAuth és az egyéb biztonsági</span><span class="sxs-lookup"><span data-stu-id="d3a3e-152">Adding Azure Active Directory, OAuth, or other security</span></span>

<span data-ttu-id="d3a3e-153">további hitelesítési protokollok egy logikai alkalmazás fölött tooadd [Azure API Management](https://azure.microsoft.com/services/api-management/) kínál gazdag figyelési, biztonsági, házirend, és bármely végpont hello funkció tooexpose dokumentációja, az API-k logikai alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="d3a3e-153">tooadd more authorization protocols on top of a logic app, [Azure API Management](https://azure.microsoft.com/services/api-management/) offers rich monitoring, security, policy, and documentation for any endpoint with hello capability tooexpose a logic app as an API.</span></span> <span data-ttu-id="d3a3e-154">Az Azure API Management is elérhetővé teheti a saját vagy nyilvános végpontja hello logikai alkalmazás, hogy az Azure Active Directory, a tanúsítványt, az OAuth vagy a más biztonsági szabványok használ.</span><span class="sxs-lookup"><span data-stu-id="d3a3e-154">Azure API Management can expose a public or private endpoint for hello logic app, which could use Azure Active Directory, certificate, OAuth, or other security standards.</span></span> <span data-ttu-id="d3a3e-155">Amikor egy kérelem érkezik, az Azure API Management hello kérelem toohello logikai alkalmazás (hajt végre semmilyen szükséges átalakítások vagy üzenetsoroktól korlátozások) továbbítja.</span><span class="sxs-lookup"><span data-stu-id="d3a3e-155">When a request is received, Azure API Management forwards hello request toohello logic app (performing any needed transformations or restrictions in-flight).</span></span> <span data-ttu-id="d3a3e-156">Hello beállításai hello logic app tooonly lehetővé teszik hello logic app toobe elindul az API Management bejövő IP-címtartomány is használhatja.</span><span class="sxs-lookup"><span data-stu-id="d3a3e-156">You can use hello incoming IP range settings on hello logic app tooonly allow hello logic app toobe triggered from API Management.</span></span>

## <a name="secure-access-toomanage-or-edit-logic-apps"></a><span data-ttu-id="d3a3e-157">Biztonságos hozzáférés a logic apps toomanage vagy szerkesztése</span><span class="sxs-lookup"><span data-stu-id="d3a3e-157">Secure access toomanage or edit logic apps</span></span>

<span data-ttu-id="d3a3e-158">Hozzáférési toomanagement műveleteket logikai alkalmazás korlátozhatja, hogy csak bizonyos felhasználók vagy csoportok hello erőforrás képes tooperform műveleteket.</span><span class="sxs-lookup"><span data-stu-id="d3a3e-158">You can restrict access toomanagement operations on a logic app so that only specific users or groups are able tooperform operations on hello resource.</span></span> <span data-ttu-id="d3a3e-159">A Logic apps használata hello Azure [szerepköralapú hozzáférés-vezérlést (RBAC)](../active-directory/role-based-access-control-configure.md) szolgáltatás, és testre szabható a hello ugyanazokat az eszközöket.</span><span class="sxs-lookup"><span data-stu-id="d3a3e-159">Logic apps use hello Azure [Role-Based Access Control (RBAC)](../active-directory/role-based-access-control-configure.md) feature, and can be customized with hello same tools.</span></span>  <span data-ttu-id="d3a3e-160">Az előfizetés tooas tagjai is rendelhet néhány beépített szerepkörök állnak rendelkezésre:</span><span class="sxs-lookup"><span data-stu-id="d3a3e-160">There are a few built-in roles you can assign members of your subscription tooas well:</span></span>

* <span data-ttu-id="d3a3e-161">**Logic App közreműködői** -hozzáférés tooview, szerkesztése és a logikai alkalmazás frissítése biztosít.</span><span class="sxs-lookup"><span data-stu-id="d3a3e-161">**Logic App Contributor** - Provides access tooview, edit, and update a logic app.</span></span>  <span data-ttu-id="d3a3e-162">Nem lehet eltávolítani a hello erőforrás, vagy felügyeleti műveleteket.</span><span class="sxs-lookup"><span data-stu-id="d3a3e-162">Cannot remove hello resource or perform admin operations.</span></span>
* <span data-ttu-id="d3a3e-163">**Logic App operátor** – megtekintheti hello logikai alkalmazás és futtatási előzményei, és engedélyezését vagy letiltását.</span><span class="sxs-lookup"><span data-stu-id="d3a3e-163">**Logic App Operator** - Can view hello logic app and run history, and enable/disable.</span></span>  <span data-ttu-id="d3a3e-164">Nem szerkeszthetők vagy hello definíciójának frissítése.</span><span class="sxs-lookup"><span data-stu-id="d3a3e-164">Cannot edit or update hello definition.</span></span>

<span data-ttu-id="d3a3e-165">Is [Azure erőforrás-zárolás](../azure-resource-manager/resource-group-lock-resources.md) tooprevent módosítása vagy törlése a logic Apps alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="d3a3e-165">You can also use [Azure Resource Lock](../azure-resource-manager/resource-group-lock-resources.md) tooprevent changing or deleting logic apps.</span></span> <span data-ttu-id="d3a3e-166">Ez a lehetőség akkor értékes tooprevent éles erőforrásokat módosítások vagy törlésekhez.</span><span class="sxs-lookup"><span data-stu-id="d3a3e-166">This capability is valuable tooprevent production resources from changes or deletions.</span></span>

## <a name="secure-access-toocontents-of-hello-run-history"></a><span data-ttu-id="d3a3e-167">Biztonságos hozzáférés toocontents a hello futtatási előzményei</span><span class="sxs-lookup"><span data-stu-id="d3a3e-167">Secure access toocontents of hello run history</span></span>

<span data-ttu-id="d3a3e-168">A bemeneti vagy kimeneti hozzáférés toocontents előző futtatása toospecific IP-címtartományok korlátozhatja.</span><span class="sxs-lookup"><span data-stu-id="d3a3e-168">You can restrict access toocontents of inputs or outputs from previous runs toospecific IP address ranges.</span></span>  

<span data-ttu-id="d3a3e-169">Egy munkafolyamat sorozaton belül összes adata titkosításra kerül, az átvitel során, és inaktív.</span><span class="sxs-lookup"><span data-stu-id="d3a3e-169">All data within a workflow run is encrypted in transit and at rest.</span></span> <span data-ttu-id="d3a3e-170">Hívás toorun előzményeit történik, amikor hello szolgáltatás hitelesíti hello kérelmet, és hivatkozások toohello kérés és válasz bemenetekhez és kimenetekhez biztosít.</span><span class="sxs-lookup"><span data-stu-id="d3a3e-170">When a call toorun history is made, hello service authenticates hello request and provides links toohello request and response inputs and outputs.</span></span> <span data-ttu-id="d3a3e-171">Ez a hivatkozás lehet védett, csak kérelmek tooview tartalmából áll a kijelölt IP-címtartomány hello tartalmát adja vissza.</span><span class="sxs-lookup"><span data-stu-id="d3a3e-171">This link can be protected so only requests tooview content from a designated IP address range return hello contents.</span></span> <span data-ttu-id="d3a3e-172">Ez a funkció további hozzáférés-vezérléshez is használhatja.</span><span class="sxs-lookup"><span data-stu-id="d3a3e-172">You can use this capability for additional access control.</span></span> <span data-ttu-id="d3a3e-173">Akkor kell megadni egy IP-címet, például `0.0.0.0` , nem sikerült elérni a bemenetek/kimenetek.</span><span class="sxs-lookup"><span data-stu-id="d3a3e-173">You could even specify an IP address like `0.0.0.0` so no one could access inputs/outputs.</span></span> <span data-ttu-id="d3a3e-174">Csak a személy rendszergazdai jogosultságokkal tudta eltávolítani ezt a korlátozást, és hello lehetőséget biztosít a "just-in-time" hozzáférési tooworkflow tartalmát.</span><span class="sxs-lookup"><span data-stu-id="d3a3e-174">Only someone with admin permissions could remove this restriction, providing hello possibility for 'just-in-time' access tooworkflow contents.</span></span>

<span data-ttu-id="d3a3e-175">Ez a beállítás konfigurálható hello erőforrás beállításainak hello Azure-portálon:</span><span class="sxs-lookup"><span data-stu-id="d3a3e-175">This setting can be configured within hello resource settings of hello Azure portal:</span></span>

1. <span data-ttu-id="d3a3e-176">A hello Azure-portálon nyissa meg a kívánt tooadd IP-címkorlátozások hello logikai alkalmazás</span><span class="sxs-lookup"><span data-stu-id="d3a3e-176">In hello Azure portal, open hello logic app you want tooadd IP address restrictions</span></span>
1. <span data-ttu-id="d3a3e-177">Kattintson a hello **hozzáférés-vezérlési konfiguráció** menüpont alatt **beállítások**</span><span class="sxs-lookup"><span data-stu-id="d3a3e-177">Click hello **Access control configuration** menu item under **Settings**</span></span>
1. <span data-ttu-id="d3a3e-178">Adja meg a hozzáférés toocontent IP-címtartományát hello listája</span><span class="sxs-lookup"><span data-stu-id="d3a3e-178">Specify hello list of IP address ranges for access toocontent</span></span>

#### <a name="setting-ip-ranges-on-hello-resource-definition"></a><span data-ttu-id="d3a3e-179">IP-címtartományok hello erőforrás-definíció beállítása</span><span class="sxs-lookup"><span data-stu-id="d3a3e-179">Setting IP ranges on hello resource definition</span></span>

<span data-ttu-id="d3a3e-180">Ha használ egy [központi telepítési sablont](logic-apps-create-deploy-template.md) a központi telepítések, hello IP-címtartomány beállításokat lehet megadni a hello erőforrás sablon tooautomate.</span><span class="sxs-lookup"><span data-stu-id="d3a3e-180">If you are using a [deployment template](logic-apps-create-deploy-template.md) tooautomate your deployments, hello IP range settings can be configured on hello resource template.</span></span>  

``` json
{
    "properties": {
        "definition": {
        },
        "parameters": {},
        "accessControl": {
            "contents": {
                "allowedCallerIpAddresses": [
                    {
                        "addressRange": "192.168.12.0/23"
                    },
                    {
                        "addressRange": "2001:0db8::/64"
                    }
                ]
            }
        }
    },
    "type": "Microsoft.Logic/workflows"
}
```

## <a name="secure-parameters-and-inputs-within-a-workflow"></a><span data-ttu-id="d3a3e-181">Biztonságos paraméterek és a bemenetek egy munkafolyamaton belül</span><span class="sxs-lookup"><span data-stu-id="d3a3e-181">Secure parameters and inputs within a workflow</span></span>

<span data-ttu-id="d3a3e-182">Érdemes lehet tooparameterize telepítési munkafolyamat definícióját az egyes funkcióit környezetek között.</span><span class="sxs-lookup"><span data-stu-id="d3a3e-182">You might want tooparameterize some aspects of a workflow definition for deployment across environments.</span></span> <span data-ttu-id="d3a3e-183">Egyes paraméterek előfordulhat, hogy is biztonságos paraméterek nem szeretné, hogy tooappear egy munkafolyamatot, például egy ügyfél-azonosító és a titkos ügyfélkulcs szerkesztésekor [Azure Active Directory hitelesítési](../connectors/connectors-native-http.md#authentication) egy HTTP-művelet.</span><span class="sxs-lookup"><span data-stu-id="d3a3e-183">Also, some parameters might be secure parameters you don't want tooappear when editing a workflow, such as a client ID and client secret for [Azure Active Directory authentication](../connectors/connectors-native-http.md#authentication) of an HTTP action.</span></span>

### <a name="using-parameters-and-secure-parameters"></a><span data-ttu-id="d3a3e-184">Paraméterek és biztonságos paraméterek</span><span class="sxs-lookup"><span data-stu-id="d3a3e-184">Using parameters and secure parameters</span></span>

<span data-ttu-id="d3a3e-185">tooaccess hello futásidőben, egy erőforrás paraméter értékének hello [munkafolyamatdefiníciós nyelve](http://aka.ms/logicappsdocs) biztosít egy `@parameters()` műveletet.</span><span class="sxs-lookup"><span data-stu-id="d3a3e-185">tooaccess hello value of a resource parameter at runtime, hello [workflow definition language](http://aka.ms/logicappsdocs) provides a `@parameters()` operation.</span></span> <span data-ttu-id="d3a3e-186">Akkor is, [adja meg a paraméterek hello erőforrás központi telepítési sablont a](../azure-resource-manager/resource-group-authoring-templates.md#parameters).</span><span class="sxs-lookup"><span data-stu-id="d3a3e-186">Also, you can [specify parameters in hello resource deployment template](../azure-resource-manager/resource-group-authoring-templates.md#parameters).</span></span> <span data-ttu-id="d3a3e-187">De ha hello paraméter típusa, adja meg `securestring`, hello paraméter hello többi hello erőforrás-definíció nem adható vissza, és nem lesznek elérhetők a telepítés utáni hello erőforrás megtekintésével.</span><span class="sxs-lookup"><span data-stu-id="d3a3e-187">But if you specify hello parameter type as `securestring`, hello parameter won't be returned with hello rest of hello resource definition, and won't be accessible by viewing hello resource after deployment.</span></span>

> [!NOTE]
> <span data-ttu-id="d3a3e-188">Ha a paraméter hello fejlécek vagy egy kérelem törzse hello paraméter is láthatják őket futtatása hello előzmények és kimenő HTTP-kérelem elérésével.</span><span class="sxs-lookup"><span data-stu-id="d3a3e-188">If your parameter is used in hello headers or body of a request, hello parameter might be visible by accessing hello run history and outgoing HTTP request.</span></span> <span data-ttu-id="d3a3e-189">Győződjön meg arról, hogy tooset a tartalom-hozzáférési házirendek ennek megfelelően.</span><span class="sxs-lookup"><span data-stu-id="d3a3e-189">Make sure tooset your content access policies accordingly.</span></span>
> <span data-ttu-id="d3a3e-190">Engedélyezési fejléceket keresztül bemeneti vagy kimeneti soha nem láthatók el.</span><span class="sxs-lookup"><span data-stu-id="d3a3e-190">Authorization headers are never visible through inputs or outputs.</span></span> <span data-ttu-id="d3a3e-191">Ezért ha hello titkos kulcs használatban van ott, hello titkos kulcs nincs lekérhető.</span><span class="sxs-lookup"><span data-stu-id="d3a3e-191">So if hello secret is being used there, hello secret is not retrievable.</span></span>

#### <a name="resource-deployment-template-with-secrets"></a><span data-ttu-id="d3a3e-192">Erőforrás központi telepítési sablont a titkos kulcsok</span><span class="sxs-lookup"><span data-stu-id="d3a3e-192">Resource deployment template with secrets</span></span>

<span data-ttu-id="d3a3e-193">hello alábbi példa azt mutatja a központi telepítés egy biztonságos paramétere hivatkozó `secret` futásidőben.</span><span class="sxs-lookup"><span data-stu-id="d3a3e-193">hello following example shows a deployment that references a secure parameter of `secret` at runtime.</span></span> <span data-ttu-id="d3a3e-194">Egy külön paraméterek fájlban hello hello környezet értéket kell megadni `secret`, vagy használjon [Azure Resource Manager KeyVault](../azure-resource-manager/resource-manager-keyvault-parameter.md) tooretrieve titkok, idő központi telepítése.</span><span class="sxs-lookup"><span data-stu-id="d3a3e-194">In a separate parameters file, you could specify hello environment value for hello `secret`, or use [Azure Resource Manager KeyVault](../azure-resource-manager/resource-manager-keyvault-parameter.md) tooretrieve secrets at deploy time.</span></span>

``` json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "secretDeploymentParam": {
      "type": "securestring"
    }
  },
  "variables": {},
  "resources": [
    {
      "name": "secret-deploy",
      "type": "Microsoft.Logic/workflows",
      "location": "westus",
      "tags": {
        "displayName": "LogicApp"
      },
      "apiVersion": "2016-06-01",
      "properties": {
        "definition": {
          "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
          "actions": {
            "Call_External_API": {
              "type": "http",
              "inputs": {
                "headers": {
                  "Authorization": "@parameters('secret')"
                },
                "body": "This is hello request"
              },
              "runAfter": {}
            }
          },
          "parameters": {
            "secret": {
              "type": "SecureString"
            }
          },
          "triggers": {
            "manual": {
              "type": "Request",
              "kind": "Http",
              "inputs": {
                "schema": {}
              }
            }
          },
          "contentVersion": "1.0.0.0",
          "outputs": {}
        },
        "parameters": {
          "secret": {
            "value": "[parameters('secretDeploymentParam')]"
          }
        }
      }
    }
  ],
  "outputs": {}
}
```

## <a name="secure-access-tooservices-receiving-requests-from-a-workflow"></a><span data-ttu-id="d3a3e-195">Biztonságos hozzáférés tooservices fogadó kérelmeinek munkafolyamat</span><span class="sxs-lookup"><span data-stu-id="d3a3e-195">Secure access tooservices receiving requests from a workflow</span></span>

<span data-ttu-id="d3a3e-196">Nincsenek biztonságos bármely végpont hello logic app kell tooaccess számos módon toohelp.</span><span class="sxs-lookup"><span data-stu-id="d3a3e-196">There are many ways toohelp secure any endpoint hello logic app needs tooaccess.</span></span>

### <a name="using-authentication-on-outbound-requests"></a><span data-ttu-id="d3a3e-197">A kimenő kérelmek-hitelesítéssel</span><span class="sxs-lookup"><span data-stu-id="d3a3e-197">Using authentication on outbound requests</span></span>

<span data-ttu-id="d3a3e-198">Amikor egy HTTP-, HTTP + Swagger (nyílt API-t), vagy a Webhook művelet olyan hitelesítési toohello kérelmet küldött is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="d3a3e-198">When working with an HTTP, HTTP + Swagger (Open API), or Webhook action, you can add authentication toohello request being sent.</span></span> <span data-ttu-id="d3a3e-199">Alapszintű hitelesítés, tanúsítvány vagy Azure Active Directory-hitelesítés tartalmazhatnak.</span><span class="sxs-lookup"><span data-stu-id="d3a3e-199">You could include basic authentication, certificate authentication, or Azure Active Directory authentication.</span></span> <span data-ttu-id="d3a3e-200">Hogyan tooconfigure a hitelesítés található részleteinek [ebben a cikkben](../connectors/connectors-native-http.md#authentication).</span><span class="sxs-lookup"><span data-stu-id="d3a3e-200">Details on how tooconfigure this authentication can be found [in this article](../connectors/connectors-native-http.md#authentication).</span></span>

### <a name="restricting-access-toologic-app-ip-addresses"></a><span data-ttu-id="d3a3e-201">Hozzáférés toologic app IP-címek korlátozása</span><span class="sxs-lookup"><span data-stu-id="d3a3e-201">Restricting access toologic app IP addresses</span></span>

<span data-ttu-id="d3a3e-202">A logic Apps alkalmazásokból összes hívások régiónként eltérő IP-címek meghatározott készletének határozza meg.</span><span class="sxs-lookup"><span data-stu-id="d3a3e-202">All calls from logic apps come from a specific set of IP addresses per region.</span></span> <span data-ttu-id="d3a3e-203">Hozzáadhat további szűrési tooonly azoktól, amelyeket a kijelölt IP-címek kérelmek fogadására.</span><span class="sxs-lookup"><span data-stu-id="d3a3e-203">You can add additional filtering tooonly accept requests from those designated IP addresses.</span></span> <span data-ttu-id="d3a3e-204">Egy adott IP-címek listáját lásd: [logic app korlátozásai és konfigurációja](logic-apps-limits-and-config.md#configuration).</span><span class="sxs-lookup"><span data-stu-id="d3a3e-204">For a list of those IP addresses, see [logic app limits and configuration](logic-apps-limits-and-config.md#configuration).</span></span>

### <a name="on-premises-connectivity"></a><span data-ttu-id="d3a3e-205">Helyszíni kapcsolatok</span><span class="sxs-lookup"><span data-stu-id="d3a3e-205">On-premises connectivity</span></span>

<span data-ttu-id="d3a3e-206">A Logic apps több szolgáltatások tooprovide biztonságos és megbízható integrálva a helyszíni kommunikációt biztosítanak.</span><span class="sxs-lookup"><span data-stu-id="d3a3e-206">Logic apps provide integration with several services tooprovide secure and reliable on-premises communication.</span></span>

#### <a name="on-premises-data-gateway"></a><span data-ttu-id="d3a3e-207">Helyi adatátjáró</span><span class="sxs-lookup"><span data-stu-id="d3a3e-207">On-premises data gateway</span></span>

<span data-ttu-id="d3a3e-208">Sok felügyelt összekötők logic apps a tooon helyszíni rendszerekre, például File System, a SQL, a SharePoint, a DB2 és egyéb biztonságos kapcsolatot biztosít.</span><span class="sxs-lookup"><span data-stu-id="d3a3e-208">Many managed connectors for logic apps provide secure connectivity tooon-premises systems, including File System, SQL, SharePoint, DB2, and more.</span></span> <span data-ttu-id="d3a3e-209">hello átjáró továbbítja a titkosított csatornákon keresztül hello Azure Service Bus helyszíni forrásból származó adatokat.</span><span class="sxs-lookup"><span data-stu-id="d3a3e-209">hello gateway relays data from on-premises sources on encrypted channels through hello Azure Service Bus.</span></span> <span data-ttu-id="d3a3e-210">Az összes forgalom származik, mint hello átjáró ügynök biztonságos kimenő forgalmát.</span><span class="sxs-lookup"><span data-stu-id="d3a3e-210">All traffic originates as secure outbound traffic from hello gateway agent.</span></span> <span data-ttu-id="d3a3e-211">További információ [hello adatátjáró működése](logic-apps-gateway-install.md#gateway-cloud-service).</span><span class="sxs-lookup"><span data-stu-id="d3a3e-211">Learn more about [how hello data gateway works](logic-apps-gateway-install.md#gateway-cloud-service).</span></span>

#### <a name="azure-api-management"></a><span data-ttu-id="d3a3e-212">Azure API Management</span><span class="sxs-lookup"><span data-stu-id="d3a3e-212">Azure API Management</span></span>

<span data-ttu-id="d3a3e-213">[Az Azure API Management](https://azure.microsoft.com/services/api-management/) rendelkezik a helyszíni kapcsolati lehetőségek, többek között a pont-pont VPN- és ExpressRoute integráció biztonságos proxy és a kommunikáció tooon helyszíni rendszerekben.</span><span class="sxs-lookup"><span data-stu-id="d3a3e-213">[Azure API Management](https://azure.microsoft.com/services/api-management/) has on-premises connectivity options, including site-to-site VPN and ExpressRoute integration for secured proxy and communication tooon-premises systems.</span></span> <span data-ttu-id="d3a3e-214">Hello Logic App Designer az API-k egy munkafolyamaton belül az Azure API Management kitett gyors hozzáférést biztosító tooon helyszíni rendszerekben gyorsan kiválaszthatja.</span><span class="sxs-lookup"><span data-stu-id="d3a3e-214">In hello Logic App Designer, you can quickly select an API exposed from Azure API Management within a workflow, providing quick access tooon-premises systems.</span></span>

#### <a name="hybrid-connections-from-azure-app-service"></a><span data-ttu-id="d3a3e-215">Hibrid kapcsolatok az Azure App Service</span><span class="sxs-lookup"><span data-stu-id="d3a3e-215">Hybrid connections from Azure App Service</span></span>

<span data-ttu-id="d3a3e-216">Azure API és a Web apps toocommunicate helyszíni hello helyszíni hibrid kapcsolat funkció használható.</span><span class="sxs-lookup"><span data-stu-id="d3a3e-216">You can use hello on-premises hybrid connection feature for Azure API and Web apps toocommunicate on-premises.</span></span>  <span data-ttu-id="d3a3e-217">Hibrid kapcsolatok és hogyan tooconfigure található [ebben a cikkben](../app-service-web/web-sites-hybrid-connection-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="d3a3e-217">Details on hybrid connections and how tooconfigure can be found [in this article](../app-service-web/web-sites-hybrid-connection-get-started.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="d3a3e-218">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d3a3e-218">Next steps</span></span>
[<span data-ttu-id="d3a3e-219">Központi telepítési sablon létrehozása</span><span class="sxs-lookup"><span data-stu-id="d3a3e-219">Create a deployment template</span></span>](logic-apps-create-deploy-template.md)  
[<span data-ttu-id="d3a3e-220">Kivételkezelés</span><span class="sxs-lookup"><span data-stu-id="d3a3e-220">Exception handling</span></span>](logic-apps-exception-handling.md)  
[<span data-ttu-id="d3a3e-221">Logikai alkalmazások figyelése</span><span class="sxs-lookup"><span data-stu-id="d3a3e-221">Monitor your logic apps</span></span>](logic-apps-monitor-your-logic-apps.md)  
[<span data-ttu-id="d3a3e-222">Logic app hibák és problémák diagnosztizálása</span><span class="sxs-lookup"><span data-stu-id="d3a3e-222">Diagnosing logic app failures and issues</span></span>](logic-apps-diagnosing-failures.md)  
