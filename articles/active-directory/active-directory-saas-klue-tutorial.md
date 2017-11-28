---
title: "Oktatóanyag: Azure Active Directoryval integrált Klue |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Klue között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 08341008-980b-4111-adb2-97bbabbf1e47
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: jeedes
ms.openlocfilehash: bb9134a558d6c050f428690d57a3cea850b7dbee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-klue"></a><span data-ttu-id="eb876-103">Oktatóanyag: Azure Active Directoryval integrált Klue</span><span class="sxs-lookup"><span data-stu-id="eb876-103">Tutorial: Azure Active Directory integration with Klue</span></span>

<span data-ttu-id="eb876-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Klue az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="eb876-104">In this tutorial, you learn how toointegrate Klue with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="eb876-105">Klue integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="eb876-105">Integrating Klue with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="eb876-106">Megadhatja a hozzáférés tooKlue rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="eb876-106">You can control in Azure AD who has access tooKlue</span></span>
- <span data-ttu-id="eb876-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooKlue (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="eb876-107">You can enable your users tooautomatically get signed-on tooKlue (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="eb876-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="eb876-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="eb876-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="eb876-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="eb876-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="eb876-110">Prerequisites</span></span>

<span data-ttu-id="eb876-111">az Azure AD integrálása Klue tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="eb876-111">tooconfigure Azure AD integration with Klue, you need hello following items:</span></span>

- <span data-ttu-id="eb876-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="eb876-112">An Azure AD subscription</span></span>
- <span data-ttu-id="eb876-113">Egy Klue egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="eb876-113">A Klue single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="eb876-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="eb876-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="eb876-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="eb876-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="eb876-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="eb876-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="eb876-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="eb876-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="eb876-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="eb876-118">Scenario description</span></span>
<span data-ttu-id="eb876-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="eb876-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="eb876-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="eb876-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="eb876-121">Hello gyűjteményből Klue hozzáadása</span><span class="sxs-lookup"><span data-stu-id="eb876-121">Adding Klue from hello gallery</span></span>
2. <span data-ttu-id="eb876-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="eb876-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-klue-from-hello-gallery"></a><span data-ttu-id="eb876-123">Hello gyűjteményből Klue hozzáadása</span><span class="sxs-lookup"><span data-stu-id="eb876-123">Adding Klue from hello gallery</span></span>
<span data-ttu-id="eb876-124">tooconfigure hello integrációja Klue az Azure AD-be, meg kell tooadd Klue hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="eb876-124">tooconfigure hello integration of Klue into Azure AD, you need tooadd Klue from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="eb876-125">**tooadd Klue hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="eb876-125">**tooadd Klue from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="eb876-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="eb876-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="eb876-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="eb876-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="eb876-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="eb876-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="eb876-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="eb876-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="eb876-133">Hello keresési mezőbe, írja be a **Klue**.</span><span class="sxs-lookup"><span data-stu-id="eb876-133">In hello search box, type **Klue**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-klue-tutorial/tutorial_klue_search.png)

5. <span data-ttu-id="eb876-135">A hello eredmények panelen válassza ki a **Klue**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="eb876-135">In hello results panel, select **Klue**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-klue-tutorial/tutorial_klue_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="eb876-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="eb876-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="eb876-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Klue.</span><span class="sxs-lookup"><span data-stu-id="eb876-138">In this section, you configure and test Azure AD single sign-on with Klue based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="eb876-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Klue tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="eb876-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Klue is tooa user in Azure AD.</span></span> <span data-ttu-id="eb876-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Klue közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="eb876-140">In other words, a link relationship between an Azure AD user and hello related user in Klue needs toobe established.</span></span>

<span data-ttu-id="eb876-141">Klue, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="eb876-141">In Klue, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="eb876-142">tooconfigure és az Azure AD az egyszeri bejelentkezés Klue-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="eb876-142">tooconfigure and test Azure AD single sign-on with Klue, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="eb876-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="eb876-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="eb876-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="eb876-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="eb876-145">**[Klue tesztfelhasználó létrehozása](#creating-a-klue-test-user)**  -toohave egy megfelelője a Britta Simon a Klue, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="eb876-145">**[Creating a Klue test user](#creating-a-klue-test-user)** - toohave a counterpart of Britta Simon in Klue that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="eb876-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="eb876-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="eb876-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="eb876-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="eb876-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="eb876-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="eb876-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Klue alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="eb876-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Klue application.</span></span>

<span data-ttu-id="eb876-150">**az Azure AD tooconfigure egyszeri bejelentkezést a Klue, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="eb876-150">**tooconfigure Azure AD single sign-on with Klue, perform hello following steps:**</span></span>

1. <span data-ttu-id="eb876-151">Az Azure portál, a hello hello **Klue** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="eb876-151">In hello Azure portal, on hello **Klue** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="eb876-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="eb876-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-klue-tutorial/tutorial_klue_samlbase.png)

