---
title: "Oktatóanyag: Azure Active Directoryval integrált hello kiválasztását portál |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory között, és támogatás Portal hello."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 4663cc8a-976a-4c6c-b3b4-1e5df9b66744
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: jeedes
ms.openlocfilehash: 9f4329e02f91eb6d8034f17646ac7d15afe503e8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-hello-funding-portal"></a><span data-ttu-id="7da3b-103">Oktatóanyag: Azure Active Directoryval integrált hello Portal támogatás</span><span class="sxs-lookup"><span data-stu-id="7da3b-103">Tutorial: Azure Active Directory integration with hello Funding Portal</span></span>

<span data-ttu-id="7da3b-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate hello Portal támogatás az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="7da3b-104">In this tutorial, you learn how toointegrate hello Funding Portal with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="7da3b-105">Hello integrálása Portal támogatás az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="7da3b-105">Integrating hello Funding Portal with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="7da3b-106">Megadhatja a hozzáférés toohello finanszírozás Portal rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="7da3b-106">You can control in Azure AD who has access toohello Funding Portal</span></span>
- <span data-ttu-id="7da3b-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett toohello finanszírozás portál (egyszeri bejelentkezés) az Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="7da3b-107">You can enable your users tooautomatically get signed-on toohello Funding Portal (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="7da3b-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="7da3b-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="7da3b-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="7da3b-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7da3b-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="7da3b-110">Prerequisites</span></span>

<span data-ttu-id="7da3b-111">hello finanszírozás Portal tooconfigure az Azure AD integrálása, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="7da3b-111">tooconfigure Azure AD integration with hello Funding Portal, you need hello following items:</span></span>

- <span data-ttu-id="7da3b-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="7da3b-112">An Azure AD subscription</span></span>
- <span data-ttu-id="7da3b-113">Támogatás Portal egyszeri bejelentkezéshez egy hello előfizetés engedélyezve</span><span class="sxs-lookup"><span data-stu-id="7da3b-113">A hello Funding Portal single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="7da3b-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="7da3b-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="7da3b-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="7da3b-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="7da3b-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="7da3b-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="7da3b-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="7da3b-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="7da3b-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="7da3b-118">Scenario description</span></span>
<span data-ttu-id="7da3b-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="7da3b-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="7da3b-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="7da3b-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="7da3b-121">Hozzáadását hello finanszírozás Portal hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="7da3b-121">Adding hello Funding Portal from hello gallery</span></span>
2. <span data-ttu-id="7da3b-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="7da3b-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-hello-funding-portal-from-hello-gallery"></a><span data-ttu-id="7da3b-123">Hozzáadását hello finanszírozás Portal hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="7da3b-123">Adding hello Funding Portal from hello gallery</span></span>
<span data-ttu-id="7da3b-124">tooconfigure hello integrációja hello finanszírozás portálon az Azure AD-be, meg kell tooadd hello finanszírozás Portal hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="7da3b-124">tooconfigure hello integration of hello Funding Portal into Azure AD, you need tooadd hello Funding Portal from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="7da3b-125">**tooadd hello kiválasztását Portal hello gyűjteményből, hajtsa végre az alábbi lépésekkel hello:**</span><span class="sxs-lookup"><span data-stu-id="7da3b-125">**tooadd hello Funding Portal from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="7da3b-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="7da3b-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="7da3b-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="7da3b-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="7da3b-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="7da3b-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="7da3b-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="7da3b-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="7da3b-133">Hello keresési mezőbe, írja be a **kiválasztását Portal hello**.</span><span class="sxs-lookup"><span data-stu-id="7da3b-133">In hello search box, type **hello Funding Portal**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_search.png)

5. <span data-ttu-id="7da3b-135">Hello eredmények panelen, jelölje ki a **kiválasztását Portal hello**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="7da3b-135">In hello results panel, select **hello Funding Portal**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="7da3b-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="7da3b-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="7da3b-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést a hello kiválasztását Portal "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="7da3b-138">In this section, you configure and test Azure AD single sign-on with hello Funding Portal based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="7da3b-139">Az egyszeri bejelentkezés toowork, az Azure AD kell tooknow milyen hello tartozó felhasználói hello a portál kiválasztását tooa felhasználói az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="7da3b-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in hello Funding Portal is tooa user in Azure AD.</span></span> <span data-ttu-id="7da3b-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználói hello hello a hivatkozás kapcsolatának kiválasztását Portal kell létrehozott toobe.</span><span class="sxs-lookup"><span data-stu-id="7da3b-140">In other words, a link relationship between an Azure AD user and hello related user in hello Funding Portal needs toobe established.</span></span>

