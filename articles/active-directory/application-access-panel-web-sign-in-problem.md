---
title: "bejelentkezés toohello hozzáférési panel webhely aaaProblem |} Microsoft Docs"
description: "Útmutatás tootroubleshoot problémák merülhetnek fel a toouse toosign közben hello hozzáférési Panel"
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
ms.reviwer: japere
ms.openlocfilehash: 1037f7c5fbaa9425760ad5739b383c716d5fc3a3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="problem-signing-in-toohello-access-panel-website"></a><span data-ttu-id="6d7f8-103">Bejelentkezés toohello hozzáférési panel webhelyen</span><span class="sxs-lookup"><span data-stu-id="6d7f8-103">Problem signing in toohello access panel website</span></span>

<span data-ttu-id="6d7f8-104">Hozzáférési Panel hello egy webes portál, mely lehetővé teszi, hogy egy felhasználó, aki rendelkezik a munkahelyi vagy iskolai fiókot az Azure Active Directory (Azure AD) tooview, és indítsa el felhőalapú alkalmazások, hogy az Azure AD hello rendszergazda engedélyezte őket a hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="6d7f8-104">hello Access Panel is a web-based portal which enables a user who has a work or school account in Azure Active Directory (Azure AD) tooview and launch cloud-based applications that hello Azure AD administrator has granted them access to.</span></span> <span data-ttu-id="6d7f8-105">Egy felhasználó, aki rendelkezik az Azure AD-verziók is használhatja, önkiszolgáló csoportkezelési és a hozzáférési Panel hello keresztül kezelési képességeit.</span><span class="sxs-lookup"><span data-stu-id="6d7f8-105">A user who has Azure AD editions can also use self-service group and app management capabilities through hello Access Panel.</span></span> <span data-ttu-id="6d7f8-106">Hozzáférési Panel hello elkülönül hello Azure-portálon, és nem igényli a felhasználók toohave Azure-előfizetéssel.</span><span class="sxs-lookup"><span data-stu-id="6d7f8-106">hello Access Panel is separate from hello Azure portal and does not require users toohave an Azure subscription.</span></span>

<span data-ttu-id="6d7f8-107">Ha munkahelyi vagy iskolai fiókkal az Azure AD hozzáférési Panel toohello felhasználók bejelentkezhetnek.</span><span class="sxs-lookup"><span data-stu-id="6d7f8-107">Users can sign in toohello Access Panel if they have a work or school account in Azure AD.</span></span>

-   <span data-ttu-id="6d7f8-108">Felhasználók hitelesítése közvetlenül az Azure ad.</span><span class="sxs-lookup"><span data-stu-id="6d7f8-108">Users can be authenticated by Azure AD directly.</span></span>

-   <span data-ttu-id="6d7f8-109">Felhasználók hitelesítése az Active Directory összevonási szolgáltatások (AD FS) használatával.</span><span class="sxs-lookup"><span data-stu-id="6d7f8-109">Users can be authenticated by using Active Directory Federation Services (AD FS).</span></span>

-   <span data-ttu-id="6d7f8-110">Windows Server Active Directory felhasználók hitelesíthetők.</span><span class="sxs-lookup"><span data-stu-id="6d7f8-110">Users can be authenticated by Windows Server Active Directory.</span></span>

<span data-ttu-id="6d7f8-111">Ha a felhasználó rendelkezik előfizetéssel az Azure vagy Office 365, és hello Azure-portálon vagy az Office 365 alkalmazást használ, képes toouse lesz a toosign újra anélkül zökkenőmentesen hello a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="6d7f8-111">If a user has a subscription for Azure or Office 365 and has been using hello Azure portal or an Office 365 application, they'll be able toouse hello Access Panel seamlessly without needing toosign in again.</span></span> <span data-ttu-id="6d7f8-112">Nem hitelesített felhasználók a kért toosign kell használatával hello felhasználónevet és jelszót a fiókjuk Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="6d7f8-112">Users who are not authenticated be prompted toosign in by using hello username and password for their account in Azure AD.</span></span> <span data-ttu-id="6d7f8-113">Ha hello szervezet konfigurálva van összevonási, írja be a hello felhasználónév is használhatók.</span><span class="sxs-lookup"><span data-stu-id="6d7f8-113">If hello organization has configured federation, typing hello username is sufficient.</span></span>

