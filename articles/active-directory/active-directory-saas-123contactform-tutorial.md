---
title: "Oktatóanyag: Azure Active Directoryval integrált 123ContactForm |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és 123ContactForm között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 5211910a-ab96-4709-959a-524c4d57c43e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: 931255887845edd1aa7f53b9051a82a2f898e055
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-123contactform"></a><span data-ttu-id="a6fe5-103">Oktatóanyag: Azure Active Directoryval integrált 123ContactForm</span><span class="sxs-lookup"><span data-stu-id="a6fe5-103">Tutorial: Azure Active Directory integration with 123ContactForm</span></span>

<span data-ttu-id="a6fe5-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate 123ContactForm az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="a6fe5-104">In this tutorial, you learn how toointegrate 123ContactForm with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="a6fe5-105">123ContactForm integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="a6fe5-105">Integrating 123ContactForm with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="a6fe5-106">Megadhatja a hozzáférés too123ContactForm rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="a6fe5-106">You can control in Azure AD who has access too123ContactForm</span></span>
- <span data-ttu-id="a6fe5-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett too123ContactForm (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="a6fe5-107">You can enable your users tooautomatically get signed-on too123ContactForm (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="a6fe5-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="a6fe5-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="a6fe5-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="a6fe5-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a6fe5-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="a6fe5-110">Prerequisites</span></span>

<span data-ttu-id="a6fe5-111">az Azure AD integrálása 123ContactForm tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="a6fe5-111">tooconfigure Azure AD integration with 123ContactForm, you need hello following items:</span></span>

- <span data-ttu-id="a6fe5-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="a6fe5-112">An Azure AD subscription</span></span>
- <span data-ttu-id="a6fe5-113">Egy 123ContactForm egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="a6fe5-113">A 123ContactForm single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="a6fe5-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="a6fe5-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="a6fe5-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="a6fe5-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="a6fe5-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="a6fe5-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="a6fe5-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a6fe5-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="a6fe5-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="a6fe5-118">Scenario description</span></span>
<span data-ttu-id="a6fe5-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="a6fe5-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="a6fe5-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="a6fe5-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="a6fe5-121">Hello gyűjteményből 123ContactForm hozzáadása</span><span class="sxs-lookup"><span data-stu-id="a6fe5-121">Adding 123ContactForm from hello gallery</span></span>
2. <span data-ttu-id="a6fe5-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="a6fe5-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-123contactform-from-hello-gallery"></a><span data-ttu-id="a6fe5-123">Hello gyűjteményből 123ContactForm hozzáadása</span><span class="sxs-lookup"><span data-stu-id="a6fe5-123">Adding 123ContactForm from hello gallery</span></span>
<span data-ttu-id="a6fe5-124">tooconfigure hello integrációja 123ContactForm az Azure AD-be, meg kell tooadd 123ContactForm hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="a6fe5-124">tooconfigure hello integration of 123ContactForm into Azure AD, you need tooadd 123ContactForm from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="a6fe5-125">**tooadd 123ContactForm hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="a6fe5-125">**tooadd 123ContactForm from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="a6fe5-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="a6fe5-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="a6fe5-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="a6fe5-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="a6fe5-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="a6fe5-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="a6fe5-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="a6fe5-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="a6fe5-133">Hello keresési mezőbe, írja be a **123ContactForm**.</span><span class="sxs-lookup"><span data-stu-id="a6fe5-133">In hello search box, type **123ContactForm**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-123contactform-tutorial/tutorial_123contactform_search.png)

5. <span data-ttu-id="a6fe5-135">A hello eredmények panelen válassza ki a **123ContactForm**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="a6fe5-135">In hello results panel, select **123ContactForm**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-123contactform-tutorial/tutorial_123contactform_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="a6fe5-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="a6fe5-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="a6fe5-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján 123ContactForm</span><span class="sxs-lookup"><span data-stu-id="a6fe5-138">In this section, you configure and test Azure AD single sign-on with 123ContactForm based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="a6fe5-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó 123ContactForm tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="a6fe5-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in 123ContactForm is tooa user in Azure AD.</span></span> <span data-ttu-id="a6fe5-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello 123ContactForm közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="a6fe5-140">In other words, a link relationship between an Azure AD user and hello related user in 123ContactForm needs toobe established.</span></span>

