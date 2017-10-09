---
title: "Oktatóanyag: Azure Active Directoryval integrált Aha! | Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Aha!."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: ad955d3d-896a-41bb-800d-68e8cb5ff48d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/10/2017
ms.author: jeedes
ms.openlocfilehash: d844db3c0a035560e6fb275017215171743fba56
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-aha"></a><span data-ttu-id="0d24d-104">Oktatóanyag: Azure Active Directoryval integrált Aha!</span><span class="sxs-lookup"><span data-stu-id="0d24d-104">Tutorial: Azure Active Directory integration with Aha!</span></span>

<span data-ttu-id="0d24d-105">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Aha!</span><span class="sxs-lookup"><span data-stu-id="0d24d-105">In this tutorial, you learn how toointegrate Aha!</span></span> <span data-ttu-id="0d24d-106">az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="0d24d-106">with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="0d24d-107">Integrálása Aha!</span><span class="sxs-lookup"><span data-stu-id="0d24d-107">Integrating Aha!</span></span> <span data-ttu-id="0d24d-108">az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="0d24d-108">with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="0d24d-109">Az Azure AD hozzáférési tooAha rendelkező szabályozhatja!</span><span class="sxs-lookup"><span data-stu-id="0d24d-109">You can control in Azure AD who has access tooAha!</span></span>
- <span data-ttu-id="0d24d-110">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooAha!</span><span class="sxs-lookup"><span data-stu-id="0d24d-110">You can enable your users tooautomatically get signed-on tooAha!</span></span> <span data-ttu-id="0d24d-111">(Egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="0d24d-111">(Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="0d24d-112">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="0d24d-112">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="0d24d-113">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="0d24d-113">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0d24d-114">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="0d24d-114">Prerequisites</span></span>

<span data-ttu-id="0d24d-115">az Azure AD integrálása Aha tooconfigure!, a következő elemek hello van szüksége:</span><span class="sxs-lookup"><span data-stu-id="0d24d-115">tooconfigure Azure AD integration with Aha!, you need hello following items:</span></span>

- <span data-ttu-id="0d24d-116">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="0d24d-116">An Azure AD subscription</span></span>
- <span data-ttu-id="0d24d-117">Egy Aha!</span><span class="sxs-lookup"><span data-stu-id="0d24d-117">An Aha!</span></span> <span data-ttu-id="0d24d-118">egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="0d24d-118">single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="0d24d-119">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="0d24d-119">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="0d24d-120">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="0d24d-120">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="0d24d-121">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="0d24d-121">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="0d24d-122">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="0d24d-122">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="0d24d-123">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="0d24d-123">Scenario description</span></span>
<span data-ttu-id="0d24d-124">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="0d24d-124">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="0d24d-125">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="0d24d-125">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="0d24d-126">Hozzáadás Aha!</span><span class="sxs-lookup"><span data-stu-id="0d24d-126">Adding Aha!</span></span> <span data-ttu-id="0d24d-127">hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="0d24d-127">from hello gallery</span></span>
2. <span data-ttu-id="0d24d-128">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="0d24d-128">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-aha-from-hello-gallery"></a><span data-ttu-id="0d24d-129">Hozzáadás Aha!</span><span class="sxs-lookup"><span data-stu-id="0d24d-129">Adding Aha!</span></span> <span data-ttu-id="0d24d-130">hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="0d24d-130">from hello gallery</span></span>
<span data-ttu-id="0d24d-131">hello integrációja tooconfigure Aha!</span><span class="sxs-lookup"><span data-stu-id="0d24d-131">tooconfigure hello integration of Aha!</span></span> <span data-ttu-id="0d24d-132">az Azure AD-be kell tooadd Aha!</span><span class="sxs-lookup"><span data-stu-id="0d24d-132">into Azure AD, you need tooadd Aha!</span></span> <span data-ttu-id="0d24d-133">hello gyűjtemény tooyour listája felügyelt SaaS-alkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="0d24d-133">from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="0d24d-134">**tooadd Aha! hello gyűjteményből hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="0d24d-134">**tooadd Aha! from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="0d24d-135">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="0d24d-135">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="0d24d-137">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="0d24d-137">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="0d24d-138">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="0d24d-138">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="0d24d-140">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="0d24d-140">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="0d24d-142">Hello keresési mezőbe, írja be a **Aha!**.</span><span class="sxs-lookup"><span data-stu-id="0d24d-142">In hello search box, type **Aha!**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-aha-tutorial/tutorial_aha_search.png)

5. <span data-ttu-id="0d24d-144">A hello eredmények panelen válassza ki a **Aha!**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="0d24d-144">In hello results panel, select **Aha!**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-aha-tutorial/tutorial_aha_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="0d24d-146">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="0d24d-146">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="0d24d-147">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést a Aha!</span><span class="sxs-lookup"><span data-stu-id="0d24d-147">In this section, you configure and test Azure AD single sign-on with Aha!</span></span> <span data-ttu-id="0d24d-148">a "Britta Simon." nevű tesztfelhasználó alapján</span><span class="sxs-lookup"><span data-stu-id="0d24d-148">based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="0d24d-149">Az egyszeri bejelentkezés toowork az Azure AD milyen hello megfelelőjére felhasználó Aha kell tooknow!</span><span class="sxs-lookup"><span data-stu-id="0d24d-149">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Aha!</span></span> <span data-ttu-id="0d24d-150">az tooa felhasználó, az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="0d24d-150">is tooa user in Azure AD.</span></span> <span data-ttu-id="0d24d-151">Más szóval egy hivatkozás közötti kapcsolat egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Aha!</span><span class="sxs-lookup"><span data-stu-id="0d24d-151">In other words, a link relationship between an Azure AD user and hello related user in Aha!</span></span> <span data-ttu-id="0d24d-152">létrehozott toobe kell.</span><span class="sxs-lookup"><span data-stu-id="0d24d-152">needs toobe established.</span></span>

<span data-ttu-id="0d24d-153">A Aha!, hello hello értéket **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="0d24d-153">In Aha!, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="0d24d-154">tooconfigure és tesztelése az Azure AD-egyszeri bejelentkezés az Aha!, a következő építőelemeket toocomplete hello van szüksége:</span><span class="sxs-lookup"><span data-stu-id="0d24d-154">tooconfigure and test Azure AD single sign-on with Aha!, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="0d24d-155">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="0d24d-155">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="0d24d-156">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="0d24d-156">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="0d24d-157">**[Létrehozása egy Aha! Teszt felhasználó](#creating-an-aha-test-user)**  -toohave egy megfelelője a Britta Simon a Aha!</span><span class="sxs-lookup"><span data-stu-id="0d24d-157">**[Creating an Aha! test user](#creating-an-aha-test-user)** - toohave a counterpart of Britta Simon in Aha!</span></span> <span data-ttu-id="0d24d-158">Ez a csatolt toohello felhasználói az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="0d24d-158">that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="0d24d-159">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="0d24d-159">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="0d24d-160">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="0d24d-160">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="0d24d-161">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="0d24d-161">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="0d24d-162">Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése és konfigurálása egyszeri bejelentkezéshez a Aha!</span><span class="sxs-lookup"><span data-stu-id="0d24d-162">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Aha!</span></span> <span data-ttu-id="0d24d-163">az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="0d24d-163">application.</span></span>

<span data-ttu-id="0d24d-164">**az Azure AD egyszeri bejelentkezést a Aha tooconfigure!, hajtsa végre az alábbi lépésekkel hello:**</span><span class="sxs-lookup"><span data-stu-id="0d24d-164">**tooconfigure Azure AD single sign-on with Aha!, perform hello following steps:**</span></span>

1. <span data-ttu-id="0d24d-165">Az Azure portál, a hello hello **Aha!**</span><span class="sxs-lookup"><span data-stu-id="0d24d-165">In hello Azure portal, on hello **Aha!**</span></span> <span data-ttu-id="0d24d-166">alkalmazás-integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="0d24d-166">application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="0d24d-168">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="0d24d-168">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-aha-tutorial/tutorial_aha_samlbase.png)

3. <span data-ttu-id="0d24d-170">A hello **Aha! Tartomány- és URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="0d24d-170">On hello **Aha! Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-aha-tutorial/tutorial_aha_url.png)

    <span data-ttu-id="0d24d-172">a.</span><span class="sxs-lookup"><span data-stu-id="0d24d-172">a.</span></span> <span data-ttu-id="0d24d-173">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<companyname>.aha.io/session/new`</span><span class="sxs-lookup"><span data-stu-id="0d24d-173">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.aha.io/session/new`</span></span>

    <span data-ttu-id="0d24d-174">b.</span><span class="sxs-lookup"><span data-stu-id="0d24d-174">b.</span></span> <span data-ttu-id="0d24d-175">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<companyname>.aha.io`</span><span class="sxs-lookup"><span data-stu-id="0d24d-175">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<companyname>.aha.io`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="0d24d-176">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="0d24d-176">These values are not real.</span></span> <span data-ttu-id="0d24d-177">Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="0d24d-177">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="0d24d-178">Ügyfél [Aha! Ügyfél-támogatási csoport](https://www.aha.io/company/contact) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="0d24d-178">Contact [Aha! Client support team](https://www.aha.io/company/contact) tooget these values.</span></span> 
 
4. <span data-ttu-id="0d24d-179">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="0d24d-179">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-aha-tutorial/tutorial_aha_certificate.png) 

