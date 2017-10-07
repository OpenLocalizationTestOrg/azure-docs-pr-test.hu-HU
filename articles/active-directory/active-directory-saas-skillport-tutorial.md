---
title: "Oktatóanyag: Azure Active Directoryval integrált Skillport |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Skillport között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 4df349b2-a73f-4b88-a077-ec0fbfc26527
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/05/2017
ms.author: jeedes
ms.openlocfilehash: ba504c3cae5f92767eb90d8453887904690fe0c5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-skillport"></a><span data-ttu-id="e70bc-103">Oktatóanyag: Azure Active Directoryval integrált Skillport</span><span class="sxs-lookup"><span data-stu-id="e70bc-103">Tutorial: Azure Active Directory integration with Skillport</span></span>

<span data-ttu-id="e70bc-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Skillport az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="e70bc-104">In this tutorial, you learn how toointegrate Skillport with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="e70bc-105">Skillport integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="e70bc-105">Integrating Skillport with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="e70bc-106">Megadhatja a hozzáférés tooSkillport rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="e70bc-106">You can control in Azure AD who has access tooSkillport</span></span>
- <span data-ttu-id="e70bc-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooSkillport (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="e70bc-107">You can enable your users tooautomatically get signed-on tooSkillport (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="e70bc-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="e70bc-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="e70bc-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="e70bc-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e70bc-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="e70bc-110">Prerequisites</span></span>

<span data-ttu-id="e70bc-111">az Azure AD integrálása Skillport tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="e70bc-111">tooconfigure Azure AD integration with Skillport, you need hello following items:</span></span>

- <span data-ttu-id="e70bc-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="e70bc-112">An Azure AD subscription</span></span>
- <span data-ttu-id="e70bc-113">Egy Skillport egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="e70bc-113">A Skillport single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="e70bc-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="e70bc-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="e70bc-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="e70bc-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="e70bc-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="e70bc-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="e70bc-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="e70bc-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="e70bc-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="e70bc-118">Scenario description</span></span>
<span data-ttu-id="e70bc-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="e70bc-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="e70bc-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="e70bc-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="e70bc-121">Hello gyűjteményből Skillport hozzáadása</span><span class="sxs-lookup"><span data-stu-id="e70bc-121">Adding Skillport from hello gallery</span></span>
2. <span data-ttu-id="e70bc-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="e70bc-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-skillport-from-hello-gallery"></a><span data-ttu-id="e70bc-123">Hello gyűjteményből Skillport hozzáadása</span><span class="sxs-lookup"><span data-stu-id="e70bc-123">Adding Skillport from hello gallery</span></span>
<span data-ttu-id="e70bc-124">tooconfigure hello integrációja Skillport az Azure AD-be, meg kell tooadd Skillport hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="e70bc-124">tooconfigure hello integration of Skillport into Azure AD, you need tooadd Skillport from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="e70bc-125">**tooadd Skillport hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="e70bc-125">**tooadd Skillport from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="e70bc-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="e70bc-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="e70bc-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="e70bc-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="e70bc-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="e70bc-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="e70bc-131">Kattintson a **új alkalmazás** hello párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="e70bc-131">Click **New Application** button on hello top of hello dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="e70bc-133">Hello keresési mezőbe, írja be a **Skillport**.</span><span class="sxs-lookup"><span data-stu-id="e70bc-133">In hello search box, type **Skillport**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-skillport-tutorial/tutorial_skillport_search.png)

5. <span data-ttu-id="e70bc-135">A hello eredmények panelen válassza ki a **Skillport**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="e70bc-135">In hello results panel, select **Skillport**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-skillport-tutorial/tutorial_skillport_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="e70bc-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="e70bc-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="e70bc-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Skillport.</span><span class="sxs-lookup"><span data-stu-id="e70bc-138">In this section, you configure and test Azure AD single sign-on with Skillport based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="e70bc-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Skillport tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="e70bc-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Skillport is tooa user in Azure AD.</span></span> <span data-ttu-id="e70bc-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Skillport közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="e70bc-140">In other words, a link relationship between an Azure AD user and hello related user in Skillport needs toobe established.</span></span>

<span data-ttu-id="e70bc-141">Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** a Skillport.</span><span class="sxs-lookup"><span data-stu-id="e70bc-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Skillport.</span></span>

