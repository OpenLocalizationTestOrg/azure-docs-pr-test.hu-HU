---
title: "egyszeri bejelentkezés az Azure AD alkalmazásproxy aaaManage |} Microsoft Docs"
description: "További tudnivalók az egyszeri bejelentkezés az alkalmazásproxy hello alapjai"
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/23/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: a278751a5cb1bf98c970a4e5d2eb3edc3b784096
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-does-azure-ad-application-proxy-provide-single-sign-on"></a><span data-ttu-id="84d76-103">Hogyan nyújt az Azure AD-alkalmazásproxy egyszeri bejelentkezéshez?</span><span class="sxs-lookup"><span data-stu-id="84d76-103">How does Azure AD Application Proxy provide single sign-on?</span></span>

<span data-ttu-id="84d76-104">Egyszeri bejelentkezés az Azure AD alkalmazásproxy egyik fő eleme.</span><span class="sxs-lookup"><span data-stu-id="84d76-104">Single sign-on is a key element of Azure AD Application Proxy.</span></span>  <span data-ttu-id="84d76-105">Mert a felhasználók csak toosign tooAzure Active Directory-hello felhő hello legjobb felhasználói élményt biztosít.</span><span class="sxs-lookup"><span data-stu-id="84d76-105">It provides hello best user experience because your users only have toosign in tooAzure Active Directory in hello cloud.</span></span> <span data-ttu-id="84d76-106">Amennyiben az Active Directory tooAzure hitelesítenie, hello alkalmazásproxy-összekötő kezeli hello hitelesítési toohello a helyszíni alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="84d76-106">Once they authenticate tooAzure Active Directory, hello Application Proxy connector handles hello authentication toohello on-premises application.</span></span> <span data-ttu-id="84d76-107">a háttéralkalmazás hello nem sikerült megállapítani, jelentkezzen be a Proxy és a normál használata egy tartományhoz csatlakoztatott eszközön keresztül egy távoli felhasználó hello különbsége.</span><span class="sxs-lookup"><span data-stu-id="84d76-107">hello backend application can't tell hello difference between a remote user signing in through Application Proxy and a regular use on a domain-joined device.</span></span> 

<span data-ttu-id="84d76-108">toouse Azure Active Directoryval az egyszeri bejelentkezés tooyour alkalmazások, kell tooselect **Azure Active Directory** hello előhitelesítési módszer.</span><span class="sxs-lookup"><span data-stu-id="84d76-108">toouse Azure Active Directory for single sign-on tooyour applications, you need tooselect **Azure Active Directory** as hello pre-authentication method.</span></span> <span data-ttu-id="84d76-109">Ha **csatlakoztatott** ezt követően a felhasználók hitelesítéséhez tooAzure Active Directory egyáltalán nem, de irányított egyenes toohello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="84d76-109">If you select **Passthrough** then your users don't authenticate tooAzure Active Directory at all, but are directed straight toohello application.</span></span> <span data-ttu-id="84d76-110">E beállítás konfigurálása először közzétenni egy alkalmazást, vagy keresse meg a tooyour hello Azure-portál alkalmazást és hello alkalmazásproxy beállításainak szerkesztése.</span><span class="sxs-lookup"><span data-stu-id="84d76-110">You can configure this setting when you first publish an application, or navigate tooyour application in hello Azure portal and edit hello Application Proxy settings.</span></span> 

<span data-ttu-id="84d76-111">toosee az egyszeri bejelentkezésre vonatkozó beállításokat, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="84d76-111">toosee your single sign-on options, follow these steps:</span></span>

1. <span data-ttu-id="84d76-112">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="84d76-112">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="84d76-113">Keresse meg a túl**Azure Active Directory** > **vállalati alkalmazások** > **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="84d76-113">Navigate too**Azure Active Directory** > **Enterprise applications** > **All applications**.</span></span>
3. <span data-ttu-id="84d76-114">Jelölje be hello alkalmazás beállításai közül, amelynek egyszeri bejelentkezés toomanage szeretné.</span><span class="sxs-lookup"><span data-stu-id="84d76-114">Select hello app whose single sign-on options you want toomanage.</span></span>
4. <span data-ttu-id="84d76-115">Válassza ki **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="84d76-115">Select **Single sign-on**.</span></span>

   ![Egyszeri bejelentkezés legördülő menü](./media/application-proxy-sso-overview/single-sign-on-mode.png)

<span data-ttu-id="84d76-117">hello legördülő menü egyszeri bejelentkezés tooyour alkalmazás öt lehetőségeket mutatja be:</span><span class="sxs-lookup"><span data-stu-id="84d76-117">hello dropdown menu shows five options for single sign-on tooyour application:</span></span>

