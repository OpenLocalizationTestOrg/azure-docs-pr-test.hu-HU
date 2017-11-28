---
title: "Oktatóanyag: Azure Active Directoryval integrált myPolicies |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és myPolicies között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: bf79e858-1dfb-4ab3-a6df-74b2d5a878d2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/04/2017
ms.author: jeedes
ms.openlocfilehash: d8890457ebdb1b80e0d3126d4210e6265ae7f1ee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-mypolicies"></a><span data-ttu-id="8a3f9-103">Oktatóanyag: Azure Active Directoryval integrált myPolicies</span><span class="sxs-lookup"><span data-stu-id="8a3f9-103">Tutorial: Azure Active Directory integration with myPolicies</span></span>

<span data-ttu-id="8a3f9-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate myPolicies az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="8a3f9-104">In this tutorial, you learn how toointegrate myPolicies with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="8a3f9-105">MyPolicies integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="8a3f9-105">Integrating myPolicies with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="8a3f9-106">Megadhatja a hozzáférés toomyPolicies rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="8a3f9-106">You can control in Azure AD who has access toomyPolicies</span></span>
- <span data-ttu-id="8a3f9-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett toomyPolicies (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="8a3f9-107">You can enable your users tooautomatically get signed-on toomyPolicies (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="8a3f9-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="8a3f9-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="8a3f9-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="8a3f9-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8a3f9-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="8a3f9-110">Prerequisites</span></span>

<span data-ttu-id="8a3f9-111">az Azure AD integrálása myPolicies tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="8a3f9-111">tooconfigure Azure AD integration with myPolicies, you need hello following items:</span></span>

- <span data-ttu-id="8a3f9-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="8a3f9-112">An Azure AD subscription</span></span>
- <span data-ttu-id="8a3f9-113">Egy myPolicies egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="8a3f9-113">A myPolicies single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="8a3f9-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="8a3f9-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="8a3f9-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="8a3f9-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="8a3f9-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="8a3f9-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="8a3f9-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="8a3f9-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="8a3f9-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="8a3f9-118">Scenario description</span></span>
<span data-ttu-id="8a3f9-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="8a3f9-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="8a3f9-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="8a3f9-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="8a3f9-121">Hello gyűjteményből myPolicies hozzáadása</span><span class="sxs-lookup"><span data-stu-id="8a3f9-121">Adding myPolicies from hello gallery</span></span>
2. <span data-ttu-id="8a3f9-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="8a3f9-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-mypolicies-from-hello-gallery"></a><span data-ttu-id="8a3f9-123">Hello gyűjteményből myPolicies hozzáadása</span><span class="sxs-lookup"><span data-stu-id="8a3f9-123">Adding myPolicies from hello gallery</span></span>
<span data-ttu-id="8a3f9-124">tooconfigure hello integrációja myPolicies az Azure AD-be, meg kell tooadd myPolicies hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="8a3f9-124">tooconfigure hello integration of myPolicies into Azure AD, you need tooadd myPolicies from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="8a3f9-125">**tooadd myPolicies hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="8a3f9-125">**tooadd myPolicies from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="8a3f9-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="8a3f9-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="8a3f9-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="8a3f9-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="8a3f9-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="8a3f9-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="8a3f9-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="8a3f9-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="8a3f9-133">Hello keresési mezőbe, írja be a **myPolicies**.</span><span class="sxs-lookup"><span data-stu-id="8a3f9-133">In hello search box, type **myPolicies**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-mypolicies-tutorial/tutorial_mypolicies_search.png)

5. <span data-ttu-id="8a3f9-135">A hello eredmények panelen válassza ki a **myPolicies**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="8a3f9-135">In hello results panel, select **myPolicies**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-mypolicies-tutorial/tutorial_mypolicies_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="8a3f9-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="8a3f9-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="8a3f9-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján myPolicies.</span><span class="sxs-lookup"><span data-stu-id="8a3f9-138">In this section, you configure and test Azure AD single sign-on with myPolicies based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="8a3f9-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó myPolicies tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="8a3f9-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in myPolicies is tooa user in Azure AD.</span></span> <span data-ttu-id="8a3f9-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello myPolicies közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="8a3f9-140">In other words, a link relationship between an Azure AD user and hello related user in myPolicies needs toobe established.</span></span>