<span data-ttu-id="a6fe5-141">123ContactForm, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="a6fe5-141">In 123ContactForm, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="a6fe5-142">tooconfigure és az Azure AD az egyszeri bejelentkezés 123ContactForm-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="a6fe5-142">tooconfigure and test Azure AD single sign-on with 123ContactForm, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="a6fe5-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="a6fe5-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="a6fe5-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a6fe5-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="a6fe5-145">**[123ContactForm tesztfelhasználó létrehozása](#creating-a-123contactform-test-user)**  -toohave Britta Simon egy partner, a 123ContactForm, amely a felhasználó csatolt toohello az Azure AD-ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="a6fe5-145">**[Creating a 123ContactForm test user](#creating-a-123contactform-test-user)** - toohave a counterpart of Britta Simon in 123ContactForm that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="a6fe5-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="a6fe5-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="a6fe5-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="a6fe5-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="a6fe5-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="a6fe5-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="a6fe5-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az 123ContactForm alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="a6fe5-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your 123ContactForm application.</span></span>

<span data-ttu-id="a6fe5-150">**az Azure AD tooconfigure egyszeri bejelentkezést a 123ContactForm, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="a6fe5-150">**tooconfigure Azure AD single sign-on with 123ContactForm, perform hello following steps:**</span></span>

1. <span data-ttu-id="a6fe5-151">Az Azure portál, a hello hello **123ContactForm** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="a6fe5-151">In hello Azure portal, on hello **123ContactForm** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="a6fe5-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="a6fe5-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-123contactform-tutorial/tutorial_123contactform_samlbase.png)

3. <span data-ttu-id="a6fe5-155">A hello **123ContactForm tartomány és az URL-címek** szakaszban, ha tooconfigure hello alkalmazás **IDP kezdeményezett mód**, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="a6fe5-155">On hello **123ContactForm Domain and URLs** section, If you wish tooconfigure hello application in **IDP initiated mode**, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-123contactform-tutorial/url1.png)

    <span data-ttu-id="a6fe5-157">a.</span><span class="sxs-lookup"><span data-stu-id="a6fe5-157">a.</span></span> <span data-ttu-id="a6fe5-158">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://www.123contactform.com/saml/azure_ad/<tenant_id>/metadata`</span><span class="sxs-lookup"><span data-stu-id="a6fe5-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://www.123contactform.com/saml/azure_ad/<tenant_id>/metadata`</span></span>

    <span data-ttu-id="a6fe5-159">b.</span><span class="sxs-lookup"><span data-stu-id="a6fe5-159">b.</span></span> <span data-ttu-id="a6fe5-160">A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://www.123contactform.com/saml/azure_ad/<tenant_id>/acs`</span><span class="sxs-lookup"><span data-stu-id="a6fe5-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://www.123contactform.com/saml/azure_ad/<tenant_id>/acs`</span></span>

4. <span data-ttu-id="a6fe5-161">Ha tooconfigure hello alkalmazás **Szolgáltató kezdeményezett mód**, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="a6fe5-161">If you wish tooconfigure hello application in **SP initiated mode**, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-123contactform-tutorial/url2.png)

    <span data-ttu-id="a6fe5-163">a.</span><span class="sxs-lookup"><span data-stu-id="a6fe5-163">a.</span></span> <span data-ttu-id="a6fe5-164">Kattintson a hello **megjelenítése speciális URL-beállításainak** beállítás</span><span class="sxs-lookup"><span data-stu-id="a6fe5-164">Click hello **Show advanced URL settings** option</span></span>

    <span data-ttu-id="a6fe5-165">b.</span><span class="sxs-lookup"><span data-stu-id="a6fe5-165">b.</span></span> <span data-ttu-id="a6fe5-166">A hello **URL-cím bejelentkezési** szövegmező, adja meg az URL-címet:`https://www.123contactform.com/saml/azure_ad/<tenant_id>/sso`</span><span class="sxs-lookup"><span data-stu-id="a6fe5-166">In hello **Sign On URL** textbox, type a URL as: `https://www.123contactform.com/saml/azure_ad/<tenant_id>/sso`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="a6fe5-167">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="a6fe5-167">These values are not real.</span></span> <span data-ttu-id="a6fe5-168">A tényleges URL-címek és hello oktatóanyag későbbi részében ismertetett azonosító értéket kell tooupdate.</span><span class="sxs-lookup"><span data-stu-id="a6fe5-168">You'll need tooupdate these value from actual URLs and Identifier which is explained later in hello tutorial.</span></span>
    
5. <span data-ttu-id="a6fe5-169">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="a6fe5-169">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-123contactform-tutorial/tutorial_123contactform_certificate.png) 

