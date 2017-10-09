---
title: "Oktatóanyag: Azure Active Directoryval integrált SmarterU |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és SmarterU között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 95fe3212-d052-4ac8-87eb-ac5305227e85
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: a3b81b557c47e31f09e61bcf75dd23f370e642e0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-smarteru"></a><span data-ttu-id="212e0-103">Oktatóanyag: Azure Active Directoryval integrált SmarterU</span><span class="sxs-lookup"><span data-stu-id="212e0-103">Tutorial: Azure Active Directory integration with SmarterU</span></span>

<span data-ttu-id="212e0-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate SmarterU az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="212e0-104">In this tutorial, you learn how toointegrate SmarterU with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="212e0-105">SmarterU integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="212e0-105">Integrating SmarterU with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="212e0-106">Megadhatja a hozzáférés tooSmarterU rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="212e0-106">You can control in Azure AD who has access tooSmarterU</span></span>
- <span data-ttu-id="212e0-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooSmarterU (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="212e0-107">You can enable your users tooautomatically get signed-on tooSmarterU (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="212e0-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="212e0-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="212e0-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="212e0-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="212e0-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="212e0-110">Prerequisites</span></span>

<span data-ttu-id="212e0-111">az Azure AD integrálása SmarterU tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="212e0-111">tooconfigure Azure AD integration with SmarterU, you need hello following items:</span></span>

- <span data-ttu-id="212e0-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="212e0-112">An Azure AD subscription</span></span>
- <span data-ttu-id="212e0-113">Egy SmarterU egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="212e0-113">A SmarterU single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="212e0-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="212e0-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="212e0-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="212e0-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="212e0-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="212e0-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="212e0-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="212e0-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="212e0-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="212e0-118">Scenario description</span></span>
<span data-ttu-id="212e0-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="212e0-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="212e0-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="212e0-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="212e0-121">Hello gyűjteményből SmarterU hozzáadása</span><span class="sxs-lookup"><span data-stu-id="212e0-121">Adding SmarterU from hello gallery</span></span>
2. <span data-ttu-id="212e0-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="212e0-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-smarteru-from-hello-gallery"></a><span data-ttu-id="212e0-123">Hello gyűjteményből SmarterU hozzáadása</span><span class="sxs-lookup"><span data-stu-id="212e0-123">Adding SmarterU from hello gallery</span></span>
<span data-ttu-id="212e0-124">tooconfigure hello integrációja SmarterU az Azure AD-be, meg kell tooadd SmarterU hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="212e0-124">tooconfigure hello integration of SmarterU into Azure AD, you need tooadd SmarterU from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="212e0-125">**tooadd SmarterU hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="212e0-125">**tooadd SmarterU from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="212e0-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="212e0-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="212e0-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="212e0-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="212e0-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="212e0-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="212e0-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="212e0-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="212e0-133">Hello keresési mezőbe, írja be a **SmarterU**.</span><span class="sxs-lookup"><span data-stu-id="212e0-133">In hello search box, type **SmarterU**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-smarteru-tutorial/tutorial_smarteru_search.png)

5. <span data-ttu-id="212e0-135">A hello eredmények panelen válassza ki a **SmarterU**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="212e0-135">In hello results panel, select **SmarterU**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-smarteru-tutorial/tutorial_smarteru_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="212e0-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="212e0-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="212e0-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján SmarterU</span><span class="sxs-lookup"><span data-stu-id="212e0-138">In this section, you configure and test Azure AD single sign-on with SmarterU based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="212e0-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó SmarterU tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="212e0-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in SmarterU is tooa user in Azure AD.</span></span> <span data-ttu-id="212e0-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello SmarterU közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="212e0-140">In other words, a link relationship between an Azure AD user and hello related user in SmarterU needs toobe established.</span></span>

