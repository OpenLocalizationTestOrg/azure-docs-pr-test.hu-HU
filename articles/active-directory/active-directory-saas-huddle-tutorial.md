---
title: "Oktatóanyag: Azure Active Directoryval integrált Huddle |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Huddle között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 8389ba4c-f5f8-4ede-b2f4-32eae844ceb0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: 0b2f6c4d839943cdd07699a1ff95dc8f90505699
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-huddle"></a><span data-ttu-id="ab2d9-103">Oktatóanyag: Azure Active Directoryval integrált Huddle</span><span class="sxs-lookup"><span data-stu-id="ab2d9-103">Tutorial: Azure Active Directory integration with Huddle</span></span>

<span data-ttu-id="ab2d9-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Huddle az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="ab2d9-104">In this tutorial, you learn how toointegrate Huddle with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="ab2d9-105">Huddle integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="ab2d9-105">Integrating Huddle with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="ab2d9-106">Megadhatja a hozzáférés tooHuddle rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="ab2d9-106">You can control in Azure AD who has access tooHuddle</span></span>
- <span data-ttu-id="ab2d9-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooHuddle (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="ab2d9-107">You can enable your users tooautomatically get signed-on tooHuddle (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="ab2d9-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="ab2d9-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="ab2d9-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="ab2d9-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ab2d9-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="ab2d9-110">Prerequisites</span></span>

<span data-ttu-id="ab2d9-111">Huddle tooconfigure az Azure AD integrálása, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="ab2d9-111">tooconfigure Azure AD integration with Huddle, you need hello following items:</span></span>

- <span data-ttu-id="ab2d9-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="ab2d9-112">An Azure AD subscription</span></span>
- <span data-ttu-id="ab2d9-113">Egy Huddle egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="ab2d9-113">A Huddle single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="ab2d9-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="ab2d9-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="ab2d9-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="ab2d9-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="ab2d9-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="ab2d9-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="ab2d9-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="ab2d9-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="ab2d9-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="ab2d9-118">Scenario description</span></span>

<span data-ttu-id="ab2d9-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="ab2d9-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="ab2d9-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="ab2d9-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="ab2d9-121">Hello gyűjteményből Huddle hozzáadása</span><span class="sxs-lookup"><span data-stu-id="ab2d9-121">Adding Huddle from hello gallery</span></span>
2. <span data-ttu-id="ab2d9-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="ab2d9-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-huddle-from-hello-gallery"></a><span data-ttu-id="ab2d9-123">Hello gyűjteményből Huddle hozzáadása</span><span class="sxs-lookup"><span data-stu-id="ab2d9-123">Adding Huddle from hello gallery</span></span>
<span data-ttu-id="ab2d9-124">tooconfigure hello integrálása az Azure AD-be Huddle, meg kell tooadd Huddle hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="ab2d9-124">tooconfigure hello integration of Huddle into Azure AD, you need tooadd Huddle from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="ab2d9-125">**tooadd Huddle hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="ab2d9-125">**tooadd Huddle from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="ab2d9-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="ab2d9-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="ab2d9-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="ab2d9-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="ab2d9-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="ab2d9-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="ab2d9-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="ab2d9-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="ab2d9-133">Hello keresési mezőbe, írja be a **Huddle**.</span><span class="sxs-lookup"><span data-stu-id="ab2d9-133">In hello search box, type **Huddle**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-huddle-tutorial/tutorial_huddle_search.png)

5. <span data-ttu-id="ab2d9-135">A hello eredmények panelen válassza ki a **Huddle**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="ab2d9-135">In hello results panel, select **Huddle**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-huddle-tutorial/tutorial_huddle_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="ab2d9-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="ab2d9-137">Configuring and testing Azure AD single sign-on</span></span>

<span data-ttu-id="ab2d9-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján Huddle</span><span class="sxs-lookup"><span data-stu-id="ab2d9-138">In this section, you configure and test Azure AD single sign-on with Huddle based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="ab2d9-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Huddle tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="ab2d9-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Huddle is tooa user in Azure AD.</span></span> <span data-ttu-id="ab2d9-140">Más szóval hivatkozás közötti kapcsolat egy Azure AD-felhasználó és a kapcsolódó felhasználóknak hello Huddle igényeinek toobe létrehozott.</span><span class="sxs-lookup"><span data-stu-id="ab2d9-140">In other words, a link relationship between an Azure AD user and hello related user in Huddle needs toobe established.</span></span>

