---
title: "Az Azure AD Connect: Zökkenőmentes egyszeri bejelentkezés – hogyan működik |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan hello Azure Active Directory zökkenőmentes egyszeri bejelentkezést szolgáltatás működik."
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
ms.openlocfilehash: 17ce35b32832d241068ab878cf7aac42deab74ef
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-seamless-single-sign-on-technical-deep-dive"></a><span data-ttu-id="da43e-104">Az Azure Active Directory zökkenőmentes egyszeri bejelentkezés: technikai részletes bemutatója</span><span class="sxs-lookup"><span data-stu-id="da43e-104">Azure Active Directory Seamless Single Sign-On: Technical deep dive</span></span>

<span data-ttu-id="da43e-105">Ez a cikk lehetővé teszi az technikai részleteket a hello Azure Active Directory zökkenőmentes egyszeri bejelentkezést (zökkenőmentes SSO) funkció működése.</span><span class="sxs-lookup"><span data-stu-id="da43e-105">This article gives you technical details into how hello Azure Active Directory Seamless Single Sign-On (Seamless SSO) feature works.</span></span>

>[!IMPORTANT]
><span data-ttu-id="da43e-106">hello zökkenőmentes SSO funkció jelenleg előzetes verzió.</span><span class="sxs-lookup"><span data-stu-id="da43e-106">hello Seamless SSO feature is currently in preview.</span></span>

## <a name="how-does-seamless-sso-work"></a><span data-ttu-id="da43e-107">Hogyan működik a zökkenőmentes SSO?</span><span class="sxs-lookup"><span data-stu-id="da43e-107">How does Seamless SSO work?</span></span>

<span data-ttu-id="da43e-108">Ez a szakasz két részből tooit rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="da43e-108">This section has two parts tooit:</span></span>
1. <span data-ttu-id="da43e-109">hello zökkenőmentes SSO szolgáltatás hello beállítása.</span><span class="sxs-lookup"><span data-stu-id="da43e-109">hello setup of hello Seamless SSO feature.</span></span>
2. <span data-ttu-id="da43e-110">Egyetlen felhasználó bejelentkezési tranzakcióban működése a zökkenőmentes egyszeri Bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="da43e-110">How a single user sign-in transaction works with Seamless SSO.</span></span>

### <a name="how-does-set-up-work"></a><span data-ttu-id="da43e-111">Hogyan állítsa be a munkahelyi?</span><span class="sxs-lookup"><span data-stu-id="da43e-111">How does set up work?</span></span>

<span data-ttu-id="da43e-112">Zökkenőmentes SSO engedélyezve van az Azure AD Connect használatával, ahogy [Itt](active-directory-aadconnect-sso-quick-start.md).</span><span class="sxs-lookup"><span data-stu-id="da43e-112">Seamless SSO is enabled using Azure AD Connect as shown [here](active-directory-aadconnect-sso-quick-start.md).</span></span> <span data-ttu-id="da43e-113">Hello funkció engedélyezése, miközben a lépéseket követve hello fordulhat elő:</span><span class="sxs-lookup"><span data-stu-id="da43e-113">While enabling hello feature, hello following steps occur:</span></span>
- <span data-ttu-id="da43e-114">Nevű számítógépfiók `AZUREADSSOACCT` (amely jelöli az Azure AD) jön létre a helyszíni Active Directory (AD).</span><span class="sxs-lookup"><span data-stu-id="da43e-114">A computer account named `AZUREADSSOACCT` (which represents Azure AD) is created in your on-premises Active Directory (AD).</span></span>
- <span data-ttu-id="da43e-115">hello számítógépfiók Kerberos visszafejtési kulcs biztonságosan megosztott Azure AD-val.</span><span class="sxs-lookup"><span data-stu-id="da43e-115">hello computer account's Kerberos decryption key is shared securely with Azure AD.</span></span>
- <span data-ttu-id="da43e-116">Továbbá a két Kerberos egyszerű szolgáltatásnév (SPN) toorepresent két URL-címek az Azure AD-bejelentkezés során használt jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="da43e-116">In addition, two Kerberos service principal names (SPNs) are created toorepresent two URLs that are used during Azure AD sign-in.</span></span>

>[!NOTE]
> <span data-ttu-id="da43e-117">minden Active Directory-erdőben hello számítógép fiókjának és hello Kerberos egyszerű szolgáltatásnevek jönnek létre tooAzure AD szinkronizálása (Azure AD Connect használatával) és zökkenőmentes SSO szeretne, amelynek felhasználói számára.</span><span class="sxs-lookup"><span data-stu-id="da43e-117">hello computer account and hello Kerberos SPNs are created in each AD forest you synchronize tooAzure AD (using Azure AD Connect) and for whose users you want Seamless SSO.</span></span> <span data-ttu-id="da43e-118">Helyezze át a hello `AZUREADSSOACCT` számítógép fiók tooan szervezeti egységet (OU), amely a felügyelt tárolt tooensure más számítógépes fiókok esetén hello azonos módon, és nem törlődik.</span><span class="sxs-lookup"><span data-stu-id="da43e-118">Move hello `AZUREADSSOACCT` computer account tooan Organization Unit (OU) where other computer accounts are stored tooensure that it is managed in hello same way and is not deleted.</span></span>

