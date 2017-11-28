---
title: "Biztonságos hozzáférés a Azure Logic Apps |} Microsoft Docs"
description: "Adja hozzá a biztonsági védelmének eseményindítók, bemeneti és kimeneti, művelet paramétereinek és a munkafolyamatok az Azure Logic Apps szolgáltatás elérésére."
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
ms.openlocfilehash: 0528d660f590e106f61729f10f8f68da3fe58cb7
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="secure-access-to-your-logic-apps"></a><span data-ttu-id="1670c-103">Biztonságos hozzáférés a logic Apps alkalmazásait</span><span class="sxs-lookup"><span data-stu-id="1670c-103">Secure access to your logic apps</span></span>

<span data-ttu-id="1670c-104">Nincsenek elérhető segítségével biztosíthatja a Logic Apps alkalmazást sok eszköz.</span><span class="sxs-lookup"><span data-stu-id="1670c-104">There are many tools available to help you secure your logic app.</span></span>

* <span data-ttu-id="1670c-105">Logikai alkalmazás (HTTP kérelem eseményindító) való hozzáférés biztosítása</span><span class="sxs-lookup"><span data-stu-id="1670c-105">Securing access to trigger a logic app (HTTP Request Trigger)</span></span>
* <span data-ttu-id="1670c-106">Kezelése, szerkesztése vagy olvassa el a logikai alkalmazás hozzáférésének biztonságossá tétele</span><span class="sxs-lookup"><span data-stu-id="1670c-106">Securing access to manage, edit, or read a logic app</span></span>
* <span data-ttu-id="1670c-107">A Futtatás bemenetekhez és kimenetekhez tartalmát való hozzáférés biztosítása</span><span class="sxs-lookup"><span data-stu-id="1670c-107">Securing access to contents of inputs and outputs for a run</span></span>
* <span data-ttu-id="1670c-108">Paraméterek vagy a műveletek a munkafolyamat belül biztonságossá tétele</span><span class="sxs-lookup"><span data-stu-id="1670c-108">Securing parameters or inputs within actions in a workflow</span></span>
* <span data-ttu-id="1670c-109">Kéréseket fogadni a munkafolyamat-szolgáltatásokhoz való hozzáférés biztonságossá tétele</span><span class="sxs-lookup"><span data-stu-id="1670c-109">Securing access to services that receive requests from a workflow</span></span>

## <a name="secure-access-to-trigger"></a><span data-ttu-id="1670c-110">Biztonságos hozzáférés a indítás</span><span class="sxs-lookup"><span data-stu-id="1670c-110">Secure access to trigger</span></span>

<span data-ttu-id="1670c-111">Használata esetén a logikai alkalmazás, amely akkor következik be, a HTTP-kérelem ([kérelem](../connectors/connectors-native-reqres.md) vagy [Webhook](../connectors/connectors-native-webhook.md)), korlátozhatja a hozzáférést, hogy csak az arra jogosult ügyfelek is érvényesítést a logikai alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="1670c-111">When you work with a logic app that fires on an HTTP Request ([Request](../connectors/connectors-native-reqres.md) or [Webhook](../connectors/connectors-native-webhook.md)), you can restrict access so that only authorized clients can fire the logic app.</span></span> <span data-ttu-id="1670c-112">Logikai alkalmazás az összes kérelem titkosítja és védi SSL.</span><span class="sxs-lookup"><span data-stu-id="1670c-112">All requests into a logic app are encrypted and secured via SSL.</span></span>

### <a name="shared-access-signature"></a><span data-ttu-id="1670c-113">Közös hozzáférésű Jogosultságkódot</span><span class="sxs-lookup"><span data-stu-id="1670c-113">Shared Access Signature</span></span>

<span data-ttu-id="1670c-114">Minden logikai alkalmazás kérés végpontja tartalmaz egy [közös hozzáférésű Jogosultságkód (SAS)](../storage/common/storage-dotnet-shared-access-signature-part-1.md) az URL-cím részeként.</span><span class="sxs-lookup"><span data-stu-id="1670c-114">Every request endpoint for a logic app includes a [Shared Access Signature (SAS)](../storage/common/storage-dotnet-shared-access-signature-part-1.md) as part of the URL.</span></span> <span data-ttu-id="1670c-115">Minden egyes URL-cím tartalmaz egy `sp`, `sv`, és `sig` lekérdezési paraméter.</span><span class="sxs-lookup"><span data-stu-id="1670c-115">Each URL contains a `sp`, `sv`, and `sig` query parameter.</span></span> <span data-ttu-id="1670c-116">Engedélyek vannak megadva `sp`, és engedélyezett, HTTP-metódus meg `sv` létrehozandó használt verzió és `sig` való hozzáférés hitelesítéséhez használt.</span><span class="sxs-lookup"><span data-stu-id="1670c-116">Permissions are specified by `sp`, and correspond to HTTP methods allowed, `sv` is the version used to generate, and `sig` is used to authenticate access to trigger.</span></span> <span data-ttu-id="1670c-117">Az aláírás generálta az SHA256 algoritmust az URL-cím elérési út és a tulajdonságok a titkos kulccsal.</span><span class="sxs-lookup"><span data-stu-id="1670c-117">The signature is generated using the SHA256 algorithm with a secret key on all the URL paths and properties.</span></span> <span data-ttu-id="1670c-118">A titkos kulcs soha nem közzétett és közzé, és a titkosított és a logikai alkalmazás tárolt.</span><span class="sxs-lookup"><span data-stu-id="1670c-118">The secret key is never exposed and published, and is kept encrypted and stored as part of the logic app.</span></span> <span data-ttu-id="1670c-119">A Logic Apps alkalmazást csak engedélyezi a titkos kulccsal létrehozott érvényes aláírást tartalmazó eseményindítók.</span><span class="sxs-lookup"><span data-stu-id="1670c-119">Your logic app only authorizes triggers that contain a valid signature created with the secret key.</span></span>

