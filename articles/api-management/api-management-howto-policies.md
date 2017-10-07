---
title: az Azure API Management aaaPolicies |} Microsoft Docs
description: "Ismerje meg, hogyan toocreate, szerkesztése és API-felügyeleti szabályzatok konfigurálására."
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
ms.openlocfilehash: 9ab0f884a655004cb10c05085034df1795f512e6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="policies-in-azure-api-management"></a><span data-ttu-id="6ee25-103">Házirendek az Azure API Management</span><span class="sxs-lookup"><span data-stu-id="6ee25-103">Policies in Azure API Management</span></span>
<span data-ttu-id="6ee25-104">Az Azure API Management a házirendek, amelyek lehetővé teszik a hello publisher toochange hello viselkedését hello API konfigurálással hello rendszer hatékony képesség.</span><span class="sxs-lookup"><span data-stu-id="6ee25-104">In Azure API Management, policies are a powerful capability of hello system that allow hello publisher toochange hello behavior of hello API through configuration.</span></span> <span data-ttu-id="6ee25-105">Házirendek olyan hello kérésre egymás után végrehajtott utasítások gyűjteménye vagy egy API-t adott válaszokat.</span><span class="sxs-lookup"><span data-stu-id="6ee25-105">Policies are a collection of Statements that are executed sequentially on hello request or response of an API.</span></span> <span data-ttu-id="6ee25-106">Népszerű utasítások XML tooJSON formátumú konverzió tartalmazza, és hívja meg a fejlesztők a bejövő hívások toorestrict hello mennyisége sebességével.</span><span class="sxs-lookup"><span data-stu-id="6ee25-106">Popular Statements include format conversion from XML tooJSON and call rate limiting toorestrict hello amount of incoming calls from a developer.</span></span> <span data-ttu-id="6ee25-107">Számos további házirendeket hello kezdő verzióról érhetők el.</span><span class="sxs-lookup"><span data-stu-id="6ee25-107">Many more policies are available out of hello box.</span></span>

<span data-ttu-id="6ee25-108">Lásd: hello [szabályzataihoz] [ Policy Reference] házirend-utasításoknál és a beállítások teljes listáját.</span><span class="sxs-lookup"><span data-stu-id="6ee25-108">See hello [Policy Reference][Policy Reference] for a full list of policy statements and their settings.</span></span>

<span data-ttu-id="6ee25-109">Házirendek hello átjárón, amely hello API fogyasztói és felügyelt hello API között helyezkedik el belül érvényesek.</span><span class="sxs-lookup"><span data-stu-id="6ee25-109">Policies are applied inside hello gateway which sits between hello API consumer and hello managed API.</span></span> <span data-ttu-id="6ee25-110">hello átjáró összes kéréseket fogad, és általában továbbítja őket a változatlan toohello az alapul szolgáló API.</span><span class="sxs-lookup"><span data-stu-id="6ee25-110">hello gateway receives all requests and usually forwards them unaltered toohello underlying API.</span></span> <span data-ttu-id="6ee25-111">A házirend azonban módosítások tooboth hello bejövő kérés- és kimenő is alkalmazhatja.</span><span class="sxs-lookup"><span data-stu-id="6ee25-111">However a policy can apply changes tooboth hello inbound request and outbound response.</span></span>

<span data-ttu-id="6ee25-112">Házirend-kifejezések használható attribútumértékek vagy szöveges értékek bármely hello API-felügyeleti házirendek, kivéve, ha hello házirend ellenkező esetben adja meg.</span><span class="sxs-lookup"><span data-stu-id="6ee25-112">Policy expressions can be used as attribute values or text values in any of hello API Management policies, unless hello policy specifies otherwise.</span></span> <span data-ttu-id="6ee25-113">Egyes házirendek, például a hello [folyamatot szabályozhatja] [ Control flow] és [Set változó] [ Set variable] házirendek a házirend-kifejezések alapulnak.</span><span class="sxs-lookup"><span data-stu-id="6ee25-113">Some policies such as hello [Control flow][Control flow] and [Set variable][Set variable] policies are based on policy expressions.</span></span> <span data-ttu-id="6ee25-114">További információkért lásd: [házirendek speciális] [ Advanced policies] és [házirend-kifejezések][Policy expressions].</span><span class="sxs-lookup"><span data-stu-id="6ee25-114">For more information, see [Advanced policies][Advanced policies] and [Policy expressions][Policy expressions].</span></span>

