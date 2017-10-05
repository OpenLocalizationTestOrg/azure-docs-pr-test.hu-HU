---
title: "Oktatóanyag: Azure Active Directory-integráció LinkedIn Learning segítségével |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és a LinkedIn tanulási között."
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
ms.openlocfilehash: 6ad28cb3adaa63ddc3d3769a650d26ca6a7e2695
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-linkedin-learning"></a><span data-ttu-id="6443f-103">Oktatóanyag: Azure Active Directory-integráció LinkedIn Learning segítségével</span><span class="sxs-lookup"><span data-stu-id="6443f-103">Tutorial: Azure Active Directory integration with LinkedIn Learning</span></span>

<span data-ttu-id="6443f-104">Ebben az oktatóanyagban elsajátíthatja LinkedIn tanulási integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="6443f-104">In this tutorial, you learn how to integrate LinkedIn Learning with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="6443f-105">LinkedIn tanulási integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="6443f-105">Integrating LinkedIn Learning with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="6443f-106">Megadhatja a LinkedIn tanulási hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="6443f-106">You can control in Azure AD who has access to LinkedIn Learning</span></span>
- <span data-ttu-id="6443f-107">Az Azure AD-fiókok a engedélyezheti a felhasználóknak, hogy automatikusan lekérni aláírt a LinkedIn tanulási (egyszeri bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="6443f-107">You can enable your users to automatically get signed-on to LinkedIn Learning (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="6443f-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="6443f-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="6443f-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="6443f-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6443f-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="6443f-110">Prerequisites</span></span>

<span data-ttu-id="6443f-111">Konfigurálása az Azure AD-integrációs LinkedIn tanulási, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="6443f-111">To configure Azure AD integration with LinkedIn Learning, you need the following items:</span></span>

- <span data-ttu-id="6443f-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="6443f-112">An Azure AD subscription</span></span>
- <span data-ttu-id="6443f-113">A LinkedIn tanulási egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="6443f-113">A LinkedIn Learning single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="6443f-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="6443f-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="6443f-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="6443f-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="6443f-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="6443f-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="6443f-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="6443f-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="6443f-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="6443f-118">Scenario description</span></span>
<span data-ttu-id="6443f-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="6443f-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="6443f-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="6443f-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="6443f-121">A gyűjteményből LinkedIn tanulási hozzáadása</span><span class="sxs-lookup"><span data-stu-id="6443f-121">Adding LinkedIn Learning from the gallery</span></span>
2. <span data-ttu-id="6443f-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="6443f-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-linkedin-learning-from-the-gallery"></a><span data-ttu-id="6443f-123">A gyűjteményből LinkedIn tanulási hozzáadása</span><span class="sxs-lookup"><span data-stu-id="6443f-123">Adding LinkedIn Learning from the gallery</span></span>
<span data-ttu-id="6443f-124">Az Azure AD integrálása a LinkedIn tanulási konfigurálásához kell hozzáadnia LinkedIn tanulási a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="6443f-124">To configure the integration of LinkedIn Learning into Azure AD, you need to add LinkedIn Learning from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="6443f-125">**A gyűjteményből LinkedIn tanulási hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="6443f-125">**To add LinkedIn Learning from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="6443f-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="6443f-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="6443f-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="6443f-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="6443f-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="6443f-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="6443f-131">Kattintson a **Hozzáadás** gombra a párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="6443f-131">Click **Add** button on the top of the dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="6443f-133">Írja be a keresőmezőbe, **LinkedIn tanulási**.</span><span class="sxs-lookup"><span data-stu-id="6443f-133">In the search box, type **LinkedIn Learning**.</span></span> <span data-ttu-id="6443f-134">Az eredmények panelen kattintson a **LinkedIn tanulási** hozzáadása az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="6443f-134">From results panel, click **LinkedIn Learning** to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-linkedinlearning-tutorial/tutorial-linkedinlearning_000.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="6443f-136">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="6443f-136">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="6443f-137">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD az egyszeri bejelentkezés LinkedIn tanulási "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="6443f-137">In this section, you configure and test Azure AD single sign-on with LinkedIn Learning based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="6443f-138">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó LinkedIn szeretnének a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="6443f-138">For single sign-on to work, Azure AD needs to know what the counterpart user in LinkedIn Learning is to a user in Azure AD.</span></span> <span data-ttu-id="6443f-139">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a LinkedIn tanulási közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="6443f-139">In other words, a link relationship between an Azure AD user and the related user in LinkedIn Learning needs to be established.</span></span>

<span data-ttu-id="6443f-140">Ez a hivatkozás kapcsolat létesíti értéket rendeli az **felhasználónév** értékeként Azure AD-ben a **felhasználónév** LinkedIn szeretnének.</span><span class="sxs-lookup"><span data-stu-id="6443f-140">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in LinkedIn Learning.</span></span>

<span data-ttu-id="6443f-141">Az Azure AD az egyszeri bejelentkezés LinkedIn Learning segítségével tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="6443f-141">To configure and test Azure AD single sign-on with LinkedIn Learning, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="6443f-142">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="6443f-142">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="6443f-143">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="6443f-143">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="6443f-144">**[LinkedIn tanulási tesztfelhasználó létrehozása](#creating-a-linkedin-learning-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="6443f-144">**[Creating a LinkedIn Learning test user](#creating-a-linkedin-learning-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
4. <span data-ttu-id="6443f-145">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="6443f-145">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="6443f-146">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="6443f-146">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="6443f-147">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="6443f-147">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="6443f-148">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez LinkedIn tanulási alkalmazásában.</span><span class="sxs-lookup"><span data-stu-id="6443f-148">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your LinkedIn Learning application.</span></span>

<span data-ttu-id="6443f-149">**Konfigurálása az Azure AD az egyszeri bejelentkezés LinkedIn tanulási, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="6443f-149">**To configure Azure AD single sign-on with LinkedIn Learning, perform the following steps:**</span></span>

1. <span data-ttu-id="6443f-150">Az Azure portálon a a **LinkedIn tanulási** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="6443f-150">In the Azure portal, on the **LinkedIn Learning** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="6443f-152">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="6443f-152">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinlearning-tutorial/tutorial-linkedin_01.png)

3. <span data-ttu-id="6443f-154">Egy másik webes böngészőablakban bejelentkezés a LinkedIn tanulási bérlő rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="6443f-154">In a different web browser window, sign-on to your LinkedIn Learning tenant as an administrator.</span></span>

4. <span data-ttu-id="6443f-155">A **Account Center**, kattintson a **globális beállítások** alatt **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="6443f-155">In **Account Center**, click **Global Settings** under **Settings**.</span></span> <span data-ttu-id="6443f-156">Jelölje ki, **tanulási - alapértelmezett** a legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="6443f-156">Also, select **Learning - Default** from the dropdown list.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinlearning-tutorial/tutorial_linkedin_admin_01.png)

5. <span data-ttu-id="6443f-158">Kattintson a **, vagy kattintson ide betölteni, és másolja az űrlap egyes mezőket** , és másolja **entitásazonosító** és **helyességi feltétel felhasználói hozzáférést (ACS) URL-címe**</span><span class="sxs-lookup"><span data-stu-id="6443f-158">Click **OR Click Here to load and copy individual fields from the form** and copy **Entity Id** and **Assertion Consumer Access (ACS) Url**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinlearning-tutorial/tutorial_linkedin_admin_03.png)