>[!IMPORTANT]
><span data-ttu-id="da43e-119">Erősen ajánlott, hogy Ön [hello Kerberos visszafejtési kulcs váltása](active-directory-aadconnect-sso-faq.md#how-can-i-roll-over-the-kerberos-decryption-key-of-the-azureadssoacct-computer-account) a hello `AZUREADSSOACCT` számítógépfiók legalább 30 nap.</span><span class="sxs-lookup"><span data-stu-id="da43e-119">We highly recommend that you [roll over hello Kerberos decryption key](active-directory-aadconnect-sso-faq.md#how-can-i-roll-over-the-kerberos-decryption-key-of-the-azureadssoacct-computer-account) of hello `AZUREADSSOACCT` computer account at least every 30 days.</span></span>

### <a name="how-does-sign-in-with-seamless-sso-work"></a><span data-ttu-id="da43e-120">Hogyan jelentkezzen be munkahelyi zökkenőmentes SSO?</span><span class="sxs-lookup"><span data-stu-id="da43e-120">How does sign-in with Seamless SSO work?</span></span>

<span data-ttu-id="da43e-121">Hello telepítés végrehajtása után a zökkenőmentes SSO hello ugyanaz, mint bármilyen más módon bejelentkezés integrált Windows-hitelesítéssel (IWA) használó működik.</span><span class="sxs-lookup"><span data-stu-id="da43e-121">Once hello set-up is complete, Seamless SSO works hello same way as any other sign-in that uses Integrated Windows Authentication (IWA).</span></span> <span data-ttu-id="da43e-122">hello folyamat a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="da43e-122">hello flow is as follows:</span></span>

1. <span data-ttu-id="da43e-123">hello felhasználó megpróbál tooaccess egy alkalmazás (például az Outlook Web App - hello https://outlook.office365.com/owa/) a tartományhoz csatlakoztatott vállalati eszköz a vállalati hálózaton belül.</span><span class="sxs-lookup"><span data-stu-id="da43e-123">hello user tries tooaccess an application (for example, hello Outlook Web App - https://outlook.office365.com/owa/) from a domain-joined corporate device inside your corporate network.</span></span>
2. <span data-ttu-id="da43e-124">Hello felhasználó már nem jelentkezett be, ha a hello felhasználó átirányított toohello az Azure AD bejelentkezési oldalára.</span><span class="sxs-lookup"><span data-stu-id="da43e-124">If hello user is not already signed in, hello user is redirected toohello Azure AD sign-in page.</span></span>

  >[!NOTE]
  ><span data-ttu-id="da43e-125">Ha hello Azure AD-bejelentkezés kérelemben egy `domain_hint` (azonosító a bérlő – például, contoso.onmicrosoft.com) vagy `login_hint` (hello felhasználó – például azonosító user@contoso.onmicrosoft.com vagy user@contoso.com) paramétert, majd a 2. lépés kimarad.</span><span class="sxs-lookup"><span data-stu-id="da43e-125">If hello Azure AD sign-in request includes a `domain_hint` (identifying your tenant- for example, contoso.onmicrosoft.com) or `login_hint` (identifying hello user - for example, user@contoso.onmicrosoft.com or user@contoso.com) parameter, then step 2 is skipped.</span></span>

3. <span data-ttu-id="da43e-126">hello felhasználótípusokat hello Azure AD bejelentkezési oldal a felhasználó nevét.</span><span class="sxs-lookup"><span data-stu-id="da43e-126">hello user types in their user name into hello Azure AD sign-in page.</span></span>
4. <span data-ttu-id="da43e-127">A JavaScript használatával hello háttérben, az Azure AD felkéri hello böngésző 401-es jogosulatlan választ, tooprovide keresztül a Kerberos jegyet.</span><span class="sxs-lookup"><span data-stu-id="da43e-127">Using JavaScript in hello background, Azure AD challenges hello browser, via a 401 Unauthorized response, tooprovide a Kerberos ticket.</span></span>
5. <span data-ttu-id="da43e-128">hello böngésző, viszont a jegyet az Active Directoryból vonatkozó kérések hello `AZUREADSSOACCT` számítógépfiók (amely az Azure AD).</span><span class="sxs-lookup"><span data-stu-id="da43e-128">hello browser, in turn, requests a ticket from Active Directory for hello `AZUREADSSOACCT` computer account (which represents Azure AD).</span></span>
6. <span data-ttu-id="da43e-129">Az Active Directory hello számítógépfiók megkeresi, és adja vissza egy Kerberos jegy toohello böngésző hello számítógépfiók titkos kulcs titkosított.</span><span class="sxs-lookup"><span data-stu-id="da43e-129">Active Directory locates hello computer account and returns a Kerberos ticket toohello browser encrypted with hello computer account's secret.</span></span>
7. <span data-ttu-id="da43e-130">hello böngésző továbbítja azért szerzett, az Active Directory tooAzure AD hello Kerberos-jegy (hello egyik [az Azure AD URL-címek korábban hozzáadott toohello böngésző Intranet zóna beállításai](active-directory-aadconnect-sso-quick-start.md#step-3-roll-out-the-feature)).</span><span class="sxs-lookup"><span data-stu-id="da43e-130">hello browser forwards hello Kerberos ticket it acquired from Active Directory tooAzure AD (on one of hello [Azure AD URLs previously added toohello browser's Intranet zone settings](active-directory-aadconnect-sso-quick-start.md#step-3-roll-out-the-feature)).</span></span>
8. <span data-ttu-id="da43e-131">Az Azure AD visszafejtése hello Kerberos-jegyet szereznek, magában foglalja a hello felhasználó identitásával az hello hello vállalati eszköz be van jelentkezve, hello használatával korábban megosztott kulcs.</span><span class="sxs-lookup"><span data-stu-id="da43e-131">Azure AD decrypts hello Kerberos ticket, which includes hello identity of hello user signed into hello corporate device, using hello previously shared key.</span></span>
9. <span data-ttu-id="da43e-132">Kiértékelésével, olyan után az Azure AD adja vissza egy token hátsó toohello alkalmazást vagy hello felhasználói tooperform további igazolást, például a multi-factor Authentication kéri.</span><span class="sxs-lookup"><span data-stu-id="da43e-132">After evaluation, Azure AD either returns a token back toohello application or asks hello user tooperform additional proofs, such as Multi-Factor Authentication.</span></span>
10. <span data-ttu-id="da43e-133">Hello felhasználói bejelentkezés sikeres, hello felhasználói akkor tudja tooaccess hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="da43e-133">If hello user sign-in is successful, hello user is able tooaccess hello application.</span></span>

<span data-ttu-id="da43e-134">hello alábbi ábrán látható összes hello összetevők és hello lépéseit.</span><span class="sxs-lookup"><span data-stu-id="da43e-134">hello following diagram illustrates all hello components and hello steps involved.</span></span>

![Zökkenőmentes egyszeri bejelentkezés](./media/active-directory-aadconnect-sso/sso2.png)

<span data-ttu-id="da43e-136">Zökkenőmentes SSO alkalmi, ami azt jelenti, ha a hiba, hello bejelentkezés során tapasztal élmény visszavált tooits rendszeres viselkedés - Egytényezős, hello felhasználó számára szükséges tooenter a jelszó toosign a.</span><span class="sxs-lookup"><span data-stu-id="da43e-136">Seamless SSO is opportunistic, which means if it fails, hello sign-in experience falls back tooits regular behavior - i.e, hello user needs tooenter their password toosign in.</span></span>

## <a name="next-steps"></a><span data-ttu-id="da43e-137">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="da43e-137">Next steps</span></span>

- <span data-ttu-id="da43e-138">[**Gyors üzembe helyezési** ](active-directory-aadconnect-sso-quick-start.md) - létrehozásához, és az Azure AD zökkenőmentes SSO futtatása.</span><span class="sxs-lookup"><span data-stu-id="da43e-138">[**Quick Start**](active-directory-aadconnect-sso-quick-start.md) - Get up and running Azure AD Seamless SSO.</span></span>
- <span data-ttu-id="da43e-139">[**Gyakran ismételt kérdések** ](active-directory-aadconnect-sso-faq.md) -kérdések toofrequently válaszol.</span><span class="sxs-lookup"><span data-stu-id="da43e-139">[**Frequently Asked Questions**](active-directory-aadconnect-sso-faq.md) - Answers toofrequently asked questions.</span></span>
- <span data-ttu-id="da43e-140">[**Hibaelhárítás** ](active-directory-aadconnect-troubleshoot-sso.md) -megtudhatja, hogyan tooresolve közös hello szolgáltatást érintő problémákat.</span><span class="sxs-lookup"><span data-stu-id="da43e-140">[**Troubleshoot**](active-directory-aadconnect-troubleshoot-sso.md) - Learn how tooresolve common issues with hello feature.</span></span>
- <span data-ttu-id="da43e-141">[**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) – új funkciókérések tárolásához.</span><span class="sxs-lookup"><span data-stu-id="da43e-141">[**UserVoice**](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) - For filing new feature requests.</span></span>
