---
title: "Oktatóanyag: Azure Active Directoryval integrált myPolicies |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és myPolicies között."
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
ms.openlocfilehash: fcb403041cb3a8bd20ff7b34aa5cc4b7bf0c0a16
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/29/2017
---
# <a name="tutorial-azure-active-directory-integration-with-mypolicies"></a><span data-ttu-id="439cb-103">Oktatóanyag: Azure Active Directoryval integrált myPolicies</span><span class="sxs-lookup"><span data-stu-id="439cb-103">Tutorial: Azure Active Directory integration with myPolicies</span></span>

<span data-ttu-id="439cb-104">Ebben az oktatóanyagban elsajátíthatja myPolicies integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="439cb-104">In this tutorial, you learn how to integrate myPolicies with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="439cb-105">MyPolicies integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="439cb-105">Integrating myPolicies with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="439cb-106">Megadhatja a myPolicies hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="439cb-106">You can control in Azure AD who has access to myPolicies</span></span>
- <span data-ttu-id="439cb-107">Engedélyezheti a felhasználóknak, hogy automatikusan lekérni aláírt a myPolicies (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="439cb-107">You can enable your users to automatically get signed-on to myPolicies (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="439cb-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="439cb-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="439cb-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="439cb-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="439cb-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="439cb-110">Prerequisites</span></span>

<span data-ttu-id="439cb-111">Konfigurálása az Azure AD-integrációs myPolicies, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="439cb-111">To configure Azure AD integration with myPolicies, you need the following items:</span></span>

- <span data-ttu-id="439cb-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="439cb-112">An Azure AD subscription</span></span>
- <span data-ttu-id="439cb-113">Egy myPolicies egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="439cb-113">A myPolicies single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="439cb-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="439cb-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="439cb-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="439cb-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="439cb-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="439cb-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="439cb-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="439cb-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="439cb-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="439cb-118">Scenario description</span></span>
<span data-ttu-id="439cb-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="439cb-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="439cb-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="439cb-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="439cb-121">A gyűjteményből myPolicies hozzáadása</span><span class="sxs-lookup"><span data-stu-id="439cb-121">Adding myPolicies from the gallery</span></span>
2. <span data-ttu-id="439cb-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="439cb-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-mypolicies-from-the-gallery"></a><span data-ttu-id="439cb-123">A gyűjteményből myPolicies hozzáadása</span><span class="sxs-lookup"><span data-stu-id="439cb-123">Adding myPolicies from the gallery</span></span>
<span data-ttu-id="439cb-124">Az Azure AD integrálása a myPolicies konfigurálásához kell hozzáadnia myPolicies a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="439cb-124">To configure the integration of myPolicies into Azure AD, you need to add myPolicies from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="439cb-125">**A gyűjteményből myPolicies hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="439cb-125">**To add myPolicies from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="439cb-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="439cb-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="439cb-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="439cb-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="439cb-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="439cb-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="439cb-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="439cb-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="439cb-133">Írja be a keresőmezőbe, **myPolicies**.</span><span class="sxs-lookup"><span data-stu-id="439cb-133">In the search box, type **myPolicies**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-mypolicies-tutorial/tutorial_mypolicies_search.png)

5. <span data-ttu-id="439cb-135">Az eredmények panelen válassza ki a **myPolicies**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="439cb-135">In the results panel, select **myPolicies**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-mypolicies-tutorial/tutorial_mypolicies_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="439cb-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="439cb-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="439cb-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján myPolicies.</span><span class="sxs-lookup"><span data-stu-id="439cb-138">In this section, you configure and test Azure AD single sign-on with myPolicies based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="439cb-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó myPolicies a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="439cb-139">For single sign-on to work, Azure AD needs to know what the counterpart user in myPolicies is to a user in Azure AD.</span></span> <span data-ttu-id="439cb-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a myPolicies közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="439cb-140">In other words, a link relationship between an Azure AD user and the related user in myPolicies needs to be established.</span></span>

<span data-ttu-id="439cb-141">MyPolicies, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="439cb-141">In myPolicies, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="439cb-142">Az Azure AD egyszeri bejelentkezést a myPolicies tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="439cb-142">To configure and test Azure AD single sign-on with myPolicies, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="439cb-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="439cb-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="439cb-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="439cb-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="439cb-145">**[MyPolicies tesztfelhasználó létrehozása](#creating-a-mypolicies-test-user)**  - Britta Simon egy partner, a felhasználó az Azure AD ábrázolását kapcsolódó myPolicies rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="439cb-145">**[Creating a myPolicies test user](#creating-a-mypolicies-test-user)** - to have a counterpart of Britta Simon in myPolicies that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="439cb-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="439cb-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="439cb-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="439cb-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="439cb-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="439cb-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="439cb-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az myPolicies alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="439cb-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your myPolicies application.</span></span>

<span data-ttu-id="439cb-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés myPolicies, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="439cb-150">**To configure Azure AD single sign-on with myPolicies, perform the following steps:**</span></span>

1. <span data-ttu-id="439cb-151">Az Azure portálon a a **myPolicies** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="439cb-151">In the Azure portal, on the **myPolicies** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="439cb-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="439cb-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-mypolicies-tutorial/tutorial_mypolicies_samlbase.png)

3. <span data-ttu-id="439cb-155">Az a **myPolicies tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="439cb-155">On the **myPolicies Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-mypolicies-tutorial/tutorial_mypolicies_url.png)

    <span data-ttu-id="439cb-157">a.</span><span class="sxs-lookup"><span data-stu-id="439cb-157">a.</span></span> <span data-ttu-id="439cb-158">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://<tenantname>.mypolicies.com/`</span><span class="sxs-lookup"><span data-stu-id="439cb-158">In the **Identifier** textbox, type a URL using the following pattern: `https://<tenantname>.mypolicies.com/`</span></span>

    <span data-ttu-id="439cb-159">b.</span><span class="sxs-lookup"><span data-stu-id="439cb-159">b.</span></span> <span data-ttu-id="439cb-160">Az a **válasz URL-CÍMEN** szövegmező, adja meg a következő minta használatával URL-címe:`https://<tenantname>.mypolicies.com/users/auth/saml/callback`</span><span class="sxs-lookup"><span data-stu-id="439cb-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://<tenantname>.mypolicies.com/users/auth/saml/callback`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="439cb-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="439cb-161">These values are not real.</span></span> <span data-ttu-id="439cb-162">Frissítheti ezeket az értékeket a tényleges azonosítója és a válasz URL-CÍMEN.</span><span class="sxs-lookup"><span data-stu-id="439cb-162">Update these values with the actual Identifier and Reply URL.</span></span> <span data-ttu-id="439cb-163">Ügyfél [myPolicies támogatási csoport](mailto:support@mypolicies.com) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="439cb-163">Contact [myPolicies support team](mailto:support@mypolicies.com) to get these values.</span></span>

