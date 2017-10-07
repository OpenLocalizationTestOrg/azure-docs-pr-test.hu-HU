---
title: "Oktatóanyag: Azure Active Directoryval integrált CompetencyIQ |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és CompetencyIQ között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: e262bf7e-cc7d-4d0e-aea7-861f00d8837d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/21/2017
ms.author: jeedes
ms.openlocfilehash: c032884b092da85684e24e98f75371475e09322d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-competencyiq"></a><span data-ttu-id="bafa9-103">Oktatóanyag: Azure Active Directoryval integrált CompetencyIQ</span><span class="sxs-lookup"><span data-stu-id="bafa9-103">Tutorial: Azure Active Directory integration with CompetencyIQ</span></span>

<span data-ttu-id="bafa9-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate CompetencyIQ az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="bafa9-104">In this tutorial, you learn how toointegrate CompetencyIQ with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="bafa9-105">CompetencyIQ integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="bafa9-105">Integrating CompetencyIQ with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="bafa9-106">Megadhatja a hozzáférés tooCompetencyIQ rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="bafa9-106">You can control in Azure AD who has access tooCompetencyIQ</span></span>
- <span data-ttu-id="bafa9-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooCompetencyIQ (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="bafa9-107">You can enable your users tooautomatically get signed-on tooCompetencyIQ (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="bafa9-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="bafa9-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="bafa9-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="bafa9-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bafa9-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="bafa9-110">Prerequisites</span></span>

<span data-ttu-id="bafa9-111">az Azure AD integrálása CompetencyIQ tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="bafa9-111">tooconfigure Azure AD integration with CompetencyIQ, you need hello following items:</span></span>

- <span data-ttu-id="bafa9-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="bafa9-112">An Azure AD subscription</span></span>
- <span data-ttu-id="bafa9-113">Egy CompetencyIQ egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="bafa9-113">A CompetencyIQ single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="bafa9-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="bafa9-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="bafa9-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="bafa9-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="bafa9-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="bafa9-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="bafa9-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="bafa9-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="bafa9-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="bafa9-118">Scenario description</span></span>
<span data-ttu-id="bafa9-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="bafa9-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="bafa9-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="bafa9-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="bafa9-121">Hello gyűjteményből CompetencyIQ hozzáadása</span><span class="sxs-lookup"><span data-stu-id="bafa9-121">Adding CompetencyIQ from hello gallery</span></span>
2. <span data-ttu-id="bafa9-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="bafa9-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-competencyiq-from-hello-gallery"></a><span data-ttu-id="bafa9-123">Hello gyűjteményből CompetencyIQ hozzáadása</span><span class="sxs-lookup"><span data-stu-id="bafa9-123">Adding CompetencyIQ from hello gallery</span></span>
<span data-ttu-id="bafa9-124">tooconfigure hello integrációja CompetencyIQ az Azure AD-be, meg kell tooadd CompetencyIQ hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="bafa9-124">tooconfigure hello integration of CompetencyIQ into Azure AD, you need tooadd CompetencyIQ from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="bafa9-125">**tooadd CompetencyIQ hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="bafa9-125">**tooadd CompetencyIQ from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="bafa9-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="bafa9-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="bafa9-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="bafa9-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="bafa9-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="bafa9-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="bafa9-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="bafa9-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="bafa9-133">Hello keresési mezőbe, írja be a **CompetencyIQ**.</span><span class="sxs-lookup"><span data-stu-id="bafa9-133">In hello search box, type **CompetencyIQ**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-competencyiq-tutorial/tutorial_competencyiq_search.png)

5. <span data-ttu-id="bafa9-135">A hello eredmények panelen válassza ki a **CompetencyIQ**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="bafa9-135">In hello results panel, select **CompetencyIQ**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-competencyiq-tutorial/tutorial_competencyiq_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="bafa9-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="bafa9-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="bafa9-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján CompetencyIQ</span><span class="sxs-lookup"><span data-stu-id="bafa9-138">In this section, you configure and test Azure AD single sign-on with CompetencyIQ based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="bafa9-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó CompetencyIQ tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="bafa9-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in CompetencyIQ is tooa user in Azure AD.</span></span> <span data-ttu-id="bafa9-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello CompetencyIQ közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="bafa9-140">In other words, a link relationship between an Azure AD user and hello related user in CompetencyIQ needs toobe established.</span></span>

