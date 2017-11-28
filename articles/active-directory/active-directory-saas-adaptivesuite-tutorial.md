---
title: "Oktatóanyag: Azure Active Directory-integráció adaptív Suite |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és adaptív Suite között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 13af9d00-116a-41b8-8ca0-4870b31e224c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: jeedes
ms.openlocfilehash: af309c27ab74098c1e229c80adb11c96dc2774fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-adaptive-suite"></a><span data-ttu-id="c36ea-103">Oktatóanyag: Azure Active Directory-integráció adaptív csomaggal</span><span class="sxs-lookup"><span data-stu-id="c36ea-103">Tutorial: Azure Active Directory integration with Adaptive Suite</span></span>

<span data-ttu-id="c36ea-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate adaptív Suite az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="c36ea-104">In this tutorial, you learn how toointegrate Adaptive Suite with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="c36ea-105">Adaptív Suite integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="c36ea-105">Integrating Adaptive Suite with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="c36ea-106">Megadhatja a hozzáférés tooAdaptive csomaggal rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="c36ea-106">You can control in Azure AD who has access tooAdaptive Suite</span></span>
- <span data-ttu-id="c36ea-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooAdaptive Suite (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="c36ea-107">You can enable your users tooautomatically get signed-on tooAdaptive Suite (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="c36ea-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="c36ea-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="c36ea-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="c36ea-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c36ea-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="c36ea-110">Prerequisites</span></span>

<span data-ttu-id="c36ea-111">tooconfigure az Azure AD-integrációs adaptív csomaggal, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="c36ea-111">tooconfigure Azure AD integration with Adaptive Suite, you need hello following items:</span></span>

- <span data-ttu-id="c36ea-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="c36ea-112">An Azure AD subscription</span></span>
- <span data-ttu-id="c36ea-113">Egy adaptív Suite egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="c36ea-113">An Adaptive Suite single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="c36ea-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="c36ea-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="c36ea-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="c36ea-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="c36ea-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="c36ea-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="c36ea-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c36ea-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="c36ea-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="c36ea-118">Scenario description</span></span>
<span data-ttu-id="c36ea-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="c36ea-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="c36ea-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="c36ea-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="c36ea-121">Hello gyűjteményből adaptív csomag hozzáadása</span><span class="sxs-lookup"><span data-stu-id="c36ea-121">Adding Adaptive Suite from hello gallery</span></span>
2. <span data-ttu-id="c36ea-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="c36ea-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-adaptive-suite-from-hello-gallery"></a><span data-ttu-id="c36ea-123">Hello gyűjteményből adaptív csomag hozzáadása</span><span class="sxs-lookup"><span data-stu-id="c36ea-123">Adding Adaptive Suite from hello gallery</span></span>
<span data-ttu-id="c36ea-124">tooconfigure hello integrációja adaptív Suite az Azure AD-be, meg kell tooadd adaptív Suite hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="c36ea-124">tooconfigure hello integration of Adaptive Suite into Azure AD, you need tooadd Adaptive Suite from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="c36ea-125">**tooadd adaptív Suite hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="c36ea-125">**tooadd Adaptive Suite from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="c36ea-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="c36ea-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="c36ea-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="c36ea-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="c36ea-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="c36ea-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="c36ea-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="c36ea-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="c36ea-133">Hello keresési mezőbe, írja be a **adaptív Suite**.</span><span class="sxs-lookup"><span data-stu-id="c36ea-133">In hello search box, type **Adaptive Suite**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_adaptivesuite_search.png)

5. <span data-ttu-id="c36ea-135">A hello eredmények panelen válassza ki a **adaptív Suite**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="c36ea-135">In hello results panel, select **Adaptive Suite**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_adaptivesuite_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="c36ea-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="c36ea-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="c36ea-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján adaptív csomaggal</span><span class="sxs-lookup"><span data-stu-id="c36ea-138">In this section, you configure and test Azure AD single sign-on with Adaptive Suite based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="c36ea-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói adaptív Suite tooa felhasználói az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="c36ea-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Adaptive Suite is tooa user in Azure AD.</span></span> <span data-ttu-id="c36ea-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználói hello adaptív csomagban található hivatkozás kapcsolatának kell toobe létrejött.</span><span class="sxs-lookup"><span data-stu-id="c36ea-140">In other words, a link relationship between an Azure AD user and hello related user in Adaptive Suite needs toobe established.</span></span>

