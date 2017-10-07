---
title: "Oktatóanyag: Azure Active Directoryval integrált LearnUpon |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és LearnUpon között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: b11c6315-c79d-4f34-9610-bd17070ab7c7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: fdb9c62172327a539f0459c98aa20e63fa441e4b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-learnupon"></a><span data-ttu-id="028f6-103">Oktatóanyag: Azure Active Directoryval integrált LearnUpon</span><span class="sxs-lookup"><span data-stu-id="028f6-103">Tutorial: Azure Active Directory integration with LearnUpon</span></span>

<span data-ttu-id="028f6-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate LearnUpon az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="028f6-104">In this tutorial, you learn how toointegrate LearnUpon with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="028f6-105">LearnUpon integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="028f6-105">Integrating LearnUpon with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="028f6-106">Megadhatja a hozzáférés tooLearnUpon rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="028f6-106">You can control in Azure AD who has access tooLearnUpon</span></span>
- <span data-ttu-id="028f6-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooLearnUpon (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="028f6-107">You can enable your users tooautomatically get signed-on tooLearnUpon (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="028f6-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="028f6-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="028f6-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="028f6-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="028f6-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="028f6-110">Prerequisites</span></span>

<span data-ttu-id="028f6-111">az Azure AD integrálása LearnUpon tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="028f6-111">tooconfigure Azure AD integration with LearnUpon, you need hello following items:</span></span>

- <span data-ttu-id="028f6-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="028f6-112">An Azure AD subscription</span></span>
- <span data-ttu-id="028f6-113">Egy LearnUpon egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="028f6-113">A LearnUpon single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="028f6-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="028f6-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="028f6-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="028f6-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="028f6-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="028f6-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="028f6-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="028f6-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="028f6-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="028f6-118">Scenario description</span></span>
<span data-ttu-id="028f6-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="028f6-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="028f6-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="028f6-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="028f6-121">Hello gyűjteményből LearnUpon hozzáadása</span><span class="sxs-lookup"><span data-stu-id="028f6-121">Adding LearnUpon from hello gallery</span></span>
2. <span data-ttu-id="028f6-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="028f6-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-learnupon-from-hello-gallery"></a><span data-ttu-id="028f6-123">Hello gyűjteményből LearnUpon hozzáadása</span><span class="sxs-lookup"><span data-stu-id="028f6-123">Adding LearnUpon from hello gallery</span></span>
<span data-ttu-id="028f6-124">tooconfigure hello integrációja LearnUpon az Azure AD-be, meg kell tooadd LearnUpon hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="028f6-124">tooconfigure hello integration of LearnUpon into Azure AD, you need tooadd LearnUpon from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="028f6-125">**tooadd LearnUpon hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="028f6-125">**tooadd LearnUpon from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="028f6-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="028f6-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="028f6-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="028f6-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="028f6-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="028f6-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="028f6-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="028f6-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="028f6-133">Hello keresési mezőbe, írja be a **LearnUpon**.</span><span class="sxs-lookup"><span data-stu-id="028f6-133">In hello search box, type **LearnUpon**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_search.png)

5. <span data-ttu-id="028f6-135">A hello eredmények panelen válassza ki a **LearnUpon**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="028f6-135">In hello results panel, select **LearnUpon**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="028f6-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="028f6-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="028f6-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján LearnUpon.</span><span class="sxs-lookup"><span data-stu-id="028f6-138">In this section, you configure and test Azure AD single sign-on with LearnUpon based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="028f6-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó LearnUpon tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="028f6-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in LearnUpon is tooa user in Azure AD.</span></span> <span data-ttu-id="028f6-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello LearnUpon közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="028f6-140">In other words, a link relationship between an Azure AD user and hello related user in LearnUpon needs toobe established.</span></span>

<span data-ttu-id="028f6-141">LearnUpon, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="028f6-141">In LearnUpon, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="028f6-142">tooconfigure és az Azure AD az egyszeri bejelentkezés LearnUpon-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="028f6-142">tooconfigure and test Azure AD single sign-on with LearnUpon, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="028f6-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="028f6-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="028f6-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="028f6-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="028f6-145">**[LearnUpon tesztfelhasználó létrehozása](#creating-a-learnupon-test-user)**  -toohave egy megfelelője a Britta Simon a LearnUpon, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="028f6-145">**[Creating a LearnUpon test user](#creating-a-learnupon-test-user)** - toohave a counterpart of Britta Simon in LearnUpon that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="028f6-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="028f6-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="028f6-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="028f6-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="028f6-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="028f6-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="028f6-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az LearnUpon alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="028f6-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your LearnUpon application.</span></span>

<span data-ttu-id="028f6-150">**az Azure AD tooconfigure egyszeri bejelentkezést a LearnUpon, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="028f6-150">**tooconfigure Azure AD single sign-on with LearnUpon, perform hello following steps:**</span></span>

1. <span data-ttu-id="028f6-151">Az Azure portál, a hello hello **LearnUpon** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="028f6-151">In hello Azure portal, on hello **LearnUpon** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="028f6-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="028f6-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_samlbase.png)

