---
title: "Oktatóanyag: Azure Active Directoryval integrált Showpad |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Showpad között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 48b6bee0-dbc5-4863-964d-75b25e517741
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 2c8c306b4b94c368a93f92123d3abe9fe35167db
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-showpad"></a><span data-ttu-id="4daf1-103">Oktatóanyag: Azure Active Directoryval integrált Showpad</span><span class="sxs-lookup"><span data-stu-id="4daf1-103">Tutorial: Azure Active Directory integration with Showpad</span></span>

<span data-ttu-id="4daf1-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Showpad az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="4daf1-104">In this tutorial, you learn how toointegrate Showpad with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="4daf1-105">Showpad integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="4daf1-105">Integrating Showpad with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="4daf1-106">Megadhatja a hozzáférés tooShowpad rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="4daf1-106">You can control in Azure AD who has access tooShowpad</span></span>
- <span data-ttu-id="4daf1-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooShowpad (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="4daf1-107">You can enable your users tooautomatically get signed-on tooShowpad (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="4daf1-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="4daf1-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="4daf1-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="4daf1-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4daf1-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="4daf1-110">Prerequisites</span></span>

<span data-ttu-id="4daf1-111">az Azure AD integrálása Showpad tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="4daf1-111">tooconfigure Azure AD integration with Showpad, you need hello following items:</span></span>

- <span data-ttu-id="4daf1-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="4daf1-112">An Azure AD subscription</span></span>
- <span data-ttu-id="4daf1-113">Egy Showpad egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="4daf1-113">A Showpad single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="4daf1-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="4daf1-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="4daf1-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="4daf1-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="4daf1-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="4daf1-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="4daf1-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="4daf1-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="4daf1-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="4daf1-118">Scenario description</span></span>
<span data-ttu-id="4daf1-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="4daf1-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="4daf1-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="4daf1-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="4daf1-121">Hello gyűjteményből Showpad hozzáadása</span><span class="sxs-lookup"><span data-stu-id="4daf1-121">Adding Showpad from hello gallery</span></span>
2. <span data-ttu-id="4daf1-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="4daf1-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-showpad-from-hello-gallery"></a><span data-ttu-id="4daf1-123">Hello gyűjteményből Showpad hozzáadása</span><span class="sxs-lookup"><span data-stu-id="4daf1-123">Adding Showpad from hello gallery</span></span>

<span data-ttu-id="4daf1-124">tooconfigure hello integrációja Showpad az Azure AD-be, meg kell tooadd Showpad hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="4daf1-124">tooconfigure hello integration of Showpad into Azure AD, you need tooadd Showpad from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="4daf1-125">**tooadd Showpad hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="4daf1-125">**tooadd Showpad from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="4daf1-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="4daf1-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="4daf1-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="4daf1-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="4daf1-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="4daf1-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="4daf1-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="4daf1-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="4daf1-133">Hello keresési mezőbe, írja be a **Showpad**.</span><span class="sxs-lookup"><span data-stu-id="4daf1-133">In hello search box, type **Showpad**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_search.png)

5. <span data-ttu-id="4daf1-135">A hello eredmények panelen válassza ki a **Showpad**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="4daf1-135">In hello results panel, select **Showpad**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="4daf1-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="4daf1-137">Configuring and testing Azure AD single sign-on</span></span>

<span data-ttu-id="4daf1-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján Showpad</span><span class="sxs-lookup"><span data-stu-id="4daf1-138">In this section, you configure and test Azure AD single sign-on with Showpad based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="4daf1-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Showpad tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="4daf1-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Showpad is tooa user in Azure AD.</span></span> <span data-ttu-id="4daf1-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Showpad közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="4daf1-140">In other words, a link relationship between an Azure AD user and hello related user in Showpad needs toobe established.</span></span>

<span data-ttu-id="4daf1-141">Showpad, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="4daf1-141">In Showpad, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="4daf1-142">tooconfigure és az Azure AD az egyszeri bejelentkezés Showpad-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="4daf1-142">tooconfigure and test Azure AD single sign-on with Showpad, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="4daf1-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="4daf1-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="4daf1-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="4daf1-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="4daf1-145">**[Showpad tesztfelhasználó létrehozása](#creating-a-showpad-test-user)**  -toohave egy megfelelője a Britta Simon a Showpad, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="4daf1-145">**[Creating a Showpad test user](#creating-a-showpad-test-user)** - toohave a counterpart of Britta Simon in Showpad that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="4daf1-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="4daf1-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="4daf1-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="4daf1-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="4daf1-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="4daf1-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="4daf1-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Showpad alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="4daf1-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Showpad application.</span></span>

<span data-ttu-id="4daf1-150">**az Azure AD tooconfigure egyszeri bejelentkezést a Showpad, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="4daf1-150">**tooconfigure Azure AD single sign-on with Showpad, perform hello following steps:**</span></span>

1. <span data-ttu-id="4daf1-151">Az Azure portál, a hello hello **Showpad** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="4daf1-151">In hello Azure portal, on hello **Showpad** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="4daf1-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="4daf1-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_samlbase.png)

