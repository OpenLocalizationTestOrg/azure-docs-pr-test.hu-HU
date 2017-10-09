---
title: "Jelentkezzen be a jelszó-konfigurációjának Azure AD-katalógusában alkalmazás tooan aaaProblems egyszeri bejelentkezés |} Microsoft Docs"
description: "Ismerteti, amely útmutatást tootroubleshoot problémák kapcsolódó toosigning tooAzure a jelszó egyszeri bejelentkezést a beállított AD gyűjtemény alkalmazások problémás területek"
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
ms.openlocfilehash: f53ef4176db37dc6b1da2d61027155a6ba8f331e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="problems-signing-in-tooan-azure-ad-gallery-application-configured-for-password-single-sign-on"></a><span data-ttu-id="306ee-103">A jelszó egyszeri bejelentkezés beállítása az Azure AD-gyűjtemény alkalmazás tooan problémák</span><span class="sxs-lookup"><span data-stu-id="306ee-103">Problems signing in tooan Azure AD Gallery application configured for password single sign-on</span></span>

<span data-ttu-id="306ee-104">Hozzáférési Panel hello egy webes portál, mely lehetővé teszi, hogy egy felhasználó, aki rendelkezik a munkahelyi vagy iskolai fiókot az Azure Active Directory (Azure AD) tooview, és indítsa el felhőalapú alkalmazások, hogy az Azure AD hello rendszergazda engedélyezte őket a hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="306ee-104">hello Access Panel is a web-based portal which enables a user who has a work or school account in Azure Active Directory (Azure AD) tooview and launch cloud-based applications that hello Azure AD administrator has granted them access to.</span></span> <span data-ttu-id="306ee-105">Egy felhasználó, aki rendelkezik az Azure AD-verziók is használhatja, önkiszolgáló csoportkezelési és a hozzáférési Panel hello keresztül kezelési képességeit.</span><span class="sxs-lookup"><span data-stu-id="306ee-105">A user who has Azure AD editions can also use self-service group and app management capabilities through hello Access Panel.</span></span> <span data-ttu-id="306ee-106">Hozzáférési Panel hello elkülönül hello Azure-portálon, és nem igényli a felhasználók toohave Azure-előfizetéssel.</span><span class="sxs-lookup"><span data-stu-id="306ee-106">hello Access Panel is separate from hello Azure portal and does not require users toohave an Azure subscription.</span></span>

<span data-ttu-id="306ee-107">toouse jelszó-alapú egyszeri bejelentkezést (SSO) a hozzáférési Panel, a hozzáférési Panel bővítmény hello hello telepíteni kell hello felhasználó böngészőjében.</span><span class="sxs-lookup"><span data-stu-id="306ee-107">toouse password-based single sign-on (SSO) in hello Access Panel, hello Access Panel extension must be installed in hello user’s browser.</span></span> <span data-ttu-id="306ee-108">A bővítmény le automatikusan, ha a felhasználó megadja egy alkalmazás, amely jelszóalapú SSO van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="306ee-108">This extension is downloaded automatically when a user selects an application that is configured for password-based SSO.</span></span>

## <a name="meeting-browser-requirements-for-hello-access-panel"></a><span data-ttu-id="306ee-109">A hozzáférési Panel hello értekezlet böngészőre vonatkozó követelményei</span><span class="sxs-lookup"><span data-stu-id="306ee-109">Meeting browser requirements for hello Access Panel</span></span>

<span data-ttu-id="306ee-110">hello hozzáférési Panel igényel, amely támogatja a JavaScript egy böngészőt, és CSS engedélyezte.</span><span class="sxs-lookup"><span data-stu-id="306ee-110">hello Access Panel requires a browser that supports JavaScript and has CSS enabled.</span></span> <span data-ttu-id="306ee-111">toouse jelszó-alapú egyszeri bejelentkezést (SSO) a hozzáférési Panel, a hozzáférési Panel bővítmény hello hello telepíteni kell hello felhasználó böngészőjében.</span><span class="sxs-lookup"><span data-stu-id="306ee-111">toouse password-based single sign-on (SSO) in hello Access Panel, hello Access Panel extension must be installed in hello user’s browser.</span></span> <span data-ttu-id="306ee-112">A bővítmény le automatikusan, ha a felhasználó megadja egy alkalmazás, amely jelszóalapú SSO van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="306ee-112">This extension is downloaded automatically when a user selects an application that is configured for password-based SSO.</span></span>

