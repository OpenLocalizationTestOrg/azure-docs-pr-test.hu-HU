---
title: "az Azure API-felügyeleti házirendek aaaHow toouse tulajdonságai"
description: "Megtudhatja, hogyan toouse tulajdonságokat az Azure API-felügyeleti házirendek."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 6f39b00f-cf6e-4cef-9bf2-1f89202c0bc0
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 1ff096deeb97543b48dcf1f40be9dbfcbcd09542
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-properties-in-azure-api-management-policies"></a><span data-ttu-id="b0a30-103">Hogyan toouse tulajdonságokat az Azure API-felügyeleti házirendek</span><span class="sxs-lookup"><span data-stu-id="b0a30-103">How toouse properties in Azure API Management policies</span></span>
<span data-ttu-id="b0a30-104">API-felügyeleti házirendek, amelyek lehetővé teszik a hello publisher toochange hello viselkedését hello API konfigurálással hello rendszer hatékony képesség.</span><span class="sxs-lookup"><span data-stu-id="b0a30-104">API Management policies are a powerful capability of hello system that allow hello publisher toochange hello behavior of hello API through configuration.</span></span> <span data-ttu-id="b0a30-105">Házirendek olyan hello kérésre egymás után végrehajtott utasítások gyűjteménye vagy egy API-t adott válaszokat.</span><span class="sxs-lookup"><span data-stu-id="b0a30-105">Policies are a collection of statements that are executed sequentially on hello request or response of an API.</span></span> <span data-ttu-id="b0a30-106">Házirend-utasításoknál szövegkonstans értékek, a házirend-kifejezések és a tulajdonságok használatával lehet létrehozni.</span><span class="sxs-lookup"><span data-stu-id="b0a30-106">Policy statements can be constructed using literal text values, policy expressions, and properties.</span></span> 

<span data-ttu-id="b0a30-107">Minden API Management szolgáltatáspéldány kulcs/érték párok, amelyek globális toohello szolgáltatáspéldány tulajdonságok gyűjteményével rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="b0a30-107">Each API Management service instance has a properties collection of key/value pairs that are global toohello service instance.</span></span> <span data-ttu-id="b0a30-108">Ezek a tulajdonságok összes API konfigurálása és házirendek lehet használt toomanage állandó karakterlánc-értékek.</span><span class="sxs-lookup"><span data-stu-id="b0a30-108">These properties can be used toomanage constant string values across all API configuration and policies.</span></span> <span data-ttu-id="b0a30-109">Minden egyes tulajdonsága a következő attribútumok hello rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="b0a30-109">Each property has hello following attributes.</span></span>

| <span data-ttu-id="b0a30-110">Attribútum</span><span class="sxs-lookup"><span data-stu-id="b0a30-110">Attribute</span></span> | <span data-ttu-id="b0a30-111">Típus</span><span class="sxs-lookup"><span data-stu-id="b0a30-111">Type</span></span> | <span data-ttu-id="b0a30-112">Leírás</span><span class="sxs-lookup"><span data-stu-id="b0a30-112">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b0a30-113">Név</span><span class="sxs-lookup"><span data-stu-id="b0a30-113">Name</span></span> |<span data-ttu-id="b0a30-114">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="b0a30-114">string</span></span> |<span data-ttu-id="b0a30-115">hello tulajdonság hello neve.</span><span class="sxs-lookup"><span data-stu-id="b0a30-115">hello name of hello property.</span></span> <span data-ttu-id="b0a30-116">Ez előfordulhat, hogy csak betűket, számokat, időszak, kötőjelet tartalmazhat, és aláhúzás karaktereket tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="b0a30-116">It may contain only letters, digits, period, dash, and underscore characters.</span></span> |
| <span data-ttu-id="b0a30-117">Érték</span><span class="sxs-lookup"><span data-stu-id="b0a30-117">Value</span></span> |<span data-ttu-id="b0a30-118">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="b0a30-118">string</span></span> |<span data-ttu-id="b0a30-119">hello hello tulajdonság értéke.</span><span class="sxs-lookup"><span data-stu-id="b0a30-119">hello value of hello property.</span></span> <span data-ttu-id="b0a30-120">Nem lehet üres vagy csak szóközök állhatnak.</span><span class="sxs-lookup"><span data-stu-id="b0a30-120">It may not be empty or consist only of whitespace.</span></span> |
| <span data-ttu-id="b0a30-121">Titkos</span><span class="sxs-lookup"><span data-stu-id="b0a30-121">Secret</span></span> |<span data-ttu-id="b0a30-122">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="b0a30-122">boolean</span></span> |<span data-ttu-id="b0a30-123">Meghatározza, hogy hello érték titkos kulcs-e, és hogy titkosítani kell-e.</span><span class="sxs-lookup"><span data-stu-id="b0a30-123">Determines whether hello value is a secret and should be encrypted or not.</span></span> |
| <span data-ttu-id="b0a30-124">Címkék</span><span class="sxs-lookup"><span data-stu-id="b0a30-124">Tags</span></span> |<span data-ttu-id="b0a30-125">A karakterlánc tömbje</span><span class="sxs-lookup"><span data-stu-id="b0a30-125">array of string</span></span> |<span data-ttu-id="b0a30-126">Nem kötelező, hogy címkéket, amikor megadja a használt toofilter hello keresésitulajdonság-lista lehet.</span><span class="sxs-lookup"><span data-stu-id="b0a30-126">Optional tags that when provided can be used toofilter hello property list.</span></span> |

