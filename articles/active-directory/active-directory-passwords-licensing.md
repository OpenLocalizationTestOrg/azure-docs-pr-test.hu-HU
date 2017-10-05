---
title: "Licencelés: Azure AD SSPR |} Microsoft Docs"
description: "Az Azure AD az önkiszolgáló jelszó-átállítási licencelési követelmények"
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: gahug
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: 936134bddad19964f809a17f200ebbeed5aa853c
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="licensing-requirements-for-azure-ad-self-service-password-reset"></a><span data-ttu-id="9a838-103">Licencelési követelmények az Azure AD az önkiszolgáló jelszó alaphelyzetbe állítása</span><span class="sxs-lookup"><span data-stu-id="9a838-103">Licensing requirements for Azure AD self-service password reset</span></span>

<span data-ttu-id="9a838-104">Ahhoz, hogy Azure AD-Jelszóvisszaállítási függvény akkor **rendelkeznie kell legalább egy licenc nélküli a szervezet**.</span><span class="sxs-lookup"><span data-stu-id="9a838-104">In order for Azure AD Password Reset to function, you **must have at least one license assigned in your organization**.</span></span> <span data-ttu-id="9a838-105">Nem határoztunk felhasználónkénti a jelszó alaphelyzetbe állítása során szerzett licencelési.</span><span class="sxs-lookup"><span data-stu-id="9a838-105">We do not enforce per-user licensing on the password reset experience.</span></span> <span data-ttu-id="9a838-106">A Microsoft licencszerződésben a megfelelőség biztosítása érdekében, licenceket rendelhet azokat a felhasználókat, a prémium szolgáltatások használatáért kell.</span><span class="sxs-lookup"><span data-stu-id="9a838-106">To maintain compliance with your Microsoft licensing agreement, you need to assign licenses to any users that use premium features.</span></span>

* <span data-ttu-id="9a838-107">**Csak a felhasználók felhőalapú** -Office 365 (Office 365) bármely fizetett SKU vagy Azure AD alapvető</span><span class="sxs-lookup"><span data-stu-id="9a838-107">**Cloud only users** - Office 365 (O365) any paid SKU, or Azure AD Basic</span></span>
* <span data-ttu-id="9a838-108">**Felhő** és/vagy **helyszíni felhasználók** -Azure AD Premium P1 vagy P2, Enterprise Mobility + Security (EMS) vagy biztonságos hatékony vállalati (Másodper)</span><span class="sxs-lookup"><span data-stu-id="9a838-108">**Cloud** and/or **on-premises users** - Azure AD Premium P1 or P2, Enterprise Mobility + Security (EMS), or Secure Productive Enterprise (SPE)</span></span>

## <a name="licenses-required-for-password-writeback"></a><span data-ttu-id="9a838-109">A jelszóvisszaírás szükséges engedélyek</span><span class="sxs-lookup"><span data-stu-id="9a838-109">Licenses required for password writeback</span></span>

<span data-ttu-id="9a838-110">A jelszóvisszaírás használatához rendelkeznie kell egyet a következő licenccel az Ön bérelt szolgáltatásának.</span><span class="sxs-lookup"><span data-stu-id="9a838-110">To use password writeback, you must have one of the following licenses assigned in your tenant.</span></span>

* <span data-ttu-id="9a838-111">Prémium szintű Azure AD P1</span><span class="sxs-lookup"><span data-stu-id="9a838-111">Azure AD Premium P1</span></span>
* <span data-ttu-id="9a838-112">Prémium szintű Azure AD P2</span><span class="sxs-lookup"><span data-stu-id="9a838-112">Azure AD Premium P2</span></span>
* <span data-ttu-id="9a838-113">Enterprise Mobility + Security E3</span><span class="sxs-lookup"><span data-stu-id="9a838-113">Enterprise Mobility + Security E3</span></span>
* <span data-ttu-id="9a838-114">Enterprise Mobility + Security E5</span><span class="sxs-lookup"><span data-stu-id="9a838-114">Enterprise Mobility + Security E5</span></span>
* <span data-ttu-id="9a838-115">Secure Productive Enterprise E3</span><span class="sxs-lookup"><span data-stu-id="9a838-115">Secure Productive Enterprise E3</span></span>
* <span data-ttu-id="9a838-116">Secure Productive Enterprise E5</span><span class="sxs-lookup"><span data-stu-id="9a838-116">Secure Productive Enterprise E5</span></span>

