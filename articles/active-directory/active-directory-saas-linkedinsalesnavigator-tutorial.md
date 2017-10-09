---
title: "Oktatóanyag: Azure Active Directoryval integrált LinkedInSalesNavigator |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és LinkedInSalesNavigator között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 7a9fa8f3-d611-4ffe-8d50-04e9586b24da
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2017
ms.author: jeedes
ms.openlocfilehash: 443d302d40d7af16aba5114e00963f23ea8d12d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-linkedin-sales-navigator"></a><span data-ttu-id="f6d6b-103">Oktatóanyag: Azure Active Directory-integráció a LinkedIn értékesítési Navigator</span><span class="sxs-lookup"><span data-stu-id="f6d6b-103">Tutorial: Azure Active Directory integration with LinkedIn Sales Navigator</span></span>

<span data-ttu-id="f6d6b-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate LinkedIn értékesítési Navigator az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="f6d6b-104">In this tutorial, you learn how toointegrate LinkedIn Sales Navigator with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="f6d6b-105">LinkedIn értékesítési Navigator integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="f6d6b-105">Integrating LinkedIn Sales Navigator with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="f6d6b-106">Megadhatja a hozzáférés tooLinkedIn értékesítési Navigator rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="f6d6b-106">You can control in Azure AD who has access tooLinkedIn Sales Navigator</span></span>
- <span data-ttu-id="f6d6b-107">Az Azure AD-fiókok a engedélyezheti a felhasználók tooautomatically get bejelentkezett tooLinkedIn értékesítési Navigator (egyszeri bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="f6d6b-107">You can enable your users tooautomatically get signed-on tooLinkedIn Sales Navigator (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="f6d6b-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="f6d6b-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="f6d6b-109">Ha szeretné tooknow az Azure AD SaaS integrálásáról további információkat, keresse meg a [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="f6d6b-109">If you want tooknow more details about SaaS app integration with Azure AD, browse [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f6d6b-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="f6d6b-110">Prerequisites</span></span>

<span data-ttu-id="f6d6b-111">az Azure AD integrálása a LinkedIn értékesítési Navigator tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="f6d6b-111">tooconfigure Azure AD integration with LinkedIn Sales Navigator, you need hello following items:</span></span>

- <span data-ttu-id="f6d6b-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="f6d6b-112">An Azure AD subscription</span></span>
- <span data-ttu-id="f6d6b-113">A LinkedIn értékesítési Navigator egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="f6d6b-113">A LinkedIn Sales Navigator single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="f6d6b-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="f6d6b-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="f6d6b-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="f6d6b-116">Kerülje az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-116">Avoid using your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="f6d6b-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="f6d6b-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="f6d6b-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="f6d6b-118">Scenario description</span></span>
<span data-ttu-id="f6d6b-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="f6d6b-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="f6d6b-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="f6d6b-121">Hello gyűjteményből LinkedIn értékesítési Navigator hozzáadása</span><span class="sxs-lookup"><span data-stu-id="f6d6b-121">Adding LinkedIn Sales Navigator from hello gallery</span></span>
2. <span data-ttu-id="f6d6b-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="f6d6b-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-linkedin-sales-navigator-from-hello-gallery"></a><span data-ttu-id="f6d6b-123">Hello gyűjteményből LinkedIn értékesítési Navigator hozzáadása</span><span class="sxs-lookup"><span data-stu-id="f6d6b-123">Adding LinkedIn Sales Navigator from hello gallery</span></span>
<span data-ttu-id="f6d6b-124">tooconfigure hello integrációs LinkedIn értékesítési Navigátor, az Azure AD-be, meg kell tooadd LinkedIn értékesítési Navigator hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-124">tooconfigure hello integration of LinkedIn Sales Navigator into Azure AD, you need tooadd LinkedIn Sales Navigator from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="f6d6b-125">**tooadd LinkedIn értékesítési Navigator hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="f6d6b-125">**tooadd LinkedIn Sales Navigator from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="f6d6b-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="f6d6b-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="f6d6b-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="f6d6b-131">Kattintson a **új alkalmazás** hello párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-131">Click **New application** button on hello top of hello dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="f6d6b-133">Hello keresési mezőbe, írja be a **LinkedIn értékesítési Navigator**.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-133">In hello search box, type **LinkedIn Sales Navigator**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_search.png)

5. <span data-ttu-id="f6d6b-135">A hello eredmények panelen válassza a **LinkedIn értékesítési Navigator**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-135">In hello results panel, select **LinkedIn Sales Navigator**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="f6d6b-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="f6d6b-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="f6d6b-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD az egyszeri bejelentkezés LinkedIn értékesítési Navigator "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-138">In this section, you configure and test Azure AD single sign-on with LinkedIn Sales Navigator based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="f6d6b-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói LinkedIn értékesítési Navigator tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in LinkedIn Sales Navigator is tooa user in Azure AD.</span></span> <span data-ttu-id="f6d6b-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználói hello LinkedIn értékesítési Navigator közötti kapcsolat kapcsolatot kell toobe létrejött.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-140">In other words, a link relationship between an Azure AD user and hello related user in LinkedIn Sales Navigator needs toobe established.</span></span>

<span data-ttu-id="f6d6b-141">Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** LinkedIn értékesítési Navigator.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in LinkedIn Sales Navigator.</span></span>

<span data-ttu-id="f6d6b-142">tooconfigure és a LinkedIn értékesítési Navigator az Azure AD az egyszeri bejelentkezés tesztelése, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="f6d6b-142">tooconfigure and test Azure AD single sign-on with LinkedIn Sales Navigator, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="f6d6b-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="f6d6b-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="f6d6b-145">**[LinkedIn értékesítési Navigator tesztfelhasználó létrehozása](#creating-a-linkedin-sales-navigator-test-user)**  -toohave egy megfelelője a Britta Simon LinkedIn értékesítési Navigator, amely hello felhasználói csatolt toohello az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-145">**[Creating a LinkedIn Sales Navigator test user](#creating-a-linkedin-sales-navigator-test-user)** - toohave a counterpart of Britta Simon in LinkedIn Sales Navigator that is linked toohello Azure AD representation of hello user.</span></span>
4. <span data-ttu-id="f6d6b-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="f6d6b-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="f6d6b-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f6d6b-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="f6d6b-149">Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és a LinkedIn értékesítési Navigator alkalmazásban egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your LinkedIn Sales Navigator application.</span></span>

<span data-ttu-id="f6d6b-150">**a LinkedIn értékesítési Navigator, az Azure AD az egyszeri bejelentkezés tooconfigure hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="f6d6b-150">**tooconfigure Azure AD single sign-on with LinkedIn Sales Navigator, perform hello following steps:**</span></span>

1. <span data-ttu-id="f6d6b-151">Az Azure portál, a hello hello **LinkedIn értékesítési Navigator** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-151">In hello Azure portal, on hello **LinkedIn Sales Navigator** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="f6d6b-153">A hello **egyszeri bejelentkezés** párbeszédpanelen, a **mód** válasszon **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-153">On hello **Single sign-on** dialog, in **Mode** select **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_samlbase.png)

3. <span data-ttu-id="f6d6b-155">Egy másik webes böngészőablakban, a bejelentkezés tooyour **LinkedIn értékesítési Navigator** webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-155">In a different web browser window, sign-on tooyour **LinkedIn Sales Navigator** website as an administrator.</span></span>

4. <span data-ttu-id="f6d6b-156">A **Account Center**, kattintson a **globális beállítások** alatt **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-156">In **Account Center**, click **Global Settings** under **Settings**.</span></span> <span data-ttu-id="f6d6b-157">Jelölje ki, **értékesítési Navigator** hello legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-157">Also, select **Sales Navigator** from hello dropdown list.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedin_admin_01.png)

5. <span data-ttu-id="f6d6b-159">Kattintson a **, vagy kattintson ide tooload, és másolja az egyes mezők hello űrlapból** , és másolja **entitásazonosító** és **helyességi feltétel felhasználói hozzáférést (ACS) URL-cím**.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-159">Click **OR Click Here tooload and copy individual fields from hello form** and copy **Entity Id** and **Assertion Consumer Access (ACS) Url**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedin_admin_031.png)

6. <span data-ttu-id="f6d6b-161">Az Azure-portál a **LinkedIn értékesítési Navigator tartomány és az URL-címek** csoportjában hajtsa végre a következő lépéseket, ha tooconfigure hello alkalmazás hello **IDP** kezdeményezett mód.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-161">On Azure portal, under **LinkedIn Sales Navigator Domain and URLs** section, perform hello following steps if you wish tooconfigure hello application in **IDP** initiated mode.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_url1.png)

    <span data-ttu-id="f6d6b-163">a.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-163">a.</span></span> <span data-ttu-id="f6d6b-164">A hello **azonosítója** szövegmező, adja meg a hello **Entitásazonosító** LinkedIn Portal átmásolva</span><span class="sxs-lookup"><span data-stu-id="f6d6b-164">In hello **Identifier** textbox, enter hello **Entity ID** copied from LinkedIn Portal</span></span> 

    <span data-ttu-id="f6d6b-165">b.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-165">b.</span></span> <span data-ttu-id="f6d6b-166">A hello **válasz URL-CÍMEN** szövegmezőhöz adja meg a hello **helyességi feltétel felhasználói hozzáférést (ACS) URL-cím** LinkedIn Portal átmásolva</span><span class="sxs-lookup"><span data-stu-id="f6d6b-166">In hello **Reply URL** textbox, enter hello **Assertion Consumer Access (ACS) Url** copied from LinkedIn Portal</span></span>

7. <span data-ttu-id="f6d6b-167">Ellenőrizze **megjelenítése speciális URL-beállításainak**, ha tooconfigure hello alkalmazás **SP** kezdeményezett mód.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-167">Check **Show advanced URL settings**, if you wish tooconfigure hello application in **SP** initiated mode.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_url2.png)

    <span data-ttu-id="f6d6b-169">A hello **bejelentkezési URL-cím** szövegmezőhöz típusú hello érték a következő mintát hello használata:`https://www.linkedin.com/checkpoint/enterprise/login/<account id>?application=salesNavigator`</span><span class="sxs-lookup"><span data-stu-id="f6d6b-169">In hello **Sign-on URL** textbox, type hello value using hello following pattern: `https://www.linkedin.com/checkpoint/enterprise/login/<account id>?application=salesNavigator`</span></span>

8. <span data-ttu-id="f6d6b-170">A **LinkedIn értékesítési Navigator** alkalmazás hello SAML helyességi feltételek vár egy meghatározott formátumban, amelyhez tooadd egyéni attribútum hozzárendelések tooyour SAML token attribútumok konfigurációja.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-170">Your **LinkedIn Sales Navigator** application expects hello SAML assertions in a specific format, which requires you tooadd custom attribute mappings tooyour SAML token attributes configuration.</span></span> <span data-ttu-id="f6d6b-171">a következő képernyőkép hello példáját mutatja be.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-171">hello following screenshot shows an example.</span></span> <span data-ttu-id="f6d6b-172">az alapértelmezett érték hello **felhasználói azonosító** van **user.userprincipalname** LinkedIn értékesítési Navigator hello a felhasználó e-mail címét leképezve toobe vár, de.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-172">hello default value of **User Identifier** is **user.userprincipalname** but LinkedIn Sales Navigator expects it toobe mapped with hello user's email address.</span></span> <span data-ttu-id="f6d6b-173">Használhat **user.mail** hello lista attribútumot, vagy használja a szervezet konfiguráció alapján hello megfelelő attribútum értéke.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-173">You can use **user.mail** attribute from hello list or use hello appropriate attribute value based on your organization configuration.</span></span> 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinsalesnavigator-tutorial/updateusermail.png)
    
9. <span data-ttu-id="f6d6b-175">A **felhasználói attribútumok** kattintson **nézet és egyéb felhasználói attribútumok szerkesztése** és hello attribútumainak beállítása.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-175">In **User Attributes** section, click **View and edit all other user attributes** and set hello attributes.</span></span> <span data-ttu-id="f6d6b-176">hello felhasználó számára szükséges tooadd négy jogcímek nevű **e-mail**, **részleg**, **Keresztnév**, és **Vezetéknév** hello értéke pedig leképezve toobe **user.mail**, **felhasználó.részleg**, **user.givenname**, és **user.surname** kulcsattribútumokkal.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-176">hello user needs tooadd four claims named **email**, **department**, **firstname**, and **lastname** and hello value is toobe mapped with **user.mail**, **user.department**, **user.givenname**, and **user.surname** respectively</span></span>

    | <span data-ttu-id="f6d6b-177">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="f6d6b-177">Attribute Name</span></span> | <span data-ttu-id="f6d6b-178">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="f6d6b-178">Attribute Value</span></span> |
    | --- | --- |    
    | <span data-ttu-id="f6d6b-179">E-mailek</span><span class="sxs-lookup"><span data-stu-id="f6d6b-179">email</span></span>| <span data-ttu-id="f6d6b-180">User.mail</span><span class="sxs-lookup"><span data-stu-id="f6d6b-180">user.mail</span></span> |
    | <span data-ttu-id="f6d6b-181">Szervezeti egység</span><span class="sxs-lookup"><span data-stu-id="f6d6b-181">department</span></span>| <span data-ttu-id="f6d6b-182">felhasználó.részleg</span><span class="sxs-lookup"><span data-stu-id="f6d6b-182">user.department</span></span> |
    | <span data-ttu-id="f6d6b-183">Utónév</span><span class="sxs-lookup"><span data-stu-id="f6d6b-183">firstname</span></span>| <span data-ttu-id="f6d6b-184">User.givenName</span><span class="sxs-lookup"><span data-stu-id="f6d6b-184">user.givenname</span></span> |
    | <span data-ttu-id="f6d6b-185">Vezetéknév</span><span class="sxs-lookup"><span data-stu-id="f6d6b-185">lastname</span></span>| <span data-ttu-id="f6d6b-186">User.surname</span><span class="sxs-lookup"><span data-stu-id="f6d6b-186">user.surname</span></span> |
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-linkedinsalesnavigator-tutorial/userattribute.png)
    
    <span data-ttu-id="f6d6b-188">a.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-188">a.</span></span> <span data-ttu-id="f6d6b-189">Kattintson a **attribútum hozzáadása** tooopen hello attribútum párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-189">Click on **Add Attribute** tooopen hello attribute dialog.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_attribute_04.png)
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_attribute_05.png)
   
    <span data-ttu-id="f6d6b-192">b.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-192">b.</span></span> <span data-ttu-id="f6d6b-193">A hello **neve** szövegmezőben, az adott sorhoz feltüntetett hello attribútum neve.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-193">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>
    
    <span data-ttu-id="f6d6b-194">c.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-194">c.</span></span> <span data-ttu-id="f6d6b-195">A hello **érték** listájában, hello attribútuma Típusérték az adott sorhoz feltüntetett.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-195">From hello **Value** list, type hello attribute value shown for that row.</span></span>
    
    <span data-ttu-id="f6d6b-196">d.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-196">d.</span></span> <span data-ttu-id="f6d6b-197">Kattintson a **Ok**</span><span class="sxs-lookup"><span data-stu-id="f6d6b-197">Click **Ok**</span></span>

