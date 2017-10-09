---
title: "Oktatóanyag: Azure Active Directoryval integrált NetDocuments |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és NetDocuments között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 1a47dc42-1a17-48a2-965e-eca4cfb2f197
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: ee9887553595a2492642aed4cb4abcd11d9cf599
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-netdocuments"></a><span data-ttu-id="4c9d8-103">Oktatóanyag: Azure Active Directoryval integrált NetDocuments</span><span class="sxs-lookup"><span data-stu-id="4c9d8-103">Tutorial: Azure Active Directory integration with NetDocuments</span></span>

<span data-ttu-id="4c9d8-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate NetDocuments az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="4c9d8-104">In this tutorial, you learn how toointegrate NetDocuments with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="4c9d8-105">NetDocuments integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="4c9d8-105">Integrating NetDocuments with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="4c9d8-106">Megadhatja a hozzáférés tooNetDocuments rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="4c9d8-106">You can control in Azure AD who has access tooNetDocuments</span></span>
- <span data-ttu-id="4c9d8-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooNetDocuments (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="4c9d8-107">You can enable your users tooautomatically get signed-on tooNetDocuments (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="4c9d8-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="4c9d8-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="4c9d8-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="4c9d8-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4c9d8-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="4c9d8-110">Prerequisites</span></span>

<span data-ttu-id="4c9d8-111">az Azure AD integrálása NetDocuments tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="4c9d8-111">tooconfigure Azure AD integration with NetDocuments, you need hello following items:</span></span>

- <span data-ttu-id="4c9d8-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="4c9d8-112">An Azure AD subscription</span></span>
- <span data-ttu-id="4c9d8-113">Egy NetDocuments egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="4c9d8-113">A NetDocuments single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="4c9d8-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="4c9d8-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="4c9d8-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="4c9d8-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="4c9d8-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="4c9d8-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="4c9d8-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="4c9d8-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="4c9d8-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="4c9d8-118">Scenario description</span></span>
<span data-ttu-id="4c9d8-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="4c9d8-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="4c9d8-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="4c9d8-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="4c9d8-121">Hello gyűjteményből NetDocuments hozzáadása</span><span class="sxs-lookup"><span data-stu-id="4c9d8-121">Adding NetDocuments from hello gallery</span></span>
2. <span data-ttu-id="4c9d8-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="4c9d8-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-netdocuments-from-hello-gallery"></a><span data-ttu-id="4c9d8-123">Hello gyűjteményből NetDocuments hozzáadása</span><span class="sxs-lookup"><span data-stu-id="4c9d8-123">Adding NetDocuments from hello gallery</span></span>
<span data-ttu-id="4c9d8-124">tooconfigure hello integrációja NetDocuments az Azure AD-be, meg kell tooadd NetDocuments hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="4c9d8-124">tooconfigure hello integration of NetDocuments into Azure AD, you need tooadd NetDocuments from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="4c9d8-125">**tooadd NetDocuments hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="4c9d8-125">**tooadd NetDocuments from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="4c9d8-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="4c9d8-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="4c9d8-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="4c9d8-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="4c9d8-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="4c9d8-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="4c9d8-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="4c9d8-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="4c9d8-133">Hello keresési mezőbe, írja be a **NetDocuments**.</span><span class="sxs-lookup"><span data-stu-id="4c9d8-133">In hello search box, type **NetDocuments**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-netdocuments-tutorial/tutorial_netdocuments_search.png)

5. <span data-ttu-id="4c9d8-135">A hello eredmények panelen válassza ki a **NetDocuments**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="4c9d8-135">In hello results panel, select **NetDocuments**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-netdocuments-tutorial/tutorial_netdocuments_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="4c9d8-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="4c9d8-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="4c9d8-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján NetDocuments.</span><span class="sxs-lookup"><span data-stu-id="4c9d8-138">In this section, you configure and test Azure AD single sign-on with NetDocuments based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="4c9d8-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó NetDocuments tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="4c9d8-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in NetDocuments is tooa user in Azure AD.</span></span> <span data-ttu-id="4c9d8-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello NetDocuments közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="4c9d8-140">In other words, a link relationship between an Azure AD user and hello related user in NetDocuments needs toobe established.</span></span>

