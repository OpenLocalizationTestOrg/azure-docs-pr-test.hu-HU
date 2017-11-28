---
title: "Oktatóanyag: Azure Active Directoryval integrált Allocadia |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Allocadia között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c415fc55-6dc1-49f2-a8a2-2fc6e3790d65
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2017
ms.author: jeedes
ms.openlocfilehash: 9a01c232f9dc50e690dd348430899db9c13f1564
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-allocadia"></a><span data-ttu-id="6ee1f-103">Oktatóanyag: Azure Active Directoryval integrált Allocadia</span><span class="sxs-lookup"><span data-stu-id="6ee1f-103">Tutorial: Azure Active Directory integration with Allocadia</span></span>

<span data-ttu-id="6ee1f-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Allocadia az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="6ee1f-104">In this tutorial, you learn how toointegrate Allocadia with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="6ee1f-105">Allocadia integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="6ee1f-105">Integrating Allocadia with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="6ee1f-106">Megadhatja a hozzáférés tooAllocadia rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="6ee1f-106">You can control in Azure AD who has access tooAllocadia</span></span>
- <span data-ttu-id="6ee1f-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooAllocadia (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="6ee1f-107">You can enable your users tooautomatically get signed-on tooAllocadia (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="6ee1f-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="6ee1f-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="6ee1f-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="6ee1f-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6ee1f-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="6ee1f-110">Prerequisites</span></span>

<span data-ttu-id="6ee1f-111">az Azure AD integrálása Allocadia tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="6ee1f-111">tooconfigure Azure AD integration with Allocadia, you need hello following items:</span></span>

- <span data-ttu-id="6ee1f-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="6ee1f-112">An Azure AD subscription</span></span>
- <span data-ttu-id="6ee1f-113">Egy Allocadia egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="6ee1f-113">An Allocadia single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="6ee1f-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="6ee1f-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="6ee1f-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="6ee1f-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="6ee1f-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="6ee1f-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="6ee1f-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="6ee1f-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="6ee1f-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="6ee1f-118">Scenario description</span></span>
<span data-ttu-id="6ee1f-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="6ee1f-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="6ee1f-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="6ee1f-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="6ee1f-121">Hello gyűjteményből Allocadia hozzáadása</span><span class="sxs-lookup"><span data-stu-id="6ee1f-121">Adding Allocadia from hello gallery</span></span>
2. <span data-ttu-id="6ee1f-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="6ee1f-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-allocadia-from-hello-gallery"></a><span data-ttu-id="6ee1f-123">Hello gyűjteményből Allocadia hozzáadása</span><span class="sxs-lookup"><span data-stu-id="6ee1f-123">Adding Allocadia from hello gallery</span></span>
<span data-ttu-id="6ee1f-124">tooconfigure hello integrációja Allocadia az Azure AD-be, meg kell tooadd Allocadia hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="6ee1f-124">tooconfigure hello integration of Allocadia into Azure AD, you need tooadd Allocadia from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="6ee1f-125">**tooadd Allocadia hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="6ee1f-125">**tooadd Allocadia from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="6ee1f-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="6ee1f-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="6ee1f-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="6ee1f-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="6ee1f-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="6ee1f-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="6ee1f-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="6ee1f-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="6ee1f-133">Hello keresési mezőbe, írja be a **Allocadia**.</span><span class="sxs-lookup"><span data-stu-id="6ee1f-133">In hello search box, type **Allocadia**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-allocadia-tutorial/tutorial_allocadia_search.png)

5. <span data-ttu-id="6ee1f-135">A hello eredmények panelen válassza ki a **Allocadia**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="6ee1f-135">In hello results panel, select **Allocadia**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-allocadia-tutorial/tutorial_allocadia_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="6ee1f-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="6ee1f-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="6ee1f-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján Allocadia</span><span class="sxs-lookup"><span data-stu-id="6ee1f-138">In this section, you configure and test Azure AD single sign-on with Allocadia based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="6ee1f-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Allocadia tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="6ee1f-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Allocadia is tooa user in Azure AD.</span></span> <span data-ttu-id="6ee1f-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Allocadia közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="6ee1f-140">In other words, a link relationship between an Azure AD user and hello related user in Allocadia needs toobe established.</span></span>