3. <span data-ttu-id="eb876-155">A hello **Klue tartomány és az URL-címek** szakaszban, ha tooconfigure hello alkalmazás **IDP** kezdeményezett mód:</span><span class="sxs-lookup"><span data-stu-id="eb876-155">On hello **Klue Domain and URLs** section, If you wish tooconfigure hello application in **IDP** initiated mode:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-klue-tutorial/tutorial_klue_url1.png)

    <span data-ttu-id="eb876-157">a.</span><span class="sxs-lookup"><span data-stu-id="eb876-157">a.</span></span> <span data-ttu-id="eb876-158">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`urn:klue:<Customer ID>`</span><span class="sxs-lookup"><span data-stu-id="eb876-158">In hello **Identifier** textbox, type a URL using hello following pattern: `urn:klue:<Customer ID>`</span></span>

    <span data-ttu-id="eb876-159">b.</span><span class="sxs-lookup"><span data-stu-id="eb876-159">b.</span></span> <span data-ttu-id="eb876-160">A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://app.klue.com/account/auth/saml/<Customer UUID>/callback`</span><span class="sxs-lookup"><span data-stu-id="eb876-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://app.klue.com/account/auth/saml/<Customer UUID>/callback`</span></span>

4. <span data-ttu-id="eb876-161">Ellenőrizze **megjelenítése speciális URL-beállításainak**.</span><span class="sxs-lookup"><span data-stu-id="eb876-161">Check **Show advanced URL settings**.</span></span> <span data-ttu-id="eb876-162">Ha tooconfigure hello alkalmazás **SP** kezdeményezett mód:</span><span class="sxs-lookup"><span data-stu-id="eb876-162">If you wish tooconfigure hello application in **SP** initiated mode:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-klue-tutorial/tutorial_klue_url2.png)

    <span data-ttu-id="eb876-164">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://app.klue.com/account/auth/saml/<Customer UUID>/`</span><span class="sxs-lookup"><span data-stu-id="eb876-164">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://app.klue.com/account/auth/saml/<Customer UUID>/`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="eb876-165">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="eb876-165">These values are not real.</span></span> <span data-ttu-id="eb876-166">Frissítheti ezeket az értékeket hello tényleges válasz URL-CÍMEN, a azonosítója és a bejelentkezési URL-cím.</span><span class="sxs-lookup"><span data-stu-id="eb876-166">Update these values with hello actual Reply URL, Identifier, and Sign-On URL.</span></span> <span data-ttu-id="eb876-167">Ügyfél [Klue ügyfél-támogatási csoport](mailto:support@klue.com) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="eb876-167">Contact [Klue Client support team](mailto:support@klue.com) tooget these values.</span></span>

5. <span data-ttu-id="eb876-168">hello Klue alkalmazás hello SAML helyességi feltételek vár egy meghatározott formátumban, és hogy tooadd egyéni attribútum hozzárendelések tooyour SAML token attribútumok konfigurációt igényel.</span><span class="sxs-lookup"><span data-stu-id="eb876-168">hello Klue application expects hello SAML assertions in a specific format, which requires you tooadd custom attribute mappings tooyour SAML token attributes configuration.</span></span> <span data-ttu-id="eb876-169">Ezek az attribútumok értékének hello hello kezelése "**felhasználói attribútumok**" szakasz alkalmazás integráció lapján.</span><span class="sxs-lookup"><span data-stu-id="eb876-169">You can manage hello values of these attributes from hello "**User Attributes**" section on application integration page.</span></span> 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-klue-tutorial/attribute.png)

