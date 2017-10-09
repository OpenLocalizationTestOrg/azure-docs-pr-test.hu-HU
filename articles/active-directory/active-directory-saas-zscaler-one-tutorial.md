---
title: "Oktatóanyag: Azure Active Directoryval integrált Zscaler egy |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a Zscaler egy között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: f352e00d-68d3-4a77-bb92-717d055da56f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 5179cb2cc54482334d574951a1ac64e722e5f578
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-zscaler-one"></a><span data-ttu-id="21f66-103">Oktatóanyag: Azure Active Directoryval integrált Zscaler egy</span><span class="sxs-lookup"><span data-stu-id="21f66-103">Tutorial: Azure Active Directory integration with Zscaler One</span></span>

<span data-ttu-id="21f66-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Zscaler egy Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="21f66-104">In this tutorial, you learn how toointegrate Zscaler One with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="21f66-105">Egy Zscaler integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="21f66-105">Integrating Zscaler One with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="21f66-106">Megadhatja a hozzáférés tooZscaler egy rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="21f66-106">You can control in Azure AD who has access tooZscaler One</span></span>
- <span data-ttu-id="21f66-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooZscaler (egyszeri bejelentkezés) egy, az Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="21f66-107">You can enable your users tooautomatically get signed-on tooZscaler One (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="21f66-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="21f66-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="21f66-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="21f66-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="21f66-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="21f66-110">Prerequisites</span></span>

<span data-ttu-id="21f66-111">tooconfigure Zscaler egy Azure AD integrálása, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="21f66-111">tooconfigure Azure AD integration with Zscaler One, you need hello following items:</span></span>

- <span data-ttu-id="21f66-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="21f66-112">An Azure AD subscription</span></span>
- <span data-ttu-id="21f66-113">Egy Zscaler egy egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="21f66-113">A Zscaler One single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="21f66-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="21f66-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="21f66-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="21f66-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="21f66-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="21f66-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="21f66-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió Itt kaphat: [próbaverzió ajánlat](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="21f66-117">If you don't have an Azure AD trial environment, you can get a one-month trial here: [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="21f66-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="21f66-118">Scenario description</span></span>
<span data-ttu-id="21f66-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="21f66-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="21f66-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="21f66-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="21f66-121">Hello gyűjteményből Zscaler egy hozzáadása</span><span class="sxs-lookup"><span data-stu-id="21f66-121">Adding Zscaler One from hello gallery</span></span>
2. <span data-ttu-id="21f66-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="21f66-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-zscaler-one-from-hello-gallery"></a><span data-ttu-id="21f66-123">Hello gyűjteményből Zscaler egy hozzáadása</span><span class="sxs-lookup"><span data-stu-id="21f66-123">Adding Zscaler One from hello gallery</span></span>
<span data-ttu-id="21f66-124">tooconfigure hello integrációja Zscaler egy az Azure AD-be, meg kell tooadd Zscaler egy hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="21f66-124">tooconfigure hello integration of Zscaler One into Azure AD, you need tooadd Zscaler One from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="21f66-125">**hello gyűjteményből egy Zscaler tooadd hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="21f66-125">**tooadd Zscaler One from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="21f66-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="21f66-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="21f66-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="21f66-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="21f66-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="21f66-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="21f66-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="21f66-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="21f66-133">Hello keresési mezőbe, írja be a **Zscaler egy**.</span><span class="sxs-lookup"><span data-stu-id="21f66-133">In hello search box, type **Zscaler One**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-zscaler-one-tutorial/tutorial_zscalerone_search.png)

