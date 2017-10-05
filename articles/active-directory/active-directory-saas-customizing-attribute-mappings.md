---
title: "Az Azure AD-attribútum-leképezésekhez testreszabása |} Microsoft Docs"
description: "Ismerje meg, milyen attribútum-leképezésekhez SaaS-alkalmazásokhoz az Azure Active Directoryban, hogyan módosíthatja azokat az üzleti igények kielégítéséhez."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 549e0b8c-87ce-4c9b-b487-b7bf0155dc77
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: markvi
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 6ca2fdc9c68ea0030d938eeaebd57aafa0e2790f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="customizing-user-provisioning-attribute-mappings-for-saas-applications-in-azure-active-directory"></a><span data-ttu-id="c0964-103">Attribútum-leképezésekhez kiépítés az SaaS-alkalmazásokhoz az Azure Active Directory felhasználói testreszabása</span><span class="sxs-lookup"><span data-stu-id="c0964-103">Customizing User Provisioning Attribute Mappings for SaaS Applications in Azure Active Directory</span></span>
<span data-ttu-id="c0964-104">Microsoft Azure AD támogatást nyújt a felhasználók átadása, például a Salesforce, a Google Apps és a többi külső SaaS-alkalmazásokban.</span><span class="sxs-lookup"><span data-stu-id="c0964-104">Microsoft Azure AD provides support for user provisioning to third-party SaaS applications such as Salesforce, Google Apps and others.</span></span> <span data-ttu-id="c0964-105">Ha egy külső SaaS-alkalmazáshoz engedélyezett kiépítés felhasználó, az Azure felügyeleti portálon szabályozza az űrlap konfiguráció "címtárattribútum-leképezésben." nevű attribútum értékei</span><span class="sxs-lookup"><span data-stu-id="c0964-105">If you have user provisioning for a third-party SaaS application enabled, the Azure Management Portal controls its attribute values in form of a configuration called “attribute mapping.”</span></span>

<span data-ttu-id="c0964-106">Nincs olyan előre konfigurált attribútum-leképezésekhez az Azure AD felhasználói és minden SaaS-alkalmazás felhasználói objektumok között.</span><span class="sxs-lookup"><span data-stu-id="c0964-106">There is a preconfigured set of attribute mappings between Azure AD user objects and each SaaS app’s user objects.</span></span> <span data-ttu-id="c0964-107">Egyes alkalmazások más típusú objektumok, például a csoportok vagy névjegyek kezelése.</span><span class="sxs-lookup"><span data-stu-id="c0964-107">Some apps manage other types of objects, such as Groups or Contacts.</span></span> <br> 
 <span data-ttu-id="c0964-108">Az alapértelmezett attribútum-leképezésekhez igényeinek megfelelően testre.</span><span class="sxs-lookup"><span data-stu-id="c0964-108">You can customize the default attribute mappings according to your business needs.</span></span> <span data-ttu-id="c0964-109">Ez azt jelenti, módosítsa vagy törölje a meglévő attribútum-leképezésekhez vagy hozzon létre új attribútum-leképezésekhez.</span><span class="sxs-lookup"><span data-stu-id="c0964-109">This means, you can change or delete existing attribute mappings or create new attribute mappings.</span></span>

<span data-ttu-id="c0964-110">Az Azure AD portálon érhető el ez a szolgáltatás kattint egy **hozzárendelések** konfigurációja **kiépítési** a a **kezelése** szakasza egy  **A vállalati alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="c0964-110">In the Azure AD portal, you can access this feature by clicking a **Mappings** configuration under **Provisioning** in the **Manage** section of an **Enterprise application**.</span></span>


![Salesforce][5] 

<span data-ttu-id="c0964-112">Kattintson egy **hozzárendelések** konfigurációs, megnyílik a kapcsolódó **attribútum leképezési** panelen.</span><span class="sxs-lookup"><span data-stu-id="c0964-112">Clicking a **Mappings** configuration, opens the related **Attribute Mapping** blade.</span></span>  
<span data-ttu-id="c0964-113">Nincsenek attribútum-leképezésekhez egy SaaS-alkalmazáshoz megfelelő működéséhez szükséges.</span><span class="sxs-lookup"><span data-stu-id="c0964-113">There are attribute mappings that are required by a SaaS application to function correctly.</span></span> <span data-ttu-id="c0964-114">A szükséges attribútumokat a **törlése** szolgáltatás nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="c0964-114">For required attributes, the **Delete** feature is unavailable.</span></span>


