---
title: "Jelszó egyszeri bejelentkezés az Azure AD-katalógusában alkalmazás konfigurálása |} Microsoft Docs"
description: "Biztonságos jelszó-alapú egyszeri bejelentkezést alkalmazás konfigurálása, ha már szerepel az Azure AD Application Gallery"
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
ms.openlocfilehash: d4dc110eb25c3e550ac4663d28e626a696b58f62
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-configure-password-single-sign-on-for-an-azure-ad-gallery-application"></a><span data-ttu-id="4174d-103">Jelszó egyszeri bejelentkezés az Azure AD-katalógusában alkalmazás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="4174d-103">How to configure password single sign-on for an Azure AD Gallery application</span></span>

<span data-ttu-id="4174d-104">Amikor felvesz egy alkalmazást a [az Azure AD Application Gallery](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#get-started-with-the-azure-ad-application-gallery), választhat, hogy hogyan kívánja a felhasználók bejelentkezni az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="4174d-104">When you add an application from the [Azure AD Application Gallery](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#get-started-with-the-azure-ad-application-gallery), you have the choice of how you want your users to sign in to that application.</span></span> <span data-ttu-id="4174d-105">Konfigurálhatja ezt a beállítást bármikor kiválasztásával a **egyszeri bejelentkezés** navigációs elem az adott vállalati alkalmazást a [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="4174d-105">You can configure this choice at any time by selecting the **Single Sign-on** navigation item on an Enterprise Application in the [Azure Portal](https://portal.azure.com/).</span></span>

<span data-ttu-id="4174d-106">Az egyszeri bejelentkezés módszer áll rendelkezésre, egyik a [jelszó-alapú egyszeri bejelentkezést](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="4174d-106">One of the single sign-on methods available to you is the [Password-Based Single Sign-on](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) option.</span></span> <span data-ttu-id="4174d-107">Első lépésként alkalmazások integrálása az Azure AD-be gyorsan remek mód, és lehetővé teszi:</span><span class="sxs-lookup"><span data-stu-id="4174d-107">This is a great way to get started integrating applications into Azure AD quickly, and allows you to:</span></span>

-   <span data-ttu-id="4174d-108">Engedélyezése **egyszeri bejelentkezéshez a felhasználók** biztonságos helyen tárolja, és visszajátszását felhasználóneveket és jelszavakat az alkalmazás által integrált már az Azure ad szolgáltatással</span><span class="sxs-lookup"><span data-stu-id="4174d-108">Enable **Single Sign-on for your users** by securely storing and replaying usernames and passwords for the application you’ve integrated with Azure AD</span></span>

-   <span data-ttu-id="4174d-109">**Több bejelentkezési mező igénylő alkalmazások támogatását** csupán felhasználónév és jelszó megadására való bejelentkezéshez igénylő alkalmazások</span><span class="sxs-lookup"><span data-stu-id="4174d-109">**Support applications that require multiple sign-in fields** for applications that require more than just username and password fields to sign in</span></span>

-   <span data-ttu-id="4174d-110">**A címkék testreszabása** megjelenik a felhasználók felhasználónév és jelszó bemeneti mező a [alkalmazás hozzáférési Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) Ha a hitelesítő adatok megadása</span><span class="sxs-lookup"><span data-stu-id="4174d-110">**Customize the labels** of the username and password input fields your users see on the [Application Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) when they enter their credentials</span></span>

-   <span data-ttu-id="4174d-111">Engedélyezi a **felhasználók** azok vannak, írja be manuálisan a meglévő alkalmazás fiókok számára a saját felhasználónevek és jelszavak biztosításához a [alkalmazás hozzáférési Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)</span><span class="sxs-lookup"><span data-stu-id="4174d-111">Allow your **users** to provide their own usernames and passwords for any existing application accounts they are typing in manually on the [Application Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)</span></span>

-   <span data-ttu-id="4174d-112">Lehetővé teszi egy **az üzleti csoport tagja** számára adja meg a felhasználónevek és jelszavak használatával a felhasználóhoz rendelt a [önkiszolgáló alkalmazás-hozzáférés](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="4174d-112">Allow a **member of the business group** to specify the usernames and passwords assigned to a user by using the [Self-Service Application Access](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) feature</span></span>

-   <span data-ttu-id="4174d-113">Lehetővé teszi egy **rendszergazda** adja meg, hogy a felhasználónevek és jelszavak a frissítés hitelesítő adatok használatával a felhasználóhoz rendelt funkciót [egy felhasználó hozzárendelése egy alkalmazás](#assign-a-user-to-an-application-directly)</span><span class="sxs-lookup"><span data-stu-id="4174d-113">Allow an **administrator** to specify the usernames and passwords assigned to a user by using the Update Credentials feature when [assigning a user to an application](#assign-a-user-to-an-application-directly)</span></span>

-   <span data-ttu-id="4174d-114">Lehetővé teszi egy **rendszergazda** adhatja meg a megosztott felhasználónév és jelszó a frissítés hitelesítő adatok segítségével egy csoport tagjainak által használt funkciót [csoport hozzárendelése egy alkalmazás](#assign-an-application-to-a-group-directly)</span><span class="sxs-lookup"><span data-stu-id="4174d-114">Allow an **administrator** to specify the shared username or password used by a group of people by using the Update Credentials feature when [assigning a group to an application](#assign-an-application-to-a-group-directly)</span></span>

<span data-ttu-id="4174d-115">Az alábbiakban ismerteti, hogyan engedélyezheti [jelszó-alapú egyszeri bejelentkezést](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) egy alkalmazás, amely már része a [az Azure AD Application Gallery](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#get-started-with-the-azure-ad-application-gallery).</span><span class="sxs-lookup"><span data-stu-id="4174d-115">Below describes how you can enable [Password-Based Single Sign-on](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) to an application that is already in the [Azure AD Application Gallery](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#get-started-with-the-azure-ad-application-gallery).</span></span>

## <a name="overview-of-steps-required"></a><span data-ttu-id="4174d-116">A szükséges lépések – áttekintés</span><span class="sxs-lookup"><span data-stu-id="4174d-116">Overview of steps required</span></span>
<span data-ttu-id="4174d-117">Egy alkalmazás kell az Azure AD-galériából konfigurálása:</span><span class="sxs-lookup"><span data-stu-id="4174d-117">To configure an application from the Azure AD gallery you need to:</span></span>

-   [<span data-ttu-id="4174d-118">Alkalmazás hozzáadása az Azure AD-galériából</span><span class="sxs-lookup"><span data-stu-id="4174d-118">Add an application from the Azure AD gallery</span></span>](#add-an-application-from-the-azure-ad-gallery)

-   [<span data-ttu-id="4174d-119">Állítsa be az alkalmazását jelszó egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="4174d-119">Configure the application for password single sign-on</span></span>](#configure-the-application-for-password-single-sign-on)

-   [<span data-ttu-id="4174d-120">Az alkalmazást egy felhasználó vagy csoport</span><span class="sxs-lookup"><span data-stu-id="4174d-120">Assign the application to a user or a group</span></span>](#assign-the-application-to-a-user-or-a-group)

    -   [<span data-ttu-id="4174d-121">Felhasználó hozzárendelése egy alkalmazás közvetlenül</span><span class="sxs-lookup"><span data-stu-id="4174d-121">Assign a user to an application directly</span></span>](#assign-a-user-to-an-application-directly)

    -   [<span data-ttu-id="4174d-122">Rendelje hozzá közvetlenül egy alkalmazás egy csoporthoz</span><span class="sxs-lookup"><span data-stu-id="4174d-122">Assign an application to a group directly</span></span>](#assign-an-application-to-a-group-directly)

## <a name="add-an-application-from-the-azure-ad-gallery"></a><span data-ttu-id="4174d-123">Alkalmazás hozzáadása az Azure AD-galériából</span><span class="sxs-lookup"><span data-stu-id="4174d-123">Add an application from the Azure AD gallery</span></span>

<span data-ttu-id="4174d-124">Hozzáadhat egy alkalmazást az Azure AD-gyűjteményből, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="4174d-124">To add an application from the Azure AD Gallery, follow the steps below:</span></span>

1.  <span data-ttu-id="4174d-125">Nyissa meg a [Azure Portal](https://portal.azure.com) , és jelentkezzen be egy **globális rendszergazda** vagy **társadminisztrátornak**</span><span class="sxs-lookup"><span data-stu-id="4174d-125">Open the [Azure Portal](https://portal.azure.com) and sign in as a **Global Administrator** or **Co-admin**</span></span>

2.  <span data-ttu-id="4174d-126">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="4174d-126">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="4174d-127">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="4174d-127">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="4174d-128">Kattintson a **vállalati alkalmazások** az Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="4174d-128">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="4174d-129">Kattintson a **Hozzáadás** gombra a jobb felső sarkában a **vállalati alkalmazások** panel</span><span class="sxs-lookup"><span data-stu-id="4174d-129">click the **Add** button at the top-right corner on the **Enterprise Applications** blade</span></span>

6.  <span data-ttu-id="4174d-130">Az a **adjon meg egy nevet** a szövegmező a **hozzáadása a gyűjteményből** területen írja be az alkalmazás nevét</span><span class="sxs-lookup"><span data-stu-id="4174d-130">In the **Enter a name** textbox from the **Add from the gallery** section, type the name of the application</span></span>

7.  <span data-ttu-id="4174d-131">Válassza ki az alkalmazást szeretne konfigurálni az egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="4174d-131">Select the application you want to configure for single sign-on</span></span>

8.  <span data-ttu-id="4174d-132">Ad hozzá az alkalmazást, mielőtt a nevét módosíthatja a **neve** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="4174d-132">Before adding the application, you can change its name from the **Name** textbox.</span></span>

9.  <span data-ttu-id="4174d-133">Kattintson a **Hozzáadás** gombra, vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="4174d-133">Click **Add** button, to add the application.</span></span>

<span data-ttu-id="4174d-134">Egy rövid időszak után el az alkalmazás konfigurációs panelen láthatók.</span><span class="sxs-lookup"><span data-stu-id="4174d-134">After a short period, you be able to see the application’s configuration blade.</span></span>

## <a name="configure-the-application-for-password-single-sign-on"></a><span data-ttu-id="4174d-135">Állítsa be az alkalmazását jelszó egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="4174d-135">Configure the application for password single sign-on</span></span>

<span data-ttu-id="4174d-136">Egyszeri bejelentkezés egy alkalmazás konfigurálásához kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="4174d-136">To configure single sign-on for an application, follow the steps below:</span></span>

1.  <span data-ttu-id="4174d-137">Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="4174d-137">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="4174d-138">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="4174d-138">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="4174d-139">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="4174d-139">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="4174d-140">Kattintson a **vállalati alkalmazások** az Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="4174d-140">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="4174d-141">Kattintson a **összes alkalmazás** az alkalmazások listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="4174d-141">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="4174d-142">Ha azt szeretné, hogy itt megjelennek az alkalmazás nem látja, használja a **szűrő** vezérlő tetején a **összes alkalmazások listáját** és állítsa be a **megjelenítése** lehetőséggel **összes alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="4174d-142">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="4174d-143">Válassza ki az egyszeri bejelentkezés konfigurálni kívánt alkalmazást</span><span class="sxs-lookup"><span data-stu-id="4174d-143">Select the application you want to configure single sign-on</span></span>

7.  <span data-ttu-id="4174d-144">Ha az alkalmazás betölt, kattintson a **egyszeri bejelentkezés** az alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="4174d-144">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="4174d-145">Válassza ki a módot **jelszóalapú bejelentkezés.**</span><span class="sxs-lookup"><span data-stu-id="4174d-145">Select the mode **Password-based Sign-on.**</span></span>

9.  <span data-ttu-id="4174d-146">[Felhasználók hozzárendelése az alkalmazás](#assign-a-user-to-an-application-directly).</span><span class="sxs-lookup"><span data-stu-id="4174d-146">[Assign users to the application](#assign-a-user-to-an-application-directly).</span></span>

10. <span data-ttu-id="4174d-147">Emellett is megadhatja a felhasználó nevében hitelesítő adatok a felhasználók a sorok kijelöléséhez és parancsával **frissítéséhez szükséges hitelesítő adatokat** , majd gépelje be a felhasználónevet és jelszót a felhasználók nevében.</span><span class="sxs-lookup"><span data-stu-id="4174d-147">Additionally, you can also provide credentials on behalf of the user by selecting the rows of the users and clicking on **Update Credentials** and entering the username and password on behalf of the users.</span></span> <span data-ttu-id="4174d-148">Ellenkező esetben megkérdezi a felhasználókat a hitelesítő adatok magukat az indítás után.</span><span class="sxs-lookup"><span data-stu-id="4174d-148">Otherwise, users be prompted to enter the credentials themselves upon launch.</span></span>

## <a name="assign-a-user-to-an-application-directly"></a><span data-ttu-id="4174d-149">Felhasználó hozzárendelése egy alkalmazás közvetlenül</span><span class="sxs-lookup"><span data-stu-id="4174d-149">Assign a user to an application directly</span></span>

<span data-ttu-id="4174d-150">Hozzárendelése egy vagy több felhasználó alkalmazás közvetlenül, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="4174d-150">To assign one or more users to an application directly, follow the steps below:</span></span>

1.  <span data-ttu-id="4174d-151">Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="4174d-151">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="4174d-152">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="4174d-152">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="4174d-153">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="4174d-153">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="4174d-154">Kattintson a **vállalati alkalmazások** az Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="4174d-154">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="4174d-155">Kattintson a **összes alkalmazás** az alkalmazások listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="4174d-155">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="4174d-156">Ha azt szeretné, hogy itt megjelennek az alkalmazás nem látja, használja a **szűrő** vezérlő tetején a **összes alkalmazások listáját** és állítsa be a **megjelenítése** lehetőséggel **összes alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="4174d-156">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="4174d-157">Válassza ki szeretné osztani a felhasználót, hogy a listában az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="4174d-157">Select the application you want to assign a user to from the list.</span></span>

7.  <span data-ttu-id="4174d-158">Ha az alkalmazás betölt, kattintson **felhasználók és csoportok** az alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="4174d-158">Once the application loads, click **Users and Groups** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="4174d-159">Kattintson a **Hozzáadás** gombra kattint, a a **felhasználók és csoportok** nyissa meg a listában a **hozzáadása hozzárendelés** panelen.</span><span class="sxs-lookup"><span data-stu-id="4174d-159">Click the **Add** button on top of the **Users and Groups** list to open the **Add Assignment** blade.</span></span>

9.  <span data-ttu-id="4174d-160">Kattintson a **felhasználók és csoportok** a választó a **hozzáadása hozzárendelés** panelen.</span><span class="sxs-lookup"><span data-stu-id="4174d-160">click the **Users and groups** selector from the **Add Assignment** blade.</span></span>

10. <span data-ttu-id="4174d-161">Írja be a **teljes név** vagy **e-mail cím** érdekli hozzárendelése a felhasználó a **Keresés név vagy e-mail cím alapján** keresőmezőbe.</span><span class="sxs-lookup"><span data-stu-id="4174d-161">Type in the **full name** or **email address** of the user you are interested in assigning into the **Search by name or email address** search box.</span></span>

11. <span data-ttu-id="4174d-162">Vigye a **felhasználói** a listában, hogy láthatóvá váljon a **jelölőnégyzet**.</span><span class="sxs-lookup"><span data-stu-id="4174d-162">Hover over the **user** in the list to reveal a **checkbox**.</span></span> <span data-ttu-id="4174d-163">A felhasználói profil fénykép vagy adja hozzá a felhasználót emblémát jelölőnégyzetét, kattintson a **kijelölt** listája.</span><span class="sxs-lookup"><span data-stu-id="4174d-163">Click the checkbox next to the user’s profile photo or logo to add your user to the **Selected** list.</span></span>

12. <span data-ttu-id="4174d-164">**Választható lehetőség:** Ha azt szeretné, hogy **egynél több felhasználó hozzáadása**, egy másik típus **teljes név** vagy **e-mail cím** be a **Keresés név vagy e-mail cím alapján** mező, és a jelölőnégyzet bejelölésével adja hozzá a felhasználót, hogy a **kijelölt** listája.</span><span class="sxs-lookup"><span data-stu-id="4174d-164">**Optional:** If you would like to **add more than one user**, type in another **full name** or **email address** into the **Search by name or email address** search box, and click the checkbox to add this user to the **Selected** list.</span></span>

13. <span data-ttu-id="4174d-165">Ha elkészült, válassza a felhasználók, kattintson a **válasszon** gombra kattintva vegye fel a listára a felhasználók és csoportok hozzá kell rendelni az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="4174d-165">When you are finished selecting users, click the **Select** button to add them to the list of users and groups to be assigned to the application.</span></span>

14. <span data-ttu-id="4174d-166">**Választható lehetőség:** kattintson a **Szerepkörválasztás** a választó a **hozzáadása hozzárendelés** hozzárendelése a kiválasztott felhasználói szerepkör kiválasztása panel.</span><span class="sxs-lookup"><span data-stu-id="4174d-166">**Optional:** click the **Select Role** selector in the **Add Assignment** blade to select a role to assign to the users you have selected.</span></span>

15. <span data-ttu-id="4174d-167">Kattintson a **hozzárendelése** gombra kattintva a kijelölt felhasználók az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="4174d-167">Click the **Assign** button to assign the application to the selected users.</span></span>

## <a name="assign-an-application-to-a-group-directly"></a><span data-ttu-id="4174d-168">Rendelje hozzá közvetlenül egy alkalmazás egy csoporthoz</span><span class="sxs-lookup"><span data-stu-id="4174d-168">Assign an application to a group directly</span></span>

<span data-ttu-id="4174d-169">Közvetlenül egy alkalmazás hozzárendelése egy vagy több csoportot, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="4174d-169">To assign one or more groups to an application directly, follow the steps below:</span></span>

1.  <span data-ttu-id="4174d-170">Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="4174d-170">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="4174d-171">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="4174d-171">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="4174d-172">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="4174d-172">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="4174d-173">Kattintson a **vállalati alkalmazások** az Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="4174d-173">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="4174d-174">Kattintson a **összes alkalmazás** az alkalmazások listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="4174d-174">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="4174d-175">Ha azt szeretné, hogy itt megjelennek az alkalmazás nem látja, használja a **szűrő** vezérlő tetején a **összes alkalmazások listáját** és állítsa be a **megjelenítése** lehetőséggel **összes alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="4174d-175">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="4174d-176">Válassza ki szeretné osztani a felhasználót, hogy a listában az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="4174d-176">Select the application you want to assign a user to from the list.</span></span>

7.  <span data-ttu-id="4174d-177">Ha az alkalmazás betölt, kattintson **felhasználók és csoportok** az alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="4174d-177">Once the application loads, click **Users and Groups** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="4174d-178">Kattintson a **Hozzáadás** gombra kattint, a a **felhasználók és csoportok** nyissa meg a listában a **hozzáadása hozzárendelés** panelen.</span><span class="sxs-lookup"><span data-stu-id="4174d-178">Click the **Add** button on top of the **Users and Groups** list to open the **Add Assignment** blade.</span></span>

9.  <span data-ttu-id="4174d-179">Kattintson a **felhasználók és csoportok** a választó a **hozzáadása hozzárendelés** panelen.</span><span class="sxs-lookup"><span data-stu-id="4174d-179">click the **Users and groups** selector from the **Add Assignment** blade.</span></span>

10. <span data-ttu-id="4174d-180">Írja be a **teljes csoportnév** érdekli való hozzárendelése a csoport a **Keresés név vagy e-mail cím alapján** keresőmezőbe.</span><span class="sxs-lookup"><span data-stu-id="4174d-180">Type in the **full group name** of the group you are interested in assigning into the **Search by name or email address** search box.</span></span>

11. <span data-ttu-id="4174d-181">Vigye a **csoport** a listában, hogy láthatóvá váljon a **jelölőnégyzet**.</span><span class="sxs-lookup"><span data-stu-id="4174d-181">Hover over the **group** in the list to reveal a **checkbox**.</span></span> <span data-ttu-id="4174d-182">A csoport profilképet vagy adja hozzá a felhasználót emblémát jelölőnégyzetét, kattintson a **kijelölt** listája.</span><span class="sxs-lookup"><span data-stu-id="4174d-182">Click the checkbox next to the group’s profile photo or logo to add your user to the **Selected** list.</span></span>

12. <span data-ttu-id="4174d-183">**Nem kötelező:** Ha azt szeretné, hogy **egynél több csoport hozzáadása**, egy másik típus **teljes csoportnév** be a **Keresés név vagy e-mail cím alapján** mező, és kattintson a jelölőnégyzetbe, a csoport hozzáadása a **kijelölt** lista.</span><span class="sxs-lookup"><span data-stu-id="4174d-183">**Optional:** If you would like to **add more than one group**, type in another **full group name** into the **Search by name or email address** search box, and click the checkbox to add this group to the **Selected** list.</span></span>

13. <span data-ttu-id="4174d-184">Ha befejezte a csoportok kiválasztásával, kattintson a **válasszon** gombra kattintva vegye fel a listára a felhasználók és csoportok hozzá kell rendelni az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="4174d-184">When you are finished selecting groups, click the **Select** button to add them to the list of users and groups to be assigned to the application.</span></span>

14. <span data-ttu-id="4174d-185">**Választható lehetőség:** kattintson a **Szerepkörválasztás** a választó a **hozzáadása hozzárendelés** panelt, és válassza ki a szerepkör hozzárendelése a kijelölt csoportok.</span><span class="sxs-lookup"><span data-stu-id="4174d-185">**Optional:** click the **Select Role** selector in the **Add Assignment** blade to select a role to assign to the groups you have selected.</span></span>

15. <span data-ttu-id="4174d-186">Kattintson a **hozzárendelése** gombra az alkalmazás a kiválasztott csoportok számára.</span><span class="sxs-lookup"><span data-stu-id="4174d-186">Click the **Assign** button to assign the application to the selected groups.</span></span>

<span data-ttu-id="4174d-187">Rövid ideig a kijelölt felhasználók tudják elindítani ezeket az alkalmazásokat a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="4174d-187">After a short period, the users you have selected be able to launch these applications in the Access Panel.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4174d-188">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4174d-188">Next steps</span></span>
[<span data-ttu-id="4174d-189">Adja meg az egyszeri bejelentkezés az alkalmazásokba a Proxy</span><span class="sxs-lookup"><span data-stu-id="4174d-189">Provide single sign-on to your apps with Application Proxy</span></span>](active-directory-application-proxy-sso-using-kcd.md)
