---
title: "az Azure AD alkalmazásproxy aaaSingle bejelentkezés tooapps |} Microsoft Docs"
description: "Kapcsolja be az egyszeri bejelentkezés az Azure AD alkalmazásproxy hello Azure-portálon a közzétett helyszíni alkalmazásokhoz."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: d94ac3f4-cd33-4c51-9d19-544a528637d4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 5ff288d36163b74215677d9e34c93c985ac33d54
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="password-vaulting-for-single-sign-on-with-application-proxy"></a><span data-ttu-id="d5b24-103">Az egyszeri bejelentkezés az alkalmazásproxy vaulting jelszó</span><span class="sxs-lookup"><span data-stu-id="d5b24-103">Password vaulting for single sign-on with Application Proxy</span></span>

<span data-ttu-id="d5b24-104">Az Azure Active Directory Alkalmazásproxyjával segít a helyszíni alkalmazások közzététele, hogy a távoli alkalmazottak biztonságosan érhetik el azokat, túl által a termelékenység növelése.</span><span class="sxs-lookup"><span data-stu-id="d5b24-104">Azure Active Directory Application Proxy helps you improve productivity by publishing on-premises applications so that remote employees can securely access them, too.</span></span> <span data-ttu-id="d5b24-105">Hello Azure-portálon, a is beállíthat egyszeri bejelentkezés (SSO) toothese alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="d5b24-105">In hello Azure portal, you can also set up single sign-on (SSO) toothese apps.</span></span> <span data-ttu-id="d5b24-106">A felhasználók csak az Azure ad-val tooauthenticate kell, és anélkül toosign újra a vállalati alkalmazás eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="d5b24-106">Your users only need tooauthenticate with Azure AD, and they can access your enterprise application without having toosign in again.</span></span>

