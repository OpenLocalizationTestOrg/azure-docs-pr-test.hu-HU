---
title: "Oktatóanyag: Azure Active Directory-integráció LinkedIn Learning segítségével |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a LinkedIn tanulási között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d5857070-bf79-4bd3-9a2a-4c1919a74946
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/15/2017
ms.author: jeedes
ms.openlocfilehash: 14610a25132ed0ccf5892cad6ccc4e1ef03ff280
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-linkedin-learning"></a><span data-ttu-id="29b23-103">Oktatóanyag: Azure Active Directory-integráció LinkedIn Learning segítségével</span><span class="sxs-lookup"><span data-stu-id="29b23-103">Tutorial: Azure Active Directory integration with LinkedIn Learning</span></span>

<span data-ttu-id="29b23-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate LinkedIn Learning Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="29b23-104">In this tutorial, you learn how toointegrate LinkedIn Learning with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="29b23-105">LinkedIn tanulási integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="29b23-105">Integrating LinkedIn Learning with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="29b23-106">Szabályozhatja az Azure AD hozzáférési tooLinkedIn rendelkező tanulási</span><span class="sxs-lookup"><span data-stu-id="29b23-106">You can control in Azure AD who has access tooLinkedIn Learning</span></span>
- <span data-ttu-id="29b23-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooLinkedIn tanulási (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="29b23-107">You can enable your users tooautomatically get signed-on tooLinkedIn Learning (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="29b23-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="29b23-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="29b23-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="29b23-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="29b23-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="29b23-110">Prerequisites</span></span>

<span data-ttu-id="29b23-111">tooconfigure LinkedIn Learning Azure AD integrálása, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="29b23-111">tooconfigure Azure AD integration with LinkedIn Learning, you need hello following items:</span></span>

- <span data-ttu-id="29b23-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="29b23-112">An Azure AD subscription</span></span>
- <span data-ttu-id="29b23-113">A LinkedIn tanulási egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="29b23-113">A LinkedIn Learning single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="29b23-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="29b23-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="29b23-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="29b23-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="29b23-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="29b23-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="29b23-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="29b23-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="29b23-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="29b23-118">Scenario description</span></span>
<span data-ttu-id="29b23-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="29b23-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="29b23-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="29b23-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="29b23-121">Hello gyűjteményből LinkedIn tanulási hozzáadása</span><span class="sxs-lookup"><span data-stu-id="29b23-121">Adding LinkedIn Learning from hello gallery</span></span>
2. <span data-ttu-id="29b23-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="29b23-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-linkedin-learning-from-hello-gallery"></a><span data-ttu-id="29b23-123">Hello gyűjteményből LinkedIn tanulási hozzáadása</span><span class="sxs-lookup"><span data-stu-id="29b23-123">Adding LinkedIn Learning from hello gallery</span></span>
<span data-ttu-id="29b23-124">tooconfigure hello integrációs LinkedIn tanulás az Azure AD-be, meg kell tooadd LinkedIn tanulási hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="29b23-124">tooconfigure hello integration of LinkedIn Learning into Azure AD, you need tooadd LinkedIn Learning from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="29b23-125">**tooadd LinkedIn tanulási hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="29b23-125">**tooadd LinkedIn Learning from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="29b23-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="29b23-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="29b23-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="29b23-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="29b23-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="29b23-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="29b23-131">Kattintson a **Hozzáadás** hello párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="29b23-131">Click **Add** button on hello top of hello dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="29b23-133">Hello keresési mezőbe, írja be a **LinkedIn tanulási**.</span><span class="sxs-lookup"><span data-stu-id="29b23-133">In hello search box, type **LinkedIn Learning**.</span></span> <span data-ttu-id="29b23-134">Az eredmények panelen kattintson a **LinkedIn tanulási** tooadd hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="29b23-134">From results panel, click **LinkedIn Learning** tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-linkedinlearning-tutorial/tutorial-linkedinlearning_000.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="29b23-136">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="29b23-136">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="29b23-137">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD az egyszeri bejelentkezés LinkedIn tanulási "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="29b23-137">In this section, you configure and test Azure AD single sign-on with LinkedIn Learning based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="29b23-138">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói LinkedIn szeretnének tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="29b23-138">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in LinkedIn Learning is tooa user in Azure AD.</span></span> <span data-ttu-id="29b23-139">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználói hello LinkedIn szeretnének közötti kapcsolat kapcsolatot kell toobe létrejött.</span><span class="sxs-lookup"><span data-stu-id="29b23-139">In other words, a link relationship between an Azure AD user and hello related user in LinkedIn Learning needs toobe established.</span></span>