6. <span data-ttu-id="6443f-160">Az Azure-portál a **LinkedIn tanulási tartomány és az URL-címek**, hajtsa végre az alábbi lépéseket, ha azt szeretné, hogy az egyszeri bejelentkezés konfigurálása **IdP kezdeményezett** mód</span><span class="sxs-lookup"><span data-stu-id="6443f-160">On Azure portal, under **LinkedIn Learning Domain and URLs**, perform the following steps if you want to configure SSO in **IdP Initiated** mode</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinlearning-tutorial/tutorial_linkedin_signon_01.png)

    <span data-ttu-id="6443f-162">a.</span><span class="sxs-lookup"><span data-stu-id="6443f-162">a.</span></span> <span data-ttu-id="6443f-163">A a **azonosító** szövegmező, adja meg a **Entitásazonosító** másolt LinkedIn portálról</span><span class="sxs-lookup"><span data-stu-id="6443f-163">In the **Identifier** textbox, enter the **Entity ID** copied from LinkedIn Portal</span></span> 

    <span data-ttu-id="6443f-164">b.</span><span class="sxs-lookup"><span data-stu-id="6443f-164">b.</span></span> <span data-ttu-id="6443f-165">A a **válasz URL-CÍMEN** szövegmező, adja meg a **helyességi feltétel felhasználói hozzáférést (ACS) URL-cím** másolt LinkedIn portálról</span><span class="sxs-lookup"><span data-stu-id="6443f-165">In the **Reply URL** textbox, enter the **Assertion Consumer Access (ACS) Url** copied from LinkedIn Portal</span></span>

