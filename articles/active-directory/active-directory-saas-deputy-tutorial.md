---
title: "Oktatóanyag: Azure Active Directoryval integrált helyettes |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és helyettes között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 5665c3ac-5689-4201-80fe-fcc677d4430d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: 42f65b758682ce2513b6bb38ef40a19f955c88c3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-deputy"></a><span data-ttu-id="4619b-103">Oktatóanyag: Azure Active Directoryval integrált helyettes</span><span class="sxs-lookup"><span data-stu-id="4619b-103">Tutorial: Azure Active Directory integration with Deputy</span></span>

<span data-ttu-id="4619b-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate helyettes az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="4619b-104">In this tutorial, you learn how toointegrate Deputy with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="4619b-105">Helyettes integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="4619b-105">Integrating Deputy with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="4619b-106">Megadhatja a hozzáférés tooDeputy rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="4619b-106">You can control in Azure AD who has access tooDeputy</span></span>
- <span data-ttu-id="4619b-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooDeputy (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="4619b-107">You can enable your users tooautomatically get signed-on tooDeputy (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="4619b-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="4619b-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="4619b-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="4619b-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4619b-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="4619b-110">Prerequisites</span></span>

<span data-ttu-id="4619b-111">az Azure AD integrálása helyettes tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="4619b-111">tooconfigure Azure AD integration with Deputy, you need hello following items:</span></span>

- <span data-ttu-id="4619b-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="4619b-112">An Azure AD subscription</span></span>
- <span data-ttu-id="4619b-113">A helyettes egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="4619b-113">A Deputy single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="4619b-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="4619b-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="4619b-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="4619b-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="4619b-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="4619b-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="4619b-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="4619b-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="4619b-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="4619b-118">Scenario description</span></span>
<span data-ttu-id="4619b-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="4619b-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="4619b-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="4619b-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="4619b-121">Helyettes hozzáadása hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="4619b-121">Adding Deputy from hello gallery</span></span>
2. <span data-ttu-id="4619b-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="4619b-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-deputy-from-hello-gallery"></a><span data-ttu-id="4619b-123">Helyettes hozzáadása hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="4619b-123">Adding Deputy from hello gallery</span></span>
<span data-ttu-id="4619b-124">tooconfigure hello integrációja helyettes az Azure AD-be, meg kell tooadd helyettes hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="4619b-124">tooconfigure hello integration of Deputy into Azure AD, you need tooadd Deputy from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="4619b-125">**tooadd helyettes hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="4619b-125">**tooadd Deputy from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="4619b-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="4619b-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="4619b-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="4619b-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="4619b-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="4619b-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="4619b-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="4619b-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="4619b-133">Hello keresési mezőbe, írja be a **helyettes**.</span><span class="sxs-lookup"><span data-stu-id="4619b-133">In hello search box, type **Deputy**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_search.png)

5. <span data-ttu-id="4619b-135">A hello eredmények panelen válassza ki a **helyettes**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="4619b-135">In hello results panel, select **Deputy**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="4619b-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="4619b-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="4619b-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján helyettes</span><span class="sxs-lookup"><span data-stu-id="4619b-138">In this section, you configure and test Azure AD single sign-on with Deputy based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="4619b-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó helyettes tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="4619b-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Deputy is tooa user in Azure AD.</span></span> <span data-ttu-id="4619b-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello helyettes közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="4619b-140">In other words, a link relationship between an Azure AD user and hello related user in Deputy needs toobe established.</span></span>

<span data-ttu-id="4619b-141">Helyettes, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="4619b-141">In Deputy, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="4619b-142">tooconfigure és az Azure AD az egyszeri bejelentkezés helyettes-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="4619b-142">tooconfigure and test Azure AD single sign-on with Deputy, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="4619b-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="4619b-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="4619b-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="4619b-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="4619b-145">**[Helyettes tesztfelhasználó létrehozása](#creating-a-deputy-test-user)**  -toohave egy megfelelője a Britta Simon a helyettes, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="4619b-145">**[Creating a Deputy test user](#creating-a-deputy-test-user)** - toohave a counterpart of Britta Simon in Deputy that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="4619b-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="4619b-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="4619b-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="4619b-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="4619b-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="4619b-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="4619b-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az helyettes alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="4619b-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Deputy application.</span></span>

<span data-ttu-id="4619b-150">**az Azure AD tooconfigure egyszeri bejelentkezést a helyettes, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="4619b-150">**tooconfigure Azure AD single sign-on with Deputy, perform hello following steps:**</span></span>

1. <span data-ttu-id="4619b-151">Az Azure portál, a hello hello **helyettes** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="4619b-151">In hello Azure portal, on hello **Deputy** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="4619b-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="4619b-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_samlbase.png)

