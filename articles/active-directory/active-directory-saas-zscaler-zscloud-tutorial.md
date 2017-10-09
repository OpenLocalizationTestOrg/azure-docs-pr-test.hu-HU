---
title: "Oktatóanyag: Azure Active Directoryval integrált Zscaler ZSCloud |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Zscaler ZSCloud között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 411d5684-a780-410a-9383-59f92cf569b5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: jeedes
ms.openlocfilehash: af6d5c1994e715cccf959cc9fd3ba998e5b9effa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-zscaler-zscloud"></a><span data-ttu-id="2dcd4-103">Oktatóanyag: Azure Active Directoryval integrált Zscaler ZSCloud</span><span class="sxs-lookup"><span data-stu-id="2dcd4-103">Tutorial: Azure Active Directory integration with Zscaler ZSCloud</span></span>

<span data-ttu-id="2dcd4-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Zscaler ZSCloud az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="2dcd4-104">In this tutorial, you learn how toointegrate Zscaler ZSCloud with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="2dcd4-105">Zscaler ZSCloud integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="2dcd4-105">Integrating Zscaler ZSCloud with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="2dcd4-106">Megadhatja a hozzáférés tooZscaler ZSCloud rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="2dcd4-106">You can control in Azure AD who has access tooZscaler ZSCloud</span></span>
- <span data-ttu-id="2dcd4-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooZscaler ZSCloud (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="2dcd4-107">You can enable your users tooautomatically get signed-on tooZscaler ZSCloud (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="2dcd4-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="2dcd4-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="2dcd4-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="2dcd4-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2dcd4-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="2dcd4-110">Prerequisites</span></span>

<span data-ttu-id="2dcd4-111">az Azure AD integrálása Zscaler ZSCloud tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="2dcd4-111">tooconfigure Azure AD integration with Zscaler ZSCloud, you need hello following items:</span></span>

- <span data-ttu-id="2dcd4-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="2dcd4-112">An Azure AD subscription</span></span>
- <span data-ttu-id="2dcd4-113">Egy Zscaler ZSCloud egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="2dcd4-113">A Zscaler ZSCloud single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="2dcd4-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="2dcd4-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="2dcd4-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="2dcd4-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="2dcd4-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="2dcd4-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="2dcd4-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="2dcd4-118">Scenario description</span></span>
<span data-ttu-id="2dcd4-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="2dcd4-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="2dcd4-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="2dcd4-121">Hello gyűjteményből Zscaler ZSCloud hozzáadása</span><span class="sxs-lookup"><span data-stu-id="2dcd4-121">Adding Zscaler ZSCloud from hello gallery</span></span>
2. <span data-ttu-id="2dcd4-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="2dcd4-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-zscaler-zscloud-from-hello-gallery"></a><span data-ttu-id="2dcd4-123">Hello gyűjteményből Zscaler ZSCloud hozzáadása</span><span class="sxs-lookup"><span data-stu-id="2dcd4-123">Adding Zscaler ZSCloud from hello gallery</span></span>
<span data-ttu-id="2dcd4-124">tooconfigure hello integrációja Zscaler ZSCloud az Azure AD-be, meg kell tooadd Zscaler ZSCloud hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-124">tooconfigure hello integration of Zscaler ZSCloud into Azure AD, you need tooadd Zscaler ZSCloud from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="2dcd4-125">**tooadd Zscaler ZSCloud hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="2dcd4-125">**tooadd Zscaler ZSCloud from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="2dcd4-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="2dcd4-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="2dcd4-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="2dcd4-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="2dcd4-133">Hello keresési mezőbe, írja be a **Zscaler ZSCloud**.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-133">In hello search box, type **Zscaler ZSCloud**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_zscalerzscloud_search.png)

5. <span data-ttu-id="2dcd4-135">A hello eredmények panelen válassza ki a **Zscaler ZSCloud**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-135">In hello results panel, select **Zscaler ZSCloud**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_zscalerzscloud_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="2dcd4-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="2dcd4-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="2dcd4-138">Ebben a szakaszban konfigurálhatja, és a vizsgálat az Azure AD egyszeri bejelentkezést a Zscaler ZSCloud "Britta Simon." nevű tesztfelhasználó alapján</span><span class="sxs-lookup"><span data-stu-id="2dcd4-138">In this section, you configure and test Azure AD single sign-on with Zscaler ZSCloud based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="2dcd4-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Zscaler ZSCloud tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Zscaler ZSCloud is tooa user in Azure AD.</span></span> <span data-ttu-id="2dcd4-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Zscaler ZSCloud közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-140">In other words, a link relationship between an Azure AD user and hello related user in Zscaler ZSCloud needs toobe established.</span></span>

