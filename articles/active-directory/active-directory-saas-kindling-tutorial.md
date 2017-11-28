---
title: "Oktatóanyag: Azure Active Directoryval integrált Kindling |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Kindling között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 71229751-74eb-4c2c-abb4-07caa95754c7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/18/2017
ms.author: jeedes
ms.openlocfilehash: 32ad6116c2690aea2060a2dd56e052f233ad7ae3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-kindling"></a><span data-ttu-id="29d4b-103">Oktatóanyag: Azure Active Directoryval integrált Kindling</span><span class="sxs-lookup"><span data-stu-id="29d4b-103">Tutorial: Azure Active Directory integration with Kindling</span></span>

<span data-ttu-id="29d4b-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Kindling az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="29d4b-104">In this tutorial, you learn how toointegrate Kindling with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="29d4b-105">Kindling integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="29d4b-105">Integrating Kindling with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="29d4b-106">Megadhatja a hozzáférés tooKindling rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="29d4b-106">You can control in Azure AD who has access tooKindling</span></span>
- <span data-ttu-id="29d4b-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooKindling (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="29d4b-107">You can enable your users tooautomatically get signed-on tooKindling (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="29d4b-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="29d4b-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="29d4b-109">Ha azt szeretné, tooknow SaaS alkalmazásintegráció az Azure AD-vel kapcsolatos további részletekért lásd:.</span><span class="sxs-lookup"><span data-stu-id="29d4b-109">If you want tooknow more details about SaaS app integration with Azure AD, see.</span></span> <span data-ttu-id="29d4b-110">[alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="29d4b-110">[what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="29d4b-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="29d4b-111">Prerequisites</span></span>

<span data-ttu-id="29d4b-112">Kindling tooconfigure az Azure AD integrálása, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="29d4b-112">tooconfigure Azure AD integration with Kindling, you need hello following items:</span></span>

- <span data-ttu-id="29d4b-113">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="29d4b-113">An Azure AD subscription</span></span>
- <span data-ttu-id="29d4b-114">Egy Kindling egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="29d4b-114">A Kindling single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="29d4b-115">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="29d4b-115">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="29d4b-116">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="29d4b-116">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="29d4b-117">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="29d4b-117">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="29d4b-118">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="29d4b-118">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="29d4b-119">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="29d4b-119">Scenario description</span></span>
<span data-ttu-id="29d4b-120">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="29d4b-120">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="29d4b-121">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="29d4b-121">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="29d4b-122">Hello gyűjteményből Kindling hozzáadása</span><span class="sxs-lookup"><span data-stu-id="29d4b-122">Adding Kindling from hello gallery</span></span>
2. <span data-ttu-id="29d4b-123">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="29d4b-123">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-kindling-from-hello-gallery"></a><span data-ttu-id="29d4b-124">Hello gyűjteményből Kindling hozzáadása</span><span class="sxs-lookup"><span data-stu-id="29d4b-124">Adding Kindling from hello gallery</span></span>
<span data-ttu-id="29d4b-125">tooconfigure hello integrálása az Azure AD-be Kindling, kell tooadd Kindling hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="29d4b-125">tooconfigure hello integration of Kindling into Azure AD, you need tooadd Kindling from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="29d4b-126">**tooadd Kindling hello gyűjteményből, hajtsa végre az alábbi lépésekkel hello:**</span><span class="sxs-lookup"><span data-stu-id="29d4b-126">**tooadd Kindling from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="29d4b-127">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="29d4b-127">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="29d4b-129">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="29d4b-129">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="29d4b-130">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="29d4b-130">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="29d4b-132">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="29d4b-132">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="29d4b-134">Hello keresési mezőbe, írja be a **Kindling**.</span><span class="sxs-lookup"><span data-stu-id="29d4b-134">In hello search box, type **Kindling**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kindling-tutorial/tutorial_kindling_search.png)

5. <span data-ttu-id="29d4b-136">A hello eredmények panelen válassza ki a **Kindling**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="29d4b-136">In hello results panel, select **Kindling**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kindling-tutorial/tutorial_kindling_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="29d4b-138">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="29d4b-138">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="29d4b-139">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján Kindling</span><span class="sxs-lookup"><span data-stu-id="29d4b-139">In this section, you configure and test Azure AD single sign-on with Kindling based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="29d4b-140">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Kindling tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="29d4b-140">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Kindling is tooa user in Azure AD.</span></span> <span data-ttu-id="29d4b-141">Más szóval hivatkozás közötti kapcsolat egy Azure AD-felhasználó és a kapcsolódó felhasználóknak hello Kindling igényeinek toobe létrehozott.</span><span class="sxs-lookup"><span data-stu-id="29d4b-141">In other words, a link relationship between an Azure AD user and hello related user in Kindling needs toobe established.</span></span>