3. <span data-ttu-id="4619b-155">A hello **helyettes tartomány és az URL-címek** szakaszban, ha tooconfigure hello alkalmazás **IDP** kezdeményezett mód:</span><span class="sxs-lookup"><span data-stu-id="4619b-155">On hello **Deputy Domain and URLs** section, If you wish tooconfigure hello application in **IDP** initiated mode:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_url1.png)

    <span data-ttu-id="4619b-157">a.</span><span class="sxs-lookup"><span data-stu-id="4619b-157">a.</span></span> <span data-ttu-id="4619b-158">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:</span><span class="sxs-lookup"><span data-stu-id="4619b-158">In hello **Identifier** textbox, type a URL using hello following pattern:</span></span>
    |  |
    | ----|
    | `https://<subdomain>.<region>.au.deputy.com` |
    | `https://<subdomain>.<region>.ent-au.deputy.com` |
    | `https://<subdomain>.<region>.na.deputy.com`|
    | `https://<subdomain>.<region>.ent-na.deputy.com`|
    | `https://<subdomain>.<region>.eu.deputy.com` |
    | `https://<subdomain>.<region>.ent-eu.deputy.com` |
    | `https://<subdomain>.<region>.as.deputy.com` |
    | `https://<subdomain>.<region>.ent-as.deputy.com` |
    | `https://<subdomain>.<region>.la.deputy.com` |
    | `https://<subdomain>.<region>.ent-la.deputy.com` |
    | `https://<subdomain>.<region>.af.deputy.com` |
    | `https://<subdomain>.<region>.ent-af.deputy.com` |
    | `https://<subdomain>.<region>.an.deputy.com` |
    | `https://<subdomain>.<region>.ent-an.deputy.com` |
    | `https://<subdomain>.<region>.deputy.com` |

    <span data-ttu-id="4619b-159">b.</span><span class="sxs-lookup"><span data-stu-id="4619b-159">b.</span></span> <span data-ttu-id="4619b-160">A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:</span><span class="sxs-lookup"><span data-stu-id="4619b-160">In hello **Reply URL** textbox, type a URL using hello following pattern:</span></span>
    | |
    |----|
    | `https://<subdomain>.<region>.au.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.ent-au.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.na.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.ent-na.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.eu.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.ent-eu.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.as.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.ent-as.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.la.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.ent-la.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.af.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.ent-af.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.an.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.ent-an.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.deputy.com/exec/devapp/samlacs.` |

4. <span data-ttu-id="4619b-161">Ellenőrizze **megjelenítése speciális URL-beállításainak**.</span><span class="sxs-lookup"><span data-stu-id="4619b-161">Check **Show advanced URL settings**.</span></span> <span data-ttu-id="4619b-162">Ha tooconfigure hello alkalmazás **SP** kezdeményezett mód:</span><span class="sxs-lookup"><span data-stu-id="4619b-162">If you wish tooconfigure hello application in **SP** initiated mode:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_url2.png)

    <span data-ttu-id="4619b-164">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<your-subdomain>.<region>.deputy.com`</span><span class="sxs-lookup"><span data-stu-id="4619b-164">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<your-subdomain>.<region>.deputy.com`</span></span>
    
    >[!NOTE]
    > <span data-ttu-id="4619b-165">Helyettes régió utótag megadása nem kötelező, vagy ezek egyikét kell használni: au |} nA |} EU |}, |} la |} af |} egy |} félellenőrzés-au |} félellenőrzés-na |} félellenőrzés – eu |} félellenőrzés-mint |} félellenőrzés-la |} félellenőrzés-af |} félellenőrzés egy</span><span class="sxs-lookup"><span data-stu-id="4619b-165">Deputy region suffix is optional, or it should use one of these: au | na | eu |as |la |af |an |ent-au |ent-na |ent-eu |ent-as | ent-la | ent-af | ent-an</span></span>

    > [!NOTE] 
    > <span data-ttu-id="4619b-166">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="4619b-166">These values are not real.</span></span> <span data-ttu-id="4619b-167">Frissítse a azonosító, a válasz URL-CÍMEN és a bejelentkezési URL-cím a tényleges hello értékeket.</span><span class="sxs-lookup"><span data-stu-id="4619b-167">Update these values with hello actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="4619b-168">Ügyfél [helyettes támogatási csoport](https://www.deputy.com/call-centers-customer-support-scheduling-software) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="4619b-168">Contact [Deputy support team](https://www.deputy.com/call-centers-customer-support-scheduling-software) tooget these values.</span></span> 

5. <span data-ttu-id="4619b-169">A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="4619b-169">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_certificate.png) 