5. <span data-ttu-id="0d24d-181">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="0d24d-181">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-aha-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="0d24d-183">Egy másik webes böngészőablakban bejelentkezési tooyour Aha!</span><span class="sxs-lookup"><span data-stu-id="0d24d-183">In a different web browser window, log in tooyour Aha!</span></span> <span data-ttu-id="0d24d-184">vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="0d24d-184">company site as an administrator.</span></span>

7. <span data-ttu-id="0d24d-185">Hello hello felső menüben kattintson a **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="0d24d-185">In hello menu on hello top, click **Settings**.</span></span>

    <span data-ttu-id="0d24d-186">![Beállítások](./media/active-directory-saas-aha-tutorial/IC798950.png "beállítások")</span><span class="sxs-lookup"><span data-stu-id="0d24d-186">![Settings](./media/active-directory-saas-aha-tutorial/IC798950.png "Settings")</span></span>

8. <span data-ttu-id="0d24d-187">Kattintson a **fiók**.</span><span class="sxs-lookup"><span data-stu-id="0d24d-187">Click **Account**.</span></span>
   
    <span data-ttu-id="0d24d-188">![Profil](./media/active-directory-saas-aha-tutorial/IC798951.png "profil")</span><span class="sxs-lookup"><span data-stu-id="0d24d-188">![Profile](./media/active-directory-saas-aha-tutorial/IC798951.png "Profile")</span></span>

