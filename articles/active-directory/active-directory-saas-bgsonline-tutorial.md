---
title: "Oktatóanyag: Azure Active Directory-integráció a Háttérkép Online |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a Háttérkép Online között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 4fd6b29b-1b46-4fd1-9f5e-16b1c9d892cd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: b728606ded7687d424a8175d0602b6b00f398497
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-bgs-online"></a><span data-ttu-id="acff0-103">Oktatóanyag: Azure Active Directory Online szolgáltatással való integráció Háttérkép</span><span class="sxs-lookup"><span data-stu-id="acff0-103">Tutorial: Azure Active Directory integration with BGS Online</span></span>

<span data-ttu-id="acff0-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Háttérkép kapcsolódik az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="acff0-104">In this tutorial, you learn how toointegrate BGS Online with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="acff0-105">Az Azure ad szolgáltatással Háttérkép Online integrációja lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="acff0-105">Integrating BGS Online with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="acff0-106">Megadhatja a hozzáférés tooBGS Online rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="acff0-106">You can control in Azure AD who has access tooBGS Online</span></span>
- <span data-ttu-id="acff0-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooBGS Online (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="acff0-107">You can enable your users tooautomatically get signed-on tooBGS Online (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="acff0-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="acff0-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="acff0-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="acff0-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="acff0-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="acff0-110">Prerequisites</span></span>

<span data-ttu-id="acff0-111">tooconfigure az Azure AD Online szolgáltatással való integráció Háttérkép, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="acff0-111">tooconfigure Azure AD integration with BGS Online, you need hello following items:</span></span>

- <span data-ttu-id="acff0-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="acff0-112">An Azure AD subscription</span></span>
- <span data-ttu-id="acff0-113">A Háttérkép Online egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="acff0-113">A BGS Online single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="acff0-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="acff0-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="acff0-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="acff0-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="acff0-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="acff0-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="acff0-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="acff0-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="acff0-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="acff0-118">Scenario description</span></span>
<span data-ttu-id="acff0-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="acff0-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="acff0-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="acff0-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="acff0-121">Hello gyűjteményből Háttérkép Online hozzáadása</span><span class="sxs-lookup"><span data-stu-id="acff0-121">Adding BGS Online from hello gallery</span></span>
2. <span data-ttu-id="acff0-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="acff0-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-bgs-online-from-hello-gallery"></a><span data-ttu-id="acff0-123">Hello gyűjteményből Háttérkép Online hozzáadása</span><span class="sxs-lookup"><span data-stu-id="acff0-123">Adding BGS Online from hello gallery</span></span>
<span data-ttu-id="acff0-124">tooconfigure hello integrációs Háttérkép online az Azure AD-be, meg kell tooadd Háttérkép Online hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="acff0-124">tooconfigure hello integration of BGS Online into Azure AD, you need tooadd BGS Online from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="acff0-125">**hello gyűjteményből Online Háttérkép tooadd hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="acff0-125">**tooadd BGS Online from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="acff0-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="acff0-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="acff0-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="acff0-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="acff0-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="acff0-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="acff0-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="acff0-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="acff0-133">Hello keresési mezőbe, írja be a **Háttérkép Online**.</span><span class="sxs-lookup"><span data-stu-id="acff0-133">In hello search box, type **BGS Online**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bgsonline-tutorial/tutorial_bgsonline_search.png)

5. <span data-ttu-id="acff0-135">A hello eredmények panelen válassza ki a **Háttérkép Online**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="acff0-135">In hello results panel, select **BGS Online**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bgsonline-tutorial/tutorial_bgsonline_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="acff0-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="acff0-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="acff0-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést a Háttérkép Online "Britta Simon." nevű tesztfelhasználó alapján</span><span class="sxs-lookup"><span data-stu-id="acff0-138">In this section, you configure and test Azure AD single sign-on with BGS Online based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="acff0-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói a Háttérkép Online tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="acff0-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in BGS Online is tooa user in Azure AD.</span></span> <span data-ttu-id="acff0-140">Ez azt jelenti hello kapcsolódó felhasználó a Háttérkép Online és az Azure AD-felhasználó közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="acff0-140">In other words, a link relationship between an Azure AD user and hello related user in BGS Online needs toobe established.</span></span>