<span data-ttu-id="2dcd4-141">Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** a Zscaler ZSCloud.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Zscaler ZSCloud.</span></span>

<span data-ttu-id="2dcd4-142">tooconfigure és az Azure AD az egyszeri bejelentkezés Zscaler ZSCloud-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="2dcd4-142">tooconfigure and test Azure AD single sign-on with Zscaler ZSCloud, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="2dcd4-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="2dcd4-144">**[Proxybeállítások konfigurálása](#configuring-proxy-settings)**  -tooconfigure hello proxybeállítások az Internet Explorerben</span><span class="sxs-lookup"><span data-stu-id="2dcd4-144">**[Configuring proxy settings](#configuring-proxy-settings)** - tooconfigure hello proxy settings in Internet Explorer</span></span>
2. <span data-ttu-id="2dcd4-145">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-145">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)**  - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="2dcd4-146">**[Zscaler ZSCloud tesztfelhasználó létrehozása](#creating-a-zscaler-zscloud-test-user)**  -toohave egy megfelelője a Britta Simon a Zscaler ZSCloud, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-146">**[Creating a Zscaler ZSCloud test user](#creating-a-zscaler-zscloud-test-user)** - toohave a counterpart of Britta Simon in Zscaler ZSCloud that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="2dcd4-147">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-147">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="2dcd4-148">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-148">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="2dcd4-149">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="2dcd4-149">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="2dcd4-150">Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és az Zscaler ZSCloud alkalmazásban egyszeri bejelentkezés beállítása.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-150">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Zscaler ZSCloud application.</span></span>

<span data-ttu-id="2dcd4-151">**tooconfigure az Azure AD egyszeri bejelentkezést a Zscaler ZSCloud, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="2dcd4-151">**tooconfigure Azure AD single sign-on with Zscaler ZSCloud, perform hello following steps:**</span></span>

1. <span data-ttu-id="2dcd4-152">Az Azure portál, a hello hello **Zscaler ZSCloud** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-152">In hello Azure portal, on hello **Zscaler ZSCloud** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="2dcd4-154">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-154">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_zscalerzscloud_samlbase.png)

3. <span data-ttu-id="2dcd4-156">A hello **Zscaler ZSCloud tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="2dcd4-156">On hello **Zscaler ZSCloud Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_zscalerzscloud_url.png)

     <span data-ttu-id="2dcd4-158">A hello **bejelentkezési URL-cím** szövegmező, a felhasználók toosign a tooyour ZScaler ZSCloud alkalmazás által használt típus hello URL.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-158">In hello **Sign-on URL** textbox, type hello URL used by your users toosign-on tooyour ZScaler ZSCloud application.</span></span>
    
    > [!NOTE] 
    > <span data-ttu-id="2dcd4-159">Ezt az értéket hello rendelkezik tooupdate tényleges bejelentkezési URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-159">You have tooupdate this value with hello actual Sign-On URL.</span></span> <span data-ttu-id="2dcd4-160">Ügyfél [Zscaler ZSCloud ügyfél-támogatási csoport](https://support.zscaler.com/hc/articles/210172606-Zscaler-is-Expanding-Its-Zscloud-Global-Footprint) tooget ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-160">Contact [Zscaler ZSCloud Client support team](https://support.zscaler.com/hc/articles/210172606-Zscaler-is-Expanding-Its-Zscloud-Global-Footprint) tooget this value.</span></span> 
 
4. <span data-ttu-id="2dcd4-161">A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-161">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_zscalerzscloud_certificate.png) 

5. <span data-ttu-id="2dcd4-163">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-163">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="2dcd4-165">A hello **Zscaler ZSCloud konfigurációs** kattintson **Zscaler ZSCloud konfigurálása** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-165">On hello **Zscaler ZSCloud Configuration** section, click **Configure Zscaler ZSCloud** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="2dcd4-166">Másolás hello **SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="2dcd4-166">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_zscalerzscloud_configure.png) 