<span data-ttu-id="6ee1f-141">Allocadia, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="6ee1f-141">In Allocadia, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="6ee1f-142">tooconfigure és az Azure AD az egyszeri bejelentkezés Allocadia-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="6ee1f-142">tooconfigure and test Azure AD single sign-on with Allocadia, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="6ee1f-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="6ee1f-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="6ee1f-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="6ee1f-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="6ee1f-145">**[Egy Allocadia tesztfelhasználó létrehozása](#creating-an-allocadia-test-user)**  -toohave egy megfelelője a Britta Simon a Allocadia, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="6ee1f-145">**[Creating an Allocadia test user](#creating-an-allocadia-test-user)** - toohave a counterpart of Britta Simon in Allocadia that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="6ee1f-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="6ee1f-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="6ee1f-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="6ee1f-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="6ee1f-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="6ee1f-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="6ee1f-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Allocadia alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="6ee1f-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Allocadia application.</span></span>

<span data-ttu-id="6ee1f-150">**az Azure AD tooconfigure egyszeri bejelentkezést a Allocadia, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="6ee1f-150">**tooconfigure Azure AD single sign-on with Allocadia, perform hello following steps:**</span></span>

1. <span data-ttu-id="6ee1f-151">Az Azure portál, a hello hello **Allocadia** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="6ee1f-151">In hello Azure portal, on hello **Allocadia** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="6ee1f-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="6ee1f-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-allocadia-tutorial/tutorial_allocadia_samlbase.png)

3. <span data-ttu-id="6ee1f-155">A hello **Allocadia tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="6ee1f-155">On hello **Allocadia Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-allocadia-tutorial/tutorial_allocadia_url.png)

    <span data-ttu-id="6ee1f-157">a.</span><span class="sxs-lookup"><span data-stu-id="6ee1f-157">a.</span></span> <span data-ttu-id="6ee1f-158">A hello **azonosító** szövegmező, írja be egy URL-CÍMÉT a következő hello mintákra:</span><span class="sxs-lookup"><span data-stu-id="6ee1f-158">In hello **Identifier** textbox, type a URL using hello following patterns:</span></span> 
       
     <span data-ttu-id="6ee1f-159">A tesztkörnyezet-`https://na2standby.allocadia.com`</span><span class="sxs-lookup"><span data-stu-id="6ee1f-159">For test environment -  `https://na2standby.allocadia.com`</span></span>
    
     <span data-ttu-id="6ee1f-160">Az éles környezetben-`https://na2.allocadia.com`</span><span class="sxs-lookup"><span data-stu-id="6ee1f-160">For production environment - `https://na2.allocadia.com`</span></span>

    <span data-ttu-id="6ee1f-161">b.</span><span class="sxs-lookup"><span data-stu-id="6ee1f-161">b.</span></span> <span data-ttu-id="6ee1f-162">A hello **válasz URL-CÍMEN** szövegmező, írja be egy URL-CÍMÉT a következő hello mintákra:</span><span class="sxs-lookup"><span data-stu-id="6ee1f-162">In hello **Reply URL** textbox, type a URL using hello following patterns:</span></span> 
    
     <span data-ttu-id="6ee1f-163">A tesztkörnyezet-`https://na2standby.allocadia.com/allocadia/saml/SSO`</span><span class="sxs-lookup"><span data-stu-id="6ee1f-163">For test environment - `https://na2standby.allocadia.com/allocadia/saml/SSO`</span></span>
    
     <span data-ttu-id="6ee1f-164">Az éles környezetben-`https://na2.allocadia.com/allocadia/saml/SSO`</span><span class="sxs-lookup"><span data-stu-id="6ee1f-164">For production environment - `https://na2.allocadia.com/allocadia/saml/SSO`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="6ee1f-165">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="6ee1f-165">These values are not real.</span></span> <span data-ttu-id="6ee1f-166">Frissítheti ezeket az értékeket hello tényleges azonosítója és a válasz URL-címmel.</span><span class="sxs-lookup"><span data-stu-id="6ee1f-166">Update these values with hello actual Identifier and Reply URL.</span></span> <span data-ttu-id="6ee1f-167">Ügyfél [Allocadia támogatási csoport](mailTo:support@allocadia.com) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="6ee1f-167">Contact [Allocadia support team](mailTo:support@allocadia.com) tooget these values.</span></span>