<span data-ttu-id="306ee-113">Az egyszeri bejelentkezés jelszóalapú hello végfelhasználó böngészőkkel lehet:</span><span class="sxs-lookup"><span data-stu-id="306ee-113">For password-based SSO, hello end user’s browsers can be:</span></span>

-   <span data-ttu-id="306ee-114">Internet Explorer 8, 9, 10, 11 – a Windows 7 vagy újabb</span><span class="sxs-lookup"><span data-stu-id="306ee-114">Internet Explorer 8, 9, 10, 11 -- on Windows 7 or later</span></span>

-   <span data-ttu-id="306ee-115">Chrome – A Windows 7 vagy újabb, és MacOS X rendszeren vagy újabb</span><span class="sxs-lookup"><span data-stu-id="306ee-115">Chrome -- on Windows 7 or later, and on MacOS X or later</span></span>

-   <span data-ttu-id="306ee-116">Firefox 26.0 vagy újabb – a Windows XP SP2 vagy újabb, és a Mac OS X 10,6 vagy újabb verzió</span><span class="sxs-lookup"><span data-stu-id="306ee-116">Firefox 26.0 or later -- on Windows XP SP2 or later, and on Mac OS X 10.6 or later</span></span>

>[!NOTE]
><span data-ttu-id="306ee-117">hello jelszóalapú SSO bővítmény a Windows 10-es szegély rendelkezésre állására böngészőbővítményeket peremhálózati lesz támogatott.</span><span class="sxs-lookup"><span data-stu-id="306ee-117">hello password-based SSO extension become available for Edge in Windows 10 when browser extensions become supported for Edge.</span></span>
>
>

## <a name="how-tooinstall-hello-access-panel-browser-extension"></a><span data-ttu-id="306ee-118">Hogyan tooinstall hello hozzáférési Panel bővítmény</span><span class="sxs-lookup"><span data-stu-id="306ee-118">How tooinstall hello Access Panel Browser extension</span></span>

<span data-ttu-id="306ee-119">tooinstall hello hozzáférési Panel bővítmény, kövesse az alábbi hello lépéseket:</span><span class="sxs-lookup"><span data-stu-id="306ee-119">tooinstall hello Access Panel Browser extension, follow hello steps below:</span></span>