<span data-ttu-id="29b23-140">Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** LinkedIn szeretnének.</span><span class="sxs-lookup"><span data-stu-id="29b23-140">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in LinkedIn Learning.</span></span>

<span data-ttu-id="29b23-141">tooconfigure és a LinkedIn Learning segítségével az Azure AD az egyszeri bejelentkezés tesztelése, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="29b23-141">tooconfigure and test Azure AD single sign-on with LinkedIn Learning, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="29b23-142">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="29b23-142">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="29b23-143">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="29b23-143">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="29b23-144">**[LinkedIn tanulási tesztfelhasználó létrehozása](#creating-a-linkedin-learning-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="29b23-144">**[Creating a LinkedIn Learning test user](#creating-a-linkedin-learning-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
4. <span data-ttu-id="29b23-145">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="29b23-145">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="29b23-146">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="29b23-146">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="29b23-147">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="29b23-147">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="29b23-148">Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és az LinkedIn tanulási alkalmazásban egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="29b23-148">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your LinkedIn Learning application.</span></span>

<span data-ttu-id="29b23-149">**az Azure AD tooconfigure egyszeri bejelentkezés LinkedIn Learning segítségével, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="29b23-149">**tooconfigure Azure AD single sign-on with LinkedIn Learning, perform hello following steps:**</span></span>

1. <span data-ttu-id="29b23-150">Az Azure portál, a hello hello **LinkedIn tanulási** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="29b23-150">In hello Azure portal, on hello **LinkedIn Learning** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="29b23-152">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="29b23-152">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinlearning-tutorial/tutorial-linkedin_01.png)

3. <span data-ttu-id="29b23-154">Egy másik webes böngészőablakban, bejelentkezés tooyour LinkedIn tanulási Bérlői rendszergazda.</span><span class="sxs-lookup"><span data-stu-id="29b23-154">In a different web browser window, sign-on tooyour LinkedIn Learning tenant as an administrator.</span></span>

4. <span data-ttu-id="29b23-155">A **Account Center**, kattintson a **globális beállítások** alatt **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="29b23-155">In **Account Center**, click **Global Settings** under **Settings**.</span></span> <span data-ttu-id="29b23-156">Jelölje ki, **tanulási - alapértelmezett** hello legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="29b23-156">Also, select **Learning - Default** from hello dropdown list.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinlearning-tutorial/tutorial_linkedin_admin_01.png)

5. <span data-ttu-id="29b23-158">Kattintson a **, vagy kattintson ide tooload, és másolja az egyes mezők hello űrlapból** , és másolja **entitásazonosító** és **helyességi feltétel felhasználói hozzáférést (ACS) URL-címe**</span><span class="sxs-lookup"><span data-stu-id="29b23-158">Click **OR Click Here tooload and copy individual fields from hello form** and copy **Entity Id** and **Assertion Consumer Access (ACS) Url**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinlearning-tutorial/tutorial_linkedin_admin_03.png)

6. <span data-ttu-id="29b23-160">Az Azure-portál a **LinkedIn tanulási tartomány és az URL-címek**, hajtsa végre a következő lépéseket, ha azt szeretné, hogy egyszeri Bejelentkezéses tooconfigure hello a **IdP kezdeményezett** mód</span><span class="sxs-lookup"><span data-stu-id="29b23-160">On Azure portal, under **LinkedIn Learning Domain and URLs**, perform hello following steps if you want tooconfigure SSO in **IdP Initiated** mode</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinlearning-tutorial/tutorial_linkedin_signon_01.png)

    <span data-ttu-id="29b23-162">a.</span><span class="sxs-lookup"><span data-stu-id="29b23-162">a.</span></span> <span data-ttu-id="29b23-163">A hello **azonosítója** szövegmező, adja meg a hello **Entitásazonosító** LinkedIn Portal átmásolva</span><span class="sxs-lookup"><span data-stu-id="29b23-163">In hello **Identifier** textbox, enter hello **Entity ID** copied from LinkedIn Portal</span></span> 

    <span data-ttu-id="29b23-164">b.</span><span class="sxs-lookup"><span data-stu-id="29b23-164">b.</span></span> <span data-ttu-id="29b23-165">A hello **válasz URL-CÍMEN** szövegmezőhöz adja meg a hello **helyességi feltétel felhasználói hozzáférést (ACS) URL-cím** LinkedIn Portal átmásolva</span><span class="sxs-lookup"><span data-stu-id="29b23-165">In hello **Reply URL** textbox, enter hello **Assertion Consumer Access (ACS) Url** copied from LinkedIn Portal</span></span>