<span data-ttu-id="e70bc-142">tooconfigure és az Azure AD az egyszeri bejelentkezés Skillport-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="e70bc-142">tooconfigure and test Azure AD single sign-on with Skillport, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="e70bc-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="e70bc-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="e70bc-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e70bc-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="e70bc-145">**[Skillport tesztfelhasználó létrehozása](#creating-a-skillport-test-user)**  -toohave egy megfelelője a Britta Simon a Skillport, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="e70bc-145">**[Creating a Skillport test user](#creating-a-skillport-test-user)** - toohave a counterpart of Britta Simon in Skillport that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="e70bc-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="e70bc-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="e70bc-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="e70bc-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="e70bc-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e70bc-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="e70bc-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Skillport alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="e70bc-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Skillport application.</span></span>

<span data-ttu-id="e70bc-150">**az Azure AD tooconfigure egyszeri bejelentkezést a Skillport, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="e70bc-150">**tooconfigure Azure AD single sign-on with Skillport, perform hello following steps:**</span></span>

1. <span data-ttu-id="e70bc-151">Az Azure portál, a hello hello **Skillport** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="e70bc-151">In hello Azure  portal, on hello **Skillport** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="e70bc-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="e70bc-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-skillport-tutorial/tutorial_skillport_samlbase.png)

3. <span data-ttu-id="e70bc-155">A hello **Skillport tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="e70bc-155">On hello **Skillport Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-skillport-tutorial/tutorial_skillport_url.png)

    <span data-ttu-id="e70bc-157">a.</span><span class="sxs-lookup"><span data-stu-id="e70bc-157">a.</span></span> <span data-ttu-id="e70bc-158">A hello **bejelentkezési URL-cím** szövegmező, írja be egy URL-CÍMÉT a következő hello mintákra:</span><span class="sxs-lookup"><span data-stu-id="e70bc-158">In hello **Sign-on URL** textbox, type a URL using hello following patterns:</span></span>
      
      <span data-ttu-id="e70bc-159">EU Datacenter:`https://<subdomain>.skillport.eu`</span><span class="sxs-lookup"><span data-stu-id="e70bc-159">EU Datacenter: `https://<subdomain>.skillport.eu`</span></span>
   
      <span data-ttu-id="e70bc-160">USA Datacenter:`https://<subdomain>.skillport.com`</span><span class="sxs-lookup"><span data-stu-id="e70bc-160">US Datacenter: `https://<subdomain>.skillport.com`</span></span>
   
    <span data-ttu-id="e70bc-161">b.</span><span class="sxs-lookup"><span data-stu-id="e70bc-161">b.</span></span> <span data-ttu-id="e70bc-162">A hello **válasz URL-CÍMEN** szövegmező, írja be egy URL-CÍMÉT a következő hello mintákra:</span><span class="sxs-lookup"><span data-stu-id="e70bc-162">In hello **Reply URL** textbox, type a URL using hello following patterns:</span></span>
    
      <span data-ttu-id="e70bc-163">EU Datacenter:`https://<subdomain>.skillport.eu/adfs/ls/`</span><span class="sxs-lookup"><span data-stu-id="e70bc-163">EU Datacenter: `https://<subdomain>.skillport.eu/adfs/ls/`</span></span>
    
      <span data-ttu-id="e70bc-164">USA Datacenter:`https://<subdomain>.skillport.com/sp/ACS.saml2`</span><span class="sxs-lookup"><span data-stu-id="e70bc-164">US Datacenter: `https://<subdomain>.skillport.com/sp/ACS.saml2`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="e70bc-165">Ezek az értékek nincsenek hello valós.</span><span class="sxs-lookup"><span data-stu-id="e70bc-165">These values are not hello real.</span></span> <span data-ttu-id="e70bc-166">Ezek az értékek frissítése hello tényleges válasz és bejelentkezési URL-címe.</span><span class="sxs-lookup"><span data-stu-id="e70bc-166">Update these values with hello actual Reply URL and Sign-On URL.</span></span> <span data-ttu-id="e70bc-167">Ügyfél [Skillport ügyfél-támogatási csoport](https://www.skillsoft.com/contact.asp) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="e70bc-167">Contact [Skillport Client support team](https://www.skillsoft.com/contact.asp) tooget these values.</span></span>
 
4. <span data-ttu-id="e70bc-168">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello XML-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="e70bc-168">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello XML file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-skillport-tutorial/tutorial_skillport_certificate.png) 