1.  <span data-ttu-id="306ee-120">Nyissa meg hello [hozzáférési Panel](https://myapps.microsoft.com) valamelyik hello támogatott böngészők és való bejelentkezést egy **felhasználói** az Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="306ee-120">Open hello [Access Panel](https://myapps.microsoft.com) in one of hello supported browsers and sign in as a **user** in your Azure AD.</span></span>

2.  <span data-ttu-id="306ee-121">Kattintson egy **jelszó-SSO alkalmazás** a hozzáférési Panel hello.</span><span class="sxs-lookup"><span data-stu-id="306ee-121">click a **password-SSO application** in hello Access Panel.</span></span>

3.  <span data-ttu-id="306ee-122">Hello Rákérdezés azzal a kérdéssel tooinstall hello szoftver, válassza ki **telepítés**.</span><span class="sxs-lookup"><span data-stu-id="306ee-122">In hello prompt asking tooinstall hello software, select **Install Now**.</span></span>

4.  <span data-ttu-id="306ee-123">A böngésző alapján kell irányított toohello letöltési hivatkozását.</span><span class="sxs-lookup"><span data-stu-id="306ee-123">Based on your browser you be directed toohello download link.</span></span> <span data-ttu-id="306ee-124">**Adja hozzá** hello bővítmény tooyour böngésző.</span><span class="sxs-lookup"><span data-stu-id="306ee-124">**Add** hello extension tooyour browser.</span></span>

5.  <span data-ttu-id="306ee-125">Ha a böngésző kéri, válassza ki a tooeither **engedélyezése** vagy **engedélyezése** hello bővítmény.</span><span class="sxs-lookup"><span data-stu-id="306ee-125">If your browser asks, select tooeither **Enable** or **Allow** hello extension.</span></span>

6.  <span data-ttu-id="306ee-126">Korábban telepítve, **indítsa újra a** a böngésző-munkamenetet.</span><span class="sxs-lookup"><span data-stu-id="306ee-126">Once installed, **restart** your browser session.</span></span>

7.  <span data-ttu-id="306ee-127">Jelentkezzen be a hozzáférési Panel hello, és tekintse meg, ha **indítása** a jelszó-egyszeri bejelentkezés alkalmazásokhoz</span><span class="sxs-lookup"><span data-stu-id="306ee-127">Sign in into hello Access Panel and see if you can **launch** your password-SSO applications</span></span>

<span data-ttu-id="306ee-128">Az alábbi hello közvetlen hivatkozások Chrome és Firefox is letöltheti hello bővítmény:</span><span class="sxs-lookup"><span data-stu-id="306ee-128">You may also download hello extension for Chrome and Firefox from hello direct links below:</span></span>

-   [<span data-ttu-id="306ee-129">Chrome hozzáférési Panel bővítmény</span><span class="sxs-lookup"><span data-stu-id="306ee-129">Chrome Access Panel Extension</span></span>](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [<span data-ttu-id="306ee-130">A Firefox hozzáférési Panel bővítmény</span><span class="sxs-lookup"><span data-stu-id="306ee-130">Firefox Access Panel Extension</span></span>](https://addons.mozilla.org/firefox/addon/access-panel-extension/)

## <a name="setting-up-a-group-policy-for-internet-explorer"></a><span data-ttu-id="306ee-131">A csoportházirend beállítása az Internet Explorerben</span><span class="sxs-lookup"><span data-stu-id="306ee-131">Setting up a group policy for Internet Explorer</span></span>

<span data-ttu-id="306ee-132">Beállíthatja a csoportházirend, amelyek lehetővé teszik tooremotely telepítés hello hozzáférési Panel bővítményt az Internet Explorer a felhasználók gépeken.</span><span class="sxs-lookup"><span data-stu-id="306ee-132">You can setup a group policy that allow you tooremotely install hello Access Panel extension for Internet Explorer on your users' machines.</span></span>

<span data-ttu-id="306ee-133">hello Előfeltételek a következők:</span><span class="sxs-lookup"><span data-stu-id="306ee-133">hello prerequisites include:</span></span>

-   <span data-ttu-id="306ee-134">Ezzel beállította [Active Directory tartományi szolgáltatások](https://msdn.microsoft.com/library/aa362244%28v=vs.85%29.aspx), és a felhasználók gépek tooyour tartományhoz csatlakozott.</span><span class="sxs-lookup"><span data-stu-id="306ee-134">You have set up [Active Directory Domain Services](https://msdn.microsoft.com/library/aa362244%28v=vs.85%29.aspx), and you have joined your users' machines tooyour domain.</span></span>

-   <span data-ttu-id="306ee-135">Hello "Beállítások módosítása" engedéllyel tooedit hello csoportházirend-objektumot (GPO) kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="306ee-135">You must have hello "Edit settings" permission tooedit hello Group Policy Object (GPO).</span></span> <span data-ttu-id="306ee-136">Alapértelmezés szerint a következő biztonsági csoportokat hello tagjai rendelkezik ilyen engedéllyel: tartományi rendszergazdák, a vállalati rendszergazdák és a Csoportházirend-létrehozó tulajdonosok.</span><span class="sxs-lookup"><span data-stu-id="306ee-136">By default, members of hello following security groups have this permission: Domain Administrators, Enterprise Administrators, and Group Policy Creator Owners.</span></span> <span data-ttu-id="306ee-137">[További információk](https://technet.microsoft.com/library/cc781991%28v=ws.10%29.aspx).</span><span class="sxs-lookup"><span data-stu-id="306ee-137">[Learn more](https://technet.microsoft.com/library/cc781991%28v=ws.10%29.aspx).</span></span>

<span data-ttu-id="306ee-138">Hello az oktatóanyagot követve [hogyan tooDeploy hello csoportházirend használatával az Internet Explorer hozzáférési Panel bővítmény](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ie-group-policy) hogyan tooconfigure hello csoportházirend, majd központilag telepítenie toousers részletes információkra van szüksége.</span><span class="sxs-lookup"><span data-stu-id="306ee-138">Follow hello tutorial [How tooDeploy hello Access Panel Extension for Internet Explorer using Group Policy](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ie-group-policy) for step by step instructions on how tooconfigure hello group policy and deploy it toousers.</span></span>

## <a name="troubleshoot-hello-access-panel-in-internet-explorer"></a><span data-ttu-id="306ee-139">Az Internet Explorerben a hozzáférési Panel hello hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="306ee-139">Troubleshoot hello Access Panel in Internet Explorer</span></span>

<span data-ttu-id="306ee-140">Hajtsa végre a hello [kapcsolatos problémák elhárítása hello hozzáférési Panel bővítményét az Internet Explorer](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ie-Troubleshoot) útmutatót a hozzáféréshez olyan diagnosztikai eszköz és részletes utasításokat az hello bővítmény konfigurálása az Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="306ee-140">Follow hello [Troubleshoot hello Access Panel Extension for Internet Explorer](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ie-Troubleshoot) guide for access a diagnostics tool and step by step instructions on configuring hello extension for IE.</span></span>

## <a name="how-tooconfigure-password-single-sign-on-for-a-non-gallery-application"></a><span data-ttu-id="306ee-141">Hogyan tooconfigure jelszó egyszeri bejelentkezés egy nem galéria alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="306ee-141">How tooconfigure password single sign-on for a non-gallery application</span></span>

<span data-ttu-id="306ee-142">egy alkalmazás hello Azure AD-galériából tooconfigure kell:</span><span class="sxs-lookup"><span data-stu-id="306ee-142">tooconfigure an application from hello Azure AD gallery you need to:</span></span>

-   [<span data-ttu-id="306ee-143">Nem galéria alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="306ee-143">Add a non-gallery application</span></span>](#add-a-non-gallery-application)

-   [<span data-ttu-id="306ee-144">Állítsa be hello alkalmazását jelszó egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="306ee-144">Configure hello application for password single sign-on</span></span>](#configure-the-application-for-password-single-sign-on)

-   [<span data-ttu-id="306ee-145">Felhasználók hozzárendelése toohello alkalmazás</span><span class="sxs-lookup"><span data-stu-id="306ee-145">Assign users toohello application</span></span>](#assign-users-to-the-application)

### <a name="add-a-non-gallery-application"></a><span data-ttu-id="306ee-146">Nem galéria alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="306ee-146">Add a non-gallery application</span></span>

<span data-ttu-id="306ee-147">az alkalmazás a hello Azure AD-katalógusában, tooadd kövesse hello alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="306ee-147">tooadd an application from hello Azure AD Gallery, follow hello steps below:</span></span>

1.  <span data-ttu-id="306ee-148">Nyissa meg hello [Azure Portal](https://portal.azure.com) , és jelentkezzen be a egy **globális rendszergazda** vagy **társadminisztrátornak**</span><span class="sxs-lookup"><span data-stu-id="306ee-148">Open hello [Azure Portal](https://portal.azure.com) and sign in as a **Global Administrator** or **Co-admin**</span></span>

2.  <span data-ttu-id="306ee-149">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="306ee-149">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="306ee-150">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="306ee-150">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="306ee-151">Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="306ee-151">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="306ee-152">Kattintson a hello **hozzáadása** hello jobb felső sarokban lévő hello a gomb **vállalati alkalmazások** panel</span><span class="sxs-lookup"><span data-stu-id="306ee-152">click hello **Add** button at hello top-right corner on hello **Enterprise Applications** blade</span></span>

6.  <span data-ttu-id="306ee-153">Kattintson a **nem-gyűjtemény alkalmazás.**</span><span class="sxs-lookup"><span data-stu-id="306ee-153">click **Non-gallery application.**</span></span>

7.  <span data-ttu-id="306ee-154">Adja meg az alkalmazás nevére hello hello **neve** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="306ee-154">Enter hello name of your application in hello **Name** textbox.</span></span> <span data-ttu-id="306ee-155">Válassza ki **hozzáadása.**</span><span class="sxs-lookup"><span data-stu-id="306ee-155">Select **Add.**</span></span>

<span data-ttu-id="306ee-156">Egy rövid időszak után kell képes toosee hello alkalmazás konfigurációs panelen.</span><span class="sxs-lookup"><span data-stu-id="306ee-156">After a short period, you be able toosee hello application’s configuration blade.</span></span>

### <a name="configure-hello-application-for-password-single-sign-on"></a><span data-ttu-id="306ee-157">Állítsa be hello alkalmazását jelszó egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="306ee-157">Configure hello application for password single sign-on</span></span>

<span data-ttu-id="306ee-158">tooconfigure egyszeri bejelentkezés egy alkalmazás kövesse az alábbi hello lépéseket:</span><span class="sxs-lookup"><span data-stu-id="306ee-158">tooconfigure single sign-on for an application, follow hello steps below:</span></span>

1.  <span data-ttu-id="306ee-159">Nyitott hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="306ee-159">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="306ee-160">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="306ee-160">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="306ee-161">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="306ee-161">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="306ee-162">Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="306ee-162">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="306ee-163">Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="306ee-163">click **All Applications** tooview a list of all your applications.</span></span>

   * <span data-ttu-id="306ee-164">Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="306ee-164">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="306ee-165">Válassza ki a kívánt tooconfigure egyszeri bejelentkezés hello alkalmazás</span><span class="sxs-lookup"><span data-stu-id="306ee-165">Select hello application you want tooconfigure single sign-on</span></span>

7.  <span data-ttu-id="306ee-166">Ha hello alkalmazás betölt, kattintson a hello **egyszeri bejelentkezés** hello alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="306ee-166">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="306ee-167">Jelölje be hello mód **jelszóalapú bejelentkezés.**</span><span class="sxs-lookup"><span data-stu-id="306ee-167">Select hello mode **Password-based Sign-on.**</span></span>

9.  <span data-ttu-id="306ee-168">Adja meg a hello **bejelentkezési URL-cím**.</span><span class="sxs-lookup"><span data-stu-id="306ee-168">Enter hello **Sign-on URL**.</span></span> <span data-ttu-id="306ee-169">Ez a hello URL-cím, ahol felhasználók adja meg a felhasználónevet és jelszót toosign a számára.</span><span class="sxs-lookup"><span data-stu-id="306ee-169">This is hello URL where users enter their username and password toosign in to.</span></span> <span data-ttu-id="306ee-170">Győződjön meg arról, mezők hello bejelentkezés láthatók hello URL-címen.</span><span class="sxs-lookup"><span data-stu-id="306ee-170">Ensure hello sign in fields are visible at hello URL.</span></span>

10. <span data-ttu-id="306ee-171">Felhasználók hozzárendelése toohello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="306ee-171">Assign users toohello application.</span></span>

11. <span data-ttu-id="306ee-172">Emellett is megadhatja hello felhasználó nevében hitelesítő adatok hello felhasználók hello sorát kiválasztásával, és kattintson a **frissítéséhez szükséges hitelesítő adatokat** és hello felhasználónév és jelszó megadásával hello felhasználók nevében.</span><span class="sxs-lookup"><span data-stu-id="306ee-172">Additionally, you can also provide credentials on behalf of hello user by selecting hello rows of hello users and clicking on **Update Credentials** and entering hello username and password on behalf of hello users.</span></span> <span data-ttu-id="306ee-173">Ellenkező esetben a felhasználók fognak felszólító tooenter hello hitelesítő adatok magukat az indítás után.</span><span class="sxs-lookup"><span data-stu-id="306ee-173">Otherwise, users be prompted tooenter hello credentials themselves upon launch.</span></span>

### <a name="assign-users-toohello-application"></a><span data-ttu-id="306ee-174">Felhasználók hozzárendelése toohello alkalmazás</span><span class="sxs-lookup"><span data-stu-id="306ee-174">Assign users toohello application</span></span>

<span data-ttu-id="306ee-175">tooassign felhasználók tooan egy vagy több alkalmazás közvetlenül, kövesse az alábbi hello lépéseket:</span><span class="sxs-lookup"><span data-stu-id="306ee-175">tooassign one or more users tooan application directly, follow hello steps below:</span></span>

1.  <span data-ttu-id="306ee-176">Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="306ee-176">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="306ee-177">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="306ee-177">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="306ee-178">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="306ee-178">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="306ee-179">Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="306ee-179">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="306ee-180">Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="306ee-180">click **All Applications** tooview a list of all your applications.</span></span>

   * <span data-ttu-id="306ee-181">Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="306ee-181">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="306ee-182">Válassza ki a kívánt felhasználó toofrom hello listáját tooassign hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="306ee-182">Select hello application you want tooassign a user toofrom hello list.</span></span>

7.  <span data-ttu-id="306ee-183">Amikor hello alkalmazás betölt, kattintson a **felhasználók és csoportok** hello alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="306ee-183">Once hello application loads, click **Users and Groups** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="306ee-184">Hello kattintson **Hozzáadás** gomb felett hello **felhasználók és csoportok** lista tooopen hello **hozzáadása hozzárendelés** panelen.</span><span class="sxs-lookup"><span data-stu-id="306ee-184">Click hello **Add** button on top of hello **Users and Groups** list tooopen hello **Add Assignment** blade.</span></span>

9.  <span data-ttu-id="306ee-185">hello kattintson **felhasználók és csoportok** hello a választó **hozzáadása hozzárendelés** panelen.</span><span class="sxs-lookup"><span data-stu-id="306ee-185">click hello **Users and groups** selector from hello **Add Assignment** blade.</span></span>

10. <span data-ttu-id="306ee-186">Hello típusának **teljes név** vagy **e-mail cím** érdekli hello való hozzárendelése hello felhasználó **Keresés név vagy e-mail cím alapján** keresőmezőbe.</span><span class="sxs-lookup"><span data-stu-id="306ee-186">Type in hello **full name** or **email address** of hello user you are interested in assigning into hello **Search by name or email address** search box.</span></span>

11. <span data-ttu-id="306ee-187">Hello rámutat **felhasználói** a hello lista tooreveal egy **jelölőnégyzet**.</span><span class="sxs-lookup"><span data-stu-id="306ee-187">Hover over hello **user** in hello list tooreveal a **checkbox**.</span></span> <span data-ttu-id="306ee-188">Kattintson a hello jelölőnégyzet következő toohello felhasználó profil fénykép vagy embléma tooadd a felhasználó toohello **kijelölt** listája.</span><span class="sxs-lookup"><span data-stu-id="306ee-188">Click hello checkbox next toohello user’s profile photo or logo tooadd your user toohello **Selected** list.</span></span>

12. <span data-ttu-id="306ee-189">**Választható lehetőség:** Ha túl szeretné**egynél több felhasználó hozzáadása**, egy másik típus **teljes név** vagy **e-mail cím** be hello **Keresés név e-mail cím vagy** keresési mezőbe, majd kattintson a hello jelölőnégyzet tooadd a felhasználó toohello **kijelölt** listája.</span><span class="sxs-lookup"><span data-stu-id="306ee-189">**Optional:** If you would like too**add more than one user**, type in another **full name** or **email address** into hello **Search by name or email address** search box, and click hello checkbox tooadd this user toohello **Selected** list.</span></span>

13. <span data-ttu-id="306ee-190">Ha elkészült, válassza a felhasználók, kattintson a hello **válasszon** gomb tooadd őket felhasználók és csoportok toobe toohello listája toohello alkalmazás hozzárendelése.</span><span class="sxs-lookup"><span data-stu-id="306ee-190">When you are finished selecting users, click hello **Select** button tooadd them toohello list of users and groups toobe assigned toohello application.</span></span>

14. <span data-ttu-id="306ee-191">**Nem kötelező:** hello kattintson **Szerepkörválasztás** hello a választó **hozzáadása hozzárendelés** panel tooselect szerepkör tooassign toohello felhasználók kijelölt.</span><span class="sxs-lookup"><span data-stu-id="306ee-191">**Optional:** click hello **Select Role** selector in hello **Add Assignment** blade tooselect a role tooassign toohello users you have selected.</span></span>

15. <span data-ttu-id="306ee-192">Kattintson a hello **hozzárendelése** gomb tooassign hello alkalmazás toohello kijelölt felhasználók.</span><span class="sxs-lookup"><span data-stu-id="306ee-192">Click hello **Assign** button tooassign hello application toohello selected users.</span></span>

<span data-ttu-id="306ee-193">Miután rövid időn belül, kijelölt hello felhasználók kell tudni toolaunch ezeket az alkalmazásokat hello a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="306ee-193">After a short period, hello users you have selected be able toolaunch these applications in hello Access Panel.</span></span>

## <a name="if-these-troubleshooting-steps-do-not-hello-resolve-hello-issue"></a><span data-ttu-id="306ee-194">Ha a lépések nem hello hello probléma megoldásához.</span><span class="sxs-lookup"><span data-stu-id="306ee-194">If these troubleshooting steps do not hello resolve hello issue</span></span>

<span data-ttu-id="306ee-195">Nyisson meg egy támogatási jegy hello ha rendelkezésre áll a következő információkat:</span><span class="sxs-lookup"><span data-stu-id="306ee-195">open a support ticket with hello following information if available:</span></span>

-   <span data-ttu-id="306ee-196">Megfelelési hiba azonosítója</span><span class="sxs-lookup"><span data-stu-id="306ee-196">Correlation error ID</span></span>

-   <span data-ttu-id="306ee-197">Egyszerű felhasználónév (felhasználó e-mail címe)</span><span class="sxs-lookup"><span data-stu-id="306ee-197">UPN (user email address)</span></span>

-   <span data-ttu-id="306ee-198">A TenantID</span><span class="sxs-lookup"><span data-stu-id="306ee-198">TenantID</span></span>

-   <span data-ttu-id="306ee-199">Böngésző típusa</span><span class="sxs-lookup"><span data-stu-id="306ee-199">Browser type</span></span>

-   <span data-ttu-id="306ee-200">Időzóna és idő/időkeretre során hiba történik.</span><span class="sxs-lookup"><span data-stu-id="306ee-200">Time zone and time/timeframe during error occurs</span></span>

-   <span data-ttu-id="306ee-201">Fiddler nyomkövetések</span><span class="sxs-lookup"><span data-stu-id="306ee-201">Fiddler traces</span></span>

## <a name="next-steps"></a><span data-ttu-id="306ee-202">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="306ee-202">Next steps</span></span>
[<span data-ttu-id="306ee-203">Adja meg az egyszeri bejelentkezés tooyour alkalmazások alkalmazásproxyval</span><span class="sxs-lookup"><span data-stu-id="306ee-203">Provide single sign-on tooyour apps with Application Proxy</span></span>](active-directory-application-proxy-sso-using-kcd.md)