<span data-ttu-id="b0a30-127">Van beállítva a hello hello publisher portálról **tulajdonságok** fülre. A következő példa hello három tulajdonságainak vannak konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="b0a30-127">Properties are configured in hello publisher portal on hello **Properties** tab. In hello following example, three properties are configured.</span></span>

![Tulajdonságok][api-management-properties]

<span data-ttu-id="b0a30-129">A tulajdonság értékek tartalmazhatnak szövegkonstansok és [házirend-kifejezések](https://msdn.microsoft.com/library/azure/dn910913.aspx).</span><span class="sxs-lookup"><span data-stu-id="b0a30-129">Property values can contain literal strings and [policy expressions](https://msdn.microsoft.com/library/azure/dn910913.aspx).</span></span> <span data-ttu-id="b0a30-130">hello következő táblázatban hello előző három minta tulajdonságok és attribútumaik.</span><span class="sxs-lookup"><span data-stu-id="b0a30-130">hello following table shows hello previous three sample properties and their attributes.</span></span> <span data-ttu-id="b0a30-131">hello értékének `ExpressionProperty` a házirend-kifejezést, amely visszaadja a karakterláncot, amely tartalmazza az aktuális dátum és idő hello.</span><span class="sxs-lookup"><span data-stu-id="b0a30-131">hello value of `ExpressionProperty` is a policy expression that returns a string containing hello current date and time.</span></span> <span data-ttu-id="b0a30-132">hello tulajdonság `ContosoHeaderValue` titkos kulcs, van megjelölve, ezért az értéke nem jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="b0a30-132">hello property `ContosoHeaderValue` is marked as a secret, so its value is not displayed.</span></span>

| <span data-ttu-id="b0a30-133">Név</span><span class="sxs-lookup"><span data-stu-id="b0a30-133">Name</span></span> | <span data-ttu-id="b0a30-134">Érték</span><span class="sxs-lookup"><span data-stu-id="b0a30-134">Value</span></span> | <span data-ttu-id="b0a30-135">Titkos</span><span class="sxs-lookup"><span data-stu-id="b0a30-135">Secret</span></span> | <span data-ttu-id="b0a30-136">Címkék</span><span class="sxs-lookup"><span data-stu-id="b0a30-136">Tags</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="b0a30-137">ContosoHeader</span><span class="sxs-lookup"><span data-stu-id="b0a30-137">ContosoHeader</span></span> |<span data-ttu-id="b0a30-138">trackingId</span><span class="sxs-lookup"><span data-stu-id="b0a30-138">TrackingId</span></span> |<span data-ttu-id="b0a30-139">False (Hamis)</span><span class="sxs-lookup"><span data-stu-id="b0a30-139">False</span></span> |<span data-ttu-id="b0a30-140">Contoso</span><span class="sxs-lookup"><span data-stu-id="b0a30-140">Contoso</span></span> |
| <span data-ttu-id="b0a30-141">ContosoHeaderValue</span><span class="sxs-lookup"><span data-stu-id="b0a30-141">ContosoHeaderValue</span></span> |<span data-ttu-id="b0a30-142">••••••••••••••••••••••</span><span class="sxs-lookup"><span data-stu-id="b0a30-142">••••••••••••••••••••••</span></span> |<span data-ttu-id="b0a30-143">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="b0a30-143">True</span></span> |<span data-ttu-id="b0a30-144">Contoso</span><span class="sxs-lookup"><span data-stu-id="b0a30-144">Contoso</span></span> |
| <span data-ttu-id="b0a30-145">ExpressionProperty</span><span class="sxs-lookup"><span data-stu-id="b0a30-145">ExpressionProperty</span></span> |<span data-ttu-id="b0a30-146">@(DateTime.Now.ToString())</span><span class="sxs-lookup"><span data-stu-id="b0a30-146">@(DateTime.Now.ToString())</span></span> |<span data-ttu-id="b0a30-147">False (Hamis)</span><span class="sxs-lookup"><span data-stu-id="b0a30-147">False</span></span> | |

## <a name="toouse-a-property"></a><span data-ttu-id="b0a30-148">toouse tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b0a30-148">toouse a property</span></span>
<span data-ttu-id="b0a30-149">toouse házirend tulajdonság, hely hello tulajdonságnév belül dupla párban kell használni, például `{{ContosoHeader}}`, ahogy az alábbi példa hello.</span><span class="sxs-lookup"><span data-stu-id="b0a30-149">toouse a property in a policy, place hello property name inside a double pair of braces like `{{ContosoHeader}}`, as shown in hello following example.</span></span>

```xml
<set-header name="{{ContosoHeader}}" exists-action="override">
  <value>{{ContosoHeaderValue}}</value>
</set-header>
```

<span data-ttu-id="b0a30-150">Ebben a példában `ContosoHeader` a fejléc neveként hello szolgál egy `set-header` házirend, és `ContosoHeaderValue` , hogy a fejléc értékeként hello szolgál.</span><span class="sxs-lookup"><span data-stu-id="b0a30-150">In this example, `ContosoHeader` is used as hello name of a header in a `set-header` policy, and `ContosoHeaderValue` is used as hello value of that header.</span></span> <span data-ttu-id="b0a30-151">Ezzel a házirend-kérés vagy válasz toohello API Management az átjáró során kiértékelésekor `{{ContosoHeader}}` és `{{ContosoHeaderValue}}` váltják fel az adott tulajdonságot értékekre.</span><span class="sxs-lookup"><span data-stu-id="b0a30-151">When this policy is evaluated during a request or response toohello API Management gateway, `{{ContosoHeader}}` and `{{ContosoHeaderValue}}` are replaced with their respective property values.</span></span>

<span data-ttu-id="b0a30-152">Tulajdonságok teljes attribútumot vagy elemet értékek hello előző példában látható módon használható, de is lehet beszúrt vagy együtt egy szövegkonstans kifejezés része, ahogy az alábbi példa hello:`<set-header name = "CustomHeader{{ContosoHeader}}" ...>`</span><span class="sxs-lookup"><span data-stu-id="b0a30-152">Properties can be used as complete attribute or element values as shown in hello previous example, but they can also be inserted into or combined with part of a literal text expression as shown in hello following example: `<set-header name = "CustomHeader{{ContosoHeader}}" ...>`</span></span>

<span data-ttu-id="b0a30-153">Tulajdonságok házirend kifejezést is tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="b0a30-153">Properties can also contain policy expressions.</span></span> <span data-ttu-id="b0a30-154">A következő példa hello, hello `ExpressionProperty` szolgál.</span><span class="sxs-lookup"><span data-stu-id="b0a30-154">In hello following example, hello `ExpressionProperty` is used.</span></span>

```xml
<set-header name="CustomHeader" exists-action="override">
    <value>{{ExpressionProperty}}</value>
</set-header>
```

<span data-ttu-id="b0a30-155">Ez a házirend kiértékeléséhez `{{ExpressionProperty}}` értéke helyére: `@(DateTime.Now.ToString())`.</span><span class="sxs-lookup"><span data-stu-id="b0a30-155">When this policy is evaluated, `{{ExpressionProperty}}` is replaced with its value: `@(DateTime.Now.ToString())`.</span></span> <span data-ttu-id="b0a30-156">Hello értéke egy házirend-kifejezést, mert hello van kiértékeli, és a végrehajtása a hello házirendet, majd.</span><span class="sxs-lookup"><span data-stu-id="b0a30-156">Since hello value is a policy expression, hello expression is evaluated and hello policy proceeds with its execution.</span></span>

<span data-ttu-id="b0a30-157">Tesztelheti a kimenő hello developer portálon egy műveletet, amelynek hatókörében szerepel egy házirend tulajdonságokkal meghívásával.</span><span class="sxs-lookup"><span data-stu-id="b0a30-157">You can test this out in hello developer portal by calling an operation that has a policy with properties in scope.</span></span> <span data-ttu-id="b0a30-158">A következő példa hello, egy művelet hívása hello két előző példa `set-header` házirendek tulajdonságokkal.</span><span class="sxs-lookup"><span data-stu-id="b0a30-158">In hello following example, an operation is called with hello two previous example `set-header` policies with properties.</span></span> <span data-ttu-id="b0a30-159">Vegye figyelembe, hogy a hello válasz házirendekkel tulajdonságokkal konfigurált, két egyéni fejléc tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="b0a30-159">Note that hello response contains two custom headers that were configured using policies with properties.</span></span>

![Fejlesztői portál][api-management-send-results]

<span data-ttu-id="b0a30-161">Ha megnézzük hello [API Inspector nyomkövetési](api-management-howto-api-inspector.md) hello két korábbi minta szabályzatok tulajdonságait tartalmazó hívásra, lásd: a két hello `set-header` beszúrni, valamint hello házirend-kifejezést hello tulajdonságértékek szabályzatok hello tulajdonság hello házirend-kifejezést tartalmazó kiértékelését.</span><span class="sxs-lookup"><span data-stu-id="b0a30-161">If you look at hello [API Inspector trace](api-management-howto-api-inspector.md) for a call that includes hello two previous sample policies with properties, you can see hello two `set-header` policies with hello property values inserted as well as hello policy expression evaluation for hello property that contained hello policy expression.</span></span>

![API-Inspector nyomkövetési][api-management-api-inspector-trace]

<span data-ttu-id="b0a30-163">Vegye figyelembe, hogy közben a tulajdonságértékek házirend-kifejezést tartalmazhat, tulajdonságértékek nem tartalmazhat más tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="b0a30-163">Note that while property values can contain policy expressions, property values can't contain other properties.</span></span> <span data-ttu-id="b0a30-164">Ha a szöveg tulajdonság a hivatkozást használja a tulajdonság értéke, például a `Property value text {{MyProperty}}`, hogy tulajdonsághivatkozás nem lehet írni, és tartalmazzák a hello tulajdonság értéke lesz.</span><span class="sxs-lookup"><span data-stu-id="b0a30-164">If text containing a property reference is used for a property value, such as `Property value text {{MyProperty}}`, that property reference won't be replaced and will be included as part of hello property value.</span></span>

## <a name="toocreate-a-property"></a><span data-ttu-id="b0a30-165">toocreate tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b0a30-165">toocreate a property</span></span>
<span data-ttu-id="b0a30-166">toocreate tulajdonság, kattintson a **tulajdonság hozzáadása** a hello **tulajdonságok** fülre.</span><span class="sxs-lookup"><span data-stu-id="b0a30-166">toocreate a property, click **Add property** on hello **Properties** tab.</span></span>

![Tulajdonság hozzáadása][api-management-properties-add-property-menu]

<span data-ttu-id="b0a30-168">**Név** és **érték** szükséges érték.</span><span class="sxs-lookup"><span data-stu-id="b0a30-168">**Name** and **Value** are required values.</span></span> <span data-ttu-id="b0a30-169">Ha ez a tulajdonság értéke egy titkos kulcsot, ellenőrizze a hello **Ez a titkos kulcs** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="b0a30-169">If this property value is a secret, check hello **This is a secret** checkbox.</span></span> <span data-ttu-id="b0a30-170">Adjon meg egy vagy több választható címkék toohelp való rendszerezését a tulajdonságokat, majd kattintson **mentése**.</span><span class="sxs-lookup"><span data-stu-id="b0a30-170">Enter one or more optional tags toohelp with organizing your properties, and click **Save**.</span></span>

![Tulajdonság hozzáadása][api-management-properties-add-property]

<span data-ttu-id="b0a30-172">Új tulajdonság mentésekor a rendszer hello **tulajdonság keresése** szövegmező hello hello új tulajdonság neve a telepítéskor és hello új tulajdonság jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="b0a30-172">When a new property is saved, hello **Search property** textbox is populated with hello name of hello new property and hello new property is displayed.</span></span> <span data-ttu-id="b0a30-173">toodisplay összes tulajdonság, törölje a jelet hello **tulajdonság keresése** szövegmező, és nyomja le az adja meg.</span><span class="sxs-lookup"><span data-stu-id="b0a30-173">toodisplay all properties, clear hello **Search property** textbox and press enter.</span></span>

![Tulajdonságok][api-management-properties-property-saved]

<span data-ttu-id="b0a30-175">Hello REST API használatával tulajdonság létrehozásával kapcsolatos további információkért lásd: [hello REST API használatával tulajdonság létrehozása](https://msdn.microsoft.com/library/azure/mt651775.aspx#Put).</span><span class="sxs-lookup"><span data-stu-id="b0a30-175">For information on creating a property using hello REST API, see [Create a property using hello REST API](https://msdn.microsoft.com/library/azure/mt651775.aspx#Put).</span></span>

## <a name="tooedit-a-property"></a><span data-ttu-id="b0a30-176">tooedit tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b0a30-176">tooedit a property</span></span>
<span data-ttu-id="b0a30-177">egy tulajdonság tooedit kattintson **szerkesztése** hello tulajdonság tooedit mellett.</span><span class="sxs-lookup"><span data-stu-id="b0a30-177">tooedit a property, click **Edit** beside hello property tooedit.</span></span>

![Tulajdonság szerkesztése][api-management-properties-edit]

<span data-ttu-id="b0a30-179">Végezze el a szükséges módosításokat, és kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="b0a30-179">Make any desired changes, and click **Save**.</span></span> <span data-ttu-id="b0a30-180">Hello tulajdonságnév módosításakor a házirendekben, amelyek az adott tulajdonsághoz hivatkoznak automatikusan frissített toouse hello új nevet.</span><span class="sxs-lookup"><span data-stu-id="b0a30-180">If you change hello property name, any policies that reference that property are automatically updated toouse hello new name.</span></span>

![Tulajdonság szerkesztése][api-management-properties-edit-property]

<span data-ttu-id="b0a30-182">Hello REST API használatával tulajdonság szerkesztési információkért lásd: [hello REST API használatával tulajdonság módosítása](https://msdn.microsoft.com/library/azure/mt651775.aspx#Patch).</span><span class="sxs-lookup"><span data-stu-id="b0a30-182">For information on editing a property using hello REST API, see [Edit a property using hello REST API](https://msdn.microsoft.com/library/azure/mt651775.aspx#Patch).</span></span>

## <a name="toodelete-a-property"></a><span data-ttu-id="b0a30-183">toodelete tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b0a30-183">toodelete a property</span></span>
<span data-ttu-id="b0a30-184">egy tulajdonság toodelete kattintson **törlése** hello tulajdonság toodelete mellett.</span><span class="sxs-lookup"><span data-stu-id="b0a30-184">toodelete a property, click **Delete** beside hello property toodelete.</span></span>

![Tulajdonság törlése][api-management-properties-delete]

<span data-ttu-id="b0a30-186">Kattintson a **Igen, törölje azt** tooconfirm.</span><span class="sxs-lookup"><span data-stu-id="b0a30-186">Click **Yes, delete it** tooconfirm.</span></span>

![Törlés megerősítése][api-management-delete-confirm]

> [!IMPORTANT]
> <span data-ttu-id="b0a30-188">Ha hello tulajdonság házirendekben hivatkozik, akkor nem lehet toosuccessfully törlése amíg hello tulajdonság távolít el, az azt használó összes házirendet.</span><span class="sxs-lookup"><span data-stu-id="b0a30-188">If hello property is referenced by any policies, you will be unable toosuccessfully delete it until you remove hello property from all policies that use it.</span></span>
> 
> 

<span data-ttu-id="b0a30-189">Hello REST API használatával tulajdonság törléséről további információkért lásd: [törölni egy tulajdonságot hello REST API használatával](https://msdn.microsoft.com/library/azure/mt651775.aspx#Delete).</span><span class="sxs-lookup"><span data-stu-id="b0a30-189">For information on deleting a property using hello REST API, see [Delete a property using hello REST API](https://msdn.microsoft.com/library/azure/mt651775.aspx#Delete).</span></span>

## <a name="toosearch-and-filter-properties"></a><span data-ttu-id="b0a30-190">toosearch és szűrő tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="b0a30-190">toosearch and filter properties</span></span>
<span data-ttu-id="b0a30-191">Hello **tulajdonságok** lapján található, a Keresés és szűrés képességek toohelp kezelheti a tulajdonságokat.</span><span class="sxs-lookup"><span data-stu-id="b0a30-191">hello **Properties** tab includes searching and filtering capabilities toohelp you manage your properties.</span></span> <span data-ttu-id="b0a30-192">toofilter hello keresésitulajdonság-lista tulajdonság nevét, adja meg a kívánt keresőkifejezést hello **tulajdonság keresése** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="b0a30-192">toofilter hello property list by property name, enter a search term in hello **Search property** textbox.</span></span> <span data-ttu-id="b0a30-193">toodisplay összes tulajdonság, törölje a jelet hello **tulajdonság keresése** szövegmező, és nyomja le az adja meg.</span><span class="sxs-lookup"><span data-stu-id="b0a30-193">toodisplay all properties, clear hello **Search property** textbox and press enter.</span></span>

![Keresés][api-management-properties-search]

<span data-ttu-id="b0a30-195">toofilter hello keresésitulajdonság-lista által előfizetéscímkék értékeit, adjon meg egy vagy több címke hello **címkék szűrés** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="b0a30-195">toofilter hello property list by tag values, enter one or more tags into hello **Filter by tags** textbox.</span></span> <span data-ttu-id="b0a30-196">toodisplay összes tulajdonság, törölje a jelet hello **címkék szűrés** szövegmező, és nyomja le az adja meg.</span><span class="sxs-lookup"><span data-stu-id="b0a30-196">toodisplay all properties, clear hello **Filter by tags** textbox and press enter.</span></span>

![Szűrés][api-management-properties-filter]

## <a name="next-steps"></a><span data-ttu-id="b0a30-198">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b0a30-198">Next steps</span></span>
* <span data-ttu-id="b0a30-199">További információ a házirendek használata</span><span class="sxs-lookup"><span data-stu-id="b0a30-199">Learn more about working with policies</span></span>
  * [<span data-ttu-id="b0a30-200">Az API Management házirendek</span><span class="sxs-lookup"><span data-stu-id="b0a30-200">Policies in API Management</span></span>](api-management-howto-policies.md)
  * [<span data-ttu-id="b0a30-201">Házirend-referencia</span><span class="sxs-lookup"><span data-stu-id="b0a30-201">Policy reference</span></span>](https://msdn.microsoft.com/library/azure/dn894081.aspx)
  * [<span data-ttu-id="b0a30-202">Házirend-kifejezések</span><span class="sxs-lookup"><span data-stu-id="b0a30-202">Policy expressions</span></span>](https://msdn.microsoft.com/library/azure/dn910913.aspx)

## <a name="watch-a-video-overview"></a><span data-ttu-id="b0a30-203">Áttekintő videó megtekintése</span><span class="sxs-lookup"><span data-stu-id="b0a30-203">Watch a video overview</span></span>
> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Use-Properties-in-Policies/player]
> 
> 

[api-management-properties]: ./media/api-management-howto-properties/api-management-properties.png
[api-management-properties-add-property]: ./media/api-management-howto-properties/api-management-properties-add-property.png
[api-management-properties-edit-property]: ./media/api-management-howto-properties/api-management-properties-edit-property.png
[api-management-properties-add-property-menu]: ./media/api-management-howto-properties/api-management-properties-add-property-menu.png
[api-management-properties-property-saved]: ./media/api-management-howto-properties/api-management-properties-property-saved.png
[api-management-properties-delete]: ./media/api-management-howto-properties/api-management-properties-delete.png
[api-management-properties-edit]: ./media/api-management-howto-properties/api-management-properties-edit.png
[api-management-delete-confirm]: ./media/api-management-howto-properties/api-management-delete-confirm.png
[api-management-properties-search]: ./media/api-management-howto-properties/api-management-properties-search.png
[api-management-send-results]: ./media/api-management-howto-properties/api-management-send-results.png
[api-management-properties-filter]: ./media/api-management-howto-properties/api-management-properties-filter.png
[api-management-api-inspector-trace]: ./media/api-management-howto-properties/api-management-api-inspector-trace.png