<span data-ttu-id="8a3f9-141">MyPolicies, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="8a3f9-141">In myPolicies, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="8a3f9-142">tooconfigure és az Azure AD az egyszeri bejelentkezés myPolicies-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="8a3f9-142">tooconfigure and test Azure AD single sign-on with myPolicies, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="8a3f9-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="8a3f9-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="8a3f9-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="8a3f9-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="8a3f9-145">**[MyPolicies tesztfelhasználó létrehozása](#creating-a-mypolicies-test-user)**  -toohave Britta Simon egy partner, a myPolicies, amely a felhasználó csatolt toohello az Azure AD-ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="8a3f9-145">**[Creating a myPolicies test user](#creating-a-mypolicies-test-user)** - toohave a counterpart of Britta Simon in myPolicies that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="8a3f9-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="8a3f9-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="8a3f9-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="8a3f9-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="8a3f9-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="8a3f9-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="8a3f9-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az myPolicies alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="8a3f9-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your myPolicies application.</span></span>

<span data-ttu-id="8a3f9-150">**az Azure AD tooconfigure egyszeri bejelentkezést a myPolicies, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="8a3f9-150">**tooconfigure Azure AD single sign-on with myPolicies, perform hello following steps:**</span></span>

1. <span data-ttu-id="8a3f9-151">Az Azure portál, a hello hello **myPolicies** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="8a3f9-151">In hello Azure portal, on hello **myPolicies** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="8a3f9-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="8a3f9-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-mypolicies-tutorial/tutorial_mypolicies_samlbase.png)

3. <span data-ttu-id="8a3f9-155">A hello **myPolicies tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="8a3f9-155">On hello **myPolicies Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-mypolicies-tutorial/tutorial_mypolicies_url.png)

    <span data-ttu-id="8a3f9-157">a.</span><span class="sxs-lookup"><span data-stu-id="8a3f9-157">a.</span></span> <span data-ttu-id="8a3f9-158">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<tenantname>.mypolicies.com/`</span><span class="sxs-lookup"><span data-stu-id="8a3f9-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<tenantname>.mypolicies.com/`</span></span>

    <span data-ttu-id="8a3f9-159">b.</span><span class="sxs-lookup"><span data-stu-id="8a3f9-159">b.</span></span> <span data-ttu-id="8a3f9-160">A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<tenantname>.mypolicies.com/users/auth/saml/callback`</span><span class="sxs-lookup"><span data-stu-id="8a3f9-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<tenantname>.mypolicies.com/users/auth/saml/callback`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="8a3f9-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="8a3f9-161">These values are not real.</span></span> <span data-ttu-id="8a3f9-162">Frissítheti ezeket az értékeket hello tényleges azonosítója és a válasz URL-címmel.</span><span class="sxs-lookup"><span data-stu-id="8a3f9-162">Update these values with hello actual Identifier and Reply URL.</span></span> <span data-ttu-id="8a3f9-163">Ügyfél [myPolicies támogatási csoport](mailto:support@mypolicies.com) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="8a3f9-163">Contact [myPolicies support team](mailto:support@mypolicies.com) tooget these values.</span></span>

4. <span data-ttu-id="8a3f9-164">hello myPolicies alkalmazás hello SAML helyességi feltételek vár egy meghatározott formátumban, és hogy tooadd egyéni attribútum hozzárendelések tooyour SAML token attribútumok konfigurációt igényel.</span><span class="sxs-lookup"><span data-stu-id="8a3f9-164">hello myPolicies application expects hello SAML assertions in a specific format, which requires you tooadd custom attribute mappings tooyour SAML token attributes configuration.</span></span> <span data-ttu-id="8a3f9-165">Az alkalmazás jogcímek a következő hello konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="8a3f9-165">Configure hello following claims for this application.</span></span> <span data-ttu-id="8a3f9-166">Ezek az attribútumok értékének hello hello kezelése "**felhasználói attribútumok**" szakasz alkalmazás integráció lapján.</span><span class="sxs-lookup"><span data-stu-id="8a3f9-166">You can manage hello values of these attributes from hello "**User Attributes**" section on application integration page.</span></span> <span data-ttu-id="8a3f9-167">a következő képernyőkép hello ezen mutat egy példát.</span><span class="sxs-lookup"><span data-stu-id="8a3f9-167">hello following screenshot shows an example for this.</span></span> 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-mypolicies-tutorial/tutorial_mypolicies_attribute.png)

