---
title: "Oktatóanyag: Azure Active Directoryval integrált Boomi |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Boomi között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 8e05afa9-2eda-4975-a0cc-6d408065860f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: ce64a4561697d311a8c7b1b244315bb552c5cfb7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-boomi"></a><span data-ttu-id="c8903-103">Oktatóanyag: Azure Active Directoryval integrált Boomi</span><span class="sxs-lookup"><span data-stu-id="c8903-103">Tutorial: Azure Active Directory integration with Boomi</span></span>

<span data-ttu-id="c8903-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Boomi az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="c8903-104">In this tutorial, you learn how toointegrate Boomi with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="c8903-105">Boomi integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="c8903-105">Integrating Boomi with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="c8903-106">Megadhatja a hozzáférés tooBoomi rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="c8903-106">You can control in Azure AD who has access tooBoomi</span></span>
- <span data-ttu-id="c8903-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooBoomi (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="c8903-107">You can enable your users tooautomatically get signed-on tooBoomi (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="c8903-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="c8903-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="c8903-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="c8903-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c8903-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="c8903-110">Prerequisites</span></span>

<span data-ttu-id="c8903-111">az Azure AD integrálása Boomi tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="c8903-111">tooconfigure Azure AD integration with Boomi, you need hello following items:</span></span>

- <span data-ttu-id="c8903-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="c8903-112">An Azure AD subscription</span></span>
- <span data-ttu-id="c8903-113">Egy Boomi egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="c8903-113">A Boomi single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="c8903-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="c8903-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="c8903-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="c8903-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="c8903-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="c8903-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="c8903-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c8903-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="c8903-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="c8903-118">Scenario description</span></span>
<span data-ttu-id="c8903-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="c8903-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="c8903-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="c8903-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="c8903-121">Hello gyűjteményből Boomi hozzáadása</span><span class="sxs-lookup"><span data-stu-id="c8903-121">Adding Boomi from hello gallery</span></span>
2. <span data-ttu-id="c8903-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="c8903-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-boomi-from-hello-gallery"></a><span data-ttu-id="c8903-123">Hello gyűjteményből Boomi hozzáadása</span><span class="sxs-lookup"><span data-stu-id="c8903-123">Adding Boomi from hello gallery</span></span>
<span data-ttu-id="c8903-124">tooconfigure hello integrációja Boomi az Azure AD-be, meg kell tooadd Boomi hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="c8903-124">tooconfigure hello integration of Boomi into Azure AD, you need tooadd Boomi from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="c8903-125">**tooadd Boomi hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="c8903-125">**tooadd Boomi from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="c8903-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="c8903-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="c8903-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="c8903-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="c8903-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="c8903-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="c8903-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="c8903-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="c8903-133">Hello keresési mezőbe, írja be a **Boomi**.</span><span class="sxs-lookup"><span data-stu-id="c8903-133">In hello search box, type **Boomi**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_search.png)

5. <span data-ttu-id="c8903-135">A hello eredmények panelen válassza ki a **Boomi**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="c8903-135">In hello results panel, select **Boomi**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="c8903-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="c8903-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="c8903-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Boomi.</span><span class="sxs-lookup"><span data-stu-id="c8903-138">In this section, you configure and test Azure AD single sign-on with Boomi based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="c8903-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Boomi tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="c8903-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Boomi is tooa user in Azure AD.</span></span> <span data-ttu-id="c8903-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Boomi közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="c8903-140">In other words, a link relationship between an Azure AD user and hello related user in Boomi needs toobe established.</span></span>

<span data-ttu-id="c8903-141">Boomi, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="c8903-141">In Boomi, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="c8903-142">tooconfigure és az Azure AD az egyszeri bejelentkezés Boomi-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="c8903-142">tooconfigure and test Azure AD single sign-on with Boomi, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="c8903-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="c8903-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="c8903-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c8903-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="c8903-145">**[Boomi tesztfelhasználó létrehozása](#creating-a-boomi-test-user)**  -toohave egy megfelelője a Britta Simon a Boomi, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="c8903-145">**[Creating a Boomi test user](#creating-a-boomi-test-user)** - toohave a counterpart of Britta Simon in Boomi that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="c8903-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="c8903-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="c8903-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="c8903-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="c8903-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c8903-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="c8903-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Boomi alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="c8903-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Boomi application.</span></span>

