---
title: "Felhasználók és csoportok hozzárendelése egy alkalmazás |} Microsoft Docs"
description: "Felhasználók hozzárendelése az alkalmazáshoz való hozzáférés engedélyezése"
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
ms.openlocfilehash: 61536612e0dd5102b8f5e911c350826846f5ed77
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-assign-users-and-groups-to-an-application"></a><span data-ttu-id="4285f-103">Felhasználók és csoportok hozzárendelése egy alkalmazás</span><span class="sxs-lookup"><span data-stu-id="4285f-103">How to assign users and groups to an application</span></span>

<span data-ttu-id="4285f-104">Mielőtt a felhasználók lehetőségek közül választhat a alább egy adott alkalmazáshoz kell első **rendeli azokat az alkalmazás** való hozzáférés engedélyezése:</span><span class="sxs-lookup"><span data-stu-id="4285f-104">Before your users can do any of the below for a specific application, you need to first **assign them to the application** to grant them access:</span></span>

-   <span data-ttu-id="4285f-105">Az alkalmazás által elérése **közvetlenül az alkalmazás URL-re navigáláskor** (más néven Szolgáltató által kezdeményezett bejelentkezés).</span><span class="sxs-lookup"><span data-stu-id="4285f-105">Access an application by **navigating to the application’s URL directly** (also known as SP-initiated sign-on).</span></span>

-   <span data-ttu-id="4285f-106">Egy alkalmazás segítségével a **felhasználói URL-CÍMEN** olyan alkalmazást **tulajdonságok** (más néven IDP által kezdeményezett bejelentkezés) lap.</span><span class="sxs-lookup"><span data-stu-id="4285f-106">Access an application by using the **User Access URL** on an application’s **Properties** page (also known as IDP-initiated sign on).</span></span>

