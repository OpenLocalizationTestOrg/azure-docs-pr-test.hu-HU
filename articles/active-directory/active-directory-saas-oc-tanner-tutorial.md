---
title: "Oktatóanyag: Azure Active Directoryval integrált O.C. Péter - AppreciateHub |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és O.C. között Péter - AppreciateHub."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: dee8fbca-0b60-4a21-8917-1fb6919de5a0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: jeedes
ms.openlocfilehash: 45052cf56e35746d7df5910162e40e3bbcad1aca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-oc-tanner---appreciatehub"></a><span data-ttu-id="43f35-105">Oktatóanyag: Azure Active Directoryval integrált O.C.</span><span class="sxs-lookup"><span data-stu-id="43f35-105">Tutorial: Azure Active Directory integration with O.C.</span></span> <span data-ttu-id="43f35-106">Péter - AppreciateHub</span><span class="sxs-lookup"><span data-stu-id="43f35-106">Tanner - AppreciateHub</span></span>

<span data-ttu-id="43f35-107">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate O.C.</span><span class="sxs-lookup"><span data-stu-id="43f35-107">In this tutorial, you learn how toointegrate O.C.</span></span> <span data-ttu-id="43f35-108">Péter - AppreciateHub az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="43f35-108">Tanner - AppreciateHub with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="43f35-109">O.C. integrálása</span><span class="sxs-lookup"><span data-stu-id="43f35-109">Integrating O.C.</span></span> <span data-ttu-id="43f35-110">Péter - AppreciateHub az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="43f35-110">Tanner - AppreciateHub with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="43f35-111">Az Azure AD hozzáférési tooO.C rendelkező szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="43f35-111">You can control in Azure AD who has access tooO.C.</span></span> <span data-ttu-id="43f35-112">Péter - AppreciateHub</span><span class="sxs-lookup"><span data-stu-id="43f35-112">Tanner - AppreciateHub</span></span>
- <span data-ttu-id="43f35-113">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooO.C.</span><span class="sxs-lookup"><span data-stu-id="43f35-113">You can enable your users tooautomatically get signed-on tooO.C.</span></span> <span data-ttu-id="43f35-114">Péter - AppreciateHub (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="43f35-114">Tanner - AppreciateHub (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="43f35-115">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="43f35-115">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="43f35-116">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="43f35-116">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="43f35-117">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="43f35-117">Prerequisites</span></span>

<span data-ttu-id="43f35-118">az Azure AD integrálása O.C. tooconfigure</span><span class="sxs-lookup"><span data-stu-id="43f35-118">tooconfigure Azure AD integration with O.C.</span></span> <span data-ttu-id="43f35-119">Péter - AppreciateHub, a következő elemek hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="43f35-119">Tanner - AppreciateHub, you need hello following items:</span></span>

- <span data-ttu-id="43f35-120">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="43f35-120">An Azure AD subscription</span></span>
- <span data-ttu-id="43f35-121">EGY O.C.</span><span class="sxs-lookup"><span data-stu-id="43f35-121">A O.C.</span></span> <span data-ttu-id="43f35-122">Péter - AppreciateHub egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="43f35-122">Tanner - AppreciateHub single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="43f35-123">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="43f35-123">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="43f35-124">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="43f35-124">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="43f35-125">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="43f35-125">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="43f35-126">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="43f35-126">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="43f35-127">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="43f35-127">Scenario description</span></span>
<span data-ttu-id="43f35-128">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="43f35-128">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="43f35-129">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="43f35-129">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="43f35-130">O.C. hozzáadása</span><span class="sxs-lookup"><span data-stu-id="43f35-130">Adding O.C.</span></span> <span data-ttu-id="43f35-131">Péter - AppreciateHub hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="43f35-131">Tanner - AppreciateHub from hello gallery</span></span>
2. <span data-ttu-id="43f35-132">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="43f35-132">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-oc-tanner---appreciatehub-from-hello-gallery"></a><span data-ttu-id="43f35-133">O.C. hozzáadása</span><span class="sxs-lookup"><span data-stu-id="43f35-133">Adding O.C.</span></span> <span data-ttu-id="43f35-134">Péter - AppreciateHub hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="43f35-134">Tanner - AppreciateHub from hello gallery</span></span>
<span data-ttu-id="43f35-135">O.C. tooconfigure hello integrációja</span><span class="sxs-lookup"><span data-stu-id="43f35-135">tooconfigure hello integration of O.C.</span></span> <span data-ttu-id="43f35-136">Péter - AppreciateHub az Azure AD-be tooadd O.C. van szüksége</span><span class="sxs-lookup"><span data-stu-id="43f35-136">Tanner - AppreciateHub into Azure AD, you need tooadd O.C.</span></span> <span data-ttu-id="43f35-137">Péter - AppreciateHub hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="43f35-137">Tanner - AppreciateHub from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="43f35-138">**tooadd O.C. Péter - AppreciateHub hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="43f35-138">**tooadd O.C. Tanner - AppreciateHub from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="43f35-139">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="43f35-139">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="43f35-141">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="43f35-141">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="43f35-142">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="43f35-142">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="43f35-144">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="43f35-144">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="43f35-146">Hello keresési mezőbe, írja be a **O.C. Péter - AppreciateHub**.</span><span class="sxs-lookup"><span data-stu-id="43f35-146">In hello search box, type **O.C. Tanner - AppreciateHub**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-oc-tanner-tutorial/tutorial_octannerappreciatehub_search.png)

5. <span data-ttu-id="43f35-148">A hello eredmények panelen válassza ki a **O.C. Péter - AppreciateHub**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="43f35-148">In hello results panel, select **O.C. Tanner - AppreciateHub**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-oc-tanner-tutorial/tutorial_octannerappreciatehub_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="43f35-150">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="43f35-150">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="43f35-151">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést a O.C.</span><span class="sxs-lookup"><span data-stu-id="43f35-151">In this section, you configure and test Azure AD single sign-on with O.C.</span></span> <span data-ttu-id="43f35-152">Péter - AppreciateHub "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="43f35-152">Tanner - AppreciateHub based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="43f35-153">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow O.C. milyen hello megfelelőjére felhasználó</span><span class="sxs-lookup"><span data-stu-id="43f35-153">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in O.C.</span></span> <span data-ttu-id="43f35-154">Péter - AppreciateHub tooa felhasználó a Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="43f35-154">Tanner - AppreciateHub is tooa user in Azure AD.</span></span> <span data-ttu-id="43f35-155">Más szóval egy Azure AD-felhasználó és a kapcsolódó felhasználó hello O.C. közötti kapcsolat kapcsolatot</span><span class="sxs-lookup"><span data-stu-id="43f35-155">In other words, a link relationship between an Azure AD user and hello related user in O.C.</span></span> <span data-ttu-id="43f35-156">Péter - AppreciateHub létrehozott toobe kell.</span><span class="sxs-lookup"><span data-stu-id="43f35-156">Tanner - AppreciateHub needs toobe established.</span></span>

<span data-ttu-id="43f35-157">A O.C.</span><span class="sxs-lookup"><span data-stu-id="43f35-157">In O.C.</span></span> <span data-ttu-id="43f35-158">Péter - AppreciateHub, hozzárendelése hello értékének hello **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="43f35-158">Tanner - AppreciateHub, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="43f35-159">tooconfigure és az Azure AD az egyszeri bejelentkezés O.C.-teszthez</span><span class="sxs-lookup"><span data-stu-id="43f35-159">tooconfigure and test Azure AD single sign-on with O.C.</span></span> <span data-ttu-id="43f35-160">Péter - AppreciateHub, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="43f35-160">Tanner - AppreciateHub, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="43f35-161">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="43f35-161">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="43f35-162">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="43f35-162">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="43f35-163">**[Egy O.C. létrehozása Péter - AppreciateHub tesztfelhasználó](#creating-a-oc-tanner---appreciatehub-test-user)**  -toohave egy megfelelője a Britta Simon a O.C.</span><span class="sxs-lookup"><span data-stu-id="43f35-163">**[Creating a O.C. Tanner - AppreciateHub test user](#creating-a-oc-tanner---appreciatehub-test-user)** - toohave a counterpart of Britta Simon in O.C.</span></span> <span data-ttu-id="43f35-164">Péter - AppreciateHub, amely csatolt toohello felhasználói az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="43f35-164">Tanner - AppreciateHub that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="43f35-165">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="43f35-165">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="43f35-166">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="43f35-166">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="43f35-167">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="43f35-167">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="43f35-168">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és a O.C. az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="43f35-168">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your O.C.</span></span> <span data-ttu-id="43f35-169">Péter - AppreciateHub alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="43f35-169">Tanner - AppreciateHub application.</span></span>

<span data-ttu-id="43f35-170">**az Azure AD tooconfigure egyszeri bejelentkezést a O.C. Péter - AppreciateHub, hajtsa végre az alábbi lépésekkel hello:**</span><span class="sxs-lookup"><span data-stu-id="43f35-170">**tooconfigure Azure AD single sign-on with O.C. Tanner - AppreciateHub, perform hello following steps:**</span></span>

1. <span data-ttu-id="43f35-171">Az Azure portál, a hello hello **O.C. Péter - AppreciateHub** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="43f35-171">In hello Azure portal, on hello **O.C. Tanner - AppreciateHub** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="43f35-173">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="43f35-173">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-oc-tanner-tutorial/tutorial_octannerappreciatehub_samlbase.png)

3. <span data-ttu-id="43f35-175">A hello **O.C. Péter - AppreciateHub tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="43f35-175">On hello **O.C. Tanner - AppreciateHub Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-oc-tanner-tutorial/tutorial_octannerappreciatehub_url.png)

    <span data-ttu-id="43f35-177">a.</span><span class="sxs-lookup"><span data-stu-id="43f35-177">a.</span></span> <span data-ttu-id="43f35-178">A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<companyname>.appreciatehub.com/fed/sp/authnResponse20`</span><span class="sxs-lookup"><span data-stu-id="43f35-178">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<companyname>.appreciatehub.com/fed/sp/authnResponse20`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="43f35-179">Ez az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="43f35-179">This value is not real.</span></span> <span data-ttu-id="43f35-180">Frissítse ezt az értéket a hello tényleges válasz URL-CÍMEN.</span><span class="sxs-lookup"><span data-stu-id="43f35-180">Update this value with hello actual Reply URL.</span></span> <span data-ttu-id="43f35-181">Ügyfél [O.C. Péter - AppreciateHub támogatási csoport](mailto:sso@octanner.com) tooget ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="43f35-181">Contact [O.C. Tanner - AppreciateHub support team](mailto:sso@octanner.com) tooget this value.</span></span>

    <span data-ttu-id="43f35-182">b.</span><span class="sxs-lookup"><span data-stu-id="43f35-182">b.</span></span> <span data-ttu-id="43f35-183">Nyissa meg hello metaadatfájl hello a következő hivatkozás használatával: [https://fed.appreciatehub.com/fed/sp/metadata](https://fed.appreciatehub.com/fed/sp/metadata).</span><span class="sxs-lookup"><span data-stu-id="43f35-183">Open hello metadata file using hello following link: [https://fed.appreciatehub.com/fed/sp/metadata](https://fed.appreciatehub.com/fed/sp/metadata).</span></span>
   
    <span data-ttu-id="43f35-184">c.</span><span class="sxs-lookup"><span data-stu-id="43f35-184">c.</span></span> <span data-ttu-id="43f35-185">Keresse meg a hello **md:AssertionConsumerService** csomópont.</span><span class="sxs-lookup"><span data-stu-id="43f35-185">Locate hello **md:AssertionConsumerService** node.</span></span> 
   
    <span data-ttu-id="43f35-186">d.</span><span class="sxs-lookup"><span data-stu-id="43f35-186">d.</span></span> <span data-ttu-id="43f35-187">Másolja a hello hello értékének **hely** attribútum.</span><span class="sxs-lookup"><span data-stu-id="43f35-187">Copy hello value of hello **Location** attribute.</span></span> 
   
    ![Alkalmazásbeállítások konfigurálása][12]
   
    <span data-ttu-id="43f35-189">e.</span><span class="sxs-lookup"><span data-stu-id="43f35-189">e.</span></span> <span data-ttu-id="43f35-190">A hello **URL-cím bejelentkezési** szövegmezőhöz túli hello érték hello előző lépésben szerzett be.</span><span class="sxs-lookup"><span data-stu-id="43f35-190">In hello **Sign On URL** textbox, past hello value you have obtained in hello previous step.</span></span>

4. <span data-ttu-id="43f35-191">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="43f35-191">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-oc-tanner-tutorial/tutorial_octannerappreciatehub_certificate.png) 

