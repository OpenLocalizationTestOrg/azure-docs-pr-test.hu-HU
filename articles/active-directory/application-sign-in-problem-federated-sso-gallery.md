---
title: "A gyűjtemény alkalmazáshoz konfigurált összevont bejelentkezés problémák egyszeri bejelentkezés |} Microsoft Docs"
description: "Útmutatás az egyes hibaüzenetek, amikor bejelentkezik egy alkalmazás már konfigurálta az SAML-alapú összevont egyszeri bejelentkezés az Azure ad szolgáltatással"
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
ms.openlocfilehash: 0fc5a8eb3d033d60bf6082d61bf1698924ab91c6
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="problems-signing-in-to-a-gallery-application-configured-for-federated-single-sign-on"></a><span data-ttu-id="82752-103">Bejelentkezés egy összevont egyszeri bejelentkezés beállítása gyűjtemény alkalmazás problémák</span><span class="sxs-lookup"><span data-stu-id="82752-103">Problems signing in to a gallery application configured for federated single sign-on</span></span>

<span data-ttu-id="82752-104">A hiba elhárításához ellenőrizze az alkalmazás konfigurációját az Azure AD a következőképpen kell:</span><span class="sxs-lookup"><span data-stu-id="82752-104">To troubleshoot your problem, you need to verify the application configuration in Azure AD as follow:</span></span>

-   <span data-ttu-id="82752-105">Az Azure AD-gyűjtemény alkalmazás a konfigurációs lépéseket követte.</span><span class="sxs-lookup"><span data-stu-id="82752-105">You have followed all the configuration steps for the Azure AD gallery application.</span></span>

-   <span data-ttu-id="82752-106">A azonosító és az aad-ben megadott válasz URL-cím egyezik meg azokat az alkalmazás a várt értékek</span><span class="sxs-lookup"><span data-stu-id="82752-106">The Identifier and Reply URL configured in AAD match they expected values in the application</span></span>

-   <span data-ttu-id="82752-107">Az alkalmazáshoz hozzárendelt felhasználók</span><span class="sxs-lookup"><span data-stu-id="82752-107">You have assigned users to the application</span></span>

## <a name="application-not-found-in-directory"></a><span data-ttu-id="82752-108">Az alkalmazás nem található a könyvtárban</span><span class="sxs-lookup"><span data-stu-id="82752-108">Application not found in directory</span></span>

<span data-ttu-id="82752-109">*Hiba AADSTS70001: "Https://contoso.com" azonosítójú alkalmazás nem található a könyvtárban*.</span><span class="sxs-lookup"><span data-stu-id="82752-109">*Error AADSTS70001: Application with Identifier ‘https://contoso.com’ was not found in the directory*.</span></span>

<span data-ttu-id="82752-110">**Lehetséges ok**</span><span class="sxs-lookup"><span data-stu-id="82752-110">**Possible cause**</span></span>

<span data-ttu-id="82752-111">A kibocsátó attribútum küldése az alkalmazásból az Azure AD-t a SAML-kérelmet nem felel meg az alkalmazás az Azure AD konfigurált azonosító értéket.</span><span class="sxs-lookup"><span data-stu-id="82752-111">The Issuer attribute sends from the application to Azure AD in the SAML request doesn’t match the Identifier value configured in the application Azure AD.</span></span>

<span data-ttu-id="82752-112">**Megoldás**</span><span class="sxs-lookup"><span data-stu-id="82752-112">**Resolution**</span></span>

<span data-ttu-id="82752-113">Győződjön meg arról, hogy a kibocsátó attribútumot a SAML-kérelmet azt van megfelelő az azonosító az Azure ad-ben konfigurált értéket:</span><span class="sxs-lookup"><span data-stu-id="82752-113">Ensure that the Issuer attribute in the SAML request it’s matching the Identifier value configured in Azure AD:</span></span>

1.  <span data-ttu-id="82752-114">Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="82752-114">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="82752-115">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="82752-115">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="82752-116">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="82752-116">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="82752-117">Kattintson a **vállalati alkalmazások** az Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="82752-117">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="82752-118">Kattintson a **összes alkalmazás** az alkalmazások listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="82752-118">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="82752-119">Ha azt szeretné, hogy itt megjelennek az alkalmazás nem látja, használja a **szűrő** vezérlő tetején a **összes alkalmazások listáját** és állítsa be a **megjelenítése** lehetőséggel **összes alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="82752-119">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="82752-120">Válassza ki az egyszeri bejelentkezés konfigurálni kívánt alkalmazást</span><span class="sxs-lookup"><span data-stu-id="82752-120">Select the application you want to configure single sign-on</span></span>