5. <span data-ttu-id="21f66-135">A hello eredmények panelen válassza ki a **Zscaler egy**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="21f66-135">In hello results panel, select **Zscaler One**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-zscaler-one-tutorial/tutorial_zscalerone_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="21f66-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="21f66-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="21f66-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD az egyszeri bejelentkezés Zscaler egy "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="21f66-138">In this section, you configure and test Azure AD single sign-on with Zscaler One based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="21f66-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Zscaler egy tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="21f66-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Zscaler One is tooa user in Azure AD.</span></span> <span data-ttu-id="21f66-140">Ez azt jelenti hello kapcsolódó felhasználói Zscaler első és az Azure AD-felhasználó közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="21f66-140">In other words, a link relationship between an Azure AD user and hello related user in Zscaler One needs toobe established.</span></span>

<span data-ttu-id="21f66-141">A Zscaler egy rendelje hello hello értékét **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="21f66-141">In Zscaler One, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="21f66-142">tooconfigure és tesztelése az Azure AD egyszeri bejelentkezést a Zscaler egy, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="21f66-142">tooconfigure and test Azure AD single sign-on with Zscaler One, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="21f66-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="21f66-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="21f66-144">**[Proxybeállítások konfigurálása](#configuring-proxy-settings)**  -tooconfigure hello proxybeállítások az Internet Explorerben</span><span class="sxs-lookup"><span data-stu-id="21f66-144">**[Configuring proxy settings](#configuring-proxy-settings)** - tooconfigure hello proxy settings in Internet Explorer</span></span>
3. <span data-ttu-id="21f66-145">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="21f66-145">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
4. <span data-ttu-id="21f66-146">**[Zscaler egy tesztfelhasználó létrehozása](#creating-a-zscaler-one-test-user)**  -toohave egy megfelelője a Britta Simon a Zscaler egy felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="21f66-146">**[Creating a Zscaler One test user](#creating-a-zscaler-one-test-user)** - toohave a counterpart of Britta Simon in Zscaler One that is linked toohello Azure AD representation of user.</span></span>
5. <span data-ttu-id="21f66-147">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="21f66-147">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
6. <span data-ttu-id="21f66-148">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="21f66-148">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="21f66-149">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="21f66-149">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="21f66-150">Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és az Zscaler egy alkalmazás egyszeri bejelentkezés beállítása.</span><span class="sxs-lookup"><span data-stu-id="21f66-150">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Zscaler One application.</span></span>

<span data-ttu-id="21f66-151">**az Azure AD tooconfigure egyszeri bejelentkezés Zscaler egy, a hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="21f66-151">**tooconfigure Azure AD single sign-on with Zscaler One, perform hello following steps:**</span></span>

1. <span data-ttu-id="21f66-152">Az Azure portál, a hello hello **Zscaler egy** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="21f66-152">In hello Azure portal, on hello **Zscaler One** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="21f66-154">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="21f66-154">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zscaler-one-tutorial/tutorial_zscalerone_samlbase.png)

3. <span data-ttu-id="21f66-156">A hello **Zscaler tartománya és URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="21f66-156">On hello **Zscaler One Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zscaler-one-tutorial/tutorial_zscalerone_url.png)

    <span data-ttu-id="21f66-158">Hello bejelentkezési URL-cím szövegmezőben írja be a felhasználók toosign a tooyour Zscaler egy alkalmazás által használt hello URL-cím.</span><span class="sxs-lookup"><span data-stu-id="21f66-158">In hello Sign-on URL textbox, type hello URL used by your users toosign-on tooyour Zscaler One application.</span></span>

    > [!NOTE] 
    > <span data-ttu-id="21f66-159">Ezt az értéket hello rendelkezik tooupdate tényleges bejelentkezési URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="21f66-159">You have tooupdate this value with hello actual Sign-On URL.</span></span> <span data-ttu-id="21f66-160">Ügyfél [Zscaler egy ügyfél-támogatási csoport](https://www.zscaler.com/company/contact) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="21f66-160">Contact [Zscaler One Client support team](https://www.zscaler.com/company/contact) tooget these values.</span></span>

4. <span data-ttu-id="21f66-161">A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="21f66-161">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zscaler-one-tutorial/tutorial_zscalerone_certificate.png) 