<span data-ttu-id="d5b24-107">Alkalmazásproxy támogatja több [az egyszeri bejelentkezési módok](application-proxy-sso-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d5b24-107">Application Proxy supports several [single sign-on modes](application-proxy-sso-overview.md).</span></span> <span data-ttu-id="d5b24-108">Jelszó alapú bejelentkezés készült alkalmazásokat, amelyek egy felhasználónév/jelszó kombináció használják a hitelesítéshez.</span><span class="sxs-lookup"><span data-stu-id="d5b24-108">Password-based sign-on is intended for applications that use a username/password combination for authentication.</span></span> <span data-ttu-id="d5b24-109">Konfigurálásakor jelszó alapú bejelentkezés az alkalmazáshoz, a felhasználók toosign egyszer toohello a helyszíni alkalmazások rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="d5b24-109">When you configure password-based sign-on for your application, your users have toosign in toohello on-premises application once.</span></span> <span data-ttu-id="d5b24-110">Ezt követően az Azure Active Directory hello bejelentkezési adatait tárolja, és automatikusan biztosítja azt toohello alkalmazás amikor a felhasználók távolról el.</span><span class="sxs-lookup"><span data-stu-id="d5b24-110">After that, Azure Active Directory stores hello sign-in information and automatically provides it toohello application when your users access it remotely.</span></span> 

<span data-ttu-id="d5b24-111">Kell már rendelkezik közzétett és tesztelni az alkalmazást az alkalmazásproxy.</span><span class="sxs-lookup"><span data-stu-id="d5b24-111">You should already have published and tested your app with Application Proxy.</span></span> <span data-ttu-id="d5b24-112">Ha nem, kövesse hello [alkalmazások közzététele az Azure AD-alkalmazásproxy használatával](application-proxy-publish-azure-portal.md) majd térjen vissza ide.</span><span class="sxs-lookup"><span data-stu-id="d5b24-112">If not, follow hello steps in [Publish applications using Azure AD Application Proxy](application-proxy-publish-azure-portal.md) then come back here.</span></span> 

## <a name="set-up-password-vaulting-for-your-application"></a><span data-ttu-id="d5b24-113">Az alkalmazás vaulting jelszó beállítása</span><span class="sxs-lookup"><span data-stu-id="d5b24-113">Set up password vaulting for your application</span></span>

1. <span data-ttu-id="d5b24-114">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com) rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="d5b24-114">Sign in toohello [Azure portal](https://portal.azure.com) as an administrator.</span></span>
2. <span data-ttu-id="d5b24-115">Válassza ki **Azure Active Directory** > **vállalati alkalmazások** > **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="d5b24-115">Select **Azure Active Directory** > **Enterprise applications** > **All applications**.</span></span>
3. <span data-ttu-id="d5b24-116">Hello listájából válassza ki, amelyet az egyszeri bejelentkezési modellel mentése tooset hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="d5b24-116">From hello list, select hello app that you want tooset up with SSO.</span></span>  
4. <span data-ttu-id="d5b24-117">Válassza ki **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="d5b24-117">Select **Single sign-on**.</span></span>

   ![Válassza ki az egyszeri bejelentkezés](./media/application-proxy-sso-azure-portal/select-sso.png)

5. <span data-ttu-id="d5b24-119">Hello SSO módot, válassza a **jelszóalapú bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="d5b24-119">For hello SSO mode, choose **Password-based Sign-on**.</span></span>
6. <span data-ttu-id="d5b24-120">Bejelentkezési URL-címhez hello meg hello URL-címet hello lap ahol felhasználók adja meg a felhasználónevet és jelszót toosign alkalmazásban tooyour hello vállalati hálózaton kívülről.</span><span class="sxs-lookup"><span data-stu-id="d5b24-120">For hello Sign-on URL, enter hello URL for hello page where users enter their username and password toosign in tooyour app outside of hello corporate network.</span></span> <span data-ttu-id="d5b24-121">Lehet, hogy a külső URL-cím hello app proxyn keresztül történő közzétételekor létrehozott hello.</span><span class="sxs-lookup"><span data-stu-id="d5b24-121">This may be hello External URL that you created when you published hello app through Application Proxy.</span></span> 

   ![Válassza a jelszó alapú bejelentkezés, és írja be az URL-cím](./media/application-proxy-sso-azure-portal/password-sso.png)

7. <span data-ttu-id="d5b24-123">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="d5b24-123">Select **Save**.</span></span>

<!-- Need toorepro?
7. hello page should tell you that a sign-in form was successfully detected at hello provided URL. If it doesn't, select **Configure [your app name] Password Single Sign-on Settings** and choose **Manually detect sign-in fields**. Follow hello instructions toopoint out where hello sign-in credentials go. 
-->

## <a name="test-your-app"></a><span data-ttu-id="d5b24-124">Az alkalmazás tesztelése</span><span class="sxs-lookup"><span data-stu-id="d5b24-124">Test your app</span></span>

<span data-ttu-id="d5b24-125">Nyissa meg a távelérés tooyour alkalmazáshoz konfigurált tooexternal URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="d5b24-125">Go tooexternal URL that you configured for remote access tooyour application.</span></span> <span data-ttu-id="d5b24-126">Jelentkezzen be a hitelesítő adatokat az adott alkalmazáshoz (vagy egy olyan fiókot, amely hozzáféréssel rendelkező beállítása hello hitelesítő adatait).</span><span class="sxs-lookup"><span data-stu-id="d5b24-126">Sign in with your credentials for that app (or hello credentials for a test account that you set up with access).</span></span> <span data-ttu-id="d5b24-127">Amikor bejelentkezik sikeresen tudja tooleave hello alkalmazás, és térjen vissza a hitelesítő adatok ismételt beírása nélkül.</span><span class="sxs-lookup"><span data-stu-id="d5b24-127">Once you sign in successfully, you should be able tooleave hello app and come back without entering your credentials again.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="d5b24-128">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d5b24-128">Next steps</span></span>

- <span data-ttu-id="d5b24-129">Olvassa el, egyéb módon tooimplement [egyszeri bejelentkezéshez az alkalmazásproxy](application-proxy-sso-overview.md)</span><span class="sxs-lookup"><span data-stu-id="d5b24-129">Read about other ways tooimplement [Single sign-on with Application Proxy](application-proxy-sso-overview.md)</span></span>
- <span data-ttu-id="d5b24-130">További tudnivalók [távolról az Azure AD alkalmazásproxy alkalmazásokhoz fér hozzá biztonsági szempontjai](application-proxy-security-considerations.md)</span><span class="sxs-lookup"><span data-stu-id="d5b24-130">Learn about [Security considerations for accessing apps remotely with Azure AD Application Proxy](application-proxy-security-considerations.md)</span></span>