---
title: "Oktatóanyag: Azure Active Directory-integráció a Mozy vállalati |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Mozy vállalati között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 489b5e62-85c2-45c9-8766-326632d48114
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: jeedes
ms.openlocfilehash: bab0df4f3621b784cd8edfda3c8e10fe5a7ced9e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-mozy-enterprise"></a><span data-ttu-id="55063-103">Oktatóanyag: Azure Active Directory-integráció a Mozy vállalati</span><span class="sxs-lookup"><span data-stu-id="55063-103">Tutorial: Azure Active Directory integration with Mozy Enterprise</span></span>

<span data-ttu-id="55063-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Mozy vállalati az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="55063-104">In this tutorial, you learn how toointegrate Mozy Enterprise with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="55063-105">Mozy vállalati integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="55063-105">Integrating Mozy Enterprise with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="55063-106">Megadhatja a vállalati hozzáférés tooMozy rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="55063-106">You can control in Azure AD who has access tooMozy Enterprise</span></span>
- <span data-ttu-id="55063-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooMozy vállalati (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="55063-107">You can enable your users tooautomatically get signed-on tooMozy Enterprise (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="55063-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="55063-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="55063-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="55063-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="55063-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="55063-110">Prerequisites</span></span>

<span data-ttu-id="55063-111">az Azure AD integrálása a Mozy vállalati tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="55063-111">tooconfigure Azure AD integration with Mozy Enterprise, you need hello following items:</span></span>

- <span data-ttu-id="55063-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="55063-112">An Azure AD subscription</span></span>
- <span data-ttu-id="55063-113">Egy Mozy vállalati egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="55063-113">A Mozy Enterprise single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="55063-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="55063-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="55063-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="55063-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="55063-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="55063-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="55063-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="55063-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="55063-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="55063-118">Scenario description</span></span>
<span data-ttu-id="55063-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="55063-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="55063-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="55063-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="55063-121">Hello gyűjteményből Mozy vállalati hozzáadása</span><span class="sxs-lookup"><span data-stu-id="55063-121">Adding Mozy Enterprise from hello gallery</span></span>
2. <span data-ttu-id="55063-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="55063-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-mozy-enterprise-from-hello-gallery"></a><span data-ttu-id="55063-123">Hello gyűjteményből Mozy vállalati hozzáadása</span><span class="sxs-lookup"><span data-stu-id="55063-123">Adding Mozy Enterprise from hello gallery</span></span>
<span data-ttu-id="55063-124">tooconfigure hello integrációs Mozy vállalat az Azure AD-be, meg kell tooadd Mozy vállalati hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="55063-124">tooconfigure hello integration of Mozy Enterprise into Azure AD, you need tooadd Mozy Enterprise from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="55063-125">**tooadd Mozy vállalati hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="55063-125">**tooadd Mozy Enterprise from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="55063-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="55063-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="55063-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="55063-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="55063-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="55063-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="55063-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="55063-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="55063-133">Hello keresési mezőbe, írja be a **Mozy vállalati**.</span><span class="sxs-lookup"><span data-stu-id="55063-133">In hello search box, type **Mozy Enterprise**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_mozyenterprise_search.png)

5. <span data-ttu-id="55063-135">A hello eredmények panelen válassza ki a **Mozy vállalati**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="55063-135">In hello results panel, select **Mozy Enterprise**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_mozyenterprise_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="55063-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="55063-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="55063-138">Ebben a szakaszban konfigurálása és tesztelése az Azure AD egyszeri bejelentkezést a Mozy vállalati "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="55063-138">In this section, you configure and test Azure AD single sign-on with Mozy Enterprise based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="55063-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói Mozy vállalati tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="55063-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Mozy Enterprise is tooa user in Azure AD.</span></span> <span data-ttu-id="55063-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználói hello Mozy vállalat közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="55063-140">In other words, a link relationship between an Azure AD user and hello related user in Mozy Enterprise needs toobe established.</span></span>