5. <span data-ttu-id="e70bc-170">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="e70bc-170">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-skillport-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="e70bc-172">tooconfigure egyszeri bejelentkezést a **Skillport** oldalon kell letöltött toosend hello **metaadatainak XML-kódja** túl[Skillport támogatási csoport](https://www.skillsoft.com/contact.asp).</span><span class="sxs-lookup"><span data-stu-id="e70bc-172">tooconfigure single sign-on on **Skillport** side, you need toosend hello downloaded **Metadata XML** too[Skillport support team](https://www.skillsoft.com/contact.asp).</span></span> <span data-ttu-id="e70bc-173">Ezek beállításához nyújt útmutatást, toohave hello SAML SSO kapcsolat mindkét oldalán megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="e70bc-173">They will set it up toohave hello SAML SSO connection set properly on both sides.</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="e70bc-174">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="e70bc-174">Creating an Azure AD test user</span></span>
<span data-ttu-id="e70bc-175">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="e70bc-175">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="e70bc-177">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="e70bc-177">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="e70bc-178">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="e70bc-178">In hello **Azure  portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-skillport-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="e70bc-180">Nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó** toodisplay hello azoknak a felhasználóknak.</span><span class="sxs-lookup"><span data-stu-id="e70bc-180">Go too**Users and groups** and click **All users** toodisplay hello list of users.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-skillport-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="e70bc-182">Hello hello párbeszédpanel, kattintson tetején **Hozzáadás** tooopen hello **felhasználói** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e70bc-182">At hello top of hello dialog, click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-skillport-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="e70bc-184">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="e70bc-184">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-skillport-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="e70bc-186">a.</span><span class="sxs-lookup"><span data-stu-id="e70bc-186">a.</span></span> <span data-ttu-id="e70bc-187">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="e70bc-187">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="e70bc-188">b.</span><span class="sxs-lookup"><span data-stu-id="e70bc-188">b.</span></span> <span data-ttu-id="e70bc-189">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="e70bc-189">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="e70bc-190">c.</span><span class="sxs-lookup"><span data-stu-id="e70bc-190">c.</span></span> <span data-ttu-id="e70bc-191">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="e70bc-191">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="e70bc-192">d.</span><span class="sxs-lookup"><span data-stu-id="e70bc-192">d.</span></span> <span data-ttu-id="e70bc-193">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="e70bc-193">Click **Create**.</span></span>
 
### <a name="creating-a-skillport-test-user"></a><span data-ttu-id="e70bc-194">Skillport tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="e70bc-194">Creating a Skillport test user</span></span>

<span data-ttu-id="e70bc-195">A sorrend toocreate Skillport tesztfelhasználó toocontact szükség van [Skillport támogatási csoport](https://www.skillsoft.com/contact.asp) végfelhasználói toohello szükség szerint több üzleti forgatókönyvek rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="e70bc-195">In order toocreate Skillport test user, you need toocontact [Skillport support team](https://www.skillsoft.com/contact.asp) as they have multiple business scenarios according toohello requirement of end user.</span></span> <span data-ttu-id="e70bc-196">Ezek konfigurálása hello felhasználók egyeztetését követően.</span><span class="sxs-lookup"><span data-stu-id="e70bc-196">They will configure it after discussion with hello users.</span></span>  

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="e70bc-197">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="e70bc-197">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="e70bc-198">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooSkillport megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="e70bc-198">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooSkillport.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="e70bc-200">**tooassign Britta Simon tooSkillport, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="e70bc-200">**tooassign Britta Simon tooSkillport, perform hello following steps:**</span></span>

1. <span data-ttu-id="e70bc-201">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="e70bc-201">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="e70bc-203">Hello alkalmazások listában válassza ki a **Skillport**.</span><span class="sxs-lookup"><span data-stu-id="e70bc-203">In hello applications list, select **Skillport**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-skillport-tutorial/tutorial_skillport_app.png) 

3. <span data-ttu-id="e70bc-205">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="e70bc-205">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="e70bc-207">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="e70bc-207">Click **Add** button.</span></span> <span data-ttu-id="e70bc-208">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e70bc-208">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="e70bc-210">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="e70bc-210">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="e70bc-211">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e70bc-211">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="e70bc-212">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e70bc-212">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="e70bc-213">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="e70bc-213">Testing single sign-on</span></span>

<span data-ttu-id="e70bc-214">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="e70bc-214">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="e70bc-215">Ha a hozzáférési Panel hello hello Skillport csempe gombra kattint, automatikusan bejelentkezett tooyour Skillport alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="e70bc-215">When you click hello Skillport tile in hello Access Panel, you should get automatically signed-on tooyour Skillport application.</span></span>
<span data-ttu-id="e70bc-216">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="e70bc-216">For more information about the Access Panel, see [Introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="e70bc-217">További források</span><span class="sxs-lookup"><span data-stu-id="e70bc-217">Additional resources</span></span>

* [<span data-ttu-id="e70bc-218">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="e70bc-218">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="e70bc-219">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="e70bc-219">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-skillport-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-skillport-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-skillport-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-skillport-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-skillport-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-skillport-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-skillport-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-skillport-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-skillport-tutorial/tutorial_general_203.png

