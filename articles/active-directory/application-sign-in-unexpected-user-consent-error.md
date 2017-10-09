---
title: "Váratlan hiba AAA hozzájárulási tooan alkalmazás végrehajtása során |} Microsoft Docs"
description: "Ismerteti a küldőnek tooan alkalmazás és a rájuk vonatkozó teendők hello folyamat során előforduló hibák"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: dfee35f3a10e3cc4313109cedd972499452320f1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="unexpected-error-when-performing-consent-tooan-application"></a><span data-ttu-id="f17c2-103">Hozzájárulás tooan alkalmazás végrehajtása során váratlan hiba</span><span class="sxs-lookup"><span data-stu-id="f17c2-103">Unexpected error when performing consent tooan application</span></span>

<span data-ttu-id="f17c2-104">A cikk ismerteti a küldőnek tooan alkalmazás hello folyamat során előforduló hibákat.</span><span class="sxs-lookup"><span data-stu-id="f17c2-104">This article discusses errors that can occur during hello process of consenting tooan application.</span></span> <span data-ttu-id="f17c2-105">Váratlan hibaelhárítás esetén beleegyezést kér, amely nem tartalmazza a hibaüzeneteket, olvassa el [hitelesítési forgatókönyvek az Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-authentication-scenarios).</span><span class="sxs-lookup"><span data-stu-id="f17c2-105">If you are Troubleshoot unexpected consent prompts that do not contain any error messages, see [Authentication Scenarios for Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-authentication-scenarios).</span></span>

<span data-ttu-id="f17c2-106">Számos olyan alkalmazás, amelyekbe beépül az Azure Active Directory szükséges engedélyek tooaccess rendelés toofunction egyéb erőforrások.</span><span class="sxs-lookup"><span data-stu-id="f17c2-106">Many applications that integrate with Azure Active Directory require permissions tooaccess other resources in order toofunction.</span></span> <span data-ttu-id="f17c2-107">Ha ezeket az erőforrásokat is az Azure Active Directoryval integrált, azokat gyakran kért közös hello segítségével engedélyek tooaccess hozzájárulás keretrendszer.</span><span class="sxs-lookup"><span data-stu-id="f17c2-107">When these resources are also integrated with Azure Active Directory, permissions tooaccess them is often requested using hello common consent framework.</span></span> 

<span data-ttu-id="f17c2-108">Az eredmény egy beleegyezést kérő üzenete jelenik meg, amely általában akkor fordul elő, hello először az alkalmazás használja, de akkor is előfordulhat, a hello alkalmazás későbbi használatra.</span><span class="sxs-lookup"><span data-stu-id="f17c2-108">This results in a consent prompt being displayed, which generally occurs hello first time an application is used but can also occur on a subsequent use of hello application.</span></span>

<span data-ttu-id="f17c2-109">Bizonyos feltételeknek kell teljesülniük, az alkalmazás futtatásához szükséges felhasználói tooconsent toohello engedélyek.</span><span class="sxs-lookup"><span data-stu-id="f17c2-109">Certain conditions must be true for a user tooconsent toohello permissions an application requires.</span></span> <span data-ttu-id="f17c2-110">Ha ezek a feltételek nem teljesülnek, több hiba is felléphet.</span><span class="sxs-lookup"><span data-stu-id="f17c2-110">If these conditions are not met, various errors can occur.</span></span> <span data-ttu-id="f17c2-111">Ezek a következők:</span><span class="sxs-lookup"><span data-stu-id="f17c2-111">These include:</span></span>

## <a name="requesting-not-authorized-permissions-error"></a><span data-ttu-id="f17c2-112">A kért nem engedélyezett engedélyek hiba</span><span class="sxs-lookup"><span data-stu-id="f17c2-112">Requesting not authorized permissions error</span></span>
* <span data-ttu-id="f17c2-113">**AADSTS90093:** &lt;clientAppDisplayName&gt; egy vagy több engedélyeket, amelyek nem engedélyezett toogrant kér.</span><span class="sxs-lookup"><span data-stu-id="f17c2-113">**AADSTS90093:** &lt;clientAppDisplayName&gt; is requesting one or more permissions that you are not authorized toogrant.</span></span> <span data-ttu-id="f17c2-114">Kérje a rendszergazda, aki is hozzájárulás toothis alkalmazás az Ön nevében.</span><span class="sxs-lookup"><span data-stu-id="f17c2-114">Contact an administrator, who can consent toothis application on your behalf.</span></span>