## <span data-ttu-id="6ee25-115"><a name="scopes"></a>Hogyan tooconfigure házirendek</span><span class="sxs-lookup"><span data-stu-id="6ee25-115"><a name="scopes"> </a>How tooconfigure policies</span></span>
<span data-ttu-id="6ee25-116">Házirendek úgy is konfigurálhatók, globálisan vagy hello hatókörét egy [termék][Product], [API] [ API] vagy [művelet] [Operation].</span><span class="sxs-lookup"><span data-stu-id="6ee25-116">Policies can be configured globally or at hello scope of a [Product][Product], [API][API] or [Operation][Operation].</span></span> <span data-ttu-id="6ee25-117">tooconfigure egy házirendet, keresse meg a toohello házirendek szerkesztő hello publisher portálon.</span><span class="sxs-lookup"><span data-stu-id="6ee25-117">tooconfigure a policy, navigate toohello Policies editor in hello publisher portal.</span></span>

![Házirendek menü][policies-menu]

<span data-ttu-id="6ee25-119">hello házirendek szerkesztő három fő részből áll: hello házirend hatókör (felső), hello házirend-definíció ahol házirendek szerkesztése (bal oldali) és hello utasítások listában (jobbra):</span><span class="sxs-lookup"><span data-stu-id="6ee25-119">hello policies editor consists of three main sections: hello policy scope (top), hello policy definition where policies are edited (left) and hello statements list (right):</span></span>

![Házirendek szerkesztő][policies-editor]

<span data-ttu-id="6ee25-121">a toobegin először válassza ki, mely hello házirend vonatkozzon hello hatókör házirend beállítása.</span><span class="sxs-lookup"><span data-stu-id="6ee25-121">toobegin configuring a policy you must first select hello scope at which hello policy should apply.</span></span> <span data-ttu-id="6ee25-122">Hello képernyőfelvételen látható, alább hello **alapszintű** termék van kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="6ee25-122">In hello screenshot below hello **Starter** product is selected.</span></span> <span data-ttu-id="6ee25-123">Vegye figyelembe, hogy hello négyzetes szimbólum következő toohello házirendnév azt jelzi, hogy egy házirend már alkalmazva van ezen a szinten.</span><span class="sxs-lookup"><span data-stu-id="6ee25-123">Note that hello square symbol next toohello policy name indicates that a policy is already applied at this level.</span></span>

![Hatókör][policies-scope]

<span data-ttu-id="6ee25-125">Egy házirend már telepítve van, mert hello konfigurációs hello típusdefiníció nézet jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="6ee25-125">Since a policy has already been applied, hello configuration is shown in hello definition view.</span></span>

![Konfigurálás][policies-configure]

<span data-ttu-id="6ee25-127">hello házirend írásvédett jelenik meg először.</span><span class="sxs-lookup"><span data-stu-id="6ee25-127">hello policy is displayed read-only at first.</span></span> <span data-ttu-id="6ee25-128">A sorrend tooedit hello definition kattintson hello **házirend konfigurálása** művelet.</span><span class="sxs-lookup"><span data-stu-id="6ee25-128">In order tooedit hello definition click hello **Configure Policy** action.</span></span>

![Szerkesztés][policies-edit]