3. <span data-ttu-id="028f6-155">A hello **LearnUpon tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="028f6-155">On hello **LearnUpon Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_url.png)

    <span data-ttu-id="028f6-157">A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<companyname>.learnupon.com/saml/consumer`</span><span class="sxs-lookup"><span data-stu-id="028f6-157">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<companyname>.learnupon.com/saml/consumer`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="028f6-158">Ne feledje, hogy ez a nem hello valódi értékek.</span><span class="sxs-lookup"><span data-stu-id="028f6-158">Please note that this is not hello real value.</span></span> <span data-ttu-id="028f6-159">tooupdate ezt az értéket hello tényleges válasz URL-címmel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="028f6-159">you have tooupdate this value with hello actual Reply URL.</span></span> <span data-ttu-id="028f6-160">tooget ezt az értéket forduljon [LearnUpon támogatási csoport](https://www.learnupon.com/features/support/).</span><span class="sxs-lookup"><span data-stu-id="028f6-160">tooget this value Contact [LearnUpon support team](https://www.learnupon.com/features/support/).</span></span>



4. <span data-ttu-id="028f6-161">A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Raw)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="028f6-161">On hello **SAML Signing Certificate** section, click **Certificate (Raw)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_certificate.png) 

5. <span data-ttu-id="028f6-163">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="028f6-163">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-learnupon-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="028f6-165">A hello **LearnUpon konfigurációs** kattintson **konfigurálása LearnUpon** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="028f6-165">On hello **LearnUpon Configuration** section, click **Configure LearnUpon** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="028f6-166">Másolás hello **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="028f6-166">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_configure.png) 

7. <span data-ttu-id="028f6-168">Nyisson meg egy másik böngészőben példány és a bejelentkezési LearnUpon be rendszergazdai fiókkal.</span><span class="sxs-lookup"><span data-stu-id="028f6-168">Open another browser instance and login into LearnUpon with an administrator account.</span></span> 

8. <span data-ttu-id="028f6-169">Kattintson a hello **beállítások** fülre.</span><span class="sxs-lookup"><span data-stu-id="028f6-169">Click hello **settings** tab.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_06.png)

9. <span data-ttu-id="028f6-171">Kattintson a **egyszeri bejelentkezés – SAML**, és kattintson a **általános beállítások** tooconfigure SAML-beállításokat.</span><span class="sxs-lookup"><span data-stu-id="028f6-171">Click **Single Sign On - SAML**, and then click **General Settings** tooconfigure SAML settings.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_07.png) 

