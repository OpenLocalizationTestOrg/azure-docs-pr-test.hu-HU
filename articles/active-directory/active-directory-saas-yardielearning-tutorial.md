---
title: "Oktatóanyag: Azure Active Directoryval integrált Yardi tanulási |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Yardi tanulási között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 7ea58b54-ec5b-4576-8586-814b11d0f4fb
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: jeedes
ms.openlocfilehash: 47c95fe024e76a67aa5c5b3ee6f81cbafc50ff07
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-yardi-elearning"></a><span data-ttu-id="4334d-103">Oktatóanyag: Azure Active Directoryval integrált Yardi tanulási</span><span class="sxs-lookup"><span data-stu-id="4334d-103">Tutorial: Azure Active Directory integration with Yardi eLearning</span></span>

<span data-ttu-id="4334d-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Yardi tanulási az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="4334d-104">In this tutorial, you learn how toointegrate Yardi eLearning with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="4334d-105">Yardi integrálása az Azure ad-val tanulási tesz lehetővé a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="4334d-105">Integrating Yardi eLearning with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="4334d-106">Megadhatja a hozzáférés tooYardi tanulási rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="4334d-106">You can control in Azure AD who has access tooYardi eLearning</span></span>
- <span data-ttu-id="4334d-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooYardi tanulási (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="4334d-107">You can enable your users tooautomatically get signed-on tooYardi eLearning (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="4334d-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="4334d-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="4334d-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="4334d-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4334d-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="4334d-110">Prerequisites</span></span>

<span data-ttu-id="4334d-111">az Azure AD integrálása Yardi tanulási tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="4334d-111">tooconfigure Azure AD integration with Yardi eLearning, you need hello following items:</span></span>

- <span data-ttu-id="4334d-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="4334d-112">An Azure AD subscription</span></span>
- <span data-ttu-id="4334d-113">Egy Yardi tanulási egyszeri bejelentkezés engedélyezett előfizetés</span><span class="sxs-lookup"><span data-stu-id="4334d-113">A Yardi eLearning single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="4334d-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="4334d-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="4334d-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="4334d-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="4334d-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="4334d-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="4334d-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="4334d-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="4334d-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="4334d-118">Scenario description</span></span>
<span data-ttu-id="4334d-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="4334d-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="4334d-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="4334d-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="4334d-121">Hello gyűjteményből Yardi tanulási hozzáadása</span><span class="sxs-lookup"><span data-stu-id="4334d-121">Adding Yardi eLearning from hello gallery</span></span>
2. <span data-ttu-id="4334d-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="4334d-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-yardi-elearning-from-hello-gallery"></a><span data-ttu-id="4334d-123">Hello gyűjteményből Yardi tanulási hozzáadása</span><span class="sxs-lookup"><span data-stu-id="4334d-123">Adding Yardi eLearning from hello gallery</span></span>
<span data-ttu-id="4334d-124">tooconfigure hello integrációja Yardi tanulási az Azure AD-be, meg kell tooadd Yardi tanulási hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="4334d-124">tooconfigure hello integration of Yardi eLearning into Azure AD, you need tooadd Yardi eLearning from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="4334d-125">**tooadd Yardi tanulási hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="4334d-125">**tooadd Yardi eLearning from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="4334d-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="4334d-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="4334d-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="4334d-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="4334d-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="4334d-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="4334d-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="4334d-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="4334d-133">Hello keresési mezőbe, írja be a **Yardi tanulási**.</span><span class="sxs-lookup"><span data-stu-id="4334d-133">In hello search box, type **Yardi eLearning**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-yardielearning-tutorial/tutorial_yardielearning_search.png)

5. <span data-ttu-id="4334d-135">A hello eredmények panelen válassza ki a **Yardi tanulási**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="4334d-135">In hello results panel, select **Yardi eLearning**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-yardielearning-tutorial/tutorial_yardielearning_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="4334d-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="4334d-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="4334d-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján Yardi tanulási</span><span class="sxs-lookup"><span data-stu-id="4334d-138">In this section, you configure and test Azure AD single sign-on with Yardi eLearning based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="4334d-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Yardi tanulási tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="4334d-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Yardi eLearning is tooa user in Azure AD.</span></span> <span data-ttu-id="4334d-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Yardi tanulási közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="4334d-140">In other words, a link relationship between an Azure AD user and hello related user in Yardi eLearning needs toobe established.</span></span>