6. <span data-ttu-id="4619b-171">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="4619b-171">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-deputy-tutorial/tutorial_general_400.png)
    
7. <span data-ttu-id="4619b-173">A hello **helyettes konfigurációs** kattintson **konfigurálása helyettes** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="4619b-173">On hello **Deputy Configuration** section, click **Configure Deputy** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="4619b-174">Másolás hello **SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="4619b-174">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_configure.png) 

8. <span data-ttu-id="4619b-176">Keresse meg a következő URL-cím toohello:[https://(your-subdomain).deputy.com/exec/config/system_config]( https://(your-subdomain).deputy.com/exec/config/system_config).</span><span class="sxs-lookup"><span data-stu-id="4619b-176">Navigate toohello following URL:[https://(your-subdomain).deputy.com/exec/config/system_config]( https://(your-subdomain).deputy.com/exec/config/system_config).</span></span> <span data-ttu-id="4619b-177">Nyissa meg túl**biztonsági beállítások** kattintson **szerkesztése**.</span><span class="sxs-lookup"><span data-stu-id="4619b-177">Go too**Security Settings** and click **Edit**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_004.png)

9. <span data-ttu-id="4619b-179">Ez a **biztonsági beállítások** lapon, a következő lépések végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="4619b-179">On this **Security Settings** page, perform below steps.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_005.png)
    
    <span data-ttu-id="4619b-181">a.</span><span class="sxs-lookup"><span data-stu-id="4619b-181">a.</span></span> <span data-ttu-id="4619b-182">Engedélyezése **közösségi bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="4619b-182">Enable **Social Login**.</span></span>
   
    <span data-ttu-id="4619b-183">b.</span><span class="sxs-lookup"><span data-stu-id="4619b-183">b.</span></span> <span data-ttu-id="4619b-184">Nyissa meg a Base64 kódolású tanúsítvány a Jegyzettömbben, a vágólapra tartalmának másolása hello Azure portálról letöltött és toohello Beillesztés **OpenSSL tanúsítvány** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="4619b-184">Open your Base64 encoded certificate downloaded from Azure portal in notepad, copy hello content of it into your clipboard, and then paste it toohello **OpenSSL Certificate** textbox.</span></span>
   
    <span data-ttu-id="4619b-185">c.</span><span class="sxs-lookup"><span data-stu-id="4619b-185">c.</span></span> <span data-ttu-id="4619b-186">A hello SAML SSO URL-cím mezőbe írja be a`https://<your subdomain>.deputy.com/exec/devapp/samlacs?dpLoginTo=<saml sso url>`</span><span class="sxs-lookup"><span data-stu-id="4619b-186">In hello SAML SSO URL textbox, type `https://<your subdomain>.deputy.com/exec/devapp/samlacs?dpLoginTo=<saml sso url>`</span></span>
    
    <span data-ttu-id="4619b-187">d.</span><span class="sxs-lookup"><span data-stu-id="4619b-187">d.</span></span> <span data-ttu-id="4619b-188">Cserélje le a hello SAML SSO URL-cím szövegmezőjének `<your subdomain>` a altartomány rendelkező.</span><span class="sxs-lookup"><span data-stu-id="4619b-188">In hello SAML SSO URL textbox, replace `<your subdomain>` with your subdomain.</span></span>
   
    <span data-ttu-id="4619b-189">e.</span><span class="sxs-lookup"><span data-stu-id="4619b-189">e.</span></span> <span data-ttu-id="4619b-190">Hello SAML SSO URL-cím beviteli mező, cserélje le `<saml sso url>` a hello **SAML-alapú egyszeri bejelentkezési URL-címe** hello Azure-portálon a másolta.</span><span class="sxs-lookup"><span data-stu-id="4619b-190">In hello SAML SSO URL textbox, replace `<saml sso url>` with hello **SAML Single Sign-On Service URL** you have copied from hello Azure portal.</span></span>
   
    <span data-ttu-id="4619b-191">f.</span><span class="sxs-lookup"><span data-stu-id="4619b-191">f.</span></span> <span data-ttu-id="4619b-192">Kattintson a **beállítások mentése**.</span><span class="sxs-lookup"><span data-stu-id="4619b-192">Click **Save Settings**.</span></span>

