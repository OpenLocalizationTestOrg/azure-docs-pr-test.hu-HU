---
title: "Oktatóanyag: Azure Active Directory-integráció a T & E Express |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a T & E Express között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: B42374E5-2559-4309-8EF2-820BEE7EBB0C
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/03/2017
ms.author: jeedes
ms.openlocfilehash: 9a568ace8dbc75fadbf37554996b1b597a813d56
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-te-express"></a><span data-ttu-id="84766-103">Oktatóanyag: Azure Active Directory-integráció a T & E Express</span><span class="sxs-lookup"><span data-stu-id="84766-103">Tutorial: Azure Active Directory integration with T&E Express</span></span>

<span data-ttu-id="84766-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate T & E Express, az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="84766-104">In this tutorial, you learn how toointegrate T&E Express with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="84766-105">T & E Express integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="84766-105">Integrating T&E Express with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="84766-106">Szabályozhatja, hogy az Azure AD hozzáférési eszköz rendelkező & E Express</span><span class="sxs-lookup"><span data-stu-id="84766-106">You can control in Azure AD who has access tooT&E Express</span></span>
- <span data-ttu-id="84766-107">Az Azure AD-fiókok a engedélyezheti a felhasználók tooautomatically get bejelentkezett eszköz & E Express (egyszeri bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="84766-107">You can enable your users tooautomatically get signed-on tooT&E Express (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="84766-108">Kezelheti a fiókokat, egy központi helyen - hello Azure felügyeleti portálon</span><span class="sxs-lookup"><span data-stu-id="84766-108">You can manage your accounts in one central location - hello Azure Management portal</span></span>

<span data-ttu-id="84766-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="84766-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="84766-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="84766-110">Prerequisites</span></span>

<span data-ttu-id="84766-111">az Azure AD integrálása a T & E Express tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="84766-111">tooconfigure Azure AD integration with T&E Express, you need hello following items:</span></span>

- <span data-ttu-id="84766-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="84766-112">An Azure AD subscription</span></span>
- <span data-ttu-id="84766-113">A T & E Express egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="84766-113">A T&E Express single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="84766-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="84766-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="84766-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="84766-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="84766-116">Ne használja az éles környezetben, ha ez nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="84766-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="84766-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="84766-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="84766-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="84766-118">Scenario description</span></span>
<span data-ttu-id="84766-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="84766-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="84766-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="84766-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="84766-121">T & E Express hozzáadása hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="84766-121">Adding T&E Express from hello gallery</span></span>
2. <span data-ttu-id="84766-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="84766-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-te-express-from-hello-gallery"></a><span data-ttu-id="84766-123">T & E Express hozzáadása hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="84766-123">Adding T&E Express from hello gallery</span></span>
<span data-ttu-id="84766-124">tooconfigure hello integrációs olyan példá & E Express, az Azure AD-be, szükség tooadd T & E Express a hello gyűjteménye tooyour kezelt SaaS-alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="84766-124">tooconfigure hello integration of T&E Express into Azure AD, you need tooadd T&E Express from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="84766-125">**tooadd T & E Express hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="84766-125">**tooadd T&E Express from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="84766-126">A hello  **[Azure felügyeleti portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="84766-126">In hello **[Azure Management Portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="84766-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="84766-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="84766-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="84766-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="84766-131">Kattintson a **Hozzáadás** hello párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="84766-131">Click **Add** button on hello top of hello dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="84766-133">Hello keresési mezőbe, írja be a **T & E Express**.</span><span class="sxs-lookup"><span data-stu-id="84766-133">In hello search box, type **T&E Express**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-tyeexpress-tutorial/tutorial_tyeexpress_search.png)

5. <span data-ttu-id="84766-135">A hello eredmények panelen válassza ki a **T & E Express**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="84766-135">In hello results panel, select **T&E Express**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-tyeexpress-tutorial/tutorial_tyeexpress_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="84766-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="84766-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="84766-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést a T & E Express "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="84766-138">In this section, you configure and test Azure AD single sign-on with T&E Express based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="84766-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói a T & E Express tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="84766-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in T&E Express is tooa user in Azure AD.</span></span> <span data-ttu-id="84766-140">Más szóval hivatkozás közötti kapcsolat egy Azure AD-felhasználó és a kapcsolódó felhasználói hello a T & E Express igények toobe létrehozott.</span><span class="sxs-lookup"><span data-stu-id="84766-140">In other words, a link relationship between an Azure AD user and hello related user in T&E Express needs toobe established.</span></span>

<span data-ttu-id="84766-141">Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** a T & E Express.</span><span class="sxs-lookup"><span data-stu-id="84766-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in T&E Express.</span></span>

<span data-ttu-id="84766-142">tooconfigure és tesztelése az Azure AD egyszeri bejelentkezést a T & E Express, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="84766-142">tooconfigure and test Azure AD single sign-on with T&E Express, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="84766-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="84766-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="84766-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="84766-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="84766-145">**[T & E Express tesztfelhasználó létrehozása](#creating-a-te-express-test-user)**  -toohave Britta Simon a T & E Express, az Azure AD csatolt toohello ábrázolása rá, hogy valami.</span><span class="sxs-lookup"><span data-stu-id="84766-145">**[Creating a T&E Express test user](#creating-a-te-express-test-user)** - toohave a counterpart of Britta Simon in T&E Express that is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="84766-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="84766-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="84766-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="84766-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="84766-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="84766-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="84766-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel hello Azure felügyeleti portálon, és a T & E Express alkalmazást az egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="84766-149">In this section, you enable Azure AD single sign-on in hello Azure Management portal and configure single sign-on in your T&E Express application.</span></span>

<span data-ttu-id="84766-150">**T & E Express, az Azure AD az egyszeri bejelentkezés tooconfigure hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="84766-150">**tooconfigure Azure AD single sign-on with T&E Express, perform hello following steps:**</span></span>

1. <span data-ttu-id="84766-151">Hello Azure felügyeleti portálon, a hello **T & E Express** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="84766-151">In hello Azure Management portal, on hello **T&E Express** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="84766-153">A hello **egyszeri bejelentkezés** párbeszédpanel, mint **mód** kiválasztása **SAML-alapú bejelentkezés** tooenable az egyszeri bejelentkezés.</span><span class="sxs-lookup"><span data-stu-id="84766-153">On hello **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** tooenable single sign on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-tyeexpress-tutorial/tutorial_tyeexpress_samlbase.png)

3. <span data-ttu-id="84766-155">A hello **& E Express tartományi és URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="84766-155">On hello **T&E Express Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-tyeexpress-tutorial/tutorial_tyeexpress_url.png)

    <span data-ttu-id="84766-157">a.</span><span class="sxs-lookup"><span data-stu-id="84766-157">a.</span></span> <span data-ttu-id="84766-158">A hello **azonosító** szövegmezőhöz hello érték típusa:`https://<domain>.tyeexpress.com`</span><span class="sxs-lookup"><span data-stu-id="84766-158">In hello **Identifier** textbox, type hello value as: `https://<domain>.tyeexpress.com`</span></span>

    <span data-ttu-id="84766-159">b.</span><span class="sxs-lookup"><span data-stu-id="84766-159">b.</span></span> <span data-ttu-id="84766-160">A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<domain>.tyeexpress.com/authorize/samlConsume.aspx`</span><span class="sxs-lookup"><span data-stu-id="84766-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<domain>.tyeexpress.com/authorize/samlConsume.aspx`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="84766-161">Ne feledje, hogy ezek nincsenek hello valódi értékek.</span><span class="sxs-lookup"><span data-stu-id="84766-161">Please note that these are not hello real values.</span></span> <span data-ttu-id="84766-162">Tooupdate rendelkezik ezekkel az értékekkel rendelkező hello tényleges azonosítója és válasz URL-címet.</span><span class="sxs-lookup"><span data-stu-id="84766-162">You have tooupdate these values with hello actual Identifier and Reply URL.</span></span> <span data-ttu-id="84766-163">Itt javasoljuk, hogy Ön toouse hello egyedi karakterlánc értéke a hello azonosítója.</span><span class="sxs-lookup"><span data-stu-id="84766-163">Here we suggest you toouse hello unique value of string in hello Identifier.</span></span> <span data-ttu-id="84766-164">Ügyfél [T & E Express támogatási csoport](http://www.tyeexpress.com/contacto.aspx) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="84766-164">Contact [T&E Express support team](http://www.tyeexpress.com/contacto.aspx) tooget these values.</span></span>

5. <span data-ttu-id="84766-165">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello XML-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="84766-165">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello XML file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-tyeexpress-tutorial/tutorial_tyeexpress_certificate.png) 

6. <span data-ttu-id="84766-167">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="84766-167">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-tyeexpress-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="84766-169">tooconfigure egyszeri bejelentkezést a **T & E expressz** ügyféloldali, bejelentkezési toohello T & E express alkalmazás nélkül SAML-alapú egyszeri bejelentkezés a rendszergazdai hitelesítő adataival.</span><span class="sxs-lookup"><span data-stu-id="84766-169">tooconfigure single sign-on on **T&E Express** side, login toohello T&E express application without SAML single sign on using admin credentials.</span></span>

9. <span data-ttu-id="84766-170">A hello **Admin** lapra, kattintson a **SAML tartomány** tooOpen hello SAML beállítások lapon.</span><span class="sxs-lookup"><span data-stu-id="84766-170">Under hello **Admin** Tab, Click on **SAML domain** tooOpen hello SAML settings page.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-tyeexpress-tutorial/tye-SAML.png)

10. <span data-ttu-id="84766-172">Jelölje be hello **Activar(Activate)** parancsát **nem** túl**SI(Yes)**.</span><span class="sxs-lookup"><span data-stu-id="84766-172">Select hello **Activar(Activate)** option from **No** too**SI(Yes)**.</span></span> <span data-ttu-id="84766-173">A hello **Identity Provider metaadatok** szövegmezőhöz Beillesztés hello metaadatok XML, amelyekre donwloaded Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="84766-173">In hello **Identity Provider Metadata** textbox, paste hello metadata XML which you have donwloaded from Azure portal.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-tyeexpress-tutorial/tyeAdmin.png)

