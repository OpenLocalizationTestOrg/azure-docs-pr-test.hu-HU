---
title: "Oktatóanyag: Azure Active Directoryval integrált Sprinklr |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Sprinklr között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: b33938a1-25a5-484c-8e75-7dc6de2d534d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/10/2017
ms.author: jeedes
ms.openlocfilehash: 14b467c72d4a453ed7ad248eadcdade710f105af
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sprinklr"></a><span data-ttu-id="63ace-103">Oktatóanyag: Azure Active Directoryval integrált Sprinklr</span><span class="sxs-lookup"><span data-stu-id="63ace-103">Tutorial: Azure Active Directory integration with Sprinklr</span></span>

<span data-ttu-id="63ace-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Sprinklr az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="63ace-104">In this tutorial, you learn how toointegrate Sprinklr with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="63ace-105">Sprinklr integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="63ace-105">Integrating Sprinklr with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="63ace-106">Megadhatja a hozzáférés tooSprinklr rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="63ace-106">You can control in Azure AD who has access tooSprinklr</span></span>
- <span data-ttu-id="63ace-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooSprinklr (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="63ace-107">You can enable your users tooautomatically get signed-on tooSprinklr (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="63ace-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="63ace-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="63ace-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="63ace-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="63ace-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="63ace-110">Prerequisites</span></span>

<span data-ttu-id="63ace-111">az Azure AD integrálása Sprinklr tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="63ace-111">tooconfigure Azure AD integration with Sprinklr, you need hello following items:</span></span>

- <span data-ttu-id="63ace-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="63ace-112">An Azure AD subscription</span></span>
- <span data-ttu-id="63ace-113">Egy Sprinklr egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="63ace-113">A Sprinklr single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="63ace-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="63ace-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="63ace-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="63ace-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="63ace-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="63ace-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="63ace-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="63ace-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="63ace-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="63ace-118">Scenario description</span></span>
<span data-ttu-id="63ace-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="63ace-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="63ace-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="63ace-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="63ace-121">Hello gyűjteményből Sprinklr hozzáadása</span><span class="sxs-lookup"><span data-stu-id="63ace-121">Adding Sprinklr from hello gallery</span></span>
2. <span data-ttu-id="63ace-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="63ace-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-sprinklr-from-hello-gallery"></a><span data-ttu-id="63ace-123">Hello gyűjteményből Sprinklr hozzáadása</span><span class="sxs-lookup"><span data-stu-id="63ace-123">Adding Sprinklr from hello gallery</span></span>
<span data-ttu-id="63ace-124">tooconfigure hello integrációja Sprinklr az Azure AD-be, meg kell tooadd Sprinklr hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="63ace-124">tooconfigure hello integration of Sprinklr into Azure AD, you need tooadd Sprinklr from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="63ace-125">**tooadd Sprinklr hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="63ace-125">**tooadd Sprinklr from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="63ace-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="63ace-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="63ace-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="63ace-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="63ace-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="63ace-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="63ace-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="63ace-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="63ace-133">Hello keresési mezőbe, írja be a **Sprinklr**.</span><span class="sxs-lookup"><span data-stu-id="63ace-133">In hello search box, type **Sprinklr**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sprinklr-tutorial/tutorial_sprinklr_search.png)

5. <span data-ttu-id="63ace-135">A hello eredmények panelen válassza ki a **Sprinklr**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="63ace-135">In hello results panel, select **Sprinklr**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sprinklr-tutorial/tutorial_sprinklr_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="63ace-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="63ace-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="63ace-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján Sprinklr</span><span class="sxs-lookup"><span data-stu-id="63ace-138">In this section, you configure and test Azure AD single sign-on with Sprinklr based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="63ace-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Sprinklr tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="63ace-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Sprinklr is tooa user in Azure AD.</span></span> <span data-ttu-id="63ace-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Sprinklr közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="63ace-140">In other words, a link relationship between an Azure AD user and hello related user in Sprinklr needs toobe established.</span></span>