<span data-ttu-id="ab2d9-141">Huddle, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="ab2d9-141">In Huddle, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="ab2d9-142">tooconfigure és az Azure AD az egyszeri bejelentkezés Huddle-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="ab2d9-142">tooconfigure and test Azure AD single sign-on with Huddle, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="ab2d9-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="ab2d9-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>

2. <span data-ttu-id="ab2d9-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ab2d9-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>

3. <span data-ttu-id="ab2d9-145">**[Huddle tesztfelhasználó létrehozása](#creating-a-huddle-test-user)**  -toohave egy megfelelője a Britta Simon a Huddle, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="ab2d9-145">**[Creating a Huddle test user](#creating-a-huddle-test-user)** - toohave a counterpart of Britta Simon in Huddle that is linked toohello Azure AD representation of user.</span></span>

4. <span data-ttu-id="ab2d9-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="ab2d9-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>

5. <span data-ttu-id="ab2d9-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="ab2d9-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="ab2d9-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="ab2d9-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="ab2d9-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Huddle alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="ab2d9-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Huddle application.</span></span>

<span data-ttu-id="ab2d9-150">**Huddle, az Azure AD az egyszeri bejelentkezés tooconfigure hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="ab2d9-150">**tooconfigure Azure AD single sign-on with Huddle, perform hello following steps:**</span></span>

1. <span data-ttu-id="ab2d9-151">Az Azure portál, a hello hello **Huddle** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="ab2d9-151">In hello Azure portal, on hello **Huddle** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="ab2d9-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="ab2d9-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-huddle-tutorial/tutorial_huddle_samlbase.png)

