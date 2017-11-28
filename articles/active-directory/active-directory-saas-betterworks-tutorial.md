---
title: "Oktatóanyag: Azure Active Directoryval integrált BetterWorks |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és BetterWorks között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 5bb9505a-be02-46ae-9979-5308715d2b47
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: 9803593124318ea82e5a8888cc5a95b5da84472e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-betterworks"></a><span data-ttu-id="48baa-103">Oktatóanyag: Azure Active Directoryval integrált BetterWorks</span><span class="sxs-lookup"><span data-stu-id="48baa-103">Tutorial: Azure Active Directory integration with BetterWorks</span></span>

<span data-ttu-id="48baa-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate BetterWorks az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="48baa-104">In this tutorial, you learn how toointegrate BetterWorks with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="48baa-105">BetterWorks integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="48baa-105">Integrating BetterWorks with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="48baa-106">Megadhatja a hozzáférés tooBetterWorks rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="48baa-106">You can control in Azure AD who has access tooBetterWorks</span></span>
- <span data-ttu-id="48baa-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooBetterWorks (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="48baa-107">You can enable your users tooautomatically get signed-on tooBetterWorks (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="48baa-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="48baa-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="48baa-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="48baa-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="48baa-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="48baa-110">Prerequisites</span></span>

<span data-ttu-id="48baa-111">az Azure AD integrálása BetterWorks tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="48baa-111">tooconfigure Azure AD integration with BetterWorks, you need hello following items:</span></span>

- <span data-ttu-id="48baa-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="48baa-112">An Azure AD subscription</span></span>
- <span data-ttu-id="48baa-113">Egy BetterWorks egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="48baa-113">A BetterWorks single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="48baa-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="48baa-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="48baa-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="48baa-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="48baa-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="48baa-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="48baa-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="48baa-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="48baa-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="48baa-118">Scenario description</span></span>
<span data-ttu-id="48baa-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="48baa-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="48baa-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="48baa-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="48baa-121">Hello gyűjteményből BetterWorks hozzáadása</span><span class="sxs-lookup"><span data-stu-id="48baa-121">Adding BetterWorks from hello gallery</span></span>
2. <span data-ttu-id="48baa-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="48baa-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-betterworks-from-hello-gallery"></a><span data-ttu-id="48baa-123">Hello gyűjteményből BetterWorks hozzáadása</span><span class="sxs-lookup"><span data-stu-id="48baa-123">Adding BetterWorks from hello gallery</span></span>
<span data-ttu-id="48baa-124">tooconfigure hello integrációja BetterWorks az Azure AD-be, meg kell tooadd BetterWorks hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="48baa-124">tooconfigure hello integration of BetterWorks into Azure AD, you need tooadd BetterWorks from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="48baa-125">**tooadd BetterWorks hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="48baa-125">**tooadd BetterWorks from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="48baa-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="48baa-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="48baa-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="48baa-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="48baa-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="48baa-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="48baa-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="48baa-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="48baa-133">Hello keresési mezőbe, írja be a **BetterWorks**.</span><span class="sxs-lookup"><span data-stu-id="48baa-133">In hello search box, type **BetterWorks**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_search.png)

5. <span data-ttu-id="48baa-135">A hello eredmények panelen válassza ki a **BetterWorks**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="48baa-135">In hello results panel, select **BetterWorks**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="48baa-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="48baa-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="48baa-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján BetterWorks</span><span class="sxs-lookup"><span data-stu-id="48baa-138">In this section, you configure and test Azure AD single sign-on with BetterWorks based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="48baa-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó BetterWorks tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="48baa-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in BetterWorks is tooa user in Azure AD.</span></span> <span data-ttu-id="48baa-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello BetterWorks közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="48baa-140">In other words, a link relationship between an Azure AD user and hello related user in BetterWorks needs toobe established.</span></span>