<span data-ttu-id="63ace-141">Sprinklr, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="63ace-141">In Sprinklr, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="63ace-142">tooconfigure és az Azure AD az egyszeri bejelentkezés Sprinklr-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="63ace-142">tooconfigure and test Azure AD single sign-on with Sprinklr, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="63ace-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="63ace-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="63ace-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="63ace-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="63ace-145">**[Sprinklr tesztfelhasználó létrehozása](#creating-a-sprinklr-test-user)**  -toohave egy megfelelője a Britta Simon a Sprinklr, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="63ace-145">**[Creating a Sprinklr test user](#creating-a-sprinklr-test-user)** - toohave a counterpart of Britta Simon in Sprinklr that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="63ace-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="63ace-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="63ace-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="63ace-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="63ace-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="63ace-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="63ace-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Sprinklr alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="63ace-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Sprinklr application.</span></span>

<span data-ttu-id="63ace-150">**az Azure AD tooconfigure egyszeri bejelentkezést a Sprinklr, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="63ace-150">**tooconfigure Azure AD single sign-on with Sprinklr, perform hello following steps:**</span></span>

1. <span data-ttu-id="63ace-151">Az Azure portál, a hello hello **Sprinklr** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="63ace-151">In hello Azure portal, on hello **Sprinklr** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="63ace-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="63ace-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sprinklr-tutorial/tutorial_sprinklr_samlbase.png)

3. <span data-ttu-id="63ace-155">A hello **Sprinklr tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="63ace-155">On hello **Sprinklr Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sprinklr-tutorial/tutorial_sprinklr_url.png)

    <span data-ttu-id="63ace-157">a.</span><span class="sxs-lookup"><span data-stu-id="63ace-157">a.</span></span> <span data-ttu-id="63ace-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<subdomain>.sprinklr.com`</span><span class="sxs-lookup"><span data-stu-id="63ace-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<subdomain>.sprinklr.com`</span></span>

    <span data-ttu-id="63ace-159">b.</span><span class="sxs-lookup"><span data-stu-id="63ace-159">b.</span></span> <span data-ttu-id="63ace-160">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<subdomain>.sprinklr.com`</span><span class="sxs-lookup"><span data-stu-id="63ace-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<subdomain>.sprinklr.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="63ace-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="63ace-161">These values are not real.</span></span> <span data-ttu-id="63ace-162">Hello érték frissíteni hello tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="63ace-162">Update hello value with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="63ace-163">Ügyfél [Sprinklr ügyfél-támogatási csoport](https://www.sprinklr.com/contact-us/) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="63ace-163">Contact [Sprinklr Client support team](https://www.sprinklr.com/contact-us/) tooget these values.</span></span> 
 
4. <span data-ttu-id="63ace-164">A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="63ace-164">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sprinklr-tutorial/tutorial_sprinklr_certificate.png) 

5. <span data-ttu-id="63ace-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="63ace-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sprinklr-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="63ace-168">A hello **Sprinklr konfigurációs** kattintson **konfigurálása Sprinklr** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="63ace-168">On hello **Sprinklr Configuration** section, click **Configure Sprinklr** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="63ace-169">Másolás hello **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="63ace-169">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

7. <span data-ttu-id="63ace-170">Egy másik webes böngészőablakban jelentkezzen tooyour Sprinklr vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="63ace-170">In a different web browser window, log in tooyour Sprinklr company site as an administrator.</span></span>

8. <span data-ttu-id="63ace-171">Nyissa meg túl**felügyeleti \> beállítások**.</span><span class="sxs-lookup"><span data-stu-id="63ace-171">Go too**Administration \> Settings**.</span></span>
   
    <span data-ttu-id="63ace-172">![Felügyeleti](./media/active-directory-saas-sprinklr-tutorial/ic782907.png "felügyeleti")</span><span class="sxs-lookup"><span data-stu-id="63ace-172">![Administration](./media/active-directory-saas-sprinklr-tutorial/ic782907.png "Administration")</span></span>