<span data-ttu-id="7da3b-141">A portál kiválasztását hello, hello hello értéket **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="7da3b-141">In hello Funding Portal, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="7da3b-142">tooconfigure és az Azure AD az egyszeri bejelentkezés hello finanszírozás Portal-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="7da3b-142">tooconfigure and test Azure AD single sign-on with hello Funding Portal, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="7da3b-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="7da3b-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="7da3b-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="7da3b-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="7da3b-145">**[Teszt felhasználó létrehozásakor hello kiválasztását Portal](#creating-the-funding-portal-test-user)**  -toohave Britta Simon egy partner, a hello finanszírozás portál, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="7da3b-145">**[Creating hello Funding Portal test user](#creating-the-funding-portal-test-user)** - toohave a counterpart of Britta Simon in hello Funding Portal that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="7da3b-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="7da3b-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="7da3b-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="7da3b-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="7da3b-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="7da3b-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="7da3b-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és a hello kiválasztását Portal-alkalmazás az egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="7da3b-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your hello Funding Portal application.</span></span>

<span data-ttu-id="7da3b-150">**tooconfigure az Azure AD egyszeri bejelentkezést a hello Portal kiválasztását, hajtsa végre az alábbi lépésekkel hello:**</span><span class="sxs-lookup"><span data-stu-id="7da3b-150">**tooconfigure Azure AD single sign-on with hello Funding Portal, perform hello following steps:**</span></span>

1. <span data-ttu-id="7da3b-151">Az Azure portál, a hello hello **kiválasztását Portal hello** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="7da3b-151">In hello Azure portal, on hello **hello Funding Portal** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="7da3b-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="7da3b-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_samlbase.png)

3. <span data-ttu-id="7da3b-155">A hello **Portal tartomány kiválasztását és URL-címek hello** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="7da3b-155">On hello **hello Funding Portal Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_url.png)

    <span data-ttu-id="7da3b-157">a.</span><span class="sxs-lookup"><span data-stu-id="7da3b-157">a.</span></span> <span data-ttu-id="7da3b-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<subdomain>.regenteducation.net/`</span><span class="sxs-lookup"><span data-stu-id="7da3b-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<subdomain>.regenteducation.net/`</span></span>

    <span data-ttu-id="7da3b-159">b.</span><span class="sxs-lookup"><span data-stu-id="7da3b-159">b.</span></span> <span data-ttu-id="7da3b-160">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<subdomain>.regenteducation.net`</span><span class="sxs-lookup"><span data-stu-id="7da3b-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<subdomain>.regenteducation.net`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="7da3b-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="7da3b-161">These values are not real.</span></span> <span data-ttu-id="7da3b-162">Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="7da3b-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="7da3b-163">Ügyfél [kiválasztását portál ügyfél-támogatási csoport hello](mailto:info@regenteducation.com) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="7da3b-163">Contact [hello Funding Portal Client support team](mailto:info@regenteducation.com) tooget these values.</span></span> 

4. <span data-ttu-id="7da3b-164">hello kiválasztását Portal alkalmazás hello SAML helyességi feltételek toocontain "externalId1" nevű attribútumot vár.</span><span class="sxs-lookup"><span data-stu-id="7da3b-164">hello Funding Portal application expects hello SAML assertions toocontain an attribute named "externalId1".</span></span> <span data-ttu-id="7da3b-165">"externalId1" Hello értékének felismert studentID kell lennie.</span><span class="sxs-lookup"><span data-stu-id="7da3b-165">hello value of "externalId1" should be a recognized studentID.</span></span> <span data-ttu-id="7da3b-166">Hello "externalId1" jogcímek alkalmazás konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="7da3b-166">Configure hello "externalId1" claim for this application.</span></span> <span data-ttu-id="7da3b-167">Ezek az attribútumok értékének hello kezelheti hello **felhasználói attribútumok** hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="7da3b-167">You can manage hello values of these attributes from hello **User Attributes** of hello application.</span></span> <span data-ttu-id="7da3b-168">a következő képernyőkép hello ezen mutat egy példát.</span><span class="sxs-lookup"><span data-stu-id="7da3b-168">hello following screenshot shows an example for this.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_attribute.png)

