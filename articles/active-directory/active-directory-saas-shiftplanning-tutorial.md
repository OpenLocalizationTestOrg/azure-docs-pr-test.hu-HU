---
title: "Oktatóanyag: Azure Active Directoryval integrált tárgyában |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és tárgyában között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 6aa771e9-31c6-48d1-8dde-024bebc06943
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/10/2017
ms.author: jeedes
ms.openlocfilehash: 7d8a04a2eb3c997f86f1e199c47809fa3dad60e8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-humanity"></a><span data-ttu-id="6bc51-103">Oktatóanyag: Azure Active Directoryval integrált tárgyában</span><span class="sxs-lookup"><span data-stu-id="6bc51-103">Tutorial: Azure Active Directory integration with Humanity</span></span>

<span data-ttu-id="6bc51-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate tárgyában az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="6bc51-104">In this tutorial, you learn how toointegrate Humanity with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="6bc51-105">Tárgyában integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="6bc51-105">Integrating Humanity with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="6bc51-106">Megadhatja a hozzáférés tooHumanity rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="6bc51-106">You can control in Azure AD who has access tooHumanity</span></span>
- <span data-ttu-id="6bc51-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooHumanity (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="6bc51-107">You can enable your users tooautomatically get signed-on tooHumanity (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="6bc51-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="6bc51-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="6bc51-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="6bc51-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6bc51-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="6bc51-110">Prerequisites</span></span>

<span data-ttu-id="6bc51-111">az Azure AD integrálása tárgyában tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="6bc51-111">tooconfigure Azure AD integration with Humanity, you need hello following items:</span></span>

- <span data-ttu-id="6bc51-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="6bc51-112">An Azure AD subscription</span></span>
- <span data-ttu-id="6bc51-113">Egy tárgyában egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="6bc51-113">A Humanity single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="6bc51-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="6bc51-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="6bc51-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="6bc51-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="6bc51-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="6bc51-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="6bc51-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="6bc51-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="6bc51-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="6bc51-118">Scenario description</span></span>
<span data-ttu-id="6bc51-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="6bc51-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="6bc51-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="6bc51-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="6bc51-121">Hello gyűjteményből tárgyában hozzáadása</span><span class="sxs-lookup"><span data-stu-id="6bc51-121">Adding Humanity from hello gallery</span></span>
2. <span data-ttu-id="6bc51-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="6bc51-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-humanity-from-hello-gallery"></a><span data-ttu-id="6bc51-123">Hello gyűjteményből tárgyában hozzáadása</span><span class="sxs-lookup"><span data-stu-id="6bc51-123">Adding Humanity from hello gallery</span></span>
<span data-ttu-id="6bc51-124">tooconfigure hello integrálása az Azure AD-be tárgyában, meg kell tooadd tárgyában hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="6bc51-124">tooconfigure hello integration of Humanity into Azure AD, you need tooadd Humanity from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="6bc51-125">**tooadd tárgyában hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="6bc51-125">**tooadd Humanity from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="6bc51-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="6bc51-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="6bc51-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="6bc51-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="6bc51-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="6bc51-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="6bc51-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="6bc51-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="6bc51-133">Hello keresési mezőbe, írja be a **tárgyában**.</span><span class="sxs-lookup"><span data-stu-id="6bc51-133">In hello search box, type **Humanity**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-shiftplanning-tutorial/tutorial_humanity_search.png)

5. <span data-ttu-id="6bc51-135">A hello eredmények panelen válassza ki a **tárgyában**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="6bc51-135">In hello results panel, select **Humanity**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-shiftplanning-tutorial/tutorial_humanity_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="6bc51-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="6bc51-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="6bc51-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján tárgyában a</span><span class="sxs-lookup"><span data-stu-id="6bc51-138">In this section, you configure and test Azure AD single sign-on with Humanity based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="6bc51-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó tárgyában tooa felhasználói az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="6bc51-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Humanity is tooa user in Azure AD.</span></span> <span data-ttu-id="6bc51-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello tárgyában közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="6bc51-140">In other words, a link relationship between an Azure AD user and hello related user in Humanity needs toobe established.</span></span>