<span data-ttu-id="55063-141">Mozy vállalati, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="55063-141">In Mozy Enterprise, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="55063-142">tooconfigure és tesztelése az Azure AD egyszeri bejelentkezést a Mozy vállalati, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="55063-142">tooconfigure and test Azure AD single sign-on with Mozy Enterprise, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="55063-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="55063-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="55063-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="55063-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="55063-145">**[Mozy vállalati tesztfelhasználó létrehozása](#creating-a-mozy-enterprise-test-user)**  -toohave egy megfelelője a Britta Simon Mozy vállalat, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="55063-145">**[Creating a Mozy Enterprise test user](#creating-a-mozy-enterprise-test-user)** - toohave a counterpart of Britta Simon in Mozy Enterprise that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="55063-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="55063-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="55063-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="55063-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="55063-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="55063-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="55063-149">Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és az Mozy vállalati alkalmazás egyszeri bejelentkezés beállítása.</span><span class="sxs-lookup"><span data-stu-id="55063-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Mozy Enterprise application.</span></span>

<span data-ttu-id="55063-150">**az Azure AD tooconfigure egyszeri bejelentkezést a Mozy vállalati, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="55063-150">**tooconfigure Azure AD single sign-on with Mozy Enterprise, perform hello following steps:**</span></span>

1. <span data-ttu-id="55063-151">Az Azure portál, a hello hello **Mozy vállalati** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="55063-151">In hello Azure portal, on hello **Mozy Enterprise** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="55063-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="55063-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_mozyenterprise_samlbase.png)