## <a name="general-issues-toocheck-first"></a><span data-ttu-id="6d7f8-114">Általános problémák első toocheck</span><span class="sxs-lookup"><span data-stu-id="6d7f8-114">General issues toocheck first</span></span> 

-   <span data-ttu-id="6d7f8-115">Ellenőrizze, hogy hello felhasználói bejelentkezéskor van toohello **javítsa ki az URL-cím**: <https://myapps.microsoft.com></span><span class="sxs-lookup"><span data-stu-id="6d7f8-115">Make sure hello user is signing in toohello **correct URL**: <https://myapps.microsoft.com></span></span>

-   <span data-ttu-id="6d7f8-116">Ellenőrizze, hogy hello felhasználó böngészője által hozzáadott hello URL-cím tooits **megbízható helyek**</span><span class="sxs-lookup"><span data-stu-id="6d7f8-116">Make sure hello user’s browser has added hello URL tooits **trusted sites**</span></span>

-   <span data-ttu-id="6d7f8-117">Ellenőrizze, hogy a hello felhasználói fiók **engedélyezett** a indított bejelentkezések.</span><span class="sxs-lookup"><span data-stu-id="6d7f8-117">Make sure hello user’s account is **enabled** for sign ins.</span></span>

-   <span data-ttu-id="6d7f8-118">Ellenőrizze, hogy a hello felhasználói fiók **nincs zárolva.**</span><span class="sxs-lookup"><span data-stu-id="6d7f8-118">Make sure hello user’s account is **not locked out.**</span></span>

-   <span data-ttu-id="6d7f8-119">Győződjön meg arról, hogy hello felhasználói **nem lejárt vagy elfelejtett jelszó.**</span><span class="sxs-lookup"><span data-stu-id="6d7f8-119">Make sure hello user’s **password is not expired or forgotten.**</span></span>

-   <span data-ttu-id="6d7f8-120">Győződjön meg arról, hogy **multi-factor Authentication** nem blokkolja a hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="6d7f8-120">Make sure **Multi-Factor Authentication** is not blocking user access.</span></span>

-   <span data-ttu-id="6d7f8-121">Győződjön meg arról, hogy egy **feltételes hozzáférési házirend** vagy **Identity Protection** házirend nem blokkolja a hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="6d7f8-121">Make sure a **Conditional Access policy** or **Identity Protection** policy is not blocking user access.</span></span>

-   <span data-ttu-id="6d7f8-122">Győződjön meg arról, hogy a felhasználó **hitelesítési kapcsolattartási adatai** működik-e toodate tooallow többtényezős hitelesítést vagy feltételes hozzáférési házirendek toobe lépnek érvénybe.</span><span class="sxs-lookup"><span data-stu-id="6d7f8-122">Make sure that a user’s **authentication contact info** is up toodate tooallow Multi-Factor Authentication or Conditional Access policies toobe enforced.</span></span>

-   <span data-ttu-id="6d7f8-123">Győződjön meg arról, hogy tooalso próbálkozzon a böngésző cookie-k törölje, majd próbálkozzon újra a toosign.</span><span class="sxs-lookup"><span data-stu-id="6d7f8-123">Make sure tooalso try clearing your browser’s cookies and trying toosign in again.</span></span>

## <a name="meeting-browser-requirements-for-hello-access-panel"></a><span data-ttu-id="6d7f8-124">A hozzáférési Panel hello értekezlet böngészőre vonatkozó követelményei</span><span class="sxs-lookup"><span data-stu-id="6d7f8-124">Meeting browser requirements for hello Access Panel</span></span>

<span data-ttu-id="6d7f8-125">hello hozzáférési Panel igényel, amely támogatja a JavaScript egy böngészőt, és CSS engedélyezte.</span><span class="sxs-lookup"><span data-stu-id="6d7f8-125">hello Access Panel requires a browser that supports JavaScript and has CSS enabled.</span></span> <span data-ttu-id="6d7f8-126">toouse jelszó-alapú egyszeri bejelentkezést (SSO) a hozzáférési Panel, a hozzáférési Panel bővítmény hello hello telepíteni kell hello felhasználó böngészőjében.</span><span class="sxs-lookup"><span data-stu-id="6d7f8-126">toouse password-based single sign-on (SSO) in hello Access Panel, hello Access Panel extension must be installed in hello user’s browser.</span></span> <span data-ttu-id="6d7f8-127">A bővítmény le automatikusan, ha a felhasználó megadja egy alkalmazás, amely jelszóalapú SSO van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="6d7f8-127">This extension is downloaded automatically when a user selects an application that is configured for password-based SSO.</span></span>