10. <span data-ttu-id="028f6-173">A hello **általános beállítások** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="028f6-173">In hello **General Settings** section, perform hello following steps:</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_08.png)  
  
    <span data-ttu-id="028f6-175">a.</span><span class="sxs-lookup"><span data-stu-id="028f6-175">a.</span></span> <span data-ttu-id="028f6-176">Válassza ki **engedélyezett**.</span><span class="sxs-lookup"><span data-stu-id="028f6-176">Select **Enabled**.</span></span>

    <span data-ttu-id="028f6-177">b.</span><span class="sxs-lookup"><span data-stu-id="028f6-177">b.</span></span> <span data-ttu-id="028f6-178">Válassza ki **verzió** , **2.0**.</span><span class="sxs-lookup"><span data-stu-id="028f6-178">Select **Version** as **2.0**.</span></span>

    <span data-ttu-id="028f6-179">c.</span><span class="sxs-lookup"><span data-stu-id="028f6-179">c.</span></span> <span data-ttu-id="028f6-180">Válassza ki **feltételek kihagyása** , **nem**.</span><span class="sxs-lookup"><span data-stu-id="028f6-180">Select **Skip conditions** as **No**.</span></span>

    <span data-ttu-id="028f6-181">d.</span><span class="sxs-lookup"><span data-stu-id="028f6-181">d.</span></span> <span data-ttu-id="028f6-182">A hello **SAML-jogkivonat utáni param neve** szövegmezőhöz kérelem feladás egy vagy több paraméter toohello SAML-alapú ügyfél URL-címe a fenti hello SAML-előfeltétel toobe ellenőrzése és a hitelesített – például tartalmazó hello nevét  **SAMLResponse**.</span><span class="sxs-lookup"><span data-stu-id="028f6-182">In hello **SAML Token Post param name** textbox, type hello name of request post parameter toohello SAML consumer URL indicated above that contains hello SAML Assertion toobe verified and authenticated - for example **SAMLResponse**.</span></span>

    <span data-ttu-id="028f6-183">e.</span><span class="sxs-lookup"><span data-stu-id="028f6-183">e.</span></span> <span data-ttu-id="028f6-184">A hello **azonosító formátuma** szövegmezőhöz típus hello érték, amely jelzi, ha a SAML-előfeltétel hello felhasználók azonosítója (E-mail címet) található - például **urn: oasis: nevek: tc: SAML:1.1:nameid-formátum: e-mail cím**.</span><span class="sxs-lookup"><span data-stu-id="028f6-184">In hello **Name Identifier Format** textbox, type hello value that indicates where in your SAML Assertion hello users identifier (Email address) resides - for example **urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress**.</span></span>
  
    <span data-ttu-id="028f6-185">f.</span><span class="sxs-lookup"><span data-stu-id="028f6-185">f.</span></span> <span data-ttu-id="028f6-186">A hello **azonosítása szolgáltató helye** szövegmező, a típus hello érték, amely jelzi, ha hello felhasználóknak legyenek elküldve tooif a feltöltött ikonra az Azure portál bejelentkezési képernyőjéről kattintanak.</span><span class="sxs-lookup"><span data-stu-id="028f6-186">In hello **Identify Provider Location** textbox, type hello value that indicates where hello users are sent tooif they click on your uploaded icon from your Azure portal login screen.</span></span>
  
    <span data-ttu-id="028f6-187">g.</span><span class="sxs-lookup"><span data-stu-id="028f6-187">g.</span></span> <span data-ttu-id="028f6-188">A hello **jelentkezzen ki az URL-cím** szövegmezőhöz Beillesztés hello **Sign-Out URL-cím** , amely az Azure-portálon hello másolta.</span><span class="sxs-lookup"><span data-stu-id="028f6-188">In hello **Sign out URL** textbox, paste hello **Sign-Out URL** which you have copied from hello Azure portal.</span></span>
    
    <span data-ttu-id="028f6-189">h.</span><span class="sxs-lookup"><span data-stu-id="028f6-189">h.</span></span> <span data-ttu-id="028f6-190">Kattintson a **ujját megrendelése kezelése**, majd töltse fel a letöltött tanúsítvány hello ujjlenyomat.</span><span class="sxs-lookup"><span data-stu-id="028f6-190">Click **Manage finger prints**, and then upload hello finger print of your downloaded certificate.</span></span>

11. <span data-ttu-id="028f6-191">Kattintson a **felhasználói beállítások**, és hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="028f6-191">Click **User Settings**, and then perform hello following steps:</span></span>
   
     ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_11.png)  
 
    <span data-ttu-id="028f6-193">a.</span><span class="sxs-lookup"><span data-stu-id="028f6-193">a.</span></span> <span data-ttu-id="028f6-194">A hello **Keresztnév azonosító formátuma** szövegmezőhöz típus hello érték, amely közli, amennyiben az a SAML-előfeltétel hello felhasználók Keresztnév található – például: **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.</span><span class="sxs-lookup"><span data-stu-id="028f6-194">In hello **First Name Identifier Format** textbox, type hello value that tells us where in your SAML Assertion hello users firstname resides - for example: **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.</span></span>
  
    <span data-ttu-id="028f6-195">b.</span><span class="sxs-lookup"><span data-stu-id="028f6-195">b.</span></span> <span data-ttu-id="028f6-196">A hello **utolsó azonosító formátuma** szövegmezőhöz típus hello érték, amely közli, amennyiben az a SAML-előfeltétel hello felhasználók Vezetéknév található – például: **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**.</span><span class="sxs-lookup"><span data-stu-id="028f6-196">In hello **Last Name Identifier Format** textbox, type hello value that tells us where in your SAML Assertion hello users lastname resides - for example: **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**.</span></span>