5. <span data-ttu-id="8a3f9-169">Kattintson a **nézet és egyéb felhasználói attribútumok szerkesztése** hello jelölőnégyzet **felhasználói attribútumok** tooexpand hello attribútumok szakasz.</span><span class="sxs-lookup"><span data-stu-id="8a3f9-169">Click **View and edit all other user attributes** checkbox in hello **User Attributes** section tooexpand hello attributes.</span></span> <span data-ttu-id="8a3f9-170">Hajtsa végre a következő lépéseket mindegyik megjelenített hello attribútumok – hello</span><span class="sxs-lookup"><span data-stu-id="8a3f9-170">Perform hello following steps on each of hello displayed attributes-</span></span>

    | <span data-ttu-id="8a3f9-171">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="8a3f9-171">Attribute Name</span></span> | <span data-ttu-id="8a3f9-172">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="8a3f9-172">Attribute Value</span></span> |
    | ------------------- | ---------- |
    | <span data-ttu-id="8a3f9-173">givenName</span><span class="sxs-lookup"><span data-stu-id="8a3f9-173">givenname</span></span> | <span data-ttu-id="8a3f9-174">User.givenName</span><span class="sxs-lookup"><span data-stu-id="8a3f9-174">user.givenname</span></span> |
    | <span data-ttu-id="8a3f9-175">Vezetéknév</span><span class="sxs-lookup"><span data-stu-id="8a3f9-175">surname</span></span> | <span data-ttu-id="8a3f9-176">User.surname</span><span class="sxs-lookup"><span data-stu-id="8a3f9-176">user.surname</span></span> |
    | <span data-ttu-id="8a3f9-177">e-mail cím</span><span class="sxs-lookup"><span data-stu-id="8a3f9-177">emailaddress</span></span> | <span data-ttu-id="8a3f9-178">User.mail</span><span class="sxs-lookup"><span data-stu-id="8a3f9-178">user.mail</span></span> |
    | <span data-ttu-id="8a3f9-179">név</span><span class="sxs-lookup"><span data-stu-id="8a3f9-179">name</span></span> | <span data-ttu-id="8a3f9-180">User.userPrincipalName</span><span class="sxs-lookup"><span data-stu-id="8a3f9-180">user.userprincipalname</span></span> |
    
    <span data-ttu-id="8a3f9-181">a.</span><span class="sxs-lookup"><span data-stu-id="8a3f9-181">a.</span></span> <span data-ttu-id="8a3f9-182">Kattintson a hello attribútum tooopen hello **attribútum szerkesztése** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="8a3f9-182">Click on hello attribute tooopen hello **Edit Attribute** dialog.</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-mypolicies-tutorial/tutorial_attribute_05.png)
    
    <span data-ttu-id="8a3f9-184">b.</span><span class="sxs-lookup"><span data-stu-id="8a3f9-184">b.</span></span> <span data-ttu-id="8a3f9-185">Hello URL-érték törlése hello **Namespace**.</span><span class="sxs-lookup"><span data-stu-id="8a3f9-185">Delete hello URL value from hello **Namespace**.</span></span>
    
    <span data-ttu-id="8a3f9-186">c.</span><span class="sxs-lookup"><span data-stu-id="8a3f9-186">c.</span></span> <span data-ttu-id="8a3f9-187">Kattintson a **Ok** toosave hello beállítást.</span><span class="sxs-lookup"><span data-stu-id="8a3f9-187">Click **Ok** toosave hello setting.</span></span>
    
6. <span data-ttu-id="8a3f9-188">A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="8a3f9-188">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-mypolicies-tutorial/tutorial_mypolicies_certificate.png) 

7. <span data-ttu-id="8a3f9-190">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="8a3f9-190">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-mypolicies-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="8a3f9-192">A hello **myPolicies konfigurációs** kattintson **myPolicies konfigurálása** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="8a3f9-192">On hello **myPolicies Configuration** section, click **Configure myPolicies** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="8a3f9-193">Másolás hello **SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="8a3f9-193">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-mypolicies-tutorial/tutorial_mypolicies_configure.png) 

9. <span data-ttu-id="8a3f9-195">tooconfigure egyszeri bejelentkezést a **myPolicies** oldalon kell letöltött toosend hello **Certificate(Base64)** és **SAML-alapú egyszeri bejelentkezési URL-címe** túl[myPolicies támogatási csoport](mailto:support@mypolicies.com).</span><span class="sxs-lookup"><span data-stu-id="8a3f9-195">tooconfigure single sign-on on **myPolicies** side, you need toosend hello downloaded **Certificate(Base64)** and **SAML Single Sign-On Service URL** too[myPolicies support team](mailto:support@mypolicies.com).</span></span> 

