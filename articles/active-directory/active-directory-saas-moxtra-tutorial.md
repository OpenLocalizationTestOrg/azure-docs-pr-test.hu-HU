---
title: "Oktatóanyag: Azure Active Directoryval integrált Moxtra |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Moxtra között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 2aed2d4b-1dcd-4839-8fed-9419d107c61c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: jeedes
ms.openlocfilehash: 82e2fcc390ba508e86a3992ec1c81d0a0ffed96b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-moxtra"></a><span data-ttu-id="acf24-103">Oktatóanyag: Azure Active Directoryval integrált Moxtra</span><span class="sxs-lookup"><span data-stu-id="acf24-103">Tutorial: Azure Active Directory integration with Moxtra</span></span>

<span data-ttu-id="acf24-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Moxtra az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="acf24-104">In this tutorial, you learn how toointegrate Moxtra with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="acf24-105">Moxtra integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="acf24-105">Integrating Moxtra with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="acf24-106">Megadhatja a hozzáférés tooMoxtra rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="acf24-106">You can control in Azure AD who has access tooMoxtra</span></span>
- <span data-ttu-id="acf24-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooMoxtra (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="acf24-107">You can enable your users tooautomatically get signed-on tooMoxtra (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="acf24-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="acf24-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="acf24-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="acf24-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="acf24-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="acf24-110">Prerequisites</span></span>

<span data-ttu-id="acf24-111">az Azure AD integrálása Moxtra tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="acf24-111">tooconfigure Azure AD integration with Moxtra, you need hello following items:</span></span>

- <span data-ttu-id="acf24-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="acf24-112">An Azure AD subscription</span></span>
- <span data-ttu-id="acf24-113">Egy Moxtra egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="acf24-113">A Moxtra single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="acf24-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="acf24-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="acf24-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="acf24-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="acf24-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="acf24-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="acf24-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="acf24-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="acf24-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="acf24-118">Scenario description</span></span>
<span data-ttu-id="acf24-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="acf24-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="acf24-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="acf24-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="acf24-121">Hello gyűjteményből Moxtra hozzáadása</span><span class="sxs-lookup"><span data-stu-id="acf24-121">Adding Moxtra from hello gallery</span></span>
2. <span data-ttu-id="acf24-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="acf24-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-moxtra-from-hello-gallery"></a><span data-ttu-id="acf24-123">Hello gyűjteményből Moxtra hozzáadása</span><span class="sxs-lookup"><span data-stu-id="acf24-123">Adding Moxtra from hello gallery</span></span>
<span data-ttu-id="acf24-124">tooconfigure hello integrációja Moxtra az Azure AD-be, meg kell tooadd Moxtra hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="acf24-124">tooconfigure hello integration of Moxtra into Azure AD, you need tooadd Moxtra from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="acf24-125">**tooadd Moxtra hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="acf24-125">**tooadd Moxtra from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="acf24-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="acf24-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="acf24-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="acf24-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="acf24-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="acf24-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="acf24-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="acf24-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="acf24-133">Hello keresési mezőbe, írja be a **Moxtra**.</span><span class="sxs-lookup"><span data-stu-id="acf24-133">In hello search box, type **Moxtra**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_search.png)

5. <span data-ttu-id="acf24-135">A hello eredmények panelen válassza ki a **Moxtra**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="acf24-135">In hello results panel, select **Moxtra**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="acf24-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="acf24-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="acf24-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Moxtra.</span><span class="sxs-lookup"><span data-stu-id="acf24-138">In this section, you configure and test Azure AD single sign-on with Moxtra based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="acf24-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Moxtra tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="acf24-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Moxtra is tooa user in Azure AD.</span></span> <span data-ttu-id="acf24-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Moxtra közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="acf24-140">In other words, a link relationship between an Azure AD user and hello related user in Moxtra needs toobe established.</span></span>

<span data-ttu-id="acf24-141">Moxtra, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="acf24-141">In Moxtra, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="acf24-142">tooconfigure és az Azure AD az egyszeri bejelentkezés Moxtra-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="acf24-142">tooconfigure and test Azure AD single sign-on with Moxtra, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="acf24-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="acf24-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="acf24-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="acf24-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="acf24-145">**[Moxtra tesztfelhasználó létrehozása](#creating-a-moxtra-test-user)**  -toohave egy megfelelője a Britta Simon a Moxtra, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="acf24-145">**[Creating a Moxtra test user](#creating-a-moxtra-test-user)** - toohave a counterpart of Britta Simon in Moxtra that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="acf24-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="acf24-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="acf24-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="acf24-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="acf24-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="acf24-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="acf24-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Moxtra alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="acf24-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Moxtra application.</span></span>

<span data-ttu-id="acf24-150">**az Azure AD tooconfigure egyszeri bejelentkezést a Moxtra, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="acf24-150">**tooconfigure Azure AD single sign-on with Moxtra, perform hello following steps:**</span></span>

1. <span data-ttu-id="acf24-151">Az Azure portál, a hello hello **Moxtra** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="acf24-151">In hello Azure portal, on hello **Moxtra** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="acf24-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="acf24-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_samlbase.png)