<span data-ttu-id="c8903-150">**az Azure AD tooconfigure egyszeri bejelentkezést a Boomi, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="c8903-150">**tooconfigure Azure AD single sign-on with Boomi, perform hello following steps:**</span></span>

1. <span data-ttu-id="c8903-151">Az Azure portál, a hello hello **Boomi** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="c8903-151">In hello Azure portal, on hello **Boomi** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="c8903-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="c8903-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_samlbase.png)

3. <span data-ttu-id="c8903-155">A hello **Boomi tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="c8903-155">On hello **Boomi Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_url.png)

    <span data-ttu-id="c8903-157">a.</span><span class="sxs-lookup"><span data-stu-id="c8903-157">a.</span></span> <span data-ttu-id="c8903-158">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://platform.boomi.com/sso/<accountname>/saml`</span><span class="sxs-lookup"><span data-stu-id="c8903-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://platform.boomi.com/sso/<accountname>/saml`</span></span>

    <span data-ttu-id="c8903-159">b.</span><span class="sxs-lookup"><span data-stu-id="c8903-159">b.</span></span> <span data-ttu-id="c8903-160">A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://platform.boomi.com/sso/<accountname>/saml`</span><span class="sxs-lookup"><span data-stu-id="c8903-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://platform.boomi.com/sso/<accountname>/saml`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="c8903-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="c8903-161">These values are not real.</span></span> <span data-ttu-id="c8903-162">Frissítheti ezeket az értékeket hello tényleges azonosítója és a válasz URL-címmel.</span><span class="sxs-lookup"><span data-stu-id="c8903-162">Update these values with hello actual Identifier and Reply URL.</span></span> <span data-ttu-id="c8903-163">Ügyfél [Boomi támogatási csoport](https://boomi.com/company/contact/) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="c8903-163">Contact [Boomi support team](https://boomi.com/company/contact/) tooget these values.</span></span>

4. <span data-ttu-id="c8903-164">A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="c8903-164">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_certificate.png)

4. <span data-ttu-id="c8903-166">Boomi alkalmazás hello SAML helyességi feltételek vár egy meghatározott formátumban.</span><span class="sxs-lookup"><span data-stu-id="c8903-166">Boomi application expects hello SAML assertions in a specific format.</span></span> <span data-ttu-id="c8903-167">Állítsa be az alkalmazás jogcímek a következő hello.</span><span class="sxs-lookup"><span data-stu-id="c8903-167">Please configure hello following claims for this application.</span></span> <span data-ttu-id="c8903-168">Ezek az attribútumok értékének hello hello kezelése "**felhasználói attribútumok**" szakasz alkalmazás integráció lapján.</span><span class="sxs-lookup"><span data-stu-id="c8903-168">You can manage hello values of these attributes from hello "**User Attributes**" section on application integration page.</span></span> <span data-ttu-id="c8903-169">a következő képernyőkép hello ezen mutat egy példát.</span><span class="sxs-lookup"><span data-stu-id="c8903-169">hello following screenshot shows an example for this.</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-boomi-tutorial/tutorial_attribute.png)

5. <span data-ttu-id="c8903-171">A hello **felhasználói attribútumok** szakaszt, hello **egyszeri bejelentkezés** párbeszédpanel, az alábbi, hello táblázatban szereplő minden egyes sorára hajtsa végre a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="c8903-171">In hello **User Attributes** section on hello **Single sign-on** dialog, for each row shown in hello table below, perform hello following steps:</span></span>

    | <span data-ttu-id="c8903-172">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="c8903-172">Attribute Name</span></span> | <span data-ttu-id="c8903-173">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="c8903-173">Attribute Value</span></span> |
    | -------------- | --------------- |
    | <span data-ttu-id="c8903-174">FEDERATION_ID</span><span class="sxs-lookup"><span data-stu-id="c8903-174">FEDERATION_ID</span></span> | <span data-ttu-id="c8903-175">User.mail</span><span class="sxs-lookup"><span data-stu-id="c8903-175">user.mail</span></span> |
    
    <span data-ttu-id="c8903-176">a.</span><span class="sxs-lookup"><span data-stu-id="c8903-176">a.</span></span> <span data-ttu-id="c8903-177">Kattintson a **Hozzáadás attribútum** tooopen hello **attribútum hozzáadása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c8903-177">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-boomi-tutorial/tutorial_attribute_04.png)
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-boomi-tutorial/tutorial_attribute_05.png)
    
    <span data-ttu-id="c8903-180">b.</span><span class="sxs-lookup"><span data-stu-id="c8903-180">b.</span></span> <span data-ttu-id="c8903-181">A hello **neve** szövegmezőben, az adott sorhoz feltüntetett hello attribútum neve.</span><span class="sxs-lookup"><span data-stu-id="c8903-181">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>
    
    <span data-ttu-id="c8903-182">c.</span><span class="sxs-lookup"><span data-stu-id="c8903-182">c.</span></span> <span data-ttu-id="c8903-183">A hello **érték** listájában, hello attribútuma Típusérték az adott sorhoz feltüntetett.</span><span class="sxs-lookup"><span data-stu-id="c8903-183">From hello **Value** list, type hello attribute value shown for that row.</span></span>
    
    <span data-ttu-id="c8903-184">d.</span><span class="sxs-lookup"><span data-stu-id="c8903-184">d.</span></span> <span data-ttu-id="c8903-185">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="c8903-185">Click **Ok**.</span></span>

