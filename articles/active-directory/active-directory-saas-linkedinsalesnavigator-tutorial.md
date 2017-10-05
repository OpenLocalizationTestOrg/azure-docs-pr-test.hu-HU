---
title: "Oktatóanyag: Azure Active Directoryval integrált LinkedInSalesNavigator |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és LinkedInSalesNavigator között."
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
ms.openlocfilehash: ef26a16e79d9c9b0654634960b57dc59827b2c24
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-linkedin-sales-navigator"></a><span data-ttu-id="c9c84-103">Oktatóanyag: Azure Active Directory-integráció a LinkedIn értékesítési Navigator</span><span class="sxs-lookup"><span data-stu-id="c9c84-103">Tutorial: Azure Active Directory integration with LinkedIn Sales Navigator</span></span>

<span data-ttu-id="c9c84-104">Ebben az oktatóanyagban elsajátíthatja LinkedIn értékesítési Navigator integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="c9c84-104">In this tutorial, you learn how to integrate LinkedIn Sales Navigator with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="c9c84-105">LinkedIn értékesítési Navigator integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="c9c84-105">Integrating LinkedIn Sales Navigator with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="c9c84-106">Megadhatja a LinkedIn értékesítési Navigator hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="c9c84-106">You can control in Azure AD who has access to LinkedIn Sales Navigator</span></span>
- <span data-ttu-id="c9c84-107">Az Azure AD-fiókok a engedélyezheti a felhasználóknak, hogy automatikusan lekérni aláírt a LinkedIn értékesítési Navigator (egyszeri bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="c9c84-107">You can enable your users to automatically get signed-on to LinkedIn Sales Navigator (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="c9c84-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="c9c84-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="c9c84-109">Ha szeretné tudni, hogy az Azure AD SaaS integrálásáról további információkat, keresse meg a [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="c9c84-109">If you want to know more details about SaaS app integration with Azure AD, browse [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c9c84-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="c9c84-110">Prerequisites</span></span>

<span data-ttu-id="c9c84-111">Az Azure AD-integráció konfigurálása a LinkedIn értékesítési Navigator, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="c9c84-111">To configure Azure AD integration with LinkedIn Sales Navigator, you need the following items:</span></span>

- <span data-ttu-id="c9c84-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="c9c84-112">An Azure AD subscription</span></span>
- <span data-ttu-id="c9c84-113">A LinkedIn értékesítési Navigator egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="c9c84-113">A LinkedIn Sales Navigator single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="c9c84-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="c9c84-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="c9c84-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="c9c84-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="c9c84-116">Kerülje az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="c9c84-116">Avoid using your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="c9c84-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c9c84-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="c9c84-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="c9c84-118">Scenario description</span></span>
<span data-ttu-id="c9c84-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="c9c84-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="c9c84-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="c9c84-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="c9c84-121">A gyűjteményből LinkedIn értékesítési Navigator hozzáadása</span><span class="sxs-lookup"><span data-stu-id="c9c84-121">Adding LinkedIn Sales Navigator from the gallery</span></span>
2. <span data-ttu-id="c9c84-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="c9c84-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-linkedin-sales-navigator-from-the-gallery"></a><span data-ttu-id="c9c84-123">A gyűjteményből LinkedIn értékesítési Navigator hozzáadása</span><span class="sxs-lookup"><span data-stu-id="c9c84-123">Adding LinkedIn Sales Navigator from the gallery</span></span>
<span data-ttu-id="c9c84-124">Az Azure AD integrálása a LinkedIn értékesítési Navigator konfigurálásához kell hozzáadnia LinkedIn értékesítési Navigator a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="c9c84-124">To configure the integration of LinkedIn Sales Navigator into Azure AD, you need to add LinkedIn Sales Navigator from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="c9c84-125">**A gyűjteményből LinkedIn értékesítési Navigator hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="c9c84-125">**To add LinkedIn Sales Navigator from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="c9c84-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="c9c84-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="c9c84-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="c9c84-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="c9c84-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="c9c84-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="c9c84-131">Kattintson a **új alkalmazás** gombra a párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="c9c84-131">Click **New application** button on the top of the dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="c9c84-133">Írja be a keresőmezőbe, **LinkedIn értékesítési Navigator**.</span><span class="sxs-lookup"><span data-stu-id="c9c84-133">In the search box, type **LinkedIn Sales Navigator**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_search.png)

5. <span data-ttu-id="c9c84-135">Az eredmények panelen válassza ki a **LinkedIn értékesítési Navigator**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="c9c84-135">In the results panel, select **LinkedIn Sales Navigator**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="c9c84-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="c9c84-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="c9c84-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD az egyszeri bejelentkezés LinkedIn értékesítési Navigator "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="c9c84-138">In this section, you configure and test Azure AD single sign-on with LinkedIn Sales Navigator based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="c9c84-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó LinkedIn értékesítési Navigator a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="c9c84-139">For single sign-on to work, Azure AD needs to know what the counterpart user in LinkedIn Sales Navigator is to a user in Azure AD.</span></span> <span data-ttu-id="c9c84-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a LinkedIn értékesítési Navigator közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="c9c84-140">In other words, a link relationship between an Azure AD user and the related user in LinkedIn Sales Navigator needs to be established.</span></span>

<span data-ttu-id="c9c84-141">Ez a hivatkozás kapcsolat létesíti értéket rendeli az **felhasználónév** értékeként Azure AD-ben a **felhasználónév** LinkedIn értékesítési Navigator.</span><span class="sxs-lookup"><span data-stu-id="c9c84-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in LinkedIn Sales Navigator.</span></span>

<span data-ttu-id="c9c84-142">Az Azure AD egyszeri bejelentkezést a LinkedIn értékesítési Navigator tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="c9c84-142">To configure and test Azure AD single sign-on with LinkedIn Sales Navigator, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="c9c84-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="c9c84-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="c9c84-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="c9c84-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="c9c84-145">**[LinkedIn értékesítési Navigator tesztfelhasználó létrehozása](#creating-a-linkedin-sales-navigator-test-user)**  - való egy megfelelője a Britta Simon LinkedIn értékesítési Navigator, amely csatolva van a felhasználó az Azure AD ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="c9c84-145">**[Creating a LinkedIn Sales Navigator test user](#creating-a-linkedin-sales-navigator-test-user)** - to have a counterpart of Britta Simon in LinkedIn Sales Navigator that is linked to the Azure AD representation of the user.</span></span>
4. <span data-ttu-id="c9c84-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="c9c84-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="c9c84-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="c9c84-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="c9c84-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c9c84-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="c9c84-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az értékesítési Navigator LinkedIn-alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="c9c84-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your LinkedIn Sales Navigator application.</span></span>

<span data-ttu-id="c9c84-150">**Az Azure AD egyszeri bejelentkezést a LinkedIn értékesítési Navigator megadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="c9c84-150">**To configure Azure AD single sign-on with LinkedIn Sales Navigator, perform the following steps:**</span></span>

1. <span data-ttu-id="c9c84-151">Az Azure portálon a a **LinkedIn értékesítési Navigator** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="c9c84-151">In the Azure portal, on the **LinkedIn Sales Navigator** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="c9c84-153">Az a **egyszeri bejelentkezés** párbeszédpanelen, a **mód** válasszon **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="c9c84-153">On the **Single sign-on** dialog, in **Mode** select **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_samlbase.png)

3. <span data-ttu-id="c9c84-155">Egy másik webes böngészőablakban bejelentkezés a **LinkedIn értékesítési Navigator** webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="c9c84-155">In a different web browser window, sign-on to your **LinkedIn Sales Navigator** website as an administrator.</span></span>

4. <span data-ttu-id="c9c84-156">A **Account Center**, kattintson a **globális beállítások** alatt **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="c9c84-156">In **Account Center**, click **Global Settings** under **Settings**.</span></span> <span data-ttu-id="c9c84-157">Jelölje ki, **értékesítési Navigator** a legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="c9c84-157">Also, select **Sales Navigator** from the dropdown list.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedin_admin_01.png)

5. <span data-ttu-id="c9c84-159">Kattintson a **, vagy kattintson ide betölteni, és másolja az űrlap egyes mezőket** , és másolja **entitásazonosító** és **helyességi feltétel felhasználói hozzáférést (ACS) URL-cím**.</span><span class="sxs-lookup"><span data-stu-id="c9c84-159">Click **OR Click Here to load and copy individual fields from the form** and copy **Entity Id** and **Assertion Consumer Access (ACS) Url**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedin_admin_031.png)

6. <span data-ttu-id="c9c84-161">Az Azure-portál a **LinkedIn értékesítési Navigator tartomány és az URL-címek** területen tegye a következőket, ha szeretne beállítani az alkalmazás **IDP** kezdeményezett mód.</span><span class="sxs-lookup"><span data-stu-id="c9c84-161">On Azure portal, under **LinkedIn Sales Navigator Domain and URLs** section, perform the following steps if you wish to configure the application in **IDP** initiated mode.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_url1.png)

    <span data-ttu-id="c9c84-163">a.</span><span class="sxs-lookup"><span data-stu-id="c9c84-163">a.</span></span> <span data-ttu-id="c9c84-164">A a **azonosító** szövegmező, adja meg a **Entitásazonosító** másolt LinkedIn portálról</span><span class="sxs-lookup"><span data-stu-id="c9c84-164">In the **Identifier** textbox, enter the **Entity ID** copied from LinkedIn Portal</span></span> 

    <span data-ttu-id="c9c84-165">b.</span><span class="sxs-lookup"><span data-stu-id="c9c84-165">b.</span></span> <span data-ttu-id="c9c84-166">A a **válasz URL-CÍMEN** szövegmező, adja meg a **helyességi feltétel felhasználói hozzáférést (ACS) URL-cím** másolt LinkedIn portálról</span><span class="sxs-lookup"><span data-stu-id="c9c84-166">In the **Reply URL** textbox, enter the **Assertion Consumer Access (ACS) Url** copied from LinkedIn Portal</span></span>