7.  <span data-ttu-id="82752-121">Ha az alkalmazás betölt, kattintson a **egyszeri bejelentkezés** az alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="82752-121">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="82752-122">Ugrás a **tartomány és az URL-címek** szakasz.</span><span class="sxs-lookup"><span data-stu-id="82752-122">Go to **Domain and URLs** section.</span></span> <span data-ttu-id="82752-123">Győződjön meg arról, hogy az azonosító szövegmező értékét a értéket az azonosító a jelennek meg a hiba a megfelelő-e.</span><span class="sxs-lookup"><span data-stu-id="82752-123">Verify that the value in the Identifier textbox is matching the value for the identifier value displayed in the error.</span></span>

<span data-ttu-id="82752-124">Miután frissítette az azonosító értéket az Azure ad-ben, és azt az értéket küld van megfelelő az alkalmazásnak a SAML-kérelmet, jelentkezhet be az alkalmazásba kell lennie.</span><span class="sxs-lookup"><span data-stu-id="82752-124">After you have updated the Identifier value in Azure AD and it’s matching the value sends by the application in the SAML request, you should be able to sign in to the application.</span></span>

## <a name="the-reply-address-does-not-match-the-reply-addresses-configured-for-the-application"></a><span data-ttu-id="82752-125">A válaszcím nem egyezik meg az alkalmazáshoz beállított válasz-címeket.</span><span class="sxs-lookup"><span data-stu-id="82752-125">The reply address does not match the reply addresses configured for the application.</span></span>

<span data-ttu-id="82752-126">*AADSTS50011. hiba: A válasz "https://contoso.com" nem egyezik a válasz címek konfigurálva az alkalmazáshoz*</span><span class="sxs-lookup"><span data-stu-id="82752-126">*Error AADSTS50011: The reply address ‘https://contoso.com’ does not match the reply addresses configured for the application*</span></span>

<span data-ttu-id="82752-127">**Lehetséges ok**</span><span class="sxs-lookup"><span data-stu-id="82752-127">**Possible cause**</span></span>

<span data-ttu-id="82752-128">A SAML-kérelmet AssertionConsumerServiceURL értéke nem egyezik, a válasz URL-címével vagy a minta az Azure ad-ben konfigurált.</span><span class="sxs-lookup"><span data-stu-id="82752-128">The AssertionConsumerServiceURL value in the SAML request doesn't match the Reply URL value or pattern configured in Azure AD.</span></span> <span data-ttu-id="82752-129">A SAML-kérelmet AssertionConsumerServiceURL érték az URL-címet, a hibaüzenet látható.</span><span class="sxs-lookup"><span data-stu-id="82752-129">The AssertionConsumerServiceURL value in the SAML request is the URL you see in the error.</span></span>

<span data-ttu-id="82752-130">**Megoldás**</span><span class="sxs-lookup"><span data-stu-id="82752-130">**Resolution**</span></span>

<span data-ttu-id="82752-131">Győződjön meg arról, hogy a SAML-kérelmet, akkor a válasz URL-cím van megfelelő AssertionConsumerServiceURL értékét az Azure ad-ben beállított értéket.</span><span class="sxs-lookup"><span data-stu-id="82752-131">Ensure that the AssertionConsumerServiceURL value in the SAML request it's matching the Reply URL value configured in Azure AD.</span></span>

1.  <span data-ttu-id="82752-132">Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="82752-132">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="82752-133">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="82752-133">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="82752-134">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="82752-134">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="82752-135">Kattintson a **vállalati alkalmazások** az Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="82752-135">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="82752-136">Kattintson a **összes alkalmazás** az alkalmazások listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="82752-136">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="82752-137">Ha azt szeretné, hogy itt megjelennek az alkalmazás nem látja, használja a **szűrő** vezérlő tetején a **összes alkalmazások listáját** és állítsa be a **megjelenítése** lehetőséggel **összes alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="82752-137">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="82752-138">Válassza ki az egyszeri bejelentkezés konfigurálni kívánt alkalmazást</span><span class="sxs-lookup"><span data-stu-id="82752-138">Select the application you want to configure single sign-on</span></span>

