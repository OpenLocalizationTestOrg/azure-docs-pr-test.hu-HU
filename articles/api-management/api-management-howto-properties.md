---
title: "Tulajdonságok használata az Azure API-felügyeleti házirendek"
description: "További tulajdonságok használata az Azure API-felügyeleti házirendek."
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
ms.openlocfilehash: 3b0fe2a300038e13cc488bdb4f50f8be270ea8f4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-properties-in-azure-api-management-policies"></a><span data-ttu-id="98b74-103">Tulajdonságok használata az Azure API-felügyeleti házirendek</span><span class="sxs-lookup"><span data-stu-id="98b74-103">How to use properties in Azure API Management policies</span></span>
<span data-ttu-id="98b74-104">API-felügyeleti házirendek, amelyek lehetővé teszik a közzétevőt úgy, hogy az API-t konfigurálással működésének módosításához a rendszer hatékony képesség.</span><span class="sxs-lookup"><span data-stu-id="98b74-104">API Management policies are a powerful capability of the system that allow the publisher to change the behavior of the API through configuration.</span></span> <span data-ttu-id="98b74-105">A házirendek utasítások gyűjteményei, amelyeket az API-k kérelmei és válaszai szerint egymást követően hajtanak végre.</span><span class="sxs-lookup"><span data-stu-id="98b74-105">Policies are a collection of statements that are executed sequentially on the request or response of an API.</span></span> <span data-ttu-id="98b74-106">Házirend-utasításoknál szövegkonstans értékek, a házirend-kifejezések és a tulajdonságok használatával lehet létrehozni.</span><span class="sxs-lookup"><span data-stu-id="98b74-106">Policy statements can be constructed using literal text values, policy expressions, and properties.</span></span> 

<span data-ttu-id="98b74-107">Minden API Management service-példány kulcs/érték párok, a service-példány általános tulajdonságok gyűjteményével rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="98b74-107">Each API Management service instance has a properties collection of key/value pairs that are global to the service instance.</span></span> <span data-ttu-id="98b74-108">Ezek a Tulajdonságok állandó karakterlánc-értékek összes API konfigurálása és házirendek kezelésére használható.</span><span class="sxs-lookup"><span data-stu-id="98b74-108">These properties can be used to manage constant string values across all API configuration and policies.</span></span> <span data-ttu-id="98b74-109">Minden egyes tulajdonsága a következő attribútumokkal rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="98b74-109">Each property has the following attributes.</span></span>

| <span data-ttu-id="98b74-110">Attribútum</span><span class="sxs-lookup"><span data-stu-id="98b74-110">Attribute</span></span> | <span data-ttu-id="98b74-111">Típus</span><span class="sxs-lookup"><span data-stu-id="98b74-111">Type</span></span> | <span data-ttu-id="98b74-112">Leírás</span><span class="sxs-lookup"><span data-stu-id="98b74-112">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="98b74-113">Név</span><span class="sxs-lookup"><span data-stu-id="98b74-113">Name</span></span> |<span data-ttu-id="98b74-114">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="98b74-114">string</span></span> |<span data-ttu-id="98b74-115">A tulajdonság nevét.</span><span class="sxs-lookup"><span data-stu-id="98b74-115">The name of the property.</span></span> <span data-ttu-id="98b74-116">Ez előfordulhat, hogy csak betűket, számokat, időszak, kötőjelet tartalmazhat, és aláhúzás karaktereket tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="98b74-116">It may contain only letters, digits, period, dash, and underscore characters.</span></span> |
| <span data-ttu-id="98b74-117">Érték</span><span class="sxs-lookup"><span data-stu-id="98b74-117">Value</span></span> |<span data-ttu-id="98b74-118">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="98b74-118">string</span></span> |<span data-ttu-id="98b74-119">A tulajdonság értéke.</span><span class="sxs-lookup"><span data-stu-id="98b74-119">The value of the property.</span></span> <span data-ttu-id="98b74-120">Nem lehet üres vagy csak szóközök állhatnak.</span><span class="sxs-lookup"><span data-stu-id="98b74-120">It may not be empty or consist only of whitespace.</span></span> |
| <span data-ttu-id="98b74-121">Titkos</span><span class="sxs-lookup"><span data-stu-id="98b74-121">Secret</span></span> |<span data-ttu-id="98b74-122">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="98b74-122">boolean</span></span> |<span data-ttu-id="98b74-123">Határozza meg, hogy az érték egy titkos kulcsot, és titkosítani kell, vagy nem.</span><span class="sxs-lookup"><span data-stu-id="98b74-123">Determines whether the value is a secret and should be encrypted or not.</span></span> |
| <span data-ttu-id="98b74-124">Címkék</span><span class="sxs-lookup"><span data-stu-id="98b74-124">Tags</span></span> |<span data-ttu-id="98b74-125">A karakterlánc tömbje</span><span class="sxs-lookup"><span data-stu-id="98b74-125">array of string</span></span> |<span data-ttu-id="98b74-126">Nem kötelező, hogy címkéket, amikor megadja a tulajdonságlista szűréséhez használható.</span><span class="sxs-lookup"><span data-stu-id="98b74-126">Optional tags that when provided can be used to filter the property list.</span></span> |

