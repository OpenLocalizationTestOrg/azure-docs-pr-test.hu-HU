---
title: "Oktatóanyag: Azure Active Directoryval integrált LinkedIn keresési |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a LinkedIn keresési között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a2757a39-1ead-4a3e-91e4-270be3055683
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2017
ms.author: jeedes
ms.openlocfilehash: d79c34baa676391699e4b49806f16422fcfe73e1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-linkedin-lookup"></a><span data-ttu-id="65d30-103">Oktatóanyag: Azure Active Directoryval integrált LinkedIn-keresés</span><span class="sxs-lookup"><span data-stu-id="65d30-103">Tutorial: Azure Active Directory integration with LinkedIn Lookup</span></span>

<span data-ttu-id="65d30-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate LinkedIn keresési az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="65d30-104">In this tutorial, you learn how toointegrate LinkedIn Lookup with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="65d30-105">LinkedIn keresési integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="65d30-105">Integrating LinkedIn Lookup with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="65d30-106">Megadhatja a hozzáférés tooLinkedIn keresési rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="65d30-106">You can control in Azure AD who has access tooLinkedIn Lookup</span></span>
- <span data-ttu-id="65d30-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooLinkedIn keresési (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="65d30-107">You can enable your users tooautomatically get signed-on tooLinkedIn Lookup (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="65d30-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="65d30-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="65d30-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="65d30-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="65d30-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="65d30-110">Prerequisites</span></span>

<span data-ttu-id="65d30-111">az Azure AD integrálása LinkedIn keresési tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="65d30-111">tooconfigure Azure AD integration with LinkedIn Lookup, you need hello following items:</span></span>

- <span data-ttu-id="65d30-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="65d30-112">An Azure AD subscription</span></span>
- <span data-ttu-id="65d30-113">Egy LinkedIn keresési egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="65d30-113">An LinkedIn Lookup single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="65d30-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="65d30-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="65d30-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="65d30-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="65d30-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="65d30-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="65d30-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="65d30-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="65d30-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="65d30-118">Scenario description</span></span>
<span data-ttu-id="65d30-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="65d30-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="65d30-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="65d30-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="65d30-121">Hello gyűjteményből LinkedIn keresési hozzáadása</span><span class="sxs-lookup"><span data-stu-id="65d30-121">Adding LinkedIn Lookup from hello gallery</span></span>
2. <span data-ttu-id="65d30-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="65d30-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-linkedin-lookup-from-hello-gallery"></a><span data-ttu-id="65d30-123">Hello gyűjteményből LinkedIn keresési hozzáadása</span><span class="sxs-lookup"><span data-stu-id="65d30-123">Adding LinkedIn Lookup from hello gallery</span></span>
<span data-ttu-id="65d30-124">tooconfigure hello integrációs LinkedIn-keresés, az Azure AD-be, meg kell tooadd LinkedIn keresési hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="65d30-124">tooconfigure hello integration of LinkedIn Lookup into Azure AD, you need tooadd LinkedIn Lookup from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="65d30-125">**tooadd LinkedIn keresési hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="65d30-125">**tooadd LinkedIn Lookup from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="65d30-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="65d30-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="65d30-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="65d30-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="65d30-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="65d30-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="65d30-131">Kattintson a **új alkalmazás** hello felül hello párbeszédpanel tooadd új alkalmazás gomb.</span><span class="sxs-lookup"><span data-stu-id="65d30-131">Click **New application** button on hello top of hello dialog tooadd new application.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="65d30-133">Hello keresési mezőbe, írja be a **LinkedIn keresési**.</span><span class="sxs-lookup"><span data-stu-id="65d30-133">In hello search box, type **LinkedIn Lookup**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedInlookup_search.png)