<span data-ttu-id="f17c2-115">Ez a hiba akkor fordul elő, amikor egy felhasználó egy vállalati rendszergazdai jogosultságokkal nem kísérli meg, amelyeket csak a rendszergazda adhat engedélyeket igénylő alkalmazás toouse.</span><span class="sxs-lookup"><span data-stu-id="f17c2-115">This error occurs when a user who is not a company administrator attempts toouse an application that is requesting permissions that only an administrator can grant.</span></span> <span data-ttu-id="f17c2-116">Ez a hiba a rendszergazda hozzáférést toohello alkalmazás a szervezet nevében megadása megoldhatók.</span><span class="sxs-lookup"><span data-stu-id="f17c2-116">This error can be resolved by an administrator granting access toohello application on behalf of their organization.</span></span>

## <a name="policy-prevents-granting-permissions-error"></a><span data-ttu-id="f17c2-117">Házirend megakadályozza, hogy jogosultságokat hiba</span><span class="sxs-lookup"><span data-stu-id="f17c2-117">Policy prevents granting permissions error</span></span>
* <span data-ttu-id="f17c2-118">**AADSTS90093:** rendszergazdája &lt;tenantDisplayName&gt; be van állítva egy házirendet, amely megakadályozza, hogy biztosítása &lt;nevű alkalmazás&gt; hello engedélyek-e a kért tartalomhoz.</span><span class="sxs-lookup"><span data-stu-id="f17c2-118">**AADSTS90093:** An administrator of &lt;tenantDisplayName&gt; has set a policy that prevents you from granting &lt;name of app&gt; hello permissions it is requesting.</span></span> <span data-ttu-id="f17c2-119">Forduljon a rendszergazdához &lt;tenantDisplayName&gt;, akik biztosíthat engedélyek toothis alkalmazás az Ön nevében.</span><span class="sxs-lookup"><span data-stu-id="f17c2-119">Contact an administrator of &lt;tenantDisplayName&gt;, who can grant permissions toothis app on your behalf.</span></span>

<span data-ttu-id="f17c2-120">Ez a hiba esetén a vállalati rendszergazda kikapcsolja hello lehetőségét, hogy a felhasználók tooconsent tooapplications, akkor egy rendszergazdai jogokkal nem rendelkező felhasználó próbál toouse jóváhagyását igénylő alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="f17c2-120">This error occurs when a company administrator turns off hello ability for users tooconsent tooapplications, then a non-administrator user attempts toouse an application that requires consent.</span></span> <span data-ttu-id="f17c2-121">Ez a hiba a rendszergazda hozzáférést toohello alkalmazás a szervezet nevében megadása megoldhatók.</span><span class="sxs-lookup"><span data-stu-id="f17c2-121">This error can be resolved by an administrator granting access toohello application on behalf of their organization.</span></span>

## <a name="intermittent-problem-error"></a><span data-ttu-id="f17c2-122">Probléma hiba</span><span class="sxs-lookup"><span data-stu-id="f17c2-122">Intermittent problem error</span></span>
* <span data-ttu-id="f17c2-123">**AADSTS90090:** úgy tűnik, túl próbált toogrant hello engedélyek rögzítése probléma történt&lt;clientAppDisplayName&gt;.</span><span class="sxs-lookup"><span data-stu-id="f17c2-123">**AADSTS90090:** It looks like we encountered an intermittent problem recording hello permissions you attempted toogrant too&lt;clientAppDisplayName&gt;.</span></span> <span data-ttu-id="f17c2-124">Próbálkozzon újra később.</span><span class="sxs-lookup"><span data-stu-id="f17c2-124">try again later.</span></span>