<span data-ttu-id="6bc51-141">Tárgyában, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="6bc51-141">In Humanity, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="6bc51-142">tooconfigure és az Azure AD az egyszeri bejelentkezés tárgyában-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="6bc51-142">tooconfigure and test Azure AD single sign-on with Humanity, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="6bc51-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="6bc51-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="6bc51-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="6bc51-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="6bc51-145">**[Tárgyában tesztfelhasználó létrehozása](#creating-a-humanity-test-user)**  -toohave egy megfelelője a Britta Simon a tárgyában, amely a felhasználó csatolt toohello az Azure AD-ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="6bc51-145">**[Creating a Humanity test user](#creating-a-humanity-test-user)** - toohave a counterpart of Britta Simon in Humanity that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="6bc51-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="6bc51-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="6bc51-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="6bc51-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="6bc51-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="6bc51-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="6bc51-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az tárgyában alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="6bc51-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Humanity application.</span></span>

<span data-ttu-id="6bc51-150">**az Azure AD tooconfigure egyszeri bejelentkezést a tárgyában, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="6bc51-150">**tooconfigure Azure AD single sign-on with Humanity, perform hello following steps:**</span></span>

1. <span data-ttu-id="6bc51-151">Az Azure portál, a hello hello **tárgyában** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="6bc51-151">In hello Azure portal, on hello **Humanity** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="6bc51-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="6bc51-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-shiftplanning-tutorial/tutorial_humanity_samlbase.png)

3. <span data-ttu-id="6bc51-155">A hello **tárgyában tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="6bc51-155">On hello **Humanity Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-shiftplanning-tutorial/tutorial_humanity_url.png)

    <span data-ttu-id="6bc51-157">a.</span><span class="sxs-lookup"><span data-stu-id="6bc51-157">a.</span></span> <span data-ttu-id="6bc51-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://company.humanity.com/includes/saml/`</span><span class="sxs-lookup"><span data-stu-id="6bc51-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://company.humanity.com/includes/saml/`</span></span>

    <span data-ttu-id="6bc51-159">b.</span><span class="sxs-lookup"><span data-stu-id="6bc51-159">b.</span></span> <span data-ttu-id="6bc51-160">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://company.humanity.com/app/`</span><span class="sxs-lookup"><span data-stu-id="6bc51-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://company.humanity.com/app/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="6bc51-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="6bc51-161">These values are not real.</span></span> <span data-ttu-id="6bc51-162">Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="6bc51-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="6bc51-163">Ügyfél [tárgyában ügyfél-támogatási csoport](https://www.humanity.com/support/) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="6bc51-163">Contact [Humanity Client support team](https://www.humanity.com/support/) tooget these values.</span></span> 
 
4. <span data-ttu-id="6bc51-164">A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="6bc51-164">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-shiftplanning-tutorial/tutorial_humanity_certificate.png) 

5. <span data-ttu-id="6bc51-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="6bc51-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-shiftplanning-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="6bc51-168">A hello **tárgyában konfigurációs** kattintson **konfigurálása tárgyában** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="6bc51-168">On hello **Humanity Configuration** section, click **Configure Humanity** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="6bc51-169">Másolás hello **SAML-alapú egyszeri bejelentkezési URL-címe és Sign-Out URL-cím** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="6bc51-169">Copy hello **SAML Single Sign-On Service URL, and Sign-Out URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-shiftplanning-tutorial/tutorial_humanity_configure.png) 

7. <span data-ttu-id="6bc51-171">Egy másik webes böngészőablakban, jelentkezzen be tooyour **tárgyában** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="6bc51-171">In a different web browser window, log in tooyour **Humanity** company site as an administrator.</span></span>

