---
title: "Oktatóanyag: Azure Active Directoryval integrált Netsuite |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Netsuite között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: dafa0864-aef2-4f5e-9eac-770504688ef4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 7cf205d5bda5333872fb589e57f4779a8670b595
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-netsuite"></a><span data-ttu-id="d9f62-103">Oktatóanyag: Azure Active Directoryval integrált Netsuite</span><span class="sxs-lookup"><span data-stu-id="d9f62-103">Tutorial: Azure Active Directory integration with Netsuite</span></span>

<span data-ttu-id="d9f62-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Netsuite az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="d9f62-104">In this tutorial, you learn how toointegrate Netsuite with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="d9f62-105">Netsuite integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="d9f62-105">Integrating Netsuite with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="d9f62-106">Megadhatja a hozzáférés tooNetsuite rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="d9f62-106">You can control in Azure AD who has access tooNetsuite</span></span>
- <span data-ttu-id="d9f62-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooNetsuite (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="d9f62-107">You can enable your users tooautomatically get signed-on tooNetsuite (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="d9f62-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="d9f62-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="d9f62-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="d9f62-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d9f62-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="d9f62-110">Prerequisites</span></span>

<span data-ttu-id="d9f62-111">az Azure AD integrálása Netsuite tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="d9f62-111">tooconfigure Azure AD integration with Netsuite, you need hello following items:</span></span>

- <span data-ttu-id="d9f62-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="d9f62-112">An Azure AD subscription</span></span>
- <span data-ttu-id="d9f62-113">Egy Netsuite egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="d9f62-113">A Netsuite single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="d9f62-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="d9f62-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="d9f62-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="d9f62-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="d9f62-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="d9f62-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="d9f62-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d9f62-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="d9f62-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="d9f62-118">Scenario description</span></span>
<span data-ttu-id="d9f62-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="d9f62-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="d9f62-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="d9f62-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="d9f62-121">Hello gyűjteményből Netsuite hozzáadása</span><span class="sxs-lookup"><span data-stu-id="d9f62-121">Adding Netsuite from hello gallery</span></span>
2. <span data-ttu-id="d9f62-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="d9f62-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-netsuite-from-hello-gallery"></a><span data-ttu-id="d9f62-123">Hello gyűjteményből Netsuite hozzáadása</span><span class="sxs-lookup"><span data-stu-id="d9f62-123">Adding Netsuite from hello gallery</span></span>
<span data-ttu-id="d9f62-124">tooconfigure hello integrációja Netsuite az Azure AD-be, meg kell tooadd Netsuite hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="d9f62-124">tooconfigure hello integration of Netsuite into Azure AD, you need tooadd Netsuite from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="d9f62-125">**tooadd Netsuite hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="d9f62-125">**tooadd Netsuite from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="d9f62-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="d9f62-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="d9f62-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="d9f62-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="d9f62-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="d9f62-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="d9f62-131">Kattintson a **új alkalmazás** hello párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="d9f62-131">Click **New application** button on hello top of hello dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="d9f62-133">Hello keresési mezőbe, írja be a **Netsuite**.</span><span class="sxs-lookup"><span data-stu-id="d9f62-133">In hello search box, type **Netsuite**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-netsuite-tutorial/tutorial_netsuite_search.png)

5. <span data-ttu-id="d9f62-135">A hello eredmények panelen válassza ki a **Netsuite**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="d9f62-135">In hello results panel, select **Netsuite**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-netsuite-tutorial/tutorial_netsuite_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="d9f62-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="d9f62-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="d9f62-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján Netsuite</span><span class="sxs-lookup"><span data-stu-id="d9f62-138">In this section, you configure and test Azure AD single sign-on with Netsuite based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="d9f62-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Netsuite tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="d9f62-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Netsuite is tooa user in Azure AD.</span></span> <span data-ttu-id="d9f62-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Netsuite közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="d9f62-140">In other words, a link relationship between an Azure AD user and hello related user in Netsuite needs toobe established.</span></span>

<span data-ttu-id="d9f62-141">Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** a Netsuite.</span><span class="sxs-lookup"><span data-stu-id="d9f62-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Netsuite.</span></span>