10. <span data-ttu-id="f6d6b-198">Hajtsa végre a következő lépéseket a hello hello **neve** attribútum -</span><span class="sxs-lookup"><span data-stu-id="f6d6b-198">Perform hello following steps on hello **name** attribute-</span></span>

    <span data-ttu-id="f6d6b-199">a.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-199">a.</span></span> <span data-ttu-id="f6d6b-200">Kattintson a hello attribútum tooopen hello **attribútum szerkesztése** ablak.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-200">Click on hello attribute tooopen hello **Edit Attribute** window.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinsalesnavigator-tutorial/url_update.png)

    <span data-ttu-id="f6d6b-202">b.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-202">b.</span></span> <span data-ttu-id="f6d6b-203">Hello URL-érték törlése hello **névtér**.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-203">Delete hello URL value from hello **namespace**.</span></span>
    
    <span data-ttu-id="f6d6b-204">c.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-204">c.</span></span> <span data-ttu-id="f6d6b-205">Kattintson a **Ok** toosave hello beállítást.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-205">Click **Ok** toosave hello setting.</span></span>

11. <span data-ttu-id="f6d6b-206">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello XML-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-206">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello XML file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_certificate.png) 

12. <span data-ttu-id="f6d6b-208">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-208">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_400.png)

13. <span data-ttu-id="f6d6b-210">Nyissa meg túl**LinkedIn rendszergazdai beállítások** szakasz.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-210">Go too**LinkedIn Admin Settings** section.</span></span> <span data-ttu-id="f6d6b-211">Kattintson a **feltöltése XML-fájl** tooupload hello letöltött hello Azure-portálon a metaadatok XML-fájl.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-211">Click **Upload XML file** tooupload hello Metadata XML file that you have downloaded from hello Azure portal.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedin_metadata_03.png)

