---
title: "Oktatóanyag: Azure Active Directoryval integrált Workpath |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Workpath között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 320b0daf-14be-4813-b59b-25a6a5070690
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: jeedes
ms.openlocfilehash: 69f443f314edb7c8c489a6c193e09b6f8fe6795a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-workpath"></a><span data-ttu-id="84b03-103">Oktatóanyag: Azure Active Directoryval integrált Workpath</span><span class="sxs-lookup"><span data-stu-id="84b03-103">Tutorial: Azure Active Directory integration with Workpath</span></span>

<span data-ttu-id="84b03-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Workpath az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="84b03-104">In this tutorial, you learn how toointegrate Workpath with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="84b03-105">Workpath integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="84b03-105">Integrating Workpath with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="84b03-106">Megadhatja a hozzáférés tooWorkpath rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="84b03-106">You can control in Azure AD who has access tooWorkpath</span></span>
- <span data-ttu-id="84b03-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooWorkpath (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="84b03-107">You can enable your users tooautomatically get signed-on tooWorkpath (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="84b03-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="84b03-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="84b03-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="84b03-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="84b03-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="84b03-110">Prerequisites</span></span>

<span data-ttu-id="84b03-111">az Azure AD integrálása Workpath tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="84b03-111">tooconfigure Azure AD integration with Workpath, you need hello following items:</span></span>

- <span data-ttu-id="84b03-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="84b03-112">An Azure AD subscription</span></span>
- <span data-ttu-id="84b03-113">Egy Workpath egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="84b03-113">A Workpath single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="84b03-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="84b03-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="84b03-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="84b03-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="84b03-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="84b03-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="84b03-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="84b03-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="84b03-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="84b03-118">Scenario description</span></span>
<span data-ttu-id="84b03-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="84b03-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="84b03-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="84b03-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="84b03-121">Hello gyűjteményből Workpath hozzáadása</span><span class="sxs-lookup"><span data-stu-id="84b03-121">Adding Workpath from hello gallery</span></span>
2. <span data-ttu-id="84b03-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="84b03-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-workpath-from-hello-gallery"></a><span data-ttu-id="84b03-123">Hello gyűjteményből Workpath hozzáadása</span><span class="sxs-lookup"><span data-stu-id="84b03-123">Adding Workpath from hello gallery</span></span>
<span data-ttu-id="84b03-124">tooconfigure hello integrációja Workpath az Azure AD-be, meg kell tooadd Workpath hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="84b03-124">tooconfigure hello integration of Workpath into Azure AD, you need tooadd Workpath from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="84b03-125">**tooadd Workpath hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="84b03-125">**tooadd Workpath from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="84b03-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="84b03-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="84b03-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="84b03-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="84b03-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="84b03-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="84b03-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="84b03-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="84b03-133">Hello keresési mezőbe, írja be a **Workpath**.</span><span class="sxs-lookup"><span data-stu-id="84b03-133">In hello search box, type **Workpath**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-workpath-tutorial/tutorial_workpath_search.png)

5. <span data-ttu-id="84b03-135">A hello eredmények panelen válassza ki a **Workpath**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="84b03-135">In hello results panel, select **Workpath**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-workpath-tutorial/tutorial_workpath_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="84b03-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="84b03-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="84b03-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján Workpath</span><span class="sxs-lookup"><span data-stu-id="84b03-138">In this section, you configure and test Azure AD single sign-on with Workpath based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="84b03-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Workpath tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="84b03-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Workpath is tooa user in Azure AD.</span></span> <span data-ttu-id="84b03-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Workpath közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="84b03-140">In other words, a link relationship between an Azure AD user and hello related user in Workpath needs toobe established.</span></span>