![Salesforce][6]  

<span data-ttu-id="c0964-116">A fenti példában láthatja, hogy a **felhasználónév** attribútum egy felügyelt objektum a Salesforce-ban a telepítéskor a **userPrincipalName** érték a csatolt Azure Active Directory objektum.</span><span class="sxs-lookup"><span data-stu-id="c0964-116">In the example above, you can see that the **Username** attribute of a managed object in Salesforce is populated with the **userPrincipalName** value of the linked Azure Active Directory Object.</span></span>

<span data-ttu-id="c0964-117">Testre szabhat meglévő **attribútum-leképezésekhez** leképezéseket kattintva.</span><span class="sxs-lookup"><span data-stu-id="c0964-117">You can customize existing **Attribute Mappings** by clicking a mapping.</span></span> <span data-ttu-id="c0964-118">Ekkor megnyílik a **attribútum szerkesztése** panelen.</span><span class="sxs-lookup"><span data-stu-id="c0964-118">This opens the **Edit Attribute** blade.</span></span>

![Salesforce][7]  


  

## <a name="understanding-attribute-mapping-types"></a><span data-ttu-id="c0964-120">Attribútum-leképezés típusainak ismertetése</span><span class="sxs-lookup"><span data-stu-id="c0964-120">Understanding attribute mapping types</span></span>
<span data-ttu-id="c0964-121">Az attribútum-leképezésekhez meghatározhatja, hogyan attribútumok megjelenik a külső SaaS-alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="c0964-121">With attribute mappings, you control how attributes are populated in a third-party SaaS application.</span></span> <span data-ttu-id="c0964-122">Számos négy különböző hozzárendelési típusokhoz támogatott.</span><span class="sxs-lookup"><span data-stu-id="c0964-122">There are four different mapping types supported:</span></span>

* <span data-ttu-id="c0964-123">**Közvetlen** – a target attribútummal fel van töltve a csatolt objektum attribútum értékét az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="c0964-123">**Direct** – the target attribute is populated with the value of an attribute of the linked object in Azure AD.</span></span>
* <span data-ttu-id="c0964-124">**Állandó** – a target attribútum a telepítéskor megadott egy adott karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="c0964-124">**Constant** – the target attribute is populated with a specific string you have specified.</span></span>
* <span data-ttu-id="c0964-125">**Kifejezés** -a target attribútum egy parancsfájl-szerű kifejezés eredménye alapján van feltöltve.</span><span class="sxs-lookup"><span data-stu-id="c0964-125">**Expression** - the target attribute is populated based on the result of a script-like expression.</span></span> 
  <span data-ttu-id="c0964-126">További információkért lásd: [írás kifejezések az Azure Active Directoryban attribútum-leképezésekhez](active-directory-saas-writing-expressions-for-attribute-mappings.md).</span><span class="sxs-lookup"><span data-stu-id="c0964-126">For more information, see [Writing Expressions for Attribute Mappings in Azure Active Directory](active-directory-saas-writing-expressions-for-attribute-mappings.md).</span></span>
* <span data-ttu-id="c0964-127">**Nincs** -marad a cél attribútumának változatlan.</span><span class="sxs-lookup"><span data-stu-id="c0964-127">**None** - the target attribute is left unmodified.</span></span> <span data-ttu-id="c0964-128">Azonban a célattribútum valaha is üres, ha a telepítéskor, amely az alapértelmezett érték.</span><span class="sxs-lookup"><span data-stu-id="c0964-128">However, if the target attribute is ever empty, it is populated with the Default value that you specify.</span></span>

<span data-ttu-id="c0964-129">Ezek négy alapvető attribútum hozzárendelési típusokhoz mellett egyéni attribútum-leképezésekhez támogatja egy nem kötelező fogalma **alapértelmezett** érték-hozzárendelés.</span><span class="sxs-lookup"><span data-stu-id="c0964-129">In addition to these four basic attribute mapping types, custom attribute mappings support the concept of an optional **default** value assignment.</span></span> <span data-ttu-id="c0964-130">Az alapértelmezett érték-hozzárendelés biztosítja, hogy a target attribútummal megadni értéket, ha nincs sem értéket, és a célobjektum, nem az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="c0964-130">The default value assignment ensures that a target attribute is populated with a value if there is neither a value in Azure AD nor on the target object.</span></span> <span data-ttu-id="c0964-131">A leggyakrabban használt beállítások mellett akkor hagyja üresen a mezőt.</span><span class="sxs-lookup"><span data-stu-id="c0964-131">The most common configuration is to leave this blank.</span></span>