14. <span data-ttu-id="f6d6b-213">Kattintson a **a** tooenable egyszeri Bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-213">Click **On** tooenable SSO.</span></span> <span data-ttu-id="f6d6b-214">Egyszeri bejelentkezés állapota a **nincs csatlakoztatva** túl**csatlakoztatva**</span><span class="sxs-lookup"><span data-stu-id="f6d6b-214">SSO status changes from **Not Connected** too**Connected**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedin_admin_05.png)


> [!TIP]
> <span data-ttu-id="f6d6b-216">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="f6d6b-216">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="f6d6b-217">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-217">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="f6d6b-218">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="f6d6b-218">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="f6d6b-219">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="f6d6b-219">Creating an Azure AD test user</span></span>
<span data-ttu-id="f6d6b-220">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-220">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="f6d6b-222">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="f6d6b-222">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="f6d6b-223">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-223">In hello **Azure  portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-linkedinsalesnavigator-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="f6d6b-225">Nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-225">Go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-linkedinsalesnavigator-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="f6d6b-227">Hello párbeszédpanel hello tetején kattintson **Hozzáadás** tooopen hello **felhasználói** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-227">At hello top of hello dialog click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-linkedinsalesnavigator-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="f6d6b-229">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="f6d6b-229">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-linkedinsalesnavigator-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="f6d6b-231">a.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-231">a.</span></span> <span data-ttu-id="f6d6b-232">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-232">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="f6d6b-233">b.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-233">b.</span></span> <span data-ttu-id="f6d6b-234">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-234">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="f6d6b-235">c.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-235">c.</span></span> <span data-ttu-id="f6d6b-236">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-236">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="f6d6b-237">d.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-237">d.</span></span> <span data-ttu-id="f6d6b-238">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-238">Click **Create**.</span></span>
 