<span data-ttu-id="4334d-141">Yardi tanulási, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="4334d-141">In Yardi eLearning, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="4334d-142">tooconfigure és az Azure AD az egyszeri bejelentkezés Yardi tanulási-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="4334d-142">tooconfigure and test Azure AD single sign-on with Yardi eLearning, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="4334d-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="4334d-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="4334d-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="4334d-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="4334d-145">**[Yardi tanulási tesztfelhasználó létrehozása](#creating-a-yardi-elearning-test-user)**  -toohave Britta Simon egy partner, a Yardi tanulási, amely a felhasználó csatolt toohello az Azure AD-ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="4334d-145">**[Creating a Yardi eLearning test user](#creating-a-yardi-elearning-test-user)** - toohave a counterpart of Britta Simon in Yardi eLearning that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="4334d-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="4334d-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="4334d-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="4334d-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="4334d-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="4334d-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="4334d-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Yardi tanulási alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="4334d-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Yardi eLearning application.</span></span>

<span data-ttu-id="4334d-150">**tooconfigure az Azure AD egyszeri bejelentkezést a Yardi tanulási, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="4334d-150">**tooconfigure Azure AD single sign-on with Yardi eLearning, perform hello following steps:**</span></span>

1. <span data-ttu-id="4334d-151">Az Azure portál, a hello hello **Yardi tanulási** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="4334d-151">In hello Azure portal, on hello **Yardi eLearning** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="4334d-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="4334d-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-yardielearning-tutorial/tutorial_yardielearning_samlbase.png)

3. <span data-ttu-id="4334d-155">A hello **Yardi tanulási tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="4334d-155">On hello **Yardi eLearning Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-yardielearning-tutorial/tutorial_yardielearning_url.png)

    <span data-ttu-id="4334d-157">a.</span><span class="sxs-lookup"><span data-stu-id="4334d-157">a.</span></span> <span data-ttu-id="4334d-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<companyname>.yardielearning.com/login`</span><span class="sxs-lookup"><span data-stu-id="4334d-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.yardielearning.com/login`</span></span>

    <span data-ttu-id="4334d-159">b.</span><span class="sxs-lookup"><span data-stu-id="4334d-159">b.</span></span> <span data-ttu-id="4334d-160">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<companyname>.yardielearning.com/trust`</span><span class="sxs-lookup"><span data-stu-id="4334d-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<companyname>.yardielearning.com/trust`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="4334d-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="4334d-161">These values are not real.</span></span> <span data-ttu-id="4334d-162">Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="4334d-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="4334d-163">Ügyfél [Yardi tanulási ügyfél támogatási csoport](mailto:elearning@yardi.com) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="4334d-163">Contact [Yardi eLearning Client support team](mailto:elearning@yardi.com) tooget these values.</span></span> 
 
4. <span data-ttu-id="4334d-164">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="4334d-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-yardielearning-tutorial/tutorial_yardielearning_certificate.png) 

5. <span data-ttu-id="4334d-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="4334d-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-yardielearning-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="4334d-168">tooconfigure egyszeri bejelentkezést a **Yardi tanulási** oldalon kell letöltött toosend hello **metaadatainak XML-kódja** túl[Yardi tanulási támogatási csoport](mailto:elearning@yardi.com).</span><span class="sxs-lookup"><span data-stu-id="4334d-168">tooconfigure single sign-on on **Yardi eLearning** side, you need toosend hello downloaded **Metadata XML** too[Yardi eLearning support team](mailto:elearning@yardi.com).</span></span> 