7.  <span data-ttu-id="82752-139">Ha az alkalmazás betölt, kattintson a **egyszeri bejelentkezés** az alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="82752-139">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="82752-140">Ugrás a **tartomány és az URL-címek** szakasz.</span><span class="sxs-lookup"><span data-stu-id="82752-140">Go to **Domain and URLs** section.</span></span> <span data-ttu-id="82752-141">Győződjön meg arról, vagy frissítse az értéket, hogy az egyezzen a SAML-kérelmet a AssertionConsumerServiceURL értékkel válasz URL-CÍMEN szövegmezőjének.</span><span class="sxs-lookup"><span data-stu-id="82752-141">Verify or update the value in the Reply URL textbox to match the AssertionConsumerServiceURL value in the SAML request.</span></span>  
    * <span data-ttu-id="82752-142">Ha a válasz URL-cím beviteli mező nem látható, válassza ki a **megjelenítése speciális URL-beállításainak** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="82752-142">If you don't see the Reply URL textbox, select the **Show advanced URL settings** checkbox.</span></span>

<span data-ttu-id="82752-143">Miután frissítette az válasz URL-cím az Azure ad-ben, és azt az értéket küld van megfelelő az alkalmazásnak a SAML-kérelmet, jelentkezhet be az alkalmazásba kell lennie.</span><span class="sxs-lookup"><span data-stu-id="82752-143">After you have updated the Reply URL value in Azure AD and it’s matching the value sends by the application in the SAML request, you should be able to sign in to the application.</span></span>

## <a name="user-not-assigned-a-role"></a><span data-ttu-id="82752-144">Nem hozzárendelt felhasználó</span><span class="sxs-lookup"><span data-stu-id="82752-144">User not assigned a role</span></span>

<span data-ttu-id="82752-145">*AADSTS50105. hiba: A bejelentkezett felhasználó nevében "brian@contoso.com" nincs hozzárendelve az alkalmazás szerepkör*.</span><span class="sxs-lookup"><span data-stu-id="82752-145">*Error AADSTS50105: The signed in user 'brian@contoso.com' is not assigned to a role for the application*.</span></span>

<span data-ttu-id="82752-146">**Lehetséges ok**</span><span class="sxs-lookup"><span data-stu-id="82752-146">**Possible cause**</span></span>

<span data-ttu-id="82752-147">A felhasználó nem rendelkezik az Azure AD-alkalmazás hozzáférését.</span><span class="sxs-lookup"><span data-stu-id="82752-147">The user has not been granted access to the application in Azure AD.</span></span>

<span data-ttu-id="82752-148">**Megoldás**</span><span class="sxs-lookup"><span data-stu-id="82752-148">**Resolution**</span></span>

<span data-ttu-id="82752-149">Hozzárendelése egy vagy több felhasználó alkalmazás közvetlenül, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="82752-149">To assign one or more users to an application directly, follow the steps below:</span></span>

1.  <span data-ttu-id="82752-150">Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="82752-150">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="82752-151">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="82752-151">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="82752-152">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="82752-152">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="82752-153">Kattintson a **vállalati alkalmazások** az Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="82752-153">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="82752-154">Kattintson a **összes alkalmazás** az alkalmazások listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="82752-154">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="82752-155">Ha azt szeretné, hogy itt megjelennek az alkalmazás nem látja, használja a **szűrő** vezérlő tetején a **összes alkalmazások listáját** és állítsa be a **megjelenítése** lehetőséggel **összes alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="82752-155">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="82752-156">Válassza ki szeretné osztani a felhasználót, hogy a listában az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="82752-156">Select the application you want to assign a user to from the list.</span></span>

7.  <span data-ttu-id="82752-157">Ha az alkalmazás betölt, kattintson **felhasználók és csoportok** az alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="82752-157">Once the application loads, click **Users and Groups** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="82752-158">Kattintson a **Hozzáadás** gombra kattint, a a **felhasználók és csoportok** nyissa meg a listában a **hozzáadása hozzárendelés** panelen.</span><span class="sxs-lookup"><span data-stu-id="82752-158">Click the **Add** button on top of the **Users and Groups** list to open the **Add Assignment** blade.</span></span>

9.  <span data-ttu-id="82752-159">Kattintson a **felhasználók és csoportok** a választó a **hozzáadása hozzárendelés** panelen.</span><span class="sxs-lookup"><span data-stu-id="82752-159">click the **Users and groups** selector from the **Add Assignment** blade.</span></span>