<span data-ttu-id="29d4b-142">Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** a Kindling.</span><span class="sxs-lookup"><span data-stu-id="29d4b-142">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Kindling.</span></span>

<span data-ttu-id="29d4b-143">tooconfigure és az Azure AD az egyszeri bejelentkezés Kindling-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="29d4b-143">tooconfigure and test Azure AD single sign-on with Kindling, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="29d4b-144">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="29d4b-144">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="29d4b-145">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="29d4b-145">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="29d4b-146">**[Kindling tesztfelhasználó létrehozása](#creating-a-kindling-test-user)**  -toohave Britta Simon egy megfelelője a Kindling, amely a kapcsolódó felhasználói toohello az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="29d4b-146">**[Creating a Kindling test user](#creating-a-kindling-test-user)** - toohave a counterpart of Britta Simon in Kindling that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="29d4b-147">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="29d4b-147">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="29d4b-148">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="29d4b-148">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="29d4b-149">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="29d4b-149">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="29d4b-150">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Kindling alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="29d4b-150">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Kindling application.</span></span>

<span data-ttu-id="29d4b-151">**Kindling, az Azure AD az egyszeri bejelentkezés tooconfigure hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="29d4b-151">**tooconfigure Azure AD single sign-on with Kindling, perform hello following steps:**</span></span>

1. <span data-ttu-id="29d4b-152">Az Azure portál, a hello hello **Kindling** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="29d4b-152">In hello Azure portal, on hello **Kindling** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="29d4b-154">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="29d4b-154">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kindling-tutorial/tutorial_kindling_samlbase.png)

3. <span data-ttu-id="29d4b-156">A hello **Kindling tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="29d4b-156">On hello **Kindling Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kindling-tutorial/tutorial_kindling_url.png)

    <span data-ttu-id="29d4b-158">a.</span><span class="sxs-lookup"><span data-stu-id="29d4b-158">a.</span></span> <span data-ttu-id="29d4b-159">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<companyname>.kindlingapp.com`</span><span class="sxs-lookup"><span data-stu-id="29d4b-159">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.kindlingapp.com`</span></span>

    <span data-ttu-id="29d4b-160">b.</span><span class="sxs-lookup"><span data-stu-id="29d4b-160">b.</span></span>  <span data-ttu-id="29d4b-161">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<companyname>.kindlingapp.com/saml/module.php/saml/sp/metadata.php/clientIDP`</span><span class="sxs-lookup"><span data-stu-id="29d4b-161">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<companyname>.kindlingapp.com/saml/module.php/saml/sp/metadata.php/clientIDP`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="29d4b-162">Ezek az értékek nincsenek hello valós.</span><span class="sxs-lookup"><span data-stu-id="29d4b-162">These values are not hello real.</span></span> <span data-ttu-id="29d4b-163">Frissítheti ezeket az értékeket a hello tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="29d4b-163">Update these values with hello actual Sign-on URL and Identifier.</span></span> <span data-ttu-id="29d4b-164">Ügyfél [Kindling támogatási csoport](mailto:support@kindlingapp.com) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="29d4b-164">Contact [Kindling support team](mailto:support@kindlingapp.com) tooget these values.</span></span>
 
4. <span data-ttu-id="29d4b-165">A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="29d4b-165">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kindling-tutorial/tutorial_kindling_certificate.png) 

5. <span data-ttu-id="29d4b-167">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="29d4b-167">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kindling-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="29d4b-169">A hello **Kindling konfigurációs** kattintson **konfigurálása Kindling** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="29d4b-169">On hello **Kindling Configuration** section, click **Configure Kindling** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="29d4b-170">Másolás hello **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="29d4b-170">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kindling-tutorial/tutorial_kindling_configure.png) 

7. <span data-ttu-id="29d4b-172">tooconfigure egyszeri bejelentkezést a **Kindling** oldalon kell letöltött toosend hello **tanúsítvány (Base64)**, **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe**túl[Kindling támogatási csoport](mailto:support@kindlingapp.com).</span><span class="sxs-lookup"><span data-stu-id="29d4b-172">tooconfigure single sign-on on **Kindling** side, you need toosend hello downloaded **Certificate (Base64)**, **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** too[Kindling support team](mailto:support@kindlingapp.com).</span></span>