<span data-ttu-id="bafa9-141">CompetencyIQ, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="bafa9-141">In CompetencyIQ, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="bafa9-142">tooconfigure és az Azure AD az egyszeri bejelentkezés CompetencyIQ-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="bafa9-142">tooconfigure and test Azure AD single sign-on with CompetencyIQ, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="bafa9-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="bafa9-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="bafa9-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="bafa9-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="bafa9-145">**[CompetencyIQ tesztfelhasználó létrehozása](#creating-a-competencyiq-test-user)**  -toohave egy megfelelője a Britta Simon a CompetencyIQ, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="bafa9-145">**[Creating a CompetencyIQ test user](#creating-a-competencyiq-test-user)** - toohave a counterpart of Britta Simon in CompetencyIQ that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="bafa9-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="bafa9-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="bafa9-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="bafa9-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="bafa9-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="bafa9-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="bafa9-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az CompetencyIQ alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="bafa9-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your CompetencyIQ application.</span></span>

<span data-ttu-id="bafa9-150">**az Azure AD tooconfigure egyszeri bejelentkezést a CompetencyIQ, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="bafa9-150">**tooconfigure Azure AD single sign-on with CompetencyIQ, perform hello following steps:**</span></span>

1. <span data-ttu-id="bafa9-151">Az Azure portál, a hello hello **CompetencyIQ** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="bafa9-151">In hello Azure portal, on hello **CompetencyIQ** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="bafa9-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="bafa9-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-competencyiq-tutorial/tutorial_competencyiq_samlbase.png)

3. <span data-ttu-id="bafa9-155">A hello **CompetencyIQ tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="bafa9-155">On hello **CompetencyIQ Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-competencyiq-tutorial/tutorial_competencyiq_url1.png)

    <span data-ttu-id="bafa9-157">a.</span><span class="sxs-lookup"><span data-stu-id="bafa9-157">a.</span></span> <span data-ttu-id="bafa9-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<customer>.competencyiq.com/`</span><span class="sxs-lookup"><span data-stu-id="bafa9-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<customer>.competencyiq.com/`</span></span>
    
    <span data-ttu-id="bafa9-159">b.</span><span class="sxs-lookup"><span data-stu-id="bafa9-159">b.</span></span> <span data-ttu-id="bafa9-160">A hello **azonosító** szövegmezőhöz típus hello URL-címe:`https://www.competencyiq.com/`</span><span class="sxs-lookup"><span data-stu-id="bafa9-160">In hello **Identifier** textbox, type hello URL: `https://www.competencyiq.com/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="bafa9-161">hello bejelentkezési URL-címével van nem valódi, így frissítheti a tényleges bejelentkezési URL-címet.</span><span class="sxs-lookup"><span data-stu-id="bafa9-161">hello Sign-on URL value is not real so update this with actual Sign-On URL.</span></span> <span data-ttu-id="bafa9-162">Ügyfél [CompetencyIQ ügyfél-támogatási csoport](https://www.competencyiq.com/) tooget ez.</span><span class="sxs-lookup"><span data-stu-id="bafa9-162">Contact [CompetencyIQ Client support team](https://www.competencyiq.com/) tooget this.</span></span> 
 
4. <span data-ttu-id="bafa9-163">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="bafa9-163">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-competencyiq-tutorial/tutorial_competencyiq_certificate.png) 

5. <span data-ttu-id="bafa9-165">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="bafa9-165">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-competencyiq-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="bafa9-167">A hello **CompetencyIQ konfigurációs** kattintson **konfigurálása CompetencyIQ** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="bafa9-167">On hello **CompetencyIQ Configuration** section, click **Configure CompetencyIQ** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="bafa9-168">Másolás hello **SAML Entitásazonosító**, és **SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="bafa9-168">Copy hello **SAML Entity ID**, and **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-competencyiq-tutorial/tutorial_competencyiq_configure.png) 