5. <span data-ttu-id="65d30-135">A hello eredmények panelen válassza ki a **LinkedIn keresési**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="65d30-135">In hello results panel, select **LinkedIn Lookup**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedInlookup_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="65d30-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="65d30-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="65d30-138">Ebben a szakaszban konfigurálja, és az Azure AD az egyszeri bejelentkezés LinkedIn keresési-teszthez "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="65d30-138">In this section, you configure and test Azure AD single sign-on with LinkedIn Lookup based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="65d30-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó LinkedIn keresési tooa felhasználói az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="65d30-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in LinkedIn Lookup is tooa user in Azure AD.</span></span> <span data-ttu-id="65d30-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello LinkedIn keresési közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="65d30-140">In other words, a link relationship between an Azure AD user and hello related user in LinkedIn Lookup needs toobe established.</span></span>

<span data-ttu-id="65d30-141">Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** LinkedIn keresés.</span><span class="sxs-lookup"><span data-stu-id="65d30-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in LinkedIn Lookup.</span></span>

<span data-ttu-id="65d30-142">tooconfigure és az Azure AD az egyszeri bejelentkezés LinkedIn keresési-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="65d30-142">tooconfigure and test Azure AD single sign-on with LinkedIn Lookup, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="65d30-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="65d30-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="65d30-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="65d30-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="65d30-145">**[Egy LinkedIn keresési tesztfelhasználó létrehozása](#creating-an-linkedin-lookup-test-user)**  -toohave egy megfelelője a Britta Simon LinkedIn-keresés, amely az Azure AD csatolt toohello megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="65d30-145">**[Creating an LinkedIn Lookup test user](#creating-an-linkedin-lookup-test-user)** - toohave a counterpart of Britta Simon in LinkedIn Lookup that is linked toohello Azure AD representation.</span></span>
4. <span data-ttu-id="65d30-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="65d30-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="65d30-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="65d30-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="65d30-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="65d30-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="65d30-149">Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és a LinkedIn keresési alkalmazásban egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="65d30-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your LinkedIn Lookup application.</span></span>

<span data-ttu-id="65d30-150">**tooconfigure az Azure AD egyszeri bejelentkezést a LinkedIn-kereséssel, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="65d30-150">**tooconfigure Azure AD single sign-on with LinkedIn Lookup, perform hello following steps:**</span></span>

1. <span data-ttu-id="65d30-151">Az Azure portál, a hello hello **LinkedIn keresési** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="65d30-151">In hello Azure portal, on hello **LinkedIn Lookup** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="65d30-153">A hello **egyszeri bejelentkezés** párbeszédpanelen, a **mód** válasszon **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="65d30-153">On hello **Single sign-on** dialog, in **Mode** select **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedInlookup_samlbase.png)

3. <span data-ttu-id="65d30-155">Egy másik webes böngészőablakban, a bejelentkezés tooyour **LinkedIn keresési** webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="65d30-155">In a different web browser window, sign-on tooyour **LinkedIn Lookup** website as an administrator.</span></span>

4. <span data-ttu-id="65d30-156">A **Account Center**, kattintson a **globális beállítások** alatt **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="65d30-156">In **Account Center**, click **Global Settings** under **Settings**.</span></span> <span data-ttu-id="65d30-157">Jelölje ki, **keresési** hello legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="65d30-157">Also, select **Lookup** from hello dropdown list.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_LinkedIn_admin_011.png)

5. <span data-ttu-id="65d30-159">Kattintson a **, vagy kattintson ide tooload, és másolja az egyes mezők hello űrlapból** , és másolja **entitásazonosító** és **helyességi feltétel felhasználói hozzáférést (ACS) URL-címe**</span><span class="sxs-lookup"><span data-stu-id="65d30-159">Click **OR Click Here tooload and copy individual fields from hello form** and copy **Entity Id** and **Assertion Consumer Access (ACS) Url**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_LinkedIn_admin_032.png)