## <a name="understanding-attribute-mapping-properties"></a><span data-ttu-id="c0964-132">Leképezési attribútumtulajdonságok ismertetése</span><span class="sxs-lookup"><span data-stu-id="c0964-132">Understanding attribute mapping properties</span></span>

<span data-ttu-id="c0964-133">Az előző szakaszban akkor már vezettek attribútum leképezés típusa tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="c0964-133">In the previous section, you have already been introduced to the attribute mapping type property.</span></span>
<span data-ttu-id="c0964-134">Mellett ez a tulajdonság a attribútum-leképezésekhez is támogatja a következő attribútumokat:</span><span class="sxs-lookup"><span data-stu-id="c0964-134">In addition to this property, attribute mappings do also support the following attributes:</span></span>

- <span data-ttu-id="c0964-135">**Adatforrás-attribútum** -a felhasználói attribútum a forrásrendszerből (pl.: Azure Active Directory).</span><span class="sxs-lookup"><span data-stu-id="c0964-135">**Source attribute** - The user attribute from the source system (e.g.: Azure Active Directory).</span></span>
- <span data-ttu-id="c0964-136">**Cél attribútumának** – a felhasználói attribútum a célrendszeren (pl.: a ServiceNow).</span><span class="sxs-lookup"><span data-stu-id="c0964-136">**Target attribute** – The user attribute in the target system (e.g.: ServiceNow).</span></span>
- <span data-ttu-id="c0964-137">**Ezzel az attribútummal objektumok megfelelő** – függetlenül attól, ez a leképezés kell használni a forrás és cél közötti felhasználók egyedi azonosítására.</span><span class="sxs-lookup"><span data-stu-id="c0964-137">**Match objects using this attribute** – Whether or not this mapping should be used to uniquely identify users between the source and target systems.</span></span> <span data-ttu-id="c0964-138">Ez általában be van állítva a userPrincipalName vagy mail attribútum az Azure AD, amely általában egy felhasználónév mező a célalkalmazásban van hozzárendelve.</span><span class="sxs-lookup"><span data-stu-id="c0964-138">This is typically set on the userPrincipalName or mail attribute in Azure AD, which is typically mapped to a username field in a target application.</span></span>
- <span data-ttu-id="c0964-139">**Megfelelő sorrend** – több egyező attribútumok állítható be.</span><span class="sxs-lookup"><span data-stu-id="c0964-139">**Matching precedence** – Multiple matching attributes can be set.</span></span> <span data-ttu-id="c0964-140">Ha több, kiértékelésük, ez a mező által megadott sorrendben.</span><span class="sxs-lookup"><span data-stu-id="c0964-140">When there are multiple, they are evaluated in the order defined by this field.</span></span> <span data-ttu-id="c0964-141">Amint a program egyezést talál, további megfelelő attribútumok kiértékelése.</span><span class="sxs-lookup"><span data-stu-id="c0964-141">As soon as a match is found, no further matching attributes are evaluated.</span></span>
- <span data-ttu-id="c0964-142">**Ez a leképezés alkalmazása**</span><span class="sxs-lookup"><span data-stu-id="c0964-142">**Apply this mapping**</span></span>
    - <span data-ttu-id="c0964-143">**Mindig** – Ez a leképezés alkalmazni mind a felhasználó létrehozása és a műveletek frissítése</span><span class="sxs-lookup"><span data-stu-id="c0964-143">**Always** – Apply this mapping on both user creation and update actions</span></span>
    - <span data-ttu-id="c0964-144">**Csak létrehozásakor** – Ez a leképezés csak a felhasználó létrehozási műveletek alkalmazása</span><span class="sxs-lookup"><span data-stu-id="c0964-144">**Only during creation** - Apply this mapping only on user creation actions</span></span>