<span data-ttu-id="6d7f8-128">Az egyszeri bejelentkezés jelszóalapú hello végfelhasználó böngészőkkel lehet:</span><span class="sxs-lookup"><span data-stu-id="6d7f8-128">For password-based SSO, hello end user’s browsers can be:</span></span>

-   <span data-ttu-id="6d7f8-129">Internet Explorer 8, 9, 10, 11 – a Windows 7 vagy újabb</span><span class="sxs-lookup"><span data-stu-id="6d7f8-129">Internet Explorer 8, 9, 10, 11 -- on Windows 7 or later</span></span>

-   <span data-ttu-id="6d7f8-130">Peremhálózati Windows 10 évforduló Edition vagy újabb</span><span class="sxs-lookup"><span data-stu-id="6d7f8-130">Edge on Windows 10 Anniversary Edition or later</span></span> 

-   <span data-ttu-id="6d7f8-131">Chrome – A Windows 7 vagy újabb, és MacOS X rendszeren vagy újabb</span><span class="sxs-lookup"><span data-stu-id="6d7f8-131">Chrome -- on Windows 7 or later, and on MacOS X or later</span></span>

-   <span data-ttu-id="6d7f8-132">Firefox 26.0 vagy újabb – a Windows XP SP2 vagy újabb, és a Mac OS X 10,6 vagy újabb verzió</span><span class="sxs-lookup"><span data-stu-id="6d7f8-132">Firefox 26.0 or later -- on Windows XP SP2 or later, and on Mac OS X 10.6 or later</span></span>


## <a name="problems-with-hello-users-account"></a><span data-ttu-id="6d7f8-133">Hello felhasználói fiókkal kapcsolatos problémák</span><span class="sxs-lookup"><span data-stu-id="6d7f8-133">Problems with hello user’s account</span></span>

<span data-ttu-id="6d7f8-134">Hozzáférés toohello hozzáférési Panel blokkolható hello felhasználói fiókkal tooa probléma miatt.</span><span class="sxs-lookup"><span data-stu-id="6d7f8-134">Access toohello Access Panel can be blocked due tooa problem with hello user’s account.</span></span> <span data-ttu-id="6d7f8-135">Az alábbiakban néhány módszert hibákat, és a felhasználó és a fiók beállításainak problémáinak megoldásával:</span><span class="sxs-lookup"><span data-stu-id="6d7f8-135">Below are some ways you can troubleshoot and solve problems with users and their account settings:</span></span>

