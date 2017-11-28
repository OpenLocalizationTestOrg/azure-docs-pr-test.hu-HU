---
title: "Oktatóanyag: Azure Active Directoryval integrált CloudPassage |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és CloudPassage között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: bfe1f14e-74e4-4680-ac9e-f7355e1c94cc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: 32fb007b90f071626c9b40fb5afc341dd3c1ae99
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-cloudpassage"></a><span data-ttu-id="4c2d4-103">Oktatóanyag: Azure Active Directoryval integrált CloudPassage</span><span class="sxs-lookup"><span data-stu-id="4c2d4-103">Tutorial: Azure Active Directory integration with CloudPassage</span></span>

<span data-ttu-id="4c2d4-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate CloudPassage az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="4c2d4-104">In this tutorial, you learn how toointegrate CloudPassage with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="4c2d4-105">CloudPassage integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="4c2d4-105">Integrating CloudPassage with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="4c2d4-106">Megadhatja a hozzáférés tooCloudPassage rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="4c2d4-106">You can control in Azure AD who has access tooCloudPassage</span></span>
- <span data-ttu-id="4c2d4-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooCloudPassage (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="4c2d4-107">You can enable your users tooautomatically get signed-on tooCloudPassage (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="4c2d4-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="4c2d4-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="4c2d4-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="4c2d4-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4c2d4-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="4c2d4-110">Prerequisites</span></span>

<span data-ttu-id="4c2d4-111">az Azure AD integrálása CloudPassage tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="4c2d4-111">tooconfigure Azure AD integration with CloudPassage, you need hello following items:</span></span>

- <span data-ttu-id="4c2d4-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="4c2d4-112">An Azure AD subscription</span></span>
- <span data-ttu-id="4c2d4-113">Egy CloudPassage egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="4c2d4-113">A CloudPassage single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="4c2d4-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="4c2d4-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="4c2d4-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="4c2d4-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="4c2d4-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="4c2d4-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="4c2d4-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="4c2d4-118">Scenario description</span></span>
<span data-ttu-id="4c2d4-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="4c2d4-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="4c2d4-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="4c2d4-121">Hello gyűjteményből CloudPassage hozzáadása</span><span class="sxs-lookup"><span data-stu-id="4c2d4-121">Adding CloudPassage from hello gallery</span></span>
2. <span data-ttu-id="4c2d4-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="4c2d4-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-cloudpassage-from-hello-gallery"></a><span data-ttu-id="4c2d4-123">Hello gyűjteményből CloudPassage hozzáadása</span><span class="sxs-lookup"><span data-stu-id="4c2d4-123">Adding CloudPassage from hello gallery</span></span>
<span data-ttu-id="4c2d4-124">tooconfigure hello integrációja CloudPassage az Azure AD-be, meg kell tooadd CloudPassage hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-124">tooconfigure hello integration of CloudPassage into Azure AD, you need tooadd CloudPassage from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="4c2d4-125">**tooadd CloudPassage hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="4c2d4-125">**tooadd CloudPassage from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="4c2d4-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="4c2d4-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="4c2d4-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="4c2d4-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="4c2d4-133">Hello keresési mezőbe, írja be a **CloudPassage**.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-133">In hello search box, type **CloudPassage**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_search.png)

5. <span data-ttu-id="4c2d4-135">A hello eredmények panelen válassza ki a **CloudPassage**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-135">In hello results panel, select **CloudPassage**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="4c2d4-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="4c2d4-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="4c2d4-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján CloudPassage</span><span class="sxs-lookup"><span data-stu-id="4c2d4-138">In this section, you configure and test Azure AD single sign-on with CloudPassage based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="4c2d4-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó CloudPassage tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in CloudPassage is tooa user in Azure AD.</span></span> <span data-ttu-id="4c2d4-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello CloudPassage közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-140">In other words, a link relationship between an Azure AD user and hello related user in CloudPassage needs toobe established.</span></span>