6. <span data-ttu-id="a6fe5-171">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="a6fe5-171">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-123contactform-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="a6fe5-173">tooconfigure egyszeri bejelentkezést a **123ContactForm** oldalán, nyissa meg túl[https://www.123contactform.com/form-2709121/](https://www.123contactform.com/form-2709121/) , és végezze el az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="a6fe5-173">tooconfigure single sign-on on **123ContactForm** side, go too[https://www.123contactform.com/form-2709121/](https://www.123contactform.com/form-2709121/) and perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-123contactform-tutorial/submit.png) 

    <span data-ttu-id="a6fe5-175">a.</span><span class="sxs-lookup"><span data-stu-id="a6fe5-175">a.</span></span> <span data-ttu-id="a6fe5-176">A hello **E-mail** szövegmezőhöz típus hello e-mail címe hello felhasználói Egytényezős</span><span class="sxs-lookup"><span data-stu-id="a6fe5-176">In hello **Email** textbox, type hello email of hello user i.e</span></span> <span data-ttu-id="a6fe5-177">**BrittaSimon@Contoso.com**.</span><span class="sxs-lookup"><span data-stu-id="a6fe5-177">**BrittaSimon@Contoso.com**.</span></span>

    <span data-ttu-id="a6fe5-178">b.</span><span class="sxs-lookup"><span data-stu-id="a6fe5-178">b.</span></span> <span data-ttu-id="a6fe5-179">Kattintson a **feltöltése** , és keresse meg a metaadatok XML-fájl, amely az Azure-portálról letöltött hello.</span><span class="sxs-lookup"><span data-stu-id="a6fe5-179">Click **Upload** and browse hello Metadata XML file, which you have downloaded from Azure portal.</span></span>

    <span data-ttu-id="a6fe5-180">c.</span><span class="sxs-lookup"><span data-stu-id="a6fe5-180">c.</span></span> <span data-ttu-id="a6fe5-181">Kattintson a **SUBMIT űrlap**.</span><span class="sxs-lookup"><span data-stu-id="a6fe5-181">Click **SUBMIT FORM**.</span></span>

8. <span data-ttu-id="a6fe5-182">A hello **Microsoft Azure AD egyszeri bejelentkezés - alkalmazások beállításainak konfigurálása** hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="a6fe5-182">On hello **Microsoft Azure AD - Single sign-on - Configure App Settings** perform hello following steps:</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-123contactform-tutorial/url3.png)

    <span data-ttu-id="a6fe5-184">a.</span><span class="sxs-lookup"><span data-stu-id="a6fe5-184">a.</span></span> <span data-ttu-id="a6fe5-185">Ha tooconfigure hello alkalmazás **IDP kezdeményezett mód**, másolása hello **azonosító** érték, a példány, és illessze be **azonosító** textbox **123ContactForm tartomány és az URL-címek** Azure-portál szakaszban.</span><span class="sxs-lookup"><span data-stu-id="a6fe5-185">If you wish tooconfigure hello application in **IDP initiated mode**, copy hello **IDENTIFIER** value for your instance and paste it in **Identifier** textbox in **123ContactForm Domain and URLs** section on Azure portal.</span></span>
    
    <span data-ttu-id="a6fe5-186">b.</span><span class="sxs-lookup"><span data-stu-id="a6fe5-186">b.</span></span> <span data-ttu-id="a6fe5-187">Ha tooconfigure hello alkalmazás **IDP kezdeményezett mód**, másolása hello **válasz URL-CÍMEN** érték, a példány, és illessze be **válasz URL-CÍMEN** textbox **123ContactForm tartomány és az URL-címek** Azure-portál szakaszban.</span><span class="sxs-lookup"><span data-stu-id="a6fe5-187">If you wish tooconfigure hello application in **IDP initiated mode**, copy hello **REPLY URL** value for your instance and paste it in **Reply URL** textbox in **123ContactForm Domain and URLs** section on Azure portal.</span></span>

    <span data-ttu-id="a6fe5-188">c.</span><span class="sxs-lookup"><span data-stu-id="a6fe5-188">c.</span></span> <span data-ttu-id="a6fe5-189">Ha tooconfigure hello alkalmazás **Szolgáltató kezdeményezett mód**, másolása hello **SIGN-ON URL** érték, a példány, és illessze be **URL-cím bejelentkezési** textbox **123ContactForm tartomány és az URL-címek** Azure-portál szakaszban.</span><span class="sxs-lookup"><span data-stu-id="a6fe5-189">If you wish tooconfigure hello application in **SP initiated mode**, copy hello **SIGN ON URL** value for your instance and paste it in **Sign On URL** textbox in **123ContactForm Domain and URLs** section on Azure portal.</span></span>