<span data-ttu-id="84b03-141">Workpath, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="84b03-141">In Workpath, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="84b03-142">tooconfigure és az Azure AD az egyszeri bejelentkezés Workpath-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="84b03-142">tooconfigure and test Azure AD single sign-on with Workpath, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="84b03-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="84b03-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="84b03-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="84b03-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="84b03-145">**[Workpath tesztfelhasználó létrehozása](#creating-a-workpath-test-user)**  -toohave egy megfelelője a Britta Simon a Workpath, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="84b03-145">**[Creating a Workpath test user](#creating-a-workpath-test-user)** - toohave a counterpart of Britta Simon in Workpath that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="84b03-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="84b03-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="84b03-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="84b03-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="84b03-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="84b03-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="84b03-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Workpath alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="84b03-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Workpath application.</span></span>

<span data-ttu-id="84b03-150">**az Azure AD tooconfigure egyszeri bejelentkezést a Workpath, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="84b03-150">**tooconfigure Azure AD single sign-on with Workpath, perform hello following steps:**</span></span>

1. <span data-ttu-id="84b03-151">Az Azure portál, a hello hello **Workpath** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="84b03-151">In hello Azure portal, on hello **Workpath** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="84b03-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="84b03-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-workpath-tutorial/tutorial_workpath_samlbase.png)

3. <span data-ttu-id="84b03-155">A hello **Workpath tartomány és az URL-címek** szakaszban, ha tooconfigure hello alkalmazás **IDP** kezdeményezett módban hajtsa végre a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="84b03-155">On hello **Workpath Domain and URLs** section, If you wish tooconfigure hello application in **IDP** initiated mode perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-workpath-tutorial/tutorial_workpath_url.png)

    <span data-ttu-id="84b03-157">a.</span><span class="sxs-lookup"><span data-stu-id="84b03-157">a.</span></span> <span data-ttu-id="84b03-158">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://api.workpath.com/v1/saml/metadata/<instancename>`</span><span class="sxs-lookup"><span data-stu-id="84b03-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://api.workpath.com/v1/saml/metadata/<instancename>`</span></span>

    <span data-ttu-id="84b03-159">b.</span><span class="sxs-lookup"><span data-stu-id="84b03-159">b.</span></span> <span data-ttu-id="84b03-160">A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://api.workpath.com/v1/saml/assert/<instancename>`</span><span class="sxs-lookup"><span data-stu-id="84b03-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://api.workpath.com/v1/saml/assert/<instancename>`</span></span>

4. <span data-ttu-id="84b03-161">Ellenőrizze **megjelenítése speciális URL-beállításainak**.</span><span class="sxs-lookup"><span data-stu-id="84b03-161">Check **Show advanced URL settings**.</span></span> <span data-ttu-id="84b03-162">Ha tooconfigure hello alkalmazás **SP** kezdeményezett mód, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="84b03-162">If you wish tooconfigure hello application in **SP** initiated mode, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-workpath-tutorial/tutorial_workpath_url1.png)

    <span data-ttu-id="84b03-164">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<subdomain>.workpath.com/ `</span><span class="sxs-lookup"><span data-stu-id="84b03-164">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<subdomain>.workpath.com/ `</span></span>

    > [!NOTE] 
    > <span data-ttu-id="84b03-165">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="84b03-165">These values are not real.</span></span> <span data-ttu-id="84b03-166">Frissítheti ezeket az értékeket a hello tényleges bejelentkezési URL-címet, a azonosítója és a válasz URL-CÍMEN.</span><span class="sxs-lookup"><span data-stu-id="84b03-166">Update these values with hello actual Sign-on URL, Identifier and Reply URL.</span></span> <span data-ttu-id="84b03-167">Ügyfél [Workpath támogatási csoport](https://help.workpath.com) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="84b03-167">Contact [Workpath support team](https://help.workpath.com) tooget these values.</span></span>

5. <span data-ttu-id="84b03-168">Workpath alkalmazás hello SAML helyességi feltételek vár egy meghatározott formátumban.</span><span class="sxs-lookup"><span data-stu-id="84b03-168">Workpath application expects hello SAML assertions in a specific format.</span></span> <span data-ttu-id="84b03-169">Az alkalmazás jogcímek a következő hello konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="84b03-169">Configure hello following claims for this application.</span></span> <span data-ttu-id="84b03-170">Ezek az attribútumok értékének hello hello kezelése "**felhasználói attribútumok**" szakasz alkalmazás integráció lapján.</span><span class="sxs-lookup"><span data-stu-id="84b03-170">You can manage hello values of these attributes from hello "**User Attributes**" section on application integration page.</span></span> <span data-ttu-id="84b03-171">a következő képernyőkép hello ehhez a konfigurációhoz példáját mutatja be.</span><span class="sxs-lookup"><span data-stu-id="84b03-171">hello following screenshot shows an example for this configuration.</span></span> 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-workpath-tutorial/tutorial_workpath_attributes.png)
    