<span data-ttu-id="d9f62-142">tooconfigure és az Azure AD az egyszeri bejelentkezés Netsuite-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="d9f62-142">tooconfigure and test Azure AD single sign-on with Netsuite, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="d9f62-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="d9f62-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="d9f62-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d9f62-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="d9f62-145">**[Netsuite tesztfelhasználó létrehozása](#creating-a-netsuite-test-user)**  -toohave egy megfelelője a Britta Simon a Netsuite, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="d9f62-145">**[Creating a Netsuite test user](#creating-a-netsuite-test-user)** - toohave a counterpart of Britta Simon in Netsuite that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="d9f62-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="d9f62-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="d9f62-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="d9f62-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="d9f62-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d9f62-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="d9f62-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Netsuite alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="d9f62-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Netsuite application.</span></span>

<span data-ttu-id="d9f62-150">**az Azure AD tooconfigure egyszeri bejelentkezést a Netsuite, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="d9f62-150">**tooconfigure Azure AD single sign-on with Netsuite, perform hello following steps:**</span></span>

1. <span data-ttu-id="d9f62-151">Az Azure portál, a hello hello **Netsuite** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="d9f62-151">In hello Azure portal, on hello **Netsuite** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="d9f62-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="d9f62-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-netsuite-tutorial/tutorial_netsuite_samlbase.png)

3. <span data-ttu-id="d9f62-155">A hello **Netsuite tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="d9f62-155">On hello **Netsuite Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-netsuite-tutorial/tutorial_netsuite_url.png)

    <span data-ttu-id="d9f62-157">A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be: `https://<tenant-name>.netsuite.com/saml2/acs` `https://<tenant-name>.na1.netsuite.com/saml2/acs` `https://<tenant-name>.na2.netsuite.com/saml2/acs` `https://<tenant-name>.sandbox.netsuite.com/saml2/acs` `https://<tenant-name>.na1.sandbox.netsuite.com/saml2/acs``https://<tenant-name>.na2.sandbox.netsuite.com/saml2/acs`</span><span class="sxs-lookup"><span data-stu-id="d9f62-157">In hello **Reply URL** textbox, type a URL using hello following pattern:   `https://<tenant-name>.netsuite.com/saml2/acs` `https://<tenant-name>.na1.netsuite.com/saml2/acs` `https://<tenant-name>.na2.netsuite.com/saml2/acs` `https://<tenant-name>.sandbox.netsuite.com/saml2/acs` `https://<tenant-name>.na1.sandbox.netsuite.com/saml2/acs` `https://<tenant-name>.na2.sandbox.netsuite.com/saml2/acs`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="d9f62-158">Ez az érték nincs valós értékének.</span><span class="sxs-lookup"><span data-stu-id="d9f62-158">This value is not real value.</span></span> <span data-ttu-id="d9f62-159">Frissítés hello értékének hello tényleges válasz URL-CÍMEN.</span><span class="sxs-lookup"><span data-stu-id="d9f62-159">Update hello value with hello actual Reply URL.</span></span> <span data-ttu-id="d9f62-160">Ügyfél [Netsuite támogatási csoport](http://www.netsuite.com/portal/services/support.shtml) tooget ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="d9f62-160">Contact [Netsuite support team](http://www.netsuite.com/portal/services/support.shtml) tooget this value.</span></span>
 
4. <span data-ttu-id="d9f62-161">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello XML-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="d9f62-161">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello XML file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-netsuite-tutorial/tutorial_netsuite_certificate.png) 

5. <span data-ttu-id="d9f62-163">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="d9f62-163">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-netsuite-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="d9f62-165">A hello **Netsuite konfigurációs** kattintson **konfigurálása Netsuite** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="d9f62-165">On hello **Netsuite Configuration** section, click **Configure Netsuite** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="d9f62-166">Másolás hello **SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="d9f62-166">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-netsuite-tutorial/tutorial_netsuite_configure.png) 

7. <span data-ttu-id="d9f62-168">Új lap megnyitása a böngészőben, és jelentkezzen be rendszergazdaként egy vállalat Netsuite webhelyét.</span><span class="sxs-lookup"><span data-stu-id="d9f62-168">Open a new tab in your browser, and sign into your Netsuite company site as an administrator.</span></span>

8. <span data-ttu-id="d9f62-169">Hello hello felső hello oldal eszköztárán a kattintson **telepítő**, majd kattintson a **Telepítéskezelő**.</span><span class="sxs-lookup"><span data-stu-id="d9f62-169">In hello toolbar at hello top of hello page, click **Setup**, then click **Setup Manager**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-Netsuite-tutorial/ns-setup.png)