3. <span data-ttu-id="acf24-155">A hello **Moxtra tartomány és az URL-címek** csoportjában hajtsa végre a következő lépés hello:</span><span class="sxs-lookup"><span data-stu-id="acf24-155">On hello **Moxtra Domain and URLs** section, perform hello following step:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_url.png)

    <span data-ttu-id="acf24-157">A hello **bejelentkezési URL-cím** szövegmező, adja meg az URL-címet:`https://www.moxtra.com/service/#login`</span><span class="sxs-lookup"><span data-stu-id="acf24-157">In hello **Sign-on URL** textbox, type a URL as: `https://www.moxtra.com/service/#login`</span></span>

4. <span data-ttu-id="acf24-158">Moxtra alkalmazás hello SAML helyességi feltételek vár egy meghatározott formátumban.</span><span class="sxs-lookup"><span data-stu-id="acf24-158">Moxtra application expects hello SAML assertions in a specific format.</span></span> <span data-ttu-id="acf24-159">Az alkalmazás jogcímek a következő hello konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="acf24-159">Configure hello following claims for this application.</span></span> <span data-ttu-id="acf24-160">Ezek az attribútumok értékének hello hello kezelése "**felhasználói attribútumok**" szakasz alkalmazás integráció lapján.</span><span class="sxs-lookup"><span data-stu-id="acf24-160">You can manage hello values of these attributes from hello "**User Attributes**" section on application integration page.</span></span> <span data-ttu-id="acf24-161">a következő képernyőkép hello ehhez a konfigurációhoz példáját mutatja be.</span><span class="sxs-lookup"><span data-stu-id="acf24-161">hello following screenshot shows an example for this configuration.</span></span> 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_attributes.png)
    
5. <span data-ttu-id="acf24-163">A hello **felhasználói attribútumok** szakaszt, hello **egyszeri bejelentkezés** párbeszédpanelen konfigurálja a SAML-jogkivonat attribútum hello ábrának megfelelően, és hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="acf24-163">In hello **User Attributes** section on hello **Single sign-on** dialog, configure SAML token attribute as shown in hello image and perform hello following steps:</span></span>
    
    | <span data-ttu-id="acf24-164">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="acf24-164">Attribute Name</span></span> | <span data-ttu-id="acf24-165">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="acf24-165">Attribute Value</span></span> |
    | ------------------- | -------------------- |    
    | <span data-ttu-id="acf24-166">Utónév</span><span class="sxs-lookup"><span data-stu-id="acf24-166">firstname</span></span> | <span data-ttu-id="acf24-167">User.givenName</span><span class="sxs-lookup"><span data-stu-id="acf24-167">user.givenname</span></span> |
    | <span data-ttu-id="acf24-168">Vezetéknév</span><span class="sxs-lookup"><span data-stu-id="acf24-168">lastname</span></span> | <span data-ttu-id="acf24-169">User.surname</span><span class="sxs-lookup"><span data-stu-id="acf24-169">user.surname</span></span> |
    | <span data-ttu-id="acf24-170">idpid</span><span class="sxs-lookup"><span data-stu-id="acf24-170">idpid</span></span>    | <span data-ttu-id="acf24-171">< SAML entitás azonosítója ></span><span class="sxs-lookup"><span data-stu-id="acf24-171">< SAML Entity ID ></span></span> 

    > [!Note]
    > <span data-ttu-id="acf24-172">hello értékének **idpid** attribútum nincs valós.</span><span class="sxs-lookup"><span data-stu-id="acf24-172">hello value of **idpid** attribute is not real.</span></span> <span data-ttu-id="acf24-173">Kaphat a tényleges érték hello **rövid összefoglaló** szakaszában **Moxtra konfigurációs**.</span><span class="sxs-lookup"><span data-stu-id="acf24-173">You can get hello actual value from **Quick reference** section under **Moxtra Configuration**.</span></span>
    
    <span data-ttu-id="acf24-174">a.</span><span class="sxs-lookup"><span data-stu-id="acf24-174">a.</span></span> <span data-ttu-id="acf24-175">Kattintson a **Hozzáadás attribútum** tooopen hello **attribútum hozzáadása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="acf24-175">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-moxtra-tutorial/tutorial_attribute_04.png)

    <span data-ttu-id="acf24-177">b.</span><span class="sxs-lookup"><span data-stu-id="acf24-177">b.</span></span> <span data-ttu-id="acf24-178">A hello **neve** szövegmezőben, az adott sorhoz feltüntetett hello attribútum neve.</span><span class="sxs-lookup"><span data-stu-id="acf24-178">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-moxtra-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="acf24-180">c.</span><span class="sxs-lookup"><span data-stu-id="acf24-180">c.</span></span> <span data-ttu-id="acf24-181">A hello **érték** listájában, hello attribútuma Típusérték az adott sorhoz feltüntetett.</span><span class="sxs-lookup"><span data-stu-id="acf24-181">From hello **Value** list, type hello attribute value shown for that row.</span></span>

    <span data-ttu-id="acf24-182">d.</span><span class="sxs-lookup"><span data-stu-id="acf24-182">d.</span></span> <span data-ttu-id="acf24-183">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="acf24-183">Click **Ok**.</span></span>
    