9. <span data-ttu-id="0d24d-189">Kattintson a **biztonsági és egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="0d24d-189">Click **Security and single sign-on**.</span></span>
   
    <span data-ttu-id="0d24d-190">![Biztonság és az egyszeri bejelentkezés](./media/active-directory-saas-aha-tutorial/IC798952.png "biztonsági és egyszeri bejelentkezést.")</span><span class="sxs-lookup"><span data-stu-id="0d24d-190">![Security and single sign-on](./media/active-directory-saas-aha-tutorial/IC798952.png "Security and single sign-on")</span></span>

10. <span data-ttu-id="0d24d-191">A **egyszeri bejelentkezés** szakaszban, mint **identitásszolgáltató**, jelölje be **SAML2.0**.</span><span class="sxs-lookup"><span data-stu-id="0d24d-191">In **Single Sign-On** section, as **Identity Provider**, select **SAML2.0**.</span></span>
   
    <span data-ttu-id="0d24d-192">![Biztonság és az egyszeri bejelentkezés](./media/active-directory-saas-aha-tutorial/IC798953.png "biztonsági és egyszeri bejelentkezést.")</span><span class="sxs-lookup"><span data-stu-id="0d24d-192">![Security and single sign-on](./media/active-directory-saas-aha-tutorial/IC798953.png "Security and single sign-on")</span></span>