<span data-ttu-id="48baa-141">BetterWorks, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="48baa-141">In BetterWorks, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="48baa-142">tooconfigure és az Azure AD az egyszeri bejelentkezés BetterWorks-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="48baa-142">tooconfigure and test Azure AD single sign-on with BetterWorks, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="48baa-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="48baa-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="48baa-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="48baa-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="48baa-145">**[BetterWorks tesztfelhasználó létrehozása](#creating-a-betterworks-test-user)**  -toohave egy megfelelője a Britta Simon a BetterWorks, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="48baa-145">**[Creating a BetterWorks test user](#creating-a-betterworks-test-user)** - toohave a counterpart of Britta Simon in BetterWorks that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="48baa-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="48baa-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="48baa-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="48baa-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="48baa-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="48baa-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="48baa-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az BetterWorks alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="48baa-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your BetterWorks application.</span></span>

<span data-ttu-id="48baa-150">**az Azure AD tooconfigure egyszeri bejelentkezést a BetterWorks, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="48baa-150">**tooconfigure Azure AD single sign-on with BetterWorks, perform hello following steps:**</span></span>

1. <span data-ttu-id="48baa-151">Az Azure portál, a hello hello **BetterWorks** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="48baa-151">In hello Azure portal, on hello **BetterWorks** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="48baa-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="48baa-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_samlbase.png)

3. <span data-ttu-id="48baa-155">A hello **BetterWorks tartomány és az URL-címek** szakaszban, ha tooconfigure hello alkalmazás **IDP kezdeményezett mód**:</span><span class="sxs-lookup"><span data-stu-id="48baa-155">On hello **BetterWorks Domain and URLs** section, If you wish tooconfigure hello application in **IDP initiated mode**:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_url.png)

    <span data-ttu-id="48baa-157">a.</span><span class="sxs-lookup"><span data-stu-id="48baa-157">a.</span></span> <span data-ttu-id="48baa-158">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://app.betterworks.com/saml2/metadata/`</span><span class="sxs-lookup"><span data-stu-id="48baa-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://app.betterworks.com/saml2/metadata/`</span></span>

    <span data-ttu-id="48baa-159">b.</span><span class="sxs-lookup"><span data-stu-id="48baa-159">b.</span></span> <span data-ttu-id="48baa-160">A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://app.betterworks.com/saml2/acs/`</span><span class="sxs-lookup"><span data-stu-id="48baa-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://app.betterworks.com/saml2/acs/`</span></span>

4. <span data-ttu-id="48baa-161">A hello **BetterWorks tartomány és az URL-címek** szakaszban, ha tooconfigure hello alkalmazás **Szolgáltató kezdeményezett mód**, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="48baa-161">On hello **BetterWorks Domain and URLs** section, If you wish tooconfigure hello application in **SP initiated mode**, perform hello following steps:</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_url1.png)

    <span data-ttu-id="48baa-163">a.</span><span class="sxs-lookup"><span data-stu-id="48baa-163">a.</span></span> <span data-ttu-id="48baa-164">Kattintson a hello **megjelenítése speciális URL-beállításainak**.</span><span class="sxs-lookup"><span data-stu-id="48baa-164">Click on hello **Show advanced URL settings**.</span></span>

    <span data-ttu-id="48baa-165">b.</span><span class="sxs-lookup"><span data-stu-id="48baa-165">b.</span></span> <span data-ttu-id="48baa-166">A hello **URL-cím bejelentkezési** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://app.betterworks.com`</span><span class="sxs-lookup"><span data-stu-id="48baa-166">In hello **Sign On URL** textbox, type a URL using hello following pattern: `https://app.betterworks.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="48baa-167">Ezek a valódi értékek nem.</span><span class="sxs-lookup"><span data-stu-id="48baa-167">These are not real values.</span></span> <span data-ttu-id="48baa-168">Ezek az értékek frissítése hello válasz URL-CÍMEN, azonosítóját és a tényleges bejelentkezési URL-cím.</span><span class="sxs-lookup"><span data-stu-id="48baa-168">Update these values with hello Reply URL, Identifier and actual Sign On URL.</span></span> <span data-ttu-id="48baa-169">Ügyfél [BetterWorks támogatási csoport](mailto:support@betterworks.com) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="48baa-169">Contact [BetterWorks support team](mailto:support@betterworks.com) tooget these values.</span></span>
 
