---
title: "Oktatóanyag: Azure Active Directoryval integrált LockPath Keylight |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és LockPath Keylight között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 234a32f1-9f56-4650-9e31-7b38ad734b1a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: jeedes
ms.openlocfilehash: 5485aeb068ba6fbdb4ea9bfc89d401e00c5b1d29
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-lockpath-keylight"></a><span data-ttu-id="f5f5a-103">Oktatóanyag: Azure Active Directoryval integrált LockPath Keylight</span><span class="sxs-lookup"><span data-stu-id="f5f5a-103">Tutorial: Azure Active Directory integration with LockPath Keylight</span></span>

<span data-ttu-id="f5f5a-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate LockPath Keylight az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="f5f5a-104">In this tutorial, you learn how toointegrate LockPath Keylight with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="f5f5a-105">LockPath Keylight integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="f5f5a-105">Integrating LockPath Keylight with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="f5f5a-106">Megadhatja a hozzáférés tooLockPath Keylight rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="f5f5a-106">You can control in Azure AD who has access tooLockPath Keylight</span></span>
- <span data-ttu-id="f5f5a-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooLockPath Keylight (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="f5f5a-107">You can enable your users tooautomatically get signed-on tooLockPath Keylight (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="f5f5a-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="f5f5a-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="f5f5a-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="f5f5a-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f5f5a-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="f5f5a-110">Prerequisites</span></span>

<span data-ttu-id="f5f5a-111">az Azure AD integrálása LockPath Keylight tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="f5f5a-111">tooconfigure Azure AD integration with LockPath Keylight, you need hello following items:</span></span>

- <span data-ttu-id="f5f5a-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="f5f5a-112">An Azure AD subscription</span></span>
- <span data-ttu-id="f5f5a-113">Egy LockPath Keylight egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="f5f5a-113">A LockPath Keylight single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="f5f5a-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="f5f5a-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="f5f5a-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="f5f5a-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="f5f5a-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="f5f5a-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="f5f5a-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="f5f5a-118">Scenario description</span></span>
<span data-ttu-id="f5f5a-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="f5f5a-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="f5f5a-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="f5f5a-121">Hello gyűjteményből LockPath Keylight hozzáadása</span><span class="sxs-lookup"><span data-stu-id="f5f5a-121">Adding LockPath Keylight from hello gallery</span></span>
2. <span data-ttu-id="f5f5a-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="f5f5a-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-lockpath-keylight-from-hello-gallery"></a><span data-ttu-id="f5f5a-123">Hello gyűjteményből LockPath Keylight hozzáadása</span><span class="sxs-lookup"><span data-stu-id="f5f5a-123">Adding LockPath Keylight from hello gallery</span></span>
<span data-ttu-id="f5f5a-124">tooconfigure hello integrációja LockPath Keylight az Azure AD-be, meg kell tooadd LockPath Keylight hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-124">tooconfigure hello integration of LockPath Keylight into Azure AD, you need tooadd LockPath Keylight from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="f5f5a-125">**tooadd LockPath Keylight hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="f5f5a-125">**tooadd LockPath Keylight from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="f5f5a-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="f5f5a-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="f5f5a-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="f5f5a-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="f5f5a-133">Hello keresési mezőbe, írja be a **LockPath Keylight**.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-133">In hello search box, type **LockPath Keylight**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_search.png)

5. <span data-ttu-id="f5f5a-135">A hello eredmények panelen válassza ki a **LockPath Keylight**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-135">In hello results panel, select **LockPath Keylight**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="f5f5a-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="f5f5a-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="f5f5a-138">Ebben a szakaszban konfigurálhatja, és a vizsgálat az Azure AD egyszeri bejelentkezést a LockPath Keylight "Britta Simon." nevű tesztfelhasználó alapján</span><span class="sxs-lookup"><span data-stu-id="f5f5a-138">In this section, you configure and test Azure AD single sign-on with LockPath Keylight based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="f5f5a-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó LockPath Keylight tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in LockPath Keylight is tooa user in Azure AD.</span></span> <span data-ttu-id="f5f5a-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello LockPath Keylight közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-140">In other words, a link relationship between an Azure AD user and hello related user in LockPath Keylight needs toobe established.</span></span>

