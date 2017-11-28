---
title: "Egyszeri bejelentkezés alkalmazásokhoz az Azure AD alkalmazásproxy |} Microsoft Docs"
description: "Kapcsolja be a egyszeri bejelentkezés az Azure AD alkalmazásproxy az Azure-portálon a közzétett helyszíni alkalmazásokhoz."
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
ms.openlocfilehash: 9ddc0c1bd5f2cbb24f6761cfd041b820ee6464b8
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="password-vaulting-for-single-sign-on-with-application-proxy"></a><span data-ttu-id="28863-103">Az egyszeri bejelentkezés az alkalmazásproxy vaulting jelszó</span><span class="sxs-lookup"><span data-stu-id="28863-103">Password vaulting for single sign-on with Application Proxy</span></span>

<span data-ttu-id="28863-104">Az Azure Active Directory Alkalmazásproxyjával segít a helyszíni alkalmazások közzététele, hogy a távoli alkalmazottak biztonságosan érhetik el azokat, túl által a termelékenység növelése.</span><span class="sxs-lookup"><span data-stu-id="28863-104">Azure Active Directory Application Proxy helps you improve productivity by publishing on-premises applications so that remote employees can securely access them, too.</span></span> <span data-ttu-id="28863-105">Az Azure portálon is állíthatja be az egyszeri bejelentkezés (SSO) ezekhez az alkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="28863-105">In the Azure portal, you can also set up single sign-on (SSO) to these apps.</span></span> <span data-ttu-id="28863-106">A felhasználók csak kell az Azure AD hitelesíti, és nem kell ismét jelentkezzen be a vállalati alkalmazás eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="28863-106">Your users only need to authenticate with Azure AD, and they can access your enterprise application without having to sign in again.</span></span>

<span data-ttu-id="28863-107">Alkalmazásproxy támogatja több [az egyszeri bejelentkezési módok](application-proxy-sso-overview.md).</span><span class="sxs-lookup"><span data-stu-id="28863-107">Application Proxy supports several [single sign-on modes](application-proxy-sso-overview.md).</span></span> <span data-ttu-id="28863-108">Jelszó alapú bejelentkezés készült alkalmazásokat, amelyek egy felhasználónév/jelszó kombináció használják a hitelesítéshez.</span><span class="sxs-lookup"><span data-stu-id="28863-108">Password-based sign-on is intended for applications that use a username/password combination for authentication.</span></span> <span data-ttu-id="28863-109">Konfigurálásakor jelszó alapú bejelentkezés az alkalmazáshoz, a felhasználók rendelkeznek a helyszíni alkalmazások egyszer bejelentkezhet.</span><span class="sxs-lookup"><span data-stu-id="28863-109">When you configure password-based sign-on for your application, your users have to sign in to the on-premises application once.</span></span> <span data-ttu-id="28863-110">Ezt követően az Azure Active Directory a bejelentkezési adatait tárolja, és automatikusan biztosítja azt az alkalmazásnak, amikor a felhasználók távolról el.</span><span class="sxs-lookup"><span data-stu-id="28863-110">After that, Azure Active Directory stores the sign-in information and automatically provides it to the application when your users access it remotely.</span></span> 

<span data-ttu-id="28863-111">Kell már rendelkezik közzétett és tesztelni az alkalmazást az alkalmazásproxy.</span><span class="sxs-lookup"><span data-stu-id="28863-111">You should already have published and tested your app with Application Proxy.</span></span> <span data-ttu-id="28863-112">Ha nem, kövesse a lépéseket [alkalmazások közzététele az Azure AD-alkalmazásproxy használatával](application-proxy-publish-azure-portal.md) majd térjen vissza ide.</span><span class="sxs-lookup"><span data-stu-id="28863-112">If not, follow the steps in [Publish applications using Azure AD Application Proxy](application-proxy-publish-azure-portal.md) then come back here.</span></span> 

## <a name="set-up-password-vaulting-for-your-application"></a><span data-ttu-id="28863-113">Az alkalmazás vaulting jelszó beállítása</span><span class="sxs-lookup"><span data-stu-id="28863-113">Set up password vaulting for your application</span></span>