11. <span data-ttu-id="0d24d-193">A hello **egyszeri bejelentkezés** konfiguráció lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="0d24d-193">On hello **Single Sign-On** configuration page, perform hello following steps:</span></span>
    
    <span data-ttu-id="0d24d-194">![Egyszeri bejelentkezés](./media/active-directory-saas-aha-tutorial/IC798954.png "egyszeri bejelentkezés")</span><span class="sxs-lookup"><span data-stu-id="0d24d-194">![Single Sign-On](./media/active-directory-saas-aha-tutorial/IC798954.png "Single Sign-On")</span></span>
    
       <span data-ttu-id="0d24d-195">a.</span><span class="sxs-lookup"><span data-stu-id="0d24d-195">a.</span></span> <span data-ttu-id="0d24d-196">A hello **neve** szövegmező, adja meg a konfiguráció nevét.</span><span class="sxs-lookup"><span data-stu-id="0d24d-196">In hello **Name** textbox, type a name for your configuration.</span></span>

       <span data-ttu-id="0d24d-197">b.</span><span class="sxs-lookup"><span data-stu-id="0d24d-197">b.</span></span> <span data-ttu-id="0d24d-198">A **beállításához használatával**, jelölje be **metaadatfájl**.</span><span class="sxs-lookup"><span data-stu-id="0d24d-198">For **Configure using**, select **Metadata File**.</span></span>
   
       <span data-ttu-id="0d24d-199">c.</span><span class="sxs-lookup"><span data-stu-id="0d24d-199">c.</span></span> <span data-ttu-id="0d24d-200">tooupload a letöltött metaadatfájl, kattintson a **Tallózás**.</span><span class="sxs-lookup"><span data-stu-id="0d24d-200">tooupload your downloaded metadata file, click **Browse**.</span></span>
   
       <span data-ttu-id="0d24d-201">d.</span><span class="sxs-lookup"><span data-stu-id="0d24d-201">d.</span></span> <span data-ttu-id="0d24d-202">Kattintson a **frissítés**.</span><span class="sxs-lookup"><span data-stu-id="0d24d-202">Click **Update**.</span></span>

> [!TIP]
> <span data-ttu-id="0d24d-203">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="0d24d-203">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="0d24d-204">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="0d24d-204">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="0d24d-205">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="0d24d-205">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="0d24d-206">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="0d24d-206">Creating an Azure AD test user</span></span>
<span data-ttu-id="0d24d-207">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="0d24d-207">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="0d24d-209">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="0d24d-209">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="0d24d-210">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="0d24d-210">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-aha-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="0d24d-212">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="0d24d-212">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-aha-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="0d24d-214">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="0d24d-214">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-aha-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="0d24d-216">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="0d24d-216">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-aha-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="0d24d-218">a.</span><span class="sxs-lookup"><span data-stu-id="0d24d-218">a.</span></span> <span data-ttu-id="0d24d-219">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="0d24d-219">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="0d24d-220">b.</span><span class="sxs-lookup"><span data-stu-id="0d24d-220">b.</span></span> <span data-ttu-id="0d24d-221">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="0d24d-221">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="0d24d-222">c.</span><span class="sxs-lookup"><span data-stu-id="0d24d-222">c.</span></span> <span data-ttu-id="0d24d-223">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="0d24d-223">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="0d24d-224">d.</span><span class="sxs-lookup"><span data-stu-id="0d24d-224">d.</span></span> <span data-ttu-id="0d24d-225">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="0d24d-225">Click **Create**.</span></span>
 