3. <span data-ttu-id="4daf1-155">A hello **Showpad tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="4daf1-155">On hello **Showpad Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_url.png)

    <span data-ttu-id="4daf1-157">a.</span><span class="sxs-lookup"><span data-stu-id="4daf1-157">a.</span></span> <span data-ttu-id="4daf1-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<comapany-name>.showpad.biz/login`</span><span class="sxs-lookup"><span data-stu-id="4daf1-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<comapany-name>.showpad.biz/login`</span></span>

    <span data-ttu-id="4daf1-159">b.</span><span class="sxs-lookup"><span data-stu-id="4daf1-159">b.</span></span> <span data-ttu-id="4daf1-160">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<company-name>.showpad.biz`</span><span class="sxs-lookup"><span data-stu-id="4daf1-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<company-name>.showpad.biz`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="4daf1-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="4daf1-161">These values are not real.</span></span> <span data-ttu-id="4daf1-162">Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="4daf1-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="4daf1-163">Ügyfél [Showpad támogatási csoport](https://help.showpad.com) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="4daf1-163">Contact [Showpad support team](https://help.showpad.com) tooget these values.</span></span> 
 


4. <span data-ttu-id="4daf1-164">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="4daf1-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_certificate.png) 

5. <span data-ttu-id="4daf1-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="4daf1-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-showpad-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="4daf1-168">Bejelentkezés tooyour Showpad Bérlői rendszergazda.</span><span class="sxs-lookup"><span data-stu-id="4daf1-168">Sign-on tooyour Showpad tenant as an administrator.</span></span>

7. <span data-ttu-id="4daf1-169">Hello hello felső menüben kattintson a hello **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="4daf1-169">In hello menu on hello top, click hello **Settings**.</span></span>
   
    ![Alkalmazás ügyféloldali egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_001.png) 

8. <span data-ttu-id="4daf1-171">Keresse meg a túl"**egyszeri bejelentkezés**", és kattintson a"**engedélyezése**."</span><span class="sxs-lookup"><span data-stu-id="4daf1-171">Navigate too"**Single Sign-On**" and click "**Enable**."</span></span>
   
    ![Alkalmazás ügyféloldali egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_002.png)

9. <span data-ttu-id="4daf1-173">A hello **hozzáadni egy SAML 2.0** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="4daf1-173">On hello **Add a SAML 2.0 Service** dialog, perform hello following steps:</span></span>
   
    ![Alkalmazás ügyféloldali egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_003.png) 
   
    <span data-ttu-id="4daf1-175">a.</span><span class="sxs-lookup"><span data-stu-id="4daf1-175">a.</span></span> <span data-ttu-id="4daf1-176">A hello **neve** szövegmezőjét azonosító szolgáltató hello nevét (például: a vállalat nevét).</span><span class="sxs-lookup"><span data-stu-id="4daf1-176">In hello **Name** textbox, type hello name of Identifier Provider (for example: your company name).</span></span>
   
    <span data-ttu-id="4daf1-177">b.</span><span class="sxs-lookup"><span data-stu-id="4daf1-177">b.</span></span> <span data-ttu-id="4daf1-178">Mint **metaadatforrás**, jelölje be **XML**.</span><span class="sxs-lookup"><span data-stu-id="4daf1-178">As **Metadata Source**, select **XML**.</span></span>
   
    <span data-ttu-id="4daf1-179">c.</span><span class="sxs-lookup"><span data-stu-id="4daf1-179">c.</span></span> <span data-ttu-id="4daf1-180">Hello tartalom metaadatok XML-fájl, ami már letöltötte az Azure-portálon hello, másolja és illessze be hello **metaadatainak XML-kódja** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="4daf1-180">Copy hello content of metadata XML file, which you have downloaded from hello Azure portal, and then paste it into hello **Metadata XML** textbox.</span></span>
   
    <span data-ttu-id="4daf1-181">d.</span><span class="sxs-lookup"><span data-stu-id="4daf1-181">d.</span></span> <span data-ttu-id="4daf1-182">Válassza ki **automatikus-kiépítési fiókok az új felhasználók bejelentkezéskor**.</span><span class="sxs-lookup"><span data-stu-id="4daf1-182">Select **Auto-provision accounts for new users when they log in**.</span></span>
   
    <span data-ttu-id="4daf1-183">e.</span><span class="sxs-lookup"><span data-stu-id="4daf1-183">e.</span></span> <span data-ttu-id="4daf1-184">Kattintson a **nyújt**.</span><span class="sxs-lookup"><span data-stu-id="4daf1-184">Click **Submit**.</span></span>