<span data-ttu-id="98b74-127">Tulajdonságok konfigurálása a közzétevő portálon a a **tulajdonságok** fülre.</span><span class="sxs-lookup"><span data-stu-id="98b74-127">Properties are configured in the publisher portal on the **Properties** tab.</span></span> <span data-ttu-id="98b74-128">A következő példában három tulajdonságainak vannak konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="98b74-128">In the following example, three properties are configured.</span></span>

![Tulajdonságok][api-management-properties]

<span data-ttu-id="98b74-130">A tulajdonság értékek tartalmazhatnak szövegkonstansok és [házirend-kifejezések](https://msdn.microsoft.com/library/azure/dn910913.aspx).</span><span class="sxs-lookup"><span data-stu-id="98b74-130">Property values can contain literal strings and [policy expressions](https://msdn.microsoft.com/library/azure/dn910913.aspx).</span></span> <span data-ttu-id="98b74-131">A következő táblázat az előző három minta tulajdonságok és attribútumaik.</span><span class="sxs-lookup"><span data-stu-id="98b74-131">The following table shows the previous three sample properties and their attributes.</span></span> <span data-ttu-id="98b74-132">Értékének `ExpressionProperty` egy házirend-kifejezés, amely az aktuális dátum és idő tartalmazó karakterláncot ad vissza.</span><span class="sxs-lookup"><span data-stu-id="98b74-132">The value of `ExpressionProperty` is a policy expression that returns a string containing the current date and time.</span></span> <span data-ttu-id="98b74-133">A tulajdonság `ContosoHeaderValue` titkos kulcs, van megjelölve, ezért az értéke nem jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="98b74-133">The property `ContosoHeaderValue` is marked as a secret, so its value is not displayed.</span></span>

| <span data-ttu-id="98b74-134">Név</span><span class="sxs-lookup"><span data-stu-id="98b74-134">Name</span></span> | <span data-ttu-id="98b74-135">Érték</span><span class="sxs-lookup"><span data-stu-id="98b74-135">Value</span></span> | <span data-ttu-id="98b74-136">Titkos</span><span class="sxs-lookup"><span data-stu-id="98b74-136">Secret</span></span> | <span data-ttu-id="98b74-137">Címkék</span><span class="sxs-lookup"><span data-stu-id="98b74-137">Tags</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="98b74-138">ContosoHeader</span><span class="sxs-lookup"><span data-stu-id="98b74-138">ContosoHeader</span></span> |<span data-ttu-id="98b74-139">trackingId</span><span class="sxs-lookup"><span data-stu-id="98b74-139">TrackingId</span></span> |<span data-ttu-id="98b74-140">False (Hamis)</span><span class="sxs-lookup"><span data-stu-id="98b74-140">False</span></span> |<span data-ttu-id="98b74-141">Contoso</span><span class="sxs-lookup"><span data-stu-id="98b74-141">Contoso</span></span> |
| <span data-ttu-id="98b74-142">ContosoHeaderValue</span><span class="sxs-lookup"><span data-stu-id="98b74-142">ContosoHeaderValue</span></span> |<span data-ttu-id="98b74-143">••••••••••••••••••••••</span><span class="sxs-lookup"><span data-stu-id="98b74-143">••••••••••••••••••••••</span></span> |<span data-ttu-id="98b74-144">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="98b74-144">True</span></span> |<span data-ttu-id="98b74-145">Contoso</span><span class="sxs-lookup"><span data-stu-id="98b74-145">Contoso</span></span> |
| <span data-ttu-id="98b74-146">ExpressionProperty</span><span class="sxs-lookup"><span data-stu-id="98b74-146">ExpressionProperty</span></span> |<span data-ttu-id="98b74-147">@(DateTime.Now.ToString())</span><span class="sxs-lookup"><span data-stu-id="98b74-147">@(DateTime.Now.ToString())</span></span> |<span data-ttu-id="98b74-148">False (Hamis)</span><span class="sxs-lookup"><span data-stu-id="98b74-148">False</span></span> | |

## <a name="to-use-a-property"></a><span data-ttu-id="98b74-149">A tulajdonság</span><span class="sxs-lookup"><span data-stu-id="98b74-149">To use a property</span></span>
<span data-ttu-id="98b74-150">Egy házirend tulajdonságot használja, helyezze a belül kell használni, például a kettős két tulajdonságnév `{{ContosoHeader}}`, a következő példában látható módon.</span><span class="sxs-lookup"><span data-stu-id="98b74-150">To use a property in a policy, place the property name inside a double pair of braces like `{{ContosoHeader}}`, as shown in the following example.</span></span>

```xml
<set-header name="{{ContosoHeader}}" exists-action="override">
  <value>{{ContosoHeaderValue}}</value>
</set-header>
```

<span data-ttu-id="98b74-151">Ebben a példában `ContosoHeader` a fejléc neveként használatos a `set-header` házirend, és `ContosoHeaderValue` szolgál, hogy a fejléc értékeként.</span><span class="sxs-lookup"><span data-stu-id="98b74-151">In this example, `ContosoHeader` is used as the name of a header in a `set-header` policy, and `ContosoHeaderValue` is used as the value of that header.</span></span> <span data-ttu-id="98b74-152">Ez a házirend olyan kérésre vagy válaszra API Management Gateway során kiértékelésekor `{{ContosoHeader}}` és `{{ContosoHeaderValue}}` váltják fel az adott tulajdonságot értékekre.</span><span class="sxs-lookup"><span data-stu-id="98b74-152">When this policy is evaluated during a request or response to the API Management gateway, `{{ContosoHeader}}` and `{{ContosoHeaderValue}}` are replaced with their respective property values.</span></span>

<span data-ttu-id="98b74-153">Tulajdonságok teljes attribútumot vagy elemet értékeket az előző példában látható módon használható, de is lehet beszúrt vagy kombinálva, a következő példában látható módon egy szövegkonstans kifejezés része:`<set-header name = "CustomHeader{{ContosoHeader}}" ...>`</span><span class="sxs-lookup"><span data-stu-id="98b74-153">Properties can be used as complete attribute or element values as shown in the previous example, but they can also be inserted into or combined with part of a literal text expression as shown in the following example: `<set-header name = "CustomHeader{{ContosoHeader}}" ...>`</span></span>

<span data-ttu-id="98b74-154">Tulajdonságok házirend kifejezést is tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="98b74-154">Properties can also contain policy expressions.</span></span> <span data-ttu-id="98b74-155">A következő példában a `ExpressionProperty` szolgál.</span><span class="sxs-lookup"><span data-stu-id="98b74-155">In the following example, the `ExpressionProperty` is used.</span></span>

```xml
<set-header name="CustomHeader" exists-action="override">
    <value>{{ExpressionProperty}}</value>
</set-header>
```

<span data-ttu-id="98b74-156">Ez a házirend kiértékeléséhez `{{ExpressionProperty}}` értéke helyére: `@(DateTime.Now.ToString())`.</span><span class="sxs-lookup"><span data-stu-id="98b74-156">When this policy is evaluated, `{{ExpressionProperty}}` is replaced with its value: `@(DateTime.Now.ToString())`.</span></span> <span data-ttu-id="98b74-157">Óta egy házirend-kifejezés értéke, a rendszer kiértékeli, és a végrehajtása a házirendet, majd.</span><span class="sxs-lookup"><span data-stu-id="98b74-157">Since the value is a policy expression, the expression is evaluated and the policy proceeds with its execution.</span></span>

<span data-ttu-id="98b74-158">A fejlesztői portálra a kimenő tesztelheti egy műveletet, amelynek hatókörében szerepel egy házirend tulajdonságokkal meghívásával.</span><span class="sxs-lookup"><span data-stu-id="98b74-158">You can test this out in the developer portal by calling an operation that has a policy with properties in scope.</span></span> <span data-ttu-id="98b74-159">A következő példában egy művelet hívása a két előző példa `set-header` házirendek tulajdonságokkal.</span><span class="sxs-lookup"><span data-stu-id="98b74-159">In the following example, an operation is called with the two previous example `set-header` policies with properties.</span></span> <span data-ttu-id="98b74-160">Vegye figyelembe, hogy a válasz tartalmazza-e a házirendekkel tulajdonságokkal konfigurált, két egyéni fejléc.</span><span class="sxs-lookup"><span data-stu-id="98b74-160">Note that the response contains two custom headers that were configured using policies with properties.</span></span>

![Fejlesztői portál][api-management-send-results]

<span data-ttu-id="98b74-162">Ha megnézi a [API Inspector nyomkövetési](api-management-howto-api-inspector.md) hívás, amely tartalmazza a két előző minta házirendek tulajdonságokkal rendelkező, lásd: a két `set-header` házirendeket és a tulajdonságértékek beszúrni, valamint a házirend-kifejezés kiértékelése a tulajdonsághoz, amely a házirend-kifejezést tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="98b74-162">If you look at the [API Inspector trace](api-management-howto-api-inspector.md) for a call that includes the two previous sample policies with properties, you can see the two `set-header` policies with the property values inserted as well as the policy expression evaluation for the property that contained the policy expression.</span></span>

![API-Inspector nyomkövetési][api-management-api-inspector-trace]

<span data-ttu-id="98b74-164">Vegye figyelembe, hogy közben a tulajdonságértékek házirend-kifejezést tartalmazhat, tulajdonságértékek nem tartalmazhat más tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="98b74-164">Note that while property values can contain policy expressions, property values can't contain other properties.</span></span> <span data-ttu-id="98b74-165">Ha a szöveg tulajdonság a hivatkozást használja a tulajdonság értéke, például a `Property value text {{MyProperty}}`, hivatkozó tulajdonság nem lehet írni, és tartalmazzák a tulajdonság értékének a lesz.</span><span class="sxs-lookup"><span data-stu-id="98b74-165">If text containing a property reference is used for a property value, such as `Property value text {{MyProperty}}`, that property reference won't be replaced and will be included as part of the property value.</span></span>

## <a name="to-create-a-property"></a><span data-ttu-id="98b74-166">A tulajdonság létrehozása</span><span class="sxs-lookup"><span data-stu-id="98b74-166">To create a property</span></span>
<span data-ttu-id="98b74-167">Hozzon létre egy tulajdonságot, kattintson a **tulajdonság hozzáadása** a a **tulajdonságok** fülre.</span><span class="sxs-lookup"><span data-stu-id="98b74-167">To create a property, click **Add property** on the **Properties** tab.</span></span>

![Tulajdonság hozzáadása][api-management-properties-add-property-menu]

<span data-ttu-id="98b74-169">**Név** és **érték** szükséges érték.</span><span class="sxs-lookup"><span data-stu-id="98b74-169">**Name** and **Value** are required values.</span></span> <span data-ttu-id="98b74-170">Ha ez a tulajdonság értéke egy titkos kulcsot, ellenőrizze a **Ez a titkos kulcs** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="98b74-170">If this property value is a secret, check the **This is a secret** checkbox.</span></span> <span data-ttu-id="98b74-171">Adja meg egy vagy több választható címkéket a Tulajdonságok rendszerezéséhez számára, és kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="98b74-171">Enter one or more optional tags to help with organizing your properties, and click **Save**.</span></span>

![Tulajdonság hozzáadása][api-management-properties-add-property]

<span data-ttu-id="98b74-173">Új tulajdonság mentésekor a rendszer a **tulajdonság keresése** szövegmező a telepítéskor az új tulajdonság nevét, és az új tulajdonság jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="98b74-173">When a new property is saved, the **Search property** textbox is populated with the name of the new property and the new property is displayed.</span></span> <span data-ttu-id="98b74-174">Megjeleníti az összes tulajdonság, vagy törölje a **tulajdonság keresése** szövegmező, és nyomja le az adja meg.</span><span class="sxs-lookup"><span data-stu-id="98b74-174">To display all properties, clear the **Search property** textbox and press enter.</span></span>

![Tulajdonságok][api-management-properties-property-saved]

<span data-ttu-id="98b74-176">A REST API használatával tulajdonság létrehozásáról további információért lásd: [hozzon létre egy tulajdonságot a REST API használatával](https://msdn.microsoft.com/library/azure/mt651775.aspx#Put).</span><span class="sxs-lookup"><span data-stu-id="98b74-176">For information on creating a property using the REST API, see [Create a property using the REST API](https://msdn.microsoft.com/library/azure/mt651775.aspx#Put).</span></span>

## <a name="to-edit-a-property"></a><span data-ttu-id="98b74-177">Tulajdonság módosítása</span><span class="sxs-lookup"><span data-stu-id="98b74-177">To edit a property</span></span>
<span data-ttu-id="98b74-178">A tulajdonság szerkesztéséhez kattintson **szerkesztése** szerkesztése tulajdonság mellett.</span><span class="sxs-lookup"><span data-stu-id="98b74-178">To edit a property, click **Edit** beside the property to edit.</span></span>

![Tulajdonság szerkesztése][api-management-properties-edit]

<span data-ttu-id="98b74-180">Végezze el a szükséges módosításokat, és kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="98b74-180">Make any desired changes, and click **Save**.</span></span> <span data-ttu-id="98b74-181">Ha módosítja a tulajdonság nevét, a házirendek, amelyek az adott tulajdonsághoz hivatkoznak automatikusan frissülnek az új nevét.</span><span class="sxs-lookup"><span data-stu-id="98b74-181">If you change the property name, any policies that reference that property are automatically updated to use the new name.</span></span>

![Tulajdonság szerkesztése][api-management-properties-edit-property]

<span data-ttu-id="98b74-183">A REST API használatával tulajdonság szerkesztési információkért lásd: [REST API használatával tulajdonság módosítása](https://msdn.microsoft.com/library/azure/mt651775.aspx#Patch).</span><span class="sxs-lookup"><span data-stu-id="98b74-183">For information on editing a property using the REST API, see [Edit a property using the REST API](https://msdn.microsoft.com/library/azure/mt651775.aspx#Patch).</span></span>

## <a name="to-delete-a-property"></a><span data-ttu-id="98b74-184">Egy tulajdonság</span><span class="sxs-lookup"><span data-stu-id="98b74-184">To delete a property</span></span>
<span data-ttu-id="98b74-185">Törölni egy tulajdonságot, kattintson a **törlése** mellett a tulajdonság törlése.</span><span class="sxs-lookup"><span data-stu-id="98b74-185">To delete a property, click **Delete** beside the property to delete.</span></span>

![Tulajdonság törlése][api-management-properties-delete]

<span data-ttu-id="98b74-187">Kattintson a **Igen, törölje azt** megerősítéséhez.</span><span class="sxs-lookup"><span data-stu-id="98b74-187">Click **Yes, delete it** to confirm.</span></span>

![Törlés megerősítése][api-management-delete-confirm]

> [!IMPORTANT]
> <span data-ttu-id="98b74-189">Ha a tulajdonság a házirendek által hivatkozott, nem fog tudni sikeresen törölni mindaddig, amíg az azt használó összes házirendektől távolítsa el a tulajdonságot.</span><span class="sxs-lookup"><span data-stu-id="98b74-189">If the property is referenced by any policies, you will be unable to successfully delete it until you remove the property from all policies that use it.</span></span>
> 
> 

<span data-ttu-id="98b74-190">A REST API használatával tulajdonság törléséről további információkért lásd: [törölni egy tulajdonságot a REST API használatával](https://msdn.microsoft.com/library/azure/mt651775.aspx#Delete).</span><span class="sxs-lookup"><span data-stu-id="98b74-190">For information on deleting a property using the REST API, see [Delete a property using the REST API](https://msdn.microsoft.com/library/azure/mt651775.aspx#Delete).</span></span>

## <a name="to-search-and-filter-properties"></a><span data-ttu-id="98b74-191">A Keresés és szűrés tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="98b74-191">To search and filter properties</span></span>
<span data-ttu-id="98b74-192">A **tulajdonságok** lapján található, a Keresés és szűrés lehetőségek közül választhat a tulajdonságainak kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="98b74-192">The **Properties** tab includes searching and filtering capabilities to help you manage your properties.</span></span> <span data-ttu-id="98b74-193">Tulajdonság neve a tulajdonságlista szűréséhez adja meg a kívánt keresőkifejezést a a **tulajdonság keresése** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="98b74-193">To filter the property list by property name, enter a search term in the **Search property** textbox.</span></span> <span data-ttu-id="98b74-194">Megjeleníti az összes tulajdonság, vagy törölje a **tulajdonság keresése** szövegmező, és nyomja le az adja meg.</span><span class="sxs-lookup"><span data-stu-id="98b74-194">To display all properties, clear the **Search property** textbox and press enter.</span></span>

![Keresés][api-management-properties-search]

<span data-ttu-id="98b74-196">Címke értékek szerint a tulajdonságlista szűréséhez adja meg egy vagy több címkéket, az a **címkék szűrés** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="98b74-196">To filter the property list by tag values, enter one or more tags into the **Filter by tags** textbox.</span></span> <span data-ttu-id="98b74-197">Megjeleníti az összes tulajdonság, vagy törölje a **címkék szűrés** szövegmező, és nyomja le az adja meg.</span><span class="sxs-lookup"><span data-stu-id="98b74-197">To display all properties, clear the **Filter by tags** textbox and press enter.</span></span>

![Szűrés][api-management-properties-filter]

## <a name="next-steps"></a><span data-ttu-id="98b74-199">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="98b74-199">Next steps</span></span>
* <span data-ttu-id="98b74-200">További információ a házirendek használata</span><span class="sxs-lookup"><span data-stu-id="98b74-200">Learn more about working with policies</span></span>
  * [<span data-ttu-id="98b74-201">Az API Management házirendek</span><span class="sxs-lookup"><span data-stu-id="98b74-201">Policies in API Management</span></span>](api-management-howto-policies.md)
  * [<span data-ttu-id="98b74-202">Házirend-referencia</span><span class="sxs-lookup"><span data-stu-id="98b74-202">Policy reference</span></span>](https://msdn.microsoft.com/library/azure/dn894081.aspx)
  * [<span data-ttu-id="98b74-203">Házirend-kifejezések</span><span class="sxs-lookup"><span data-stu-id="98b74-203">Policy expressions</span></span>](https://msdn.microsoft.com/library/azure/dn910913.aspx)

## <a name="watch-a-video-overview"></a><span data-ttu-id="98b74-204">Áttekintő videó megtekintése</span><span class="sxs-lookup"><span data-stu-id="98b74-204">Watch a video overview</span></span>
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