9. <span data-ttu-id="d9f62-171">A hello **beállítási feladatok** listáról válassza ki **integrációs**.</span><span class="sxs-lookup"><span data-stu-id="d9f62-171">From hello **Setup Tasks** list, select **Integration**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-Netsuite-tutorial/ns-integration.png)

10. <span data-ttu-id="d9f62-173">A hello **kezelése hitelesítési** kattintson **SAML-alapú egyszeri bejelentkezést**.</span><span class="sxs-lookup"><span data-stu-id="d9f62-173">In hello **Manage Authentication** section, click **SAML Single Sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-Netsuite-tutorial/ns-saml.png)

11. <span data-ttu-id="d9f62-175">A hello **SAML-alapú telepítő** lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="d9f62-175">On hello **SAML Setup** page, perform hello following steps:</span></span>
   
    <span data-ttu-id="d9f62-176">a.</span><span class="sxs-lookup"><span data-stu-id="d9f62-176">a.</span></span> <span data-ttu-id="d9f62-177">Másolás hello **SAML-alapú egyszeri bejelentkezési URL-címe** értéket **rövid összefoglaló** szakasza **bejelentkezés konfigurálása** és illessze be hello **identitásszolgáltató Bejelentkezési oldal** Netsuite mezőbe.</span><span class="sxs-lookup"><span data-stu-id="d9f62-177">Copy hello **SAML Single Sign-On Service URL** value from **Quick Reference** section of **Configure sign-on** and paste it into hello **Identity Provider Login Page** field in Netsuite.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-netsuite-tutorial/ns-saml-setup.png)
  
    <span data-ttu-id="d9f62-179">b.</span><span class="sxs-lookup"><span data-stu-id="d9f62-179">b.</span></span> <span data-ttu-id="d9f62-180">Válassza ki a Netsuite, **elsődleges hitelesítési módszert**.</span><span class="sxs-lookup"><span data-stu-id="d9f62-180">In Netsuite, select **Primary Authentication Method**.</span></span>

    <span data-ttu-id="d9f62-181">c.</span><span class="sxs-lookup"><span data-stu-id="d9f62-181">c.</span></span> <span data-ttu-id="d9f62-182">Hello mező feliratú **SAMLV2 Identity Provider metaadatok**, jelölje be **IDP metaadatait tartalmazó fájl feltöltése**.</span><span class="sxs-lookup"><span data-stu-id="d9f62-182">For hello field labeled **SAMLV2 Identity Provider Metadata**, select **Upload IDP Metadata File**.</span></span> <span data-ttu-id="d9f62-183">Kattintson a **Tallózás** tooupload hello metaadatait tartalmazó fájl az Azure-portálról letöltött.</span><span class="sxs-lookup"><span data-stu-id="d9f62-183">Then click **Browse** tooupload hello metadata file that you downloaded from Azure portal.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-netsuite-tutorial/ns-sso-setup.png)

    <span data-ttu-id="d9f62-185">d.</span><span class="sxs-lookup"><span data-stu-id="d9f62-185">d.</span></span> <span data-ttu-id="d9f62-186">Kattintson a **nyújt**.</span><span class="sxs-lookup"><span data-stu-id="d9f62-186">Click **Submit**.</span></span>

12. <span data-ttu-id="d9f62-187">Az Azure ad-ben, kattintson a **nézet és egyéb felhasználói attribútumok szerkesztése** jelölőnégyzetet, és adja hozzá attribútumot.</span><span class="sxs-lookup"><span data-stu-id="d9f62-187">In Azure AD, Click on **View and edit all other user attributes** check-box and add attribute.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-Netsuite-tutorial/ns-attributes.png)