9. <span data-ttu-id="63ace-173">Nyissa meg túl**kezelése Partner \> egyszeri bejelentkezési** a hello bal oldali ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="63ace-173">Go too**Manage Partner \> Single Sign** on from hello left pane.</span></span>
   
    <span data-ttu-id="63ace-174">![Partner kezelése](./media/active-directory-saas-sprinklr-tutorial/ic782908.png "Partner kezelése")</span><span class="sxs-lookup"><span data-stu-id="63ace-174">![Manage Partner](./media/active-directory-saas-sprinklr-tutorial/ic782908.png "Manage Partner")</span></span>

10. <span data-ttu-id="63ace-175">Kattintson a **+ Hozzáadás egyszeri bejelentkezéssel**.</span><span class="sxs-lookup"><span data-stu-id="63ace-175">Click **+Add Single Sign Ons**.</span></span>
   
    <span data-ttu-id="63ace-176">![Az egyszeri bejelentkezés le](./media/active-directory-saas-sprinklr-tutorial/ic782909.png "az egyszeri bejelentkezés le")</span><span class="sxs-lookup"><span data-stu-id="63ace-176">![Single Sign-Ons](./media/active-directory-saas-sprinklr-tutorial/ic782909.png "Single Sign-Ons")</span></span>

11. <span data-ttu-id="63ace-177">A hello **az egyszeri bejelentkezés** lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="63ace-177">On hello **Single Sign on** page, perform hello following steps:</span></span>
   
    <span data-ttu-id="63ace-178">![Az egyszeri bejelentkezés le](./media/active-directory-saas-sprinklr-tutorial/ic782910.png "az egyszeri bejelentkezés le")</span><span class="sxs-lookup"><span data-stu-id="63ace-178">![Single Sign-Ons](./media/active-directory-saas-sprinklr-tutorial/ic782910.png "Single Sign-Ons")</span></span>

    <span data-ttu-id="63ace-179">a.</span><span class="sxs-lookup"><span data-stu-id="63ace-179">a.</span></span> <span data-ttu-id="63ace-180">A hello **neve** szövegmező, adja meg a konfiguráció nevét (például: *WAADSSOTest*).</span><span class="sxs-lookup"><span data-stu-id="63ace-180">In hello **Name** textbox, type a name for your configuration (for example: *WAADSSOTest*).</span></span>

    <span data-ttu-id="63ace-181">b.</span><span class="sxs-lookup"><span data-stu-id="63ace-181">b.</span></span> <span data-ttu-id="63ace-182">Válassza ki **engedélyezett**.</span><span class="sxs-lookup"><span data-stu-id="63ace-182">Select **Enabled**.</span></span>

    <span data-ttu-id="63ace-183">c.</span><span class="sxs-lookup"><span data-stu-id="63ace-183">c.</span></span> <span data-ttu-id="63ace-184">Válassza ki **új SSO-tanúsítvány használatára**.</span><span class="sxs-lookup"><span data-stu-id="63ace-184">Select **Use new SSO Certificate**.</span></span>
             
    <span data-ttu-id="63ace-185">e.</span><span class="sxs-lookup"><span data-stu-id="63ace-185">e.</span></span> <span data-ttu-id="63ace-186">Nyissa meg a base-64 kódolású tanúsítvány a Jegyzettömbben, a vágólapra tartalmának másolása hello és toohello Beillesztés **szolgáltató Identitástanúsítvány** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="63ace-186">Open your base-64 encoded certificate in notepad, copy hello content of it into your clipboard, and then paste it toohello **Identity Provider Certificate** textbox.</span></span>

    <span data-ttu-id="63ace-187">f.</span><span class="sxs-lookup"><span data-stu-id="63ace-187">f.</span></span> <span data-ttu-id="63ace-188">Beillesztés hello **SAML Entitásazonosító** érték, amely az Azure portálról történő hello másolta **entitásazonosító** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="63ace-188">Paste hello **SAML Entity ID** value which you have copied from Azure Portal into hello **Entity Id** textbox.</span></span>

    <span data-ttu-id="63ace-189">g.</span><span class="sxs-lookup"><span data-stu-id="63ace-189">g.</span></span> <span data-ttu-id="63ace-190">Beillesztés hello **SAML-alapú egyszeri bejelentkezési URL-címe** érték, amely az Azure portálról történő hello másolta **Identity Provider bejelentkezési URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="63ace-190">Paste hello **SAML Single Sign-On Service URL** value which you have copied from Azure Portal into hello **Identity Provider Login URL** textbox.</span></span>

    <span data-ttu-id="63ace-191">h.</span><span class="sxs-lookup"><span data-stu-id="63ace-191">h.</span></span> <span data-ttu-id="63ace-192">Beillesztés hello **Sign-Out URL-cím** érték, amely az Azure portálról történő hello másolta **Identity Provider kijelentkezési URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="63ace-192">Paste hello **Sign-Out URL** value which you have copied from Azure Portal into hello **Identity Provider Logout URL** textbox.</span></span>
     
    <span data-ttu-id="63ace-193">i.</span><span class="sxs-lookup"><span data-stu-id="63ace-193">i.</span></span> <span data-ttu-id="63ace-194">Mint **SAML felhasználói azonosító típusa**, jelölje be **helyességi feltételt tartalmaz felhasználói "s sprinklr.com felhasználónév**.</span><span class="sxs-lookup"><span data-stu-id="63ace-194">As **SAML User ID Type**, select **Assertion contains User”s sprinklr.com username**.</span></span>

    <span data-ttu-id="63ace-195">j.</span><span class="sxs-lookup"><span data-stu-id="63ace-195">j.</span></span> <span data-ttu-id="63ace-196">Mint **SAML felhasználói azonosító hely**, jelölje be **hello névazonosítója elemében hello tulajdonos utasítás alkalmazás felhasználói Azonosítóját**.</span><span class="sxs-lookup"><span data-stu-id="63ace-196">As **SAML User ID Location**, select **User ID is in hello Name Identifier element of hello Subject statement**.</span></span>

    <span data-ttu-id="63ace-197">k.</span><span class="sxs-lookup"><span data-stu-id="63ace-197">k.</span></span> <span data-ttu-id="63ace-198">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="63ace-198">Click **Save**.</span></span>
       
    <span data-ttu-id="63ace-199">![SAML](./media/active-directory-saas-sprinklr-tutorial/ic782911.png "SAML")</span><span class="sxs-lookup"><span data-stu-id="63ace-199">![SAML](./media/active-directory-saas-sprinklr-tutorial/ic782911.png "SAML")</span></span>

