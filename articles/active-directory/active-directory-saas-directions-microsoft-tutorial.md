---
title: "Oktatóanyag: Azure Active Directory-integráció, amelyen Microsoft |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a Microsoft irányú között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: e0c8986f-2acd-418d-a306-437abc44b640
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: 59a8b856fd2dc75a37e9bb8f46ced066bcd0fd56
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-directions-on-microsoft"></a><span data-ttu-id="a83af-103">Oktatóanyag: Azure Active Directory-integráció, amelyen Microsoft</span><span class="sxs-lookup"><span data-stu-id="a83af-103">Tutorial: Azure Active Directory integration with Directions on Microsoft</span></span>

<span data-ttu-id="a83af-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate a Microsoft Azure Active Directory (Azure AD) szakasz utasításait.</span><span class="sxs-lookup"><span data-stu-id="a83af-104">In this tutorial, you learn how toointegrate Directions on Microsoft with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="a83af-105">A Microsoft irányban integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="a83af-105">Integrating Directions on Microsoft with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="a83af-106">Megadhatja a Microsoft access tooDirections rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="a83af-106">You can control in Azure AD who has access tooDirections on Microsoft</span></span>
- <span data-ttu-id="a83af-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooDirections Microsoft (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="a83af-107">You can enable your users tooautomatically get signed-on tooDirections on Microsoft (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="a83af-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="a83af-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="a83af-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="a83af-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a83af-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="a83af-110">Prerequisites</span></span>

<span data-ttu-id="a83af-111">tooconfigure, amelyen a Microsoft Azure AD integrálása, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="a83af-111">tooconfigure Azure AD integration with Directions on Microsoft, you need hello following items:</span></span>

- <span data-ttu-id="a83af-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="a83af-112">An Azure AD subscription</span></span>
- <span data-ttu-id="a83af-113">A Microsoft egyszeri bejelentkezéshez egy irányban engedélyezett előfizetés</span><span class="sxs-lookup"><span data-stu-id="a83af-113">A Directions on Microsoft single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="a83af-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="a83af-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="a83af-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="a83af-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="a83af-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="a83af-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="a83af-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a83af-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="a83af-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="a83af-118">Scenario description</span></span>
<span data-ttu-id="a83af-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="a83af-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="a83af-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="a83af-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="a83af-121">Irányban hozzáadása a Microsoft hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="a83af-121">Adding Directions on Microsoft from hello gallery</span></span>
2. <span data-ttu-id="a83af-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="a83af-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-directions-on-microsoft-from-hello-gallery"></a><span data-ttu-id="a83af-123">Irányban hozzáadása a Microsoft hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="a83af-123">Adding Directions on Microsoft from hello gallery</span></span>
<span data-ttu-id="a83af-124">tooconfigure hello integrációja utasításait a Microsoft az Azure AD-be, meg kell tooadd utasításait a Microsoft hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="a83af-124">tooconfigure hello integration of Directions on Microsoft into Azure AD, you need tooadd Directions on Microsoft from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="a83af-125">**tooadd utasításait a Microsoft hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="a83af-125">**tooadd Directions on Microsoft from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="a83af-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="a83af-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="a83af-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="a83af-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="a83af-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="a83af-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="a83af-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="a83af-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="a83af-133">Hello keresési mezőbe, írja be a **Microsoft irányú**.</span><span class="sxs-lookup"><span data-stu-id="a83af-133">In hello search box, type **Directions on Microsoft**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-directions-microsoft-tutorial/tutorial_directionsonmicrosoft_search.png)

5. <span data-ttu-id="a83af-135">Hello eredmények panelen, jelölje ki a **Microsoft irányú**, és kattintson a **hozzáadása** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="a83af-135">In hello results panel, select **Directions on Microsoft**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-directions-microsoft-tutorial/tutorial_directionsonmicrosoft_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="a83af-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="a83af-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="a83af-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést, amelyen Microsoft "Britta Simon." nevű tesztfelhasználó alapján</span><span class="sxs-lookup"><span data-stu-id="a83af-138">In this section, you configure and test Azure AD single sign-on with Directions on Microsoft based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="a83af-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói Microsoft irányú tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="a83af-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Directions on Microsoft is tooa user in Azure AD.</span></span> <span data-ttu-id="a83af-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználói hello Microsoft irányú közötti kapcsolat kapcsolatot kell toobe létrejött.</span><span class="sxs-lookup"><span data-stu-id="a83af-140">In other words, a link relationship between an Azure AD user and hello related user in Directions on Microsoft needs toobe established.</span></span>