4. <span data-ttu-id="48baa-170">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="48baa-170">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_certificate.png)  

5. <span data-ttu-id="48baa-172">BetterWorks alkalmazás hello SAML helyességi feltételek vár egy meghatározott formátumban.</span><span class="sxs-lookup"><span data-stu-id="48baa-172">BetterWorks application expects hello SAML assertions in a specific format.</span></span> <span data-ttu-id="48baa-173">Az alkalmazás jogcímek a következő hello konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="48baa-173">Configure hello following claims for this application.</span></span> <span data-ttu-id="48baa-174">Ezek az attribútumok értékének hello hello kezelése "**attribútum**" hello alkalmazás lapján.</span><span class="sxs-lookup"><span data-stu-id="48baa-174">You can manage hello values of these attributes from hello "**Attribute**" tab of hello application.</span></span> <span data-ttu-id="48baa-175">a következő képernyőkép hello ezen mutat egy példát.</span><span class="sxs-lookup"><span data-stu-id="48baa-175">hello following screenshot shows an example for this.</span></span> 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_attribute.png)

6. <span data-ttu-id="48baa-177">A hello **SAML-jogkivonat attribútumok** párbeszédpanel, az alábbi, hello táblázatban szereplő minden egyes sorára hajtsa végre a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="48baa-177">On hello **SAML token attributes** dialog, for each row shown in hello table below, perform hello following steps:</span></span>
 
   | <span data-ttu-id="48baa-178">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="48baa-178">Attribute Name</span></span> | <span data-ttu-id="48baa-179">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="48baa-179">Attribute Value</span></span> |
   | -------------- |  ------------ |
   | <span data-ttu-id="48baa-180">saml_token</span><span class="sxs-lookup"><span data-stu-id="48baa-180">saml_token</span></span>     | <span data-ttu-id="48baa-181">bd189cf6-1701-11e6-8f90-d26992eca2a5</span><span class="sxs-lookup"><span data-stu-id="48baa-181">bd189cf6-1701-11e6-8f90-d26992eca2a5</span></span> |

   <span data-ttu-id="48baa-182">a.</span><span class="sxs-lookup"><span data-stu-id="48baa-182">a.</span></span> <span data-ttu-id="48baa-183">Kattintson a **Hozzáadás attribútum** tooopen hello **attribútum hozzáadása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="48baa-183">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-betterworks-tutorial/tutorial_officespace_04.png)

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-betterworks-tutorial/tutorial_officespace_05.png)

   <span data-ttu-id="48baa-186">b.</span><span class="sxs-lookup"><span data-stu-id="48baa-186">b.</span></span> <span data-ttu-id="48baa-187">A hello **neve** szövegmezőben, az adott sorhoz feltüntetett hello attribútum neve.</span><span class="sxs-lookup"><span data-stu-id="48baa-187">In hello **Name** textbox, type hello attribute name shown for that row.</span></span> 

   <span data-ttu-id="48baa-188">c.</span><span class="sxs-lookup"><span data-stu-id="48baa-188">c.</span></span> <span data-ttu-id="48baa-189">A hello **érték** listájában, hello attribútuma Típusérték az adott sorhoz feltüntetett.</span><span class="sxs-lookup"><span data-stu-id="48baa-189">From hello **Value** list, type hello attribute value shown for that row.</span></span>
    
   <span data-ttu-id="48baa-190">d.</span><span class="sxs-lookup"><span data-stu-id="48baa-190">d.</span></span> <span data-ttu-id="48baa-191">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="48baa-191">Click **Ok**.</span></span>

7. <span data-ttu-id="48baa-192">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="48baa-192">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-betterworks-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="48baa-194">tooconfigure egyszeri bejelentkezést a **BetterWorks** oldalon kell letöltött toosend hello **metaadatainak XML-kódja** túl[BetterWorks támogatási csoport](mailto:support@betterworks.com).</span><span class="sxs-lookup"><span data-stu-id="48baa-194">tooconfigure single sign-on on **BetterWorks** side, you need toosend hello downloaded **Metadata XML** too[BetterWorks support team](mailto:support@betterworks.com).</span></span>


