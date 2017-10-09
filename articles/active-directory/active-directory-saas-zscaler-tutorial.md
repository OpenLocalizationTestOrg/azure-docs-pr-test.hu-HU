---
title: "Oktatóanyag: Azure Active Directoryval integrált Zscaler |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Zscaler között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 68c453f7-aff1-4614-92d3-9b86f3ad99dc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.openlocfilehash: e2894534f5d6711fd6af618cd699fa5837b5bf26
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-zscaler"></a><span data-ttu-id="b393f-103">Oktatóanyag: Azure Active Directoryval integrált Zscaler</span><span class="sxs-lookup"><span data-stu-id="b393f-103">Tutorial: Azure Active Directory integration with Zscaler</span></span>

<span data-ttu-id="b393f-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Zscaler az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="b393f-104">In this tutorial, you learn how toointegrate Zscaler with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="b393f-105">Zscaler integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="b393f-105">Integrating Zscaler with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="b393f-106">Megadhatja a hozzáférés tooZscaler rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="b393f-106">You can control in Azure AD who has access tooZscaler</span></span>
- <span data-ttu-id="b393f-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooZscaler (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="b393f-107">You can enable your users tooautomatically get signed-on tooZscaler (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="b393f-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="b393f-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="b393f-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="b393f-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b393f-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="b393f-110">Prerequisites</span></span>

<span data-ttu-id="b393f-111">az Azure AD integrálása Zscaler tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="b393f-111">tooconfigure Azure AD integration with Zscaler, you need hello following items:</span></span>

- <span data-ttu-id="b393f-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="b393f-112">An Azure AD subscription</span></span>
- <span data-ttu-id="b393f-113">Egy Zscaler egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="b393f-113">A Zscaler single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="b393f-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="b393f-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="b393f-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="b393f-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="b393f-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="b393f-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="b393f-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b393f-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="b393f-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="b393f-118">Scenario description</span></span>
<span data-ttu-id="b393f-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="b393f-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="b393f-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="b393f-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="b393f-121">Hello gyűjteményből Zscaler hozzáadása</span><span class="sxs-lookup"><span data-stu-id="b393f-121">Adding Zscaler from hello gallery</span></span>
2. <span data-ttu-id="b393f-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="b393f-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-zscaler-from-hello-gallery"></a><span data-ttu-id="b393f-123">Hello gyűjteményből Zscaler hozzáadása</span><span class="sxs-lookup"><span data-stu-id="b393f-123">Adding Zscaler from hello gallery</span></span>
<span data-ttu-id="b393f-124">tooconfigure hello integrációja Zscaler az Azure AD-be, meg kell tooadd Zscaler hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="b393f-124">tooconfigure hello integration of Zscaler into Azure AD, you need tooadd Zscaler from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="b393f-125">**tooadd Zscaler hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="b393f-125">**tooadd Zscaler from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="b393f-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="b393f-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="b393f-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="b393f-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="b393f-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="b393f-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="b393f-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="b393f-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="b393f-133">Hello keresési mezőbe, írja be a **Zscaler**.</span><span class="sxs-lookup"><span data-stu-id="b393f-133">In hello search box, type **Zscaler**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-zscaler-tutorial/tutorial_zscaler_search.png)

5. <span data-ttu-id="b393f-135">A hello eredmények panelen válassza ki a **Zscaler**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="b393f-135">In hello results panel, select **Zscaler**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-zscaler-tutorial/tutorial_zscaler_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="b393f-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="b393f-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="b393f-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Zscaler.</span><span class="sxs-lookup"><span data-stu-id="b393f-138">In this section, you configure and test Azure AD single sign-on with Zscaler based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="b393f-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Zscaler tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="b393f-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Zscaler is tooa user in Azure AD.</span></span> <span data-ttu-id="b393f-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Zscaler közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="b393f-140">In other words, a link relationship between an Azure AD user and hello related user in Zscaler needs toobe established.</span></span>

