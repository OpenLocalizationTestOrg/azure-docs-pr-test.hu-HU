---
title: "A jelszó-konfigurációjának Azure AD-katalógusában alkalmazáshoz való bejelentkezés problémák egyszeri bejelentkezés |} Microsoft Docs"
description: "Ismerteti a problémás területek, hogy miként kell bejelentkezni a jelszót az egyszeri bejelentkezés beállítása az Azure AD-gyűjtemény alkalmazások kapcsolatos problémák elhárítása"
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
ms.openlocfilehash: c90b61812affb7e7af05cf3e302d045958da59be
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="problems-signing-in-to-an-azure-ad-gallery-application-configured-for-password-single-sign-on"></a><span data-ttu-id="bc72b-103">Jelszó az egyszeri bejelentkezés beállítása az Azure AD-gyűjtemény alkalmazáshoz való bejelentkezés problémák</span><span class="sxs-lookup"><span data-stu-id="bc72b-103">Problems signing in to an Azure AD Gallery application configured for password single sign-on</span></span>

<span data-ttu-id="bc72b-104">A hozzáférési Panel egy webes portál, amely lehetővé teszi a felhasználó, aki rendelkezik munkahelyi vagy iskolai fiókkal az Azure Active Directory (Azure AD) egy megtekintheti és elindíthatja felhőalapú alkalmazásokhoz az Azure AD-rendszergazda engedélyezte őket a hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="bc72b-104">The Access Panel is a web-based portal which enables a user who has a work or school account in Azure Active Directory (Azure AD) to view and launch cloud-based applications that the Azure AD administrator has granted them access to.</span></span> <span data-ttu-id="bc72b-105">Azure AD-verziók rendelkező felhasználó az önkiszolgáló csoportkezelési és a hozzáférési Panel az alkalmazás-felügyeleti képességeit használhatja.</span><span class="sxs-lookup"><span data-stu-id="bc72b-105">A user who has Azure AD editions can also use self-service group and app management capabilities through the Access Panel.</span></span> <span data-ttu-id="bc72b-106">A hozzáférési Panel elkülönül az Azure-portálon, és nem igényli a felhasználók Azure-előfizetésre.</span><span class="sxs-lookup"><span data-stu-id="bc72b-106">The Access Panel is separate from the Azure portal and does not require users to have an Azure subscription.</span></span>

<span data-ttu-id="bc72b-107">Jelszó-alapú egyszeri bejelentkezést (SSO) használ a hozzáférési panelen, a hozzáférési Panel bővítményt telepíteni kell a felhasználó böngészőben.</span><span class="sxs-lookup"><span data-stu-id="bc72b-107">To use password-based single sign-on (SSO) in the Access Panel, the Access Panel extension must be installed in the user’s browser.</span></span> <span data-ttu-id="bc72b-108">A bővítmény le automatikusan, ha a felhasználó megadja egy alkalmazás, amely jelszóalapú SSO van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="bc72b-108">This extension is downloaded automatically when a user selects an application that is configured for password-based SSO.</span></span>

## <a name="meeting-browser-requirements-for-the-access-panel"></a><span data-ttu-id="bc72b-109">A hozzáférési Panel értekezlet böngészőre vonatkozó követelményei</span><span class="sxs-lookup"><span data-stu-id="bc72b-109">Meeting browser requirements for the Access Panel</span></span>

<span data-ttu-id="bc72b-110">A hozzáférési Panel igényel, amely támogatja a JavaScript egy böngészőt, és CSS engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="bc72b-110">The Access Panel requires a browser that supports JavaScript and has CSS enabled.</span></span> <span data-ttu-id="bc72b-111">Jelszó-alapú egyszeri bejelentkezést (SSO) használ a hozzáférési panelen, a hozzáférési Panel bővítményt telepíteni kell a felhasználó böngészőben.</span><span class="sxs-lookup"><span data-stu-id="bc72b-111">To use password-based single sign-on (SSO) in the Access Panel, the Access Panel extension must be installed in the user’s browser.</span></span> <span data-ttu-id="bc72b-112">A bővítmény le automatikusan, ha a felhasználó megadja egy alkalmazás, amely jelszóalapú SSO van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="bc72b-112">This extension is downloaded automatically when a user selects an application that is configured for password-based SSO.</span></span>