8. <span data-ttu-id="6bc51-172">Hello hello felső menüben kattintson a **Admin**.</span><span class="sxs-lookup"><span data-stu-id="6bc51-172">In hello menu on hello top, click **Admin**.</span></span>
   
    <span data-ttu-id="6bc51-173">![Felügyeleti](./media/active-directory-saas-shiftplanning-tutorial/iC786619.png "rendszergazda")</span><span class="sxs-lookup"><span data-stu-id="6bc51-173">![Admin](./media/active-directory-saas-shiftplanning-tutorial/iC786619.png "Admin")</span></span>

9. <span data-ttu-id="6bc51-174">A **integrációs**, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="6bc51-174">Under **Integration**, click **Single Sign-On**.</span></span>
   
    <span data-ttu-id="6bc51-175">![Egyszeri bejelentkezés](./media/active-directory-saas-shiftplanning-tutorial/iC786620.png "egyszeri bejelentkezés")</span><span class="sxs-lookup"><span data-stu-id="6bc51-175">![Single Sign-On](./media/active-directory-saas-shiftplanning-tutorial/iC786620.png "Single Sign-On")</span></span>

10. <span data-ttu-id="6bc51-176">A hello **egyszeri bejelentkezés** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="6bc51-176">In hello **Single Sign-On** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="6bc51-177">![Egyszeri bejelentkezés](./media/active-directory-saas-shiftplanning-tutorial/iC786905.png "egyszeri bejelentkezés")</span><span class="sxs-lookup"><span data-stu-id="6bc51-177">![Single Sign-On](./media/active-directory-saas-shiftplanning-tutorial/iC786905.png "Single Sign-On")</span></span>
   
    <span data-ttu-id="6bc51-178">a.</span><span class="sxs-lookup"><span data-stu-id="6bc51-178">a.</span></span> <span data-ttu-id="6bc51-179">Válassza ki **SAML engedélyezett**.</span><span class="sxs-lookup"><span data-stu-id="6bc51-179">Select **SAML Enabled**.</span></span>

    <span data-ttu-id="6bc51-180">b.</span><span class="sxs-lookup"><span data-stu-id="6bc51-180">b.</span></span> <span data-ttu-id="6bc51-181">Válassza ki **engedélyezte a bejelentkezést jelszó**.</span><span class="sxs-lookup"><span data-stu-id="6bc51-181">Select **Allow Password Login**.</span></span>

    <span data-ttu-id="6bc51-182">c.</span><span class="sxs-lookup"><span data-stu-id="6bc51-182">c.</span></span> <span data-ttu-id="6bc51-183">Beillesztés hello **SAML-alapú egyszeri bejelentkezési URL-címe** hello érték **SAML kiállítójának URL-címe** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="6bc51-183">Paste hello **SAML Single Sign-On Service URL** value into hello **SAML Issuer URL** textbox.</span></span>

    <span data-ttu-id="6bc51-184">d.</span><span class="sxs-lookup"><span data-stu-id="6bc51-184">d.</span></span> <span data-ttu-id="6bc51-185">Beillesztés hello **Sign-Out URL-cím** hello érték **távoli kijelentkezési URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="6bc51-185">Paste hello **Sign-Out URL** value into hello **Remote Logout URL** textbox.</span></span>
   
    <span data-ttu-id="6bc51-186">e.</span><span class="sxs-lookup"><span data-stu-id="6bc51-186">e.</span></span> <span data-ttu-id="6bc51-187">Nyissa meg a base-64 kódolású tanúsítvány a Jegyzettömbben, a vágólapra tartalmának másolása hello és toohello Beillesztés **X.509 tanúsítvány** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="6bc51-187">Open your base-64 encoded certificate in notepad, copy hello content of it into your clipboard, and then paste it toohello **X.509 Certificate** textbox.</span></span>

11. <span data-ttu-id="6bc51-188">Kattintson a **beállítások mentése**.</span><span class="sxs-lookup"><span data-stu-id="6bc51-188">Click **Save Settings**.</span></span>