* <span data-ttu-id="84d76-118">Az Azure AD az egyszeri bejelentkezés le van tiltva</span><span class="sxs-lookup"><span data-stu-id="84d76-118">Azure AD single sign-on disabled</span></span>
* <span data-ttu-id="84d76-119">Jelszó alapú bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="84d76-119">Password-based sign-on</span></span>
* <span data-ttu-id="84d76-120">Csatolt bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="84d76-120">Linked sign-on</span></span>
* <span data-ttu-id="84d76-121">Integrált Windows-hitelesítés</span><span class="sxs-lookup"><span data-stu-id="84d76-121">Integrated Windows Authentication</span></span>
* <span data-ttu-id="84d76-122">Fejléc-alapú bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="84d76-122">Header-based sign-on</span></span>

## <a name="azure-ad-single-sign-on-disabled"></a><span data-ttu-id="84d76-123">Az Azure AD az egyszeri bejelentkezés le van tiltva</span><span class="sxs-lookup"><span data-stu-id="84d76-123">Azure AD single sign-on disabled</span></span>

<span data-ttu-id="84d76-124">Ha nem szeretné, egyszeri bejelentkezés tooyour alkalmazás Azure Active Directory-integráció toouse, **az Azure AD az egyszeri bejelentkezés le van tiltva**.</span><span class="sxs-lookup"><span data-stu-id="84d76-124">If you don't want toouse Azure Active Directory integration for single sign-on tooyour application, choose **Azure AD single sign-on disabled**.</span></span> <span data-ttu-id="84d76-125">Ezt a lehetőséget választja a felhasználók kétszer hitelesíthetők.</span><span class="sxs-lookup"><span data-stu-id="84d76-125">With this option selected, your users may authenticate twice.</span></span> <span data-ttu-id="84d76-126">Először tooAzure Active Directory hitelesítéséhez, és jelentkezzen be magának toohello alkalmazásnak.</span><span class="sxs-lookup"><span data-stu-id="84d76-126">First, they authenticate tooAzure Active Directory and then sign in toohello application itself.</span></span> 

<span data-ttu-id="84d76-127">Ez a beállítás akkor hasznos, ha a helyszíni alkalmazások felhasználók tooauthenticate nem igényel, de a tooadd Azure Active Directory biztonsági réteget, a távoli hozzáféréshez.</span><span class="sxs-lookup"><span data-stu-id="84d76-127">This option is a good choice if your on-premises application doesn't require users tooauthenticate, but you want tooadd Azure Active Directory as a layer of security for remote access.</span></span> 

## <a name="password-based-sign-on"></a><span data-ttu-id="84d76-128">Jelszó alapú bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="84d76-128">Password-based sign-on</span></span>

<span data-ttu-id="84d76-129">Ha a helyszíni alkalmazások szeretné toouse Azure Active Directory és a jelszó-tárolónak, **jelszóalapú bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="84d76-129">If you want toouse Azure Active Directory as a password vault for your on-premises applications, choose **Password-based sign-on**.</span></span> <span data-ttu-id="84d76-130">Ez a beállítás akkor hasznos, ha az alkalmazás végzi a hitelesítést egy felhasználónév/jelszó kombinált jogkivonatot, vagy a fejlécek helyett.</span><span class="sxs-lookup"><span data-stu-id="84d76-130">This option is a good choice if your application authenticates with a username/password combo instead of access tokens or headers.</span></span> <span data-ttu-id="84d76-131">A jelszó alapú bejelentkezés a a felhasználók első alkalommal kell a toohello alkalmazás hello toosign.</span><span class="sxs-lookup"><span data-stu-id="84d76-131">With password-based sign-on, your users need toosign in toohello application hello first time they access it.</span></span> <span data-ttu-id="84d76-132">Ezt követően Azure Active Directory megadja az hello felhasználónév és jelszó hello felhasználó nevében.</span><span class="sxs-lookup"><span data-stu-id="84d76-132">After that, Azure Active Directory supplies hello username and password on behalf of hello user.</span></span> 

<span data-ttu-id="84d76-133">Jelszó alapú bejelentkezés beállításával kapcsolatos információkért lásd: [az egyszeri bejelentkezés az alkalmazásproxy vaulting jelszó](application-proxy-sso-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="84d76-133">For information about setting up password-based sign-on, see [Password vaulting for single sign-on with Application Proxy](application-proxy-sso-azure-portal.md).</span></span>

## <a name="linked-sign-on"></a><span data-ttu-id="84d76-134">Csatolt bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="84d76-134">Linked sign-on</span></span>

<span data-ttu-id="84d76-135">Ha már rendelkezik egyszeri bejelentkezéshez megoldás beállítása a helyszíni identitások, válassza a **bejelentkezés kapcsolódó**.</span><span class="sxs-lookup"><span data-stu-id="84d76-135">If you already have a single sign-on solution set up for your on-premises identities, choose **Linked sign-on**.</span></span> <span data-ttu-id="84d76-136">Ez a beállítás lehetővé teszi az Azure Active Directory tooleverage meglévő SSO-megoldások, de továbbra is biztosít a felhasználók távelérési toohello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="84d76-136">This option enables Azure Active Directory tooleverage existing SSO solutions, but still gives your users remote access toohello application.</span></span> 

<span data-ttu-id="84d76-137">Csatolt bejelentkezés (hivatalosan néven meglévő egyszeri bejelentkezés) kapcsolatos információkért lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval?](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work).</span><span class="sxs-lookup"><span data-stu-id="84d76-137">For information about linked sign-on (formally known as existing single sign-on), see [What is application access and single sign-on with Azure Active Directory?](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work).</span></span>

