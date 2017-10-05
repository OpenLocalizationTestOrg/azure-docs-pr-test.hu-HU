---
title: "Házirendek az Azure API Management |} Microsoft Docs"
description: "Megtudhatja, hogyan létrehozása, szerkesztése és API-felügyeleti szabályzatok konfigurálására."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 537e5caf-708b-430e-a83f-72b70af28aa9
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 7c1f235343074ec11c635097f2b094a10f3fe781
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="policies-in-azure-api-management"></a><span data-ttu-id="0414f-103">Házirendek az Azure API Management</span><span class="sxs-lookup"><span data-stu-id="0414f-103">Policies in Azure API Management</span></span>
<span data-ttu-id="0414f-104">Az Azure API Management a házirendek olyan egy hatékony képesség, a rendszer, amelyek lehetővé teszik a közzétevőt úgy, hogy az API-t konfigurálással működésének módosításához.</span><span class="sxs-lookup"><span data-stu-id="0414f-104">In Azure API Management, policies are a powerful capability of the system that allow the publisher to change the behavior of the API through configuration.</span></span> <span data-ttu-id="0414f-105">Házirendek olyan utasításokat, amelyek egymás után, ha a kérés végrehajtása gyűjteménye vagy egy API-t adott válaszokat.</span><span class="sxs-lookup"><span data-stu-id="0414f-105">Policies are a collection of Statements that are executed sequentially on the request or response of an API.</span></span> <span data-ttu-id="0414f-106">Népszerű utasítások JSON az XML-formátumú átalakítás tartalmazza, és hívja meg egy fejlesztő a bejövő hívások mennyiségének korlátozásához sebességével.</span><span class="sxs-lookup"><span data-stu-id="0414f-106">Popular Statements include format conversion from XML to JSON and call rate limiting to restrict the amount of incoming calls from a developer.</span></span> <span data-ttu-id="0414f-107">Számos további házirendeket nem érhetők el.</span><span class="sxs-lookup"><span data-stu-id="0414f-107">Many more policies are available out of the box.</span></span>

<span data-ttu-id="0414f-108">Tekintse meg a [szabályzataihoz] [ Policy Reference] házirend-utasításoknál és a beállítások teljes listáját.</span><span class="sxs-lookup"><span data-stu-id="0414f-108">See the [Policy Reference][Policy Reference] for a full list of policy statements and their settings.</span></span>

<span data-ttu-id="0414f-109">Házirendek az átjárón, amely az API-ügyfél és a felügyelt API között helyezkedik el belül érvényesek.</span><span class="sxs-lookup"><span data-stu-id="0414f-109">Policies are applied inside the gateway which sits between the API consumer and the managed API.</span></span> <span data-ttu-id="0414f-110">Az átjáró összes kéréseket fogad, és általában továbbítja őket a mögöttes API változatlan.</span><span class="sxs-lookup"><span data-stu-id="0414f-110">The gateway receives all requests and usually forwards them unaltered to the underlying API.</span></span> <span data-ttu-id="0414f-111">Azonban egy házirend módosításokat is alkalmazhatja a bejövő kérelem és a kimenő válasz.</span><span class="sxs-lookup"><span data-stu-id="0414f-111">However a policy can apply changes to both the inbound request and outbound response.</span></span>

<span data-ttu-id="0414f-112">A házirend-kifejezéseket attribútumértékekként vagy szövegértékekként lehet használni bármelyik API Management házirendben, hacsak a házirend másként nem rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="0414f-112">Policy expressions can be used as attribute values or text values in any of the API Management policies, unless the policy specifies otherwise.</span></span> <span data-ttu-id="0414f-113">Egyes házirendek, például a [folyamatot szabályozhatja] [ Control flow] és [Set változó] [ Set variable] házirendek a házirend-kifejezések alapulnak.</span><span class="sxs-lookup"><span data-stu-id="0414f-113">Some policies such as the [Control flow][Control flow] and [Set variable][Set variable] policies are based on policy expressions.</span></span> <span data-ttu-id="0414f-114">További információkért lásd: [házirendek speciális] [ Advanced policies] és [házirend-kifejezések][Policy expressions].</span><span class="sxs-lookup"><span data-stu-id="0414f-114">For more information, see [Advanced policies][Advanced policies] and [Policy expressions][Policy expressions].</span></span>