<span data-ttu-id="c36ea-141">Adaptív Suite, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="c36ea-141">In Adaptive Suite, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="c36ea-142">tooconfigure és az Azure AD az egyszeri bejelentkezés tesztelése adaptív csomaggal, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="c36ea-142">tooconfigure and test Azure AD single sign-on with Adaptive Suite, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="c36ea-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="c36ea-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="c36ea-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c36ea-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="c36ea-145">**[Egy adaptív Suite tesztfelhasználó létrehozása](#creating-an-adaptive-suite-test-user)**  -toohave egy megfelelője a Britta Simon az adaptív protokollsorozat, amelyet a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="c36ea-145">**[Creating an Adaptive Suite test user](#creating-an-adaptive-suite-test-user)** - toohave a counterpart of Britta Simon in Adaptive Suite that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="c36ea-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="c36ea-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="c36ea-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="c36ea-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="c36ea-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c36ea-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="c36ea-149">Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és az adaptív Suite alkalmazásban egyszeri bejelentkezés beállítása.</span><span class="sxs-lookup"><span data-stu-id="c36ea-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Adaptive Suite application.</span></span>

<span data-ttu-id="c36ea-150">**az Azure AD tooconfigure egyszeri bejelentkezés adaptív csomaggal, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="c36ea-150">**tooconfigure Azure AD single sign-on with Adaptive Suite, perform hello following steps:**</span></span>

1. <span data-ttu-id="c36ea-151">Az Azure portál, a hello hello **adaptív Suite** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="c36ea-151">In hello Azure portal, on hello **Adaptive Suite** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="c36ea-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="c36ea-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_adaptivesuite_samlbase.png)

3. <span data-ttu-id="c36ea-155">A hello **adaptív Suite tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="c36ea-155">On hello **Adaptive Suite Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_adaptivesuite_url.png)

    <span data-ttu-id="c36ea-157">A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://login.adaptiveinsights.com:443/samlsso/<unique-id>`</span><span class="sxs-lookup"><span data-stu-id="c36ea-157">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://login.adaptiveinsights.com:443/samlsso/<unique-id>`</span></span>

    >[!NOTE]
    > <span data-ttu-id="c36ea-158">Ez az érték letölthető hello adaptív Suite **SAML SSO beállítások** lap.</span><span class="sxs-lookup"><span data-stu-id="c36ea-158">You can get this value from hello Adaptive Suite’s **SAML SSO Settings** page.</span></span>
    >  

4. <span data-ttu-id="c36ea-159">A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="c36ea-159">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_adaptivesuite_certificate.png) 

5. <span data-ttu-id="c36ea-161">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="c36ea-161">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="c36ea-163">A hello **adaptív Suite konfigurációs** kattintson **adaptív Suite konfigurálása** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="c36ea-163">On hello **Adaptive Suite Configuration** section, click **Configure Adaptive Suite** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="c36ea-164">Másolás hello **SAML Entitásazonosító és SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="c36ea-164">Copy hello **SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_adaptivesuite_configure.png) 

7. <span data-ttu-id="c36ea-166">Egy másik webes böngészőablakban jelentkezzen tooyour adaptív Suite vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="c36ea-166">In a different web browser window, log in tooyour Adaptive Suite company site as an administrator.</span></span>

8. <span data-ttu-id="c36ea-167">Nyissa meg túl**Admin**.</span><span class="sxs-lookup"><span data-stu-id="c36ea-167">Go too**Admin**.</span></span>
   
    <span data-ttu-id="c36ea-168">![Felügyeleti](./media/active-directory-saas-adaptivesuite-tutorial/IC805644.png "rendszergazda")</span><span class="sxs-lookup"><span data-stu-id="c36ea-168">![Admin](./media/active-directory-saas-adaptivesuite-tutorial/IC805644.png "Admin")</span></span>