5. <span data-ttu-id="43f35-193">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="43f35-193">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="43f35-195">tooconfigure egyszeri bejelentkezést a **O.C. Péter - AppreciateHub** oldalon kell letöltött toosend hello **metaadatainak XML-kódja** túl[O.C. Péter - AppreciateHub támogatási csoport](mailto:sso@octanner.com).</span><span class="sxs-lookup"><span data-stu-id="43f35-195">tooconfigure single sign-on on **O.C. Tanner - AppreciateHub** side, you need toosend hello downloaded **Metadata XML** too[O.C. Tanner - AppreciateHub support team](mailto:sso@octanner.com).</span></span>

> [!TIP]
> <span data-ttu-id="43f35-196">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="43f35-196">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="43f35-197">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="43f35-197">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="43f35-198">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="43f35-198">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="43f35-199">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="43f35-199">Creating an Azure AD test user</span></span>
<span data-ttu-id="43f35-200">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="43f35-200">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="43f35-202">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="43f35-202">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="43f35-203">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="43f35-203">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="43f35-205">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="43f35-205">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="43f35-207">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="43f35-207">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="43f35-209">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="43f35-209">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="43f35-211">a.</span><span class="sxs-lookup"><span data-stu-id="43f35-211">a.</span></span> <span data-ttu-id="43f35-212">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="43f35-212">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="43f35-213">b.</span><span class="sxs-lookup"><span data-stu-id="43f35-213">b.</span></span> <span data-ttu-id="43f35-214">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="43f35-214">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="43f35-215">c.</span><span class="sxs-lookup"><span data-stu-id="43f35-215">c.</span></span> <span data-ttu-id="43f35-216">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="43f35-216">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="43f35-217">d.</span><span class="sxs-lookup"><span data-stu-id="43f35-217">d.</span></span> <span data-ttu-id="43f35-218">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="43f35-218">Click **Create**.</span></span>
 