<span data-ttu-id="4c9d8-141">NetDocuments, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="4c9d8-141">In NetDocuments, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="4c9d8-142">tooconfigure és az Azure AD az egyszeri bejelentkezés NetDocuments-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="4c9d8-142">tooconfigure and test Azure AD single sign-on with NetDocuments, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="4c9d8-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="4c9d8-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="4c9d8-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="4c9d8-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="4c9d8-145">**[NetDocuments tesztfelhasználó létrehozása](#creating-a-netdocuments-test-user)**  -toohave egy megfelelője a Britta Simon a NetDocuments, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="4c9d8-145">**[Creating a NetDocuments test user](#creating-a-netdocuments-test-user)** - toohave a counterpart of Britta Simon in NetDocuments that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="4c9d8-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="4c9d8-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="4c9d8-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="4c9d8-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="4c9d8-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="4c9d8-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="4c9d8-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az NetDocuments alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="4c9d8-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your NetDocuments application.</span></span>

<span data-ttu-id="4c9d8-150">**az Azure AD tooconfigure egyszeri bejelentkezést a NetDocuments, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="4c9d8-150">**tooconfigure Azure AD single sign-on with NetDocuments, perform hello following steps:**</span></span>

1. <span data-ttu-id="4c9d8-151">Az Azure portál, a hello hello **NetDocuments** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="4c9d8-151">In hello Azure portal, on hello **NetDocuments** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="4c9d8-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="4c9d8-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-netdocuments-tutorial/tutorial_netdocuments_samlbase.png)

3. <span data-ttu-id="4c9d8-155">A hello **NetDocuments tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="4c9d8-155">On hello **NetDocuments Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-netdocuments-tutorial/tutorial_netdocuments_url.png)

    <span data-ttu-id="4c9d8-157">a.</span><span class="sxs-lookup"><span data-stu-id="4c9d8-157">a.</span></span> <span data-ttu-id="4c9d8-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://vault.netvoyage.com/neWeb2/docCent.aspx?whr=<user identifier>`</span><span class="sxs-lookup"><span data-stu-id="4c9d8-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://vault.netvoyage.com/neWeb2/docCent.aspx?whr=<user identifier>`</span></span>

    <span data-ttu-id="4c9d8-159">b.</span><span class="sxs-lookup"><span data-stu-id="4c9d8-159">b.</span></span> <span data-ttu-id="4c9d8-160">A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://vault.netvoyage.com/neWeb2/docCent.aspx?whr=<user identifier>`</span><span class="sxs-lookup"><span data-stu-id="4c9d8-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://vault.netvoyage.com/neWeb2/docCent.aspx?whr=<user identifier>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="4c9d8-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="4c9d8-161">These values are not real.</span></span> <span data-ttu-id="4c9d8-162">Frissítheti ezeket az értékeket a hello tényleges bejelentkezési URL-cím és a válasz URL-CÍMEN.</span><span class="sxs-lookup"><span data-stu-id="4c9d8-162">Update these values with hello actual Sign-on URL and Reply URL.</span></span> <span data-ttu-id="4c9d8-163">Ügyfél [NetDocuments támogatási csoport](https://support.netdocuments.com/hc/) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="4c9d8-163">Contact [NetDocuments support team](https://support.netdocuments.com/hc/) tooget these values.</span></span>
 
4. <span data-ttu-id="4c9d8-164">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="4c9d8-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-netdocuments-tutorial/tutorial_netdocuments_certificate.png) 

5. <span data-ttu-id="4c9d8-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="4c9d8-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-netdocuments-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="4c9d8-168">Egy másik webes böngészőablakban jelentkezzen be a NetDocuments vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="4c9d8-168">In a different web browser window, log into your NetDocuments company site as an administrator.</span></span>

7. <span data-ttu-id="4c9d8-169">Nyissa meg túl**Admin**.</span><span class="sxs-lookup"><span data-stu-id="4c9d8-169">Go too**Admin**.</span></span>

8. <span data-ttu-id="4c9d8-170">Kattintson a **Hozzáadás és eltávolítás felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="4c9d8-170">Click **Add and remove users and groups**.</span></span>
   
    <span data-ttu-id="4c9d8-171">![Tárház](./media/active-directory-saas-netdocuments-tutorial/ic795047.png "tárház")</span><span class="sxs-lookup"><span data-stu-id="4c9d8-171">![Repository](./media/active-directory-saas-netdocuments-tutorial/ic795047.png "Repository")</span></span>

