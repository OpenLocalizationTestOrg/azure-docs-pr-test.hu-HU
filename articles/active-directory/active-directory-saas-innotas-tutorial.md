---
title: "Oktatóanyag: Azure Active Directoryval integrált Innotas |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Innotas között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: eb9e9c2c-4b09-4177-bbab-423fd657448e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: jeedes
ms.openlocfilehash: 31d787a351fe9362e35afee28a292c927f43702d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-innotas"></a><span data-ttu-id="2db0d-103">Oktatóanyag: Azure Active Directoryval integrált Innotas</span><span class="sxs-lookup"><span data-stu-id="2db0d-103">Tutorial: Azure Active Directory integration with Innotas</span></span>

<span data-ttu-id="2db0d-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Innotas az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="2db0d-104">In this tutorial, you learn how toointegrate Innotas with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="2db0d-105">Innotas integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="2db0d-105">Integrating Innotas with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="2db0d-106">Megadhatja a hozzáférés tooInnotas rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="2db0d-106">You can control in Azure AD who has access tooInnotas</span></span>
- <span data-ttu-id="2db0d-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooInnotas (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="2db0d-107">You can enable your users tooautomatically get signed-on tooInnotas (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="2db0d-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="2db0d-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="2db0d-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="2db0d-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2db0d-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="2db0d-110">Prerequisites</span></span>

<span data-ttu-id="2db0d-111">az Azure AD integrálása Innotas tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="2db0d-111">tooconfigure Azure AD integration with Innotas, you need hello following items:</span></span>

- <span data-ttu-id="2db0d-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="2db0d-112">An Azure AD subscription</span></span>
- <span data-ttu-id="2db0d-113">Egy Innotas egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="2db0d-113">An Innotas single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="2db0d-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="2db0d-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="2db0d-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="2db0d-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="2db0d-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="2db0d-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="2db0d-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="2db0d-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="2db0d-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="2db0d-118">Scenario description</span></span>

<span data-ttu-id="2db0d-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="2db0d-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="2db0d-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="2db0d-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="2db0d-121">Hello gyűjteményből Innotas hozzáadása</span><span class="sxs-lookup"><span data-stu-id="2db0d-121">Adding Innotas from hello gallery</span></span>
2. <span data-ttu-id="2db0d-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="2db0d-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-innotas-from-hello-gallery"></a><span data-ttu-id="2db0d-123">Hello gyűjteményből Innotas hozzáadása</span><span class="sxs-lookup"><span data-stu-id="2db0d-123">Adding Innotas from hello gallery</span></span>
<span data-ttu-id="2db0d-124">tooconfigure hello integrációja Innotas az Azure AD-be, meg kell tooadd Innotas hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="2db0d-124">tooconfigure hello integration of Innotas into Azure AD, you need tooadd Innotas from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="2db0d-125">**tooadd Innotas hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="2db0d-125">**tooadd Innotas from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="2db0d-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="2db0d-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="2db0d-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="2db0d-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="2db0d-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="2db0d-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="2db0d-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="2db0d-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="2db0d-133">Hello keresési mezőbe, írja be a **Innotas**.</span><span class="sxs-lookup"><span data-stu-id="2db0d-133">In hello search box, type **Innotas**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-innotas-tutorial/tutorial_innotas_search.png)

5. <span data-ttu-id="2db0d-135">A hello eredmények panelen válassza ki a **Innotas**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="2db0d-135">In hello results panel, select **Innotas**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-innotas-tutorial/tutorial_innotas_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="2db0d-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="2db0d-137">Configuring and testing Azure AD single sign-on</span></span>

<span data-ttu-id="2db0d-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján Innotas</span><span class="sxs-lookup"><span data-stu-id="2db0d-138">In this section, you configure and test Azure AD single sign-on with Innotas based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="2db0d-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Innotas tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="2db0d-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Innotas is tooa user in Azure AD.</span></span> <span data-ttu-id="2db0d-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Innotas közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="2db0d-140">In other words, a link relationship between an Azure AD user and hello related user in Innotas needs toobe established.</span></span>

<span data-ttu-id="2db0d-141">Innotas, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="2db0d-141">In Innotas, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="2db0d-142">tooconfigure és az Azure AD az egyszeri bejelentkezés Innotas-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="2db0d-142">tooconfigure and test Azure AD single sign-on with Innotas, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="2db0d-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="2db0d-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="2db0d-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="2db0d-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="2db0d-145">**[Egy Innotas tesztfelhasználó létrehozása](#creating-an-innotas-test-user)**  -toohave egy megfelelője a Britta Simon a Innotas, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="2db0d-145">**[Creating an Innotas test user](#creating-an-innotas-test-user)** - toohave a counterpart of Britta Simon in Innotas that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="2db0d-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="2db0d-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="2db0d-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="2db0d-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="2db0d-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="2db0d-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="2db0d-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Innotas alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="2db0d-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Innotas application.</span></span>

<span data-ttu-id="2db0d-150">**az Azure AD tooconfigure egyszeri bejelentkezést a Innotas, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="2db0d-150">**tooconfigure Azure AD single sign-on with Innotas, perform hello following steps:**</span></span>

1. <span data-ttu-id="2db0d-151">Az Azure portál, a hello hello **Innotas** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="2db0d-151">In hello Azure portal, on hello **Innotas** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="2db0d-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="2db0d-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-innotas-tutorial/tutorial_innotas_samlbase.png)