11. <span data-ttu-id="84766-175">Kattintson a hello **Guardar(Save)** toosave hello-beállítások gombra.</span><span class="sxs-lookup"><span data-stu-id="84766-175">Click on hello **Guardar(Save)** button toosave hello settings.</span></span> 


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="84766-176">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="84766-176">Creating an Azure AD test user</span></span>
<span data-ttu-id="84766-177">hello ebben a szakaszban célja toocreate tesztfelhasználó Britta Simon nevű hello Azure felügyeleti portálon.</span><span class="sxs-lookup"><span data-stu-id="84766-177">hello objective of this section is toocreate a test user in hello Azure Management portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="84766-179">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="84766-179">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="84766-180">A hello **Azure Management portal**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="84766-180">In hello **Azure Management portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-tyeexpress-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="84766-182">Nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó** toodisplay hello azoknak a felhasználóknak.</span><span class="sxs-lookup"><span data-stu-id="84766-182">Go too**Users and groups** and click **All users** toodisplay hello list of users.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-tyeexpress-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="84766-184">Hello párbeszédpanel hello tetején kattintson **Hozzáadás** tooopen hello **felhasználói** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="84766-184">At hello top of hello dialog click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-tyeexpress-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="84766-186">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="84766-186">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-tyeexpress-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="84766-188">a.</span><span class="sxs-lookup"><span data-stu-id="84766-188">a.</span></span> <span data-ttu-id="84766-189">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="84766-189">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="84766-190">b.</span><span class="sxs-lookup"><span data-stu-id="84766-190">b.</span></span> <span data-ttu-id="84766-191">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="84766-191">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="84766-192">c.</span><span class="sxs-lookup"><span data-stu-id="84766-192">c.</span></span> <span data-ttu-id="84766-193">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="84766-193">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="84766-194">d.</span><span class="sxs-lookup"><span data-stu-id="84766-194">d.</span></span> <span data-ttu-id="84766-195">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="84766-195">Click **Create**.</span></span>
 
