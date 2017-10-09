---
title: "Oktatóanyag: Azure Active Directory integrálása vxMaintain |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és vxMaintain között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 841a1066-593c-4603-9abe-f48496d73d10
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.openlocfilehash: 937ea276d898986fc5a953c96fddabdc8940309f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-integrate-azure-active-directory-with-vxmaintain"></a><span data-ttu-id="40633-103">Oktatóanyag: Azure Active Directory integrálása vxMaintain</span><span class="sxs-lookup"><span data-stu-id="40633-103">Tutorial: Integrate Azure Active Directory with vxMaintain</span></span>

<span data-ttu-id="40633-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate vxMaintain az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="40633-104">In this tutorial, you learn how toointegrate vxMaintain with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="40633-105">Ez az integráció kínál számos fontos előnnyel jár.</span><span class="sxs-lookup"><span data-stu-id="40633-105">This integration provides several important benefits.</span></span> <span data-ttu-id="40633-106">A következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="40633-106">You can:</span></span>

- <span data-ttu-id="40633-107">Az Azure ad-ben rendelkező vezérlő toovxMaintain érhető el.</span><span class="sxs-lookup"><span data-stu-id="40633-107">Control in Azure AD who has access toovxMaintain.</span></span>
- <span data-ttu-id="40633-108">A felhasználók tooautomatically bejelentkezés toovxMaintain az egyszeri bejelentkezés (SSO) engedélyezése az Azure AD-fiókok használatával.</span><span class="sxs-lookup"><span data-stu-id="40633-108">Enable your users tooautomatically sign in toovxMaintain with single sign-on (SSO) by using their Azure AD accounts.</span></span>
- <span data-ttu-id="40633-109">A fiók egyetlen központi helyen kezelheti: hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="40633-109">Manage your accounts in one central location: hello Azure portal.</span></span>