5. <span data-ttu-id="acf24-184">A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="acf24-184">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_certificate.png) 

6. <span data-ttu-id="acf24-186">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="acf24-186">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-moxtra-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="acf24-188">A hello **Moxtra konfigurációs** kattintson **konfigurálása Moxtra** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="acf24-188">On hello **Moxtra Configuration** section, click **Configure Moxtra** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="acf24-189">Másolás hello **SAML Entitásazonosító és SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="acf24-189">Copy hello **SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_configure.png) 

8. <span data-ttu-id="acf24-191">Egy másik böngészőablakban bejelentkezéskor tooyour Moxtra vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="acf24-191">In another browser window, sign on tooyour Moxtra company site as an administrator.</span></span>

9. <span data-ttu-id="acf24-192">Hello bal oldali hello eszköztárában kattintson **felügyeleti konzol > SAML-alapú egyszeri bejelentkezést**, és kattintson a **új**.</span><span class="sxs-lookup"><span data-stu-id="acf24-192">In hello toolbar on hello left, click **Admin Console > SAML Single Sign-on**, and then click **New**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_06.png) 

10. <span data-ttu-id="acf24-194">A hello **SAML** lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="acf24-194">On hello **SAML** page, perform hello following steps:</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_08.png)   
 
    <span data-ttu-id="acf24-196">a.</span><span class="sxs-lookup"><span data-stu-id="acf24-196">a.</span></span> <span data-ttu-id="acf24-197">A hello **neve** szövegmező, adja meg a konfiguráció nevét (pl.: *SAML*).</span><span class="sxs-lookup"><span data-stu-id="acf24-197">In hello **Name** textbox, type a name for your configuration (e.g.: *SAML*).</span></span> 
  
    <span data-ttu-id="acf24-198">b.</span><span class="sxs-lookup"><span data-stu-id="acf24-198">b.</span></span> <span data-ttu-id="acf24-199">A hello **IdP Entitásazonosító** szövegmezőhöz Beillesztés hello értékének **SAML Entitásazonosító** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="acf24-199">In hello **IdP Entity ID** textbox, paste hello value of **SAML Entity ID** which you have copied from Azure portal.</span></span> 
 
    <span data-ttu-id="acf24-200">c.</span><span class="sxs-lookup"><span data-stu-id="acf24-200">c.</span></span> <span data-ttu-id="acf24-201">A **bejelentkezési URL-cím** szövegmezőhöz Beillesztés hello értékének **SAML-alapú egyszeri bejelentkezési URL-címe** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="acf24-201">In **Login URL** textbox, paste hello value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span> 
 
    <span data-ttu-id="acf24-202">d.</span><span class="sxs-lookup"><span data-stu-id="acf24-202">d.</span></span> <span data-ttu-id="acf24-203">A hello **AuthnContextClassRef** szövegmezőhöz típus **urn: oasis: nevek: tc: SAML:2.0:ac:classes:Password**.</span><span class="sxs-lookup"><span data-stu-id="acf24-203">In hello **AuthnContextClassRef** textbox, type **urn:oasis:names:tc:SAML:2.0:ac:classes:Password**.</span></span> 
 
    <span data-ttu-id="acf24-204">e.</span><span class="sxs-lookup"><span data-stu-id="acf24-204">e.</span></span> <span data-ttu-id="acf24-205">A hello **NameID formátum** szövegmezőhöz típus **urn: oasis: nevek: tc: SAML:1.1:nameid-formátum: emailAddress**.</span><span class="sxs-lookup"><span data-stu-id="acf24-205">In hello **NameID Format** textbox, type **urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress**.</span></span> 
 
    <span data-ttu-id="acf24-206">f.</span><span class="sxs-lookup"><span data-stu-id="acf24-206">f.</span></span> <span data-ttu-id="acf24-207">Nyissa meg a tanúsítványt, amely a Jegyzettömbben az Azure portálról letöltött hello tartalmát, és illessze be hello **tanúsítvány** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="acf24-207">Open certificate which you have downloaded from Azure portal in notepad, copy hello content, and then paste it into hello **Certificate** textbox.</span></span>    
 
    <span data-ttu-id="acf24-208">g.</span><span class="sxs-lookup"><span data-stu-id="acf24-208">g.</span></span> <span data-ttu-id="acf24-209">Hello SAML e-mail tartomány szövegmezőben írja be a SAML-alapú e-mail tartománya.</span><span class="sxs-lookup"><span data-stu-id="acf24-209">In hello SAML email domain textbox, type your SAML email domain.</span></span>    
  
    >[!NOTE]
    ><span data-ttu-id="acf24-210">toosee hello lépéseket tooverify hello tartomány, kattintson a hello "**i**" alatt.</span><span class="sxs-lookup"><span data-stu-id="acf24-210">toosee hello steps tooverify hello domain, click hello "**i**" below.</span></span>

    <span data-ttu-id="acf24-211">h.</span><span class="sxs-lookup"><span data-stu-id="acf24-211">h.</span></span> <span data-ttu-id="acf24-212">Kattintson a **frissítés**.</span><span class="sxs-lookup"><span data-stu-id="acf24-212">Click **Update**.</span></span>