<span data-ttu-id="f5f5a-141">LockPath Keylight, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-141">In LockPath Keylight, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="f5f5a-142">tooconfigure és az Azure AD az egyszeri bejelentkezés LockPath Keylight-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="f5f5a-142">tooconfigure and test Azure AD single sign-on with LockPath Keylight, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="f5f5a-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="f5f5a-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="f5f5a-145">**[LockPath Keylight tesztfelhasználó létrehozása](#creating-a-lockpath-keylight-test-user)**  -toohave egy megfelelője a Britta Simon a LockPath Keylight, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-145">**[Creating a LockPath Keylight test user](#creating-a-lockpath-keylight-test-user)** - toohave a counterpart of Britta Simon in LockPath Keylight that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="f5f5a-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="f5f5a-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="f5f5a-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f5f5a-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="f5f5a-149">Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és az LockPath Keylight alkalmazásban egyszeri bejelentkezés beállítása.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your LockPath Keylight application.</span></span>

<span data-ttu-id="f5f5a-150">**tooconfigure az Azure AD egyszeri bejelentkezést a LockPath Keylight, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="f5f5a-150">**tooconfigure Azure AD single sign-on with LockPath Keylight, perform hello following steps:**</span></span>

1. <span data-ttu-id="f5f5a-151">Az Azure portál, a hello hello **LockPath Keylight** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-151">In hello Azure portal, on hello **LockPath Keylight** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="f5f5a-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_samlbase.png)