7. <span data-ttu-id="29b23-166">Ha azt szeretné, hogy egyszeri Bejelentkezéses tooconfigure a **Szolgáltató kezdeményezett**, majd kattintson a speciális URL-cím megjelenítése beállítás hello konfigurációs szakaszban, és hello bejelentkezési URL-cím a következő mintát hello konfigurálása:</span><span class="sxs-lookup"><span data-stu-id="29b23-166">If you want tooconfigure SSO in **SP Initiated**, then click Show Advanced URL setting option in hello configuration section and configure hello sign-on URL with hello following pattern:</span></span>

    `https://www.linkedin.com/checkpoint/enterprise/login/<AccountId>?application=learning&applicationInstanceId=<InstanceId>`

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinlearning-tutorial/tutorial_linkedin_signon_02.png)   
    
8. <span data-ttu-id="29b23-168">A LinkedIn tanulási alkalmazás hello SAML helyességi feltételek vár egy meghatározott formátumban, és hogy tooadd egyéni attribútum hozzárendelések tooyour SAML token attribútumok konfigurációt igényel.</span><span class="sxs-lookup"><span data-stu-id="29b23-168">Your LinkedIn Learning application expects hello SAML assertions in a specific format, which requires you tooadd custom attribute mappings tooyour SAML token attributes configuration.</span></span> <span data-ttu-id="29b23-169">a következő képernyőkép hello ezen mutat egy példát.</span><span class="sxs-lookup"><span data-stu-id="29b23-169">hello following screenshot shows an example for this.</span></span> <span data-ttu-id="29b23-170">az alapértelmezett érték hello **felhasználói azonosító** van **user.userprincipalname** LinkedIn tanulási vár a toobe hello a felhasználó e-mail címét leképezve, de.</span><span class="sxs-lookup"><span data-stu-id="29b23-170">hello default value of **User Identifier** is **user.userprincipalname** but LinkedIn Learning expects this toobe mapped with hello user's email address.</span></span> <span data-ttu-id="29b23-171">Az adott használhatja **user.mail** hello lista attribútumot, vagy használja a szervezet konfiguráció alapján hello megfelelő attribútum értéke.</span><span class="sxs-lookup"><span data-stu-id="29b23-171">For that you can use **user.mail** attribute from hello list or use hello appropriate attribute value based on your organization configuration.</span></span> 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinlearning-tutorial/updateusermail.png)
    
9. <span data-ttu-id="29b23-173">A **felhasználói attribútumok** kattintson **nézet és egyéb felhasználói attribútumok szerkesztése** és hello attribútumainak beállítása.</span><span class="sxs-lookup"><span data-stu-id="29b23-173">In **User Attributes** section, click **View and edit all other user attributes** and set hello attributes.</span></span> <span data-ttu-id="29b23-174">hello felhasználó számára szükséges tooadd négy jogcímek nevű **e-mail**, **részleg**, **Keresztnév**, és **Vezetéknév** hello értéke pedig leképezve toobe **user.mail**, **felhasználó.részleg**, **user.givenname**, és **user.surname** kulcsattribútumokkal.</span><span class="sxs-lookup"><span data-stu-id="29b23-174">hello user needs tooadd four claims named **email**, **department**, **firstname**, and **lastname** and hello value is toobe mapped with **user.mail**, **user.department**, **user.givenname**, and **user.surname** respectively</span></span>

    | <span data-ttu-id="29b23-175">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="29b23-175">Attribute Name</span></span> | <span data-ttu-id="29b23-176">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="29b23-176">Attribute Value</span></span> |
    | --- | --- |
    | <span data-ttu-id="29b23-177">E-mailek</span><span class="sxs-lookup"><span data-stu-id="29b23-177">email</span></span>| <span data-ttu-id="29b23-178">User.mail</span><span class="sxs-lookup"><span data-stu-id="29b23-178">user.mail</span></span> |    
    | <span data-ttu-id="29b23-179">Szervezeti egység</span><span class="sxs-lookup"><span data-stu-id="29b23-179">department</span></span>| <span data-ttu-id="29b23-180">felhasználó.részleg</span><span class="sxs-lookup"><span data-stu-id="29b23-180">user.department</span></span> |
    | <span data-ttu-id="29b23-181">Utónév</span><span class="sxs-lookup"><span data-stu-id="29b23-181">firstname</span></span>| <span data-ttu-id="29b23-182">User.givenName</span><span class="sxs-lookup"><span data-stu-id="29b23-182">user.givenname</span></span> |
    | <span data-ttu-id="29b23-183">Vezetéknév</span><span class="sxs-lookup"><span data-stu-id="29b23-183">lastname</span></span>| <span data-ttu-id="29b23-184">User.surname</span><span class="sxs-lookup"><span data-stu-id="29b23-184">user.surname</span></span> |
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-linkedinlearning-tutorial/userattribute.png)
    
    <span data-ttu-id="29b23-186">a.</span><span class="sxs-lookup"><span data-stu-id="29b23-186">a.</span></span> <span data-ttu-id="29b23-187">Kattintson a **attribútum hozzáadása** tooopen hello attribútum párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="29b23-187">Click **Add Attribute** tooopen hello attribute dialog.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-linkedinLearning-tutorial/tutorial_attribute_04.png)

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-linkedinLearning-tutorial/tutorial_attribute_05.png)
    
    <span data-ttu-id="29b23-190">b.</span><span class="sxs-lookup"><span data-stu-id="29b23-190">b.</span></span> <span data-ttu-id="29b23-191">A hello **neve** szövegmezőben, az adott sorhoz feltüntetett hello attribútum neve.</span><span class="sxs-lookup"><span data-stu-id="29b23-191">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>
    
    <span data-ttu-id="29b23-192">c.</span><span class="sxs-lookup"><span data-stu-id="29b23-192">c.</span></span> <span data-ttu-id="29b23-193">A hello **érték** listájában, hello attribútuma Típusérték az adott sorhoz feltüntetett.</span><span class="sxs-lookup"><span data-stu-id="29b23-193">From hello **Value** list, type hello attribute value shown for that row.</span></span>
    
    <span data-ttu-id="29b23-194">d.</span><span class="sxs-lookup"><span data-stu-id="29b23-194">d.</span></span> <span data-ttu-id="29b23-195">Kattintson a **Ok**</span><span class="sxs-lookup"><span data-stu-id="29b23-195">Click **Ok**</span></span>