## <span data-ttu-id="0414f-115"><a name="scopes"></a>Házirendek konfigurálása</span><span class="sxs-lookup"><span data-stu-id="0414f-115"><a name="scopes"> </a>How to configure policies</span></span>
<span data-ttu-id="0414f-116">Házirendek úgy is konfigurálhatók, globálisan vagy a hatókörben egy [termék][Product], [API] [ API] vagy [művelet] [Operation].</span><span class="sxs-lookup"><span data-stu-id="0414f-116">Policies can be configured globally or at the scope of a [Product][Product], [API][API] or [Operation][Operation].</span></span> <span data-ttu-id="0414f-117">Konfigurálja a házirendet, navigáljon a házirendek szerkesztő a közzétevő portálon.</span><span class="sxs-lookup"><span data-stu-id="0414f-117">To configure a policy, navigate to the Policies editor in the publisher portal.</span></span>

![Házirendek menü][policies-menu]

<span data-ttu-id="0414f-119">A házirendek szerkesztő három fő részből áll: a házirend-hatókörnek (felső), a házirend-definíció, ahol házirendek szerkesztése (bal oldali) és a kimutatások listában (jobbra):</span><span class="sxs-lookup"><span data-stu-id="0414f-119">The policies editor consists of three main sections: the policy scope (top), the policy definition where policies are edited (left) and the statements list (right):</span></span>

![Házirendek szerkesztő][policies-editor]

<span data-ttu-id="0414f-121">Egy házirend konfigurálásának a megkezdéséhez ki kell választania a hatókör, ahol a házirendet alkalmazni kell.</span><span class="sxs-lookup"><span data-stu-id="0414f-121">To begin configuring a policy you must first select the scope at which the policy should apply.</span></span> <span data-ttu-id="0414f-122">Az alábbi képernyőképen a **alapszintű** termék van kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="0414f-122">In the screenshot below the **Starter** product is selected.</span></span> <span data-ttu-id="0414f-123">Vegye figyelembe, hogy a házirend neve melletti négyzet szimbólum azt jelzi, hogy egy házirend már alkalmazva van ezen a szinten.</span><span class="sxs-lookup"><span data-stu-id="0414f-123">Note that the square symbol next to the policy name indicates that a policy is already applied at this level.</span></span>

![Hatókör][policies-scope]

<span data-ttu-id="0414f-125">Óta egy házirend már telepítve van, a definíció nézetben jelenik meg a konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="0414f-125">Since a policy has already been applied, the configuration is shown in the definition view.</span></span>

![Konfigurálás][policies-configure]

<span data-ttu-id="0414f-127">A házirend csak olvasható jelenik meg először.</span><span class="sxs-lookup"><span data-stu-id="0414f-127">The policy is displayed read-only at first.</span></span> <span data-ttu-id="0414f-128">Kattintson a definíció szerkesztéséhez a **házirend konfigurálása** művelet.</span><span class="sxs-lookup"><span data-stu-id="0414f-128">In order to edit the definition click the **Configure Policy** action.</span></span>

![Szerkesztés][policies-edit]

<span data-ttu-id="0414f-130">A házirend-definíció egy egyszerű XML-dokumentumban, amely leírja a bejövő és kimenő utasítás sorozata.</span><span class="sxs-lookup"><span data-stu-id="0414f-130">The policy definition is a simple XML document that describes a sequence of inbound and outbound statements.</span></span> <span data-ttu-id="0414f-131">Az XML-fájl közvetlenül a definíció ablakban szerkeszthető.</span><span class="sxs-lookup"><span data-stu-id="0414f-131">The XML can be edited directly in the definition window.</span></span> <span data-ttu-id="0414f-132">Egy utasítás listája jobbra, és az aktuális hatókör vonatkozó utasítások engedélyezve van, és a kijelölt; ahogy ezt a **korlát hívás arány** utasítás a fenti képernyőfelvételen látható.</span><span class="sxs-lookup"><span data-stu-id="0414f-132">A list of statements is provided to the right and statements applicable to the current scope are enabled and highlighted; as demonstrated by the **Limit Call Rate** statement in the screenshot above.</span></span>