<span data-ttu-id="6ee25-130">hello házirend-definíció egy egyszerű XML-dokumentumban, amely leírja a bejövő és kimenő utasítás sorozata.</span><span class="sxs-lookup"><span data-stu-id="6ee25-130">hello policy definition is a simple XML document that describes a sequence of inbound and outbound statements.</span></span> <span data-ttu-id="6ee25-131">hello XML közvetlenül hello definition ablak szerkeszthetők.</span><span class="sxs-lookup"><span data-stu-id="6ee25-131">hello XML can be edited directly in hello definition window.</span></span> <span data-ttu-id="6ee25-132">Utasítások a listáját, feltéve toohello jobb utasítások alkalmazható toohello aktuális hatókör engedélyezett és a kiemelt; Amint azt a hello **korlát hívás arány** hello a képernyőfelvételen látható a fenti utasítást.</span><span class="sxs-lookup"><span data-stu-id="6ee25-132">A list of statements is provided toohello right and statements applicable toohello current scope are enabled and highlighted; as demonstrated by hello **Limit Call Rate** statement in hello screenshot above.</span></span>

<span data-ttu-id="6ee25-133">Kattintson egy engedélyezett utasítás felveszi hello megfelelő XML hello helyen hello kurzor hello definition nézetben.</span><span class="sxs-lookup"><span data-stu-id="6ee25-133">Clicking an enabled statement will add hello appropriate XML at hello location of hello cursor in hello definition view.</span></span> 

> [!NOTE]
> <span data-ttu-id="6ee25-134">Ha hello házirend, amelyet az tooadd nincs engedélyezve, ellenőrizze, hogy hello a házirendhez tartozó megfelelő hatókörben.</span><span class="sxs-lookup"><span data-stu-id="6ee25-134">If hello policy that you want tooadd is not enabled, ensure that you are in hello correct scope for that policy.</span></span> <span data-ttu-id="6ee25-135">Minden egyes házirend-utasítás az egyes hatókörök és házirend szakaszok használatra szolgál.</span><span class="sxs-lookup"><span data-stu-id="6ee25-135">Each policy statement is designed for use in certain scopes and policy sections.</span></span> <span data-ttu-id="6ee25-136">tooreview hello házirend szakaszok és egy házirend hatókörök ellenőrizze hello **használati** hello a házirendhez tartozó szakasz [szabályzataihoz][Policy Reference].</span><span class="sxs-lookup"><span data-stu-id="6ee25-136">tooreview hello policy sections and scopes for a policy, check hello **Usage** section for that policy in hello [Policy Reference][Policy Reference].</span></span>
> 
> 

<span data-ttu-id="6ee25-137">A házirend-utasításoknál teljes listáját és a beállítások érhetők el a hello [szabályzataihoz][Policy Reference].</span><span class="sxs-lookup"><span data-stu-id="6ee25-137">A full list of policy statements and their settings are available in hello [Policy Reference][Policy Reference].</span></span>

<span data-ttu-id="6ee25-138">Például tooadd egy új utasítás toorestrict bejövő kérelmek toospecified IP-címek, hello kurzorral hello hello tartalmának belülre `inbound` XML-elem, és kattintson hello **IP-címek korlátozása hívó** utasítást.</span><span class="sxs-lookup"><span data-stu-id="6ee25-138">For example, tooadd a new statement toorestrict incoming requests toospecified IP addresses, place hello cursor just inside hello content of hello `inbound` XML element and click hello **Restrict caller IPs** statement.</span></span>

![A szoftverkorlátozó házirendek][policies-restrict]

<span data-ttu-id="6ee25-140">Ez hozzáadja egy XML-részlet toohello `inbound` elem, hogyan tooconfigure hello utasítás nyújt útmutatást.</span><span class="sxs-lookup"><span data-stu-id="6ee25-140">This will add an XML snippet toohello `inbound` element that provides guidance on how tooconfigure hello statement.</span></span>

```xml
<ip-filter action="allow | forbid">
    <address>address</address>
    <address-range from="address" to="address"/>
</ip-filter>
```

<span data-ttu-id="6ee25-141">toolimit a bejövő kéréseket, és fogadja el az alábbiak szerint módosítsa 1.2.3.4 IP-cím csak az hello XML:</span><span class="sxs-lookup"><span data-stu-id="6ee25-141">toolimit inbound requests and accept only those from an IP address of 1.2.3.4 modify hello XML as follows:</span></span>