10. <span data-ttu-id="29b23-196">Hajtsa végre a következő lépéseket a hello hello **neve** attribútum -</span><span class="sxs-lookup"><span data-stu-id="29b23-196">Perform hello following steps on hello **name** attribute-</span></span>

    <span data-ttu-id="29b23-197">a.</span><span class="sxs-lookup"><span data-stu-id="29b23-197">a.</span></span> <span data-ttu-id="29b23-198">Kattintson a hello attribútum tooopen hello **attribútum szerkesztése** ablak.</span><span class="sxs-lookup"><span data-stu-id="29b23-198">Click on hello attribute tooopen hello **Edit Attribute** window.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinLearning-tutorial/url_update.png)

    <span data-ttu-id="29b23-200">b.</span><span class="sxs-lookup"><span data-stu-id="29b23-200">b.</span></span> <span data-ttu-id="29b23-201">Hello URL-érték törlése hello **névtér**.</span><span class="sxs-lookup"><span data-stu-id="29b23-201">Delete hello URL value from hello **namespace**.</span></span>
    
    <span data-ttu-id="29b23-202">c.</span><span class="sxs-lookup"><span data-stu-id="29b23-202">c.</span></span> <span data-ttu-id="29b23-203">Kattintson a **Ok** toosave hello beállítást.</span><span class="sxs-lookup"><span data-stu-id="29b23-203">Click **Ok** toosave hello setting.</span></span>

11. <span data-ttu-id="29b23-204">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello XML-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="29b23-204">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello XML file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinlearning-tutorial/tutorial-linkedinlearning_certificate.png) 

12. <span data-ttu-id="29b23-206">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="29b23-206">Click **Save**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_400.png)

13. <span data-ttu-id="29b23-208">Nyissa meg túl**LinkedIn rendszergazdai beállítások** szakasz.</span><span class="sxs-lookup"><span data-stu-id="29b23-208">Go too**LinkedIn Admin Settings** section.</span></span> <span data-ttu-id="29b23-209">Hello feltöltése XML-fájlba lehetőségre kattintva hello Azure-portálon letöltésének hello XML-fájl feltöltése.</span><span class="sxs-lookup"><span data-stu-id="29b23-209">Upload hello XML file you downloaded from hello Azure portal by clicking hello Upload XML file option.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinlearning-tutorial/tutorial_linkedin_metadata_03.png)