7. <span data-ttu-id="6443f-166">Ha azt szeretné, hogy az egyszeri bejelentkezés konfigurálása **Szolgáltató kezdeményezett**, majd kattintson a speciális URL-cím megjelenítése a konfigurációs szakaszban beállítás, és a bejelentkezési URL-cím konfigurálása a következő mintát:</span><span class="sxs-lookup"><span data-stu-id="6443f-166">If you want to configure SSO in **SP Initiated**, then click Show Advanced URL setting option in the configuration section and configure the sign-on URL with the following pattern:</span></span>

    `https://www.linkedin.com/checkpoint/enterprise/login/<AccountId>?application=learning&applicationInstanceId=<InstanceId>`

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinlearning-tutorial/tutorial_linkedin_signon_02.png)   
    
8. <span data-ttu-id="6443f-168">A LinkedIn tanulási alkalmazás a SAML helyességi feltételek egy meghatározott formátumban, amelyek megkövetelik olyan egyéni attribútum-leképezésekhez hozzáadása a SAML-jogkivonat attribútumok konfigurációs vár.</span><span class="sxs-lookup"><span data-stu-id="6443f-168">Your LinkedIn Learning application expects the SAML assertions in a specific format, which requires you to add custom attribute mappings to your SAML token attributes configuration.</span></span> <span data-ttu-id="6443f-169">Az alábbi képernyőfelvételen látható egy példa a.</span><span class="sxs-lookup"><span data-stu-id="6443f-169">The following screenshot shows an example for this.</span></span> <span data-ttu-id="6443f-170">Az alapértelmezett érték **felhasználói azonosító** van **user.userprincipalname** de LinkedIn tanulási vár a képezhető le a felhasználó e-mail címmel.</span><span class="sxs-lookup"><span data-stu-id="6443f-170">The default value of **User Identifier** is **user.userprincipalname** but LinkedIn Learning expects this to be mapped with the user's email address.</span></span> <span data-ttu-id="6443f-171">Az adott használhatja **user.mail** attribútumot a listából, vagy használja a megfelelő attribútum értéket a szervezet konfiguráció alapján.</span><span class="sxs-lookup"><span data-stu-id="6443f-171">For that you can use **user.mail** attribute from the list or use the appropriate attribute value based on your organization configuration.</span></span> 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinlearning-tutorial/updateusermail.png)
    
9. <span data-ttu-id="6443f-173">A **felhasználói attribútumok** kattintson **nézet és egyéb felhasználói attribútumok szerkesztése** és attribútumainak beállítása.</span><span class="sxs-lookup"><span data-stu-id="6443f-173">In **User Attributes** section, click **View and edit all other user attributes** and set the attributes.</span></span> <span data-ttu-id="6443f-174">A felhasználónak kell nevű négy jogcímeket adhatnak hozzá **e-mail**, **részleg**, **Keresztnév**, és **Vezetéknév** , és le kell képezni a értéke **user.mail**, **felhasználó.részleg**, **user.givenname**, és **user.surname** kulcsattribútumokkal.</span><span class="sxs-lookup"><span data-stu-id="6443f-174">The user needs to add four claims named **email**, **department**, **firstname**, and **lastname** and the value is to be mapped with **user.mail**, **user.department**, **user.givenname**, and **user.surname** respectively</span></span>

    | <span data-ttu-id="6443f-175">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="6443f-175">Attribute Name</span></span> | <span data-ttu-id="6443f-176">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="6443f-176">Attribute Value</span></span> |
    | --- | --- |
    | <span data-ttu-id="6443f-177">E-mailek</span><span class="sxs-lookup"><span data-stu-id="6443f-177">email</span></span>| <span data-ttu-id="6443f-178">User.mail</span><span class="sxs-lookup"><span data-stu-id="6443f-178">user.mail</span></span> |    
    | <span data-ttu-id="6443f-179">Szervezeti egység</span><span class="sxs-lookup"><span data-stu-id="6443f-179">department</span></span>| <span data-ttu-id="6443f-180">felhasználó.részleg</span><span class="sxs-lookup"><span data-stu-id="6443f-180">user.department</span></span> |
    | <span data-ttu-id="6443f-181">Utónév</span><span class="sxs-lookup"><span data-stu-id="6443f-181">firstname</span></span>| <span data-ttu-id="6443f-182">User.givenName</span><span class="sxs-lookup"><span data-stu-id="6443f-182">user.givenname</span></span> |
    | <span data-ttu-id="6443f-183">Vezetéknév</span><span class="sxs-lookup"><span data-stu-id="6443f-183">lastname</span></span>| <span data-ttu-id="6443f-184">User.surname</span><span class="sxs-lookup"><span data-stu-id="6443f-184">user.surname</span></span> |
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-linkedinlearning-tutorial/userattribute.png)
    
    <span data-ttu-id="6443f-186">a.</span><span class="sxs-lookup"><span data-stu-id="6443f-186">a.</span></span> <span data-ttu-id="6443f-187">Kattintson a **attribútum hozzáadása** a attribútum párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="6443f-187">Click **Add Attribute** to open the attribute dialog.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-linkedinLearning-tutorial/tutorial_attribute_04.png)

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-linkedinLearning-tutorial/tutorial_attribute_05.png)
    
    <span data-ttu-id="6443f-190">b.</span><span class="sxs-lookup"><span data-stu-id="6443f-190">b.</span></span> <span data-ttu-id="6443f-191">Az a **neve** szövegmező, írja be az adott sorhoz feltüntetett attribútumot nevét.</span><span class="sxs-lookup"><span data-stu-id="6443f-191">In the **Name** textbox, type the attribute name shown for that row.</span></span>
    
    <span data-ttu-id="6443f-192">c.</span><span class="sxs-lookup"><span data-stu-id="6443f-192">c.</span></span> <span data-ttu-id="6443f-193">Az a **érték** kilistázásához írja be a sorhoz látható attribútum értéke.</span><span class="sxs-lookup"><span data-stu-id="6443f-193">From the **Value** list, type the attribute value shown for that row.</span></span>
    
    <span data-ttu-id="6443f-194">d.</span><span class="sxs-lookup"><span data-stu-id="6443f-194">d.</span></span> <span data-ttu-id="6443f-195">Kattintson a **Ok**</span><span class="sxs-lookup"><span data-stu-id="6443f-195">Click **Ok**</span></span>