3. <span data-ttu-id="f5f5a-155">A hello **LockPath Keylight tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello::</span><span class="sxs-lookup"><span data-stu-id="f5f5a-155">On hello **LockPath Keylight Domain and URLs** section, perform hello following steps::</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_url.png)

    <span data-ttu-id="f5f5a-157">a.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-157">a.</span></span> <span data-ttu-id="f5f5a-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<company name>.keylightgrc.com/`</span><span class="sxs-lookup"><span data-stu-id="f5f5a-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<company name>.keylightgrc.com/`</span></span>

    <span data-ttu-id="f5f5a-159">b.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-159">b.</span></span> <span data-ttu-id="f5f5a-160">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<company name>.keylightgrc.com`</span><span class="sxs-lookup"><span data-stu-id="f5f5a-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<company name>.keylightgrc.com`</span></span>

    <span data-ttu-id="f5f5a-161">c.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-161">c.</span></span> <span data-ttu-id="f5f5a-162">A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<company name>.keylightgrc.com/Login.aspx`</span><span class="sxs-lookup"><span data-stu-id="f5f5a-162">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<company name>.keylightgrc.com/Login.aspx`</span></span>
    
    > [!NOTE] 
    > <span data-ttu-id="f5f5a-163">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-163">These values are not real.</span></span> <span data-ttu-id="f5f5a-164">Frissítse a azonosító, a válasz URL-CÍMEN és a bejelentkezési URL-cím a tényleges hello értékeket.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-164">Update these values with hello actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="f5f5a-165">Ügyfél [LockPath Keylight ügyfél-támogatási csoport](https://www.lockpath.com/contact/) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-165">Contact [LockPath Keylight Client support team](https://www.lockpath.com/contact/) tooget these values.</span></span> 

4. <span data-ttu-id="f5f5a-166">A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Raw)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-166">On hello **SAML Signing Certificate** section, click **Certificate(Raw)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_certificate.png) 

5. <span data-ttu-id="f5f5a-168">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-168">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-keylight-tutorial/tutorial_general_400.png)
    
6. <span data-ttu-id="f5f5a-170">A hello **LockPath Keylight konfigurációs** kattintson **LockPath Keylight konfigurálása** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-170">On hello **LockPath Keylight Configuration** section, click **Configure LockPath Keylight** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="f5f5a-171">Másolás hello **Sign-Out és SAML-alapú egyszeri bejelentkezés szolgáltatás URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="f5f5a-171">Copy hello **Sign-Out URL and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_configure.png) 

7. <span data-ttu-id="f5f5a-173">tooenable SSO LockPath Keylight, hajtsa végre a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="f5f5a-173">tooenable SSO in LockPath Keylight, perform hello following steps:</span></span>
   
    <span data-ttu-id="f5f5a-174">a.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-174">a.</span></span> <span data-ttu-id="f5f5a-175">Bejelentkezés tooyour LockPath Keylight fiók rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-175">Sign-on tooyour LockPath Keylight account as administrator.</span></span>
    
    <span data-ttu-id="f5f5a-176">b.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-176">b.</span></span> <span data-ttu-id="f5f5a-177">Hello hello felső menüben kattintson a **személy**, és válassza ki **Keylight telepítő**.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-177">In hello menu on hello top, click **Person**, and select **Keylight Setup**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-keylight-tutorial/401.png) 

    <span data-ttu-id="f5f5a-179">c.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-179">c.</span></span> <span data-ttu-id="f5f5a-180">Hello treeview hello bal oldalon, kattintson **SAML**.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-180">In hello treeview on hello left, click **SAML**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-keylight-tutorial/402.png) 

    <span data-ttu-id="f5f5a-182">d.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-182">d.</span></span> <span data-ttu-id="f5f5a-183">A hello **SAML beállítások** párbeszédpanel, kattintson a **szerkesztése**.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-183">On hello **SAML Settings** dialog, click **Edit**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-keylight-tutorial/404.png) 

8. <span data-ttu-id="f5f5a-185">A hello **SAML beállításainak szerkesztése** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="f5f5a-185">On hello **Edit SAML Settings** dialog page, perform hello following steps:</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-keylight-tutorial/405.png) 
   
    <span data-ttu-id="f5f5a-187">a.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-187">a.</span></span> <span data-ttu-id="f5f5a-188">Állítsa be **SAML-alapú hitelesítés** túl**aktív**.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-188">Set **SAML authentication** too**Active**.</span></span>

    <span data-ttu-id="f5f5a-189">b.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-189">b.</span></span> <span data-ttu-id="f5f5a-190">Beillesztés hello **SAML-alapú egyszeri bejelentkezési URL-címe** értéket, amelyet akkor másolta, az Azure-portálon hello hello **Identity Provider bejelentkezési URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-190">Paste hello **SAML Single Sign-On Service URL** value which you have copied from hello Azure portal into hello **Identity Provider Login URL** textbox.</span></span>

    <span data-ttu-id="f5f5a-191">c.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-191">c.</span></span> <span data-ttu-id="f5f5a-192">Beillesztés hello **egyetlen Sign-Out URL-címe** értéket, amelyet akkor másolta, az Azure-portálon hello hello **Identity Provider kijelentkezési URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-192">Paste hello **Single Sign-Out Service URL** value which you have copied from hello Azure portal into hello **Identity Provider Logout URL** textbox.</span></span>

    <span data-ttu-id="f5f5a-193">d.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-193">d.</span></span> <span data-ttu-id="f5f5a-194">Kattintson a **Choose File** tooselect a letöltött LockPath Keylight tanúsítvány, és kattintson a **nyitott** tooupload hello tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-194">Click **Choose File** tooselect your downloaded LockPath Keylight certificate, and then click **Open** tooupload hello certificate.</span></span>

    <span data-ttu-id="f5f5a-195">e.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-195">e.</span></span> <span data-ttu-id="f5f5a-196">Állítsa be **SAML felhasználóazonosító hely** túl**NameIdentifier elem hello tulajdonos utasítás**.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-196">Set **SAML User Id location** too**NameIdentifier element of hello subject statement**.</span></span>
    
    <span data-ttu-id="f5f5a-197">f.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-197">f.</span></span> <span data-ttu-id="f5f5a-198">Adja meg a hello **Keylight szolgáltató** hello mintát a következő használatával: **https://&lt;#companyname&gt;. keylightgrc.com**.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-198">Provide hello **Keylight Service Provider** using hello following pattern: **https://&lt;CompanyName&gt;.keylightgrc.com**.</span></span>
    
    <span data-ttu-id="f5f5a-199">g.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-199">g.</span></span> <span data-ttu-id="f5f5a-200">Állítsa be **automatikus-kiépítési felhasználók** túl**aktív**.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-200">Set **Auto-provision users** too**Active**.</span></span>

    <span data-ttu-id="f5f5a-201">h.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-201">h.</span></span> <span data-ttu-id="f5f5a-202">Állítsa be **automatikus-kiépítési fióktípus** túl**teljes felhasználói**.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-202">Set **Auto-provision account type** too**Full User**.</span></span>

    <span data-ttu-id="f5f5a-203">i.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-203">i.</span></span> <span data-ttu-id="f5f5a-204">Állítsa be **automatikus-kiépítési biztonsági szerepkör**, jelölje be **SAML az általános jogú felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-204">Set **Auto-provision security role**, select **Standard User with SAML**.</span></span>
    
    <span data-ttu-id="f5f5a-205">j.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-205">j.</span></span> <span data-ttu-id="f5f5a-206">Állítsa be **automatikus-kiépítési biztonsági config**, jelölje be **normál felhasználói konfigurációban**.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-206">Set **Auto-provision security config**, select **Standard User Configuration**.</span></span>
     
    <span data-ttu-id="f5f5a-207">k.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-207">k.</span></span> <span data-ttu-id="f5f5a-208">A hello **E-mail attribútum** szövegmezőhöz típus `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-208">In hello **Email attribute** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.</span></span>
    
    <span data-ttu-id="f5f5a-209">l.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-209">l.</span></span> <span data-ttu-id="f5f5a-210">A hello **Keresztnév attribútum** szövegmezőhöz típus `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-210">In hello **First name attribute** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`.</span></span>
    
    <span data-ttu-id="f5f5a-211">m.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-211">m.</span></span> <span data-ttu-id="f5f5a-212">A hello **utolsó name attribútum** szövegmezőhöz típus `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-212">In hello **Last name attribute** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`.</span></span>
    
    <span data-ttu-id="f5f5a-213">n.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-213">n.</span></span> <span data-ttu-id="f5f5a-214">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-214">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="f5f5a-215">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="f5f5a-215">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="f5f5a-216">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-216">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="f5f5a-217">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="f5f5a-217">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="f5f5a-218">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="f5f5a-218">Creating an Azure AD test user</span></span>