<span data-ttu-id="4c2d4-141">CloudPassage, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-141">In CloudPassage, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="4c2d4-142">tooconfigure és az Azure AD az egyszeri bejelentkezés CloudPassage-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="4c2d4-142">tooconfigure and test Azure AD single sign-on with CloudPassage, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="4c2d4-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="4c2d4-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="4c2d4-145">**[CloudPassage tesztfelhasználó létrehozása](#creating-a-cloudpassage-test-user)**  -toohave egy megfelelője a Britta Simon a CloudPassage, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-145">**[Creating a CloudPassage test user](#creating-a-cloudpassage-test-user)** - toohave a counterpart of Britta Simon in CloudPassage that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="4c2d4-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="4c2d4-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="4c2d4-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="4c2d4-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="4c2d4-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az CloudPassage alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your CloudPassage application.</span></span>

<span data-ttu-id="4c2d4-150">**az Azure AD tooconfigure egyszeri bejelentkezést a CloudPassage, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="4c2d4-150">**tooconfigure Azure AD single sign-on with CloudPassage, perform hello following steps:**</span></span>

1. <span data-ttu-id="4c2d4-151">Az Azure portál, a hello hello **CloudPassage** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-151">In hello Azure portal, on hello **CloudPassage** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="4c2d4-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_samlbase.png)

3. <span data-ttu-id="4c2d4-155">A hello **CloudPassage tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="4c2d4-155">On hello **CloudPassage Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_url.png)

    <span data-ttu-id="4c2d4-157">a.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-157">a.</span></span> <span data-ttu-id="4c2d4-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://portal.cloudpassage.com/saml/init/accountid`</span><span class="sxs-lookup"><span data-stu-id="4c2d4-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://portal.cloudpassage.com/saml/init/accountid`</span></span>

    <span data-ttu-id="4c2d4-159">b.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-159">b.</span></span> <span data-ttu-id="4c2d4-160">A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be: `https://portal.cloudpassage.com/saml/consume/accountid`.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://portal.cloudpassage.com/saml/consume/accountid`.</span></span> <span data-ttu-id="4c2d4-161">Kaphat a érték ehhez az attribútumhoz kattintva **egyszeri bejelentkezés beállítása dokumentáció** a hello **egyszeri bejelentkezési beállítások** a CloudPassage portál szakaszban.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-161">You can get your value for this attribute by clicking **SSO Setup documentation** in hello **Single Sign-on Settings** section of your CloudPassage portal.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_05.png)
     
    > [!NOTE] 
    > <span data-ttu-id="4c2d4-163">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-163">These values are not real.</span></span> <span data-ttu-id="4c2d4-164">Frissítheti ezeket az értékeket hello tényleges válasz URL-CÍMEN, és a bejelentkezési URL-cím.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-164">Update these values with hello actual Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="4c2d4-165">Ügyfél [CloudPassage ügyfél-támogatási csoport](https://www.cloudpassage.com/company/contact/) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-165">Contact [CloudPassage Client support team](https://www.cloudpassage.com/company/contact/) tooget these values.</span></span> 

4. <span data-ttu-id="4c2d4-166">A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-166">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_certificate.png) 

5. <span data-ttu-id="4c2d4-168">A CloudPassage alkalmazás hello SAML helyességi feltételek vár egy meghatározott formátumban, és hogy tooadd egyéni attribútum hozzárendelések tooyour SAML token attribútumok konfigurációt igényel.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-168">Your CloudPassage application expects hello SAML assertions in a specific format, which requires you tooadd custom attribute mappings tooyour SAML token attributes configuration.</span></span>   
<span data-ttu-id="4c2d4-169">a következő képernyőkép hello ezen mutat egy példát.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-169">hello following screenshot shows an example for this.</span></span>
   
   ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_25.png) 

