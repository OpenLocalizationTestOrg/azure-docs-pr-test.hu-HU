---
title: "Oktatóanyag: Azure Active Directory-integráció halogén szoftverrel |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és halogén szoftverek között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 2ca2298d-9a0c-4f14-925c-fa23f2659d28
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: bdb67de713771d6e306f287c4b13895f6336f7ba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-halogen-software"></a><span data-ttu-id="53cd6-103">Oktatóanyag: Azure Active Directory-integráció halogén szoftverrel</span><span class="sxs-lookup"><span data-stu-id="53cd6-103">Tutorial: Azure Active Directory integration with Halogen Software</span></span>

<span data-ttu-id="53cd6-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate halogén szoftver az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="53cd6-104">In this tutorial, you learn how toointegrate Halogen Software with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="53cd6-105">Halogén szoftver integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="53cd6-105">Integrating Halogen Software with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="53cd6-106">Megadhatja a hozzáférés tooHalogen szoftver rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="53cd6-106">You can control in Azure AD who has access tooHalogen Software</span></span>
- <span data-ttu-id="53cd6-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooHalogen szoftver (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="53cd6-107">You can enable your users tooautomatically get signed-on tooHalogen Software (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="53cd6-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="53cd6-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="53cd6-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="53cd6-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="53cd6-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="53cd6-110">Prerequisites</span></span>

<span data-ttu-id="53cd6-111">tooconfigure halogén szoftver az Azure AD integrálása, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="53cd6-111">tooconfigure Azure AD integration with Halogen Software, you need hello following items:</span></span>

- <span data-ttu-id="53cd6-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="53cd6-112">An Azure AD subscription</span></span>
- <span data-ttu-id="53cd6-113">Egy halogén szoftver egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="53cd6-113">A Halogen Software single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="53cd6-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="53cd6-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="53cd6-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="53cd6-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="53cd6-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="53cd6-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="53cd6-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="53cd6-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="53cd6-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="53cd6-118">Scenario description</span></span>

<span data-ttu-id="53cd6-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="53cd6-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="53cd6-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="53cd6-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="53cd6-121">Hello gyűjteményből halogén szoftver hozzáadása</span><span class="sxs-lookup"><span data-stu-id="53cd6-121">Adding Halogen Software from hello gallery</span></span>
2. <span data-ttu-id="53cd6-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="53cd6-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-halogen-software-from-hello-gallery"></a><span data-ttu-id="53cd6-123">Hello gyűjteményből halogén szoftver hozzáadása</span><span class="sxs-lookup"><span data-stu-id="53cd6-123">Adding Halogen Software from hello gallery</span></span>

<span data-ttu-id="53cd6-124">tooconfigure hello integrációs halogén szoftver az Azure AD-be, meg kell tooadd halogén szoftver hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="53cd6-124">tooconfigure hello integration of Halogen Software into Azure AD, you need tooadd Halogen Software from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="53cd6-125">**tooadd halogén szoftver hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="53cd6-125">**tooadd Halogen Software from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="53cd6-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="53cd6-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="53cd6-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="53cd6-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="53cd6-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="53cd6-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="53cd6-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="53cd6-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="53cd6-133">Hello keresési mezőbe, írja be a **halogén szoftver**.</span><span class="sxs-lookup"><span data-stu-id="53cd6-133">In hello search box, type **Halogen Software**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-halogen-software-tutorial/tutorial_halogensoftware_search.png)

5. <span data-ttu-id="53cd6-135">A hello eredmények panelen válassza ki a **halogén szoftver**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="53cd6-135">In hello results panel, select **Halogen Software**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-halogen-software-tutorial/tutorial_halogensoftware_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="53cd6-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="53cd6-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="53cd6-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján halogén szoftverrel.</span><span class="sxs-lookup"><span data-stu-id="53cd6-138">In this section, you configure and test Azure AD single sign-on with Halogen Software based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="53cd6-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói halogén szoftverfrissítési tooa felhasználói az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="53cd6-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Halogen Software is tooa user in Azure AD.</span></span> <span data-ttu-id="53cd6-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználói hello halogén szoftver közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="53cd6-140">In other words, a link relationship between an Azure AD user and hello related user in Halogen Software needs toobe established.</span></span>

