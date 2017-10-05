---
title: "Hozzájárul az alkalmazás végrehajtása során váratlan hiba |} Microsoft Docs"
description: "Ismerteti, amelyek hozzájárul ahhoz, hogy az alkalmazások és a rájuk vonatkozó teendők során előforduló hibák"
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
ms.openlocfilehash: fd500fdd4c8642bad96dcf71eebcf1fad461a35f
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="unexpected-error-when-performing-consent-to-an-application"></a><span data-ttu-id="10b72-103">Hozzájárul az alkalmazás végrehajtása során váratlan hiba</span><span class="sxs-lookup"><span data-stu-id="10b72-103">Unexpected error when performing consent to an application</span></span>

<span data-ttu-id="10b72-104">A cikkből megtudhatja, hozzájárul ahhoz, hogy egy alkalmazás során előforduló hibákat.</span><span class="sxs-lookup"><span data-stu-id="10b72-104">This article discusses errors that can occur during the process of consenting to an application.</span></span> <span data-ttu-id="10b72-105">Váratlan hibaelhárítás esetén beleegyezést kér, amely nem tartalmazza a hibaüzeneteket, olvassa el [hitelesítési forgatókönyvek az Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-authentication-scenarios).</span><span class="sxs-lookup"><span data-stu-id="10b72-105">If you are Troubleshoot unexpected consent prompts that do not contain any error messages, see [Authentication Scenarios for Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-authentication-scenarios).</span></span>

<span data-ttu-id="10b72-106">Számos alkalmazás, amelyekbe beépül az Azure Active Directory működéséhez egyéb erőforrásainak elérésére engedély szükséges.</span><span class="sxs-lookup"><span data-stu-id="10b72-106">Many applications that integrate with Azure Active Directory require permissions to access other resources in order to function.</span></span> <span data-ttu-id="10b72-107">Ha ezeket az erőforrásokat is integrálva vannak az Azure Active Directoryval, azok eléréséhez engedélyek gyakran kérik a hozzájárulási közös keretrendszer használatával.</span><span class="sxs-lookup"><span data-stu-id="10b72-107">When these resources are also integrated with Azure Active Directory, permissions to access them is often requested using the common consent framework.</span></span> 

<span data-ttu-id="10b72-108">Az eredmény egy beleegyezést kérő üzenete jelenik meg, amely általában akkor következik be, először az alkalmazás használja, de akkor is előfordulhat egy későbbi használja az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="10b72-108">This results in a consent prompt being displayed, which generally occurs the first time an application is used but can also occur on a subsequent use of the application.</span></span>

<span data-ttu-id="10b72-109">Bizonyos feltételeknek kell teljesülniük felhasználói számára, hogy az engedélyeket, az alkalmazás futtatásához szükséges.</span><span class="sxs-lookup"><span data-stu-id="10b72-109">Certain conditions must be true for a user to consent to the permissions an application requires.</span></span> <span data-ttu-id="10b72-110">Ha ezek a feltételek nem teljesülnek, több hiba is felléphet.</span><span class="sxs-lookup"><span data-stu-id="10b72-110">If these conditions are not met, various errors can occur.</span></span> <span data-ttu-id="10b72-111">Ezek a következők:</span><span class="sxs-lookup"><span data-stu-id="10b72-111">These include:</span></span>

## <a name="requesting-not-authorized-permissions-error"></a><span data-ttu-id="10b72-112">A kért nem engedélyezett engedélyek hiba</span><span class="sxs-lookup"><span data-stu-id="10b72-112">Requesting not authorized permissions error</span></span>
* <span data-ttu-id="10b72-113">**AADSTS90093:** &lt;clientAppDisplayName&gt; egy vagy több engedélyek, amelynek Ön nem jogosult a megadását kéri.</span><span class="sxs-lookup"><span data-stu-id="10b72-113">**AADSTS90093:** &lt;clientAppDisplayName&gt; is requesting one or more permissions that you are not authorized to grant.</span></span> <span data-ttu-id="10b72-114">Kérje a rendszergazda, aki is hozzájárul az alkalmazás az Ön nevében.</span><span class="sxs-lookup"><span data-stu-id="10b72-114">Contact an administrator, who can consent to this application on your behalf.</span></span>