6. <span data-ttu-id="84b03-173">A hello **felhasználói attribútumok** szakaszt, hello **egyszeri bejelentkezés** párbeszédpanelen konfigurálja a SAML-jogkivonat attribútum hello ábrának megfelelően, és hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="84b03-173">In hello **User Attributes** section on hello **Single sign-on** dialog, configure SAML token attribute as shown in hello image and perform hello following steps:</span></span>
    
    | <span data-ttu-id="84b03-174">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="84b03-174">Attribute Name</span></span> | <span data-ttu-id="84b03-175">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="84b03-175">Attribute Value</span></span> |
    | ------------------- | -------------------- |    
    | <span data-ttu-id="84b03-176">Utónév</span><span class="sxs-lookup"><span data-stu-id="84b03-176">first_name</span></span> | <span data-ttu-id="84b03-177">User.givenName</span><span class="sxs-lookup"><span data-stu-id="84b03-177">user.givenname</span></span> |
    | <span data-ttu-id="84b03-178">Vezetéknév</span><span class="sxs-lookup"><span data-stu-id="84b03-178">last_name</span></span> | <span data-ttu-id="84b03-179">User.surname</span><span class="sxs-lookup"><span data-stu-id="84b03-179">user.surname</span></span> |
    
    <span data-ttu-id="84b03-180">a.</span><span class="sxs-lookup"><span data-stu-id="84b03-180">a.</span></span> <span data-ttu-id="84b03-181">Kattintson a **Hozzáadás attribútum** tooopen hello **attribútum hozzáadása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="84b03-181">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-workpath-tutorial/tutorial_attribute_04.png)

    <span data-ttu-id="84b03-183">b.</span><span class="sxs-lookup"><span data-stu-id="84b03-183">b.</span></span> <span data-ttu-id="84b03-184">A hello **neve** szövegmezőben, az adott sorhoz feltüntetett hello attribútum neve.</span><span class="sxs-lookup"><span data-stu-id="84b03-184">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-workpath-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="84b03-186">c.</span><span class="sxs-lookup"><span data-stu-id="84b03-186">c.</span></span> <span data-ttu-id="84b03-187">A hello **érték** listájában, hello attribútuma Típusérték az adott sorhoz feltüntetett.</span><span class="sxs-lookup"><span data-stu-id="84b03-187">From hello **Value** list, type hello attribute value shown for that row.</span></span>

    <span data-ttu-id="84b03-188">d.</span><span class="sxs-lookup"><span data-stu-id="84b03-188">d.</span></span> <span data-ttu-id="84b03-189">Hagyja hello **Namespace** szövegbeviteli mező üres.</span><span class="sxs-lookup"><span data-stu-id="84b03-189">Leave hello **Namespace** textbox blank.</span></span>
    
    <span data-ttu-id="84b03-190">e.</span><span class="sxs-lookup"><span data-stu-id="84b03-190">e.</span></span> <span data-ttu-id="84b03-191">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="84b03-191">Click **Ok**.</span></span>
    

7. <span data-ttu-id="84b03-192">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="84b03-192">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-workpath-tutorial/tutorial_workpath_certificate.png) 

8. <span data-ttu-id="84b03-194">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="84b03-194">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-workpath-tutorial/tutorial_general_400.png)

9. <span data-ttu-id="84b03-196">A hello **Workpath konfigurációs** kattintson **konfigurálása Workpath** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="84b03-196">On hello **Workpath Configuration** section, click **Configure Workpath** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="84b03-197">Másolás hello **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="84b03-197">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-workpath-tutorial/tutorial_workpath_configure.png) 

