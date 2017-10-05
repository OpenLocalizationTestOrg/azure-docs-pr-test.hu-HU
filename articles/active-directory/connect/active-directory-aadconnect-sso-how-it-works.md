---
title: "Az Azure AD Connect: Zökkenőmentes egyszeri bejelentkezés – hogyan működik |} Microsoft Docs"
description: "Ez a cikk ismerteti az Azure Active Directory zökkenőmentes egyszeri bejelentkezés funkció működése."
services: active-directory
keywords: "Mi az Azure AD Connect telepítés Active Directory szükséges összetevőket az Azure AD, SSO, egyszeri bejelentkezést."
documentationcenter: 
author: swkrish
manager: femila
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: billmath
ms.openlocfilehash: f0bcbdb03fbb70ff91ac3a56974a88eb1b26c245
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="azure-active-directory-seamless-single-sign-on-technical-deep-dive"></a><span data-ttu-id="06c39-104">Az Azure Active Directory zökkenőmentes egyszeri bejelentkezés: technikai részletes bemutatója</span><span class="sxs-lookup"><span data-stu-id="06c39-104">Azure Active Directory Seamless Single Sign-On: Technical deep dive</span></span>

<span data-ttu-id="06c39-105">Ez a cikk lehetőséget ad olyan technikai részleteket be az Azure Active Directory zökkenőmentes egyszeri bejelentkezést (zökkenőmentes SSO) funkció működése.</span><span class="sxs-lookup"><span data-stu-id="06c39-105">This article gives you technical details into how the Azure Active Directory Seamless Single Sign-On (Seamless SSO) feature works.</span></span>

>[!IMPORTANT]
><span data-ttu-id="06c39-106">A zökkenőmentes SSO-funkció jelenleg előzetes verzió.</span><span class="sxs-lookup"><span data-stu-id="06c39-106">The Seamless SSO feature is currently in preview.</span></span>

## <a name="how-does-seamless-sso-work"></a><span data-ttu-id="06c39-107">Hogyan működik a zökkenőmentes SSO?</span><span class="sxs-lookup"><span data-stu-id="06c39-107">How does Seamless SSO work?</span></span>

<span data-ttu-id="06c39-108">Ez a szakasz két részből áll rá:</span><span class="sxs-lookup"><span data-stu-id="06c39-108">This section has two parts to it:</span></span>
1. <span data-ttu-id="06c39-109">A telepítő a zökkenőmentes SSO-funkció.</span><span class="sxs-lookup"><span data-stu-id="06c39-109">The setup of the Seamless SSO feature.</span></span>
2. <span data-ttu-id="06c39-110">Egyetlen felhasználó bejelentkezési tranzakcióban működése a zökkenőmentes egyszeri Bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="06c39-110">How a single user sign-in transaction works with Seamless SSO.</span></span>

### <a name="how-does-set-up-work"></a><span data-ttu-id="06c39-111">Hogyan állítsa be a munkahelyi?</span><span class="sxs-lookup"><span data-stu-id="06c39-111">How does set up work?</span></span>

<span data-ttu-id="06c39-112">Zökkenőmentes SSO engedélyezve van az Azure AD Connect használatával, ahogy [Itt](active-directory-aadconnect-sso-quick-start.md).</span><span class="sxs-lookup"><span data-stu-id="06c39-112">Seamless SSO is enabled using Azure AD Connect as shown [here](active-directory-aadconnect-sso-quick-start.md).</span></span> <span data-ttu-id="06c39-113">Engedélyezze a szolgáltatást, a következő lépések következnek:</span><span class="sxs-lookup"><span data-stu-id="06c39-113">While enabling the feature, the following steps occur:</span></span>
- <span data-ttu-id="06c39-114">Nevű számítógépfiók `AZUREADSSOACCT` (amely jelöli az Azure AD) jön létre a helyszíni Active Directory (AD).</span><span class="sxs-lookup"><span data-stu-id="06c39-114">A computer account named `AZUREADSSOACCT` (which represents Azure AD) is created in your on-premises Active Directory (AD).</span></span>
- <span data-ttu-id="06c39-115">A számítógépfiók Kerberos visszafejtési kulcs biztonságosan megosztott Azure AD-val.</span><span class="sxs-lookup"><span data-stu-id="06c39-115">The computer account's Kerberos decryption key is shared securely with Azure AD.</span></span>
- <span data-ttu-id="06c39-116">Továbbá két Kerberos egyszerű szolgáltatásnév (SPN) két URL-címek az Azure AD-bejelentkezés során használt képviselő jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="06c39-116">In addition, two Kerberos service principal names (SPNs) are created to represent two URLs that are used during Azure AD sign-in.</span></span>