5. <span data-ttu-id="21f66-163">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="21f66-163">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zscaler-one-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="21f66-165">A hello **Zscaler egy konfigurációs** kattintson **Zscaler egy konfigurálása** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="21f66-165">On hello **Zscaler One Configuration** section, click **Configure Zscaler One** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="21f66-166">Másolás hello **SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="21f66-166">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zscaler-one-tutorial/tutorial_zscalerone_configure.png) 

7. <span data-ttu-id="21f66-168">Egy másik webes böngészőablakban jelentkezzen tooyour Zscaler egy vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="21f66-168">In a different web browser window, log in tooyour Zscaler One company site as an administrator.</span></span>

8. <span data-ttu-id="21f66-169">Hello hello felső menüben kattintson a **felügyeleti**.</span><span class="sxs-lookup"><span data-stu-id="21f66-169">In hello menu on hello top, click **Administration**.</span></span>
   
    <span data-ttu-id="21f66-170">![Felügyeleti](./media/active-directory-saas-zscaler-one-tutorial/ic800206.png "felügyeleti")</span><span class="sxs-lookup"><span data-stu-id="21f66-170">![Administration](./media/active-directory-saas-zscaler-one-tutorial/ic800206.png "Administration")</span></span>

9. <span data-ttu-id="21f66-171">A **rendszergazdák kezelése & szerepkörök**, kattintson a **felhasználók kezelése & hitelesítési**.</span><span class="sxs-lookup"><span data-stu-id="21f66-171">Under **Manage Administrators & Roles**, click **Manage Users & Authentication**.</span></span>   
            
    <span data-ttu-id="21f66-172">![Felhasználók & hitelesítés kezelése](./media/active-directory-saas-zscaler-one-tutorial/ic800207.png "felhasználók & hitelesítés kezelése")</span><span class="sxs-lookup"><span data-stu-id="21f66-172">![Manage Users & Authentication](./media/active-directory-saas-zscaler-one-tutorial/ic800207.png "Manage Users & Authentication")</span></span>

10. <span data-ttu-id="21f66-173">A hello **hitelesítési beállítások kiválasztása a szervezet** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="21f66-173">In hello **Choose Authentication Options for your Organization** section, perform hello following steps:</span></span>   
                
    <span data-ttu-id="21f66-174">![Hitelesítési](./media/active-directory-saas-zscaler-one-tutorial/ic800208.png "hitelesítés")</span><span class="sxs-lookup"><span data-stu-id="21f66-174">![Authentication](./media/active-directory-saas-zscaler-one-tutorial/ic800208.png "Authentication")</span></span>
   
    <span data-ttu-id="21f66-175">a.</span><span class="sxs-lookup"><span data-stu-id="21f66-175">a.</span></span> <span data-ttu-id="21f66-176">Válassza ki **hitelesítés, SAML-alapú egyszeri bejelentkezést használó**.</span><span class="sxs-lookup"><span data-stu-id="21f66-176">Select **Authenticate using SAML Single Sign-On**.</span></span>

    <span data-ttu-id="21f66-177">b.</span><span class="sxs-lookup"><span data-stu-id="21f66-177">b.</span></span> <span data-ttu-id="21f66-178">Kattintson a **paramétereinek a konfigurálása SAML-alapú egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="21f66-178">Click **Configure SAML Single Sign-On Parameters**.</span></span>