> [!TIP]
> <span data-ttu-id="4619b-193">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="4619b-193">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="4619b-194">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="4619b-194">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="4619b-195">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="4619b-195">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="4619b-196">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="4619b-196">Creating an Azure AD test user</span></span>
<span data-ttu-id="4619b-197">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="4619b-197">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="4619b-199">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="4619b-199">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="4619b-200">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="4619b-200">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-deputy-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="4619b-202">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="4619b-202">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-deputy-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="4619b-204">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="4619b-204">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-deputy-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="4619b-206">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="4619b-206">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-deputy-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="4619b-208">a.</span><span class="sxs-lookup"><span data-stu-id="4619b-208">a.</span></span> <span data-ttu-id="4619b-209">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="4619b-209">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="4619b-210">b.</span><span class="sxs-lookup"><span data-stu-id="4619b-210">b.</span></span> <span data-ttu-id="4619b-211">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="4619b-211">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="4619b-212">c.</span><span class="sxs-lookup"><span data-stu-id="4619b-212">c.</span></span> <span data-ttu-id="4619b-213">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="4619b-213">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="4619b-214">d.</span><span class="sxs-lookup"><span data-stu-id="4619b-214">d.</span></span> <span data-ttu-id="4619b-215">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="4619b-215">Click **Create**.</span></span>
 
### <a name="creating-a-deputy-test-user"></a><span data-ttu-id="4619b-216">Helyettes tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="4619b-216">Creating a Deputy test user</span></span>

<span data-ttu-id="4619b-217">az Azure AD tooenable felhasználók toolog a tooDeputy, akkor ki kell építenie helyettes be.</span><span class="sxs-lookup"><span data-stu-id="4619b-217">tooenable Azure AD users toolog in tooDeputy, they must be provisioned into Deputy.</span></span> <span data-ttu-id="4619b-218">Helyettes, ha egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="4619b-218">In case of Deputy, provisioning is a manual task.</span></span>

#### <a name="tooprovision-a-user-account-perform-hello-following-steps"></a><span data-ttu-id="4619b-219">tooprovision egy felhasználói fiókot, hajtsa végre a következő lépéseket hello:</span><span class="sxs-lookup"><span data-stu-id="4619b-219">tooprovision a user account, perform hello following steps:</span></span>
1. <span data-ttu-id="4619b-220">Jelentkezzen be tooyour helyettes vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="4619b-220">Log in tooyour Deputy company site as an administrator.</span></span>

2. <span data-ttu-id="4619b-221">A hello felső navigációs ablaktábláján kattintson **személyek**.</span><span class="sxs-lookup"><span data-stu-id="4619b-221">On hello top navigation pane, click **People**.</span></span>
   
   <span data-ttu-id="4619b-222">![Személyek](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_001.png "személyek")</span><span class="sxs-lookup"><span data-stu-id="4619b-222">![People](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_001.png "People")</span></span>