> [!TIP]
> <span data-ttu-id="6bc51-189">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="6bc51-189">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="6bc51-190">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="6bc51-190">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="6bc51-191">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="6bc51-191">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="6bc51-192">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="6bc51-192">Creating an Azure AD test user</span></span>
<span data-ttu-id="6bc51-193">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="6bc51-193">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="6bc51-195">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="6bc51-195">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="6bc51-196">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="6bc51-196">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-shiftplanning-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="6bc51-198">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="6bc51-198">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-shiftplanning-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="6bc51-200">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="6bc51-200">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-shiftplanning-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="6bc51-202">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="6bc51-202">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-shiftplanning-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="6bc51-204">a.</span><span class="sxs-lookup"><span data-stu-id="6bc51-204">a.</span></span> <span data-ttu-id="6bc51-205">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="6bc51-205">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="6bc51-206">b.</span><span class="sxs-lookup"><span data-stu-id="6bc51-206">b.</span></span> <span data-ttu-id="6bc51-207">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="6bc51-207">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="6bc51-208">c.</span><span class="sxs-lookup"><span data-stu-id="6bc51-208">c.</span></span> <span data-ttu-id="6bc51-209">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="6bc51-209">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="6bc51-210">d.</span><span class="sxs-lookup"><span data-stu-id="6bc51-210">d.</span></span> <span data-ttu-id="6bc51-211">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="6bc51-211">Click **Create**.</span></span>
 
### <a name="creating-a-humanity-test-user"></a><span data-ttu-id="6bc51-212">Tárgyában tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="6bc51-212">Creating a Humanity test user</span></span>

<span data-ttu-id="6bc51-213">A rendelés tooenable az Azure AD felhasználók toolog a tooHumanity akkor ki kell építenie tárgyában történő.</span><span class="sxs-lookup"><span data-stu-id="6bc51-213">In order tooenable Azure AD users toolog in tooHumanity, they must be provisioned into Humanity.</span></span> <span data-ttu-id="6bc51-214">Tárgyában hello esetben egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="6bc51-214">In hello case of Humanity, provisioning is a manual task.</span></span>

<span data-ttu-id="6bc51-215">**tooprovision egy felhasználói fiókot, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="6bc51-215">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="6bc51-216">Jelentkezzen be tooyour **tárgyában** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="6bc51-216">Log in tooyour **Humanity** company site as an administrator.</span></span>

2. <span data-ttu-id="6bc51-217">Kattintson a **Admin**.</span><span class="sxs-lookup"><span data-stu-id="6bc51-217">Click **Admin**.</span></span>
   
    <span data-ttu-id="6bc51-218">![Felügyeleti](./media/active-directory-saas-shiftplanning-tutorial/iC786619.png "rendszergazda")</span><span class="sxs-lookup"><span data-stu-id="6bc51-218">![Admin](./media/active-directory-saas-shiftplanning-tutorial/iC786619.png "Admin")</span></span>

3. <span data-ttu-id="6bc51-219">Kattintson a **személyzet**.</span><span class="sxs-lookup"><span data-stu-id="6bc51-219">Click **Staff**.</span></span>
   
    <span data-ttu-id="6bc51-220">![Személyzet](./media/active-directory-saas-shiftplanning-tutorial/ic786623.png "személyzet")</span><span class="sxs-lookup"><span data-stu-id="6bc51-220">![Staff](./media/active-directory-saas-shiftplanning-tutorial/ic786623.png "Staff")</span></span>