> [!TIP]
> <span data-ttu-id="a6fe5-190">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="a6fe5-190">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="a6fe5-191">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="a6fe5-191">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="a6fe5-192">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="a6fe5-192">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="a6fe5-193">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="a6fe5-193">Creating an Azure AD test user</span></span>
<span data-ttu-id="a6fe5-194">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="a6fe5-194">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="a6fe5-196">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="a6fe5-196">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="a6fe5-197">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="a6fe5-197">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-123contactform-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="a6fe5-199">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="a6fe5-199">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-123contactform-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="a6fe5-201">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="a6fe5-201">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-123contactform-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="a6fe5-203">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="a6fe5-203">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-123contactform-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="a6fe5-205">a.</span><span class="sxs-lookup"><span data-stu-id="a6fe5-205">a.</span></span> <span data-ttu-id="a6fe5-206">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="a6fe5-206">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="a6fe5-207">b.</span><span class="sxs-lookup"><span data-stu-id="a6fe5-207">b.</span></span> <span data-ttu-id="a6fe5-208">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="a6fe5-208">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="a6fe5-209">c.</span><span class="sxs-lookup"><span data-stu-id="a6fe5-209">c.</span></span> <span data-ttu-id="a6fe5-210">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="a6fe5-210">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="a6fe5-211">d.</span><span class="sxs-lookup"><span data-stu-id="a6fe5-211">d.</span></span> <span data-ttu-id="a6fe5-212">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="a6fe5-212">Click **Create**.</span></span>
 
### <a name="creating-a-123contactform-test-user"></a><span data-ttu-id="a6fe5-213">123ContactForm tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="a6fe5-213">Creating a 123ContactForm test user</span></span>

<span data-ttu-id="a6fe5-214">Alkalmazás támogatja a csak az idő a felhasználók átadása, miután a felhasználók hitelesítésére hello alkalmazás automatikusan létrejön.</span><span class="sxs-lookup"><span data-stu-id="a6fe5-214">Application supports Just in time user provisioning and after authentication users will be created in hello application automatically.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="a6fe5-215">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="a6fe5-215">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="a6fe5-216">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés too123ContactForm megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="a6fe5-216">In this section, you enable Britta Simon toouse Azure single sign-on by granting access too123ContactForm.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="a6fe5-218">**tooassign Britta Simon too123ContactForm, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="a6fe5-218">**tooassign Britta Simon too123ContactForm, perform hello following steps:**</span></span>

1. <span data-ttu-id="a6fe5-219">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="a6fe5-219">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="a6fe5-221">Hello alkalmazások listában válassza ki a **123ContactForm**.</span><span class="sxs-lookup"><span data-stu-id="a6fe5-221">In hello applications list, select **123ContactForm**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-123contactform-tutorial/tutorial_123contactform_app.png) 

3. <span data-ttu-id="a6fe5-223">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="a6fe5-223">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="a6fe5-225">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="a6fe5-225">Click **Add** button.</span></span> <span data-ttu-id="a6fe5-226">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="a6fe5-226">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="a6fe5-228">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="a6fe5-228">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="a6fe5-229">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="a6fe5-229">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="a6fe5-230">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="a6fe5-230">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="a6fe5-231">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="a6fe5-231">Testing single sign-on</span></span>

<span data-ttu-id="a6fe5-232">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="a6fe5-232">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="a6fe5-233">Ha a hozzáférési Panel hello hello 123ContactForm csempe gombra kattint, automatikusan bejelentkezett tooyour 123ContactForm alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="a6fe5-233">When you click hello 123ContactForm tile in hello Access Panel, you should get automatically signed-on tooyour 123ContactForm application.</span></span>
<span data-ttu-id="a6fe5-234">A hozzáférési Panel kapcsolatos további információkért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="a6fe5-234">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a6fe5-235">További források</span><span class="sxs-lookup"><span data-stu-id="a6fe5-235">Additional resources</span></span>

* [<span data-ttu-id="a6fe5-236">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="a6fe5-236">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="a6fe5-237">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="a6fe5-237">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-123contactform-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-123contactform-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-123contactform-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-123contactform-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-123contactform-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-123contactform-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-123contactform-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-123contactform-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-123contactform-tutorial/tutorial_general_203.png

