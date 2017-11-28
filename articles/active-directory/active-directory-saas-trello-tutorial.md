---
title: "Oktatóanyag: Azure Active Directoryval integrált Trello |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a Trello között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: cd5ae365-9ed6-43a6-920b-f7814b993949
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: jeedes
ms.openlocfilehash: de2f2ba6a0e5545983c351f26f99d14f436618c8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-trello"></a><span data-ttu-id="fa2af-103">Oktatóanyag: Azure Active Directoryval integrált Trello</span><span class="sxs-lookup"><span data-stu-id="fa2af-103">Tutorial: Azure Active Directory integration with Trello</span></span>

<span data-ttu-id="fa2af-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Trello az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="fa2af-104">In this tutorial, you learn how toointegrate Trello with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="fa2af-105">Trello integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="fa2af-105">Integrating Trello with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="fa2af-106">Megadhatja a hozzáférés tooTrello rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="fa2af-106">You can control in Azure AD who has access tooTrello</span></span>
- <span data-ttu-id="fa2af-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooTrello (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="fa2af-107">You can enable your users tooautomatically get signed-on tooTrello (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="fa2af-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="fa2af-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="fa2af-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="fa2af-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fa2af-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="fa2af-110">Prerequisites</span></span>

<span data-ttu-id="fa2af-111">az Azure AD integrálása Trello tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="fa2af-111">tooconfigure Azure AD integration with Trello, you need hello following items:</span></span>

- <span data-ttu-id="fa2af-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="fa2af-112">An Azure AD subscription</span></span>
- <span data-ttu-id="fa2af-113">A Trello egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="fa2af-113">A Trello single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="fa2af-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="fa2af-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="fa2af-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="fa2af-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="fa2af-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="fa2af-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="fa2af-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="fa2af-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="fa2af-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="fa2af-118">Scenario description</span></span>
<span data-ttu-id="fa2af-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="fa2af-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="fa2af-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="fa2af-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="fa2af-121">Hello gyűjteményből Trello hozzáadása</span><span class="sxs-lookup"><span data-stu-id="fa2af-121">Adding Trello from hello gallery</span></span>
2. <span data-ttu-id="fa2af-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="fa2af-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-trello-from-hello-gallery"></a><span data-ttu-id="fa2af-123">Hello gyűjteményből Trello hozzáadása</span><span class="sxs-lookup"><span data-stu-id="fa2af-123">Adding Trello from hello gallery</span></span>
<span data-ttu-id="fa2af-124">tooconfigure hello integrációja Trello az Azure AD-be, meg kell tooadd Trello hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="fa2af-124">tooconfigure hello integration of Trello into Azure AD, you need tooadd Trello from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="fa2af-125">**tooadd Trello hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="fa2af-125">**tooadd Trello from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="fa2af-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="fa2af-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="fa2af-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="fa2af-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="fa2af-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="fa2af-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="fa2af-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="fa2af-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="fa2af-133">Hello keresési mezőbe, írja be a **Trello**.</span><span class="sxs-lookup"><span data-stu-id="fa2af-133">In hello search box, type **Trello**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-trello-tutorial/tutorial_trello_search.png)

5. <span data-ttu-id="fa2af-135">A hello eredmények panelen válassza ki a **Trello**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="fa2af-135">In hello results panel, select **Trello**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-trello-tutorial/tutorial_trello_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="fa2af-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="fa2af-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="fa2af-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Trello.</span><span class="sxs-lookup"><span data-stu-id="fa2af-138">In this section, you configure and test Azure AD single sign-on with Trello based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="fa2af-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói a Trello tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="fa2af-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Trello is tooa user in Azure AD.</span></span> <span data-ttu-id="fa2af-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Trello közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="fa2af-140">In other words, a link relationship between an Azure AD user and hello related user in Trello needs toobe established.</span></span>