### <a name="creating-a-oc-tanner---appreciatehub-test-user"></a><span data-ttu-id="43f35-219">Egy O.C. létrehozása</span><span class="sxs-lookup"><span data-stu-id="43f35-219">Creating a O.C.</span></span> <span data-ttu-id="43f35-220">Péter - AppreciateHub tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="43f35-220">Tanner - AppreciateHub test user</span></span>

<span data-ttu-id="43f35-221">hello ebben a szakaszban célja toocreate O.C. Britta Simon nevű felhasználó</span><span class="sxs-lookup"><span data-stu-id="43f35-221">hello objective of this section is toocreate a user called Britta Simon in O.C.</span></span> <span data-ttu-id="43f35-222">Péter - AppreciateHub.</span><span class="sxs-lookup"><span data-stu-id="43f35-222">Tanner - AppreciateHub.</span></span>

<span data-ttu-id="43f35-223">**toocreate egy felhasználó meghívta Britta Simon O.C. Péter - AppreciateHub, hajtsa végre az alábbi lépésekkel hello:**</span><span class="sxs-lookup"><span data-stu-id="43f35-223">**toocreate a user called Britta Simon in O.C. Tanner - AppreciateHub, perform hello following steps:**</span></span>

<span data-ttu-id="43f35-224">Kérje meg a [O.C. Péter - AppreciateHub támogatási csoport](mailto:sso@octanner.com) toocreate megegyező nameID attribútum hello ugyanezt az értéket a Britta Simon hello felhasználónév a Azure AD felhasználó.</span><span class="sxs-lookup"><span data-stu-id="43f35-224">Ask your [O.C. Tanner - AppreciateHub support team](mailto:sso@octanner.com) toocreate a user that has as nameID attribute hello same value as hello user name of Britta Simon in Azure AD.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="43f35-225">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="43f35-225">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="43f35-226">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooO.C megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="43f35-226">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooO.C.</span></span> <span data-ttu-id="43f35-227">Péter - AppreciateHub.</span><span class="sxs-lookup"><span data-stu-id="43f35-227">Tanner - AppreciateHub.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="43f35-229">**tooassign Britta Simon tooO.C. Péter - AppreciateHub, hajtsa végre az alábbi lépésekkel hello:**</span><span class="sxs-lookup"><span data-stu-id="43f35-229">**tooassign Britta Simon tooO.C. Tanner - AppreciateHub, perform hello following steps:**</span></span>

