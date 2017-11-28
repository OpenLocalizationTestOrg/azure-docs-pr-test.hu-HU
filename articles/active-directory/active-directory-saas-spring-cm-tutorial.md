---
title: "Oktatóanyag: Azure Active Directoryval integrált SpringCM |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és SpringCM között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 4a42f797-ac58-4aca-a8e6-53bfe5529083
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: jeedes
ms.openlocfilehash: 12c8ebe765e2c6e61115256e9343d90ec132e1f8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-springcm"></a><span data-ttu-id="56b82-103">Oktatóanyag: Azure Active Directoryval integrált SpringCM</span><span class="sxs-lookup"><span data-stu-id="56b82-103">Tutorial: Azure Active Directory integration with SpringCM</span></span>

<span data-ttu-id="56b82-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate SpringCM az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="56b82-104">In this tutorial, you learn how toointegrate SpringCM with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="56b82-105">SpringCM integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="56b82-105">Integrating SpringCM with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="56b82-106">Megadhatja a hozzáférés tooSpringCM rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="56b82-106">You can control in Azure AD who has access tooSpringCM</span></span>
- <span data-ttu-id="56b82-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooSpringCM (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="56b82-107">You can enable your users tooautomatically get signed-on tooSpringCM (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="56b82-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="56b82-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="56b82-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="56b82-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="56b82-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="56b82-110">Prerequisites</span></span>

<span data-ttu-id="56b82-111">az Azure AD integrálása SpringCM tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="56b82-111">tooconfigure Azure AD integration with SpringCM, you need hello following items:</span></span>

- <span data-ttu-id="56b82-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="56b82-112">An Azure AD subscription</span></span>
- <span data-ttu-id="56b82-113">Egy SpringCM egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="56b82-113">A SpringCM single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="56b82-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="56b82-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="56b82-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="56b82-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="56b82-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="56b82-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="56b82-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="56b82-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="56b82-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="56b82-118">Scenario description</span></span>
<span data-ttu-id="56b82-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="56b82-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="56b82-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="56b82-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="56b82-121">Hello gyűjteményből SpringCM hozzáadása</span><span class="sxs-lookup"><span data-stu-id="56b82-121">Adding SpringCM from hello gallery</span></span>
2. <span data-ttu-id="56b82-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="56b82-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-springcm-from-hello-gallery"></a><span data-ttu-id="56b82-123">Hello gyűjteményből SpringCM hozzáadása</span><span class="sxs-lookup"><span data-stu-id="56b82-123">Adding SpringCM from hello gallery</span></span>
<span data-ttu-id="56b82-124">tooconfigure hello integrációja SpringCM az Azure AD-be, meg kell tooadd SpringCM hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="56b82-124">tooconfigure hello integration of SpringCM into Azure AD, you need tooadd SpringCM from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="56b82-125">**tooadd SpringCM hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="56b82-125">**tooadd SpringCM from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="56b82-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="56b82-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="56b82-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="56b82-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="56b82-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="56b82-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="56b82-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="56b82-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="56b82-133">Hello keresési mezőbe, írja be a **SpringCM**.</span><span class="sxs-lookup"><span data-stu-id="56b82-133">In hello search box, type **SpringCM**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-spring-cm-tutorial/tutorial_springcm_search.png)

5. <span data-ttu-id="56b82-135">A hello eredmények panelen válassza ki a **SpringCM**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="56b82-135">In hello results panel, select **SpringCM**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-spring-cm-tutorial/tutorial_springcm_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="56b82-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="56b82-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="56b82-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján SpringCM</span><span class="sxs-lookup"><span data-stu-id="56b82-138">In this section, you configure and test Azure AD single sign-on with SpringCM based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="56b82-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó SpringCM tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="56b82-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in SpringCM is tooa user in Azure AD.</span></span> <span data-ttu-id="56b82-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello SpringCM közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="56b82-140">In other words, a link relationship between an Azure AD user and hello related user in SpringCM needs toobe established.</span></span>