## <a name="integrated-windows-authentication"></a><span data-ttu-id="84d76-138">Integrált Windows-hitelesítés</span><span class="sxs-lookup"><span data-stu-id="84d76-138">Integrated Windows Authentication</span></span>

<span data-ttu-id="84d76-139">Ha a helyszíni alkalmazások integrált Windows-Authentication(IWA) használja, vagy ha az egyszeri bejelentkezés toouse Kerberos által korlátozott delegálás (KCD) szeretne, válassza ki a **integrált Windows-hitelesítés**.</span><span class="sxs-lookup"><span data-stu-id="84d76-139">If your on-premises applications use Integrated Windows Authentication(IWA) or if you want toouse Kerberos Constrained Delegation (KCD) for single sign-on, choose **Integrated Windows Authentication**.</span></span> <span data-ttu-id="84d76-140">Ezzel a beállítással a felhasználók csak az Active Directory tooauthenticate tooAzure kell, és majd hello Application Proxy connector megszemélyesít hello felhasználói tooget egy Kerberos-jogkivonata, és jelentkezzen be toohello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="84d76-140">With this option, your users only need tooauthenticate tooAzure Active Directory, and then hello Application Proxy connector impersonates hello user tooget a Kerberos token and sign in toohello application.</span></span> 

<span data-ttu-id="84d76-141">Integrált Windows-hitelesítés beállításával kapcsolatos információkért lásd: [Kerberos által korlátozott delegálás az egyszeri bejelentkezés az alkalmazásproxy](active-directory-application-proxy-sso-using-kcd.md).</span><span class="sxs-lookup"><span data-stu-id="84d76-141">For information about setting up Integrated Windows Authentication, see [Kerberos Constrained Delegation for single sign-on with Application Proxy](active-directory-application-proxy-sso-using-kcd.md).</span></span>

## <a name="header-based-sign-on"></a><span data-ttu-id="84d76-142">Fejléc-alapú bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="84d76-142">Header-based sign-on</span></span> 

<span data-ttu-id="84d76-143">Ha az alkalmazások fejlécek használnak a hitelesítéshez, válassza a **fejléc-alapú bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="84d76-143">If your applications use headers for authentication, choose **Header-based sign-on**.</span></span> <span data-ttu-id="84d76-144">Ezzel a beállítással a felhasználók csak kell tooauthentication hello Azure Active Directoryban.</span><span class="sxs-lookup"><span data-stu-id="84d76-144">With this option, your users only need tooauthentication hello Azure Active Directory.</span></span> <span data-ttu-id="84d76-145">Microsoft-partnereknek, egy harmadik fél hitelesítési szolgáltatással PingAccess, amely hello Azure Active Directory hozzáférési jogkivonat lefordítani a fejléc formátuma hello alkalmazás neve.</span><span class="sxs-lookup"><span data-stu-id="84d76-145">Microsoft partners with a third-party authentication service called PingAccess, which translated hello Azure Active Directory access token into a header format for hello application.</span></span> 

<span data-ttu-id="84d76-146">Fejléc-alapú hitelesítés beállításával kapcsolatos információkért lásd: [fejléc-alapú hitelesítés egyszeri bejelentkezéshez az alkalmazásproxy](application-proxy-ping-access.md).</span><span class="sxs-lookup"><span data-stu-id="84d76-146">For information about setting up header-based authentication, see [Header-based authentication for single sign-on with Application Proxy](application-proxy-ping-access.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="84d76-147">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="84d76-147">Next steps</span></span>

- [<span data-ttu-id="84d76-148">Az egyszeri bejelentkezés az alkalmazásproxy vaulting jelszó</span><span class="sxs-lookup"><span data-stu-id="84d76-148">Password vaulting for single sign-on with Application Proxy</span></span>](application-proxy-sso-azure-portal.md)
- [<span data-ttu-id="84d76-149">Kerberos által korlátozott delegálás az egyszeri bejelentkezés az alkalmazásproxy</span><span class="sxs-lookup"><span data-stu-id="84d76-149">Kerberos Constrained Delegation for single sign-on with Application Proxy</span></span>](active-directory-application-proxy-sso-using-kcd.md)
- [<span data-ttu-id="84d76-150">Az egyszeri bejelentkezés az alkalmazásproxy fejléc-alapú hitelesítés</span><span class="sxs-lookup"><span data-stu-id="84d76-150">Header-based authentication for single sign-on with Application Proxy</span></span>](application-proxy-ping-access.md) 
