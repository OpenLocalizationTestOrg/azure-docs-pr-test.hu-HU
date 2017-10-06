---
title: "Oktatóanyag: Azure Active Directory-integráció Capriza platformon |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Capriza Platform között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 48d92247-f00a-47b9-8d4e-137028d9e200
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: 1c4adb737bb5ba4690bbf74688010238c5c83f3b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-capriza-platform"></a><span data-ttu-id="87d22-103">Oktatóanyag: Azure Active Directoryval integrált Capriza Platform</span><span class="sxs-lookup"><span data-stu-id="87d22-103">Tutorial: Azure Active Directory integration with Capriza Platform</span></span>

<span data-ttu-id="87d22-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Capriza Platform az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="87d22-104">In this tutorial, you learn how toointegrate Capriza Platform with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="87d22-105">Capriza Platform integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="87d22-105">Integrating Capriza Platform with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="87d22-106">Megadhatja a hozzáférés tooCapriza Platform rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="87d22-106">You can control in Azure AD who has access tooCapriza Platform</span></span>
- <span data-ttu-id="87d22-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooCapriza Platform (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="87d22-107">You can enable your users tooautomatically get signed-on tooCapriza Platform (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="87d22-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="87d22-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="87d22-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="87d22-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="87d22-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="87d22-110">Prerequisites</span></span>

<span data-ttu-id="87d22-111">tooconfigure Capriza platformon az Azure AD integrálása, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="87d22-111">tooconfigure Azure AD integration with Capriza Platform, you need hello following items:</span></span>

- <span data-ttu-id="87d22-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="87d22-112">An Azure AD subscription</span></span>
- <span data-ttu-id="87d22-113">Egy Capriza Platform egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="87d22-113">A Capriza Platform single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="87d22-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="87d22-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="87d22-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="87d22-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="87d22-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="87d22-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="87d22-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="87d22-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="87d22-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="87d22-118">Scenario description</span></span>
<span data-ttu-id="87d22-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="87d22-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="87d22-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="87d22-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="87d22-121">Hello gyűjteményből Capriza Platform hozzáadása</span><span class="sxs-lookup"><span data-stu-id="87d22-121">Adding Capriza Platform from hello gallery</span></span>
2. <span data-ttu-id="87d22-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="87d22-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-capriza-platform-from-hello-gallery"></a><span data-ttu-id="87d22-123">Hello gyűjteményből Capriza Platform hozzáadása</span><span class="sxs-lookup"><span data-stu-id="87d22-123">Adding Capriza Platform from hello gallery</span></span>
<span data-ttu-id="87d22-124">tooconfigure hello integrációs Capriza platform, az Azure AD-be, meg kell tooadd Capriza Platform hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="87d22-124">tooconfigure hello integration of Capriza Platform into Azure AD, you need tooadd Capriza Platform from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="87d22-125">**tooadd Capriza Platform hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="87d22-125">**tooadd Capriza Platform from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="87d22-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="87d22-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="87d22-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="87d22-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="87d22-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="87d22-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="87d22-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="87d22-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="87d22-133">Hello keresési mezőbe, írja be a **Capriza Platform**.</span><span class="sxs-lookup"><span data-stu-id="87d22-133">In hello search box, type **Capriza Platform**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-capriza-tutorial/tutorial_caprizaplatform_search.png)

5. <span data-ttu-id="87d22-135">A hello eredmények panelen válassza ki a **Capriza Platform**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="87d22-135">In hello results panel, select **Capriza Platform**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-capriza-tutorial/tutorial_caprizaplatform_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="87d22-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="87d22-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="87d22-138">Ebben a szakaszban konfigurálhatja, és a "Britta Simon." nevű tesztfelhasználó alapján Capriza platformon az Azure AD az egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="87d22-138">In this section, you configure and test Azure AD single sign-on with Capriza Platform based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="87d22-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói Capriza platform tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="87d22-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Capriza Platform is tooa user in Azure AD.</span></span> <span data-ttu-id="87d22-140">Ez azt jelenti hello kapcsolódó felhasználói Capriza platform és az Azure AD-felhasználó közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="87d22-140">In other words, a link relationship between an Azure AD user and hello related user in Capriza Platform needs toobe established.</span></span>