<span data-ttu-id="56b82-141">SpringCM, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="56b82-141">In SpringCM, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="56b82-142">tooconfigure és az Azure AD az egyszeri bejelentkezés SpringCM-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="56b82-142">tooconfigure and test Azure AD single sign-on with SpringCM, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="56b82-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="56b82-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="56b82-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="56b82-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="56b82-145">**[SpringCM tesztfelhasználó létrehozása](#creating-a-springcm-test-user)**  -toohave egy megfelelője a Britta Simon a SpringCM, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="56b82-145">**[Creating a SpringCM test user](#creating-a-springcm-test-user)** - toohave a counterpart of Britta Simon in SpringCM that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="56b82-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="56b82-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="56b82-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="56b82-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="56b82-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="56b82-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="56b82-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az SpringCM alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="56b82-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your SpringCM application.</span></span>

<span data-ttu-id="56b82-150">**az Azure AD tooconfigure egyszeri bejelentkezést a SpringCM, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="56b82-150">**tooconfigure Azure AD single sign-on with SpringCM, perform hello following steps:**</span></span>

1. <span data-ttu-id="56b82-151">Az Azure portál, a hello hello **SpringCM** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="56b82-151">In hello Azure portal, on hello **SpringCM** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="56b82-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="56b82-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-spring-cm-tutorial/tutorial_springcm_samlbase.png)

3. <span data-ttu-id="56b82-155">A hello **SpringCM tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="56b82-155">On hello **SpringCM Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-spring-cm-tutorial/tutorial_springcm_url.png)

    <span data-ttu-id="56b82-157">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://na11.springcm.com/atlas/SSO/SSOEndpoint.ashx?aid=<identifier>`</span><span class="sxs-lookup"><span data-stu-id="56b82-157">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://na11.springcm.com/atlas/SSO/SSOEndpoint.ashx?aid=<identifier>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="56b82-158">Ez az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="56b82-158">This value is not real.</span></span> <span data-ttu-id="56b82-159">Frissítse ezt az értéket hello tényleges bejelentkezési URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="56b82-159">Update this value with hello actual Sign-On URL.</span></span> <span data-ttu-id="56b82-160">Ügyfél [SpringCM ügyfél-támogatási csoport](https://knowledge.springcm.com/support) tooget ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="56b82-160">Contact [SpringCM Client support team](https://knowledge.springcm.com/support) tooget this value.</span></span> 
 
4. <span data-ttu-id="56b82-161">A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Raw)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="56b82-161">On hello **SAML Signing Certificate** section, click **Certificate(Raw)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-spring-cm-tutorial/tutorial_springcm_certificate.png) 

5. <span data-ttu-id="56b82-163">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="56b82-163">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-spring-cm-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="56b82-165">A hello **SpringCM konfigurációs** kattintson **konfigurálása SpringCM** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="56b82-165">On hello **SpringCM Configuration** section, click **Configure SpringCM** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="56b82-166">Másolás hello **SAML Entitásazonosító és SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="56b82-166">Copy hello **SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-spring-cm-tutorial/tutorial_springcm_configure.png)   

7. <span data-ttu-id="56b82-168">Egy másik webes böngészőablakban tooyour bejelentkezés **SpringCM** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="56b82-168">In a different web browser window, sign on tooyour **SpringCM** company site as administrator.</span></span>

8. <span data-ttu-id="56b82-169">Hello hello felső menüben kattintson a **Ugrás**, kattintson a **beállítások**, majd a hello **fiók beállítások** területen kattintson **SAML SSO**.</span><span class="sxs-lookup"><span data-stu-id="56b82-169">In hello menu on hello top, click **GO TO**, click **Preferences**, and then, in hello **Account Preferences** section, click **SAML SSO**.</span></span>
   
    <span data-ttu-id="56b82-170">![SAML SSO](./media/active-directory-saas-spring-cm-tutorial/ic797051.png "SAML SSO")</span><span class="sxs-lookup"><span data-stu-id="56b82-170">![SAML SSO](./media/active-directory-saas-spring-cm-tutorial/ic797051.png "SAML SSO")</span></span>

