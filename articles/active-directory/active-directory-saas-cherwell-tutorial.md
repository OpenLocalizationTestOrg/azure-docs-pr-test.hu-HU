---
title: "Oktatóanyag: Azure Active Directoryval integrált Cherwell |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Cherwell között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: ad891f99-179e-4487-834d-35f3bc01c1ec
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/03/2017
ms.author: jeedes
ms.openlocfilehash: a67b3d346a6f7b43a7e87fb4d9c533f9363f2e02
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-cherwell"></a><span data-ttu-id="6bc3f-103">Oktatóanyag: Azure Active Directoryval integrált Cherwell</span><span class="sxs-lookup"><span data-stu-id="6bc3f-103">Tutorial: Azure Active Directory integration with Cherwell</span></span>

<span data-ttu-id="6bc3f-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Cherwell az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="6bc3f-104">In this tutorial, you learn how toointegrate Cherwell with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="6bc3f-105">Cherwell integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="6bc3f-105">Integrating Cherwell with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="6bc3f-106">Megadhatja a hozzáférés tooCherwell rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="6bc3f-106">You can control in Azure AD who has access tooCherwell</span></span>
- <span data-ttu-id="6bc3f-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooCherwell (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="6bc3f-107">You can enable your users tooautomatically get signed-on tooCherwell (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="6bc3f-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="6bc3f-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="6bc3f-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="6bc3f-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6bc3f-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="6bc3f-110">Prerequisites</span></span>

<span data-ttu-id="6bc3f-111">az Azure AD integrálása Cherwell tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="6bc3f-111">tooconfigure Azure AD integration with Cherwell, you need hello following items:</span></span>

- <span data-ttu-id="6bc3f-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="6bc3f-112">An Azure AD subscription</span></span>
- <span data-ttu-id="6bc3f-113">Egy Cherwell egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="6bc3f-113">A Cherwell single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="6bc3f-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="6bc3f-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="6bc3f-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="6bc3f-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="6bc3f-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="6bc3f-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="6bc3f-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="6bc3f-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="6bc3f-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="6bc3f-118">Scenario description</span></span>
<span data-ttu-id="6bc3f-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="6bc3f-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="6bc3f-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="6bc3f-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="6bc3f-121">Hello gyűjteményből Cherwell hozzáadása</span><span class="sxs-lookup"><span data-stu-id="6bc3f-121">Adding Cherwell from hello gallery</span></span>
2. <span data-ttu-id="6bc3f-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="6bc3f-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-cherwell-from-hello-gallery"></a><span data-ttu-id="6bc3f-123">Hello gyűjteményből Cherwell hozzáadása</span><span class="sxs-lookup"><span data-stu-id="6bc3f-123">Adding Cherwell from hello gallery</span></span>
<span data-ttu-id="6bc3f-124">tooconfigure hello integrációja Cherwell az Azure AD-be, meg kell tooadd Cherwell hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="6bc3f-124">tooconfigure hello integration of Cherwell into Azure AD, you need tooadd Cherwell from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="6bc3f-125">**tooadd Cherwell hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="6bc3f-125">**tooadd Cherwell from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="6bc3f-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="6bc3f-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="6bc3f-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="6bc3f-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="6bc3f-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="6bc3f-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="6bc3f-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="6bc3f-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="6bc3f-133">Hello keresési mezőbe, írja be a **Cherwell**.</span><span class="sxs-lookup"><span data-stu-id="6bc3f-133">In hello search box, type **Cherwell**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cherwell-tutorial/tutorial_cherwell_search.png)

5. <span data-ttu-id="6bc3f-135">A hello eredmények panelen válassza ki a **Cherwell**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="6bc3f-135">In hello results panel, select **Cherwell**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cherwell-tutorial/tutorial_cherwell_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="6bc3f-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="6bc3f-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="6bc3f-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján Cherwell</span><span class="sxs-lookup"><span data-stu-id="6bc3f-138">In this section, you configure and test Azure AD single sign-on with Cherwell based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="6bc3f-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Cherwell tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="6bc3f-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Cherwell is tooa user in Azure AD.</span></span> <span data-ttu-id="6bc3f-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Cherwell közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="6bc3f-140">In other words, a link relationship between an Azure AD user and hello related user in Cherwell needs toobe established.</span></span>

