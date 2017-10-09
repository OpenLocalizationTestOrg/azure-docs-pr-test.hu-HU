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
ms.openlocfilehash: 9cecaaac429165346f7082f1965dc8a21063fe7a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="licensing-requirements-for-azure-ad-self-service-password-reset"></a><span data-ttu-id="9b5dc-103">Licencelési követelmények az Azure AD az önkiszolgáló jelszó alaphelyzetbe állítása</span><span class="sxs-lookup"><span data-stu-id="9b5dc-103">Licensing requirements for Azure AD self-service password reset</span></span>

<span data-ttu-id="9b5dc-104">Ahhoz, hogy az Azure AD jelszó-átállítási toofunction hogy **rendelkeznie kell legalább egy licenc nélküli a szervezet**.</span><span class="sxs-lookup"><span data-stu-id="9b5dc-104">In order for Azure AD Password Reset toofunction, you **must have at least one license assigned in your organization**.</span></span> <span data-ttu-id="9b5dc-105">Nem határoztunk felhasználónkénti hello jelszó alaphelyzetbe állítása során szerzett licencelési.</span><span class="sxs-lookup"><span data-stu-id="9b5dc-105">We do not enforce per-user licensing on hello password reset experience.</span></span> <span data-ttu-id="9b5dc-106">a Microsoft-licencszerződés toomaintain betartását, tooassign licencek tooany felhasználók prémium szolgáltatások használatáért kell.</span><span class="sxs-lookup"><span data-stu-id="9b5dc-106">toomaintain compliance with your Microsoft licensing agreement, you need tooassign licenses tooany users that use premium features.</span></span>

* <span data-ttu-id="9b5dc-107">**Csak a felhasználók felhőalapú** -Office 365 (Office 365) bármely fizetett SKU vagy Azure AD alapvető</span><span class="sxs-lookup"><span data-stu-id="9b5dc-107">**Cloud only users** - Office 365 (O365) any paid SKU, or Azure AD Basic</span></span>
* <span data-ttu-id="9b5dc-108">**Felhő** és/vagy **helyszíni felhasználók** -Azure AD Premium P1 vagy P2, Enterprise Mobility + Security (EMS) vagy biztonságos hatékony vállalati (Másodper)</span><span class="sxs-lookup"><span data-stu-id="9b5dc-108">**Cloud** and/or **on-premises users** - Azure AD Premium P1 or P2, Enterprise Mobility + Security (EMS), or Secure Productive Enterprise (SPE)</span></span>

## <a name="licenses-required-for-password-writeback"></a><span data-ttu-id="9b5dc-109">A jelszóvisszaírás szükséges engedélyek</span><span class="sxs-lookup"><span data-stu-id="9b5dc-109">Licenses required for password writeback</span></span>

<span data-ttu-id="9b5dc-110">toouse jelszóvisszaírás, rendelkeznie kell a következő bérlőben licenccel hello egyikét.</span><span class="sxs-lookup"><span data-stu-id="9b5dc-110">toouse password writeback, you must have one of hello following licenses assigned in your tenant.</span></span>

* <span data-ttu-id="9b5dc-111">Prémium szintű Azure AD P1</span><span class="sxs-lookup"><span data-stu-id="9b5dc-111">Azure AD Premium P1</span></span>
* <span data-ttu-id="9b5dc-112">Prémium szintű Azure AD P2</span><span class="sxs-lookup"><span data-stu-id="9b5dc-112">Azure AD Premium P2</span></span>
* <span data-ttu-id="9b5dc-113">Enterprise Mobility + Security E3</span><span class="sxs-lookup"><span data-stu-id="9b5dc-113">Enterprise Mobility + Security E3</span></span>
* <span data-ttu-id="9b5dc-114">Enterprise Mobility + Security E5</span><span class="sxs-lookup"><span data-stu-id="9b5dc-114">Enterprise Mobility + Security E5</span></span>
* <span data-ttu-id="9b5dc-115">Secure Productive Enterprise E3</span><span class="sxs-lookup"><span data-stu-id="9b5dc-115">Secure Productive Enterprise E3</span></span>
* <span data-ttu-id="9b5dc-116">Secure Productive Enterprise E5</span><span class="sxs-lookup"><span data-stu-id="9b5dc-116">Secure Productive Enterprise E5</span></span>