4. <span data-ttu-id="439cb-164">A myPolicies alkalmazás a SAML helyességi feltételek egy meghatározott formátumban, amelyek megkövetelik olyan egyéni attribútum-leképezésekhez hozzáadása a SAML-jogkivonat attribútumok konfigurációs vár.</span><span class="sxs-lookup"><span data-stu-id="439cb-164">The myPolicies application expects the SAML assertions in a specific format, which requires you to add custom attribute mappings to your SAML token attributes configuration.</span></span> <span data-ttu-id="439cb-165">A következő jogcímek alkalmazás konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="439cb-165">Configure the following claims for this application.</span></span> <span data-ttu-id="439cb-166">Ezek az attribútumok értékének kezelheti a "**felhasználói attribútumok**" szakasz alkalmazás integráció lapján.</span><span class="sxs-lookup"><span data-stu-id="439cb-166">You can manage the values of these attributes from the "**User Attributes**" section on application integration page.</span></span> <span data-ttu-id="439cb-167">Az alábbi képernyőfelvételen látható egy példa a.</span><span class="sxs-lookup"><span data-stu-id="439cb-167">The following screenshot shows an example for this.</span></span> 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-mypolicies-tutorial/tutorial_mypolicies_attribute.png)

5. <span data-ttu-id="439cb-169">Kattintson a **nézet és egyéb felhasználói attribútumok szerkesztése** jelölőnégyzet a **felhasználói attribútumok** szakaszban bontsa ki az attribútumok.</span><span class="sxs-lookup"><span data-stu-id="439cb-169">Click **View and edit all other user attributes** checkbox in the **User Attributes** section to expand the attributes.</span></span> <span data-ttu-id="439cb-170">Hajtsa végre a következő lépéseket mindegyik a megjelenített attribútumok-</span><span class="sxs-lookup"><span data-stu-id="439cb-170">Perform the following steps on each of the displayed attributes-</span></span>

    | <span data-ttu-id="439cb-171">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="439cb-171">Attribute Name</span></span> | <span data-ttu-id="439cb-172">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="439cb-172">Attribute Value</span></span> |
    | ------------------- | ---------- |
    | <span data-ttu-id="439cb-173">givenName</span><span class="sxs-lookup"><span data-stu-id="439cb-173">givenname</span></span> | <span data-ttu-id="439cb-174">User.givenName</span><span class="sxs-lookup"><span data-stu-id="439cb-174">user.givenname</span></span> |
    | <span data-ttu-id="439cb-175">Vezetéknév</span><span class="sxs-lookup"><span data-stu-id="439cb-175">surname</span></span> | <span data-ttu-id="439cb-176">User.surname</span><span class="sxs-lookup"><span data-stu-id="439cb-176">user.surname</span></span> |
    | <span data-ttu-id="439cb-177">e-mail cím</span><span class="sxs-lookup"><span data-stu-id="439cb-177">emailaddress</span></span> | <span data-ttu-id="439cb-178">User.mail</span><span class="sxs-lookup"><span data-stu-id="439cb-178">user.mail</span></span> |
    | <span data-ttu-id="439cb-179">név</span><span class="sxs-lookup"><span data-stu-id="439cb-179">name</span></span> | <span data-ttu-id="439cb-180">User.userPrincipalName</span><span class="sxs-lookup"><span data-stu-id="439cb-180">user.userprincipalname</span></span> |
    
    <span data-ttu-id="439cb-181">a.</span><span class="sxs-lookup"><span data-stu-id="439cb-181">a.</span></span> <span data-ttu-id="439cb-182">Az attribútum megnyitásához kattintson a **attribútum szerkesztése** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="439cb-182">Click on the attribute to open the **Edit Attribute** dialog.</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-mypolicies-tutorial/tutorial_attribute_05.png)
    
    <span data-ttu-id="439cb-184">b.</span><span class="sxs-lookup"><span data-stu-id="439cb-184">b.</span></span> <span data-ttu-id="439cb-185">Az URL-cím érték törlése a **Namespace**.</span><span class="sxs-lookup"><span data-stu-id="439cb-185">Delete the URL value from the **Namespace**.</span></span>
    
    <span data-ttu-id="439cb-186">c.</span><span class="sxs-lookup"><span data-stu-id="439cb-186">c.</span></span> <span data-ttu-id="439cb-187">Kattintson a **Ok** a beállítás mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="439cb-187">Click **Ok** to save the setting.</span></span>
    