### <a name="creating-a-linkedin-sales-navigator-test-user"></a><span data-ttu-id="f6d6b-239">LinkedIn értékesítési Navigator tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="f6d6b-239">Creating a LinkedIn Sales Navigator test user</span></span>

<span data-ttu-id="f6d6b-240">Csatolt értékesítési Navigator alkalmazás támogatja közvetlenül az az idő szerinti (JIT) a felhasználók átadása, miután a felhasználók hitelesítésére hello alkalmazás automatikusan létrejönnek.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-240">Linked Sales Navigator Application supports Just in Time (JIT) user provisioning and after authentication users are created in hello application automatically.</span></span> <span data-ttu-id="f6d6b-241">Aktiválása **automatikusan a szükséges licencek kiosztása** tooassign licenc toohello felhasználó.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-241">Activate **Automatically assign licenses** tooassign a license toohello user.</span></span>
   
   ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-linkedinsalesnavigator-tutorial/LinkedinUserprovswitch.png)

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="f6d6b-243">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="f6d6b-243">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="f6d6b-244">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooLinkedIn értékesítési Navigator megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-244">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooLinkedIn Sales Navigator.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="f6d6b-246">**tooassign Britta Simon tooLinkedIn értékesítési Navigator, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="f6d6b-246">**tooassign Britta Simon tooLinkedIn Sales Navigator, perform hello following steps:**</span></span>