#### <a name="regenerate-access-keys"></a><span data-ttu-id="1670c-120">Tárelérési kulcsok újragenerálása</span><span class="sxs-lookup"><span data-stu-id="1670c-120">Regenerate access keys</span></span>

<span data-ttu-id="1670c-121">Új biztonsági kulcsot következő a REST API-t vagy az Azure portálon keresztül bármikor újragenerálás.</span><span class="sxs-lookup"><span data-stu-id="1670c-121">You can regenerate a new secure key at anytime through the REST API or Azure portal.</span></span> <span data-ttu-id="1670c-122">Az aktuális URL-címet a régi kulccsal korábban létrehozott érvénytelenítve, és nem jogosult az érvényesítést a logikai alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="1670c-122">All current URLs that were generated previously using the old key are invalidated and no longer authorized to fire the logic app.</span></span>

1. <span data-ttu-id="1670c-123">Nyissa meg a logikai alkalmazást újragenerálja a kulcsot az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="1670c-123">In the Azure portal, open the logic app you want to regenerate a key</span></span>
1. <span data-ttu-id="1670c-124">Kattintson a **hívóbetűk** menüpont alatt **beállítások**</span><span class="sxs-lookup"><span data-stu-id="1670c-124">Click the **Access Keys** menu item under **Settings**</span></span>
1. <span data-ttu-id="1670c-125">Válassza ki a kulcs újbóli létrehozása és a folyamat befejezéséhez</span><span class="sxs-lookup"><span data-stu-id="1670c-125">Choose the key to regenerate and complete the process</span></span>

<span data-ttu-id="1670c-126">URL-címek lekérése után az új hozzáférési kulcs újragenerálása aláírt esetén.</span><span class="sxs-lookup"><span data-stu-id="1670c-126">URLs you retrieve after regeneration are signed with the new access key.</span></span>

#### <a name="creating-callback-urls-with-an-expiration-date"></a><span data-ttu-id="1670c-127">A lejárati dátum visszahívási URL-címek létrehozása</span><span class="sxs-lookup"><span data-stu-id="1670c-127">Creating callback URLs with an expiration date</span></span>