6. <span data-ttu-id="4c2d4-171">A hello **felhasználói attribútumok** hello című szakaszban **egyszeri bejelentkezés** párbeszédpanelen konfigurálja a SAML-jogkivonat attribútum fenti hello ábrán látható módon, és hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="4c2d4-171">In hello **User Attributes** section on hello **Single sign-on** dialog, configure SAML token attribute as shown in hello image above and perform hello following steps:</span></span>

    | <span data-ttu-id="4c2d4-172">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="4c2d4-172">Attribute Name</span></span> | <span data-ttu-id="4c2d4-173">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="4c2d4-173">Attribute Value</span></span> |
    | --- | --- |
    | <span data-ttu-id="4c2d4-174">Utónév</span><span class="sxs-lookup"><span data-stu-id="4c2d4-174">firstname</span></span> |<span data-ttu-id="4c2d4-175">User.givenName</span><span class="sxs-lookup"><span data-stu-id="4c2d4-175">user.givenname</span></span> |
    | <span data-ttu-id="4c2d4-176">Vezetéknév</span><span class="sxs-lookup"><span data-stu-id="4c2d4-176">lastname</span></span> |<span data-ttu-id="4c2d4-177">User.surname</span><span class="sxs-lookup"><span data-stu-id="4c2d4-177">user.surname</span></span> |
    | <span data-ttu-id="4c2d4-178">E-mailek</span><span class="sxs-lookup"><span data-stu-id="4c2d4-178">email</span></span> |<span data-ttu-id="4c2d4-179">User.mail</span><span class="sxs-lookup"><span data-stu-id="4c2d4-179">user.mail</span></span> |
    
    <span data-ttu-id="4c2d4-180">a.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-180">a.</span></span> <span data-ttu-id="4c2d4-181">Kattintson a **Hozzáadás attribútum** tooopen hello **attribútum hozzáadása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-181">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cloudpassage-tutorial/tutorial_attribute_04.png)
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cloudpassage-tutorial/tutorial_attribute_05.png)
    
    <span data-ttu-id="4c2d4-184">b.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-184">b.</span></span> <span data-ttu-id="4c2d4-185">A hello **neve** szövegmezőben, az adott sorhoz feltüntetett hello attribútum neve.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-185">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>

    <span data-ttu-id="4c2d4-186">c.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-186">c.</span></span> <span data-ttu-id="4c2d4-187">A hello **érték** listájában, hello attribútuma Típusérték az adott sorhoz feltüntetett.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-187">From hello **Value** list, type hello attribute value shown for that row.</span></span>
    
    <span data-ttu-id="4c2d4-188">d.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-188">d.</span></span> <span data-ttu-id="4c2d4-189">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-189">Click **Ok**.</span></span>

7. <span data-ttu-id="4c2d4-190">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-190">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_400.png)
    
8. <span data-ttu-id="4c2d4-192">A hello **CloudPassage konfigurációs** kattintson **konfigurálása CloudPassage** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-192">On hello **CloudPassage Configuration** section, click **Configure CloudPassage** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="4c2d4-193">Másolás hello **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="4c2d4-193">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_configure.png) 

9. <span data-ttu-id="4c2d4-195">Egy másik böngészőablakban, bejelentkezés tooyour CloudPassage vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-195">In a different browser window, sign-on tooyour CloudPassage company site as administrator.</span></span>

10. <span data-ttu-id="4c2d4-196">Hello hello felső menüben kattintson a **beállítások**, és kattintson a **helyfelügyelet**.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-196">In hello menu on hello top, click **Settings**, and then click **Site Administration**.</span></span> 
   
    ![Egyszeri bejelentkezés konfigurálása][12]

11. <span data-ttu-id="4c2d4-198">Kattintson a hello **hitelesítési beállítások** fülre.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-198">Click hello **Authentication Settings** tab.</span></span> 
   
    ![Egyszeri bejelentkezés konfigurálása][13]