### <a name="creating-a-te-express-test-user"></a><span data-ttu-id="84766-196">T & E Express tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="84766-196">Creating a T&E Express test user</span></span>

<span data-ttu-id="84766-197">A sorrend tooenable az Azure AD felhasználók toolog történő T & E Express azok ki kell építenie történő T & E Express.</span><span class="sxs-lookup"><span data-stu-id="84766-197">In order tooenable Azure AD users toolog into T&E Express, they must be provisioned into T&E Express.</span></span>  
<span data-ttu-id="84766-198">T & E Express, ha egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="84766-198">In case of T&E Express, provisioning is a manual task.</span></span>

<span data-ttu-id="84766-199">**tooprovision felhasználói fiókok, hajtsa végre hello a következő lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="84766-199">**tooprovision a user accounts, perform hello following steps:**</span></span>

1. <span data-ttu-id="84766-200">Jelentkezzen be a tooyour T & E Express vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="84766-200">Log in tooyour T&E Express company site as an administrator.</span></span>

2. <span data-ttu-id="84766-201">Admin címke alatt kattintson a felhasználók tooopen hello felhasználók fő lapon.</span><span class="sxs-lookup"><span data-stu-id="84766-201">Under Admin tag, click on Users tooopen hello Users master page.</span></span>

    ![Alkalmazott hozzáadása](./media/active-directory-saas-tyeexpress-tutorial/tye-adminusers.png)