10. <span data-ttu-id="82752-160">Írja be a **teljes név** vagy **e-mail cím** érdekli hozzárendelése a felhasználó a **Keresés név vagy e-mail cím alapján** keresőmezőbe.</span><span class="sxs-lookup"><span data-stu-id="82752-160">Type in the **full name** or **email address** of the user you are interested in assigning into the **Search by name or email address** search box.</span></span>

11. <span data-ttu-id="82752-161">Vigye a **felhasználói** a listában, hogy láthatóvá váljon a **jelölőnégyzet**.</span><span class="sxs-lookup"><span data-stu-id="82752-161">Hover over the **user** in the list to reveal a **checkbox**.</span></span> <span data-ttu-id="82752-162">A felhasználói profil fénykép vagy adja hozzá a felhasználót emblémát jelölőnégyzetét, kattintson a **kijelölt** listája.</span><span class="sxs-lookup"><span data-stu-id="82752-162">Click the checkbox next to the user’s profile photo or logo to add your user to the **Selected** list.</span></span>

12. <span data-ttu-id="82752-163">**Választható lehetőség:** Ha azt szeretné, hogy **egynél több felhasználó hozzáadása**, egy másik típus **teljes név** vagy **e-mail cím** be a **Keresés név vagy e-mail cím alapján** mező, és a jelölőnégyzet bejelölésével adja hozzá a felhasználót, hogy a **kijelölt** listája.</span><span class="sxs-lookup"><span data-stu-id="82752-163">**Optional:** If you would like to **add more than one user**, type in another **full name** or **email address** into the **Search by name or email address** search box, and click the checkbox to add this user to the **Selected** list.</span></span>

13. <span data-ttu-id="82752-164">Ha elkészült, válassza a felhasználók, kattintson a **válasszon** gombra kattintva vegye fel a listára a felhasználók és csoportok hozzá kell rendelni az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="82752-164">When you are finished selecting users, click the **Select** button to add them to the list of users and groups to be assigned to the application.</span></span>

14. <span data-ttu-id="82752-165">**Választható lehetőség:** kattintson a **Szerepkörválasztás** a választó a **hozzáadása hozzárendelés** hozzárendelése a kiválasztott felhasználói szerepkör kiválasztása panel.</span><span class="sxs-lookup"><span data-stu-id="82752-165">**Optional:** click the **Select Role** selector in the **Add Assignment** blade to select a role to assign to the users you have selected.</span></span>

15. <span data-ttu-id="82752-166">Kattintson a **hozzárendelése** gombra kattintva a kijelölt felhasználók az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="82752-166">Click the **Assign** button to assign the application to the selected users.</span></span>

<span data-ttu-id="82752-167">Rövid időn belül a kijelölt felhasználók tudják elindítani ezeket az alkalmazásokat a megoldás leírása szakaszban ismertetett módszerekkel.</span><span class="sxs-lookup"><span data-stu-id="82752-167">After a short period of time, the users you have selected be able to launch these applications using the methods described in the solution description section.</span></span>

## <a name="not-a-valid-saml-request"></a><span data-ttu-id="82752-168">Nem egy érvényes SAML-kérés</span><span class="sxs-lookup"><span data-stu-id="82752-168">Not a valid SAML Request</span></span>

<span data-ttu-id="82752-169">*AADSTS75005. hiba: A kérelme, mert nem egy Saml2 protokoll érvényes üzenetet.*</span><span class="sxs-lookup"><span data-stu-id="82752-169">*Error AADSTS75005: The request is not a valid Saml2 protocol message.*</span></span>

<span data-ttu-id="82752-170">**Lehetséges ok**</span><span class="sxs-lookup"><span data-stu-id="82752-170">**Possible cause**</span></span>

<span data-ttu-id="82752-171">Az Azure AD nem támogatja az egyszeri bejelentkezést az alkalmazás által küldött SAML-kérelem.</span><span class="sxs-lookup"><span data-stu-id="82752-171">Azure AD doesn’t support the SAML Request sent by the application for Single Sign-on.</span></span> <span data-ttu-id="82752-172">Néhány gyakori kérdések a következők:</span><span class="sxs-lookup"><span data-stu-id="82752-172">Some common issues are:</span></span>

-   <span data-ttu-id="82752-173">A SAML-kérelmet a kötelező mezők hiányoznak</span><span class="sxs-lookup"><span data-stu-id="82752-173">Missing required fields in the SAML request</span></span>

