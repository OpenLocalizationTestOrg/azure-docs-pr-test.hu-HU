---
title: "Oktatóanyag: Azure Active Directory-integráció Cezanne HR szoftverrel |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Cezanne HR szoftverek között."
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
ms.openlocfilehash: 623c438edfce5f98c2d32d8bb25a97d86aa77909
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="tutorial-integrate-azure-active-directory-with-cezanne-hr-software"></a><span data-ttu-id="eafa5-103">Oktatóanyag: Azure Active Directory integrálása Cezanne HR szoftver</span><span class="sxs-lookup"><span data-stu-id="eafa5-103">Tutorial: Integrate Azure Active Directory with Cezanne HR software</span></span>

<span data-ttu-id="eafa5-104">Ebben az oktatóanyagban elsajátíthatja Cezanne HR szoftver integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="eafa5-104">In this tutorial, you learn how to integrate Cezanne HR software with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="eafa5-105">Cezanne HR szoftver integrálása az Azure AD a következő előnyt biztosít.</span><span class="sxs-lookup"><span data-stu-id="eafa5-105">Integrating Cezanne HR software with Azure AD provides you with the following benefits.</span></span> <span data-ttu-id="eafa5-106">A következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="eafa5-106">You can:</span></span>

- <span data-ttu-id="eafa5-107">Szabályozhatja az Azure AD, aki hozzáfér Cezanne HR szoftver.</span><span class="sxs-lookup"><span data-stu-id="eafa5-107">Control in Azure AD who has access to Cezanne HR software.</span></span>
- <span data-ttu-id="eafa5-108">Lehetővé teszi a felhasználók, az egyszeri bejelentkezés (SSO) és az Azure AD-fiókok Cezanne HR szoftver automatikusan bejelentkezhet.</span><span class="sxs-lookup"><span data-stu-id="eafa5-108">Enable your users to automatically sign in to Cezanne HR software with single sign-on (SSO) with their Azure AD accounts.</span></span>
- <span data-ttu-id="eafa5-109">A fiók egyetlen központi helyen kezelheti: az Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="eafa5-109">Manage your accounts in one central location: the Azure portal.</span></span>