```xml
<ip-filter action="allow">
    <address>1.2.3.4</address>
</ip-filter>
```

![Mentés][policies-save]

<span data-ttu-id="6ee25-143">Amikor végzett hello utasítások hello házirend konfigurálásával, kattintson **mentése** és hello módosítások azonnal lesz propagált toohello API adatkezelési átjáró.</span><span class="sxs-lookup"><span data-stu-id="6ee25-143">When complete configuring hello statements for hello policy, click **Save** and hello changes will be propagated toohello API Management gateway immediately.</span></span>

## <span data-ttu-id="6ee25-144"><a name="sections"></a>Ismertetése házirend konfigurálása</span><span class="sxs-lookup"><span data-stu-id="6ee25-144"><a name="sections"> </a>Understanding policy configuration</span></span>
<span data-ttu-id="6ee25-145">A házirend utasításokat, amelyek ahhoz, hogy egy kérés- és a végrehajtása több.</span><span class="sxs-lookup"><span data-stu-id="6ee25-145">A policy is a series of statements that execute in order for a request and a response.</span></span> <span data-ttu-id="6ee25-146">hello konfigurációs oszlik megfelelően `inbound`, `backend`, `outbound`, és `on-error` látható módon hello a következő konfigurációs szakasz.</span><span class="sxs-lookup"><span data-stu-id="6ee25-146">hello configuration is divided appropriately into `inbound`, `backend`, `outbound`, and `on-error` sections as shown in hello following configuration.</span></span>

```xml
<policies>
  <inbound>
    <!-- statements toobe applied toohello request go here -->
  </inbound>
  <backend>
    <!-- statements toobe applied before hello request is forwarded too
         hello backend service go here -->
  </backend>
  <outbound>
    <!-- statements toobe applied toohello response go here -->
  </outbound>
  <on-error>
    <!-- statements toobe applied if there is an error condition go here -->
  </on-error>
</policies> 
```