11. <span data-ttu-id="21f66-179">A hello **konfigurálása SAML-alapú egyszeri bejelentkezés paraméterek** párbeszédpanel lapon hajtsa végre az alábbi lépésekkel hello, és kattintson a **kész**</span><span class="sxs-lookup"><span data-stu-id="21f66-179">On hello **Configure SAML Single Sign-On Parameters** dialog page, perform hello following steps, and then click **Done**</span></span>

    <span data-ttu-id="21f66-180">![Egyszeri bejelentkezés](./media/active-directory-saas-zscaler-one-tutorial/ic800209.png "egyszeri bejelentkezés")</span><span class="sxs-lookup"><span data-stu-id="21f66-180">![Single Sign-On](./media/active-directory-saas-zscaler-one-tutorial/ic800209.png "Single Sign-On")</span></span>
    
    <span data-ttu-id="21f66-181">a.</span><span class="sxs-lookup"><span data-stu-id="21f66-181">a.</span></span> <span data-ttu-id="21f66-182">Beillesztés hello **SAML-alapú egyszeri bejelentkezési URL-címe** értéket, amely akkor másolta, az Azure-portálon hello hello **hello SAML Portal toowhich felhasználók URL-CÍMÉT a hitelesítéshez küldött** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="21f66-182">Paste hello **SAML Single Sign-On Service URL** value, which you have copied from hello Azure portal into hello **URL of hello SAML Portal toowhich users are sent for authentication** textbox.</span></span>
    
    <span data-ttu-id="21f66-183">b.</span><span class="sxs-lookup"><span data-stu-id="21f66-183">b.</span></span> <span data-ttu-id="21f66-184">A hello **attribútumot a bejelentkezési nevet tartalmazó** szövegmezőhöz típus **NameID**.</span><span class="sxs-lookup"><span data-stu-id="21f66-184">In hello **Attribute containing Login Name** textbox, type **NameID**.</span></span>
    
    <span data-ttu-id="21f66-185">c.</span><span class="sxs-lookup"><span data-stu-id="21f66-185">c.</span></span> <span data-ttu-id="21f66-186">tooupload a letöltött tanúsítvány kattintson **Zscaler pem**.</span><span class="sxs-lookup"><span data-stu-id="21f66-186">tooupload your downloaded certificate, click **Zscaler pem**.</span></span>
    
    <span data-ttu-id="21f66-187">d.</span><span class="sxs-lookup"><span data-stu-id="21f66-187">d.</span></span> <span data-ttu-id="21f66-188">Válassza ki **SAML-alapú automatikus-kiépítés engedélyezése**.</span><span class="sxs-lookup"><span data-stu-id="21f66-188">Select **Enable SAML Auto-Provisioning**.</span></span>

12. <span data-ttu-id="21f66-189">A hello **felhasználói hitelesítés beállítása** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="21f66-189">On hello **Configure User Authentication** dialog page, perform hello following steps:</span></span>

    <span data-ttu-id="21f66-190">![Felügyeleti](./media/active-directory-saas-zscaler-one-tutorial/ic800210.png "felügyeleti")</span><span class="sxs-lookup"><span data-stu-id="21f66-190">![Administration](./media/active-directory-saas-zscaler-one-tutorial/ic800210.png "Administration")</span></span>
    
    <span data-ttu-id="21f66-191">a.</span><span class="sxs-lookup"><span data-stu-id="21f66-191">a.</span></span> <span data-ttu-id="21f66-192">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="21f66-192">Click **Save**.</span></span>

    <span data-ttu-id="21f66-193">b.</span><span class="sxs-lookup"><span data-stu-id="21f66-193">b.</span></span> <span data-ttu-id="21f66-194">Kattintson a **aktiválásához**.</span><span class="sxs-lookup"><span data-stu-id="21f66-194">Click **Activate Now**.</span></span>

## <a name="configuring-proxy-settings"></a><span data-ttu-id="21f66-195">Proxybeállítások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="21f66-195">Configuring proxy settings</span></span>
### <a name="tooconfigure-hello-proxy-settings-in-internet-explorer"></a><span data-ttu-id="21f66-196">az Internet Explorer tooconfigure hello proxykiszolgáló beállításai</span><span class="sxs-lookup"><span data-stu-id="21f66-196">tooconfigure hello proxy settings in Internet Explorer</span></span>

1. <span data-ttu-id="21f66-197">Start **Internet Explorer**.</span><span class="sxs-lookup"><span data-stu-id="21f66-197">Start **Internet Explorer**.</span></span>