> [!TIP]
> <span data-ttu-id="4daf1-185">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="4daf1-185">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="4daf1-186">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="4daf1-186">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="4daf1-187">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="4daf1-187">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="4daf1-188">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="4daf1-188">Creating an Azure AD test user</span></span>
<span data-ttu-id="4daf1-189">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="4daf1-189">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="4daf1-191">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="4daf1-191">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="4daf1-192">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="4daf1-192">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-showpad-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="4daf1-194">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="4daf1-194">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-showpad-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="4daf1-196">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="4daf1-196">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-showpad-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="4daf1-198">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="4daf1-198">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-showpad-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="4daf1-200">a.</span><span class="sxs-lookup"><span data-stu-id="4daf1-200">a.</span></span> <span data-ttu-id="4daf1-201">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="4daf1-201">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="4daf1-202">b.</span><span class="sxs-lookup"><span data-stu-id="4daf1-202">b.</span></span> <span data-ttu-id="4daf1-203">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="4daf1-203">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="4daf1-204">c.</span><span class="sxs-lookup"><span data-stu-id="4daf1-204">c.</span></span> <span data-ttu-id="4daf1-205">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="4daf1-205">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="4daf1-206">d.</span><span class="sxs-lookup"><span data-stu-id="4daf1-206">d.</span></span> <span data-ttu-id="4daf1-207">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="4daf1-207">Click **Create**.</span></span>
 
### <a name="creating-a-showpad-test-user"></a><span data-ttu-id="4daf1-208">Showpad tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="4daf1-208">Creating a Showpad test user</span></span>

<span data-ttu-id="4daf1-209">hello ebben a szakaszban célja toocreate Showpad Britta Simon nevű felhasználó.</span><span class="sxs-lookup"><span data-stu-id="4daf1-209">hello objective of this section is toocreate a user called Britta Simon in Showpad.</span></span> 

<span data-ttu-id="4daf1-210">Showpad támogatja közvetlenül az időponthoz kötött kiosztást.</span><span class="sxs-lookup"><span data-stu-id="4daf1-210">Showpad supports just-in-time provisioning.</span></span> <span data-ttu-id="4daf1-211">Engedélyezte a kiépítési  **[az Azure AD konfigurálása egyszeri bejelentkezéshez](#configuring-azure-ad-single-sign-on)**.</span><span class="sxs-lookup"><span data-stu-id="4daf1-211">You have enabled provisioning in **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**.</span></span> 

<span data-ttu-id="4daf1-212">Nincs ebben a szakaszban az Ön művelet elem.</span><span class="sxs-lookup"><span data-stu-id="4daf1-212">There is no action item for you in this section.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="4daf1-213">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="4daf1-213">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="4daf1-214">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooShowpad megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="4daf1-214">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooShowpad.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="4daf1-216">**tooassign Britta Simon tooShowpad, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="4daf1-216">**tooassign Britta Simon tooShowpad, perform hello following steps:**</span></span>

1. <span data-ttu-id="4daf1-217">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="4daf1-217">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="4daf1-219">Hello alkalmazások listában válassza ki a **Showpad**.</span><span class="sxs-lookup"><span data-stu-id="4daf1-219">In hello applications list, select **Showpad**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_app.png) 

3. <span data-ttu-id="4daf1-221">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="4daf1-221">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="4daf1-223">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="4daf1-223">Click **Add** button.</span></span> <span data-ttu-id="4daf1-224">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="4daf1-224">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="4daf1-226">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="4daf1-226">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="4daf1-227">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="4daf1-227">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="4daf1-228">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="4daf1-228">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="4daf1-229">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="4daf1-229">Testing single sign-on</span></span>

<span data-ttu-id="4daf1-230">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="4daf1-230">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="4daf1-231">Ha a hozzáférési Panel hello hello Showpad csempe gombra kattint, automatikusan bejelentkezett tooShowpad alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="4daf1-231">When you click hello Showpad tile in hello Access Panel, you should get automatically signed-on tooShowpad application.</span></span>
<span data-ttu-id="4daf1-232">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="4daf1-232">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4daf1-233">További források</span><span class="sxs-lookup"><span data-stu-id="4daf1-233">Additional resources</span></span>

* [<span data-ttu-id="4daf1-234">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="4daf1-234">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="4daf1-235">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="4daf1-235">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_203.png