<span data-ttu-id="acff0-141">A Háttérkép Online rendelje hello hello értékét **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="acff0-141">In BGS Online, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="acff0-142">tooconfigure és a Háttérkép Online az Azure AD az egyszeri bejelentkezés tesztelése, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="acff0-142">tooconfigure and test Azure AD single sign-on with BGS Online, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="acff0-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="acff0-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="acff0-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="acff0-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="acff0-145">**[Háttérkép Online tesztfelhasználó létrehozása](#creating-a-bgs-online-test-user)**  -toohave egy megfelelője a Britta Simon Háttérkép Online felhasználói ábrázolása csatolt toohello az Azure AD-ban.</span><span class="sxs-lookup"><span data-stu-id="acff0-145">**[Creating a BGS Online test user](#creating-a-bgs-online-test-user)** - toohave a counterpart of Britta Simon in BGS Online that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="acff0-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="acff0-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="acff0-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="acff0-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="acff0-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="acff0-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="acff0-149">Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése és konfigurálása egyszeri bejelentkezéshez a Háttérkép Online alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="acff0-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your BGS Online application.</span></span>

<span data-ttu-id="acff0-150">**az Azure AD tooconfigure egyszeri bejelentkezést a Háttérkép Online, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="acff0-150">**tooconfigure Azure AD single sign-on with BGS Online, perform hello following steps:**</span></span>

1. <span data-ttu-id="acff0-151">Az Azure portál, a hello hello **Háttérkép Online** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="acff0-151">In hello Azure portal, on hello **BGS Online** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="acff0-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="acff0-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bgsonline-tutorial/tutorial_bgsonline_samlbase.png)

3. <span data-ttu-id="acff0-155">A hello **Háttérkép Online tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="acff0-155">On hello **BGS Online Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bgsonline-tutorial/tutorial_bgsonline_url.png)

    <span data-ttu-id="acff0-157">a.</span><span class="sxs-lookup"><span data-stu-id="acff0-157">a.</span></span> <span data-ttu-id="acff0-158">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:</span><span class="sxs-lookup"><span data-stu-id="acff0-158">In hello **Identifier** textbox, type a URL using hello following pattern:</span></span>

    <span data-ttu-id="acff0-159">Az éles környezetben használja ezt a mintát`https://<company name>.millwardbrown.report`</span><span class="sxs-lookup"><span data-stu-id="acff0-159">For production environment, use this pattern `https://<company name>.millwardbrown.report`</span></span> 

    <span data-ttu-id="acff0-160">A tesztkörnyezetben használja ezt a mintát`https://millwardbrown.marketingtracker.nl/mt5/`</span><span class="sxs-lookup"><span data-stu-id="acff0-160">For test environment, use this pattern `https://millwardbrown.marketingtracker.nl/mt5/`</span></span>

    <span data-ttu-id="acff0-161">b.</span><span class="sxs-lookup"><span data-stu-id="acff0-161">b.</span></span> <span data-ttu-id="acff0-162">A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:</span><span class="sxs-lookup"><span data-stu-id="acff0-162">In hello **Reply URL** textbox, type a URL using hello following pattern:</span></span>
    
    <span data-ttu-id="acff0-163">Az éles környezetben használja ezt a mintát`https://<company name>.millwardbrown.report/sso/saml/AssertionConsumerService.aspx`</span><span class="sxs-lookup"><span data-stu-id="acff0-163">For production environment, use this pattern `https://<company name>.millwardbrown.report/sso/saml/AssertionConsumerService.aspx`</span></span> 
      
    <span data-ttu-id="acff0-164">A tesztkörnyezetben használja ezt a mintát`https://millwardbrown.marketingtracker.nl/mt5/sso/saml/AssertionConsumerService.aspx`</span><span class="sxs-lookup"><span data-stu-id="acff0-164">For test environment, use this pattern `https://millwardbrown.marketingtracker.nl/mt5/sso/saml/AssertionConsumerService.aspx`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="acff0-165">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="acff0-165">These values are not real.</span></span> <span data-ttu-id="acff0-166">Frissítheti ezeket az értékeket hello tényleges azonosítója és a válasz URL-címmel.</span><span class="sxs-lookup"><span data-stu-id="acff0-166">Update these values with hello actual Identifier and Reply URL.</span></span> <span data-ttu-id="acff0-167">Ügyfél [Háttérkép Online támogatási csoport](mailTo:bgsdashboardteam@millwardbrown.com) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="acff0-167">Contact [BGS Online support team](mailTo:bgsdashboardteam@millwardbrown.com) tooget these values.</span></span>
 

4. <span data-ttu-id="acff0-168">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="acff0-168">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bgsonline-tutorial/tutorial_bgsonline_certificate.png) 

5. <span data-ttu-id="acff0-170">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="acff0-170">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bgsonline-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="acff0-172">A hello **Háttérkép Online konfigurációs** kattintson **Háttérkép Online konfigurálása** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="acff0-172">On hello **BGS Online Configuration** section, click **Configure BGS Online** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="acff0-173">Másolás hello **SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="acff0-173">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bgsonline-tutorial/tutorial_bgsonline_configure.png) 