1. <span data-ttu-id="f6d6b-247">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-247">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="f6d6b-249">Hello alkalmazások listában válassza ki a **LinkedIn értékesítési Navigator**.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-249">In hello applications list, select **LinkedIn Sales Navigator**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_app.png) 

3. <span data-ttu-id="f6d6b-251">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-251">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="f6d6b-253">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-253">Click **Add** button.</span></span> <span data-ttu-id="f6d6b-254">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-254">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="f6d6b-256">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-256">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="f6d6b-257">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-257">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="f6d6b-258">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-258">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="f6d6b-259">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="f6d6b-259">Testing single sign-on</span></span>

<span data-ttu-id="f6d6b-260">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-260">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="f6d6b-261">Amikor hello LinkedIn értékesítési Navigator csempe a hozzáférési Panel hello gombra kattint, melyekben tooprovide a személyes LinkedIn fiókadatok átirányított tooOrganizational lapon kell.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-261">When you click hello LinkedIn Sales Navigator tile in hello Access Panel, you should be redirected tooOrganizational page where you have tooprovide your personal LinkedIn account details.</span></span> <span data-ttu-id="f6d6b-262">A személyes fiókot a LinkedIn üzleti fiókjával hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-262">It links your personal account with your LinkedIn business account.</span></span> <span data-ttu-id="f6d6b-263">További információ a hozzáférési Panel hello: [a hozzáférési Panel bemutatása](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="f6d6b-263">For more information about hello Access Panel, see [Introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="f6d6b-264">További források</span><span class="sxs-lookup"><span data-stu-id="f6d6b-264">Additional resources</span></span>

* [<span data-ttu-id="f6d6b-265">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="f6d6b-265">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="f6d6b-266">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="f6d6b-266">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_203.png