9. <span data-ttu-id="4c9d8-172">Kattintson a **speciális hitelesítési beállítások konfigurálása**.</span><span class="sxs-lookup"><span data-stu-id="4c9d8-172">Click **Configure advanced authentication options**.</span></span>
    
    <span data-ttu-id="4c9d8-173">![Speciális hitelesítési beállítások konfigurálása](./media/active-directory-saas-netdocuments-tutorial/ic795048.png "speciális hitelesítési beállítások konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="4c9d8-173">![Configure advanced authentication options](./media/active-directory-saas-netdocuments-tutorial/ic795048.png "Configure advanced authentication options")</span></span>

10. <span data-ttu-id="4c9d8-174">A hello **összevont identitási** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="4c9d8-174">On hello **Federated Identity** dialog, perform hello following steps:</span></span>
   
    <span data-ttu-id="4c9d8-175">![Összevont Identitty](./media/active-directory-saas-netdocuments-tutorial/ic795049.png "Identitty összevont")</span><span class="sxs-lookup"><span data-stu-id="4c9d8-175">![Federated Identitty](./media/active-directory-saas-netdocuments-tutorial/ic795049.png "Federated Identitty")</span></span>
   
    <span data-ttu-id="4c9d8-176">a.</span><span class="sxs-lookup"><span data-stu-id="4c9d8-176">a.</span></span> <span data-ttu-id="4c9d8-177">Mint **összevont identitás kiszolgálótípus**, jelölje be **Active Directory összevonási szolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="4c9d8-177">As **Federated identity server type**, select **Active Directory Federation Services**.</span></span>
   
    <span data-ttu-id="4c9d8-178">b.</span><span class="sxs-lookup"><span data-stu-id="4c9d8-178">b.</span></span> <span data-ttu-id="4c9d8-179">Kattintson a **fájl kiválasztása**, tooupload hello letöltötte az Azure-portálról letöltött metaadat-fájlt.</span><span class="sxs-lookup"><span data-stu-id="4c9d8-179">Click **Choose file**, tooupload hello downloaded metadata file which you have downloaded from Azure portal.</span></span>
   
    <span data-ttu-id="4c9d8-180">c.</span><span class="sxs-lookup"><span data-stu-id="4c9d8-180">c.</span></span> <span data-ttu-id="4c9d8-181">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="4c9d8-181">Click **OK**.</span></span>

> [!TIP]
> <span data-ttu-id="4c9d8-182">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="4c9d8-182">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="4c9d8-183">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="4c9d8-183">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="4c9d8-184">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="4c9d8-184">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="4c9d8-185">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="4c9d8-185">Creating an Azure AD test user</span></span>
<span data-ttu-id="4c9d8-186">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="4c9d8-186">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="4c9d8-188">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="4c9d8-188">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="4c9d8-189">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="4c9d8-189">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-netdocuments-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="4c9d8-191">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="4c9d8-191">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-netdocuments-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="4c9d8-193">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="4c9d8-193">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-netdocuments-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="4c9d8-195">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="4c9d8-195">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-netdocuments-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="4c9d8-197">a.</span><span class="sxs-lookup"><span data-stu-id="4c9d8-197">a.</span></span> <span data-ttu-id="4c9d8-198">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="4c9d8-198">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="4c9d8-199">b.</span><span class="sxs-lookup"><span data-stu-id="4c9d8-199">b.</span></span> <span data-ttu-id="4c9d8-200">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="4c9d8-200">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="4c9d8-201">c.</span><span class="sxs-lookup"><span data-stu-id="4c9d8-201">c.</span></span> <span data-ttu-id="4c9d8-202">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="4c9d8-202">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="4c9d8-203">d.</span><span class="sxs-lookup"><span data-stu-id="4c9d8-203">d.</span></span> <span data-ttu-id="4c9d8-204">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="4c9d8-204">Click **Create**.</span></span>
 
### <a name="creating-a-netdocuments-test-user"></a><span data-ttu-id="4c9d8-205">NetDocuments tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="4c9d8-205">Creating a NetDocuments test user</span></span>

<span data-ttu-id="4c9d8-206">az Azure AD tooenable felhasználók toolog a tooNetDocuments, akkor ki kell építenie NetDocuments be.</span><span class="sxs-lookup"><span data-stu-id="4c9d8-206">tooenable Azure AD users toolog in tooNetDocuments, they must be provisioned into NetDocuments.</span></span>  
<span data-ttu-id="4c9d8-207">NetDocuments hello esetben egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="4c9d8-207">In hello case of NetDocuments, provisioning is a manual task.</span></span>

<span data-ttu-id="4c9d8-208">**tooprovision egy felhasználói fiókot, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="4c9d8-208">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="4c9d8-209">A tooyour Sign **NetDocuments** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="4c9d8-209">Sing on tooyour **NetDocuments** company site as administrator.</span></span>

2. <span data-ttu-id="4c9d8-210">Hello hello felső menüben kattintson a **Admin**.</span><span class="sxs-lookup"><span data-stu-id="4c9d8-210">In hello menu on hello top, click **Admin**.</span></span>
   
    <span data-ttu-id="4c9d8-211">![Felügyeleti](./media/active-directory-saas-netdocuments-tutorial/ic795051.png "rendszergazda")</span><span class="sxs-lookup"><span data-stu-id="4c9d8-211">![Admin](./media/active-directory-saas-netdocuments-tutorial/ic795051.png "Admin")</span></span>

3. <span data-ttu-id="4c9d8-212">Kattintson a **Hozzáadás és eltávolítás felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="4c9d8-212">Click **Add and remove users and groups**.</span></span>
   
    <span data-ttu-id="4c9d8-213">![Tárház](./media/active-directory-saas-netdocuments-tutorial/ic795047.png "tárház")</span><span class="sxs-lookup"><span data-stu-id="4c9d8-213">![Repository](./media/active-directory-saas-netdocuments-tutorial/ic795047.png "Repository")</span></span>

4. <span data-ttu-id="4c9d8-214">A hello **E-mail cím** szövegmezőhöz típus hello e-mail címe érvényes Azure Active Directory-fiókot szeretné, hogy tooprovision, és kattintson a **felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="4c9d8-214">In hello **Email Address** textbox, type hello email address of a valid Azure Active Directory account you want tooprovision, and then click **Add User**.</span></span>
   
    <span data-ttu-id="4c9d8-215">![E-mail cím](./media/active-directory-saas-netdocuments-tutorial/ic795053.png "E-mail cím")</span><span class="sxs-lookup"><span data-stu-id="4c9d8-215">![Email Address](./media/active-directory-saas-netdocuments-tutorial/ic795053.png "Email Address")</span></span>
   
   >[!NOTE]
   ><span data-ttu-id="4c9d8-216">hello Azure Active Directory fióktulajdonos kap egy e-mailt, amely tartalmazza egy hivatkozás tooconfirm hello fiókot, mielőtt aktívvá válik.</span><span class="sxs-lookup"><span data-stu-id="4c9d8-216">hello Azure Active Directory account holder will get an email that includes a link tooconfirm hello account before it becomes active.</span></span> <span data-ttu-id="4c9d8-217">Bármely más NetDocuments felhasználói fiók létrehozása eszközök vagy NetDocuments tooprovision Azure Active Directory által nyújtott API-k felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="4c9d8-217">You can use any other NetDocuments user account creation tools or APIs provided by NetDocuments tooprovision Azure Active Directory user accounts.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="4c9d8-218">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="4c9d8-218">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="4c9d8-219">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooNetDocuments megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="4c9d8-219">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooNetDocuments.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="4c9d8-221">**tooassign Britta Simon tooNetDocuments, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="4c9d8-221">**tooassign Britta Simon tooNetDocuments, perform hello following steps:**</span></span>

1. <span data-ttu-id="4c9d8-222">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="4c9d8-222">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="4c9d8-224">Hello alkalmazások listában válassza ki a **NetDocuments**.</span><span class="sxs-lookup"><span data-stu-id="4c9d8-224">In hello applications list, select **NetDocuments**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-netdocuments-tutorial/tutorial_netdocuments_app.png) 

