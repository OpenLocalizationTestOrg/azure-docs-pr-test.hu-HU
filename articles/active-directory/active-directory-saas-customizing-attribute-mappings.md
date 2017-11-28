---
title: "Azure AD attribútum-leképezésekhez aaaCustomizing |} Microsoft Docs"
description: "Ismerje meg, milyen attribútum-leképezésekhez SaaS-alkalmazásokhoz az Azure Active Directoryban, hogyan módosíthatja is tooaddress vállalatának szüksége van."
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
ms.openlocfilehash: 14db5303f06fc8df3b07a0a8b75713312e71bbfd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="customizing-user-provisioning-attribute-mappings-for-saas-applications-in-azure-active-directory"></a><span data-ttu-id="23611-103">Attribútum-leképezésekhez kiépítés az SaaS-alkalmazásokhoz az Azure Active Directory felhasználói testreszabása</span><span class="sxs-lookup"><span data-stu-id="23611-103">Customizing User Provisioning Attribute Mappings for SaaS Applications in Azure Active Directory</span></span>
<span data-ttu-id="23611-104">Microsoft Azure AD a felhasználók átadásához toothird féltől SaaS-alkalmazások például Salesforce, a Google Apps és a mások támogatást nyújt.</span><span class="sxs-lookup"><span data-stu-id="23611-104">Microsoft Azure AD provides support for user provisioning toothird-party SaaS applications such as Salesforce, Google Apps and others.</span></span> <span data-ttu-id="23611-105">Ha egy külső SaaS-alkalmazáshoz engedélyezett kiépítés felhasználói, hello Azure felügyeleti portálon szabályozza az űrlap konfiguráció "címtárattribútum-leképezésben." nevű attribútum értékei</span><span class="sxs-lookup"><span data-stu-id="23611-105">If you have user provisioning for a third-party SaaS application enabled, hello Azure Management Portal controls its attribute values in form of a configuration called “attribute mapping.”</span></span>

<span data-ttu-id="23611-106">Nincs olyan előre konfigurált attribútum-leképezésekhez az Azure AD felhasználói és minden SaaS-alkalmazás felhasználói objektumok között.</span><span class="sxs-lookup"><span data-stu-id="23611-106">There is a preconfigured set of attribute mappings between Azure AD user objects and each SaaS app’s user objects.</span></span> <span data-ttu-id="23611-107">Egyes alkalmazások más típusú objektumok, például a csoportok vagy névjegyek kezelése.</span><span class="sxs-lookup"><span data-stu-id="23611-107">Some apps manage other types of objects, such as Groups or Contacts.</span></span> <br> 
 <span data-ttu-id="23611-108">Hello alapértelmezett attribútum-leképezésekhez tooyour az üzleti igények szerint testre.</span><span class="sxs-lookup"><span data-stu-id="23611-108">You can customize hello default attribute mappings according tooyour business needs.</span></span> <span data-ttu-id="23611-109">Ez azt jelenti, módosítsa vagy törölje a meglévő attribútum-leképezésekhez vagy hozzon létre új attribútum-leképezésekhez.</span><span class="sxs-lookup"><span data-stu-id="23611-109">This means, you can change or delete existing attribute mappings or create new attribute mappings.</span></span>

<span data-ttu-id="23611-110">Hello Azure AD portálon, a funkció eléréséhez kattintson egy **hozzárendelések** konfigurációja **kiépítési** a hello **kezelése** szakasza egy  **A vállalati alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="23611-110">In hello Azure AD portal, you can access this feature by clicking a **Mappings** configuration under **Provisioning** in hello **Manage** section of an **Enterprise application**.</span></span>


![Salesforce][5] 

<span data-ttu-id="23611-112">Kattintson egy **hozzárendelések** megnyílt kapcsolódó hello beállítása, **attribútum leképezési** panelen.</span><span class="sxs-lookup"><span data-stu-id="23611-112">Clicking a **Mappings** configuration, opens hello related **Attribute Mapping** blade.</span></span>  
<span data-ttu-id="23611-113">Nincsenek attribútum-leképezésekhez szükséges egy SaaS-alkalmazás toofunction megfelelően.</span><span class="sxs-lookup"><span data-stu-id="23611-113">There are attribute mappings that are required by a SaaS application toofunction correctly.</span></span> <span data-ttu-id="23611-114">A szükséges attribútumokat hello **törlése** szolgáltatás nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="23611-114">For required attributes, hello **Delete** feature is unavailable.</span></span>