<span data-ttu-id="bc72b-113">Jelszó-alapú egyszeri bejelentkezéshez a végfelhasználó böngészőkkel lehet:</span><span class="sxs-lookup"><span data-stu-id="bc72b-113">For password-based SSO, the end user’s browsers can be:</span></span>

-   <span data-ttu-id="bc72b-114">Internet Explorer 8, 9, 10, 11 – a Windows 7 vagy újabb</span><span class="sxs-lookup"><span data-stu-id="bc72b-114">Internet Explorer 8, 9, 10, 11 -- on Windows 7 or later</span></span>

-   <span data-ttu-id="bc72b-115">Chrome – A Windows 7 vagy újabb, és MacOS X rendszeren vagy újabb</span><span class="sxs-lookup"><span data-stu-id="bc72b-115">Chrome -- on Windows 7 or later, and on MacOS X or later</span></span>

-   <span data-ttu-id="bc72b-116">Firefox 26.0 vagy újabb – a Windows XP SP2 vagy újabb, és a Mac OS X 10,6 vagy újabb verzió</span><span class="sxs-lookup"><span data-stu-id="bc72b-116">Firefox 26.0 or later -- on Windows XP SP2 or later, and on Mac OS X 10.6 or later</span></span>

>[!NOTE]
><span data-ttu-id="bc72b-117">A jelszó-alapú egyszeri Bejelentkezést bővítmény szegély a Windows 10-es rendelkezésre állására böngészőbővítményeket peremhálózati lesz támogatott.</span><span class="sxs-lookup"><span data-stu-id="bc72b-117">The password-based SSO extension become available for Edge in Windows 10 when browser extensions become supported for Edge.</span></span>
>
>

## <a name="how-to-install-the-access-panel-browser-extension"></a><span data-ttu-id="bc72b-118">A hozzáférési Panel bővítmény telepítése</span><span class="sxs-lookup"><span data-stu-id="bc72b-118">How to install the Access Panel Browser extension</span></span>

<span data-ttu-id="bc72b-119">A hozzáférési Panel bővítmény telepítéséhez kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="bc72b-119">To install the Access Panel Browser extension, follow the steps below:</span></span>