-   [<span data-ttu-id="6d7f8-136">Ellenőrizze, hogy egy felhasználói fiók létezik-e az Azure Active Directoryban</span><span class="sxs-lookup"><span data-stu-id="6d7f8-136">Check if a user account exists in Azure Active Directory</span></span>](#check-if-a-user-account-exists-in-azure-active-directory)

-   [<span data-ttu-id="6d7f8-137">A felhasználói fiók állapotának ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="6d7f8-137">Check a user’s account status</span></span>](#check-a-users-account-status)

-   [<span data-ttu-id="6d7f8-138">A jelszó alaphelyzetbe állítása</span><span class="sxs-lookup"><span data-stu-id="6d7f8-138">Reset a user’s password</span></span>](#reset-a-users-password)

-   [<span data-ttu-id="6d7f8-139">Önkiszolgáló jelszóátállítás engedélyezése</span><span class="sxs-lookup"><span data-stu-id="6d7f8-139">Enable self-service password reset</span></span>](#enable-self-service-password-reset)

-   [<span data-ttu-id="6d7f8-140">A felhasználó a multi-factor authentication állapotának ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="6d7f8-140">Check a user’s multi-factor authentication status</span></span>](#check-a-users-multi-factor-authentication-status)

-   [<span data-ttu-id="6d7f8-141">Ellenőrizze a felhasználó hitelesítési kapcsolattartási adatai</span><span class="sxs-lookup"><span data-stu-id="6d7f8-141">Check a user’s authentication contact info</span></span>](#check-a-users-authentication-contact-info)

-   [<span data-ttu-id="6d7f8-142">A felhasználói csoporttagság ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="6d7f8-142">Check a user’s group memberships</span></span>](#check-a-users-group-memberships)

-   [<span data-ttu-id="6d7f8-143">A felhasználó licenc-hozzárendeléseket ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="6d7f8-143">Check a user’s assigned licenses</span></span>](#check-a-users-assigned-licenses)

-   [<span data-ttu-id="6d7f8-144">A felhasználó a licenc hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="6d7f8-144">Assign a user a license</span></span>](#assign-a-user-a-license)

### <a name="check-if-a-user-account-exists-in-azure-active-directory"></a><span data-ttu-id="6d7f8-145">Ellenőrizze, hogy egy felhasználói fiók létezik-e az Azure Active Directoryban</span><span class="sxs-lookup"><span data-stu-id="6d7f8-145">Check if a user account exists in Azure Active Directory</span></span>

<span data-ttu-id="6d7f8-146">toocheck, ha egy felhasználói fiók található, kövesse a hello alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="6d7f8-146">toocheck if a user’s account is present, follow hello steps below:</span></span>

1.  <span data-ttu-id="6d7f8-147">Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="6d7f8-147">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="6d7f8-148">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="6d7f8-148">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="6d7f8-149">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="6d7f8-149">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="6d7f8-150">Kattintson a **felhasználók és csoportok** hello navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="6d7f8-150">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="6d7f8-151">Kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="6d7f8-151">click **All users**.</span></span>

6.  <span data-ttu-id="6d7f8-152">**Keresési** hello felhasználó érdekli, és **hello sorban kattintson** tooselect.</span><span class="sxs-lookup"><span data-stu-id="6d7f8-152">**Search** for hello user you are interested in and **click hello row** tooselect.</span></span>

7.  <span data-ttu-id="6d7f8-153">Ellenőrizze a hello tulajdonságainak hello felhasználói objektum toobe meg arról, hogy azok meg, mint a várt, és nincs adat hiányzik.</span><span class="sxs-lookup"><span data-stu-id="6d7f8-153">Check hello properties of hello user object toobe sure that they look as you expect and no data is missing.</span></span>

### <a name="check-a-users-account-status"></a><span data-ttu-id="6d7f8-154">A felhasználói fiók állapotának ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="6d7f8-154">Check a user’s account status</span></span>

<span data-ttu-id="6d7f8-155">toocheck egy felhasználói fiók állapota, kövesse az alábbi hello lépéseket:</span><span class="sxs-lookup"><span data-stu-id="6d7f8-155">toocheck a user’s account status, follow hello steps below:</span></span>

1.  <span data-ttu-id="6d7f8-156">Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="6d7f8-156">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="6d7f8-157">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="6d7f8-157">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="6d7f8-158">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="6d7f8-158">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="6d7f8-159">Kattintson a **felhasználók és csoportok** hello navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="6d7f8-159">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="6d7f8-160">Kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="6d7f8-160">click **All users**.</span></span>

6.  <span data-ttu-id="6d7f8-161">**Keresési** hello felhasználó érdekli, és **hello sorban kattintson** tooselect.</span><span class="sxs-lookup"><span data-stu-id="6d7f8-161">**Search** for hello user you are interested in and **click hello row** tooselect.</span></span>

7.  <span data-ttu-id="6d7f8-162">Kattintson a **profil**.</span><span class="sxs-lookup"><span data-stu-id="6d7f8-162">click **Profile**.</span></span>

8.  <span data-ttu-id="6d7f8-163">A **beállítások** ügyeljen arra, hogy **blokk bejelentkezés** értéke túl**nem**.</span><span class="sxs-lookup"><span data-stu-id="6d7f8-163">Under **Settings** ensure that **Block sign in** is set too**No**.</span></span>

### <a name="reset-a-users-password"></a><span data-ttu-id="6d7f8-164">A jelszó alaphelyzetbe állítása</span><span class="sxs-lookup"><span data-stu-id="6d7f8-164">Reset a user’s password</span></span>

<span data-ttu-id="6d7f8-165">tooreset egy felhasználó jelszavát, hajtsa végre hello alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="6d7f8-165">tooreset a user’s password, follow hello steps below:</span></span>

1.  <span data-ttu-id="6d7f8-166">Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="6d7f8-166">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="6d7f8-167">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="6d7f8-167">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="6d7f8-168">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="6d7f8-168">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="6d7f8-169">Kattintson a **felhasználók és csoportok** hello navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="6d7f8-169">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="6d7f8-170">Kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="6d7f8-170">click **All users**.</span></span>

6.  <span data-ttu-id="6d7f8-171">**Keresési** hello felhasználó érdekli, és **hello sorban kattintson** tooselect.</span><span class="sxs-lookup"><span data-stu-id="6d7f8-171">**Search** for hello user you are interested in and **click hello row** tooselect.</span></span>

7.  <span data-ttu-id="6d7f8-172">Kattintson a hello **jelszó-átállítási** hello felhasználói panel felső hello gombra.</span><span class="sxs-lookup"><span data-stu-id="6d7f8-172">click hello **Reset password** button at hello top of hello user blade.</span></span>

8.  <span data-ttu-id="6d7f8-173">Kattintson a hello **jelszó-átállítási** hello gombjára **jelszó-átállítási** megjelenő panelen.</span><span class="sxs-lookup"><span data-stu-id="6d7f8-173">click hello **Reset password** button on hello **Reset password** blade that appears.</span></span>

9.  <span data-ttu-id="6d7f8-174">Másolás hello **ideiglenes jelszó** vagy **adjon meg egy új jelszót** hello felhasználó számára.</span><span class="sxs-lookup"><span data-stu-id="6d7f8-174">Copy hello **temporary password** or **enter a new password** for hello user.</span></span>

10. <span data-ttu-id="6d7f8-175">Az új jelszó toohello felhasználói kommunikációhoz, ezt a jelszót, a következő során jelentkezzen be az Active Directory tooAzure szükséges toochange legyenek.</span><span class="sxs-lookup"><span data-stu-id="6d7f8-175">Communicate this new password toohello user, they be required toochange this password during their next sign in tooAzure Active Directory.</span></span>

### <a name="enable-self-service-password-reset"></a><span data-ttu-id="6d7f8-176">Az önkiszolgáló jelszó-visszaállítás engedélyezése</span><span class="sxs-lookup"><span data-stu-id="6d7f8-176">Enable self-service password reset</span></span>

<span data-ttu-id="6d7f8-177">tooenable önkiszolgáló jelszó alaphelyzetbe állítása, kövesse az alábbi hello telepítési lépéseket:</span><span class="sxs-lookup"><span data-stu-id="6d7f8-177">tooenable self-service password reset, follow hello deployment steps below:</span></span>

-   [<span data-ttu-id="6d7f8-178">Engedélyezze a felhasználók tooreset a Azure Active Directory-jelszavaikat</span><span class="sxs-lookup"><span data-stu-id="6d7f8-178">Enable users tooreset their Azure Active Directory passwords</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started#enable-users-to-reset-their-azure-ad-passwords)

-   [<span data-ttu-id="6d7f8-179">Felhasználók tooreset engedélyezése vagy az Active Directory helyszíni jelszavak módosítása</span><span class="sxs-lookup"><span data-stu-id="6d7f8-179">Enable users tooreset or change their Active Directory on-premises passwords</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started#enable-users-to-reset-or-change-their-ad-passwords)

### <a name="check-a-users-multi-factor-authentication-status"></a><span data-ttu-id="6d7f8-180">A felhasználó a multi-factor authentication állapotának ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="6d7f8-180">Check a user’s multi-factor authentication status</span></span>

<span data-ttu-id="6d7f8-181">a felhasználó toocheck a multi-factor Authentication hitelesítési állapot, hajtsa végre hello alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="6d7f8-181">toocheck a user’s multi-factor authentication status, follow hello steps below:</span></span>

1.  <span data-ttu-id="6d7f8-182">Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="6d7f8-182">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="6d7f8-183">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="6d7f8-183">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="6d7f8-184">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="6d7f8-184">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="6d7f8-185">Kattintson a **felhasználók és csoportok** hello navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="6d7f8-185">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="6d7f8-186">Kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="6d7f8-186">click **All users**.</span></span>

6.  <span data-ttu-id="6d7f8-187">Kattintson a hello **multi-factor Authentication** hello panel felső hello gombra.</span><span class="sxs-lookup"><span data-stu-id="6d7f8-187">click hello **Multi-Factor Authentication** button at hello top of hello blade.</span></span>

7.  <span data-ttu-id="6d7f8-188">Egyszer hello **multi-factor Authentication felügyeleti portál** terhelés esetén gondoskodjon arról, hogy hello **felhasználók** fülre.</span><span class="sxs-lookup"><span data-stu-id="6d7f8-188">Once hello **Multi-Factor Authentication Administration Portal** loads, ensure you are on hello **Users** tab.</span></span>

8.  <span data-ttu-id="6d7f8-189">Keresés, szűréshez vagy rendezés hello felhasználói keresése hello azoknak a felhasználóknak.</span><span class="sxs-lookup"><span data-stu-id="6d7f8-189">Find hello user in hello list of users by searching, filtering, or sorting.</span></span>

9.  <span data-ttu-id="6d7f8-190">Azon felhasználók hello listájáról válassza hello felhasználói és **engedélyezése**, **tiltsa le a**, vagy **érvényesítése** többtényezős hitelesítést a kívánt módon működjenek.</span><span class="sxs-lookup"><span data-stu-id="6d7f8-190">Select hello user from hello list of users and **Enable**, **Disable**, or **Enforce** multi-factor authentication as desired.</span></span>

   >[!NOTE]
   ><span data-ttu-id="6d7f8-191">Ha egy felhasználó egy **kényszerített** állapotba kerül, előfordulhat, hogy túl beállítása**letiltott** ideiglenesen toolet újra be a fiókba.</span><span class="sxs-lookup"><span data-stu-id="6d7f8-191">If a user is in an **Enforced** state, you may set them too**Disabled** temporarily toolet them back into their account.</span></span> <span data-ttu-id="6d7f8-192">Miután bekerültek vissza, majd módosíthatja állapotukra túl**engedélyezve** újra toorequire őket toore regisztrálása során a következő kapcsolattartási adatait jelentkezzen be.</span><span class="sxs-lookup"><span data-stu-id="6d7f8-192">Once they are back in, you can then change their state too**Enabled** again toorequire them toore-register their contact information during their next sign in.</span></span> <span data-ttu-id="6d7f8-193">Másik lehetőségként a lépésekkel hello a hello [ellenőrizni kell a felhasználó hitelesítési kapcsolattartási adatait](#check-a-users-authentication-contact-info) tooverify, vagy állítsa be ezeket az adatokat a számukra.</span><span class="sxs-lookup"><span data-stu-id="6d7f8-193">Alternatively, you can follow hello steps in hello [Check a user’s authentication contact info](#check-a-users-authentication-contact-info) tooverify or set this data for them.</span></span>
   >
   >

### <a name="check-a-users-authentication-contact-info"></a><span data-ttu-id="6d7f8-194">Ellenőrizze a felhasználó hitelesítési kapcsolattartási adatai</span><span class="sxs-lookup"><span data-stu-id="6d7f8-194">Check a user’s authentication contact info</span></span>

<span data-ttu-id="6d7f8-195">a felhasználó hitelesítési kapcsolattartási adatok többtényezős hitelesítést, a feltételes hozzáférés, a Identity Protection és a jelszó alaphelyzetbe állításához használt toocheck hello lépéseket kövesse:</span><span class="sxs-lookup"><span data-stu-id="6d7f8-195">toocheck a user’s authentication contact info used for Multi-factor authentication, Conditional Access, Identity Protection, and Password Reset, follow hello steps below:</span></span>

1.  <span data-ttu-id="6d7f8-196">Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="6d7f8-196">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="6d7f8-197">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="6d7f8-197">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="6d7f8-198">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="6d7f8-198">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="6d7f8-199">Kattintson a **felhasználók és csoportok** hello navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="6d7f8-199">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="6d7f8-200">Kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="6d7f8-200">click **All users**.</span></span>

6.  <span data-ttu-id="6d7f8-201">**Keresési** hello felhasználó érdekli, és **hello sorban kattintson** tooselect.</span><span class="sxs-lookup"><span data-stu-id="6d7f8-201">**Search** for hello user you are interested in and **click hello row** tooselect.</span></span>

7.  <span data-ttu-id="6d7f8-202">Kattintson a **profil**.</span><span class="sxs-lookup"><span data-stu-id="6d7f8-202">click **Profile**.</span></span>

8.  <span data-ttu-id="6d7f8-203">Görgessen lefelé, túl**hitelesítési kapcsolattartási adatai**.</span><span class="sxs-lookup"><span data-stu-id="6d7f8-203">Scroll down too**Authentication contact info**.</span></span>

9.  <span data-ttu-id="6d7f8-204">**Felülvizsgálati** hello adatok regisztrált hello felhasználói és a frissítési igény szerint.</span><span class="sxs-lookup"><span data-stu-id="6d7f8-204">**Review** hello data registered for hello user and update as needed.</span></span>

### <a name="check-a-users-group-memberships"></a><span data-ttu-id="6d7f8-205">A felhasználói csoporttagság ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="6d7f8-205">Check a user’s group memberships</span></span>

<span data-ttu-id="6d7f8-206">toocheck egy felhasználó csoporttagságok hajtsa végre az alábbi hello lépéseket:</span><span class="sxs-lookup"><span data-stu-id="6d7f8-206">toocheck a user’s group memberships, follow hello steps below:</span></span>

1.  <span data-ttu-id="6d7f8-207">Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="6d7f8-207">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="6d7f8-208">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="6d7f8-208">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="6d7f8-209">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="6d7f8-209">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="6d7f8-210">Kattintson a **felhasználók és csoportok** hello navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="6d7f8-210">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="6d7f8-211">Kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="6d7f8-211">click **All users**.</span></span>

6.  <span data-ttu-id="6d7f8-212">**Keresési** hello felhasználó érdekli, és **hello sorban kattintson** tooselect.</span><span class="sxs-lookup"><span data-stu-id="6d7f8-212">**Search** for hello user you are interested in and **click hello row** tooselect.</span></span>

7.  <span data-ttu-id="6d7f8-213">Kattintson a **csoportok** toosee, amely hello felhasználói csoportok tagja.</span><span class="sxs-lookup"><span data-stu-id="6d7f8-213">click **Groups** toosee which groups hello user is a member of.</span></span>

### <a name="check-a-users-assigned-licenses"></a><span data-ttu-id="6d7f8-214">A felhasználó licenc-hozzárendeléseket ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="6d7f8-214">Check a user’s assigned licenses</span></span>

<span data-ttu-id="6d7f8-215">toocheck a felhasználó hozzárendelt licencek, hajtsa végre hello alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="6d7f8-215">toocheck a user’s assigned licenses, follow hello steps below:</span></span>

1.  <span data-ttu-id="6d7f8-216">Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="6d7f8-216">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="6d7f8-217">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="6d7f8-217">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="6d7f8-218">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="6d7f8-218">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="6d7f8-219">Kattintson a **felhasználók és csoportok** hello navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="6d7f8-219">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="6d7f8-220">Kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="6d7f8-220">click **All users**.</span></span>

6.  <span data-ttu-id="6d7f8-221">**Keresési** hello felhasználó érdekli, és **hello sorban kattintson** tooselect.</span><span class="sxs-lookup"><span data-stu-id="6d7f8-221">**Search** for hello user you are interested in and **click hello row** tooselect.</span></span>

7.  <span data-ttu-id="6d7f8-222">Kattintson a **licencek** toosee licencek hello felhasználóhoz van rendelve.</span><span class="sxs-lookup"><span data-stu-id="6d7f8-222">click **Licenses** toosee which licenses hello user currently has assigned.</span></span>

### <a name="assign-a-user-a-license"></a><span data-ttu-id="6d7f8-223">A felhasználó a licenc hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="6d7f8-223">Assign a user a license</span></span> 

<span data-ttu-id="6d7f8-224">a licenc tooa felhasználó tooassign kövesse hello alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="6d7f8-224">tooassign a license tooa user, follow hello steps below:</span></span>

1.  <span data-ttu-id="6d7f8-225">Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="6d7f8-225">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="6d7f8-226">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="6d7f8-226">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="6d7f8-227">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="6d7f8-227">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="6d7f8-228">Kattintson a **felhasználók és csoportok** hello navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="6d7f8-228">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="6d7f8-229">Kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="6d7f8-229">click **All users**.</span></span>

6.  <span data-ttu-id="6d7f8-230">**Keresési** hello felhasználó érdekli, és **hello sorban kattintson** tooselect.</span><span class="sxs-lookup"><span data-stu-id="6d7f8-230">**Search** for hello user you are interested in and **click hello row** tooselect.</span></span>

7.  <span data-ttu-id="6d7f8-231">Kattintson a **licencek** toosee licencek hello felhasználóhoz van rendelve.</span><span class="sxs-lookup"><span data-stu-id="6d7f8-231">click **Licenses** toosee which licenses hello user currently has assigned.</span></span>

8.  <span data-ttu-id="6d7f8-232">Kattintson a hello **hozzárendelése** gombra.</span><span class="sxs-lookup"><span data-stu-id="6d7f8-232">click hello **Assign** button.</span></span>

9.  <span data-ttu-id="6d7f8-233">Válassza ki **egy vagy több termék** választható termékek hello listája.</span><span class="sxs-lookup"><span data-stu-id="6d7f8-233">Select **one or more products** from hello list of available products.</span></span>

10. <span data-ttu-id="6d7f8-234">**Nem kötelező** hello kattintson **hozzárendelés beállításai** elem toogranularly rendelje hozzá a termékek.</span><span class="sxs-lookup"><span data-stu-id="6d7f8-234">**Optional** click hello **assignment options** item toogranularly assign products.</span></span> <span data-ttu-id="6d7f8-235">Kattintson a **Ok** amikor ez befejeződik.</span><span class="sxs-lookup"><span data-stu-id="6d7f8-235">Click **Ok** when this is completed.</span></span>

11. <span data-ttu-id="6d7f8-236">Kattintson a hello **hozzárendelése** tooassign ezen licencek toothis felhasználói gombra.</span><span class="sxs-lookup"><span data-stu-id="6d7f8-236">Click hello **Assign** button tooassign these licenses toothis user.</span></span>

## <a name="if-these-troubleshooting-steps-do-not-resolve-hello-issue"></a><span data-ttu-id="6d7f8-237">Ha ezek a hibaelhárítási lépéseket nem oldható meg hello probléma</span><span class="sxs-lookup"><span data-stu-id="6d7f8-237">If these troubleshooting steps do not resolve hello issue</span></span>

<span data-ttu-id="6d7f8-238">Nyisson meg egy támogatási jegy hello ha rendelkezésre áll a következő információkat:</span><span class="sxs-lookup"><span data-stu-id="6d7f8-238">open a support ticket with hello following information if available:</span></span>

-   <span data-ttu-id="6d7f8-239">Megfelelési hiba azonosítója</span><span class="sxs-lookup"><span data-stu-id="6d7f8-239">Correlation error ID</span></span>

-   <span data-ttu-id="6d7f8-240">Egyszerű felhasználónév (felhasználó e-mail címe)</span><span class="sxs-lookup"><span data-stu-id="6d7f8-240">UPN (user email address)</span></span>

-   <span data-ttu-id="6d7f8-241">Bérlőazonosító</span><span class="sxs-lookup"><span data-stu-id="6d7f8-241">Tenant ID</span></span>

-   <span data-ttu-id="6d7f8-242">Böngésző típusa</span><span class="sxs-lookup"><span data-stu-id="6d7f8-242">Browser type</span></span>

-   <span data-ttu-id="6d7f8-243">Időzóna és idő/időkeretre során hiba történik.</span><span class="sxs-lookup"><span data-stu-id="6d7f8-243">Time zone and time/timeframe during error occurs</span></span>

-   <span data-ttu-id="6d7f8-244">Fiddler nyomkövetések</span><span class="sxs-lookup"><span data-stu-id="6d7f8-244">Fiddler traces</span></span>

## <a name="next-steps"></a><span data-ttu-id="6d7f8-245">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6d7f8-245">Next steps</span></span>
[<span data-ttu-id="6d7f8-246">Adja meg az egyszeri bejelentkezés tooyour alkalmazások alkalmazásproxyval</span><span class="sxs-lookup"><span data-stu-id="6d7f8-246">Provide single sign-on tooyour apps with Application Proxy</span></span>](active-directory-application-proxy-sso-using-kcd.md)
