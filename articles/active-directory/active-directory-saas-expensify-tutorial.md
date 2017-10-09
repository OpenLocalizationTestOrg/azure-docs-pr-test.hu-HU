---
title: "Oktatóanyag: Azure Active Directoryval integrált Expensify |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Expensify között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 1e761484-7a2f-4321-91f4-6d5d0b69344e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/12/2017
ms.author: jeedes
ms.openlocfilehash: 141513ef27c90dae2d77a52ecab2f89c4e5a55ee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-expensify"></a><span data-ttu-id="836b6-103">Oktatóanyag: Azure Active Directoryval integrált Expensify</span><span class="sxs-lookup"><span data-stu-id="836b6-103">Tutorial: Azure Active Directory integration with Expensify</span></span>

<span data-ttu-id="836b6-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Expensify az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="836b6-104">In this tutorial, you learn how toointegrate Expensify with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="836b6-105">Expensify integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="836b6-105">Integrating Expensify with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="836b6-106">Megadhatja a hozzáférés tooExpensify rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="836b6-106">You can control in Azure AD who has access tooExpensify</span></span>
- <span data-ttu-id="836b6-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooExpensify (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="836b6-107">You can enable your users tooautomatically get signed-on tooExpensify (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="836b6-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="836b6-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="836b6-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="836b6-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="836b6-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="836b6-110">Prerequisites</span></span>

<span data-ttu-id="836b6-111">az Azure AD integrálása Expensify tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="836b6-111">tooconfigure Azure AD integration with Expensify, you need hello following items:</span></span>

- <span data-ttu-id="836b6-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="836b6-112">An Azure AD subscription</span></span>
- <span data-ttu-id="836b6-113">Egy Expensify egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="836b6-113">An Expensify single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="836b6-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="836b6-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="836b6-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="836b6-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="836b6-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="836b6-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="836b6-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="836b6-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="836b6-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="836b6-118">Scenario description</span></span>
<span data-ttu-id="836b6-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="836b6-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="836b6-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="836b6-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="836b6-121">Hello gyűjteményből Expensify hozzáadása</span><span class="sxs-lookup"><span data-stu-id="836b6-121">Adding Expensify from hello gallery</span></span>
2. <span data-ttu-id="836b6-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="836b6-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-expensify-from-hello-gallery"></a><span data-ttu-id="836b6-123">Hello gyűjteményből Expensify hozzáadása</span><span class="sxs-lookup"><span data-stu-id="836b6-123">Adding Expensify from hello gallery</span></span>
<span data-ttu-id="836b6-124">tooconfigure hello integrációja Expensify az Azure AD-be, meg kell tooadd Expensify hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="836b6-124">tooconfigure hello integration of Expensify into Azure AD, you need tooadd Expensify from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="836b6-125">**tooadd Expensify hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="836b6-125">**tooadd Expensify from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="836b6-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="836b6-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="836b6-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="836b6-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="836b6-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="836b6-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="836b6-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="836b6-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="836b6-133">Hello keresési mezőbe, írja be a **Expensify**.</span><span class="sxs-lookup"><span data-stu-id="836b6-133">In hello search box, type **Expensify**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-expensify-tutorial/tutorial_expensify_search.png)

5. <span data-ttu-id="836b6-135">A hello eredmények panelen válassza ki a **Expensify**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="836b6-135">In hello results panel, select **Expensify**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-expensify-tutorial/tutorial_expensify_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="836b6-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="836b6-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="836b6-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Expensify.</span><span class="sxs-lookup"><span data-stu-id="836b6-138">In this section, you configure and test Azure AD single sign-on with Expensify based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="836b6-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Expensify tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="836b6-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Expensify is tooa user in Azure AD.</span></span> <span data-ttu-id="836b6-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Expensify közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="836b6-140">In other words, a link relationship between an Azure AD user and hello related user in Expensify needs toobe established.</span></span>

<span data-ttu-id="836b6-141">Expensify, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="836b6-141">In Expensify, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="836b6-142">tooconfigure és az Azure AD az egyszeri bejelentkezés Expensify-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="836b6-142">tooconfigure and test Azure AD single sign-on with Expensify, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="836b6-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="836b6-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="836b6-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="836b6-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="836b6-145">**[Egy Expensify tesztfelhasználó létrehozása](#creating-an-expensify-test-user)**  -toohave egy megfelelője a Britta Simon a Expensify, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="836b6-145">**[Creating an Expensify test user](#creating-an-expensify-test-user)** - toohave a counterpart of Britta Simon in Expensify that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="836b6-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="836b6-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="836b6-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="836b6-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="836b6-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="836b6-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="836b6-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Expensify alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="836b6-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Expensify application.</span></span>

<span data-ttu-id="836b6-150">**az Azure AD tooconfigure egyszeri bejelentkezést a Expensify, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="836b6-150">**tooconfigure Azure AD single sign-on with Expensify, perform hello following steps:**</span></span>

1. <span data-ttu-id="836b6-151">Az Azure portál, a hello hello **Expensify** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="836b6-151">In hello Azure portal, on hello **Expensify** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="836b6-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="836b6-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-expensify-tutorial/tutorial_expensify_samlbase.png)