<span data-ttu-id="6bc3f-141">Cherwell, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="6bc3f-141">In Cherwell, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="6bc3f-142">tooconfigure és az Azure AD az egyszeri bejelentkezés Cherwell-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="6bc3f-142">tooconfigure and test Azure AD single sign-on with Cherwell, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="6bc3f-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="6bc3f-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="6bc3f-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="6bc3f-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="6bc3f-145">**[Cherwell tesztfelhasználó létrehozása](#creating-a-cherwell-test-user)**  -toohave egy megfelelője a Britta Simon a Cherwell, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="6bc3f-145">**[Creating a Cherwell test user](#creating-a-cherwell-test-user)** - toohave a counterpart of Britta Simon in Cherwell that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="6bc3f-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="6bc3f-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="6bc3f-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="6bc3f-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="6bc3f-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="6bc3f-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="6bc3f-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Cherwell alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="6bc3f-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Cherwell application.</span></span>

<span data-ttu-id="6bc3f-150">**az Azure AD tooconfigure egyszeri bejelentkezést a Cherwell, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="6bc3f-150">**tooconfigure Azure AD single sign-on with Cherwell, perform hello following steps:**</span></span>

1. <span data-ttu-id="6bc3f-151">Az Azure portál, a hello hello **Cherwell** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="6bc3f-151">In hello Azure portal, on hello **Cherwell** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="6bc3f-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="6bc3f-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cherwell-tutorial/tutorial_cherwell_samlbase.png)

3. <span data-ttu-id="6bc3f-155">A hello **Cherwell tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="6bc3f-155">On hello **Cherwell Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cherwell-tutorial/tutorial_cherwell_url.png)

    <span data-ttu-id="6bc3f-157">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<companyname>.cherwellondemand.com/cherwellclient`</span><span class="sxs-lookup"><span data-stu-id="6bc3f-157">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.cherwellondemand.com/cherwellclient`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="6bc3f-158">Ez az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="6bc3f-158">This value is not real.</span></span> <span data-ttu-id="6bc3f-159">Ez az érték frissítése hello tényleges bejelentkezési URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="6bc3f-159">Update this value with hello actual Sign-on URL.</span></span> <span data-ttu-id="6bc3f-160">Ügyfél [Cherwell támogatási csoport](https://csm.cherwell.com/contact) tooget ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="6bc3f-160">Contact [Cherwell support team](https://csm.cherwell.com/contact) tooget this value.</span></span>
 
4. <span data-ttu-id="6bc3f-161">A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="6bc3f-161">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cherwell-tutorial/tutorial_cherwell_certificate.png) 

5. <span data-ttu-id="6bc3f-163">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="6bc3f-163">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cherwell-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="6bc3f-165">A hello **Cherwell konfigurációs** kattintson **konfigurálása Cherwell** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="6bc3f-165">On hello **Cherwell Configuration** section, click **Configure Cherwell** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="6bc3f-166">Másolás hello **SAML Entitásazonosító és SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="6bc3f-166">Copy hello **SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cherwell-tutorial/tutorial_cherwell_configure.png) 