<span data-ttu-id="0414f-133">Egy engedélyezett utasítás gombra kattintva adja hozzá a megfelelő XML a kurzor a definíció nézetben a helyen.</span><span class="sxs-lookup"><span data-stu-id="0414f-133">Clicking an enabled statement will add the appropriate XML at the location of the cursor in the definition view.</span></span> 

> [!NOTE]
> <span data-ttu-id="0414f-134">Ha hozzá szeretne adni a házirend nincs engedélyezve, ellenőrizze, hogy az adott házirendnek megfelelő hatókörben.</span><span class="sxs-lookup"><span data-stu-id="0414f-134">If the policy that you want to add is not enabled, ensure that you are in the correct scope for that policy.</span></span> <span data-ttu-id="0414f-135">Minden egyes házirend-utasítás az egyes hatókörök és házirend szakaszok használatra szolgál.</span><span class="sxs-lookup"><span data-stu-id="0414f-135">Each policy statement is designed for use in certain scopes and policy sections.</span></span> <span data-ttu-id="0414f-136">Tekintse át a házirend szakaszok és egy házirend hatókörök, ellenőrizze a **használati** a házirendhez tartozó szakasz a [szabályzataihoz][Policy Reference].</span><span class="sxs-lookup"><span data-stu-id="0414f-136">To review the policy sections and scopes for a policy, check the **Usage** section for that policy in the [Policy Reference][Policy Reference].</span></span>
> 
> 

<span data-ttu-id="0414f-137">A házirend-utasításoknál teljes listáját és a beállítások érhetők el a [szabályzataihoz][Policy Reference].</span><span class="sxs-lookup"><span data-stu-id="0414f-137">A full list of policy statements and their settings are available in the [Policy Reference][Policy Reference].</span></span>

<span data-ttu-id="0414f-138">Például a megadott IP-címek korlátozása a bejövő kérelmeket egy új utasítás hozzáadása, vigye a kurzort, belülre tartalmát a `inbound` XML-elemet, és kattintson a **korlátozása hívó IP-címek** utasítás.</span><span class="sxs-lookup"><span data-stu-id="0414f-138">For example, to add a new statement to restrict incoming requests to specified IP addresses, place the cursor just inside the content of the `inbound` XML element and click the **Restrict caller IPs** statement.</span></span>

![A szoftverkorlátozó házirendek][policies-restrict]

<span data-ttu-id="0414f-140">Ez egy XML-részletet, hogy hozzáadja a `inbound` elem, amely az utasítás konfigurálásához nyújt útmutatást.</span><span class="sxs-lookup"><span data-stu-id="0414f-140">This will add an XML snippet to the `inbound` element that provides guidance on how to configure the statement.</span></span>

```xml
<ip-filter action="allow | forbid">
    <address>address</address>
    <address-range from="address" to="address"/>
</ip-filter>
```

<span data-ttu-id="0414f-141">Korlátozza a bejövő kéréseket, és fogadja csak azokat az IP-címről 1.2.3.4 módosítsa az XML-fájl az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="0414f-141">To limit inbound requests and accept only those from an IP address of 1.2.3.4 modify the XML as follows:</span></span>

```xml
<ip-filter action="allow">
    <address>1.2.3.4</address>
</ip-filter>
```

![Mentés][policies-save]

<span data-ttu-id="0414f-143">Amikor végzett a kimutatások a házirend konfigurálása, kattintson **mentése** és a módosítások propagálódik az API Management átjáró azonnal.</span><span class="sxs-lookup"><span data-stu-id="0414f-143">When complete configuring the statements for the policy, click **Save** and the changes will be propagated to the API Management gateway immediately.</span></span>