<span data-ttu-id="fa2af-141">A Trello, rendelje a hello hello értékét **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="fa2af-141">In Trello, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="fa2af-142">tooconfigure és az Azure AD az egyszeri bejelentkezés Trello-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="fa2af-142">tooconfigure and test Azure AD single sign-on with Trello, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="fa2af-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="fa2af-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="fa2af-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="fa2af-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="fa2af-145">**[Trello tesztfelhasználó létrehozása](#creating-a-trello-test-user)**  -toohave egy megfelelője a Britta Simon a Trello, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="fa2af-145">**[Creating a Trello test user](#creating-a-trello-test-user)** - toohave a counterpart of Britta Simon in Trello that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="fa2af-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="fa2af-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="fa2af-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="fa2af-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="fa2af-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="fa2af-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="fa2af-149">Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és a Trello alkalmazásban egyszeri bejelentkezés beállítása.</span><span class="sxs-lookup"><span data-stu-id="fa2af-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Trello application.</span></span>

<span data-ttu-id="fa2af-150">**az Azure AD tooconfigure egyszeri bejelentkezést a Trello, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="fa2af-150">**tooconfigure Azure AD single sign-on with Trello, perform hello following steps:**</span></span>

1. <span data-ttu-id="fa2af-151">Az Azure portál, a hello hello **Trello** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="fa2af-151">In hello Azure portal, on hello **Trello** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="fa2af-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="fa2af-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-trello-tutorial/tutorial_trello_samlbase.png)

3. <span data-ttu-id="fa2af-155">A hello **Trello tartomány és az URL-címek** szakaszban, ha tooconfigure hello alkalmazás **IDP kezdeményezett mód**, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="fa2af-155">On hello **Trello Domain and URLs** section, If you wish tooconfigure hello application in **IDP initiated mode**, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-trello-tutorial/tutorial_trello_url.png)

    <span data-ttu-id="fa2af-157">A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://trello.com/auth/saml/consume/<enterprise>`</span><span class="sxs-lookup"><span data-stu-id="fa2af-157">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://trello.com/auth/saml/consume/<enterprise>`</span></span>

4. <span data-ttu-id="fa2af-158">A hello **Trello tartomány és az URL-címek** szakaszban, ha tooconfigure hello alkalmazás **Szolgáltató kezdeményezett mód**, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="fa2af-158">On hello **Trello Domain and URLs** section, If you wish tooconfigure hello application in **SP initiated mode**, perform hello following steps:</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-trello-tutorial/tutorial_trello_url1.png)

    <span data-ttu-id="fa2af-160">a.</span><span class="sxs-lookup"><span data-stu-id="fa2af-160">a.</span></span> <span data-ttu-id="fa2af-161">Kattintson a hello **megjelenítése speciális URL-beállításainak**.</span><span class="sxs-lookup"><span data-stu-id="fa2af-161">Click on hello **Show advanced URL settings**.</span></span>

    <span data-ttu-id="fa2af-162">b.</span><span class="sxs-lookup"><span data-stu-id="fa2af-162">b.</span></span> <span data-ttu-id="fa2af-163">A hello **URL-cím bejelentkezési** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://trello.com/auth/saml/consume/<enterprise>`</span><span class="sxs-lookup"><span data-stu-id="fa2af-163">In hello **Sign On URL** textbox, type a URL using hello following pattern: `https://trello.com/auth/saml/consume/<enterprise>`</span></span>

    >[!NOTE]
    ><span data-ttu-id="fa2af-164">Hello kell kapnia  **\<vállalati\>**  a Trello slug.</span><span class="sxs-lookup"><span data-stu-id="fa2af-164">You should get hello **\<enterprise\>** slug from Trello.</span></span> <span data-ttu-id="fa2af-165">Ha még nem rendelkezik hello slug érték, forduljon a [Trello támogatási csoport](mailto:support@trello.com) tooget hello slug, vállalati.</span><span class="sxs-lookup"><span data-stu-id="fa2af-165">If you don't have hello slug value, contact [Trello support team](mailto:support@trello.com) tooget hello slug for you enterprise.</span></span>
    > 

5. <span data-ttu-id="fa2af-166">Trello alkalmazás hello SAML helyességi feltételek toocontain adott attribútumok vár.</span><span class="sxs-lookup"><span data-stu-id="fa2af-166">Trello application expects hello SAML assertions toocontain specific attributes.</span></span> <span data-ttu-id="fa2af-167">A következő attribútumok ehhez az alkalmazáshoz hello konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="fa2af-167">Configure hello following attributes  for this application.</span></span> <span data-ttu-id="fa2af-168">Ezek az attribútumok értékének hello kezelheti hello **"Felhasználói attribútumok"** hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="fa2af-168">You can manage hello values of these attributes from hello **"User Attributes"** of hello application.</span></span> <span data-ttu-id="fa2af-169">a következő képernyőkép hello ezen mutat egy példát.</span><span class="sxs-lookup"><span data-stu-id="fa2af-169">hello following screenshot shows an example for this.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-trello-tutorial/tutorial_trello_attribute.png)