<span data-ttu-id="6ee25-147">Hello egy kérelem feldolgozása során hiba történik, ha a többi szükséges lépések hello `inbound`, `backend`, vagy `outbound` szakaszok kimarad, és végrehajtási áttérés toohello utasítások a hello `on-error` szakasz.</span><span class="sxs-lookup"><span data-stu-id="6ee25-147">If there is an error during hello processing of a request, any remaining steps in hello `inbound`, `backend`, or `outbound` sections are skipped and execution jumps toohello statements in hello `on-error` section.</span></span> <span data-ttu-id="6ee25-148">Házirend-utasításoknál hello úgy `on-error` hello segítségével áttekintheti a hello hiba szakasz `context.LastError` tulajdonság, nézze meg, és testre szabhatja a hello hibaválaszba hello használata `set-body` házirend, és konfigurálása, mi történik, ha a hiba akkor fordul elő.</span><span class="sxs-lookup"><span data-stu-id="6ee25-148">By placing policy statements in hello `on-error` section you can review hello error by using hello `context.LastError` property, inspect and customize hello error response using hello `set-body` policy, and configure what happens if an error occurs.</span></span> <span data-ttu-id="6ee25-149">Nincsenek hibakódok beépített lépéseket és a házirend-utasításoknál hello feldolgozása során előforduló hibákat.</span><span class="sxs-lookup"><span data-stu-id="6ee25-149">There are error codes for built-in steps and for errors that may occur during hello processing of policy statements.</span></span> <span data-ttu-id="6ee25-150">További információkért lásd: [hiba történt az API-felügyeleti házirendek kezelése](https://msdn.microsoft.com/library/azure/mt629506.aspx).</span><span class="sxs-lookup"><span data-stu-id="6ee25-150">For more information, see [Error handling in API Management policies](https://msdn.microsoft.com/library/azure/mt629506.aspx).</span></span>

<span data-ttu-id="6ee25-151">Mivel a házirendek (globális, termék, api és művelet) különböző szinteken adható meg hello konfigurációs lehetőséget biztosít az Ön toospecify hello sorrendben, amelyben hello házirend-definíció utasításokat illetően toohello szülő irányelvnek hajtható végre.</span><span class="sxs-lookup"><span data-stu-id="6ee25-151">Since policies can be specified at different levels (global, product, api and operation) hello configuration provides a way for you toospecify hello order in which hello policy definition's statements execute with respect toohello parent policy.</span></span> 

<span data-ttu-id="6ee25-152">Házirend hatókörök sorrend hello kiértékelése.</span><span class="sxs-lookup"><span data-stu-id="6ee25-152">Policy scopes are evaluated in hello following order.</span></span>

1. <span data-ttu-id="6ee25-153">Globális hatókörű</span><span class="sxs-lookup"><span data-stu-id="6ee25-153">Global scope</span></span>
2. <span data-ttu-id="6ee25-154">A termék hatókör</span><span class="sxs-lookup"><span data-stu-id="6ee25-154">Product scope</span></span>
3. <span data-ttu-id="6ee25-155">API-hatókör</span><span class="sxs-lookup"><span data-stu-id="6ee25-155">API scope</span></span>
4. <span data-ttu-id="6ee25-156">Műveleti hatókör</span><span class="sxs-lookup"><span data-stu-id="6ee25-156">Operation scope</span></span>

<span data-ttu-id="6ee25-157">hello rajtuk utasítás kiértékelése hello toohello elhelyezését szerint `base` elem, ha telepítve.</span><span class="sxs-lookup"><span data-stu-id="6ee25-157">hello statements within them are evaluated according toohello placement of hello `base` element, if it is present.</span></span> <span data-ttu-id="6ee25-158">Globális-házirendben engedélyezve van, nincs szülő házirend és hello segítségével `<base>` az elem nincs hatása.</span><span class="sxs-lookup"><span data-stu-id="6ee25-158">Global policy has no parent policy and using hello `<base>` element in it has no effect.</span></span>

<span data-ttu-id="6ee25-159">Például ha egy házirend hello globális szinten és az API-k konfigurált házirendek, majd, hogy adott API-t igénybe mindkét házirendeket a rendszer alkalmazza.</span><span class="sxs-lookup"><span data-stu-id="6ee25-159">For example, if you have a policy at hello global level and a policy configured for an API, then whenever that particular API is used both policies will be applied.</span></span> <span data-ttu-id="6ee25-160">API-kezelés lehetővé teszi a kombinált házirend kimutatások keresztül hello a(z) determinisztikus rendezéshez.</span><span class="sxs-lookup"><span data-stu-id="6ee25-160">API Management allows for deterministic ordering of combined policy statements via hello base element.</span></span> 

```xml
<policies>
    <inbound>
        <cross-domain />
        <base />
        <find-and-replace from="xyz" to="abc" />
    </inbound>
</policies>
```

<span data-ttu-id="6ee25-161">A fenti hello példa házirend definíció, hello `cross-domain` utasítás futtatása előtt minden magasabb házirendet, amely viszont hello követi `find-and-replace` házirend.</span><span class="sxs-lookup"><span data-stu-id="6ee25-161">In hello example policy definition above, hello `cross-domain` statement would execute before any higher policies which would in turn, be followed by hello `find-and-replace` policy.</span></span> 

<span data-ttu-id="6ee25-162">toosee hello házirendek a jelenlegi hatókörben hello hello csoportházirend-szerkesztőben kattintson **számítsa ki újra a kijelölt hatókör hatékony házirend**.</span><span class="sxs-lookup"><span data-stu-id="6ee25-162">toosee hello policies in hello current scope in hello policy editor, click **Recalculate effective policy for selected scope**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6ee25-163">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6ee25-163">Next steps</span></span>
<span data-ttu-id="6ee25-164">Tekintse meg a következő videót a házirend-kifejezések.</span><span class="sxs-lookup"><span data-stu-id="6ee25-164">Check out following video on policy expressions.</span></span>

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
