---
title: "Oktatóanyag: Azure Active Directoryval integrált Anaplan |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Anaplan között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 4a9c2914-6c8c-4a88-93e3-3753afb40e6b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jeedes
ms.openlocfilehash: 38eb210d090e7aa720060680259124e941e30541
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-anaplan"></a><span data-ttu-id="63221-103">Oktatóanyag: Azure Active Directoryval integrált Anaplan</span><span class="sxs-lookup"><span data-stu-id="63221-103">Tutorial: Azure Active Directory integration with Anaplan</span></span>

<span data-ttu-id="63221-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Anaplan az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="63221-104">In this tutorial, you learn how toointegrate Anaplan with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="63221-105">Anaplan integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="63221-105">Integrating Anaplan with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="63221-106">Megadhatja a hozzáférés tooAnaplan rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="63221-106">You can control in Azure AD who has access tooAnaplan</span></span>
- <span data-ttu-id="63221-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooAnaplan (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="63221-107">You can enable your users tooautomatically get signed-on tooAnaplan (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="63221-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="63221-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="63221-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="63221-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="63221-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="63221-110">Prerequisites</span></span>

<span data-ttu-id="63221-111">az Azure AD integrálása Anaplan tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="63221-111">tooconfigure Azure AD integration with Anaplan, you need hello following items:</span></span>

- <span data-ttu-id="63221-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="63221-112">An Azure AD subscription</span></span>
- <span data-ttu-id="63221-113">Egy Anaplan egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="63221-113">An Anaplan single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="63221-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="63221-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="63221-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="63221-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="63221-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="63221-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="63221-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="63221-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="63221-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="63221-118">Scenario description</span></span>
<span data-ttu-id="63221-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="63221-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="63221-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="63221-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="63221-121">Hello gyűjteményből Anaplan hozzáadása</span><span class="sxs-lookup"><span data-stu-id="63221-121">Adding Anaplan from hello gallery</span></span>
2. <span data-ttu-id="63221-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="63221-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-anaplan-from-hello-gallery"></a><span data-ttu-id="63221-123">Hello gyűjteményből Anaplan hozzáadása</span><span class="sxs-lookup"><span data-stu-id="63221-123">Adding Anaplan from hello gallery</span></span>
<span data-ttu-id="63221-124">tooconfigure hello integrációja Anaplan az Azure AD-be, meg kell tooadd Anaplan hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="63221-124">tooconfigure hello integration of Anaplan into Azure AD, you need tooadd Anaplan from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="63221-125">**tooadd Anaplan hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="63221-125">**tooadd Anaplan from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="63221-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="63221-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="63221-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="63221-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="63221-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="63221-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="63221-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="63221-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="63221-133">Hello keresési mezőbe, írja be a **Anaplan**.</span><span class="sxs-lookup"><span data-stu-id="63221-133">In hello search box, type **Anaplan**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-anaplan-tutorial/tutorial_anaplan_search.png)

5. <span data-ttu-id="63221-135">A hello eredmények panelen válassza ki a **Anaplan**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="63221-135">In hello results panel, select **Anaplan**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-anaplan-tutorial/tutorial_anaplan_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="63221-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="63221-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="63221-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján Anaplan</span><span class="sxs-lookup"><span data-stu-id="63221-138">In this section, you configure and test Azure AD single sign-on with Anaplan based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="63221-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Anaplan tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="63221-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Anaplan is tooa user in Azure AD.</span></span> <span data-ttu-id="63221-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Anaplan közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="63221-140">In other words, a link relationship between an Azure AD user and hello related user in Anaplan needs toobe established.</span></span>

<span data-ttu-id="63221-141">Anaplan, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="63221-141">In Anaplan, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="63221-142">tooconfigure és az Azure AD az egyszeri bejelentkezés Anaplan-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="63221-142">tooconfigure and test Azure AD single sign-on with Anaplan, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="63221-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="63221-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="63221-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="63221-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="63221-145">**[Egy Anaplan tesztfelhasználó létrehozása](#creating-an-anaplan-test-user)**  -toohave egy megfelelője a Britta Simon a Anaplan, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="63221-145">**[Creating an Anaplan test user](#creating-an-anaplan-test-user)** - toohave a counterpart of Britta Simon in Anaplan that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="63221-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="63221-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="63221-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="63221-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="63221-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="63221-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="63221-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Anaplan alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="63221-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Anaplan application.</span></span>

<span data-ttu-id="63221-150">**az Azure AD tooconfigure egyszeri bejelentkezést a Anaplan, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="63221-150">**tooconfigure Azure AD single sign-on with Anaplan, perform hello following steps:**</span></span>

1. <span data-ttu-id="63221-151">Az Azure portál, a hello hello **Anaplan** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="63221-151">In hello Azure portal, on hello **Anaplan** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="63221-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="63221-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-anaplan-tutorial/tutorial_anaplan_samlbase.png)