> [!NOTE]
> <span data-ttu-id="9b5dc-117">Önálló Office 365 tervek licencelési **nem támogatják a jelszóvisszaírás** és jelszó használata kötelezővé tehető a funkciók toowork tervei megelőző hello.</span><span class="sxs-lookup"><span data-stu-id="9b5dc-117">Standalone Office 365 licensing plans **do not support password writeback** and require one of hello preceding plans for this functionality toowork.</span></span>

<span data-ttu-id="9b5dc-118">További licencelési adatait, beleértve a költségek található meg a következő lapok hello</span><span class="sxs-lookup"><span data-stu-id="9b5dc-118">Additional licensing info including costs can be found on hello following pages</span></span>

* [<span data-ttu-id="9b5dc-119">Az Azure Active Directory árképzési hely</span><span class="sxs-lookup"><span data-stu-id="9b5dc-119">Azure Active Directory Pricing site</span></span>](https://azure.microsoft.com/pricing/details/active-directory/)
* [<span data-ttu-id="9b5dc-120">Enterprise Mobility + Security</span><span class="sxs-lookup"><span data-stu-id="9b5dc-120">Enterprise Mobility + Security</span></span>](https://www.microsoft.com/cloud-platform/enterprise-mobility-security)
* [<span data-ttu-id="9b5dc-121">Biztonságos hatékony Enterprise</span><span class="sxs-lookup"><span data-stu-id="9b5dc-121">Secure Productive Enterprise</span></span>](https://www.microsoft.com/secure-productive-enterprise/default.aspx)

## <a name="enable-group-or-user-based-licensing"></a><span data-ttu-id="9b5dc-122">Csoport vagy felhasználó alapú licencelés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="9b5dc-122">Enable group or user-based licensing</span></span>

<span data-ttu-id="9b5dc-123">Mostantól az Azure AD támogatja a csoport-alapú tömeges tooa csoport számára licencek, így a rendszergazdák tooassign licencelési, mint az őket egyenként.</span><span class="sxs-lookup"><span data-stu-id="9b5dc-123">Azure AD now supports group-based licensing allowing administrators tooassign licenses in bulk tooa group of users, rather than assigning them one at a time.</span></span> [<span data-ttu-id="9b5dc-124">Rendelje hozzá, győződjön meg arról, és a licencek kapcsolatos problémák megoldásához</span><span class="sxs-lookup"><span data-stu-id="9b5dc-124">Assign, verify, and resolve problems with licenses</span></span>](active-directory-licensing-group-assignment-azure-portal.md#step-1-assign-the-required-licenses)

<span data-ttu-id="9b5dc-125">Egyes Microsoft-szolgáltatások nem érhetők el az összes helyen.</span><span class="sxs-lookup"><span data-stu-id="9b5dc-125">Some Microsoft services are not available in all locations.</span></span> <span data-ttu-id="9b5dc-126">Licenc tooa felhasználói rendelhető, mielőtt hello rendszergazda hello felhasználói kell adnia hello "Használat helye" tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="9b5dc-126">Before a license can be assigned tooa user, hello administrator must specify hello “Usage location” property on hello user.</span></span> <span data-ttu-id="9b5dc-127">Licenc hozzárendelése felhasználói végezhető > Profil > Bővítménybeállítási szakasza hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="9b5dc-127">Assignment of licenses can be done under User > Profile > Settings section in hello Azure portal.</span></span> <span data-ttu-id="9b5dc-128">**Licenc-hozzárendelést használ, anélkül, hogy a megadott felhasználási hely összes felhasználója örökli hello könyvtár hello helye.**</span><span class="sxs-lookup"><span data-stu-id="9b5dc-128">**When using group license assignment, any users without a usage location specified inherit hello location of hello directory.**</span></span>

## <a name="next-steps"></a><span data-ttu-id="9b5dc-129">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9b5dc-129">Next steps</span></span>

<span data-ttu-id="9b5dc-130">a következő hivatkozások hello adja meg a jelszó alaphelyzetbe állítása, az Azure AD használatával kapcsolatos további információk</span><span class="sxs-lookup"><span data-stu-id="9b5dc-130">hello following links provide additional information regarding password reset using Azure AD</span></span>

* <span data-ttu-id="9b5dc-131">[**Gyors üzembe helyezés**](active-directory-passwords-getting-started.md) – Percek alatt üzembe helyezheti az Azure AD önkiszolgáló jelszókezelőjét.</span><span class="sxs-lookup"><span data-stu-id="9b5dc-131">[**Quick Start**](active-directory-passwords-getting-started.md) - Get up and running with Azure AD self service password management</span></span> 
* <span data-ttu-id="9b5dc-132">[**Adatok** ](active-directory-passwords-data.md) - szükséges hello adatok megismeréséhez, és hogyan használja fel azokat a jelszókezelés</span><span class="sxs-lookup"><span data-stu-id="9b5dc-132">[**Data**](active-directory-passwords-data.md) - Understand hello data that is required and how it is used for password management</span></span>
* <span data-ttu-id="9b5dc-133">[**Bevezetés** ](active-directory-passwords-best-practices.md) -megtervezése és telepítése az önkiszolgáló jelszó-Változtatási tooyour felhasználók hello útmutatást itt talál</span><span class="sxs-lookup"><span data-stu-id="9b5dc-133">[**Rollout**](active-directory-passwords-best-practices.md) - Plan and deploy SSPR tooyour users using hello guidance found here</span></span>
* <span data-ttu-id="9b5dc-134">[**Testre szabhatja** ](active-directory-passwords-customize.md) -testreszabása, önkiszolgáló jelszó-Változtatási élményt a vállalata hello hello megjelenését és működését.</span><span class="sxs-lookup"><span data-stu-id="9b5dc-134">[**Customize**](active-directory-passwords-customize.md) - Customize hello look and feel of hello SSPR experience for your company.</span></span>
* <span data-ttu-id="9b5dc-135">[**Jelentéskészítés**](active-directory-passwords-reporting.md) – Megtudhatja, mikor és hol érik el a felhasználói az SSPR funkcióit.</span><span class="sxs-lookup"><span data-stu-id="9b5dc-135">[**Reporting**](active-directory-passwords-reporting.md) - Discover if, when, and where your users are accessing SSPR functionality</span></span>
* <span data-ttu-id="9b5dc-136">[**Műszaki mélyreható** ](active-directory-passwords-how-it-works.md) -mögött hello függöny toounderstand nyissa meg annak működéséről</span><span class="sxs-lookup"><span data-stu-id="9b5dc-136">[**Technical Deep Dive**](active-directory-passwords-how-it-works.md) - Go behind hello curtain toounderstand how it works</span></span>
* <span data-ttu-id="9b5dc-137">[**Gyakori kérdések**](active-directory-passwords-faq.md) – Hogyan?</span><span class="sxs-lookup"><span data-stu-id="9b5dc-137">[**Frequently Asked Questions**](active-directory-passwords-faq.md) - How?</span></span> <span data-ttu-id="9b5dc-138">Hogy miért?</span><span class="sxs-lookup"><span data-stu-id="9b5dc-138">Why?</span></span> <span data-ttu-id="9b5dc-139">Mi?</span><span class="sxs-lookup"><span data-stu-id="9b5dc-139">What?</span></span> <span data-ttu-id="9b5dc-140">Hová?</span><span class="sxs-lookup"><span data-stu-id="9b5dc-140">Where?</span></span> <span data-ttu-id="9b5dc-141">Ki?</span><span class="sxs-lookup"><span data-stu-id="9b5dc-141">Who?</span></span> <span data-ttu-id="9b5dc-142">Mikor?</span><span class="sxs-lookup"><span data-stu-id="9b5dc-142">When?</span></span> <span data-ttu-id="9b5dc-143">-Válaszok mindig kívánta tooask tooquestions</span><span class="sxs-lookup"><span data-stu-id="9b5dc-143">- Answers tooquestions you always wanted tooask</span></span>
* <span data-ttu-id="9b5dc-144">[**Hibaelhárítás** ](active-directory-passwords-troubleshoot.md) -megtudhatja, hogyan tooresolve közös állít ki, hogy az önkiszolgáló jelszó-Változtatási látható</span><span class="sxs-lookup"><span data-stu-id="9b5dc-144">[**Troubleshoot**](active-directory-passwords-troubleshoot.md) - Learn how tooresolve common issues that we see with SSPR</span></span>