<span data-ttu-id="f17c2-125">Ez a hiba azt jelzi, hogy az időszakos szolgáltatás kiszolgálóoldali hiba történt.</span><span class="sxs-lookup"><span data-stu-id="f17c2-125">This error indicates that an intermittent service side issue has occurred.</span></span> <span data-ttu-id="f17c2-126">Létrehozására tett kísérlettel tooconsent toohello alkalmazást megoldhatók.</span><span class="sxs-lookup"><span data-stu-id="f17c2-126">It can be resolved by attempting tooconsent toohello application again.</span></span>

## <a name="resource-not-available-error"></a><span data-ttu-id="f17c2-127">Erőforrás nem érhető el hiba</span><span class="sxs-lookup"><span data-stu-id="f17c2-127">Resource not available error</span></span>
* <span data-ttu-id="f17c2-128">**AADSTS65005:** hello app &lt;clientAppDisplayName&gt; engedélyek tooaccess erőforrás kért &lt;resourceAppDisplayName&gt; , amely nem áll rendelkezésre.</span><span class="sxs-lookup"><span data-stu-id="f17c2-128">**AADSTS65005:** hello app &lt;clientAppDisplayName&gt; requested permissions tooaccess a resource &lt;resourceAppDisplayName&gt; that is not available.</span></span> 

<span data-ttu-id="f17c2-129">Hello kapcsolattartási alkalmazásfejlesztő.</span><span class="sxs-lookup"><span data-stu-id="f17c2-129">Contact hello application developer.</span></span>

##  <a name="resource-not-available-in-tenant-error"></a><span data-ttu-id="f17c2-130">Az erőforrás nem érhető el a bérlői hiba</span><span class="sxs-lookup"><span data-stu-id="f17c2-130">Resource not available in tenant error</span></span>
* <span data-ttu-id="f17c2-131">**AADSTS65005:** &lt;clientAppDisplayName&gt; kér hozzáférést tooa erőforrás &lt;resourceAppDisplayName&gt; , amely nem érhető el a szervezet &lt; tenantDisplayName&gt;.</span><span class="sxs-lookup"><span data-stu-id="f17c2-131">**AADSTS65005:** &lt;clientAppDisplayName&gt; is requesting access tooa resource &lt;resourceAppDisplayName&gt; that is not available in your organization &lt;tenantDisplayName&gt;.</span></span> 

<span data-ttu-id="f17c2-132">Győződjön meg arról, hogy rendelkezésre áll-e ehhez az erőforráshoz, vagy forduljon a rendszergazdához &lt;tenantDisplayName&gt;.</span><span class="sxs-lookup"><span data-stu-id="f17c2-132">Ensure that this resource is available or contact an administrator of &lt;tenantDisplayName&gt;.</span></span>

## <a name="permissions-mismatch-error"></a><span data-ttu-id="f17c2-133">Engedélyek verzióeltérési hiba</span><span class="sxs-lookup"><span data-stu-id="f17c2-133">Permissions mismatch error</span></span>
* <span data-ttu-id="f17c2-134">**AADSTS65005:** hello app kért hozzájárulási tooaccess erőforrás &lt;resourceAppDisplayName&gt;.</span><span class="sxs-lookup"><span data-stu-id="f17c2-134">**AADSTS65005:** hello app requested consent tooaccess resource &lt;resourceAppDisplayName&gt;.</span></span> <span data-ttu-id="f17c2-135">A kérelem sikertelen volt, mert nem felel meg milyen hello app előre konfigurált volt app regisztráció során.</span><span class="sxs-lookup"><span data-stu-id="f17c2-135">This request failed because it does not match how hello app was pre-configured during app registration.</span></span> <span data-ttu-id="f17c2-136">Forduljon a hello app vendor.* *</span><span class="sxs-lookup"><span data-stu-id="f17c2-136">Contact hello app vendor.**</span></span>