10. <span data-ttu-id="84b03-199">tooconfigure egyszeri bejelentkezést a **Workpath** oldalon kell letöltött toosend hello **metaadatainak XML-kódja**, **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** túl[Workpath támogatási csoport](https://help.workpath.com).</span><span class="sxs-lookup"><span data-stu-id="84b03-199">tooconfigure single sign-on on **Workpath** side, you need toosend hello downloaded **Metadata XML**, **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** too[Workpath support team](https://help.workpath.com).</span></span> 

> [!TIP]
> <span data-ttu-id="84b03-200">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="84b03-200">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="84b03-201">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="84b03-201">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="84b03-202">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="84b03-202">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="84b03-203">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="84b03-203">Creating an Azure AD test user</span></span>
<span data-ttu-id="84b03-204">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="84b03-204">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="84b03-206">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="84b03-206">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="84b03-207">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="84b03-207">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-workpath-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="84b03-209">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="84b03-209">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-workpath-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="84b03-211">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="84b03-211">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-workpath-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="84b03-213">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="84b03-213">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-workpath-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="84b03-215">a.</span><span class="sxs-lookup"><span data-stu-id="84b03-215">a.</span></span> <span data-ttu-id="84b03-216">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="84b03-216">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="84b03-217">b.</span><span class="sxs-lookup"><span data-stu-id="84b03-217">b.</span></span> <span data-ttu-id="84b03-218">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="84b03-218">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="84b03-219">c.</span><span class="sxs-lookup"><span data-stu-id="84b03-219">c.</span></span> <span data-ttu-id="84b03-220">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="84b03-220">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="84b03-221">d.</span><span class="sxs-lookup"><span data-stu-id="84b03-221">d.</span></span> <span data-ttu-id="84b03-222">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="84b03-222">Click **Create**.</span></span>
 
### <a name="creating-a-workpath-test-user"></a><span data-ttu-id="84b03-223">Workpath tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="84b03-223">Creating a Workpath test user</span></span>

<span data-ttu-id="84b03-224">Workpath csak az idő a felhasználók átadása támogatja.</span><span class="sxs-lookup"><span data-stu-id="84b03-224">Workpath supports Just in time user provisioning.</span></span> <span data-ttu-id="84b03-225">A hitelesítés után felhasználók hello alkalmazás automatikusan létrejönnek.</span><span class="sxs-lookup"><span data-stu-id="84b03-225">After authentication users are created in hello application automatically.</span></span> 


### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="84b03-226">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="84b03-226">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="84b03-227">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooWorkpath megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="84b03-227">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooWorkpath.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="84b03-229">**tooassign Britta Simon tooWorkpath, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="84b03-229">**tooassign Britta Simon tooWorkpath, perform hello following steps:**</span></span>

1. <span data-ttu-id="84b03-230">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="84b03-230">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="84b03-232">Hello alkalmazások listában válassza ki a **Workpath**.</span><span class="sxs-lookup"><span data-stu-id="84b03-232">In hello applications list, select **Workpath**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-workpath-tutorial/tutorial_workpath_app.png) 

3. <span data-ttu-id="84b03-234">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="84b03-234">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="84b03-236">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="84b03-236">Click **Add** button.</span></span> <span data-ttu-id="84b03-237">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="84b03-237">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="84b03-239">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="84b03-239">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="84b03-240">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="84b03-240">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="84b03-241">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="84b03-241">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="84b03-242">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="84b03-242">Testing single sign-on</span></span>

<span data-ttu-id="84b03-243">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="84b03-243">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="84b03-244">Ha a hozzáférési Panel hello hello Workpath csempe gombra kattint, automatikusan bejelentkezett tooyour Workpath alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="84b03-244">When you click hello Workpath tile in hello Access Panel, you should get automatically signed-on tooyour Workpath application.</span></span>
<span data-ttu-id="84b03-245">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="84b03-245">For more information about the Access Panel, see [introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="84b03-246">További források</span><span class="sxs-lookup"><span data-stu-id="84b03-246">Additional resources</span></span>

* [<span data-ttu-id="84b03-247">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="84b03-247">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="84b03-248">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="84b03-248">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-workpath-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-workpath-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-workpath-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-workpath-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-workpath-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-workpath-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-workpath-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-workpath-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-workpath-tutorial/tutorial_general_203.png

