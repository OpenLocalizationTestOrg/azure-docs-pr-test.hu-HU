---
title: "Oktatóanyag: Azure Active Directoryval integrált DocuSign |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és DocuSign között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a691288b-84c1-40fb-84bd-5b06878865f0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: jeedes
ms.openlocfilehash: e4ef40b8f5af20d811d8d806d2bd7e2039c55052
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-docusign"></a><span data-ttu-id="7124a-103">Oktatóanyag: Azure Active Directoryval integrált DocuSign</span><span class="sxs-lookup"><span data-stu-id="7124a-103">Tutorial: Azure Active Directory integration with DocuSign</span></span>

<span data-ttu-id="7124a-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate DocuSign az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="7124a-104">In this tutorial, you learn how toointegrate DocuSign with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="7124a-105">DocuSign integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="7124a-105">Integrating DocuSign with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="7124a-106">Megadhatja a hozzáférés tooDocuSign rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="7124a-106">You can control in Azure AD who has access tooDocuSign</span></span>
- <span data-ttu-id="7124a-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooDocuSign (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="7124a-107">You can enable your users tooautomatically get signed-on tooDocuSign (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="7124a-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="7124a-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="7124a-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="7124a-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7124a-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="7124a-110">Prerequisites</span></span>

<span data-ttu-id="7124a-111">az Azure AD integrálása DocuSign tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="7124a-111">tooconfigure Azure AD integration with DocuSign, you need hello following items:</span></span>

- <span data-ttu-id="7124a-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="7124a-112">An Azure AD subscription</span></span>
- <span data-ttu-id="7124a-113">Egy DocuSign egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="7124a-113">A DocuSign single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="7124a-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="7124a-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="7124a-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="7124a-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="7124a-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="7124a-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="7124a-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="7124a-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="7124a-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="7124a-118">Scenario description</span></span>
<span data-ttu-id="7124a-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="7124a-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="7124a-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="7124a-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="7124a-121">Hello gyűjteményből DocuSign hozzáadása</span><span class="sxs-lookup"><span data-stu-id="7124a-121">Adding DocuSign from hello gallery</span></span>
2. <span data-ttu-id="7124a-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="7124a-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-docusign-from-hello-gallery"></a><span data-ttu-id="7124a-123">Hello gyűjteményből DocuSign hozzáadása</span><span class="sxs-lookup"><span data-stu-id="7124a-123">Adding DocuSign from hello gallery</span></span>
<span data-ttu-id="7124a-124">tooconfigure hello integrációja DocuSign az Azure AD-be, meg kell tooadd DocuSign hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="7124a-124">tooconfigure hello integration of DocuSign into Azure AD, you need tooadd DocuSign from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="7124a-125">**tooadd DocuSign hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="7124a-125">**tooadd DocuSign from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="7124a-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="7124a-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="7124a-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="7124a-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="7124a-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="7124a-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="7124a-131">Kattintson a **új alkalmazás** hello párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="7124a-131">Click **New application** button on hello top of hello dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="7124a-133">Hello keresési mezőbe, írja be a **DocuSign**.</span><span class="sxs-lookup"><span data-stu-id="7124a-133">In hello search box, type **DocuSign**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-docusign-tutorial/tutorial_docusign_search.png)

5. <span data-ttu-id="7124a-135">A hello eredmények panelen válassza ki a **DocuSign**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="7124a-135">In hello results panel, select **DocuSign**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-docusign-tutorial/tutorial_docusign_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="7124a-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="7124a-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="7124a-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján DocuSign</span><span class="sxs-lookup"><span data-stu-id="7124a-138">In this section, you configure and test Azure AD single sign-on with DocuSign based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="7124a-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó DocuSign tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="7124a-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in DocuSign is tooa user in Azure AD.</span></span> <span data-ttu-id="7124a-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello DocuSign közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="7124a-140">In other words, a link relationship between an Azure AD user and hello related user in DocuSign needs toobe established.</span></span>

<span data-ttu-id="7124a-141">Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** a DocuSign.</span><span class="sxs-lookup"><span data-stu-id="7124a-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in DocuSign.</span></span>