6. <span data-ttu-id="65d30-161">Az Azure-portál a **LinkedIn keresési tartomány és az URL-címek** csoportjában hajtsa végre a következő lépéseket, ha tooconfigure hello alkalmazás hello **IDP** kezdeményezett mód:</span><span class="sxs-lookup"><span data-stu-id="65d30-161">On Azure portal, under **LinkedIn Lookup Domain and URLs** section, perform hello following steps if you wish tooconfigure hello application in **IDP** initiated mode:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedInlookup_url.png)

    <span data-ttu-id="65d30-163">a.</span><span class="sxs-lookup"><span data-stu-id="65d30-163">a.</span></span> <span data-ttu-id="65d30-164">A hello **azonosítója** szövegmező, adja meg a hello **Entitásazonosító** LinkedIn Portal átmásolva</span><span class="sxs-lookup"><span data-stu-id="65d30-164">In hello **Identifier** textbox, enter hello **Entity ID** copied from LinkedIn Portal</span></span> 

    <span data-ttu-id="65d30-165">b.</span><span class="sxs-lookup"><span data-stu-id="65d30-165">b.</span></span> <span data-ttu-id="65d30-166">A hello **válasz URL-CÍMEN** szövegmezőhöz adja meg a hello **helyességi feltétel felhasználói hozzáférést (ACS) URL-cím** LinkedIn Portal átmásolva</span><span class="sxs-lookup"><span data-stu-id="65d30-166">In hello **Reply URL** textbox, enter hello **Assertion Consumer Access (ACS) Url** copied from LinkedIn Portal</span></span>

7. <span data-ttu-id="65d30-167">Ellenőrizze **megjelenítése speciális URL-beállításainak**, ha tooconfigure hello alkalmazás **SP** kezdeményezett mód:</span><span class="sxs-lookup"><span data-stu-id="65d30-167">Check **Show advanced URL settings**, if you wish tooconfigure hello application in **SP** initiated mode:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedInlookup_url1.png)

    <span data-ttu-id="65d30-169">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://www.linkedIn.com/checkpoint/enterprise/login/<AccountId>?application=lookup`</span><span class="sxs-lookup"><span data-stu-id="65d30-169">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://www.linkedIn.com/checkpoint/enterprise/login/<AccountId>?application=lookup`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="65d30-170">Ez nem valódi értékek.</span><span class="sxs-lookup"><span data-stu-id="65d30-170">This is not real value.</span></span> <span data-ttu-id="65d30-171">hello felhasználó rendelkezik tooupdate ezeket az értékeket hello a tényleges bejelentkezési URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="65d30-171">hello user has tooupdate these values with hello actual Sign-On URL.</span></span> <span data-ttu-id="65d30-172">Ügyfél [LinkedIn keresési ügyfél-támogatási csoport](https://business.LinkedIn.com/lookup) tooget ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="65d30-172">Contact [LinkedIn Lookup Client support team](https://business.LinkedIn.com/lookup) tooget this value.</span></span>

8. <span data-ttu-id="65d30-173">A **LinkedIn keresési** alkalmazás hello SAML helyességi feltételek vár egy meghatározott formátumban.</span><span class="sxs-lookup"><span data-stu-id="65d30-173">Your **LinkedIn Lookup** application expects hello SAML assertions in a specific format.</span></span> <span data-ttu-id="65d30-174">hello felhasználó rendelkezik-e tooadd egyéni attribútum hozzárendelések toohello SAML token attribútumok konfigurációja.</span><span class="sxs-lookup"><span data-stu-id="65d30-174">hello user has tooadd custom attribute mappings toohello SAML token attributes configuration.</span></span> <span data-ttu-id="65d30-175">a következő képernyőkép hello példáját mutatja be.</span><span class="sxs-lookup"><span data-stu-id="65d30-175">hello following screenshot shows an example.</span></span> <span data-ttu-id="65d30-176">alapértelmezett értékének hello **felhasználói azonosító** van **user.userprincipalname** de LinkedIn keresési vár a toobe leképezve hello a felhasználó e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="65d30-176">hello default value of **User Identifier** is **user.userprincipalname** but LinkedIn Lookup expects this toobe mapped with hello user's email address.</span></span> <span data-ttu-id="65d30-177">Használhat **user.mail** hello lista attribútumot, vagy használja a szervezet konfiguráció alapján hello megfelelő attribútum értéke.</span><span class="sxs-lookup"><span data-stu-id="65d30-177">You can use **user.mail** attribute from hello list or use hello appropriate attribute value based on your organization configuration.</span></span> 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-LinkedInlookup-tutorial/updateusermail.png)
    