4. <span data-ttu-id="6ee1f-168">Allocadia alkalmazás hello SAML helyességi feltételek vár egy meghatározott formátumban.</span><span class="sxs-lookup"><span data-stu-id="6ee1f-168">Allocadia application expects hello SAML assertions in a specific format.</span></span> <span data-ttu-id="6ee1f-169">Az alkalmazás jogcímek a következő hello konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="6ee1f-169">Configure hello following claims for this application.</span></span> <span data-ttu-id="6ee1f-170">Ezek az attribútumok értékének hello hello kezelése "**felhasználói attribútumok**" szakasz alkalmazás integráció lapján.</span><span class="sxs-lookup"><span data-stu-id="6ee1f-170">You can manage hello values of these attributes from hello "**User Attributes**" section on application integration page.</span></span> <span data-ttu-id="6ee1f-171">a következő képernyőkép hello ehhez a konfigurációhoz példáját mutatja be.</span><span class="sxs-lookup"><span data-stu-id="6ee1f-171">hello following screenshot shows an example for this configuration.</span></span> 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-allocadia-tutorial/tutorial_allocadia_attributes.png)
    
5. <span data-ttu-id="6ee1f-173">A hello **felhasználói attribútumok** szakaszt, hello **egyszeri bejelentkezés** párbeszédpanelen konfigurálja a SAML-jogkivonat attribútum hello ábrának megfelelően, és hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="6ee1f-173">In hello **User Attributes** section on hello **Single sign-on** dialog, configure SAML token attribute as shown in hello image and perform hello following steps:</span></span>
    
    | <span data-ttu-id="6ee1f-174">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="6ee1f-174">Attribute Name</span></span> | <span data-ttu-id="6ee1f-175">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="6ee1f-175">Attribute Value</span></span> |
    | ------------------- | -------------------- |    
    | <span data-ttu-id="6ee1f-176">Utónév</span><span class="sxs-lookup"><span data-stu-id="6ee1f-176">firstname</span></span> | <span data-ttu-id="6ee1f-177">User.givenName</span><span class="sxs-lookup"><span data-stu-id="6ee1f-177">user.givenname</span></span> |
    | <span data-ttu-id="6ee1f-178">Vezetéknév</span><span class="sxs-lookup"><span data-stu-id="6ee1f-178">lastname</span></span> | <span data-ttu-id="6ee1f-179">User.surname</span><span class="sxs-lookup"><span data-stu-id="6ee1f-179">user.surname</span></span> |
    | <span data-ttu-id="6ee1f-180">E-mailek</span><span class="sxs-lookup"><span data-stu-id="6ee1f-180">email</span></span> | <span data-ttu-id="6ee1f-181">User.mail</span><span class="sxs-lookup"><span data-stu-id="6ee1f-181">user.mail</span></span> |
    
    <span data-ttu-id="6ee1f-182">a.</span><span class="sxs-lookup"><span data-stu-id="6ee1f-182">a.</span></span> <span data-ttu-id="6ee1f-183">Kattintson a **Hozzáadás attribútum** tooopen hello **attribútum hozzáadása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="6ee1f-183">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-allocadia-tutorial/tutorial_attribute_04.png)

    <span data-ttu-id="6ee1f-185">b.</span><span class="sxs-lookup"><span data-stu-id="6ee1f-185">b.</span></span> <span data-ttu-id="6ee1f-186">A hello **neve** szövegmezőben, az adott sorhoz feltüntetett hello attribútum neve.</span><span class="sxs-lookup"><span data-stu-id="6ee1f-186">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-allocadia-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="6ee1f-188">c.</span><span class="sxs-lookup"><span data-stu-id="6ee1f-188">c.</span></span> <span data-ttu-id="6ee1f-189">A hello **érték** listájában, hello attribútuma Típusérték az adott sorhoz feltüntetett.</span><span class="sxs-lookup"><span data-stu-id="6ee1f-189">From hello **Value** list, type hello attribute value shown for that row.</span></span>
 
    <span data-ttu-id="6ee1f-190">d.</span><span class="sxs-lookup"><span data-stu-id="6ee1f-190">d.</span></span> <span data-ttu-id="6ee1f-191">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="6ee1f-191">Click **Ok**.</span></span>



6. <span data-ttu-id="6ee1f-192">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="6ee1f-192">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-allocadia-tutorial/tutorial_allocadia_certificate.png) 