3. <span data-ttu-id="ab2d9-155">A hello **Huddle tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="ab2d9-155">On hello **Huddle Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-huddle-tutorial/tutorial_huddle_url.png)

    <span data-ttu-id="ab2d9-157">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`http://<company name>.huddle.com`</span><span class="sxs-lookup"><span data-stu-id="ab2d9-157">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `http://<company name>.huddle.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="ab2d9-158">Ez az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="ab2d9-158">This value is not real.</span></span> <span data-ttu-id="ab2d9-159">Frissítse ezt az értéket hello tényleges bejelentkezési URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="ab2d9-159">Update this value with hello actual Sign-On URL.</span></span> <span data-ttu-id="ab2d9-160">Ügyfél [Huddle ügyfél-támogatási csoport](https://huddle.zendesk.com) tooget ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="ab2d9-160">Contact [Huddle Client support team](https://huddle.zendesk.com) tooget this value.</span></span> 

4. <span data-ttu-id="ab2d9-161">A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="ab2d9-161">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-huddle-tutorial/tutorial_huddle_certificate.png) 

5. <span data-ttu-id="ab2d9-163">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="ab2d9-163">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-huddle-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="ab2d9-165">A hello **Huddle konfigurációs** kattintson **konfigurálása Huddle** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="ab2d9-165">On hello **Huddle Configuration** section, click **Configure Huddle** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="ab2d9-166">Másolás hello **SAML Entitásazonosító és SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="ab2d9-166">Copy hello **SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span> 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-huddle-tutorial/tutorial_huddle_configure.png) 
    
7. <span data-ttu-id="ab2d9-168">tooconfigure egyszeri bejelentkezés Huddle oldalon, a letöltött toosend hello kell **tanúsítvány**, **SAML-alapú egyszeri bejelentkezési URL-címe**, és **SAML Entitásazonosító** túl[Huddle ügyfél-támogatási csoport](https://huddle.zendesk.com).</span><span class="sxs-lookup"><span data-stu-id="ab2d9-168">tooconfigure single sign-on on Huddle side, you need toosend hello downloaded  **Certificate**, **SAML Single Sign-On Service URL**, and **SAML Entity ID** too[Huddle Client support team](https://huddle.zendesk.com).</span></span> <span data-ttu-id="ab2d9-169">Maguk állítják be ezt a beállítást toohave hello SAML SSO kapcsolat mindkét oldalán megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="ab2d9-169">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>  
   
    >[!NOTE]
    > <span data-ttu-id="ab2d9-170">Egyszeri bejelentkezés kell toobe hello Huddle támogatási csoport által engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="ab2d9-170">Single sign-on needs toobe enabled by hello Huddle support team.</span></span> <span data-ttu-id="ab2d9-171">Értesítést kap, amikor hello konfigurálása befejeződött.</span><span class="sxs-lookup"><span data-stu-id="ab2d9-171">You get a notification when hello configuration has been completed.</span></span> 
    > 

> [!TIP]
> <span data-ttu-id="ab2d9-172">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="ab2d9-172">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="ab2d9-173">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="ab2d9-173">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="ab2d9-174">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="ab2d9-174">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 
   
### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="ab2d9-175">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="ab2d9-175">Creating an Azure AD test user</span></span>

<span data-ttu-id="ab2d9-176">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="ab2d9-176">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="ab2d9-178">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="ab2d9-178">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="ab2d9-179">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="ab2d9-179">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-huddle-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="ab2d9-181">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="ab2d9-181">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-huddle-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="ab2d9-183">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="ab2d9-183">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-huddle-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="ab2d9-185">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="ab2d9-185">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-huddle-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="ab2d9-187">a.</span><span class="sxs-lookup"><span data-stu-id="ab2d9-187">a.</span></span> <span data-ttu-id="ab2d9-188">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="ab2d9-188">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="ab2d9-189">b.</span><span class="sxs-lookup"><span data-stu-id="ab2d9-189">b.</span></span> <span data-ttu-id="ab2d9-190">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="ab2d9-190">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="ab2d9-191">c.</span><span class="sxs-lookup"><span data-stu-id="ab2d9-191">c.</span></span> <span data-ttu-id="ab2d9-192">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="ab2d9-192">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="ab2d9-193">d.</span><span class="sxs-lookup"><span data-stu-id="ab2d9-193">d.</span></span> <span data-ttu-id="ab2d9-194">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="ab2d9-194">Click **Create**.</span></span>
 
### <a name="creating-a-huddle-test-user"></a><span data-ttu-id="ab2d9-195">Huddle tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="ab2d9-195">Creating a Huddle test user</span></span>

<span data-ttu-id="ab2d9-196">az Azure AD tooenable felhasználók toolog a tooHuddle, akkor ki kell építenie Huddle be.</span><span class="sxs-lookup"><span data-stu-id="ab2d9-196">tooenable Azure AD users toolog in tooHuddle, they must be provisioned into Huddle.</span></span> <span data-ttu-id="ab2d9-197">Huddle hello esetben egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="ab2d9-197">In hello case of Huddle, provisioning is a manual task.</span></span>

<span data-ttu-id="ab2d9-198">**tooconfigure felhasználók átadásához, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="ab2d9-198">**tooconfigure user provisioning, perform hello following steps:**</span></span>

1. <span data-ttu-id="ab2d9-199">Jelentkezzen be tooyour **Huddle** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="ab2d9-199">Log in tooyour **Huddle** company site as administrator.</span></span>
2. <span data-ttu-id="ab2d9-200">Kattintson a **munkaterület**.</span><span class="sxs-lookup"><span data-stu-id="ab2d9-200">Click **Workspace**.</span></span>
3. <span data-ttu-id="ab2d9-201">Kattintson a **személyek \> felkérése**.</span><span class="sxs-lookup"><span data-stu-id="ab2d9-201">Click **People \> Invite People**.</span></span>
   
   <span data-ttu-id="ab2d9-202">![Személyek](./media/active-directory-saas-huddle-tutorial/IC787838.png "személyek")</span><span class="sxs-lookup"><span data-stu-id="ab2d9-202">![People](./media/active-directory-saas-huddle-tutorial/IC787838.png "People")</span></span>

4. <span data-ttu-id="ab2d9-203">A hello **hozzon létre egy új meghívó** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="ab2d9-203">In hello **Create a new invitation** section, perform hello following steps:</span></span>
   
   <span data-ttu-id="ab2d9-204">![Új meghívó](./media/active-directory-saas-huddle-tutorial/IC787839.png "új meghívó")</span><span class="sxs-lookup"><span data-stu-id="ab2d9-204">![New Invitation](./media/active-directory-saas-huddle-tutorial/IC787839.png "New Invitation")</span></span>
   
   <span data-ttu-id="ab2d9-205">a.</span><span class="sxs-lookup"><span data-stu-id="ab2d9-205">a.</span></span> <span data-ttu-id="ab2d9-206">A hello **válasszon egy csoport tooinvite személyek toojoin** listáról válassza ki **team**.</span><span class="sxs-lookup"><span data-stu-id="ab2d9-206">In hello **Choose a team tooinvite people toojoin** list, select **team**.</span></span>

   <span data-ttu-id="ab2d9-207">b.</span><span class="sxs-lookup"><span data-stu-id="ab2d9-207">b.</span></span> <span data-ttu-id="ab2d9-208">Típus hello **E-mail cím** egy érvényes Azure ad fiók kívánt tooprovision túl**adja meg e-mail címét számára szeretné tooinvite** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="ab2d9-208">Type hello **Email Address** of a valid Azure AD account you want tooprovision in too**Enter email address for people you'd like tooinvite** textbox.</span></span>

   <span data-ttu-id="ab2d9-209">c.</span><span class="sxs-lookup"><span data-stu-id="ab2d9-209">c.</span></span> <span data-ttu-id="ab2d9-210">Kattintson a **meghívása**.</span><span class="sxs-lookup"><span data-stu-id="ab2d9-210">Click **Invite**.</span></span>   
   
    >[!NOTE]
    > <span data-ttu-id="ab2d9-211">hello Azure AD-fiókot tulajdonosa kap egy e-mailt, egy hivatkozás tooconfirm hello fiókot is beleértve, mielőtt aktívvá válik.</span><span class="sxs-lookup"><span data-stu-id="ab2d9-211">hello Azure AD account holder will receive an email including a link tooconfirm hello account before it becomes active.</span></span> 
    > 

>[!NOTE]
><span data-ttu-id="ab2d9-212">Bármely más Huddle felhasználói fiók létrehozása eszközök vagy Huddle tooprovision által nyújtott API-kat az Azure AD felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="ab2d9-212">You can use any other Huddle user account creation tools or APIs provided by Huddle tooprovision Azure AD user accounts.</span></span> 
> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="ab2d9-213">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="ab2d9-213">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="ab2d9-214">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooHuddle megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="ab2d9-214">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooHuddle.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="ab2d9-216">**tooassign Britta Simon tooHuddle, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="ab2d9-216">**tooassign Britta Simon tooHuddle, perform hello following steps:**</span></span>

1. <span data-ttu-id="ab2d9-217">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="ab2d9-217">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="ab2d9-219">Hello alkalmazások listában válassza ki a **Huddle**.</span><span class="sxs-lookup"><span data-stu-id="ab2d9-219">In hello applications list, select **Huddle**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-huddle-tutorial/tutorial_huddle_app.png) 

3. <span data-ttu-id="ab2d9-221">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="ab2d9-221">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="ab2d9-223">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="ab2d9-223">Click **Add** button.</span></span> <span data-ttu-id="ab2d9-224">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="ab2d9-224">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="ab2d9-226">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="ab2d9-226">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="ab2d9-227">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="ab2d9-227">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="ab2d9-228">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="ab2d9-228">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="ab2d9-229">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="ab2d9-229">Testing single sign-on</span></span>

<span data-ttu-id="ab2d9-230">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="ab2d9-230">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="ab2d9-231">Ha a hozzáférési Panel hello hello Huddle csempe gombra kattint, szerezheti be automatikusan Huddle alkalmazás bejelentkezési oldalán.</span><span class="sxs-lookup"><span data-stu-id="ab2d9-231">When you click hello Huddle tile in hello Access Panel, you should get automatically login page of Huddle application.</span></span>
<span data-ttu-id="ab2d9-232">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="ab2d9-232">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ab2d9-233">További források</span><span class="sxs-lookup"><span data-stu-id="ab2d9-233">Additional resources</span></span>

* [<span data-ttu-id="ab2d9-234">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="ab2d9-234">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="ab2d9-235">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="ab2d9-235">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-huddle-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-huddle-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-huddle-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-huddle-tutorial/tutorial_general_04.png
[100]: ./media/active-directory-saas-huddle-tutorial/tutorial_general_100.png
[200]: ./media/active-directory-saas-huddle-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-huddle-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-huddle-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-huddle-tutorial/tutorial_general_203.png