<span data-ttu-id="b393f-141">Zscaler, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="b393f-141">In Zscaler, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="b393f-142">tooconfigure és az Azure AD az egyszeri bejelentkezés Zscaler-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="b393f-142">tooconfigure and test Azure AD single sign-on with Zscaler, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="b393f-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="b393f-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="b393f-144">**[Proxybeállítások konfigurálása](#configuring-proxy-settings)**  -tooconfigure hello proxybeállítások az Internet Explorerben</span><span class="sxs-lookup"><span data-stu-id="b393f-144">**[Configuring proxy settings](#configuring-proxy-settings)** - tooconfigure hello proxy settings in Internet Explorer</span></span>
3. <span data-ttu-id="b393f-145">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b393f-145">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
4. <span data-ttu-id="b393f-146">**[Zscaler tesztfelhasználó létrehozása](#creating-a-zscaler-test-user)**  -toohave egy megfelelője a Britta Simon a Zscaler, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="b393f-146">**[Creating a Zscaler test user](#creating-a-zscaler-test-user)** - toohave a counterpart of Britta Simon in Zscaler that is linked toohello Azure AD representation of user.</span></span>
5. <span data-ttu-id="b393f-147">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="b393f-147">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
6. <span data-ttu-id="b393f-148">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="b393f-148">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="b393f-149">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b393f-149">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="b393f-150">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Zscaler alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="b393f-150">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Zscaler application.</span></span>

<span data-ttu-id="b393f-151">**az Azure AD tooconfigure egyszeri bejelentkezést a Zscaler, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="b393f-151">**tooconfigure Azure AD single sign-on with Zscaler, perform hello following steps:**</span></span>

1. <span data-ttu-id="b393f-152">Az Azure portál, a hello hello **Zscaler** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="b393f-152">In hello Azure portal, on hello **Zscaler** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="b393f-154">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="b393f-154">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zscaler-tutorial/tutorial_zscaler_samlbase.png)

3. <span data-ttu-id="b393f-156">A hello **Zscaler tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="b393f-156">On hello **Zscaler Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zscaler-tutorial/tutorial_zscaler_url.png)

    <span data-ttu-id="b393f-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<companyname>.zsclaer.net`</span><span class="sxs-lookup"><span data-stu-id="b393f-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.zsclaer.net`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="b393f-159">Ez az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="b393f-159">This value is not real.</span></span> <span data-ttu-id="b393f-160">Frissítse ezt az értéket hello tényleges bejelentkezési URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="b393f-160">Update this value with hello actual Sign-On URL.</span></span> <span data-ttu-id="b393f-161">Ügyfél [Zscaler ügyfél-támogatási csoport](https://www.zscaler.com/company/contact) tooget ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="b393f-161">Contact [Zscaler Client support team](https://www.zscaler.com/company/contact) tooget this value.</span></span> 

4. <span data-ttu-id="b393f-162">A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="b393f-162">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zscaler-tutorial/tutorial_zscaler_certificate.png) 

5. <span data-ttu-id="b393f-164">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="b393f-164">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zscaler-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="b393f-166">A hello **Zscaler konfigurációs** kattintson **konfigurálása Zscaler** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="b393f-166">On hello **Zscaler Configuration** section, click **Configure Zscaler** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="b393f-167">Másolás hello **SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="b393f-167">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zscaler-tutorial/tutorial_zscaler_configure.png) 

7. <span data-ttu-id="b393f-169">Egy másik webes böngészőablakban jelentkezzen tooyour ZScaler vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="b393f-169">In a different web browser window, log in tooyour ZScaler company site as an administrator.</span></span>

8. <span data-ttu-id="b393f-170">Hello hello felső menüben kattintson a **felügyeleti**.</span><span class="sxs-lookup"><span data-stu-id="b393f-170">In hello menu on hello top, click **Administration**.</span></span>
   
    <span data-ttu-id="b393f-171">![Felügyeleti](./media/active-directory-saas-zscaler-tutorial/ic800206.png "felügyeleti")</span><span class="sxs-lookup"><span data-stu-id="b393f-171">![Administration](./media/active-directory-saas-zscaler-tutorial/ic800206.png "Administration")</span></span>

9. <span data-ttu-id="b393f-172">A **rendszergazdák kezelése & szerepkörök**, kattintson a **felhasználók kezelése & hitelesítési**.</span><span class="sxs-lookup"><span data-stu-id="b393f-172">Under **Manage Administrators & Roles**, click **Manage Users & Authentication**.</span></span>   
            
    <span data-ttu-id="b393f-173">![Felhasználók & hitelesítés kezelése](./media/active-directory-saas-zscaler-tutorial/ic800207.png "felhasználók & hitelesítés kezelése")</span><span class="sxs-lookup"><span data-stu-id="b393f-173">![Manage Users & Authentication](./media/active-directory-saas-zscaler-tutorial/ic800207.png "Manage Users & Authentication")</span></span>

10. <span data-ttu-id="b393f-174">A hello **hitelesítési beállítások kiválasztása a szervezet** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="b393f-174">In hello **Choose Authentication Options for your Organization** section, perform hello following steps:</span></span>   
                
    <span data-ttu-id="b393f-175">![Hitelesítési](./media/active-directory-saas-zscaler-tutorial/ic800208.png "hitelesítés")</span><span class="sxs-lookup"><span data-stu-id="b393f-175">![Authentication](./media/active-directory-saas-zscaler-tutorial/ic800208.png "Authentication")</span></span>
   
    <span data-ttu-id="b393f-176">a.</span><span class="sxs-lookup"><span data-stu-id="b393f-176">a.</span></span> <span data-ttu-id="b393f-177">Válassza ki **hitelesítés, SAML-alapú egyszeri bejelentkezést használó**.</span><span class="sxs-lookup"><span data-stu-id="b393f-177">Select **Authenticate using SAML Single Sign-On**.</span></span>

    <span data-ttu-id="b393f-178">b.</span><span class="sxs-lookup"><span data-stu-id="b393f-178">b.</span></span> <span data-ttu-id="b393f-179">Kattintson a **paramétereinek a konfigurálása SAML-alapú egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="b393f-179">Click **Configure SAML Single Sign-On Parameters**.</span></span>

11. <span data-ttu-id="b393f-180">A hello **konfigurálása SAML-alapú egyszeri bejelentkezés paraméterek** párbeszédpanel lapon hajtsa végre az alábbi lépésekkel hello, és kattintson a **kész**</span><span class="sxs-lookup"><span data-stu-id="b393f-180">On hello **Configure SAML Single Sign-On Parameters** dialog page, perform hello following steps, and then click **Done**</span></span>

    <span data-ttu-id="b393f-181">![Egyszeri bejelentkezés](./media/active-directory-saas-zscaler-tutorial/ic800209.png "egyszeri bejelentkezés")</span><span class="sxs-lookup"><span data-stu-id="b393f-181">![Single Sign-On](./media/active-directory-saas-zscaler-tutorial/ic800209.png "Single Sign-On")</span></span>
    
    <span data-ttu-id="b393f-182">a.</span><span class="sxs-lookup"><span data-stu-id="b393f-182">a.</span></span> <span data-ttu-id="b393f-183">Beillesztés hello **SAML-alapú egyszeri bejelentkezési URL-címe** értéket, amely akkor másolta, az Azure-portálon hello hello **hello SAML Portal toowhich felhasználók URL-CÍMÉT a hitelesítéshez küldött** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="b393f-183">Paste hello **SAML Single Sign-On Service URL** value, which you have copied from hello Azure portal into hello **URL of hello SAML Portal toowhich users are sent for authentication** textbox.</span></span>
    
    <span data-ttu-id="b393f-184">b.</span><span class="sxs-lookup"><span data-stu-id="b393f-184">b.</span></span> <span data-ttu-id="b393f-185">A hello **attribútumot a bejelentkezési nevet tartalmazó** szövegmezőhöz típus **NameID**.</span><span class="sxs-lookup"><span data-stu-id="b393f-185">In hello **Attribute containing Login Name** textbox, type **NameID**.</span></span>
    
    <span data-ttu-id="b393f-186">c.</span><span class="sxs-lookup"><span data-stu-id="b393f-186">c.</span></span> <span data-ttu-id="b393f-187">tooupload a letöltött tanúsítvány kattintson **Zscaler pem**.</span><span class="sxs-lookup"><span data-stu-id="b393f-187">tooupload your downloaded certificate, click **Zscaler pem**.</span></span>
    
    <span data-ttu-id="b393f-188">d.</span><span class="sxs-lookup"><span data-stu-id="b393f-188">d.</span></span> <span data-ttu-id="b393f-189">Válassza ki **SAML-alapú automatikus-kiépítés engedélyezése**.</span><span class="sxs-lookup"><span data-stu-id="b393f-189">Select **Enable SAML Auto-Provisioning**.</span></span>

12. <span data-ttu-id="b393f-190">A hello **felhasználói hitelesítés beállítása** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="b393f-190">On hello **Configure User Authentication** dialog page, perform hello following steps:</span></span>

    <span data-ttu-id="b393f-191">![Felügyeleti](./media/active-directory-saas-zscaler-tutorial/ic800210.png "felügyeleti")</span><span class="sxs-lookup"><span data-stu-id="b393f-191">![Administration](./media/active-directory-saas-zscaler-tutorial/ic800210.png "Administration")</span></span>
    
    <span data-ttu-id="b393f-192">a.</span><span class="sxs-lookup"><span data-stu-id="b393f-192">a.</span></span> <span data-ttu-id="b393f-193">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="b393f-193">Click **Save**.</span></span>

    <span data-ttu-id="b393f-194">b.</span><span class="sxs-lookup"><span data-stu-id="b393f-194">b.</span></span> <span data-ttu-id="b393f-195">Kattintson a **aktiválásához**.</span><span class="sxs-lookup"><span data-stu-id="b393f-195">Click **Activate Now**.</span></span>

## <a name="configuring-proxy-settings"></a><span data-ttu-id="b393f-196">Proxybeállítások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b393f-196">Configuring proxy settings</span></span>
### <a name="tooconfigure-hello-proxy-settings-in-internet-explorer"></a><span data-ttu-id="b393f-197">az Internet Explorer tooconfigure hello proxykiszolgáló beállításai</span><span class="sxs-lookup"><span data-stu-id="b393f-197">tooconfigure hello proxy settings in Internet Explorer</span></span>

1. <span data-ttu-id="b393f-198">Start **Internet Explorer**.</span><span class="sxs-lookup"><span data-stu-id="b393f-198">Start **Internet Explorer**.</span></span>

2. <span data-ttu-id="b393f-199">Válassza ki **Internetbeállítások** a hello **eszközök** menü megnyitása hello **Internetbeállítások** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b393f-199">Select **Internet options** from hello **Tools** menu for open hello **Internet Options** dialog.</span></span>   
    
     <span data-ttu-id="b393f-200">![Internetbeállítások](./media/active-directory-saas-zscaler-tutorial/ic769492.png "Internetbeállítások")</span><span class="sxs-lookup"><span data-stu-id="b393f-200">![Internet Options](./media/active-directory-saas-zscaler-tutorial/ic769492.png "Internet Options")</span></span>

3. <span data-ttu-id="b393f-201">Kattintson a hello **kapcsolatok** fülre.</span><span class="sxs-lookup"><span data-stu-id="b393f-201">Click hello **Connections** tab.</span></span>   
  
     <span data-ttu-id="b393f-202">![Kapcsolatok](./media/active-directory-saas-zscaler-tutorial/ic769493.png "kapcsolatok")</span><span class="sxs-lookup"><span data-stu-id="b393f-202">![Connections](./media/active-directory-saas-zscaler-tutorial/ic769493.png "Connections")</span></span>

4. <span data-ttu-id="b393f-203">Kattintson a **LAN-beállítások** tooopen hello **LAN-beállítások** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b393f-203">Click **LAN settings** tooopen hello **LAN Settings** dialog.</span></span>

5. <span data-ttu-id="b393f-204">A Proxy server szakasz hello hajtsa végre a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="b393f-204">In hello Proxy server section, perform hello following steps:</span></span>   
   
    <span data-ttu-id="b393f-205">![Proxykiszolgáló](./media/active-directory-saas-zscaler-tutorial/ic769494.png "proxykiszolgáló")</span><span class="sxs-lookup"><span data-stu-id="b393f-205">![Proxy server](./media/active-directory-saas-zscaler-tutorial/ic769494.png "Proxy server")</span></span>

    <span data-ttu-id="b393f-206">a.</span><span class="sxs-lookup"><span data-stu-id="b393f-206">a.</span></span> <span data-ttu-id="b393f-207">Válassza ki **proxykiszolgálót használni a helyi hálózaton**.</span><span class="sxs-lookup"><span data-stu-id="b393f-207">Select **Use a proxy server for your LAN**.</span></span>

    <span data-ttu-id="b393f-208">b.</span><span class="sxs-lookup"><span data-stu-id="b393f-208">b.</span></span> <span data-ttu-id="b393f-209">Hello cím szövegmezőben, írja be a **gateway.zscaler.net**.</span><span class="sxs-lookup"><span data-stu-id="b393f-209">In hello Address textbox, type **gateway.zscaler.net**.</span></span>

    <span data-ttu-id="b393f-210">c.</span><span class="sxs-lookup"><span data-stu-id="b393f-210">c.</span></span> <span data-ttu-id="b393f-211">Hello Port szövegmezőben, írja be a **80**.</span><span class="sxs-lookup"><span data-stu-id="b393f-211">In hello Port textbox, type **80**.</span></span>

    <span data-ttu-id="b393f-212">d.</span><span class="sxs-lookup"><span data-stu-id="b393f-212">d.</span></span> <span data-ttu-id="b393f-213">Válassza ki **proxykiszolgáló kihagyása helyi címek esetén**.</span><span class="sxs-lookup"><span data-stu-id="b393f-213">Select **Bypass proxy server for local addresses**.</span></span>

    <span data-ttu-id="b393f-214">e.</span><span class="sxs-lookup"><span data-stu-id="b393f-214">e.</span></span> <span data-ttu-id="b393f-215">Kattintson a **OK** tooclose hello **helyi hálózati (LAN) beállításai** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b393f-215">Click **OK** tooclose hello **Local Area Network (LAN) Settings** dialog.</span></span>

6. <span data-ttu-id="b393f-216">Kattintson a **OK** tooclose hello **Internetbeállítások** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b393f-216">Click **OK** tooclose hello **Internet Options** dialog.</span></span>

> [!TIP]
> <span data-ttu-id="b393f-217">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="b393f-217">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="b393f-218">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="b393f-218">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="b393f-219">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="b393f-219">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="b393f-220">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="b393f-220">Creating an Azure AD test user</span></span>
<span data-ttu-id="b393f-221">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="b393f-221">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="b393f-223">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="b393f-223">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="b393f-224">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="b393f-224">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-zscaler-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="b393f-226">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="b393f-226">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-zscaler-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="b393f-228">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b393f-228">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-zscaler-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="b393f-230">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="b393f-230">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-zscaler-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="b393f-232">a.</span><span class="sxs-lookup"><span data-stu-id="b393f-232">a.</span></span> <span data-ttu-id="b393f-233">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="b393f-233">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="b393f-234">b.</span><span class="sxs-lookup"><span data-stu-id="b393f-234">b.</span></span> <span data-ttu-id="b393f-235">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="b393f-235">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="b393f-236">c.</span><span class="sxs-lookup"><span data-stu-id="b393f-236">c.</span></span> <span data-ttu-id="b393f-237">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="b393f-237">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="b393f-238">d.</span><span class="sxs-lookup"><span data-stu-id="b393f-238">d.</span></span> <span data-ttu-id="b393f-239">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="b393f-239">Click **Create**.</span></span>
 
### <a name="creating-a-zscaler-test-user"></a><span data-ttu-id="b393f-240">Zscaler tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="b393f-240">Creating a Zscaler test user</span></span>

<span data-ttu-id="b393f-241">az Azure AD tooenable felhasználók toolog tooZScaler, a kiépített tooZScaler kell lenniük.</span><span class="sxs-lookup"><span data-stu-id="b393f-241">tooenable Azure AD users toolog in tooZScaler, they must be provisioned tooZScaler.</span></span>  
<span data-ttu-id="b393f-242">ZScaler hello esetben egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="b393f-242">In hello case of ZScaler, provisioning is a manual task.</span></span>

### <a name="tooconfigure-user-provisioning-perform-hello-following-steps"></a><span data-ttu-id="b393f-243">tooconfigure felhasználók átadásához, hajtsa végre a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="b393f-243">tooconfigure user provisioning, perform hello following steps:</span></span>

1. <span data-ttu-id="b393f-244">Jelentkezzen be tooyour **Zscaler** bérlő.</span><span class="sxs-lookup"><span data-stu-id="b393f-244">Log in tooyour **Zscaler** tenant.</span></span>

2. <span data-ttu-id="b393f-245">Kattintson a **felügyeleti**.</span><span class="sxs-lookup"><span data-stu-id="b393f-245">Click **Administration**.</span></span>   
   
    <span data-ttu-id="b393f-246">![Felügyeleti](./media/active-directory-saas-zscaler-tutorial/ic781035.png "felügyeleti")</span><span class="sxs-lookup"><span data-stu-id="b393f-246">![Administration](./media/active-directory-saas-zscaler-tutorial/ic781035.png "Administration")</span></span>

3. <span data-ttu-id="b393f-247">Kattintson a **felhasználókezelés**.</span><span class="sxs-lookup"><span data-stu-id="b393f-247">Click **User Management**.</span></span>   
        
     <span data-ttu-id="b393f-248">![Adja hozzá](./media/active-directory-saas-zscaler-tutorial/ic781036.png "hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="b393f-248">![Add](./media/active-directory-saas-zscaler-tutorial/ic781036.png "Add")</span></span>

4. <span data-ttu-id="b393f-249">A hello **felhasználók** lapra, majd **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="b393f-249">In hello **Users** tab, click **Add**.</span></span>
      
    <span data-ttu-id="b393f-250">![Adja hozzá](./media/active-directory-saas-zscaler-tutorial/ic781037.png "hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="b393f-250">![Add](./media/active-directory-saas-zscaler-tutorial/ic781037.png "Add")</span></span>

5. <span data-ttu-id="b393f-251">A felhasználó hozzáadása szakasz hello hajtsa végre a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="b393f-251">In hello Add User section, perform hello following steps:</span></span>
        
    <span data-ttu-id="b393f-252">![Felhasználó hozzáadása](./media/active-directory-saas-zscaler-tutorial/ic781038.png "felhasználó hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="b393f-252">![Add User](./media/active-directory-saas-zscaler-tutorial/ic781038.png "Add User")</span></span>
   
    <span data-ttu-id="b393f-253">a.</span><span class="sxs-lookup"><span data-stu-id="b393f-253">a.</span></span> <span data-ttu-id="b393f-254">Típus hello **UserID**, **felhasználó megjelenített neve**, **jelszó**, **jelszó megerősítése**, majd válassza ki **csoportok**és hello **részleg** tooprovision kívánt fiók érvényes aad-ben.</span><span class="sxs-lookup"><span data-stu-id="b393f-254">Type hello **UserID**, **User Display Name**, **Password**, **Confirm Password**, and then select **Groups** and hello **Department** of a valid AAD account you want tooprovision.</span></span>

    <span data-ttu-id="b393f-255">b.</span><span class="sxs-lookup"><span data-stu-id="b393f-255">b.</span></span> <span data-ttu-id="b393f-256">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="b393f-256">Click **Save**.</span></span>

> [!NOTE]
> <span data-ttu-id="b393f-257">Bármely más ZScaler felhasználói fiók létrehozása eszközök vagy ZScaler tooprovision által nyújtott API-k AAD felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="b393f-257">You can use any other ZScaler user account creation tools or APIs provided by ZScaler tooprovision AAD user accounts.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="b393f-258">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="b393f-258">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="b393f-259">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooZscaler megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="b393f-259">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooZscaler.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="b393f-261">**tooassign Britta Simon tooZscaler, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="b393f-261">**tooassign Britta Simon tooZscaler, perform hello following steps:**</span></span>

1. <span data-ttu-id="b393f-262">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="b393f-262">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="b393f-264">Hello alkalmazások listában válassza ki a **Zscaler**.</span><span class="sxs-lookup"><span data-stu-id="b393f-264">In hello applications list, select **Zscaler**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zscaler-tutorial/tutorial_zscaler_app.png) 

3. <span data-ttu-id="b393f-266">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="b393f-266">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="b393f-268">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="b393f-268">Click **Add** button.</span></span> <span data-ttu-id="b393f-269">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b393f-269">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="b393f-271">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="b393f-271">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="b393f-272">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b393f-272">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="b393f-273">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b393f-273">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="b393f-274">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="b393f-274">Testing single sign-on</span></span>

<span data-ttu-id="b393f-275">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="b393f-275">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="b393f-276">Ha a hozzáférési Panel hello hello Zscaler csempe gombra kattint, automatikusan bejelentkezett tooyour Zscaler alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="b393f-276">When you click hello Zscaler tile in hello Access Panel, you should get automatically signed-on tooyour Zscaler application.</span></span>
<span data-ttu-id="b393f-277">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="b393f-277">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b393f-278">További források</span><span class="sxs-lookup"><span data-stu-id="b393f-278">Additional resources</span></span>

* [<span data-ttu-id="b393f-279">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="b393f-279">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="b393f-280">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="b393f-280">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-zscaler-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-zscaler-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-zscaler-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-zscaler-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-zscaler-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-zscaler-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-zscaler-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-zscaler-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-zscaler-tutorial/tutorial_general_203.png

