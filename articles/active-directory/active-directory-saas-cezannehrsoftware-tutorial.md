---
title: "Oktatóanyag: Azure Active Directory-integráció Cezanne HR szoftverrel |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Cezanne HR szoftverek között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: fb8f95bf-c3c1-4e1f-b2b3-3b67526be72d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: jeedes
ms.openlocfilehash: 3675acd8871d62c2277def8074f7aa39ac46e2a3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-integrate-azure-active-directory-with-cezanne-hr-software"></a><span data-ttu-id="19ec3-103">Oktatóanyag: Azure Active Directory integrálása Cezanne HR szoftver</span><span class="sxs-lookup"><span data-stu-id="19ec3-103">Tutorial: Integrate Azure Active Directory with Cezanne HR software</span></span>

<span data-ttu-id="19ec3-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Cezanne HR szoftver az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="19ec3-104">In this tutorial, you learn how toointegrate Cezanne HR software with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="19ec3-105">Cezanne HR szoftver integrálása az Azure AD lehetőséget biztosít a következő előnyöket hello.</span><span class="sxs-lookup"><span data-stu-id="19ec3-105">Integrating Cezanne HR software with Azure AD provides you with hello following benefits.</span></span> <span data-ttu-id="19ec3-106">A következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="19ec3-106">You can:</span></span>

- <span data-ttu-id="19ec3-107">Az Azure AD hozzáférési tooCezanne rendelkező vezérlő HR szoftver.</span><span class="sxs-lookup"><span data-stu-id="19ec3-107">Control in Azure AD who has access tooCezanne HR software.</span></span>
- <span data-ttu-id="19ec3-108">A tooCezanne HR szoftver egyszeri bejelentkezés (SSO) az Azure AD-fiókkal rendelkező felhasználók tooautomatically bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="19ec3-108">Enable your users tooautomatically sign in tooCezanne HR software with single sign-on (SSO) with their Azure AD accounts.</span></span>
- <span data-ttu-id="19ec3-109">A fiók egyetlen központi helyen kezelheti: hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="19ec3-109">Manage your accounts in one central location: hello Azure portal.</span></span>