6. <span data-ttu-id="439cb-188">Az a **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="439cb-188">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-mypolicies-tutorial/tutorial_mypolicies_certificate.png) 

7. <span data-ttu-id="439cb-190">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="439cb-190">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-mypolicies-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="439cb-192">A a **myPolicies konfigurációs** kattintson **myPolicies konfigurálása** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="439cb-192">On the **myPolicies Configuration** section, click **Configure myPolicies** to open **Configure sign-on** window.</span></span> <span data-ttu-id="439cb-193">Másolás a **SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="439cb-193">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-mypolicies-tutorial/tutorial_mypolicies_configure.png) 

9. <span data-ttu-id="439cb-195">Egyszeri bejelentkezés konfigurálása **myPolicies** oldalon kell küldeniük a letöltött **Certificate(Base64)** és **SAML-alapú egyszeri bejelentkezési URL-címe** való [ myPolicies támogatási csoport](mailto:support@mypolicies.com).</span><span class="sxs-lookup"><span data-stu-id="439cb-195">To configure single sign-on on **myPolicies** side, you need to send the downloaded **Certificate(Base64)** and **SAML Single Sign-On Service URL** to [myPolicies support team](mailto:support@mypolicies.com).</span></span> 

> [!TIP]
> <span data-ttu-id="439cb-196">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="439cb-196">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="439cb-197">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="439cb-197">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="439cb-198">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="439cb-198">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="439cb-199">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="439cb-199">Creating an Azure AD test user</span></span>
<span data-ttu-id="439cb-200">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="439cb-200">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="439cb-202">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="439cb-202">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="439cb-203">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="439cb-203">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-mypolicies-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="439cb-205">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="439cb-205">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-mypolicies-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="439cb-207">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="439cb-207">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-mypolicies-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="439cb-209">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="439cb-209">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-mypolicies-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="439cb-211">a.</span><span class="sxs-lookup"><span data-stu-id="439cb-211">a.</span></span> <span data-ttu-id="439cb-212">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="439cb-212">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="439cb-213">b.</span><span class="sxs-lookup"><span data-stu-id="439cb-213">b.</span></span> <span data-ttu-id="439cb-214">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="439cb-214">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="439cb-215">c.</span><span class="sxs-lookup"><span data-stu-id="439cb-215">c.</span></span> <span data-ttu-id="439cb-216">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="439cb-216">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="439cb-217">d.</span><span class="sxs-lookup"><span data-stu-id="439cb-217">d.</span></span> <span data-ttu-id="439cb-218">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="439cb-218">Click **Create**.</span></span>
 