2. <span data-ttu-id="21f66-198">Válassza ki **Internetbeállítások** a hello **eszközök** menü megnyitása hello **Internetbeállítások** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="21f66-198">Select **Internet options** from hello **Tools** menu for open hello **Internet Options** dialog.</span></span>   
    
     <span data-ttu-id="21f66-199">![Internetbeállítások](./media/active-directory-saas-zscaler-one-tutorial/ic769492.png "Internetbeállítások")</span><span class="sxs-lookup"><span data-stu-id="21f66-199">![Internet Options](./media/active-directory-saas-zscaler-one-tutorial/ic769492.png "Internet Options")</span></span>

3. <span data-ttu-id="21f66-200">Kattintson a hello **kapcsolatok** fülre.</span><span class="sxs-lookup"><span data-stu-id="21f66-200">Click hello **Connections** tab.</span></span>   
  
     <span data-ttu-id="21f66-201">![Kapcsolatok](./media/active-directory-saas-zscaler-one-tutorial/ic769493.png "kapcsolatok")</span><span class="sxs-lookup"><span data-stu-id="21f66-201">![Connections](./media/active-directory-saas-zscaler-one-tutorial/ic769493.png "Connections")</span></span>

4. <span data-ttu-id="21f66-202">Kattintson a **LAN-beállítások** tooopen hello **LAN-beállítások** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="21f66-202">Click **LAN settings** tooopen hello **LAN Settings** dialog.</span></span>

5. <span data-ttu-id="21f66-203">A Proxy server szakasz hello hajtsa végre a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="21f66-203">In hello Proxy server section, perform hello following steps:</span></span>   
   
    <span data-ttu-id="21f66-204">![Proxykiszolgáló](./media/active-directory-saas-zscaler-one-tutorial/ic769494.png "proxykiszolgáló")</span><span class="sxs-lookup"><span data-stu-id="21f66-204">![Proxy server](./media/active-directory-saas-zscaler-one-tutorial/ic769494.png "Proxy server")</span></span>

    <span data-ttu-id="21f66-205">a.</span><span class="sxs-lookup"><span data-stu-id="21f66-205">a.</span></span> <span data-ttu-id="21f66-206">Válassza ki **proxykiszolgálót használni a helyi hálózaton**.</span><span class="sxs-lookup"><span data-stu-id="21f66-206">Select **Use a proxy server for your LAN**.</span></span>

    <span data-ttu-id="21f66-207">b.</span><span class="sxs-lookup"><span data-stu-id="21f66-207">b.</span></span> <span data-ttu-id="21f66-208">Hello cím szövegmezőben, írja be a **gateway.zscalerone.net**.</span><span class="sxs-lookup"><span data-stu-id="21f66-208">In hello Address textbox, type **gateway.zscalerone.net**.</span></span>

    <span data-ttu-id="21f66-209">c.</span><span class="sxs-lookup"><span data-stu-id="21f66-209">c.</span></span> <span data-ttu-id="21f66-210">Hello Port szövegmezőben, írja be a **80**.</span><span class="sxs-lookup"><span data-stu-id="21f66-210">In hello Port textbox, type **80**.</span></span>

    <span data-ttu-id="21f66-211">d.</span><span class="sxs-lookup"><span data-stu-id="21f66-211">d.</span></span> <span data-ttu-id="21f66-212">Válassza ki **proxykiszolgáló kihagyása helyi címek esetén**.</span><span class="sxs-lookup"><span data-stu-id="21f66-212">Select **Bypass proxy server for local addresses**.</span></span>

    <span data-ttu-id="21f66-213">e.</span><span class="sxs-lookup"><span data-stu-id="21f66-213">e.</span></span> <span data-ttu-id="21f66-214">Kattintson a **OK** tooclose hello **helyi hálózati (LAN) beállításai** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="21f66-214">Click **OK** tooclose hello **Local Area Network (LAN) Settings** dialog.</span></span>

6. <span data-ttu-id="21f66-215">Kattintson a **OK** tooclose hello **Internetbeállítások** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="21f66-215">Click **OK** tooclose hello **Internet Options** dialog.</span></span>

