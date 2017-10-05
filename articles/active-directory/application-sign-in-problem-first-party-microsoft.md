---
title: "Bejelentkezés Microsoft-alkalmazáshoz problémák |} Microsoft Docs"
description: "A belső Microsoft Applications (például Office 365) az Azure AD használatával történő bejelentkezés során tapasztalt gyakori problémák elhárítása"
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
ms.openlocfilehash: 5638434270ee82d2b9737ea8eed8b5a8c62f7121
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
## <a name="problems-signing-in-to-a-microsoft-application"></a><span data-ttu-id="41e3b-103">Bejelentkezés Microsoft-alkalmazáshoz problémák</span><span class="sxs-lookup"><span data-stu-id="41e3b-103">Problems signing in to a Microsoft application</span></span>

<span data-ttu-id="41e3b-104">Microsoft Applications (például Office 365 Exchange, SharePoint, Yammer, stb.) rendelve, és egy kicsit másképp, mint 3. fél SaaS-alkalmazások vagy más alkalmazásokat, az egyszeri bejelentkezéshez az Azure AD integrálása felügyelni.</span><span class="sxs-lookup"><span data-stu-id="41e3b-104">Microsoft Applications (like Office 365 Exchange, SharePoint, Yammer, etc.) are assigned and managed a bit differently than 3rd party SaaS applications or other applications you integrate with Azure AD for single sign on.</span></span>

<span data-ttu-id="41e3b-105">Háromféleképpen fő, hogy a felhasználó beszerezheti a Microsoft közzétett alkalmazások eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="41e3b-105">There are three main ways that a user can get access to a Microsoft-published application.</span></span>

-   <span data-ttu-id="41e3b-106">Az Office 365- vagy más fizetős csomagok-alkalmazások, a felhasználók keresztül hozzáférést kapnak **licenc hozzárendelése** vagy közvetlenül a felhasználói fiókot, vagy egy csoport, a licenc csoport-alapú hozzárendelés funkcióval.</span><span class="sxs-lookup"><span data-stu-id="41e3b-106">For applications in the Office 365 or other paid suites, users are granted access through **license assignment** either directly to their user account, or through a group using our group-based license assignment capability.</span></span>

-   <span data-ttu-id="41e3b-107">Alkalmazások a Microsoft vagy valamely harmadik fél közzétevő szabadon bárki, aki használni, a felhasználók adható hozzáférés a **felhasználói hozzájárulás**.</span><span class="sxs-lookup"><span data-stu-id="41e3b-107">For applications that Microsoft or a Third Party publishes freely for anyone to use, users may be granted access through **user consent**.</span></span> <span data-ttu-id="41e3b-108">This0 azt jelenti, hogy azok az alkalmazás az Azure AD munkahelyi vagy iskolai fiókkal bejelentkezni, és engedélyezze a fiókjuk lehet elérni egyes adatok korlátozott készletét.</span><span class="sxs-lookup"><span data-stu-id="41e3b-108">This0 means that they sign in to the application with their Azure AD Work or School account and allow it to have access to some limited set of data on their account.</span></span>

-   <span data-ttu-id="41e3b-109">Alkalmazások a Microsoft vagy a 3. fél közzétevő szabadon bárki, aki használni, a felhasználók is adható hozzáférés a **rendszergazda jóváhagyását**.</span><span class="sxs-lookup"><span data-stu-id="41e3b-109">For applications that Microsoft or a 3rd Party publishes freely for anyone to use, users may also be granted access through **administrator consent**.</span></span> <span data-ttu-id="41e3b-110">Ez azt jelenti, hogy a rendszergazda azt észlelte, az alkalmazás is használhat a szervezet minden tagja számára, hogy jelentkezzen be az alkalmazás a globális rendszergazdai fiókkal, és hozzáférést biztosítson a szervezet minden tagja számára.</span><span class="sxs-lookup"><span data-stu-id="41e3b-110">This means that an administrator has determined the application may be used by everyone in the organization, so they sign in to the application with a Global Administrator account and grant access to everyone in the organization.</span></span>