<span data-ttu-id="eafa5-110">További információért, egy szolgáltatott szoftverként (SaaS) alkalmazás integráció az Azure ad-vel kapcsolatban lásd: [Mi az az alkalmazás-hozzáférés és az egyszeri bejelentkezés az Azure Active Directoryval?](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="eafa5-110">To learn more about software as a service (SaaS) app integration with Azure AD, see [What is application access and SSO with Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="eafa5-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="eafa5-111">Prerequisites</span></span>

<span data-ttu-id="eafa5-112">Az Azure AD-integráció konfigurálása Cezanne HR szoftverrel, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="eafa5-112">To configure Azure AD integration with Cezanne HR software, you need the following items:</span></span>

- <span data-ttu-id="eafa5-113">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="eafa5-113">An Azure AD subscription</span></span>
- <span data-ttu-id="eafa5-114">Egy Cezanne HR szoftver előfizetés SSO engedélyezése</span><span class="sxs-lookup"><span data-stu-id="eafa5-114">A Cezanne HR software SSO-enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="eafa5-115">Ez az oktatóanyag lépéseit teszteléséhez azt javasoljuk, hogy nem használ egy éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="eafa5-115">To test the steps in this tutorial, we recommend that you do not use a production environment.</span></span>

<span data-ttu-id="eafa5-116">Ez az oktatóanyag lépéseit teszteléséhez hajtsa végre az ezek az ajánlások:</span><span class="sxs-lookup"><span data-stu-id="eafa5-116">To test the steps in this tutorial, follow these recommendations:</span></span>

- <span data-ttu-id="eafa5-117">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="eafa5-117">Don't use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="eafa5-118">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="eafa5-118">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="eafa5-119">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="eafa5-119">Scenario description</span></span>
<span data-ttu-id="eafa5-120">Ebben az oktatóanyagban a Azure AD SSO tesztkörnyezetben tesztelni.</span><span class="sxs-lookup"><span data-stu-id="eafa5-120">In this tutorial, you test Azure AD SSO in a test environment.</span></span> 

<span data-ttu-id="eafa5-121">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="eafa5-121">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

* <span data-ttu-id="eafa5-122">A gyűjteményből Cezanne HR szoftver hozzáadása</span><span class="sxs-lookup"><span data-stu-id="eafa5-122">Adding Cezanne HR software from the gallery</span></span>
* <span data-ttu-id="eafa5-123">Azure AD egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="eafa5-123">Configuring and testing Azure AD SSO</span></span>

## <a name="add-cezanne-hr-software-from-the-gallery"></a><span data-ttu-id="eafa5-124">A gyűjteményből Cezanne HR szoftver hozzáadása</span><span class="sxs-lookup"><span data-stu-id="eafa5-124">Add Cezanne HR software from the gallery</span></span>
<span data-ttu-id="eafa5-125">Az Azure AD integrálása a Cezanne HR szoftver konfigurálásához Cezanne HR szoftver hozzáadása a gyűjteményből a kezelt SaaS-alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="eafa5-125">To configure the integration of Cezanne HR software into Azure AD, add Cezanne HR software from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="eafa5-126">Cezanne HR hozzáadása a gyűjteményből, tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="eafa5-126">To add Cezanne HR software from the gallery, do the following:</span></span>

1. <span data-ttu-id="eafa5-127">Az a  **[Azure-portálon](https://portal.azure.com)**, a bal oldali panelen válassza ki a **Azure Active Directory** gombra.</span><span class="sxs-lookup"><span data-stu-id="eafa5-127">In the **[Azure portal](https://portal.azure.com)**, in the left pane, select the **Azure Active Directory** button.</span></span> 

    ![Az "Azure Active Directory" gomb][1]

2. <span data-ttu-id="eafa5-129">Válassza ki **vállalati alkalmazások** > **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="eafa5-129">Select **Enterprise applications** > **All applications**.</span></span>

    ![Az "Összes alkalmazás" hivatkozásra][2]
    
3. <span data-ttu-id="eafa5-131">Egy új alkalmazás hozzáadása tetején a **összes alkalmazás** párbeszédpanelen jelölje ki **új alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="eafa5-131">To add a new application, at the top of the **All applications** dialog box, select **New application**.</span></span>

    ![Az "új alkalmazás" gomb][3]

4. <span data-ttu-id="eafa5-133">Írja be a keresőmezőbe, **Cezanne HR szoftver**.</span><span class="sxs-lookup"><span data-stu-id="eafa5-133">In the search box, type **Cezanne HR Software**.</span></span>

    ![A keresési mezőbe](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_search.png)

5. <span data-ttu-id="eafa5-135">Az eredmények listájában válassza **Cezanne HR szoftver** , és válassza a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="eafa5-135">In the results list, select **Cezanne HR Software** and then select the **Add** button to add the application.</span></span>

    ![Az eredmények listájában](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="eafa5-137">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="eafa5-137">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="eafa5-138">Ebben a szakaszban, tesztelése és konfigurálása Azure AD SSO "Britta Simon." nevű tesztfelhasználó alapján Cezanne HR szoftverrel</span><span class="sxs-lookup"><span data-stu-id="eafa5-138">In this section, you configure and test Azure AD SSO with Cezanne HR software based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="eafa5-139">Az egyszeri bejelentkezés működjön az Azure AD tudnia kell, az Azure AD-felhasználó a Cezanne HR szoftvere.</span><span class="sxs-lookup"><span data-stu-id="eafa5-139">For SSO to work, Azure AD needs to know the Cezanne HR software counterpart to the Azure AD user.</span></span> <span data-ttu-id="eafa5-140">Ez azt jelenti a Cezanne HR szoftver egy Azure AD-felhasználó és a kapcsolódó felhasználó közötti kapcsolat kapcsolatot kell létesítenie.</span><span class="sxs-lookup"><span data-stu-id="eafa5-140">In other words, you must establish a link relationship between an Azure AD user and the related user in the Cezanne HR software.</span></span>

<span data-ttu-id="eafa5-141">A hivatkozás kapcsolatot létesíteni, rendelje hozzá a Cezanne HR szoftver **felhasználónév** érték, mint az Azure AD **felhasználónév** érték.</span><span class="sxs-lookup"><span data-stu-id="eafa5-141">To establish the link relationship, assign the Cezanne HR software **user name** value as the Azure AD **Username** value.</span></span>

<span data-ttu-id="eafa5-142">Konfigurálása és tesztelése az Azure AD SSO Cezanne HR szoftvernek a használatával végezze el a következő építőelemeit.</span><span class="sxs-lookup"><span data-stu-id="eafa5-142">To configure and test Azure AD SSO by using Cezanne HR software, complete the following building blocks.</span></span>

### <a name="configure-azure-ad-sso"></a><span data-ttu-id="eafa5-143">Az Azure AD-egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="eafa5-143">Configure Azure AD SSO</span></span>

<span data-ttu-id="eafa5-144">Ebben a szakaszban engedélyezze az Azure AD egyszeri Bejelentkezést az Azure portálon, és egyszeri bejelentkezés konfigurálása a Cezanne HR alkalmazás a következő módon:</span><span class="sxs-lookup"><span data-stu-id="eafa5-144">In this section, you can enable Azure AD SSO in the Azure portal and configure SSO in your Cezanne HR software application by doing the following:</span></span>

1. <span data-ttu-id="eafa5-145">Az Azure portálon a a **Cezanne HR szoftver** alkalmazás integrációs lapon jelölje be **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="eafa5-145">In the Azure portal, on the **Cezanne HR Software** application integration page, select **Single sign-on**.</span></span>

    ![Az "Egyszeri bejelentkezés" parancs][4]

2. <span data-ttu-id="eafa5-147">Is engedélyezhető az egyszeri bejelentkezés, a **egyszeri bejelentkezés** párbeszédpanelen jelölje ki a **mód** , **SAML-alapú bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="eafa5-147">To enable SSO, in the **Single sign-on** dialog box, select the **Mode** as **SAML-based Sign-on**.</span></span>
 
    ![A "Mód" mezőt](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_samlbase.png)

3. <span data-ttu-id="eafa5-149">A **Cezanne HR szoftver tartomány és az URL-címek**, tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="eafa5-149">Under **Cezanne HR Software Domain and URLs**, do the following:</span></span>

    ![A "Cezanne HR szoftver tartományi és URL-címek" szakasz](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_url.png)

    <span data-ttu-id="eafa5-151">a.</span><span class="sxs-lookup"><span data-stu-id="eafa5-151">a.</span></span> <span data-ttu-id="eafa5-152">Az a **bejelentkezési URL-cím** mezőbe írjon be egy URL-címet, amely rendelkezik a következő szintaxist:`https://w3.cezanneondemand.com/cezannehr/-/<tenant id>`</span><span class="sxs-lookup"><span data-stu-id="eafa5-152">In the **Sign-on URL** box, type a URL that has the following syntax: `https://w3.cezanneondemand.com/cezannehr/-/<tenant id>`</span></span>

    <span data-ttu-id="eafa5-153">b.</span><span class="sxs-lookup"><span data-stu-id="eafa5-153">b.</span></span> <span data-ttu-id="eafa5-154">Az a **válasz URL-CÍMEN** mezőbe írjon be egy URL-címet, amely rendelkezik a következő szintaxist:`https://w3.cezanneondemand.com:443/<tenantid>`</span><span class="sxs-lookup"><span data-stu-id="eafa5-154">In the **Reply URL** box, type a URL that has the following syntax: `https://w3.cezanneondemand.com:443/<tenantid>`</span></span>    
     
    > [!NOTE] 
    > <span data-ttu-id="eafa5-155">Az előző értékei nem valódi.</span><span class="sxs-lookup"><span data-stu-id="eafa5-155">The preceding values are not real.</span></span> <span data-ttu-id="eafa5-156">Frissítse azokat a tényleges válasz URL-cím és a bejelentkezési URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="eafa5-156">Update them with the actual reply URL and the sign-on URL.</span></span> <span data-ttu-id="eafa5-157">Szerezze be az értékeket, lépjen kapcsolatba a [Cezanne HR szoftver ügyfél támogatási csoport](mailto:info@cezannehr.com).</span><span class="sxs-lookup"><span data-stu-id="eafa5-157">To obtain the values, contact the [Cezanne HR software client support team](mailto:info@cezannehr.com).</span></span>

4. <span data-ttu-id="eafa5-158">A **SAML-aláíró tanúsítványa**, jelölje be **tanúsítvány (Base64)**, és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="eafa5-158">Under **SAML Signing Certificate**, select **Certificate (Base64)**, and then save the certificate file on your computer.</span></span>

    ![A "SAML aláíró tanúsítvány" szakasz](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_certificate.png) 

5. <span data-ttu-id="eafa5-160">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="eafa5-160">Select **Save**.</span></span>

    ![A "Mentés" gombra](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_400.png)
    
6. <span data-ttu-id="eafa5-162">A **Cezanne HR szoftverkonfigurációt**, jelölje be **Cezanne HR szoftver konfigurálása** megnyitásához a **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="eafa5-162">Under **Cezanne HR Software Configuration**, select **Configure Cezanne HR Software** to open the **Configure sign-on** window.</span></span> <span data-ttu-id="eafa5-163">Másolás a **SAML Entitásazonosító** és **SAML-alapú egyszeri bejelentkezési szolgáltatás** URL-CÍMÉT a **rövid összefoglaló** szakasz.</span><span class="sxs-lookup"><span data-stu-id="eafa5-163">Copy the **SAML Entity ID** and **SAML Single Sign-On Service** URL from the **Quick Reference** section.</span></span>

    ![A "Cezanne HR szoftver konfiguráció" szakasz](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_configure.png) 

7. <span data-ttu-id="eafa5-165">Egy másik webes böngészőablakban jelentkezzen be rendszergazdaként a Cezanne HR szoftver bérlő.</span><span class="sxs-lookup"><span data-stu-id="eafa5-165">In a different web browser window, sign on to your Cezanne HR software tenant as an administrator.</span></span>

8. <span data-ttu-id="eafa5-166">A bal oldali panelen válassza ki a **rendszerbeállítás**.</span><span class="sxs-lookup"><span data-stu-id="eafa5-166">In the left pane, select **System Setup**.</span></span> <span data-ttu-id="eafa5-167">Válassza ki **biztonsági beállítások** > **az egyszeri bejelentkezés konfigurációs**.</span><span class="sxs-lookup"><span data-stu-id="eafa5-167">Select **Security Settings** > **Single Sign-On Configuration**.</span></span>

    ![Az "Egyszeri bejelentkezés konfiguráció" hivatkozásra](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_000.png)

9. <span data-ttu-id="eafa5-169">Az a **engedélyezése a felhasználóknak, hogy jelentkezzen be a következő egyszeri bejelentkezés (SSO) szolgáltatások** ablaktáblán válassza előbb a **SAML 2.0** jelölőnégyzetet, és válassza a **speciális konfiguráció** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="eafa5-169">In the **Allow users to log in using the following Single Sign-On (SSO) services** pane, select the **SAML 2.0** check box and select the **Advanced Configuration** option.</span></span>

    ![Egyszeri bejelentkezés szolgáltatás beállításai](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_001.png)

10. <span data-ttu-id="eafa5-171">Válassza ki **hozzáadhat új**.</span><span class="sxs-lookup"><span data-stu-id="eafa5-171">Select **Add New**.</span></span>

    ![Az "Új hozzáadása" gombra](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_002.png)

11. <span data-ttu-id="eafa5-173">A **SAML 2.0 identitás-szolgáltatóktól**, tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="eafa5-173">Under **SAML 2.0 Identity Providers**, do the following:</span></span>

    ![A "SAML 2.0 identitás-szolgáltatóktól" szakasz](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_003.png)
    
    <span data-ttu-id="eafa5-175">a.</span><span class="sxs-lookup"><span data-stu-id="eafa5-175">a.</span></span> <span data-ttu-id="eafa5-176">Az a **megjelenített név** mezőben adja meg az identitás-szolgáltató neve.</span><span class="sxs-lookup"><span data-stu-id="eafa5-176">In the **Display Name** box, enter the name of your identity provider.</span></span>

    <span data-ttu-id="eafa5-177">b.</span><span class="sxs-lookup"><span data-stu-id="eafa5-177">b.</span></span> <span data-ttu-id="eafa5-178">Az a **entitásazonosító** mezőbe illessze be a **SAML Entitásazonosító** másolt Azure-portálról.</span><span class="sxs-lookup"><span data-stu-id="eafa5-178">In the **Entity Identifier** box, paste the **SAML Entity ID** that you copied from the Azure portal.</span></span> 

    <span data-ttu-id="eafa5-179">c.</span><span class="sxs-lookup"><span data-stu-id="eafa5-179">c.</span></span> <span data-ttu-id="eafa5-180">Az a **SAML kötés** listáján jelölje ki **POST**.</span><span class="sxs-lookup"><span data-stu-id="eafa5-180">In the **SAML Binding** list box, select **POST**.</span></span>

    <span data-ttu-id="eafa5-181">d.</span><span class="sxs-lookup"><span data-stu-id="eafa5-181">d.</span></span> <span data-ttu-id="eafa5-182">Az a **biztonsági jogkivonat szolgáltatásvégpont** mezőbe illessze be a **SAML-alapú egyszeri bejelentkezési szolgáltatás** Azure-portálról másolt URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="eafa5-182">In the **Security Token Service Endpoint** box, paste the **SAML Single Sign-On Service** URL that you copied from the Azure portal.</span></span> 
    
    <span data-ttu-id="eafa5-183">e.</span><span class="sxs-lookup"><span data-stu-id="eafa5-183">e.</span></span> <span data-ttu-id="eafa5-184">Az a **felhasználói azonosító attribútum neve** adja meg a `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.</span><span class="sxs-lookup"><span data-stu-id="eafa5-184">In the **User ID Attribute Name** box, enter `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.</span></span>
    
    <span data-ttu-id="eafa5-185">f.</span><span class="sxs-lookup"><span data-stu-id="eafa5-185">f.</span></span> <span data-ttu-id="eafa5-186">Töltse fel a letöltött az Azure AD, válassza ki a **feltöltése** gombra.</span><span class="sxs-lookup"><span data-stu-id="eafa5-186">To upload the downloaded certificate from Azure AD, select the **Upload** button.</span></span>
    
    <span data-ttu-id="eafa5-187">g.</span><span class="sxs-lookup"><span data-stu-id="eafa5-187">g.</span></span> <span data-ttu-id="eafa5-188">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="eafa5-188">Select **OK**.</span></span> 

12. <span data-ttu-id="eafa5-189">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="eafa5-189">Select **Save**.</span></span>

    ![A "Mentés" gombra](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_004.png)

> [!TIP]
> <span data-ttu-id="eafa5-191">Állít be az alkalmazást, mert egy előző utasításait tömör verziója elolvashatja a [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="eafa5-191">As you set up the app, you can read a concise version of the preceding instructions in the [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="eafa5-192">Miután hozzáadta az alkalmazásból a **Active Directory** > **vállalati alkalmazások** szakaszban jelölje be a **egyszeri bejelentkezés** fülre. A beágyazott dokumentációjának majd hozzáférni a **konfigurációs** szakasz.</span><span class="sxs-lookup"><span data-stu-id="eafa5-192">After you add the app from the **Active Directory** > **Enterprise applications** section, select the **Single sign-on** tab. Then access the embedded documentation from the **Configuration** section.</span></span> 

<span data-ttu-id="eafa5-193">A beágyazott dokumentáció szolgáltatással kapcsolatos további tudnivalókért lásd: [az Azure AD dokumentációjában beágyazott]( https://go.microsoft.com/fwlink/?linkid=845985).</span><span class="sxs-lookup"><span data-stu-id="eafa5-193">To learn more about the embedded documentation feature, see [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985).</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="eafa5-194">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="eafa5-194">Create an Azure AD test user</span></span>
<span data-ttu-id="eafa5-195">Ebben a szakaszban az Azure portálon Britta Simon tesztfelhasználó hoz létre.</span><span class="sxs-lookup"><span data-stu-id="eafa5-195">In this section, you create test user Britta Simon in the Azure portal.</span></span>

![A tesztfelhasználó számára Britta Simon][100]

<span data-ttu-id="eafa5-197">Tesztfelhasználó létrehozása az Azure ad-ben, tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="eafa5-197">To create a test user in Azure AD, do the following:</span></span>

1. <span data-ttu-id="eafa5-198">Az a **Azure-portálon**, a bal oldali panelen válassza ki a **Azure Active Directory** gombra.</span><span class="sxs-lookup"><span data-stu-id="eafa5-198">In the **Azure portal**, in the left pane, select the **Azure Active Directory** button.</span></span>

    ![Az "Azure Active Directory" gomb](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="eafa5-200">Válassza ki azon felhasználók listájának megjelenítéséhez **felhasználók és csoportok** > **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="eafa5-200">To display the list of users, select **Users and groups** > **All users**.</span></span>
    
    ![A "Minden felhasználó" hivatkozásra](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_02.png) 
    
    <span data-ttu-id="eafa5-202">A **minden felhasználó** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="eafa5-202">The **All users** dialog box opens.</span></span>

3. <span data-ttu-id="eafa5-203">Lehetőségre a **felhasználói** párbeszédpanelen jelölje ki **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="eafa5-203">To open the **User** dialog box, select **Add**.</span></span>
 
    ![A "Hozzáadás" gombra](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="eafa5-205">Az a **felhasználói** párbeszédpanelen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="eafa5-205">In the **User** dialog box, do the following:</span></span>
 
    ![A "User" párbeszédpanel](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="eafa5-207">a.</span><span class="sxs-lookup"><span data-stu-id="eafa5-207">a.</span></span> <span data-ttu-id="eafa5-208">Az a **neve** mezőbe írja be **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="eafa5-208">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="eafa5-209">b.</span><span class="sxs-lookup"><span data-stu-id="eafa5-209">b.</span></span> <span data-ttu-id="eafa5-210">Az a **felhasználónév** mezőbe írja be a felhasználó Britta Simon **e-mail cím**.</span><span class="sxs-lookup"><span data-stu-id="eafa5-210">In the **User name** box, type user Britta Simon's **email address**.</span></span>

    <span data-ttu-id="eafa5-211">c.</span><span class="sxs-lookup"><span data-stu-id="eafa5-211">c.</span></span> <span data-ttu-id="eafa5-212">Válassza ki a **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel az értéket, amelyet hozták létre a **jelszó** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="eafa5-212">Select the **Show Password** check box, and then note the value that was generated in the **Password** box.</span></span>

    <span data-ttu-id="eafa5-213">d.</span><span class="sxs-lookup"><span data-stu-id="eafa5-213">d.</span></span> <span data-ttu-id="eafa5-214">Kattintson a **Létrehozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="eafa5-214">Select **Create**.</span></span>
 
### <a name="create-a-cezanne-hr-software-test-user"></a><span data-ttu-id="eafa5-215">Cezanne HR szoftver tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="eafa5-215">Create a Cezanne HR software test user</span></span>

<span data-ttu-id="eafa5-216">Ahhoz, hogy az Azure AD-felhasználók Cezanne HR szoftver bejelentkezni, akkor ki kell építenie Cezanne HR szoftver.</span><span class="sxs-lookup"><span data-stu-id="eafa5-216">To enable Azure AD users to sign in to Cezanne HR software, they must be provisioned into Cezanne HR software.</span></span> <span data-ttu-id="eafa5-217">Cezanne HR szoftvert, ha egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="eafa5-217">In the case of Cezanne HR software, provisioning is a manual task.</span></span>

<span data-ttu-id="eafa5-218">A felhasználói fiók kiépítése a következő módon:</span><span class="sxs-lookup"><span data-stu-id="eafa5-218">Provision a user account by doing the following:</span></span>

1.  <span data-ttu-id="eafa5-219">Jelentkezzen be rendszergazdaként a Cezanne HR szoftver vállalati webhelyre.</span><span class="sxs-lookup"><span data-stu-id="eafa5-219">Sign in to your Cezanne HR software company site as an administrator.</span></span>

2.  <span data-ttu-id="eafa5-220">A bal oldali panelen válassza ki a **rendszerbeállítás** > **felhasználók kezelése** > **új felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="eafa5-220">In the left pane, select **System Setup** > **Manage Users** > **Add New User**.</span></span>

    <span data-ttu-id="eafa5-221">![Az "Új felhasználó hozzáadása" hivatkozást](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_005.png "új felhasználó")</span><span class="sxs-lookup"><span data-stu-id="eafa5-221">![The "Add New User" link](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_005.png "New User")</span></span>

3.  <span data-ttu-id="eafa5-222">A **személy adatai**, tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="eafa5-222">Under **Person Details**, do the following:</span></span>

    <span data-ttu-id="eafa5-223">![A "Személy részletei" szakasz](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_006.png "új felhasználó")</span><span class="sxs-lookup"><span data-stu-id="eafa5-223">![The "Person Details" section](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_006.png "New User")</span></span>
    
    <span data-ttu-id="eafa5-224">a.</span><span class="sxs-lookup"><span data-stu-id="eafa5-224">a.</span></span> <span data-ttu-id="eafa5-225">Állítsa be **belső felhasználói** , **OFF**.</span><span class="sxs-lookup"><span data-stu-id="eafa5-225">Set **Internal User** as **OFF**.</span></span>
    
    <span data-ttu-id="eafa5-226">b.</span><span class="sxs-lookup"><span data-stu-id="eafa5-226">b.</span></span> <span data-ttu-id="eafa5-227">Az a **Utónév** mezőbe írja be például a felhasználó utónevét, **Britta**.</span><span class="sxs-lookup"><span data-stu-id="eafa5-227">In the **First Name** box, type the user's first name, for example, **Britta**.</span></span>  
 
    <span data-ttu-id="eafa5-228">c.</span><span class="sxs-lookup"><span data-stu-id="eafa5-228">c.</span></span> <span data-ttu-id="eafa5-229">Az a **Vezetéknév** mezőbe írja be például a felhasználó vezetékneve **Simon**.</span><span class="sxs-lookup"><span data-stu-id="eafa5-229">In the **Last Name** box, type the user's last name, for example, **Simon**.</span></span>
    
    <span data-ttu-id="eafa5-230">d.</span><span class="sxs-lookup"><span data-stu-id="eafa5-230">d.</span></span> <span data-ttu-id="eafa5-231">Az a **E-mail** mezőbe írja be a felhasználó e-mail címét, például Brittasimon@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="eafa5-231">In the **E-mail** box, type the user's email address, for example, Brittasimon@contoso.com.</span></span>

4.  <span data-ttu-id="eafa5-232">A **fiókadatok**, tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="eafa5-232">Under **Account Information**, do the following:</span></span>

    <span data-ttu-id="eafa5-233">![A "Fiók információk" című](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_007.png "új felhasználó")</span><span class="sxs-lookup"><span data-stu-id="eafa5-233">![The "Account Information" section](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_007.png "New User")</span></span>
    
    <span data-ttu-id="eafa5-234">a.</span><span class="sxs-lookup"><span data-stu-id="eafa5-234">a.</span></span> <span data-ttu-id="eafa5-235">Az a **felhasználónév** mezőbe írja be a felhasználó e-mail címét, például Brittasimon@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="eafa5-235">In the **Username** box, type the user's email address, for example, Brittasimon@contoso.com.</span></span>
    
    <span data-ttu-id="eafa5-236">b.</span><span class="sxs-lookup"><span data-stu-id="eafa5-236">b.</span></span> <span data-ttu-id="eafa5-237">Az a **jelszó** mezőbe írja be a jelszót.</span><span class="sxs-lookup"><span data-stu-id="eafa5-237">In the **Password** box, type the user's password.</span></span>
    
    <span data-ttu-id="eafa5-238">c.</span><span class="sxs-lookup"><span data-stu-id="eafa5-238">c.</span></span> <span data-ttu-id="eafa5-239">Az a **biztonsági szerepkör** mezőben válassza **HR Professional**.</span><span class="sxs-lookup"><span data-stu-id="eafa5-239">In the **Security Role** box, select **HR Professional**.</span></span>
    
    <span data-ttu-id="eafa5-240">d.</span><span class="sxs-lookup"><span data-stu-id="eafa5-240">d.</span></span> <span data-ttu-id="eafa5-241">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="eafa5-241">Select **OK**.</span></span>

5. <span data-ttu-id="eafa5-242">Az a **egyszeri bejelentkezés** lap a **SAML 2.0 azonosítók** szakaszban jelölje be **új hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="eafa5-242">On the **Single sign-on** tab, in the **SAML 2.0 Identifiers** section, select **Add New**.</span></span>

    <span data-ttu-id="eafa5-243">![Az "Új hozzáadása" gombra](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_008.png "felhasználó")</span><span class="sxs-lookup"><span data-stu-id="eafa5-243">![The "Add New" button](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_008.png "User")</span></span>

6. <span data-ttu-id="eafa5-244">Az a **identitásszolgáltató** listán, válassza ki az identitásszolgáltató.</span><span class="sxs-lookup"><span data-stu-id="eafa5-244">In the **Identity Provider** list box, select your identity provider.</span></span> <span data-ttu-id="eafa5-245">Az a **felhasználói azonosító** mezőbe írja be az e-mail cím teszt felhasználó Britta Simon fiók.</span><span class="sxs-lookup"><span data-stu-id="eafa5-245">In the **User Identifier** box, enter the email address for test user Britta Simon's account.</span></span>

    <span data-ttu-id="eafa5-246">![A "Identitásszolgáltató" és "Felhasználói azonosítója" mezőben](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_009.png "felhasználó")</span><span class="sxs-lookup"><span data-stu-id="eafa5-246">![The "Identity Provider" and "User Identifier" boxes](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_009.png "User")</span></span>
    
7. <span data-ttu-id="eafa5-247">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="eafa5-247">Select **Save**.</span></span>

    <span data-ttu-id="eafa5-248">![A "Mentés" gombra](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_010.png "felhasználó")</span><span class="sxs-lookup"><span data-stu-id="eafa5-248">![The "Save" button](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_010.png "User")</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="eafa5-249">Rendelje hozzá az Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="eafa5-249">Assign the Azure AD test user</span></span>

<span data-ttu-id="eafa5-250">Ebben a szakaszban Azure SSO Cezanne HR szoftver való hozzáférés biztosítása által használandó Britta Simon tesztfelhasználó engedélyezi.</span><span class="sxs-lookup"><span data-stu-id="eafa5-250">In this section, you enable test user Britta Simon to use Azure SSO by granting access to Cezanne HR software.</span></span>

![Felhasználói hozzáférés tesztelése][200] 

1. <span data-ttu-id="eafa5-252">Az Azure portálon az alkalmazások nézet megnyitásához, és keresse meg a könyvtár nézet.</span><span class="sxs-lookup"><span data-stu-id="eafa5-252">In the Azure portal, open the applications view and then go to the directory view.</span></span> <span data-ttu-id="eafa5-253">Válassza ki **vállalati alkalmazások** > **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="eafa5-253">Select **Enterprise applications** > **All applications**.</span></span>

    ![Az "Összes alkalmazás" hivatkozásra][201] 

2. <span data-ttu-id="eafa5-255">Az alkalmazások listában válassza ki a **Cezanne HR szoftver**.</span><span class="sxs-lookup"><span data-stu-id="eafa5-255">In the applications list, select **Cezanne HR Software**.</span></span>

    ![Az "Alkalmazás" lista](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_app.png) 

3. <span data-ttu-id="eafa5-257">A bal oldali menüben válasszon ki **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="eafa5-257">In the menu on the left, select **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="eafa5-259">Válassza a **Hozzáadás** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="eafa5-259">Select **Add**.</span></span> <span data-ttu-id="eafa5-260">Ezt a a **hozzáadása hozzárendelés** párbeszédpanelen jelölje ki **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="eafa5-260">Then in the **Add Assignment** dialog box, select **Users and groups**.</span></span>

    !["Felhasználók és csoportok" hivatkozásra][203]

5. <span data-ttu-id="eafa5-262">Az a **felhasználók és csoportok** párbeszédpanel a **felhasználók** listáról válassza ki **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="eafa5-262">In the **Users and groups** dialog box, in the **Users** list, select **Britta Simon**.</span></span>

6. <span data-ttu-id="eafa5-263">Az a **felhasználók és csoportok** párbeszédpanelen jelölje ki **válasszon**.</span><span class="sxs-lookup"><span data-stu-id="eafa5-263">In the **Users and groups** dialog box, select **Select**.</span></span>

7. <span data-ttu-id="eafa5-264">Az a **hozzáadása hozzárendelés** párbeszédpanelen jelölje ki **hozzárendelése**.</span><span class="sxs-lookup"><span data-stu-id="eafa5-264">In the **Add Assignment** dialog box, select **Assign**.</span></span>
    
### <a name="test-sso"></a><span data-ttu-id="eafa5-265">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="eafa5-265">Test SSO</span></span>

<span data-ttu-id="eafa5-266">Ebben a szakaszban az Azure AD SSO konfigurációját a hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="eafa5-266">In this section, you test your Azure AD SSO configuration by using the Access Panel.</span></span>

<span data-ttu-id="eafa5-267">Ha bejelöli a Cezanne HR szoftver csempe a hozzáférési panelen, amikor bejelentkezik automatikusan a Cezanne HR alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="eafa5-267">When you select the Cezanne HR software tile in the Access Panel, you sign on automatically to your Cezanne HR software application.</span></span>

## <a name="next-steps"></a><span data-ttu-id="eafa5-268">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="eafa5-268">Next steps</span></span>

* [<span data-ttu-id="eafa5-269">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="eafa5-269">List of tutorials on how to integrate SaaS apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="eafa5-270">Mi az az alkalmazás-hozzáférés és az egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="eafa5-270">What is application access and SSO with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

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

