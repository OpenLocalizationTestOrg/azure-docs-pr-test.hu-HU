---
title: "Oktatóanyag: Azure Active Directoryval integrált Neota logika Studio |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Neota logika Studio között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 842605e6-a91d-42cc-a0bb-e23e67173ae2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/03/2017
ms.author: jeedes
ms.openlocfilehash: a479ee49618de8275132dbc2c21a8ef723d92d02
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-neota-logic-studio"></a><span data-ttu-id="9cb91-103">Oktatóanyag: Azure Active Directoryval integrált Neota logika Studio</span><span class="sxs-lookup"><span data-stu-id="9cb91-103">Tutorial: Azure Active Directory integration with Neota Logic Studio</span></span>

<span data-ttu-id="9cb91-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Neota logika Studio az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="9cb91-104">In this tutorial, you learn how toointegrate Neota Logic Studio with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="9cb91-105">Neota logika Studio integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="9cb91-105">Integrating Neota Logic Studio with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="9cb91-106">Megadhatja a hozzáférés tooNeota logika Studio rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="9cb91-106">You can control in Azure AD who has access tooNeota Logic Studio</span></span>
- <span data-ttu-id="9cb91-107">Az Azure AD-fiókok a engedélyezheti a felhasználók tooautomatically get bejelentkezett tooNeota logika Studio (egyszeri bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="9cb91-107">You can enable your users tooautomatically get signed-on tooNeota Logic Studio (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="9cb91-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="9cb91-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="9cb91-109">Ha azt szeretné, tooknow SaaS alkalmazásintegráció az Azure AD-vel kapcsolatos további részletekért lásd:.</span><span class="sxs-lookup"><span data-stu-id="9cb91-109">If you want tooknow more details about SaaS app integration with Azure AD, see.</span></span> <span data-ttu-id="9cb91-110">[Alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="9cb91-110">[What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9cb91-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="9cb91-111">Prerequisites</span></span>

<span data-ttu-id="9cb91-112">az Azure AD integrálása Neota logika Studio tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="9cb91-112">tooconfigure Azure AD integration with Neota Logic Studio, you need hello following items:</span></span>

- <span data-ttu-id="9cb91-113">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="9cb91-113">An Azure AD subscription</span></span>
- <span data-ttu-id="9cb91-114">Egy Neota logika Studio egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="9cb91-114">A Neota Logic Studio single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="9cb91-115">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="9cb91-115">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="9cb91-116">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="9cb91-116">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="9cb91-117">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="9cb91-117">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="9cb91-118">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="9cb91-118">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="9cb91-119">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="9cb91-119">Scenario description</span></span>

<span data-ttu-id="9cb91-120">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="9cb91-120">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="9cb91-121">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="9cb91-121">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="9cb91-122">Hello gyűjteményből Neota logika Studio hozzáadása</span><span class="sxs-lookup"><span data-stu-id="9cb91-122">Adding Neota Logic Studio from hello gallery</span></span>
2. <span data-ttu-id="9cb91-123">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="9cb91-123">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-neota-logic-studio-from-hello-gallery"></a><span data-ttu-id="9cb91-124">Hello gyűjteményből Neota logika Studio hozzáadása</span><span class="sxs-lookup"><span data-stu-id="9cb91-124">Adding Neota Logic Studio from hello gallery</span></span>

<span data-ttu-id="9cb91-125">tooconfigure hello integrációs Neota logika Studio az Azure AD-be, meg kell tooadd Neota logika Studio hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="9cb91-125">tooconfigure hello integration of Neota Logic Studio into Azure AD, you need tooadd Neota Logic Studio from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="9cb91-126">**tooadd Neota logika Studio hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="9cb91-126">**tooadd Neota Logic Studio from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="9cb91-127">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="9cb91-127">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="9cb91-129">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="9cb91-129">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="9cb91-130">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="9cb91-130">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="9cb91-132">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="9cb91-132">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="9cb91-134">Hello keresési mezőbe, írja be a **Neota logika Studio**.</span><span class="sxs-lookup"><span data-stu-id="9cb91-134">In hello search box, type **Neota Logic Studio**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_neotalogicstudio_search.png)

5. <span data-ttu-id="9cb91-136">A hello eredmények panelen válassza ki a **Neota logika Studio**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="9cb91-136">In hello results panel, select **Neota Logic Studio**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_neotalogicstudio_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="9cb91-138">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="9cb91-138">Configuring and testing Azure AD single sign-on</span></span>

<span data-ttu-id="9cb91-139">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD az egyszeri bejelentkezés Neota logika Studio "Britta Simon." nevű tesztfelhasználó alapján</span><span class="sxs-lookup"><span data-stu-id="9cb91-139">In this section, you configure and test Azure AD single sign-on with Neota Logic Studio based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="9cb91-140">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói Neota logika Studio tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="9cb91-140">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Neota Logic Studio is tooa user in Azure AD.</span></span> <span data-ttu-id="9cb91-141">Ez azt jelenti hello kapcsolódó felhasználói Neota logika Studio és az Azure AD-felhasználó közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="9cb91-141">In other words, a link relationship between an Azure AD user and hello related user in Neota Logic Studio needs toobe established.</span></span>

<span data-ttu-id="9cb91-142">Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** Neota logika Studio.</span><span class="sxs-lookup"><span data-stu-id="9cb91-142">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Neota Logic Studio.</span></span>

<span data-ttu-id="9cb91-143">tooconfigure és az Azure AD az egyszeri bejelentkezés tesztelése Neota logika Studio, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="9cb91-143">tooconfigure and test Azure AD single sign-on with Neota Logic Studio, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="9cb91-144">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="9cb91-144">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="9cb91-145">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9cb91-145">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="9cb91-146">**[Neota logika Studio tesztfelhasználó létrehozása](#creating-a-neota-logic-studio-test-user)**  -toohave egy megfelelője a Britta Simon Neota logika Studio, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="9cb91-146">**[Creating a Neota Logic Studio test user](#creating-a-neota-logic-studio-test-user)** - toohave a counterpart of Britta Simon in Neota Logic Studio that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="9cb91-147">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="9cb91-147">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="9cb91-148">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="9cb91-148">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="9cb91-149">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="9cb91-149">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="9cb91-150">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Neota logika Studio alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="9cb91-150">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Neota Logic Studio application.</span></span>

<span data-ttu-id="9cb91-151">**az Azure AD az egyszeri bejelentkezés tooconfigure Neota logika Studio, hajtsa végre a hello a következő lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="9cb91-151">**tooconfigure Azure AD single sign-on with Neota Logic Studio, perform hello following steps:**</span></span>

1. <span data-ttu-id="9cb91-152">Az Azure portál, a hello hello **Neota logika Studio** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="9cb91-152">In hello Azure portal, on hello **Neota Logic Studio** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="9cb91-154">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="9cb91-154">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_neotalogicstudio_samlbase.png)

3. <span data-ttu-id="9cb91-156">A hello **Neota logika Studio tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="9cb91-156">On hello **Neota Logic Studio Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_neotalogicstudio_url.png)

    <span data-ttu-id="9cb91-158">a.</span><span class="sxs-lookup"><span data-stu-id="9cb91-158">a.</span></span> <span data-ttu-id="9cb91-159">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<sub domain>.neotalogic.com/a/<sub application>`</span><span class="sxs-lookup"><span data-stu-id="9cb91-159">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<sub domain>.neotalogic.com/a/<sub application>`</span></span>

    <span data-ttu-id="9cb91-160">b.</span><span class="sxs-lookup"><span data-stu-id="9cb91-160">b.</span></span> <span data-ttu-id="9cb91-161">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<sub domain>.neotalogic.com/wb`</span><span class="sxs-lookup"><span data-stu-id="9cb91-161">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<sub domain>.neotalogic.com/wb`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="9cb91-162">Ezek az értékek nincsenek hello valós.</span><span class="sxs-lookup"><span data-stu-id="9cb91-162">These values are not hello real.</span></span> <span data-ttu-id="9cb91-163">Frissítse a bejelentkezési URL-cím & azonosítója a tényleges hello értékeket.</span><span class="sxs-lookup"><span data-stu-id="9cb91-163">Update these values with hello actual Sign-On URL & Identifier.</span></span> <span data-ttu-id="9cb91-164">Itt javasoljuk, hogy Ön toouse hello egyedi karakterlánc értéke a hello azonosítója.</span><span class="sxs-lookup"><span data-stu-id="9cb91-164">Here we suggest you toouse hello unique value of string in hello Identifier.</span></span> <span data-ttu-id="9cb91-165">Ügyfél [Neota logika Studio ügyfél-támogatási csoport](https://www.neotalogic.com/contact-us/) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="9cb91-165">Contact [Neota Logic Studio Client support team](https://www.neotalogic.com/contact-us/) tooget these values.</span></span> 
 
4. <span data-ttu-id="9cb91-166">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="9cb91-166">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_neotalogicstudio_certificate.png) 

5. <span data-ttu-id="9cb91-168">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="9cb91-168">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="9cb91-170">tooget konfigurált alkalmazás, ügyfél SSO [Neota logika Studio támogatási](https://www.neotalogic.com/contact-us/) vonja össze, és adja meg a letöltött **metaadatainak XML-kódja** fájl.</span><span class="sxs-lookup"><span data-stu-id="9cb91-170">tooget SSO configured for your application, Contact [Neota Logic Studio support](https://www.neotalogic.com/contact-us/) team and provide them with downloaded **Metadata XML** file.</span></span>

> [!TIP]
> <span data-ttu-id="9cb91-171">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="9cb91-171">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="9cb91-172">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="9cb91-172">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="9cb91-173">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="9cb91-173">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="9cb91-174">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="9cb91-174">Creating an Azure AD test user</span></span>
<span data-ttu-id="9cb91-175">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="9cb91-175">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="9cb91-177">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="9cb91-177">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="9cb91-178">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="9cb91-178">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-neotalogicstudio-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="9cb91-180">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="9cb91-180">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-neotalogicstudio-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="9cb91-182">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="9cb91-182">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-neotalogicstudio-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="9cb91-184">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="9cb91-184">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-neotalogicstudio-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="9cb91-186">a.</span><span class="sxs-lookup"><span data-stu-id="9cb91-186">a.</span></span> <span data-ttu-id="9cb91-187">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="9cb91-187">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="9cb91-188">b.</span><span class="sxs-lookup"><span data-stu-id="9cb91-188">b.</span></span> <span data-ttu-id="9cb91-189">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="9cb91-189">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="9cb91-190">c.</span><span class="sxs-lookup"><span data-stu-id="9cb91-190">c.</span></span> <span data-ttu-id="9cb91-191">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="9cb91-191">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="9cb91-192">d.</span><span class="sxs-lookup"><span data-stu-id="9cb91-192">d.</span></span> <span data-ttu-id="9cb91-193">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="9cb91-193">Click **Create**.</span></span>
 
### <a name="creating-a-neota-logic-studio-test-user"></a><span data-ttu-id="9cb91-194">Neota logika Studio tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="9cb91-194">Creating a Neota Logic Studio test user</span></span>

<span data-ttu-id="9cb91-195">Ebben a szakaszban egy Neota logika Studio Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="9cb91-195">In this section, you create a user called Britta Simon in Neota Logic Studio.</span></span> <span data-ttu-id="9cb91-196">a [Neota logika Studio ügyfél-támogatási csoport](https://www.neotalogic.com/contact-us/) felhasználót is hozzáadhat hello hello Neota logika Studio platform.</span><span class="sxs-lookup"><span data-stu-id="9cb91-196">work with [Neota Logic Studio Client support team](https://www.neotalogic.com/contact-us/) to add hello users in hello Neota Logic Studio platform.</span></span> <span data-ttu-id="9cb91-197">Felhasználók kell létrehoznia és aktiválni az egyszeri bejelentkezés használata előtt.</span><span class="sxs-lookup"><span data-stu-id="9cb91-197">Users must be created and activated before you use single sign-on.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="9cb91-198">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="9cb91-198">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="9cb91-199">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooNeota logika Studio megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="9cb91-199">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooNeota Logic Studio.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="9cb91-201">**tooassign Britta Simon tooNeota Studio programot, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="9cb91-201">**tooassign Britta Simon tooNeota Logic Studio, perform hello following steps:**</span></span>

1. <span data-ttu-id="9cb91-202">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="9cb91-202">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="9cb91-204">Hello alkalmazások listában válassza ki a **Neota logika Studio**.</span><span class="sxs-lookup"><span data-stu-id="9cb91-204">In hello applications list, select **Neota Logic Studio**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_neotalogicstudio_app.png) 

3. <span data-ttu-id="9cb91-206">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="9cb91-206">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="9cb91-208">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="9cb91-208">Click **Add** button.</span></span> <span data-ttu-id="9cb91-209">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="9cb91-209">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="9cb91-211">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="9cb91-211">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="9cb91-212">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="9cb91-212">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="9cb91-213">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="9cb91-213">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="9cb91-214">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="9cb91-214">Testing single sign-on</span></span>

<span data-ttu-id="9cb91-215">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="9cb91-215">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="9cb91-216">Hello Neota logika Studio csempén kattintson a hozzáférési Panel hello, átirányított tooOrganization bejelentkezési oldalra lesz.</span><span class="sxs-lookup"><span data-stu-id="9cb91-216">Click hello Neota Logic Studio tile in hello Access Panel, you will be redirected tooOrganization sign-on page.</span></span> <span data-ttu-id="9cb91-217">Sikeres bejelentkezés után fog bejelentkezett tooyour Neota logika Studio alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="9cb91-217">After successful login, you will be signed-on tooyour Neota Logic Studio application.</span></span> <span data-ttu-id="9cb91-218">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="9cb91-218">For more information about the Access Panel, see [Introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span>

<span data-ttu-id="9cb91-219">További információ a hozzáférési Panel hello: [a hozzáférési Panel bemutatása](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="9cb91-219">For more information about hello Access Panel, see [introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="9cb91-220">További források</span><span class="sxs-lookup"><span data-stu-id="9cb91-220">Additional resources</span></span>

* [<span data-ttu-id="9cb91-221">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="9cb91-221">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="9cb91-222">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="9cb91-222">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_general_203.png