<span data-ttu-id="212e0-141">SmarterU, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="212e0-141">In SmarterU, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="212e0-142">tooconfigure és az Azure AD az egyszeri bejelentkezés SmarterU-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="212e0-142">tooconfigure and test Azure AD single sign-on with SmarterU, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="212e0-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="212e0-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="212e0-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="212e0-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="212e0-145">**[SmarterU tesztfelhasználó létrehozása](#creating-a-smarteru-test-user)**  -toohave egy megfelelője a Britta Simon a SmarterU, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="212e0-145">**[Creating a SmarterU test user](#creating-a-smarteru-test-user)** - toohave a counterpart of Britta Simon in SmarterU that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="212e0-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="212e0-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="212e0-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="212e0-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="212e0-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="212e0-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="212e0-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az SmarterU alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="212e0-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your SmarterU application.</span></span>

<span data-ttu-id="212e0-150">**az Azure AD tooconfigure egyszeri bejelentkezést a SmarterU, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="212e0-150">**tooconfigure Azure AD single sign-on with SmarterU, perform hello following steps:**</span></span>

1. <span data-ttu-id="212e0-151">Az Azure portál, a hello hello **SmarterU** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="212e0-151">In hello Azure portal, on hello **SmarterU** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="212e0-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="212e0-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-smarteru-tutorial/tutorial_smarteru_samlbase.png)

3. <span data-ttu-id="212e0-155">A hello **SmarterU tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="212e0-155">On hello **SmarterU Domain and URLs** section, perform hello following steps:</span></span> 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-smarteru-tutorial/tutorial_smarteru_url.png)

    <span data-ttu-id="212e0-157">A hello **azonosító** szövegmezőhöz típus hello URL-címe:`https://www.smarteru.com/`</span><span class="sxs-lookup"><span data-stu-id="212e0-157">In hello **Identifier** textbox, type hello URL: `https://www.smarteru.com/`</span></span>

4. <span data-ttu-id="212e0-158">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="212e0-158">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-smarteru-tutorial/tutorial_smarteru_certificate.png) 

5. <span data-ttu-id="212e0-160">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="212e0-160">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-smarteru-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="212e0-162">Egy másik webes böngészőablakban jelentkezzen tooyour SmarterU vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="212e0-162">In a different web browser window, log in tooyour SmarterU company site as an administrator.</span></span>

7. <span data-ttu-id="212e0-163">Hello hello felső eszköztárán kattintson **Fiókbeállítások**.</span><span class="sxs-lookup"><span data-stu-id="212e0-163">In hello toolbar on hello top, click **Account Settings**.</span></span>
   
    <span data-ttu-id="212e0-164">![Fiókbeállítások](./media/active-directory-saas-smarteru-tutorial/IC777326.png "Fiókbeállítások")</span><span class="sxs-lookup"><span data-stu-id="212e0-164">![Account Settings](./media/active-directory-saas-smarteru-tutorial/IC777326.png "Account Settings")</span></span>

