---
title: "Oktatóanyag: Azure Active Directoryval integrált mezőben |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a mező között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 5f3517f8-30f2-4be7-9e47-43d702701797
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: e13a7979761a0b30ecdaac242f1f57a7f8da54c4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-box"></a><span data-ttu-id="ede63-103">Oktatóanyag: Azure Active Directory-integráció segítségével</span><span class="sxs-lookup"><span data-stu-id="ede63-103">Tutorial: Azure Active Directory integration with Box</span></span>

<span data-ttu-id="ede63-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate az Azure Active Directoryval (Azure AD) mezőben.</span><span class="sxs-lookup"><span data-stu-id="ede63-104">In this tutorial, you learn how toointegrate Box with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="ede63-105">Mezőbe integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="ede63-105">Integrating Box with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="ede63-106">Megadhatja a hozzáférés tooBox rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="ede63-106">You can control in Azure AD who has access tooBox</span></span>
- <span data-ttu-id="ede63-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooBox (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="ede63-107">You can enable your users tooautomatically get signed-on tooBox (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="ede63-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="ede63-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="ede63-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="ede63-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ede63-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="ede63-110">Prerequisites</span></span>

<span data-ttu-id="ede63-111">tooconfigure be az Azure AD integrálása, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="ede63-111">tooconfigure Azure AD integration with Box, you need hello following items:</span></span>

- <span data-ttu-id="ede63-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="ede63-112">An Azure AD subscription</span></span>
- <span data-ttu-id="ede63-113">A mezőben egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="ede63-113">A Box single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="ede63-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="ede63-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="ede63-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="ede63-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="ede63-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="ede63-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="ede63-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="ede63-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="ede63-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="ede63-118">Scenario description</span></span>
<span data-ttu-id="ede63-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="ede63-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="ede63-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="ede63-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="ede63-121">Hello gyűjteményből mező hozzáadása</span><span class="sxs-lookup"><span data-stu-id="ede63-121">Adding Box from hello gallery</span></span>
2. <span data-ttu-id="ede63-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="ede63-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-box-from-hello-gallery"></a><span data-ttu-id="ede63-123">Hello gyűjteményből mező hozzáadása</span><span class="sxs-lookup"><span data-stu-id="ede63-123">Adding Box from hello gallery</span></span>
<span data-ttu-id="ede63-124">tooconfigure hello integrációs párbeszédpanel, az Azure AD-be, meg kell tooadd mezőben hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="ede63-124">tooconfigure hello integration of Box into Azure AD, you need tooadd Box from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="ede63-125">**tooadd mezőben hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="ede63-125">**tooadd Box from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="ede63-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="ede63-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="ede63-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="ede63-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="ede63-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="ede63-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="ede63-131">Kattintson a **új alkalmazás** hello párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="ede63-131">Click **New application** button on hello top of hello dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="ede63-133">Hello keresési mezőbe, írja be a **mezőben**.</span><span class="sxs-lookup"><span data-stu-id="ede63-133">In hello search box, type **Box**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-box-tutorial/tutorial_box_search.png)

5. <span data-ttu-id="ede63-135">A hello eredmények panelen válassza a **mezőben**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="ede63-135">In hello results panel, select **Box**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-box-tutorial/tutorial_box_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="ede63-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="ede63-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="ede63-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján</span><span class="sxs-lookup"><span data-stu-id="ede63-138">In this section, you configure and test Azure AD single sign-on with Box based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="ede63-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói mezőben tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="ede63-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Box is tooa user in Azure AD.</span></span> <span data-ttu-id="ede63-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználói hello mezőben közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="ede63-140">In other words, a link relationship between an Azure AD user and hello related user in Box needs toobe established.</span></span>

<span data-ttu-id="ede63-141">Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="ede63-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Box.</span></span>

<span data-ttu-id="ede63-142">tooconfigure és a vizsgálat az Azure AD egyszeri bejelentkezést a mezőbe, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="ede63-142">tooconfigure and test Azure AD single sign-on with Box, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="ede63-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="ede63-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="ede63-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ede63-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="ede63-145">**[A mezőben tesztfelhasználó létrehozása](#creating-a-box-test-user)**  -toohave egy megfelelője a Britta Simon be, amely a felhasználó csatolt toohello az Azure AD-ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="ede63-145">**[Creating a Box test user](#creating-a-box-test-user)** - toohave a counterpart of Britta Simon in Box that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="ede63-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="ede63-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="ede63-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="ede63-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="ede63-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="ede63-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="ede63-149">Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és be alkalmazásában egyszeri bejelentkezés beállítása.</span><span class="sxs-lookup"><span data-stu-id="ede63-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Box application.</span></span>

<span data-ttu-id="ede63-150">**az Azure AD tooconfigure egyszeri bejelentkezést a jelölését, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="ede63-150">**tooconfigure Azure AD single sign-on with Box, perform hello following steps:**</span></span>

1. <span data-ttu-id="ede63-151">Az Azure portál, a hello hello **mezőben** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="ede63-151">In hello Azure portal, on hello **Box** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="ede63-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="ede63-153">On hello **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-box-tutorial/tutorial_box_samlbase.png)