9. <span data-ttu-id="65d30-179">A **felhasználói attribútumok** kattintson **nézet és egyéb felhasználói attribútumok szerkesztése** és hello attribútumainak beállítása.</span><span class="sxs-lookup"><span data-stu-id="65d30-179">In **User Attributes** section, click **View and edit all other user attributes** and set hello attributes.</span></span> <span data-ttu-id="65d30-180">hello felhasználó számára szükséges tooadd négy jogcímek nevű **e-mail**, **részleg**, **Keresztnév**, és **Vezetéknév** hello értéke pedig leképezve toobe **user.mail**, **felhasználó.részleg**, **user.givenname**, és **user.surname** kulcsattribútumokkal.</span><span class="sxs-lookup"><span data-stu-id="65d30-180">hello user needs tooadd four claims named **email**,  **department**, **firstname**, and **lastname** and hello value is toobe mapped with **user.mail**, **user.department**, **user.givenname**, and **user.surname** respectively</span></span>

    | <span data-ttu-id="65d30-181">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="65d30-181">Attribute Name</span></span> | <span data-ttu-id="65d30-182">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="65d30-182">Attribute Value</span></span> |
    | --- | --- |
    | <span data-ttu-id="65d30-183">E-mailek</span><span class="sxs-lookup"><span data-stu-id="65d30-183">email</span></span>| <span data-ttu-id="65d30-184">User.mail</span><span class="sxs-lookup"><span data-stu-id="65d30-184">user.mail</span></span> |    
    | <span data-ttu-id="65d30-185">Szervezeti egység</span><span class="sxs-lookup"><span data-stu-id="65d30-185">department</span></span>| <span data-ttu-id="65d30-186">felhasználó.részleg</span><span class="sxs-lookup"><span data-stu-id="65d30-186">user.department</span></span> |
    | <span data-ttu-id="65d30-187">Utónév</span><span class="sxs-lookup"><span data-stu-id="65d30-187">firstname</span></span>| <span data-ttu-id="65d30-188">User.givenName</span><span class="sxs-lookup"><span data-stu-id="65d30-188">user.givenname</span></span> |
    | <span data-ttu-id="65d30-189">Vezetéknév</span><span class="sxs-lookup"><span data-stu-id="65d30-189">lastname</span></span>| <span data-ttu-id="65d30-190">User.surname</span><span class="sxs-lookup"><span data-stu-id="65d30-190">user.surname</span></span> |

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-LinkedInlookup-tutorial/userattribute.png)

    <span data-ttu-id="65d30-192">a.</span><span class="sxs-lookup"><span data-stu-id="65d30-192">a.</span></span> <span data-ttu-id="65d30-193">Kattintson a **attribútum hozzáadása** tooopen hello **attribútum hozzáadása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="65d30-193">Click **Add Attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-LinkedInlookup-tutorial/4.png)
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-LinkedInlookup-tutorial/5.png)
   
    <span data-ttu-id="65d30-196">b.</span><span class="sxs-lookup"><span data-stu-id="65d30-196">b.</span></span> <span data-ttu-id="65d30-197">A hello **neve** szövegmezőben, az adott sorhoz feltüntetett hello attribútum neve.</span><span class="sxs-lookup"><span data-stu-id="65d30-197">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>
    
    <span data-ttu-id="65d30-198">c.</span><span class="sxs-lookup"><span data-stu-id="65d30-198">c.</span></span> <span data-ttu-id="65d30-199">A hello **érték** listájában, hello attribútuma Típusérték az adott sorhoz feltüntetett.</span><span class="sxs-lookup"><span data-stu-id="65d30-199">From hello **Value** list, type hello attribute value shown for that row.</span></span>
    
    <span data-ttu-id="65d30-200">d.</span><span class="sxs-lookup"><span data-stu-id="65d30-200">d.</span></span> <span data-ttu-id="65d30-201">Kattintson a **Ok**</span><span class="sxs-lookup"><span data-stu-id="65d30-201">Click **Ok**</span></span>