<span data-ttu-id="1670c-128">Ha az URL-címet oszt meg más felek, a URL-címeket adott kulcsokkal és a lejárati dátumok igény szerint is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="1670c-128">If you are sharing the URL with other parties, you can generate URLs with specific keys and expiration dates as needed.</span></span> <span data-ttu-id="1670c-129">Ezután zökkenőmentesen kulcsok visszaállítani, vagy győződjön meg arról, bizonyos timespan hozzáférés az alkalmazás érvényesítést korlátozódik.</span><span class="sxs-lookup"><span data-stu-id="1670c-129">You can then seamlessly roll keys, or ensure access to fire an app is restricted to a certain timespan.</span></span> <span data-ttu-id="1670c-130">Adott URL esetén keresztül is megadhat a lejárati dátumot a [REST API-t a logic apps](https://docs.microsoft.com/rest/api/logic/workflowtriggers):</span><span class="sxs-lookup"><span data-stu-id="1670c-130">You can specify an expiration date for a URL through the [logic apps REST API](https://docs.microsoft.com/rest/api/logic/workflowtriggers):</span></span>

``` http
POST 
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Logic/workflows/{workflowName}/triggers/{triggerName}/listCallbackUrl?api-version=2016-06-01
```

<span data-ttu-id="1670c-131">A törzsben szereplő tartalmazza a tulajdonságot `NotAfter` karakterláncként JSON dátum, amely adja vissza egy visszahívási URL-címet, amely csak akkor érvényes, amíg a `NotAfter` dátuma és időpontja.</span><span class="sxs-lookup"><span data-stu-id="1670c-131">In the body, include the property `NotAfter` as a JSON date string, which returns a callback URL that is only valid until the `NotAfter` date and time.</span></span>

#### <a name="creating-urls-with-primary-or-secondary-secret-key"></a><span data-ttu-id="1670c-132">Elsődleges vagy másodlagos titkos kulccsal URL-címek létrehozása</span><span class="sxs-lookup"><span data-stu-id="1670c-132">Creating URLs with primary or secondary secret key</span></span>

<span data-ttu-id="1670c-133">Létrehozni, vagy a visszahívási kérelem-alapú eseményindítók URL-listában, mely kulcs használatával írják alá az URL-cím is megadható.</span><span class="sxs-lookup"><span data-stu-id="1670c-133">When you generate or list callback URLs for request-based triggers, you can also specify which key to use to sign the URL.</span></span>  <span data-ttu-id="1670c-134">Létrehozhat egy URL-cím, egy adott kulcs használatával írja alá a [REST API-t a logic apps](https://docs.microsoft.com/rest/api/logic/workflowtriggers) az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="1670c-134">You can generate a URL signed by a specific key through the [logic apps REST API](https://docs.microsoft.com/rest/api/logic/workflowtriggers) as follows:</span></span>

``` http
POST 
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Logic/workflows/{workflowName}/triggers/{triggerName}/listCallbackUrl?api-version=2016-06-01
```

<span data-ttu-id="1670c-135">A törzsben szereplő tartalmazza a tulajdonságot `KeyType` , vagyis `Primary` vagy `Secondary`.</span><span class="sxs-lookup"><span data-stu-id="1670c-135">In the body, include the property `KeyType` as either `Primary` or `Secondary`.</span></span>  <span data-ttu-id="1670c-136">Ez visszaadja a megadott biztonságos kulcs által aláírt URL-címet.</span><span class="sxs-lookup"><span data-stu-id="1670c-136">This returns a URL signed by the secure key specified.</span></span>

### <a name="restrict-incoming-ip-addresses"></a><span data-ttu-id="1670c-137">Bejövő IP-címek korlátozása</span><span class="sxs-lookup"><span data-stu-id="1670c-137">Restrict incoming IP addresses</span></span>

<span data-ttu-id="1670c-138">A közös hozzáférésű Jogosultságkód mellett Kezdésként korlátozhatja a logikai alkalmazás hívása csak az adott ügyfelektől.</span><span class="sxs-lookup"><span data-stu-id="1670c-138">In addition to the Shared Access Signature, you may wish to restrict calling a logic app only from specific clients.</span></span>  <span data-ttu-id="1670c-139">Például ha Ön kezeli a végpont Azure API Management keresztül, korlátozhatja a logikai alkalmazás csak a kérés elfogadása, ha a kérelem érkezik, az API Management példány IP-címről.</span><span class="sxs-lookup"><span data-stu-id="1670c-139">For example, if you manage your endpoint through Azure API Management, you can restrict the logic app to only accept the request when the request comes from the API Management instance IP address.</span></span>

<span data-ttu-id="1670c-140">Ez a beállítás konfigurálható a logic app beállításokat:</span><span class="sxs-lookup"><span data-stu-id="1670c-140">This setting can be configured within the logic app settings:</span></span>

1. <span data-ttu-id="1670c-141">Az Azure-portálon nyissa meg a logikai alkalmazást szeretne hozzáadni az IP-címkorlátozások</span><span class="sxs-lookup"><span data-stu-id="1670c-141">In the Azure portal, open the logic app you want to add IP address restrictions</span></span>
1. <span data-ttu-id="1670c-142">Kattintson a **hozzáférés-vezérlési konfiguráció** menüpont alatt **beállítások**</span><span class="sxs-lookup"><span data-stu-id="1670c-142">Click the **Access control configuration** menu item under **Settings**</span></span>
1. <span data-ttu-id="1670c-143">Adja meg az eseményindító fogadja el az IP-címtartományok listáját</span><span class="sxs-lookup"><span data-stu-id="1670c-143">Specify the list of IP address ranges to be accepted by the trigger</span></span>

<span data-ttu-id="1670c-144">Egy érvényes IP-címtartomány veszi a formátum `192.168.1.1/255`.</span><span class="sxs-lookup"><span data-stu-id="1670c-144">A valid IP range takes the format `192.168.1.1/255`.</span></span> <span data-ttu-id="1670c-145">Ha azt szeretné, hogy a logikai alkalmazás csak indul el egy beágyazott logikai alkalmazást, válassza a **csak más logic apps** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="1670c-145">If you want the logic app to only fire as a nested logic app, select the **Only other logic apps** option.</span></span> <span data-ttu-id="1670c-146">Ez a beállítás ír üres tömb az erőforrás értelmében csak a saját magát (szülő logic apps) szolgáltatás hívásait sikeresen érvényesítést.</span><span class="sxs-lookup"><span data-stu-id="1670c-146">This option writes an empty array to the resource, meaning only calls from the service itself (parent logic apps) fire successfully.</span></span>

> [!NOTE]
> <span data-ttu-id="1670c-147">Továbbra is futtatható a logikai alkalmazást az kérelem eseményindító a REST API-n keresztül / felügyeleti `/triggers/{triggerName}/run` IP függetlenül.</span><span class="sxs-lookup"><span data-stu-id="1670c-147">You can still run a logic app with a request trigger through the REST API / Management `/triggers/{triggerName}/run` regardless of IP.</span></span> <span data-ttu-id="1670c-148">Ebben a forgatókönyvben szükséges hitelesítés engedélyezésébe az Azure REST API, és az összes esemény jelenik meg az Azure-naplózást.</span><span class="sxs-lookup"><span data-stu-id="1670c-148">This scenario requires authentication against the Azure REST API, and all events would appear in the Azure Audit Log.</span></span> <span data-ttu-id="1670c-149">Ennek megfelelően beállított hozzáférés-vezérlési házirendek.</span><span class="sxs-lookup"><span data-stu-id="1670c-149">Set access control policies accordingly.</span></span>

#### <a name="setting-ip-ranges-on-the-resource-definition"></a><span data-ttu-id="1670c-150">Az erőforrás-definíció IP-címtartományok beállítása</span><span class="sxs-lookup"><span data-stu-id="1670c-150">Setting IP ranges on the resource definition</span></span>

<span data-ttu-id="1670c-151">Ha használ egy [központi telepítési sablont](logic-apps-create-deploy-template.md) a központi telepítések automatizálásához, az IP-címtartomány beállításait konfigurálhatja az erőforrás-sablont.</span><span class="sxs-lookup"><span data-stu-id="1670c-151">If you are using a [deployment template](logic-apps-create-deploy-template.md) to automate your deployments, the IP range settings can be configured on the resource template.</span></span>  

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

### <a name="adding-azure-active-directory-oauth-or-other-security"></a><span data-ttu-id="1670c-152">Azure Active Directory, az OAuth és az egyéb biztonsági</span><span class="sxs-lookup"><span data-stu-id="1670c-152">Adding Azure Active Directory, OAuth, or other security</span></span>

<span data-ttu-id="1670c-153">További hitelesítési protokollok fölött egy logikai alkalmazás hozzáadása [Azure API Management](https://azure.microsoft.com/services/api-management/) hatékony monitoring, a biztonsági, a házirend és a tetszőleges végpontot, és ezt teszi közzé az API-k, logikai alkalmazás dokumentációja nyújt.</span><span class="sxs-lookup"><span data-stu-id="1670c-153">To add more authorization protocols on top of a logic app, [Azure API Management](https://azure.microsoft.com/services/api-management/) offers rich monitoring, security, policy, and documentation for any endpoint with the capability to expose a logic app as an API.</span></span> <span data-ttu-id="1670c-154">Az Azure API Management is elérhetővé teheti a logikai alkalmazást, hogy az Azure Active Directory, a tanúsítványt, az OAuth vagy a más biztonsági szabványok használ a saját vagy nyilvános végpontja.</span><span class="sxs-lookup"><span data-stu-id="1670c-154">Azure API Management can expose a public or private endpoint for the logic app, which could use Azure Active Directory, certificate, OAuth, or other security standards.</span></span> <span data-ttu-id="1670c-155">Amikor egy kérelem érkezik, a Azure API Management továbbítja a kérést a logikai alkalmazás (hajt végre semmilyen szükséges átalakítások vagy üzenetsoroktól korlátozások).</span><span class="sxs-lookup"><span data-stu-id="1670c-155">When a request is received, Azure API Management forwards the request to the logic app (performing any needed transformations or restrictions in-flight).</span></span> <span data-ttu-id="1670c-156">Az a logikai alkalmazást a bejövő IP-címtartomány beállítások segítségével engedélyezése csak a logikai alkalmazást az API Management aktiválására.</span><span class="sxs-lookup"><span data-stu-id="1670c-156">You can use the incoming IP range settings on the logic app to only allow the logic app to be triggered from API Management.</span></span>

## <a name="secure-access-to-manage-or-edit-logic-apps"></a><span data-ttu-id="1670c-157">Biztonságos hozzáférés kezeléséhez vagy a logic apps szerkesztése</span><span class="sxs-lookup"><span data-stu-id="1670c-157">Secure access to manage or edit logic apps</span></span>

<span data-ttu-id="1670c-158">Hozzáférés a logikai alkalmazás felügyeleti műveleteihez korlátozhatja, hogy csak bizonyos felhasználók vagy csoportok képesek az erőforrás-műveletek végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="1670c-158">You can restrict access to management operations on a logic app so that only specific users or groups are able to perform operations on the resource.</span></span> <span data-ttu-id="1670c-159">A Logic apps használata az Azure [szerepköralapú hozzáférés-vezérlést (RBAC)](../active-directory/role-based-access-control-configure.md) a beállítást, és testre szabható eszközök.</span><span class="sxs-lookup"><span data-stu-id="1670c-159">Logic apps use the Azure [Role-Based Access Control (RBAC)](../active-directory/role-based-access-control-configure.md) feature, and can be customized with the same tools.</span></span>  <span data-ttu-id="1670c-160">Az előfizetés tagjai, valamint rendelje hozzá néhány beépített szerepkörök állnak rendelkezésre:</span><span class="sxs-lookup"><span data-stu-id="1670c-160">There are a few built-in roles you can assign members of your subscription to as well:</span></span>

* <span data-ttu-id="1670c-161">**Logic App közreműködői** -megtekintése, szerkesztése és frissítse a logikai alkalmazás hozzáférést biztosít.</span><span class="sxs-lookup"><span data-stu-id="1670c-161">**Logic App Contributor** - Provides access to view, edit, and update a logic app.</span></span>  <span data-ttu-id="1670c-162">Nem lehet eltávolítani az erőforrás, vagy felügyeleti műveleteket.</span><span class="sxs-lookup"><span data-stu-id="1670c-162">Cannot remove the resource or perform admin operations.</span></span>
* <span data-ttu-id="1670c-163">**Logic App operátor** – megtekintheti a logikai alkalmazást és futtatási előzményei, és engedélyezését vagy letiltását.</span><span class="sxs-lookup"><span data-stu-id="1670c-163">**Logic App Operator** - Can view the logic app and run history, and enable/disable.</span></span>  <span data-ttu-id="1670c-164">Nem szerkeszthetők, vagy a definíció frissítése.</span><span class="sxs-lookup"><span data-stu-id="1670c-164">Cannot edit or update the definition.</span></span>

<span data-ttu-id="1670c-165">Is [Azure erőforrás-zárolás](../azure-resource-manager/resource-group-lock-resources.md) módosítása vagy törlése a logic apps megelőzése érdekében.</span><span class="sxs-lookup"><span data-stu-id="1670c-165">You can also use [Azure Resource Lock](../azure-resource-manager/resource-group-lock-resources.md) to prevent changing or deleting logic apps.</span></span> <span data-ttu-id="1670c-166">Ez a lehetőség akkor hasznos, ha a termelési erőforrások a módosítások és a törlések megakadályozása.</span><span class="sxs-lookup"><span data-stu-id="1670c-166">This capability is valuable to prevent production resources from changes or deletions.</span></span>

## <a name="secure-access-to-contents-of-the-run-history"></a><span data-ttu-id="1670c-167">Biztonságos hozzáférés a tartalmát a futtatási előzményei</span><span class="sxs-lookup"><span data-stu-id="1670c-167">Secure access to contents of the run history</span></span>

<span data-ttu-id="1670c-168">Hozzáférés az előző fut az adott IP-címtartományokhoz korlátozhatja bemeneti vagy kimeneti tartalmát.</span><span class="sxs-lookup"><span data-stu-id="1670c-168">You can restrict access to contents of inputs or outputs from previous runs to specific IP address ranges.</span></span>  

<span data-ttu-id="1670c-169">Egy munkafolyamat sorozaton belül összes adata titkosításra kerül, az átvitel során, és inaktív.</span><span class="sxs-lookup"><span data-stu-id="1670c-169">All data within a workflow run is encrypted in transit and at rest.</span></span> <span data-ttu-id="1670c-170">Előzmények futtatásához a rendszer telefonhívást indít, amikor a szolgáltatás hitelesíti a kérelmet, és a kérelem-válasz bemenetekhez és kimenetekhez mutató hivatkozásokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="1670c-170">When a call to run history is made, the service authenticates the request and provides links to the request and response inputs and outputs.</span></span> <span data-ttu-id="1670c-171">Ez a hivatkozás csak kérések megtekintése a kijelölt IP-címtartomány tartalmat ad vissza a tartalom védelme.</span><span class="sxs-lookup"><span data-stu-id="1670c-171">This link can be protected so only requests to view content from a designated IP address range return the contents.</span></span> <span data-ttu-id="1670c-172">Ez a funkció további hozzáférés-vezérléshez is használhatja.</span><span class="sxs-lookup"><span data-stu-id="1670c-172">You can use this capability for additional access control.</span></span> <span data-ttu-id="1670c-173">Akkor kell megadni egy IP-címet, például `0.0.0.0` , nem sikerült elérni a bemenetek/kimenetek.</span><span class="sxs-lookup"><span data-stu-id="1670c-173">You could even specify an IP address like `0.0.0.0` so no one could access inputs/outputs.</span></span> <span data-ttu-id="1670c-174">Csak a személy rendszergazdai jogosultságokkal tudta eltávolítani ezt a korlátozást, lehetőséget biztosít munkafolyamat tartalma "just-in-time" elérésére.</span><span class="sxs-lookup"><span data-stu-id="1670c-174">Only someone with admin permissions could remove this restriction, providing the possibility for 'just-in-time' access to workflow contents.</span></span>

<span data-ttu-id="1670c-175">Ezt a beállítást az erőforrás-beállítások, az Azure portál belül kell megadni:</span><span class="sxs-lookup"><span data-stu-id="1670c-175">This setting can be configured within the resource settings of the Azure portal:</span></span>

1. <span data-ttu-id="1670c-176">Az Azure-portálon nyissa meg a logikai alkalmazást szeretne hozzáadni az IP-címkorlátozások</span><span class="sxs-lookup"><span data-stu-id="1670c-176">In the Azure portal, open the logic app you want to add IP address restrictions</span></span>
1. <span data-ttu-id="1670c-177">Kattintson a **hozzáférés-vezérlési konfiguráció** menüpont alatt **beállítások**</span><span class="sxs-lookup"><span data-stu-id="1670c-177">Click the **Access control configuration** menu item under **Settings**</span></span>
1. <span data-ttu-id="1670c-178">A tartalmakhoz való hozzáférést az IP-címtartományok listájának megadása</span><span class="sxs-lookup"><span data-stu-id="1670c-178">Specify the list of IP address ranges for access to content</span></span>

#### <a name="setting-ip-ranges-on-the-resource-definition"></a><span data-ttu-id="1670c-179">Az erőforrás-definíció IP-címtartományok beállítása</span><span class="sxs-lookup"><span data-stu-id="1670c-179">Setting IP ranges on the resource definition</span></span>

<span data-ttu-id="1670c-180">Ha használ egy [központi telepítési sablont](logic-apps-create-deploy-template.md) a központi telepítések automatizálásához, az IP-címtartomány beállításait konfigurálhatja az erőforrás-sablont.</span><span class="sxs-lookup"><span data-stu-id="1670c-180">If you are using a [deployment template](logic-apps-create-deploy-template.md) to automate your deployments, the IP range settings can be configured on the resource template.</span></span>  

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

## <a name="secure-parameters-and-inputs-within-a-workflow"></a><span data-ttu-id="1670c-181">Biztonságos paraméterek és a bemenetek egy munkafolyamaton belül</span><span class="sxs-lookup"><span data-stu-id="1670c-181">Secure parameters and inputs within a workflow</span></span>

<span data-ttu-id="1670c-182">Előfordulhat, hogy szeretné a telepítési munkafolyamat definícióját egyes funkcióit parametrizálja környezetek között.</span><span class="sxs-lookup"><span data-stu-id="1670c-182">You might want to parameterize some aspects of a workflow definition for deployment across environments.</span></span> <span data-ttu-id="1670c-183">Egyes paraméterek előfordulhat, hogy is biztonságos paraméterek, akkor nem jelenik meg a munkafolyamat, például egy ügyfél-azonosító és a titkos ügyfélkulcs szerkesztésekor [Azure Active Directory hitelesítési](../connectors/connectors-native-http.md#authentication) egy HTTP-művelet.</span><span class="sxs-lookup"><span data-stu-id="1670c-183">Also, some parameters might be secure parameters you don't want to appear when editing a workflow, such as a client ID and client secret for [Azure Active Directory authentication](../connectors/connectors-native-http.md#authentication) of an HTTP action.</span></span>

### <a name="using-parameters-and-secure-parameters"></a><span data-ttu-id="1670c-184">Paraméterek és biztonságos paraméterek</span><span class="sxs-lookup"><span data-stu-id="1670c-184">Using parameters and secure parameters</span></span>

<span data-ttu-id="1670c-185">Alkalmazása futásidőben egy erőforrás paraméter értékének eléréséhez a [munkafolyamatdefiníciós nyelve](http://aka.ms/logicappsdocs) biztosít egy `@parameters()` műveletet.</span><span class="sxs-lookup"><span data-stu-id="1670c-185">To access the value of a resource parameter at runtime, the [workflow definition language](http://aka.ms/logicappsdocs) provides a `@parameters()` operation.</span></span> <span data-ttu-id="1670c-186">Akkor is, [adja meg a paraméterek az erőforrás központi telepítési sablont a](../azure-resource-manager/resource-group-authoring-templates.md#parameters).</span><span class="sxs-lookup"><span data-stu-id="1670c-186">Also, you can [specify parameters in the resource deployment template](../azure-resource-manager/resource-group-authoring-templates.md#parameters).</span></span> <span data-ttu-id="1670c-187">De ha a paraméter típusa, adja meg `securestring`, a paraméter és a többi az erőforrás-definíció nem adható vissza, és nem sikerült az erőforrás-telepítést követően elérhető lesz.</span><span class="sxs-lookup"><span data-stu-id="1670c-187">But if you specify the parameter type as `securestring`, the parameter won't be returned with the rest of the resource definition, and won't be accessible by viewing the resource after deployment.</span></span>

> [!NOTE]
> <span data-ttu-id="1670c-188">Ha a paraméter a fejléc vagy a kérelem törzse paraméter is láthatják őket a futtatási előzményei, majd kimenő HTTP-kérelem.</span><span class="sxs-lookup"><span data-stu-id="1670c-188">If your parameter is used in the headers or body of a request, the parameter might be visible by accessing the run history and outgoing HTTP request.</span></span> <span data-ttu-id="1670c-189">Ügyeljen arra, hogy a tartalom-hozzáférési szabályzatok beállítását ennek megfelelően.</span><span class="sxs-lookup"><span data-stu-id="1670c-189">Make sure to set your content access policies accordingly.</span></span>
> <span data-ttu-id="1670c-190">Engedélyezési fejléceket keresztül bemeneti vagy kimeneti soha nem láthatók el.</span><span class="sxs-lookup"><span data-stu-id="1670c-190">Authorization headers are never visible through inputs or outputs.</span></span> <span data-ttu-id="1670c-191">Ezért ha a titkos kulcs van használatban van, a titkos kulcs nincs lekérhető.</span><span class="sxs-lookup"><span data-stu-id="1670c-191">So if the secret is being used there, the secret is not retrievable.</span></span>

#### <a name="resource-deployment-template-with-secrets"></a><span data-ttu-id="1670c-192">Erőforrás központi telepítési sablont a titkos kulcsok</span><span class="sxs-lookup"><span data-stu-id="1670c-192">Resource deployment template with secrets</span></span>

<span data-ttu-id="1670c-193">A következő példa bemutatja a központi telepítés egy biztonságos paramétere hivatkozó `secret` futásidőben.</span><span class="sxs-lookup"><span data-stu-id="1670c-193">The following example shows a deployment that references a secure parameter of `secret` at runtime.</span></span> <span data-ttu-id="1670c-194">Egy külön paraméterek fájlban kell megadni a környezet értéke a `secret`, vagy használjon [Azure Resource Manager KeyVault](../azure-resource-manager/resource-manager-keyvault-parameter.md) beolvasni a titkos kulcsok telepítése idő.</span><span class="sxs-lookup"><span data-stu-id="1670c-194">In a separate parameters file, you could specify the environment value for the `secret`, or use [Azure Resource Manager KeyVault](../azure-resource-manager/resource-manager-keyvault-parameter.md) to retrieve secrets at deploy time.</span></span>

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
                "body": "This is the request"
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

## <a name="secure-access-to-services-receiving-requests-from-a-workflow"></a><span data-ttu-id="1670c-195">Biztonságos hozzáférés a munkafolyamat érkező kérelmek fogadására szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="1670c-195">Secure access to services receiving requests from a workflow</span></span>

<span data-ttu-id="1670c-196">Számos módon biztonságossá a logic app hozzáférésre van szüksége a tetszőleges végpontot.</span><span class="sxs-lookup"><span data-stu-id="1670c-196">There are many ways to help secure any endpoint the logic app needs to access.</span></span>

### <a name="using-authentication-on-outbound-requests"></a><span data-ttu-id="1670c-197">A kimenő kérelmek-hitelesítéssel</span><span class="sxs-lookup"><span data-stu-id="1670c-197">Using authentication on outbound requests</span></span>

<span data-ttu-id="1670c-198">Az egy HTTP-, HTTP + Swagger (nyílt API-t), vagy a Webhook művelet használatakor a küldési kérelem hitelesítési is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="1670c-198">When working with an HTTP, HTTP + Swagger (Open API), or Webhook action, you can add authentication to the request being sent.</span></span> <span data-ttu-id="1670c-199">Alapszintű hitelesítés, tanúsítvány vagy Azure Active Directory-hitelesítés tartalmazhatnak.</span><span class="sxs-lookup"><span data-stu-id="1670c-199">You could include basic authentication, certificate authentication, or Azure Active Directory authentication.</span></span> <span data-ttu-id="1670c-200">Ez a hitelesítés konfigurálásával kapcsolatos részletek megtalálhatók [ebben a cikkben](../connectors/connectors-native-http.md#authentication).</span><span class="sxs-lookup"><span data-stu-id="1670c-200">Details on how to configure this authentication can be found [in this article](../connectors/connectors-native-http.md#authentication).</span></span>

### <a name="restricting-access-to-logic-app-ip-addresses"></a><span data-ttu-id="1670c-201">Logic app IP-címek való hozzáférés korlátozása</span><span class="sxs-lookup"><span data-stu-id="1670c-201">Restricting access to logic app IP addresses</span></span>

<span data-ttu-id="1670c-202">A logic Apps alkalmazásokból összes hívások régiónként eltérő IP-címek meghatározott készletének határozza meg.</span><span class="sxs-lookup"><span data-stu-id="1670c-202">All calls from logic apps come from a specific set of IP addresses per region.</span></span> <span data-ttu-id="1670c-203">Hozzáadhat további szűrés csak az adott kijelölt IP-címekről érkező kérelmek fogadására.</span><span class="sxs-lookup"><span data-stu-id="1670c-203">You can add additional filtering to only accept requests from those designated IP addresses.</span></span> <span data-ttu-id="1670c-204">Egy adott IP-címek listáját lásd: [logic app korlátozásai és konfigurációja](logic-apps-limits-and-config.md#configuration).</span><span class="sxs-lookup"><span data-stu-id="1670c-204">For a list of those IP addresses, see [logic app limits and configuration](logic-apps-limits-and-config.md#configuration).</span></span>

### <a name="on-premises-connectivity"></a><span data-ttu-id="1670c-205">Helyszíni kapcsolatok</span><span class="sxs-lookup"><span data-stu-id="1670c-205">On-premises connectivity</span></span>

<span data-ttu-id="1670c-206">A Logic apps a biztonságos és megbízható több szolgáltatásokkal való integrációt a helyszíni kommunikációt biztosítanak.</span><span class="sxs-lookup"><span data-stu-id="1670c-206">Logic apps provide integration with several services to provide secure and reliable on-premises communication.</span></span>

#### <a name="on-premises-data-gateway"></a><span data-ttu-id="1670c-207">Helyi adatátjáró</span><span class="sxs-lookup"><span data-stu-id="1670c-207">On-premises data gateway</span></span>

<span data-ttu-id="1670c-208">Sok felügyelt összekötők logic apps a helyszíni rendszerekre, például File System, a SQL, a SharePoint, a DB2 és egyéb biztonságos kapcsolatot biztosít.</span><span class="sxs-lookup"><span data-stu-id="1670c-208">Many managed connectors for logic apps provide secure connectivity to on-premises systems, including File System, SQL, SharePoint, DB2, and more.</span></span> <span data-ttu-id="1670c-209">Az átjáró továbbítja a titkosított csatornákon keresztül az Azure Service Bus helyszíni forrásból származó adatokat.</span><span class="sxs-lookup"><span data-stu-id="1670c-209">The gateway relays data from on-premises sources on encrypted channels through the Azure Service Bus.</span></span> <span data-ttu-id="1670c-210">Az összes forgalom származik, az átjáró ügynök biztonságos kimenő forgalmát.</span><span class="sxs-lookup"><span data-stu-id="1670c-210">All traffic originates as secure outbound traffic from the gateway agent.</span></span> <span data-ttu-id="1670c-211">További információ [az átjáró működése](logic-apps-gateway-install.md#gateway-cloud-service).</span><span class="sxs-lookup"><span data-stu-id="1670c-211">Learn more about [how the data gateway works](logic-apps-gateway-install.md#gateway-cloud-service).</span></span>

#### <a name="azure-api-management"></a><span data-ttu-id="1670c-212">Azure API Management</span><span class="sxs-lookup"><span data-stu-id="1670c-212">Azure API Management</span></span>

<span data-ttu-id="1670c-213">[Az Azure API Management](https://azure.microsoft.com/services/api-management/) rendelkezik a helyszíni kapcsolati lehetőségek, többek között pont-pont VPN- és ExpressRoute integráció biztonságos proxy és a helyszíni rendszerekre történő kommunikációhoz.</span><span class="sxs-lookup"><span data-stu-id="1670c-213">[Azure API Management](https://azure.microsoft.com/services/api-management/) has on-premises connectivity options, including site-to-site VPN and ExpressRoute integration for secured proxy and communication to on-premises systems.</span></span> <span data-ttu-id="1670c-214">A Logic App Designer az API-k felfedni a Azure API Management egy munkafolyamaton belül a helyszíni rendszerekben gyors elérését biztosító gyorsan kiválaszthatja.</span><span class="sxs-lookup"><span data-stu-id="1670c-214">In the Logic App Designer, you can quickly select an API exposed from Azure API Management within a workflow, providing quick access to on-premises systems.</span></span>

#### <a name="hybrid-connections-from-azure-app-service"></a><span data-ttu-id="1670c-215">Hibrid kapcsolatok az Azure App Service</span><span class="sxs-lookup"><span data-stu-id="1670c-215">Hybrid connections from Azure App Service</span></span>

<span data-ttu-id="1670c-216">A helyszíni hibrid kapcsolat funkció az Azure API és a Web apps segítségével helyszíni kommunikációhoz.</span><span class="sxs-lookup"><span data-stu-id="1670c-216">You can use the on-premises hybrid connection feature for Azure API and Web apps to communicate on-premises.</span></span>  <span data-ttu-id="1670c-217">Hibrid kapcsolatok és konfigurálása a részletek megtalálhatók [ebben a cikkben](../app-service-web/web-sites-hybrid-connection-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="1670c-217">Details on hybrid connections and how to configure can be found [in this article](../app-service-web/web-sites-hybrid-connection-get-started.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="1670c-218">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1670c-218">Next steps</span></span>
[<span data-ttu-id="1670c-219">Központi telepítési sablon létrehozása</span><span class="sxs-lookup"><span data-stu-id="1670c-219">Create a deployment template</span></span>](logic-apps-create-deploy-template.md)  
[<span data-ttu-id="1670c-220">Kivételkezelés</span><span class="sxs-lookup"><span data-stu-id="1670c-220">Exception handling</span></span>](logic-apps-exception-handling.md)  
[<span data-ttu-id="1670c-221">Logikai alkalmazások figyelése</span><span class="sxs-lookup"><span data-stu-id="1670c-221">Monitor your logic apps</span></span>](logic-apps-monitor-your-logic-apps.md)  
[<span data-ttu-id="1670c-222">Logic app hibák és problémák diagnosztizálása</span><span class="sxs-lookup"><span data-stu-id="1670c-222">Diagnosing logic app failures and issues</span></span>](logic-apps-diagnosing-failures.md)  