![Salesforce][6]  

<span data-ttu-id="23611-116">Hello a fenti példában láthatja, hogy hello **felhasználónév** attribútum egy felügyelt objektum a Salesforce-ban a telepítéskor hello **userPrincipalName** hello értékének csatolt Azure Active Directory-objektum.</span><span class="sxs-lookup"><span data-stu-id="23611-116">In hello example above, you can see that hello **Username** attribute of a managed object in Salesforce is populated with hello **userPrincipalName** value of hello linked Azure Active Directory Object.</span></span>

<span data-ttu-id="23611-117">Testre szabhat meglévő **attribútum-leképezésekhez** leképezéseket kattintva.</span><span class="sxs-lookup"><span data-stu-id="23611-117">You can customize existing **Attribute Mappings** by clicking a mapping.</span></span> <span data-ttu-id="23611-118">Ekkor megnyílik a hello **attribútum szerkesztése** panelen.</span><span class="sxs-lookup"><span data-stu-id="23611-118">This opens hello **Edit Attribute** blade.</span></span>

![Salesforce][7]  


  

## <a name="understanding-attribute-mapping-types"></a><span data-ttu-id="23611-120">Attribútum-leképezés típusainak ismertetése</span><span class="sxs-lookup"><span data-stu-id="23611-120">Understanding attribute mapping types</span></span>
<span data-ttu-id="23611-121">Az attribútum-leképezésekhez meghatározhatja, hogyan attribútumok megjelenik a külső SaaS-alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="23611-121">With attribute mappings, you control how attributes are populated in a third-party SaaS application.</span></span> <span data-ttu-id="23611-122">Számos négy különböző hozzárendelési típusokhoz támogatott.</span><span class="sxs-lookup"><span data-stu-id="23611-122">There are four different mapping types supported:</span></span>

* <span data-ttu-id="23611-123">**Közvetlen** – hello cél attribútumának a telepítéskor hello csatolt objektum attribútum értékének hello Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="23611-123">**Direct** – hello target attribute is populated with hello value of an attribute of hello linked object in Azure AD.</span></span>
* <span data-ttu-id="23611-124">**Állandó** – hello cél attribútumának a telepítéskor megadott egy adott karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="23611-124">**Constant** – hello target attribute is populated with a specific string you have specified.</span></span>
* <span data-ttu-id="23611-125">**Kifejezés** -hello target attribútummal hello egy parancsfájl-szerű kifejezés eredménye alapján van feltöltve.</span><span class="sxs-lookup"><span data-stu-id="23611-125">**Expression** - hello target attribute is populated based on hello result of a script-like expression.</span></span> 
  <span data-ttu-id="23611-126">További információkért lásd: [írás kifejezések az Azure Active Directoryban attribútum-leképezésekhez](active-directory-saas-writing-expressions-for-attribute-mappings.md).</span><span class="sxs-lookup"><span data-stu-id="23611-126">For more information, see [Writing Expressions for Attribute Mappings in Azure Active Directory](active-directory-saas-writing-expressions-for-attribute-mappings.md).</span></span>
* <span data-ttu-id="23611-127">**Nincs** -hello target attribútummal marad változatlan.</span><span class="sxs-lookup"><span data-stu-id="23611-127">**None** - hello target attribute is left unmodified.</span></span> <span data-ttu-id="23611-128">Azonban Ha valaha is üres hello target attribútummal, a telepítéskor megadott hello alapértelmezett értéket.</span><span class="sxs-lookup"><span data-stu-id="23611-128">However, if hello target attribute is ever empty, it is populated with hello Default value that you specify.</span></span>

<span data-ttu-id="23611-129">Továbbá toothese négy alapvető attribútum hozzárendelési típusokhoz, egyéni attribútum-leképezésekhez támogatják a nem kötelező hello fogalma **alapértelmezett** érték-hozzárendelés.</span><span class="sxs-lookup"><span data-stu-id="23611-129">In addition toothese four basic attribute mapping types, custom attribute mappings support hello concept of an optional **default** value assignment.</span></span> <span data-ttu-id="23611-130">alapértelmezett érték-hozzárendelés hello biztosítja, hogy a target attribútummal megadni értéket, ha nincs érték sem, az Azure ad-ben, és nem hello célobjektum.</span><span class="sxs-lookup"><span data-stu-id="23611-130">hello default value assignment ensures that a target attribute is populated with a value if there is neither a value in Azure AD nor on hello target object.</span></span> <span data-ttu-id="23611-131">hello leggyakoribb konfigurálása ezen üres tooleave számára.</span><span class="sxs-lookup"><span data-stu-id="23611-131">hello most common configuration is tooleave this blank.</span></span>