7. <span data-ttu-id="c9c84-167">Ellenőrizze **megjelenítése speciális URL-beállításainak**, ha szeretne beállítani az alkalmazás **SP** kezdeményezett mód.</span><span class="sxs-lookup"><span data-stu-id="c9c84-167">Check **Show advanced URL settings**, if you wish to configure the application in **SP** initiated mode.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_url2.png)

    <span data-ttu-id="c9c84-169">Az a **bejelentkezési URL-cím** szövegmező, írja be az értéket a következő minta használatával:`https://www.linkedin.com/checkpoint/enterprise/login/<account id>?application=salesNavigator`</span><span class="sxs-lookup"><span data-stu-id="c9c84-169">In the **Sign-on URL** textbox, type the value using the following pattern: `https://www.linkedin.com/checkpoint/enterprise/login/<account id>?application=salesNavigator`</span></span>

8. <span data-ttu-id="c9c84-170">A **LinkedIn értékesítési Navigator** alkalmazás vár a SAML helyességi feltételek egy meghatározott formátumban, amelyhez adja hozzá az egyéni attribútum-leképezésekhez SAML-jogkivonat attribútumok konfigurációjához.</span><span class="sxs-lookup"><span data-stu-id="c9c84-170">Your **LinkedIn Sales Navigator** application expects the SAML assertions in a specific format, which requires you to add custom attribute mappings to your SAML token attributes configuration.</span></span> <span data-ttu-id="c9c84-171">Az alábbi képernyőfelvételen szereplő példán látható.</span><span class="sxs-lookup"><span data-stu-id="c9c84-171">The following screenshot shows an example.</span></span> <span data-ttu-id="c9c84-172">Az alapértelmezett érték **felhasználói azonosító** van **user.userprincipalname** de LinkedIn értékesítési Navigator vár képezhető le a felhasználó e-mail címmel.</span><span class="sxs-lookup"><span data-stu-id="c9c84-172">The default value of **User Identifier** is **user.userprincipalname** but LinkedIn Sales Navigator expects it to be mapped with the user's email address.</span></span> <span data-ttu-id="c9c84-173">Használhat **user.mail** attribútumot a listából, vagy használja a megfelelő attribútum értéket a szervezet konfiguráció alapján.</span><span class="sxs-lookup"><span data-stu-id="c9c84-173">You can use **user.mail** attribute from the list or use the appropriate attribute value based on your organization configuration.</span></span> 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinsalesnavigator-tutorial/updateusermail.png)
    