3. <span data-ttu-id="ede63-155">A hello **mezőben tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="ede63-155">On hello **Box Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-box-tutorial/tutorial_box_url.png)

    <span data-ttu-id="ede63-157">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<subdomain>.box.com`</span><span class="sxs-lookup"><span data-stu-id="ede63-157">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<subdomain>.box.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="ede63-158">Ez az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="ede63-158">This value is not real.</span></span> <span data-ttu-id="ede63-159">Frissítse a hello érték hello tényleges bejelentkezési URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="ede63-159">Update hello value with hello actual Sign-on URL.</span></span> <span data-ttu-id="ede63-160">Ügyfél [mezőben ügyfél-támogatási csoport](https://community.box.com/t5/custom/page/page-id/submit_sso_questionaire) tooget ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="ede63-160">Contact [Box Client support team](https://community.box.com/t5/custom/page/page-id/submit_sso_questionaire) tooget this value.</span></span> 
 
4. <span data-ttu-id="ede63-161">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello XML-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="ede63-161">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello XML file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-box-tutorial/tutorial_box_certificate.png) 

5. <span data-ttu-id="ede63-163">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="ede63-163">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-box-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="ede63-165">az alkalmazás, ügyfél konfigurált SSO tooget [mezőben ügyfél-támogatási csoport](https://community.box.com/t5/custom/page/page-id/submit_sso_questionaire) és adja meg a letöltött hello XML-fájl.</span><span class="sxs-lookup"><span data-stu-id="ede63-165">tooget SSO configured for your application, Contact [Box Client support team](https://community.box.com/t5/custom/page/page-id/submit_sso_questionaire) and provide them with hello downloaded XML file.</span></span>

> [!TIP]
> <span data-ttu-id="ede63-166">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="ede63-166">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="ede63-167">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="ede63-167">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="ede63-168">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="ede63-168">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="ede63-169">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="ede63-169">Creating an Azure AD test user</span></span>
<span data-ttu-id="ede63-170">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="ede63-170">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="ede63-172">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="ede63-172">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="ede63-173">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="ede63-173">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-box-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="ede63-175">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="ede63-175">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-box-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="ede63-177">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="ede63-177">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-box-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="ede63-179">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="ede63-179">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-box-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="ede63-181">a.</span><span class="sxs-lookup"><span data-stu-id="ede63-181">a.</span></span> <span data-ttu-id="ede63-182">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="ede63-182">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="ede63-183">b.</span><span class="sxs-lookup"><span data-stu-id="ede63-183">b.</span></span> <span data-ttu-id="ede63-184">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="ede63-184">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="ede63-185">c.</span><span class="sxs-lookup"><span data-stu-id="ede63-185">c.</span></span> <span data-ttu-id="ede63-186">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="ede63-186">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="ede63-187">d.</span><span class="sxs-lookup"><span data-stu-id="ede63-187">d.</span></span> <span data-ttu-id="ede63-188">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="ede63-188">Click **Create**.</span></span>
 
### <a name="creating-a-box-test-user"></a><span data-ttu-id="ede63-189">A mezőben tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="ede63-189">Creating a Box test user</span></span>

<span data-ttu-id="ede63-190">Ebben a szakaszban a felhasználók Britta Simon nevű létrehozása a mezőbe.</span><span class="sxs-lookup"><span data-stu-id="ede63-190">In this section, a user called Britta Simon is created in Box.</span></span> <span data-ttu-id="ede63-191">Mezőbe támogatja közvetlenül az időponthoz kötött kiosztást, amely alapértelmezés szerint engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="ede63-191">Box supports just-in-time provisioning, which is enabled by default.</span></span>
<span data-ttu-id="ede63-192">Nincs ebben a szakaszban az Ön művelet elem.</span><span class="sxs-lookup"><span data-stu-id="ede63-192">There is no action item for you in this section.</span></span> <span data-ttu-id="ede63-193">Ha a felhasználó nem létezik a mezőbe, egy új tooaccess mezőben tett kísérlet során jön létre.</span><span class="sxs-lookup"><span data-stu-id="ede63-193">If a user doesn't already exist in Box, a new one is created when you attempt tooaccess Box.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="ede63-194">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="ede63-194">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="ede63-195">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooBox megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="ede63-195">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooBox.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="ede63-197">**tooassign Britta Simon tooBox, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="ede63-197">**tooassign Britta Simon tooBox, perform hello following steps:**</span></span>

1. <span data-ttu-id="ede63-198">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="ede63-198">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="ede63-200">Hello alkalmazások listában válassza ki a **mezőben**.</span><span class="sxs-lookup"><span data-stu-id="ede63-200">In hello applications list, select **Box**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-box-tutorial/tutorial_box_app.png) 

3. <span data-ttu-id="ede63-202">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="ede63-202">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="ede63-204">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="ede63-204">Click **Add** button.</span></span> <span data-ttu-id="ede63-205">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="ede63-205">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="ede63-207">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="ede63-207">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="ede63-208">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="ede63-208">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="ede63-209">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="ede63-209">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="ede63-210">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="ede63-210">Testing single sign-on</span></span>

<span data-ttu-id="ede63-211">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="ede63-211">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="ede63-212">Ha hello mezőben csempe a hozzáférési Panel hello gombra kattint, bejelentkezési oldal tooget bejelentkezett tooyour mezőben alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="ede63-212">When you click hello Box tile in hello Access Panel, you should get login page tooget signed-on tooyour Box application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ede63-213">További források</span><span class="sxs-lookup"><span data-stu-id="ede63-213">Additional resources</span></span>

* [<span data-ttu-id="ede63-214">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="ede63-214">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="ede63-215">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="ede63-215">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="ede63-216">A felhasználók átadása konfigurálása</span><span class="sxs-lookup"><span data-stu-id="ede63-216">Configure User Provisioning</span></span>](active-directory-saas-box-userprovisioning-tutorial.md)


<!--Image references-->

[1]: ./media/active-directory-saas-box-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-box-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-box-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-box-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-box-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-box-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-box-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-box-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-box-tutorial/tutorial_general_203.png