<span data-ttu-id="10b72-115">Ez a hiba akkor fordul elő, amikor egy felhasználó egy vállalati rendszergazdai jogosultságokkal nem kísérli meg, amelyeket csak a rendszergazda adhat engedélyeket igénylő alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="10b72-115">This error occurs when a user who is not a company administrator attempts to use an application that is requesting permissions that only an administrator can grant.</span></span> <span data-ttu-id="10b72-116">Ez a hiba a szervezet nevében az alkalmazáshoz való hozzáférés biztosítása a rendszergazda megoldhatók.</span><span class="sxs-lookup"><span data-stu-id="10b72-116">This error can be resolved by an administrator granting access to the application on behalf of their organization.</span></span>

## <a name="policy-prevents-granting-permissions-error"></a><span data-ttu-id="10b72-117">Házirend megakadályozza, hogy jogosultságokat hiba</span><span class="sxs-lookup"><span data-stu-id="10b72-117">Policy prevents granting permissions error</span></span>
* <span data-ttu-id="10b72-118">**AADSTS90093:** rendszergazdája &lt;tenantDisplayName&gt; be van állítva egy házirendet, amely megakadályozza, hogy biztosítása &lt;nevű alkalmazás&gt; az engedélyek e a kért tartalomhoz.</span><span class="sxs-lookup"><span data-stu-id="10b72-118">**AADSTS90093:** An administrator of &lt;tenantDisplayName&gt; has set a policy that prevents you from granting &lt;name of app&gt; the permissions it is requesting.</span></span> <span data-ttu-id="10b72-119">Forduljon a rendszergazdához &lt;tenantDisplayName&gt;, akik engedélyeket ennek az alkalmazásnak az Ön nevében.</span><span class="sxs-lookup"><span data-stu-id="10b72-119">Contact an administrator of &lt;tenantDisplayName&gt;, who can grant permissions to this app on your behalf.</span></span>

<span data-ttu-id="10b72-120">Ez a hiba akkor fordul elő, amikor a vállalati rendszergazda kikapcsolja alkalmazások, hogy a felhasználók, akkor egy rendszergazdai jogokkal nem rendelkező felhasználó próbál jóváhagyását igénylő alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="10b72-120">This error occurs when a company administrator turns off the ability for users to consent to applications, then a non-administrator user attempts to use an application that requires consent.</span></span> <span data-ttu-id="10b72-121">Ez a hiba a szervezet nevében az alkalmazáshoz való hozzáférés biztosítása a rendszergazda megoldhatók.</span><span class="sxs-lookup"><span data-stu-id="10b72-121">This error can be resolved by an administrator granting access to the application on behalf of their organization.</span></span>

## <a name="intermittent-problem-error"></a><span data-ttu-id="10b72-122">Probléma hiba</span><span class="sxs-lookup"><span data-stu-id="10b72-122">Intermittent problem error</span></span>
* <span data-ttu-id="10b72-123">**AADSTS90090:** úgy tűnik, az engedélyek megadása a próbált rögzítése probléma történt &lt;clientAppDisplayName&gt;.</span><span class="sxs-lookup"><span data-stu-id="10b72-123">**AADSTS90090:** It looks like we encountered an intermittent problem recording the permissions you attempted to grant to &lt;clientAppDisplayName&gt;.</span></span> <span data-ttu-id="10b72-124">Próbálkozzon újra később.</span><span class="sxs-lookup"><span data-stu-id="10b72-124">try again later.</span></span>