> [!TIP]
> <span data-ttu-id="48baa-195">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="48baa-195">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="48baa-196">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="48baa-196">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="48baa-197">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="48baa-197">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="48baa-198">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="48baa-198">Creating an Azure AD test user</span></span>
<span data-ttu-id="48baa-199">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="48baa-199">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="48baa-201">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="48baa-201">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="48baa-202">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="48baa-202">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-betterworks-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="48baa-204">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="48baa-204">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-betterworks-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="48baa-206">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="48baa-206">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-betterworks-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="48baa-208">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="48baa-208">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-betterworks-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="48baa-210">a.</span><span class="sxs-lookup"><span data-stu-id="48baa-210">a.</span></span> <span data-ttu-id="48baa-211">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="48baa-211">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="48baa-212">b.</span><span class="sxs-lookup"><span data-stu-id="48baa-212">b.</span></span> <span data-ttu-id="48baa-213">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="48baa-213">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="48baa-214">c.</span><span class="sxs-lookup"><span data-stu-id="48baa-214">c.</span></span> <span data-ttu-id="48baa-215">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="48baa-215">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="48baa-216">d.</span><span class="sxs-lookup"><span data-stu-id="48baa-216">d.</span></span> <span data-ttu-id="48baa-217">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="48baa-217">Click **Create**.</span></span>
 
### <a name="creating-a-betterworks-test-user"></a><span data-ttu-id="48baa-218">BetterWorks tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="48baa-218">Creating a BetterWorks test user</span></span>

<span data-ttu-id="48baa-219">Ebben a szakaszban egy BetterWorks Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="48baa-219">In this section, you create a user called Britta Simon in BetterWorks.</span></span> <span data-ttu-id="48baa-220">Együttműködve [BetterWorks támogatási csoport](mailto:support@betterworks.com) tooadd hello felhasználók hello BetterWorks platform.</span><span class="sxs-lookup"><span data-stu-id="48baa-220">Work with [BetterWorks support team](mailto:support@betterworks.com) tooadd hello users in hello BetterWorks platform.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="48baa-221">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="48baa-221">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="48baa-222">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooBetterWorks megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="48baa-222">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooBetterWorks.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="48baa-224">**tooassign Britta Simon tooBetterWorks, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="48baa-224">**tooassign Britta Simon tooBetterWorks, perform hello following steps:**</span></span>

1. <span data-ttu-id="48baa-225">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="48baa-225">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="48baa-227">Hello alkalmazások listában válassza ki a **BetterWorks**.</span><span class="sxs-lookup"><span data-stu-id="48baa-227">In hello applications list, select **BetterWorks**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_app.png) 

3. <span data-ttu-id="48baa-229">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="48baa-229">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="48baa-231">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="48baa-231">Click **Add** button.</span></span> <span data-ttu-id="48baa-232">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="48baa-232">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="48baa-234">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="48baa-234">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="48baa-235">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="48baa-235">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="48baa-236">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="48baa-236">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="48baa-237">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="48baa-237">Testing single sign-on</span></span>

<span data-ttu-id="48baa-238">hello ebben a szakaszban célja tootest az egyszeri bejelentkezés konfigurációs használatával hello a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="48baa-238">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="48baa-239">Hello BetterWorks hello hozzáférési Panel csempére kattintva kapja meg automatikusan bejelentkezett tooyour BetterWorks alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="48baa-239">When you click hello BetterWorks tile in hello Access Panel, you should get automatically signed-on tooyour BetterWorks application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="48baa-240">További források</span><span class="sxs-lookup"><span data-stu-id="48baa-240">Additional resources</span></span>

* [<span data-ttu-id="48baa-241">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="48baa-241">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="48baa-242">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="48baa-242">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_203.png