7. <span data-ttu-id="6ee1f-194">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="6ee1f-194">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-allocadia-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="6ee1f-196">tooconfigure egyszeri bejelentkezést a **Allocadia** oldalon kell letöltött toosend hello **metaadatainak XML-kódja** túl[Allocadia támogatási csoport](mailTo:support@allocadia.com).</span><span class="sxs-lookup"><span data-stu-id="6ee1f-196">tooconfigure single sign-on on **Allocadia** side, you need toosend hello downloaded **Metadata XML** too[Allocadia support team](mailTo:support@allocadia.com).</span></span> <span data-ttu-id="6ee1f-197">Maguk állítják be ezt a beállítást toohave hello SAML SSO kapcsolat mindkét oldalán megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="6ee1f-197">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="6ee1f-198">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="6ee1f-198">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="6ee1f-199">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="6ee1f-199">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="6ee1f-200">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="6ee1f-200">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="6ee1f-201">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="6ee1f-201">Creating an Azure AD test user</span></span>
<span data-ttu-id="6ee1f-202">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="6ee1f-202">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="6ee1f-204">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="6ee1f-204">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="6ee1f-205">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="6ee1f-205">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-allocadia-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="6ee1f-207">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="6ee1f-207">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-allocadia-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="6ee1f-209">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="6ee1f-209">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-allocadia-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="6ee1f-211">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="6ee1f-211">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-allocadia-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="6ee1f-213">a.</span><span class="sxs-lookup"><span data-stu-id="6ee1f-213">a.</span></span> <span data-ttu-id="6ee1f-214">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="6ee1f-214">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="6ee1f-215">b.</span><span class="sxs-lookup"><span data-stu-id="6ee1f-215">b.</span></span> <span data-ttu-id="6ee1f-216">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="6ee1f-216">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="6ee1f-217">c.</span><span class="sxs-lookup"><span data-stu-id="6ee1f-217">c.</span></span> <span data-ttu-id="6ee1f-218">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="6ee1f-218">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="6ee1f-219">d.</span><span class="sxs-lookup"><span data-stu-id="6ee1f-219">d.</span></span> <span data-ttu-id="6ee1f-220">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="6ee1f-220">Click **Create**.</span></span>
 
### <a name="creating-an-allocadia-test-user"></a><span data-ttu-id="6ee1f-221">Egy Allocadia tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="6ee1f-221">Creating an Allocadia test user</span></span>

<span data-ttu-id="6ee1f-222">Alkalmazás csak az idő a felhasználók átadása támogatja.</span><span class="sxs-lookup"><span data-stu-id="6ee1f-222">Application supports Just in time user provisioning.</span></span> <span data-ttu-id="6ee1f-223">A hitelesítés után felhasználók hello alkalmazás automatikusan létrejönnek.</span><span class="sxs-lookup"><span data-stu-id="6ee1f-223">After authentication users are created in hello application automatically.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="6ee1f-224">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="6ee1f-224">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="6ee1f-225">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooAllocadia megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="6ee1f-225">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooAllocadia.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="6ee1f-227">**tooassign Britta Simon tooAllocadia, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="6ee1f-227">**tooassign Britta Simon tooAllocadia, perform hello following steps:**</span></span>

1. <span data-ttu-id="6ee1f-228">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="6ee1f-228">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="6ee1f-230">Hello alkalmazások listában válassza ki a **Allocadia**.</span><span class="sxs-lookup"><span data-stu-id="6ee1f-230">In hello applications list, select **Allocadia**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-allocadia-tutorial/tutorial_allocadia_app.png) 

3. <span data-ttu-id="6ee1f-232">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="6ee1f-232">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="6ee1f-234">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="6ee1f-234">Click **Add** button.</span></span> <span data-ttu-id="6ee1f-235">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="6ee1f-235">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="6ee1f-237">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="6ee1f-237">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="6ee1f-238">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="6ee1f-238">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="6ee1f-239">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="6ee1f-239">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="6ee1f-240">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="6ee1f-240">Testing single sign-on</span></span>

<span data-ttu-id="6ee1f-241">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="6ee1f-241">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="6ee1f-242">Ha a hozzáférési Panel hello hello Allocadia csempe gombra kattint, automatikusan bejelentkezett tooyour Allocadia alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="6ee1f-242">When you click hello Allocadia tile in hello Access Panel, you should get automatically signed-on tooyour Allocadia application.</span></span>
<span data-ttu-id="6ee1f-243">A hozzáférési Panel kapcsolatos további információkért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md)</span><span class="sxs-lookup"><span data-stu-id="6ee1f-243">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md)</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6ee1f-244">További források</span><span class="sxs-lookup"><span data-stu-id="6ee1f-244">Additional resources</span></span>

* [<span data-ttu-id="6ee1f-245">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="6ee1f-245">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="6ee1f-246">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="6ee1f-246">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_203.png