13. <span data-ttu-id="d9f62-189">A hello **attribútumnév** mezőbe írja be a `account`.</span><span class="sxs-lookup"><span data-stu-id="d9f62-189">For hello **Attribute Name** field, type in `account`.</span></span> <span data-ttu-id="d9f62-190">A hello **attribútumérték** mezőbe írja be a Netsuite fiók azonosítójaként. Ezt az értéket az állandót és módosítási fiókkal.</span><span class="sxs-lookup"><span data-stu-id="d9f62-190">For hello **Attribute Value** field, type in your Netsuite account ID.This value is constant and change with account.</span></span> <span data-ttu-id="d9f62-191">Útmutatás a Fiókazonosító az alábbiakban találhatók toofind:</span><span class="sxs-lookup"><span data-stu-id="d9f62-191">Instructions on how toofind your account ID are included below:</span></span>

      ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-Netsuite-tutorial/ns-add-attribute.png)

    <span data-ttu-id="d9f62-193">a.</span><span class="sxs-lookup"><span data-stu-id="d9f62-193">a.</span></span> <span data-ttu-id="d9f62-194">Netsuite, kattintson **telepítő** hello felső navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="d9f62-194">In Netsuite, click **Setup** from hello top navigation menu.</span></span>

    <span data-ttu-id="d9f62-195">b.</span><span class="sxs-lookup"><span data-stu-id="d9f62-195">b.</span></span> <span data-ttu-id="d9f62-196">Kattintson a hello **beállítási feladatok** hello bal oldali navigációs menü, jelölje be hello részét **integrációs** szakaszt, és kattintson **Web Services beállítások**.</span><span class="sxs-lookup"><span data-stu-id="d9f62-196">Then click under hello **Setup Tasks** section of hello left navigation menu, select hello **Integration** section, and click **Web Services Preferences**.</span></span>

    <span data-ttu-id="d9f62-197">c.</span><span class="sxs-lookup"><span data-stu-id="d9f62-197">c.</span></span> <span data-ttu-id="d9f62-198">A Netsuite Fiókazonosító másolja és illessze be hello **attribútumérték** mezőjét az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="d9f62-198">Copy your Netsuite Account ID and paste it into hello **Attribute Value** field in Azure AD.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-Netsuite-tutorial/ns-account-id.png)