> [!TIP]
> <span data-ttu-id="63ace-200">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="63ace-200">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="63ace-201">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="63ace-201">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="63ace-202">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="63ace-202">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="63ace-203">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="63ace-203">Creating an Azure AD test user</span></span>
<span data-ttu-id="63ace-204">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="63ace-204">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="63ace-206">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="63ace-206">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="63ace-207">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="63ace-207">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sprinklr-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="63ace-209">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="63ace-209">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sprinklr-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="63ace-211">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="63ace-211">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sprinklr-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="63ace-213">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="63ace-213">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sprinklr-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="63ace-215">a.</span><span class="sxs-lookup"><span data-stu-id="63ace-215">a.</span></span> <span data-ttu-id="63ace-216">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="63ace-216">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="63ace-217">b.</span><span class="sxs-lookup"><span data-stu-id="63ace-217">b.</span></span> <span data-ttu-id="63ace-218">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="63ace-218">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="63ace-219">c.</span><span class="sxs-lookup"><span data-stu-id="63ace-219">c.</span></span> <span data-ttu-id="63ace-220">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="63ace-220">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="63ace-221">d.</span><span class="sxs-lookup"><span data-stu-id="63ace-221">d.</span></span> <span data-ttu-id="63ace-222">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="63ace-222">Click **Create**.</span></span>
 