> [!TIP]
> <span data-ttu-id="21f66-216">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="21f66-216">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="21f66-217">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="21f66-217">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="21f66-218">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="21f66-218">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="21f66-219">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="21f66-219">Creating an Azure AD test user</span></span>
<span data-ttu-id="21f66-220">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="21f66-220">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="21f66-222">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="21f66-222">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="21f66-223">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="21f66-223">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-zscaler-one-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="21f66-225">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="21f66-225">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-zscaler-one-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="21f66-227">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="21f66-227">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-zscaler-one-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="21f66-229">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="21f66-229">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-zscaler-one-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="21f66-231">a.</span><span class="sxs-lookup"><span data-stu-id="21f66-231">a.</span></span> <span data-ttu-id="21f66-232">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="21f66-232">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="21f66-233">b.</span><span class="sxs-lookup"><span data-stu-id="21f66-233">b.</span></span> <span data-ttu-id="21f66-234">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="21f66-234">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="21f66-235">c.</span><span class="sxs-lookup"><span data-stu-id="21f66-235">c.</span></span> <span data-ttu-id="21f66-236">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="21f66-236">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="21f66-237">d.</span><span class="sxs-lookup"><span data-stu-id="21f66-237">d.</span></span> <span data-ttu-id="21f66-238">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="21f66-238">Click **Create**.</span></span>
 
### <a name="creating-a-zscaler-one-test-user"></a><span data-ttu-id="21f66-239">Zscaler egy tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="21f66-239">Creating a Zscaler One test user</span></span>

<span data-ttu-id="21f66-240">az Azure AD tooenable felhasználók toolog tooZscaler egy, az egyik kiosztott tooZscaler kell lenniük.</span><span class="sxs-lookup"><span data-stu-id="21f66-240">tooenable Azure AD users toolog in tooZscaler One, they must be provisioned tooZscaler One.</span></span> <span data-ttu-id="21f66-241">Hello esetében Zscaler egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="21f66-241">In hello case of Zscaler One, provisioning is a manual task.</span></span>

### <a name="tooconfigure-user-provisioning-perform-hello-following-steps"></a><span data-ttu-id="21f66-242">tooconfigure felhasználók átadásához, hajtsa végre a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="21f66-242">tooconfigure user provisioning, perform hello following steps:</span></span>

1. <span data-ttu-id="21f66-243">Jelentkezzen be tooyour **Zscaler egy** bérlő.</span><span class="sxs-lookup"><span data-stu-id="21f66-243">Log in tooyour **Zscaler One** tenant.</span></span>

2. <span data-ttu-id="21f66-244">Kattintson a **felügyeleti**.</span><span class="sxs-lookup"><span data-stu-id="21f66-244">Click **Administration**.</span></span>   
   
    <span data-ttu-id="21f66-245">![Felügyeleti](./media/active-directory-saas-zscaler-one-tutorial/ic781035.png "felügyeleti")</span><span class="sxs-lookup"><span data-stu-id="21f66-245">![Administration](./media/active-directory-saas-zscaler-one-tutorial/ic781035.png "Administration")</span></span>

