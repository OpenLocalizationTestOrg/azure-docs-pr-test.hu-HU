---
title: "Oktatóanyag: Azure Active Directoryval integrált QuickHelp |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és QuickHelp között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 655c9ad3-2076-4e2c-8e47-9ed3bf04be56
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/03/2017
ms.author: jeedes
ms.openlocfilehash: bbde5eb9bdad89680923ccd36c321b6923f91789
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-quickhelp"></a><span data-ttu-id="f06de-103">Oktatóanyag: Azure Active Directoryval integrált QuickHelp</span><span class="sxs-lookup"><span data-stu-id="f06de-103">Tutorial: Azure Active Directory integration with QuickHelp</span></span>

<span data-ttu-id="f06de-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate QuickHelp az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="f06de-104">In this tutorial, you learn how toointegrate QuickHelp with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="f06de-105">QuickHelp integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="f06de-105">Integrating QuickHelp with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="f06de-106">Megadhatja a hozzáférés tooQuickHelp rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="f06de-106">You can control in Azure AD who has access tooQuickHelp</span></span>
- <span data-ttu-id="f06de-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooQuickHelp (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="f06de-107">You can enable your users tooautomatically get signed-on tooQuickHelp (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="f06de-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="f06de-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="f06de-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="f06de-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f06de-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="f06de-110">Prerequisites</span></span>

<span data-ttu-id="f06de-111">az Azure AD integrálása QuickHelp tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="f06de-111">tooconfigure Azure AD integration with QuickHelp, you need hello following items:</span></span>

- <span data-ttu-id="f06de-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="f06de-112">An Azure AD subscription</span></span>
- <span data-ttu-id="f06de-113">Egy QuickHelp egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="f06de-113">A QuickHelp single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="f06de-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="f06de-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="f06de-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="f06de-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="f06de-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="f06de-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="f06de-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="f06de-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="f06de-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="f06de-118">Scenario description</span></span>
<span data-ttu-id="f06de-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="f06de-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="f06de-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="f06de-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="f06de-121">Hello gyűjteményből QuickHelp hozzáadása</span><span class="sxs-lookup"><span data-stu-id="f06de-121">Adding QuickHelp from hello gallery</span></span>
2. <span data-ttu-id="f06de-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="f06de-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-quickhelp-from-hello-gallery"></a><span data-ttu-id="f06de-123">Hello gyűjteményből QuickHelp hozzáadása</span><span class="sxs-lookup"><span data-stu-id="f06de-123">Adding QuickHelp from hello gallery</span></span>
<span data-ttu-id="f06de-124">tooconfigure hello integrációja QuickHelp az Azure AD-be, meg kell tooadd QuickHelp hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="f06de-124">tooconfigure hello integration of QuickHelp into Azure AD, you need tooadd QuickHelp from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="f06de-125">**tooadd QuickHelp hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="f06de-125">**tooadd QuickHelp from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="f06de-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="f06de-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="f06de-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="f06de-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="f06de-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="f06de-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="f06de-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="f06de-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="f06de-133">Hello keresési mezőbe, írja be a **QuickHelp**.</span><span class="sxs-lookup"><span data-stu-id="f06de-133">In hello search box, type **QuickHelp**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_search.png)

5. <span data-ttu-id="f06de-135">A hello eredmények panelen válassza ki a **QuickHelp**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="f06de-135">In hello results panel, select **QuickHelp**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="f06de-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="f06de-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="f06de-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján QuickHelp.</span><span class="sxs-lookup"><span data-stu-id="f06de-138">In this section, you configure and test Azure AD single sign-on with QuickHelp based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="f06de-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó QuickHelp tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="f06de-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in QuickHelp is tooa user in Azure AD.</span></span> <span data-ttu-id="f06de-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello QuickHelp közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="f06de-140">In other words, a link relationship between an Azure AD user and hello related user in QuickHelp needs toobe established.</span></span>

<span data-ttu-id="f06de-141">QuickHelp, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="f06de-141">In QuickHelp, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="f06de-142">tooconfigure és az Azure AD az egyszeri bejelentkezés QuickHelp-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="f06de-142">tooconfigure and test Azure AD single sign-on with QuickHelp, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="f06de-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="f06de-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="f06de-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="f06de-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="f06de-145">**[QuickHelp tesztfelhasználó létrehozása](#creating-a-quickhelp-test-user)**  -toohave egy megfelelője a Britta Simon a QuickHelp, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="f06de-145">**[Creating a QuickHelp test user](#creating-a-quickhelp-test-user)** - toohave a counterpart of Britta Simon in QuickHelp that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="f06de-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="f06de-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="f06de-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="f06de-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="f06de-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f06de-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="f06de-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az QuickHelp alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="f06de-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your QuickHelp application.</span></span>

<span data-ttu-id="f06de-150">**az Azure AD tooconfigure egyszeri bejelentkezést a QuickHelp, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="f06de-150">**tooconfigure Azure AD single sign-on with QuickHelp, perform hello following steps:**</span></span>

1. <span data-ttu-id="f06de-151">Az Azure portál, a hello hello **QuickHelp** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="f06de-151">In hello Azure portal, on hello **QuickHelp** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="f06de-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="f06de-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_samlbase.png)