7. <span data-ttu-id="2dcd4-168">Egy másik webes böngészőablakban jelentkezzen be tooyour ZScaler ZSCloud vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-168">In a different web browser window, log in tooyour ZScaler ZSCloud company site as an administrator.</span></span>

8. <span data-ttu-id="2dcd4-169">Hello hello felső menüben kattintson a **felügyeleti**.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-169">In hello menu on hello top, click **Administration**.</span></span>
   
    <span data-ttu-id="2dcd4-170">![Felügyeleti](./media/active-directory-saas-zscaler-zscloud-tutorial/ic800206.png "felügyeleti")</span><span class="sxs-lookup"><span data-stu-id="2dcd4-170">![Administration](./media/active-directory-saas-zscaler-zscloud-tutorial/ic800206.png "Administration")</span></span>

9. <span data-ttu-id="2dcd4-171">A **rendszergazdák kezelése & szerepkörök**, kattintson a **felhasználók kezelése & hitelesítési**.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-171">Under **Manage Administrators & Roles**, click **Manage Users & Authentication**.</span></span>   
            
    <span data-ttu-id="2dcd4-172">![Felhasználók & hitelesítés kezelése](./media/active-directory-saas-zscaler-zscloud-tutorial/ic800207.png "felhasználók & hitelesítés kezelése")</span><span class="sxs-lookup"><span data-stu-id="2dcd4-172">![Manage Users & Authentication](./media/active-directory-saas-zscaler-zscloud-tutorial/ic800207.png "Manage Users & Authentication")</span></span>

10. <span data-ttu-id="2dcd4-173">A hello **hitelesítési beállítások kiválasztása a szervezet** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="2dcd4-173">In hello **Choose Authentication Options for your Organization** section, perform hello following steps:</span></span>   
                
    <span data-ttu-id="2dcd4-174">![Hitelesítési](./media/active-directory-saas-zscaler-zscloud-tutorial/ic800208.png "hitelesítés")</span><span class="sxs-lookup"><span data-stu-id="2dcd4-174">![Authentication](./media/active-directory-saas-zscaler-zscloud-tutorial/ic800208.png "Authentication")</span></span>
   
    <span data-ttu-id="2dcd4-175">a.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-175">a.</span></span> <span data-ttu-id="2dcd4-176">Válassza ki **hitelesítés, SAML-alapú egyszeri bejelentkezést használó**.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-176">Select **Authenticate using SAML Single Sign-On**.</span></span>

    <span data-ttu-id="2dcd4-177">b.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-177">b.</span></span> <span data-ttu-id="2dcd4-178">Kattintson a **paramétereinek a konfigurálása SAML-alapú egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-178">Click **Configure SAML Single Sign-On Parameters**.</span></span>

11. <span data-ttu-id="2dcd4-179">A hello **konfigurálása SAML-alapú egyszeri bejelentkezés paraméterek** párbeszédpanel lapon hajtsa végre az alábbi lépésekkel hello, és kattintson a **kész**</span><span class="sxs-lookup"><span data-stu-id="2dcd4-179">On hello **Configure SAML Single Sign-On Parameters** dialog page, perform hello following steps, and then click **Done**</span></span>

    <span data-ttu-id="2dcd4-180">![Egyszeri bejelentkezés](./media/active-directory-saas-zscaler-zscloud-tutorial/ic800209.png "egyszeri bejelentkezés")</span><span class="sxs-lookup"><span data-stu-id="2dcd4-180">![Single Sign-On](./media/active-directory-saas-zscaler-zscloud-tutorial/ic800209.png "Single Sign-On")</span></span>
    
    <span data-ttu-id="2dcd4-181">a.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-181">a.</span></span> <span data-ttu-id="2dcd4-182">Beillesztés hello **SAML-alapú egyszeri bejelentkezési URL-címe** hello érték **hello SAML Portal toowhich felhasználók URL-CÍMÉT a hitelesítéshez küldött** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-182">Paste hello **SAML Single Sign-On Service URL** value into hello **URL of hello SAML Portal toowhich users are sent for authentication** textbox.</span></span>
    
    <span data-ttu-id="2dcd4-183">b.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-183">b.</span></span> <span data-ttu-id="2dcd4-184">A hello **attribútumot a bejelentkezési nevet tartalmazó** szövegmezőhöz típus **NameID**.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-184">In hello **Attribute containing Login Name** textbox, type **NameID**.</span></span>
    
    <span data-ttu-id="2dcd4-185">c.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-185">c.</span></span> <span data-ttu-id="2dcd4-186">tooupload a letöltött tanúsítvány kattintson **Zscaler pem**.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-186">tooupload your downloaded certificate, click **Zscaler pem**.</span></span>
    
    <span data-ttu-id="2dcd4-187">d.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-187">d.</span></span> <span data-ttu-id="2dcd4-188">Válassza ki **SAML-alapú automatikus-kiépítés engedélyezése**.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-188">Select **Enable SAML Auto-Provisioning**.</span></span>