3. <span data-ttu-id="63221-155">A hello **Anaplan tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="63221-155">On hello **Anaplan Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-anaplan-tutorial/tutorial_anaplan_url.png)

    <span data-ttu-id="63221-157">a.</span><span class="sxs-lookup"><span data-stu-id="63221-157">a.</span></span> <span data-ttu-id="63221-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://sdp.anaplan.com/frontdoor/saml/<tenant name>`</span><span class="sxs-lookup"><span data-stu-id="63221-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://sdp.anaplan.com/frontdoor/saml/<tenant name>`</span></span>

    <span data-ttu-id="63221-159">b.</span><span class="sxs-lookup"><span data-stu-id="63221-159">b.</span></span> <span data-ttu-id="63221-160">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<subdomain>.anaplan.com`</span><span class="sxs-lookup"><span data-stu-id="63221-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<subdomain>.anaplan.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="63221-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="63221-161">These values are not real.</span></span> <span data-ttu-id="63221-162">Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="63221-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="63221-163">Ügyfél [Anaplan ügyfél-támogatási csoport](mailto:support@anaplan.com) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="63221-163">Contact [Anaplan Client support team](mailto:support@anaplan.com) tooget these values.</span></span> 
 
4. <span data-ttu-id="63221-164">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="63221-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-anaplan-tutorial/tutorial_anaplan_certificate.png) 

5. <span data-ttu-id="63221-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="63221-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-anaplan-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="63221-168">A hello **Anaplan konfigurációs** kattintson **konfigurálása Anaplan** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="63221-168">On hello **Anaplan Configuration** section, click **Configure Anaplan** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="63221-169">Másolás hello **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="63221-169">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-anaplan-tutorial/tutorial_anaplan_configure.png) 

7. <span data-ttu-id="63221-171">tooconfigure egyszeri bejelentkezést a **Anaplan** oldalon kell letöltött toosend hello **metaadatainak XML-kódja**, **SAML Entitásazonosító**, **SAML-alapú egyszeri bejelentkezési URL-címe**  és **Sign-Out URL-cím** túl[Anaplan támogatási csoport](mailto:support@anaplan.com).</span><span class="sxs-lookup"><span data-stu-id="63221-171">tooconfigure single sign-on on **Anaplan** side, you need toosend hello downloaded **Metadata XML**, **SAML Entity ID**, **SAML Single Sign-On Service URL** and **Sign-Out URL**   too[Anaplan support team](mailto:support@anaplan.com).</span></span> <span data-ttu-id="63221-172">Maguk állítják be ezt a beállítást toohave hello SAML SSO kapcsolat mindkét oldalán megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="63221-172">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="63221-173">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="63221-173">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="63221-174">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="63221-174">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="63221-175">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="63221-175">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="63221-176">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="63221-176">Creating an Azure AD test user</span></span>
<span data-ttu-id="63221-177">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="63221-177">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="63221-179">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="63221-179">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="63221-180">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="63221-180">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-anaplan-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="63221-182">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="63221-182">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-anaplan-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="63221-184">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="63221-184">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-anaplan-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="63221-186">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="63221-186">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-anaplan-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="63221-188">a.</span><span class="sxs-lookup"><span data-stu-id="63221-188">a.</span></span> <span data-ttu-id="63221-189">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="63221-189">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="63221-190">b.</span><span class="sxs-lookup"><span data-stu-id="63221-190">b.</span></span> <span data-ttu-id="63221-191">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="63221-191">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="63221-192">c.</span><span class="sxs-lookup"><span data-stu-id="63221-192">c.</span></span> <span data-ttu-id="63221-193">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="63221-193">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="63221-194">d.</span><span class="sxs-lookup"><span data-stu-id="63221-194">d.</span></span> <span data-ttu-id="63221-195">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="63221-195">Click **Create**.</span></span>
 
### <a name="creating-an-anaplan-test-user"></a><span data-ttu-id="63221-196">Egy Anaplan tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="63221-196">Creating an Anaplan test user</span></span>

<span data-ttu-id="63221-197">Ebben a szakaszban egy Anaplan Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="63221-197">In this section, you create a user called Britta Simon in Anaplan.</span></span> <span data-ttu-id="63221-198">Adjon együttműködve [Anaplan támogatási csoport](mailto:support@anaplan.com) tooadd hello felhasználók hello Anaplan platform.</span><span class="sxs-lookup"><span data-stu-id="63221-198">Please work with [Anaplan support team](mailto:support@anaplan.com) tooadd hello users in hello Anaplan platform.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="63221-199">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="63221-199">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="63221-200">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooAnaplan megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="63221-200">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooAnaplan.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="63221-202">**tooassign Britta Simon tooAnaplan, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="63221-202">**tooassign Britta Simon tooAnaplan, perform hello following steps:**</span></span>

1. <span data-ttu-id="63221-203">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="63221-203">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="63221-205">Hello alkalmazások listában válassza ki a **Anaplan**.</span><span class="sxs-lookup"><span data-stu-id="63221-205">In hello applications list, select **Anaplan**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-anaplan-tutorial/tutorial_anaplan_app.png) 

3. <span data-ttu-id="63221-207">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="63221-207">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="63221-209">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="63221-209">Click **Add** button.</span></span> <span data-ttu-id="63221-210">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="63221-210">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="63221-212">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="63221-212">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="63221-213">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="63221-213">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="63221-214">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="63221-214">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="63221-215">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="63221-215">Testing single sign-on</span></span>

<span data-ttu-id="63221-216">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="63221-216">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="63221-217">Ha a hozzáférési Panel hello hello Anaplan csempe gombra kattint, automatikusan bejelentkezett tooyour Anaplan alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="63221-217">When you click hello Anaplan tile in hello Access Panel, you should get automatically signed-on tooyour Anaplan application.</span></span>

<span data-ttu-id="63221-218">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="63221-218">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="63221-219">További források</span><span class="sxs-lookup"><span data-stu-id="63221-219">Additional resources</span></span>

* [<span data-ttu-id="63221-220">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="63221-220">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="63221-221">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="63221-221">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-anaplan-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-anaplan-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-anaplan-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-anaplan-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-anaplan-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-anaplan-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-anaplan-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-anaplan-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-anaplan-tutorial/tutorial_general_203.png