14. <span data-ttu-id="d9f62-200">Mielőtt a felhasználók Netsuite történő egyszeri bejelentkezéshez hajthatnak végre, akkor először meg kell adni Netsuite hello megfelelő engedélyek.</span><span class="sxs-lookup"><span data-stu-id="d9f62-200">Before users can perform single sign-on into Netsuite, they must first be assigned hello appropriate permissions in Netsuite.</span></span> <span data-ttu-id="d9f62-201">Útmutatás alapján hello alatt tooassign azokat az engedélyeket.</span><span class="sxs-lookup"><span data-stu-id="d9f62-201">Follow hello instructions below tooassign these permissions.</span></span>

    <span data-ttu-id="d9f62-202">a.</span><span class="sxs-lookup"><span data-stu-id="d9f62-202">a.</span></span> <span data-ttu-id="d9f62-203">Hello felső navigációs menüjében kattintson **telepítő**, majd kattintson a **Telepítéskezelő**.</span><span class="sxs-lookup"><span data-stu-id="d9f62-203">On hello top navigation menu, click **Setup**, then click **Setup Manager**.</span></span>
      
      ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-Netsuite-tutorial/ns-setup.png)

    <span data-ttu-id="d9f62-205">b.</span><span class="sxs-lookup"><span data-stu-id="d9f62-205">b.</span></span> <span data-ttu-id="d9f62-206">Hello bal oldali navigációs menüjében kattintson a **felhasználók vagy szerepkörök**, majd kattintson a **szerepkörök kezelése**.</span><span class="sxs-lookup"><span data-stu-id="d9f62-206">On hello left navigation menu, select **Users/Roles**, then click **Manage Roles**.</span></span>
      
      ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-Netsuite-tutorial/ns-manage-roles.png)

    <span data-ttu-id="d9f62-208">c.</span><span class="sxs-lookup"><span data-stu-id="d9f62-208">c.</span></span> <span data-ttu-id="d9f62-209">Kattintson a **új szerepkör**.</span><span class="sxs-lookup"><span data-stu-id="d9f62-209">Click **New Role**.</span></span>

    <span data-ttu-id="d9f62-210">d.</span><span class="sxs-lookup"><span data-stu-id="d9f62-210">d.</span></span> <span data-ttu-id="d9f62-211">Írja be a **neve** az új szerepkör, és jelölje be hello **egyszeri bejelentkezés csak** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="d9f62-211">Type in a **Name** for your new role, and select hello **Single Sign-On Only** checkbox.</span></span>
      
      ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-Netsuite-tutorial/ns-new-role.png)

    <span data-ttu-id="d9f62-213">e.</span><span class="sxs-lookup"><span data-stu-id="d9f62-213">e.</span></span> <span data-ttu-id="d9f62-214">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="d9f62-214">Click **Save**.</span></span>

    <span data-ttu-id="d9f62-215">f.</span><span class="sxs-lookup"><span data-stu-id="d9f62-215">f.</span></span> <span data-ttu-id="d9f62-216">Hello hello felső menüben kattintson a **engedélyek**.</span><span class="sxs-lookup"><span data-stu-id="d9f62-216">In hello menu on hello top, click **Permissions**.</span></span> <span data-ttu-id="d9f62-217">Kattintson a **telepítő**.</span><span class="sxs-lookup"><span data-stu-id="d9f62-217">Then click **Setup**.</span></span>
      
       ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-Netsuite-tutorial/ns-sso.png)

    <span data-ttu-id="d9f62-219">g.</span><span class="sxs-lookup"><span data-stu-id="d9f62-219">g.</span></span> <span data-ttu-id="d9f62-220">Válassza ki **beállítva fel SAM egyszeri bejelentkezés**, és kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="d9f62-220">Select **Set Up SAM Single Sign-on**, and then click **Add**.</span></span>

    <span data-ttu-id="d9f62-221">h.</span><span class="sxs-lookup"><span data-stu-id="d9f62-221">h.</span></span> <span data-ttu-id="d9f62-222">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="d9f62-222">Click **Save**.</span></span>

    <span data-ttu-id="d9f62-223">i.</span><span class="sxs-lookup"><span data-stu-id="d9f62-223">i.</span></span> <span data-ttu-id="d9f62-224">Hello felső navigációs menüjében kattintson **telepítő**, majd kattintson a **Telepítéskezelő**.</span><span class="sxs-lookup"><span data-stu-id="d9f62-224">On hello top navigation menu, click **Setup**, then click **Setup Manager**.</span></span>
      
       ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-Netsuite-tutorial/ns-setup.png)

    <span data-ttu-id="d9f62-226">j.</span><span class="sxs-lookup"><span data-stu-id="d9f62-226">j.</span></span> <span data-ttu-id="d9f62-227">Hello bal oldali navigációs menüjében kattintson a **felhasználók vagy szerepkörök**, majd kattintson a **felhasználók kezelése**.</span><span class="sxs-lookup"><span data-stu-id="d9f62-227">On hello left navigation menu, select **Users/Roles**, then click **Manage Users**.</span></span>
      
       ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-Netsuite-tutorial/ns-manage-users.png)

    <span data-ttu-id="d9f62-229">k.</span><span class="sxs-lookup"><span data-stu-id="d9f62-229">k.</span></span> <span data-ttu-id="d9f62-230">Válassza ki a tesztfelhasználó számára.</span><span class="sxs-lookup"><span data-stu-id="d9f62-230">Select a test user.</span></span> <span data-ttu-id="d9f62-231">Kattintson a **szerkesztése**.</span><span class="sxs-lookup"><span data-stu-id="d9f62-231">Then click **Edit**.</span></span>
      
       ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-Netsuite-tutorial/ns-edit-user.png)

    <span data-ttu-id="d9f62-233">l.</span><span class="sxs-lookup"><span data-stu-id="d9f62-233">l.</span></span> <span data-ttu-id="d9f62-234">A következő párbeszédpanelen: hello szerepkörök, válassza ki a hello szerepkör, amely hozott létre, és kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="d9f62-234">On hello Roles dialog, select hello role that you have created and click **Add**.</span></span>
      
       ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-Netsuite-tutorial/ns-add-role.png)

    <span data-ttu-id="d9f62-236">m.</span><span class="sxs-lookup"><span data-stu-id="d9f62-236">m.</span></span> <span data-ttu-id="d9f62-237">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="d9f62-237">Click **Save**.</span></span>
    
> [!TIP]
> <span data-ttu-id="d9f62-238">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="d9f62-238">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="d9f62-239">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="d9f62-239">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="d9f62-240">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="d9f62-240">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="d9f62-241">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="d9f62-241">Creating an Azure AD test user</span></span>
<span data-ttu-id="d9f62-242">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="d9f62-242">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="d9f62-244">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="d9f62-244">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="d9f62-245">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="d9f62-245">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-netsuite-tutorial/create_aaduser_01.png) 