1.  <span data-ttu-id="bc72b-120">Nyissa meg a [hozzáférési Panel](https://myapps.microsoft.com) valamelyik támogatott böngészők és való bejelentkezést egy **felhasználói** az Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="bc72b-120">Open the [Access Panel](https://myapps.microsoft.com) in one of the supported browsers and sign in as a **user** in your Azure AD.</span></span>

2.  <span data-ttu-id="bc72b-121">Kattintson egy **jelszó-SSO alkalmazás** a hozzáférési panelen.</span><span class="sxs-lookup"><span data-stu-id="bc72b-121">click a **password-SSO application** in the Access Panel.</span></span>

3.  <span data-ttu-id="bc72b-122">Válassza ki a szoftver telepítéséhez megkérdezi **telepítés**.</span><span class="sxs-lookup"><span data-stu-id="bc72b-122">In the prompt asking to install the software, select **Install Now**.</span></span>

4.  <span data-ttu-id="bc72b-123">A böngésző alapján kell irányítani a letöltési hivatkozás.</span><span class="sxs-lookup"><span data-stu-id="bc72b-123">Based on your browser you be directed to the download link.</span></span> <span data-ttu-id="bc72b-124">**Adja hozzá** az bővítményt, hogy a böngészőben.</span><span class="sxs-lookup"><span data-stu-id="bc72b-124">**Add** the extension to your browser.</span></span>

5.  <span data-ttu-id="bc72b-125">Ha a böngésző, válassza ki vagy **engedélyezése** vagy **engedélyezése** a bővítményt.</span><span class="sxs-lookup"><span data-stu-id="bc72b-125">If your browser asks, select to either **Enable** or **Allow** the extension.</span></span>

6.  <span data-ttu-id="bc72b-126">Korábban telepítve, **indítsa újra a** a böngésző-munkamenetet.</span><span class="sxs-lookup"><span data-stu-id="bc72b-126">Once installed, **restart** your browser session.</span></span>

7.  <span data-ttu-id="bc72b-127">Jelentkezzen be a hozzáférési panelre, és tekintse meg, ha **indítása** a jelszó-egyszeri bejelentkezés alkalmazásokhoz</span><span class="sxs-lookup"><span data-stu-id="bc72b-127">Sign in into the Access Panel and see if you can **launch** your password-SSO applications</span></span>

<span data-ttu-id="bc72b-128">Az alábbi közvetlen hivatkozások közül a Chrome és Firefox is letöltheti a bővítmény:</span><span class="sxs-lookup"><span data-stu-id="bc72b-128">You may also download the extension for Chrome and Firefox from the direct links below:</span></span>

-   [<span data-ttu-id="bc72b-129">Chrome hozzáférési Panel bővítmény</span><span class="sxs-lookup"><span data-stu-id="bc72b-129">Chrome Access Panel Extension</span></span>](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [<span data-ttu-id="bc72b-130">A Firefox hozzáférési Panel bővítmény</span><span class="sxs-lookup"><span data-stu-id="bc72b-130">Firefox Access Panel Extension</span></span>](https://addons.mozilla.org/firefox/addon/access-panel-extension/)

## <a name="setting-up-a-group-policy-for-internet-explorer"></a><span data-ttu-id="bc72b-131">A csoportházirend beállítása az Internet Explorerben</span><span class="sxs-lookup"><span data-stu-id="bc72b-131">Setting up a group policy for Internet Explorer</span></span>

<span data-ttu-id="bc72b-132">A csoportházirend lehetővé tevő távoli telepítését a felhasználók gépeken az Internet Explorer a hozzáférési Panel bővítmény beállítása.</span><span class="sxs-lookup"><span data-stu-id="bc72b-132">You can setup a group policy that allow you to remotely install the Access Panel extension for Internet Explorer on your users' machines.</span></span>

<span data-ttu-id="bc72b-133">Az Előfeltételek a következők:</span><span class="sxs-lookup"><span data-stu-id="bc72b-133">The prerequisites include:</span></span>

-   <span data-ttu-id="bc72b-134">Ezzel beállította [Active Directory tartományi szolgáltatások](https://msdn.microsoft.com/library/aa362244%28v=vs.85%29.aspx), és a felhasználók gépek csatlakozott a tartományhoz.</span><span class="sxs-lookup"><span data-stu-id="bc72b-134">You have set up [Active Directory Domain Services](https://msdn.microsoft.com/library/aa362244%28v=vs.85%29.aspx), and you have joined your users' machines to your domain.</span></span>

-   <span data-ttu-id="bc72b-135">A csoportházirend-objektum (GPO) szerkesztése "Beállítások módosítása" engedéllyel kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="bc72b-135">You must have the "Edit settings" permission to edit the Group Policy Object (GPO).</span></span> <span data-ttu-id="bc72b-136">Alapértelmezés szerint a következő biztonsági csoportok tagjai ezzel az engedéllyel rendelkeznek: a tartományi rendszergazdák, a vállalati rendszergazdák és a Csoportházirend-létrehozó tulajdonosok.</span><span class="sxs-lookup"><span data-stu-id="bc72b-136">By default, members of the following security groups have this permission: Domain Administrators, Enterprise Administrators, and Group Policy Creator Owners.</span></span> <span data-ttu-id="bc72b-137">[További információk](https://technet.microsoft.com/library/cc781991%28v=ws.10%29.aspx).</span><span class="sxs-lookup"><span data-stu-id="bc72b-137">[Learn more](https://technet.microsoft.com/library/cc781991%28v=ws.10%29.aspx).</span></span>

<span data-ttu-id="bc72b-138">Az útmutató bevezeti Önt [a hozzáférési Panel bővítmény telepítése csoportházirend használatával az Internet Explorer](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ie-group-policy) részletes útmutatást a csoportházirend konfigurálásához, és telepítheti azt felhasználók számára.</span><span class="sxs-lookup"><span data-stu-id="bc72b-138">Follow the tutorial [How to Deploy the Access Panel Extension for Internet Explorer using Group Policy](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ie-group-policy) for step by step instructions on how to configure the group policy and deploy it to users.</span></span>

## <a name="troubleshoot-the-access-panel-in-internet-explorer"></a><span data-ttu-id="bc72b-139">A hozzáférési Panel az Internet Explorer hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="bc72b-139">Troubleshoot the Access Panel in Internet Explorer</span></span>

<span data-ttu-id="bc72b-140">Kövesse a [hibaelhárítása a hozzáférési Panel bővítményét az Internet Explorer](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ie-Troubleshoot) útmutatót a hozzáféréshez olyan diagnosztikai eszköz és részletes útmutatást tartalmaz az a bővítmény konfigurálása az Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="bc72b-140">Follow the [Troubleshoot the Access Panel Extension for Internet Explorer](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ie-Troubleshoot) guide for access a diagnostics tool and step by step instructions on configuring the extension for IE.</span></span>

## <a name="how-to-configure-password-single-sign-on-for-a-non-gallery-application"></a><span data-ttu-id="bc72b-141">Jelszó egyszeri bejelentkezés nem galéria alkalmazások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="bc72b-141">How to configure password single sign-on for a non-gallery application</span></span>

<span data-ttu-id="bc72b-142">Egy alkalmazás kell az Azure AD-galériából konfigurálása:</span><span class="sxs-lookup"><span data-stu-id="bc72b-142">To configure an application from the Azure AD gallery you need to:</span></span>

-   [<span data-ttu-id="bc72b-143">Nem galéria alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="bc72b-143">Add a non-gallery application</span></span>](#add-a-non-gallery-application)

-   [<span data-ttu-id="bc72b-144">Állítsa be az alkalmazását jelszó egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="bc72b-144">Configure the application for password single sign-on</span></span>](#configure-the-application-for-password-single-sign-on)

-   [<span data-ttu-id="bc72b-145">Az alkalmazás felhasználók hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="bc72b-145">Assign users to the application</span></span>](#assign-users-to-the-application)

### <a name="add-a-non-gallery-application"></a><span data-ttu-id="bc72b-146">Nem galéria alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="bc72b-146">Add a non-gallery application</span></span>

<span data-ttu-id="bc72b-147">Hozzáadhat egy alkalmazást az Azure AD-gyűjteményből, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="bc72b-147">To add an application from the Azure AD Gallery, follow the steps below:</span></span>

1.  <span data-ttu-id="bc72b-148">Nyissa meg a [Azure Portal](https://portal.azure.com) , és jelentkezzen be egy **globális rendszergazda** vagy **társadminisztrátornak**</span><span class="sxs-lookup"><span data-stu-id="bc72b-148">Open the [Azure Portal](https://portal.azure.com) and sign in as a **Global Administrator** or **Co-admin**</span></span>

2.  <span data-ttu-id="bc72b-149">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="bc72b-149">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="bc72b-150">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="bc72b-150">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="bc72b-151">Kattintson a **vállalati alkalmazások** az Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="bc72b-151">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="bc72b-152">Kattintson a **Hozzáadás** gombra a jobb felső sarkában a **vállalati alkalmazások** panel</span><span class="sxs-lookup"><span data-stu-id="bc72b-152">click the **Add** button at the top-right corner on the **Enterprise Applications** blade</span></span>

6.  <span data-ttu-id="bc72b-153">Kattintson a **nem-gyűjtemény alkalmazás.**</span><span class="sxs-lookup"><span data-stu-id="bc72b-153">click **Non-gallery application.**</span></span>

7.  <span data-ttu-id="bc72b-154">Adja meg az alkalmazás nevét a **neve** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="bc72b-154">Enter the name of your application in the **Name** textbox.</span></span> <span data-ttu-id="bc72b-155">Válassza ki **hozzáadása.**</span><span class="sxs-lookup"><span data-stu-id="bc72b-155">Select **Add.**</span></span>

<span data-ttu-id="bc72b-156">Egy rövid időszak után el az alkalmazás konfigurációs panelen láthatók.</span><span class="sxs-lookup"><span data-stu-id="bc72b-156">After a short period, you be able to see the application’s configuration blade.</span></span>

### <a name="configure-the-application-for-password-single-sign-on"></a><span data-ttu-id="bc72b-157">Állítsa be az alkalmazását jelszó egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="bc72b-157">Configure the application for password single sign-on</span></span>

<span data-ttu-id="bc72b-158">Egyszeri bejelentkezés egy alkalmazás konfigurálásához kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="bc72b-158">To configure single sign-on for an application, follow the steps below:</span></span>

1.  <span data-ttu-id="bc72b-159">Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="bc72b-159">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="bc72b-160">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="bc72b-160">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="bc72b-161">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="bc72b-161">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="bc72b-162">Kattintson a **vállalati alkalmazások** az Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="bc72b-162">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="bc72b-163">Kattintson a **összes alkalmazás** az alkalmazások listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="bc72b-163">click **All Applications** to view a list of all your applications.</span></span>

   * <span data-ttu-id="bc72b-164">Ha azt szeretné, hogy itt megjelennek az alkalmazás nem látja, használja a **szűrő** vezérlő tetején a **összes alkalmazások listáját** és állítsa be a **megjelenítése** lehetőséggel **összes alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="bc72b-164">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="bc72b-165">Válassza ki az egyszeri bejelentkezés konfigurálni kívánt alkalmazást</span><span class="sxs-lookup"><span data-stu-id="bc72b-165">Select the application you want to configure single sign-on</span></span>

7.  <span data-ttu-id="bc72b-166">Ha az alkalmazás betölt, kattintson a **egyszeri bejelentkezés** az alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="bc72b-166">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="bc72b-167">Válassza ki a módot **jelszóalapú bejelentkezés.**</span><span class="sxs-lookup"><span data-stu-id="bc72b-167">Select the mode **Password-based Sign-on.**</span></span>

9.  <span data-ttu-id="bc72b-168">Adja meg a **bejelentkezési URL-cím**.</span><span class="sxs-lookup"><span data-stu-id="bc72b-168">Enter the **Sign-on URL**.</span></span> <span data-ttu-id="bc72b-169">Ez az URL-CÍMÉT ahol adja meg a felhasználók a felhasználónevével és jelszavával bejelentkezni.</span><span class="sxs-lookup"><span data-stu-id="bc72b-169">This is the URL where users enter their username and password to sign in to.</span></span> <span data-ttu-id="bc72b-170">Győződjön meg arról, a mezők bejelentkezési URL-címen láthatók.</span><span class="sxs-lookup"><span data-stu-id="bc72b-170">Ensure the sign in fields are visible at the URL.</span></span>

10. <span data-ttu-id="bc72b-171">Felhasználók hozzárendelése az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="bc72b-171">Assign users to the application.</span></span>

11. <span data-ttu-id="bc72b-172">Emellett is megadhatja a felhasználó nevében hitelesítő adatok a felhasználók a sorok kijelöléséhez és parancsával **frissítéséhez szükséges hitelesítő adatokat** , majd gépelje be a felhasználónevet és jelszót a felhasználók nevében.</span><span class="sxs-lookup"><span data-stu-id="bc72b-172">Additionally, you can also provide credentials on behalf of the user by selecting the rows of the users and clicking on **Update Credentials** and entering the username and password on behalf of the users.</span></span> <span data-ttu-id="bc72b-173">Ellenkező esetben megkérdezi a felhasználókat a hitelesítő adatok magukat az indítás után.</span><span class="sxs-lookup"><span data-stu-id="bc72b-173">Otherwise, users be prompted to enter the credentials themselves upon launch.</span></span>

### <a name="assign-users-to-the-application"></a><span data-ttu-id="bc72b-174">Az alkalmazás felhasználók hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="bc72b-174">Assign users to the application</span></span>

<span data-ttu-id="bc72b-175">Hozzárendelése egy vagy több felhasználó alkalmazás közvetlenül, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="bc72b-175">To assign one or more users to an application directly, follow the steps below:</span></span>

1.  <span data-ttu-id="bc72b-176">Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="bc72b-176">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="bc72b-177">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="bc72b-177">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="bc72b-178">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="bc72b-178">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="bc72b-179">Kattintson a **vállalati alkalmazások** az Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="bc72b-179">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="bc72b-180">Kattintson a **összes alkalmazás** az alkalmazások listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="bc72b-180">click **All Applications** to view a list of all your applications.</span></span>

   * <span data-ttu-id="bc72b-181">Ha azt szeretné, hogy itt megjelennek az alkalmazás nem látja, használja a **szűrő** vezérlő tetején a **összes alkalmazások listáját** és állítsa be a **megjelenítése** lehetőséggel **összes alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="bc72b-181">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="bc72b-182">Válassza ki szeretné osztani a felhasználót, hogy a listában az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="bc72b-182">Select the application you want to assign a user to from the list.</span></span>

7.  <span data-ttu-id="bc72b-183">Ha az alkalmazás betölt, kattintson **felhasználók és csoportok** az alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="bc72b-183">Once the application loads, click **Users and Groups** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="bc72b-184">Kattintson a **Hozzáadás** gombra kattint, a a **felhasználók és csoportok** nyissa meg a listában a **hozzáadása hozzárendelés** panelen.</span><span class="sxs-lookup"><span data-stu-id="bc72b-184">Click the **Add** button on top of the **Users and Groups** list to open the **Add Assignment** blade.</span></span>

9.  <span data-ttu-id="bc72b-185">Kattintson a **felhasználók és csoportok** a választó a **hozzáadása hozzárendelés** panelen.</span><span class="sxs-lookup"><span data-stu-id="bc72b-185">click the **Users and groups** selector from the **Add Assignment** blade.</span></span>

10. <span data-ttu-id="bc72b-186">Írja be a **teljes név** vagy **e-mail cím** érdekli hozzárendelése a felhasználó a **Keresés név vagy e-mail cím alapján** keresőmezőbe.</span><span class="sxs-lookup"><span data-stu-id="bc72b-186">Type in the **full name** or **email address** of the user you are interested in assigning into the **Search by name or email address** search box.</span></span>

11. <span data-ttu-id="bc72b-187">Vigye a **felhasználói** a listában, hogy láthatóvá váljon a **jelölőnégyzet**.</span><span class="sxs-lookup"><span data-stu-id="bc72b-187">Hover over the **user** in the list to reveal a **checkbox**.</span></span> <span data-ttu-id="bc72b-188">A felhasználói profil fénykép vagy adja hozzá a felhasználót emblémát jelölőnégyzetét, kattintson a **kijelölt** listája.</span><span class="sxs-lookup"><span data-stu-id="bc72b-188">Click the checkbox next to the user’s profile photo or logo to add your user to the **Selected** list.</span></span>

12. <span data-ttu-id="bc72b-189">**Választható lehetőség:** Ha azt szeretné, hogy **egynél több felhasználó hozzáadása**, egy másik típus **teljes név** vagy **e-mail cím** be a **Keresés név vagy e-mail cím alapján** mező, és a jelölőnégyzet bejelölésével adja hozzá a felhasználót, hogy a **kijelölt** listája.</span><span class="sxs-lookup"><span data-stu-id="bc72b-189">**Optional:** If you would like to **add more than one user**, type in another **full name** or **email address** into the **Search by name or email address** search box, and click the checkbox to add this user to the **Selected** list.</span></span>

13. <span data-ttu-id="bc72b-190">Ha elkészült, válassza a felhasználók, kattintson a **válasszon** gombra kattintva vegye fel a listára a felhasználók és csoportok hozzá kell rendelni az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="bc72b-190">When you are finished selecting users, click the **Select** button to add them to the list of users and groups to be assigned to the application.</span></span>

14. <span data-ttu-id="bc72b-191">**Választható lehetőség:** kattintson a **Szerepkörválasztás** a választó a **hozzáadása hozzárendelés** hozzárendelése a kiválasztott felhasználói szerepkör kiválasztása panel.</span><span class="sxs-lookup"><span data-stu-id="bc72b-191">**Optional:** click the **Select Role** selector in the **Add Assignment** blade to select a role to assign to the users you have selected.</span></span>

15. <span data-ttu-id="bc72b-192">Kattintson a **hozzárendelése** gombra kattintva a kijelölt felhasználók az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="bc72b-192">Click the **Assign** button to assign the application to the selected users.</span></span>

<span data-ttu-id="bc72b-193">Rövid ideig a kijelölt felhasználók tudják elindítani ezeket az alkalmazásokat a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="bc72b-193">After a short period, the users you have selected be able to launch these applications in the Access Panel.</span></span>

## <a name="if-these-troubleshooting-steps-do-not-the-resolve-the-issue"></a><span data-ttu-id="bc72b-194">Ha ezek a hibaelhárítási lépéseket nem a hárítsa el a problémát</span><span class="sxs-lookup"><span data-stu-id="bc72b-194">If these troubleshooting steps do not the resolve the issue</span></span>

<span data-ttu-id="bc72b-195">támogatási jegy megnyitása a következő információkat, ha rendelkezésre áll:</span><span class="sxs-lookup"><span data-stu-id="bc72b-195">open a support ticket with the following information if available:</span></span>

-   <span data-ttu-id="bc72b-196">Megfelelési hiba azonosítója</span><span class="sxs-lookup"><span data-stu-id="bc72b-196">Correlation error ID</span></span>

-   <span data-ttu-id="bc72b-197">Egyszerű felhasználónév (felhasználó e-mail címe)</span><span class="sxs-lookup"><span data-stu-id="bc72b-197">UPN (user email address)</span></span>

-   <span data-ttu-id="bc72b-198">A TenantID</span><span class="sxs-lookup"><span data-stu-id="bc72b-198">TenantID</span></span>

-   <span data-ttu-id="bc72b-199">Böngésző típusa</span><span class="sxs-lookup"><span data-stu-id="bc72b-199">Browser type</span></span>

-   <span data-ttu-id="bc72b-200">Időzóna és idő/időkeretre során hiba történik.</span><span class="sxs-lookup"><span data-stu-id="bc72b-200">Time zone and time/timeframe during error occurs</span></span>

-   <span data-ttu-id="bc72b-201">Fiddler nyomkövetések</span><span class="sxs-lookup"><span data-stu-id="bc72b-201">Fiddler traces</span></span>

## <a name="next-steps"></a><span data-ttu-id="bc72b-202">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="bc72b-202">Next steps</span></span>
[<span data-ttu-id="bc72b-203">Adja meg az egyszeri bejelentkezés az alkalmazásokba a Proxy</span><span class="sxs-lookup"><span data-stu-id="bc72b-203">Provide single sign-on to your apps with Application Proxy</span></span>](active-directory-application-proxy-sso-using-kcd.md)