6. <span data-ttu-id="eb876-171">A hello **felhasználói attribútumok** hello szakaszban **egyszeri bejelentkezés** párbeszédpanelen konfigurálja a SAML-jogkivonat attribútum, a lemezkép megelőző hello látható módon, és hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="eb876-171">In hello **User Attributes** section on hello **Single sign-on** dialog, configure SAML token attribute as shown in hello preceding image and perform hello following steps:</span></span>
    
    | <span data-ttu-id="eb876-172">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="eb876-172">Attribute Name</span></span>      | <span data-ttu-id="eb876-173">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="eb876-173">Attribute Value</span></span>      |
    | ------------------- | -------------------- |
    | <span data-ttu-id="eb876-174">Utónév</span><span class="sxs-lookup"><span data-stu-id="eb876-174">first_name</span></span>          | <span data-ttu-id="eb876-175">User.givenName</span><span class="sxs-lookup"><span data-stu-id="eb876-175">user.givenname</span></span> |
    | <span data-ttu-id="eb876-176">Vezetéknév</span><span class="sxs-lookup"><span data-stu-id="eb876-176">last_name</span></span>           | <span data-ttu-id="eb876-177">User.surname</span><span class="sxs-lookup"><span data-stu-id="eb876-177">user.surname</span></span> |
    | <span data-ttu-id="eb876-178">E-mailek</span><span class="sxs-lookup"><span data-stu-id="eb876-178">email</span></span>               | <span data-ttu-id="eb876-179">User.userPrincipalName</span><span class="sxs-lookup"><span data-stu-id="eb876-179">user.userprincipalname</span></span>|
    
    <span data-ttu-id="eb876-180">a.</span><span class="sxs-lookup"><span data-stu-id="eb876-180">a.</span></span> <span data-ttu-id="eb876-181">Kattintson a **Hozzáadás attribútum** tooopen hello **attribútum hozzáadása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="eb876-181">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-klue-tutorial/tutorial_attribute_04.png)

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-klue-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="eb876-184">b.</span><span class="sxs-lookup"><span data-stu-id="eb876-184">b.</span></span> <span data-ttu-id="eb876-185">A hello **neve** szövegmezőben, az adott sorhoz feltüntetett hello attribútum neve.</span><span class="sxs-lookup"><span data-stu-id="eb876-185">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>

    <span data-ttu-id="eb876-186">c.</span><span class="sxs-lookup"><span data-stu-id="eb876-186">c.</span></span> <span data-ttu-id="eb876-187">A hello **érték** listájában, hello attribútuma Típusérték az adott sorhoz feltüntetett.</span><span class="sxs-lookup"><span data-stu-id="eb876-187">From hello **Value** list, type hello attribute value shown for that row.</span></span>
    
    <span data-ttu-id="eb876-188">d.</span><span class="sxs-lookup"><span data-stu-id="eb876-188">d.</span></span> <span data-ttu-id="eb876-189">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="eb876-189">Click **Ok**.</span></span>

7. <span data-ttu-id="eb876-190">A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="eb876-190">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-klue-tutorial/tutorial_klue_certificate.png) 

8. <span data-ttu-id="eb876-192">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="eb876-192">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-klue-tutorial/tutorial_general_400.png)
    
9. <span data-ttu-id="eb876-194">A hello **Klue konfigurációs** kattintson **konfigurálása Klue** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="eb876-194">On hello **Klue Configuration** section, click **Configure Klue** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="eb876-195">Másolás hello **SAML Entitásazonosító és SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="eb876-195">Copy hello **SAML Entity ID and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-klue-tutorial/tutorial_klue_configure.png) 

10. <span data-ttu-id="eb876-197">tooconfigure egyszeri bejelentkezést a **Klue** oldalon kell letöltött toosend hello **Certificate(Base64), SAML-alapú egyszeri bejelentkezési URL-címe és SAML Entitásazonosító** túl[Klue támogatási csoport](mailto:support@klue.com).</span><span class="sxs-lookup"><span data-stu-id="eb876-197">tooconfigure single sign-on on **Klue** side, you need toosend hello downloaded **Certificate(Base64), SAML Single Sign-On Service URL, and SAML Entity ID** too[Klue support team](mailto:support@klue.com).</span></span>