<span data-ttu-id="53cd6-141">Halogén szoftver, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="53cd6-141">In Halogen Software, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="53cd6-142">tooconfigure és az Azure AD az egyszeri bejelentkezés tesztelése halogén szoftverrel, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="53cd6-142">tooconfigure and test Azure AD single sign-on with Halogen Software, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="53cd6-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="53cd6-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="53cd6-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="53cd6-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="53cd6-145">**[Halogén szoftver tesztfelhasználó létrehozása](#creating-a-halogen-software-test-user)**  -toohave egy megfelelője a Britta Simon halogén szoftver, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="53cd6-145">**[Creating a Halogen Software test user](#creating-a-halogen-software-test-user)** - toohave a counterpart of Britta Simon in Halogen Software that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="53cd6-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="53cd6-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="53cd6-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="53cd6-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="53cd6-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="53cd6-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="53cd6-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az halogén szoftver alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="53cd6-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Halogen Software application.</span></span>

<span data-ttu-id="53cd6-150">**az Azure AD tooconfigure egyszeri bejelentkezés halogén szoftvert, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="53cd6-150">**tooconfigure Azure AD single sign-on with Halogen Software, perform hello following steps:**</span></span>

1. <span data-ttu-id="53cd6-151">Az Azure portál, a hello hello **halogén szoftver** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="53cd6-151">In hello Azure portal, on hello **Halogen Software** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="53cd6-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="53cd6-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-halogen-software-tutorial/tutorial_halogensoftware_samlbase.png)

3. <span data-ttu-id="53cd6-155">A hello **halogén szoftver tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="53cd6-155">On hello **Halogen Software Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-halogen-software-tutorial/tutorial_halogensoftware_url.png)

    <span data-ttu-id="53cd6-157">a.</span><span class="sxs-lookup"><span data-stu-id="53cd6-157">a.</span></span> <span data-ttu-id="53cd6-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://global.hgncloud.com/<companyname>`</span><span class="sxs-lookup"><span data-stu-id="53cd6-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://global.hgncloud.com/<companyname>`</span></span>

    <span data-ttu-id="53cd6-159">b.</span><span class="sxs-lookup"><span data-stu-id="53cd6-159">b.</span></span> <span data-ttu-id="53cd6-160">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be: `https://global.halogensoftware.com/<companyname>`,`https://global.hgncloud.com/<companyname>`</span><span class="sxs-lookup"><span data-stu-id="53cd6-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://global.halogensoftware.com/<companyname>`, `https://global.hgncloud.com/<companyname>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="53cd6-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="53cd6-161">These values are not real.</span></span> <span data-ttu-id="53cd6-162">Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="53cd6-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="53cd6-163">Ügyfél [halogén szoftver ügyfél-támogatási csoport](https://support.halogensoftware.com/) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="53cd6-163">Contact [Halogen Software Client support team](https://support.halogensoftware.com/) tooget these values.</span></span> 
 


4. <span data-ttu-id="53cd6-164">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="53cd6-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-halogen-software-tutorial/tutorial_halogensoftware_certificate.png) 

5. <span data-ttu-id="53cd6-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="53cd6-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-halogen-software-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="53cd6-168">Egy másik böngészőablakban, bejelentkezés tooyour **halogén szoftver** alkalmazást rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="53cd6-168">In a different browser window, sign-on tooyour **Halogen Software** application as an administrator.</span></span>

7. <span data-ttu-id="53cd6-169">Kattintson a hello **beállítások** fülre.</span><span class="sxs-lookup"><span data-stu-id="53cd6-169">Click hello **Options** tab.</span></span> 
   
    ![Mi az az Azure AD Connect?][12]

8. <span data-ttu-id="53cd6-171">Hello bal oldali navigációs ablaktábláján kattintson **SAML-alapú konfigurációs**.</span><span class="sxs-lookup"><span data-stu-id="53cd6-171">In hello left navigation pane, click **SAML Configuration**.</span></span> 
   
    ![Mi az az Azure AD Connect?][13]