### <a name="creating-an-aha-test-user"></a><span data-ttu-id="0d24d-226">Létrehozása egy Aha!</span><span class="sxs-lookup"><span data-stu-id="0d24d-226">Creating an Aha!</span></span> <span data-ttu-id="0d24d-227">Teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="0d24d-227">test user</span></span>

<span data-ttu-id="0d24d-228">a tooAha tooenable az Azure AD-felhasználók toolog!, azok ki kell építenie a Aha!.</span><span class="sxs-lookup"><span data-stu-id="0d24d-228">tooenable Azure AD users toolog in tooAha!, they must be provisioned into Aha!.</span></span>  

<span data-ttu-id="0d24d-229">Hello esetében Aha!, az automatizált feladat.</span><span class="sxs-lookup"><span data-stu-id="0d24d-229">In hello case of Aha!, provisioning is an automated task.</span></span> <span data-ttu-id="0d24d-230">Nincs művelet elem meg.</span><span class="sxs-lookup"><span data-stu-id="0d24d-230">There is no action item for you.</span></span>

<span data-ttu-id="0d24d-231">Felhasználók automatikusan létrejönnek szükség hello első egyszeri bejelentkezési kísérlet során.</span><span class="sxs-lookup"><span data-stu-id="0d24d-231">Users are automatically created if necessary during hello first single sign-on attempt.</span></span>

>[!NOTE]
><span data-ttu-id="0d24d-232">Bármely más Aha is használhatja!</span><span class="sxs-lookup"><span data-stu-id="0d24d-232">You can use any other Aha!</span></span> <span data-ttu-id="0d24d-233">felhasználói fiók létrehozása eszközök vagy Aha által nyújtott API-kat!</span><span class="sxs-lookup"><span data-stu-id="0d24d-233">user account creation tools or APIs provided by Aha!</span></span> <span data-ttu-id="0d24d-234">tooprovision AAD felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="0d24d-234">tooprovision AAD user accounts.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="0d24d-235">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="0d24d-235">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="0d24d-236">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooAha megadásával engedélyeznie!.</span><span class="sxs-lookup"><span data-stu-id="0d24d-236">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooAha!.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="0d24d-238">**tooassign Britta Simon tooAha!, hajtsa végre az alábbi lépésekkel hello:**</span><span class="sxs-lookup"><span data-stu-id="0d24d-238">**tooassign Britta Simon tooAha!, perform hello following steps:**</span></span>

1. <span data-ttu-id="0d24d-239">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="0d24d-239">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="0d24d-241">Hello alkalmazások listában válassza ki a **Aha!**.</span><span class="sxs-lookup"><span data-stu-id="0d24d-241">In hello applications list, select **Aha!**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-aha-tutorial/tutorial_aha_app.png) 

3. <span data-ttu-id="0d24d-243">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="0d24d-243">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="0d24d-245">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="0d24d-245">Click **Add** button.</span></span> <span data-ttu-id="0d24d-246">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="0d24d-246">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="0d24d-248">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="0d24d-248">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="0d24d-249">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="0d24d-249">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="0d24d-250">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="0d24d-250">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="0d24d-251">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="0d24d-251">Testing single sign-on</span></span>

<span data-ttu-id="0d24d-252">Ha tootest az egyszeri bejelentkezés a beállításokat, nyissa meg a hozzáférési Panel hello.</span><span class="sxs-lookup"><span data-stu-id="0d24d-252">If you want tootest your single sign-on settings, open hello Access Panel.</span></span> <span data-ttu-id="0d24d-253">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="0d24d-253">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0d24d-254">További források</span><span class="sxs-lookup"><span data-stu-id="0d24d-254">Additional resources</span></span>

* [<span data-ttu-id="0d24d-255">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="0d24d-255">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="0d24d-256">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="0d24d-256">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-aha-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-aha-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-aha-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-aha-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-aha-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-aha-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-aha-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-aha-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-aha-tutorial/tutorial_general_203.png