-   <span data-ttu-id="4285f-107">Tekintse meg az alkalmazás jelennek meg azok [alkalmazás hozzáférési Panel](https://myapps.microsoft.com/) vagy mobilalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="4285f-107">See an application appear on their [Application Access Panel](https://myapps.microsoft.com/) or mobile application.</span></span>

-   <span data-ttu-id="4285f-108">Tekintse meg az alkalmazás jelennek meg azok [Office 365 alkalmazásindító](https://support.office.com/article/Meet-the-Office-365-app-launcher-79f12104-6fed-442f-96a0-eb089a3f476a).</span><span class="sxs-lookup"><span data-stu-id="4285f-108">See an application appear on their [Office 365 Application Launcher](https://support.office.com/article/Meet-the-Office-365-app-launcher-79f12104-6fed-442f-96a0-eb089a3f476a).</span></span>

## <a name="methods-to-assign-applications-with-azure-active-directory"></a><span data-ttu-id="4285f-109">Alkalmazások az Azure Active Directoryval hozzárendelni módszerek</span><span class="sxs-lookup"><span data-stu-id="4285f-109">Methods to assign applications with Azure Active Directory</span></span> 

<span data-ttu-id="4285f-110">Alkalmazások az Azure Active Directoryval rendelhet 3 módja van:</span><span class="sxs-lookup"><span data-stu-id="4285f-110">There are 3 ways you can assign applications with Azure Active Directory:</span></span>

-   [<span data-ttu-id="4285f-111">Felhasználó hozzárendelése közvetlenül egy alkalmazás rendszergazdaként</span><span class="sxs-lookup"><span data-stu-id="4285f-111">Assign a user directly to an application as an administrator</span></span>](#assign-a-user-directly-as-an-administrator)

-   [<span data-ttu-id="4285f-112">Rendszergazdaként alkalmazáshoz való közvetlenül is kiadhatjuk egy csoportnak</span><span class="sxs-lookup"><span data-stu-id="4285f-112">Assign a group directly to an application as an administrator</span></span>](#assign-a-group-directly-to-an-application-as-an-administrator)

-   [<span data-ttu-id="4285f-113">A felhasználók a saját alkalmazások keresése az önkiszolgáló alkalmazás hozzáférésének engedélyezése</span><span class="sxs-lookup"><span data-stu-id="4285f-113">Enable self-service application access to allow users to find their own applications</span></span>](#enable-self-service-application-access-to-allow-users-to-find-their-own-applications)

## <a name="assign-a-user-directly-as-an-administrator"></a><span data-ttu-id="4285f-114">Rendelje hozzá a felhasználó közvetlenül rendszergazdaként</span><span class="sxs-lookup"><span data-stu-id="4285f-114">Assign a user directly as an administrator</span></span>

<span data-ttu-id="4285f-115">Hozzárendelése egy vagy több felhasználó alkalmazás közvetlenül, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="4285f-115">To assign one or more users to an application directly, follow the steps below:</span></span>

1.  <span data-ttu-id="4285f-116">Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="4285f-116">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="4285f-117">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="4285f-117">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="4285f-118">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="4285f-118">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="4285f-119">Kattintson a **vállalati alkalmazások** az Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="4285f-119">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="4285f-120">Kattintson a **összes alkalmazás** az alkalmazások listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="4285f-120">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="4285f-121">Ha azt szeretné, hogy itt megjelennek az alkalmazás nem látja, használja a **szűrő** vezérlő tetején a **összes alkalmazások listáját** és állítsa be a **megjelenítése** lehetőséggel **összes alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="4285f-121">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="4285f-122">Válassza ki szeretné osztani a felhasználót, hogy a listában az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="4285f-122">Select the application you want to assign a user to from the list.</span></span>

7.  <span data-ttu-id="4285f-123">Ha az alkalmazás betölt, kattintson **felhasználók és csoportok** az alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="4285f-123">Once the application loads, click **Users and Groups** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="4285f-124">Kattintson a **Hozzáadás** gombra kattint, a a **felhasználók és csoportok** nyissa meg a listában a **hozzáadása hozzárendelés** panelen.</span><span class="sxs-lookup"><span data-stu-id="4285f-124">Click the **Add** button on top of the **Users and Groups** list to open the **Add Assignment** blade.</span></span>

9.  <span data-ttu-id="4285f-125">Kattintson a **felhasználók és csoportok** a választó a **hozzáadása hozzárendelés** panelen.</span><span class="sxs-lookup"><span data-stu-id="4285f-125">click the **Users and groups** selector from the **Add Assignment** blade.</span></span>

10. <span data-ttu-id="4285f-126">Írja be a **teljes név** vagy **e-mail cím** érdekli hozzárendelése a felhasználó a **Keresés név vagy e-mail cím alapján** keresőmezőbe.</span><span class="sxs-lookup"><span data-stu-id="4285f-126">Type in the **full name** or **email address** of the user you are interested in assigning into the **Search by name or email address** search box.</span></span>

11. <span data-ttu-id="4285f-127">Vigye a **felhasználói** a listában, hogy láthatóvá váljon a **jelölőnégyzet**.</span><span class="sxs-lookup"><span data-stu-id="4285f-127">Hover over the **user** in the list to reveal a **checkbox**.</span></span> <span data-ttu-id="4285f-128">A felhasználói profil fénykép vagy adja hozzá a felhasználót emblémát jelölőnégyzetét, kattintson a **kijelölt** listája.</span><span class="sxs-lookup"><span data-stu-id="4285f-128">Click the checkbox next to the user’s profile photo or logo to add your user to the **Selected** list.</span></span>

12. <span data-ttu-id="4285f-129">**Választható lehetőség:** Ha azt szeretné, hogy **egynél több felhasználó hozzáadása**, egy másik típus **teljes név** vagy **e-mail cím** be a **Keresés név vagy e-mail cím alapján** mező, és a jelölőnégyzet bejelölésével adja hozzá a felhasználót, hogy a **kijelölt** listája.</span><span class="sxs-lookup"><span data-stu-id="4285f-129">**Optional:** If you would like to **add more than one user**, type in another **full name** or **email address** into the **Search by name or email address** search box, and click the checkbox to add this user to the **Selected** list.</span></span>

13. <span data-ttu-id="4285f-130">Ha elkészült, válassza a felhasználók, kattintson a **válasszon** gombra kattintva vegye fel a listára a felhasználók és csoportok hozzá kell rendelni az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="4285f-130">When you are finished selecting users, click the **Select** button to add them to the list of users and groups to be assigned to the application.</span></span>

14. <span data-ttu-id="4285f-131">**Választható lehetőség:** kattintson a **Szerepkörválasztás** a választó a **hozzáadása hozzárendelés** hozzárendelése a kiválasztott felhasználói szerepkör kiválasztása panel.</span><span class="sxs-lookup"><span data-stu-id="4285f-131">**Optional:** click the **Select Role** selector in the **Add Assignment** blade to select a role to assign to the users you have selected.</span></span>

15. <span data-ttu-id="4285f-132">Kattintson a **hozzárendelése** gombra kattintva a kijelölt felhasználók az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="4285f-132">Click the **Assign** button to assign the application to the selected users.</span></span>

<span data-ttu-id="4285f-133">Rövid időn belül a kijelölt felhasználók tudják elindítani ezeket az alkalmazásokat a megoldás leírása szakaszban ismertetett módszerekkel.</span><span class="sxs-lookup"><span data-stu-id="4285f-133">After a short period of time, the users you have selected be able to launch these applications using the methods described in the solution description section.</span></span>

## <a name="assign-a-group-directly-to-an-application-as-an-administrator"></a><span data-ttu-id="4285f-134">Rendszergazdaként alkalmazáshoz való közvetlenül is kiadhatjuk egy csoportnak</span><span class="sxs-lookup"><span data-stu-id="4285f-134">Assign a group directly to an application as an administrator</span></span>

<span data-ttu-id="4285f-135">Közvetlenül egy alkalmazás hozzárendelése egy vagy több csoportot, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="4285f-135">To assign one or more groups to an application directly, follow the steps below:</span></span>

1.  <span data-ttu-id="4285f-136">Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="4285f-136">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="4285f-137">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="4285f-137">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="4285f-138">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="4285f-138">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="4285f-139">Kattintson a **vállalati alkalmazások** az Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="4285f-139">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="4285f-140">Kattintson a **összes alkalmazás** az alkalmazások listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="4285f-140">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="4285f-141">Ha azt szeretné, hogy itt megjelennek az alkalmazás nem látja, használja a **szűrő** vezérlő tetején a **összes alkalmazások listáját** és állítsa be a **megjelenítése** lehetőséggel **összes alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="4285f-141">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="4285f-142">Válassza ki szeretné osztani a felhasználót, hogy a listában az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="4285f-142">Select the application you want to assign a user to from the list.</span></span>

7.  <span data-ttu-id="4285f-143">Ha az alkalmazás betölt, kattintson **felhasználók és csoportok** az alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="4285f-143">Once the application loads, click **Users and Groups** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="4285f-144">Kattintson a **Hozzáadás** gombra kattint, a a **felhasználók és csoportok** nyissa meg a listában a **hozzáadása hozzárendelés** panelen.</span><span class="sxs-lookup"><span data-stu-id="4285f-144">Click the **Add** button on top of the **Users and Groups** list to open the **Add Assignment** blade.</span></span>

9.  <span data-ttu-id="4285f-145">Kattintson a **felhasználók és csoportok** a választó a **hozzáadása hozzárendelés** panelen.</span><span class="sxs-lookup"><span data-stu-id="4285f-145">click the **Users and groups** selector from the **Add Assignment** blade.</span></span>

10. <span data-ttu-id="4285f-146">Írja be a **teljes csoportnév** érdekli való hozzárendelése a csoport a **Keresés név vagy e-mail cím alapján** keresőmezőbe.</span><span class="sxs-lookup"><span data-stu-id="4285f-146">Type in the **full group name** of the group you are interested in assigning into the **Search by name or email address** search box.</span></span>

11. <span data-ttu-id="4285f-147">Vigye a **csoport** a listában, hogy láthatóvá váljon a **jelölőnégyzet**.</span><span class="sxs-lookup"><span data-stu-id="4285f-147">Hover over the **group** in the list to reveal a **checkbox**.</span></span> <span data-ttu-id="4285f-148">A csoport profilképet vagy adja hozzá a felhasználót emblémát jelölőnégyzetét, kattintson a **kijelölt** listája.</span><span class="sxs-lookup"><span data-stu-id="4285f-148">Click the checkbox next to the group’s profile photo or logo to add your user to the **Selected** list.</span></span>

12. <span data-ttu-id="4285f-149">**Nem kötelező:** Ha azt szeretné, hogy **egynél több csoport hozzáadása**, egy másik típus **teljes csoportnév** be a **Keresés név vagy e-mail cím alapján** mező, és kattintson a jelölőnégyzetbe, a csoport hozzáadása a **kijelölt** lista.</span><span class="sxs-lookup"><span data-stu-id="4285f-149">**Optional:** If you would like to **add more than one group**, type in another **full group name** into the **Search by name or email address** search box, and click the checkbox to add this group to the **Selected** list.</span></span>

13. <span data-ttu-id="4285f-150">Ha befejezte a csoportok kiválasztásával, kattintson a **válasszon** gombra kattintva vegye fel a listára a felhasználók és csoportok hozzá kell rendelni az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="4285f-150">When you are finished selecting groups, click the **Select** button to add them to the list of users and groups to be assigned to the application.</span></span>

14. <span data-ttu-id="4285f-151">**Választható lehetőség:** kattintson a **Szerepkörválasztás** a választó a **hozzáadása hozzárendelés** panelt, és válassza ki a szerepkör hozzárendelése a kijelölt csoportok.</span><span class="sxs-lookup"><span data-stu-id="4285f-151">**Optional:** click the **Select Role** selector in the **Add Assignment** blade to select a role to assign to the groups you have selected.</span></span>

15. <span data-ttu-id="4285f-152">Kattintson a **hozzárendelése** gombra az alkalmazás a kiválasztott csoportok számára.</span><span class="sxs-lookup"><span data-stu-id="4285f-152">Click the **Assign** button to assign the application to the selected groups.</span></span>

<span data-ttu-id="4285f-153">Rövid időn belül a kijelölt csoportok belül a felhasználók tudják elindítani ezeket az alkalmazásokat a megoldás leírása szakaszban ismertetett módszerekkel.</span><span class="sxs-lookup"><span data-stu-id="4285f-153">After a short period of time, the users within the groups you have selected be able to launch these applications using the methods described in the solution description section.</span></span> <span data-ttu-id="4285f-154">Ha ezek a dinamikus csoportok, néhány további feldolgozás késés előfordulhat ezek a felhasználók számára csoportok magukba belül szereplő hozzárendelésekben.</span><span class="sxs-lookup"><span data-stu-id="4285f-154">If these are dynamic groups, there may be some additional processing delay in these assignments appearing for users within these assigned groups.</span></span>

## <a name="enable-self-service-application-access-to-allow-users-to-find-their-own-applications"></a><span data-ttu-id="4285f-155">A felhasználók a saját alkalmazások keresése az önkiszolgáló alkalmazás hozzáférésének engedélyezése</span><span class="sxs-lookup"><span data-stu-id="4285f-155">Enable self-service application access to allow users to find their own applications</span></span>

<span data-ttu-id="4285f-156">Önkiszolgáló alkalmazás-hozzáférés kiváló módja annak, hogy önálló felderítéséhez az alkalmazások, felhasználók igény szerint engedélyezett az üzleti csoport ezeket az alkalmazásokat a hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="4285f-156">Self-service application access is a great way to allow users to self-discover applications, optionally allow the business group to approve access to those applications.</span></span> <span data-ttu-id="4285f-157">Engedélyezze az üzleti csoport társítva a hozzáférési panel azoknak a felhasználóknak jelszót egyszeri bejelentkezést az alkalmazások jobb a hitelesítő adatok kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="4285f-157">You can allow the business group to manage the credentials assigned to those users for Password Single-Sign On Applications right from their access panels.</span></span>

<span data-ttu-id="4285f-158">Ahhoz, hogy az alkalmazás önkiszolgáló hozzáférést egy alkalmazáshoz, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="4285f-158">To enable self-service application access to an application, follow the steps below:</span></span>

1.  <span data-ttu-id="4285f-159">Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="4285f-159">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="4285f-160">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="4285f-160">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="4285f-161">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="4285f-161">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="4285f-162">Kattintson a **vállalati alkalmazások** az Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="4285f-162">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="4285f-163">Kattintson a **összes alkalmazás** az alkalmazások listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="4285f-163">click **All Applications** to view a list of all your applications.</span></span>

   * <span data-ttu-id="4285f-164">Ha azt szeretné, hogy itt megjelennek az alkalmazás nem látja, használja a **szűrő** vezérlő tetején a **összes alkalmazások listáját** és állítsa be a **megjelenítése** lehetőséggel **összes alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="4285f-164">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="4285f-165">Válassza ki az önkiszolgáló engedélyezni szeretné a hozzáférést a listából.</span><span class="sxs-lookup"><span data-stu-id="4285f-165">Select the application you want to enable Self-service access to from the list.</span></span>

7.  <span data-ttu-id="4285f-166">Ha az alkalmazás betölt, kattintson **önkiszolgáló** az alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="4285f-166">Once the application loads, click **Self-service** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="4285f-167">Ahhoz, hogy az önkiszolgáló alkalmazás hozzáférése az alkalmazáshoz, kapcsolja be a **az alkalmazáshoz való hozzáférés kérését?** kapcsolót **igen.**</span><span class="sxs-lookup"><span data-stu-id="4285f-167">To enable Self-service application access for this application, turn the **Allow users to request access to this application?** toggle to **Yes.**</span></span>

9.  <span data-ttu-id="4285f-168">A következő, amelyekhez a felhasználók, akik kérnek az alkalmazáshoz való hozzáférés hozzá kell adni a csoport kijelöléséhez kattintson a felirat melletti választó **mely csoporthoz rendelt felhasználók vehet fel?** válasszon ki egy csoportot.</span><span class="sxs-lookup"><span data-stu-id="4285f-168">Next, to select the group to which users who request access to this application should be added, click the selector next to the label **To which group should assigned users be added?** and select a group.</span></span>

10. <span data-ttu-id="4285f-169">**Választható lehetőség:** előtt egy üzleti jóváhagyás megkövetelése, ha a felhasználók jogosultak-e a hozzáférést, és állítsa a **az alkalmazáshoz való hozzáférés megadása előtt jóvá kell hagyni?** kapcsolót **Igen**.</span><span class="sxs-lookup"><span data-stu-id="4285f-169">**Optional:** If you wish to require a business approval before users are allowed access, set the **Require approval before granting access to this application?** toggle to **Yes**.</span></span>

11. <span data-ttu-id="4285f-170">**Választható lehetőség: az alkalmazások csak a jelszó egyszeri bejelentkezés használatával** adott üzleti jóváhagyóknak adhatja meg a jelszavakat, a jóváhagyott felhasználók számára az alkalmazás küldött engedélyezni szeretné, ha a **engedélyezése az alkalmazás felhasználói jelszavak beállítása jóváhagyóknak?** kapcsolót **Igen**.</span><span class="sxs-lookup"><span data-stu-id="4285f-170">**Optional: For applications using password single-sign on only,** if you wish to allow those business approvers to specify the passwords that are sent to this application for approved users, set the **Allow approvers to set user’s passwords for this application?** toggle to **Yes**.</span></span>

12. <span data-ttu-id="4285f-171">**Választható lehetőség:** az üzleti jóváhagyóknak, akik jogosultak a hagyja jóvá az alkalmazáshoz való hozzáférés megadásához kattintson a felirat melletti választó **ki jogosult az alkalmazáshoz való hozzáférés jóváhagyásához?** legfeljebb 10 egyéni üzleti jóváhagyóknak kiválasztásához.</span><span class="sxs-lookup"><span data-stu-id="4285f-171">**Optional:** To specify the business approvers who are allowed to approve access to this application, click the selector next to the label **Who is allowed to approve access to this application?** to select up to 10 individual business approvers.</span></span>

  >[!NOTE]
  ><span data-ttu-id="4285f-172">Csoportok használata nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="4285f-172">Groups are not supported.</span></span>
  >
  >

13. <span data-ttu-id="4285f-173">**Választható lehetőség:** **az alkalmazások, amelyeknél a szerepkörök**, ha önkiszolgáló jóváhagyott felhasználók hozzárendelése egy szerepkörhöz, kattintson a Tovább gombra a választó a **mely szerepkörhöz felhasználók hozzárendeli az alkalmazásban?** válassza ki a szerepkört, amelyhez hozzá kell rendelni a felhasználók számára.</span><span class="sxs-lookup"><span data-stu-id="4285f-173">**Optional:** **For applications which expose roles**, if you wish to assign self-service approved users to a role, click the selector next to the **To which role should users be assigned in this application?** to select the role to which these users should be assigned.</span></span>

14. <span data-ttu-id="4285f-174">Kattintson a **mentése** gombra a befejezéshez panel tetején.</span><span class="sxs-lookup"><span data-stu-id="4285f-174">Click the **Save** button at the top of the blade to finish.</span></span>

<span data-ttu-id="4285f-175">Ha befejezte az önkiszolgáló Alkalmazáskonfiguráció, felhasználók lépjen a [alkalmazás hozzáférési Panel](https://myapps.microsoft.com/) , és kattintson a **+ Hozzáadás** gombra kattintva keresse meg az alkalmazások, amelyhez engedélyezte a hozzáférést az önkiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="4285f-175">Once you complete Self-service application configuration, users can navigate to their [Application Access Panel](https://myapps.microsoft.com/) and click the **+Add** button to find the apps to which you have enabled Self-service access.</span></span> <span data-ttu-id="4285f-176">Üzleti jóváhagyóknak is megjelenik egy értesítés a saját [alkalmazás hozzáférési Panel](https://myapps.microsoft.com/).</span><span class="sxs-lookup"><span data-stu-id="4285f-176">Business approvers also see a notification in their [Application Access Panel](https://myapps.microsoft.com/).</span></span> <span data-ttu-id="4285f-177">Egy e-mailt, amely értesíti őket, amikor a felhasználó által kért a jóváhagyást igénylő alkalmazáshoz való hozzáférés engedélyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="4285f-177">You can enable an email notifying them when a user has requested access to an application that requires their approval.</span></span> 

<span data-ttu-id="4285f-178">Ezek a jóváhagyások támogatja egyetlen jóváhagyási munkafolyamatok csak, ami azt jelenti, hogy több jóváhagyó ad meg, ha bármely egyetlen jóváhagyó előfordulhat, hogy jóváhagyó hozzáférni az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="4285f-178">These approvals support single approval workflows only, meaning that if you specify multiple approvers, any single approver may approver access to the application.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4285f-179">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4285f-179">Next steps</span></span>
[<span data-ttu-id="4285f-180">Adja meg az egyszeri bejelentkezés az alkalmazásokba a Proxy</span><span class="sxs-lookup"><span data-stu-id="4285f-180">Provide single sign-on to your apps with Application Proxy</span></span>](active-directory-application-proxy-sso-using-kcd.md)