<span data-ttu-id="7124a-142">tooconfigure és az Azure AD az egyszeri bejelentkezés DocuSign-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="7124a-142">tooconfigure and test Azure AD single sign-on with DocuSign, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="7124a-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="7124a-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="7124a-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="7124a-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="7124a-145">**[DocuSign tesztfelhasználó létrehozása](#creating-a-docusign-test-user)**  -toohave egy megfelelője a Britta Simon a DocuSign, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="7124a-145">**[Creating a DocuSign test user](#creating-a-docusign-test-user)** - toohave a counterpart of Britta Simon in DocuSign that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="7124a-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="7124a-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="7124a-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="7124a-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="7124a-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="7124a-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="7124a-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az DocuSign alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="7124a-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your DocuSign application.</span></span>

<span data-ttu-id="7124a-150">**az Azure AD tooconfigure egyszeri bejelentkezést a DocuSign, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="7124a-150">**tooconfigure Azure AD single sign-on with DocuSign, perform hello following steps:**</span></span>

1. <span data-ttu-id="7124a-151">Az Azure portál, a hello hello **DocuSign** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="7124a-151">In hello Azure portal, on hello **DocuSign** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="7124a-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="7124a-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-docusign-tutorial/tutorial_docusign_samlbase.png)

3. <span data-ttu-id="7124a-155">A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base 64)** , és mentse a fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="7124a-155">On hello **SAML Signing Certificate** section, click **Certificate(Base 64)** and then save certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-docusign-tutorial/tutorial_docusign_certificate.png) 

4. <span data-ttu-id="7124a-157">A hello **DocuSign konfigurációs** az Azure portál, kattintson a szakasz **DocuSign konfigurálása** konfigurálása bejelentkezés tooopen ablak.</span><span class="sxs-lookup"><span data-stu-id="7124a-157">On hello **DocuSign Configuration** section of Azure portal, Click **Configure DocuSign** tooopen Configure sign-on window.</span></span> <span data-ttu-id="7124a-158">Másolás hello **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="7124a-158">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-docusign-tutorial/tutorial_docusign_configure.png)

5. <span data-ttu-id="7124a-160">Egy másik webes böngészőablakban, bejelentkezési tooyour **DocuSign felügyeleti portál** rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="7124a-160">In a different web browser window, login tooyour **DocuSign admin portal** as an administrator.</span></span>

6. <span data-ttu-id="7124a-161">Kattintson a navigációs menü hello hello bal oldali **tartományok**.</span><span class="sxs-lookup"><span data-stu-id="7124a-161">In hello navigation menu on hello left, click **Domains**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása][51]

7. <span data-ttu-id="7124a-163">Kattintson a jobb oldali hello **jogcím tartomány**.</span><span class="sxs-lookup"><span data-stu-id="7124a-163">On hello right pane, click **Claim Domain**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása][52]

8. <span data-ttu-id="7124a-165">A hello **tartomány jogcím** párbeszédpanelen hello **tartománynév** szövegmező, írja be a vállalati tartományhoz, és kattintson **jogcím**.</span><span class="sxs-lookup"><span data-stu-id="7124a-165">On hello **Claim a domain** dialog, in hello **Domain Name** textbox, type your company domain, and then click **Claim**.</span></span> <span data-ttu-id="7124a-166">Győződjön meg arról, hogy hello tartomány ellenőrzése és hello állapota aktív.</span><span class="sxs-lookup"><span data-stu-id="7124a-166">Make sure that you verify hello domain and hello status is active.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása][53]

9. <span data-ttu-id="7124a-168">Hello bal oldali menüben kattintson a **identitás-szolgáltatóktól**</span><span class="sxs-lookup"><span data-stu-id="7124a-168">In menu on hello left side, click **Identity Providers**</span></span>  
   
    ![Egyszeri bejelentkezés konfigurálása][54]
10. <span data-ttu-id="7124a-170">Hello jobb oldali ablaktáblában kattintson **identitásszolgáltató hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="7124a-170">In hello right pane, click **Add Identity Provider**.</span></span> 
   
    ![Egyszeri bejelentkezés konfigurálása][55]