3. <span data-ttu-id="f06de-155">A hello **QuickHelp tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="f06de-155">On hello **QuickHelp Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_url.png)

    <span data-ttu-id="f06de-157">a.</span><span class="sxs-lookup"><span data-stu-id="f06de-157">a.</span></span> <span data-ttu-id="f06de-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://quickhelp.com/<instancename>/#/Login`</span><span class="sxs-lookup"><span data-stu-id="f06de-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://quickhelp.com/<instancename>/#/Login`</span></span>

    <span data-ttu-id="f06de-159">b.</span><span class="sxs-lookup"><span data-stu-id="f06de-159">b.</span></span> <span data-ttu-id="f06de-160">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<subdomain>.quickhelp.com`</span><span class="sxs-lookup"><span data-stu-id="f06de-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<subdomain>.quickhelp.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="f06de-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="f06de-161">These values are not real.</span></span> <span data-ttu-id="f06de-162">Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="f06de-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="f06de-163">Ügyfél [QuickHelp ügyfél-támogatási csoport](https://support.quickhelp.com/) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="f06de-163">Contact [QuickHelp Client support team](https://support.quickhelp.com/) tooget these values.</span></span> 
 
4. <span data-ttu-id="f06de-164">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="f06de-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_certificate.png) 

5. <span data-ttu-id="f06de-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="f06de-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-quickhelp-tutorial/tutorial_general_400.png) 

6. <span data-ttu-id="f06de-168">Bejelentkezés tooyour QuickHelp vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="f06de-168">Sign-on tooyour QuickHelp company site as administrator.</span></span>

7. <span data-ttu-id="f06de-169">Hello hello felső menüben kattintson a **Admin**.</span><span class="sxs-lookup"><span data-stu-id="f06de-169">In hello menu on hello top, click **Admin**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása][21]

8. <span data-ttu-id="f06de-171">A hello **QuickHelp Admin** menüben kattintson a **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="f06de-171">In hello **QuickHelp Admin** menu, click **Settings**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása][22]

9. <span data-ttu-id="f06de-173">Kattintson a **hitelesítési beállítások**.</span><span class="sxs-lookup"><span data-stu-id="f06de-173">Click **Authentication Settings**.</span></span>

10. <span data-ttu-id="f06de-174">A hello **hitelesítési beállítások** lapon, hajtsa végre az alábbi lépésekkel hello</span><span class="sxs-lookup"><span data-stu-id="f06de-174">On hello **Authentication Settings** page, perform hello following steps</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása][23]
   
    <span data-ttu-id="f06de-176">a.</span><span class="sxs-lookup"><span data-stu-id="f06de-176">a.</span></span> <span data-ttu-id="f06de-177">Mint **egyszeri bejelentkezés típusa**, jelölje be **WSFederation**.</span><span class="sxs-lookup"><span data-stu-id="f06de-177">As **SSO Type**, select **WSFederation**.</span></span>
   
    <span data-ttu-id="f06de-178">b.</span><span class="sxs-lookup"><span data-stu-id="f06de-178">b.</span></span> <span data-ttu-id="f06de-179">tooupload a letöltött Azure metaadatfájl kattintson **Tallózás**toohello fájl lépjen, befejezéséhez kattintson **metaadatok feltöltése**.</span><span class="sxs-lookup"><span data-stu-id="f06de-179">tooupload your downloaded Azure metadata file, click **Browse**, navigate toohello file, end then click **Upload Metadata**.</span></span>
   
    <span data-ttu-id="f06de-180">c.</span><span class="sxs-lookup"><span data-stu-id="f06de-180">c.</span></span> <span data-ttu-id="f06de-181">A hello **E-mail** szövegmezőhöz típus `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.</span><span class="sxs-lookup"><span data-stu-id="f06de-181">In hello **Email** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.</span></span>
   
    <span data-ttu-id="f06de-182">d.</span><span class="sxs-lookup"><span data-stu-id="f06de-182">d.</span></span> <span data-ttu-id="f06de-183">A hello **Utónév** szövegmezőhöz `type http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`.</span><span class="sxs-lookup"><span data-stu-id="f06de-183">In hello **First Name** textbox, `type http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`.</span></span>
   
    <span data-ttu-id="f06de-184">e.</span><span class="sxs-lookup"><span data-stu-id="f06de-184">e.</span></span> <span data-ttu-id="f06de-185">A hello **Vezetéknév** szövegmezőhöz `type http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`.</span><span class="sxs-lookup"><span data-stu-id="f06de-185">In hello **Last Name** textbox, `type http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`.</span></span>
   
    <span data-ttu-id="f06de-186">f.</span><span class="sxs-lookup"><span data-stu-id="f06de-186">f.</span></span> <span data-ttu-id="f06de-187">A hello **műveletsávon**, kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="f06de-187">In hello **Action Bar**, click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="f06de-188">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="f06de-188">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="f06de-189">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="f06de-189">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="f06de-190">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="f06de-190">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="f06de-191">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="f06de-191">Creating an Azure AD test user</span></span>