## <a name="understanding-attribute-mapping-properties"></a><span data-ttu-id="23611-132">Leképezési attribútumtulajdonságok ismertetése</span><span class="sxs-lookup"><span data-stu-id="23611-132">Understanding attribute mapping properties</span></span>

<span data-ttu-id="23611-133">Hello előző szakaszban már megtörtént a bevezetett toohello attribútum leképezési type tulajdonságot.</span><span class="sxs-lookup"><span data-stu-id="23611-133">In hello previous section, you have already been introduced toohello attribute mapping type property.</span></span>
<span data-ttu-id="23611-134">Továbbá toothis tulajdonságban attribútum-leképezésekhez is támogatja a következő attribútumok hello:</span><span class="sxs-lookup"><span data-stu-id="23611-134">In addition toothis property, attribute mappings do also support hello following attributes:</span></span>

- <span data-ttu-id="23611-135">**Adatforrás-attribútum** -hello felhasználói attribútum hello forrásrendszerből (pl.: Azure Active Directory).</span><span class="sxs-lookup"><span data-stu-id="23611-135">**Source attribute** - hello user attribute from hello source system (e.g.: Azure Active Directory).</span></span>
- <span data-ttu-id="23611-136">**Cél attribútumának** – hello felhasználói attribútum hello célrendszerben (pl.: a ServiceNow).</span><span class="sxs-lookup"><span data-stu-id="23611-136">**Target attribute** – hello user attribute in hello target system (e.g.: ServiceNow).</span></span>
- <span data-ttu-id="23611-137">**Ezzel az attribútummal objektumok megfelelő** – függetlenül attól, ez a leképezés használandó toouniquely azonosítsa azokat a felhasználókat hello forrása és célja rendszerek között.</span><span class="sxs-lookup"><span data-stu-id="23611-137">**Match objects using this attribute** – Whether or not this mapping should be used toouniquely identify users between hello source and target systems.</span></span> <span data-ttu-id="23611-138">Ez általában hello userPrincipalName van beállítva, vagy az Azure AD, amely általában a levél attribútum leképezve tooa felhasználónév mező a célalkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="23611-138">This is typically set on hello userPrincipalName or mail attribute in Azure AD, which is typically mapped tooa username field in a target application.</span></span>
- <span data-ttu-id="23611-139">**Megfelelő sorrend** – több egyező attribútumok állítható be.</span><span class="sxs-lookup"><span data-stu-id="23611-139">**Matching precedence** – Multiple matching attributes can be set.</span></span> <span data-ttu-id="23611-140">Ha több, azok kiértékelése sorrendben történik hello határozzák meg ezt a mezőt.</span><span class="sxs-lookup"><span data-stu-id="23611-140">When there are multiple, they are evaluated in hello order defined by this field.</span></span> <span data-ttu-id="23611-141">Amint a program egyezést talál, további megfelelő attribútumok kiértékelése.</span><span class="sxs-lookup"><span data-stu-id="23611-141">As soon as a match is found, no further matching attributes are evaluated.</span></span>
- <span data-ttu-id="23611-142">**Ez a leképezés alkalmazása**</span><span class="sxs-lookup"><span data-stu-id="23611-142">**Apply this mapping**</span></span>
    - <span data-ttu-id="23611-143">**Mindig** – Ez a leképezés alkalmazni mind a felhasználó létrehozása és a műveletek frissítése</span><span class="sxs-lookup"><span data-stu-id="23611-143">**Always** – Apply this mapping on both user creation and update actions</span></span>
    - <span data-ttu-id="23611-144">**Csak létrehozásakor** – Ez a leképezés csak a felhasználó létrehozási műveletek alkalmazása</span><span class="sxs-lookup"><span data-stu-id="23611-144">**Only during creation** - Apply this mapping only on user creation actions</span></span>