3. <span data-ttu-id="4619b-223">Hello kattintson **hozzáadása személyek** gombra, majd kattintson a **hozzáadása egy személy**.</span><span class="sxs-lookup"><span data-stu-id="4619b-223">Click hello **Add People** button and click **Add a single person**.</span></span>
   
   <span data-ttu-id="4619b-224">![Adja hozzá a személyek](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_002.png "személyek hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="4619b-224">![Add People](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_002.png "Add People")</span></span>

4. <span data-ttu-id="4619b-225">Hajtsa végre az alábbi lépésekkel hello, és kattintson a **meghívása & mentése**.</span><span class="sxs-lookup"><span data-stu-id="4619b-225">Perform hello following steps and click **Save & Invite**.</span></span>
   
   <span data-ttu-id="4619b-226">![Új felhasználó](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_003.png "új felhasználó")</span><span class="sxs-lookup"><span data-stu-id="4619b-226">![New User](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_003.png "New User")</span></span>

   <span data-ttu-id="4619b-227">a.</span><span class="sxs-lookup"><span data-stu-id="4619b-227">a.</span></span> <span data-ttu-id="4619b-228">A hello **neve** szövegmezőhöz típusnév hello felhasználó például **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="4619b-228">In hello **Name** textbox, type name of hello user like **BrittaSimon**.</span></span>
   
   <span data-ttu-id="4619b-229">b.</span><span class="sxs-lookup"><span data-stu-id="4619b-229">b.</span></span> <span data-ttu-id="4619b-230">A hello **E-mail** szövegmezőhöz típus hello e-mail címét egy tooprovision kívánt Azure AD-fiókot.</span><span class="sxs-lookup"><span data-stu-id="4619b-230">In hello **Email** textbox, type hello email address of an Azure AD account you want tooprovision.</span></span>
   
   <span data-ttu-id="4619b-231">c.</span><span class="sxs-lookup"><span data-stu-id="4619b-231">c.</span></span> <span data-ttu-id="4619b-232">A hello **működéséhez** szövegmezőhöz hello üzleti típusnév.</span><span class="sxs-lookup"><span data-stu-id="4619b-232">In hello **Work at** textbox, type hello business name.</span></span>
   
   <span data-ttu-id="4619b-233">d.</span><span class="sxs-lookup"><span data-stu-id="4619b-233">d.</span></span> <span data-ttu-id="4619b-234">Kattintson a **meghívása & mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="4619b-234">Click **Save & Invite** button.</span></span>

5. <span data-ttu-id="4619b-235">hello AAD fióktulajdonos kap egy e-mailt legyen és megfeleljen a fiókjuk egy hivatkozás tooconfirm mielőtt aktívvá válik.</span><span class="sxs-lookup"><span data-stu-id="4619b-235">hello AAD account holder receives an email and follows a link tooconfirm their account before it becomes active.</span></span> <span data-ttu-id="4619b-236">Bármely más helyettes felhasználói fiók létrehozása eszközök vagy helyettes tooprovision AAD által nyújtott API-k felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="4619b-236">You can use any other Deputy user account creation tools or APIs provided by Deputy tooprovision AAD user accounts.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="4619b-237">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="4619b-237">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="4619b-238">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooDeputy megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="4619b-238">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooDeputy.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="4619b-240">**tooassign Britta Simon tooDeputy, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="4619b-240">**tooassign Britta Simon tooDeputy, perform hello following steps:**</span></span>

1. <span data-ttu-id="4619b-241">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="4619b-241">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="4619b-243">Hello alkalmazások listában válassza ki a **helyettes**.</span><span class="sxs-lookup"><span data-stu-id="4619b-243">In hello applications list, select **Deputy**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_app.png) 

3. <span data-ttu-id="4619b-245">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="4619b-245">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="4619b-247">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="4619b-247">Click **Add** button.</span></span> <span data-ttu-id="4619b-248">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="4619b-248">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="4619b-250">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="4619b-250">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="4619b-251">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="4619b-251">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="4619b-252">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="4619b-252">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="4619b-253">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="4619b-253">Testing single sign-on</span></span>

<span data-ttu-id="4619b-254">hello ebben a szakaszban célja tootest hozzáférési Panel az Azure AD SSO konfigurációs használatával hello.</span><span class="sxs-lookup"><span data-stu-id="4619b-254">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="4619b-255">Hello helyettes hello hozzáférési Panel csempére kattintva kapja meg automatikusan bejelentkezett tooyour helyettes alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="4619b-255">When you click hello Deputy tile in hello Access Panel, you should get automatically signed-on tooyour Deputy application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4619b-256">További források</span><span class="sxs-lookup"><span data-stu-id="4619b-256">Additional resources</span></span>

* [<span data-ttu-id="4619b-257">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="4619b-257">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="4619b-258">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="4619b-258">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_203.png