4. <span data-ttu-id="6bc51-221">A **műveletek**, kattintson a **adja hozzá az alkalmazottak**.</span><span class="sxs-lookup"><span data-stu-id="6bc51-221">Under **Actions**, click **Add Employees**.</span></span>
   
    <span data-ttu-id="6bc51-222">![Adja hozzá az alkalmazottak](./media/active-directory-saas-shiftplanning-tutorial/iC786624.png "alkalmazottak hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="6bc51-222">![Add Employees](./media/active-directory-saas-shiftplanning-tutorial/iC786624.png "Add Employees")</span></span>

5. <span data-ttu-id="6bc51-223">A hello **adja hozzá az alkalmazottak** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="6bc51-223">In hello **Add Employees** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="6bc51-224">![Az alkalmazottak mentése](./media/active-directory-saas-shiftplanning-tutorial/iC786625.png "alkalmazottak mentése")</span><span class="sxs-lookup"><span data-stu-id="6bc51-224">![Save Employees](./media/active-directory-saas-shiftplanning-tutorial/iC786625.png "Save Employees")</span></span>
   
    <span data-ttu-id="6bc51-225">a.</span><span class="sxs-lookup"><span data-stu-id="6bc51-225">a.</span></span> <span data-ttu-id="6bc51-226">Típus hello **Utónév**, **Vezetéknév**, és **E-mail** a egy érvényes AAD-fiókba, a kívánt tooprovision hello kapcsolódó szövegmezőből.</span><span class="sxs-lookup"><span data-stu-id="6bc51-226">Type hello **First Name**, **Last Name**, and **Email** of a valid AAD account you want tooprovision into hello related textboxes.</span></span>

    <span data-ttu-id="6bc51-227">b.</span><span class="sxs-lookup"><span data-stu-id="6bc51-227">b.</span></span> <span data-ttu-id="6bc51-228">Kattintson a **alkalmazottak mentése**.</span><span class="sxs-lookup"><span data-stu-id="6bc51-228">Click **Save Employees**.</span></span>

>[!NOTE]
><span data-ttu-id="6bc51-229">Bármely más tárgyában felhasználói fiók létrehozása eszközök vagy tárgyában tooprovision által nyújtott API-k AAD felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="6bc51-229">You can use any other Humanity user account creation tools or APIs provided by Humanity tooprovision AAD user accounts.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="6bc51-230">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="6bc51-230">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="6bc51-231">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooHumanity megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="6bc51-231">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooHumanity.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="6bc51-233">**tooassign Britta Simon tooHumanity, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="6bc51-233">**tooassign Britta Simon tooHumanity, perform hello following steps:**</span></span>

1. <span data-ttu-id="6bc51-234">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="6bc51-234">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="6bc51-236">Hello alkalmazások listában válassza ki a **tárgyában**.</span><span class="sxs-lookup"><span data-stu-id="6bc51-236">In hello applications list, select **Humanity**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-shiftplanning-tutorial/tutorial_humanity_app.png) 

3. <span data-ttu-id="6bc51-238">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="6bc51-238">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="6bc51-240">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="6bc51-240">Click **Add** button.</span></span> <span data-ttu-id="6bc51-241">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="6bc51-241">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="6bc51-243">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="6bc51-243">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="6bc51-244">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="6bc51-244">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="6bc51-245">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="6bc51-245">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="6bc51-246">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="6bc51-246">Testing single sign-on</span></span>

<span data-ttu-id="6bc51-247">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="6bc51-247">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="6bc51-248">Ha a hozzáférési Panel hello hello tárgyában csempe gombra kattint, automatikusan bejelentkezett tooyour tárgyában alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="6bc51-248">When you click hello Humanity tile in hello Access Panel, you should get automatically signed-on tooyour Humanity application.</span></span>
<span data-ttu-id="6bc51-249">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="6bc51-249">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6bc51-250">További források</span><span class="sxs-lookup"><span data-stu-id="6bc51-250">Additional resources</span></span>

* [<span data-ttu-id="6bc51-251">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="6bc51-251">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="6bc51-252">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="6bc51-252">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-shiftplanning-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-shiftplanning-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-shiftplanning-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-shiftplanning-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-shiftplanning-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-shiftplanning-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-shiftplanning-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-shiftplanning-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-shiftplanning-tutorial/tutorial_general_203.png