6. <span data-ttu-id="c8903-186">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="c8903-186">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-boomi-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="c8903-188">A hello **Boomi konfigurációs** kattintson **konfigurálása Boomi** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="c8903-188">On hello **Boomi Configuration** section, click **Configure Boomi** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="c8903-189">Másolás hello **SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="c8903-189">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_configure.png) 

8. <span data-ttu-id="c8903-191">Egy másik webes böngészőablakban jelentkezzen be a Boomi vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="c8903-191">In a different web browser window, log into your Boomi company site as an administrator.</span></span> 

9. <span data-ttu-id="c8903-192">Keresse meg a túl**vállalatnév** , és túl**beállítása**.</span><span class="sxs-lookup"><span data-stu-id="c8903-192">Navigate too**Company Name** and go too**Set up**.</span></span>

10. <span data-ttu-id="c8903-193">Kattintson a hello **egyszeri bejelentkezési beállítások** lapra, és hajtsa végre a következő lépések.</span><span class="sxs-lookup"><span data-stu-id="c8903-193">Click hello **SSO Options** tab and perform below steps.</span></span>

    ![Alkalmazás ügyféloldali egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_11.png)

    <span data-ttu-id="c8903-195">a.</span><span class="sxs-lookup"><span data-stu-id="c8903-195">a.</span></span> <span data-ttu-id="c8903-196">Ellenőrizze **engedélyezése SAML-alapú egyszeri bejelentkezést** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="c8903-196">Check **Enable SAML Single Sign-On** checkbox.</span></span>

    <span data-ttu-id="c8903-197">b.</span><span class="sxs-lookup"><span data-stu-id="c8903-197">b.</span></span> <span data-ttu-id="c8903-198">Kattintson a **importálási** tooupload hello letöltött tanúsítvány az Azure AD túl**szolgáltató Identitástanúsítvány**.</span><span class="sxs-lookup"><span data-stu-id="c8903-198">Click **Import** tooupload hello downloaded certificate from Azure AD too**Identity Provider Certificate**.</span></span>
    
    <span data-ttu-id="c8903-199">c.</span><span class="sxs-lookup"><span data-stu-id="c8903-199">c.</span></span> <span data-ttu-id="c8903-200">A hello **Identity Provider bejelentkezési URL-cím** szövegmező, írja be a hello értéket **SAML-alapú egyszeri bejelentkezési URL-címe** az Azure AD alkalmazás-konfigurációs ablakban.</span><span class="sxs-lookup"><span data-stu-id="c8903-200">In hello **Identity Provider Login URL** textbox, put hello value of **SAML Single Sign-On Service URL** from Azure AD application configuration window.</span></span>

    <span data-ttu-id="c8903-201">d.</span><span class="sxs-lookup"><span data-stu-id="c8903-201">d.</span></span> <span data-ttu-id="c8903-202">Mint **összevonási azonosító hely**, jelölje be **összevonási azonosító FEDERATION_ID attribútum elem van** választógombot.</span><span class="sxs-lookup"><span data-stu-id="c8903-202">As **Federation Id Location**, select **Federation Id is in FEDERATION_ID Attribute element** radio button.</span></span> 

    <span data-ttu-id="c8903-203">e.</span><span class="sxs-lookup"><span data-stu-id="c8903-203">e.</span></span> <span data-ttu-id="c8903-204">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="c8903-204">Click **Save** button.</span></span>