12. <span data-ttu-id="2dcd4-189">A hello **felhasználói hitelesítés beállítása** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="2dcd4-189">On hello **Configure User Authentication** dialog page, perform hello following steps:</span></span>

    <span data-ttu-id="2dcd4-190">![Felügyeleti](./media/active-directory-saas-zscaler-zscloud-tutorial/ic800210.png "felügyeleti")</span><span class="sxs-lookup"><span data-stu-id="2dcd4-190">![Administration](./media/active-directory-saas-zscaler-zscloud-tutorial/ic800210.png "Administration")</span></span>
    
    <span data-ttu-id="2dcd4-191">a.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-191">a.</span></span> <span data-ttu-id="2dcd4-192">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-192">Click **Save**.</span></span>

    <span data-ttu-id="2dcd4-193">b.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-193">b.</span></span> <span data-ttu-id="2dcd4-194">Kattintson a **aktiválásához**.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-194">Click **Activate Now**.</span></span>

## <a name="configuring-proxy-settings"></a><span data-ttu-id="2dcd4-195">Proxybeállítások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="2dcd4-195">Configuring proxy settings</span></span>
### <a name="tooconfigure-hello-proxy-settings-in-internet-explorer"></a><span data-ttu-id="2dcd4-196">az Internet Explorer tooconfigure hello proxykiszolgáló beállításai</span><span class="sxs-lookup"><span data-stu-id="2dcd4-196">tooconfigure hello proxy settings in Internet Explorer</span></span>

1. <span data-ttu-id="2dcd4-197">Start **Internet Explorer**.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-197">Start **Internet Explorer**.</span></span>

2. <span data-ttu-id="2dcd4-198">Válassza ki **Internetbeállítások** a hello **eszközök** menü megnyitása hello **Internetbeállítások** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-198">Select **Internet options** from hello **Tools** menu for open hello **Internet Options** dialog.</span></span>   
    
     <span data-ttu-id="2dcd4-199">![Internetbeállítások](./media/active-directory-saas-zscaler-zscloud-tutorial/ic769492.png "Internetbeállítások")</span><span class="sxs-lookup"><span data-stu-id="2dcd4-199">![Internet Options](./media/active-directory-saas-zscaler-zscloud-tutorial/ic769492.png "Internet Options")</span></span>

3. <span data-ttu-id="2dcd4-200">Kattintson a hello **kapcsolatok** fülre.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-200">Click hello **Connections** tab.</span></span>   
  
     <span data-ttu-id="2dcd4-201">![Kapcsolatok](./media/active-directory-saas-zscaler-zscloud-tutorial/ic769493.png "kapcsolatok")</span><span class="sxs-lookup"><span data-stu-id="2dcd4-201">![Connections](./media/active-directory-saas-zscaler-zscloud-tutorial/ic769493.png "Connections")</span></span>

4. <span data-ttu-id="2dcd4-202">Kattintson a **LAN-beállítások** tooopen hello **LAN-beállítások** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-202">Click **LAN settings** tooopen hello **LAN Settings** dialog.</span></span>