9. <span data-ttu-id="c9c84-175">A **felhasználói attribútumok** kattintson **nézet és egyéb felhasználói attribútumok szerkesztése** és attribútumainak beállítása.</span><span class="sxs-lookup"><span data-stu-id="c9c84-175">In **User Attributes** section, click **View and edit all other user attributes** and set the attributes.</span></span> <span data-ttu-id="c9c84-176">A felhasználónak kell nevű négy jogcímeket adhatnak hozzá **e-mail**, **részleg**, **Keresztnév**, és **Vezetéknév** , és le kell képezni a értéke **user.mail**, **felhasználó.részleg**, **user.givenname**, és **user.surname** kulcsattribútumokkal.</span><span class="sxs-lookup"><span data-stu-id="c9c84-176">The user needs to add four claims named **email**, **department**, **firstname**, and **lastname** and the value is to be mapped with **user.mail**, **user.department**, **user.givenname**, and **user.surname** respectively</span></span>

    | <span data-ttu-id="c9c84-177">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="c9c84-177">Attribute Name</span></span> | <span data-ttu-id="c9c84-178">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="c9c84-178">Attribute Value</span></span> |
    | --- | --- |    
    | <span data-ttu-id="c9c84-179">E-mailek</span><span class="sxs-lookup"><span data-stu-id="c9c84-179">email</span></span>| <span data-ttu-id="c9c84-180">User.mail</span><span class="sxs-lookup"><span data-stu-id="c9c84-180">user.mail</span></span> |
    | <span data-ttu-id="c9c84-181">Szervezeti egység</span><span class="sxs-lookup"><span data-stu-id="c9c84-181">department</span></span>| <span data-ttu-id="c9c84-182">felhasználó.részleg</span><span class="sxs-lookup"><span data-stu-id="c9c84-182">user.department</span></span> |
    | <span data-ttu-id="c9c84-183">Utónév</span><span class="sxs-lookup"><span data-stu-id="c9c84-183">firstname</span></span>| <span data-ttu-id="c9c84-184">User.givenName</span><span class="sxs-lookup"><span data-stu-id="c9c84-184">user.givenname</span></span> |
    | <span data-ttu-id="c9c84-185">Vezetéknév</span><span class="sxs-lookup"><span data-stu-id="c9c84-185">lastname</span></span>| <span data-ttu-id="c9c84-186">User.surname</span><span class="sxs-lookup"><span data-stu-id="c9c84-186">user.surname</span></span> |
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-linkedinsalesnavigator-tutorial/userattribute.png)
    
    <span data-ttu-id="c9c84-188">a.</span><span class="sxs-lookup"><span data-stu-id="c9c84-188">a.</span></span> <span data-ttu-id="c9c84-189">Kattintson a **attribútum hozzáadása** a attribútum párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="c9c84-189">Click on **Add Attribute** to open the attribute dialog.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_attribute_04.png)
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_attribute_05.png)
   
    <span data-ttu-id="c9c84-192">b.</span><span class="sxs-lookup"><span data-stu-id="c9c84-192">b.</span></span> <span data-ttu-id="c9c84-193">Az a **neve** szövegmező, írja be az adott sorhoz feltüntetett attribútumot nevét.</span><span class="sxs-lookup"><span data-stu-id="c9c84-193">In the **Name** textbox, type the attribute name shown for that row.</span></span>
    
    <span data-ttu-id="c9c84-194">c.</span><span class="sxs-lookup"><span data-stu-id="c9c84-194">c.</span></span> <span data-ttu-id="c9c84-195">Az a **érték** kilistázásához írja be a sorhoz látható attribútum értéke.</span><span class="sxs-lookup"><span data-stu-id="c9c84-195">From the **Value** list, type the attribute value shown for that row.</span></span>
    
    <span data-ttu-id="c9c84-196">d.</span><span class="sxs-lookup"><span data-stu-id="c9c84-196">d.</span></span> <span data-ttu-id="c9c84-197">Kattintson a **Ok**</span><span class="sxs-lookup"><span data-stu-id="c9c84-197">Click **Ok**</span></span>