3. <span data-ttu-id="836b6-155">A hello **Expensify tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="836b6-155">On hello **Expensify Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-expensify-tutorial/tutorial_expensify_url.png)

    <span data-ttu-id="836b6-157">a.</span><span class="sxs-lookup"><span data-stu-id="836b6-157">a.</span></span> <span data-ttu-id="836b6-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://www.expensify.com/authentication/saml/login`</span><span class="sxs-lookup"><span data-stu-id="836b6-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://www.expensify.com/authentication/saml/login`</span></span>

    <span data-ttu-id="836b6-159">b.</span><span class="sxs-lookup"><span data-stu-id="836b6-159">b.</span></span> <span data-ttu-id="836b6-160">A hello **azonosító URL-t** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://www.<companyname>.expensify.com/`</span><span class="sxs-lookup"><span data-stu-id="836b6-160">In hello **Identifier URL** textbox, type a URL using hello following pattern: `https://www.<companyname>.expensify.com/`</span></span> 

    > [!NOTE] 
    > <span data-ttu-id="836b6-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="836b6-161">These values are not real.</span></span> <span data-ttu-id="836b6-162">Frissítheti ezeket az értékeket hello tényleges bejelentkezési URL-cím és azonosító URL-címmel.</span><span class="sxs-lookup"><span data-stu-id="836b6-162">Update these values with hello actual Sign-On URL and Identifier URL.</span></span> <span data-ttu-id="836b6-163">Ügyfél [Expensify ügyfél-támogatási csoport](mailto:help@expensify.com) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="836b6-163">Contact [Expensify Client support team](mailto:help@expensify.com) tooget these values.</span></span> 
 
4. <span data-ttu-id="836b6-164">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="836b6-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-expensify-tutorial/tutorial_expensify_certificate.png) 