-   <span data-ttu-id="82752-174">SAML kódolt kérelmek módja</span><span class="sxs-lookup"><span data-stu-id="82752-174">SAML request encoded method</span></span>

<span data-ttu-id="82752-175">**Megoldás**</span><span class="sxs-lookup"><span data-stu-id="82752-175">**Resolution**</span></span>

1.  <span data-ttu-id="82752-176">Rögzítse a SAML-kérelmet.</span><span class="sxs-lookup"><span data-stu-id="82752-176">Capture SAML request.</span></span> <span data-ttu-id="82752-177">az útmutató bevezeti Önt [hibakeresés SAML-alapú egyszeri bejelentkezés alkalmazásokhoz az Azure AD-ben](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-debugging) megtudhatja, hogyan rögzítheti a SAML-kérelmet.</span><span class="sxs-lookup"><span data-stu-id="82752-177">follow the tutorial [How to debug SAML-based single sign-on to applications in Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-debugging) to learn how to capture the SAML request.</span></span>

2.  <span data-ttu-id="82752-178">Lépjen kapcsolatba az alkalmazás gyártójának és megosztás:</span><span class="sxs-lookup"><span data-stu-id="82752-178">Contact the application vendor and share:</span></span>

   -   <span data-ttu-id="82752-179">SAML-kérelmet</span><span class="sxs-lookup"><span data-stu-id="82752-179">SAML request</span></span>

   -   [<span data-ttu-id="82752-180">Azure AD-egyszeri bejelentkezés SAML protokoll követelmények</span><span class="sxs-lookup"><span data-stu-id="82752-180">Azure AD Single Sign-on SAML protocol requirements</span></span>](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference)

<span data-ttu-id="82752-181">Ellenőrizni kell az egyszeri bejelentkezés az Azure AD SAML-alapú megvalósítás támogatják.</span><span class="sxs-lookup"><span data-stu-id="82752-181">They should validate they support the Azure AD SAML implementation for Single Sign-on.</span></span>

## <a name="no-resource-in-requiredresourceaccess-list"></a><span data-ttu-id="82752-182">Nincs erőforrás requiredResourceAccess listában</span><span class="sxs-lookup"><span data-stu-id="82752-182">No resource in requiredResourceAccess list</span></span>

<span data-ttu-id="82752-183">*Hiba AADSTS65005: az ügyfélalkalmazás kért erőforrás elérésére "00000002-0000-0000-c000-000000000000'. A kérelem sikertelen volt, mert az ügyfél nem megadva ehhez az erőforráshoz requiredResourceAccess listáján található*.</span><span class="sxs-lookup"><span data-stu-id="82752-183">*Error AADSTS65005:The client application has requested access to resource '00000002-0000-0000-c000-000000000000'. This request has failed because the client has not specified this resource in its requiredResourceAccess list*.</span></span>

<span data-ttu-id="82752-184">**Lehetséges ok**</span><span class="sxs-lookup"><span data-stu-id="82752-184">**Possible cause**</span></span>

<span data-ttu-id="82752-185">Az application objektum sérült.</span><span class="sxs-lookup"><span data-stu-id="82752-185">The application object is corrupted.</span></span>

<span data-ttu-id="82752-186">**Megoldás: 1. lehetőséget**</span><span class="sxs-lookup"><span data-stu-id="82752-186">**Resolution: option 1**</span></span>

<span data-ttu-id="82752-187">A probléma megoldásához, adja hozzá az Azure Active Directory beállítása az egyedi azonosító értéket.</span><span class="sxs-lookup"><span data-stu-id="82752-187">To solve the problem, add the unique Identifier value in the Azure AD configuration.</span></span> <span data-ttu-id="82752-188">Egy azonosító értéket hozzáadásához kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="82752-188">To add a Identifier value, follow the steps below:</span></span>

1.  <span data-ttu-id="82752-189">Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="82752-189">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="82752-190">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="82752-190">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="82752-191">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="82752-191">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="82752-192">Kattintson a **vállalati alkalmazások** az Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="82752-192">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="82752-193">Kattintson a **összes alkalmazás** az alkalmazások listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="82752-193">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="82752-194">Ha azt szeretné, hogy itt megjelennek az alkalmazás nem látja, használja a **szűrő** vezérlő tetején a **összes alkalmazások listáját** és állítsa be a **megjelenítése** lehetőséggel **összes alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="82752-194">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="82752-195">Válassza ki az alkalmazást, az egyszeri bejelentkezés konfigurálta.</span><span class="sxs-lookup"><span data-stu-id="82752-195">Select the application you have configured single sign-on.</span></span>

