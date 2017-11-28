---
title: "Bejelentkezés Microsoft-alkalmazáshoz tooa aaaProblems |} Microsoft Docs"
description: "Bejelentkezés toofirst féltől Microsoft Applications (például Office 365) az Azure AD használatával tapasztalt kapcsolatos gyakori problémák elhárításában"
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
ms.openlocfilehash: 35849ca8dbaa909d17b6d0da572f5c11041a8559
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
## <a name="problems-signing-in-tooa-microsoft-application"></a><span data-ttu-id="89347-103">Bejelentkezés Microsoft-alkalmazáshoz tooa problémák</span><span class="sxs-lookup"><span data-stu-id="89347-103">Problems signing in tooa Microsoft application</span></span>

<span data-ttu-id="89347-104">Microsoft Applications (például Office 365 Exchange, SharePoint, Yammer, stb.) rendelve, és egy kicsit másképp, mint 3. fél SaaS-alkalmazások vagy más alkalmazásokat, az egyszeri bejelentkezéshez az Azure AD integrálása felügyelni.</span><span class="sxs-lookup"><span data-stu-id="89347-104">Microsoft Applications (like Office 365 Exchange, SharePoint, Yammer, etc.) are assigned and managed a bit differently than 3rd party SaaS applications or other applications you integrate with Azure AD for single sign on.</span></span>

<span data-ttu-id="89347-105">Háromféleképpen fő, amely a felhasználó kérheti le a hozzáférési tooa Microsoft közzétett alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="89347-105">There are three main ways that a user can get access tooa Microsoft-published application.</span></span>

-   <span data-ttu-id="89347-106">Office 365 hello vagy más fizetős csomagok-alkalmazások, a felhasználók keresztül hozzáférést kapnak **licenc hozzárendelése** vagy közvetlenül tootheir felhasználói fiókot, vagy egy csoport, a licenc csoport-alapú hozzárendelés funkcióval keresztül.</span><span class="sxs-lookup"><span data-stu-id="89347-106">For applications in hello Office 365 or other paid suites, users are granted access through **license assignment** either directly tootheir user account, or through a group using our group-based license assignment capability.</span></span>

-   <span data-ttu-id="89347-107">Az, hogy a Microsoft vagy valamely harmadik fél közzéteszi szabadon bárki toouse alkalmazások, felhasználók adható hozzáférés a **felhasználói hozzájárulás**.</span><span class="sxs-lookup"><span data-stu-id="89347-107">For applications that Microsoft or a Third Party publishes freely for anyone toouse, users may be granted access through **user consent**.</span></span> <span data-ttu-id="89347-108">This0 azt jelenti, hogy toohello alkalmazásokban az Azure AD munkahelyi vagy iskolai fiókkal jelentkeznek, és engedélyezze a toohave hozzáférés korlátozott toosome adatkészletet a fiókjuk.</span><span class="sxs-lookup"><span data-stu-id="89347-108">This0 means that they sign in toohello application with their Azure AD Work or School account and allow it toohave access toosome limited set of data on their account.</span></span>

-   <span data-ttu-id="89347-109">Az, hogy a Microsoft vagy a 3. fél közzéteszi szabadon bárki toouse alkalmazások, felhasználók is adható hozzáférés a **rendszergazda jóváhagyását**.</span><span class="sxs-lookup"><span data-stu-id="89347-109">For applications that Microsoft or a 3rd Party publishes freely for anyone toouse, users may also be granted access through **administrator consent**.</span></span> <span data-ttu-id="89347-110">Ez azt jelenti, hogy a rendszergazda azt észlelte, hello alkalmazás felhasználhatja hello szervezet minden tagja számára, hogy jelentkezzen be egy globális rendszergazdai fiókkal toohello alkalmazás, és adjon hozzáférést tooeveryone hello szervezetében.</span><span class="sxs-lookup"><span data-stu-id="89347-110">This means that an administrator has determined hello application may be used by everyone in hello organization, so they sign in toohello application with a Global Administrator account and grant access tooeveryone in hello organization.</span></span>