10. <span data-ttu-id="65d30-202">Hajtsa végre a következő lépéseket a hello hello **neve** attribútum -</span><span class="sxs-lookup"><span data-stu-id="65d30-202">Perform hello following steps on hello **name** attribute-</span></span>

    <span data-ttu-id="65d30-203">a.</span><span class="sxs-lookup"><span data-stu-id="65d30-203">a.</span></span> <span data-ttu-id="65d30-204">Kattintson a hello attribútum tooopen hello **attribútum szerkesztése** ablak.</span><span class="sxs-lookup"><span data-stu-id="65d30-204">Click on hello attribute tooopen hello **Edit Attribute** window.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-LinkedInlookup-tutorial/url_update.png)

    <span data-ttu-id="65d30-206">b.</span><span class="sxs-lookup"><span data-stu-id="65d30-206">b.</span></span> <span data-ttu-id="65d30-207">Hello URL-érték törlése hello **névtér**.</span><span class="sxs-lookup"><span data-stu-id="65d30-207">Delete hello URL value from hello **namespace**.</span></span>
    
    <span data-ttu-id="65d30-208">c.</span><span class="sxs-lookup"><span data-stu-id="65d30-208">c.</span></span> <span data-ttu-id="65d30-209">Kattintson a **Ok** toosave hello beállítást.</span><span class="sxs-lookup"><span data-stu-id="65d30-209">Click **Ok** toosave hello setting.</span></span>

10. <span data-ttu-id="65d30-210">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello XML-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="65d30-210">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello XML file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedInlookup_certificate.png) 

11. <span data-ttu-id="65d30-212">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="65d30-212">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_400.png)

12. <span data-ttu-id="65d30-214">Nyissa meg túl**LinkedIn rendszergazdai beállítások** szakasz.</span><span class="sxs-lookup"><span data-stu-id="65d30-214">Go too**LinkedIn Admin Settings** section.</span></span> <span data-ttu-id="65d30-215">Feltöltés hello XML-fájl hello kattintva hello Azure-portálon letöltésének **feltöltése XML-fájl** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="65d30-215">Upload hello XML file you downloaded from hello Azure portal by clicking hello **Upload XML file** option.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedIn_metadata_03.png)

13. <span data-ttu-id="65d30-217">Kattintson a **a** tooenable egyszeri Bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="65d30-217">Click **On** tooenable SSO.</span></span> <span data-ttu-id="65d30-218">Egyszeri bejelentkezés állapota a **nincs csatlakoztatva** túl**csatlakoztatva**</span><span class="sxs-lookup"><span data-stu-id="65d30-218">SSO status changes from **Not Connected** too**Connected**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedIn_admin_05.png)

> [!TIP]
> <span data-ttu-id="65d30-220">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="65d30-220">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="65d30-221">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="65d30-221">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="65d30-222">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="65d30-222">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="65d30-223">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="65d30-223">Creating an Azure AD test user</span></span>
<span data-ttu-id="65d30-224">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="65d30-224">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="65d30-226">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="65d30-226">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="65d30-227">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="65d30-227">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-LinkedInlookup-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="65d30-229">Nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="65d30-229">Go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-LinkedInlookup-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="65d30-231">Kattintson a **Hozzáadás** tooopen hello **felhasználói** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="65d30-231">Click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-LinkedInlookup-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="65d30-233">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="65d30-233">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-LinkedInlookup-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="65d30-235">a.</span><span class="sxs-lookup"><span data-stu-id="65d30-235">a.</span></span> <span data-ttu-id="65d30-236">A hello **neve** szövegmezőhöz típus **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="65d30-236">In hello **Name** textbox, type **Britta Simon**.</span></span>

    <span data-ttu-id="65d30-237">b.</span><span class="sxs-lookup"><span data-stu-id="65d30-237">b.</span></span> <span data-ttu-id="65d30-238">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="65d30-238">In hello **User name** textbox, type hello **email address** of Britta Simon.</span></span>

    <span data-ttu-id="65d30-239">c.</span><span class="sxs-lookup"><span data-stu-id="65d30-239">c.</span></span> <span data-ttu-id="65d30-240">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="65d30-240">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="65d30-241">d.</span><span class="sxs-lookup"><span data-stu-id="65d30-241">d.</span></span> <span data-ttu-id="65d30-242">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="65d30-242">Click **Create**.</span></span>
 