<span data-ttu-id="10b72-125">Ez a hiba azt jelzi, hogy az időszakos szolgáltatás kiszolgálóoldali hiba történt.</span><span class="sxs-lookup"><span data-stu-id="10b72-125">This error indicates that an intermittent service side issue has occurred.</span></span> <span data-ttu-id="10b72-126">Ez hozzájárul az alkalmazást újra megpróbálja megoldhatók.</span><span class="sxs-lookup"><span data-stu-id="10b72-126">It can be resolved by attempting to consent to the application again.</span></span>

## <a name="resource-not-available-error"></a><span data-ttu-id="10b72-127">Erőforrás nem érhető el hiba</span><span class="sxs-lookup"><span data-stu-id="10b72-127">Resource not available error</span></span>
* <span data-ttu-id="10b72-128">**AADSTS65005:** az alkalmazás &lt;clientAppDisplayName&gt; kért engedélyeket az erőforrás eléréséhez &lt;resourceAppDisplayName&gt; , amely nem áll rendelkezésre.</span><span class="sxs-lookup"><span data-stu-id="10b72-128">**AADSTS65005:** The app &lt;clientAppDisplayName&gt; requested permissions to access a resource &lt;resourceAppDisplayName&gt; that is not available.</span></span> 

<span data-ttu-id="10b72-129">Lépjen kapcsolatba az alkalmazás fejlesztőjének.</span><span class="sxs-lookup"><span data-stu-id="10b72-129">Contact the application developer.</span></span>

##  <a name="resource-not-available-in-tenant-error"></a><span data-ttu-id="10b72-130">Az erőforrás nem érhető el a bérlői hiba</span><span class="sxs-lookup"><span data-stu-id="10b72-130">Resource not available in tenant error</span></span>
* <span data-ttu-id="10b72-131">**AADSTS65005:** &lt;clientAppDisplayName&gt; kér az erőforráshoz való hozzáférés &lt;resourceAppDisplayName&gt; , amely nem érhető el a szervezet &lt; tenantDisplayName&gt;.</span><span class="sxs-lookup"><span data-stu-id="10b72-131">**AADSTS65005:** &lt;clientAppDisplayName&gt; is requesting access to a resource &lt;resourceAppDisplayName&gt; that is not available in your organization &lt;tenantDisplayName&gt;.</span></span> 

<span data-ttu-id="10b72-132">Győződjön meg arról, hogy rendelkezésre áll-e ehhez az erőforráshoz, vagy forduljon a rendszergazdához &lt;tenantDisplayName&gt;.</span><span class="sxs-lookup"><span data-stu-id="10b72-132">Ensure that this resource is available or contact an administrator of &lt;tenantDisplayName&gt;.</span></span>

## <a name="permissions-mismatch-error"></a><span data-ttu-id="10b72-133">Engedélyek verzióeltérési hiba</span><span class="sxs-lookup"><span data-stu-id="10b72-133">Permissions mismatch error</span></span>
* <span data-ttu-id="10b72-134">**AADSTS65005:** az alkalmazás hozzáférési erőforrás hozzájárul kért &lt;resourceAppDisplayName&gt;.</span><span class="sxs-lookup"><span data-stu-id="10b72-134">**AADSTS65005:** The app requested consent to access resource &lt;resourceAppDisplayName&gt;.</span></span> <span data-ttu-id="10b72-135">A kérelem sikertelen volt, mert nem felel meg milyen az alkalmazás előre konfigurált volt app regisztráció során.</span><span class="sxs-lookup"><span data-stu-id="10b72-135">This request failed because it does not match how the app was pre-configured during app registration.</span></span> <span data-ttu-id="10b72-136">Lépjen kapcsolatba az alkalmazás vendor.* *</span><span class="sxs-lookup"><span data-stu-id="10b72-136">Contact the app vendor.**</span></span>

