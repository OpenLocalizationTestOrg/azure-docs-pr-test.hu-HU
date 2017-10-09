---
title: "aaaHow tooconfigure jelszó egyszeri bejelentkezés egy nem galéria applicationn a(z) |} Microsoft Docs"
description: "Hogyan tooconfigure egyéni nem galéria alkalmazás biztonságos jelszó-alapú egyszeri bejelentkezést Ha nem szerepel az Azure AD Application Gallery hello"
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
ms.openlocfilehash: e3d0e658f792a0a63110a198811edc66acd6ccf4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-password-single-sign-on-for-a-non-gallery-application"></a><span data-ttu-id="76cd5-103">Hogyan tooconfigure jelszó egyszeri bejelentkezés egy nem galéria alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="76cd5-103">How tooconfigure password single sign-on for a non-gallery application</span></span>

<span data-ttu-id="76cd5-104">Ezenkívül toohello lehetőségek található belül hello Azure AD Application Gallery, akkor is hello beállítás tooadd egy **nem galéria alkalmazás** amikor hello kívánt alkalmazás nem szerepel ott.</span><span class="sxs-lookup"><span data-stu-id="76cd5-104">In addition toohello choices found within hello Azure AD Application Gallery, you also have hello option tooadd a **non-gallery application** when hello application you want is not listed there.</span></span> <span data-ttu-id="76cd5-105">Használja ezt a képességet, hozzáadhat bármely alkalmazás, amely a szervezet már létezik, vagy bármely alkalmazás, amelyet esetleg olyan szállítótól, akik már nem része a hello [az Azure AD Application Gallery](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#get-started-with-the-azure-ad-application-gallery).</span><span class="sxs-lookup"><span data-stu-id="76cd5-105">Using this capability, you can add any application that already exists in your organization, or any third-party application that you might use from a vendor who is not already part of hello [Azure AD Application Gallery](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#get-started-with-the-azure-ad-application-gallery).</span></span>

<span data-ttu-id="76cd5-106">Egy nem galéria alkalmazás felvétele után is konfigurálhat hello egyszeri bejelentkezés az alkalmazás által használt módszer hello kiválasztásával **egyszeri bejelentkezés** navigációs elem a hello a vállalati alkalmazás [Azure portál ](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="76cd5-106">Once you add a non-gallery application, you can then configure hello Single sign-on method this application uses by selecting hello **Single Sign-on** navigation item on an Enterprise Application in hello [Azure Portal](https://portal.azure.com/).</span></span>

<span data-ttu-id="76cd5-107">Hello egyszeri bejelentkezés módszer elérhető tooyou egyik hello [jelszó-alapú egyszeri bejelentkezést](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="76cd5-107">One of hello Single Sign-on methods available tooyou is hello [Password-Based Single Sign-on](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) option.</span></span> <span data-ttu-id="76cd5-108">A hello **nem galéria alkalmazás hozzáadása** élményt, egy HTML-alapú felhasználónév Renderelés bármely olyan alkalmazás integrálható, és a jelszó bemeneti mező, még akkor is, ha nincs-e az előre integrált alkalmazások csoportját.</span><span class="sxs-lookup"><span data-stu-id="76cd5-108">With hello **add a non-gallery application** experience, you can integrate any application that renders an HTML-based username and password input field, even if it is not in our set of pre-integrated applications.</span></span>

<span data-ttu-id="76cd5-109">Ez a módszer hello úgy, hogy a technológia, amely része hello lekaparást oldal hozzáférési Panel-bővítménnyel, amely lehetővé teszi tooauto-felhasználónév és jelszó bemeneti mező észleléséhez, tárolja őket biztonságos helyen az adott alkalmazáspéldány esetében.</span><span class="sxs-lookup"><span data-stu-id="76cd5-109">hello way this works is by a page scraping technology that is part of hello Access Panel extension that allows us tooauto-detect username and password input fields, store them securely for your specific application instance.</span></span> <span data-ttu-id="76cd5-110">Biztonságosan visszajátszásos felhasználónevek és jelszavak toothose mezők a felhasználó navigál toothat alkalmazás hello alkalmazás-hozzáférési panelre.</span><span class="sxs-lookup"><span data-stu-id="76cd5-110">Then securely replay usernames and passwords toothose fields when a user navigates toothat application on hello application access panel.</span></span>

<span data-ttu-id="76cd5-111">Ez az tooget bármilyen alkalmazás integrálása az Azure AD gyorsan elindult, és lehetővé teszi kiváló módja:</span><span class="sxs-lookup"><span data-stu-id="76cd5-111">This is a great way tooget started integrating any kind of application into Azure AD quickly, and allows you to:</span></span>

-   <span data-ttu-id="76cd5-112">Integrálni **hello world bármely alkalmazás** az Azure AD-bérlőn, akkor ez a beállítás egy HTML felhasználónév és jelszó beviteli mezőt, amennyiben</span><span class="sxs-lookup"><span data-stu-id="76cd5-112">Integrate **any application in hello world** with your Azure AD tenant, so long as it renders an HTML username and password input field</span></span>

-   <span data-ttu-id="76cd5-113">Engedélyezése **egyszeri bejelentkezéshez a felhasználók** biztonságosan tárolja, és felhasználónevek és jelszavak hello alkalmazás visszajátszását integrált már az Azure ad-vel</span><span class="sxs-lookup"><span data-stu-id="76cd5-113">Enable **Single Sign-on for your users** by securely storing and replaying usernames and passwords for hello application you’ve integrated with Azure AD</span></span>

-   <span data-ttu-id="76cd5-114">**Az automatikus észlelés bemeneti** mezők bármely alkalmazás, és lehetővé teszik toomanually észleli azokat a mezőket hello hozzáférési Panel bővítmény, abban az esetben, ha automatikus észlelés nem találja meg őket</span><span class="sxs-lookup"><span data-stu-id="76cd5-114">**Auto-detect input** fields for any application and allow you toomanually detect those fields using hello Access Panel Browser Extension, in case auto-detection does not find them</span></span>

-   <span data-ttu-id="76cd5-115">**Több bejelentkezési mező igénylő alkalmazások támogatását** csupán felhasználónévvel és jelszóval mezők toosign a igénylő alkalmazások</span><span class="sxs-lookup"><span data-stu-id="76cd5-115">**Support applications that require multiple sign-in fields** for applications that require more than just username and password fields toosign in</span></span>

-   <span data-ttu-id="76cd5-116">**Hello címkék testreszabása** hello felhasználónév és jelszó bemeneti mezők jelennek meg a felhasználók a hello [alkalmazás hozzáférési Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) Ha a hitelesítő adatok megadása</span><span class="sxs-lookup"><span data-stu-id="76cd5-116">**Customize hello labels** of hello username and password input fields your users see on hello [Application Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) when they enter their credentials</span></span>

-   <span data-ttu-id="76cd5-117">Engedélyezi a **felhasználók** tooprovide saját felhasználónevek és jelszavak adatbevitel az manuálisan hello a meglévő alkalmazás fiókok számára [alkalmazás hozzáférési Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)</span><span class="sxs-lookup"><span data-stu-id="76cd5-117">Allow your **users** tooprovide their own usernames and passwords for any existing application accounts they are typing in manually on hello [Application Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)</span></span>

-   <span data-ttu-id="76cd5-118">Lehetővé teszi egy **hello üzleti csoport** toospecify hello felhasználónevek és jelszavak hozzárendelt tooa felhasználó hello segítségével [önkiszolgáló alkalmazás-hozzáférés](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="76cd5-118">Allow a **member of hello business group** toospecify hello usernames and passwords assigned tooa user by using hello [Self-Service Application Access](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) feature</span></span>

-   <span data-ttu-id="76cd5-119">Lehetővé teszi egy **rendszergazda** toospecify hello felhasználónevek és jelszavak hozzárendelt tooa felhasználó hello frissítés hitelesítő adatok használatával funkciót [hozzárendelése egy felhasználói tooan alkalmazást](#_How_to_configure_1)</span><span class="sxs-lookup"><span data-stu-id="76cd5-119">Allow an **administrator** toospecify hello usernames and passwords assigned tooa user by using hello Update Credentials feature when [assigning a user tooan application](#_How_to_configure_1)</span></span>

-   <span data-ttu-id="76cd5-120">Lehetővé teszi egy **rendszergazda** toospecify megosztott hello felhasználónév vagy jelszó hello frissítés hitelesítő adatok segítségével egy csoport tagjainak által használt funkciót [egy csoport tooan alkalmazás hozzárendelése](#assign-an-application-to-a-group-directly)</span><span class="sxs-lookup"><span data-stu-id="76cd5-120">Allow an **administrator** toospecify hello shared username or password used by a group of people by using hello Update Credentials feature when [assigning a group tooan application](#assign-an-application-to-a-group-directly)</span></span>

<span data-ttu-id="76cd5-121">Az alábbiakban ismerteti, hogyan engedélyezheti [jelszó-alapú egyszeri bejelentkezést](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) hello segítségével hozzáadása tooany alkalmazás **nem galéria alkalmazás hozzáadása** tapasztalhat.</span><span class="sxs-lookup"><span data-stu-id="76cd5-121">Below describes how you can enable [Password-Based Single Sign-on](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) tooany application that you add using hello **add a non-gallery application** experience.</span></span>

## <a name="overview-of-steps-required"></a><span data-ttu-id="76cd5-122">A szükséges lépések – áttekintés</span><span class="sxs-lookup"><span data-stu-id="76cd5-122">Overview of steps required</span></span>

<span data-ttu-id="76cd5-123">egy alkalmazás hello Azure AD-galériából tooconfigure kell:</span><span class="sxs-lookup"><span data-stu-id="76cd5-123">tooconfigure an application from hello Azure AD gallery you need to:</span></span>

-   [<span data-ttu-id="76cd5-124">Nem galéria alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="76cd5-124">Add a non-gallery application</span></span>](#add-a-non-gallery-application)

-   [<span data-ttu-id="76cd5-125">Állítsa be hello alkalmazását jelszó egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="76cd5-125">Configure hello application for password single sign-on</span></span>](#configure-the-application-for-password-single-sign-on)

-   [<span data-ttu-id="76cd5-126">Hello alkalmazás tooa felhasználó vagy csoport hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="76cd5-126">Assign hello application tooa user or a group</span></span>](#assign-the-application-to-a-user-or-a-group)

    -   [<span data-ttu-id="76cd5-127">Rendelje hozzá egy felhasználói tooan alkalmazás közvetlenül</span><span class="sxs-lookup"><span data-stu-id="76cd5-127">Assign a user tooan application directly</span></span>](#assign-a-user-to-an-application-directly)

    -   [<span data-ttu-id="76cd5-128">Alkalmazáscsoport tooa közvetlenül</span><span class="sxs-lookup"><span data-stu-id="76cd5-128">Assign an application tooa group directly</span></span>](#assign-an-application-to-a-group-directly)

## <a name="add-a-non-gallery-application"></a><span data-ttu-id="76cd5-129">Nem galéria alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="76cd5-129">Add a non-gallery application</span></span>

<span data-ttu-id="76cd5-130">az alkalmazás a hello Azure AD-katalógusában, tooadd kövesse hello alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="76cd5-130">tooadd an application from hello Azure AD Gallery, follow hello steps below:</span></span>

1.  <span data-ttu-id="76cd5-131">Nyissa meg hello [Azure Portal](https://portal.azure.com) , és jelentkezzen be a egy **globális rendszergazda** vagy **társadminisztrátornak**</span><span class="sxs-lookup"><span data-stu-id="76cd5-131">Open hello [Azure Portal](https://portal.azure.com) and sign in as a **Global Administrator** or **Co-admin**</span></span>

2.  <span data-ttu-id="76cd5-132">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="76cd5-132">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="76cd5-133">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="76cd5-133">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="76cd5-134">Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="76cd5-134">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="76cd5-135">Kattintson a hello **hozzáadása** hello jobb felső sarokban lévő hello a gomb **vállalati alkalmazások** panel</span><span class="sxs-lookup"><span data-stu-id="76cd5-135">click hello **Add** button at hello top-right corner on hello **Enterprise Applications** blade</span></span>

6.  <span data-ttu-id="76cd5-136">Kattintson a **nem-gyűjtemény alkalmazás.**</span><span class="sxs-lookup"><span data-stu-id="76cd5-136">click **Non-gallery application.**</span></span>

7.  <span data-ttu-id="76cd5-137">Adja meg az alkalmazás nevére hello hello **neve** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="76cd5-137">Enter hello name of your application in hello **Name** textbox.</span></span> <span data-ttu-id="76cd5-138">Válassza ki **hozzáadása.**</span><span class="sxs-lookup"><span data-stu-id="76cd5-138">Select **Add.**</span></span>

<span data-ttu-id="76cd5-139">Egy rövid időszak után kell képes toosee hello alkalmazás konfigurációs panelen.</span><span class="sxs-lookup"><span data-stu-id="76cd5-139">After a short period, you be able toosee hello application’s configuration blade.</span></span>

## <a name="configure-hello-application-for-password-single-sign-on"></a><span data-ttu-id="76cd5-140">Állítsa be hello alkalmazását jelszó egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="76cd5-140">Configure hello application for password single sign-on</span></span>

<span data-ttu-id="76cd5-141">tooconfigure egyszeri bejelentkezés egy alkalmazás kövesse az alábbi hello lépéseket:</span><span class="sxs-lookup"><span data-stu-id="76cd5-141">tooconfigure single sign-on for an application, follow hello steps below:</span></span>

1.  <span data-ttu-id="76cd5-142">Nyitott hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="76cd5-142">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="76cd5-143">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="76cd5-143">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="76cd5-144">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="76cd5-144">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="76cd5-145">Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="76cd5-145">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="76cd5-146">Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="76cd5-146">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="76cd5-147">Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="76cd5-147">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="76cd5-148">Válassza ki a kívánt tooconfigure egyszeri bejelentkezés hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="76cd5-148">Select hello application you want tooconfigure single sign-on.</span></span>

7.  <span data-ttu-id="76cd5-149">Ha hello alkalmazás betölt, kattintson a hello **egyszeri bejelentkezés** hello alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="76cd5-149">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="76cd5-150">Jelölje be hello mód **jelszóalapú bejelentkezés.**</span><span class="sxs-lookup"><span data-stu-id="76cd5-150">Select hello mode **Password-based Sign-on.**</span></span>

9.  <span data-ttu-id="76cd5-151">Adja meg a hello **bejelentkezési URL-cím**.</span><span class="sxs-lookup"><span data-stu-id="76cd5-151">Enter hello **Sign-on URL**.</span></span> <span data-ttu-id="76cd5-152">Ez a hello URL-cím, ahol felhasználók adja meg a felhasználónevet és jelszót toosign a számára.</span><span class="sxs-lookup"><span data-stu-id="76cd5-152">This is hello URL where users enter their username and password toosign in to.</span></span> <span data-ttu-id="76cd5-153">Győződjön meg arról, mezők hello bejelentkezés láthatók hello URL-címen.</span><span class="sxs-lookup"><span data-stu-id="76cd5-153">Ensure hello sign in fields are visible at hello URL.</span></span>

10. <span data-ttu-id="76cd5-154">Felhasználók hozzárendelése toohello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="76cd5-154">Assign users toohello application.</span></span>

11. <span data-ttu-id="76cd5-155">Emellett is megadhatja hello felhasználó nevében hitelesítő adatok hello felhasználók hello sorát kiválasztásával, és kattintson a **frissítéséhez szükséges hitelesítő adatokat** és hello felhasználónév és jelszó megadásával hello felhasználók nevében.</span><span class="sxs-lookup"><span data-stu-id="76cd5-155">Additionally, you can also provide credentials on behalf of hello user by selecting hello rows of hello users and clicking on **Update Credentials** and entering hello username and password on behalf of hello users.</span></span> <span data-ttu-id="76cd5-156">Ellenkező esetben a felhasználók fognak felszólító tooenter hello hitelesítő adatok magukat az indítás után.</span><span class="sxs-lookup"><span data-stu-id="76cd5-156">Otherwise, users be prompted tooenter hello credentials themselves upon launch.</span></span>

## <a name="assign-a-user-tooan-application-directly"></a><span data-ttu-id="76cd5-157">Rendelje hozzá egy felhasználói tooan alkalmazás közvetlenül</span><span class="sxs-lookup"><span data-stu-id="76cd5-157">Assign a user tooan application directly</span></span>

<span data-ttu-id="76cd5-158">tooassign felhasználók tooan egy vagy több alkalmazás közvetlenül, kövesse az alábbi hello lépéseket:</span><span class="sxs-lookup"><span data-stu-id="76cd5-158">tooassign one or more users tooan application directly, follow hello steps below:</span></span>

1.  <span data-ttu-id="76cd5-159">Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="76cd5-159">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="76cd5-160">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="76cd5-160">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="76cd5-161">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="76cd5-161">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="76cd5-162">Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="76cd5-162">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="76cd5-163">Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="76cd5-163">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="76cd5-164">Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="76cd5-164">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="76cd5-165">Válassza ki a kívánt felhasználó toofrom hello listáját tooassign hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="76cd5-165">Select hello application you want tooassign a user toofrom hello list.</span></span>

7.  <span data-ttu-id="76cd5-166">Amikor hello alkalmazás betölt, kattintson a **felhasználók és csoportok** hello alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="76cd5-166">Once hello application loads, click **Users and Groups** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="76cd5-167">Hello kattintson **Hozzáadás** gomb felett hello **felhasználók és csoportok** lista tooopen hello **hozzáadása hozzárendelés** panelen.</span><span class="sxs-lookup"><span data-stu-id="76cd5-167">Click hello **Add** button on top of hello **Users and Groups** list tooopen hello **Add Assignment** blade.</span></span>

9.  <span data-ttu-id="76cd5-168">hello kattintson **felhasználók és csoportok** hello a választó **hozzáadása hozzárendelés** panelen.</span><span class="sxs-lookup"><span data-stu-id="76cd5-168">click hello **Users and groups** selector from hello **Add Assignment** blade.</span></span>

10. <span data-ttu-id="76cd5-169">Hello típusának **teljes név** vagy **e-mail cím** érdekli hello való hozzárendelése hello felhasználó **Keresés név vagy e-mail cím alapján** keresőmezőbe.</span><span class="sxs-lookup"><span data-stu-id="76cd5-169">Type in hello **full name** or **email address** of hello user you are interested in assigning into hello **Search by name or email address** search box.</span></span>

11. <span data-ttu-id="76cd5-170">Hello rámutat **felhasználói** a hello lista tooreveal egy **jelölőnégyzet**.</span><span class="sxs-lookup"><span data-stu-id="76cd5-170">Hover over hello **user** in hello list tooreveal a **checkbox**.</span></span> <span data-ttu-id="76cd5-171">Kattintson a hello jelölőnégyzet következő toohello felhasználó profil fénykép vagy embléma tooadd a felhasználó toohello **kijelölt** listája.</span><span class="sxs-lookup"><span data-stu-id="76cd5-171">Click hello checkbox next toohello user’s profile photo or logo tooadd your user toohello **Selected** list.</span></span>

12. <span data-ttu-id="76cd5-172">**Választható lehetőség:** Ha túl szeretné**egynél több felhasználó hozzáadása**, egy másik típus **teljes név** vagy **e-mail cím** be hello **Keresés név e-mail cím vagy** keresési mezőbe, majd kattintson a hello jelölőnégyzet tooadd a felhasználó toohello **kijelölt** listája.</span><span class="sxs-lookup"><span data-stu-id="76cd5-172">**Optional:** If you would like too**add more than one user**, type in another **full name** or **email address** into hello **Search by name or email address** search box, and click hello checkbox tooadd this user toohello **Selected** list.</span></span>

13. <span data-ttu-id="76cd5-173">Ha elkészült, válassza a felhasználók, kattintson a hello **válasszon** gomb tooadd őket felhasználók és csoportok toobe toohello listája toohello alkalmazás hozzárendelése.</span><span class="sxs-lookup"><span data-stu-id="76cd5-173">When you are finished selecting users, click hello **Select** button tooadd them toohello list of users and groups toobe assigned toohello application.</span></span>

14. <span data-ttu-id="76cd5-174">**Nem kötelező:** hello kattintson **Szerepkörválasztás** hello a választó **hozzáadása hozzárendelés** panel tooselect szerepkör tooassign toohello felhasználók kijelölt.</span><span class="sxs-lookup"><span data-stu-id="76cd5-174">**Optional:** click hello **Select Role** selector in hello **Add Assignment** blade tooselect a role tooassign toohello users you have selected.</span></span>

15. <span data-ttu-id="76cd5-175">Kattintson a hello **hozzárendelése** gomb tooassign hello alkalmazás toohello kijelölt felhasználók.</span><span class="sxs-lookup"><span data-stu-id="76cd5-175">Click hello **Assign** button tooassign hello application toohello selected users.</span></span>

## <a name="assign-an-application-tooa-group-directly"></a><span data-ttu-id="76cd5-176">Alkalmazáscsoport tooa közvetlenül</span><span class="sxs-lookup"><span data-stu-id="76cd5-176">Assign an application tooa group directly</span></span>

<span data-ttu-id="76cd5-177">egy vagy több tooassign csoportok közvetlenül, tooan alkalmazás kövesse hello alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="76cd5-177">tooassign one or more groups tooan application directly, follow hello steps below:</span></span>

1.  <span data-ttu-id="76cd5-178">Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="76cd5-178">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="76cd5-179">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="76cd5-179">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="76cd5-180">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="76cd5-180">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="76cd5-181">Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="76cd5-181">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="76cd5-182">Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="76cd5-182">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="76cd5-183">Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="76cd5-183">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="76cd5-184">Válassza ki a kívánt felhasználó toofrom hello listáját tooassign hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="76cd5-184">Select hello application you want tooassign a user toofrom hello list.</span></span>

7.  <span data-ttu-id="76cd5-185">Amikor hello alkalmazás betölt, kattintson a **felhasználók és csoportok** hello alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="76cd5-185">Once hello application loads, click **Users and Groups** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="76cd5-186">Hello kattintson **Hozzáadás** gomb felett hello **felhasználók és csoportok** lista tooopen hello **hozzáadása hozzárendelés** panelen.</span><span class="sxs-lookup"><span data-stu-id="76cd5-186">Click hello **Add** button on top of hello **Users and Groups** list tooopen hello **Add Assignment** blade.</span></span>

9.  <span data-ttu-id="76cd5-187">hello kattintson **felhasználók és csoportok** hello a választó **hozzáadása hozzárendelés** panelen.</span><span class="sxs-lookup"><span data-stu-id="76cd5-187">click hello **Users and groups** selector from hello **Add Assignment** blade.</span></span>

10. <span data-ttu-id="76cd5-188">Hello típusa **teljes csoportnév** érdekli hello való hozzárendelése hello csoport **Keresés név vagy e-mail cím alapján** keresőmezőbe.</span><span class="sxs-lookup"><span data-stu-id="76cd5-188">Type in hello **full group name** of hello group you are interested in assigning into hello **Search by name or email address** search box.</span></span>

11. <span data-ttu-id="76cd5-189">Hello rámutat **csoport** a hello lista tooreveal egy **jelölőnégyzet**.</span><span class="sxs-lookup"><span data-stu-id="76cd5-189">Hover over hello **group** in hello list tooreveal a **checkbox**.</span></span> <span data-ttu-id="76cd5-190">Kattintson a profil fénykép vagy embléma tooadd hello jelölőnégyzet következő toohello csoport a felhasználó toohello **kijelölt** listája.</span><span class="sxs-lookup"><span data-stu-id="76cd5-190">Click hello checkbox next toohello group’s profile photo or logo tooadd your user toohello **Selected** list.</span></span>

12. <span data-ttu-id="76cd5-191">**Nem kötelező:** Ha túl szeretné**egynél több csoport hozzáadása**, egy másik típus **teljes csoport neve** hello be **Keresés név vagy e-mail cím alapján** keresőmezőbe hello jelölőnégyzet tooadd kattintson a csoport toohello **kijelölt** listája.</span><span class="sxs-lookup"><span data-stu-id="76cd5-191">**Optional:** If you would like too**add more than one group**, type in another **full group name** into hello **Search by name or email address** search box, and click hello checkbox tooadd this group toohello **Selected** list.</span></span>

13. <span data-ttu-id="76cd5-192">Ha befejezte a csoportok kiválasztásával, kattintson a hello **válasszon** gomb tooadd őket felhasználók és csoportok toobe toohello listája toohello alkalmazás hozzárendelése.</span><span class="sxs-lookup"><span data-stu-id="76cd5-192">When you are finished selecting groups, click hello **Select** button tooadd them toohello list of users and groups toobe assigned toohello application.</span></span>

14. <span data-ttu-id="76cd5-193">**Választható lehetőség:** hello kattintson **Szerepkörválasztás** hello a választó **hozzáadása hozzárendelés** panel tooselect egy szerepkör tooassign toohello csoportokat kijelölt.</span><span class="sxs-lookup"><span data-stu-id="76cd5-193">**Optional:** click hello **Select Role** selector in hello **Add Assignment** blade tooselect a role tooassign toohello groups you have selected.</span></span>

15. <span data-ttu-id="76cd5-194">Kattintson a hello **hozzárendelése** gomb tooassign hello alkalmazás toohello kijelölt csoportok.</span><span class="sxs-lookup"><span data-stu-id="76cd5-194">Click hello **Assign** button tooassign hello application toohello selected groups.</span></span>

<span data-ttu-id="76cd5-195">Miután rövid időn belül, kijelölt hello felhasználók kell tudni toolaunch ezeket az alkalmazásokat hello a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="76cd5-195">After a short period, hello users you have selected be able toolaunch these applications in hello Access Panel.</span></span>

## <a name="next-steps"></a><span data-ttu-id="76cd5-196">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="76cd5-196">Next steps</span></span>
[<span data-ttu-id="76cd5-197">Adja meg az egyszeri bejelentkezés tooyour alkalmazások alkalmazásproxyval</span><span class="sxs-lookup"><span data-stu-id="76cd5-197">Provide single sign-on tooyour apps with Application Proxy</span></span>](active-directory-application-proxy-sso-using-kcd.md)