10. <span data-ttu-id="c9c84-198">Hajtsa végre a következő lépéseket a **neve** attribútum -</span><span class="sxs-lookup"><span data-stu-id="c9c84-198">Perform the following steps on the **name** attribute-</span></span>

    <span data-ttu-id="c9c84-199">a.</span><span class="sxs-lookup"><span data-stu-id="c9c84-199">a.</span></span> <span data-ttu-id="c9c84-200">Az attribútum megnyitásához kattintson a **attribútum szerkesztése** ablak.</span><span class="sxs-lookup"><span data-stu-id="c9c84-200">Click on the attribute to open the **Edit Attribute** window.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinsalesnavigator-tutorial/url_update.png)

    <span data-ttu-id="c9c84-202">b.</span><span class="sxs-lookup"><span data-stu-id="c9c84-202">b.</span></span> <span data-ttu-id="c9c84-203">Az URL-cím érték törlése a **névtér**.</span><span class="sxs-lookup"><span data-stu-id="c9c84-203">Delete the URL value from the **namespace**.</span></span>
    
    <span data-ttu-id="c9c84-204">c.</span><span class="sxs-lookup"><span data-stu-id="c9c84-204">c.</span></span> <span data-ttu-id="c9c84-205">Kattintson a **Ok** a beállítás mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="c9c84-205">Click **Ok** to save the setting.</span></span>