<span data-ttu-id="a83af-141">Microsoft irányú, rendelje a hello hello értékét **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="a83af-141">In Directions on Microsoft, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="a83af-142">tooconfigure és, amelyen a Microsoft Azure AD az egyszeri bejelentkezés tesztelése, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="a83af-142">tooconfigure and test Azure AD single sign-on with Directions on Microsoft, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="a83af-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="a83af-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="a83af-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a83af-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="a83af-145">**[Egy irányban létrehozása a Microsoft tesztfelhasználó](#creating-a-directions-on-microsoft-test-user)**  -toohave egy megfelelője a Britta Simon irányban a Microsoftnak, amely a felhasználó csatolt toohello az Azure AD-ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="a83af-145">**[Creating a Directions on Microsoft test user](#creating-a-directions-on-microsoft-test-user)** - toohave a counterpart of Britta Simon in Directions on Microsoft that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="a83af-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="a83af-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="a83af-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="a83af-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="a83af-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="a83af-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="a83af-149">Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és a Microsoft alkalmazás irányú egyszeri bejelentkezés beállítása.</span><span class="sxs-lookup"><span data-stu-id="a83af-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Directions on Microsoft application.</span></span>

<span data-ttu-id="a83af-150">**az Azure AD tooconfigure egyszeri bejelentkezést, amelyen a Microsoft, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="a83af-150">**tooconfigure Azure AD single sign-on with Directions on Microsoft, perform hello following steps:**</span></span>

1. <span data-ttu-id="a83af-151">Az Azure portál, a hello hello **Microsoft irányú** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="a83af-151">In hello Azure portal, on hello **Directions on Microsoft** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="a83af-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="a83af-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-directions-microsoft-tutorial/tutorial_directionsonmicrosoft_samlbase.png)

3. <span data-ttu-id="a83af-155">A hello **Microsoft Domain és URL-címek irányú** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="a83af-155">On hello **Directions on Microsoft Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-directions-microsoft-tutorial/tutorial_directionsonmicrosoft_url.png)

    <span data-ttu-id="a83af-157">a.</span><span class="sxs-lookup"><span data-stu-id="a83af-157">a.</span></span> <span data-ttu-id="a83af-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:</span><span class="sxs-lookup"><span data-stu-id="a83af-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern:</span></span>
    |  |
    | --- |
    | `https://www.directionsonmicrosoft.com/user/login` |
    | `https://<subdomain>.devcloud.acquia-sites.com/<companyname>` |

    <span data-ttu-id="a83af-159">b.</span><span class="sxs-lookup"><span data-stu-id="a83af-159">b.</span></span> <span data-ttu-id="a83af-160">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:</span><span class="sxs-lookup"><span data-stu-id="a83af-160">In hello **Identifier** textbox, type a URL using hello following pattern:</span></span>
    |  |
    | --- |
    | `https://rhelmdirectionsonmicrosoftcomtest.devcloud.acquia-sites.com/simplesaml/<companyname>` |
    | `https://www.directionsonmicrosoft.com/simplesaml/<companyname>` |

    > [!NOTE] 
    > <span data-ttu-id="a83af-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="a83af-161">These values are not real.</span></span> <span data-ttu-id="a83af-162">Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="a83af-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="a83af-163">Ügyfél [Microsoft Client irányú támogatási csoport](mailto:service@DirectionsOnMicrosoft.com) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="a83af-163">Contact [Directions on Microsoft Client support team](mailto:service@DirectionsOnMicrosoft.com) tooget these values.</span></span> 
 
4. <span data-ttu-id="a83af-164">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="a83af-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-directions-microsoft-tutorial/tutorial_directionsonmicrosoft_certificate.png) 

5. <span data-ttu-id="a83af-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="a83af-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-directions-microsoft-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="a83af-168">tooconfigure egyszeri bejelentkezést a **Microsoft irányú** oldalon kell letöltött toosend hello **metaadatainak XML-kódja** túl[utasításait a Microsoft támogatási csoport](mailto:service@DirectionsOnMicrosoft.com).</span><span class="sxs-lookup"><span data-stu-id="a83af-168">tooconfigure single sign-on on **Directions on Microsoft** side, you need toosend hello downloaded **Metadata XML** too[Directions on Microsoft support team](mailto:service@DirectionsOnMicrosoft.com).</span></span> <span data-ttu-id="a83af-169">tooenable hello utasításait a Microsoft team toolocate támogatja az összevont telephelyi tagság, a vállalati adatok szerepeljenek az e-maileket.</span><span class="sxs-lookup"><span data-stu-id="a83af-169">tooenable hello Directions on Microsoft support team toolocate your federated site membership, include your company information in your email.</span></span>
    
    >[!NOTE]
    ><span data-ttu-id="a83af-170">Egyszeri bejelentkezés a Microsoft irányú kell hello által engedélyezett toobe [Microsoft Client irányú támogatási csoport](mailto:service@DirectionsOnMicrosoft.com).</span><span class="sxs-lookup"><span data-stu-id="a83af-170">Single sign-on for Directions on Microsoft needs toobe enabled by hello [Directions on Microsoft Client support team](mailto:service@DirectionsOnMicrosoft.com).</span></span> <span data-ttu-id="a83af-171">Egy értesítést fog kapni, amikor az egyszeri bejelentkezés engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="a83af-171">You will receive a notification when single sign-on has been enabled.</span></span>