### <a name="creating-a-sprinklr-test-user"></a><span data-ttu-id="63ace-223">Sprinklr tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="63ace-223">Creating a Sprinklr test user</span></span>

1. <span data-ttu-id="63ace-224">Jelentkezzen be tooyour Sprinklr vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="63ace-224">Log in tooyour Sprinklr company site as an administrator.</span></span>

2. <span data-ttu-id="63ace-225">Nyissa meg túl**felügyeleti \> beállítások**.</span><span class="sxs-lookup"><span data-stu-id="63ace-225">Go too**Administration \> Settings**.</span></span>
   
    <span data-ttu-id="63ace-226">![Felügyeleti](./media/active-directory-saas-sprinklr-tutorial/ic782907.png "felügyeleti")</span><span class="sxs-lookup"><span data-stu-id="63ace-226">![Administration](./media/active-directory-saas-sprinklr-tutorial/ic782907.png "Administration")</span></span>

3. <span data-ttu-id="63ace-227">Nyissa meg túl**kezelése ügyfél \> felhasználók** hello bal oldali ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="63ace-227">Go too**Manage Client \> Users** from hello left pane.</span></span>
   
    <span data-ttu-id="63ace-228">![Beállítások](./media/active-directory-saas-sprinklr-tutorial/ic782914.png "beállítások")</span><span class="sxs-lookup"><span data-stu-id="63ace-228">![Settings](./media/active-directory-saas-sprinklr-tutorial/ic782914.png "Settings")</span></span>

4. <span data-ttu-id="63ace-229">Kattintson a **felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="63ace-229">Click **Add User**.</span></span>
   
    <span data-ttu-id="63ace-230">![Beállítások](./media/active-directory-saas-sprinklr-tutorial/ic782915.png "beállítások")</span><span class="sxs-lookup"><span data-stu-id="63ace-230">![Settings](./media/active-directory-saas-sprinklr-tutorial/ic782915.png "Settings")</span></span>

5. <span data-ttu-id="63ace-231">A hello **Szerkesztés felhasználói** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="63ace-231">On hello **Edit user** dialog, perform hello following steps:</span></span>
   
    <span data-ttu-id="63ace-232">![Felhasználó szerkesztése](./media/active-directory-saas-sprinklr-tutorial/ic782916.png "felhasználó szerkesztése")</span><span class="sxs-lookup"><span data-stu-id="63ace-232">![Edit user](./media/active-directory-saas-sprinklr-tutorial/ic782916.png "Edit user")</span></span> 

    <span data-ttu-id="63ace-233">a.</span><span class="sxs-lookup"><span data-stu-id="63ace-233">a.</span></span> <span data-ttu-id="63ace-234">A hello **E-mail**, **Utónév** és **Vezetéknév** szövegmezőből, hello típusinformációt tooprovision kívánt Azure AD felhasználói fiók.</span><span class="sxs-lookup"><span data-stu-id="63ace-234">In hello **Email**, **First Name** and **Last Name** textboxes, type hello information of an Azure AD user account you want tooprovision.</span></span>

    <span data-ttu-id="63ace-235">b.</span><span class="sxs-lookup"><span data-stu-id="63ace-235">b.</span></span> <span data-ttu-id="63ace-236">Válassza ki **jelszó tiltva**.</span><span class="sxs-lookup"><span data-stu-id="63ace-236">Select **Password Disabled**.</span></span>

    <span data-ttu-id="63ace-237">c.</span><span class="sxs-lookup"><span data-stu-id="63ace-237">c.</span></span> <span data-ttu-id="63ace-238">Válassza ki **nyelvi**.</span><span class="sxs-lookup"><span data-stu-id="63ace-238">Select **Language**.</span></span>

    <span data-ttu-id="63ace-239">d.</span><span class="sxs-lookup"><span data-stu-id="63ace-239">d.</span></span> <span data-ttu-id="63ace-240">Válassza ki **felhasználótípust**.</span><span class="sxs-lookup"><span data-stu-id="63ace-240">Select **User Type**.</span></span>

    <span data-ttu-id="63ace-241">e.</span><span class="sxs-lookup"><span data-stu-id="63ace-241">e.</span></span> <span data-ttu-id="63ace-242">Kattintson a **frissítés**.</span><span class="sxs-lookup"><span data-stu-id="63ace-242">Click **Update**.</span></span>
   
     >[!IMPORTANT]
     ><span data-ttu-id="63ace-243">**Jelszó tiltva** kell kijelölt tooenable egy felhasználó toolog az identitásszolgáltató keresztül.</span><span class="sxs-lookup"><span data-stu-id="63ace-243">**Password Disabled** must be selected tooenable a user toolog in via an Identity provider.</span></span> 
     