9. <span data-ttu-id="c36ea-169">A hello **felhasználók és szerepkörök** kattintson **SAML SSO beállítások kezelése**.</span><span class="sxs-lookup"><span data-stu-id="c36ea-169">In hello **Users and Roles** section, click **Manage SAML SSO Settings**.</span></span>
   
    <span data-ttu-id="c36ea-170">![SAML SSO beállításainak kezelése](./media/active-directory-saas-adaptivesuite-tutorial/IC805645.png "SAML SSO beállításainak kezelése")</span><span class="sxs-lookup"><span data-stu-id="c36ea-170">![Manage SAML SSO Settings](./media/active-directory-saas-adaptivesuite-tutorial/IC805645.png "Manage SAML SSO Settings")</span></span>

10. <span data-ttu-id="c36ea-171">A hello **SAML SSO beállítások** lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="c36ea-171">On hello **SAML SSO Settings** page, perform hello following steps:</span></span>
   
    <span data-ttu-id="c36ea-172">![SAML SSO beállítások](./media/active-directory-saas-adaptivesuite-tutorial/IC805646.png "SAML SSO-beállítások")</span><span class="sxs-lookup"><span data-stu-id="c36ea-172">![SAML SSO Settings](./media/active-directory-saas-adaptivesuite-tutorial/IC805646.png "SAML SSO Settings")</span></span>

    <span data-ttu-id="c36ea-173">a.</span><span class="sxs-lookup"><span data-stu-id="c36ea-173">a.</span></span> <span data-ttu-id="c36ea-174">A hello **identitás szolgáltatónevet** szövegmező, adja meg a konfiguráció nevét.</span><span class="sxs-lookup"><span data-stu-id="c36ea-174">In hello **Identity provider name** textbox, type a name for your configuration.</span></span>
    
    <span data-ttu-id="c36ea-175">b.</span><span class="sxs-lookup"><span data-stu-id="c36ea-175">b.</span></span> <span data-ttu-id="c36ea-176">Beillesztés hello **SAML Entitásazonosító** érték átkerül az Azure portálról hello **identitásszolgáltató Entitásazonosító** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="c36ea-176">Paste hello **SAML Entity ID** value copied from Azure portal into hello **Identity provider Entity ID** textbox.</span></span>
  
    <span data-ttu-id="c36ea-177">c.</span><span class="sxs-lookup"><span data-stu-id="c36ea-177">c.</span></span> <span data-ttu-id="c36ea-178">Beillesztés hello **SAML-alapú egyszeri bejelentkezési URL-címe** érték átkerül az Azure portálról hello **identitásszolgáltató egyszeri bejelentkezési URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="c36ea-178">Paste hello **SAML Single Sign-On Service URL** value copied from Azure portal into hello **Identity provider SSO URL** textbox.</span></span>
  
    <span data-ttu-id="c36ea-179">d.</span><span class="sxs-lookup"><span data-stu-id="c36ea-179">d.</span></span> <span data-ttu-id="c36ea-180">Beillesztés hello **SAML-alapú egyszeri bejelentkezési URL-címe** érték átkerül az Azure portálról hello **egyéni kijelentkezési URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="c36ea-180">Paste hello **SAML Single Sign-On Service URL** value copied from Azure portal into hello **Custom logout URL** textbox.</span></span>
  
    <span data-ttu-id="c36ea-181">e.</span><span class="sxs-lookup"><span data-stu-id="c36ea-181">e.</span></span> <span data-ttu-id="c36ea-182">tooupload a letöltött tanúsítvány kattintson **fájl kiválasztása**.</span><span class="sxs-lookup"><span data-stu-id="c36ea-182">tooupload your downloaded certificate, click **Choose file**.</span></span>
  
    <span data-ttu-id="c36ea-183">f.</span><span class="sxs-lookup"><span data-stu-id="c36ea-183">f.</span></span> <span data-ttu-id="c36ea-184">Hello következő kiválasztása:</span><span class="sxs-lookup"><span data-stu-id="c36ea-184">Select hello following, for:</span></span>
    * <span data-ttu-id="c36ea-185">**SAML-alapú felhasználói azonosító**, jelölje be **adaptív Insights felhasználónevét**.</span><span class="sxs-lookup"><span data-stu-id="c36ea-185">**SAML user id**, select **User’s Adaptive Insights user name**.</span></span>
    * <span data-ttu-id="c36ea-186">**SAML-alapú felhasználói azonosító hely**, jelölje be **felhasználóazonosító tárgyában csupa kisbetűvel NameID**.</span><span class="sxs-lookup"><span data-stu-id="c36ea-186">**SAML user id location**, select **User id in NameID of Subject**.</span></span>
    * <span data-ttu-id="c36ea-187">**SAML NameID formátum**, jelölje be **E-mail cím**.</span><span class="sxs-lookup"><span data-stu-id="c36ea-187">**SAML NameID format**, select **Email address**.</span></span>
    * <span data-ttu-id="c36ea-188">**SAML engedélyezése**, jelölje be **SAML SSO engedélyezése és a közvetlen adaptív Insights bejelentkezési**.</span><span class="sxs-lookup"><span data-stu-id="c36ea-188">**Enable SAML**, select **Allow SAML SSO and direct Adaptive Insights login**.</span></span>
    
    <span data-ttu-id="c36ea-189">g.</span><span class="sxs-lookup"><span data-stu-id="c36ea-189">g.</span></span> <span data-ttu-id="c36ea-190">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="c36ea-190">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="c36ea-191">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="c36ea-191">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="c36ea-192">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="c36ea-192">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="c36ea-193">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="c36ea-193">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="c36ea-194">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="c36ea-194">Creating an Azure AD test user</span></span>