8. <span data-ttu-id="212e0-165">Hello fiók konfigurációs lapon hajtsa végre a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="212e0-165">On hello account configuration page, perform hello following steps:</span></span>
   
    <span data-ttu-id="212e0-166">![Külső engedélyezési](./media/active-directory-saas-smarteru-tutorial/IC777327.png "külső engedélyezési")</span><span class="sxs-lookup"><span data-stu-id="212e0-166">![External Authorization](./media/active-directory-saas-smarteru-tutorial/IC777327.png "External Authorization")</span></span> 
 
      <span data-ttu-id="212e0-167">a.</span><span class="sxs-lookup"><span data-stu-id="212e0-167">a.</span></span> <span data-ttu-id="212e0-168">Válassza ki **külső engedélyezése**.</span><span class="sxs-lookup"><span data-stu-id="212e0-168">Select **Enable External Authorization**.</span></span>
  
      <span data-ttu-id="212e0-169">b.</span><span class="sxs-lookup"><span data-stu-id="212e0-169">b.</span></span> <span data-ttu-id="212e0-170">A hello **fő bejelentkezési vezérlő** szakaszban, jelölje be hello **SmarterU** fülre.</span><span class="sxs-lookup"><span data-stu-id="212e0-170">In hello **Master Login Control** section, select hello **SmarterU** tab.</span></span>
  
      <span data-ttu-id="212e0-171">c.</span><span class="sxs-lookup"><span data-stu-id="212e0-171">c.</span></span> <span data-ttu-id="212e0-172">A hello **felhasználó alapértelmezett bejelentkezési** szakaszban, jelölje be hello **SmarterU** fülre.</span><span class="sxs-lookup"><span data-stu-id="212e0-172">In hello **User Default Login** section, select hello **SmarterU** tab.</span></span>
  
      <span data-ttu-id="212e0-173">d.</span><span class="sxs-lookup"><span data-stu-id="212e0-173">d.</span></span> <span data-ttu-id="212e0-174">Válassza ki **Okta engedélyezése**.</span><span class="sxs-lookup"><span data-stu-id="212e0-174">Select **Enable Okta**.</span></span>
  
      <span data-ttu-id="212e0-175">e.</span><span class="sxs-lookup"><span data-stu-id="212e0-175">e.</span></span> <span data-ttu-id="212e0-176">Másolja a letöltött metaadatfájl hello hello tartalmat, és illessze be hello **Okta metaadatok** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="212e0-176">Copy hello content of hello downloaded metadata file, and then paste it into hello **Okta Metadata** textbox.</span></span>
  
      <span data-ttu-id="212e0-177">f.</span><span class="sxs-lookup"><span data-stu-id="212e0-177">f.</span></span> <span data-ttu-id="212e0-178">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="212e0-178">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="212e0-179">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="212e0-179">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="212e0-180">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="212e0-180">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="212e0-181">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="212e0-181">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="212e0-182">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="212e0-182">Creating an Azure AD test user</span></span>
<span data-ttu-id="212e0-183">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="212e0-183">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="212e0-185">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="212e0-185">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="212e0-186">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="212e0-186">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-smarteru-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="212e0-188">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="212e0-188">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-smarteru-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="212e0-190">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="212e0-190">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-smarteru-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="212e0-192">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="212e0-192">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-smarteru-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="212e0-194">a.</span><span class="sxs-lookup"><span data-stu-id="212e0-194">a.</span></span> <span data-ttu-id="212e0-195">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="212e0-195">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="212e0-196">b.</span><span class="sxs-lookup"><span data-stu-id="212e0-196">b.</span></span> <span data-ttu-id="212e0-197">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="212e0-197">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="212e0-198">c.</span><span class="sxs-lookup"><span data-stu-id="212e0-198">c.</span></span> <span data-ttu-id="212e0-199">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="212e0-199">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="212e0-200">d.</span><span class="sxs-lookup"><span data-stu-id="212e0-200">d.</span></span> <span data-ttu-id="212e0-201">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="212e0-201">Click **Create**.</span></span>
 
### <a name="creating-a-smarteru-test-user"></a><span data-ttu-id="212e0-202">SmarterU tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="212e0-202">Creating a SmarterU test user</span></span>

<span data-ttu-id="212e0-203">az Azure AD tooenable felhasználók toolog a tooSmarterU, akkor ki kell építenie SmarterU be.</span><span class="sxs-lookup"><span data-stu-id="212e0-203">tooenable Azure AD users toolog in tooSmarterU, they must be provisioned into SmarterU.</span></span>

<span data-ttu-id="212e0-204">SmarterU, ha a kézi tevékenység kiépítés.</span><span class="sxs-lookup"><span data-stu-id="212e0-204">When SmarterU, provisioning is a manual task.</span></span>

<span data-ttu-id="212e0-205">**tooprovision egy felhasználói fiókot, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="212e0-205">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="212e0-206">Jelentkezzen be tooyour **SmarterU** bérlő.</span><span class="sxs-lookup"><span data-stu-id="212e0-206">Log in tooyour **SmarterU** tenant.</span></span>

2. <span data-ttu-id="212e0-207">Nyissa meg túl**felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="212e0-207">Go too**Users**.</span></span>