<span data-ttu-id="89347-111">tootroubleshoot a problémát, hello kezdődik [általános probléma területet, és alkalmazás-hozzáférés tooconsider](#general-problem-areas-with-application-access-to-consider) majd elolvashatják a hello [forgatókönyv: lépéseket tootroubleshoot Microsoft Application hozzáférés](#walkthrough-steps-to-troubleshoot-microsoft-application-access) hello részleteinek tooget.</span><span class="sxs-lookup"><span data-stu-id="89347-111">tootroubleshoot your issue, start with hello [General Problem Areas with Application Access tooconsider](#general-problem-areas-with-application-access-to-consider) and then read hello [Walkthrough: Steps tootroubleshoot Microsoft Application access](#walkthrough-steps-to-troubleshoot-microsoft-application-access) tooget into hello details.</span></span>

## <a name="general-problem-areas-with-application-access-tooconsider"></a><span data-ttu-id="89347-112">Az alkalmazás-hozzáférés tooconsider általános problémás területek</span><span class="sxs-lookup"><span data-stu-id="89347-112">General Problem Areas with Application Access tooconsider</span></span>

<span data-ttu-id="89347-113">Az alábbiakban felsoroljuk a hello részletezhető Ha felmérheti, ahol toostart, de javasoljuk, olvassa el a hello forgatókönyv tooget is gyorsan általános problématerületeket: [forgatókönyv: lépéseket tootroubleshoot Microsoft Application hozzáférés](#walkthrough-steps-to-troubleshoot-microsoft-application-access).</span><span class="sxs-lookup"><span data-stu-id="89347-113">Below is a list of hello general problem areas that you can drill into if you have an idea of where toostart, but we recommend you read hello walkthrough tooget going quickly: [Walkthrough: Steps tootroubleshoot Microsoft Application access](#walkthrough-steps-to-troubleshoot-microsoft-application-access).</span></span>

-   [<span data-ttu-id="89347-114">Hello felhasználói fiókkal kapcsolatos problémák</span><span class="sxs-lookup"><span data-stu-id="89347-114">Problems with hello user’s account</span></span>](#problems-with-the-users-account)

-   [<span data-ttu-id="89347-115">A csoportokkal kapcsolatos problémák</span><span class="sxs-lookup"><span data-stu-id="89347-115">Problems with groups</span></span>](#problems-with-groups)

-   [<span data-ttu-id="89347-116">Feltételes hozzáférési szabályzatokkal kapcsolatos problémák</span><span class="sxs-lookup"><span data-stu-id="89347-116">Problems with conditional access policies</span></span>](#problems-with-conditional-access-policies)

-   [<span data-ttu-id="89347-117">Az alkalmazás hozzájárulásával problémák</span><span class="sxs-lookup"><span data-stu-id="89347-117">Problems with application consent</span></span>](#problems-with-application-consent)

## <a name="steps-tootroubleshoot-microsoft-application-access"></a><span data-ttu-id="89347-118">Microsoft Application hozzáférés lépéseket tootroubleshoot</span><span class="sxs-lookup"><span data-stu-id="89347-118">Steps tootroubleshoot Microsoft Application access</span></span>

<span data-ttu-id="89347-119">Alább néhány gyakori problémák segítsen futnak be amikor a felhasználók nem jelentkezhet be Microsoft-alkalmazáshoz tooa.</span><span class="sxs-lookup"><span data-stu-id="89347-119">Below are some common issues folks run into when their users cannot sign in tooa Microsoft application.</span></span>

-   <span data-ttu-id="89347-120">Általános problémák első toocheck</span><span class="sxs-lookup"><span data-stu-id="89347-120">General issues toocheck first</span></span>

  * <span data-ttu-id="89347-121">Ellenőrizze, hogy hello felhasználói bejelentkezéskor van toohello **javítsa ki az URL-cím** és nem egy helyi alkalmazás URL-címet.</span><span class="sxs-lookup"><span data-stu-id="89347-121">Make sure hello user is signing in toohello **correct URL** and not a local application URL.</span></span>

  * <span data-ttu-id="89347-122">Ellenőrizze, hogy a hello felhasználói fiók **nincs zárolva.**</span><span class="sxs-lookup"><span data-stu-id="89347-122">Make sure hello user’s account is **not locked out.**</span></span>

  * <span data-ttu-id="89347-123">Győződjön meg arról, hogy hello **felhasználói fiók létezik** az Azure Active Directoryban.</span><span class="sxs-lookup"><span data-stu-id="89347-123">Make sure hello **user’s account exists** in Azure Active Directory.</span></span> [<span data-ttu-id="89347-124">Ellenőrizze, hogy egy felhasználói fiók létezik-e az Azure Active Directoryban</span><span class="sxs-lookup"><span data-stu-id="89347-124">Check if a user account exists in Azure Active Directory</span></span>](#problems-with-the-users-account)

  * <span data-ttu-id="89347-125">Ellenőrizze, hogy a hello felhasználói fiók **engedélyezett** a indított bejelentkezések. [A felhasználói fiók állapotának ellenőrzése](#problems-with-the-users-account)</span><span class="sxs-lookup"><span data-stu-id="89347-125">Make sure hello user’s account is **enabled** for sign ins. [Check a user’s account status](#problems-with-the-users-account)</span></span>

  * <span data-ttu-id="89347-126">Győződjön meg arról, hogy hello felhasználói **nem lejárt vagy elfelejtett jelszó.**</span><span class="sxs-lookup"><span data-stu-id="89347-126">Make sure hello user’s **password is not expired or forgotten.**</span></span> <span data-ttu-id="89347-127">[A jelszó alaphelyzetbe állítása](#reset-a-users-password) vagy [önkiszolgáló jelszó-visszaállítás engedélyezése](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started)</span><span class="sxs-lookup"><span data-stu-id="89347-127">[Reset a user’s password](#reset-a-users-password) or [Enable self-service password reset](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started)</span></span>

   * <span data-ttu-id="89347-128">Győződjön meg arról, hogy **multi-factor Authentication** nem blokkolja a hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="89347-128">Make sure **Multi-Factor Authentication** is not blocking user access.</span></span> <span data-ttu-id="89347-129">[A felhasználó a multi-factor authentication állapotának](#check-a-users-multi-factor-authentication-status) vagy [ellenőrizni kell a felhasználó hitelesítési kapcsolattartási adatait](#check-a-users-authentication-contact-info)</span><span class="sxs-lookup"><span data-stu-id="89347-129">[Check a user’s multi-factor authentication status](#check-a-users-multi-factor-authentication-status) or [Check a user’s authentication contact info](#check-a-users-authentication-contact-info)</span></span>

   * <span data-ttu-id="89347-130">Győződjön meg arról, hogy egy **feltételes hozzáférési házirend** vagy **Identity Protection** házirend nem blokkolja a hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="89347-130">Make sure a **Conditional Access policy** or **Identity Protection** policy is not blocking user access.</span></span> <span data-ttu-id="89347-131">[Ellenőrizze a megadott feltételes hozzáférési házirend](#problems-with-conditional-access-policies) vagy [egy adott alkalmazás feltételes hozzáférési házirendben](#check-a-specific-applications-conditional-access-policy) vagy [letiltása adott feltételes hozzáférési házirend](#disable-a-specific-conditional-access-policy)</span><span class="sxs-lookup"><span data-stu-id="89347-131">[Check a specific conditional access policy](#problems-with-conditional-access-policies) or [Check a specific application’s conditional access policy](#check-a-specific-applications-conditional-access-policy) or [Disable a specific conditional access policy](#disable-a-specific-conditional-access-policy)</span></span>

   * <span data-ttu-id="89347-132">Győződjön meg arról, hogy a felhasználó **hitelesítési kapcsolattartási adatai** működik-e toodate tooallow többtényezős hitelesítést vagy feltételes hozzáférési házirendek toobe lépnek érvénybe.</span><span class="sxs-lookup"><span data-stu-id="89347-132">Make sure that a user’s **authentication contact info** is up toodate tooallow Multi-Factor Authentication or Conditional Access policies toobe enforced.</span></span> <span data-ttu-id="89347-133">[A felhasználó a multi-factor authentication állapotának](#check-a-users-multi-factor-authentication-status) vagy [ellenőrizni kell a felhasználó hitelesítési kapcsolattartási adatait](#check-a-users-authentication-contact-info)</span><span class="sxs-lookup"><span data-stu-id="89347-133">[Check a user’s multi-factor authentication status](#check-a-users-multi-factor-authentication-status) or [Check a user’s authentication contact info](#check-a-users-authentication-contact-info)</span></span>

-   <span data-ttu-id="89347-134">A **Microsoft** **licenc igénylő alkalmazások** (például az Office365), az alábbiakban néhány konkrét problémák toocheck után kiderül, hogy a fenti általános problémák hello:</span><span class="sxs-lookup"><span data-stu-id="89347-134">For **Microsoft** **applications that require a license** (like Office365), here are some specific issues toocheck once you have ruled out hello general issues above:</span></span>

   * <span data-ttu-id="89347-135">Győződjön meg arról hello felhasználó vagy van egy **licenccel.**</span><span class="sxs-lookup"><span data-stu-id="89347-135">Ensure hello user or has a **license assigned.**</span></span> <span data-ttu-id="89347-136">[Ellenőrizze a felhasználó licenc-hozzárendeléseket](#check-a-users-assigned-licenses) vagy [egy csoporthoz hozzárendelt licencekkel ellenőrzése](#check-a-groups-assigned-licenses)</span><span class="sxs-lookup"><span data-stu-id="89347-136">[Check a user’s assigned licenses](#check-a-users-assigned-licenses) or [Check a group’s assigned licenses](#check-a-groups-assigned-licenses)</span></span>

   * <span data-ttu-id="89347-137">Ha hello licenc **tooa hozzárendelt** **statikus csoport**, győződjön meg arról, hogy hello **felhasználó tagja** csoport.</span><span class="sxs-lookup"><span data-stu-id="89347-137">If hello license is **assigned tooa** **static group**, ensure that hello **user is a member** of that group.</span></span> [<span data-ttu-id="89347-138">A felhasználói csoporttagság ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="89347-138">Check a user’s group memberships</span></span>](#check-a-users-group-memberships)

   * <span data-ttu-id="89347-139">Ha hello licenc **tooa hozzárendelt** **dinamikus csoport**, győződjön meg arról, hogy hello **dinamikus csoport szabály megfelelően van-e beállítva**.</span><span class="sxs-lookup"><span data-stu-id="89347-139">If hello license is **assigned tooa** **dynamic group**, ensure that hello **dynamic group rule is set correctly**.</span></span> [<span data-ttu-id="89347-140">A dinamikus csoport tagsági feltételek ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="89347-140">Check a dynamic group’s membership criteria</span></span>](#check-a-dynamic-groups-membership-criteria)

   * <span data-ttu-id="89347-141">Ha hello licenc **tooa hozzárendelt** **dinamikus csoport**, győződjön meg arról, hogy hello dinamikus csoport rendelkezik **fejezett** tagságát és, hogy hello **felhasználó egy tag** (Ez eltarthat egy ideig).</span><span class="sxs-lookup"><span data-stu-id="89347-141">If hello license is **assigned tooa** **dynamic group**, ensure that hello dynamic group has **finished processing** its membership and that hello **user is a member** (this can take some time).</span></span> [<span data-ttu-id="89347-142">A felhasználói csoporttagság ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="89347-142">Check a user’s group memberships</span></span>](#check-a-users-group-memberships)

   *  <span data-ttu-id="89347-143">Ha mindenképpen rendel hozzá licencet hello, ellenőrizze, hogy hello licenc **nem járt le**.</span><span class="sxs-lookup"><span data-stu-id="89347-143">Once you make sure hello license is assigned, make sure hello license is **not expired**.</span></span>

   *  <span data-ttu-id="89347-144">Ellenőrizze, hogy hello licenc **hello alkalmazás** érik el.</span><span class="sxs-lookup"><span data-stu-id="89347-144">Make sure hello license is **for hello application** they are accessing.</span></span>

-   <span data-ttu-id="89347-145">A **Microsoft** **alkalmazások, amelyek nem igényelnek licencet**, az alábbiakban néhány más dolgokat toocheck:</span><span class="sxs-lookup"><span data-stu-id="89347-145">For **Microsoft** **applications that don’t require a license**, here are some other things toocheck:</span></span>

   * <span data-ttu-id="89347-146">Ha hello alkalmazás **felhasználói szintű engedélyek** (például "érhetik el a felhasználó postaládájához"), győződjön meg arról, hogy hello a felhasználó bejelentkezett-e toohello alkalmazás, és hajtott végre egy **felhasználói szintű hozzájárulási művelet**  toolet hello alkalmazás fér hozzá az adatokhoz.</span><span class="sxs-lookup"><span data-stu-id="89347-146">If hello application is requesting **user-level permissions** (for example “Access this user’s mailbox”), make sure that hello user has signed in toohello application and has performed a **user-level consent operation** toolet hello application access her data.</span></span>

   * <span data-ttu-id="89347-147">Ha hello alkalmazás **rendszergazdai engedélyek** (például "érhetik el az összes felhasználó postaládák"), győződjön meg arról, hogy elvégezte-e globális rendszergazda egy **rendszergazdai hozzájárulási művelet az összes olyan felhasználó nevében** hello szervezetében.</span><span class="sxs-lookup"><span data-stu-id="89347-147">If hello application is requesting **administrator-level permissions** (for example “Access all user’s mailboxes”), make sure that a Global Administrator has performed an **administrator-level consent operation on behalf of all users** in hello organization.</span></span>

## <a name="problems-with-hello-users-account"></a><span data-ttu-id="89347-148">Hello felhasználói fiókkal kapcsolatos problémák</span><span class="sxs-lookup"><span data-stu-id="89347-148">Problems with hello user’s account</span></span>

<span data-ttu-id="89347-149">Alkalmazás-hozzáférés blokkolható toohello alkalmazáshoz rendelt felhasználó tooa kapcsolatos probléma miatt.</span><span class="sxs-lookup"><span data-stu-id="89347-149">Application access can be blocked due tooa problem with a user that is assigned toohello application.</span></span> <span data-ttu-id="89347-150">Az alábbiakban néhány módszert hibákat, és a felhasználó és a fiók beállításainak problémáinak megoldásával:</span><span class="sxs-lookup"><span data-stu-id="89347-150">Below are some ways you can troubleshoot and solve problems with users and their account settings:</span></span>

-   [<span data-ttu-id="89347-151">Ellenőrizze, hogy egy felhasználói fiók létezik-e az Azure Active Directoryban</span><span class="sxs-lookup"><span data-stu-id="89347-151">Check if a user account exists in Azure Active Directory</span></span>](#check-if-a-user-account-exists-in-azure-active-directory)

-   [<span data-ttu-id="89347-152">A felhasználói fiók állapotának ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="89347-152">Check a user’s account status</span></span>](#check-a-users-account-status)

-   [<span data-ttu-id="89347-153">A jelszó alaphelyzetbe állítása</span><span class="sxs-lookup"><span data-stu-id="89347-153">Reset a user’s password</span></span>](#reset-a-users-password)

-   [<span data-ttu-id="89347-154">Önkiszolgáló jelszóátállítás engedélyezése</span><span class="sxs-lookup"><span data-stu-id="89347-154">Enable self-service password reset</span></span>](#enable-self-service-password-reset)

-   [<span data-ttu-id="89347-155">A felhasználó a multi-factor authentication állapotának ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="89347-155">Check a user’s multi-factor authentication status</span></span>](#check-a-users-multi-factor-authentication-status)

-   [<span data-ttu-id="89347-156">Ellenőrizze a felhasználó hitelesítési kapcsolattartási adatai</span><span class="sxs-lookup"><span data-stu-id="89347-156">Check a user’s authentication contact info</span></span>](#check-a-users-authentication-contact-info)

-   [<span data-ttu-id="89347-157">A felhasználói csoporttagság ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="89347-157">Check a user’s group memberships</span></span>](#check-a-users-group-memberships)

-   [<span data-ttu-id="89347-158">A felhasználó licenc-hozzárendeléseket ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="89347-158">Check a user’s assigned licenses</span></span>](#check-a-users-assigned-licenses)

-   [<span data-ttu-id="89347-159">A felhasználó a licenc hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="89347-159">Assign a user a license</span></span>](#assign-a-user-a-license)

### <a name="check-if-a-user-account-exists-in-azure-active-directory"></a><span data-ttu-id="89347-160">Ellenőrizze, hogy egy felhasználói fiók létezik-e az Azure Active Directoryban</span><span class="sxs-lookup"><span data-stu-id="89347-160">Check if a user account exists in Azure Active Directory</span></span>

<span data-ttu-id="89347-161">toocheck, ha egy felhasználói fiók található, kövesse a hello alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="89347-161">toocheck if a user’s account is present, follow hello steps below:</span></span>

1.  <span data-ttu-id="89347-162">Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="89347-162">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="89347-163">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="89347-163">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="89347-164">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="89347-164">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="89347-165">Kattintson a **felhasználók és csoportok** hello navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="89347-165">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="89347-166">Kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="89347-166">click **All users**.</span></span>

6.  <span data-ttu-id="89347-167">**Keresési** hello felhasználó érdekli, és **hello sorban kattintson** tooselect.</span><span class="sxs-lookup"><span data-stu-id="89347-167">**Search** for hello user you are interested in and **click hello row** tooselect.</span></span>

7.  <span data-ttu-id="89347-168">Ellenőrizze a hello tulajdonságainak hello felhasználói objektum toobe meg arról, hogy azok meg, mint a várt, és nincs adat hiányzik.</span><span class="sxs-lookup"><span data-stu-id="89347-168">Check hello properties of hello user object toobe sure that they look as you expect and no data is missing.</span></span>

### <a name="check-a-users-account-status"></a><span data-ttu-id="89347-169">A felhasználói fiók állapotának ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="89347-169">Check a user’s account status</span></span>

<span data-ttu-id="89347-170">toocheck egy felhasználói fiók állapota, kövesse az alábbi hello lépéseket:</span><span class="sxs-lookup"><span data-stu-id="89347-170">toocheck a user’s account status, follow hello steps below:</span></span>

1.  <span data-ttu-id="89347-171">Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="89347-171">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="89347-172">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="89347-172">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="89347-173">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="89347-173">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="89347-174">Kattintson a **felhasználók és csoportok** hello navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="89347-174">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="89347-175">Kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="89347-175">click **All users**.</span></span>

6.  <span data-ttu-id="89347-176">**Keresési** hello felhasználó érdekli, és **hello sorban kattintson** tooselect.</span><span class="sxs-lookup"><span data-stu-id="89347-176">**Search** for hello user you are interested in and **click hello row** tooselect.</span></span>

7.  <span data-ttu-id="89347-177">Kattintson a **profil**.</span><span class="sxs-lookup"><span data-stu-id="89347-177">click **Profile**.</span></span>

8.  <span data-ttu-id="89347-178">A **beállítások** ügyeljen arra, hogy **blokk bejelentkezés** értéke túl**nem**.</span><span class="sxs-lookup"><span data-stu-id="89347-178">Under **Settings** ensure that **Block sign in** is set too**No**.</span></span>

### <a name="reset-a-users-password"></a><span data-ttu-id="89347-179">A jelszó alaphelyzetbe állítása</span><span class="sxs-lookup"><span data-stu-id="89347-179">Reset a user’s password</span></span>

<span data-ttu-id="89347-180">tooreset egy felhasználó jelszavát, hajtsa végre hello alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="89347-180">tooreset a user’s password, follow hello steps below:</span></span>

1.  <span data-ttu-id="89347-181">Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="89347-181">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="89347-182">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="89347-182">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="89347-183">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="89347-183">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="89347-184">Kattintson a **felhasználók és csoportok** hello navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="89347-184">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="89347-185">Kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="89347-185">click **All users**.</span></span>

6.  <span data-ttu-id="89347-186">**Keresési** hello felhasználó érdekli, és **hello sorban kattintson** tooselect.</span><span class="sxs-lookup"><span data-stu-id="89347-186">**Search** for hello user you are interested in and **click hello row** tooselect.</span></span>

7.  <span data-ttu-id="89347-187">Kattintson a hello **jelszó-átállítási** hello felhasználói panel felső hello gombra.</span><span class="sxs-lookup"><span data-stu-id="89347-187">click hello **Reset password** button at hello top of hello user blade.</span></span>

8.  <span data-ttu-id="89347-188">Kattintson a hello **jelszó-átállítási** hello gombjára **jelszó-átállítási** megjelenő panelen.</span><span class="sxs-lookup"><span data-stu-id="89347-188">click hello **Reset password** button on hello **Reset password** blade that appears.</span></span>

9.  <span data-ttu-id="89347-189">Másolás hello **ideiglenes jelszó** vagy **adjon meg egy új jelszót** hello felhasználó számára.</span><span class="sxs-lookup"><span data-stu-id="89347-189">Copy hello **temporary password** or **enter a new password** for hello user.</span></span>

10. <span data-ttu-id="89347-190">Az új jelszó toohello felhasználói kommunikációhoz, ezt a jelszót, a következő során jelentkezzen be az Active Directory tooAzure szükséges toochange legyenek.</span><span class="sxs-lookup"><span data-stu-id="89347-190">Communicate this new password toohello user, they be required toochange this password during their next sign in tooAzure Active Directory.</span></span>

### <a name="enable-self-service-password-reset"></a><span data-ttu-id="89347-191">Az önkiszolgáló jelszó-visszaállítás engedélyezése</span><span class="sxs-lookup"><span data-stu-id="89347-191">Enable self-service password reset</span></span>

<span data-ttu-id="89347-192">tooenable önkiszolgáló jelszó alaphelyzetbe állítása, kövesse az alábbi hello telepítési lépéseket:</span><span class="sxs-lookup"><span data-stu-id="89347-192">tooenable self-service password reset, follow hello deployment steps below:</span></span>

-   [<span data-ttu-id="89347-193">Engedélyezze a felhasználók tooreset a Azure Active Directory-jelszavaikat</span><span class="sxs-lookup"><span data-stu-id="89347-193">Enable users tooreset their Azure Active Directory passwords</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started#enable-users-to-reset-their-azure-ad-passwords)

-   [<span data-ttu-id="89347-194">Felhasználók tooreset engedélyezése vagy az Active Directory helyszíni jelszavak módosítása</span><span class="sxs-lookup"><span data-stu-id="89347-194">Enable users tooreset or change their Active Directory on-premises passwords</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started#enable-users-to-reset-or-change-their-ad-passwords)

### <a name="check-a-users-multi-factor-authentication-status"></a><span data-ttu-id="89347-195">A felhasználó a multi-factor authentication állapotának ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="89347-195">Check a user’s multi-factor authentication status</span></span>

<span data-ttu-id="89347-196">a felhasználó toocheck a multi-factor Authentication hitelesítési állapot, hajtsa végre hello alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="89347-196">toocheck a user’s multi-factor authentication status, follow hello steps below:</span></span>

1.  <span data-ttu-id="89347-197">Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="89347-197">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="89347-198">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="89347-198">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="89347-199">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="89347-199">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="89347-200">Kattintson a **felhasználók és csoportok** hello navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="89347-200">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="89347-201">Kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="89347-201">click **All users**.</span></span>

6.  <span data-ttu-id="89347-202">Kattintson a hello **multi-factor Authentication** hello panel felső hello gombra.</span><span class="sxs-lookup"><span data-stu-id="89347-202">click hello **Multi-Factor Authentication** button at hello top of hello blade.</span></span>

7.  <span data-ttu-id="89347-203">Egyszer hello **multi-factor Authentication felügyeleti portál** terhelés esetén gondoskodjon arról, hogy hello **felhasználók** fülre.</span><span class="sxs-lookup"><span data-stu-id="89347-203">Once hello **Multi-Factor Authentication Administration Portal** loads, ensure you are on hello **Users** tab.</span></span>

8.  <span data-ttu-id="89347-204">Keresés, szűréshez vagy rendezés hello felhasználói keresése hello azoknak a felhasználóknak.</span><span class="sxs-lookup"><span data-stu-id="89347-204">Find hello user in hello list of users by searching, filtering, or sorting.</span></span>

9.  <span data-ttu-id="89347-205">Azon felhasználók hello listájáról válassza hello felhasználói és **engedélyezése**, **tiltsa le a**, vagy **érvényesítése** többtényezős hitelesítést a kívánt módon működjenek.</span><span class="sxs-lookup"><span data-stu-id="89347-205">Select hello user from hello list of users and **Enable**, **Disable**, or **Enforce** multi-factor authentication as desired.</span></span>

  * <span data-ttu-id="89347-206">**Megjegyzés:**: Ha egy felhasználó egy **kényszerített** állapotba kerül, előfordulhat, hogy túl beállítása**letiltott** ideiglenesen toolet újra be a fiókba.</span><span class="sxs-lookup"><span data-stu-id="89347-206">**Note**: If a user is in an **Enforced** state, you may set them too**Disabled** temporarily toolet them back into their account.</span></span> <span data-ttu-id="89347-207">Miután bekerültek vissza, majd módosíthatja állapotukra túl**engedélyezve** újra toorequire őket toore regisztrálása során a következő kapcsolattartási adatait jelentkezzen be.</span><span class="sxs-lookup"><span data-stu-id="89347-207">Once they are back in, you can then change their state too**Enabled** again toorequire them toore-register their contact information during their next sign in.</span></span> <span data-ttu-id="89347-208">Másik lehetőségként a lépésekkel hello a hello [ellenőrizni kell a felhasználó hitelesítési kapcsolattartási adatait](#check-a-users-authentication-contact-info) tooverify, vagy állítsa be ezeket az adatokat a számukra.</span><span class="sxs-lookup"><span data-stu-id="89347-208">Alternatively, you can follow hello steps in hello [Check a user’s authentication contact info](#check-a-users-authentication-contact-info) tooverify or set this data for them.</span></span>

### <a name="check-a-users-authentication-contact-info"></a><span data-ttu-id="89347-209">Ellenőrizze a felhasználó hitelesítési kapcsolattartási adatai</span><span class="sxs-lookup"><span data-stu-id="89347-209">Check a user’s authentication contact info</span></span>

<span data-ttu-id="89347-210">a felhasználó hitelesítési kapcsolattartási adatok többtényezős hitelesítést, a feltételes hozzáférés, a Identity Protection és a jelszó alaphelyzetbe állításához használt toocheck hello lépéseket kövesse:</span><span class="sxs-lookup"><span data-stu-id="89347-210">toocheck a user’s authentication contact info used for Multi-factor authentication, Conditional Access, Identity Protection, and Password Reset, follow hello steps below:</span></span>

1.  <span data-ttu-id="89347-211">Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="89347-211">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="89347-212">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="89347-212">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="89347-213">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="89347-213">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="89347-214">Kattintson a **felhasználók és csoportok** hello navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="89347-214">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="89347-215">Kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="89347-215">click **All users**.</span></span>

6.  <span data-ttu-id="89347-216">**Keresési** hello felhasználó érdekli, és **hello sorban kattintson** tooselect.</span><span class="sxs-lookup"><span data-stu-id="89347-216">**Search** for hello user you are interested in and **click hello row** tooselect.</span></span>

7.  <span data-ttu-id="89347-217">Kattintson a **profil**.</span><span class="sxs-lookup"><span data-stu-id="89347-217">click **Profile**.</span></span>

8.  <span data-ttu-id="89347-218">Görgessen lefelé, túl**hitelesítési kapcsolattartási adatai**.</span><span class="sxs-lookup"><span data-stu-id="89347-218">Scroll down too**Authentication contact info**.</span></span>

9.  <span data-ttu-id="89347-219">**Felülvizsgálati** hello adatok regisztrált hello felhasználói és a frissítési igény szerint.</span><span class="sxs-lookup"><span data-stu-id="89347-219">**Review** hello data registered for hello user and update as needed.</span></span>

### <a name="check-a-users-group-memberships"></a><span data-ttu-id="89347-220">A felhasználói csoporttagság ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="89347-220">Check a user’s group memberships</span></span>

<span data-ttu-id="89347-221">toocheck egy felhasználó csoporttagságok hajtsa végre az alábbi hello lépéseket:</span><span class="sxs-lookup"><span data-stu-id="89347-221">toocheck a user’s group memberships, follow hello steps below:</span></span>

1.  <span data-ttu-id="89347-222">Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="89347-222">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="89347-223">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="89347-223">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="89347-224">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="89347-224">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="89347-225">Kattintson a **felhasználók és csoportok** hello navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="89347-225">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="89347-226">Kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="89347-226">click **All users**.</span></span>

6.  <span data-ttu-id="89347-227">**Keresési** hello felhasználó érdekli, és **hello sorban kattintson** tooselect.</span><span class="sxs-lookup"><span data-stu-id="89347-227">**Search** for hello user you are interested in and **click hello row** tooselect.</span></span>

7.  <span data-ttu-id="89347-228">Kattintson a **csoportok** toosee, amely hello felhasználói csoportok tagja.</span><span class="sxs-lookup"><span data-stu-id="89347-228">click **Groups** toosee which groups hello user is a member of.</span></span>

### <a name="check-a-users-assigned-licenses"></a><span data-ttu-id="89347-229">A felhasználó licenc-hozzárendeléseket ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="89347-229">Check a user’s assigned licenses</span></span>

<span data-ttu-id="89347-230">toocheck a felhasználó hozzárendelt licencek, hajtsa végre hello alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="89347-230">toocheck a user’s assigned licenses, follow hello steps below:</span></span>

1.  <span data-ttu-id="89347-231">Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="89347-231">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="89347-232">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="89347-232">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="89347-233">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="89347-233">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="89347-234">Kattintson a **felhasználók és csoportok** hello navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="89347-234">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="89347-235">Kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="89347-235">click **All users**.</span></span>

6.  <span data-ttu-id="89347-236">**Keresési** hello felhasználó érdekli, és **hello sorban kattintson** tooselect.</span><span class="sxs-lookup"><span data-stu-id="89347-236">**Search** for hello user you are interested in and **click hello row** tooselect.</span></span>

7.  <span data-ttu-id="89347-237">Kattintson a **licencek** toosee licencek hello felhasználóhoz van rendelve.</span><span class="sxs-lookup"><span data-stu-id="89347-237">click **Licenses** toosee which licenses hello user currently has assigned.</span></span>

### <a name="assign-a-user-a-license"></a><span data-ttu-id="89347-238">A felhasználó a licenc hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="89347-238">Assign a user a license</span></span> 

<span data-ttu-id="89347-239">a licenc tooa felhasználó tooassign kövesse hello alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="89347-239">tooassign a license tooa user, follow hello steps below:</span></span>

1.  <span data-ttu-id="89347-240">Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="89347-240">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="89347-241">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="89347-241">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="89347-242">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="89347-242">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="89347-243">Kattintson a **felhasználók és csoportok** hello navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="89347-243">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="89347-244">Kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="89347-244">click **All users**.</span></span>

6.  <span data-ttu-id="89347-245">**Keresési** hello felhasználó érdekli, és **hello sorban kattintson** tooselect.</span><span class="sxs-lookup"><span data-stu-id="89347-245">**Search** for hello user you are interested in and **click hello row** tooselect.</span></span>

7.  <span data-ttu-id="89347-246">Kattintson a **licencek** toosee licencek hello felhasználóhoz van rendelve.</span><span class="sxs-lookup"><span data-stu-id="89347-246">click **Licenses** toosee which licenses hello user currently has assigned.</span></span>

8.  <span data-ttu-id="89347-247">Kattintson a hello **hozzárendelése** gombra.</span><span class="sxs-lookup"><span data-stu-id="89347-247">click hello **Assign** button.</span></span>

9.  <span data-ttu-id="89347-248">Válassza ki **egy vagy több termék** választható termékek hello listája.</span><span class="sxs-lookup"><span data-stu-id="89347-248">Select **one or more products** from hello list of available products.</span></span>

10. <span data-ttu-id="89347-249">**Nem kötelező** hello kattintson **hozzárendelés beállításai** elem toogranularly rendelje hozzá a termékek.</span><span class="sxs-lookup"><span data-stu-id="89347-249">**Optional** click hello **assignment options** item toogranularly assign products.</span></span> <span data-ttu-id="89347-250">Kattintson a **Ok** amikor ez befejeződik.</span><span class="sxs-lookup"><span data-stu-id="89347-250">Click **Ok** when this is completed.</span></span>

11. <span data-ttu-id="89347-251">Kattintson a hello **hozzárendelése** tooassign ezen licencek toothis felhasználói gombra.</span><span class="sxs-lookup"><span data-stu-id="89347-251">Click hello **Assign** button tooassign these licenses toothis user.</span></span>

## <a name="problems-with-groups"></a><span data-ttu-id="89347-252">A csoportokkal kapcsolatos problémák</span><span class="sxs-lookup"><span data-stu-id="89347-252">Problems with groups</span></span>

<span data-ttu-id="89347-253">Alkalmazás-hozzáférés blokkolható egy csoportot, amely hozzá van rendelve toohello alkalmazás tooa kapcsolatos probléma miatt.</span><span class="sxs-lookup"><span data-stu-id="89347-253">Application access can be blocked due tooa problem with a group that is assigned toohello application.</span></span> <span data-ttu-id="89347-254">Az alábbiakban néhány módszert, hibaelhárítási és csoportokkal vagy csoporttagsággal kapcsolatos problémák megoldásához:</span><span class="sxs-lookup"><span data-stu-id="89347-254">Below are some ways you can troubleshoot and solve problems with groups and group memberships:</span></span>

-   [<span data-ttu-id="89347-255">Ellenőrizze a csoport tagságát</span><span class="sxs-lookup"><span data-stu-id="89347-255">Check a group’s membership</span></span>](#check-a-groups-membership)

-   [<span data-ttu-id="89347-256">A dinamikus csoport tagsági feltételek ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="89347-256">Check a dynamic group’s membership criteria</span></span>](#check-a-dynamic-groups-membership-criteria)

-   [<span data-ttu-id="89347-257">Ellenőrizze a csoporthoz hozzárendelt licencekkel</span><span class="sxs-lookup"><span data-stu-id="89347-257">Check a group’s assigned licenses</span></span>](#check-a-groups-assigned-licenses)

-   [<span data-ttu-id="89347-258">Újból feldolgozza a csoport licencek</span><span class="sxs-lookup"><span data-stu-id="89347-258">Reprocess a group’s licenses</span></span>](#reprocess-a-groups-licenses)

-   [<span data-ttu-id="89347-259">Egy csoport a licenc hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="89347-259">Assign a group a license</span></span>](#assign-a-group-a-license)

### <a name="check-a-groups-membership"></a><span data-ttu-id="89347-260">Ellenőrizze a csoport tagságát</span><span class="sxs-lookup"><span data-stu-id="89347-260">Check a group’s membership</span></span>

<span data-ttu-id="89347-261">toocheck egy csoport tagságát, hajtsa végre hello alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="89347-261">toocheck a group’s membership, follow hello steps below:</span></span>

1.  <span data-ttu-id="89347-262">Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="89347-262">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="89347-263">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="89347-263">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="89347-264">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="89347-264">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="89347-265">Kattintson a **felhasználók és csoportok** hello navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="89347-265">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="89347-266">Kattintson a **összes csoport**.</span><span class="sxs-lookup"><span data-stu-id="89347-266">click **All groups**.</span></span>

6.  <span data-ttu-id="89347-267">**Keresési** hello csoport érdekli, és **hello sorban kattintson** tooselect.</span><span class="sxs-lookup"><span data-stu-id="89347-267">**Search** for hello group you are interested in and **click hello row** tooselect.</span></span>

7.  <span data-ttu-id="89347-268">Kattintson a **tagok** tooreview hello azoknak a felhasználóknak hozzárendelt toothis csoport.</span><span class="sxs-lookup"><span data-stu-id="89347-268">click **Members** tooreview hello list of users assigned toothis group.</span></span>

### <a name="check-a-dynamic-groups-membership-criteria"></a><span data-ttu-id="89347-269">A dinamikus csoport tagsági feltételek ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="89347-269">Check a dynamic group’s membership criteria</span></span> 

<span data-ttu-id="89347-270">toocheck egy dinamikus csoport tagsági feltételek, kövesse az alábbi hello lépéseket:</span><span class="sxs-lookup"><span data-stu-id="89347-270">toocheck a dynamic group’s membership criteria, follow hello steps below:</span></span>

1.  <span data-ttu-id="89347-271">Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="89347-271">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="89347-272">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="89347-272">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="89347-273">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="89347-273">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="89347-274">Kattintson a **felhasználók és csoportok** hello navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="89347-274">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="89347-275">Kattintson a **összes csoport**.</span><span class="sxs-lookup"><span data-stu-id="89347-275">click **All groups**.</span></span>

6.  <span data-ttu-id="89347-276">**Keresési** hello csoport érdekli, és **hello sorban kattintson** tooselect.</span><span class="sxs-lookup"><span data-stu-id="89347-276">**Search** for hello group you are interested in and **click hello row** tooselect.</span></span>

7.  <span data-ttu-id="89347-277">Kattintson a **dinamikus tagsági szabályok szempontjából.**</span><span class="sxs-lookup"><span data-stu-id="89347-277">click **Dynamic membership rules.**</span></span>

8.  <span data-ttu-id="89347-278">Felülvizsgálati hello **egyszerű** vagy **speciális** szabály definiálva, és azt szeretné, hogy a csoport tagjai toobe hello felhasználó megfelel-e ezek a feltételek.</span><span class="sxs-lookup"><span data-stu-id="89347-278">Review hello **simple** or **advanced** rule defined for this group and ensure that hello user you want toobe a member of this group meets these criteria.</span></span>

### <a name="check-a-groups-assigned-licenses"></a><span data-ttu-id="89347-279">Ellenőrizze a csoporthoz hozzárendelt licencekkel</span><span class="sxs-lookup"><span data-stu-id="89347-279">Check a group’s assigned licenses</span></span>

<span data-ttu-id="89347-280">toocheck egy csoporthoz hozzárendelt licencek, hajtsa végre hello alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="89347-280">toocheck a group’s assigned licenses, follow hello steps below:</span></span>

1.  <span data-ttu-id="89347-281">Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="89347-281">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="89347-282">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="89347-282">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="89347-283">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="89347-283">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="89347-284">Kattintson a **felhasználók és csoportok** hello navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="89347-284">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="89347-285">Kattintson a **összes csoport**.</span><span class="sxs-lookup"><span data-stu-id="89347-285">click **All groups**.</span></span>

6.  <span data-ttu-id="89347-286">**Keresési** hello csoport érdekli, és **hello sorban kattintson** tooselect.</span><span class="sxs-lookup"><span data-stu-id="89347-286">**Search** for hello group you are interested in and **click hello row** tooselect.</span></span>

7.  <span data-ttu-id="89347-287">Kattintson a **licencek** toosee mely licencek hello csoport jelenleg hozzá van rendelve.</span><span class="sxs-lookup"><span data-stu-id="89347-287">click **Licenses** toosee which licenses hello group currently has assigned.</span></span>

### <a name="reprocess-a-groups-licenses"></a><span data-ttu-id="89347-288">Újból feldolgozza a csoport licencek</span><span class="sxs-lookup"><span data-stu-id="89347-288">Reprocess a group’s licenses</span></span>

<span data-ttu-id="89347-289">tooreprocess egy csoporthoz hozzárendelt licencek, hajtsa végre hello alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="89347-289">tooreprocess a group’s assigned licenses, follow hello steps below:</span></span>

1.  <span data-ttu-id="89347-290">Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="89347-290">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="89347-291">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="89347-291">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="89347-292">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="89347-292">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="89347-293">Kattintson a **felhasználók és csoportok** hello navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="89347-293">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="89347-294">Kattintson a **összes csoport**.</span><span class="sxs-lookup"><span data-stu-id="89347-294">click **All groups**.</span></span>

6.  <span data-ttu-id="89347-295">**Keresési** hello csoport érdekli, és **hello sorban kattintson** tooselect.</span><span class="sxs-lookup"><span data-stu-id="89347-295">**Search** for hello group you are interested in and **click hello row** tooselect.</span></span>

7.  <span data-ttu-id="89347-296">Kattintson a **licencek** toosee mely licencek hello csoport jelenleg hozzá van rendelve.</span><span class="sxs-lookup"><span data-stu-id="89347-296">click **Licenses** toosee which licenses hello group currently has assigned.</span></span>

8.  <span data-ttu-id="89347-297">Kattintson a hello **újból feldolgozza** gomb tooensure, amely a hozzárendelt licencek toothis csoportnak a tagjai hello naprakészek legyenek.</span><span class="sxs-lookup"><span data-stu-id="89347-297">click hello **Reprocess** button tooensure that hello licenses assigned toothis group’s members are up-to-date.</span></span> <span data-ttu-id="89347-298">Ez eltarthat egy ideig, attól függően, hogy hello méretét és összetettségét hello csoport.</span><span class="sxs-lookup"><span data-stu-id="89347-298">This may take a long time, depending on hello size and complexity of hello group.</span></span>

   >[!NOTE]
   ><span data-ttu-id="89347-299">toodo Ez gyorsabb, érdemes lehet ideiglenesen hozzárendelés közvetlenül a licenc toohello felhasználó.</span><span class="sxs-lookup"><span data-stu-id="89347-299">toodo this faster, consider temporarily assigning a license toohello user directly.</span></span> <span data-ttu-id="89347-300">[A felhasználó rendel egy licencet](#problems-with-application-consent).</span><span class="sxs-lookup"><span data-stu-id="89347-300">[Assign a user a license](#problems-with-application-consent).</span></span>
   >
   >

### <a name="assign-a-group-a-license"></a><span data-ttu-id="89347-301">Egy csoport a licenc hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="89347-301">Assign a group a license</span></span>

<span data-ttu-id="89347-302">tooassign tooa licenccsoport, kövesse a hello alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="89347-302">tooassign a license tooa group, follow hello steps below:</span></span>

1.  <span data-ttu-id="89347-303">Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="89347-303">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="89347-304">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="89347-304">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="89347-305">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="89347-305">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="89347-306">Kattintson a **felhasználók és csoportok** hello navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="89347-306">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="89347-307">Kattintson a **összes csoport**.</span><span class="sxs-lookup"><span data-stu-id="89347-307">click **All groups**.</span></span>

6.  <span data-ttu-id="89347-308">**Keresési** hello csoport érdekli, és **hello sorban kattintson** tooselect.</span><span class="sxs-lookup"><span data-stu-id="89347-308">**Search** for hello group you are interested in and **click hello row** tooselect.</span></span>

7.  <span data-ttu-id="89347-309">Kattintson a **licencek** toosee mely licencek hello csoport jelenleg hozzá van rendelve.</span><span class="sxs-lookup"><span data-stu-id="89347-309">click **Licenses** toosee which licenses hello group currently has assigned.</span></span>

8.  <span data-ttu-id="89347-310">Kattintson a hello **hozzárendelése** gombra.</span><span class="sxs-lookup"><span data-stu-id="89347-310">click hello **Assign** button.</span></span>

9.  <span data-ttu-id="89347-311">Válassza ki **egy vagy több termék** választható termékek hello listája.</span><span class="sxs-lookup"><span data-stu-id="89347-311">Select **one or more products** from hello list of available products.</span></span>

10. <span data-ttu-id="89347-312">**Nem kötelező** hello kattintson **hozzárendelés beállításai** elem toogranularly rendelje hozzá a termékek.</span><span class="sxs-lookup"><span data-stu-id="89347-312">**Optional** click hello **assignment options** item toogranularly assign products.</span></span> <span data-ttu-id="89347-313">Kattintson a **Ok** amikor ez befejeződik.</span><span class="sxs-lookup"><span data-stu-id="89347-313">Click **Ok** when this is completed.</span></span>

11. <span data-ttu-id="89347-314">Kattintson a hello **hozzárendelése** tooassign licencek toothis csoportbiztonsági gombra.</span><span class="sxs-lookup"><span data-stu-id="89347-314">Click hello **Assign** button tooassign these licenses toothis group.</span></span> <span data-ttu-id="89347-315">Ez eltarthat egy ideig, attól függően, hogy hello méretét és összetettségét hello csoport.</span><span class="sxs-lookup"><span data-stu-id="89347-315">This may take a long time, depending on hello size and complexity of hello group.</span></span>

   >[!NOTE]
   ><span data-ttu-id="89347-316">toodo Ez gyorsabb, érdemes lehet ideiglenesen hozzárendelés közvetlenül a licenc toohello felhasználó.</span><span class="sxs-lookup"><span data-stu-id="89347-316">toodo this faster, consider temporarily assigning a license toohello user directly.</span></span> <span data-ttu-id="89347-317">[A felhasználó rendel egy licencet](#problems-with-application-consent).</span><span class="sxs-lookup"><span data-stu-id="89347-317">[Assign a user a license](#problems-with-application-consent).</span></span>
   > 
   >

## <a name="problems-with-conditional-access-policies"></a><span data-ttu-id="89347-318">Feltételes hozzáférési szabályzatokkal kapcsolatos problémák</span><span class="sxs-lookup"><span data-stu-id="89347-318">Problems with conditional access policies</span></span>

### <a name="check-a-specific-conditional-access-policy"></a><span data-ttu-id="89347-319">Egy adott feltételes hozzáférési házirend ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="89347-319">Check a specific conditional access policy</span></span>

<span data-ttu-id="89347-320">toocheck, illetve ellenőrizhető egy feltételes hozzáférési szabályzatot:</span><span class="sxs-lookup"><span data-stu-id="89347-320">toocheck or validate a single conditional access policy:</span></span>

1.  <span data-ttu-id="89347-321">Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="89347-321">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="89347-322">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="89347-322">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="89347-323">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="89347-323">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="89347-324">Kattintson a **vállalati alkalmazások** hello navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="89347-324">click **Enterprise applications** in hello navigation menu.</span></span>

5.  <span data-ttu-id="89347-325">Kattintson a hello **feltételes hozzáférés** navigációs elem.</span><span class="sxs-lookup"><span data-stu-id="89347-325">click hello **Conditional access** navigation item.</span></span>

6.  <span data-ttu-id="89347-326">Kattintson a hello házirend érdekli ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="89347-326">click hello policy you are interested in inspecting.</span></span>

7.  <span data-ttu-id="89347-327">Ellenőrizze, hogy nincsenek-e nincs meghatározott feltételek, hozzárendelések vagy egyéb beállításokat, amelyek letilthatja a felhasználói hozzáférés.</span><span class="sxs-lookup"><span data-stu-id="89347-327">Review that there are no specific conditions, assignments, or other settings which may be blocking user access.</span></span>

   >[!NOTE]
   ><span data-ttu-id="89347-328">Kezdésként érdemes lehet tootemporarily tiltsa le a házirend tooensure, ez nem érinti bejelentkezési biztosítási toodo, a set hello **házirend engedélyezése** túl váltása**nem** hello kattintson **mentése** gomb .</span><span class="sxs-lookup"><span data-stu-id="89347-328">You may wish tootemporarily disable this policy tooensure it is not affecting sign ins. toodo this, set hello **Enable policy** toggle too**No** and click hello **Save** button.</span></span>
   >
   >

### <a name="check-a-specific-applications-conditional-access-policy"></a><span data-ttu-id="89347-329">Egy adott alkalmazás feltételes hozzáférési házirend ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="89347-329">Check a specific application’s conditional access policy</span></span>

<span data-ttu-id="89347-330">toocheck vagy ellenőrizni az egyetlen alkalmazást jelenleg beállított feltételes hozzáférési szabályzatot:</span><span class="sxs-lookup"><span data-stu-id="89347-330">toocheck or validate a single application’s currently configured conditional access policy:</span></span>

1.  <span data-ttu-id="89347-331">Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="89347-331">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="89347-332">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="89347-332">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="89347-333">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="89347-333">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="89347-334">Kattintson a **vállalati alkalmazások** hello navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="89347-334">click **Enterprise applications** in hello navigation menu.</span></span>

5.  <span data-ttu-id="89347-335">Kattintson a **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="89347-335">click **All applications**.</span></span>

6.  <span data-ttu-id="89347-336">Keresés a hello alkalmazás kapcsolatban, vagy felhasználói hello kísérel meg toosign tooby alkalmazásban megjelenítése vagy egy alkalmazás azonosítója.</span><span class="sxs-lookup"><span data-stu-id="89347-336">Search for hello application you are interested in, or hello user is attempting toosign in tooby application display name or application id.</span></span>

     >[!NOTE]
     ><span data-ttu-id="89347-337">Ha nem látja a keresett hello alkalmazás, kattintson a hello **szűrő** gombra, majd bontsa ki a hello hatókör hello lista túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="89347-337">If you don’t see hello application you are looking for, click hello **Filter** button and expand hello scope of hello list too**All applications**.</span></span> <span data-ttu-id="89347-338">Toosee több oszlopot, kattintson a hello **oszlopok** tooadd további részletek az alkalmazások gombra.</span><span class="sxs-lookup"><span data-stu-id="89347-338">If you want toosee more columns, click hello **Columns** button tooadd additional details for your applications.</span></span>
     >
     >

7.  <span data-ttu-id="89347-339">Kattintson a hello **feltételes hozzáférés** navigációs elem.</span><span class="sxs-lookup"><span data-stu-id="89347-339">click hello **Conditional access** navigation item.</span></span>

8.  <span data-ttu-id="89347-340">Kattintson a hello házirend érdekli ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="89347-340">click hello policy you are interested in inspecting.</span></span>

9.  <span data-ttu-id="89347-341">Ellenőrizze, hogy nincsenek-e nincs meghatározott feltételek, hozzárendelések vagy egyéb beállításokat, amelyek letilthatja a felhasználói hozzáférés.</span><span class="sxs-lookup"><span data-stu-id="89347-341">Review that there are no specific conditions, assignments, or other settings which may be blocking user access.</span></span>

     >[!NOTE]
     ><span data-ttu-id="89347-342">Kezdésként érdemes lehet tootemporarily tiltsa le a házirend tooensure, ez nem érinti bejelentkezési biztosítási toodo, a set hello **házirend engedélyezése** túl váltása**nem** hello kattintson **mentése** gomb .</span><span class="sxs-lookup"><span data-stu-id="89347-342">You may wish tootemporarily disable this policy tooensure it is not affecting sign ins. toodo this, set hello **Enable policy** toggle too**No** and click hello **Save** button.</span></span>
     >
     >

### <a name="disable-a-specific-conditional-access-policy"></a><span data-ttu-id="89347-343">Egy adott feltételes hozzáférési házirend letiltása</span><span class="sxs-lookup"><span data-stu-id="89347-343">Disable a specific conditional access policy</span></span>

<span data-ttu-id="89347-344">toocheck, illetve ellenőrizhető egy feltételes hozzáférési szabályzatot:</span><span class="sxs-lookup"><span data-stu-id="89347-344">toocheck or validate a single conditional access policy:</span></span>

1.  <span data-ttu-id="89347-345">Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="89347-345">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="89347-346">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="89347-346">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="89347-347">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="89347-347">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="89347-348">Kattintson a **vállalati alkalmazások** hello navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="89347-348">click **Enterprise applications** in hello navigation menu.</span></span>

5.  <span data-ttu-id="89347-349">Kattintson a hello **feltételes hozzáférés** navigációs elem.</span><span class="sxs-lookup"><span data-stu-id="89347-349">click hello **Conditional access** navigation item.</span></span>

6.  <span data-ttu-id="89347-350">Kattintson a hello házirend érdekli ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="89347-350">click hello policy you are interested in inspecting.</span></span>

7.  <span data-ttu-id="89347-351">Tiltsa le az hello házirend hello beállítása az **házirend engedélyezése** túl váltása**nem** hello kattintson **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="89347-351">Disable hello policy by setting hello **Enable policy** toggle too**No** and click hello **Save** button.</span></span>

## <a name="problems-with-application-consent"></a><span data-ttu-id="89347-352">Az alkalmazás hozzájárulásával problémák</span><span class="sxs-lookup"><span data-stu-id="89347-352">Problems with application consent</span></span>

<span data-ttu-id="89347-353">Alkalmazás-hozzáférés is lehet tiltva, mert a megfelelő engedélyekkel a hozzájárulási művelet hello nem történt.</span><span class="sxs-lookup"><span data-stu-id="89347-353">Application access can be blocked because hello proper permissions consent operation has not occurred.</span></span> <span data-ttu-id="89347-354">Az alábbiakban néhány alkalmazás hozzájárulási problémák megoldásához és hibaelhárítást végezhessen módját:</span><span class="sxs-lookup"><span data-stu-id="89347-354">Below are some ways you can troubleshoot and solve application consent issues:</span></span>

-   [<span data-ttu-id="89347-355">A felhasználói szintű hozzájárulási művelet végrehajtása</span><span class="sxs-lookup"><span data-stu-id="89347-355">Perform a user-level consent operation</span></span>](#perform-a-user-level-consent-operation)

-   [<span data-ttu-id="89347-356">Bármely alkalmazás rendszergazdai hozzájárulási művelethez</span><span class="sxs-lookup"><span data-stu-id="89347-356">Perform administrator-level consent operation for any application</span></span>](#perform-administrator-level-consent-operation-for-any-application)

-   [<span data-ttu-id="89347-357">Hajtsa végre a rendszergazdai hozzájárulási egyetlen bérlői alkalmazások</span><span class="sxs-lookup"><span data-stu-id="89347-357">Perform administrator-level consent for a single-tenant application</span></span>](#perform-administrator-level-consent-for-a-single-tenant-application)

-   [<span data-ttu-id="89347-358">Hajtsa végre a rendszergazdai hozzájárulási egy több-bérlős alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="89347-358">Perform administrator-level consent for a multi-tenant application</span></span>](#perform-administrator-level-consent-for-a-multi-tenant-application)

### <a name="perform-a-user-level-consent-operation"></a><span data-ttu-id="89347-359">A felhasználói szintű hozzájárulási művelet végrehajtása</span><span class="sxs-lookup"><span data-stu-id="89347-359">Perform a user-level consent operation</span></span>

-   <span data-ttu-id="89347-360">Bármely Open ID Connect-kompatibilis alkalmazás, amelyet engedélyeket igényel toohello alkalmazás bejelentkezési képernyő, lépjen a felhasználói szintű hozzájárulási toohello alkalmazás hello bejelentkezett felhasználó hajt végre.</span><span class="sxs-lookup"><span data-stu-id="89347-360">For any Open ID Connect-enabled application that requests permissions, navigating toohello application’s sign in screen performs a user level consent toohello application for hello signed-in user.</span></span>

-   <span data-ttu-id="89347-361">Ha a toodo Ez programozott módon, lásd: [egyes felhasználói hozzájárulás kérése](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#requesting-individual-user-consent).</span><span class="sxs-lookup"><span data-stu-id="89347-361">If you wish toodo this programmatically, see [Requesting individual user consent](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#requesting-individual-user-consent).</span></span>

### <a name="perform-administrator-level-consent-operation-for-any-application"></a><span data-ttu-id="89347-362">Bármely alkalmazás rendszergazdai hozzájárulási művelethez</span><span class="sxs-lookup"><span data-stu-id="89347-362">Perform administrator-level consent operation for any application</span></span>

-   <span data-ttu-id="89347-363">A **csak a hello V1 alkalmazásmodell használatával fejlesztett alkalmazások**, adja hozzá a rendszergazda szintű hozzájárulási toooccur kényszerítheti "**? rendszergazdai parancssorból =\_hozzájárulás**" toohello végét egy az alkalmazás bejelentkezési URL-címben.</span><span class="sxs-lookup"><span data-stu-id="89347-363">For **only applications developed using hello V1 application model**, you can force this administrator level consent toooccur by adding “**?prompt=admin\_consent**” toohello end of an application’s sign in URL.</span></span>

-   <span data-ttu-id="89347-364">A **bármely hello V2 alkalmazásmodell segítségével létrehozott alkalmazást**, a rendszergazdai hozzájárulási toooccur kényszerítheti a hello hello utasításokat követve **hello engedélyeket kérhet egy könyvtárat felügyeleti** szakasza [hello rendszergazda jóváhagyását végpont használatával](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#using-the-admin-consent-endpoint).</span><span class="sxs-lookup"><span data-stu-id="89347-364">For **any application developed using hello V2 application model**, you can enforce this administrator-level consent toooccur by following hello instructions under hello **Request hello permissions from a directory admin** section of [Using hello admin consent endpoint](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#using-the-admin-consent-endpoint).</span></span>

### <a name="perform-administrator-level-consent-for-a-single-tenant-application"></a><span data-ttu-id="89347-365">Hajtsa végre a rendszergazdai hozzájárulási egyetlen bérlői alkalmazások</span><span class="sxs-lookup"><span data-stu-id="89347-365">Perform administrator-level consent for a single-tenant application</span></span>

-   <span data-ttu-id="89347-366">A **egyetlen bérlői alkalmazások** (mint fejleszt, vagy a szervezet saját), engedélyek kéréséhez, amely hajthat végre egy **rendszergazdai szintű hozzájárulási** nevében összes művelet Jelentkezzen be globális rendszergazdaként, majd kattintson a hello felhasználók **engedélyeket** hello hello tetején gomb **alkalmazás beállításjegyzék -&gt; összes alkalmazás -&gt; válasszon ki egy alkalmazást – &gt; Szükséges engedélyek** panelen.</span><span class="sxs-lookup"><span data-stu-id="89347-366">For **single-tenant applications** that request permissions (like those you are developing or own in your organization), you can perform an **administrative-level consent** operation on behalf of all users by signing in as a Global Administrator and clicking on hello **Grant permissions** button at hello top of hello **Application Registry -&gt; All Applications -&gt; Select an App -&gt; Required Permissions** blade.</span></span>

-   <span data-ttu-id="89347-367">A **bármely hello V1 vagy V2 alkalmazásmodell segítségével létrehozott alkalmazást**, a rendszergazdai hozzájárulási toooccur kényszerítheti a hello hello utasításokat követve **a hello engedélyek kéréséhez egy Directory-rendszergazda** szakasza [hello rendszergazda jóváhagyását végpont használatával](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#using-the-admin-consent-endpoint).</span><span class="sxs-lookup"><span data-stu-id="89347-367">For **any application developed using hello V1 or V2 application model**, you can enforce this administrator-level consent toooccur by following hello instructions under hello **Request hello permissions from a directory admin** section of [Using hello admin consent endpoint](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#using-the-admin-consent-endpoint).</span></span>

### <a name="perform-administrator-level-consent-for-a-multi-tenant-application"></a><span data-ttu-id="89347-368">Hajtsa végre a rendszergazdai hozzájárulási egy több-bérlős alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="89347-368">Perform administrator-level consent for a multi-tenant application</span></span>

-   <span data-ttu-id="89347-369">A **több-bérlős alkalmazásokhoz** adott kérelem engedélyek (például egy alkalmazás egy harmadik féltől származó, vagy a Microsoft, házon belül fejlesztett alkalmazásokra) hajthat végre egy **rendszergazdai szintű hozzájárulási** műveletet.</span><span class="sxs-lookup"><span data-stu-id="89347-369">For **multi-tenant applications** that request permissions (like an application a third party, or Microsoft, develops), you can perform an **administrative-level consent** operation.</span></span> <span data-ttu-id="89347-370">Jelentkezzen be globális rendszergazdaként, és kattintson a hello **engedélyeket** alatt hello gomb **vállalati alkalmazások –&gt; összes alkalmazás -&gt; válasszon ki egy alkalmazást -&gt; Engedélyek** panel (rendelkezésre álló hamarosan).</span><span class="sxs-lookup"><span data-stu-id="89347-370">Sign in as a Global Administrator and clicking on hello **Grant permissions** button under hello **Enterprise Applications -&gt; All Applications -&gt; Select an App -&gt; Permissions** blade (available soon).</span></span>

-   <span data-ttu-id="89347-371">A hello hello utasításokat követve is kényszerítheti a rendszergazdai hozzájárulási toooccur **hello engedélyeket kérhet a directory-rendszergazda** szakasza [hello rendszergazda jóváhagyását végponthasználatával](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#using-the-admin-consent-endpoint).</span><span class="sxs-lookup"><span data-stu-id="89347-371">You can also enforce this administrator-level consent toooccur by following hello instructions under hello **Request hello permissions from a directory admin** section of [Using hello admin consent endpoint](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#using-the-admin-consent-endpoint).</span></span>

## <a name="next-steps"></a><span data-ttu-id="89347-372">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="89347-372">Next steps</span></span>
[<span data-ttu-id="89347-373">Hello rendszergazda jóváhagyását végpont használatával</span><span class="sxs-lookup"><span data-stu-id="89347-373">Using hello admin consent endpoint</span></span>](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#using-the-admin-consent-endpoint)