> [!TIP]
> <span data-ttu-id="8a3f9-196">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="8a3f9-196">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="8a3f9-197">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="8a3f9-197">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="8a3f9-198">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="8a3f9-198">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="8a3f9-199">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="8a3f9-199">Creating an Azure AD test user</span></span>
<span data-ttu-id="8a3f9-200">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="8a3f9-200">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="8a3f9-202">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="8a3f9-202">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="8a3f9-203">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="8a3f9-203">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-mypolicies-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="8a3f9-205">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="8a3f9-205">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-mypolicies-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="8a3f9-207">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="8a3f9-207">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-mypolicies-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="8a3f9-209">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="8a3f9-209">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-mypolicies-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="8a3f9-211">a.</span><span class="sxs-lookup"><span data-stu-id="8a3f9-211">a.</span></span> <span data-ttu-id="8a3f9-212">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="8a3f9-212">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="8a3f9-213">b.</span><span class="sxs-lookup"><span data-stu-id="8a3f9-213">b.</span></span> <span data-ttu-id="8a3f9-214">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="8a3f9-214">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="8a3f9-215">c.</span><span class="sxs-lookup"><span data-stu-id="8a3f9-215">c.</span></span> <span data-ttu-id="8a3f9-216">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="8a3f9-216">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="8a3f9-217">d.</span><span class="sxs-lookup"><span data-stu-id="8a3f9-217">d.</span></span> <span data-ttu-id="8a3f9-218">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="8a3f9-218">Click **Create**.</span></span>
 
### <a name="creating-a-mypolicies-test-user"></a><span data-ttu-id="8a3f9-219">MyPolicies tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="8a3f9-219">Creating a myPolicies test user</span></span>

<span data-ttu-id="8a3f9-220">Ebben a szakaszban egy myPolicies Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="8a3f9-220">In this section, you create a user called Britta Simon in myPolicies.</span></span> <span data-ttu-id="8a3f9-221">Együttműködve [myPolicies támogatási csoport](mailto:support@mypolicies.com) felhasználót is hozzáadhat hello hello myPolicies platform.</span><span class="sxs-lookup"><span data-stu-id="8a3f9-221">Work with [myPolicies support team](mailto:support@mypolicies.com) to add hello users in hello myPolicies platform.</span></span> <span data-ttu-id="8a3f9-222">Felhasználók kell létrehoznia és aktiválni az egyszeri bejelentkezés használata előtt.</span><span class="sxs-lookup"><span data-stu-id="8a3f9-222">Users must be created and activated before you use single sign-on.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="8a3f9-223">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="8a3f9-223">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="8a3f9-224">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés toomyPolicies megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="8a3f9-224">In this section, you enable Britta Simon toouse Azure single sign-on by granting access toomyPolicies.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="8a3f9-226">**tooassign Britta Simon toomyPolicies, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="8a3f9-226">**tooassign Britta Simon toomyPolicies, perform hello following steps:**</span></span>

1. <span data-ttu-id="8a3f9-227">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="8a3f9-227">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="8a3f9-229">Hello alkalmazások listában válassza ki a **myPolicies**.</span><span class="sxs-lookup"><span data-stu-id="8a3f9-229">In hello applications list, select **myPolicies**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-mypolicies-tutorial/tutorial_mypolicies_app.png) 

3. <span data-ttu-id="8a3f9-231">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="8a3f9-231">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="8a3f9-233">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="8a3f9-233">Click **Add** button.</span></span> <span data-ttu-id="8a3f9-234">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="8a3f9-234">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="8a3f9-236">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="8a3f9-236">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="8a3f9-237">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="8a3f9-237">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="8a3f9-238">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="8a3f9-238">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="8a3f9-239">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="8a3f9-239">Testing single sign-on</span></span>

<span data-ttu-id="8a3f9-240">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="8a3f9-240">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="8a3f9-241">Hello myPolicies hello hozzáférési Panel csempére kattintva kapja meg automatikusan bejelentkezett tooyour myPolicies alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="8a3f9-241">When you click hello myPolicies tile in hello Access Panel, you should get automatically signed-on tooyour myPolicies application.</span></span>
<span data-ttu-id="8a3f9-242">A hozzáférési Panel kapcsolatos további információkért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="8a3f9-242">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8a3f9-243">További források</span><span class="sxs-lookup"><span data-stu-id="8a3f9-243">Additional resources</span></span>

* [<span data-ttu-id="8a3f9-244">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="8a3f9-244">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="8a3f9-245">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="8a3f9-245">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-mypolicies-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-mypolicies-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-mypolicies-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-mypolicies-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-mypolicies-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-mypolicies-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-mypolicies-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-mypolicies-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-mypolicies-tutorial/tutorial_general_203.png