### <a name="creating-a-mypolicies-test-user"></a><span data-ttu-id="439cb-219">MyPolicies tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="439cb-219">Creating a myPolicies test user</span></span>

<span data-ttu-id="439cb-220">Ebben a szakaszban egy myPolicies Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="439cb-220">In this section, you create a user called Britta Simon in myPolicies.</span></span> <span data-ttu-id="439cb-221">Együttműködve [myPolicies támogatási csoport](mailto:support@mypolicies.com) a felhasználók hozzáadása a myPolicies platform.</span><span class="sxs-lookup"><span data-stu-id="439cb-221">Work with [myPolicies support team](mailto:support@mypolicies.com) to add the users in the myPolicies platform.</span></span> <span data-ttu-id="439cb-222">Felhasználók kell létrehoznia és aktiválni az egyszeri bejelentkezés használata előtt.</span><span class="sxs-lookup"><span data-stu-id="439cb-222">Users must be created and activated before you use single sign-on.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="439cb-223">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="439cb-223">Assigning the Azure AD test user</span></span>

<span data-ttu-id="439cb-224">Ebben a szakaszban engedélyezze Britta Simon Azure egyszeri bejelentkezéshez használandó myPolicies való hozzáférés biztosítása.</span><span class="sxs-lookup"><span data-stu-id="439cb-224">In this section, you enable Britta Simon to use Azure single sign-on by granting access to myPolicies.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="439cb-226">**Britta Simon hozzárendelése myPolicies, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="439cb-226">**To assign Britta Simon to myPolicies, perform the following steps:**</span></span>

1. <span data-ttu-id="439cb-227">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="439cb-227">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="439cb-229">Az alkalmazások listában válassza ki a **myPolicies**.</span><span class="sxs-lookup"><span data-stu-id="439cb-229">In the applications list, select **myPolicies**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-mypolicies-tutorial/tutorial_mypolicies_app.png) 

3. <span data-ttu-id="439cb-231">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="439cb-231">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="439cb-233">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="439cb-233">Click **Add** button.</span></span> <span data-ttu-id="439cb-234">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="439cb-234">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="439cb-236">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="439cb-236">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="439cb-237">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="439cb-237">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="439cb-238">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="439cb-238">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="439cb-239">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="439cb-239">Testing single sign-on</span></span>

<span data-ttu-id="439cb-240">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="439cb-240">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="439cb-241">Ha a hozzáférési panelen myPolicies csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az myPolicies alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="439cb-241">When you click the myPolicies tile in the Access Panel, you should get automatically signed-on to your myPolicies application.</span></span>
<span data-ttu-id="439cb-242">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="439cb-242">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="439cb-243">További források</span><span class="sxs-lookup"><span data-stu-id="439cb-243">Additional resources</span></span>

* [<span data-ttu-id="439cb-244">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="439cb-244">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="439cb-245">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="439cb-245">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

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