## <a name="what-you-should-know"></a><span data-ttu-id="23611-145">Tudnivalók</span><span class="sxs-lookup"><span data-stu-id="23611-145">What you should know</span></span>

<span data-ttu-id="23611-146">Microsoft Azure AD egy hatékony megvalósítása a szinkronizálási folyamat nyújt.</span><span class="sxs-lookup"><span data-stu-id="23611-146">Microsoft Azure AD provides an efficient implementation of a synchronization process.</span></span> <span data-ttu-id="23611-147">Inicializált környezetben csak a frissítést igénylő objektumok feldolgozása során szinkronizálási ciklust.</span><span class="sxs-lookup"><span data-stu-id="23611-147">In an initialized environment, only objects requiring updates are processed during a synchronization cycle.</span></span> <span data-ttu-id="23611-148">Attribútum-leképezésekhez frissítése szinkronizálási ciklust hello teljesítményére hatással van.</span><span class="sxs-lookup"><span data-stu-id="23611-148">Updating attribute mappings has an impact on hello performance of a synchronization cycle.</span></span> <span data-ttu-id="23611-149">Egy frissítés toohello attribútum hozzárendelési konfigurációja minden felügyelt objektumok toobe reevaluated igényel.</span><span class="sxs-lookup"><span data-stu-id="23611-149">An update toohello attribute mapping configuration requires all managed objects toobe reevaluated.</span></span> <span data-ttu-id="23611-150">Ajánlott gyakorlati eljárás tookeep hello számos egymást követő módosítások tooyour attribútum-leképezésekhez minimális.</span><span class="sxs-lookup"><span data-stu-id="23611-150">It is a recommended best practice tookeep hello number of consecutive changes tooyour attribute mappings at a minimum.</span></span>

## <a name="next-steps"></a><span data-ttu-id="23611-151">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="23611-151">Next steps</span></span>

* [<span data-ttu-id="23611-152">Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke</span><span class="sxs-lookup"><span data-stu-id="23611-152">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
* [<span data-ttu-id="23611-153">Felhasználói kiépítési/megszüntetés tooSaaS alkalmazások automatizálása</span><span class="sxs-lookup"><span data-stu-id="23611-153">Automate User Provisioning/Deprovisioning tooSaaS Apps</span></span>](active-directory-saas-app-provisioning.md)
* [<span data-ttu-id="23611-154">Attribútum-leképezésekhez kifejezések írása</span><span class="sxs-lookup"><span data-stu-id="23611-154">Writing Expressions for Attribute Mappings</span></span>](active-directory-saas-writing-expressions-for-attribute-mappings.md)
* [<span data-ttu-id="23611-155">Helyezése Hatókörszűrőkkel felhasználói történő üzembe helyezéséhez</span><span class="sxs-lookup"><span data-stu-id="23611-155">Scoping Filters for User Provisioning</span></span>](active-directory-saas-scoping-filters.md)
* [<span data-ttu-id="23611-156">SCIM tooenable automatikus kiépítés a felhasználók és csoportok az Azure Active Directory tooapplications használatával</span><span class="sxs-lookup"><span data-stu-id="23611-156">Using SCIM tooenable automatic provisioning of users and groups from Azure Active Directory tooapplications</span></span>](active-directory-scim-provisioning.md)
* [<span data-ttu-id="23611-157">Alkalmazás-kiépítési értesítések</span><span class="sxs-lookup"><span data-stu-id="23611-157">Account Provisioning Notifications</span></span>](active-directory-saas-account-provisioning-notifications.md)
* [<span data-ttu-id="23611-158">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz</span><span class="sxs-lookup"><span data-stu-id="23611-158">List of Tutorials on How tooIntegrate SaaS Apps</span></span>](active-directory-saas-tutorial-list.md)

<!--Image references-->
[1]: ./media/active-directory-saas-customizing-attribute-mappings/ic765497.png
[2]: ./media/active-directory-saas-customizing-attribute-mappings/ic775419.png
[3]: ./media/active-directory-saas-customizing-attribute-mappings/ic775420.png
[4]: ./media/active-directory-saas-customizing-attribute-mappings/ic775421.png
[5]: ./media/active-directory-saas-customizing-attribute-mappings/21.png
[6]: ./media/active-directory-saas-customizing-attribute-mappings/22.png
[7]: ./media/active-directory-saas-customizing-attribute-mappings/23.png