<span data-ttu-id="f17c2-137">Ezek a hibák összes fordulhat elő, ha hello alkalmazás a felhasználó megpróbál tooconsent toois kér engedélyeket tooaccess egy erőforrás-alkalmazás, amely nem található a hello szervezet címtárához (bérlői).</span><span class="sxs-lookup"><span data-stu-id="f17c2-137">These errors all occur when hello application a user is trying tooconsent toois requesting permissions tooaccess a resource application that cannot be found in hello organization’s directory (tenant).</span></span> <span data-ttu-id="f17c2-138">Ez akkor fordulhat elő, több okból:</span><span class="sxs-lookup"><span data-stu-id="f17c2-138">This can occur for several reasons:</span></span>

-   <span data-ttu-id="f17c2-139">hello ügyfél alkalmazásfejlesztő rendelkezik az alkalmazás nincs megfelelően konfigurálva, toorequest hozzáférés tooan érvénytelen erőforrás okozza.</span><span class="sxs-lookup"><span data-stu-id="f17c2-139">hello client application developer has configured their application incorrectly, causing it toorequest access tooan invalid resource.</span></span> <span data-ttu-id="f17c2-140">Ebben az esetben hello alkalmazásfejlesztő frissítenie kell hello ügyfél alkalmazás tooresolve hello konfigurálása a probléma.</span><span class="sxs-lookup"><span data-stu-id="f17c2-140">In this case, hello application developer must update hello configuration of hello client application tooresolve this issue.</span></span>

-   <span data-ttu-id="f17c2-141">Egy egyszerű szolgáltatást képviselő hello célalkalmazás erőforrás nem létezik hello a szervezeten belül, vagy múltbeli hello létezett, de el lett távolítva.</span><span class="sxs-lookup"><span data-stu-id="f17c2-141">A Service Principal representing hello target resource application does not exist in hello organization, or existed in hello past but has been removed.</span></span> <span data-ttu-id="f17c2-142">Ez állítja ki, egy egyszerű hello erőforrás alkalmazást ki kell építenie hello szervezetében, hello ügyfélalkalmazás kérheti az engedélyek tooit tooresolve.</span><span class="sxs-lookup"><span data-stu-id="f17c2-142">tooresolve this issue, a Service Principal for hello resource application must be provisioned in hello organization so hello client application can request permissions tooit.</span></span> <span data-ttu-id="f17c2-143">Ez azonban egy számos módon, attól függően, hogy hello alkalmazás típusát, a hiba akkor fordulhat elő többek között:</span><span class="sxs-lookup"><span data-stu-id="f17c2-143">This can happen in an number of ways, depending on hello type of application, including:</span></span>

    -   <span data-ttu-id="f17c2-144">Előfizetés (Microsoft közzétett alkalmazások) hello erőforrás alkalmazás beszerzése</span><span class="sxs-lookup"><span data-stu-id="f17c2-144">Acquiring a subscription for hello resource application (Microsoft published applications)</span></span>

    -   <span data-ttu-id="f17c2-145">Küldőnek toohello erőforrás-alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="f17c2-145">Consenting toohello resource application</span></span>

    -   <span data-ttu-id="f17c2-146">Hello alkalmazás jogosultságokat hello Azure portálon keresztül</span><span class="sxs-lookup"><span data-stu-id="f17c2-146">Granting hello application permissions via hello Azure Portal</span></span>

    -   <span data-ttu-id="f17c2-147">Az Azure AD Application Gallery hello hello alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="f17c2-147">Adding hello application from hello Azure AD Application Gallery</span></span>

## <a name="next-steps"></a><span data-ttu-id="f17c2-148">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f17c2-148">Next steps</span></span> 

[<span data-ttu-id="f17c2-149">Alkalmazások, engedélyek és az Azure Active Directoryban (v1 végpont) hozzájárulás</span><span class="sxs-lookup"><span data-stu-id="f17c2-149">Apps, permissions, and consent in Azure Active Directory (v1 endpoint)</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-apps-permissions-consent)<br>

[<span data-ttu-id="f17c2-150">Hatókörök, engedélyek és az Azure Active Directoryban (v2.0-végponttól) hello hozzájárulás</span><span class="sxs-lookup"><span data-stu-id="f17c2-150">Scopes, permissions, and consent in hello Azure Active Directory (v2.0 endpoint)</span></span>](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes)