<span data-ttu-id="40633-110">toolearn SaaS alkalmazásintegráció az Azure AD-vel kapcsolatos további információkért lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval?](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="40633-110">toolearn more about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="40633-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="40633-111">Prerequisites</span></span>

<span data-ttu-id="40633-112">az Azure AD integrálása vxMaintain tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="40633-112">tooconfigure Azure AD integration with vxMaintain, you need hello following items:</span></span>

- <span data-ttu-id="40633-113">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="40633-113">An Azure AD subscription</span></span>
- <span data-ttu-id="40633-114">Egy vxMaintain előfizetés SSO engedélyezése</span><span class="sxs-lookup"><span data-stu-id="40633-114">A vxMaintain SSO-enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="40633-115">Ebben az oktatóanyagban hello lépéseket tesztelésekor, azt javasoljuk, hogy nem használ egy éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="40633-115">When you test hello steps in this tutorial, we recommend that you do not use a production environment.</span></span>

<span data-ttu-id="40633-116">Ebben az oktatóanyagban tootest hello lépéseit kövesse az alábbi ajánlásokat:</span><span class="sxs-lookup"><span data-stu-id="40633-116">tootest hello steps in this tutorial, follow these recommendations:</span></span>

- <span data-ttu-id="40633-117">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="40633-117">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="40633-118">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="40633-118">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="40633-119">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="40633-119">Scenario description</span></span>
<span data-ttu-id="40633-120">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="40633-120">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> 

<span data-ttu-id="40633-121">hello forgatókönyv, hogy ez az oktatóanyag bemutatja a két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="40633-121">hello scenario that this tutorial outlines consists of two main building blocks:</span></span>

* <span data-ttu-id="40633-122">Hello gyűjteményből vxMaintain hozzáadása</span><span class="sxs-lookup"><span data-stu-id="40633-122">Adding vxMaintain from hello gallery</span></span>
* <span data-ttu-id="40633-123">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="40633-123">Configuring and testing Azure AD single sign-on</span></span>

## <a name="add-vxmaintain-from-hello-gallery"></a><span data-ttu-id="40633-124">Adja hozzá a vxMaintain hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="40633-124">Add vxMaintain from hello gallery</span></span>
<span data-ttu-id="40633-125">az Azure AD-val vxMaintain tooconfigure hello integrálását, meg kell tooadd vxMaintain hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="40633-125">tooconfigure hello integration of vxMaintain with Azure AD, you need tooadd vxMaintain from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="40633-126">hello gyűjteményből tooadd vxMaintain hello a következő:</span><span class="sxs-lookup"><span data-stu-id="40633-126">tooadd vxMaintain from hello gallery, do hello following:</span></span>

1. <span data-ttu-id="40633-127">A hello [Azure-portálon](https://portal.azure.com), a bal oldali ablaktáblán hello, válassza ki a hello **Azure Active Directory** gombra.</span><span class="sxs-lookup"><span data-stu-id="40633-127">In hello [Azure portal](https://portal.azure.com), in hello left pane, select hello **Azure Active Directory** button.</span></span> 

    ![hello Azure Active Directory gomb][1]

2. <span data-ttu-id="40633-129">Válassza ki **vállalati alkalmazások** > **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="40633-129">Select **Enterprise applications** > **All applications**.</span></span>

    ![hello "Vállalati alkalmazások" ablak][2]
    
3. <span data-ttu-id="40633-131">az alkalmazás hello tooadd **összes alkalmazás** párbeszédpanelen jelölje ki **új alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="40633-131">tooadd an application, in hello **All applications** dialog box, select **New application**.</span></span>

    !["Az új alkalmazás" Hello gomb][3]

4. <span data-ttu-id="40633-133">Hello keresési mezőbe, írja be a **vxMaintain**.</span><span class="sxs-lookup"><span data-stu-id="40633-133">In hello search box, type **vxMaintain**.</span></span>

    ![hello "Egyszeri bejelentkezés mód" legördülő lista](./media/active-directory-saas-vxmaintain-tutorial/tutorial_vxmaintain_search.png)

5. <span data-ttu-id="40633-135">Hello eredmények listában válassza ki a **vxMaintain**, majd válassza ki **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="40633-135">In hello results list, select **vxMaintain**, and then select **Add**.</span></span>

    ![hello vxMaintain hivatkozás](./media/active-directory-saas-vxmaintain-tutorial/tutorial_vxmaintain_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="40633-137">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="40633-137">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="40633-138">Ebben a szakaszban konfigurálása és tesztelése az Azure AD SSO vxMaintain "Britta Simon." nevű tesztfelhasználó alapján használatával</span><span class="sxs-lookup"><span data-stu-id="40633-138">In this section, you configure and test Azure AD SSO by using vxMaintain, based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="40633-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow hello vxMaintain megfelelőjére toohello Azure AD-felhasználó.</span><span class="sxs-lookup"><span data-stu-id="40633-139">For SSO toowork, Azure AD needs tooknow hello vxMaintain counterpart toohello Azure AD user.</span></span> <span data-ttu-id="40633-140">Ez azt jelenti, hogy hello Azure AD-felhasználó és hello megfelelő vxMaintain felhasználó közötti kapcsolat kapcsolatot kell létesítenie.</span><span class="sxs-lookup"><span data-stu-id="40633-140">That is, you must establish a link relationship between hello Azure AD user and hello corresponding vxMaintain user.</span></span>

<span data-ttu-id="40633-141">tooestablish hello hivatkozás kapcsolat hozzárendelése hello vxMaintain **felhasználónév** érték, mint az Azure AD hello **felhasználónév** érték.</span><span class="sxs-lookup"><span data-stu-id="40633-141">tooestablish hello link relationship, assign hello vxMaintain **user name** value as hello Azure AD **Username** value.</span></span>

<span data-ttu-id="40633-142">tooconfigure és tesztelése az Azure AD SSO vxMaintain, a következő építőelemeket teljes hello segítségével.</span><span class="sxs-lookup"><span data-stu-id="40633-142">tooconfigure and test Azure AD SSO by using vxMaintain, complete hello following building blocks.</span></span>

### <a name="configure-azure-ad-sso"></a><span data-ttu-id="40633-143">Az Azure AD-egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="40633-143">Configure Azure AD SSO</span></span>

<span data-ttu-id="40633-144">Ebben a szakaszban akkor is engedélyezze az Azure AD egyszeri Bejelentkezést a hello Azure-portálon és beállíthatja SSO vxMaintain alkalmazásában hello következő tevékenységek végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="40633-144">In this section, you can both enable Azure AD SSO in hello Azure portal and configure SSO in your vxMaintain application by doing hello following:</span></span>

1. <span data-ttu-id="40633-145">Az Azure portál, a hello hello **vxMaintain** alkalmazás integrációs lapon jelölje be **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="40633-145">In hello Azure portal, on hello **vxMaintain** application integration page, select **Single sign-on**.</span></span>

    !["Egyszeri bejelentkezés" parancs hello][4]

2. <span data-ttu-id="40633-147">a hello SSO, tooenable **egyszeri bejelentkezés mód** legördülő listában válassza **SAML-alapú bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="40633-147">tooenable SSO, in hello **Single Sign-on Mode** drop-down list, select **SAML-based Sign-on**.</span></span>
 
    ![hello "SAML-alapú bejelentkezés" parancs](./media/active-directory-saas-vxmaintain-tutorial/tutorial_vxmaintain_samlbase.png)

3. <span data-ttu-id="40633-149">A **vxMaintain tartomány és az URL-címek**, a következő hello:</span><span class="sxs-lookup"><span data-stu-id="40633-149">Under **vxMaintain Domain and URLs**, do hello following:</span></span>

    ![hello vxMaintain tartomány és az URL-címek szakasz](./media/active-directory-saas-vxmaintain-tutorial/tutorial_vxmaintain_url.png)

    <span data-ttu-id="40633-151">a.</span><span class="sxs-lookup"><span data-stu-id="40633-151">a.</span></span> <span data-ttu-id="40633-152">A hello **azonosító** mezőbe írja be egy URL-címet, amely rendelkezik hello szintaxisa a következő:`https://<company name>.verisae.com`</span><span class="sxs-lookup"><span data-stu-id="40633-152">In hello **Identifier** box, type a URL that has hello following syntax: `https://<company name>.verisae.com`</span></span>

    <span data-ttu-id="40633-153">b.</span><span class="sxs-lookup"><span data-stu-id="40633-153">b.</span></span> <span data-ttu-id="40633-154">A hello **válasz URL-CÍMEN** mezőbe írja be egy URL-címet, amely rendelkezik hello szintaxisa a következő:`https://<company name>.verisae.com/DataNett/action/ssoConsume/mobile?_log=true`</span><span class="sxs-lookup"><span data-stu-id="40633-154">In hello **Reply URL** box, type a URL that has hello following syntax: `https://<company name>.verisae.com/DataNett/action/ssoConsume/mobile?_log=true`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="40633-155">hello előző értékei nem valódi.</span><span class="sxs-lookup"><span data-stu-id="40633-155">hello preceding values are not real.</span></span> <span data-ttu-id="40633-156">Hello tényleges azonosítójú frissítheti, illetve válasz URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="40633-156">Update them with hello actual identifier and reply URL.</span></span> <span data-ttu-id="40633-157">tooobtain hello értékek kapcsolattartási hello [vxMaintain támogatási csoport](http://www.verisae.com/contact-us).</span><span class="sxs-lookup"><span data-stu-id="40633-157">tooobtain hello values, contact hello [vxMaintain support team](http://www.verisae.com/contact-us).</span></span>
 
4. <span data-ttu-id="40633-158">A **SAML-aláíró tanúsítványa**, jelölje be **metaadatainak XML-kódja**, majd mentse a hello metaadatok fájl tooyour számítógép.</span><span class="sxs-lookup"><span data-stu-id="40633-158">Under **SAML Signing Certificate**, select **Metadata XML**, and then save hello metadata file tooyour computer.</span></span>

    ![hello "SAML aláíró tanúsítvány" szakasz](./media/active-directory-saas-vxmaintain-tutorial/tutorial_vxmaintain_certificate.png) 

5. <span data-ttu-id="40633-160">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="40633-160">Select **Save**.</span></span>

    ![hello Mentés gombra](./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="40633-162">tooconfigure **vxMaintain** SSO, letöltött küldési hello **metaadatainak XML-kódja** toohello fájl [vxMaintain támogatási csoport](http://www.verisae.com/contact-us).</span><span class="sxs-lookup"><span data-stu-id="40633-162">tooconfigure **vxMaintain** SSO, send hello downloaded **Metadata XML** file toohello [vxMaintain support team](http://www.verisae.com/contact-us).</span></span>

> [!TIP]
> <span data-ttu-id="40633-163">Hello app állít be, mivel el tudja olvasni az utasításokat megelőző hello hello tömör verziójának [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="40633-163">As you set up hello app, you can read a concise version of hello preceding instructions in hello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="40633-164">A hello hello alkalmazás hozzáadása után **Active Directory** > **vállalati alkalmazások** szakaszban, jelölje be hello **egyszeri bejelentkezés** fülre, és hozzáférést hello a beágyazott hello dokumentáció **konfigurációs** szakasz.</span><span class="sxs-lookup"><span data-stu-id="40633-164">After you add hello app from hello **Active Directory** > **Enterprise Applications** section, select hello **Single Sign-On** tab, and then access hello embedded documentation from hello **Configuration** section.</span></span> 
>
><span data-ttu-id="40633-165">toolearn hello embedded dokumentációjából funkció, bővebben lásd: [egyszeri bejelentkezéshez a vállalati alkalmazásokat kezeléséhez](https://go.microsoft.com/fwlink/?linkid=845985).</span><span class="sxs-lookup"><span data-stu-id="40633-165">toolearn more about hello embedded documentation feature, see [Managing single sign-on for enterprise apps](https://go.microsoft.com/fwlink/?linkid=845985).</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="40633-166">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="40633-166">Create an Azure AD test user</span></span>
<span data-ttu-id="40633-167">Ebben a szakaszban Britta Simon tesztfelhasználó hello Azure-portálon létrehozhat hello következő tevékenységek végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="40633-167">In this section, you create test user Britta Simon in hello Azure portal by doing hello following:</span></span>

![az Azure AD hello tesztfelhasználó számára][100]

1. <span data-ttu-id="40633-169">A hello **Azure-portálon**, a bal oldali ablaktáblán hello, válassza ki a hello **Azure Active Directory** gombra.</span><span class="sxs-lookup"><span data-stu-id="40633-169">In hello **Azure portal**, in hello left pane, select hello **Azure Active Directory** button.</span></span>

    ![hello "Azure Active Directory" gomb](./media/active-directory-saas-vxmaintain-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="40633-171">túl nyissa meg azoknak a felhasználóknak, toodisplay**felhasználók és csoportok** > **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="40633-171">toodisplay a list of users, go too**Users and groups** > **All users**.</span></span>
    
    <span data-ttu-id="40633-172">![hello "Minden felhasználó" hivatkozásra.](./media/active-directory-saas-vxmaintain-tutorial/create_aaduser_02.png)</span><span class="sxs-lookup"><span data-stu-id="40633-172">![hello "All users" link](./media/active-directory-saas-vxmaintain-tutorial/create_aaduser_02.png)</span></span>  
    <span data-ttu-id="40633-173">Hello **minden felhasználó** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="40633-173">hello **All users** dialog box opens.</span></span> 

3. <span data-ttu-id="40633-174">tooopen hello **felhasználói** párbeszédpanelen jelölje ki **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="40633-174">tooopen hello **User** dialog box, select **Add**.</span></span>
 
    ![hello Hozzáadás gomb](./media/active-directory-saas-vxmaintain-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="40633-176">A hello **felhasználói** párbeszédpanel mezőbe hello a következő:</span><span class="sxs-lookup"><span data-stu-id="40633-176">In hello **User** dialog box, do hello following:</span></span>
 
    ![hello felhasználó párbeszédpanel](./media/active-directory-saas-vxmaintain-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="40633-178">a.</span><span class="sxs-lookup"><span data-stu-id="40633-178">a.</span></span> <span data-ttu-id="40633-179">A hello **neve** mezőbe írja be **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="40633-179">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="40633-180">b.</span><span class="sxs-lookup"><span data-stu-id="40633-180">b.</span></span> <span data-ttu-id="40633-181">A hello **felhasználónév** mezőben, a teszt felhasználó Britta Simon típus hello e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="40633-181">In hello **User name** box, type hello email address of test user Britta Simon.</span></span>

    <span data-ttu-id="40633-182">c.</span><span class="sxs-lookup"><span data-stu-id="40633-182">c.</span></span> <span data-ttu-id="40633-183">Jelölje be hello **megjelenítése jelszó** jelölőnégyzetet, majd a Megjegyzés hello értéket hello okozó **jelszó** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="40633-183">Select hello **Show Password** check box, and then note hello value that was generated in hello **Password** box.</span></span>

    <span data-ttu-id="40633-184">d.</span><span class="sxs-lookup"><span data-stu-id="40633-184">d.</span></span> <span data-ttu-id="40633-185">Kattintson a **Létrehozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="40633-185">Select **Create**.</span></span>
 
### <a name="create-a-vxmaintain-test-user"></a><span data-ttu-id="40633-186">VxMaintain tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="40633-186">Create a vxMaintain test user</span></span>

<span data-ttu-id="40633-187">Ebben a szakaszban Britta Simon tesztfelhasználó vxMaintain hoz létre.</span><span class="sxs-lookup"><span data-stu-id="40633-187">In this section, you create test user Britta Simon in vxMaintain.</span></span> <span data-ttu-id="40633-188">hello vxMaintain platform, tooadd felhasználók dolgozni a [vxMaintain támogatási csoport](http://www.verisae.com/contact-us).</span><span class="sxs-lookup"><span data-stu-id="40633-188">tooadd users in hello vxMaintain platform, work with the [vxMaintain support team](http://www.verisae.com/contact-us).</span></span> <span data-ttu-id="40633-189">Mielőtt használná az egyszeri Bejelentkezést, hozzon létre, és hello felhasználók aktiválása.</span><span class="sxs-lookup"><span data-stu-id="40633-189">Before you use SSO, create and activate hello users.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="40633-190">Rendelje hozzá az Azure AD hello tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="40633-190">Assign hello Azure AD test user</span></span>

<span data-ttu-id="40633-191">Ebben a szakaszban a tesztfelhasználó Britta Simon toouse Azure SSO hozzáférés toovxMaintain megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="40633-191">In this section, you enable test user Britta Simon toouse Azure SSO by granting access toovxMaintain.</span></span> <span data-ttu-id="40633-192">toodo tehát hello a következő:</span><span class="sxs-lookup"><span data-stu-id="40633-192">toodo so, do hello following:</span></span>

![Teszt felhasználó hello megjelenítendő név listában][200] 

1. <span data-ttu-id="40633-194">Az Azure-portálon hello **alkalmazások** túl megtekintheti**Directory** Nézet > **vállalati alkalmazások** > **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="40633-194">In hello Azure portal **Applications** view, go too**Directory** view > **Enterprise applications** > **All applications**.</span></span>

    !["az összes alkalmazások" Hello][201] 

2. <span data-ttu-id="40633-196">A hello **alkalmazások** listáról válassza ki **vxMaintain**.</span><span class="sxs-lookup"><span data-stu-id="40633-196">In hello **Applications** list, select **vxMaintain**.</span></span>

    ![hello vxMaintain hivatkozás](./media/active-directory-saas-vxmaintain-tutorial/tutorial_vxmaintain_app.png) 

3. <span data-ttu-id="40633-198">Hello bal oldali ablaktáblában jelöljön ki **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="40633-198">In hello left pane, select **Users and groups**.</span></span>

    ![hello "Felhasználók és csoportok" hivatkozásra.][202] 

4. <span data-ttu-id="40633-200">Válassza ki **Hozzáadás** , majd a hello **hozzáadása hozzárendelés** ablaktáblán válassza előbb **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="40633-200">Select **Add** and then, in hello **Add Assignment** pane, select **Users and groups**.</span></span>

    ![hello "Felhasználók és csoportok" hivatkozásra.][203]

5. <span data-ttu-id="40633-202">A hello **felhasználók és csoportok** hello párbeszédpanel **felhasználók** listáról válassza ki **Britta Simon**, majd válassza ki a hello **válassza ki** gomb.</span><span class="sxs-lookup"><span data-stu-id="40633-202">In hello **Users and groups** dialog box, in hello **Users** list, select **Britta Simon**, and then select hello **Select** button.</span></span>

7. <span data-ttu-id="40633-203">A hello **hozzáadása hozzárendelés** párbeszédpanelen jelölje ki **hozzárendelése**.</span><span class="sxs-lookup"><span data-stu-id="40633-203">In hello **Add Assignment** dialog box, select **Assign**.</span></span>
    
### <a name="test-your-azure-ad-single-sign-on"></a><span data-ttu-id="40633-204">Az Azure AD az egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="40633-204">Test your Azure AD single sign-on</span></span>

<span data-ttu-id="40633-205">Ebben a szakaszban az Azure AD SSO konfigurációs hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="40633-205">In this section, you test your Azure AD SSO configuration by using hello Access Panel.</span></span>

<span data-ttu-id="40633-206">Kiválasztásával hello **vxMaintain** csempe a hozzáférési Panel hello kell bejelentkezés tooyour vxMaintain alkalmazás automatikusan.</span><span class="sxs-lookup"><span data-stu-id="40633-206">Selecting hello **vxMaintain** tile in hello Access Panel should sign you in tooyour vxMaintain application automatically.</span></span>

<span data-ttu-id="40633-207">A hozzáférési Panel kapcsolatos további információkért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="40633-207">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="40633-208">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="40633-208">Next steps</span></span>

* [<span data-ttu-id="40633-209">SaaS-alkalmazások integrálása az Azure Active Directoryval kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="40633-209">List of tutorials on integrating SaaS apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="40633-210">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="40633-210">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_203.png