10. <span data-ttu-id="6443f-196">Hajtsa végre a következő lépéseket a **neve** attribútum -</span><span class="sxs-lookup"><span data-stu-id="6443f-196">Perform the following steps on the **name** attribute-</span></span>

    <span data-ttu-id="6443f-197">a.</span><span class="sxs-lookup"><span data-stu-id="6443f-197">a.</span></span> <span data-ttu-id="6443f-198">Az attribútum megnyitásához kattintson a **attribútum szerkesztése** ablak.</span><span class="sxs-lookup"><span data-stu-id="6443f-198">Click on the attribute to open the **Edit Attribute** window.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinLearning-tutorial/url_update.png)

    <span data-ttu-id="6443f-200">b.</span><span class="sxs-lookup"><span data-stu-id="6443f-200">b.</span></span> <span data-ttu-id="6443f-201">Az URL-cím érték törlése a **névtér**.</span><span class="sxs-lookup"><span data-stu-id="6443f-201">Delete the URL value from the **namespace**.</span></span>
    
    <span data-ttu-id="6443f-202">c.</span><span class="sxs-lookup"><span data-stu-id="6443f-202">c.</span></span> <span data-ttu-id="6443f-203">Kattintson a **Ok** a beállítás mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="6443f-203">Click **Ok** to save the setting.</span></span>

11. <span data-ttu-id="6443f-204">Az a **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse az XML-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="6443f-204">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the XML file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinlearning-tutorial/tutorial-linkedinlearning_certificate.png) 

12. <span data-ttu-id="6443f-206">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="6443f-206">Click **Save**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_400.png)

13. <span data-ttu-id="6443f-208">Ugrás a **LinkedIn rendszergazdai beállítások** szakasz.</span><span class="sxs-lookup"><span data-stu-id="6443f-208">Go to **LinkedIn Admin Settings** section.</span></span> <span data-ttu-id="6443f-209">A feltöltés XML-fájl lehetőségre kattintva Azure-portálról letöltött XML-fájl feltöltése.</span><span class="sxs-lookup"><span data-stu-id="6443f-209">Upload the XML file you downloaded from the Azure portal by clicking the Upload XML file option.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinlearning-tutorial/tutorial_linkedin_metadata_03.png)