<span data-ttu-id="f06de-192">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="f06de-192">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="f06de-194">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="f06de-194">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="f06de-195">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="f06de-195">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-quickhelp-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="f06de-197">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="f06de-197">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-quickhelp-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="f06de-199">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="f06de-199">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-quickhelp-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="f06de-201">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="f06de-201">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-quickhelp-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="f06de-203">a.</span><span class="sxs-lookup"><span data-stu-id="f06de-203">a.</span></span> <span data-ttu-id="f06de-204">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="f06de-204">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="f06de-205">b.</span><span class="sxs-lookup"><span data-stu-id="f06de-205">b.</span></span> <span data-ttu-id="f06de-206">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="f06de-206">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="f06de-207">c.</span><span class="sxs-lookup"><span data-stu-id="f06de-207">c.</span></span> <span data-ttu-id="f06de-208">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="f06de-208">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="f06de-209">d.</span><span class="sxs-lookup"><span data-stu-id="f06de-209">d.</span></span> <span data-ttu-id="f06de-210">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="f06de-210">Click **Create**.</span></span>
 
### <a name="creating-a-quickhelp-test-user"></a><span data-ttu-id="f06de-211">QuickHelp tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="f06de-211">Creating a QuickHelp test user</span></span>

<span data-ttu-id="f06de-212">hello ebben a szakaszban célja toocreate QuickHelp Britta Simon nevű felhasználó.</span><span class="sxs-lookup"><span data-stu-id="f06de-212">hello objective of this section is toocreate a user called Britta Simon in QuickHelp.</span></span>
<span data-ttu-id="f06de-213">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó QuickHelp tooa felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="f06de-213">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in QuickHelp tooa user in Azure AD is.</span></span> <span data-ttu-id="f06de-214">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello QuickHelp közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="f06de-214">In other words, a link relationship between an Azure AD user and hello related user in QuickHelp needs toobe established.</span></span>

<span data-ttu-id="f06de-215">QuickHelp támogatja közvetlenül az időponthoz kötött kiosztást.</span><span class="sxs-lookup"><span data-stu-id="f06de-215">QuickHelp supports just-in-time provisioning.</span></span> <span data-ttu-id="f06de-216">Ez azt jelenti, ha szükséges, egy felhasználói fiókot a rendszer automatikusan létrehoz egyet QuickHelp és hello fiók csatolt toohello Azure AD-fiókot.</span><span class="sxs-lookup"><span data-stu-id="f06de-216">This means, if necessary, a user account is automatically created in QuickHelp and hello account is linked toohello Azure AD account.</span></span>

<span data-ttu-id="f06de-217">Nincs ebben a szakaszban az Ön művelet elem.</span><span class="sxs-lookup"><span data-stu-id="f06de-217">There is no action item for you in this section.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="f06de-218">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="f06de-218">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="f06de-219">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooQuickHelp megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="f06de-219">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooQuickHelp.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="f06de-221">**tooassign Britta Simon tooQuickHelp, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="f06de-221">**tooassign Britta Simon tooQuickHelp, perform hello following steps:**</span></span>

1. <span data-ttu-id="f06de-222">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="f06de-222">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="f06de-224">Hello alkalmazások listában válassza ki a **QuickHelp**.</span><span class="sxs-lookup"><span data-stu-id="f06de-224">In hello applications list, select **QuickHelp**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_app.png) 

3. <span data-ttu-id="f06de-226">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="f06de-226">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="f06de-228">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="f06de-228">Click **Add** button.</span></span> <span data-ttu-id="f06de-229">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="f06de-229">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="f06de-231">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="f06de-231">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="f06de-232">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="f06de-232">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="f06de-233">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="f06de-233">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="f06de-234">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="f06de-234">Testing single sign-on</span></span>

<span data-ttu-id="f06de-235">hello ebben a szakaszban célja tootest az egyszeri bejelentkezés konfigurációs használatával hello a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="f06de-235">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>  

<span data-ttu-id="f06de-236">Ha a hozzáférési Panel hello hello QuickHelp csempe gombra kattint, automatikusan bejelentkezett tooyour QuickHelp alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="f06de-236">When you click hello QuickHelp tile in hello Access Panel, you should get automatically signed-on tooyour QuickHelp application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="f06de-237">További források</span><span class="sxs-lookup"><span data-stu-id="f06de-237">Additional resources</span></span>

* [<span data-ttu-id="f06de-238">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="f06de-238">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="f06de-239">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="f06de-239">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_203.png
[21]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_05.png
[22]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_06.png
[23]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_07.png