7. <span data-ttu-id="acff0-175">tooconfigure egyszeri bejelentkezést a **Háttérkép Online** oldalon kell letöltött toosend hello **metaadatainak XML-kódja** és **SAML-alapú egyszeri bejelentkezési URL-címe** túl[Háttérkép Online támogatási csoport](mailto:bgsdashboardteam@millwardbrown.com).</span><span class="sxs-lookup"><span data-stu-id="acff0-175">tooconfigure single sign-on on **BGS Online** side, you need toosend hello downloaded **Metadata XML** and **SAML Single Sign-On Service URL** too[BGS Online support team](mailto:bgsdashboardteam@millwardbrown.com).</span></span> 


> [!TIP]
> <span data-ttu-id="acff0-176">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="acff0-176">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="acff0-177">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="acff0-177">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="acff0-178">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="acff0-178">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="acff0-179">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="acff0-179">Creating an Azure AD test user</span></span>
<span data-ttu-id="acff0-180">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="acff0-180">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="acff0-182">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="acff0-182">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="acff0-183">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="acff0-183">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bgsonline-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="acff0-185">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="acff0-185">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bgsonline-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="acff0-187">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="acff0-187">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bgsonline-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="acff0-189">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="acff0-189">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bgsonline-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="acff0-191">a.</span><span class="sxs-lookup"><span data-stu-id="acff0-191">a.</span></span> <span data-ttu-id="acff0-192">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="acff0-192">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="acff0-193">b.</span><span class="sxs-lookup"><span data-stu-id="acff0-193">b.</span></span> <span data-ttu-id="acff0-194">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="acff0-194">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="acff0-195">c.</span><span class="sxs-lookup"><span data-stu-id="acff0-195">c.</span></span> <span data-ttu-id="acff0-196">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="acff0-196">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="acff0-197">d.</span><span class="sxs-lookup"><span data-stu-id="acff0-197">d.</span></span> <span data-ttu-id="acff0-198">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="acff0-198">Click **Create**.</span></span>
 
### <a name="creating-a-bgs-online-test-user"></a><span data-ttu-id="acff0-199">Háttérkép Online tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="acff0-199">Creating a BGS Online test user</span></span>

<span data-ttu-id="acff0-200">Ebben a szakaszban a felhasználók, Háttérkép Online Britta Simon meghívta hoz létre.</span><span class="sxs-lookup"><span data-stu-id="acff0-200">In this section, you create a user called Britta Simon in BGS Online.</span></span> <span data-ttu-id="acff0-201">Együttműködve [Háttérkép Online támogatási csoport](mailto:bgsdashboardteam@millwardbrown.com) tooadd hello felhasználók, Háttérkép Online hello platform.</span><span class="sxs-lookup"><span data-stu-id="acff0-201">Work with [BGS Online support team](mailto:bgsdashboardteam@millwardbrown.com) tooadd hello users in hello BGS Online platform.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="acff0-202">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="acff0-202">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="acff0-203">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés Online hozzáférés tooBGS megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="acff0-203">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooBGS Online.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="acff0-205">**tooassign Britta Simon tooBGS Online, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="acff0-205">**tooassign Britta Simon tooBGS Online, perform hello following steps:**</span></span>

1. <span data-ttu-id="acff0-206">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="acff0-206">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="acff0-208">Hello alkalmazások listában válassza ki a **Háttérkép Online**.</span><span class="sxs-lookup"><span data-stu-id="acff0-208">In hello applications list, select **BGS Online**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bgsonline-tutorial/tutorial_bgsonline_app.png) 

3. <span data-ttu-id="acff0-210">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="acff0-210">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="acff0-212">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="acff0-212">Click **Add** button.</span></span> <span data-ttu-id="acff0-213">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="acff0-213">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="acff0-215">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="acff0-215">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="acff0-216">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="acff0-216">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="acff0-217">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="acff0-217">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="acff0-218">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="acff0-218">Testing single sign-on</span></span>

<span data-ttu-id="acff0-219">Ebben a szakaszban tesztelése az Azure AD SSO konfigurációs hello hozzáférési Panel használatával.</span><span class="sxs-lookup"><span data-stu-id="acff0-219">In this section, you test your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="acff0-220">Hello Háttérkép Online csempe a hozzáférési Panel hello kattintáskor automatikusan bejelentkezett tooyour Háttérkép Online alkalmazás kapja meg.</span><span class="sxs-lookup"><span data-stu-id="acff0-220">When you click hello BGS Online tile in hello Access Panel, you should get automatically signed-on tooyour BGS Online application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="acff0-221">További források</span><span class="sxs-lookup"><span data-stu-id="acff0-221">Additional resources</span></span>

* [<span data-ttu-id="acff0-222">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="acff0-222">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="acff0-223">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="acff0-223">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-bgsonline-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-bgsonline-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-bgsonline-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-bgsonline-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-bgsonline-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-bgsonline-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-bgsonline-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-bgsonline-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-bgsonline-tutorial/tutorial_general_203.png