7. <span data-ttu-id="bafa9-170">tooconfigure egyszeri bejelentkezést a **CompetencyIQ** oldalon kell letöltött toosend hello **metaadatainak XML-kódja**, **SAML Entitásazonosító** és **SAML-alapú egyszeri bejelentkezést. URL-címe** túl[CompetencyIQ támogatási csoport](https://www.competencyiq.com/).</span><span class="sxs-lookup"><span data-stu-id="bafa9-170">tooconfigure single sign-on on **CompetencyIQ** side, you need toosend hello downloaded **Metadata XML**, **SAML Entity ID** and **SAML Single Sign-On Service URL** too[CompetencyIQ support team](https://www.competencyiq.com/).</span></span> <span data-ttu-id="bafa9-171">Maguk állítják be ezt a beállítást toohave hello SAML SSO kapcsolat mindkét oldalán megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="bafa9-171">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="bafa9-172">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="bafa9-172">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="bafa9-173">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="bafa9-173">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="bafa9-174">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="bafa9-174">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="bafa9-175">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="bafa9-175">Creating an Azure AD test user</span></span>
<span data-ttu-id="bafa9-176">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="bafa9-176">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="bafa9-178">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="bafa9-178">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="bafa9-179">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="bafa9-179">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-competencyiq-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="bafa9-181">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="bafa9-181">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-competencyiq-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="bafa9-183">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="bafa9-183">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-competencyiq-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="bafa9-185">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="bafa9-185">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-competencyiq-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="bafa9-187">a.</span><span class="sxs-lookup"><span data-stu-id="bafa9-187">a.</span></span> <span data-ttu-id="bafa9-188">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="bafa9-188">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="bafa9-189">b.</span><span class="sxs-lookup"><span data-stu-id="bafa9-189">b.</span></span> <span data-ttu-id="bafa9-190">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="bafa9-190">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="bafa9-191">c.</span><span class="sxs-lookup"><span data-stu-id="bafa9-191">c.</span></span> <span data-ttu-id="bafa9-192">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="bafa9-192">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="bafa9-193">d.</span><span class="sxs-lookup"><span data-stu-id="bafa9-193">d.</span></span> <span data-ttu-id="bafa9-194">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="bafa9-194">Click **Create**.</span></span>
 
### <a name="creating-a-competencyiq-test-user"></a><span data-ttu-id="bafa9-195">CompetencyIQ tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="bafa9-195">Creating a CompetencyIQ test user</span></span>

<span data-ttu-id="bafa9-196">az Azure AD tooenable felhasználók toolog a tooCompetencyIQ, akkor ki kell építenie CompetencyIQ be.</span><span class="sxs-lookup"><span data-stu-id="bafa9-196">tooenable Azure AD users toolog in tooCompetencyIQ, they must be provisioned into CompetencyIQ.</span></span> <span data-ttu-id="bafa9-197">Ügyfél [CompetencyIQ támogatási csoport](https://www.competencyiq.com/) toocreate felhasználók hello alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="bafa9-197">Contact [CompetencyIQ support team](https://www.competencyiq.com/) toocreate users in hello application.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="bafa9-198">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="bafa9-198">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="bafa9-199">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooCompetencyIQ megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="bafa9-199">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooCompetencyIQ.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="bafa9-201">**tooassign Britta Simon tooCompetencyIQ, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="bafa9-201">**tooassign Britta Simon tooCompetencyIQ, perform hello following steps:**</span></span>

1. <span data-ttu-id="bafa9-202">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="bafa9-202">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="bafa9-204">Hello alkalmazások listában válassza ki a **CompetencyIQ**.</span><span class="sxs-lookup"><span data-stu-id="bafa9-204">In hello applications list, select **CompetencyIQ**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-competencyiq-tutorial/tutorial_competencyiq_app.png) 

3. <span data-ttu-id="bafa9-206">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="bafa9-206">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="bafa9-208">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="bafa9-208">Click **Add** button.</span></span> <span data-ttu-id="bafa9-209">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="bafa9-209">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="bafa9-211">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="bafa9-211">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="bafa9-212">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="bafa9-212">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="bafa9-213">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="bafa9-213">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="bafa9-214">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="bafa9-214">Testing single sign-on</span></span>

<span data-ttu-id="bafa9-215">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="bafa9-215">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="bafa9-216">Ha hello CompetencyIQ csempe a hozzáférési Panel hello gombra kattint, meg kell automatikusan naplózásra hello alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="bafa9-216">When you click hello CompetencyIQ tile in hello Access Panel, you should get automatically logged into hello application.</span></span>
<span data-ttu-id="bafa9-217">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="bafa9-217">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="bafa9-218">További források</span><span class="sxs-lookup"><span data-stu-id="bafa9-218">Additional resources</span></span>

* [<span data-ttu-id="bafa9-219">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="bafa9-219">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="bafa9-220">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="bafa9-220">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-competencyiq-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-competencyiq-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-competencyiq-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-competencyiq-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-competencyiq-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-competencyiq-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-competencyiq-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-competencyiq-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-competencyiq-tutorial/tutorial_general_203.png