## <a name="what-you-should-know"></a><span data-ttu-id="c0964-145">Tudnivalók</span><span class="sxs-lookup"><span data-stu-id="c0964-145">What you should know</span></span>

<span data-ttu-id="c0964-146">Microsoft Azure AD egy hatékony megvalósítása a szinkronizálási folyamat nyújt.</span><span class="sxs-lookup"><span data-stu-id="c0964-146">Microsoft Azure AD provides an efficient implementation of a synchronization process.</span></span> <span data-ttu-id="c0964-147">Inicializált környezetben csak a frissítést igénylő objektumok feldolgozása során szinkronizálási ciklust.</span><span class="sxs-lookup"><span data-stu-id="c0964-147">In an initialized environment, only objects requiring updates are processed during a synchronization cycle.</span></span> <span data-ttu-id="c0964-148">Attribútum-leképezésekhez frissítése szinkronizálási ciklust teljesítményére hatással van.</span><span class="sxs-lookup"><span data-stu-id="c0964-148">Updating attribute mappings has an impact on the performance of a synchronization cycle.</span></span> <span data-ttu-id="c0964-149">Az leképezési konfigurációjának frissítése valamennyi felügyelt objektum reevaluated kell igényel.</span><span class="sxs-lookup"><span data-stu-id="c0964-149">An update to the attribute mapping configuration requires all managed objects to be reevaluated.</span></span> <span data-ttu-id="c0964-150">Javasoljuk, hogy az attribútum-leképezésekhez minimális egymást követő módosítások számát is.</span><span class="sxs-lookup"><span data-stu-id="c0964-150">It is a recommended best practice to keep the number of consecutive changes to your attribute mappings at a minimum.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c0964-151">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c0964-151">Next steps</span></span>

* [<span data-ttu-id="c0964-152">Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke</span><span class="sxs-lookup"><span data-stu-id="c0964-152">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
* [<span data-ttu-id="c0964-153">Felhasználói létesítési vagy megszüntetési SaaS-alkalmazásokhoz való automatizálásához</span><span class="sxs-lookup"><span data-stu-id="c0964-153">Automate User Provisioning/Deprovisioning to SaaS Apps</span></span>](active-directory-saas-app-provisioning.md)
* [<span data-ttu-id="c0964-154">Attribútum-leképezésekhez kifejezések írása</span><span class="sxs-lookup"><span data-stu-id="c0964-154">Writing Expressions for Attribute Mappings</span></span>](active-directory-saas-writing-expressions-for-attribute-mappings.md)
* [<span data-ttu-id="c0964-155">Helyezése Hatókörszűrőkkel felhasználói történő üzembe helyezéséhez</span><span class="sxs-lookup"><span data-stu-id="c0964-155">Scoping Filters for User Provisioning</span></span>](active-directory-saas-scoping-filters.md)
* [<span data-ttu-id="c0964-156">SCIM használata a felhasználók és csoportok automatikus üzembe helyezésének engedélyezéséhez az Azure Active Directoryból az alkalmazásokba</span><span class="sxs-lookup"><span data-stu-id="c0964-156">Using SCIM to enable automatic provisioning of users and groups from Azure Active Directory to applications</span></span>](active-directory-scim-provisioning.md)
* [<span data-ttu-id="c0964-157">Alkalmazás-kiépítési értesítések</span><span class="sxs-lookup"><span data-stu-id="c0964-157">Account Provisioning Notifications</span></span>](active-directory-saas-account-provisioning-notifications.md)
* [<span data-ttu-id="c0964-158">SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="c0964-158">List of Tutorials on How to Integrate SaaS Apps</span></span>](active-directory-saas-tutorial-list.md)

<!--Image references-->
[1]: ./media/active-directory-saas-customizing-attribute-mappings/ic765497.png
[2]: ./media/active-directory-saas-customizing-attribute-mappings/ic775419.png
[3]: ./media/active-directory-saas-customizing-attribute-mappings/ic775420.png
[4]: ./media/active-directory-saas-customizing-attribute-mappings/ic775421.png
[5]: ./media/active-directory-saas-customizing-attribute-mappings/21.png
[6]: ./media/active-directory-saas-customizing-attribute-mappings/22.png
[7]: ./media/active-directory-saas-customizing-attribute-mappings/23.png