<span data-ttu-id="f5f5a-219">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-219">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="f5f5a-221">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="f5f5a-221">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="f5f5a-222">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-222">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-keylight-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="f5f5a-224">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-224">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-keylight-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="f5f5a-226">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-226">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-keylight-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="f5f5a-228">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="f5f5a-228">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-keylight-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="f5f5a-230">a.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-230">a.</span></span> <span data-ttu-id="f5f5a-231">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-231">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="f5f5a-232">b.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-232">b.</span></span> <span data-ttu-id="f5f5a-233">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-233">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="f5f5a-234">c.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-234">c.</span></span> <span data-ttu-id="f5f5a-235">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-235">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="f5f5a-236">d.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-236">d.</span></span> <span data-ttu-id="f5f5a-237">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-237">Click **Create**.</span></span>
 
### <a name="creating-a-lockpath-keylight-test-user"></a><span data-ttu-id="f5f5a-238">LockPath Keylight tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="f5f5a-238">Creating a LockPath Keylight test user</span></span>

<span data-ttu-id="f5f5a-239">Ebben a szakaszban egy LockPath Keylight Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-239">In this section, you create a user called Britta Simon in LockPath Keylight.</span></span> <span data-ttu-id="f5f5a-240">LockPath Keylight támogatja közvetlenül az időponthoz kötött kiosztást, amely alapértelmezés szerint engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-240">LockPath Keylight supports just-in-time provisioning, which is enabled by default.</span></span>

<span data-ttu-id="f5f5a-241">Nincs ebben a szakaszban az Ön művelet elem.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-241">There is no action item for you in this section.</span></span> <span data-ttu-id="f5f5a-242">Új felhasználó jön létre, LockPath Keylight elérésekor, ha hello felhasználó még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-242">A new user is created when accessing LockPath Keylight if hello user doesn't exist yet.</span></span> 

>[!NOTE]
><span data-ttu-id="f5f5a-243">A felhasználó toocreate manuálisan kell, ha szüksége van-e toocontact hello [LockPath Keylight ügyfél-támogatási csoport](https://www.lockpath.com/contact/).</span><span class="sxs-lookup"><span data-stu-id="f5f5a-243">If you need toocreate a user manually, you need toocontact hello [LockPath Keylight Client support team](https://www.lockpath.com/contact/).</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="f5f5a-244">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="f5f5a-244">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="f5f5a-245">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooLockPath Keylight megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-245">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooLockPath Keylight.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="f5f5a-247">**tooassign Britta Simon tooLockPath Keylight, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="f5f5a-247">**tooassign Britta Simon tooLockPath Keylight, perform hello following steps:**</span></span>

1. <span data-ttu-id="f5f5a-248">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-248">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="f5f5a-250">Hello alkalmazások listában válassza ki a **LockPath Keylight**.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-250">In hello applications list, select **LockPath Keylight**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_app.png) 

3. <span data-ttu-id="f5f5a-252">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-252">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="f5f5a-254">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-254">Click **Add** button.</span></span> <span data-ttu-id="f5f5a-255">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-255">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="f5f5a-257">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-257">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="f5f5a-258">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-258">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="f5f5a-259">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-259">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="f5f5a-260">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="f5f5a-260">Testing single sign-on</span></span>

<span data-ttu-id="f5f5a-261">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-261">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="f5f5a-262">Hello LockPath Keylight hello hozzáférési Panel csempére kattintva kapja meg automatikusan bejelentkezett tooyour LockPath Keylight alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="f5f5a-262">When you click hello LockPath Keylight tile in hello Access Panel, you should get automatically signed-on tooyour LockPath Keylight application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="f5f5a-263">További források</span><span class="sxs-lookup"><span data-stu-id="f5f5a-263">Additional resources</span></span>

* [<span data-ttu-id="f5f5a-264">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="f5f5a-264">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="f5f5a-265">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="f5f5a-265">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_203.png