3. <span data-ttu-id="55063-155">A hello **Mozy vállalati tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="55063-155">On hello **Mozy Enterprise Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_mozyenterprise_url.png)

    <span data-ttu-id="55063-157">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<tenantname>.Mozyenterprise.com`</span><span class="sxs-lookup"><span data-stu-id="55063-157">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<tenantname>.Mozyenterprise.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="55063-158">Ez az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="55063-158">This value is not real.</span></span> <span data-ttu-id="55063-159">Frissítse ezt az értéket hello tényleges bejelentkezési URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="55063-159">Update this value with hello actual Sign-On URL.</span></span> <span data-ttu-id="55063-160">Ügyfél [Mozy vállalati ügyfél-támogatási csoport](http://support.mozy.com/) tooget ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="55063-160">Contact [Mozy Enterprise Client support team](http://support.mozy.com/) tooget this value.</span></span>

4. <span data-ttu-id="55063-161">A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="55063-161">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_mozyenterprise_certificate.png) 

5. <span data-ttu-id="55063-163">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="55063-163">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="55063-165">A hello **Mozy vállalati konfiguráció** kattintson **Mozy vállalati konfigurálása** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="55063-165">On hello **Mozy Enterprise Configuration** section, click **Configure Mozy Enterprise** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="55063-166">Másolás hello **SAML Entitásazonosító és SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="55063-166">Copy hello **SAML Entity ID and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_mozyenterprise_configure.png) 

7. <span data-ttu-id="55063-168">Egy másik webes böngészőablakban jelentkezzen be a vállalati Mozy vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="55063-168">In a different web browser window, log into your Mozy Enterprise company site as an administrator.</span></span>

8. <span data-ttu-id="55063-169">A hello **konfigurációs** kattintson **hitelesítési házirend**.</span><span class="sxs-lookup"><span data-stu-id="55063-169">In hello **Configuration** section, click **Authentication Policy**.</span></span>
   
   <span data-ttu-id="55063-170">![Hitelesítési házirend](./media/active-directory-saas-mozy-enterprise-tutorial/ic777314.png "hitelesítési házirend")</span><span class="sxs-lookup"><span data-stu-id="55063-170">![Authentication policy](./media/active-directory-saas-mozy-enterprise-tutorial/ic777314.png "Authentication policy")</span></span>

9. <span data-ttu-id="55063-171">A hello **hitelesítési házirend** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="55063-171">On hello **Authentication Policy** section, perform hello following steps:</span></span>
   
   <span data-ttu-id="55063-172">![Hitelesítési házirend](./media/active-directory-saas-mozy-enterprise-tutorial/ic777315.png "hitelesítési házirend")</span><span class="sxs-lookup"><span data-stu-id="55063-172">![Authentication policy](./media/active-directory-saas-mozy-enterprise-tutorial/ic777315.png "Authentication policy")</span></span>
   
   <span data-ttu-id="55063-173">a.</span><span class="sxs-lookup"><span data-stu-id="55063-173">a.</span></span> <span data-ttu-id="55063-174">Válassza ki **címtárszolgáltatás** , **szolgáltató**.</span><span class="sxs-lookup"><span data-stu-id="55063-174">Select **Directory Service** as **Provider**.</span></span>
   
   <span data-ttu-id="55063-175">b.</span><span class="sxs-lookup"><span data-stu-id="55063-175">b.</span></span> <span data-ttu-id="55063-176">Válassza ki **LDAP leküldéses használja**.</span><span class="sxs-lookup"><span data-stu-id="55063-176">Select **Use LDAP Push**.</span></span>
   
   <span data-ttu-id="55063-177">c.</span><span class="sxs-lookup"><span data-stu-id="55063-177">c.</span></span> <span data-ttu-id="55063-178">Kattintson a hello **SAML-alapú hitelesítés** fülre.</span><span class="sxs-lookup"><span data-stu-id="55063-178">Click hello **SAML Authentication** tab.</span></span>
   
   <span data-ttu-id="55063-179">d.</span><span class="sxs-lookup"><span data-stu-id="55063-179">d.</span></span> <span data-ttu-id="55063-180">Beillesztés **SAML-alapú egyszeri bejelentkezési URL-címe**, amely akkor másolta, az Azure-portálon hello hello **hitelesítés URL-címe** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="55063-180">Paste **SAML Single Sign-On Service URL**, which you have copied from hello Azure portal into hello **Authentication URL** textbox.</span></span>
   
   <span data-ttu-id="55063-181">e.</span><span class="sxs-lookup"><span data-stu-id="55063-181">e.</span></span> <span data-ttu-id="55063-182">Beillesztés **SAML Entitásazonosító**, amely akkor másolta, az Azure-portálon hello hello **SAML-végpont** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="55063-182">Paste **SAML Entity ID**, which you have copied from hello Azure portal into hello **SAML Endpoint** textbox.</span></span>
   
   <span data-ttu-id="55063-183">f.</span><span class="sxs-lookup"><span data-stu-id="55063-183">f.</span></span> <span data-ttu-id="55063-184">Nyissa meg a letöltött base-64 kódolású tanúsítvány a Jegyzettömbben, másolása hello a vágólapra tartalma, és illessze be a teljes tanúsítvány hello **SAML tanúsítvány** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="55063-184">Open your downloaded base-64 encoded certificate in notepad, copy hello content of it into your clipboard, and then paste hello entire Certificate into **SAML Certificate** textbox.</span></span>
   
   <span data-ttu-id="55063-185">g.</span><span class="sxs-lookup"><span data-stu-id="55063-185">g.</span></span> <span data-ttu-id="55063-186">Válassza ki **egyszeri bejelentkezés engedélyezése a rendszergazdák toolog be a hálózati hitelesítő adataik**.</span><span class="sxs-lookup"><span data-stu-id="55063-186">Select **Enable SSO for Admins toolog in with their network credentials**.</span></span>
   
   <span data-ttu-id="55063-187">h.</span><span class="sxs-lookup"><span data-stu-id="55063-187">h.</span></span> <span data-ttu-id="55063-188">Kattintson a **módosítások mentése**.</span><span class="sxs-lookup"><span data-stu-id="55063-188">Click **Save Changes**.</span></span>

> [!TIP]
> <span data-ttu-id="55063-189">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="55063-189">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="55063-190">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="55063-190">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="55063-191">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="55063-191">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="55063-192">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="55063-192">Creating an Azure AD test user</span></span>
<span data-ttu-id="55063-193">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="55063-193">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="55063-195">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="55063-195">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="55063-196">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="55063-196">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-mozy-enterprise-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="55063-198">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="55063-198">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-mozy-enterprise-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="55063-200">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="55063-200">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-mozy-enterprise-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="55063-202">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="55063-202">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-mozy-enterprise-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="55063-204">a.</span><span class="sxs-lookup"><span data-stu-id="55063-204">a.</span></span> <span data-ttu-id="55063-205">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="55063-205">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="55063-206">b.</span><span class="sxs-lookup"><span data-stu-id="55063-206">b.</span></span> <span data-ttu-id="55063-207">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="55063-207">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="55063-208">c.</span><span class="sxs-lookup"><span data-stu-id="55063-208">c.</span></span> <span data-ttu-id="55063-209">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="55063-209">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="55063-210">d.</span><span class="sxs-lookup"><span data-stu-id="55063-210">d.</span></span> <span data-ttu-id="55063-211">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="55063-211">Click **Create**.</span></span>
 
### <a name="creating-a-mozy-enterprise-test-user"></a><span data-ttu-id="55063-212">Mozy vállalati tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="55063-212">Creating a Mozy Enterprise test user</span></span>

<span data-ttu-id="55063-213">A sorrend tooenable az Azure AD felhasználók toolog Mozy vállalat akkor kell üzembe Mozy vállalat.</span><span class="sxs-lookup"><span data-stu-id="55063-213">In order tooenable Azure AD users toolog into Mozy Enterprise, they must be provisioned into Mozy Enterprise.</span></span> <span data-ttu-id="55063-214">Mozy vállalati hello esetben egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="55063-214">In hello case of Mozy Enterprise, provisioning is a manual task.</span></span>

>[!NOTE]
><span data-ttu-id="55063-215">Bármely más Mozy vállalati felhasználói fiók létrehozása eszközök vagy Mozy vállalati tooprovision által nyújtott API-k AAD felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="55063-215">You can use any other Mozy Enterprise user account creation tools or APIs provided by Mozy Enterprise tooprovision AAD user accounts.</span></span>

<span data-ttu-id="55063-216">**tooprovision felhasználói fiókok, hajtsa végre hello a következő lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="55063-216">**tooprovision a user accounts, perform hello following steps:**</span></span>

1. <span data-ttu-id="55063-217">Jelentkezzen be tooyour **Mozy vállalati** bérlő.</span><span class="sxs-lookup"><span data-stu-id="55063-217">Log in tooyour **Mozy Enterprise** tenant.</span></span>

2. <span data-ttu-id="55063-218">Kattintson a **felhasználók**, és kattintson a **új felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="55063-218">Click **Users**, and then click **Add New User**.</span></span>
   
   <span data-ttu-id="55063-219">![Felhasználók](./media/active-directory-saas-mozy-enterprise-tutorial/ic777317.png "felhasználók")</span><span class="sxs-lookup"><span data-stu-id="55063-219">![Users](./media/active-directory-saas-mozy-enterprise-tutorial/ic777317.png "Users")</span></span>
   
   >[!NOTE]
   ><span data-ttu-id="55063-220">Hello **új felhasználó hozzáadása** a beállítás csak akkor látható, ha csak **Mozy** hello szolgáltató alatt legyen kijelölve **hitelesítési házirend**.</span><span class="sxs-lookup"><span data-stu-id="55063-220">hello **Add New User** option is only displayed only if **Mozy** is selected as hello provider under **Authentication policy**.</span></span> <span data-ttu-id="55063-221">Ha a SAML-alapú hitelesítés van beállítva, majd hello felhasználót adnak hozzá automatikusan az egyszeri bejelentkezési keresztül az első bejelentkezés a.</span><span class="sxs-lookup"><span data-stu-id="55063-221">If SAML Authentication is configured, then hello users are added automatically on their first login through Single sign on.</span></span>
    
3. <span data-ttu-id="55063-222">Hello új felhasználói párbeszédpanelen hajtsa végre a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="55063-222">On hello new user dialog, perform hello following steps:</span></span>
   
   <span data-ttu-id="55063-223">![Felhasználók hozzáadása az](./media/active-directory-saas-mozy-enterprise-tutorial/ic777318.png "felhasználók hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="55063-223">![Add Users](./media/active-directory-saas-mozy-enterprise-tutorial/ic777318.png "Add Users")</span></span>
   
   <span data-ttu-id="55063-224">a.</span><span class="sxs-lookup"><span data-stu-id="55063-224">a.</span></span> <span data-ttu-id="55063-225">A hello **válasszon ki egy csoportot** listában, válasszon ki egy csoportot.</span><span class="sxs-lookup"><span data-stu-id="55063-225">From hello **Choose a Group** list, select a group.</span></span>
   
   <span data-ttu-id="55063-226">b.</span><span class="sxs-lookup"><span data-stu-id="55063-226">b.</span></span> <span data-ttu-id="55063-227">A hello **felhasználó milyen típusú** listára, válassza ki a típus.</span><span class="sxs-lookup"><span data-stu-id="55063-227">From hello **What type of user** list, select a type.</span></span>
   
   <span data-ttu-id="55063-228">c.</span><span class="sxs-lookup"><span data-stu-id="55063-228">c.</span></span> <span data-ttu-id="55063-229">A hello **felhasználónév** szövegmezőhöz hello típusnév hello Azure AD-felhasználó.</span><span class="sxs-lookup"><span data-stu-id="55063-229">In hello **Username** textbox, type hello name of hello Azure AD user.</span></span>
   
   <span data-ttu-id="55063-230">d.</span><span class="sxs-lookup"><span data-stu-id="55063-230">d.</span></span> <span data-ttu-id="55063-231">A hello **E-mail** szövegmezőhöz típus hello hello Azure AD-felhasználó e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="55063-231">In hello **Email** textbox, type hello email address of hello Azure AD user.</span></span>
   
   <span data-ttu-id="55063-232">e.</span><span class="sxs-lookup"><span data-stu-id="55063-232">e.</span></span> <span data-ttu-id="55063-233">Válassza ki **felhasználói utasítás e-mailek küldése**.</span><span class="sxs-lookup"><span data-stu-id="55063-233">Select **Send user instruction email**.</span></span>
   
   <span data-ttu-id="55063-234">f.</span><span class="sxs-lookup"><span data-stu-id="55063-234">f.</span></span> <span data-ttu-id="55063-235">Kattintson a **felhasználó(k) hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="55063-235">Click **Add User(s)**.</span></span>

     >[!NOTE]
     > <span data-ttu-id="55063-236">Miután létrehozta a hello felhasználó, egy e-mailt küldünk toohello Azure AD-felhasználó, amely tartalmazza egy hivatkozás tooconfirm hello fiókot, mielőtt aktívvá válik.</span><span class="sxs-lookup"><span data-stu-id="55063-236">After creating hello user, an email will be sent toohello Azure AD user that includes a link tooconfirm hello account before it becomes active.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="55063-237">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="55063-237">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="55063-238">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooMozy vállalati megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="55063-238">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooMozy Enterprise.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="55063-240">**tooassign Britta Simon tooMozy vállalati, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="55063-240">**tooassign Britta Simon tooMozy Enterprise, perform hello following steps:**</span></span>

1. <span data-ttu-id="55063-241">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="55063-241">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="55063-243">Hello alkalmazások listában válassza ki a **Mozy vállalati**.</span><span class="sxs-lookup"><span data-stu-id="55063-243">In hello applications list, select **Mozy Enterprise**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_mozyenterprise_app.png) 

3. <span data-ttu-id="55063-245">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="55063-245">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="55063-247">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="55063-247">Click **Add** button.</span></span> <span data-ttu-id="55063-248">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="55063-248">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="55063-250">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="55063-250">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="55063-251">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="55063-251">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="55063-252">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="55063-252">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="55063-253">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="55063-253">Testing single sign-on</span></span>

<span data-ttu-id="55063-254">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="55063-254">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="55063-255">Hello Mozy vállalati hello hozzáférési Panel csempére kattintva Mozy vállalati alkalmazás bejelentkezési oldalán szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="55063-255">When you click hello Mozy Enterprise tile in hello Access Panel, you should get login page of Mozy Enterprise application.</span></span>
<span data-ttu-id="55063-256">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="55063-256">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="55063-257">További források</span><span class="sxs-lookup"><span data-stu-id="55063-257">Additional resources</span></span>

* [<span data-ttu-id="55063-258">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="55063-258">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="55063-259">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="55063-259">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_general_203.png