9. <span data-ttu-id="53cd6-173">A hello **SAML-alapú konfigurációs** lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="53cd6-173">On hello **SAML Configuration** page, perform hello following steps:</span></span> 

    ![Mi az az Azure AD Connect?][14]

     <span data-ttu-id="53cd6-175">a.</span><span class="sxs-lookup"><span data-stu-id="53cd6-175">a.</span></span> <span data-ttu-id="53cd6-176">Mint **egyedi azonosítója**, jelölje be **NameID**.</span><span class="sxs-lookup"><span data-stu-id="53cd6-176">As **Unique Identifier**, select **NameID**.</span></span>

     <span data-ttu-id="53cd6-177">b.</span><span class="sxs-lookup"><span data-stu-id="53cd6-177">b.</span></span> <span data-ttu-id="53cd6-178">Mint **egyedi azonosítót a Maps a való**, jelölje be **felhasználónév**.</span><span class="sxs-lookup"><span data-stu-id="53cd6-178">As **Unique Identifier Maps To**, select **Username**.</span></span>
  
     <span data-ttu-id="53cd6-179">c.</span><span class="sxs-lookup"><span data-stu-id="53cd6-179">c.</span></span> <span data-ttu-id="53cd6-180">tooupload a letöltött metaadatait tartalmazó fájl kattintson **Tallózás** tooselect hello fájlt, majd **fájl feltöltése**.</span><span class="sxs-lookup"><span data-stu-id="53cd6-180">tooupload your downloaded metadata file, click **Browse** tooselect hello file, and then **Upload File**.</span></span>
 
     <span data-ttu-id="53cd6-181">d.</span><span class="sxs-lookup"><span data-stu-id="53cd6-181">d.</span></span> <span data-ttu-id="53cd6-182">tootest hello konfigurációs, kattintson a **teszt futtatása**.</span><span class="sxs-lookup"><span data-stu-id="53cd6-182">tootest hello configuration, click **Run Test**.</span></span> 
    
    >[!NOTE]
    ><span data-ttu-id="53cd6-183">Szüksége toowait üdvözlőüzenetére "*hello SAML teszt befejeződött. Zárja be ezt az ablakot*".</span><span class="sxs-lookup"><span data-stu-id="53cd6-183">You need toowait for hello message "*hello SAML test is complete. Please close this window*".</span></span> <span data-ttu-id="53cd6-184">Zárja be, majd hello megnyitott böngészőablakot.</span><span class="sxs-lookup"><span data-stu-id="53cd6-184">Then, close hello opened browser window.</span></span> <span data-ttu-id="53cd6-185">Hello **SAML engedélyezése** jelölőnégyzet csak akkor engedélyezett, ha hello teszt befejezése után.</span><span class="sxs-lookup"><span data-stu-id="53cd6-185">hello **Enable SAML** checkbox is only enabled if hello test has been completed.</span></span> 
     
     <span data-ttu-id="53cd6-186">e.</span><span class="sxs-lookup"><span data-stu-id="53cd6-186">e.</span></span> <span data-ttu-id="53cd6-187">Válassza ki **SAML engedélyezése**.</span><span class="sxs-lookup"><span data-stu-id="53cd6-187">Select **Enable SAML**.</span></span>
    
     <span data-ttu-id="53cd6-188">f.</span><span class="sxs-lookup"><span data-stu-id="53cd6-188">f.</span></span> <span data-ttu-id="53cd6-189">Kattintson a **módosítások mentése**.</span><span class="sxs-lookup"><span data-stu-id="53cd6-189">Click **Save Changes**.</span></span> 

> [!TIP]
> <span data-ttu-id="53cd6-190">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="53cd6-190">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="53cd6-191">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="53cd6-191">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="53cd6-192">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="53cd6-192">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="53cd6-193">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="53cd6-193">Creating an Azure AD test user</span></span>

<span data-ttu-id="53cd6-194">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="53cd6-194">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="53cd6-196">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="53cd6-196">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="53cd6-197">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="53cd6-197">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-halogen-software-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="53cd6-199">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="53cd6-199">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-halogen-software-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="53cd6-201">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="53cd6-201">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-halogen-software-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="53cd6-203">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="53cd6-203">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-halogen-software-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="53cd6-205">a.</span><span class="sxs-lookup"><span data-stu-id="53cd6-205">a.</span></span> <span data-ttu-id="53cd6-206">A hello **neve** szövegmezőhöz típus neve megegyezik **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="53cd6-206">In hello **Name** textbox, type name as **BrittaSimon**.</span></span>

    <span data-ttu-id="53cd6-207">b.</span><span class="sxs-lookup"><span data-stu-id="53cd6-207">b.</span></span> <span data-ttu-id="53cd6-208">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="53cd6-208">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="53cd6-209">c.</span><span class="sxs-lookup"><span data-stu-id="53cd6-209">c.</span></span> <span data-ttu-id="53cd6-210">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="53cd6-210">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="53cd6-211">d.</span><span class="sxs-lookup"><span data-stu-id="53cd6-211">d.</span></span> <span data-ttu-id="53cd6-212">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="53cd6-212">Click **Create**.</span></span>
 
### <a name="creating-a-halogen-software-test-user"></a><span data-ttu-id="53cd6-213">Halogén szoftver tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="53cd6-213">Creating a Halogen Software test user</span></span>

<span data-ttu-id="53cd6-214">hello ebben a szakaszban célja toocreate halogén szoftver Britta Simon nevű felhasználó.</span><span class="sxs-lookup"><span data-stu-id="53cd6-214">hello objective of this section is toocreate a user called Britta Simon in Halogen Software.</span></span>

<span data-ttu-id="53cd6-215">**toocreate Britta Simon meghívta halogén szoftver, a felhasználó hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="53cd6-215">**toocreate a user called Britta Simon in Halogen Software, perform hello following steps:**</span></span>