9. <span data-ttu-id="56b82-171">Az Identity Provider konfigurációs szakasz hello hajtsa végre a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="56b82-171">In hello Identity Provider Configuration section, perform hello following steps:</span></span>
   
    <span data-ttu-id="56b82-172">![Szolgáltató konfigurálása identitás](./media/active-directory-saas-spring-cm-tutorial/ic797052.png "identitás szolgáltató konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="56b82-172">![Identity Provider Configuration](./media/active-directory-saas-spring-cm-tutorial/ic797052.png "Identity Provider Configuration")</span></span>
    
    <span data-ttu-id="56b82-173">a.</span><span class="sxs-lookup"><span data-stu-id="56b82-173">a.</span></span> <span data-ttu-id="56b82-174">tooupload a letöltött Azure Active Directory-tanúsítvány, kattintson a **válassza ki a tanúsítványt kibocsátó** vagy **módosítás kibocsátói tanúsítvány**.</span><span class="sxs-lookup"><span data-stu-id="56b82-174">tooupload your downloaded Azure Active Directory certificate, click **Select Issuer Certificate** or **Change Issuer Certificate**.</span></span>
    
    <span data-ttu-id="56b82-175">b.</span><span class="sxs-lookup"><span data-stu-id="56b82-175">b.</span></span> <span data-ttu-id="56b82-176">Beillesztés **SAML Entitásazonosító** értéket, amely az Azure portálról történő hello másolta **kibocsátó** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="56b82-176">Paste **SAML Entity ID** value, which you have copied from Azure portal into hello **Issuer** textbox.</span></span>
    
    <span data-ttu-id="56b82-177">c.</span><span class="sxs-lookup"><span data-stu-id="56b82-177">c.</span></span> <span data-ttu-id="56b82-178">Beillesztés **SAML-alapú egyszeri bejelentkezési URL-címe** értéket, amely akkor másolta, az Azure-portálon hello hello **Service Provider (SP) kezdeményezett végpont** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="56b82-178">Paste **SAML Single Sign-On Service URL** value, which you have copied from hello Azure portal into hello **Service Provider (SP) Initiated Endpoint** textbox.</span></span>
            
    <span data-ttu-id="56b82-179">d.</span><span class="sxs-lookup"><span data-stu-id="56b82-179">d.</span></span> <span data-ttu-id="56b82-180">Válassza ki **SAML engedélyezett** , **engedélyezése**.</span><span class="sxs-lookup"><span data-stu-id="56b82-180">Select **SAML Enabled** as **Enable**.</span></span>

    <span data-ttu-id="56b82-181">e.</span><span class="sxs-lookup"><span data-stu-id="56b82-181">e.</span></span> <span data-ttu-id="56b82-182">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="56b82-182">Click **Save**.</span></span>
 
> [!TIP]
> <span data-ttu-id="56b82-183">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="56b82-183">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="56b82-184">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="56b82-184">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="56b82-185">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="56b82-185">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="56b82-186">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="56b82-186">Creating an Azure AD test user</span></span>
<span data-ttu-id="56b82-187">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="56b82-187">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="56b82-189">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="56b82-189">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="56b82-190">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="56b82-190">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-spring-cm-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="56b82-192">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="56b82-192">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-spring-cm-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="56b82-194">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="56b82-194">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-spring-cm-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="56b82-196">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="56b82-196">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-spring-cm-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="56b82-198">a.</span><span class="sxs-lookup"><span data-stu-id="56b82-198">a.</span></span> <span data-ttu-id="56b82-199">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="56b82-199">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="56b82-200">b.</span><span class="sxs-lookup"><span data-stu-id="56b82-200">b.</span></span> <span data-ttu-id="56b82-201">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="56b82-201">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="56b82-202">c.</span><span class="sxs-lookup"><span data-stu-id="56b82-202">c.</span></span> <span data-ttu-id="56b82-203">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="56b82-203">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="56b82-204">d.</span><span class="sxs-lookup"><span data-stu-id="56b82-204">d.</span></span> <span data-ttu-id="56b82-205">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="56b82-205">Click **Create**.</span></span>
 
### <a name="creating-a-springcm-test-user"></a><span data-ttu-id="56b82-206">SpringCM tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="56b82-206">Creating a SpringCM test user</span></span>

<span data-ttu-id="56b82-207">Azure Active Directory-felhasználók toolog tooenable a tooSpringCM, akkor ki kell építenie SpringCM be.</span><span class="sxs-lookup"><span data-stu-id="56b82-207">tooenable Azure Active Directory users toolog in tooSpringCM, they must be provisioned into SpringCM.</span></span> <span data-ttu-id="56b82-208">SpringCM hello esetben egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="56b82-208">In hello case of SpringCM, provisioning is a manual task.</span></span>

>[!NOTE]
><span data-ttu-id="56b82-209">További információkért lásd: [létrehozása és szerkesztése a SpringCM felhasználó](http://knowledge.springcm.com/create-and-edit-a-springcm-user).</span><span class="sxs-lookup"><span data-stu-id="56b82-209">For more information, see [Create and Edit a SpringCM User](http://knowledge.springcm.com/create-and-edit-a-springcm-user).</span></span> 

<span data-ttu-id="56b82-210">**egy felhasználói fiók tooSpringCM tooprovision hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="56b82-210">**tooprovision a user account tooSpringCM, perform hello following steps:**</span></span>

1. <span data-ttu-id="56b82-211">Jelentkezzen be tooyour **SpringCM** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="56b82-211">Log in tooyour **SpringCM** company site as administrator.</span></span>

2. <span data-ttu-id="56b82-212">Kattintson a **GOTO**, és kattintson a **CÍMJEGYZÉK**.</span><span class="sxs-lookup"><span data-stu-id="56b82-212">Click **GOTO**, and then click **ADDRESS BOOK**.</span></span>
   
    <span data-ttu-id="56b82-213">![Hozzon létre felhasználói](./media/active-directory-saas-spring-cm-tutorial/ic797054.png "felhasználó létrehozása")</span><span class="sxs-lookup"><span data-stu-id="56b82-213">![Create User](./media/active-directory-saas-spring-cm-tutorial/ic797054.png "Create User")</span></span>

3. <span data-ttu-id="56b82-214">Kattintson a **létrehozza a felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="56b82-214">Click **Create User**.</span></span>

4. <span data-ttu-id="56b82-215">Válassza ki a **felhasználói szerepkör**.</span><span class="sxs-lookup"><span data-stu-id="56b82-215">Select a **User Role**.</span></span>

5. <span data-ttu-id="56b82-216">Válassza ki **aktiválási e-mailek küldése**.</span><span class="sxs-lookup"><span data-stu-id="56b82-216">Select **Send Activation Email**.</span></span>

6. <span data-ttu-id="56b82-217">Típus hello utónevét, vezetéknevét és egy érvényes Azure Active Directory felhasználói fiók tooprovision hello a kívánt e-mail címet kapcsolódó szövegmezőből.</span><span class="sxs-lookup"><span data-stu-id="56b82-217">Type hello first name, last name, and email address of a valid Azure Active Directory user account you want tooprovision into hello related textboxes.</span></span>

7. <span data-ttu-id="56b82-218">Adja hozzá a hello felhasználói tooa **biztonsági csoport**.</span><span class="sxs-lookup"><span data-stu-id="56b82-218">Add hello user tooa **Security group**.</span></span>

8. <span data-ttu-id="56b82-219">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="56b82-219">Click **Save**.</span></span>

  >[!NOTE]
  ><span data-ttu-id="56b82-220">Bármely más SpringCM felhasználói fiók létrehozása eszközök vagy SpringCM tooprovision által nyújtott API-k AAD felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="56b82-220">You can use any other SpringCM user account creation tools or APIs provided by SpringCM tooprovision AAD user accounts.</span></span>  
  > 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="56b82-221">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="56b82-221">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="56b82-222">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooSpringCM megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="56b82-222">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooSpringCM.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="56b82-224">**tooassign Britta Simon tooSpringCM, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="56b82-224">**tooassign Britta Simon tooSpringCM, perform hello following steps:**</span></span>

1. <span data-ttu-id="56b82-225">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="56b82-225">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="56b82-227">Hello alkalmazások listában válassza ki a **SpringCM**.</span><span class="sxs-lookup"><span data-stu-id="56b82-227">In hello applications list, select **SpringCM**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-spring-cm-tutorial/tutorial_springcm_app.png) 

3. <span data-ttu-id="56b82-229">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="56b82-229">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="56b82-231">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="56b82-231">Click **Add** button.</span></span> <span data-ttu-id="56b82-232">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="56b82-232">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="56b82-234">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="56b82-234">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="56b82-235">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="56b82-235">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="56b82-236">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="56b82-236">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="56b82-237">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="56b82-237">Testing single sign-on</span></span>

<span data-ttu-id="56b82-238">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="56b82-238">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>
 
<span data-ttu-id="56b82-239">Ha a hozzáférési Panel hello hello SpringCM csempe gombra kattint, automatikusan bejelentkezett tooyour SpringCM alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="56b82-239">When you click hello SpringCM tile in hello Access Panel, you should get automatically signed-on tooyour SpringCM application.</span></span>

<span data-ttu-id="56b82-240">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="56b82-240">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="56b82-241">További források</span><span class="sxs-lookup"><span data-stu-id="56b82-241">Additional resources</span></span>

* [<span data-ttu-id="56b82-242">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="56b82-242">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="56b82-243">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="56b82-243">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-spring-cm-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-spring-cm-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-spring-cm-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-spring-cm-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-spring-cm-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-spring-cm-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-spring-cm-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-spring-cm-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-spring-cm-tutorial/tutorial_general_203.png