2.  <span data-ttu-id="d9f62-247">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="d9f62-247">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-netsuite-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="d9f62-249">Hello hello párbeszédpanel, kattintson tetején **Hozzáadás** tooopen hello **felhasználói** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d9f62-249">At hello top of hello dialog, click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-netsuite-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="d9f62-251">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="d9f62-251">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-netsuite-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="d9f62-253">a.</span><span class="sxs-lookup"><span data-stu-id="d9f62-253">a.</span></span> <span data-ttu-id="d9f62-254">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="d9f62-254">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="d9f62-255">b.</span><span class="sxs-lookup"><span data-stu-id="d9f62-255">b.</span></span> <span data-ttu-id="d9f62-256">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="d9f62-256">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="d9f62-257">c.</span><span class="sxs-lookup"><span data-stu-id="d9f62-257">c.</span></span> <span data-ttu-id="d9f62-258">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="d9f62-258">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="d9f62-259">d.</span><span class="sxs-lookup"><span data-stu-id="d9f62-259">d.</span></span> <span data-ttu-id="d9f62-260">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="d9f62-260">Click **Create**.</span></span> 

### <a name="creating-a-netsuite-test-user"></a><span data-ttu-id="d9f62-261">Netsuite tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="d9f62-261">Creating a Netsuite test user</span></span>

<span data-ttu-id="d9f62-262">Ebben a szakaszban egy felhasználó Britta Simon nevű Netsuite jön létre.</span><span class="sxs-lookup"><span data-stu-id="d9f62-262">In this section, a user called Britta Simon is created in Netsuite.</span></span> <span data-ttu-id="d9f62-263">Netsuite támogatja közvetlenül az időponthoz kötött kiosztást, amely alapértelmezés szerint engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="d9f62-263">Netsuite supports just-in-time provisioning, which is enabled by default.</span></span>
<span data-ttu-id="d9f62-264">Nincs ebben a szakaszban az Ön művelet elem.</span><span class="sxs-lookup"><span data-stu-id="d9f62-264">There is no action item for you in this section.</span></span> <span data-ttu-id="d9f62-265">Ha a felhasználó nem létezik a Netsuite, egy új tooaccess Netsuite tett kísérlet során jön létre.</span><span class="sxs-lookup"><span data-stu-id="d9f62-265">If a user doesn't already exist in Netsuite, a new one is created when you attempt tooaccess Netsuite.</span></span>


### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="d9f62-266">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="d9f62-266">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="d9f62-267">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooNetsuite megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="d9f62-267">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooNetsuite.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="d9f62-269">**tooassign Britta Simon tooNetsuite, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="d9f62-269">**tooassign Britta Simon tooNetsuite, perform hello following steps:**</span></span>

1. <span data-ttu-id="d9f62-270">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="d9f62-270">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="d9f62-272">Hello alkalmazások listában válassza ki a **Netsuite**.</span><span class="sxs-lookup"><span data-stu-id="d9f62-272">In hello applications list, select **Netsuite**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-netsuite-tutorial/tutorial_netsuite_app.png) 

3. <span data-ttu-id="d9f62-274">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="d9f62-274">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="d9f62-276">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="d9f62-276">Click **Add** button.</span></span> <span data-ttu-id="d9f62-277">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d9f62-277">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="d9f62-279">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="d9f62-279">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="d9f62-280">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d9f62-280">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="d9f62-281">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d9f62-281">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="d9f62-282">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="d9f62-282">Testing single sign-on</span></span>

<span data-ttu-id="d9f62-283">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="d9f62-283">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="d9f62-284">tootest a egyszeri bejelentkezés beállításokat, nyissa meg hello hozzáférési panelre a [https://myapps.microsoft.com](https://myapps.microsoft.com/), jelentkezzen be a hello olyan fiókot, és kattintson **Netsuite**.</span><span class="sxs-lookup"><span data-stu-id="d9f62-284">tootest your single sign-on settings, open hello Access Panel at [https://myapps.microsoft.com](https://myapps.microsoft.com/), sign into hello test account, and click **Netsuite**.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d9f62-285">További források</span><span class="sxs-lookup"><span data-stu-id="d9f62-285">Additional resources</span></span>

* [<span data-ttu-id="d9f62-286">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="d9f62-286">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="d9f62-287">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="d9f62-287">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="d9f62-288">A felhasználók átadása konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d9f62-288">Configure User Provisioning</span></span>](active-directory-saas-netsuite-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_203.png