### <a name="creating-an-linkedin-lookup-test-user"></a><span data-ttu-id="65d30-243">Egy LinkedIn keresési tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="65d30-243">Creating an LinkedIn Lookup test user</span></span>

<span data-ttu-id="65d30-244">Csatolt keresési alkalmazás támogatja a csak az idő szerinti (JIT) a felhasználók átadása, miután a felhasználók hitelesítésére hello alkalmazás automatikusan létrejönnek.</span><span class="sxs-lookup"><span data-stu-id="65d30-244">Linked Lookup Application supports Just in Time (JIT) user provisioning and after authentication users are created in hello application automatically.</span></span> <span data-ttu-id="65d30-245">Aktiválása **automatikusan a szükséges licencek kiosztása** tooassign licenc toohello felhasználó.</span><span class="sxs-lookup"><span data-stu-id="65d30-245">Activate **Automatically assign licenses** tooassign a license toohello user.</span></span>
   
   ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedin_admin_license.png)

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="65d30-247">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="65d30-247">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="65d30-248">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooLinkedIn keresési megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="65d30-248">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooLinkedIn Lookup.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="65d30-250">**tooassign Britta Simon tooLinkedIn keresési, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="65d30-250">**tooassign Britta Simon tooLinkedIn Lookup, perform hello following steps:**</span></span>

1. <span data-ttu-id="65d30-251">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="65d30-251">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="65d30-253">Hello alkalmazások listában válassza ki a **LinkedIn keresési**.</span><span class="sxs-lookup"><span data-stu-id="65d30-253">In hello applications list, select **LinkedIn Lookup**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedInlookup_app.png) 

3. <span data-ttu-id="65d30-255">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="65d30-255">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="65d30-257">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="65d30-257">Click **Add** button.</span></span> <span data-ttu-id="65d30-258">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="65d30-258">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="65d30-260">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="65d30-260">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="65d30-261">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="65d30-261">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="65d30-262">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="65d30-262">Click **Assign** button on **Add Assignment** dialog.</span></span>

    
### <a name="testing-single-sign-on"></a><span data-ttu-id="65d30-263">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="65d30-263">Testing single sign-on</span></span>

<span data-ttu-id="65d30-264">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="65d30-264">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="65d30-265">Amikor hello LinkedIn keresési hello hozzáférési Panel csempére kattint, melyekben tooprovide a személyes LinkedIn fiókadatok átirányított tooOrganizational lapon kell.</span><span class="sxs-lookup"><span data-stu-id="65d30-265">When you click hello LinkedIn Lookup tile in hello Access Panel, you should be redirected tooOrganizational page where you have tooprovide your personal LinkedIn account details.</span></span> <span data-ttu-id="65d30-266">A személyes fiókot a LinkedIn üzleti fiókjával hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="65d30-266">It links your personal account with your LinkedIn business account.</span></span> 

<span data-ttu-id="65d30-267">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="65d30-267">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="65d30-268">További források</span><span class="sxs-lookup"><span data-stu-id="65d30-268">Additional resources</span></span>

* [<span data-ttu-id="65d30-269">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="65d30-269">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="65d30-270">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="65d30-270">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_203.png