7. <span data-ttu-id="6bc3f-168">tooconfigure egyszeri bejelentkezést a **Cherwell** oldalon kell letöltött toosend hello **tanúsítvány (Base64)**, **SAML-alapú egyszeri bejelentkezési URL-címe**, és  **SAML Entitásazonosító** túl[Cherwell támogatási csoport](https://csm.cherwell.com/contact).</span><span class="sxs-lookup"><span data-stu-id="6bc3f-168">tooconfigure single sign-on on **Cherwell** side, you need toosend hello downloaded **Certificate (Base64)**, **SAML Single Sign-On Service URL**, and **SAML Entity ID** too[Cherwell support team](https://csm.cherwell.com/contact).</span></span> 

    >[!NOTE]
    ><span data-ttu-id="6bc3f-169">A Cherwell támogatási csoport rendelkezik toodo hello tényleges SSO konfigurációval.</span><span class="sxs-lookup"><span data-stu-id="6bc3f-169">Your Cherwell support team has toodo hello actual SSO configuration.</span></span> <span data-ttu-id="6bc3f-170">Ha egyszeri bejelentkezés engedélyezve van az előfizetés értesítést kap.</span><span class="sxs-lookup"><span data-stu-id="6bc3f-170">You will get a notification when SSO has been enabled for your subscription.</span></span>
    > 
    
> [!TIP]
> <span data-ttu-id="6bc3f-171">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="6bc3f-171">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="6bc3f-172">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="6bc3f-172">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="6bc3f-173">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="6bc3f-173">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="6bc3f-174">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="6bc3f-174">Creating an Azure AD test user</span></span>
<span data-ttu-id="6bc3f-175">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="6bc3f-175">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="6bc3f-177">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="6bc3f-177">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="6bc3f-178">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="6bc3f-178">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cherwell-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="6bc3f-180">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="6bc3f-180">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cherwell-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="6bc3f-182">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="6bc3f-182">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cherwell-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="6bc3f-184">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="6bc3f-184">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cherwell-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="6bc3f-186">a.</span><span class="sxs-lookup"><span data-stu-id="6bc3f-186">a.</span></span> <span data-ttu-id="6bc3f-187">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="6bc3f-187">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="6bc3f-188">b.</span><span class="sxs-lookup"><span data-stu-id="6bc3f-188">b.</span></span> <span data-ttu-id="6bc3f-189">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="6bc3f-189">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="6bc3f-190">c.</span><span class="sxs-lookup"><span data-stu-id="6bc3f-190">c.</span></span> <span data-ttu-id="6bc3f-191">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="6bc3f-191">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="6bc3f-192">d.</span><span class="sxs-lookup"><span data-stu-id="6bc3f-192">d.</span></span> <span data-ttu-id="6bc3f-193">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="6bc3f-193">Click **Create**.</span></span>
 
### <a name="creating-a-cherwell-test-user"></a><span data-ttu-id="6bc3f-194">Cherwell tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="6bc3f-194">Creating a Cherwell test user</span></span>

<span data-ttu-id="6bc3f-195">az Azure AD tooenable felhasználók toolog a tooCherwell, akkor ki kell építenie Cherwell be.</span><span class="sxs-lookup"><span data-stu-id="6bc3f-195">tooenable Azure AD users toolog in tooCherwell, they must be provisioned into Cherwell.</span></span>

<span data-ttu-id="6bc3f-196">Cherwell hello esetben hello felhasználói fiókokat kell toobe hozta létre a [Cherwell támogatási csoport](https://csm.cherwell.com/contact).</span><span class="sxs-lookup"><span data-stu-id="6bc3f-196">In hello case of Cherwell, hello user accounts need toobe created by your [Cherwell support team](https://csm.cherwell.com/contact).</span></span>

>[!NOTE]
><span data-ttu-id="6bc3f-197">Bármely más Cherwell felhasználói fiók létrehozása eszközök vagy Cherwell tooprovision Azure Active Directory által nyújtott API-k felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="6bc3f-197">You can use any other Cherwell user account creation tools or APIs provided by Cherwell tooprovision Azure Active Directory user accounts.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="6bc3f-198">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="6bc3f-198">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="6bc3f-199">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooCherwell megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="6bc3f-199">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooCherwell.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="6bc3f-201">**tooassign Britta Simon tooCherwell, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="6bc3f-201">**tooassign Britta Simon tooCherwell, perform hello following steps:**</span></span>

1. <span data-ttu-id="6bc3f-202">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="6bc3f-202">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="6bc3f-204">Hello alkalmazások listában válassza ki a **Cherwell**.</span><span class="sxs-lookup"><span data-stu-id="6bc3f-204">In hello applications list, select **Cherwell**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cherwell-tutorial/tutorial_cherwell_app.png) 

3. <span data-ttu-id="6bc3f-206">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="6bc3f-206">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="6bc3f-208">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="6bc3f-208">Click **Add** button.</span></span> <span data-ttu-id="6bc3f-209">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="6bc3f-209">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="6bc3f-211">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="6bc3f-211">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="6bc3f-212">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="6bc3f-212">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="6bc3f-213">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="6bc3f-213">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="6bc3f-214">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="6bc3f-214">Testing single sign-on</span></span>

<span data-ttu-id="6bc3f-215">Ha tootest az egyszeri bejelentkezés a beállításokat, nyissa meg a hozzáférési Panel hello.</span><span class="sxs-lookup"><span data-stu-id="6bc3f-215">If you want tootest your single sign-on settings, open hello Access Panel.</span></span> <span data-ttu-id="6bc3f-216">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="6bc3f-216">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6bc3f-217">További források</span><span class="sxs-lookup"><span data-stu-id="6bc3f-217">Additional resources</span></span>

* [<span data-ttu-id="6bc3f-218">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="6bc3f-218">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="6bc3f-219">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="6bc3f-219">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-cherwell-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cherwell-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cherwell-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cherwell-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-cherwell-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cherwell-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cherwell-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-cherwell-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-cherwell-tutorial/tutorial_general_203.png