> [!TIP]
> <span data-ttu-id="c8903-205">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="c8903-205">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="c8903-206">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="c8903-206">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="c8903-207">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="c8903-207">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="c8903-208">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="c8903-208">Creating an Azure AD test user</span></span>
<span data-ttu-id="c8903-209">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="c8903-209">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="c8903-211">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="c8903-211">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="c8903-212">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="c8903-212">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-boomi-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="c8903-214">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="c8903-214">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-boomi-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="c8903-216">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c8903-216">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-boomi-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="c8903-218">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="c8903-218">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-boomi-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="c8903-220">a.</span><span class="sxs-lookup"><span data-stu-id="c8903-220">a.</span></span> <span data-ttu-id="c8903-221">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="c8903-221">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="c8903-222">b.</span><span class="sxs-lookup"><span data-stu-id="c8903-222">b.</span></span> <span data-ttu-id="c8903-223">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="c8903-223">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="c8903-224">c.</span><span class="sxs-lookup"><span data-stu-id="c8903-224">c.</span></span> <span data-ttu-id="c8903-225">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="c8903-225">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="c8903-226">d.</span><span class="sxs-lookup"><span data-stu-id="c8903-226">d.</span></span> <span data-ttu-id="c8903-227">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="c8903-227">Click **Create**.</span></span>
 
### <a name="creating-a-boomi-test-user"></a><span data-ttu-id="c8903-228">Boomi tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="c8903-228">Creating a Boomi test user</span></span>

<span data-ttu-id="c8903-229">A sorrend tooenable az Azure AD felhasználók toolog a tooBoomi azok ki kell építenie Boomi be.</span><span class="sxs-lookup"><span data-stu-id="c8903-229">In order tooenable Azure AD users toolog in tooBoomi, they must be provisioned into Boomi.</span></span> <span data-ttu-id="c8903-230">Boomi hello esetben egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="c8903-230">In hello case of Boomi, provisioning is a manual task.</span></span>

### <a name="tooprovision-a-user-account-perform-hello-following-steps"></a><span data-ttu-id="c8903-231">tooprovision egy felhasználói fiókot, hajtsa végre a következő lépéseket hello:</span><span class="sxs-lookup"><span data-stu-id="c8903-231">tooprovision a user account, perform hello following steps:</span></span>

1. <span data-ttu-id="c8903-232">Jelentkezzen be tooyour Boomi vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="c8903-232">Log in tooyour Boomi company site as an administrator.</span></span>

2. <span data-ttu-id="c8903-233">A bejelentkezés után lépjen túl**felhasználókezelés** , és túl**felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="c8903-233">After logging in, navigate too**User Management** and go too**Users**.</span></span>

    <span data-ttu-id="c8903-234">![Felhasználók](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_001.png "felhasználók")</span><span class="sxs-lookup"><span data-stu-id="c8903-234">![Users](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_001.png "Users")</span></span>