7.  <span data-ttu-id="82752-196">Ha az alkalmazás betölt, kattintson a a **egyszeri bejelentkezés** az alkalmazás bal oldali navigációs menü</span><span class="sxs-lookup"><span data-stu-id="82752-196">Once the application loads, click on the **Single sign-on** from the application’s left hand navigation menu</span></span>

8.  <span data-ttu-id="82752-197">A a **tartomány és az URL-cím** területen jelölje be a **megjelenítése speciális URL-beállításainak**.</span><span class="sxs-lookup"><span data-stu-id="82752-197">Under the **Domain and URL** section, check on the **Show advanced URL settings**.</span></span>

9.  <span data-ttu-id="82752-198">az a **azonosító** szövegmezőbe írja be az alkalmazás egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="82752-198">in the **Identifier** textbox type a unique identifier for the application.</span></span>

10. <span data-ttu-id="82752-199">**Mentés** konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="82752-199">**Save** the configuration.</span></span>


<span data-ttu-id="82752-200">**Névfeloldási beállítást 2**</span><span class="sxs-lookup"><span data-stu-id="82752-200">**Resolution option 2**</span></span>

<span data-ttu-id="82752-201">Ha a fenti 1 beállítás nem működik az Ön, távolítsa el az alkalmazás a könyvtárból.</span><span class="sxs-lookup"><span data-stu-id="82752-201">If option 1 above did not work for you, try removing the application from the directory.</span></span> <span data-ttu-id="82752-202">Adja hozzá, és konfigurálja újra az alkalmazást, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="82752-202">Then, add and reconfigure the application, follow the steps below:</span></span>

1.  <span data-ttu-id="82752-203">Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="82752-203">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="82752-204">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="82752-204">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="82752-205">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="82752-205">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="82752-206">Kattintson a **vállalati alkalmazások** az Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="82752-206">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="82752-207">Kattintson a **összes alkalmazás** az alkalmazások listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="82752-207">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="82752-208">Ha azt szeretné, hogy itt megjelennek az alkalmazás nem látja, használja a **szűrő** vezérlő tetején a **összes alkalmazások listáját** és állítsa be a **megjelenítése** lehetőséggel **összes alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="82752-208">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="82752-209">Válassza ki az egyszeri bejelentkezés konfigurálni kívánt alkalmazást</span><span class="sxs-lookup"><span data-stu-id="82752-209">Select the application you want to configure single sign-on</span></span>

7.  <span data-ttu-id="82752-210">Kattintson a **törlése** , a bal felső az alkalmazás **áttekintése** panelen.</span><span class="sxs-lookup"><span data-stu-id="82752-210">Click **Delete** at the top-left of the application **Overview** blade.</span></span>

8.  <span data-ttu-id="82752-211">Frissítse az Azure AD, és vegye fel az alkalmazást az Azure AD-galériából.</span><span class="sxs-lookup"><span data-stu-id="82752-211">Refresh Azure AD and Add the application from the Azure AD gallery.</span></span> <span data-ttu-id="82752-212">Ezután konfigurálja az alkalmazást</span><span class="sxs-lookup"><span data-stu-id="82752-212">Then, Configure the application</span></span>

<span data-ttu-id="82752-213"><span id="_Hlk477190176" class="anchor"></span>Az alkalmazás újbóli beállításához után jelentkezhet be az alkalmazásba kell lennie.</span><span class="sxs-lookup"><span data-stu-id="82752-213"><span id="_Hlk477190176" class="anchor"></span>After reconfiguring the application, you should be able to sign in to the application.</span></span>

## <a name="certificate-or-key-not-configured"></a><span data-ttu-id="82752-214">Tanúsítvány és kulcs nincs konfigurálva</span><span class="sxs-lookup"><span data-stu-id="82752-214">Certificate or key not configured</span></span>

<span data-ttu-id="82752-215">*Hiba AADSTS50003: Nincs aláírási kulcs konfigurálva.*</span><span class="sxs-lookup"><span data-stu-id="82752-215">*Error AADSTS50003: No signing key configured.*</span></span>