1. <span data-ttu-id="53cd6-216">Jelentkezzen be tooyour **halogén szoftver** alkalmazást rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="53cd6-216">Sign on tooyour **Halogen Software** application as an administrator.</span></span>

2. <span data-ttu-id="53cd6-217">Kattintson a hello **felhasználói Center** fülre, majd **felhasználó létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="53cd6-217">Click hello **User Center** tab, and then click **Create User**.</span></span>
   
    ![Mi az az Azure AD Connect?][300]  

3. <span data-ttu-id="53cd6-219">A hello **új felhasználó** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="53cd6-219">On hello **New User** dialog page, perform hello following steps:</span></span>
   
    ![Mi az az Azure AD Connect?][301]

    <span data-ttu-id="53cd6-221">a.</span><span class="sxs-lookup"><span data-stu-id="53cd6-221">a.</span></span> <span data-ttu-id="53cd6-222">A hello **Utónév** szövegmezőhöz első típusnév hello felhasználó például **Britta**.</span><span class="sxs-lookup"><span data-stu-id="53cd6-222">In hello **First Name** textbox, type first name of hello user like **Britta**.</span></span>
    
    <span data-ttu-id="53cd6-223">b.</span><span class="sxs-lookup"><span data-stu-id="53cd6-223">b.</span></span> <span data-ttu-id="53cd6-224">A hello **Vezetéknév** szövegmezőhöz utolsó típusnév hello felhasználó például **Simon**.</span><span class="sxs-lookup"><span data-stu-id="53cd6-224">In hello **Last Name** textbox, type last name of hello user like **Simon**.</span></span> 

    <span data-ttu-id="53cd6-225">c.</span><span class="sxs-lookup"><span data-stu-id="53cd6-225">c.</span></span> <span data-ttu-id="53cd6-226">A hello **felhasználónév** szövegmezőhöz típus **Britta Simon**, hello felhasználónév hasonlóan hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="53cd6-226">In hello **Username** textbox, type **Britta Simon**, hello user name as in hello Azure portal.</span></span>

    <span data-ttu-id="53cd6-227">d.</span><span class="sxs-lookup"><span data-stu-id="53cd6-227">d.</span></span> <span data-ttu-id="53cd6-228">A hello **jelszó** szövegmezőhöz Britta jelszavát adja meg.</span><span class="sxs-lookup"><span data-stu-id="53cd6-228">In hello **Password** textbox, type a password for Britta.</span></span>
    
    <span data-ttu-id="53cd6-229">e.</span><span class="sxs-lookup"><span data-stu-id="53cd6-229">e.</span></span> <span data-ttu-id="53cd6-230">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="53cd6-230">Click **Save**.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="53cd6-231">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="53cd6-231">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="53cd6-232">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooHalogen szoftver megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="53cd6-232">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooHalogen Software.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="53cd6-234">**tooassign Britta Simon tooHalogen szoftver, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="53cd6-234">**tooassign Britta Simon tooHalogen Software, perform hello following steps:**</span></span>

1. <span data-ttu-id="53cd6-235">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="53cd6-235">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="53cd6-237">Hello alkalmazások listában válassza ki a **halogén szoftver**.</span><span class="sxs-lookup"><span data-stu-id="53cd6-237">In hello applications list, select **Halogen Software**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-halogen-software-tutorial/tutorial_halogensoftware_app.png) 

3. <span data-ttu-id="53cd6-239">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="53cd6-239">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="53cd6-241">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="53cd6-241">Click **Add** button.</span></span> <span data-ttu-id="53cd6-242">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="53cd6-242">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="53cd6-244">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="53cd6-244">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="53cd6-245">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="53cd6-245">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="53cd6-246">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="53cd6-246">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="53cd6-247">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="53cd6-247">Testing single sign-on</span></span>

<span data-ttu-id="53cd6-248">hello ebben a szakaszban célja tootest hozzáférési Panel az Azure AD SSO konfigurációs használatával hello.</span><span class="sxs-lookup"><span data-stu-id="53cd6-248">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="53cd6-249">Ha a hozzáférési Panel hello hello halogén szoftver csempe gombra kattint, automatikusan bejelentkezett tooyour halogén alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="53cd6-249">When you click hello Halogen Software tile in hello Access Panel, you should get automatically signed-on tooyour Halogen Software application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="53cd6-250">További források</span><span class="sxs-lookup"><span data-stu-id="53cd6-250">Additional resources</span></span>

* [<span data-ttu-id="53cd6-251">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="53cd6-251">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="53cd6-252">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="53cd6-252">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_general_04.png

[12]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_12.png

[13]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_13.png

[14]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_14.png

[100]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_general_203.png

[300]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_300.png

[301]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_301.png