12. <span data-ttu-id="4c2d4-200">A hello **egyszeri bejelentkezési beállítások** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="4c2d4-200">In hello **Single Sign-on Settings** section, perform hello following steps:</span></span> 
   
    ![Egyszeri bejelentkezés konfigurálása][14]

    <span data-ttu-id="4c2d4-202">a.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-202">a.</span></span> <span data-ttu-id="4c2d4-203">Válassza ki **engedélyezése egyetlen sign-on(SSO) (egyszeri bejelentkezés beállítása dokumentáció)** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-203">Select **Enable Single sign-on(SSO)(SSO Setup Documentation)** checkbox.</span></span>
    
    <span data-ttu-id="4c2d4-204">b.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-204">b.</span></span> <span data-ttu-id="4c2d4-205">Beillesztés **SAML Entitásazonosító** történő hello **SAML kiállítójának URL-címe** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-205">Paste **SAML Entity ID** into hello **SAML issuer URL** textbox.</span></span>
  
    <span data-ttu-id="4c2d4-206">c.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-206">c.</span></span> <span data-ttu-id="4c2d4-207">Beillesztés **SAML-alapú egyszeri bejelentkezési URL-címe** történő hello **SAML végponti URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-207">Paste **SAML Single Sign-On Service URL** into hello **SAML endpoint URL** textbox.</span></span>
  
    <span data-ttu-id="4c2d4-208">d.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-208">d.</span></span> <span data-ttu-id="4c2d4-209">Beillesztés **Sign-Out URL-cím** történő hello **kijelentkezési kezdőlapja** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-209">Paste **Sign-Out URL** into hello **Logout landing page** textbox.</span></span>
  
    <span data-ttu-id="4c2d4-210">e.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-210">e.</span></span> <span data-ttu-id="4c2d4-211">Nyissa meg a letöltött tanúsítvány a Jegyzettömbben, másolása hello tartalom letöltött tanúsítvány a vágólapra és illessze be hello **x 509 tanúsítvány** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-211">Open your downloaded certificate in notepad, copy hello content of downloaded certificate into your clipboard, and then paste it into hello **x 509 certificate** textbox.</span></span>
  
    <span data-ttu-id="4c2d4-212">f.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-212">f.</span></span> <span data-ttu-id="4c2d4-213">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-213">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="4c2d4-214">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="4c2d4-214">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="4c2d4-215">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-215">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="4c2d4-216">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="4c2d4-216">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="4c2d4-217">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="4c2d4-217">Creating an Azure AD test user</span></span>
<span data-ttu-id="4c2d4-218">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-218">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="4c2d4-220">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="4c2d4-220">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="4c2d4-221">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-221">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cloudpassage-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="4c2d4-223">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-223">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cloudpassage-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="4c2d4-225">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-225">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cloudpassage-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="4c2d4-227">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="4c2d4-227">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cloudpassage-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="4c2d4-229">a.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-229">a.</span></span> <span data-ttu-id="4c2d4-230">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-230">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="4c2d4-231">b.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-231">b.</span></span> <span data-ttu-id="4c2d4-232">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-232">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="4c2d4-233">c.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-233">c.</span></span> <span data-ttu-id="4c2d4-234">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-234">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="4c2d4-235">d.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-235">d.</span></span> <span data-ttu-id="4c2d4-236">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-236">Click **Create**.</span></span>
 
### <a name="creating-a-cloudpassage-test-user"></a><span data-ttu-id="4c2d4-237">CloudPassage tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="4c2d4-237">Creating a CloudPassage test user</span></span>

<span data-ttu-id="4c2d4-238">hello ebben a szakaszban célja toocreate CloudPassage Britta Simon nevű felhasználó.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-238">hello objective of this section is toocreate a user called Britta Simon in CloudPassage.</span></span>

<span data-ttu-id="4c2d4-239">**toocreate Britta Simon meghívta CloudPassage, a felhasználó hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="4c2d4-239">**toocreate a user called Britta Simon in CloudPassage, perform hello following steps:**</span></span>

1. <span data-ttu-id="4c2d4-240">Bejelentkezés tooyour **CloudPassage** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-240">Sign-on tooyour **CloudPassage** company site as an administrator.</span></span> 

2. <span data-ttu-id="4c2d4-241">Hello hello felső eszköztárán kattintson **beállítások**, és kattintson a **helyfelügyelet**.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-241">In hello toolbar on hello top, click **Settings**, and then click **Site Administration**.</span></span> 
   
   ![CloudPassage tesztfelhasználó létrehozása][22] 