5. <span data-ttu-id="2dcd4-203">A Proxy server szakasz hello hajtsa végre a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="2dcd4-203">In hello Proxy server section, perform hello following steps:</span></span>   
   
    <span data-ttu-id="2dcd4-204">![Proxykiszolgáló](./media/active-directory-saas-zscaler-zscloud-tutorial/ic769494.png "proxykiszolgáló")</span><span class="sxs-lookup"><span data-stu-id="2dcd4-204">![Proxy server](./media/active-directory-saas-zscaler-zscloud-tutorial/ic769494.png "Proxy server")</span></span>

    <span data-ttu-id="2dcd4-205">a.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-205">a.</span></span> <span data-ttu-id="2dcd4-206">Válassza ki **proxykiszolgálót használni a helyi hálózaton**.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-206">Select **Use a proxy server for your LAN**.</span></span>

    <span data-ttu-id="2dcd4-207">b.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-207">b.</span></span> <span data-ttu-id="2dcd4-208">Hello cím szövegmezőben, írja be a **gateway.zscalerone.net**.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-208">In hello Address textbox, type **gateway.zscalerone.net**.</span></span>

    <span data-ttu-id="2dcd4-209">c.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-209">c.</span></span> <span data-ttu-id="2dcd4-210">Hello Port szövegmezőben, írja be a **80**.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-210">In hello Port textbox, type **80**.</span></span>

    <span data-ttu-id="2dcd4-211">d.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-211">d.</span></span> <span data-ttu-id="2dcd4-212">Válassza ki **proxykiszolgáló kihagyása helyi címek esetén**.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-212">Select **Bypass proxy server for local addresses**.</span></span>

    <span data-ttu-id="2dcd4-213">e.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-213">e.</span></span> <span data-ttu-id="2dcd4-214">Kattintson a **OK** tooclose hello **helyi hálózati (LAN) beállításai** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-214">Click **OK** tooclose hello **Local Area Network (LAN) Settings** dialog.</span></span>

6. <span data-ttu-id="2dcd4-215">Kattintson a **OK** tooclose hello **Internetbeállítások** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-215">Click **OK** tooclose hello **Internet Options** dialog.</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="2dcd4-216">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="2dcd4-216">Creating an Azure AD test user</span></span>
<span data-ttu-id="2dcd4-217">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-217">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="2dcd4-219">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="2dcd4-219">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="2dcd4-220">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-220">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-zscaler-zscloud-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="2dcd4-222">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-222">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-zscaler-zscloud-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="2dcd4-224">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-224">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-zscaler-zscloud-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="2dcd4-226">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="2dcd4-226">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-zscaler-zscloud-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="2dcd4-228">a.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-228">a.</span></span> <span data-ttu-id="2dcd4-229">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-229">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="2dcd4-230">b.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-230">b.</span></span> <span data-ttu-id="2dcd4-231">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-231">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="2dcd4-232">c.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-232">c.</span></span> <span data-ttu-id="2dcd4-233">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-233">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="2dcd4-234">d.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-234">d.</span></span> <span data-ttu-id="2dcd4-235">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-235">Click **Create**.</span></span>

### <a name="creating-a-zscaler-zscloud-test-user"></a><span data-ttu-id="2dcd4-236">Zscaler ZSCloud tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="2dcd4-236">Creating a Zscaler ZSCloud test user</span></span>

<span data-ttu-id="2dcd4-237">az Azure AD tooenable felhasználók toolog tooZScaler ZSCloud, a kiépített tooZScaler ZSCloud kell lenniük.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-237">tooenable Azure AD users toolog in tooZScaler ZSCloud, they must be provisioned tooZScaler ZSCloud.</span></span>  
<span data-ttu-id="2dcd4-238">ZScaler ZSCloud hello esetben egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-238">In hello case of ZScaler ZSCloud, provisioning is a manual task.</span></span>

### <a name="tooconfigure-user-provisioning-perform-hello-following-steps"></a><span data-ttu-id="2dcd4-239">tooconfigure felhasználók átadásához, hajtsa végre a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="2dcd4-239">tooconfigure user provisioning, perform hello following steps:</span></span>

1. <span data-ttu-id="2dcd4-240">Jelentkezzen be tooyour **Zscaler** bérlő.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-240">Log in tooyour **Zscaler** tenant.</span></span>

2. <span data-ttu-id="2dcd4-241">Kattintson a **felügyeleti**.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-241">Click **Administration**.</span></span>   
   
    <span data-ttu-id="2dcd4-242">![Felügyeleti](./media/active-directory-saas-zscaler-zscloud-tutorial/ic781035.png "felügyeleti")</span><span class="sxs-lookup"><span data-stu-id="2dcd4-242">![Administration](./media/active-directory-saas-zscaler-zscloud-tutorial/ic781035.png "Administration")</span></span>