> [!TIP]
> <span data-ttu-id="4334d-169">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="4334d-169">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="4334d-170">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="4334d-170">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="4334d-171">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="4334d-171">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="4334d-172">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="4334d-172">Creating an Azure AD test user</span></span>
<span data-ttu-id="4334d-173">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="4334d-173">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="4334d-175">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="4334d-175">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="4334d-176">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="4334d-176">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-yardielearning-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="4334d-178">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="4334d-178">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-yardielearning-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="4334d-180">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="4334d-180">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-yardielearning-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="4334d-182">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="4334d-182">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-yardielearning-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="4334d-184">a.</span><span class="sxs-lookup"><span data-stu-id="4334d-184">a.</span></span> <span data-ttu-id="4334d-185">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="4334d-185">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="4334d-186">b.</span><span class="sxs-lookup"><span data-stu-id="4334d-186">b.</span></span> <span data-ttu-id="4334d-187">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="4334d-187">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="4334d-188">c.</span><span class="sxs-lookup"><span data-stu-id="4334d-188">c.</span></span> <span data-ttu-id="4334d-189">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="4334d-189">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="4334d-190">d.</span><span class="sxs-lookup"><span data-stu-id="4334d-190">d.</span></span> <span data-ttu-id="4334d-191">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="4334d-191">Click **Create**.</span></span>
 
### <a name="creating-a-yardi-elearning-test-user"></a><span data-ttu-id="4334d-192">Yardi tanulási tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="4334d-192">Creating a Yardi eLearning test user</span></span>

<span data-ttu-id="4334d-193">hello ebben a szakaszban célja toocreate Yardi tanulási Britta Simon nevű felhasználó.</span><span class="sxs-lookup"><span data-stu-id="4334d-193">hello objective of this section is toocreate a user called Britta Simon in Yardi eLearning.</span></span> <span data-ttu-id="4334d-194">Yardi tanulási támogatja just-in-time kiosztást, amely alapértelmezés szerint van engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="4334d-194">Yardi eLearning supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="4334d-195">Nincs ebben a szakaszban az Ön művelet elem.</span><span class="sxs-lookup"><span data-stu-id="4334d-195">There is no action item for you in this section.</span></span> <span data-ttu-id="4334d-196">Új felhasználó jön létre egy kísérlet tooaccess Yardi tanulási során, ha még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="4334d-196">A new user is created during an attempt tooaccess Yardi eLearning if it doesn't exist yet.</span></span> 

>[!NOTE]
><span data-ttu-id="4334d-197">A felhasználó toocreate manuálisan kell, ha szüksége van-e toocontact hello [Yardi tanulási támogatási csoport](mailto:elearning@yardi.com).</span><span class="sxs-lookup"><span data-stu-id="4334d-197">If you need toocreate a user manually, you need toocontact hello [Yardi eLearning support team](mailto:elearning@yardi.com).</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="4334d-198">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="4334d-198">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="4334d-199">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooYardi tanulási megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="4334d-199">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooYardi eLearning.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="4334d-201">**tooassign Britta Simon tooYardi tanulási, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="4334d-201">**tooassign Britta Simon tooYardi eLearning, perform hello following steps:**</span></span>

1. <span data-ttu-id="4334d-202">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="4334d-202">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="4334d-204">Hello alkalmazások listában válassza ki a **Yardi tanulási**.</span><span class="sxs-lookup"><span data-stu-id="4334d-204">In hello applications list, select **Yardi eLearning**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-yardielearning-tutorial/tutorial_yardielearning_app.png) 

3. <span data-ttu-id="4334d-206">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="4334d-206">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="4334d-208">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="4334d-208">Click **Add** button.</span></span> <span data-ttu-id="4334d-209">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="4334d-209">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="4334d-211">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="4334d-211">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="4334d-212">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="4334d-212">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="4334d-213">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="4334d-213">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="4334d-214">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="4334d-214">Testing single sign-on</span></span>

<span data-ttu-id="4334d-215">hello ebben a szakaszban célja tootest az egyszeri bejelentkezés konfigurációs használatával hello a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="4334d-215">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="4334d-216">Hello Yardi tanulási csempe a hozzáférési Panel hello kattintáskor automatikusan bejelentkezett tooyour Yardi tanulási alkalmazás kapja meg.</span><span class="sxs-lookup"><span data-stu-id="4334d-216">When you click hello Yardi eLearning tile in hello Access Panel, you should get automatically signed-on tooyour Yardi eLearning application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="4334d-217">További források</span><span class="sxs-lookup"><span data-stu-id="4334d-217">Additional resources</span></span>

* [<span data-ttu-id="4334d-218">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="4334d-218">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="4334d-219">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="4334d-219">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-yardielearning-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-yardielearning-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-yardielearning-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-yardielearning-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-yardielearning-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-yardielearning-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-yardielearning-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-yardielearning-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-yardielearning-tutorial/tutorial_general_203.png