3. <span data-ttu-id="212e0-208">Hello felhasználói szakaszban hajtsa végre a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="212e0-208">In hello user section, perform hello following steps:</span></span>
   
    <span data-ttu-id="212e0-209">![Új felhasználó](./media/active-directory-saas-smarteru-tutorial/IC777329.png "új felhasználó")</span><span class="sxs-lookup"><span data-stu-id="212e0-209">![New User](./media/active-directory-saas-smarteru-tutorial/IC777329.png "New User")</span></span>  

    <span data-ttu-id="212e0-210">a.</span><span class="sxs-lookup"><span data-stu-id="212e0-210">a.</span></span> <span data-ttu-id="212e0-211">Kattintson a **+ felhasználói**.</span><span class="sxs-lookup"><span data-stu-id="212e0-211">Click **+User**.</span></span>
    
    <span data-ttu-id="212e0-212">b.</span><span class="sxs-lookup"><span data-stu-id="212e0-212">b.</span></span> <span data-ttu-id="212e0-213">Típus hello kapcsolódó attribútumértékek hello Azure AD felhasználói fiók be a következő szövegmezők hello: **elsődleges E-mail**, **Alkalmazottazonosító**, **jelszó**,  **Ellenőrizze a jelszót**, **Utónév**, **vezetékneve**.</span><span class="sxs-lookup"><span data-stu-id="212e0-213">Type hello related attribute values of hello Azure AD user account into hello following textboxes: **Primary Email**, **Employee ID**, **Password**, **Verify Password**, **Given Name**, **Surname**.</span></span>
    
    <span data-ttu-id="212e0-214">c.</span><span class="sxs-lookup"><span data-stu-id="212e0-214">c.</span></span> <span data-ttu-id="212e0-215">Kattintson a **aktív**.</span><span class="sxs-lookup"><span data-stu-id="212e0-215">Click **Active**.</span></span> 
    
    <span data-ttu-id="212e0-216">d.</span><span class="sxs-lookup"><span data-stu-id="212e0-216">d.</span></span> <span data-ttu-id="212e0-217">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="212e0-217">Click **Save**.</span></span>

>[!NOTE]
><span data-ttu-id="212e0-218">Bármely más SmarterU felhasználói fiók létrehozása eszközök vagy SmarterU tooprovision által nyújtott API-k AAD felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="212e0-218">You can use any other SmarterU user account creation tools or APIs provided by SmarterU tooprovision AAD user accounts.</span></span>
> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="212e0-219">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="212e0-219">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="212e0-220">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooSmarterU megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="212e0-220">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooSmarterU.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="212e0-222">**tooassign Britta Simon tooSmarterU, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="212e0-222">**tooassign Britta Simon tooSmarterU, perform hello following steps:**</span></span>

1. <span data-ttu-id="212e0-223">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="212e0-223">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="212e0-225">Hello alkalmazások listában válassza ki a **SmarterU**.</span><span class="sxs-lookup"><span data-stu-id="212e0-225">In hello applications list, select **SmarterU**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-smarteru-tutorial/tutorial_smarteru_app.png) 

3. <span data-ttu-id="212e0-227">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="212e0-227">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="212e0-229">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="212e0-229">Click **Add** button.</span></span> <span data-ttu-id="212e0-230">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="212e0-230">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="212e0-232">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="212e0-232">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="212e0-233">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="212e0-233">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="212e0-234">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="212e0-234">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="212e0-235">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="212e0-235">Testing single sign-on</span></span>

<span data-ttu-id="212e0-236">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="212e0-236">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>
 
<span data-ttu-id="212e0-237">Ha a hozzáférési Panel hello hello SmarterU csempe gombra kattint, automatikusan bejelentkezett tooyour SmarterU alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="212e0-237">When you click hello SmarterU tile in hello Access Panel, you should get automatically signed-on tooyour SmarterU application.</span></span>
<span data-ttu-id="212e0-238">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="212e0-238">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 


## <a name="additional-resources"></a><span data-ttu-id="212e0-239">További források</span><span class="sxs-lookup"><span data-stu-id="212e0-239">Additional resources</span></span>

* [<span data-ttu-id="212e0-240">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="212e0-240">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="212e0-241">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="212e0-241">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-smarteru-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-smarteru-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-smarteru-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-smarteru-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-smarteru-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-smarteru-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-smarteru-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-smarteru-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-smarteru-tutorial/tutorial_general_203.png