6. <span data-ttu-id="63ace-244">Nyissa meg túl**szerepkör**, és hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="63ace-244">Go too**Role**, and then perform hello following steps:</span></span>
   
    <span data-ttu-id="63ace-245">![Szerepkörök partneri](./media/active-directory-saas-sprinklr-tutorial/ic782917.png "partneri szerepkörök")</span><span class="sxs-lookup"><span data-stu-id="63ace-245">![Partner Roles](./media/active-directory-saas-sprinklr-tutorial/ic782917.png "Partner Roles")</span></span>

    <span data-ttu-id="63ace-246">a.</span><span class="sxs-lookup"><span data-stu-id="63ace-246">a.</span></span> <span data-ttu-id="63ace-247">A hello **globális** listáról válassza ki **összes\_engedélyek**.</span><span class="sxs-lookup"><span data-stu-id="63ace-247">From hello **Global** list, select **ALL\_Permissions**.</span></span>  

    <span data-ttu-id="63ace-248">b.</span><span class="sxs-lookup"><span data-stu-id="63ace-248">b.</span></span> <span data-ttu-id="63ace-249">Kattintson a **frissítés**.</span><span class="sxs-lookup"><span data-stu-id="63ace-249">Click **Update**.</span></span>

>[!NOTE]
><span data-ttu-id="63ace-250">Bármely más Sprinklr felhasználói fiók létrehozása eszközök vagy Sprinklr tooprovision által nyújtott API-kat az Azure AD felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="63ace-250">You can use any other Sprinklr user account creation tools or APIs provided by Sprinklr tooprovision Azure AD user accounts.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="63ace-251">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="63ace-251">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="63ace-252">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooSprinklr megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="63ace-252">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooSprinklr.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="63ace-254">**tooassign Britta Simon tooSprinklr, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="63ace-254">**tooassign Britta Simon tooSprinklr, perform hello following steps:**</span></span>

1. <span data-ttu-id="63ace-255">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="63ace-255">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="63ace-257">Hello alkalmazások listában válassza ki a **Sprinklr**.</span><span class="sxs-lookup"><span data-stu-id="63ace-257">In hello applications list, select **Sprinklr**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sprinklr-tutorial/tutorial_sprinklr_app.png) 

3. <span data-ttu-id="63ace-259">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="63ace-259">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="63ace-261">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="63ace-261">Click **Add** button.</span></span> <span data-ttu-id="63ace-262">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="63ace-262">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="63ace-264">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="63ace-264">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="63ace-265">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="63ace-265">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="63ace-266">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="63ace-266">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="63ace-267">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="63ace-267">Testing single sign-on</span></span>

<span data-ttu-id="63ace-268">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="63ace-268">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="63ace-269">Ha a hozzáférési Panel hello hello Sprinklr csempe gombra kattint, kell automatikusan bejelentkezett tooyour Sprinklr alkalmazás hello hozzáférési Panel további információt, lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="63ace-269">When you click hello Sprinklr tile in hello Access Panel, you should get automatically signed-on tooyour Sprinklr application For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="63ace-270">További források</span><span class="sxs-lookup"><span data-stu-id="63ace-270">Additional resources</span></span>

* [<span data-ttu-id="63ace-271">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="63ace-271">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="63ace-272">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="63ace-272">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_203.png