<span data-ttu-id="87d22-141">Capriza Platform, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="87d22-141">In Capriza Platform, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="87d22-142">tooconfigure és Capriza platformon az Azure AD az egyszeri bejelentkezés tesztelése, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="87d22-142">tooconfigure and test Azure AD single sign-on with Capriza Platform, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="87d22-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="87d22-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="87d22-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="87d22-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="87d22-145">**[Capriza Platform tesztfelhasználó létrehozása](#creating-a-capriza-platform-test-user)**  -toohave egy megfelelője a Britta Simon Capriza platform, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="87d22-145">**[Creating a Capriza Platform test user](#creating-a-capriza-platform-test-user)** - toohave a counterpart of Britta Simon in Capriza Platform that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="87d22-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="87d22-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="87d22-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="87d22-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="87d22-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="87d22-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="87d22-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Capriza Platform alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="87d22-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Capriza Platform application.</span></span>

<span data-ttu-id="87d22-150">**az Azure AD tooconfigure egyszeri bejelentkezés Capriza platformon, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="87d22-150">**tooconfigure Azure AD single sign-on with Capriza Platform, perform hello following steps:**</span></span>

1. <span data-ttu-id="87d22-151">Az Azure portál, a hello hello **Capriza Platform** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="87d22-151">In hello Azure portal, on hello **Capriza Platform** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="87d22-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="87d22-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-capriza-tutorial/tutorial_caprizaplatform_samlbase.png)