> [!TIP]
> <span data-ttu-id="acf24-213">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="acf24-213">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="acf24-214">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="acf24-214">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="acf24-215">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="acf24-215">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="acf24-216">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="acf24-216">Creating an Azure AD test user</span></span>
<span data-ttu-id="acf24-217">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="acf24-217">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="acf24-219">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="acf24-219">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="acf24-220">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="acf24-220">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-moxtra-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="acf24-222">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="acf24-222">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-moxtra-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="acf24-224">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="acf24-224">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-moxtra-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="acf24-226">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="acf24-226">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-moxtra-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="acf24-228">a.</span><span class="sxs-lookup"><span data-stu-id="acf24-228">a.</span></span> <span data-ttu-id="acf24-229">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="acf24-229">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="acf24-230">b.</span><span class="sxs-lookup"><span data-stu-id="acf24-230">b.</span></span> <span data-ttu-id="acf24-231">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="acf24-231">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="acf24-232">c.</span><span class="sxs-lookup"><span data-stu-id="acf24-232">c.</span></span> <span data-ttu-id="acf24-233">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="acf24-233">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="acf24-234">d.</span><span class="sxs-lookup"><span data-stu-id="acf24-234">d.</span></span> <span data-ttu-id="acf24-235">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="acf24-235">Click **Create**.</span></span>
 
### <a name="creating-a-moxtra-test-user"></a><span data-ttu-id="acf24-236">Moxtra tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="acf24-236">Creating a Moxtra test user</span></span>

<span data-ttu-id="acf24-237">hello ebben a szakaszban célja toocreate Moxtra Britta Simon nevű felhasználó.</span><span class="sxs-lookup"><span data-stu-id="acf24-237">hello objective of this section is toocreate a user called Britta Simon in Moxtra.</span></span>

<span data-ttu-id="acf24-238">**toocreate Britta Simon meghívta Moxtra, a felhasználó hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="acf24-238">**toocreate a user called Britta Simon in Moxtra, perform hello following steps:**</span></span>

1. <span data-ttu-id="acf24-239">Bejelentkezés tooyour Moxtra vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="acf24-239">Sign on tooyour Moxtra company site as an administrator.</span></span>

2. <span data-ttu-id="acf24-240">Hello bal oldali hello eszköztárában kattintson **felügyeleti konzol > felhasználók kezelése**, majd **felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="acf24-240">In hello toolbar on hello left, click **Admin Console > User Management**, and then **Add User**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_10.png) 