## <span data-ttu-id="0414f-144"><a name="sections"></a>Ismertetése házirend konfigurálása</span><span class="sxs-lookup"><span data-stu-id="0414f-144"><a name="sections"> </a>Understanding policy configuration</span></span>
<span data-ttu-id="0414f-145">A házirend utasításokat, amelyek ahhoz, hogy egy kérés- és a végrehajtása több.</span><span class="sxs-lookup"><span data-stu-id="0414f-145">A policy is a series of statements that execute in order for a request and a response.</span></span> <span data-ttu-id="0414f-146">A konfigurációs oszlik megfelelően `inbound`, `backend`, `outbound`, és `on-error` látható módon a következő konfigurációs szakasz.</span><span class="sxs-lookup"><span data-stu-id="0414f-146">The configuration is divided appropriately into `inbound`, `backend`, `outbound`, and `on-error` sections as shown in the following configuration.</span></span>

```xml
<policies>
  <inbound>
    <!-- statements to be applied to the request go here -->
  </inbound>
  <backend>
    <!-- statements to be applied before the request is forwarded to 
         the backend service go here -->
  </backend>
  <outbound>
    <!-- statements to be applied to the response go here -->
  </outbound>
  <on-error>
    <!-- statements to be applied if there is an error condition go here -->
  </on-error>
</policies> 
```