5. <span data-ttu-id="7da3b-170">A hello **felhasználói attribútumok** szakaszt, hello **egyszeri bejelentkezés** párbeszédpanelen konfigurálja a SAML-jogkivonat attribútum hello ábrának megfelelően, és hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="7da3b-170">In hello **User Attributes** section on hello **Single sign-on** dialog, configure SAML token attribute as shown in hello image and perform hello following steps:</span></span>

    | <span data-ttu-id="7da3b-171">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="7da3b-171">Attribute Name</span></span> | <span data-ttu-id="7da3b-172">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="7da3b-172">Attribute Value</span></span> |
    | ------------------- | ---------------- |
    | <span data-ttu-id="7da3b-173">externalId1</span><span class="sxs-lookup"><span data-stu-id="7da3b-173">externalId1</span></span> | <span data-ttu-id="7da3b-174">User.extensionAttribute1</span><span class="sxs-lookup"><span data-stu-id="7da3b-174">user.extensionattribute1</span></span> |

    <span data-ttu-id="7da3b-175">a.</span><span class="sxs-lookup"><span data-stu-id="7da3b-175">a.</span></span> <span data-ttu-id="7da3b-176">Kattintson a **Hozzáadás attribútum** tooopen hello **attribútum hozzáadása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="7da3b-176">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-thefundingportal-tutorial/tutorial_attribute_04.png)

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-thefundingportal-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="7da3b-179">b.</span><span class="sxs-lookup"><span data-stu-id="7da3b-179">b.</span></span> <span data-ttu-id="7da3b-180">A hello **neve** szövegmezőben, az adott sorhoz feltüntetett hello attribútum neve.</span><span class="sxs-lookup"><span data-stu-id="7da3b-180">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>

    <span data-ttu-id="7da3b-181">c.</span><span class="sxs-lookup"><span data-stu-id="7da3b-181">c.</span></span> <span data-ttu-id="7da3b-182">A hello **attribútumérték** listán, válassza hello attribútum a megvalósítás toouse használni szeretne.</span><span class="sxs-lookup"><span data-stu-id="7da3b-182">From hello **Attribute Value** list, select hello attribute you want toouse for your implementation.</span></span> <span data-ttu-id="7da3b-183">Például ha hello ExtensionAttribute1 tárolt hello StudentID értéket, majd válassza ki user.extensionattribute1.</span><span class="sxs-lookup"><span data-stu-id="7da3b-183">For example, if you have stored hello StudentID value in hello ExtensionAttribute1, then select user.extensionattribute1.</span></span>
    
    <span data-ttu-id="7da3b-184">d.</span><span class="sxs-lookup"><span data-stu-id="7da3b-184">d.</span></span> <span data-ttu-id="7da3b-185">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="7da3b-185">Click **Ok**.</span></span>
 
6. <span data-ttu-id="7da3b-186">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="7da3b-186">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_certificate.png) 