11. <span data-ttu-id="c9c84-206">Az a **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse az XML-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="c9c84-206">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the XML file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_certificate.png) 

12. <span data-ttu-id="c9c84-208">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="c9c84-208">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_400.png)

13. <span data-ttu-id="c9c84-210">Ugrás a **LinkedIn rendszergazdai beállítások** szakasz.</span><span class="sxs-lookup"><span data-stu-id="c9c84-210">Go to **LinkedIn Admin Settings** section.</span></span> <span data-ttu-id="c9c84-211">Kattintson a **feltöltése XML-fájl** Azure-portálról letöltött metaadatok XML-fájl feltöltése.</span><span class="sxs-lookup"><span data-stu-id="c9c84-211">Click **Upload XML file** to upload the Metadata XML file that you have downloaded from the Azure portal.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedin_metadata_03.png)

14. <span data-ttu-id="c9c84-213">Kattintson a **a** SSO engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="c9c84-213">Click **On** to enable SSO.</span></span> <span data-ttu-id="c9c84-214">Egyszeri bejelentkezés állapota a **nincs csatlakoztatva** való **csatlakoztatva**</span><span class="sxs-lookup"><span data-stu-id="c9c84-214">SSO status changes from **Not Connected** to **Connected**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedin_admin_05.png)


> [!TIP]
> <span data-ttu-id="c9c84-216">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="c9c84-216">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="c9c84-217">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="c9c84-217">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="c9c84-218">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="c9c84-218">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="c9c84-219">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="c9c84-219">Creating an Azure AD test user</span></span>
<span data-ttu-id="c9c84-220">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="c9c84-220">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="c9c84-222">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="c9c84-222">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="c9c84-223">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="c9c84-223">In the **Azure  portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-linkedinsalesnavigator-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="c9c84-225">Ugrás a **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="c9c84-225">Go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-linkedinsalesnavigator-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="c9c84-227">Kattintson a párbeszédpanel tetején **Hozzáadás** megnyitásához a **felhasználói** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c9c84-227">At the top of the dialog click **Add** to open the **User** dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-linkedinsalesnavigator-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="c9c84-229">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="c9c84-229">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-linkedinsalesnavigator-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="c9c84-231">a.</span><span class="sxs-lookup"><span data-stu-id="c9c84-231">a.</span></span> <span data-ttu-id="c9c84-232">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="c9c84-232">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="c9c84-233">b.</span><span class="sxs-lookup"><span data-stu-id="c9c84-233">b.</span></span> <span data-ttu-id="c9c84-234">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="c9c84-234">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="c9c84-235">c.</span><span class="sxs-lookup"><span data-stu-id="c9c84-235">c.</span></span> <span data-ttu-id="c9c84-236">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="c9c84-236">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="c9c84-237">d.</span><span class="sxs-lookup"><span data-stu-id="c9c84-237">d.</span></span> <span data-ttu-id="c9c84-238">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="c9c84-238">Click **Create**.</span></span>
 