11. <span data-ttu-id="7124a-172">A hello **identitás szolgáltató beállításainak** lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="7124a-172">On hello **Identity Provider Settings** page, perform hello following steps:</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása][56]

    <span data-ttu-id="7124a-174">a.</span><span class="sxs-lookup"><span data-stu-id="7124a-174">a.</span></span> <span data-ttu-id="7124a-175">A hello **neve** szövegmező, írjon be egy egyedi nevet a konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="7124a-175">In hello **Name** textbox, type a unique name for your configuration.</span></span> <span data-ttu-id="7124a-176">Ne használjon szóközt.</span><span class="sxs-lookup"><span data-stu-id="7124a-176">Do not use spaces.</span></span>

    <span data-ttu-id="7124a-177">b.</span><span class="sxs-lookup"><span data-stu-id="7124a-177">b.</span></span> <span data-ttu-id="7124a-178">Beillesztés **SAML Entitásazonosító** történő hello **Identity Provider kibocsátó** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="7124a-178">Paste **SAML Entity ID** into hello **Identity Provider Issuer** textbox.</span></span>

    <span data-ttu-id="7124a-179">c.</span><span class="sxs-lookup"><span data-stu-id="7124a-179">c.</span></span> <span data-ttu-id="7124a-180">Beillesztés **SAML-alapú egyszeri bejelentkezési URL-címe** történő hello **Identity Provider bejelentkezési URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="7124a-180">Paste **SAML Single Sign-On Service URL** into hello **Identity Provider Login URL** textbox.</span></span>

    <span data-ttu-id="7124a-181">d.</span><span class="sxs-lookup"><span data-stu-id="7124a-181">d.</span></span> <span data-ttu-id="7124a-182">Beillesztés **Sign-Out URL-cím** történő hello **Identity Provider kijelentkezési URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="7124a-182">Paste **Sign-Out URL** into hello **Identity Provider Logout URL** textbox.</span></span>

    <span data-ttu-id="7124a-183">e.</span><span class="sxs-lookup"><span data-stu-id="7124a-183">e.</span></span> <span data-ttu-id="7124a-184">Válassza ki **AuthN kérelmet aláíró**.</span><span class="sxs-lookup"><span data-stu-id="7124a-184">Select **Sign AuthN Request**.</span></span>

    <span data-ttu-id="7124a-185">f.</span><span class="sxs-lookup"><span data-stu-id="7124a-185">f.</span></span> <span data-ttu-id="7124a-186">Mint **küldése AuthN kérésére**, jelölje be **POST**.</span><span class="sxs-lookup"><span data-stu-id="7124a-186">As **Send AuthN request by**, select **POST**.</span></span>

    <span data-ttu-id="7124a-187">g.</span><span class="sxs-lookup"><span data-stu-id="7124a-187">g.</span></span> <span data-ttu-id="7124a-188">Mint **küldési kijelentkezési kérésére**, jelölje be **beolvasása**.</span><span class="sxs-lookup"><span data-stu-id="7124a-188">As **Send logout request by**, select **GET**.</span></span>

12. <span data-ttu-id="7124a-189">A hello **egyéni attribútum leképezési** területen válasszon hello mezőt azt szeretné, hogy Azure AD jogcímet toomap.</span><span class="sxs-lookup"><span data-stu-id="7124a-189">In hello **Custom Attribute Mapping** section, choose hello field you want toomap with Azure AD Claim.</span></span> <span data-ttu-id="7124a-190">Ebben a példában hello **emailaddress** jogcím hozzá van rendelve, hello értékű **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.</span><span class="sxs-lookup"><span data-stu-id="7124a-190">In this example, hello **emailaddress** claim is mapped with hello value of **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.</span></span> <span data-ttu-id="7124a-191">Hello alapértelmezett jogcím neve az Azure AD e-mail jogcímet.</span><span class="sxs-lookup"><span data-stu-id="7124a-191">It is hello default claim name from Azure AD for email claim.</span></span> 
   
    > [!NOTE]
    > <span data-ttu-id="7124a-192">Használjon hello megfelelő **felhasználói azonosító** toomap hello felhasználói le az Azure AD tooDocuSign felhasználó.</span><span class="sxs-lookup"><span data-stu-id="7124a-192">Use hello appropriate **User identifier** toomap hello user from Azure AD tooDocuSign user mapping.</span></span> <span data-ttu-id="7124a-193">Válassza ki hello megfelelő mező, és írja be a hello a szervezet beállításoknak megfelelő értékre.</span><span class="sxs-lookup"><span data-stu-id="7124a-193">Select hello proper Field and enter hello appropriate value based on your organization settings.</span></span>
          
    ![Egyszeri bejelentkezés konfigurálása][57]