1. <span data-ttu-id="28863-114">Jelentkezzen be az [Azure Portal](https://portal.azure.com) felületére rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="28863-114">Sign in to the [Azure portal](https://portal.azure.com) as an administrator.</span></span>
2. <span data-ttu-id="28863-115">Válassza ki **Azure Active Directory** > **vállalati alkalmazások** > **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="28863-115">Select **Azure Active Directory** > **Enterprise applications** > **All applications**.</span></span>
3. <span data-ttu-id="28863-116">A listában jelölje ki az egyszeri bejelentkezés állítsa be a kívánt alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="28863-116">From the list, select the app that you want to set up with SSO.</span></span>  
4. <span data-ttu-id="28863-117">Válassza ki **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="28863-117">Select **Single sign-on**.</span></span>

   ![Válassza ki az egyszeri bejelentkezés](./media/application-proxy-sso-azure-portal/select-sso.png)

5. <span data-ttu-id="28863-119">Az egyszeri bejelentkezés mód kiválasztása **jelszóalapú bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="28863-119">For the SSO mode, choose **Password-based Sign-on**.</span></span>
6. <span data-ttu-id="28863-120">A bejelentkezési URL-címet adja meg az URL-címet a lap ahol adja meg a felhasználók a felhasználónevével és jelszavával bejelentkezni az alkalmazást a vállalati hálózaton kívülről.</span><span class="sxs-lookup"><span data-stu-id="28863-120">For the Sign-on URL, enter the URL for the page where users enter their username and password to sign in to your app outside of the corporate network.</span></span> <span data-ttu-id="28863-121">Ennek az lehet az alkalmazás-proxyn keresztül történő közzétételekor létrehozott külső URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="28863-121">This may be the External URL that you created when you published the app through Application Proxy.</span></span> 

   ![Válassza a jelszó alapú bejelentkezés, és írja be az URL-cím](./media/application-proxy-sso-azure-portal/password-sso.png)

7. <span data-ttu-id="28863-123">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="28863-123">Select **Save**.</span></span>

<!-- Need to repro?
7. The page should tell you that a sign-in form was successfully detected at the provided URL. If it doesn't, select **Configure [your app name] Password Single Sign-on Settings** and choose **Manually detect sign-in fields**. Follow the instructions to point out where the sign-in credentials go. 
-->

## <a name="test-your-app"></a><span data-ttu-id="28863-124">Az alkalmazás tesztelése</span><span class="sxs-lookup"><span data-stu-id="28863-124">Test your app</span></span>

<span data-ttu-id="28863-125">Ugrás a külső URL-címet, az alkalmazás állított be a távoli hozzáféréshez.</span><span class="sxs-lookup"><span data-stu-id="28863-125">Go to external URL that you configured for remote access to your application.</span></span> <span data-ttu-id="28863-126">Jelentkezzen be a hitelesítő adatait az alkalmazás (vagy egy Ön által beállított hozzáféréssel rendelkező teszt fiók hitelesítő adatait).</span><span class="sxs-lookup"><span data-stu-id="28863-126">Sign in with your credentials for that app (or the credentials for a test account that you set up with access).</span></span> <span data-ttu-id="28863-127">Amikor bejelentkezik sikeresen hagyja meg az alkalmazást, és térjen vissza a hitelesítő adatok ismételt beírása nélkül kell lennie.</span><span class="sxs-lookup"><span data-stu-id="28863-127">Once you sign in successfully, you should be able to leave the app and come back without entering your credentials again.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="28863-128">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="28863-128">Next steps</span></span>

- <span data-ttu-id="28863-129">Olvassa el, egyéb módjai megvalósításához [egyszeri bejelentkezéshez az alkalmazásproxy](application-proxy-sso-overview.md)</span><span class="sxs-lookup"><span data-stu-id="28863-129">Read about other ways to implement [Single sign-on with Application Proxy](application-proxy-sso-overview.md)</span></span>
- <span data-ttu-id="28863-130">További tudnivalók [távolról az Azure AD alkalmazásproxy alkalmazásokhoz fér hozzá biztonsági szempontjai](application-proxy-security-considerations.md)</span><span class="sxs-lookup"><span data-stu-id="28863-130">Learn about [Security considerations for accessing apps remotely with Azure AD Application Proxy](application-proxy-security-considerations.md)</span></span>