3. <span data-ttu-id="4c9d8-226">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="4c9d8-226">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="4c9d8-228">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="4c9d8-228">Click **Add** button.</span></span> <span data-ttu-id="4c9d8-229">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="4c9d8-229">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="4c9d8-231">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="4c9d8-231">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="4c9d8-232">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="4c9d8-232">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="4c9d8-233">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="4c9d8-233">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="4c9d8-234">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="4c9d8-234">Testing single sign-on</span></span>

<span data-ttu-id="4c9d8-235">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="4c9d8-235">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="4c9d8-236">Hello NetDocuments hello hozzáférési Panel csempére kattintva kapja meg automatikusan bejelentkezett tooyour NetDocuments alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="4c9d8-236">When you click hello NetDocuments tile in hello Access Panel, you should get automatically signed-on tooyour NetDocuments application.</span></span>
<span data-ttu-id="4c9d8-237">A hozzáférési Panel kapcsolatos további információkért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="4c9d8-237">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4c9d8-238">További források</span><span class="sxs-lookup"><span data-stu-id="4c9d8-238">Additional resources</span></span>

* [<span data-ttu-id="4c9d8-239">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="4c9d8-239">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="4c9d8-240">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="4c9d8-240">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-netdocuments-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-netdocuments-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-netdocuments-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-netdocuments-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-netdocuments-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-netdocuments-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-netdocuments-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-netdocuments-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-netdocuments-tutorial/tutorial_general_203.png