13. <span data-ttu-id="7124a-195">A hello **szolgáltató Identitástanúsítvány** kattintson **tanúsítvány hozzáadása**, majd töltse fel az Azure AD-portálról letöltött hello tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="7124a-195">In hello **Identity Provider Certificate** section, click **Add Certificate**, and then upload hello certificate you have downloaded from Azure AD portal.</span></span>   
   
    ![Egyszeri bejelentkezés konfigurálása][58]

14. <span data-ttu-id="7124a-197">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="7124a-197">Click **Save**.</span></span>

15. <span data-ttu-id="7124a-198">A hello **identitás-szolgáltatóktól** kattintson **műveletek**, és kattintson a **végpontok**.</span><span class="sxs-lookup"><span data-stu-id="7124a-198">In hello **Identity Providers** section, click **Actions**, and then click **Endpoints**.</span></span>   
   
    ![Egyszeri bejelentkezés konfigurálása][59]
 
16. <span data-ttu-id="7124a-200">A hello **SAML 2.0 végpontok megtekintése** lap **DocuSign felügyeleti portál**, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="7124a-200">In hello **View SAML 2.0 Endpoints** section on **DocuSign admin portal**, perform hello following steps:</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása][60]
   
    <span data-ttu-id="7124a-202">a.</span><span class="sxs-lookup"><span data-stu-id="7124a-202">a.</span></span> <span data-ttu-id="7124a-203">Másolási hello **szolgáltató kibocsátó URL-címe**, majd illessze be hello és **azonosítója** a szövegmező **DocuSign tartomány és az URL-címek** hello Azure portál következő hello szakasza minta: `https://<subdomain>.docusign.com/organization/<uniqueID>/saml2/login/sp/<uniqueID>`.</span><span class="sxs-lookup"><span data-stu-id="7124a-203">Copy hello **Service Provider Issuer URL**, and then paste into hello **Identifier** textbox on **DocuSign Domain and URLs** section of hello Azure portal following hello pattern: `https://<subdomain>.docusign.com/organization/<uniqueID>/saml2/login/sp/<uniqueID>`.</span></span>
   
    <span data-ttu-id="7124a-204">b.</span><span class="sxs-lookup"><span data-stu-id="7124a-204">b.</span></span> <span data-ttu-id="7124a-205">Másolás hello **szolgáltató bejelentkezési URL-címe**, és illessze be hello **URL-cím bejelentkezési** a szövegmező **DocuSign tartomány és az URL-címek** hello Azure portál következő hello szakasza minta: `https://<subdomain>.docusign.com/organization/<uniqueID>/saml2/`.</span><span class="sxs-lookup"><span data-stu-id="7124a-205">Copy hello **Service Provider Login URL**, and then paste into hello **Sign On URL** textbox on **DocuSign Domain and URLs** section of hello Azure portal following hello pattern: `https://<subdomain>.docusign.com/organization/<uniqueID>/saml2/`.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-docusign-tutorial/tutorial_docusign_url.png)
      
    <span data-ttu-id="7124a-207">c.</span><span class="sxs-lookup"><span data-stu-id="7124a-207">c.</span></span>  <span data-ttu-id="7124a-208">Kattintson a **Bezárás**</span><span class="sxs-lookup"><span data-stu-id="7124a-208">Click **Close**</span></span>
    
17. <span data-ttu-id="7124a-209">A hello Azure-portálon, kattintson **mentése**.</span><span class="sxs-lookup"><span data-stu-id="7124a-209">On hello Azure portal, click **Save**.</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-docusign-tutorial/tutorial_general_400.png)