<span data-ttu-id="10b72-137">Ezek a hibák összes fordulhat elő, ha a felhasználó megpróbálja járul hozzá az alkalmazás engedélyeket, hogy egy erőforrás-alkalmazás, amely nem található a munkahely címtárában (bérlői).</span><span class="sxs-lookup"><span data-stu-id="10b72-137">These errors all occur when the application a user is trying to consent to is requesting permissions to access a resource application that cannot be found in the organization’s directory (tenant).</span></span> <span data-ttu-id="10b72-138">Ez akkor fordulhat elő, több okból:</span><span class="sxs-lookup"><span data-stu-id="10b72-138">This can occur for several reasons:</span></span>

-   <span data-ttu-id="10b72-139">Az ügyfél alkalmazásfejlesztő rendelkezik az alkalmazás nincs megfelelően konfigurálva, azt, hogy hozzáférést igényelhessen érvénytelen erőforrás.</span><span class="sxs-lookup"><span data-stu-id="10b72-139">The client application developer has configured their application incorrectly, causing it to request access to an invalid resource.</span></span> <span data-ttu-id="10b72-140">Ebben az esetben az alkalmazás fejlesztőjének frissítenie kell a probléma megoldásához az ügyfélalkalmazás konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="10b72-140">In this case, the application developer must update the configuration of the client application to resolve this issue.</span></span>

-   <span data-ttu-id="10b72-141">A célalkalmazás erőforrás képviselő egyszerű szolgáltatás nem létezik a szervezeten belül, vagy a korábban létezett, de el lett távolítva.</span><span class="sxs-lookup"><span data-stu-id="10b72-141">A Service Principal representing the target resource application does not exist in the organization, or existed in the past but has been removed.</span></span> <span data-ttu-id="10b72-142">A probléma megoldásához, az erőforrás alkalmazás egy egyszerű szolgáltatást ki kell építenie a szervezet, az ügyfélalkalmazás kérhetnek a szükséges fájlengedélyekkel.</span><span class="sxs-lookup"><span data-stu-id="10b72-142">To resolve this issue, a Service Principal for the resource application must be provisioned in the organization so the client application can request permissions to it.</span></span> <span data-ttu-id="10b72-143">Ez azonban a hiba akkor fordulhat elő a egy számos módon, attól függően, hogy az alkalmazás, beleértve a következőket:</span><span class="sxs-lookup"><span data-stu-id="10b72-143">This can happen in an number of ways, depending on the type of application, including:</span></span>

    -   <span data-ttu-id="10b72-144">Előfizetés (Microsoft közzétett alkalmazásokat) az erőforrás-alkalmazáshoz az beszerzése</span><span class="sxs-lookup"><span data-stu-id="10b72-144">Acquiring a subscription for the resource application (Microsoft published applications)</span></span>

    -   <span data-ttu-id="10b72-145">Beleegyezik abba, hogy az erőforrás-alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="10b72-145">Consenting to the resource application</span></span>

    -   <span data-ttu-id="10b72-146">Az alkalmazás jogosultságokat az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="10b72-146">Granting the application permissions via the Azure Portal</span></span>

    -   <span data-ttu-id="10b72-147">Az alkalmazás hozzáadása az Azure AD alkalmazás gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="10b72-147">Adding the application from the Azure AD Application Gallery</span></span>

## <a name="next-steps"></a><span data-ttu-id="10b72-148">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="10b72-148">Next steps</span></span> 

[<span data-ttu-id="10b72-149">Alkalmazások, engedélyek és az Azure Active Directoryban (v1 végpont) hozzájárulás</span><span class="sxs-lookup"><span data-stu-id="10b72-149">Apps, permissions, and consent in Azure Active Directory (v1 endpoint)</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-apps-permissions-consent)<br>

[<span data-ttu-id="10b72-150">Hatókörök, engedélyek és az Azure Active Directoryban (v2.0-végponttól) hozzájárulás</span><span class="sxs-lookup"><span data-stu-id="10b72-150">Scopes, permissions, and consent in the Azure Active Directory (v2.0 endpoint)</span></span>](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes)