7. <span data-ttu-id="7da3b-188">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="7da3b-188">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="7da3b-190">tooconfigure egyszeri bejelentkezést a **kiválasztását Portal hello** oldalon kell letöltött toosend hello **metaadatainak XML-kódja** túl[kiválasztását Portal támogatási csoport hello](mailto:info@regenteducation.com).</span><span class="sxs-lookup"><span data-stu-id="7da3b-190">tooconfigure single sign-on on **hello Funding Portal** side, you need toosend hello downloaded **Metadata XML** too[hello Funding Portal support team](mailto:info@regenteducation.com).</span></span> <span data-ttu-id="7da3b-191">Maguk állítják be ezt a beállítást toohave hello SAML SSO kapcsolat mindkét oldalán megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="7da3b-191">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="7da3b-192">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="7da3b-192">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="7da3b-193">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="7da3b-193">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="7da3b-194">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="7da3b-194">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="7da3b-195">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="7da3b-195">Creating an Azure AD test user</span></span>
<span data-ttu-id="7da3b-196">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="7da3b-196">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="7da3b-198">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="7da3b-198">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="7da3b-199">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="7da3b-199">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-thefundingportal-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="7da3b-201">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="7da3b-201">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-thefundingportal-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="7da3b-203">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="7da3b-203">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-thefundingportal-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="7da3b-205">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="7da3b-205">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-thefundingportal-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="7da3b-207">a.</span><span class="sxs-lookup"><span data-stu-id="7da3b-207">a.</span></span> <span data-ttu-id="7da3b-208">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="7da3b-208">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="7da3b-209">b.</span><span class="sxs-lookup"><span data-stu-id="7da3b-209">b.</span></span> <span data-ttu-id="7da3b-210">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="7da3b-210">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="7da3b-211">c.</span><span class="sxs-lookup"><span data-stu-id="7da3b-211">c.</span></span> <span data-ttu-id="7da3b-212">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="7da3b-212">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="7da3b-213">d.</span><span class="sxs-lookup"><span data-stu-id="7da3b-213">d.</span></span> <span data-ttu-id="7da3b-214">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="7da3b-214">Click **Create**.</span></span>
 
### <a name="creating-hello-funding-portal-test-user"></a><span data-ttu-id="7da3b-215">Hello kiválasztását Portal tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="7da3b-215">Creating hello Funding Portal test user</span></span>

<span data-ttu-id="7da3b-216">Ebben a szakaszban egy Britta Simon hello finanszírozás portál nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="7da3b-216">In this section, you create a user called Britta Simon in hello Funding Portal.</span></span> <span data-ttu-id="7da3b-217">Együttműködve [kiválasztását Portal támogatási csoport hello](mailto:info@regenteducation.com) tooadd hello tesztfelhasználó, és engedélyezze az egyszeri Bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="7da3b-217">Work with [hello Funding Portal support team](mailto:info@regenteducation.com) tooadd hello test user and enable SSO.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="7da3b-218">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="7da3b-218">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="7da3b-219">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés toohello finanszírozás Portal megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="7da3b-219">In this section, you enable Britta Simon toouse Azure single sign-on by granting access toohello Funding Portal.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="7da3b-221">**tooassign Britta Simon toohello finanszírozás Portal, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="7da3b-221">**tooassign Britta Simon toohello Funding Portal, perform hello following steps:**</span></span>

1. <span data-ttu-id="7da3b-222">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="7da3b-222">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="7da3b-224">Hello alkalmazások listában válassza ki a **kiválasztását Portal hello**.</span><span class="sxs-lookup"><span data-stu-id="7da3b-224">In hello applications list, select **hello Funding Portal**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_app.png) 

3. <span data-ttu-id="7da3b-226">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="7da3b-226">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="7da3b-228">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="7da3b-228">Click **Add** button.</span></span> <span data-ttu-id="7da3b-229">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="7da3b-229">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="7da3b-231">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="7da3b-231">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="7da3b-232">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="7da3b-232">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="7da3b-233">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="7da3b-233">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="7da3b-234">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="7da3b-234">Testing single sign-on</span></span>

<span data-ttu-id="7da3b-235">hello ebben a szakaszban célja tootest az egyszeri bejelentkezés konfigurációs használatával hello a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="7da3b-235">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="7da3b-236">Hello hello kiválasztását Portal csempe a hozzáférési Panel hello kattintáskor automatikusan bejelentkezett tooyour hello kiválasztását Portal alkalmazás kapja meg.</span><span class="sxs-lookup"><span data-stu-id="7da3b-236">When you click hello hello Funding Portal tile in hello Access Panel, you should get automatically signed-on tooyour hello Funding Portal application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7da3b-237">További források</span><span class="sxs-lookup"><span data-stu-id="7da3b-237">Additional resources</span></span>

* [<span data-ttu-id="7da3b-238">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="7da3b-238">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="7da3b-239">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="7da3b-239">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_203.png