3. <span data-ttu-id="21f66-246">Kattintson a **felhasználókezelés**.</span><span class="sxs-lookup"><span data-stu-id="21f66-246">Click **User Management**.</span></span>   
        
     <span data-ttu-id="21f66-247">![Adja hozzá](./media/active-directory-saas-zscaler-one-tutorial/ic781036.png "hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="21f66-247">![Add](./media/active-directory-saas-zscaler-one-tutorial/ic781036.png "Add")</span></span>

4. <span data-ttu-id="21f66-248">A hello **felhasználók** lapra, majd **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="21f66-248">In hello **Users** tab, click **Add**.</span></span>
      
    <span data-ttu-id="21f66-249">![Adja hozzá](./media/active-directory-saas-zscaler-one-tutorial/ic781037.png "hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="21f66-249">![Add](./media/active-directory-saas-zscaler-one-tutorial/ic781037.png "Add")</span></span>

5. <span data-ttu-id="21f66-250">A felhasználó hozzáadása szakasz hello hajtsa végre a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="21f66-250">In hello Add User section, perform hello following steps:</span></span>
        
    <span data-ttu-id="21f66-251">![Felhasználó hozzáadása](./media/active-directory-saas-zscaler-one-tutorial/ic781038.png "felhasználó hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="21f66-251">![Add User](./media/active-directory-saas-zscaler-one-tutorial/ic781038.png "Add User")</span></span>
   
    <span data-ttu-id="21f66-252">a.</span><span class="sxs-lookup"><span data-stu-id="21f66-252">a.</span></span> <span data-ttu-id="21f66-253">Típus hello **UserID**, **felhasználó megjelenített neve**, **jelszó**, **jelszó megerősítése**, majd válassza ki **csoportok**és hello **részleg** egy érvényes Azure AD-fiókot szeretne tooprovision.</span><span class="sxs-lookup"><span data-stu-id="21f66-253">Type hello **UserID**, **User Display Name**, **Password**, **Confirm Password**, and then select **Groups** and hello **Department** of a valid Azure AD account you want tooprovision.</span></span>

    <span data-ttu-id="21f66-254">b.</span><span class="sxs-lookup"><span data-stu-id="21f66-254">b.</span></span> <span data-ttu-id="21f66-255">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="21f66-255">Click **Save**.</span></span>

> [!NOTE]
> <span data-ttu-id="21f66-256">Bármely más Zscaler egy felhasználói fiók létrehozása eszközök, vagy egy Zscaler tooprovision által nyújtott API-k az Azure AD felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="21f66-256">You can use any other Zscaler One user account creation tools or APIs provided by Zscaler One tooprovision Azure AD user accounts.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="21f66-257">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="21f66-257">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="21f66-258">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezéshez egy hozzáférési tooZscaler megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="21f66-258">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooZscaler One.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="21f66-260">**tooassign Britta Simon tooZscaler, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="21f66-260">**tooassign Britta Simon tooZscaler One, perform hello following steps:**</span></span>

1. <span data-ttu-id="21f66-261">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="21f66-261">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="21f66-263">Hello alkalmazások listában válassza ki a **Zscaler egy**.</span><span class="sxs-lookup"><span data-stu-id="21f66-263">In hello applications list, select **Zscaler One**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zscaler-one-tutorial/tutorial_zscalerone_app.png) 

3. <span data-ttu-id="21f66-265">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="21f66-265">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="21f66-267">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="21f66-267">Click **Add** button.</span></span> <span data-ttu-id="21f66-268">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="21f66-268">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="21f66-270">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="21f66-270">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="21f66-271">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="21f66-271">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="21f66-272">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="21f66-272">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="21f66-273">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="21f66-273">Testing single sign-on</span></span>

<span data-ttu-id="21f66-274">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="21f66-274">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="21f66-275">Hello Zscaler egy csempe a hozzáférési Panel hello kattintáskor automatikusan bejelentkezett tooyour Zscaler egy alkalmazás kapja meg.</span><span class="sxs-lookup"><span data-stu-id="21f66-275">When you click hello Zscaler One tile in hello Access Panel, you should get automatically signed-on tooyour Zscaler One application.</span></span>
<span data-ttu-id="21f66-276">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="21f66-276">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="21f66-277">További források</span><span class="sxs-lookup"><span data-stu-id="21f66-277">Additional resources</span></span>

* [<span data-ttu-id="21f66-278">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="21f66-278">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="21f66-279">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="21f66-279">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-zscaler-one-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-zscaler-one-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-zscaler-one-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-zscaler-one-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-zscaler-one-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-zscaler-one-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-zscaler-one-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-zscaler-one-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-zscaler-one-tutorial/tutorial_general_203.png