3. <span data-ttu-id="2db0d-155">A hello **Innotas tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="2db0d-155">On hello **Innotas Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-innotas-tutorial/tutorial_innotas_url.png)

    <span data-ttu-id="2db0d-157">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<tenant-name>.Innotas.com`</span><span class="sxs-lookup"><span data-stu-id="2db0d-157">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<tenant-name>.Innotas.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="2db0d-158">Ez az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="2db0d-158">This value is not real.</span></span> <span data-ttu-id="2db0d-159">Frissítse ezt az értéket hello tényleges bejelentkezési URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="2db0d-159">Update this value with hello actual Sign-On URL.</span></span> <span data-ttu-id="2db0d-160">Ügyfél [Innotas ügyfél-támogatási csoport](https://www.innotas.com/contact) tooget ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="2db0d-160">Contact [Innotas Client support team](https://www.innotas.com/contact) tooget this value.</span></span> 
 
4. <span data-ttu-id="2db0d-161">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="2db0d-161">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-innotas-tutorial/tutorial_innotas_certificate.png) 

5. <span data-ttu-id="2db0d-163">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="2db0d-163">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-innotas-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="2db0d-165">tooconfigure egyszeri bejelentkezést a **Innotas** oldalon kell letöltött toosend hello **metaadatainak XML-kódja** túl[Innotas támogatási csoport](https://www.innotas.com/contact).</span><span class="sxs-lookup"><span data-stu-id="2db0d-165">tooconfigure single sign-on on **Innotas** side, you need toosend hello downloaded **Metadata XML** too[Innotas support team](https://www.innotas.com/contact).</span></span> <span data-ttu-id="2db0d-166">Maguk állítják be ezt a beállítást toohave hello SAML SSO kapcsolat mindkét oldalán megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="2db0d-166">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="2db0d-167">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="2db0d-167">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="2db0d-168">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="2db0d-168">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="2db0d-169">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="2db0d-169">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="2db0d-170">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="2db0d-170">Creating an Azure AD test user</span></span>

<span data-ttu-id="2db0d-171">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="2db0d-171">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="2db0d-173">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="2db0d-173">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="2db0d-174">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="2db0d-174">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-innotas-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="2db0d-176">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="2db0d-176">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-innotas-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="2db0d-178">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="2db0d-178">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-innotas-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="2db0d-180">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="2db0d-180">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-innotas-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="2db0d-182">a.</span><span class="sxs-lookup"><span data-stu-id="2db0d-182">a.</span></span> <span data-ttu-id="2db0d-183">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="2db0d-183">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="2db0d-184">b.</span><span class="sxs-lookup"><span data-stu-id="2db0d-184">b.</span></span> <span data-ttu-id="2db0d-185">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="2db0d-185">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="2db0d-186">c.</span><span class="sxs-lookup"><span data-stu-id="2db0d-186">c.</span></span> <span data-ttu-id="2db0d-187">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="2db0d-187">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="2db0d-188">d.</span><span class="sxs-lookup"><span data-stu-id="2db0d-188">d.</span></span> <span data-ttu-id="2db0d-189">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="2db0d-189">Click **Create**.</span></span>
 
### <a name="creating-an-innotas-test-user"></a><span data-ttu-id="2db0d-190">Egy Innotas tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="2db0d-190">Creating an Innotas test user</span></span>

<span data-ttu-id="2db0d-191">Nincs a akkor tooconfigure felhasználók átadásához tooInnotas művelet elem.</span><span class="sxs-lookup"><span data-stu-id="2db0d-191">There is no action item for you tooconfigure user provisioning tooInnotas.</span></span>  
<span data-ttu-id="2db0d-192">Ha egy hozzárendelt felhasználó megpróbál a hozzáférési panelen hello tooInnotas toolog, Innotas ellenőrzi, hogy létezik-e hello felhasználó.</span><span class="sxs-lookup"><span data-stu-id="2db0d-192">When an assigned user tries toolog in tooInnotas using hello access panel, Innotas checks whether hello user exists.</span></span>  

>[!NOTE]
><span data-ttu-id="2db0d-193">Nincs még nincs felhasználói fiók érhető el, ha a Innotas automatikusan létrehozni.</span><span class="sxs-lookup"><span data-stu-id="2db0d-193">If there is no user account available yet, it is automatically created by Innotas.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="2db0d-194">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="2db0d-194">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="2db0d-195">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooInnotas megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="2db0d-195">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooInnotas.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="2db0d-197">**tooassign Britta Simon tooInnotas, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="2db0d-197">**tooassign Britta Simon tooInnotas, perform hello following steps:**</span></span>

1. <span data-ttu-id="2db0d-198">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="2db0d-198">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="2db0d-200">Hello alkalmazások listában válassza ki a **Innotas**.</span><span class="sxs-lookup"><span data-stu-id="2db0d-200">In hello applications list, select **Innotas**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-innotas-tutorial/tutorial_innotas_app.png) 

3. <span data-ttu-id="2db0d-202">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="2db0d-202">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="2db0d-204">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="2db0d-204">Click **Add** button.</span></span> <span data-ttu-id="2db0d-205">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="2db0d-205">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="2db0d-207">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="2db0d-207">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="2db0d-208">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="2db0d-208">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="2db0d-209">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="2db0d-209">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="2db0d-210">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="2db0d-210">Testing single sign-on</span></span>

<span data-ttu-id="2db0d-211">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="2db0d-211">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="2db0d-212">Hello Innotas hello hozzáférési Panel csempére kattintva kapja meg automatikusan bejelentkezett tooyour Innotas alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="2db0d-212">When you click hello Innotas tile in hello Access Panel, you should get automatically signed-on tooyour Innotas application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2db0d-213">További források</span><span class="sxs-lookup"><span data-stu-id="2db0d-213">Additional resources</span></span>

* [<span data-ttu-id="2db0d-214">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="2db0d-214">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="2db0d-215">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="2db0d-215">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-innotas-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-innotas-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-innotas-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-innotas-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-innotas-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-innotas-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-innotas-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-innotas-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-innotas-tutorial/tutorial_general_203.png