14. <span data-ttu-id="29b23-211">Kattintson a **a** tooenable egyszeri Bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="29b23-211">Click **On** tooenable SSO.</span></span> <span data-ttu-id="29b23-212">Egyszeri bejelentkezés állapota a **nincs csatlakoztatva** túl**csatlakoztatva**</span><span class="sxs-lookup"><span data-stu-id="29b23-212">SSO status changes from **Not Connected** too**Connected**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinlearning-tutorial/tutorial_linkedin_admin_05.png)

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="29b23-214">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="29b23-214">Creating an Azure AD test user</span></span>
<span data-ttu-id="29b23-215">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="29b23-215">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="29b23-217">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="29b23-217">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="29b23-218">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="29b23-218">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-linkedinlearning-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="29b23-220">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="29b23-220">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-linkedinlearning-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="29b23-222">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="29b23-222">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-linkedinlearning-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="29b23-224">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="29b23-224">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-linkedinlearning-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="29b23-226">a.</span><span class="sxs-lookup"><span data-stu-id="29b23-226">a.</span></span> <span data-ttu-id="29b23-227">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="29b23-227">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="29b23-228">b.</span><span class="sxs-lookup"><span data-stu-id="29b23-228">b.</span></span> <span data-ttu-id="29b23-229">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="29b23-229">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="29b23-230">c.</span><span class="sxs-lookup"><span data-stu-id="29b23-230">c.</span></span> <span data-ttu-id="29b23-231">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="29b23-231">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="29b23-232">d.</span><span class="sxs-lookup"><span data-stu-id="29b23-232">d.</span></span> <span data-ttu-id="29b23-233">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="29b23-233">Click **Create**.</span></span> 

### <a name="creating-a-linkedin-learning-test-user"></a><span data-ttu-id="29b23-234">LinkedIn tanulási tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="29b23-234">Creating a LinkedIn Learning test user</span></span>

<span data-ttu-id="29b23-235">Csatolt tanulási alkalmazás támogatja.</span><span class="sxs-lookup"><span data-stu-id="29b23-235">Linked Learning Application supports.</span></span> <span data-ttu-id="29b23-236">Csak az idő a felhasználók átadása, és a hitelesítés után felhasználók hello alkalmazás automatikusan létrejönnek.</span><span class="sxs-lookup"><span data-stu-id="29b23-236">Just in time user provisioning and after authentication users are created in hello application automatically.</span></span> <span data-ttu-id="29b23-237">Hello rendszergazdai beállítások lapján hello LinkedIn tanulási portál tükrözés hello kapcsolón **automatikus hozzárendelése licencek** tooactive tooenable csak idő üzembe helyezése, és ez is hozzárendelése a licenc toohello felhasználó.</span><span class="sxs-lookup"><span data-stu-id="29b23-237">On hello admin settings page on hello LinkedIn Learning portal flip hello switch **Automatically Assign licenses** tooactive tooenable Just in time provisioning and this will also assign a license toohello user.</span></span>
   
   ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-linkedinLearning-tutorial/LinkedinUserprovswitch.png)

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="29b23-239">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="29b23-239">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="29b23-240">Ebben a szakaszban hozzáférés tooLinkedIn megadásával engedélyeznie Britta Simon toouse Azure egyszeri bejelentkezés tanulási.</span><span class="sxs-lookup"><span data-stu-id="29b23-240">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooLinkedIn Learning.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="29b23-242">**tooassign Britta Simon tooLinkedIn tanulás, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="29b23-242">**tooassign Britta Simon tooLinkedIn Learning, perform hello following steps:**</span></span>

1. <span data-ttu-id="29b23-243">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="29b23-243">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="29b23-245">Hello alkalmazások listában válassza ki a **LinkedIn tanulási**.</span><span class="sxs-lookup"><span data-stu-id="29b23-245">In hello applications list, select **LinkedIn Learning**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinlearning-tutorial/tutorial-linkedinlearning_0001.png) 

3. <span data-ttu-id="29b23-247">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="29b23-247">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="29b23-249">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="29b23-249">Click **Add** button.</span></span> <span data-ttu-id="29b23-250">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="29b23-250">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="29b23-252">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="29b23-252">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="29b23-253">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="29b23-253">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="29b23-254">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="29b23-254">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="29b23-255">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="29b23-255">Testing single sign-on</span></span>

<span data-ttu-id="29b23-256">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="29b23-256">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="29b23-257">Hello LinkedIn tanulási csempe a hozzáférési Panel hello kattintáskor szerezheti be hello Azure bejelentkezési oldalra, és a után sikeres bejelentkezés, szerezheti be a LinkedIn Learning alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="29b23-257">When you click hello LinkedIn Learning tile in hello Access Panel, you should get hello Azure Sign-on page and on after successful sign-on, you should get into your LinkedIn Learning application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="29b23-258">További források</span><span class="sxs-lookup"><span data-stu-id="29b23-258">Additional resources</span></span>

* [<span data-ttu-id="29b23-259">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="29b23-259">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="29b23-260">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="29b23-260">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_203.png