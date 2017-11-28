---
title: "hello alkalmazás hozzáférési panel bővítmény telepítése aaaProblem |} Microsoft Docs"
description: "Hogyan toofix előforduló hibákat észlelt, amikor hello hozzáférési panel bővítmény telepítése"
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
ms.reviewer: japere
ms.openlocfilehash: 5f750d12c5f9b405ec4f81596d5cc5e0a48f9a62
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="problem-installing-hello-application-access-panel-browser-extension"></a><span data-ttu-id="90af6-103">A probléma hello alkalmazás hozzáférési panel bővítmény telepítése</span><span class="sxs-lookup"><span data-stu-id="90af6-103">Problem installing hello application access panel browser extension</span></span>

<span data-ttu-id="90af6-104">Hozzáférési Panel hello egy webes portál, mely lehetővé teszi, hogy egy felhasználó, aki rendelkezik a munkahelyi vagy iskolai fiókot az Azure Active Directory (Azure AD) tooview, és indítsa el felhőalapú alkalmazások, hogy az Azure AD hello rendszergazda engedélyezte őket a hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="90af6-104">hello Access Panel is a web-based portal which enables a user who has a work or school account in Azure Active Directory (Azure AD) tooview and launch cloud-based applications that hello Azure AD administrator has granted them access to.</span></span> <span data-ttu-id="90af6-105">Egy felhasználó, aki rendelkezik az Azure AD-verziók is használhatja, önkiszolgáló csoportkezelési és a hozzáférési Panel hello keresztül kezelési képességeit.</span><span class="sxs-lookup"><span data-stu-id="90af6-105">A user who has Azure AD editions can also use self-service group and app management capabilities through hello Access Panel.</span></span> <span data-ttu-id="90af6-106">Hozzáférési Panel hello elkülönül hello Azure-portálon, és nem igényli a felhasználók toohave Azure-előfizetéssel.</span><span class="sxs-lookup"><span data-stu-id="90af6-106">hello Access Panel is separate from hello Azure portal and does not require users toohave an Azure subscription.</span></span>

<span data-ttu-id="90af6-107">toouse jelszó-alapú egyszeri bejelentkezést (SSO) a hozzáférési Panel, a hozzáférési Panel bővítmény hello hello telepíteni kell hello felhasználó böngészőjében.</span><span class="sxs-lookup"><span data-stu-id="90af6-107">toouse password-based single sign-on (SSO) in hello Access Panel, hello Access Panel extension must be installed in hello user’s browser.</span></span> <span data-ttu-id="90af6-108">A bővítmény le automatikusan, ha a felhasználó megadja egy alkalmazás, amely jelszóalapú SSO van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="90af6-108">This extension is downloaded automatically when a user selects an application that is configured for password-based SSO.</span></span>

## <a name="meeting-browser-requirements-for-hello-access-panel"></a><span data-ttu-id="90af6-109">A hozzáférési Panel hello értekezlet böngészőre vonatkozó követelményei</span><span class="sxs-lookup"><span data-stu-id="90af6-109">Meeting browser requirements for hello Access Panel</span></span>

<span data-ttu-id="90af6-110">hello hozzáférési Panel igényel, amely támogatja a JavaScript egy böngészőt, és CSS engedélyezte.</span><span class="sxs-lookup"><span data-stu-id="90af6-110">hello Access Panel requires a browser that supports JavaScript and has CSS enabled.</span></span> <span data-ttu-id="90af6-111">toouse jelszó-alapú egyszeri bejelentkezést (SSO) a hozzáférési Panel, a hozzáférési Panel bővítmény hello hello telepíteni kell hello felhasználó böngészőjében.</span><span class="sxs-lookup"><span data-stu-id="90af6-111">toouse password-based single sign-on (SSO) in hello Access Panel, hello Access Panel extension must be installed in hello user’s browser.</span></span> <span data-ttu-id="90af6-112">A bővítmény le automatikusan, ha a felhasználó megadja egy alkalmazás, amely jelszóalapú SSO van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="90af6-112">This extension is downloaded automatically when a user selects an application that is configured for password-based SSO.</span></span>

<span data-ttu-id="90af6-113">Az egyszeri bejelentkezés jelszóalapú hello végfelhasználó böngészőkkel lehet:</span><span class="sxs-lookup"><span data-stu-id="90af6-113">For password-based SSO, hello end user’s browsers can be:</span></span>

-   <span data-ttu-id="90af6-114">Internet Explorer 8, 9, 10, 11 – a Windows 7 vagy újabb</span><span class="sxs-lookup"><span data-stu-id="90af6-114">Internet Explorer 8, 9, 10, 11 -- on Windows 7 or later</span></span>

-   <span data-ttu-id="90af6-115">Peremhálózati Windows 10 évforduló Edition vagy újabb</span><span class="sxs-lookup"><span data-stu-id="90af6-115">Edge on Windows 10 Anniversary Edition or later</span></span> 

-   <span data-ttu-id="90af6-116">Chrome – A Windows 7 vagy újabb, és MacOS X rendszeren vagy újabb</span><span class="sxs-lookup"><span data-stu-id="90af6-116">Chrome -- on Windows 7 or later, and on MacOS X or later</span></span>