6. <span data-ttu-id="fa2af-171">A hello **SAML-jogkivonat attribútumok** párbeszédpanel, az alábbi, hello táblázatban szereplő minden egyes sorára hajtsa végre a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="fa2af-171">On hello **SAML token attributes** dialog, for each row shown in hello table below, perform hello following steps:</span></span>
 
    | <span data-ttu-id="fa2af-172">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="fa2af-172">Attribute Name</span></span> | <span data-ttu-id="fa2af-173">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="fa2af-173">Attribute Value</span></span> |
    | --- | --- |
    | <span data-ttu-id="fa2af-174">User.Email</span><span class="sxs-lookup"><span data-stu-id="fa2af-174">User.Email</span></span> | <span data-ttu-id="fa2af-175">User.mail</span><span class="sxs-lookup"><span data-stu-id="fa2af-175">user.mail</span></span> |
    | <span data-ttu-id="fa2af-176">User.FirstName</span><span class="sxs-lookup"><span data-stu-id="fa2af-176">User.FirstName</span></span> | <span data-ttu-id="fa2af-177">User.givenName</span><span class="sxs-lookup"><span data-stu-id="fa2af-177">user.givenname</span></span> |
    | <span data-ttu-id="fa2af-178">User.LastName</span><span class="sxs-lookup"><span data-stu-id="fa2af-178">User.LastName</span></span> | <span data-ttu-id="fa2af-179">User.surname</span><span class="sxs-lookup"><span data-stu-id="fa2af-179">user.surname</span></span> |

    <span data-ttu-id="fa2af-180">a.</span><span class="sxs-lookup"><span data-stu-id="fa2af-180">a.</span></span> <span data-ttu-id="fa2af-181">Kattintson a **Hozzáadás attribútum** tooopen hello **attribútum hozzáadása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="fa2af-181">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-trello-tutorial/tutorial_officespace_04.png)

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-trello-tutorial/tutorial_officespace_05.png)

    <span data-ttu-id="fa2af-184">b.</span><span class="sxs-lookup"><span data-stu-id="fa2af-184">b.</span></span> <span data-ttu-id="fa2af-185">A hello **neve** szövegmezőben, az adott sorhoz feltüntetett hello attribútum neve.</span><span class="sxs-lookup"><span data-stu-id="fa2af-185">In hello **Name** textbox, type hello attribute name shown for that row.</span></span> 

    <span data-ttu-id="fa2af-186">c.</span><span class="sxs-lookup"><span data-stu-id="fa2af-186">c.</span></span> <span data-ttu-id="fa2af-187">A hello **érték** listájában, hello attribútuma Típusérték az adott sorhoz feltüntetett.</span><span class="sxs-lookup"><span data-stu-id="fa2af-187">From hello **Value** list, type hello attribute value shown for that row.</span></span>
    
    <span data-ttu-id="fa2af-188">d.</span><span class="sxs-lookup"><span data-stu-id="fa2af-188">d.</span></span> <span data-ttu-id="fa2af-189">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="fa2af-189">Click **Ok**.</span></span> 
 
7. <span data-ttu-id="fa2af-190">A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="fa2af-190">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-trello-tutorial/tutorial_trello_certificate.png) 

8. <span data-ttu-id="fa2af-192">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="fa2af-192">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-trello-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="fa2af-194">A hello **Trello konfigurációs** kattintson **konfigurálása Trello** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="fa2af-194">On hello **Trello Configuration** section, click **Configure Trello** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="fa2af-195">Másolás hello **SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="fa2af-195">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-trello-tutorial/tutorial_trello_configure.png) 