<span data-ttu-id="19ec3-110">További információ az Azure AD-val egy szolgáltatott szoftverként (SaaS) alkalmazásintegráció szoftverként toolearn lásd [Mi az az alkalmazás-hozzáférés és az egyszeri bejelentkezés az Azure Active Directoryval?](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="19ec3-110">toolearn more about software as a service (SaaS) app integration with Azure AD, see [What is application access and SSO with Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="19ec3-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="19ec3-111">Prerequisites</span></span>

<span data-ttu-id="19ec3-112">tooconfigure Cezanne HR szoftver az Azure AD integrálása, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="19ec3-112">tooconfigure Azure AD integration with Cezanne HR software, you need hello following items:</span></span>

- <span data-ttu-id="19ec3-113">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="19ec3-113">An Azure AD subscription</span></span>
- <span data-ttu-id="19ec3-114">Egy Cezanne HR szoftver előfizetés SSO engedélyezése</span><span class="sxs-lookup"><span data-stu-id="19ec3-114">A Cezanne HR software SSO-enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="19ec3-115">tootest hello lépéseit az oktatóanyag, javasoljuk, hogy ne használjon egy éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="19ec3-115">tootest hello steps in this tutorial, we recommend that you do not use a production environment.</span></span>

<span data-ttu-id="19ec3-116">Ebben az oktatóanyagban tootest hello lépéseit kövesse az alábbi ajánlásokat:</span><span class="sxs-lookup"><span data-stu-id="19ec3-116">tootest hello steps in this tutorial, follow these recommendations:</span></span>

- <span data-ttu-id="19ec3-117">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="19ec3-117">Don't use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="19ec3-118">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="19ec3-118">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="19ec3-119">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="19ec3-119">Scenario description</span></span>
<span data-ttu-id="19ec3-120">Ebben az oktatóanyagban a Azure AD SSO tesztkörnyezetben tesztelni.</span><span class="sxs-lookup"><span data-stu-id="19ec3-120">In this tutorial, you test Azure AD SSO in a test environment.</span></span> 

<span data-ttu-id="19ec3-121">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="19ec3-121">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

* <span data-ttu-id="19ec3-122">Hello gyűjteményből Cezanne HR szoftver hozzáadása</span><span class="sxs-lookup"><span data-stu-id="19ec3-122">Adding Cezanne HR software from hello gallery</span></span>
* <span data-ttu-id="19ec3-123">Azure AD egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="19ec3-123">Configuring and testing Azure AD SSO</span></span>

## <a name="add-cezanne-hr-software-from-hello-gallery"></a><span data-ttu-id="19ec3-124">Hello gyűjteményből Cezanne HR szoftver hozzáadása</span><span class="sxs-lookup"><span data-stu-id="19ec3-124">Add Cezanne HR software from hello gallery</span></span>
<span data-ttu-id="19ec3-125">tooconfigure hello integrációs Cezanne HR szoftver az Azure AD-be Cezanne HR szoftver hozzáadása hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="19ec3-125">tooconfigure hello integration of Cezanne HR software into Azure AD, add Cezanne HR software from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="19ec3-126">tooadd Cezanne HR szoftver hello gyűjteményből hello a következő:</span><span class="sxs-lookup"><span data-stu-id="19ec3-126">tooadd Cezanne HR software from hello gallery, do hello following:</span></span>

1. <span data-ttu-id="19ec3-127">A hello  **[Azure-portálon](https://portal.azure.com)**, a bal oldali ablaktáblán hello, válassza ki a hello **Azure Active Directory** gombra.</span><span class="sxs-lookup"><span data-stu-id="19ec3-127">In hello **[Azure portal](https://portal.azure.com)**, in hello left pane, select hello **Azure Active Directory** button.</span></span> 

    ![hello "Azure Active Directory" gomb][1]

2. <span data-ttu-id="19ec3-129">Válassza ki **vállalati alkalmazások** > **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="19ec3-129">Select **Enterprise applications** > **All applications**.</span></span>

    !["az összes alkalmazások" Hello][2]
    
3. <span data-ttu-id="19ec3-131">egy új alkalmazást, hello hello tetején tooadd **összes alkalmazás** párbeszédpanelen jelölje ki **új alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="19ec3-131">tooadd a new application, at hello top of hello **All applications** dialog box, select **New application**.</span></span>

    !["Az új alkalmazás" Hello gomb][3]

4. <span data-ttu-id="19ec3-133">Hello keresési mezőbe, írja be a **Cezanne HR szoftver**.</span><span class="sxs-lookup"><span data-stu-id="19ec3-133">In hello search box, type **Cezanne HR Software**.</span></span>

    ![hello keresőmezőbe](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_search.png)

5. <span data-ttu-id="19ec3-135">Hello eredmények listában válassza ki a **Cezanne HR szoftver** , és válassza a hello **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="19ec3-135">In hello results list, select **Cezanne HR Software** and then select hello **Add** button tooadd hello application.</span></span>

    ![hello eredményeinek listája](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="19ec3-137">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="19ec3-137">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="19ec3-138">Ebben a szakaszban, tesztelése és konfigurálása Azure AD SSO "Britta Simon." nevű tesztfelhasználó alapján Cezanne HR szoftverrel</span><span class="sxs-lookup"><span data-stu-id="19ec3-138">In this section, you configure and test Azure AD SSO with Cezanne HR software based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="19ec3-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow hello Cezanne HR szoftver megfelelőjére toohello Azure AD-felhasználó.</span><span class="sxs-lookup"><span data-stu-id="19ec3-139">For SSO toowork, Azure AD needs tooknow hello Cezanne HR software counterpart toohello Azure AD user.</span></span> <span data-ttu-id="19ec3-140">Más szóval hello Cezanne HR szoftver hello kapcsolódó felhasználói és az Azure AD-felhasználó közötti kapcsolat kapcsolatot kell létesítenie.</span><span class="sxs-lookup"><span data-stu-id="19ec3-140">In other words, you must establish a link relationship between an Azure AD user and hello related user in hello Cezanne HR software.</span></span>

<span data-ttu-id="19ec3-141">tooestablish hello hivatkozás kapcsolat hozzárendelése hello Cezanne HR szoftver **felhasználónév** érték, mint az Azure AD hello **felhasználónév** érték.</span><span class="sxs-lookup"><span data-stu-id="19ec3-141">tooestablish hello link relationship, assign hello Cezanne HR software **user name** value as hello Azure AD **Username** value.</span></span>

<span data-ttu-id="19ec3-142">tooconfigure és tesztelése az Azure AD SSO Cezanne HR szoftver, a következő építőelemeket teljes hello segítségével.</span><span class="sxs-lookup"><span data-stu-id="19ec3-142">tooconfigure and test Azure AD SSO by using Cezanne HR software, complete hello following building blocks.</span></span>

### <a name="configure-azure-ad-sso"></a><span data-ttu-id="19ec3-143">Az Azure AD-egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="19ec3-143">Configure Azure AD SSO</span></span>

<span data-ttu-id="19ec3-144">Ebben a szakaszban engedélyezze az Azure AD egyszeri Bejelentkezést a hello Azure-portálon, és egyszeri bejelentkezés konfigurálása a Cezanne HR alkalmazás hello következő tevékenységek végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="19ec3-144">In this section, you can enable Azure AD SSO in hello Azure portal and configure SSO in your Cezanne HR software application by doing hello following:</span></span>

1. <span data-ttu-id="19ec3-145">Az Azure portál, a hello hello **Cezanne HR szoftver** alkalmazás integrációs lapon jelölje be **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="19ec3-145">In hello Azure portal, on hello **Cezanne HR Software** application integration page, select **Single sign-on**.</span></span>

    !["Egyszeri bejelentkezés" parancs hello][4]

2. <span data-ttu-id="19ec3-147">a hello SSO, tooenable **egyszeri bejelentkezés** párbeszédpanel megnyitásához, jelölje be hello **mód** , **SAML-alapú bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="19ec3-147">tooenable SSO, in hello **Single sign-on** dialog box, select hello **Mode** as **SAML-based Sign-on**.</span></span>
 
    ![hello "Mód" mezőt](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_samlbase.png)

3. <span data-ttu-id="19ec3-149">A **Cezanne HR szoftver tartomány és az URL-címek**, a következő hello:</span><span class="sxs-lookup"><span data-stu-id="19ec3-149">Under **Cezanne HR Software Domain and URLs**, do hello following:</span></span>

    ![hello "Cezanne HR szoftver tartományi és URL-címek" szakasz](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_url.png)

    <span data-ttu-id="19ec3-151">a.</span><span class="sxs-lookup"><span data-stu-id="19ec3-151">a.</span></span> <span data-ttu-id="19ec3-152">A hello **bejelentkezési URL-cím** mezőbe írja be egy URL-címet, amely rendelkezik hello szintaxisa a következő:`https://w3.cezanneondemand.com/cezannehr/-/<tenant id>`</span><span class="sxs-lookup"><span data-stu-id="19ec3-152">In hello **Sign-on URL** box, type a URL that has hello following syntax: `https://w3.cezanneondemand.com/cezannehr/-/<tenant id>`</span></span>

    <span data-ttu-id="19ec3-153">b.</span><span class="sxs-lookup"><span data-stu-id="19ec3-153">b.</span></span> <span data-ttu-id="19ec3-154">A hello **válasz URL-CÍMEN** mezőbe írja be egy URL-címet, amely rendelkezik hello szintaxisa a következő:`https://w3.cezanneondemand.com:443/<tenantid>`</span><span class="sxs-lookup"><span data-stu-id="19ec3-154">In hello **Reply URL** box, type a URL that has hello following syntax: `https://w3.cezanneondemand.com:443/<tenantid>`</span></span>    
     
    > [!NOTE] 
    > <span data-ttu-id="19ec3-155">hello előző értékei nem valódi.</span><span class="sxs-lookup"><span data-stu-id="19ec3-155">hello preceding values are not real.</span></span> <span data-ttu-id="19ec3-156">Frissítse azokat a hello tényleges válasz URL-CÍMEN és hello bejelentkezési URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="19ec3-156">Update them with hello actual reply URL and hello sign-on URL.</span></span> <span data-ttu-id="19ec3-157">tooobtain hello értékek kapcsolattartási hello [Cezanne HR szoftver ügyfél támogatási csoport](mailto:info@cezannehr.com).</span><span class="sxs-lookup"><span data-stu-id="19ec3-157">tooobtain hello values, contact hello [Cezanne HR software client support team](mailto:info@cezannehr.com).</span></span>

4. <span data-ttu-id="19ec3-158">A **SAML-aláíró tanúsítványa**, jelölje be **tanúsítvány (Base64)**, és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="19ec3-158">Under **SAML Signing Certificate**, select **Certificate (Base64)**, and then save hello certificate file on your computer.</span></span>

    ![hello "SAML aláíró tanúsítvány" szakasz](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_certificate.png) 

5. <span data-ttu-id="19ec3-160">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="19ec3-160">Select **Save**.</span></span>

    ![hello "Mentés" gombra.](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_400.png)
    
6. <span data-ttu-id="19ec3-162">A **Cezanne HR szoftverkonfigurációt**, jelölje be **Cezanne HR szoftver konfigurálása** tooopen hello **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="19ec3-162">Under **Cezanne HR Software Configuration**, select **Configure Cezanne HR Software** tooopen hello **Configure sign-on** window.</span></span> <span data-ttu-id="19ec3-163">Másolás hello **SAML Entitásazonosító** és **SAML-alapú egyszeri bejelentkezési szolgáltatás** hello URL-cím- **rövid összefoglaló** szakasz.</span><span class="sxs-lookup"><span data-stu-id="19ec3-163">Copy hello **SAML Entity ID** and **SAML Single Sign-On Service** URL from hello **Quick Reference** section.</span></span>

    ![hello "Cezanne HR szoftver konfiguráció" szakasz](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_configure.png) 

7. <span data-ttu-id="19ec3-165">Egy másik webes böngészőablakban tooyour Cezanne HR szoftver bérlői rendszergazdai bejelentkezés.</span><span class="sxs-lookup"><span data-stu-id="19ec3-165">In a different web browser window, sign on tooyour Cezanne HR software tenant as an administrator.</span></span>

8. <span data-ttu-id="19ec3-166">Hello bal oldali ablaktáblában jelöljön ki **rendszerbeállítás**.</span><span class="sxs-lookup"><span data-stu-id="19ec3-166">In hello left pane, select **System Setup**.</span></span> <span data-ttu-id="19ec3-167">Válassza ki **biztonsági beállítások** > **az egyszeri bejelentkezés konfigurációs**.</span><span class="sxs-lookup"><span data-stu-id="19ec3-167">Select **Security Settings** > **Single Sign-On Configuration**.</span></span>

    ![hello "Egyszeri bejelentkezés konfiguráció" hivatkozásra](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_000.png)

9. <span data-ttu-id="19ec3-169">A hello **engedélyezése az egyszeri bejelentkezés (SSO) szolgáltatások a következő hello segítségével a felhasználók toolog** ablaktáblában válassza hello **SAML 2.0** jelölőnégyzetet, és jelölje be hello **speciális konfiguráció** a beállítás.</span><span class="sxs-lookup"><span data-stu-id="19ec3-169">In hello **Allow users toolog in using hello following Single Sign-On (SSO) services** pane, select hello **SAML 2.0** check box and select hello **Advanced Configuration** option.</span></span>

    ![Egyszeri bejelentkezés szolgáltatás beállításai](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_001.png)

10. <span data-ttu-id="19ec3-171">Válassza ki **hozzáadhat új**.</span><span class="sxs-lookup"><span data-stu-id="19ec3-171">Select **Add New**.</span></span>

    ![hello "Új hozzáadása" gombra](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_002.png)

11. <span data-ttu-id="19ec3-173">A **SAML 2.0 identitás-szolgáltatóktól**, a következő hello:</span><span class="sxs-lookup"><span data-stu-id="19ec3-173">Under **SAML 2.0 Identity Providers**, do hello following:</span></span>

    ![hello "SAML 2.0 identitás-szolgáltatóktól" szakasz](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_003.png)
    
    <span data-ttu-id="19ec3-175">a.</span><span class="sxs-lookup"><span data-stu-id="19ec3-175">a.</span></span> <span data-ttu-id="19ec3-176">A hello **megjelenített név** mezőbe írja be az identitásszolgáltató hello nevét.</span><span class="sxs-lookup"><span data-stu-id="19ec3-176">In hello **Display Name** box, enter hello name of your identity provider.</span></span>

    <span data-ttu-id="19ec3-177">b.</span><span class="sxs-lookup"><span data-stu-id="19ec3-177">b.</span></span> <span data-ttu-id="19ec3-178">A hello **entitásazonosító** mezőbe illessze be a hello **SAML Entitásazonosító** hello Azure-portálon fájlból másolt.</span><span class="sxs-lookup"><span data-stu-id="19ec3-178">In hello **Entity Identifier** box, paste hello **SAML Entity ID** that you copied from hello Azure portal.</span></span> 

    <span data-ttu-id="19ec3-179">c.</span><span class="sxs-lookup"><span data-stu-id="19ec3-179">c.</span></span> <span data-ttu-id="19ec3-180">A hello **SAML kötés** listáján jelölje ki **POST**.</span><span class="sxs-lookup"><span data-stu-id="19ec3-180">In hello **SAML Binding** list box, select **POST**.</span></span>

    <span data-ttu-id="19ec3-181">d.</span><span class="sxs-lookup"><span data-stu-id="19ec3-181">d.</span></span> <span data-ttu-id="19ec3-182">A hello **biztonsági jogkivonat szolgáltatásvégpont** mezőbe illessze be a hello **SAML-alapú egyszeri bejelentkezési szolgáltatás** hello Azure-portálon fájlból másolt URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="19ec3-182">In hello **Security Token Service Endpoint** box, paste hello **SAML Single Sign-On Service** URL that you copied from hello Azure portal.</span></span> 
    
    <span data-ttu-id="19ec3-183">e.</span><span class="sxs-lookup"><span data-stu-id="19ec3-183">e.</span></span> <span data-ttu-id="19ec3-184">A hello **felhasználói azonosító attribútum neve** adja meg a `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.</span><span class="sxs-lookup"><span data-stu-id="19ec3-184">In hello **User ID Attribute Name** box, enter `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.</span></span>
    
    <span data-ttu-id="19ec3-185">f.</span><span class="sxs-lookup"><span data-stu-id="19ec3-185">f.</span></span> <span data-ttu-id="19ec3-186">tooupload hello letöltött tanúsítvány az Azure AD, jelölje be hello **feltöltése** gombra.</span><span class="sxs-lookup"><span data-stu-id="19ec3-186">tooupload hello downloaded certificate from Azure AD, select hello **Upload** button.</span></span>
    
    <span data-ttu-id="19ec3-187">g.</span><span class="sxs-lookup"><span data-stu-id="19ec3-187">g.</span></span> <span data-ttu-id="19ec3-188">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="19ec3-188">Select **OK**.</span></span> 

12. <span data-ttu-id="19ec3-189">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="19ec3-189">Select **Save**.</span></span>

    ![hello "Mentés" gombra.](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_004.png)

> [!TIP]
> <span data-ttu-id="19ec3-191">Hello app állít be, mivel el tudja olvasni az utasításokat megelőző hello hello tömör verziójának [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="19ec3-191">As you set up hello app, you can read a concise version of hello preceding instructions in hello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="19ec3-192">A hello hello alkalmazás hozzáadása után **Active Directory** > **vállalati alkalmazások** szakaszban, jelölje be hello **egyszeri bejelentkezés** fülre. Ezután a hozzáférés hello beágyazott hello dokumentáció **konfigurációs** szakasz.</span><span class="sxs-lookup"><span data-stu-id="19ec3-192">After you add hello app from hello **Active Directory** > **Enterprise applications** section, select hello **Single sign-on** tab. Then access hello embedded documentation from hello **Configuration** section.</span></span> 

<span data-ttu-id="19ec3-193">toolearn hello embedded dokumentációjából funkció, bővebben lásd: [az Azure AD dokumentációjában beágyazott]( https://go.microsoft.com/fwlink/?linkid=845985).</span><span class="sxs-lookup"><span data-stu-id="19ec3-193">toolearn more about hello embedded documentation feature, see [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985).</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="19ec3-194">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="19ec3-194">Create an Azure AD test user</span></span>
<span data-ttu-id="19ec3-195">Ebben a szakaszban Britta Simon tesztfelhasználó hello Azure-portálon hoz létre.</span><span class="sxs-lookup"><span data-stu-id="19ec3-195">In this section, you create test user Britta Simon in hello Azure portal.</span></span>

![hello tesztfelhasználó Britta Simon][100]

<span data-ttu-id="19ec3-197">az Azure AD-tesztfelhasználó toocreate hello a következő:</span><span class="sxs-lookup"><span data-stu-id="19ec3-197">toocreate a test user in Azure AD, do hello following:</span></span>

1. <span data-ttu-id="19ec3-198">A hello **Azure-portálon**, a bal oldali ablaktáblán hello, válassza ki a hello **Azure Active Directory** gombra.</span><span class="sxs-lookup"><span data-stu-id="19ec3-198">In hello **Azure portal**, in hello left pane, select hello **Azure Active Directory** button.</span></span>

    ![hello "Azure Active Directory" gomb](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="19ec3-200">Válassza ki a felhasználók toodisplay hello lista **felhasználók és csoportok** > **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="19ec3-200">toodisplay hello list of users, select **Users and groups** > **All users**.</span></span>
    
    ![hello "Minden felhasználó" hivatkozásra.](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_02.png) 
    
    <span data-ttu-id="19ec3-202">Hello **minden felhasználó** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="19ec3-202">hello **All users** dialog box opens.</span></span>

3. <span data-ttu-id="19ec3-203">tooopen hello **felhasználói** párbeszédpanelen jelölje ki **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="19ec3-203">tooopen hello **User** dialog box, select **Add**.</span></span>
 
    ![hello "Hozzáadás" gombra](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="19ec3-205">A hello **felhasználói** párbeszédpanel mezőbe hello a következő:</span><span class="sxs-lookup"><span data-stu-id="19ec3-205">In hello **User** dialog box, do hello following:</span></span>
 
    ![hello "User" párbeszédpanel](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="19ec3-207">a.</span><span class="sxs-lookup"><span data-stu-id="19ec3-207">a.</span></span> <span data-ttu-id="19ec3-208">A hello **neve** mezőbe írja be **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="19ec3-208">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="19ec3-209">b.</span><span class="sxs-lookup"><span data-stu-id="19ec3-209">b.</span></span> <span data-ttu-id="19ec3-210">A hello **felhasználónév** mezőbe írja be a felhasználó Britta Simon **e-mail cím**.</span><span class="sxs-lookup"><span data-stu-id="19ec3-210">In hello **User name** box, type user Britta Simon's **email address**.</span></span>

    <span data-ttu-id="19ec3-211">c.</span><span class="sxs-lookup"><span data-stu-id="19ec3-211">c.</span></span> <span data-ttu-id="19ec3-212">Jelölje be hello **megjelenítése jelszó** jelölőnégyzetet, majd a Megjegyzés hello értéket hello okozó **jelszó** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="19ec3-212">Select hello **Show Password** check box, and then note hello value that was generated in hello **Password** box.</span></span>

    <span data-ttu-id="19ec3-213">d.</span><span class="sxs-lookup"><span data-stu-id="19ec3-213">d.</span></span> <span data-ttu-id="19ec3-214">Kattintson a **Létrehozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="19ec3-214">Select **Create**.</span></span>
 
### <a name="create-a-cezanne-hr-software-test-user"></a><span data-ttu-id="19ec3-215">Cezanne HR szoftver tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="19ec3-215">Create a Cezanne HR software test user</span></span>

<span data-ttu-id="19ec3-216">az Azure AD tooenable felhasználók toosign tooCezanne HR szoftver, akkor ki kell építenie Cezanne HR szoftver.</span><span class="sxs-lookup"><span data-stu-id="19ec3-216">tooenable Azure AD users toosign in tooCezanne HR software, they must be provisioned into Cezanne HR software.</span></span> <span data-ttu-id="19ec3-217">Cezanne HR szoftver hello esetben egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="19ec3-217">In hello case of Cezanne HR software, provisioning is a manual task.</span></span>

<span data-ttu-id="19ec3-218">A felhasználói fiók kiépítése hello következő tevékenységek végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="19ec3-218">Provision a user account by doing hello following:</span></span>

1.  <span data-ttu-id="19ec3-219">Jelentkezzen be tooyour Cezanne HR szoftver vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="19ec3-219">Sign in tooyour Cezanne HR software company site as an administrator.</span></span>

2.  <span data-ttu-id="19ec3-220">Hello bal oldali ablaktáblában jelöljön ki **rendszerbeállítás** > **felhasználók kezelése** > **új felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="19ec3-220">In hello left pane, select **System Setup** > **Manage Users** > **Add New User**.</span></span>

    <span data-ttu-id="19ec3-221">![hello "Az új felhasználó hozzáadása" hivatkozást](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_005.png "új felhasználó")</span><span class="sxs-lookup"><span data-stu-id="19ec3-221">![hello "Add New User" link](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_005.png "New User")</span></span>

3.  <span data-ttu-id="19ec3-222">A **személy adatai**, a következő hello:</span><span class="sxs-lookup"><span data-stu-id="19ec3-222">Under **Person Details**, do hello following:</span></span>

    <span data-ttu-id="19ec3-223">![hello "Személy részletei" szakasz](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_006.png "új felhasználó")</span><span class="sxs-lookup"><span data-stu-id="19ec3-223">![hello "Person Details" section](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_006.png "New User")</span></span>
    
    <span data-ttu-id="19ec3-224">a.</span><span class="sxs-lookup"><span data-stu-id="19ec3-224">a.</span></span> <span data-ttu-id="19ec3-225">Állítsa be **belső felhasználói** , **OFF**.</span><span class="sxs-lookup"><span data-stu-id="19ec3-225">Set **Internal User** as **OFF**.</span></span>
    
    <span data-ttu-id="19ec3-226">b.</span><span class="sxs-lookup"><span data-stu-id="19ec3-226">b.</span></span> <span data-ttu-id="19ec3-227">A hello **Utónév** mezőbe, a típus hello felhasználó utónevét, például **Britta**.</span><span class="sxs-lookup"><span data-stu-id="19ec3-227">In hello **First Name** box, type hello user's first name, for example, **Britta**.</span></span>  
 
    <span data-ttu-id="19ec3-228">c.</span><span class="sxs-lookup"><span data-stu-id="19ec3-228">c.</span></span> <span data-ttu-id="19ec3-229">A hello **Vezetéknév** mezőbe, a típus hello felhasználó vezetéknevét, például **Simon**.</span><span class="sxs-lookup"><span data-stu-id="19ec3-229">In hello **Last Name** box, type hello user's last name, for example, **Simon**.</span></span>
    
    <span data-ttu-id="19ec3-230">d.</span><span class="sxs-lookup"><span data-stu-id="19ec3-230">d.</span></span> <span data-ttu-id="19ec3-231">A hello **E-mail** mezőbe írja be a hello a felhasználó e-mail címét, például Brittasimon@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="19ec3-231">In hello **E-mail** box, type hello user's email address, for example, Brittasimon@contoso.com.</span></span>

4.  <span data-ttu-id="19ec3-232">A **fiókadatok**, a következő hello:</span><span class="sxs-lookup"><span data-stu-id="19ec3-232">Under **Account Information**, do hello following:</span></span>

    <span data-ttu-id="19ec3-233">![hello "Fiók információk" című](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_007.png "új felhasználó")</span><span class="sxs-lookup"><span data-stu-id="19ec3-233">![hello "Account Information" section](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_007.png "New User")</span></span>
    
    <span data-ttu-id="19ec3-234">a.</span><span class="sxs-lookup"><span data-stu-id="19ec3-234">a.</span></span> <span data-ttu-id="19ec3-235">A hello **felhasználónév** mezőbe írja be a hello a felhasználó e-mail címét, például Brittasimon@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="19ec3-235">In hello **Username** box, type hello user's email address, for example, Brittasimon@contoso.com.</span></span>
    
    <span data-ttu-id="19ec3-236">b.</span><span class="sxs-lookup"><span data-stu-id="19ec3-236">b.</span></span> <span data-ttu-id="19ec3-237">A hello **jelszó** mezőbe írja be a hello felhasználó jelszavát.</span><span class="sxs-lookup"><span data-stu-id="19ec3-237">In hello **Password** box, type hello user's password.</span></span>
    
    <span data-ttu-id="19ec3-238">c.</span><span class="sxs-lookup"><span data-stu-id="19ec3-238">c.</span></span> <span data-ttu-id="19ec3-239">A hello **biztonsági szerepkör** mezőben válassza **HR Professional**.</span><span class="sxs-lookup"><span data-stu-id="19ec3-239">In hello **Security Role** box, select **HR Professional**.</span></span>
    
    <span data-ttu-id="19ec3-240">d.</span><span class="sxs-lookup"><span data-stu-id="19ec3-240">d.</span></span> <span data-ttu-id="19ec3-241">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="19ec3-241">Select **OK**.</span></span>

5. <span data-ttu-id="19ec3-242">A hello **egyszeri bejelentkezés** lap hello **SAML 2.0 azonosítók** szakaszban jelölje be **új hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="19ec3-242">On hello **Single sign-on** tab, in hello **SAML 2.0 Identifiers** section, select **Add New**.</span></span>

    <span data-ttu-id="19ec3-243">![hello "Új hozzáadása" gombra](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_008.png "felhasználó")</span><span class="sxs-lookup"><span data-stu-id="19ec3-243">![hello "Add New" button](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_008.png "User")</span></span>

6. <span data-ttu-id="19ec3-244">A hello **identitásszolgáltató** listán, válassza ki az identitásszolgáltató.</span><span class="sxs-lookup"><span data-stu-id="19ec3-244">In hello **Identity Provider** list box, select your identity provider.</span></span> <span data-ttu-id="19ec3-245">A hello **felhasználói azonosító** mezőbe írja be a címre küldi hello teszt felhasználó Britta Simon fiók.</span><span class="sxs-lookup"><span data-stu-id="19ec3-245">In hello **User Identifier** box, enter hello email address for test user Britta Simon's account.</span></span>

    <span data-ttu-id="19ec3-246">!["Identitásszolgáltató" és "Felhasználói azonosítója" mezőben hello](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_009.png "felhasználó")</span><span class="sxs-lookup"><span data-stu-id="19ec3-246">![hello "Identity Provider" and "User Identifier" boxes](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_009.png "User")</span></span>
    
7. <span data-ttu-id="19ec3-247">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="19ec3-247">Select **Save**.</span></span>

    <span data-ttu-id="19ec3-248">!["a Mentés" gombra hello](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_010.png "felhasználó")</span><span class="sxs-lookup"><span data-stu-id="19ec3-248">![hello "Save" button](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_010.png "User")</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="19ec3-249">Rendelje hozzá az Azure AD hello tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="19ec3-249">Assign hello Azure AD test user</span></span>

<span data-ttu-id="19ec3-250">Ebben a szakaszban a tesztfelhasználó Britta Simon toouse Azure SSO hozzáférés tooCezanne HR szoftver megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="19ec3-250">In this section, you enable test user Britta Simon toouse Azure SSO by granting access tooCezanne HR software.</span></span>

![Felhasználói hozzáférés tesztelése][200] 

1. <span data-ttu-id="19ec3-252">Hello Azure-portálon, a hello alkalmazások nézet megnyitásához, és folytassa a toohello könyvtár nézetben.</span><span class="sxs-lookup"><span data-stu-id="19ec3-252">In hello Azure portal, open hello applications view and then go toohello directory view.</span></span> <span data-ttu-id="19ec3-253">Válassza ki **vállalati alkalmazások** > **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="19ec3-253">Select **Enterprise applications** > **All applications**.</span></span>

    !["az összes alkalmazások" Hello][201] 

2. <span data-ttu-id="19ec3-255">Hello alkalmazások listában válassza ki a **Cezanne HR szoftver**.</span><span class="sxs-lookup"><span data-stu-id="19ec3-255">In hello applications list, select **Cezanne HR Software**.</span></span>

    ![hello "Alkalmazás" lista](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_app.png) 

3. <span data-ttu-id="19ec3-257">Hello hello bal oldali menüben válasszon ki **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="19ec3-257">In hello menu on hello left, select **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="19ec3-259">Válassza a **Hozzáadás** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="19ec3-259">Select **Add**.</span></span> <span data-ttu-id="19ec3-260">Ezt a hello **hozzáadása hozzárendelés** párbeszédpanelen jelölje ki **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="19ec3-260">Then in hello **Add Assignment** dialog box, select **Users and groups**.</span></span>

    !["Felhasználók és csoportok" hivatkozásra][203]

5. <span data-ttu-id="19ec3-262">A hello **felhasználók és csoportok** párbeszédpanel hello **felhasználók** listáról válassza ki **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="19ec3-262">In hello **Users and groups** dialog box, in hello **Users** list, select **Britta Simon**.</span></span>

6. <span data-ttu-id="19ec3-263">A hello **felhasználók és csoportok** párbeszédpanelen jelölje ki **válasszon**.</span><span class="sxs-lookup"><span data-stu-id="19ec3-263">In hello **Users and groups** dialog box, select **Select**.</span></span>

7. <span data-ttu-id="19ec3-264">A hello **hozzáadása hozzárendelés** párbeszédpanelen jelölje ki **hozzárendelése**.</span><span class="sxs-lookup"><span data-stu-id="19ec3-264">In hello **Add Assignment** dialog box, select **Assign**.</span></span>
    
### <a name="test-sso"></a><span data-ttu-id="19ec3-265">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="19ec3-265">Test SSO</span></span>

<span data-ttu-id="19ec3-266">Ebben a szakaszban az Azure AD SSO konfigurációs hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="19ec3-266">In this section, you test your Azure AD SSO configuration by using hello Access Panel.</span></span>

<span data-ttu-id="19ec3-267">A hozzáférési Panel hello kiválasztásakor hello Cezanne HR szoftver csempe bejelentkezéskor automatikusan tooyour Cezanne HR alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="19ec3-267">When you select hello Cezanne HR software tile in hello Access Panel, you sign on automatically tooyour Cezanne HR software application.</span></span>

## <a name="next-steps"></a><span data-ttu-id="19ec3-268">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="19ec3-268">Next steps</span></span>

* [<span data-ttu-id="19ec3-269">Hogyan kapcsolatos bemutatók felsorolása toointegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="19ec3-269">List of tutorials on how toointegrate SaaS apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="19ec3-270">Mi az az alkalmazás-hozzáférés és az egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="19ec3-270">What is application access and SSO with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_203.png