3. <span data-ttu-id="c8903-235">Kattintson a  **+**  ikon és hello **Add/karbantartása felhasználói szerepkörök** párbeszédpanel nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="c8903-235">Click **+**  icon and hello **Add/Maintain User Roles** dialog opens.</span></span>

    <span data-ttu-id="c8903-236">![Felhasználók](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_002.png "felhasználók")</span><span class="sxs-lookup"><span data-stu-id="c8903-236">![Users](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_002.png "Users")</span></span>

    <span data-ttu-id="c8903-237">![Felhasználók](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_003.png "felhasználók")</span><span class="sxs-lookup"><span data-stu-id="c8903-237">![Users](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_003.png "Users")</span></span>

    <span data-ttu-id="c8903-238">a.</span><span class="sxs-lookup"><span data-stu-id="c8903-238">a.</span></span> <span data-ttu-id="c8903-239">A hello **felhasználó e-mail címe** szövegmezőhöz: hello e-mail felhasználó például BrittaSimon@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="c8903-239">In hello **User e-mail address** textbox, type hello email of user like BrittaSimon@contoso.com.</span></span>
    
    <span data-ttu-id="c8903-240">b.</span><span class="sxs-lookup"><span data-stu-id="c8903-240">b.</span></span> <span data-ttu-id="c8903-241">A hello **Keresztnév** szövegmező, például Britta felhasználói hello első nevét.</span><span class="sxs-lookup"><span data-stu-id="c8903-241">In hello **First name** textbox, type hello First name of user like Britta.</span></span>

    <span data-ttu-id="c8903-242">c.</span><span class="sxs-lookup"><span data-stu-id="c8903-242">c.</span></span> <span data-ttu-id="c8903-243">A hello **Vezetéknév** szövegmezőhöz hello utolsó típusnév Simon például felhasználó.</span><span class="sxs-lookup"><span data-stu-id="c8903-243">In hello **Last name** textbox, type hello Last name of user like Simon.</span></span>
    
    <span data-ttu-id="c8903-244">d.</span><span class="sxs-lookup"><span data-stu-id="c8903-244">d.</span></span> <span data-ttu-id="c8903-245">Adja meg hello felhasználó **összevonási azonosító**.</span><span class="sxs-lookup"><span data-stu-id="c8903-245">Enter hello user's **Federation ID**.</span></span> <span data-ttu-id="c8903-246">Minden felhasználónak rendelkeznie kell egy összevonási azonosító, amely egyedileg azonosítja a hello felhasználói hello fiókon belül.</span><span class="sxs-lookup"><span data-stu-id="c8903-246">Each user must have a Federation ID that uniquely identifies hello user within hello account.</span></span>
    
    <span data-ttu-id="c8903-247">e.</span><span class="sxs-lookup"><span data-stu-id="c8903-247">e.</span></span> <span data-ttu-id="c8903-248">Rendelje hozzá a hello **általános jogú felhasználó** toohello felhasználói szerepkör.</span><span class="sxs-lookup"><span data-stu-id="c8903-248">Assign hello **Standard User** role toohello user.</span></span> <span data-ttu-id="c8903-249">Ne rendeljen hello rendszergazdai szerepkör, mert, amely ad neki normál légkör hozzáférés, valamint az egyszeri bejelentkezéses hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="c8903-249">Do not assign hello Administrator role because that would give him normal Atmosphere access as well as single sign-on access.</span></span>
    
    <span data-ttu-id="c8903-250">f.</span><span class="sxs-lookup"><span data-stu-id="c8903-250">f.</span></span> <span data-ttu-id="c8903-251">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="c8903-251">Click **OK**.</span></span>
    
    > [!NOTE]
    > <span data-ttu-id="c8903-252">hello felhasználó nem kap meg az üdvözlő e-mailben értesítést tartalmazó egy jelszót, amely a toohello AtomSphere fiókhoz használt toolog lehet, mivel a hello identitásszolgáltató kezeli a jelszavát.</span><span class="sxs-lookup"><span data-stu-id="c8903-252">hello user will not receive a welcome notification email containing a password that can be used toolog in toohello AtomSphere account because his password is managed through hello identity provider.</span></span> <span data-ttu-id="c8903-253">Bármely más Boomi felhasználói fiók létrehozása eszközt, vagy Boomi tooprovision által nyújtott API-k AAD felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="c8903-253">You may use any other Boomi user account creation tools or APIs provided by Boomi tooprovision AAD user accounts.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="c8903-254">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="c8903-254">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="c8903-255">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooBoomi megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="c8903-255">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooBoomi.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="c8903-257">**tooassign Britta Simon tooBoomi, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="c8903-257">**tooassign Britta Simon tooBoomi, perform hello following steps:**</span></span>

1. <span data-ttu-id="c8903-258">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="c8903-258">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="c8903-260">Hello alkalmazások listában válassza ki a **Boomi**.</span><span class="sxs-lookup"><span data-stu-id="c8903-260">In hello applications list, select **Boomi**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_app.png) 

3. <span data-ttu-id="c8903-262">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="c8903-262">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="c8903-264">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="c8903-264">Click **Add** button.</span></span> <span data-ttu-id="c8903-265">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c8903-265">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="c8903-267">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="c8903-267">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="c8903-268">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c8903-268">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="c8903-269">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c8903-269">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="c8903-270">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="c8903-270">Testing single sign-on</span></span>

<span data-ttu-id="c8903-271">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="c8903-271">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="c8903-272">Ha a hozzáférési Panel hello hello Boomi csempe gombra kattint, automatikusan bejelentkezett tooyour Boomi alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="c8903-272">When you click hello Boomi tile in hello Access Panel, you should get automatically signed-on tooyour Boomi application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c8903-273">További források</span><span class="sxs-lookup"><span data-stu-id="c8903-273">Additional resources</span></span>

* [<span data-ttu-id="c8903-274">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="c8903-274">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="c8903-275">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="c8903-275">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_203.png