3. <span data-ttu-id="acf24-242">A hello **felhasználó hozzáadása** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="acf24-242">On hello **Add User** dialog, perform hello following steps:</span></span>
  
    <span data-ttu-id="acf24-243">a.</span><span class="sxs-lookup"><span data-stu-id="acf24-243">a.</span></span> <span data-ttu-id="acf24-244">A hello **Utónév** szövegmezőhöz típus **Britta**.</span><span class="sxs-lookup"><span data-stu-id="acf24-244">In hello **First Name** textbox, type **Britta**.</span></span>
  
    <span data-ttu-id="acf24-245">b.</span><span class="sxs-lookup"><span data-stu-id="acf24-245">b.</span></span> <span data-ttu-id="acf24-246">A hello **Vezetéknév** szövegmezőhöz típus **Simon**.</span><span class="sxs-lookup"><span data-stu-id="acf24-246">In hello **Last Name** textbox, type **Simon**.</span></span>
  
    <span data-ttu-id="acf24-247">c.</span><span class="sxs-lookup"><span data-stu-id="acf24-247">c.</span></span> <span data-ttu-id="acf24-248">A hello **E-mail** szövegmezőhöz Britta tartozó e-mail cím megegyezik az Azure-portál típusa.</span><span class="sxs-lookup"><span data-stu-id="acf24-248">In hello **Email** textbox, type Britta's email address same as on Azure portal.</span></span>
  
    <span data-ttu-id="acf24-249">d.</span><span class="sxs-lookup"><span data-stu-id="acf24-249">d.</span></span> <span data-ttu-id="acf24-250">A hello **osztás** szövegmezőhöz típus **fejlesztői**.</span><span class="sxs-lookup"><span data-stu-id="acf24-250">In hello **Division** textbox, type **Dev**.</span></span>
  
    <span data-ttu-id="acf24-251">e.</span><span class="sxs-lookup"><span data-stu-id="acf24-251">e.</span></span> <span data-ttu-id="acf24-252">A hello **részleg** szövegmezőhöz típus **informatikai**.</span><span class="sxs-lookup"><span data-stu-id="acf24-252">In hello **Department** textbox, type **IT**.</span></span>
  
    <span data-ttu-id="acf24-253">f.</span><span class="sxs-lookup"><span data-stu-id="acf24-253">f.</span></span> <span data-ttu-id="acf24-254">Válassza ki **rendszergazda**.</span><span class="sxs-lookup"><span data-stu-id="acf24-254">Select **Administrator**.</span></span>
  
    <span data-ttu-id="acf24-255">g.</span><span class="sxs-lookup"><span data-stu-id="acf24-255">g.</span></span> <span data-ttu-id="acf24-256">Kattintson az **Add** (Hozzáadás) parancsra.</span><span class="sxs-lookup"><span data-stu-id="acf24-256">Click **Add**.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="acf24-257">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="acf24-257">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="acf24-258">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooMoxtra megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="acf24-258">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooMoxtra.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="acf24-260">**tooassign Britta Simon tooMoxtra, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="acf24-260">**tooassign Britta Simon tooMoxtra, perform hello following steps:**</span></span>

1. <span data-ttu-id="acf24-261">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="acf24-261">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="acf24-263">Hello alkalmazások listában válassza ki a **Moxtra**.</span><span class="sxs-lookup"><span data-stu-id="acf24-263">In hello applications list, select **Moxtra**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_app.png) 

3. <span data-ttu-id="acf24-265">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="acf24-265">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="acf24-267">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="acf24-267">Click **Add** button.</span></span> <span data-ttu-id="acf24-268">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="acf24-268">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="acf24-270">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="acf24-270">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="acf24-271">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="acf24-271">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="acf24-272">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="acf24-272">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="acf24-273">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="acf24-273">Testing single sign-on</span></span>

<span data-ttu-id="acf24-274">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="acf24-274">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="acf24-275">Ha a hozzáférési Panel hello hello Moxtra csempe gombra kattint, automatikusan bejelentkezett tooyour Moxtra alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="acf24-275">When you click hello Moxtra tile in hello Access Panel, you should get automatically signed-on tooyour Moxtra application.</span></span>
<span data-ttu-id="acf24-276">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="acf24-276">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="acf24-277">További források</span><span class="sxs-lookup"><span data-stu-id="acf24-277">Additional resources</span></span>

* [<span data-ttu-id="acf24-278">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="acf24-278">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="acf24-279">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="acf24-279">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_203.png