3. <span data-ttu-id="2dcd4-243">Kattintson a **felhasználókezelés**.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-243">Click **User Management**.</span></span>   
        
     <span data-ttu-id="2dcd4-244">![Adja hozzá](./media/active-directory-saas-zscaler-zscloud-tutorial/ic781037.png "hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="2dcd4-244">![Add](./media/active-directory-saas-zscaler-zscloud-tutorial/ic781037.png "Add")</span></span>

4. <span data-ttu-id="2dcd4-245">A hello **felhasználók** lapra, majd **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-245">In hello **Users** tab, click **Add**.</span></span>
      
    <span data-ttu-id="2dcd4-246">![Adja hozzá](./media/active-directory-saas-zscaler-zscloud-tutorial/ic781037.png "hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="2dcd4-246">![Add](./media/active-directory-saas-zscaler-zscloud-tutorial/ic781037.png "Add")</span></span>

5. <span data-ttu-id="2dcd4-247">A felhasználó hozzáadása szakasz hello hajtsa végre a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="2dcd4-247">In hello Add User section, perform hello following steps:</span></span>
        
    <span data-ttu-id="2dcd4-248">![Felhasználó hozzáadása](./media/active-directory-saas-zscaler-zscloud-tutorial/ic781038.png "felhasználó hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="2dcd4-248">![Add User](./media/active-directory-saas-zscaler-zscloud-tutorial/ic781038.png "Add User")</span></span>
   
    <span data-ttu-id="2dcd4-249">a.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-249">a.</span></span> <span data-ttu-id="2dcd4-250">Típus hello **UserID**, **felhasználó megjelenített neve**, **jelszó**, **jelszó megerősítése**, majd válassza ki **csoportok**és hello **részleg** tooprovision kívánt fiók érvényes aad-ben.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-250">Type hello **UserID**, **User Display Name**, **Password**, **Confirm Password**, and then select **Groups** and hello **Department** of a valid AAD account you want tooprovision.</span></span>

    <span data-ttu-id="2dcd4-251">b.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-251">b.</span></span> <span data-ttu-id="2dcd4-252">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-252">Click **Save**.</span></span>

> [!NOTE]
> <span data-ttu-id="2dcd4-253">Bármely más ZScaler ZSCloud felhasználói fiók létrehozása eszközök vagy ZScaler ZSCloud tooprovision által nyújtott API-k AAD felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-253">You can use any other ZScaler ZSCloud user account creation tools or APIs provided by ZScaler ZSCloud tooprovision AAD user accounts.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="2dcd4-254">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="2dcd4-254">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="2dcd4-255">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooZscaler ZSCloud megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-255">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooZscaler ZSCloud.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="2dcd4-257">**tooassign Britta Simon tooZscaler ZSCloud, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="2dcd4-257">**tooassign Britta Simon tooZscaler ZSCloud, perform hello following steps:**</span></span>

1. <span data-ttu-id="2dcd4-258">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-258">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="2dcd4-260">Hello alkalmazások listában válassza ki a **Zscaler ZSCloud**.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-260">In hello applications list, select **Zscaler ZSCloud**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_zscalerzscloud_app.png) 

3. <span data-ttu-id="2dcd4-262">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-262">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="2dcd4-264">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-264">Click **Add** button.</span></span> <span data-ttu-id="2dcd4-265">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-265">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="2dcd4-267">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-267">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="2dcd4-268">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-268">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="2dcd4-269">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-269">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="2dcd4-270">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="2dcd4-270">Testing single sign-on</span></span>

<span data-ttu-id="2dcd4-271">Ha tootest az egyszeri bejelentkezés a beállításokat, nyissa meg a hozzáférési Panel hello.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-271">If you want tootest your single sign-on settings, open hello Access Panel.</span></span>

<span data-ttu-id="2dcd4-272">Hello Zscaler ZSCloud hello hozzáférési Panel csempére kattintva kapja meg automatikusan bejelentkezett tooyour Zscaler ZSCloud alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="2dcd4-272">When you click hello Zscaler ZSCloud tile in hello Access Panel, you should get automatically signed-on tooyour Zscaler ZSCloud application.</span></span>

<span data-ttu-id="2dcd4-273">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="2dcd4-273">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="2dcd4-274">További források</span><span class="sxs-lookup"><span data-stu-id="2dcd4-274">Additional resources</span></span>

* [<span data-ttu-id="2dcd4-275">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="2dcd4-275">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="2dcd4-276">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="2dcd4-276">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_203.png