3. <span data-ttu-id="87d22-155">A hello **Capriza Platform tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="87d22-155">On hello **Capriza Platform Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-capriza-tutorial/tutorial_caprizaplatform_url.png)

    <span data-ttu-id="87d22-157">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<companyname>.capriza.com/<tenantid>`</span><span class="sxs-lookup"><span data-stu-id="87d22-157">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.capriza.com/<tenantid>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="87d22-158">Ez az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="87d22-158">This value is not real.</span></span> <span data-ttu-id="87d22-159">Frissítse ezt az értéket hello tényleges bejelentkezési URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="87d22-159">Update this value with hello actual Sign-On URL.</span></span> <span data-ttu-id="87d22-160">Ügyfél [Capriza Platform ügyfél-támogatási csoport](mailTo:support@capriza.com) tooget ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="87d22-160">Contact [Capriza Platform Client support team](mailTo:support@capriza.com) tooget this value.</span></span> 

4. <span data-ttu-id="87d22-161">A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="87d22-161">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-capriza-tutorial/tutorial_caprizaplatform_certificate.png) 

5. <span data-ttu-id="87d22-163">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="87d22-163">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-capriza-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="87d22-165">A hello **Capriza platformkonfiguráció** kattintson **Capriza Platform konfigurálása** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="87d22-165">On hello **Capriza Platform Configuration** section, click **Configure Capriza Platform** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="87d22-166">Másolás hello **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="87d22-166">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-capriza-tutorial/tutorial_caprizaplatform_configure.png) 

7. <span data-ttu-id="87d22-168">tooconfigure egyszeri bejelentkezést a **Capriza Platform** oldalon kell letöltött toosend hello **tanúsítvány**, **Sign-Out URL-cím**, **SAML Entitásazonosító** és **SAML-alapú egyszeri bejelentkezési URL-címe** túl[Capriza Platform támogatási csoport](mailTo:support@capriza.com).</span><span class="sxs-lookup"><span data-stu-id="87d22-168">tooconfigure single sign-on on **Capriza Platform** side, you need toosend hello downloaded **Certificate**, **Sign-Out URL**, **SAML Entity ID** and **SAML Single Sign-On Service URL** too[Capriza Platform support team](mailTo:support@capriza.com).</span></span> <span data-ttu-id="87d22-169">Maguk állítják be ezt a beállítást toohave hello SAML SSO kapcsolat mindkét oldalán megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="87d22-169">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="87d22-170">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="87d22-170">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="87d22-171">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="87d22-171">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="87d22-172">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="87d22-172">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="87d22-173">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="87d22-173">Creating an Azure AD test user</span></span>
<span data-ttu-id="87d22-174">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="87d22-174">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="87d22-176">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="87d22-176">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="87d22-177">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="87d22-177">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-capriza-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="87d22-179">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="87d22-179">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-capriza-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="87d22-181">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="87d22-181">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-capriza-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="87d22-183">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="87d22-183">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-capriza-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="87d22-185">a.</span><span class="sxs-lookup"><span data-stu-id="87d22-185">a.</span></span> <span data-ttu-id="87d22-186">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="87d22-186">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="87d22-187">b.</span><span class="sxs-lookup"><span data-stu-id="87d22-187">b.</span></span> <span data-ttu-id="87d22-188">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="87d22-188">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="87d22-189">c.</span><span class="sxs-lookup"><span data-stu-id="87d22-189">c.</span></span> <span data-ttu-id="87d22-190">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="87d22-190">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="87d22-191">d.</span><span class="sxs-lookup"><span data-stu-id="87d22-191">d.</span></span> <span data-ttu-id="87d22-192">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="87d22-192">Click **Create**.</span></span>
 
### <a name="creating-a-capriza-platform-test-user"></a><span data-ttu-id="87d22-193">Capriza Platform tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="87d22-193">Creating a Capriza Platform test user</span></span>

<span data-ttu-id="87d22-194">hello ebben a szakaszban célja toocreate Capriza Britta Simon nevű felhasználó.</span><span class="sxs-lookup"><span data-stu-id="87d22-194">hello objective of this section is toocreate a user called Britta Simon in Capriza.</span></span> <span data-ttu-id="87d22-195">Capriza támogatja just-in-time kiosztást, amely alapértelmezés szerint van engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="87d22-195">Capriza supports just-in-time provisioning, which is by default enabled.</span></span> <span data-ttu-id="87d22-196">**Győződjön meg arról, hogy a tartománynév beállítása a Capriza a felhasználók átadása. Adott csak hello után közvetlenül az idő a felhasználók átadása fog működni.**</span><span class="sxs-lookup"><span data-stu-id="87d22-196">**Please make sure that your domain name is configured with Capriza for user provisioning. After that only hello just-in-time user provisioning will work.**</span></span>

<span data-ttu-id="87d22-197">Nincs ebben a szakaszban az Ön művelet elem.</span><span class="sxs-lookup"><span data-stu-id="87d22-197">There is no action item for you in this section.</span></span> <span data-ttu-id="87d22-198">Új felhasználó létrejön egy kísérlet tooaccess Capriza során, ha még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="87d22-198">A new user will be created during an attempt tooaccess Capriza if it doesn't exist yet.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="87d22-199">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="87d22-199">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="87d22-200">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooCapriza Platform megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="87d22-200">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooCapriza Platform.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="87d22-202">**tooassign Britta Simon tooCapriza Platform, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="87d22-202">**tooassign Britta Simon tooCapriza Platform, perform hello following steps:**</span></span>

1. <span data-ttu-id="87d22-203">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="87d22-203">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="87d22-205">Hello alkalmazások listában válassza ki a **Capriza Platform**.</span><span class="sxs-lookup"><span data-stu-id="87d22-205">In hello applications list, select **Capriza Platform**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-capriza-tutorial/tutorial_caprizaplatform_app.png) 

3. <span data-ttu-id="87d22-207">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="87d22-207">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="87d22-209">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="87d22-209">Click **Add** button.</span></span> <span data-ttu-id="87d22-210">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="87d22-210">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="87d22-212">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="87d22-212">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="87d22-213">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="87d22-213">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="87d22-214">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="87d22-214">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="87d22-215">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="87d22-215">Testing single sign-on</span></span>

<span data-ttu-id="87d22-216">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="87d22-216">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="87d22-217">Hello Capriza Platform hello hozzáférési Panel csempére kattintva kapja meg automatikusan bejelentkezett tooyour Capriza alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="87d22-217">When you click hello Capriza Platform tile in hello Access Panel, you should get automatically signed-on tooyour Capriza application.</span></span> <span data-ttu-id="87d22-218">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="87d22-218">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="87d22-219">További források</span><span class="sxs-lookup"><span data-stu-id="87d22-219">Additional resources</span></span>

* [<span data-ttu-id="87d22-220">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="87d22-220">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="87d22-221">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="87d22-221">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-capriza-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-capriza-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-capriza-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-capriza-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-capriza-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-capriza-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-capriza-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-capriza-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-capriza-tutorial/tutorial_general_203.png