>[!NOTE]
> <span data-ttu-id="06c39-117">A számítógép fiókjának és a Kerberos SPN-ek minden Active Directory-erdőben szinkronizálja az Azure AD (Azure AD Connect használatával) és, amelynek felhasználói számára zökkenőmentes SSO kívánt jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="06c39-117">The computer account and the Kerberos SPNs are created in each AD forest you synchronize to Azure AD (using Azure AD Connect) and for whose users you want Seamless SSO.</span></span> <span data-ttu-id="06c39-118">Helyezze át a `AZUREADSSOACCT` számítógépfiókja számára egy szervezeti egységet (OU) más számítógépfiókokat tároló annak biztosítására, hogy ugyanúgy kezeli, és nem törlődik.</span><span class="sxs-lookup"><span data-stu-id="06c39-118">Move the `AZUREADSSOACCT` computer account to an Organization Unit (OU) where other computer accounts are stored to ensure that it is managed in the same way and is not deleted.</span></span>

>[!IMPORTANT]
><span data-ttu-id="06c39-119">Erősen ajánlott, hogy Ön [a Kerberos visszafejtési kulcs váltása](active-directory-aadconnect-sso-faq.md#how-can-i-roll-over-the-kerberos-decryption-key-of-the-azureadssoacct-computer-account) , a `AZUREADSSOACCT` számítógépfiók legalább 30 nap.</span><span class="sxs-lookup"><span data-stu-id="06c39-119">We highly recommend that you [roll over the Kerberos decryption key](active-directory-aadconnect-sso-faq.md#how-can-i-roll-over-the-kerberos-decryption-key-of-the-azureadssoacct-computer-account) of the `AZUREADSSOACCT` computer account at least every 30 days.</span></span>

### <a name="how-does-sign-in-with-seamless-sso-work"></a><span data-ttu-id="06c39-120">Hogyan jelentkezzen be munkahelyi zökkenőmentes SSO?</span><span class="sxs-lookup"><span data-stu-id="06c39-120">How does sign-in with Seamless SSO work?</span></span>

<span data-ttu-id="06c39-121">Ha a telepítés befejeződött, a zökkenőmentes SSO működik ugyanúgy, mint bármely más bejelentkezés integrált Windows-hitelesítéssel (IWA) használó.</span><span class="sxs-lookup"><span data-stu-id="06c39-121">Once the set-up is complete, Seamless SSO works the same way as any other sign-in that uses Integrated Windows Authentication (IWA).</span></span> <span data-ttu-id="06c39-122">A folyamat a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="06c39-122">The flow is as follows:</span></span>

1. <span data-ttu-id="06c39-123">A felhasználó megpróbál hozzáférni az alkalmazáshoz (például az Outlook Web App - https://outlook.office365.com/owa/) a tartományhoz csatlakoztatott vállalati eszköz a vállalati hálózaton belül.</span><span class="sxs-lookup"><span data-stu-id="06c39-123">The user tries to access an application (for example, the Outlook Web App - https://outlook.office365.com/owa/) from a domain-joined corporate device inside your corporate network.</span></span>
2. <span data-ttu-id="06c39-124">Ha a felhasználó nem jelentkezett, a felhasználót a rendszer átirányítja az Azure AD bejelentkezési oldalára.</span><span class="sxs-lookup"><span data-stu-id="06c39-124">If the user is not already signed in, the user is redirected to the Azure AD sign-in page.</span></span>

  >[!NOTE]
  ><span data-ttu-id="06c39-125">Ha az Azure AD bejelentkezési kérelemben egy `domain_hint` (azonosító a bérlő – például, contoso.onmicrosoft.com) vagy `login_hint` (például a felhasználó - azonosító user@contoso.onmicrosoft.com vagy user@contoso.com) paramétert, majd a 2. lépés kimarad.</span><span class="sxs-lookup"><span data-stu-id="06c39-125">If the Azure AD sign-in request includes a `domain_hint` (identifying your tenant- for example, contoso.onmicrosoft.com) or `login_hint` (identifying the user - for example, user@contoso.onmicrosoft.com or user@contoso.com) parameter, then step 2 is skipped.</span></span>

3. <span data-ttu-id="06c39-126">A felhasználó beírja az Azure AD bejelentkezési oldal a felhasználó nevét.</span><span class="sxs-lookup"><span data-stu-id="06c39-126">The user types in their user name into the Azure AD sign-in page.</span></span>
4. <span data-ttu-id="06c39-127">A JavaScript használatával a háttérben, az Azure AD felkéri a böngésző keresztül 401-es jogosulatlan választ, adja meg a Kerberos jegyet.</span><span class="sxs-lookup"><span data-stu-id="06c39-127">Using JavaScript in the background, Azure AD challenges the browser, via a 401 Unauthorized response, to provide a Kerberos ticket.</span></span>
5. <span data-ttu-id="06c39-128">A böngésző jegyet, az Active Directory kér a `AZUREADSSOACCT` számítógépfiók (amely az Azure AD).</span><span class="sxs-lookup"><span data-stu-id="06c39-128">The browser, in turn, requests a ticket from Active Directory for the `AZUREADSSOACCT` computer account (which represents Azure AD).</span></span>
6. <span data-ttu-id="06c39-129">Az Active Directory megkeresi a számítógépfiók, és visszaadja a Kerberos jegyet a böngészőben a számítógépfiók titkos kulcs titkosított.</span><span class="sxs-lookup"><span data-stu-id="06c39-129">Active Directory locates the computer account and returns a Kerberos ticket to the browser encrypted with the computer account's secret.</span></span>
7. <span data-ttu-id="06c39-130">A böngésző továbbítja a Kerberos-jegy azért szerzett, az Active Directoryból az Azure AD (az egyik a [korábban hozzáadott, a böngésző Intranet zóna beállításait az Azure AD URL](active-directory-aadconnect-sso-quick-start.md#step-3-roll-out-the-feature)).</span><span class="sxs-lookup"><span data-stu-id="06c39-130">The browser forwards the Kerberos ticket it acquired from Active Directory to Azure AD (on one of the [Azure AD URLs previously added to the browser's Intranet zone settings](active-directory-aadconnect-sso-quick-start.md#step-3-roll-out-the-feature)).</span></span>
8. <span data-ttu-id="06c39-131">Az Azure AD visszafejti a Kerberos jegy, amely tartalmazza a felhasználó a vállalati eszköz be van jelentkezve a korábban megosztott kulcs használatával.</span><span class="sxs-lookup"><span data-stu-id="06c39-131">Azure AD decrypts the Kerberos ticket, which includes the identity of the user signed into the corporate device, using the previously shared key.</span></span>
9. <span data-ttu-id="06c39-132">Kiértékelésével, olyan után az Azure AD vagy egy token visszaadja az alkalmazásnak vagy megkérdezi a felhasználót a kiegészítő igazolás, például a többtényezős hitelesítés végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="06c39-132">After evaluation, Azure AD either returns a token back to the application or asks the user to perform additional proofs, such as Multi-Factor Authentication.</span></span>
10. <span data-ttu-id="06c39-133">Ha a felhasználói bejelentkezés sikeres, a felhasználó nem tud hozzáférni az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="06c39-133">If the user sign-in is successful, the user is able to access the application.</span></span>

<span data-ttu-id="06c39-134">A következő ábra összetevőit és a lépéseit mutatja be.</span><span class="sxs-lookup"><span data-stu-id="06c39-134">The following diagram illustrates all the components and the steps involved.</span></span>

![Zökkenőmentes egyszeri bejelentkezés](./media/active-directory-aadconnect-sso/sso2.png)

<span data-ttu-id="06c39-136">Zökkenőmentes SSO alkalmi, ami azt jelenti, ha nem sikerül, a bejelentkezés során tapasztal élmény visszaáll a rendszeres viselkedését - Egytényezős, a felhasználó igényeinek meg kell adnia a jelszót a bejelentkezéshez.</span><span class="sxs-lookup"><span data-stu-id="06c39-136">Seamless SSO is opportunistic, which means if it fails, the sign-in experience falls back to its regular behavior - i.e, the user needs to enter their password to sign in.</span></span>

## <a name="next-steps"></a><span data-ttu-id="06c39-137">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="06c39-137">Next steps</span></span>

- <span data-ttu-id="06c39-138">[**Gyors üzembe helyezési** ](active-directory-aadconnect-sso-quick-start.md) - létrehozásához, és az Azure AD zökkenőmentes SSO futtatása.</span><span class="sxs-lookup"><span data-stu-id="06c39-138">[**Quick Start**](active-directory-aadconnect-sso-quick-start.md) - Get up and running Azure AD Seamless SSO.</span></span>
- <span data-ttu-id="06c39-139">[**Gyakran ismételt kérdések** ](active-directory-aadconnect-sso-faq.md) -gyakran feltett kérdésekre adott válaszokat.</span><span class="sxs-lookup"><span data-stu-id="06c39-139">[**Frequently Asked Questions**](active-directory-aadconnect-sso-faq.md) - Answers to frequently asked questions.</span></span>
- <span data-ttu-id="06c39-140">[**Hibaelhárítás** ](active-directory-aadconnect-troubleshoot-sso.md) -Útmutató: a szolgáltatással kapcsolatos gyakori problémák megoldása.</span><span class="sxs-lookup"><span data-stu-id="06c39-140">[**Troubleshoot**](active-directory-aadconnect-troubleshoot-sso.md) - Learn how to resolve common issues with the feature.</span></span>
- <span data-ttu-id="06c39-141">[**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) – új funkciókérések tárolásához.</span><span class="sxs-lookup"><span data-stu-id="06c39-141">[**UserVoice**](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) - For filing new feature requests.</span></span>