> [!TIP]
> <span data-ttu-id="a83af-172">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="a83af-172">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="a83af-173">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="a83af-173">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="a83af-174">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="a83af-174">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="a83af-175">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="a83af-175">Creating an Azure AD test user</span></span>
<span data-ttu-id="a83af-176">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="a83af-176">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="a83af-178">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="a83af-178">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="a83af-179">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="a83af-179">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-directions-microsoft-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="a83af-181">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="a83af-181">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-directions-microsoft-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="a83af-183">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="a83af-183">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-directions-microsoft-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="a83af-185">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="a83af-185">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-directions-microsoft-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="a83af-187">a.</span><span class="sxs-lookup"><span data-stu-id="a83af-187">a.</span></span> <span data-ttu-id="a83af-188">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="a83af-188">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="a83af-189">b.</span><span class="sxs-lookup"><span data-stu-id="a83af-189">b.</span></span> <span data-ttu-id="a83af-190">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="a83af-190">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="a83af-191">c.</span><span class="sxs-lookup"><span data-stu-id="a83af-191">c.</span></span> <span data-ttu-id="a83af-192">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="a83af-192">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="a83af-193">d.</span><span class="sxs-lookup"><span data-stu-id="a83af-193">d.</span></span> <span data-ttu-id="a83af-194">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="a83af-194">Click **Create**.</span></span>
 
### <a name="creating-a-directions-on-microsoft-test-user"></a><span data-ttu-id="a83af-195">Egy irányban Microsoft tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="a83af-195">Creating a Directions on Microsoft test user</span></span>

<span data-ttu-id="a83af-196">Nincs a akkor tooconfigure felhasználók átadásához Microsoft tooDirections művelet elem.</span><span class="sxs-lookup"><span data-stu-id="a83af-196">There is no action item for you tooconfigure user provisioning tooDirections on Microsoft.</span></span>  

<span data-ttu-id="a83af-197">Ha egy hozzárendelt felhasználó megpróbál a Microsoft hello hozzáférési panelen tooDirections toolog, utasításait a Microsoft ellenőrzi, hogy létezik-e hello felhasználó.</span><span class="sxs-lookup"><span data-stu-id="a83af-197">When an assigned user tries toolog in tooDirections on Microsoft using hello access panel, Directions on Microsoft checks whether hello user exists.</span></span> <span data-ttu-id="a83af-198">Nincs még nincs felhasználói fiók érhető el, ha a Microsoft irányú automatikusan létrehozni.</span><span class="sxs-lookup"><span data-stu-id="a83af-198">If there is no user account available yet, it is automatically created by Directions on Microsoft.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="a83af-199">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="a83af-199">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="a83af-200">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés a Microsoft access tooDirections megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="a83af-200">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooDirections on Microsoft.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="a83af-202">**tooassign Britta Simon tooDirections a Microsoft, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="a83af-202">**tooassign Britta Simon tooDirections on Microsoft, perform hello following steps:**</span></span>

1. <span data-ttu-id="a83af-203">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="a83af-203">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="a83af-205">Hello alkalmazások listában válassza ki a **Microsoft irányú**.</span><span class="sxs-lookup"><span data-stu-id="a83af-205">In hello applications list, select **Directions on Microsoft**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-directions-microsoft-tutorial/tutorial_directionsonmicrosoft_app.png) 

3. <span data-ttu-id="a83af-207">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="a83af-207">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="a83af-209">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="a83af-209">Click **Add** button.</span></span> <span data-ttu-id="a83af-210">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="a83af-210">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="a83af-212">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="a83af-212">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="a83af-213">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="a83af-213">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="a83af-214">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="a83af-214">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="a83af-215">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="a83af-215">Testing single sign-on</span></span>

<span data-ttu-id="a83af-216">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="a83af-216">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>
 
<span data-ttu-id="a83af-217">Hello utasításait a hozzáférési Panel hello a Microsoft csempére kattintva, a Microsoft alkalmazás kapja meg automatikusan bejelentkezett tooyour utasításait.</span><span class="sxs-lookup"><span data-stu-id="a83af-217">When you click hello Directions on Microsoft tile in hello Access Panel, you should get automatically signed-on tooyour Directions on Microsoft application.</span></span>

<span data-ttu-id="a83af-218">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="a83af-218">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="a83af-219">További források</span><span class="sxs-lookup"><span data-stu-id="a83af-219">Additional resources</span></span>

* [<span data-ttu-id="a83af-220">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="a83af-220">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="a83af-221">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="a83af-221">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-directions-microsoft-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-directions-microsoft-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-directions-microsoft-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-directions-microsoft-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-directions-microsoft-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-directions-microsoft-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-directions-microsoft-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-directions-microsoft-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-directions-microsoft-tutorial/tutorial_general_203.png