> [!TIP]
> <span data-ttu-id="028f6-197">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="028f6-197">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="028f6-198">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="028f6-198">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="028f6-199">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="028f6-199">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="028f6-200">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="028f6-200">Creating an Azure AD test user</span></span>
<span data-ttu-id="028f6-201">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="028f6-201">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="028f6-203">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="028f6-203">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="028f6-204">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="028f6-204">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-learnupon-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="028f6-206">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="028f6-206">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-learnupon-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="028f6-208">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="028f6-208">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-learnupon-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="028f6-210">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="028f6-210">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-learnupon-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="028f6-212">a.</span><span class="sxs-lookup"><span data-stu-id="028f6-212">a.</span></span> <span data-ttu-id="028f6-213">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="028f6-213">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="028f6-214">b.</span><span class="sxs-lookup"><span data-stu-id="028f6-214">b.</span></span> <span data-ttu-id="028f6-215">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="028f6-215">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="028f6-216">c.</span><span class="sxs-lookup"><span data-stu-id="028f6-216">c.</span></span> <span data-ttu-id="028f6-217">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="028f6-217">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="028f6-218">d.</span><span class="sxs-lookup"><span data-stu-id="028f6-218">d.</span></span> <span data-ttu-id="028f6-219">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="028f6-219">Click **Create**.</span></span>
 
### <a name="creating-a-learnupon-test-user"></a><span data-ttu-id="028f6-220">LearnUpon tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="028f6-220">Creating a LearnUpon test user</span></span>

<span data-ttu-id="028f6-221">hello ebben a szakaszban célja toocreate LearnUpon Britta Simon nevű felhasználó.</span><span class="sxs-lookup"><span data-stu-id="028f6-221">hello objective of this section is toocreate a user called Britta Simon in LearnUpon.</span></span> <span data-ttu-id="028f6-222">LearnUpon támogatja just-in-time kiosztást, amely alapértelmezés szerint van engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="028f6-222">LearnUpon supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="028f6-223">Nincs ebben a szakaszban az Ön művelet elem.</span><span class="sxs-lookup"><span data-stu-id="028f6-223">There is no action item for you in this section.</span></span> <span data-ttu-id="028f6-224">Új felhasználó létrejön egy kísérlet tooaccess LearnUpon során, ha még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="028f6-224">A new user will be created during an attempt tooaccess LearnUpon if it doesn't exist yet.</span></span> <span data-ttu-id="028f6-225">[Az Azure AD-egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-single-sign-on).</span><span class="sxs-lookup"><span data-stu-id="028f6-225">[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-single-sign-on).</span></span>

>[!NOTE]
><span data-ttu-id="028f6-226">Ha egy felhasználó toocreate manuálisan kell, toocontact kell [LearnUpon támogatási csoport](https://www.learnupon.com/features/support/).</span><span class="sxs-lookup"><span data-stu-id="028f6-226">If you need toocreate an user manually, you need toocontact [LearnUpon support team](https://www.learnupon.com/features/support/).</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="028f6-227">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="028f6-227">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="028f6-228">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooLearnUpon megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="028f6-228">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooLearnUpon.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="028f6-230">**tooassign Britta Simon tooLearnUpon, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="028f6-230">**tooassign Britta Simon tooLearnUpon, perform hello following steps:**</span></span>

1. <span data-ttu-id="028f6-231">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="028f6-231">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="028f6-233">Hello alkalmazások listában válassza ki a **LearnUpon**.</span><span class="sxs-lookup"><span data-stu-id="028f6-233">In hello applications list, select **LearnUpon**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_app.png) 

3. <span data-ttu-id="028f6-235">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="028f6-235">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="028f6-237">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="028f6-237">Click **Add** button.</span></span> <span data-ttu-id="028f6-238">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="028f6-238">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="028f6-240">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="028f6-240">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="028f6-241">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="028f6-241">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="028f6-242">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="028f6-242">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="028f6-243">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="028f6-243">Testing single sign-on</span></span>

<span data-ttu-id="028f6-244">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="028f6-244">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="028f6-245">Ha a hozzáférési Panel hello hello LearnUpon csempe gombra kattint, automatikusan bejelentkezett tooyour LearnUpon alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="028f6-245">When you click hello LearnUpon tile in hello Access Panel, you should get automatically signed-on tooyour LearnUpon application.</span></span>
<span data-ttu-id="028f6-246">A hozzáférési Panel kapcsolatos további információkért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="028f6-246">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="028f6-247">További források</span><span class="sxs-lookup"><span data-stu-id="028f6-247">Additional resources</span></span>

* [<span data-ttu-id="028f6-248">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="028f6-248">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="028f6-249">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="028f6-249">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_203.png