> [!TIP]
> <span data-ttu-id="7124a-211">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="7124a-211">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="7124a-212">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="7124a-212">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="7124a-213">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="7124a-213">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="7124a-214">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="7124a-214">Creating an Azure AD test user</span></span>
<span data-ttu-id="7124a-215">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="7124a-215">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="7124a-217">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="7124a-217">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="7124a-218">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="7124a-218">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-docusign-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="7124a-220">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="7124a-220">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-docusign-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="7124a-222">Hello hello párbeszédpanel, kattintson tetején **Hozzáadás** tooopen hello **felhasználói** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="7124a-222">At hello top of hello dialog, click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-docusign-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="7124a-224">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="7124a-224">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-docusign-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="7124a-226">a.</span><span class="sxs-lookup"><span data-stu-id="7124a-226">a.</span></span> <span data-ttu-id="7124a-227">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="7124a-227">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="7124a-228">b.</span><span class="sxs-lookup"><span data-stu-id="7124a-228">b.</span></span> <span data-ttu-id="7124a-229">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="7124a-229">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="7124a-230">c.</span><span class="sxs-lookup"><span data-stu-id="7124a-230">c.</span></span> <span data-ttu-id="7124a-231">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="7124a-231">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="7124a-232">d.</span><span class="sxs-lookup"><span data-stu-id="7124a-232">d.</span></span> <span data-ttu-id="7124a-233">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="7124a-233">Click **Create**.</span></span>
 
### <a name="creating-a-docusign-test-user"></a><span data-ttu-id="7124a-234">DocuSign tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="7124a-234">Creating a DocuSign test user</span></span>

<span data-ttu-id="7124a-235">Alkalmazás által támogatott **csak az idő a felhasználók átadása** és felhasználók hitelesítésére automatikusan létrehozott hello alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="7124a-235">Application supports **Just in time user provisioning** and after authentication users are created in hello application automatically.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="7124a-236">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="7124a-236">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="7124a-237">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés saját hozzáférés tooDocuSign megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="7124a-237">In this section, you enable Britta Simon toouse Azure single sign-on by granting her access tooDocuSign.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="7124a-239">**tooassign Britta Simon tooDocuSign, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="7124a-239">**tooassign Britta Simon tooDocuSign, perform hello following steps:**</span></span>

1. <span data-ttu-id="7124a-240">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="7124a-240">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="7124a-242">Hello alkalmazások listában válassza ki a **DocuSign**.</span><span class="sxs-lookup"><span data-stu-id="7124a-242">In hello applications list, select **DocuSign**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-docusign-tutorial/tutorial_docusign_app.png) 

3. <span data-ttu-id="7124a-244">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="7124a-244">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="7124a-246">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="7124a-246">Click **Add** button.</span></span> <span data-ttu-id="7124a-247">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="7124a-247">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="7124a-249">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="7124a-249">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="7124a-250">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="7124a-250">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="7124a-251">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="7124a-251">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="7124a-252">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="7124a-252">Testing single sign-on</span></span>

<span data-ttu-id="7124a-253">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="7124a-253">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="7124a-254">Ha a hozzáférési Panel hello hello DocuSign csempe gombra kattint, automatikusan bejelentkezett tooyour DocuSign alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="7124a-254">When you click hello DocuSign tile in hello Access Panel, you should get automatically signed-on tooyour DocuSign application.</span></span>
<span data-ttu-id="7124a-255">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="7124a-255">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="7124a-256">További források</span><span class="sxs-lookup"><span data-stu-id="7124a-256">Additional resources</span></span>

* [<span data-ttu-id="7124a-257">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="7124a-257">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="7124a-258">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="7124a-258">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="7124a-259">A felhasználók átadása konfigurálása</span><span class="sxs-lookup"><span data-stu-id="7124a-259">Configure User Provisioning</span></span>](active-directory-saas-docusign-provisioning-tutorial.md)


<!--Image references-->

[1]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_04.png
[51]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_21.png
[52]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_22.png
[53]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_23.png
[54]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_19.png
[55]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_20.png
[56]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_24.png
[57]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_25.png
[58]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_26.png
[59]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_27.png
[60]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_28.png
[61]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_29.png
[100]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_203.png