> [!TIP]
> <span data-ttu-id="eb876-198">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="eb876-198">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="eb876-199">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="eb876-199">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="eb876-200">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="eb876-200">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="eb876-201">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="eb876-201">Creating an Azure AD test user</span></span>
<span data-ttu-id="eb876-202">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="eb876-202">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="eb876-204">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="eb876-204">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="eb876-205">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="eb876-205">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-klue-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="eb876-207">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="eb876-207">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-klue-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="eb876-209">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="eb876-209">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-klue-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="eb876-211">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="eb876-211">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-klue-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="eb876-213">a.</span><span class="sxs-lookup"><span data-stu-id="eb876-213">a.</span></span> <span data-ttu-id="eb876-214">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="eb876-214">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="eb876-215">b.</span><span class="sxs-lookup"><span data-stu-id="eb876-215">b.</span></span> <span data-ttu-id="eb876-216">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="eb876-216">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="eb876-217">c.</span><span class="sxs-lookup"><span data-stu-id="eb876-217">c.</span></span> <span data-ttu-id="eb876-218">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="eb876-218">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="eb876-219">d.</span><span class="sxs-lookup"><span data-stu-id="eb876-219">d.</span></span> <span data-ttu-id="eb876-220">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="eb876-220">Click **Create**.</span></span>
 
### <a name="creating-a-klue-test-user"></a><span data-ttu-id="eb876-221">Klue tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="eb876-221">Creating a Klue test user</span></span>

<span data-ttu-id="eb876-222">hello ebben a szakaszban célja toocreate Klue Britta Simon nevű felhasználó.</span><span class="sxs-lookup"><span data-stu-id="eb876-222">hello objective of this section is toocreate a user called Britta Simon in Klue.</span></span> <span data-ttu-id="eb876-223">Klue támogatja just-in-time kiosztást, amely alapértelmezés szerint van engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="eb876-223">Klue supports just-in-time provisioning, which is by default enabled.</span></span> <span data-ttu-id="eb876-224">Nincs ebben a szakaszban az Ön művelet elem.</span><span class="sxs-lookup"><span data-stu-id="eb876-224">There is no action item for you in this section.</span></span> <span data-ttu-id="eb876-225">Új felhasználó jön létre egy kísérlet tooaccess Klue során, ha még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="eb876-225">A new user is created during an attempt tooaccess Klue if it doesn't exist yet.</span></span>

>[!Note]
><span data-ttu-id="eb876-226">Ha a felhasználó manuálisan, forduljon a toocreate kell [Klue támogatási csoport](mailto:support@klue.com).</span><span class="sxs-lookup"><span data-stu-id="eb876-226">If you need toocreate a user manually, Contact [Klue support team](mailto:support@klue.com).</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="eb876-227">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="eb876-227">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="eb876-228">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooKlue megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="eb876-228">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooKlue.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="eb876-230">**tooassign Britta Simon tooKlue, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="eb876-230">**tooassign Britta Simon tooKlue, perform hello following steps:**</span></span>

1. <span data-ttu-id="eb876-231">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="eb876-231">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="eb876-233">Hello alkalmazások listában válassza ki a **Klue**.</span><span class="sxs-lookup"><span data-stu-id="eb876-233">In hello applications list, select **Klue**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-klue-tutorial/tutorial_klue_app.png) 

3. <span data-ttu-id="eb876-235">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="eb876-235">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="eb876-237">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="eb876-237">Click **Add** button.</span></span> <span data-ttu-id="eb876-238">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="eb876-238">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="eb876-240">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="eb876-240">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="eb876-241">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="eb876-241">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="eb876-242">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="eb876-242">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="eb876-243">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="eb876-243">Testing single sign-on</span></span>

<span data-ttu-id="eb876-244">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="eb876-244">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="eb876-245">Ha a hozzáférési Panel hello hello Klue csempe gombra kattint, automatikusan bejelentkezett tooyour Klue alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="eb876-245">When you click hello Klue tile in hello Access Panel, you should get automatically signed-on tooyour Klue application.</span></span>
<span data-ttu-id="eb876-246">A hozzáférési Panel kapcsolatos további információkért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="eb876-246">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="eb876-247">További források</span><span class="sxs-lookup"><span data-stu-id="eb876-247">Additional resources</span></span>

* [<span data-ttu-id="eb876-248">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="eb876-248">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="eb876-249">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="eb876-249">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-klue-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-klue-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-klue-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-klue-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-klue-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-klue-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-klue-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-klue-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-klue-tutorial/tutorial_general_203.png