-   <span data-ttu-id="90af6-117">Firefox 26.0 vagy újabb – a Windows XP SP2 vagy újabb, és a Mac OS X 10,6 vagy újabb verzió</span><span class="sxs-lookup"><span data-stu-id="90af6-117">Firefox 26.0 or later -- on Windows XP SP2 or later, and on Mac OS X 10.6 or later</span></span>

## <a name="how-tooinstall-hello-access-panel-browser-extension"></a><span data-ttu-id="90af6-118">Hogyan tooinstall hello hozzáférési Panel bővítmény</span><span class="sxs-lookup"><span data-stu-id="90af6-118">How tooinstall hello Access Panel Browser extension</span></span>

<span data-ttu-id="90af6-119">tooinstall hello hozzáférési Panel bővítmény, kövesse az alábbi hello lépéseket:</span><span class="sxs-lookup"><span data-stu-id="90af6-119">tooinstall hello Access Panel Browser extension, follow hello steps below:</span></span>

1.  <span data-ttu-id="90af6-120">Nyissa meg hello [hozzáférési Panel](https://myapps.microsoft.com) valamelyik hello támogatott böngészők és való bejelentkezést egy **felhasználói** az Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="90af6-120">Open hello [Access Panel](https://myapps.microsoft.com) in one of hello supported browsers and sign in as a **user** in your Azure AD.</span></span>

2.  <span data-ttu-id="90af6-121">Kattintson egy **jelszó-SSO alkalmazás** a hozzáférési Panel hello.</span><span class="sxs-lookup"><span data-stu-id="90af6-121">Click a **password-SSO application** in hello Access Panel.</span></span>

3.  <span data-ttu-id="90af6-122">Hello Rákérdezés azzal a kérdéssel tooinstall hello szoftver, válassza ki **telepítés**.</span><span class="sxs-lookup"><span data-stu-id="90af6-122">In hello prompt asking tooinstall hello software, select **Install Now**.</span></span>

4.  <span data-ttu-id="90af6-123">A böngésző alapján kell irányított toohello letöltési hivatkozását.</span><span class="sxs-lookup"><span data-stu-id="90af6-123">Based on your browser you be directed toohello download link.</span></span> <span data-ttu-id="90af6-124">**Adja hozzá** hello bővítmény tooyour böngésző.</span><span class="sxs-lookup"><span data-stu-id="90af6-124">**Add** hello extension tooyour browser.</span></span>

5.  <span data-ttu-id="90af6-125">Ha a böngésző kéri, válassza ki a tooeither **engedélyezése** vagy **engedélyezése** hello bővítmény.</span><span class="sxs-lookup"><span data-stu-id="90af6-125">If your browser asks, select tooeither **Enable** or **Allow** hello extension.</span></span>

6.  <span data-ttu-id="90af6-126">Korábban telepítve, **indítsa újra a** a böngésző-munkamenetet.</span><span class="sxs-lookup"><span data-stu-id="90af6-126">Once installed, **restart** your browser session.</span></span>

7.  <span data-ttu-id="90af6-127">Jelentkezzen be a hozzáférési Panel hello, és tekintse meg, ha **indítása** a jelszó-egyszeri bejelentkezés alkalmazásokhoz</span><span class="sxs-lookup"><span data-stu-id="90af6-127">Sign in into hello Access Panel and see if you can **launch** your password-SSO applications</span></span>

<span data-ttu-id="90af6-128">Az alábbi hello közvetlen hivatkozások Chrome és a peremhálózati is letöltheti hello bővítmény:</span><span class="sxs-lookup"><span data-stu-id="90af6-128">You may also download hello extension for Chrome and Edge from hello direct links below:</span></span>

-   [<span data-ttu-id="90af6-129">Chrome hozzáférési Panel bővítmény</span><span class="sxs-lookup"><span data-stu-id="90af6-129">Chrome Access Panel Extension</span></span>](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [<span data-ttu-id="90af6-130">Peremhálózati hozzáférési Panel bővítmény</span><span class="sxs-lookup"><span data-stu-id="90af6-130">Edge Access Panel Extension</span></span>](https://www.microsoft.com/store/apps/9pc9sckkzk84) 

## <a name="setting-up-a-group-policy-for-internet-explorer"></a><span data-ttu-id="90af6-131">A csoportházirend beállítása az Internet Explorerben</span><span class="sxs-lookup"><span data-stu-id="90af6-131">Setting up a group policy for Internet Explorer</span></span>

<span data-ttu-id="90af6-132">Beállíthatja a csoportházirend, amelyek lehetővé teszik tooremotely telepítés hello hozzáférési Panel bővítményt az Internet Explorer a felhasználók gépeken.</span><span class="sxs-lookup"><span data-stu-id="90af6-132">You can setup a group policy that allow you tooremotely install hello Access Panel extension for Internet Explorer on your users' machines.</span></span>

<span data-ttu-id="90af6-133">hello Előfeltételek a következők:</span><span class="sxs-lookup"><span data-stu-id="90af6-133">hello prerequisites include:</span></span>

-   <span data-ttu-id="90af6-134">Ezzel beállította [Active Directory tartományi szolgáltatások](https://msdn.microsoft.com/library/aa362244%28v=vs.85%29.aspx), és a felhasználók gépek tooyour tartományhoz csatlakozott.</span><span class="sxs-lookup"><span data-stu-id="90af6-134">You have set up [Active Directory Domain Services](https://msdn.microsoft.com/library/aa362244%28v=vs.85%29.aspx), and you have joined your users' machines tooyour domain.</span></span>

-   <span data-ttu-id="90af6-135">Hello "Beállítások módosítása" engedéllyel tooedit hello csoportházirend-objektumot (GPO) kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="90af6-135">You must have hello "Edit settings" permission tooedit hello Group Policy Object (GPO).</span></span> <span data-ttu-id="90af6-136">Alapértelmezés szerint a következő biztonsági csoportokat hello tagjai rendelkezik ilyen engedéllyel: tartományi rendszergazdák, a vállalati rendszergazdák és a Csoportházirend-létrehozó tulajdonosok.</span><span class="sxs-lookup"><span data-stu-id="90af6-136">By default, members of hello following security groups have this permission: Domain Administrators, Enterprise Administrators, and Group Policy Creator Owners.</span></span> <span data-ttu-id="90af6-137">[További információk](https://technet.microsoft.com/library/cc781991%28v=ws.10%29.aspx).</span><span class="sxs-lookup"><span data-stu-id="90af6-137">[Learn more](https://technet.microsoft.com/library/cc781991%28v=ws.10%29.aspx).</span></span>

<span data-ttu-id="90af6-138">Hello az oktatóanyagot követve [hogyan tooDeploy hello csoportházirend használatával az Internet Explorer hozzáférési Panel bővítmény](active-directory-saas-ie-group-policy.md) hogyan tooconfigure hello csoportházirend, majd központilag telepítenie toousers részletes információkra van szüksége.</span><span class="sxs-lookup"><span data-stu-id="90af6-138">Follow hello tutorial [How tooDeploy hello Access Panel Extension for Internet Explorer using Group Policy](active-directory-saas-ie-group-policy.md) for step by step instructions on how tooconfigure hello group policy and deploy it toousers.</span></span>

## <a name="troubleshoot-hello-access-panel-in-internet-explorer"></a><span data-ttu-id="90af6-139">Az Internet Explorerben a hozzáférési Panel hello hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="90af6-139">Troubleshoot hello Access Panel in Internet Explorer</span></span>

<span data-ttu-id="90af6-140">Hajtsa végre a hello [kapcsolatos problémák elhárítása hello hozzáférési Panel bővítményét az Internet Explorer](active-directory-saas-ie-troubleshooting.md) útmutatót a hozzáféréshez olyan diagnosztikai eszköz és részletes utasításokat az hello bővítmény konfigurálása az Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="90af6-140">Follow hello [Troubleshoot hello Access Panel Extension for Internet Explorer](active-directory-saas-ie-troubleshooting.md) guide for access a diagnostics tool and step by step instructions on configuring hello extension for IE.</span></span>

## <a name="if-these-troubleshooting-steps-do-not-resolve-hello-issue"></a><span data-ttu-id="90af6-141">Ha ezek a hibaelhárítási lépéseket nem oldható meg hello probléma</span><span class="sxs-lookup"><span data-stu-id="90af6-141">If these troubleshooting steps do not resolve hello issue</span></span>

<span data-ttu-id="90af6-142">Nyisson meg egy támogatási jegy hello ha rendelkezésre áll a következő információkat:</span><span class="sxs-lookup"><span data-stu-id="90af6-142">open a support ticket with hello following information if available:</span></span>

-   <span data-ttu-id="90af6-143">Megfelelési hiba azonosítója</span><span class="sxs-lookup"><span data-stu-id="90af6-143">Correlation error ID</span></span>

-   <span data-ttu-id="90af6-144">Egyszerű felhasználónév (felhasználó e-mail címe)</span><span class="sxs-lookup"><span data-stu-id="90af6-144">UPN (user email address)</span></span>

-   <span data-ttu-id="90af6-145">A TenantID</span><span class="sxs-lookup"><span data-stu-id="90af6-145">TenantID</span></span>

-   <span data-ttu-id="90af6-146">Böngésző típusa</span><span class="sxs-lookup"><span data-stu-id="90af6-146">Browser type</span></span>

-   <span data-ttu-id="90af6-147">Időzóna és idő/időkeretre során hiba történik.</span><span class="sxs-lookup"><span data-stu-id="90af6-147">Time zone and time/timeframe during error occurs</span></span>

-   <span data-ttu-id="90af6-148">Fiddler nyomkövetések</span><span class="sxs-lookup"><span data-stu-id="90af6-148">Fiddler traces</span></span>

## <a name="next-steps"></a><span data-ttu-id="90af6-149">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="90af6-149">Next steps</span></span>
[<span data-ttu-id="90af6-150">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="90af6-150">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