<span data-ttu-id="41e3b-111">A probléma elhárításához indítsa el a a [általános probléma területet, és fontolja meg az alkalmazás-hozzáférés](#general-problem-areas-with-application-access-to-consider) majd elolvashatják a [forgatókönyv: hibaelhárítása a Microsoft Application hozzáférés](#walkthrough-steps-to-troubleshoot-microsoft-application-access) be a részletes adatait.</span><span class="sxs-lookup"><span data-stu-id="41e3b-111">To troubleshoot your issue, start with the [General Problem Areas with Application Access to consider](#general-problem-areas-with-application-access-to-consider) and then read the [Walkthrough: Steps to troubleshoot Microsoft Application access](#walkthrough-steps-to-troubleshoot-microsoft-application-access) to get into the details.</span></span>

## <a name="general-problem-areas-with-application-access-to-consider"></a><span data-ttu-id="41e3b-112">Általános probléma területet, és fontolja meg az alkalmazás-hozzáférés</span><span class="sxs-lookup"><span data-stu-id="41e3b-112">General Problem Areas with Application Access to consider</span></span>

<span data-ttu-id="41e3b-113">Az alábbiakban olvashat egy listát az általános problémás területek, amely tovább részletezhető Ha egy meghatározni, hogy hol kell elkezdeni, de ajánlott elolvasni a bemutató lépéseit az induláshoz gyorsan: [forgatókönyv: hibaelhárítása a Microsoft Application hozzáférés](#walkthrough-steps-to-troubleshoot-microsoft-application-access).</span><span class="sxs-lookup"><span data-stu-id="41e3b-113">Below is a list of the general problem areas that you can drill into if you have an idea of where to start, but we recommend you read the walkthrough to get going quickly: [Walkthrough: Steps to troubleshoot Microsoft Application access](#walkthrough-steps-to-troubleshoot-microsoft-application-access).</span></span>

-   [<span data-ttu-id="41e3b-114">A felhasználói fiókkal kapcsolatos problémák</span><span class="sxs-lookup"><span data-stu-id="41e3b-114">Problems with the user’s account</span></span>](#problems-with-the-users-account)

-   [<span data-ttu-id="41e3b-115">A csoportokkal kapcsolatos problémák</span><span class="sxs-lookup"><span data-stu-id="41e3b-115">Problems with groups</span></span>](#problems-with-groups)

-   [<span data-ttu-id="41e3b-116">Feltételes hozzáférési szabályzatokkal kapcsolatos problémák</span><span class="sxs-lookup"><span data-stu-id="41e3b-116">Problems with conditional access policies</span></span>](#problems-with-conditional-access-policies)

-   [<span data-ttu-id="41e3b-117">Az alkalmazás hozzájárulásával problémák</span><span class="sxs-lookup"><span data-stu-id="41e3b-117">Problems with application consent</span></span>](#problems-with-application-consent)

## <a name="steps-to-troubleshoot-microsoft-application-access"></a><span data-ttu-id="41e3b-118">Microsoft Application hozzáférés hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="41e3b-118">Steps to troubleshoot Microsoft Application access</span></span>

<span data-ttu-id="41e3b-119">Alább néhány gyakori problémák segítsen futnak be, amikor a felhasználók nem jelentkezhetnek be a Microsoft-alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="41e3b-119">Below are some common issues folks run into when their users cannot sign in to a Microsoft application.</span></span>

-   <span data-ttu-id="41e3b-120">Először ellenőrizze a általános problémák</span><span class="sxs-lookup"><span data-stu-id="41e3b-120">General issues to check first</span></span>

  * <span data-ttu-id="41e3b-121">Győződjön meg arról, hogy a felhasználó próbál bejelentkezni a **javítsa ki az URL-cím** és nem egy helyi alkalmazás URL-címet.</span><span class="sxs-lookup"><span data-stu-id="41e3b-121">Make sure the user is signing in to the **correct URL** and not a local application URL.</span></span>

  * <span data-ttu-id="41e3b-122">Győződjön meg arról, hogy a felhasználói fiók **nincs zárolva.**</span><span class="sxs-lookup"><span data-stu-id="41e3b-122">Make sure the user’s account is **not locked out.**</span></span>

  * <span data-ttu-id="41e3b-123">Győződjön meg arról, hogy a **felhasználói fiók létezik** az Azure Active Directoryban.</span><span class="sxs-lookup"><span data-stu-id="41e3b-123">Make sure the **user’s account exists** in Azure Active Directory.</span></span> [<span data-ttu-id="41e3b-124">Ellenőrizze, hogy egy felhasználói fiók létezik-e az Azure Active Directoryban</span><span class="sxs-lookup"><span data-stu-id="41e3b-124">Check if a user account exists in Azure Active Directory</span></span>](#problems-with-the-users-account)

  * <span data-ttu-id="41e3b-125">Győződjön meg arról, hogy a felhasználói fiók **engedélyezett** a indított bejelentkezések.</span><span class="sxs-lookup"><span data-stu-id="41e3b-125">Make sure the user’s account is **enabled** for sign ins.</span></span> [<span data-ttu-id="41e3b-126">A felhasználói fiók állapotának ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="41e3b-126">Check a user’s account status</span></span>](#problems-with-the-users-account)

  * <span data-ttu-id="41e3b-127">Győződjön meg arról, hogy a felhasználó **nem lejárt vagy elfelejtett jelszó.**</span><span class="sxs-lookup"><span data-stu-id="41e3b-127">Make sure the user’s **password is not expired or forgotten.**</span></span> <span data-ttu-id="41e3b-128">[A jelszó alaphelyzetbe állítása](#reset-a-users-password) vagy [önkiszolgáló jelszó-visszaállítás engedélyezése](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started)</span><span class="sxs-lookup"><span data-stu-id="41e3b-128">[Reset a user’s password](#reset-a-users-password) or [Enable self-service password reset](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started)</span></span>

   * <span data-ttu-id="41e3b-129">Győződjön meg arról, hogy **multi-factor Authentication** nem blokkolja a hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="41e3b-129">Make sure **Multi-Factor Authentication** is not blocking user access.</span></span> <span data-ttu-id="41e3b-130">[A felhasználó a multi-factor authentication állapotának](#check-a-users-multi-factor-authentication-status) vagy [ellenőrizni kell a felhasználó hitelesítési kapcsolattartási adatait](#check-a-users-authentication-contact-info)</span><span class="sxs-lookup"><span data-stu-id="41e3b-130">[Check a user’s multi-factor authentication status](#check-a-users-multi-factor-authentication-status) or [Check a user’s authentication contact info](#check-a-users-authentication-contact-info)</span></span>

   * <span data-ttu-id="41e3b-131">Győződjön meg arról, hogy egy **feltételes hozzáférési házirend** vagy **Identity Protection** házirend nem blokkolja a hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="41e3b-131">Make sure a **Conditional Access policy** or **Identity Protection** policy is not blocking user access.</span></span> <span data-ttu-id="41e3b-132">[Ellenőrizze a megadott feltételes hozzáférési házirend](#problems-with-conditional-access-policies) vagy [egy adott alkalmazás feltételes hozzáférési házirendben](#check-a-specific-applications-conditional-access-policy) vagy [letiltása adott feltételes hozzáférési házirend](#disable-a-specific-conditional-access-policy)</span><span class="sxs-lookup"><span data-stu-id="41e3b-132">[Check a specific conditional access policy](#problems-with-conditional-access-policies) or [Check a specific application’s conditional access policy](#check-a-specific-applications-conditional-access-policy) or [Disable a specific conditional access policy](#disable-a-specific-conditional-access-policy)</span></span>

   * <span data-ttu-id="41e3b-133">Győződjön meg arról, hogy a felhasználó **hitelesítési kapcsolattartási adatai** esetén a multi-factor Authentication vagy feltételes hozzáférési házirendek kényszerítését dátum.</span><span class="sxs-lookup"><span data-stu-id="41e3b-133">Make sure that a user’s **authentication contact info** is up to date to allow Multi-Factor Authentication or Conditional Access policies to be enforced.</span></span> <span data-ttu-id="41e3b-134">[A felhasználó a multi-factor authentication állapotának](#check-a-users-multi-factor-authentication-status) vagy [ellenőrizni kell a felhasználó hitelesítési kapcsolattartási adatait](#check-a-users-authentication-contact-info)</span><span class="sxs-lookup"><span data-stu-id="41e3b-134">[Check a user’s multi-factor authentication status](#check-a-users-multi-factor-authentication-status) or [Check a user’s authentication contact info](#check-a-users-authentication-contact-info)</span></span>

-   <span data-ttu-id="41e3b-135">A **Microsoft** **licenc igénylő alkalmazások** (például az Office365), az alábbiakban néhány konkrét problémák ellenőrzése után kiderül, hogy a fenti általános problémák:</span><span class="sxs-lookup"><span data-stu-id="41e3b-135">For **Microsoft** **applications that require a license** (like Office365), here are some specific issues to check once you have ruled out the general issues above:</span></span>

   * <span data-ttu-id="41e3b-136">Ellenőrizze a felhasználó vagy van egy **licenccel.**</span><span class="sxs-lookup"><span data-stu-id="41e3b-136">Ensure the user or has a **license assigned.**</span></span> <span data-ttu-id="41e3b-137">[Ellenőrizze a felhasználó licenc-hozzárendeléseket](#check-a-users-assigned-licenses) vagy [egy csoporthoz hozzárendelt licencekkel ellenőrzése](#check-a-groups-assigned-licenses)</span><span class="sxs-lookup"><span data-stu-id="41e3b-137">[Check a user’s assigned licenses](#check-a-users-assigned-licenses) or [Check a group’s assigned licenses](#check-a-groups-assigned-licenses)</span></span>

   * <span data-ttu-id="41e3b-138">Ha a licenc **rendelt egy** **statikus csoport**, ügyeljen arra, hogy a **felhasználó tagja** csoport.</span><span class="sxs-lookup"><span data-stu-id="41e3b-138">If the license is **assigned to a** **static group**, ensure that the **user is a member** of that group.</span></span> [<span data-ttu-id="41e3b-139">A felhasználói csoporttagság ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="41e3b-139">Check a user’s group memberships</span></span>](#check-a-users-group-memberships)

   * <span data-ttu-id="41e3b-140">Ha a licenc **rendelt egy** **dinamikus csoport**, ügyeljen arra, hogy a **dinamikus csoport szabály megfelelően van-e beállítva**.</span><span class="sxs-lookup"><span data-stu-id="41e3b-140">If the license is **assigned to a** **dynamic group**, ensure that the **dynamic group rule is set correctly**.</span></span> [<span data-ttu-id="41e3b-141">A dinamikus csoport tagsági feltételek ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="41e3b-141">Check a dynamic group’s membership criteria</span></span>](#check-a-dynamic-groups-membership-criteria)

   * <span data-ttu-id="41e3b-142">Ha a licenc **rendelt egy** **dinamikus csoport**, győződjön meg arról, hogy rendelkezik-e a dinamikus csoport **fejezett** tagságát, és hogy a **felhasználó tagja** (Ez eltarthat egy ideig).</span><span class="sxs-lookup"><span data-stu-id="41e3b-142">If the license is **assigned to a** **dynamic group**, ensure that the dynamic group has **finished processing** its membership and that the **user is a member** (this can take some time).</span></span> [<span data-ttu-id="41e3b-143">A felhasználói csoporttagság ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="41e3b-143">Check a user’s group memberships</span></span>](#check-a-users-group-memberships)

   *  <span data-ttu-id="41e3b-144">Ha mindenképpen rendel hozzá a licencet, ellenőrizze, hogy a licenc **nem járt le**.</span><span class="sxs-lookup"><span data-stu-id="41e3b-144">Once you make sure the license is assigned, make sure the license is **not expired**.</span></span>

   *  <span data-ttu-id="41e3b-145">Győződjön meg arról, hogy a licenc **az alkalmazás** érik el.</span><span class="sxs-lookup"><span data-stu-id="41e3b-145">Make sure the license is **for the application** they are accessing.</span></span>

-   <span data-ttu-id="41e3b-146">A **Microsoft** **alkalmazások, amelyek nem igényelnek licencet**, a következőkben más kereséséhez:</span><span class="sxs-lookup"><span data-stu-id="41e3b-146">For **Microsoft** **applications that don’t require a license**, here are some other things to check:</span></span>

   * <span data-ttu-id="41e3b-147">Ha az alkalmazás **felhasználói szintű engedélyek** (például "érhetik el a felhasználó postaládájához"), ellenőrizze, hogy a felhasználó van bejelentkezve az alkalmazás, és végrehajtotta a **felhasználói szintű hozzájárulási művelet** ahhoz, hogy az alkalmazás saját adatok eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="41e3b-147">If the application is requesting **user-level permissions** (for example “Access this user’s mailbox”), make sure that the user has signed in to the application and has performed a **user-level consent operation** to let the application access her data.</span></span>

   * <span data-ttu-id="41e3b-148">Ha az alkalmazás **rendszergazdai engedélyek** (például "érhetik el az összes felhasználó postaládák"), győződjön meg arról, hogy elvégezte-e globális rendszergazda egy **rendszergazdai hozzájárulási művelet minden felhasználó nevében** a szervezetében.</span><span class="sxs-lookup"><span data-stu-id="41e3b-148">If the application is requesting **administrator-level permissions** (for example “Access all user’s mailboxes”), make sure that a Global Administrator has performed an **administrator-level consent operation on behalf of all users** in the organization.</span></span>

## <a name="problems-with-the-users-account"></a><span data-ttu-id="41e3b-149">A felhasználói fiókkal kapcsolatos problémák</span><span class="sxs-lookup"><span data-stu-id="41e3b-149">Problems with the user’s account</span></span>

<span data-ttu-id="41e3b-150">Alkalmazás-hozzáférés blokkolható az alkalmazáshoz rendelt felhasználó kapcsolatos probléma miatt.</span><span class="sxs-lookup"><span data-stu-id="41e3b-150">Application access can be blocked due to a problem with a user that is assigned to the application.</span></span> <span data-ttu-id="41e3b-151">Az alábbiakban néhány módszert hibákat, és a felhasználó és a fiók beállításainak problémáinak megoldásával:</span><span class="sxs-lookup"><span data-stu-id="41e3b-151">Below are some ways you can troubleshoot and solve problems with users and their account settings:</span></span>

-   [<span data-ttu-id="41e3b-152">Ellenőrizze, hogy egy felhasználói fiók létezik-e az Azure Active Directoryban</span><span class="sxs-lookup"><span data-stu-id="41e3b-152">Check if a user account exists in Azure Active Directory</span></span>](#check-if-a-user-account-exists-in-azure-active-directory)

-   [<span data-ttu-id="41e3b-153">A felhasználói fiók állapotának ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="41e3b-153">Check a user’s account status</span></span>](#check-a-users-account-status)

-   [<span data-ttu-id="41e3b-154">A jelszó alaphelyzetbe állítása</span><span class="sxs-lookup"><span data-stu-id="41e3b-154">Reset a user’s password</span></span>](#reset-a-users-password)

-   [<span data-ttu-id="41e3b-155">Önkiszolgáló jelszóátállítás engedélyezése</span><span class="sxs-lookup"><span data-stu-id="41e3b-155">Enable self-service password reset</span></span>](#enable-self-service-password-reset)

-   [<span data-ttu-id="41e3b-156">A felhasználó a multi-factor authentication állapotának ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="41e3b-156">Check a user’s multi-factor authentication status</span></span>](#check-a-users-multi-factor-authentication-status)

-   [<span data-ttu-id="41e3b-157">Ellenőrizze a felhasználó hitelesítési kapcsolattartási adatai</span><span class="sxs-lookup"><span data-stu-id="41e3b-157">Check a user’s authentication contact info</span></span>](#check-a-users-authentication-contact-info)

-   [<span data-ttu-id="41e3b-158">A felhasználói csoporttagság ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="41e3b-158">Check a user’s group memberships</span></span>](#check-a-users-group-memberships)

-   [<span data-ttu-id="41e3b-159">A felhasználó licenc-hozzárendeléseket ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="41e3b-159">Check a user’s assigned licenses</span></span>](#check-a-users-assigned-licenses)

-   [<span data-ttu-id="41e3b-160">A felhasználó a licenc hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="41e3b-160">Assign a user a license</span></span>](#assign-a-user-a-license)

### <a name="check-if-a-user-account-exists-in-azure-active-directory"></a><span data-ttu-id="41e3b-161">Ellenőrizze, hogy egy felhasználói fiók létezik-e az Azure Active Directoryban</span><span class="sxs-lookup"><span data-stu-id="41e3b-161">Check if a user account exists in Azure Active Directory</span></span>

<span data-ttu-id="41e3b-162">Ellenőrizze, hogy jelen-e a felhasználói fiók, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="41e3b-162">To check if a user’s account is present, follow the steps below:</span></span>

1.  <span data-ttu-id="41e3b-163">Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="41e3b-163">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="41e3b-164">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="41e3b-164">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="41e3b-165">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="41e3b-165">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="41e3b-166">Kattintson a **felhasználók és csoportok** a navigációs menü.</span><span class="sxs-lookup"><span data-stu-id="41e3b-166">click **Users and groups** in the navigation menu.</span></span>

5.  <span data-ttu-id="41e3b-167">Kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="41e3b-167">click **All users**.</span></span>

6.  <span data-ttu-id="41e3b-168">**Keresési** érdekli, felhasználó és **sorára kattintson** kiválasztásához.</span><span class="sxs-lookup"><span data-stu-id="41e3b-168">**Search** for the user you are interested in and **click the row** to select.</span></span>

7.  <span data-ttu-id="41e3b-169">Ellenőrizze, győződjön meg arról, hogy azok meg, mint a várt, és nincs adat hiányzik a user objektum tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="41e3b-169">Check the properties of the user object to be sure that they look as you expect and no data is missing.</span></span>

### <a name="check-a-users-account-status"></a><span data-ttu-id="41e3b-170">A felhasználói fiók állapotának ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="41e3b-170">Check a user’s account status</span></span>

<span data-ttu-id="41e3b-171">A felhasználói fiók állapotának ellenőrzéséhez kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="41e3b-171">To check a user’s account status, follow the steps below:</span></span>

1.  <span data-ttu-id="41e3b-172">Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="41e3b-172">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="41e3b-173">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="41e3b-173">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="41e3b-174">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="41e3b-174">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="41e3b-175">Kattintson a **felhasználók és csoportok** a navigációs menü.</span><span class="sxs-lookup"><span data-stu-id="41e3b-175">click **Users and groups** in the navigation menu.</span></span>

5.  <span data-ttu-id="41e3b-176">Kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="41e3b-176">click **All users**.</span></span>

6.  <span data-ttu-id="41e3b-177">**Keresési** érdekli, felhasználó és **sorára kattintson** kiválasztásához.</span><span class="sxs-lookup"><span data-stu-id="41e3b-177">**Search** for the user you are interested in and **click the row** to select.</span></span>

7.  <span data-ttu-id="41e3b-178">Kattintson a **profil**.</span><span class="sxs-lookup"><span data-stu-id="41e3b-178">click **Profile**.</span></span>

8.  <span data-ttu-id="41e3b-179">A **beállítások** ügyeljen arra, hogy **blokk bejelentkezés** értékre van állítva **nem**.</span><span class="sxs-lookup"><span data-stu-id="41e3b-179">Under **Settings** ensure that **Block sign in** is set to **No**.</span></span>

### <a name="reset-a-users-password"></a><span data-ttu-id="41e3b-180">A jelszó alaphelyzetbe állítása</span><span class="sxs-lookup"><span data-stu-id="41e3b-180">Reset a user’s password</span></span>

<span data-ttu-id="41e3b-181">A jelszó visszaállításához kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="41e3b-181">To reset a user’s password, follow the steps below:</span></span>

1.  <span data-ttu-id="41e3b-182">Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="41e3b-182">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="41e3b-183">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="41e3b-183">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="41e3b-184">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="41e3b-184">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="41e3b-185">Kattintson a **felhasználók és csoportok** a navigációs menü.</span><span class="sxs-lookup"><span data-stu-id="41e3b-185">click **Users and groups** in the navigation menu.</span></span>

5.  <span data-ttu-id="41e3b-186">Kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="41e3b-186">click **All users**.</span></span>

6.  <span data-ttu-id="41e3b-187">**Keresési** érdekli, felhasználó és **sorára kattintson** kiválasztásához.</span><span class="sxs-lookup"><span data-stu-id="41e3b-187">**Search** for the user you are interested in and **click the row** to select.</span></span>

7.  <span data-ttu-id="41e3b-188">Kattintson a **jelszó-átállítási** gombra a felhasználó panel tetején.</span><span class="sxs-lookup"><span data-stu-id="41e3b-188">click the **Reset password** button at the top of the user blade.</span></span>

8.  <span data-ttu-id="41e3b-189">Kattintson a **jelszó-átállítási** gombra a **jelszó-átállítási** panel, amely akkor jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="41e3b-189">click the **Reset password** button on the **Reset password** blade that appears.</span></span>

9.  <span data-ttu-id="41e3b-190">Másolás a **ideiglenes jelszó** vagy **adjon meg egy új jelszót** a felhasználó számára.</span><span class="sxs-lookup"><span data-stu-id="41e3b-190">Copy the **temporary password** or **enter a new password** for the user.</span></span>

10. <span data-ttu-id="41e3b-191">Az új jelszót, amely a felhasználó kommunikációhoz, azok szükséges jelszó módosításához a következő bejelentkezéskor a során az Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="41e3b-191">Communicate this new password to the user, they be required to change this password during their next sign in to Azure Active Directory.</span></span>

### <a name="enable-self-service-password-reset"></a><span data-ttu-id="41e3b-192">Az önkiszolgáló jelszó-visszaállítás engedélyezése</span><span class="sxs-lookup"><span data-stu-id="41e3b-192">Enable self-service password reset</span></span>

<span data-ttu-id="41e3b-193">Önkiszolgáló jelszóváltoztatás, kövesse az alábbi telepítési lépéseket:</span><span class="sxs-lookup"><span data-stu-id="41e3b-193">To enable self-service password reset, follow the deployment steps below:</span></span>

-   [<span data-ttu-id="41e3b-194">Segítségével a felhasználók visszaállíthassák az Azure Active Directory-jelszavaikat</span><span class="sxs-lookup"><span data-stu-id="41e3b-194">Enable users to reset their Azure Active Directory passwords</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started#enable-users-to-reset-their-azure-ad-passwords)

-   [<span data-ttu-id="41e3b-195">Lehetővé teszi a felhasználók visszaállíthassák vagy módosíthassák a helyszíni Active Directory-jelszavaikat</span><span class="sxs-lookup"><span data-stu-id="41e3b-195">Enable users to reset or change their Active Directory on-premises passwords</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started#enable-users-to-reset-or-change-their-ad-passwords)

### <a name="check-a-users-multi-factor-authentication-status"></a><span data-ttu-id="41e3b-196">A felhasználó a multi-factor authentication állapotának ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="41e3b-196">Check a user’s multi-factor authentication status</span></span>

<span data-ttu-id="41e3b-197">A felhasználó a multi-factor authentication állapotának ellenőrzéséhez kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="41e3b-197">To check a user’s multi-factor authentication status, follow the steps below:</span></span>

1.  <span data-ttu-id="41e3b-198">Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="41e3b-198">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="41e3b-199">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="41e3b-199">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="41e3b-200">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="41e3b-200">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="41e3b-201">Kattintson a **felhasználók és csoportok** a navigációs menü.</span><span class="sxs-lookup"><span data-stu-id="41e3b-201">click **Users and groups** in the navigation menu.</span></span>

5.  <span data-ttu-id="41e3b-202">Kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="41e3b-202">click **All users**.</span></span>

6.  <span data-ttu-id="41e3b-203">Kattintson a **multi-factor Authentication** gomb a panel tetején.</span><span class="sxs-lookup"><span data-stu-id="41e3b-203">click the **Multi-Factor Authentication** button at the top of the blade.</span></span>

7.  <span data-ttu-id="41e3b-204">Egyszer a **multi-factor Authentication felügyeleti portál** terhelés esetén gondoskodjon arról, hogy a **felhasználók** fülre.</span><span class="sxs-lookup"><span data-stu-id="41e3b-204">Once the **Multi-Factor Authentication Administration Portal** loads, ensure you are on the **Users** tab.</span></span>

8.  <span data-ttu-id="41e3b-205">Keresés, rendezés vagy szűréssel keresse meg a felhasználót a felhasználók listáját.</span><span class="sxs-lookup"><span data-stu-id="41e3b-205">Find the user in the list of users by searching, filtering, or sorting.</span></span>

9.  <span data-ttu-id="41e3b-206">Válassza ki a felhasználót a felhasználók listájának és **engedélyezése**, **tiltsa le a**, vagy **érvényesítése** többtényezős hitelesítést a kívánt módon működjenek.</span><span class="sxs-lookup"><span data-stu-id="41e3b-206">Select the user from the list of users and **Enable**, **Disable**, or **Enforce** multi-factor authentication as desired.</span></span>

  * <span data-ttu-id="41e3b-207">**Megjegyzés:**: Ha egy felhasználó egy **kényszerített** állapotba kerül, előfordulhat, hogy meg őket **letiltott** ideiglenesen ahhoz, hogy azokat vissza a figyelembe.</span><span class="sxs-lookup"><span data-stu-id="41e3b-207">**Note**: If a user is in an **Enforced** state, you may set them to **Disabled** temporarily to let them back into their account.</span></span> <span data-ttu-id="41e3b-208">Miután bekerültek vissza, módosíthatja az állapot **engedélyezve** újra a megköveteli tőlük, hogy regisztrálja újra a során a következő bejelentkezéskor a kapcsolattartási adatait.</span><span class="sxs-lookup"><span data-stu-id="41e3b-208">Once they are back in, you can then change their state to **Enabled** again to require them to re-register their contact information during their next sign in.</span></span> <span data-ttu-id="41e3b-209">Azt is megteheti, hogy is kövesse a [ellenőrizni kell a felhasználó hitelesítési kapcsolattartási adatait](#check-a-users-authentication-contact-info) ellenőrizze vagy állítsa be ezeket az adatokat a számukra.</span><span class="sxs-lookup"><span data-stu-id="41e3b-209">Alternatively, you can follow the steps in the [Check a user’s authentication contact info](#check-a-users-authentication-contact-info) to verify or set this data for them.</span></span>

### <a name="check-a-users-authentication-contact-info"></a><span data-ttu-id="41e3b-210">Ellenőrizze a felhasználó hitelesítési kapcsolattartási adatai</span><span class="sxs-lookup"><span data-stu-id="41e3b-210">Check a user’s authentication contact info</span></span>

<span data-ttu-id="41e3b-211">A felhasználó hitelesítési használt kapcsolattartási adatokat a többtényezős hitelesítést, a feltételes hozzáférés, a Identity Protection és a jelszó-átállítási ellenőrzéséhez kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="41e3b-211">To check a user’s authentication contact info used for Multi-factor authentication, Conditional Access, Identity Protection, and Password Reset, follow the steps below:</span></span>

1.  <span data-ttu-id="41e3b-212">Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="41e3b-212">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="41e3b-213">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="41e3b-213">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="41e3b-214">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="41e3b-214">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="41e3b-215">Kattintson a **felhasználók és csoportok** a navigációs menü.</span><span class="sxs-lookup"><span data-stu-id="41e3b-215">click **Users and groups** in the navigation menu.</span></span>

5.  <span data-ttu-id="41e3b-216">Kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="41e3b-216">click **All users**.</span></span>

6.  <span data-ttu-id="41e3b-217">**Keresési** érdekli, felhasználó és **sorára kattintson** kiválasztásához.</span><span class="sxs-lookup"><span data-stu-id="41e3b-217">**Search** for the user you are interested in and **click the row** to select.</span></span>

7.  <span data-ttu-id="41e3b-218">Kattintson a **profil**.</span><span class="sxs-lookup"><span data-stu-id="41e3b-218">click **Profile**.</span></span>

8.  <span data-ttu-id="41e3b-219">Görgessen le a **hitelesítési kapcsolattartási adatai**.</span><span class="sxs-lookup"><span data-stu-id="41e3b-219">Scroll down to **Authentication contact info**.</span></span>

9.  <span data-ttu-id="41e3b-220">**Tekintse át** az adatok regisztrálva a felhasználó és a frissítési igény szerint.</span><span class="sxs-lookup"><span data-stu-id="41e3b-220">**Review** the data registered for the user and update as needed.</span></span>

### <a name="check-a-users-group-memberships"></a><span data-ttu-id="41e3b-221">A felhasználói csoporttagság ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="41e3b-221">Check a user’s group memberships</span></span>

<span data-ttu-id="41e3b-222">A felhasználói csoporttagság ellenőrzéséhez kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="41e3b-222">To check a user’s group memberships, follow the steps below:</span></span>

1.  <span data-ttu-id="41e3b-223">Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="41e3b-223">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="41e3b-224">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="41e3b-224">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="41e3b-225">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="41e3b-225">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="41e3b-226">Kattintson a **felhasználók és csoportok** a navigációs menü.</span><span class="sxs-lookup"><span data-stu-id="41e3b-226">click **Users and groups** in the navigation menu.</span></span>

5.  <span data-ttu-id="41e3b-227">Kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="41e3b-227">click **All users**.</span></span>

6.  <span data-ttu-id="41e3b-228">**Keresési** érdekli, felhasználó és **sorára kattintson** kiválasztásához.</span><span class="sxs-lookup"><span data-stu-id="41e3b-228">**Search** for the user you are interested in and **click the row** to select.</span></span>

7.  <span data-ttu-id="41e3b-229">Kattintson a **csoportok** megtekintéséhez, amely csoportosítja a felhasználó tagja legyen.</span><span class="sxs-lookup"><span data-stu-id="41e3b-229">click **Groups** to see which groups the user is a member of.</span></span>

### <a name="check-a-users-assigned-licenses"></a><span data-ttu-id="41e3b-230">A felhasználó licenc-hozzárendeléseket ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="41e3b-230">Check a user’s assigned licenses</span></span>

<span data-ttu-id="41e3b-231">A felhasználó licenc-hozzárendeléseket ellenőrzéséhez kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="41e3b-231">To check a user’s assigned licenses, follow the steps below:</span></span>

1.  <span data-ttu-id="41e3b-232">Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="41e3b-232">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="41e3b-233">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="41e3b-233">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="41e3b-234">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="41e3b-234">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="41e3b-235">Kattintson a **felhasználók és csoportok** a navigációs menü.</span><span class="sxs-lookup"><span data-stu-id="41e3b-235">click **Users and groups** in the navigation menu.</span></span>

5.  <span data-ttu-id="41e3b-236">Kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="41e3b-236">click **All users**.</span></span>

6.  <span data-ttu-id="41e3b-237">**Keresési** érdekli, felhasználó és **sorára kattintson** kiválasztásához.</span><span class="sxs-lookup"><span data-stu-id="41e3b-237">**Search** for the user you are interested in and **click the row** to select.</span></span>

7.  <span data-ttu-id="41e3b-238">Kattintson a **licencek** megtekintéséhez, amely licencek, a felhasználó jelenleg hozzá van rendelve.</span><span class="sxs-lookup"><span data-stu-id="41e3b-238">click **Licenses** to see which licenses the user currently has assigned.</span></span>

### <a name="assign-a-user-a-license"></a><span data-ttu-id="41e3b-239">A felhasználó a licenc hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="41e3b-239">Assign a user a license</span></span> 

<span data-ttu-id="41e3b-240">A licenc hozzárendelése egy felhasználóhoz, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="41e3b-240">To assign a license to a user, follow the steps below:</span></span>

1.  <span data-ttu-id="41e3b-241">Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="41e3b-241">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="41e3b-242">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="41e3b-242">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="41e3b-243">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="41e3b-243">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="41e3b-244">Kattintson a **felhasználók és csoportok** a navigációs menü.</span><span class="sxs-lookup"><span data-stu-id="41e3b-244">click **Users and groups** in the navigation menu.</span></span>

5.  <span data-ttu-id="41e3b-245">Kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="41e3b-245">click **All users**.</span></span>

6.  <span data-ttu-id="41e3b-246">**Keresési** érdekli, felhasználó és **sorára kattintson** kiválasztásához.</span><span class="sxs-lookup"><span data-stu-id="41e3b-246">**Search** for the user you are interested in and **click the row** to select.</span></span>

7.  <span data-ttu-id="41e3b-247">Kattintson a **licencek** megtekintéséhez, amely licencek, a felhasználó jelenleg hozzá van rendelve.</span><span class="sxs-lookup"><span data-stu-id="41e3b-247">click **Licenses** to see which licenses the user currently has assigned.</span></span>

8.  <span data-ttu-id="41e3b-248">Kattintson a **hozzárendelése** gombra.</span><span class="sxs-lookup"><span data-stu-id="41e3b-248">click the **Assign** button.</span></span>

9.  <span data-ttu-id="41e3b-249">Válassza ki **egy vagy több termék** választható termékek közül.</span><span class="sxs-lookup"><span data-stu-id="41e3b-249">Select **one or more products** from the list of available products.</span></span>

10. <span data-ttu-id="41e3b-250">**Nem kötelező** kattintson a **hozzárendelés beállításai** elem termékek granularly hozzárendelni.</span><span class="sxs-lookup"><span data-stu-id="41e3b-250">**Optional** click the **assignment options** item to granularly assign products.</span></span> <span data-ttu-id="41e3b-251">Kattintson a **Ok** amikor ez befejeződik.</span><span class="sxs-lookup"><span data-stu-id="41e3b-251">Click **Ok** when this is completed.</span></span>

11. <span data-ttu-id="41e3b-252">Kattintson a **hozzárendelése** gomb a licencek hozzárendelése a felhasználóhoz.</span><span class="sxs-lookup"><span data-stu-id="41e3b-252">Click the **Assign** button to assign these licenses to this user.</span></span>

## <a name="problems-with-groups"></a><span data-ttu-id="41e3b-253">A csoportokkal kapcsolatos problémák</span><span class="sxs-lookup"><span data-stu-id="41e3b-253">Problems with groups</span></span>

<span data-ttu-id="41e3b-254">Alkalmazás-hozzáférés blokkolható egy csoportot, amely hozzá van rendelve az alkalmazás kapcsolatos probléma miatt.</span><span class="sxs-lookup"><span data-stu-id="41e3b-254">Application access can be blocked due to a problem with a group that is assigned to the application.</span></span> <span data-ttu-id="41e3b-255">Az alábbiakban néhány módszert, hibaelhárítási és csoportokkal vagy csoporttagsággal kapcsolatos problémák megoldásához:</span><span class="sxs-lookup"><span data-stu-id="41e3b-255">Below are some ways you can troubleshoot and solve problems with groups and group memberships:</span></span>

-   [<span data-ttu-id="41e3b-256">Ellenőrizze a csoport tagságát</span><span class="sxs-lookup"><span data-stu-id="41e3b-256">Check a group’s membership</span></span>](#check-a-groups-membership)

-   [<span data-ttu-id="41e3b-257">A dinamikus csoport tagsági feltételek ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="41e3b-257">Check a dynamic group’s membership criteria</span></span>](#check-a-dynamic-groups-membership-criteria)

-   [<span data-ttu-id="41e3b-258">Ellenőrizze a csoporthoz hozzárendelt licencekkel</span><span class="sxs-lookup"><span data-stu-id="41e3b-258">Check a group’s assigned licenses</span></span>](#check-a-groups-assigned-licenses)

-   [<span data-ttu-id="41e3b-259">Újból feldolgozza a csoport licencek</span><span class="sxs-lookup"><span data-stu-id="41e3b-259">Reprocess a group’s licenses</span></span>](#reprocess-a-groups-licenses)

-   [<span data-ttu-id="41e3b-260">Egy csoport a licenc hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="41e3b-260">Assign a group a license</span></span>](#assign-a-group-a-license)

### <a name="check-a-groups-membership"></a><span data-ttu-id="41e3b-261">Ellenőrizze a csoport tagságát</span><span class="sxs-lookup"><span data-stu-id="41e3b-261">Check a group’s membership</span></span>

<span data-ttu-id="41e3b-262">Ellenőrizze a csoport tagságát, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="41e3b-262">To check a group’s membership, follow the steps below:</span></span>

1.  <span data-ttu-id="41e3b-263">Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="41e3b-263">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="41e3b-264">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="41e3b-264">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="41e3b-265">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="41e3b-265">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="41e3b-266">Kattintson a **felhasználók és csoportok** a navigációs menü.</span><span class="sxs-lookup"><span data-stu-id="41e3b-266">click **Users and groups** in the navigation menu.</span></span>

5.  <span data-ttu-id="41e3b-267">Kattintson a **összes csoport**.</span><span class="sxs-lookup"><span data-stu-id="41e3b-267">click **All groups**.</span></span>

6.  <span data-ttu-id="41e3b-268">**Keresési** érdekli csoport és **sorára kattintson** kiválasztásához.</span><span class="sxs-lookup"><span data-stu-id="41e3b-268">**Search** for the group you are interested in and **click the row** to select.</span></span>

7.  <span data-ttu-id="41e3b-269">Kattintson a **tagok** tekintse át a listában, ehhez a csoporthoz tartozó felhasználók számára.</span><span class="sxs-lookup"><span data-stu-id="41e3b-269">click **Members** to review the list of users assigned to this group.</span></span>

### <a name="check-a-dynamic-groups-membership-criteria"></a><span data-ttu-id="41e3b-270">A dinamikus csoport tagsági feltételek ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="41e3b-270">Check a dynamic group’s membership criteria</span></span> 

<span data-ttu-id="41e3b-271">A dinamikus csoport tagsági feltételek ellenőrzéséhez kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="41e3b-271">To check a dynamic group’s membership criteria, follow the steps below:</span></span>

1.  <span data-ttu-id="41e3b-272">Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="41e3b-272">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="41e3b-273">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="41e3b-273">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="41e3b-274">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="41e3b-274">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="41e3b-275">Kattintson a **felhasználók és csoportok** a navigációs menü.</span><span class="sxs-lookup"><span data-stu-id="41e3b-275">click **Users and groups** in the navigation menu.</span></span>

5.  <span data-ttu-id="41e3b-276">Kattintson a **összes csoport**.</span><span class="sxs-lookup"><span data-stu-id="41e3b-276">click **All groups**.</span></span>

6.  <span data-ttu-id="41e3b-277">**Keresési** érdekli csoport és **sorára kattintson** kiválasztásához.</span><span class="sxs-lookup"><span data-stu-id="41e3b-277">**Search** for the group you are interested in and **click the row** to select.</span></span>

7.  <span data-ttu-id="41e3b-278">Kattintson a **dinamikus tagsági szabályok szempontjából.**</span><span class="sxs-lookup"><span data-stu-id="41e3b-278">click **Dynamic membership rules.**</span></span>

8.  <span data-ttu-id="41e3b-279">Tekintse át a **egyszerű** vagy **speciális** szabály definiálva, és győződjön meg arról, hogy ez a csoport tagjai kívánt felhasználó megfelel-e ezek a feltételek.</span><span class="sxs-lookup"><span data-stu-id="41e3b-279">Review the **simple** or **advanced** rule defined for this group and ensure that the user you want to be a member of this group meets these criteria.</span></span>

### <a name="check-a-groups-assigned-licenses"></a><span data-ttu-id="41e3b-280">Ellenőrizze a csoporthoz hozzárendelt licencekkel</span><span class="sxs-lookup"><span data-stu-id="41e3b-280">Check a group’s assigned licenses</span></span>

<span data-ttu-id="41e3b-281">A csoporthoz hozzárendelt licencekkel ellenőrzéséhez kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="41e3b-281">To check a group’s assigned licenses, follow the steps below:</span></span>

1.  <span data-ttu-id="41e3b-282">Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="41e3b-282">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="41e3b-283">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="41e3b-283">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="41e3b-284">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="41e3b-284">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="41e3b-285">Kattintson a **felhasználók és csoportok** a navigációs menü.</span><span class="sxs-lookup"><span data-stu-id="41e3b-285">click **Users and groups** in the navigation menu.</span></span>

5.  <span data-ttu-id="41e3b-286">Kattintson a **összes csoport**.</span><span class="sxs-lookup"><span data-stu-id="41e3b-286">click **All groups**.</span></span>

6.  <span data-ttu-id="41e3b-287">**Keresési** érdekli csoport és **sorára kattintson** kiválasztásához.</span><span class="sxs-lookup"><span data-stu-id="41e3b-287">**Search** for the group you are interested in and **click the row** to select.</span></span>

7.  <span data-ttu-id="41e3b-288">Kattintson a **licencek** megtekintéséhez, amely licencek, a csoport jelenleg hozzá van rendelve.</span><span class="sxs-lookup"><span data-stu-id="41e3b-288">click **Licenses** to see which licenses the group currently has assigned.</span></span>

### <a name="reprocess-a-groups-licenses"></a><span data-ttu-id="41e3b-289">Újból feldolgozza a csoport licencek</span><span class="sxs-lookup"><span data-stu-id="41e3b-289">Reprocess a group’s licenses</span></span>

<span data-ttu-id="41e3b-290">Újból feldolgozza a csoporthoz hozzárendelt licenceket, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="41e3b-290">To reprocess a group’s assigned licenses, follow the steps below:</span></span>

1.  <span data-ttu-id="41e3b-291">Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="41e3b-291">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="41e3b-292">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="41e3b-292">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="41e3b-293">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="41e3b-293">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="41e3b-294">Kattintson a **felhasználók és csoportok** a navigációs menü.</span><span class="sxs-lookup"><span data-stu-id="41e3b-294">click **Users and groups** in the navigation menu.</span></span>

5.  <span data-ttu-id="41e3b-295">Kattintson a **összes csoport**.</span><span class="sxs-lookup"><span data-stu-id="41e3b-295">click **All groups**.</span></span>

6.  <span data-ttu-id="41e3b-296">**Keresési** érdekli csoport és **sorára kattintson** kiválasztásához.</span><span class="sxs-lookup"><span data-stu-id="41e3b-296">**Search** for the group you are interested in and **click the row** to select.</span></span>

7.  <span data-ttu-id="41e3b-297">Kattintson a **licencek** megtekintéséhez, amely licencek, a csoport jelenleg hozzá van rendelve.</span><span class="sxs-lookup"><span data-stu-id="41e3b-297">click **Licenses** to see which licenses the group currently has assigned.</span></span>

8.  <span data-ttu-id="41e3b-298">Kattintson a **újból feldolgozza** gombra kattintva győződjön meg arról, hogy a csoport tagjai a licenccel naprakészek legyenek.</span><span class="sxs-lookup"><span data-stu-id="41e3b-298">click the **Reprocess** button to ensure that the licenses assigned to this group’s members are up-to-date.</span></span> <span data-ttu-id="41e3b-299">Ez eltarthat egy ideig, attól függően, hogy méretét és összetettségét, a csoport.</span><span class="sxs-lookup"><span data-stu-id="41e3b-299">This may take a long time, depending on the size and complexity of the group.</span></span>

   >[!NOTE]
   ><span data-ttu-id="41e3b-300">Ehhez gyorsabb, érdemes lehet ideiglenesen rendel egy licencet a felhasználó közvetlenül.</span><span class="sxs-lookup"><span data-stu-id="41e3b-300">To do this faster, consider temporarily assigning a license to the user directly.</span></span> <span data-ttu-id="41e3b-301">[A felhasználó rendel egy licencet](#problems-with-application-consent).</span><span class="sxs-lookup"><span data-stu-id="41e3b-301">[Assign a user a license](#problems-with-application-consent).</span></span>
   >
   >

### <a name="assign-a-group-a-license"></a><span data-ttu-id="41e3b-302">Egy csoport a licenc hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="41e3b-302">Assign a group a license</span></span>

<span data-ttu-id="41e3b-303">A licenc hozzárendelése egy csoporthoz, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="41e3b-303">To assign a license to a group, follow the steps below:</span></span>

1.  <span data-ttu-id="41e3b-304">Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="41e3b-304">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="41e3b-305">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="41e3b-305">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="41e3b-306">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="41e3b-306">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="41e3b-307">Kattintson a **felhasználók és csoportok** a navigációs menü.</span><span class="sxs-lookup"><span data-stu-id="41e3b-307">click **Users and groups** in the navigation menu.</span></span>

5.  <span data-ttu-id="41e3b-308">Kattintson a **összes csoport**.</span><span class="sxs-lookup"><span data-stu-id="41e3b-308">click **All groups**.</span></span>

6.  <span data-ttu-id="41e3b-309">**Keresési** érdekli csoport és **sorára kattintson** kiválasztásához.</span><span class="sxs-lookup"><span data-stu-id="41e3b-309">**Search** for the group you are interested in and **click the row** to select.</span></span>

7.  <span data-ttu-id="41e3b-310">Kattintson a **licencek** megtekintéséhez, amely licencek, a csoport jelenleg hozzá van rendelve.</span><span class="sxs-lookup"><span data-stu-id="41e3b-310">click **Licenses** to see which licenses the group currently has assigned.</span></span>

8.  <span data-ttu-id="41e3b-311">Kattintson a **hozzárendelése** gombra.</span><span class="sxs-lookup"><span data-stu-id="41e3b-311">click the **Assign** button.</span></span>

9.  <span data-ttu-id="41e3b-312">Válassza ki **egy vagy több termék** választható termékek közül.</span><span class="sxs-lookup"><span data-stu-id="41e3b-312">Select **one or more products** from the list of available products.</span></span>

10. <span data-ttu-id="41e3b-313">**Nem kötelező** kattintson a **hozzárendelés beállításai** elem termékek granularly hozzárendelni.</span><span class="sxs-lookup"><span data-stu-id="41e3b-313">**Optional** click the **assignment options** item to granularly assign products.</span></span> <span data-ttu-id="41e3b-314">Kattintson a **Ok** amikor ez befejeződik.</span><span class="sxs-lookup"><span data-stu-id="41e3b-314">Click **Ok** when this is completed.</span></span>

11. <span data-ttu-id="41e3b-315">Kattintson a **hozzárendelése** gomb a licencek hozzárendelése ehhez a csoporthoz.</span><span class="sxs-lookup"><span data-stu-id="41e3b-315">Click the **Assign** button to assign these licenses to this group.</span></span> <span data-ttu-id="41e3b-316">Ez eltarthat egy ideig, attól függően, hogy méretét és összetettségét, a csoport.</span><span class="sxs-lookup"><span data-stu-id="41e3b-316">This may take a long time, depending on the size and complexity of the group.</span></span>

   >[!NOTE]
   ><span data-ttu-id="41e3b-317">Ehhez gyorsabb, érdemes lehet ideiglenesen rendel egy licencet a felhasználó közvetlenül.</span><span class="sxs-lookup"><span data-stu-id="41e3b-317">To do this faster, consider temporarily assigning a license to the user directly.</span></span> <span data-ttu-id="41e3b-318">[A felhasználó rendel egy licencet](#problems-with-application-consent).</span><span class="sxs-lookup"><span data-stu-id="41e3b-318">[Assign a user a license](#problems-with-application-consent).</span></span>
   > 
   >

## <a name="problems-with-conditional-access-policies"></a><span data-ttu-id="41e3b-319">Feltételes hozzáférési szabályzatokkal kapcsolatos problémák</span><span class="sxs-lookup"><span data-stu-id="41e3b-319">Problems with conditional access policies</span></span>

### <a name="check-a-specific-conditional-access-policy"></a><span data-ttu-id="41e3b-320">Egy adott feltételes hozzáférési házirend ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="41e3b-320">Check a specific conditional access policy</span></span>

<span data-ttu-id="41e3b-321">Ellenőrizze, illetve ellenőrizhető egy feltételes hozzáférési szabályzatot:</span><span class="sxs-lookup"><span data-stu-id="41e3b-321">To check or validate a single conditional access policy:</span></span>

1.  <span data-ttu-id="41e3b-322">Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="41e3b-322">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="41e3b-323">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="41e3b-323">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="41e3b-324">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="41e3b-324">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="41e3b-325">Kattintson a **vállalati alkalmazások** a navigációs menü.</span><span class="sxs-lookup"><span data-stu-id="41e3b-325">click **Enterprise applications** in the navigation menu.</span></span>

5.  <span data-ttu-id="41e3b-326">Kattintson a **feltételes hozzáférés** navigációs elem.</span><span class="sxs-lookup"><span data-stu-id="41e3b-326">click the **Conditional access** navigation item.</span></span>

6.  <span data-ttu-id="41e3b-327">Kattintson a érdekli ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="41e3b-327">click the policy you are interested in inspecting.</span></span>

7.  <span data-ttu-id="41e3b-328">Ellenőrizze, hogy nincsenek-e nincs meghatározott feltételek, hozzárendelések vagy egyéb beállításokat, amelyek letilthatja a felhasználói hozzáférés.</span><span class="sxs-lookup"><span data-stu-id="41e3b-328">Review that there are no specific conditions, assignments, or other settings which may be blocking user access.</span></span>

   >[!NOTE]
   ><span data-ttu-id="41e3b-329">Kezdésként érdemes lehet ideiglenesen letilthatja ezt a házirendet, győződjön meg arról, hogy nem érinti a indított bejelentkezések.</span><span class="sxs-lookup"><span data-stu-id="41e3b-329">You may wish to temporarily disable this policy to ensure it is not affecting sign ins.</span></span> <span data-ttu-id="41e3b-330">Ehhez az szükséges, állítsa be a **házirend engedélyezése** kapcsolót **nem** , és kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="41e3b-330">To do this, set the **Enable policy** toggle to **No** and click the **Save** button.</span></span>
   >
   >

### <a name="check-a-specific-applications-conditional-access-policy"></a><span data-ttu-id="41e3b-331">Egy adott alkalmazás feltételes hozzáférési házirend ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="41e3b-331">Check a specific application’s conditional access policy</span></span>

<span data-ttu-id="41e3b-332">Ellenőrizze, vagy egyetlen alkalmazás jelenleg ellenőrizni konfigurált feltételes hozzáférési szabályzatot:</span><span class="sxs-lookup"><span data-stu-id="41e3b-332">To check or validate a single application’s currently configured conditional access policy:</span></span>

1.  <span data-ttu-id="41e3b-333">Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="41e3b-333">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="41e3b-334">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="41e3b-334">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="41e3b-335">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="41e3b-335">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="41e3b-336">Kattintson a **vállalati alkalmazások** a navigációs menü.</span><span class="sxs-lookup"><span data-stu-id="41e3b-336">click **Enterprise applications** in the navigation menu.</span></span>

5.  <span data-ttu-id="41e3b-337">Kattintson a **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="41e3b-337">click **All applications**.</span></span>

6.  <span data-ttu-id="41e3b-338">Keresse meg az alkalmazás érdekli, vagy a felhasználó próbál bejelentkezni az alkalmazás megjelenített neve vagy azonosítója.</span><span class="sxs-lookup"><span data-stu-id="41e3b-338">Search for the application you are interested in, or the user is attempting to sign in to by application display name or application id.</span></span>

     >[!NOTE]
     ><span data-ttu-id="41e3b-339">Ha nem látja a keresett alkalmazás, kattintson a **szűrő** gombra, majd bontsa ki a listában hatóköre **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="41e3b-339">If you don’t see the application you are looking for, click the **Filter** button and expand the scope of the list to **All applications**.</span></span> <span data-ttu-id="41e3b-340">Ha meg szeretné tekinteni a további oszlopok, kattintson a **oszlopok** gombra kattintva adja hozzá a további részletek az alkalmazások számára.</span><span class="sxs-lookup"><span data-stu-id="41e3b-340">If you want to see more columns, click the **Columns** button to add additional details for your applications.</span></span>
     >
     >

7.  <span data-ttu-id="41e3b-341">Kattintson a **feltételes hozzáférés** navigációs elem.</span><span class="sxs-lookup"><span data-stu-id="41e3b-341">click the **Conditional access** navigation item.</span></span>

8.  <span data-ttu-id="41e3b-342">Kattintson a érdekli ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="41e3b-342">click the policy you are interested in inspecting.</span></span>

9.  <span data-ttu-id="41e3b-343">Ellenőrizze, hogy nincsenek-e nincs meghatározott feltételek, hozzárendelések vagy egyéb beállításokat, amelyek letilthatja a felhasználói hozzáférés.</span><span class="sxs-lookup"><span data-stu-id="41e3b-343">Review that there are no specific conditions, assignments, or other settings which may be blocking user access.</span></span>

     >[!NOTE]
     ><span data-ttu-id="41e3b-344">Kezdésként érdemes lehet ideiglenesen letilthatja ezt a házirendet, győződjön meg arról, hogy nem érinti a indított bejelentkezések.</span><span class="sxs-lookup"><span data-stu-id="41e3b-344">You may wish to temporarily disable this policy to ensure it is not affecting sign ins.</span></span> <span data-ttu-id="41e3b-345">Ehhez az szükséges, állítsa be a **házirend engedélyezése** kapcsolót **nem** , és kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="41e3b-345">To do this, set the **Enable policy** toggle to **No** and click the **Save** button.</span></span>
     >
     >

### <a name="disable-a-specific-conditional-access-policy"></a><span data-ttu-id="41e3b-346">Egy adott feltételes hozzáférési házirend letiltása</span><span class="sxs-lookup"><span data-stu-id="41e3b-346">Disable a specific conditional access policy</span></span>

<span data-ttu-id="41e3b-347">Ellenőrizze, illetve ellenőrizhető egy feltételes hozzáférési szabályzatot:</span><span class="sxs-lookup"><span data-stu-id="41e3b-347">To check or validate a single conditional access policy:</span></span>

1.  <span data-ttu-id="41e3b-348">Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="41e3b-348">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="41e3b-349">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="41e3b-349">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="41e3b-350">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="41e3b-350">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="41e3b-351">Kattintson a **vállalati alkalmazások** a navigációs menü.</span><span class="sxs-lookup"><span data-stu-id="41e3b-351">click **Enterprise applications** in the navigation menu.</span></span>

5.  <span data-ttu-id="41e3b-352">Kattintson a **feltételes hozzáférés** navigációs elem.</span><span class="sxs-lookup"><span data-stu-id="41e3b-352">click the **Conditional access** navigation item.</span></span>

6.  <span data-ttu-id="41e3b-353">Kattintson a érdekli ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="41e3b-353">click the policy you are interested in inspecting.</span></span>

7.  <span data-ttu-id="41e3b-354">Tiltsa le a házirendet úgy, hogy a **házirend engedélyezése** kapcsolót **nem** , és kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="41e3b-354">Disable the policy by setting the **Enable policy** toggle to **No** and click the **Save** button.</span></span>

## <a name="problems-with-application-consent"></a><span data-ttu-id="41e3b-355">Az alkalmazás hozzájárulásával problémák</span><span class="sxs-lookup"><span data-stu-id="41e3b-355">Problems with application consent</span></span>

<span data-ttu-id="41e3b-356">Alkalmazás-hozzáférés blokkolható, mert a megfelelő engedélyekkel a hozzájárulási művelet nem történt.</span><span class="sxs-lookup"><span data-stu-id="41e3b-356">Application access can be blocked because the proper permissions consent operation has not occurred.</span></span> <span data-ttu-id="41e3b-357">Az alábbiakban néhány alkalmazás hozzájárulási problémák megoldásához és hibaelhárítást végezhessen módját:</span><span class="sxs-lookup"><span data-stu-id="41e3b-357">Below are some ways you can troubleshoot and solve application consent issues:</span></span>

-   [<span data-ttu-id="41e3b-358">A felhasználói szintű hozzájárulási művelet végrehajtása</span><span class="sxs-lookup"><span data-stu-id="41e3b-358">Perform a user-level consent operation</span></span>](#perform-a-user-level-consent-operation)

-   [<span data-ttu-id="41e3b-359">Bármely alkalmazás rendszergazdai hozzájárulási művelethez</span><span class="sxs-lookup"><span data-stu-id="41e3b-359">Perform administrator-level consent operation for any application</span></span>](#perform-administrator-level-consent-operation-for-any-application)

-   [<span data-ttu-id="41e3b-360">Hajtsa végre a rendszergazdai hozzájárulási egyetlen bérlői alkalmazások</span><span class="sxs-lookup"><span data-stu-id="41e3b-360">Perform administrator-level consent for a single-tenant application</span></span>](#perform-administrator-level-consent-for-a-single-tenant-application)

-   [<span data-ttu-id="41e3b-361">Hajtsa végre a rendszergazdai hozzájárulási egy több-bérlős alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="41e3b-361">Perform administrator-level consent for a multi-tenant application</span></span>](#perform-administrator-level-consent-for-a-multi-tenant-application)

### <a name="perform-a-user-level-consent-operation"></a><span data-ttu-id="41e3b-362">A felhasználói szintű hozzájárulási művelet végrehajtása</span><span class="sxs-lookup"><span data-stu-id="41e3b-362">Perform a user-level consent operation</span></span>

-   <span data-ttu-id="41e3b-363">Bármely Open ID Connect-kompatibilis alkalmazás, amelyet engedélyeket igényel az alkalmazás bejelentkezési képernyő, lépjen az alkalmazásnak a bejelentkezett felhasználó számára egy felhasználói szintű hozzájárulás hajt végre.</span><span class="sxs-lookup"><span data-stu-id="41e3b-363">For any Open ID Connect-enabled application that requests permissions, navigating to the application’s sign in screen performs a user level consent to the application for the signed-in user.</span></span>

-   <span data-ttu-id="41e3b-364">Ha szeretne ehhez programozott módon, lásd: [egyes felhasználói hozzájárulás kérése](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#requesting-individual-user-consent).</span><span class="sxs-lookup"><span data-stu-id="41e3b-364">If you wish to do this programmatically, see [Requesting individual user consent](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#requesting-individual-user-consent).</span></span>

### <a name="perform-administrator-level-consent-operation-for-any-application"></a><span data-ttu-id="41e3b-365">Bármely alkalmazás rendszergazdai hozzájárulási művelethez</span><span class="sxs-lookup"><span data-stu-id="41e3b-365">Perform administrator-level consent operation for any application</span></span>

-   <span data-ttu-id="41e3b-366">A **csak a V1 alkalmazásmodell használatával fejlesztett alkalmazások**, beállíthatja, hogy a rendszergazda szintű hozzájárulási hozzáadásával megtörténik "**? rendszergazdai parancssorból =\_hozzájárulás**", az alkalmazás bejelentkezési URL-cím végén.</span><span class="sxs-lookup"><span data-stu-id="41e3b-366">For **only applications developed using the V1 application model**, you can force this administrator level consent to occur by adding “**?prompt=admin\_consent**” to the end of an application’s sign in URL.</span></span>

-   <span data-ttu-id="41e3b-367">A **bármely alkalmazás fejlesztett az V2 alkalmazásmodell**, kényszerítheti a rendszergazdai hozzájárul az utasítások alapján történik a **az engedélyeket kérhet a directory-rendszergazda** szakasza [használatával a rendszergazda jóváhagyását végpont](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#using-the-admin-consent-endpoint).</span><span class="sxs-lookup"><span data-stu-id="41e3b-367">For **any application developed using the V2 application model**, you can enforce this administrator-level consent to occur by following the instructions under the **Request the permissions from a directory admin** section of [Using the admin consent endpoint](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#using-the-admin-consent-endpoint).</span></span>

### <a name="perform-administrator-level-consent-for-a-single-tenant-application"></a><span data-ttu-id="41e3b-368">Hajtsa végre a rendszergazdai hozzájárulási egyetlen bérlői alkalmazások</span><span class="sxs-lookup"><span data-stu-id="41e3b-368">Perform administrator-level consent for a single-tenant application</span></span>

-   <span data-ttu-id="41e3b-369">A **egyetlen bérlői alkalmazások** (mint fejleszt, vagy a szervezet saját), engedélyek kéréséhez, amely végezheti el egy **rendszergazdai szintű hozzájárulási** művelet globális rendszergazdaként jelentkezik be, majd kattintson az összes felhasználó nevében a **engedélyeket** gomb tetején a **alkalmazás beállításjegyzék -&gt; összes alkalmazás -&gt; válasszon ki egy alkalmazást -&gt; szükséges engedélyek** panelen.</span><span class="sxs-lookup"><span data-stu-id="41e3b-369">For **single-tenant applications** that request permissions (like those you are developing or own in your organization), you can perform an **administrative-level consent** operation on behalf of all users by signing in as a Global Administrator and clicking on the **Grant permissions** button at the top of the **Application Registry -&gt; All Applications -&gt; Select an App -&gt; Required Permissions** blade.</span></span>

-   <span data-ttu-id="41e3b-370">A **bármely alkalmazás fejlesztett a V1 vagy V2 alkalmazásmodell**, kényszerítheti a rendszergazdai hozzájárul az utasítások alapján történik a **az engedélyeket kérhet a directory-rendszergazda** szakasza [használatával a rendszergazda jóváhagyását végpont](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#using-the-admin-consent-endpoint).</span><span class="sxs-lookup"><span data-stu-id="41e3b-370">For **any application developed using the V1 or V2 application model**, you can enforce this administrator-level consent to occur by following the instructions under the **Request the permissions from a directory admin** section of [Using the admin consent endpoint](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#using-the-admin-consent-endpoint).</span></span>

### <a name="perform-administrator-level-consent-for-a-multi-tenant-application"></a><span data-ttu-id="41e3b-371">Hajtsa végre a rendszergazdai hozzájárulási egy több-bérlős alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="41e3b-371">Perform administrator-level consent for a multi-tenant application</span></span>

-   <span data-ttu-id="41e3b-372">A **több-bérlős alkalmazásokhoz** adott kérelem engedélyek (például egy alkalmazás egy harmadik féltől származó, vagy a Microsoft, házon belül fejlesztett alkalmazásokra) hajthat végre egy **rendszergazdai szintű hozzájárulási** műveletet.</span><span class="sxs-lookup"><span data-stu-id="41e3b-372">For **multi-tenant applications** that request permissions (like an application a third party, or Microsoft, develops), you can perform an **administrative-level consent** operation.</span></span> <span data-ttu-id="41e3b-373">Jelentkezzen be globális rendszergazdaként, és kattintson a a **engedélyeket** gombra kattint, az a **vállalati alkalmazások –&gt; összes alkalmazás -&gt; válasszon ki egy alkalmazást -&gt; engedélyek** panel (rendelkezésre álló hamarosan).</span><span class="sxs-lookup"><span data-stu-id="41e3b-373">Sign in as a Global Administrator and clicking on the **Grant permissions** button under the **Enterprise Applications -&gt; All Applications -&gt; Select an App -&gt; Permissions** blade (available soon).</span></span>

-   <span data-ttu-id="41e3b-374">Is előírható, a rendszergazdai hozzájárul az utasítások alapján történik a **az engedélyeket kérhet a directory-rendszergazda** szakasza [használatával a rendszergazda jóváhagyását végpont](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#using-the-admin-consent-endpoint).</span><span class="sxs-lookup"><span data-stu-id="41e3b-374">You can also enforce this administrator-level consent to occur by following the instructions under the **Request the permissions from a directory admin** section of [Using the admin consent endpoint](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#using-the-admin-consent-endpoint).</span></span>

## <a name="next-steps"></a><span data-ttu-id="41e3b-375">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="41e3b-375">Next steps</span></span>
[<span data-ttu-id="41e3b-376">A rendszergazda jóváhagyását végpont használatával</span><span class="sxs-lookup"><span data-stu-id="41e3b-376">Using the admin consent endpoint</span></span>](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#using-the-admin-consent-endpoint)