<span data-ttu-id="0414f-147">Ha a kérelem feldolgozása során hiba történt, a többi szükséges lépések a `inbound`, `backend`, vagy `outbound` szakaszok kimarad, és végrehajtási lévő utasítások ugrik a `on-error` szakasz.</span><span class="sxs-lookup"><span data-stu-id="0414f-147">If there is an error during the processing of a request, any remaining steps in the `inbound`, `backend`, or `outbound` sections are skipped and execution jumps to the statements in the `on-error` section.</span></span> <span data-ttu-id="0414f-148">Úgy, hogy a házirend-utasításoknál a `on-error` tekintse át a hiba a szakasz a `context.LastError` tulajdonság, nézze meg, és testre szabhatja a hiba válasz használatával a `set-body` házirend, és konfigurálása, mi történik, ha a hiba akkor fordul elő.</span><span class="sxs-lookup"><span data-stu-id="0414f-148">By placing policy statements in the `on-error` section you can review the error by using the `context.LastError` property, inspect and customize the error response using the `set-body` policy, and configure what happens if an error occurs.</span></span> <span data-ttu-id="0414f-149">Nincsenek hibakódok beépített lépéseket és a házirend-utasításoknál feldolgozása során előforduló hibákat.</span><span class="sxs-lookup"><span data-stu-id="0414f-149">There are error codes for built-in steps and for errors that may occur during the processing of policy statements.</span></span> <span data-ttu-id="0414f-150">További információkért lásd: [hiba történt az API-felügyeleti házirendek kezelése](https://msdn.microsoft.com/library/azure/mt629506.aspx).</span><span class="sxs-lookup"><span data-stu-id="0414f-150">For more information, see [Error handling in API Management policies](https://msdn.microsoft.com/library/azure/mt629506.aspx).</span></span>

<span data-ttu-id="0414f-151">Mivel a házirendek (globális, termék, api és művelet) különböző szinteken adható meg a konfigurációs módot biztosít a megadhatja a sorrendet, amelyben a házirend-definíció utasítások tekintetében a szülő házirend hajtható végre.</span><span class="sxs-lookup"><span data-stu-id="0414f-151">Since policies can be specified at different levels (global, product, api and operation) the configuration provides a way for you to specify the order in which the policy definition's statements execute with respect to the parent policy.</span></span> 

<span data-ttu-id="0414f-152">Házirend hatókörök a következő sorrendben értékeli ki a rendszer.</span><span class="sxs-lookup"><span data-stu-id="0414f-152">Policy scopes are evaluated in the following order.</span></span>

1. <span data-ttu-id="0414f-153">Globális hatókörű</span><span class="sxs-lookup"><span data-stu-id="0414f-153">Global scope</span></span>
2. <span data-ttu-id="0414f-154">A termék hatókör</span><span class="sxs-lookup"><span data-stu-id="0414f-154">Product scope</span></span>
3. <span data-ttu-id="0414f-155">API-hatókör</span><span class="sxs-lookup"><span data-stu-id="0414f-155">API scope</span></span>
4. <span data-ttu-id="0414f-156">Műveleti hatókör</span><span class="sxs-lookup"><span data-stu-id="0414f-156">Operation scope</span></span>

<span data-ttu-id="0414f-157">Azokat a utasítási kiértékelése az elhelyezkedését a `base` elem, ha telepítve.</span><span class="sxs-lookup"><span data-stu-id="0414f-157">The statements within them are evaluated according to the placement of the `base` element, if it is present.</span></span> <span data-ttu-id="0414f-158">Általános házirend nem szülője házirend és a használatával a `<base>` az elem nincs hatása.</span><span class="sxs-lookup"><span data-stu-id="0414f-158">Global policy has no parent policy and using the `<base>` element in it has no effect.</span></span>

<span data-ttu-id="0414f-159">Például, ha a globális szinten és az API-k konfigurált házirendek egy házirendet, majd, hogy adott API-t igénybe mindkét házirendeket a rendszer alkalmazza.</span><span class="sxs-lookup"><span data-stu-id="0414f-159">For example, if you have a policy at the global level and a policy configured for an API, then whenever that particular API is used both policies will be applied.</span></span> <span data-ttu-id="0414f-160">API-kezelés lehetővé teszi, hogy az Alap elem keresztül kombinált házirend kimutatások determinisztikus rendezéshez.</span><span class="sxs-lookup"><span data-stu-id="0414f-160">API Management allows for deterministic ordering of combined policy statements via the base element.</span></span> 

```xml
<policies>
    <inbound>
        <cross-domain />
        <base />
        <find-and-replace from="xyz" to="abc" />
    </inbound>
</policies>
```

<span data-ttu-id="0414f-161">A fenti példa házirend-definíció a `cross-domain` utasítás futtatása előtt minden magasabb házirendet, amely viszont követnie a `find-and-replace` házirend.</span><span class="sxs-lookup"><span data-stu-id="0414f-161">In the example policy definition above, the `cross-domain` statement would execute before any higher policies which would in turn, be followed by the `find-and-replace` policy.</span></span> 

<span data-ttu-id="0414f-162">A házirendek a Helyicsoportházirend-szerkesztő a jelenlegi hatókörben megtekintéséhez kattintson **számítsa ki újra a kijelölt hatókör hatékony házirend**.</span><span class="sxs-lookup"><span data-stu-id="0414f-162">To see the policies in the current scope in the policy editor, click **Recalculate effective policy for selected scope**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0414f-163">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0414f-163">Next steps</span></span>
<span data-ttu-id="0414f-164">Tekintse meg a következő videót a házirend-kifejezések.</span><span class="sxs-lookup"><span data-stu-id="0414f-164">Check out following video on policy expressions.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Policy-Expressions-in-Azure-API-Management/player]
> 
> 

[Policy Reference]: api-management-policy-reference.md
[Product]: api-management-howto-add-products.md
[API]: api-management-howto-add-products.md#add-apis 
[Operation]: api-management-howto-add-operations.md

[Advanced policies]: https://msdn.microsoft.com/library/azure/dn894085.aspx
[Control flow]: https://msdn.microsoft.com/library/azure/dn894085.aspx#choose
[Set variable]: https://msdn.microsoft.com/library/azure/dn894085.aspx#set_variable
[Policy expressions]: https://msdn.microsoft.com/library/azure/dn910913.aspx

[policies-menu]: ./media/api-management-howto-policies/api-management-policies-menu.png
[policies-editor]: ./media/api-management-howto-policies/api-management-policies-editor.png
[policies-scope]: ./media/api-management-howto-policies/api-management-policies-scope.png
[policies-configure]: ./media/api-management-howto-policies/api-management-policies-configure.png
[policies-edit]: ./media/api-management-howto-policies/api-management-policies-edit.png
[policies-restrict]: ./media/api-management-howto-policies/api-management-policies-restrict.png
[policies-save]: ./media/api-management-howto-policies/api-management-policies-save.png