> [!TIP]
> <span data-ttu-id="29d4b-173">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="29d4b-173">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="29d4b-174">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="29d4b-174">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="29d4b-175">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="29d4b-175">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="29d4b-176">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="29d4b-176">Creating an Azure AD test user</span></span>
<span data-ttu-id="29d4b-177">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="29d4b-177">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="29d4b-179">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="29d4b-179">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="29d4b-180">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="29d4b-180">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kindling-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="29d4b-182">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="29d4b-182">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kindling-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="29d4b-184">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="29d4b-184">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kindling-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="29d4b-186">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="29d4b-186">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kindling-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="29d4b-188">a.</span><span class="sxs-lookup"><span data-stu-id="29d4b-188">a.</span></span> <span data-ttu-id="29d4b-189">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="29d4b-189">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="29d4b-190">b.</span><span class="sxs-lookup"><span data-stu-id="29d4b-190">b.</span></span> <span data-ttu-id="29d4b-191">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="29d4b-191">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="29d4b-192">c.</span><span class="sxs-lookup"><span data-stu-id="29d4b-192">c.</span></span> <span data-ttu-id="29d4b-193">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="29d4b-193">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="29d4b-194">d.</span><span class="sxs-lookup"><span data-stu-id="29d4b-194">d.</span></span> <span data-ttu-id="29d4b-195">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="29d4b-195">Click **Create**.</span></span>
 
### <a name="creating-a-kindling-test-user"></a><span data-ttu-id="29d4b-196">Kindling tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="29d4b-196">Creating a Kindling test user</span></span>

<span data-ttu-id="29d4b-197">hello ebben a szakaszban célja toocreate Kindling Britta Simon nevű felhasználó.</span><span class="sxs-lookup"><span data-stu-id="29d4b-197">hello objective of this section is toocreate a user called Britta Simon in Kindling.</span></span> <span data-ttu-id="29d4b-198">Kindling támogatja közvetlenül az időponthoz kötött kiosztást.</span><span class="sxs-lookup"><span data-stu-id="29d4b-198">Kindling supports just-in-time provisioning.</span></span> <span data-ttu-id="29d4b-199">Már engedélyezett legyen [az Azure AD konfigurálása egyszeri bejelentkezéshez](#configuring-azure-ad-single-sign-on).</span><span class="sxs-lookup"><span data-stu-id="29d4b-199">You have already enabled it in [Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on).</span></span>

<span data-ttu-id="29d4b-200">Nincs ebben a szakaszban az Ön művelet elem.</span><span class="sxs-lookup"><span data-stu-id="29d4b-200">There is no action item for you in this section.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="29d4b-201">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="29d4b-201">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="29d4b-202">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooKindling megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="29d4b-202">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooKindling.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="29d4b-204">**tooassign Britta Simon tooKindling, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="29d4b-204">**tooassign Britta Simon tooKindling, perform hello following steps:**</span></span>

1. <span data-ttu-id="29d4b-205">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="29d4b-205">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="29d4b-207">Hello alkalmazások listában válassza ki a **Kindling**.</span><span class="sxs-lookup"><span data-stu-id="29d4b-207">In hello applications list, select **Kindling**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kindling-tutorial/tutorial_kindling_app.png) 

3. <span data-ttu-id="29d4b-209">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="29d4b-209">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="29d4b-211">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="29d4b-211">Click **Add** button.</span></span> <span data-ttu-id="29d4b-212">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="29d4b-212">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="29d4b-214">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="29d4b-214">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="29d4b-215">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="29d4b-215">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="29d4b-216">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="29d4b-216">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="29d4b-217">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="29d4b-217">Testing single sign-on</span></span>

<span data-ttu-id="29d4b-218">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="29d4b-218">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="29d4b-219">Ha a hozzáférési Panel hello hello Kindling csempe gombra kattint, automatikusan bejelentkezett tooyour Kindling alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="29d4b-219">When you click hello Kindling tile in hello Access Panel, you should get automatically signed-on tooyour Kindling application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="29d4b-220">További források</span><span class="sxs-lookup"><span data-stu-id="29d4b-220">Additional resources</span></span>

* [<span data-ttu-id="29d4b-221">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="29d4b-221">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="29d4b-222">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="29d4b-222">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-kindling-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-kindling-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-kindling-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-kindling-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-kindling-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-kindling-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-kindling-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-kindling-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-kindling-tutorial/tutorial_general_203.png