> [!NOTE]
> <span data-ttu-id="9a838-117">Önálló Office 365 tervek licencelési **nem támogatják a jelszóvisszaírás** és jelszó használata kötelezővé tehető az előző csomagok esetében ez a funkció működéséhez.</span><span class="sxs-lookup"><span data-stu-id="9a838-117">Standalone Office 365 licensing plans **do not support password writeback** and require one of the preceding plans for this functionality to work.</span></span>

<span data-ttu-id="9a838-118">A következő oldalakon található további licencelési adatait, beleértve a költségek</span><span class="sxs-lookup"><span data-stu-id="9a838-118">Additional licensing info including costs can be found on the following pages</span></span>

* [<span data-ttu-id="9a838-119">Az Azure Active Directory árképzési hely</span><span class="sxs-lookup"><span data-stu-id="9a838-119">Azure Active Directory Pricing site</span></span>](https://azure.microsoft.com/pricing/details/active-directory/)
* [<span data-ttu-id="9a838-120">Enterprise Mobility + Security</span><span class="sxs-lookup"><span data-stu-id="9a838-120">Enterprise Mobility + Security</span></span>](https://www.microsoft.com/cloud-platform/enterprise-mobility-security)
* [<span data-ttu-id="9a838-121">Biztonságos hatékony Enterprise</span><span class="sxs-lookup"><span data-stu-id="9a838-121">Secure Productive Enterprise</span></span>](https://www.microsoft.com/secure-productive-enterprise/default.aspx)

## <a name="enable-group-or-user-based-licensing"></a><span data-ttu-id="9a838-122">Csoport vagy felhasználó alapú licencelés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="9a838-122">Enable group or user-based licensing</span></span>

<span data-ttu-id="9a838-123">Mostantól az Azure AD támogatja a biztonságicsoport-alapú licencelés engedélyezése rendszergazdák számára, hogy egyszerre több licenceket rendelhet egy felhasználói csoporton, mint az őket egyenként.</span><span class="sxs-lookup"><span data-stu-id="9a838-123">Azure AD now supports group-based licensing allowing administrators to assign licenses in bulk to a group of users, rather than assigning them one at a time.</span></span> [<span data-ttu-id="9a838-124">Rendelje hozzá, győződjön meg arról, és a licencek kapcsolatos problémák megoldásához</span><span class="sxs-lookup"><span data-stu-id="9a838-124">Assign, verify, and resolve problems with licenses</span></span>](active-directory-licensing-group-assignment-azure-portal.md#step-1-assign-the-required-licenses)

<span data-ttu-id="9a838-125">Egyes Microsoft-szolgáltatások nem érhetők el az összes helyen.</span><span class="sxs-lookup"><span data-stu-id="9a838-125">Some Microsoft services are not available in all locations.</span></span> <span data-ttu-id="9a838-126">Licenc rendelhet egy felhasználót, mielőtt a rendszergazda a felhasználó a "Használati hely" tulajdonság adjon meg.</span><span class="sxs-lookup"><span data-stu-id="9a838-126">Before a license can be assigned to a user, the administrator must specify the “Usage location” property on the user.</span></span> <span data-ttu-id="9a838-127">Licenc hozzárendelése felhasználói végezhető > Profil > Beállítások szakaszban az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="9a838-127">Assignment of licenses can be done under User > Profile > Settings section in the Azure portal.</span></span> <span data-ttu-id="9a838-128">**Licenc-hozzárendelést használ, anélkül, hogy a megadott felhasználási hely összes felhasználója örökli a könyvtár helye.**</span><span class="sxs-lookup"><span data-stu-id="9a838-128">**When using group license assignment, any users without a usage location specified inherit the location of the directory.**</span></span>

## <a name="next-steps"></a><span data-ttu-id="9a838-129">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9a838-129">Next steps</span></span>

<span data-ttu-id="9a838-130">Az alábbi hivatkozásokat követve az Azure AD jelszóátállításáról olvashat további információkat.</span><span class="sxs-lookup"><span data-stu-id="9a838-130">The following links provide additional information regarding password reset using Azure AD</span></span>

* <span data-ttu-id="9a838-131">[**Gyors üzembe helyezés**](active-directory-passwords-getting-started.md) – Percek alatt üzembe helyezheti az Azure AD önkiszolgáló jelszókezelőjét.</span><span class="sxs-lookup"><span data-stu-id="9a838-131">[**Quick Start**](active-directory-passwords-getting-started.md) - Get up and running with Azure AD self service password management</span></span> 
* <span data-ttu-id="9a838-132">[**Adatok**](active-directory-passwords-data.md) – A szükséges adatok megismerése, és az adatok használata a rendszer jelszókezelésre.</span><span class="sxs-lookup"><span data-stu-id="9a838-132">[**Data**](active-directory-passwords-data.md) - Understand the data that is required and how it is used for password management</span></span>
* <span data-ttu-id="9a838-133">[**Bevezetés**](active-directory-passwords-best-practices.md) – Az SSPR funkció tervezése és üzembe helyezése a felhasználók számára az itt található útmutató segítségével.</span><span class="sxs-lookup"><span data-stu-id="9a838-133">[**Rollout**](active-directory-passwords-best-practices.md) - Plan and deploy SSPR to your users using the guidance found here</span></span>
* <span data-ttu-id="9a838-134">[**Testreszabás**](active-directory-passwords-customize.md) – Az SSPR-felület megjelenésének és működésének testre szabása a cége számára.</span><span class="sxs-lookup"><span data-stu-id="9a838-134">[**Customize**](active-directory-passwords-customize.md) - Customize the look and feel of the SSPR experience for your company.</span></span>
* <span data-ttu-id="9a838-135">[**Jelentéskészítés**](active-directory-passwords-reporting.md) – Megtudhatja, mikor és hol érik el a felhasználói az SSPR funkcióit.</span><span class="sxs-lookup"><span data-stu-id="9a838-135">[**Reporting**](active-directory-passwords-reporting.md) - Discover if, when, and where your users are accessing SSPR functionality</span></span>
* <span data-ttu-id="9a838-136">[**Részletes műszaki bemutatás**](active-directory-passwords-how-it-works.md) – Egy pillantás a függöny mögé, hogy megértse, hogyan is működik.</span><span class="sxs-lookup"><span data-stu-id="9a838-136">[**Technical Deep Dive**](active-directory-passwords-how-it-works.md) - Go behind the curtain to understand how it works</span></span>
* <span data-ttu-id="9a838-137">[**Gyakori kérdések**](active-directory-passwords-faq.md) – Hogyan?</span><span class="sxs-lookup"><span data-stu-id="9a838-137">[**Frequently Asked Questions**](active-directory-passwords-faq.md) - How?</span></span> <span data-ttu-id="9a838-138">Hogy miért?</span><span class="sxs-lookup"><span data-stu-id="9a838-138">Why?</span></span> <span data-ttu-id="9a838-139">Mi?</span><span class="sxs-lookup"><span data-stu-id="9a838-139">What?</span></span> <span data-ttu-id="9a838-140">Hová?</span><span class="sxs-lookup"><span data-stu-id="9a838-140">Where?</span></span> <span data-ttu-id="9a838-141">Ki?</span><span class="sxs-lookup"><span data-stu-id="9a838-141">Who?</span></span> <span data-ttu-id="9a838-142">Mikor?</span><span class="sxs-lookup"><span data-stu-id="9a838-142">When?</span></span> <span data-ttu-id="9a838-143">– Válaszok olyan kérdésekre, amiket mindig is fel akart tenni</span><span class="sxs-lookup"><span data-stu-id="9a838-143">- Answers to questions you always wanted to ask</span></span>
* <span data-ttu-id="9a838-144">[**Hibaelhárítás**](active-directory-passwords-troubleshoot.md) – Ismerje meg, hogyan oldhat meg általános, az SSPR működése során jelentkező hibákat.</span><span class="sxs-lookup"><span data-stu-id="9a838-144">[**Troubleshoot**](active-directory-passwords-troubleshoot.md) - Learn how to resolve common issues that we see with SSPR</span></span>