3. <span data-ttu-id="4c2d4-243">Kattintson a hello **felhasználók** fülre, majd **új felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-243">Click hello **Users** tab, and then click **Add New User**.</span></span> 
   
   ![CloudPassage tesztfelhasználó létrehozása][23]

4. <span data-ttu-id="4c2d4-245">A hello **új felhasználó hozzáadása** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="4c2d4-245">In hello **Add New User** section, perform hello following steps:</span></span> 
   
   ![CloudPassage tesztfelhasználó létrehozása][24]
    
    <span data-ttu-id="4c2d4-247">a.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-247">a.</span></span> <span data-ttu-id="4c2d4-248">A hello **Keresztnév** szövegmező, írja be a Britta.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-248">In hello **First Name** textbox, type Britta.</span></span> 
  
    <span data-ttu-id="4c2d4-249">b.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-249">b.</span></span> <span data-ttu-id="4c2d4-250">A hello **Vezetéknév** szövegmezőhöz Simon írja be.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-250">In hello **Last Name** textbox, type Simon.</span></span>
  
    <span data-ttu-id="4c2d4-251">c.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-251">c.</span></span> <span data-ttu-id="4c2d4-252">A hello **felhasználónév** szövegmezőhöz hello **E-mail** szövegmező és hello **E-mail írja be újra** szövegmező, írja be a Britta tartozó felhasználónevet az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-252">In hello **Username** textbox, hello **Email** textbox and hello **Retype Email** textbox, type Britta's user name in Azure AD.</span></span>
  
    <span data-ttu-id="4c2d4-253">d.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-253">d.</span></span> <span data-ttu-id="4c2d4-254">Mint **hozzáférési típus**, jelölje be **Halo Portal hozzáférés engedélyezése**.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-254">As **Access Type**, select **Enable Halo Portal Access**.</span></span>
  
    <span data-ttu-id="4c2d4-255">e.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-255">e.</span></span> <span data-ttu-id="4c2d4-256">Kattintson az **Add** (Hozzáadás) parancsra.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-256">Click **Add**.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="4c2d4-257">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="4c2d4-257">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="4c2d4-258">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooCloudPassage megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-258">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooCloudPassage.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="4c2d4-260">**tooassign Britta Simon tooCloudPassage, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="4c2d4-260">**tooassign Britta Simon tooCloudPassage, perform hello following steps:**</span></span>

1. <span data-ttu-id="4c2d4-261">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-261">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="4c2d4-263">Hello alkalmazások listában válassza ki a **CloudPassage**.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-263">In hello applications list, select **CloudPassage**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_app.png) 

3. <span data-ttu-id="4c2d4-265">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-265">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="4c2d4-267">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-267">Click **Add** button.</span></span> <span data-ttu-id="4c2d4-268">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-268">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="4c2d4-270">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-270">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="4c2d4-271">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-271">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="4c2d4-272">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-272">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="4c2d4-273">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="4c2d4-273">Testing single sign-on</span></span>

<span data-ttu-id="4c2d4-274">hello ebben a szakaszban célja tootest hozzáférési Panel az Azure AD SSO konfigurációs használatával hello.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-274">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="4c2d4-275">Ha a hozzáférési Panel hello hello CloudPassage csempe gombra kattint, automatikusan bejelentkezett tooyour CloudPassage alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="4c2d4-275">When you click hello CloudPassage tile in hello Access Panel, you should get automatically signed-on tooyour CloudPassage application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4c2d4-276">További források</span><span class="sxs-lookup"><span data-stu-id="4c2d4-276">Additional resources</span></span>

* [<span data-ttu-id="4c2d4-277">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="4c2d4-277">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="4c2d4-278">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="4c2d4-278">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_04.png
[12]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_07.png
[13]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_08.png
[14]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_09.png
[15]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_10.png
[22]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_15.png
[23]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_16.png
[24]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_17.png

[100]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_203.png