1. <span data-ttu-id="43f35-230">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="43f35-230">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="43f35-232">Hello alkalmazások listában válassza ki a **O.C. Péter - AppreciateHub**.</span><span class="sxs-lookup"><span data-stu-id="43f35-232">In hello applications list, select **O.C. Tanner - AppreciateHub**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-oc-tanner-tutorial/tutorial_octannerappreciatehub_app.png) 

3. <span data-ttu-id="43f35-234">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="43f35-234">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="43f35-236">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="43f35-236">Click **Add** button.</span></span> <span data-ttu-id="43f35-237">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="43f35-237">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="43f35-239">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="43f35-239">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="43f35-240">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="43f35-240">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="43f35-241">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="43f35-241">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="43f35-242">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="43f35-242">Testing single sign-on</span></span>

<span data-ttu-id="43f35-243">hello ebben a szakaszban célja tootest az egyszeri bejelentkezés konfigurációs használatával hello a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="43f35-243">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>  
<span data-ttu-id="43f35-244">Hello O.C. elemre</span><span class="sxs-lookup"><span data-stu-id="43f35-244">When you click hello O.C.</span></span> <span data-ttu-id="43f35-245">Péter - AppreciateHub csempe az hello hozzáférési panelre, akkor kapja meg automatikusan bejelentkezett tooyour O.C.</span><span class="sxs-lookup"><span data-stu-id="43f35-245">Tanner - AppreciateHub tile in hello Access Panel, you should get automatically signed-on tooyour O.C.</span></span> <span data-ttu-id="43f35-246">Péter - AppreciateHub alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="43f35-246">Tanner - AppreciateHub application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="43f35-247">További források</span><span class="sxs-lookup"><span data-stu-id="43f35-247">Additional resources</span></span>

* [<span data-ttu-id="43f35-248">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="43f35-248">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="43f35-249">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="43f35-249">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_04.png

[12]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_octanner_08.png

[100]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_203.png