<span data-ttu-id="82752-216">**Lehetséges ok**</span><span class="sxs-lookup"><span data-stu-id="82752-216">**Possible cause**</span></span>

<span data-ttu-id="82752-217">Az application objektum sérült, és az Azure AD nem ismeri fel az alkalmazáshoz beállított tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="82752-217">The application object is corrupted and Azure AD doesn’t recognize the certificate configured for the application.</span></span>

<span data-ttu-id="82752-218">**Megoldás**</span><span class="sxs-lookup"><span data-stu-id="82752-218">**Resolution**</span></span>

<span data-ttu-id="82752-219">Törölje, majd hozzon létre egy új tanúsítványt, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="82752-219">To delete and create a new certificate, follow the steps below:</span></span>

1.  <span data-ttu-id="82752-220">Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="82752-220">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="82752-221">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="82752-221">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="82752-222">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="82752-222">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="82752-223">Kattintson a **vállalati alkalmazások** az Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="82752-223">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="82752-224">Kattintson a **összes alkalmazás** az alkalmazások listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="82752-224">click **All Applications** to view a list of all your applications.</span></span>

 * <span data-ttu-id="82752-225">Ha azt szeretné, hogy itt megjelennek az alkalmazás nem látja, használja a **szűrő** vezérlő tetején a **összes alkalmazások listáját** és állítsa be a **megjelenítése** lehetőséggel **összes alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="82752-225">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="82752-226">Válassza ki az egyszeri bejelentkezés konfigurálni kívánt alkalmazást</span><span class="sxs-lookup"><span data-stu-id="82752-226">Select the application you want to configure single sign-on</span></span>

7.  <span data-ttu-id="82752-227">Ha az alkalmazás betölt, kattintson a **egyszeri bejelentkezés** az alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="82752-227">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="82752-228">Kattintson a **hozzon létre új tanúsítvány** alatt a **aláíró tanúsítvány SAML** szakasz.</span><span class="sxs-lookup"><span data-stu-id="82752-228">click **Create new certificate** under the **SAML signing Certificate** section.</span></span>

9.  <span data-ttu-id="82752-229">Válassza ki a lejárati dátum.</span><span class="sxs-lookup"><span data-stu-id="82752-229">Select Expiration date.</span></span> <span data-ttu-id="82752-230">Kattintson a **mentéséhez.**</span><span class="sxs-lookup"><span data-stu-id="82752-230">Then, click **Save.**</span></span>

10. <span data-ttu-id="82752-231">Ellenőrizze **új tanúsítvány aktiválásához** az aktív tanúsítvány felülbírálására.</span><span class="sxs-lookup"><span data-stu-id="82752-231">Check **Make new certificate active** to override the active certificate.</span></span> <span data-ttu-id="82752-232">Kattintson a **mentése** a panel tetején, és fogadja el a helyettesítő tanúsítvány aktiválásához.</span><span class="sxs-lookup"><span data-stu-id="82752-232">Then, click **Save** at the top of the blade and accept to activate the rollover certificate.</span></span>

11. <span data-ttu-id="82752-233">Az a **SAML-aláíró tanúsítványa** területén kattintson **eltávolítása** eltávolítása a **nem használt** tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="82752-233">Under the **SAML Signing Certificate** section, click **remove** to remove the **Unused** certificate.</span></span>

## <a name="problem-when-customizing-the-saml-claims-sent-to-an-application"></a><span data-ttu-id="82752-234">Probléma a alkalmazás küldött SAML-jogcímek testreszabása</span><span class="sxs-lookup"><span data-stu-id="82752-234">Problem when customizing the SAML claims sent to an application</span></span>

<span data-ttu-id="82752-235">Megtudhatja, hogyan szabhatja testre a SAML attribútum típusú jogcímek az alkalmazás számára, lásd: [hozzárendelése az Azure Active Directory-jogcímek](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) további információt.</span><span class="sxs-lookup"><span data-stu-id="82752-235">To learn how to customize the SAML attribute claims sent to your application, see [Claims mapping in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) for more information.</span></span>

## <a name="next-steps"></a><span data-ttu-id="82752-236">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="82752-236">Next steps</span></span>
[<span data-ttu-id="82752-237">Hibakeresés SAML-alapú egyszeri bejelentkezés alkalmazásokhoz az Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="82752-237">How to debug SAML-based single sign-on to applications in Azure AD</span></span>](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-debugging)