5. <span data-ttu-id="836b6-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="836b6-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-expensify-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="836b6-168">tooenable SSO Expensify, először tooenable **tartomány vezérlő** hello alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="836b6-168">tooenable SSO in Expensify, you first need tooenable **Domain Control** in hello application.</span></span> <span data-ttu-id="836b6-169">Tartomány vezérlő engedélyezheti a hello alkalmazás hello lépésben felsorolt [Itt](http://help.expensify.com/domain-control).</span><span class="sxs-lookup"><span data-stu-id="836b6-169">You can enable Domain Control in hello application through hello steps listed [here](http://help.expensify.com/domain-control).</span></span> <span data-ttu-id="836b6-170">További együttműködve [Expensify ügyfél-támogatási csoport](mailto:help@expensify.com).</span><span class="sxs-lookup"><span data-stu-id="836b6-170">For additional support, work with [Expensify Client support team](mailto:help@expensify.com).</span></span> <span data-ttu-id="836b6-171">Miután tartományi vezérlő engedélyezve van, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="836b6-171">Once you have Domain Control enabled, follow these steps:</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-expensify-tutorial/tutorial_expensify_51.png)
    
    <span data-ttu-id="836b6-173">a.</span><span class="sxs-lookup"><span data-stu-id="836b6-173">a.</span></span> <span data-ttu-id="836b6-174">Bejelentkezés tooyour Expensify alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="836b6-174">Sign on tooyour Expensify application.</span></span>
    
    <span data-ttu-id="836b6-175">b.</span><span class="sxs-lookup"><span data-stu-id="836b6-175">b.</span></span> <span data-ttu-id="836b6-176">Hello hello felső eszköztárán kattintson **Admin**.</span><span class="sxs-lookup"><span data-stu-id="836b6-176">In hello toolbar on hello top, click **Admin**.</span></span>
    
    <span data-ttu-id="836b6-177">c.</span><span class="sxs-lookup"><span data-stu-id="836b6-177">c.</span></span> <span data-ttu-id="836b6-178">A hello bal oldali panelen kattintson a **tartomány**.</span><span class="sxs-lookup"><span data-stu-id="836b6-178">In hello left panel, click **Domain**.</span></span>
    
    <span data-ttu-id="836b6-179">d.</span><span class="sxs-lookup"><span data-stu-id="836b6-179">d.</span></span> <span data-ttu-id="836b6-180">Kattintson a ellenőrzött tartomány nevét.</span><span class="sxs-lookup"><span data-stu-id="836b6-180">Click your verified domain name.</span></span>
    
    <span data-ttu-id="836b6-181">e.</span><span class="sxs-lookup"><span data-stu-id="836b6-181">e.</span></span> <span data-ttu-id="836b6-182">A hello bal oldali panelen kattintson a **SAML**, majd válassza ki **engedélyezve**.</span><span class="sxs-lookup"><span data-stu-id="836b6-182">In hello left panel, click **SAML**, and then select **Enabled**.</span></span>
    
    <span data-ttu-id="836b6-183">f.</span><span class="sxs-lookup"><span data-stu-id="836b6-183">f.</span></span> <span data-ttu-id="836b6-184">Nyissa meg hello összevonási metaadatok letöltötte az Azure AD a Jegyzettömbben másolási hello tartalom, és illessze be hello **Identity Provider metaadatok** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="836b6-184">Open hello downloaded Federation Metadata from Azure AD in notepad, copy hello content, and then paste it into hello **Identity Provider Metadata** textbox.</span></span>

> [!TIP]
> <span data-ttu-id="836b6-185">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="836b6-185">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="836b6-186">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="836b6-186">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="836b6-187">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="836b6-187">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="836b6-188">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="836b6-188">Creating an Azure AD test user</span></span>
<span data-ttu-id="836b6-189">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="836b6-189">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="836b6-191">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="836b6-191">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="836b6-192">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="836b6-192">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-expensify-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="836b6-194">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="836b6-194">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-expensify-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="836b6-196">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="836b6-196">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-expensify-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="836b6-198">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="836b6-198">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-expensify-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="836b6-200">a.</span><span class="sxs-lookup"><span data-stu-id="836b6-200">a.</span></span> <span data-ttu-id="836b6-201">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="836b6-201">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="836b6-202">b.</span><span class="sxs-lookup"><span data-stu-id="836b6-202">b.</span></span> <span data-ttu-id="836b6-203">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="836b6-203">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="836b6-204">c.</span><span class="sxs-lookup"><span data-stu-id="836b6-204">c.</span></span> <span data-ttu-id="836b6-205">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="836b6-205">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="836b6-206">d.</span><span class="sxs-lookup"><span data-stu-id="836b6-206">d.</span></span> <span data-ttu-id="836b6-207">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="836b6-207">Click **Create**.</span></span>
 
### <a name="creating-an-expensify-test-user"></a><span data-ttu-id="836b6-208">Egy Expensify tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="836b6-208">Creating an Expensify test user</span></span>

<span data-ttu-id="836b6-209">Ebben a szakaszban egy Expensify Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="836b6-209">In this section, you create a user called Britta Simon in Expensify.</span></span> <span data-ttu-id="836b6-210">Együttműködve [Expensify ügyfél-támogatási csoport](mailto:help@expensify.com) tooadd hello felhasználók hello Expensify platform.</span><span class="sxs-lookup"><span data-stu-id="836b6-210">Work with [Expensify Client support team](mailto:help@expensify.com) tooadd hello users in hello Expensify platform.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="836b6-211">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="836b6-211">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="836b6-212">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooExpensify megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="836b6-212">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooExpensify.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="836b6-214">**tooassign Britta Simon tooExpensify, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="836b6-214">**tooassign Britta Simon tooExpensify, perform hello following steps:**</span></span>

1. <span data-ttu-id="836b6-215">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="836b6-215">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="836b6-217">Hello alkalmazások listában válassza ki a **Expensify**.</span><span class="sxs-lookup"><span data-stu-id="836b6-217">In hello applications list, select **Expensify**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-expensify-tutorial/tutorial_expensify_app.png) 

3. <span data-ttu-id="836b6-219">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="836b6-219">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="836b6-221">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="836b6-221">Click **Add** button.</span></span> <span data-ttu-id="836b6-222">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="836b6-222">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="836b6-224">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="836b6-224">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="836b6-225">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="836b6-225">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="836b6-226">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="836b6-226">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="836b6-227">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="836b6-227">Testing single sign-on</span></span>

<span data-ttu-id="836b6-228">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="836b6-228">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>  

<span data-ttu-id="836b6-229">Ha a hozzáférési Panel hello hello Expensify csempe gombra kattint, automatikusan bejelentkezett tooyour Expensify alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="836b6-229">When you click hello Expensify tile in hello Access Panel, you should get automatically signed-on tooyour Expensify application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="836b6-230">További források</span><span class="sxs-lookup"><span data-stu-id="836b6-230">Additional resources</span></span>

* [<span data-ttu-id="836b6-231">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="836b6-231">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="836b6-232">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="836b6-232">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-expensify-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-expensify-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-expensify-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-expensify-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-expensify-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-expensify-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-expensify-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-expensify-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-expensify-tutorial/tutorial_general_203.png