<span data-ttu-id="c36ea-195">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="c36ea-195">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="c36ea-197">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="c36ea-197">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="c36ea-198">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="c36ea-198">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adaptivesuite-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="c36ea-200">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="c36ea-200">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adaptivesuite-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="c36ea-202">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c36ea-202">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adaptivesuite-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="c36ea-204">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="c36ea-204">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adaptivesuite-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="c36ea-206">a.</span><span class="sxs-lookup"><span data-stu-id="c36ea-206">a.</span></span> <span data-ttu-id="c36ea-207">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="c36ea-207">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="c36ea-208">b.</span><span class="sxs-lookup"><span data-stu-id="c36ea-208">b.</span></span> <span data-ttu-id="c36ea-209">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="c36ea-209">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="c36ea-210">c.</span><span class="sxs-lookup"><span data-stu-id="c36ea-210">c.</span></span> <span data-ttu-id="c36ea-211">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="c36ea-211">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="c36ea-212">d.</span><span class="sxs-lookup"><span data-stu-id="c36ea-212">d.</span></span> <span data-ttu-id="c36ea-213">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="c36ea-213">Click **Create**.</span></span>
 
### <a name="creating-an-adaptive-suite-test-user"></a><span data-ttu-id="c36ea-214">Egy adaptív Suite tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="c36ea-214">Creating an Adaptive Suite test user</span></span>

<span data-ttu-id="c36ea-215">az Azure AD tooenable felhasználók toolog tooAdaptive Suite, a azok ki kell építenie az adaptív Suite.</span><span class="sxs-lookup"><span data-stu-id="c36ea-215">tooenable Azure AD users toolog in tooAdaptive Suite, they must be provisioned into Adaptive Suite.</span></span>  

* <span data-ttu-id="c36ea-216">Adaptív Suite hello esetben egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="c36ea-216">In hello case of Adaptive Suite, provisioning is a manual task.</span></span>

<span data-ttu-id="c36ea-217">**tooconfigure felhasználók átadásához, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="c36ea-217">**tooconfigure user provisioning, perform hello following steps:**</span></span> 

1. <span data-ttu-id="c36ea-218">Jelentkezzen be tooyour **adaptív Suite** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="c36ea-218">Log in tooyour **Adaptive Suite** company site as an administrator.</span></span>
2. <span data-ttu-id="c36ea-219">Nyissa meg túl**Admin**.</span><span class="sxs-lookup"><span data-stu-id="c36ea-219">Go too**Admin**.</span></span>
   
   <span data-ttu-id="c36ea-220">![Felügyeleti](./media/active-directory-saas-adaptivesuite-tutorial/IC805644.png "rendszergazda")</span><span class="sxs-lookup"><span data-stu-id="c36ea-220">![Admin](./media/active-directory-saas-adaptivesuite-tutorial/IC805644.png "Admin")</span></span>