3. <span data-ttu-id="84766-203">A hello kezdőlapján kattintson a  **+**  tooadd hello felhasználók.</span><span class="sxs-lookup"><span data-stu-id="84766-203">On hello home page, click on **+** tooadd hello users.</span></span>

    ![Alkalmazott hozzáadása](./media/active-directory-saas-tyeexpress-tutorial/tye-usershome.png)

4. <span data-ttu-id="84766-205">Itt adhatja meg minden hello kötelező adatai hello formában kéri, és kattintson a Mentés gombra toosave hello részletek hello.</span><span class="sxs-lookup"><span data-stu-id="84766-205">Enter all hello mandatory details as asked in hello form and click hello save button toosave hello details.</span></span>

    ![Alkalmazott hozzáadása](./media/active-directory-saas-tyeexpress-tutorial/tye-usersadd.png)

    ![Alkalmazott hozzáadása](./media/active-directory-saas-tyeexpress-tutorial/tye-userssave.png)


### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="84766-208">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="84766-208">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="84766-209">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés saját hozzáférés eszköz & E Express megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="84766-209">In this section, you enable Britta Simon toouse Azure single sign-on by granting her access tooT&E Express.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="84766-211">**Britta Simon eszköz tooassign & E Express, hajtsa végre az alábbi lépésekkel hello:**</span><span class="sxs-lookup"><span data-stu-id="84766-211">**tooassign Britta Simon tooT&E Express, perform hello following steps:**</span></span>

1. <span data-ttu-id="84766-212">Hello Azure felügyeleti portálján, nyissa meg a hello alkalmazások megtekintése, majd toohello könyvtár nézetben keresse meg, és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="84766-212">In hello Azure Management portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="84766-214">Hello alkalmazások listában válassza ki a **T & E Express**.</span><span class="sxs-lookup"><span data-stu-id="84766-214">In hello applications list, select **T&E Express**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-tyeexpress-tutorial/tutorial_tyeexpress_app.png) 

3. <span data-ttu-id="84766-216">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="84766-216">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="84766-218">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="84766-218">Click **Add** button.</span></span> <span data-ttu-id="84766-219">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="84766-219">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="84766-221">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="84766-221">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="84766-222">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="84766-222">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="84766-223">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="84766-223">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="84766-224">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="84766-224">Testing single sign-on</span></span>

<span data-ttu-id="84766-225">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="84766-225">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="84766-226">Hello T & E Express csempe a hozzáférési Panel hello kattintáskor automatikusan bejelentkezett tooyour T & E Express alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="84766-226">When you click hello T&E Express tile in hello Access Panel, you should get automatically signed-on tooyour T&E Express application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="84766-227">További források</span><span class="sxs-lookup"><span data-stu-id="84766-227">Additional resources</span></span>

* [<span data-ttu-id="84766-228">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="84766-228">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="84766-229">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="84766-229">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-tyeexpress-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tyeexpress-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tyeexpress-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tyeexpress-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-tyeexpress-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tyeexpress-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tyeexpress-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-tyeexpress-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-tyeexpress-tutorial/tutorial_general_203.png