### <a name="creating-a-linkedin-sales-navigator-test-user"></a><span data-ttu-id="c9c84-239">LinkedIn értékesítési Navigator tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="c9c84-239">Creating a LinkedIn Sales Navigator test user</span></span>

<span data-ttu-id="c9c84-240">Csatolt értékesítési Navigator alkalmazás támogatja közvetlenül az az idő szerinti (JIT) a felhasználók átadása, miután a felhasználók hitelesítésére az alkalmazás automatikusan létrejönnek.</span><span class="sxs-lookup"><span data-stu-id="c9c84-240">Linked Sales Navigator Application supports Just in Time (JIT) user provisioning and after authentication users are created in the application automatically.</span></span> <span data-ttu-id="c9c84-241">Aktiválása **automatikusan a szükséges licencek kiosztása** licenc hozzárendelése a felhasználóhoz.</span><span class="sxs-lookup"><span data-stu-id="c9c84-241">Activate **Automatically assign licenses** to assign a license to the user.</span></span>
   
   ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-linkedinsalesnavigator-tutorial/LinkedinUserprovswitch.png)

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="c9c84-243">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="c9c84-243">Assigning the Azure AD test user</span></span>

<span data-ttu-id="c9c84-244">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés LinkedIn értékesítési Navigator Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="c9c84-244">In this section, you enable Britta Simon to use Azure single sign-on by granting access to LinkedIn Sales Navigator.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="c9c84-246">**Britta Simon hozzárendelése LinkedIn értékesítési Navigator, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="c9c84-246">**To assign Britta Simon to LinkedIn Sales Navigator, perform the following steps:**</span></span>

1. <span data-ttu-id="c9c84-247">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="c9c84-247">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="c9c84-249">Az alkalmazások listában válassza ki a **LinkedIn értékesítési Navigator**.</span><span class="sxs-lookup"><span data-stu-id="c9c84-249">In the applications list, select **LinkedIn Sales Navigator**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_app.png) 

3. <span data-ttu-id="c9c84-251">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="c9c84-251">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="c9c84-253">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="c9c84-253">Click **Add** button.</span></span> <span data-ttu-id="c9c84-254">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c9c84-254">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="c9c84-256">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="c9c84-256">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="c9c84-257">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c9c84-257">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="c9c84-258">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c9c84-258">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="c9c84-259">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="c9c84-259">Testing single sign-on</span></span>

<span data-ttu-id="c9c84-260">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="c9c84-260">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="c9c84-261">Ha a hozzáférési panelen LinkedIn értékesítési Navigator csempére kattint, szervezeti oldalra, ahol meg kell adnia a személyes LinkedIn fiók adatait a rendszer átirányítja.</span><span class="sxs-lookup"><span data-stu-id="c9c84-261">When you click the LinkedIn Sales Navigator tile in the Access Panel, you should be redirected to Organizational page where you have to provide your personal LinkedIn account details.</span></span> <span data-ttu-id="c9c84-262">A személyes fiókot a LinkedIn üzleti fiókjával hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="c9c84-262">It links your personal account with your LinkedIn business account.</span></span> <span data-ttu-id="c9c84-263">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="c9c84-263">For more information about the Access Panel, see [Introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="c9c84-264">További források</span><span class="sxs-lookup"><span data-stu-id="c9c84-264">Additional resources</span></span>

* [<span data-ttu-id="c9c84-265">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="c9c84-265">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="c9c84-266">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="c9c84-266">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



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