3. <span data-ttu-id="c36ea-221">A hello **felhasználók és szerepkörök** kattintson **felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="c36ea-221">In hello **Users and Roles** section, click **Add User**.</span></span>
   
   <span data-ttu-id="c36ea-222">![Felhasználó hozzáadása](./media/active-directory-saas-adaptivesuite-tutorial/IC805648.png "felhasználó hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="c36ea-222">![Add User](./media/active-directory-saas-adaptivesuite-tutorial/IC805648.png "Add User")</span></span>
4. <span data-ttu-id="c36ea-223">A hello **új felhasználó** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="c36ea-223">In hello **New User** section, perform hello following steps:</span></span>
   
   <span data-ttu-id="c36ea-224">![Küldje el](./media/active-directory-saas-adaptivesuite-tutorial/IC805649.png "elküldése")</span><span class="sxs-lookup"><span data-stu-id="c36ea-224">![Submit](./media/active-directory-saas-adaptivesuite-tutorial/IC805649.png "Submit")</span></span>   

   <span data-ttu-id="c36ea-225">a.</span><span class="sxs-lookup"><span data-stu-id="c36ea-225">a.</span></span> <span data-ttu-id="c36ea-226">Típus hello **neve**, **bejelentkezési**, **E-mail**, **jelszó** egy érvényes Azure Active Directory-felhasználó a kívánt tooprovision kapcsolódó hello szövegmezőből.</span><span class="sxs-lookup"><span data-stu-id="c36ea-226">Type hello **Name**, **Login**, **Email**, **Password** of a valid Azure Active Directory user you want tooprovision into hello related textboxes.</span></span>
  
   <span data-ttu-id="c36ea-227">b.</span><span class="sxs-lookup"><span data-stu-id="c36ea-227">b.</span></span> <span data-ttu-id="c36ea-228">Válassza ki a **szerepkör**.</span><span class="sxs-lookup"><span data-stu-id="c36ea-228">Select a **Role**.</span></span>
  
   <span data-ttu-id="c36ea-229">c.</span><span class="sxs-lookup"><span data-stu-id="c36ea-229">c.</span></span> <span data-ttu-id="c36ea-230">Kattintson a **nyújt**.</span><span class="sxs-lookup"><span data-stu-id="c36ea-230">Click **Submit**.</span></span>

>[!NOTE]
><span data-ttu-id="c36ea-231">Bármely más adaptív Suite felhasználói fiók létrehozása eszközök vagy adaptív Suite tooprovision által nyújtott API-k AAD felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="c36ea-231">You can use any other Adaptive Suite user account creation tools or APIs provided by Adaptive Suite tooprovision AAD user accounts.</span></span>
>  

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="c36ea-232">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="c36ea-232">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="c36ea-233">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooAdaptive Suite megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="c36ea-233">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooAdaptive Suite.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="c36ea-235">**tooassign Britta Simon tooAdaptive Suite, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="c36ea-235">**tooassign Britta Simon tooAdaptive Suite, perform hello following steps:**</span></span>

1. <span data-ttu-id="c36ea-236">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="c36ea-236">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="c36ea-238">Hello alkalmazások listában válassza ki a **adaptív Suite**.</span><span class="sxs-lookup"><span data-stu-id="c36ea-238">In hello applications list, select **Adaptive Suite**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_adaptivesuite_app.png) 

3. <span data-ttu-id="c36ea-240">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="c36ea-240">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="c36ea-242">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="c36ea-242">Click **Add** button.</span></span> <span data-ttu-id="c36ea-243">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c36ea-243">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="c36ea-245">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="c36ea-245">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="c36ea-246">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c36ea-246">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="c36ea-247">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c36ea-247">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="c36ea-248">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="c36ea-248">Testing single sign-on</span></span>

<span data-ttu-id="c36ea-249">hello ebben a szakaszban célja tootest a Microsoft Azure AD egyszeri bejelentkezéshez használt konfiguráció hello a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="c36ea-249">hello objective of this section is tootest your Microsoft Azure AD Single Sign-On configuration using hello Access Panel.</span></span>

<span data-ttu-id="c36ea-250">Ha a hozzáférési Panel hello hello adaptív Suite csempe gombra kattint, automatikusan bejelentkezett tooyour adaptív Suite alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="c36ea-250">When you click hello Adaptive Suite tile in hello Access Panel, you should get automatically signed-on tooyour Adaptive Suite application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="c36ea-251">További források</span><span class="sxs-lookup"><span data-stu-id="c36ea-251">Additional resources</span></span>

* [<span data-ttu-id="c36ea-252">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="c36ea-252">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="c36ea-253">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="c36ea-253">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_203.png