9. <span data-ttu-id="fa2af-197">túl nyissa meg az alkalmazáshoz konfigurált SSO tooget[Trello vállalati SSO konfigurációs](https://trello.com/sso-configuration) lap toosend [Trello támogatási csoport](mailto:support@trello.com) hello **SAML-alapú egyszeri bejelentkezési URL-címe** és hello csatolása **tanúsítvány (Base64)**.</span><span class="sxs-lookup"><span data-stu-id="fa2af-197">tooget SSO configured for your application, go too[Trello enterprise SSO configuration](https://trello.com/sso-configuration) page toosend [Trello support team](mailto:support@trello.com) hello **SAML Single Sign-On Service URL** and attach hello **Certificate (Base64)**.</span></span>

> [!TIP]
> <span data-ttu-id="fa2af-198">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="fa2af-198">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="fa2af-199">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="fa2af-199">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="fa2af-200">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="fa2af-200">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="fa2af-201">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="fa2af-201">Creating an Azure AD test user</span></span>
<span data-ttu-id="fa2af-202">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="fa2af-202">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="fa2af-204">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="fa2af-204">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="fa2af-205">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="fa2af-205">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-trello-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="fa2af-207">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="fa2af-207">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-trello-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="fa2af-209">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="fa2af-209">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-trello-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="fa2af-211">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="fa2af-211">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-trello-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="fa2af-213">a.</span><span class="sxs-lookup"><span data-stu-id="fa2af-213">a.</span></span> <span data-ttu-id="fa2af-214">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="fa2af-214">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="fa2af-215">b.</span><span class="sxs-lookup"><span data-stu-id="fa2af-215">b.</span></span> <span data-ttu-id="fa2af-216">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="fa2af-216">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="fa2af-217">c.</span><span class="sxs-lookup"><span data-stu-id="fa2af-217">c.</span></span> <span data-ttu-id="fa2af-218">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="fa2af-218">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="fa2af-219">d.</span><span class="sxs-lookup"><span data-stu-id="fa2af-219">d.</span></span> <span data-ttu-id="fa2af-220">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="fa2af-220">Click **Create**.</span></span>
 
### <a name="creating-a-trello-test-user"></a><span data-ttu-id="fa2af-221">Trello tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="fa2af-221">Creating a Trello test user</span></span>

<span data-ttu-id="fa2af-222">Ebben a szakaszban a Trello Britta Simon nevű felhasználó hoz létre.</span><span class="sxs-lookup"><span data-stu-id="fa2af-222">In this section, you create a user called Britta Simon in Trello.</span></span> <span data-ttu-id="fa2af-223">Ebben a szakaszban a Trello Britta Simon nevű felhasználó hoz létre.</span><span class="sxs-lookup"><span data-stu-id="fa2af-223">In this section, you create a user called Britta Simon in Trello.</span></span> <span data-ttu-id="fa2af-224">Trello támogatja a létesítést just-in-time és új fiók jön létre hello első alkalommal jelentkezik be az Azure AD.</span><span class="sxs-lookup"><span data-stu-id="fa2af-224">Trello supports just-in-time provisioning and a new account is created hello first time you sign in from Azure AD.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="fa2af-225">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="fa2af-225">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="fa2af-226">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooTrello megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="fa2af-226">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooTrello.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="fa2af-228">**tooassign Britta Simon tooTrello, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="fa2af-228">**tooassign Britta Simon tooTrello, perform hello following steps:**</span></span>

1. <span data-ttu-id="fa2af-229">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="fa2af-229">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="fa2af-231">Hello alkalmazások listában válassza ki a **Trello**.</span><span class="sxs-lookup"><span data-stu-id="fa2af-231">In hello applications list, select **Trello**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-trello-tutorial/tutorial_trello_app.png) 

3. <span data-ttu-id="fa2af-233">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="fa2af-233">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="fa2af-235">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="fa2af-235">Click **Add** button.</span></span> <span data-ttu-id="fa2af-236">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="fa2af-236">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="fa2af-238">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="fa2af-238">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="fa2af-239">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="fa2af-239">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="fa2af-240">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="fa2af-240">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="fa2af-241">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="fa2af-241">Testing single sign-on</span></span>

<span data-ttu-id="fa2af-242">hello ebben a szakaszban célja tootest hozzáférési Panel az Azure AD SSO konfigurációs használatával hello.</span><span class="sxs-lookup"><span data-stu-id="fa2af-242">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="fa2af-243">Ha a hozzáférési Panel hello hello Trello csempe gombra kattint, automatikusan bejelentkezett tooyour Trello alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="fa2af-243">When you click hello Trello tile in hello Access Panel, you should get automatically signed-on tooyour Trello application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="fa2af-244">További források</span><span class="sxs-lookup"><span data-stu-id="fa2af-244">Additional resources</span></span>

* [<span data-ttu-id="fa2af-245">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="fa2af-245">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="fa2af-246">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="fa2af-246">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-trello-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-trello-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-trello-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-trello-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-trello-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-trello-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-trello-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-trello-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-trello-tutorial/tutorial_general_203.png

