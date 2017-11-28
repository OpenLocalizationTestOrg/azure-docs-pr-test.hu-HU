---
title: "Jelszó egyszeri bejelentkezés egy nem galéria applicationn a konfigurálása |} Microsoft Docs"
description: "Egyéni nem galéria alkalmazás biztonságos jelszó-alapú egyszeri bejelentkezés konfigurálása, ha nem szerepel az Azure AD Application Gallery"
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
ms.openlocfilehash: f629ec99824199306ebf825901beaa99d83d434d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-configure-password-single-sign-on-for-a-non-gallery-application"></a><span data-ttu-id="2a0d5-103">Jelszó egyszeri bejelentkezés nem galéria alkalmazások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="2a0d5-103">How to configure password single sign-on for a non-gallery application</span></span>

<span data-ttu-id="2a0d5-104">Az Azure AD Application Gallery belül található lehetőségek mellett is is felvehet egy **nem galéria alkalmazás** amikor kívánt alkalmazás nem szerepel ott.</span><span class="sxs-lookup"><span data-stu-id="2a0d5-104">In addition to the choices found within the Azure AD Application Gallery, you also have the option to add a **non-gallery application** when the application you want is not listed there.</span></span> <span data-ttu-id="2a0d5-105">Használja ezt a képességet, hozzáadhat bármely alkalmazás, amely a szervezet már létezik, vagy bármilyen alkalmazás, amelyet esetleg olyan szállítótól, aki nem már része a [az Azure AD Application Gallery](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#get-started-with-the-azure-ad-application-gallery).</span><span class="sxs-lookup"><span data-stu-id="2a0d5-105">Using this capability, you can add any application that already exists in your organization, or any third-party application that you might use from a vendor who is not already part of the [Azure AD Application Gallery](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#get-started-with-the-azure-ad-application-gallery).</span></span>

<span data-ttu-id="2a0d5-106">Miután hozzáadja egy nem galéria-alkalmazást, majd konfigurálhatja az egyszeri bejelentkezés az alkalmazás által használt módszer kiválasztásával a **egyszeri bejelentkezés** navigációs elem az adott vállalati alkalmazást a [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="2a0d5-106">Once you add a non-gallery application, you can then configure the Single sign-on method this application uses by selecting the **Single Sign-on** navigation item on an Enterprise Application in the [Azure Portal](https://portal.azure.com/).</span></span>

<span data-ttu-id="2a0d5-107">Az egyszeri bejelentkezés módszer áll rendelkezésre, egyik a [jelszó-alapú egyszeri bejelentkezést](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="2a0d5-107">One of the Single Sign-on methods available to you is the [Password-Based Single Sign-on](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) option.</span></span> <span data-ttu-id="2a0d5-108">Az a **nem galéria alkalmazás hozzáadása** élményt, egy HTML-alapú felhasználónév Renderelés bármely olyan alkalmazás integrálható, és a jelszó bemeneti mező, még akkor is, ha nincs-e az előre integrált alkalmazások csoportját.</span><span class="sxs-lookup"><span data-stu-id="2a0d5-108">With the **add a non-gallery application** experience, you can integrate any application that renders an HTML-based username and password input field, even if it is not in our set of pre-integrated applications.</span></span>

<span data-ttu-id="2a0d5-109">A ez működik úgy, hogy egy lap lekaparást technológia, amely része a hozzáférési Panel bővítmény, amely lehetővé teszi, hogy a felhasználónév és jelszó bemeneti mező automatikus észlelésű, tárolja őket biztonságos helyen az adott alkalmazáspéldány esetében.</span><span class="sxs-lookup"><span data-stu-id="2a0d5-109">The way this works is by a page scraping technology that is part of the Access Panel extension that allows us to auto-detect username and password input fields, store them securely for your specific application instance.</span></span> <span data-ttu-id="2a0d5-110">Majd biztonságosan visszajátszásos felhasználónevek és jelszavak mezőkhöz, ha a felhasználók megnyitják az alkalmazást az alkalmazás-hozzáférési panelre.</span><span class="sxs-lookup"><span data-stu-id="2a0d5-110">Then securely replay usernames and passwords to those fields when a user navigates to that application on the application access panel.</span></span>

<span data-ttu-id="2a0d5-111">Első lépésként bármilyen alkalmazás integrálása az Azure AD gyorsan remek mód, és lehetővé teszi:</span><span class="sxs-lookup"><span data-stu-id="2a0d5-111">This is a great way to get started integrating any kind of application into Azure AD quickly, and allows you to:</span></span>

-   <span data-ttu-id="2a0d5-112">Integrálni **a világ bármely alkalmazás** az Azure AD-bérlőn, akkor ez a beállítás egy HTML felhasználónév és jelszó beviteli mezőt, amennyiben</span><span class="sxs-lookup"><span data-stu-id="2a0d5-112">Integrate **any application in the world** with your Azure AD tenant, so long as it renders an HTML username and password input field</span></span>

-   <span data-ttu-id="2a0d5-113">Engedélyezése **egyszeri bejelentkezéshez a felhasználók** biztonságos helyen tárolja, és visszajátszását felhasználóneveket és jelszavakat az alkalmazás által integrált már az Azure ad szolgáltatással</span><span class="sxs-lookup"><span data-stu-id="2a0d5-113">Enable **Single Sign-on for your users** by securely storing and replaying usernames and passwords for the application you’ve integrated with Azure AD</span></span>

-   <span data-ttu-id="2a0d5-114">**Az automatikus észlelés bemeneti** mezők bármely alkalmazás, és lehetővé teszi, hogy manuálisan ezeket a mezőket a hozzáférési Panel bővítmény, abban az esetben, ha automatikus észlelés nem talál őket</span><span class="sxs-lookup"><span data-stu-id="2a0d5-114">**Auto-detect input** fields for any application and allow you to manually detect those fields using the Access Panel Browser Extension, in case auto-detection does not find them</span></span>

-   <span data-ttu-id="2a0d5-115">**Több bejelentkezési mező igénylő alkalmazások támogatását** csupán felhasználónév és jelszó megadására való bejelentkezéshez igénylő alkalmazások</span><span class="sxs-lookup"><span data-stu-id="2a0d5-115">**Support applications that require multiple sign-in fields** for applications that require more than just username and password fields to sign in</span></span>

-   <span data-ttu-id="2a0d5-116">**A címkék testreszabása** megjelenik a felhasználók felhasználónév és jelszó bemeneti mező a [alkalmazás hozzáférési Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) Ha a hitelesítő adatok megadása</span><span class="sxs-lookup"><span data-stu-id="2a0d5-116">**Customize the labels** of the username and password input fields your users see on the [Application Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) when they enter their credentials</span></span>

-   <span data-ttu-id="2a0d5-117">Engedélyezi a **felhasználók** azok vannak, írja be manuálisan a meglévő alkalmazás fiókok számára a saját felhasználónevek és jelszavak biztosításához a [alkalmazás hozzáférési Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)</span><span class="sxs-lookup"><span data-stu-id="2a0d5-117">Allow your **users** to provide their own usernames and passwords for any existing application accounts they are typing in manually on the [Application Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)</span></span>

-   <span data-ttu-id="2a0d5-118">Lehetővé teszi egy **az üzleti csoport tagja** számára adja meg a felhasználónevek és jelszavak használatával a felhasználóhoz rendelt a [önkiszolgáló alkalmazás-hozzáférés](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="2a0d5-118">Allow a **member of the business group** to specify the usernames and passwords assigned to a user by using the [Self-Service Application Access](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) feature</span></span>

-   <span data-ttu-id="2a0d5-119">Lehetővé teszi egy **rendszergazda** adja meg, hogy a felhasználónevek és jelszavak a frissítés hitelesítő adatok használatával a felhasználóhoz rendelt funkciót [egy felhasználó hozzárendelése egy alkalmazás](#_How_to_configure_1)</span><span class="sxs-lookup"><span data-stu-id="2a0d5-119">Allow an **administrator** to specify the usernames and passwords assigned to a user by using the Update Credentials feature when [assigning a user to an application](#_How_to_configure_1)</span></span>

-   <span data-ttu-id="2a0d5-120">Lehetővé teszi egy **rendszergazda** adhatja meg a megosztott felhasználónév és jelszó a frissítés hitelesítő adatok segítségével egy csoport tagjainak által használt funkciót [csoport hozzárendelése egy alkalmazás](#assign-an-application-to-a-group-directly)</span><span class="sxs-lookup"><span data-stu-id="2a0d5-120">Allow an **administrator** to specify the shared username or password used by a group of people by using the Update Credentials feature when [assigning a group to an application](#assign-an-application-to-a-group-directly)</span></span>

<span data-ttu-id="2a0d5-121">Az alábbiakban ismerteti, hogyan engedélyezheti [jelszó-alapú egyszeri bejelentkezést](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) bármely alkalmazás használatával hozzáadott a **nem galéria alkalmazás hozzáadása** tapasztalhat.</span><span class="sxs-lookup"><span data-stu-id="2a0d5-121">Below describes how you can enable [Password-Based Single Sign-on](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) to any application that you add using the **add a non-gallery application** experience.</span></span>

## <a name="overview-of-steps-required"></a><span data-ttu-id="2a0d5-122">A szükséges lépések – áttekintés</span><span class="sxs-lookup"><span data-stu-id="2a0d5-122">Overview of steps required</span></span>

<span data-ttu-id="2a0d5-123">Egy alkalmazás kell az Azure AD-galériából konfigurálása:</span><span class="sxs-lookup"><span data-stu-id="2a0d5-123">To configure an application from the Azure AD gallery you need to:</span></span>

-   [<span data-ttu-id="2a0d5-124">Nem galéria alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="2a0d5-124">Add a non-gallery application</span></span>](#add-a-non-gallery-application)

-   [<span data-ttu-id="2a0d5-125">Állítsa be az alkalmazását jelszó egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="2a0d5-125">Configure the application for password single sign-on</span></span>](#configure-the-application-for-password-single-sign-on)

-   [<span data-ttu-id="2a0d5-126">Az alkalmazást egy felhasználó vagy csoport</span><span class="sxs-lookup"><span data-stu-id="2a0d5-126">Assign the application to a user or a group</span></span>](#assign-the-application-to-a-user-or-a-group)

    -   [<span data-ttu-id="2a0d5-127">Felhasználó hozzárendelése egy alkalmazás közvetlenül</span><span class="sxs-lookup"><span data-stu-id="2a0d5-127">Assign a user to an application directly</span></span>](#assign-a-user-to-an-application-directly)

    -   [<span data-ttu-id="2a0d5-128">Rendelje hozzá közvetlenül egy alkalmazás egy csoporthoz</span><span class="sxs-lookup"><span data-stu-id="2a0d5-128">Assign an application to a group directly</span></span>](#assign-an-application-to-a-group-directly)

## <a name="add-a-non-gallery-application"></a><span data-ttu-id="2a0d5-129">Nem galéria alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="2a0d5-129">Add a non-gallery application</span></span>

<span data-ttu-id="2a0d5-130">Hozzáadhat egy alkalmazást az Azure AD-gyűjteményből, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="2a0d5-130">To add an application from the Azure AD Gallery, follow the steps below:</span></span>

1.  <span data-ttu-id="2a0d5-131">Nyissa meg a [Azure Portal](https://portal.azure.com) , és jelentkezzen be egy **globális rendszergazda** vagy **társadminisztrátornak**</span><span class="sxs-lookup"><span data-stu-id="2a0d5-131">Open the [Azure Portal](https://portal.azure.com) and sign in as a **Global Administrator** or **Co-admin**</span></span>

2.  <span data-ttu-id="2a0d5-132">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="2a0d5-132">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="2a0d5-133">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="2a0d5-133">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="2a0d5-134">Kattintson a **vállalati alkalmazások** az Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="2a0d5-134">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="2a0d5-135">Kattintson a **Hozzáadás** gombra a jobb felső sarkában a **vállalati alkalmazások** panel</span><span class="sxs-lookup"><span data-stu-id="2a0d5-135">click the **Add** button at the top-right corner on the **Enterprise Applications** blade</span></span>

6.  <span data-ttu-id="2a0d5-136">Kattintson a **nem-gyűjtemény alkalmazás.**</span><span class="sxs-lookup"><span data-stu-id="2a0d5-136">click **Non-gallery application.**</span></span>

7.  <span data-ttu-id="2a0d5-137">Adja meg az alkalmazás nevét a **neve** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="2a0d5-137">Enter the name of your application in the **Name** textbox.</span></span> <span data-ttu-id="2a0d5-138">Válassza ki **hozzáadása.**</span><span class="sxs-lookup"><span data-stu-id="2a0d5-138">Select **Add.**</span></span>

<span data-ttu-id="2a0d5-139">Egy rövid időszak után el az alkalmazás konfigurációs panelen láthatók.</span><span class="sxs-lookup"><span data-stu-id="2a0d5-139">After a short period, you be able to see the application’s configuration blade.</span></span>

## <a name="configure-the-application-for-password-single-sign-on"></a><span data-ttu-id="2a0d5-140">Állítsa be az alkalmazását jelszó egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="2a0d5-140">Configure the application for password single sign-on</span></span>

<span data-ttu-id="2a0d5-141">Egyszeri bejelentkezés egy alkalmazás konfigurálásához kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="2a0d5-141">To configure single sign-on for an application, follow the steps below:</span></span>

1.  <span data-ttu-id="2a0d5-142">Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="2a0d5-142">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="2a0d5-143">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="2a0d5-143">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="2a0d5-144">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="2a0d5-144">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="2a0d5-145">Kattintson a **vállalati alkalmazások** az Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="2a0d5-145">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="2a0d5-146">Kattintson a **összes alkalmazás** az alkalmazások listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="2a0d5-146">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="2a0d5-147">Ha azt szeretné, hogy itt megjelennek az alkalmazás nem látja, használja a **szűrő** vezérlő tetején a **összes alkalmazások listáját** és állítsa be a **megjelenítése** lehetőséggel **összes alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="2a0d5-147">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="2a0d5-148">Válassza ki az egyszeri bejelentkezés konfigurálni kívánt alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="2a0d5-148">Select the application you want to configure single sign-on.</span></span>

7.  <span data-ttu-id="2a0d5-149">Ha az alkalmazás betölt, kattintson a **egyszeri bejelentkezés** az alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="2a0d5-149">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="2a0d5-150">Válassza ki a módot **jelszóalapú bejelentkezés.**</span><span class="sxs-lookup"><span data-stu-id="2a0d5-150">Select the mode **Password-based Sign-on.**</span></span>

9.  <span data-ttu-id="2a0d5-151">Adja meg a **bejelentkezési URL-cím**.</span><span class="sxs-lookup"><span data-stu-id="2a0d5-151">Enter the **Sign-on URL**.</span></span> <span data-ttu-id="2a0d5-152">Ez az URL-CÍMÉT ahol adja meg a felhasználók a felhasználónevével és jelszavával bejelentkezni.</span><span class="sxs-lookup"><span data-stu-id="2a0d5-152">This is the URL where users enter their username and password to sign in to.</span></span> <span data-ttu-id="2a0d5-153">Győződjön meg arról, a mezők bejelentkezési URL-címen láthatók.</span><span class="sxs-lookup"><span data-stu-id="2a0d5-153">Ensure the sign in fields are visible at the URL.</span></span>

10. <span data-ttu-id="2a0d5-154">Felhasználók hozzárendelése az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="2a0d5-154">Assign users to the application.</span></span>

11. <span data-ttu-id="2a0d5-155">Emellett is megadhatja a felhasználó nevében hitelesítő adatok a felhasználók a sorok kijelöléséhez és parancsával **frissítéséhez szükséges hitelesítő adatokat** , majd gépelje be a felhasználónevet és jelszót a felhasználók nevében.</span><span class="sxs-lookup"><span data-stu-id="2a0d5-155">Additionally, you can also provide credentials on behalf of the user by selecting the rows of the users and clicking on **Update Credentials** and entering the username and password on behalf of the users.</span></span> <span data-ttu-id="2a0d5-156">Ellenkező esetben megkérdezi a felhasználókat a hitelesítő adatok magukat az indítás után.</span><span class="sxs-lookup"><span data-stu-id="2a0d5-156">Otherwise, users be prompted to enter the credentials themselves upon launch.</span></span>

## <a name="assign-a-user-to-an-application-directly"></a><span data-ttu-id="2a0d5-157">Felhasználó hozzárendelése egy alkalmazás közvetlenül</span><span class="sxs-lookup"><span data-stu-id="2a0d5-157">Assign a user to an application directly</span></span>

<span data-ttu-id="2a0d5-158">Hozzárendelése egy vagy több felhasználó alkalmazás közvetlenül, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="2a0d5-158">To assign one or more users to an application directly, follow the steps below:</span></span>

1.  <span data-ttu-id="2a0d5-159">Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="2a0d5-159">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="2a0d5-160">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="2a0d5-160">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="2a0d5-161">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="2a0d5-161">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="2a0d5-162">Kattintson a **vállalati alkalmazások** az Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="2a0d5-162">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="2a0d5-163">Kattintson a **összes alkalmazás** az alkalmazások listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="2a0d5-163">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="2a0d5-164">Ha azt szeretné, hogy itt megjelennek az alkalmazás nem látja, használja a **szűrő** vezérlő tetején a **összes alkalmazások listáját** és állítsa be a **megjelenítése** lehetőséggel **összes alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="2a0d5-164">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="2a0d5-165">Válassza ki szeretné osztani a felhasználót, hogy a listában az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="2a0d5-165">Select the application you want to assign a user to from the list.</span></span>

7.  <span data-ttu-id="2a0d5-166">Ha az alkalmazás betölt, kattintson **felhasználók és csoportok** az alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="2a0d5-166">Once the application loads, click **Users and Groups** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="2a0d5-167">Kattintson a **Hozzáadás** gombra kattint, a a **felhasználók és csoportok** nyissa meg a listában a **hozzáadása hozzárendelés** panelen.</span><span class="sxs-lookup"><span data-stu-id="2a0d5-167">Click the **Add** button on top of the **Users and Groups** list to open the **Add Assignment** blade.</span></span>

9.  <span data-ttu-id="2a0d5-168">Kattintson a **felhasználók és csoportok** a választó a **hozzáadása hozzárendelés** panelen.</span><span class="sxs-lookup"><span data-stu-id="2a0d5-168">click the **Users and groups** selector from the **Add Assignment** blade.</span></span>

10. <span data-ttu-id="2a0d5-169">Írja be a **teljes név** vagy **e-mail cím** érdekli hozzárendelése a felhasználó a **Keresés név vagy e-mail cím alapján** keresőmezőbe.</span><span class="sxs-lookup"><span data-stu-id="2a0d5-169">Type in the **full name** or **email address** of the user you are interested in assigning into the **Search by name or email address** search box.</span></span>

11. <span data-ttu-id="2a0d5-170">Vigye a **felhasználói** a listában, hogy láthatóvá váljon a **jelölőnégyzet**.</span><span class="sxs-lookup"><span data-stu-id="2a0d5-170">Hover over the **user** in the list to reveal a **checkbox**.</span></span> <span data-ttu-id="2a0d5-171">A felhasználói profil fénykép vagy adja hozzá a felhasználót emblémát jelölőnégyzetét, kattintson a **kijelölt** listája.</span><span class="sxs-lookup"><span data-stu-id="2a0d5-171">Click the checkbox next to the user’s profile photo or logo to add your user to the **Selected** list.</span></span>

12. <span data-ttu-id="2a0d5-172">**Választható lehetőség:** Ha azt szeretné, hogy **egynél több felhasználó hozzáadása**, egy másik típus **teljes név** vagy **e-mail cím** be a **Keresés név vagy e-mail cím alapján** mező, és a jelölőnégyzet bejelölésével adja hozzá a felhasználót, hogy a **kijelölt** listája.</span><span class="sxs-lookup"><span data-stu-id="2a0d5-172">**Optional:** If you would like to **add more than one user**, type in another **full name** or **email address** into the **Search by name or email address** search box, and click the checkbox to add this user to the **Selected** list.</span></span>

13. <span data-ttu-id="2a0d5-173">Ha elkészült, válassza a felhasználók, kattintson a **válasszon** gombra kattintva vegye fel a listára a felhasználók és csoportok hozzá kell rendelni az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="2a0d5-173">When you are finished selecting users, click the **Select** button to add them to the list of users and groups to be assigned to the application.</span></span>

14. <span data-ttu-id="2a0d5-174">**Választható lehetőség:** kattintson a **Szerepkörválasztás** a választó a **hozzáadása hozzárendelés** hozzárendelése a kiválasztott felhasználói szerepkör kiválasztása panel.</span><span class="sxs-lookup"><span data-stu-id="2a0d5-174">**Optional:** click the **Select Role** selector in the **Add Assignment** blade to select a role to assign to the users you have selected.</span></span>

15. <span data-ttu-id="2a0d5-175">Kattintson a **hozzárendelése** gombra kattintva a kijelölt felhasználók az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="2a0d5-175">Click the **Assign** button to assign the application to the selected users.</span></span>

## <a name="assign-an-application-to-a-group-directly"></a><span data-ttu-id="2a0d5-176">Rendelje hozzá közvetlenül egy alkalmazás egy csoporthoz</span><span class="sxs-lookup"><span data-stu-id="2a0d5-176">Assign an application to a group directly</span></span>

<span data-ttu-id="2a0d5-177">Közvetlenül egy alkalmazás hozzárendelése egy vagy több csoportot, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="2a0d5-177">To assign one or more groups to an application directly, follow the steps below:</span></span>

1.  <span data-ttu-id="2a0d5-178">Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="2a0d5-178">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="2a0d5-179">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="2a0d5-179">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="2a0d5-180">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="2a0d5-180">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="2a0d5-181">Kattintson a **vállalati alkalmazások** az Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="2a0d5-181">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="2a0d5-182">Kattintson a **összes alkalmazás** az alkalmazások listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="2a0d5-182">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="2a0d5-183">Ha azt szeretné, hogy itt megjelennek az alkalmazás nem látja, használja a **szűrő** vezérlő tetején a **összes alkalmazások listáját** és állítsa be a **megjelenítése** lehetőséggel **összes alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="2a0d5-183">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="2a0d5-184">Válassza ki szeretné osztani a felhasználót, hogy a listában az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="2a0d5-184">Select the application you want to assign a user to from the list.</span></span>

7.  <span data-ttu-id="2a0d5-185">Ha az alkalmazás betölt, kattintson **felhasználók és csoportok** az alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="2a0d5-185">Once the application loads, click **Users and Groups** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="2a0d5-186">Kattintson a **Hozzáadás** gombra kattint, a a **felhasználók és csoportok** nyissa meg a listában a **hozzáadása hozzárendelés** panelen.</span><span class="sxs-lookup"><span data-stu-id="2a0d5-186">Click the **Add** button on top of the **Users and Groups** list to open the **Add Assignment** blade.</span></span>

9.  <span data-ttu-id="2a0d5-187">Kattintson a **felhasználók és csoportok** a választó a **hozzáadása hozzárendelés** panelen.</span><span class="sxs-lookup"><span data-stu-id="2a0d5-187">click the **Users and groups** selector from the **Add Assignment** blade.</span></span>

10. <span data-ttu-id="2a0d5-188">Írja be a **teljes csoportnév** érdekli való hozzárendelése a csoport a **Keresés név vagy e-mail cím alapján** keresőmezőbe.</span><span class="sxs-lookup"><span data-stu-id="2a0d5-188">Type in the **full group name** of the group you are interested in assigning into the **Search by name or email address** search box.</span></span>

11. <span data-ttu-id="2a0d5-189">Vigye a **csoport** a listában, hogy láthatóvá váljon a **jelölőnégyzet**.</span><span class="sxs-lookup"><span data-stu-id="2a0d5-189">Hover over the **group** in the list to reveal a **checkbox**.</span></span> <span data-ttu-id="2a0d5-190">A csoport profilképet vagy adja hozzá a felhasználót emblémát jelölőnégyzetét, kattintson a **kijelölt** listája.</span><span class="sxs-lookup"><span data-stu-id="2a0d5-190">Click the checkbox next to the group’s profile photo or logo to add your user to the **Selected** list.</span></span>

12. <span data-ttu-id="2a0d5-191">**Nem kötelező:** Ha azt szeretné, hogy **egynél több csoport hozzáadása**, egy másik típus **teljes csoportnév** be a **Keresés név vagy e-mail cím alapján** mező, és kattintson a jelölőnégyzetbe, a csoport hozzáadása a **kijelölt** lista.</span><span class="sxs-lookup"><span data-stu-id="2a0d5-191">**Optional:** If you would like to **add more than one group**, type in another **full group name** into the **Search by name or email address** search box, and click the checkbox to add this group to the **Selected** list.</span></span>

13. <span data-ttu-id="2a0d5-192">Ha befejezte a csoportok kiválasztásával, kattintson a **válasszon** gombra kattintva vegye fel a listára a felhasználók és csoportok hozzá kell rendelni az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="2a0d5-192">When you are finished selecting groups, click the **Select** button to add them to the list of users and groups to be assigned to the application.</span></span>

14. <span data-ttu-id="2a0d5-193">**Választható lehetőség:** kattintson a **Szerepkörválasztás** a választó a **hozzáadása hozzárendelés** panelt, és válassza ki a szerepkör hozzárendelése a kijelölt csoportok.</span><span class="sxs-lookup"><span data-stu-id="2a0d5-193">**Optional:** click the **Select Role** selector in the **Add Assignment** blade to select a role to assign to the groups you have selected.</span></span>

15. <span data-ttu-id="2a0d5-194">Kattintson a **hozzárendelése** gombra az alkalmazás a kiválasztott csoportok számára.</span><span class="sxs-lookup"><span data-stu-id="2a0d5-194">Click the **Assign** button to assign the application to the selected groups.</span></span>

<span data-ttu-id="2a0d5-195">Rövid ideig a kijelölt felhasználók tudják elindítani ezeket az alkalmazásokat a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="2a0d5-195">After a short period, the users you have selected be able to launch these applications in the Access Panel.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2a0d5-196">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2a0d5-196">Next steps</span></span>
[<span data-ttu-id="2a0d5-197">Adja meg az egyszeri bejelentkezés az alkalmazásokba a Proxy</span><span class="sxs-lookup"><span data-stu-id="2a0d5-197">Provide single sign-on to your apps with Application Proxy</span></span>](active-directory-application-proxy-sso-using-kcd.md)