14. <span data-ttu-id="6443f-211">Kattintson a **a** SSO engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="6443f-211">Click **On** to enable SSO.</span></span> <span data-ttu-id="6443f-212">Egyszeri bejelentkezés állapota a **nincs csatlakoztatva** való **csatlakoztatva**</span><span class="sxs-lookup"><span data-stu-id="6443f-212">SSO status changes from **Not Connected** to **Connected**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinlearning-tutorial/tutorial_linkedin_admin_05.png)

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="6443f-214">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="6443f-214">Creating an Azure AD test user</span></span>
<span data-ttu-id="6443f-215">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="6443f-215">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="6443f-217">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="6443f-217">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="6443f-218">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="6443f-218">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-linkedinlearning-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="6443f-220">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="6443f-220">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-linkedinlearning-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="6443f-222">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="6443f-222">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-linkedinlearning-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="6443f-224">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="6443f-224">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-linkedinlearning-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="6443f-226">a.</span><span class="sxs-lookup"><span data-stu-id="6443f-226">a.</span></span> <span data-ttu-id="6443f-227">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="6443f-227">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="6443f-228">b.</span><span class="sxs-lookup"><span data-stu-id="6443f-228">b.</span></span> <span data-ttu-id="6443f-229">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="6443f-229">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="6443f-230">c.</span><span class="sxs-lookup"><span data-stu-id="6443f-230">c.</span></span> <span data-ttu-id="6443f-231">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="6443f-231">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="6443f-232">d.</span><span class="sxs-lookup"><span data-stu-id="6443f-232">d.</span></span> <span data-ttu-id="6443f-233">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="6443f-233">Click **Create**.</span></span> 

### <a name="creating-a-linkedin-learning-test-user"></a><span data-ttu-id="6443f-234">LinkedIn tanulási tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="6443f-234">Creating a LinkedIn Learning test user</span></span>

<span data-ttu-id="6443f-235">Csatolt tanulási alkalmazás támogatja.</span><span class="sxs-lookup"><span data-stu-id="6443f-235">Linked Learning Application supports.</span></span> <span data-ttu-id="6443f-236">Csak az idő a felhasználók átadása, és a hitelesítés után felhasználók automatikusan jönnek létre az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="6443f-236">Just in time user provisioning and after authentication users are created in the application automatically.</span></span> <span data-ttu-id="6443f-237">A rendszergazda a beállítások lapon a a LinkedIn tanulási portál tükrözés a kapcsoló **automatikus hozzárendelése licencek** közvetlenül ahhoz, hogy időben aktív kiépítés, és ez lesz is rendel egy licencet a felhasználó.</span><span class="sxs-lookup"><span data-stu-id="6443f-237">On the admin settings page on the LinkedIn Learning portal flip the switch **Automatically Assign licenses** to active to enable Just in time provisioning and this will also assign a license to the user.</span></span>
   
   ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-linkedinLearning-tutorial/LinkedinUserprovswitch.png)

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="6443f-239">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="6443f-239">Assigning the Azure AD test user</span></span>

<span data-ttu-id="6443f-240">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés LinkedIn Learning Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="6443f-240">In this section, you enable Britta Simon to use Azure single sign-on by granting access to LinkedIn Learning.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="6443f-242">**Britta Simon hozzárendelése LinkedIn tanulási, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="6443f-242">**To assign Britta Simon to LinkedIn Learning, perform the following steps:**</span></span>

1. <span data-ttu-id="6443f-243">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="6443f-243">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="6443f-245">Az alkalmazások listában válassza ki a **LinkedIn tanulási**.</span><span class="sxs-lookup"><span data-stu-id="6443f-245">In the applications list, select **LinkedIn Learning**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinlearning-tutorial/tutorial-linkedinlearning_0001.png) 

3. <span data-ttu-id="6443f-247">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="6443f-247">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="6443f-249">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="6443f-249">Click **Add** button.</span></span> <span data-ttu-id="6443f-250">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="6443f-250">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="6443f-252">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="6443f-252">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="6443f-253">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="6443f-253">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="6443f-254">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="6443f-254">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="6443f-255">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="6443f-255">Testing single sign-on</span></span>

<span data-ttu-id="6443f-256">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="6443f-256">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="6443f-257">Ha a hozzáférési Panel LinkedIn tanulási mozaik gombra kattint, szerezheti be az Azure bejelentkezési oldalra, és a sikeres bejelentkezést, után szerezheti be az LinkedIn tanulási alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="6443f-257">When you click the LinkedIn Learning tile in the Access Panel, you should get the Azure Sign-on page and on after successful sign-on, you should get into your LinkedIn Learning application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6443f-258">További források</span><span class="sxs-lookup"><span data-stu-id="6443f-258">Additional resources</span></span>

* [<span data-ttu-id="6443f-259">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="6443f-259">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="6443f-260">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="6443f-260">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

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