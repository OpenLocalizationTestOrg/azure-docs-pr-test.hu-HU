---
title: "Oktatóanyag: Azure Active Directoryval integrált LinkedIn jogosultságszint-emelés |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és a LinkedIn jogosultságszint-emelés között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 2ad9941b-c574-42c3-bd0f-5d6ec68537ef
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: jeedes
ms.openlocfilehash: 5336543e06d60be555722a615568b12048c2aa2f
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="tutorial-azure-active-directory-integration-with-linkedin-elevate"></a><span data-ttu-id="d25b0-103">Oktatóanyag: Azure Active Directoryval integrált LinkedIn jogosultságszint-emelés</span><span class="sxs-lookup"><span data-stu-id="d25b0-103">Tutorial: Azure Active Directory integration with LinkedIn Elevate</span></span>

<span data-ttu-id="d25b0-104">Ebben az oktatóanyagban elsajátíthatja LinkedIn jogosultságszint-emelés integrálása Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="d25b0-104">In this tutorial, you learn how to integrate LinkedIn Elevate with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="d25b0-105">Jogosultságszint-emelés LinkedIn integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="d25b0-105">Integrating LinkedIn Elevate with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="d25b0-106">Szabályozhatja az Azure AD, aki hozzáfér a LinkedIn jogosultságszint-emelés</span><span class="sxs-lookup"><span data-stu-id="d25b0-106">You can control in Azure AD who has access to LinkedIn Elevate</span></span>
- <span data-ttu-id="d25b0-107">Az Azure AD-fiókok a engedélyezheti a felhasználóknak, hogy automatikusan lekérni bejelentkezett való LinkedIn jogosultságszint-emelés (egyszeri bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="d25b0-107">You can enable your users to automatically get signed-on to LinkedIn Elevate (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="d25b0-108">Kezelheti a fiókokat, egy központi helyen – az Azure felügyeleti portálon</span><span class="sxs-lookup"><span data-stu-id="d25b0-108">You can manage your accounts in one central location - the Azure Management portal</span></span>

<span data-ttu-id="d25b0-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="d25b0-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d25b0-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="d25b0-110">Prerequisites</span></span>

<span data-ttu-id="d25b0-111">Konfigurálása az Azure AD-integrációs LinkedIn jogosultságszint-emelés, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="d25b0-111">To configure Azure AD integration with LinkedIn Elevate, you need the following items:</span></span>

- <span data-ttu-id="d25b0-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="d25b0-112">An Azure AD subscription</span></span>
- <span data-ttu-id="d25b0-113">A LinkedIn jogosultságszint-emelés egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="d25b0-113">A LinkedIn Elevate single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="d25b0-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="d25b0-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="d25b0-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="d25b0-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="d25b0-116">Ne használja az éles környezetben, ha ez nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="d25b0-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="d25b0-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d25b0-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="d25b0-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="d25b0-118">Scenario description</span></span>
<span data-ttu-id="d25b0-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="d25b0-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="d25b0-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="d25b0-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="d25b0-121">Jogosultságszint-emelés LinkedIn hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="d25b0-121">Adding LinkedIn Elevate from the gallery</span></span>
2. <span data-ttu-id="d25b0-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="d25b0-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-linkedin-elevate-from-the-gallery"></a><span data-ttu-id="d25b0-123">Jogosultságszint-emelés LinkedIn hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="d25b0-123">Adding LinkedIn Elevate from the gallery</span></span>
<span data-ttu-id="d25b0-124">Az Azure AD integrálása a LinkedIn jogosultságszint-emelés konfigurálásához kell hozzáadnia LinkedIn jogosultságszint-emelés a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="d25b0-124">To configure the integration of LinkedIn Elevate into Azure AD, you need to add LinkedIn Elevate from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="d25b0-125">**Adja hozzá a LinkedIn jogosultságszint-emelés a gyűjteményből, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="d25b0-125">**To add LinkedIn Elevate from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="d25b0-126">Az a  **[Azure felügyeleti portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="d25b0-126">In the **[Azure Management Portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="d25b0-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="d25b0-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="d25b0-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="d25b0-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="d25b0-131">Kattintson a **Hozzáadás** gombra a párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="d25b0-131">Click **Add** button on the top of the dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="d25b0-133">Írja be a keresőmezőbe, **LinkedIn jogosultságszint-emelés**.</span><span class="sxs-lookup"><span data-stu-id="d25b0-133">In the search box, type **LinkedIn Elevate**.</span></span> <span data-ttu-id="d25b0-134">Az eredmények panelen kattintson a **LinkedIn jogosultságszint-emelés** hozzáadása az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="d25b0-134">From results panel, click **LinkedIn Elevate** to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-linkedinElevate-tutorial/tutorial-linkedinElevate_000.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="d25b0-136">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="d25b0-136">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="d25b0-137">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD az egyszeri bejelentkezés LinkedIn jogosultságszint-emelés "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="d25b0-137">In this section, you configure and test Azure AD single sign-on with LinkedIn Elevate based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="d25b0-138">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó LinkedIn jogosultságszint-emelés a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="d25b0-138">For single sign-on to work, Azure AD needs to know what the counterpart user in LinkedIn Elevate is to a user in Azure AD.</span></span> <span data-ttu-id="d25b0-139">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a LinkedIn jogosultságszint-emelés közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="d25b0-139">In other words, a link relationship between an Azure AD user and the related user in LinkedIn Elevate needs to be established.</span></span>

<span data-ttu-id="d25b0-140">Ez a hivatkozás kapcsolat létesíti értéket rendeli az **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a LinkedIn jogosultságszint-emelés.</span><span class="sxs-lookup"><span data-stu-id="d25b0-140">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in LinkedIn Elevate.</span></span>

<span data-ttu-id="d25b0-141">Az Azure AD egyszeri bejelentkezést a LinkedIn jogosultságszint-emelés tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="d25b0-141">To configure and test Azure AD single sign-on with LinkedIn Elevate, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="d25b0-142">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="d25b0-142">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="d25b0-143">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="d25b0-143">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="d25b0-144">**[Jogosultságszint-emelés LinkedIn tesztfelhasználó létrehozása](#creating-a-linkedin-elevate-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="d25b0-144">**[Creating a LinkedIn Elevate test user](#creating-a-linkedin-elevate-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
4. <span data-ttu-id="d25b0-145">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="d25b0-145">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="d25b0-146">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="d25b0-146">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="d25b0-147">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d25b0-147">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="d25b0-148">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure felügyeleti portálon, és konfigurálása egyszeri bejelentkezéshez az LinkedIn jogosultságszint-emelés alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="d25b0-148">In this section, you enable Azure AD single sign-on in the Azure Management portal and configure single sign-on in your LinkedIn Elevate application.</span></span>

<span data-ttu-id="d25b0-149">**Jogosultságszint-emelés LinkedIn konfigurálása az Azure AD egyszeri bejelentkezést, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="d25b0-149">**To configure Azure AD single sign-on with LinkedIn Elevate, perform the following steps:**</span></span>

1. <span data-ttu-id="d25b0-150">Az Azure felügyeleti portálján a a **LinkedIn jogosultságszint-emelés** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="d25b0-150">In the Azure Management portal, on the **LinkedIn Elevate** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="d25b0-152">A a **egyszeri bejelentkezés** párbeszédpanel, mint **mód** kiválasztása **SAML-alapú bejelentkezés** a engedélyezése az egyszeri bejelentkezéshez.</span><span class="sxs-lookup"><span data-stu-id="d25b0-152">On the **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** to enable single sign on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinElevate-tutorial/tutorial-linkedin_01.png)

3. <span data-ttu-id="d25b0-154">Egy másik webes böngészőablakban bejelentkezés a jogosultságszint-emelés LinkedIn-bérlő rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="d25b0-154">In a different web browser window, sign-on to your LinkedIn Elevate tenant as an administrator.</span></span>

4. <span data-ttu-id="d25b0-155">A **Account Center**, kattintson a **globális beállítások** alatt **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="d25b0-155">In **Account Center**, click **Global Settings** under **Settings**.</span></span> <span data-ttu-id="d25b0-156">Jelölje ki, **jogosultságszint-emelés - jogosultságszint-emelés AAD teszt** a legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="d25b0-156">Also, select **Elevate - Elevate AAD Test** from the dropdown list.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinElevate-tutorial/tutorial_linkedin_admin_01.png)

5. <span data-ttu-id="d25b0-158">Kattintson a **, vagy kattintson ide betölteni, és másolja az űrlap egyes mezőket** , és másolja **entitásazonosító** és **helyességi feltétel felhasználói hozzáférést (ACS) URL-címe**</span><span class="sxs-lookup"><span data-stu-id="d25b0-158">Click on **OR Click Here to load and copy individual fields from the form** and copy **Entity Id** and **Assertion Consumer Access (ACS) Url**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinElevate-tutorial/tutorial_linkedin_admin_03.png)

6. <span data-ttu-id="d25b0-160">Azure-portál a **LinkedIn jogosultságszint-emelés tartomány és az URL-címek**, hajtsa végre az alábbi lépéseket, ha azt szeretné, hogy az egyszeri bejelentkezés konfigurálása **IdP kezdeményezett** mód</span><span class="sxs-lookup"><span data-stu-id="d25b0-160">On Azure Portal, under **LinkedIn Elevate Domain and URLs**, perform the following steps if you want to configure SSO in **IdP Initiated** mode</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinElevate-tutorial/tutorial_linkedin_signon_01.png)

    <span data-ttu-id="d25b0-162">a.</span><span class="sxs-lookup"><span data-stu-id="d25b0-162">a.</span></span> <span data-ttu-id="d25b0-163">A a **azonosító** szövegmező, adja meg a **Entitásazonosító** másolt LinkedIn portálról</span><span class="sxs-lookup"><span data-stu-id="d25b0-163">In the **Identifier** textbox, enter the **Entity ID** copied from LinkedIn Portal</span></span> 

    <span data-ttu-id="d25b0-164">b.</span><span class="sxs-lookup"><span data-stu-id="d25b0-164">b.</span></span> <span data-ttu-id="d25b0-165">A a **válasz URL-CÍMEN** szövegmező, adja meg a **helyességi feltétel felhasználói hozzáférést (ACS) URL-cím** másolt LinkedIn portálról</span><span class="sxs-lookup"><span data-stu-id="d25b0-165">In the **Reply URL** textbox, enter the **Assertion Consumer Access (ACS) Url** copied from LinkedIn Portal</span></span>

7. <span data-ttu-id="d25b0-166">Ha azt szeretné, hogy az egyszeri bejelentkezés konfigurálása **Szolgáltató kezdeményezett**, majd kattintson a speciális URL-cím megjelenítése a konfigurációs szakaszban beállítás, és a bejelentkezési URL-cím konfigurálása a következő mintát:</span><span class="sxs-lookup"><span data-stu-id="d25b0-166">If you want to configure SSO in **SP Initiated**, then click Show Advanced URL setting option in the configuration section and configure the sign on URL with the following pattern:</span></span>

    `https://www.linkedin.com/checkpoint/enterprise/login/<AccountId>?application=elevate&applicationInstanceId=<InstanceId>` 
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinElevate-tutorial/tutorial_linkedin_signon_02.png) 
    
8. <span data-ttu-id="d25b0-168">A LinkedIn jogosultságszint-emelés alkalmazás a SAML helyességi feltételek egy meghatározott formátumban, amelyek megkövetelik olyan egyéni attribútum-leképezésekhez hozzáadása a SAML-jogkivonat attribútumok konfigurációs vár.</span><span class="sxs-lookup"><span data-stu-id="d25b0-168">Your LinkedIn Elevate application expects the SAML assertions in a specific format, which requires you to add custom attribute mappings to your SAML token attributes configuration.</span></span> <span data-ttu-id="d25b0-169">Az alábbi képernyőfelvételen látható egy példa a.</span><span class="sxs-lookup"><span data-stu-id="d25b0-169">The following screenshot shows an example for this.</span></span> <span data-ttu-id="d25b0-170">Az alapértelmezett érték **felhasználói azonosító** van **user.userprincipalname** , de ez le kell képezni a felhasználó e-mail címmel rendelkező LinkedIn jogosultságszint-emelés vár.</span><span class="sxs-lookup"><span data-stu-id="d25b0-170">The default value of **User Identifier** is **user.userprincipalname** but LinkedIn Elevate expects this to be mapped with the user's email address.</span></span> <span data-ttu-id="d25b0-171">Az adott használhatja **user.mail** attribútumot a listából, vagy használja a megfelelő attribútum értéket a szervezet konfiguráció alapján.</span><span class="sxs-lookup"><span data-stu-id="d25b0-171">For that you can use **user.mail** attribute from the list or use the appropriate attribute value based on your organization configuration.</span></span> 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinElevate-tutorial/updateusermail.png)

9. <span data-ttu-id="d25b0-173">A **felhasználói attribútumok** kattintson **nézet és egyéb felhasználói attribútumok szerkesztése** és attribútumainak beállítása.</span><span class="sxs-lookup"><span data-stu-id="d25b0-173">In **User Attributes** section, click **View and edit all other user attributes** and set the attributes.</span></span> <span data-ttu-id="d25b0-174">Hozzá kell adnia egy másik jogcím nevű **részleg** és az értéket meg kell feleltetni a **felhasználó.részleg**.</span><span class="sxs-lookup"><span data-stu-id="d25b0-174">You need to add another claim named **department** and the value needs to be mapped to **user.department**.</span></span>

    | <span data-ttu-id="d25b0-175">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="d25b0-175">Attribute Name</span></span> | <span data-ttu-id="d25b0-176">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="d25b0-176">Attribute Value</span></span> |
    | --- | --- |    
    | <span data-ttu-id="d25b0-177">Szervezeti egység</span><span class="sxs-lookup"><span data-stu-id="d25b0-177">department</span></span>| <span data-ttu-id="d25b0-178">felhasználó.részleg</span><span class="sxs-lookup"><span data-stu-id="d25b0-178">user.department</span></span> |

      ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-linkedinElevate-tutorial/userattribute.png)

      <span data-ttu-id="d25b0-180">a.</span><span class="sxs-lookup"><span data-stu-id="d25b0-180">a.</span></span> <span data-ttu-id="d25b0-181">Kattintson a Hozzáadás attribútum attribútum részletek lapjának megnyitásához adja hozzá a részleg attribútumot, ahogy az alábbi-</span><span class="sxs-lookup"><span data-stu-id="d25b0-181">Click on Add attribute to open the attribute details page add the department attribute as shown below-</span></span>

      ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-linkedinElevate-tutorial/adduserattribute.png)

      <span data-ttu-id="d25b0-183">b.</span><span class="sxs-lookup"><span data-stu-id="d25b0-183">b.</span></span> <span data-ttu-id="d25b0-184">Kattintson a **Ok** menteni az attribútum.</span><span class="sxs-lookup"><span data-stu-id="d25b0-184">Click on **Ok** to save the attribute.</span></span>

      <span data-ttu-id="d25b0-185">c.</span><span class="sxs-lookup"><span data-stu-id="d25b0-185">c.</span></span> <span data-ttu-id="d25b0-186">Változtassa meg az attribútum nevét **emailaddress** való **e-mail**.</span><span class="sxs-lookup"><span data-stu-id="d25b0-186">Change the name of the attribute **emailaddress** to **email**.</span></span>


10. <span data-ttu-id="d25b0-187">Az a **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse az XML-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="d25b0-187">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the XML file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinElevate-tutorial/tutorial-linkedinElevate_certificate.png) 

11. <span data-ttu-id="d25b0-189">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="d25b0-189">Click **Save**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_400.png)

12. <span data-ttu-id="d25b0-191">Ugrás a **LinkedIn rendszergazdai beállítások** szakasz.</span><span class="sxs-lookup"><span data-stu-id="d25b0-191">Go to **LinkedIn Admin Settings** section.</span></span> <span data-ttu-id="d25b0-192">Töltse fel az imént letöltött Azure-portálról feltöltése XML beállítást kattintva XML-fájlt.</span><span class="sxs-lookup"><span data-stu-id="d25b0-192">Upload the XML file you just downloaded from the Azure portal by clicking on the Upload XML file option.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinElevate-tutorial/tutorial_linkedin_metadata_03.png)

13. <span data-ttu-id="d25b0-194">Kattintson a **a** SSO engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="d25b0-194">Click **On** to enable SSO.</span></span> <span data-ttu-id="d25b0-195">Egyszeri bejelentkezési állapot állapotúról **nincs csatlakoztatva** való **csatlakoztatva**</span><span class="sxs-lookup"><span data-stu-id="d25b0-195">SSO status will change from **Not Connected** to **Connected**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinElevate-tutorial/tutorial_linkedin_admin_05.png)

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="d25b0-197">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="d25b0-197">Creating an Azure AD test user</span></span>
<span data-ttu-id="d25b0-198">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure felügyeleti portálján Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="d25b0-198">The objective of this section is to create a test user in the Azure Management portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="d25b0-200">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="d25b0-200">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="d25b0-201">Az a **Azure Management portal**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="d25b0-201">In the **Azure Management portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-linkedinElevate-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="d25b0-203">Ugrás a **felhasználók és csoportok** kattintson **minden felhasználó** azon felhasználók listájának megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="d25b0-203">Go to **Users and groups** and click **All users** to display the list of users.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-linkedinElevate-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="d25b0-205">Kattintson a párbeszédpanel tetején **Hozzáadás** megnyitásához a **felhasználói** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d25b0-205">At the top of the dialog click **Add** to open the **User** dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-linkedinElevate-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="d25b0-207">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="d25b0-207">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-linkedinElevate-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="d25b0-209">a.</span><span class="sxs-lookup"><span data-stu-id="d25b0-209">a.</span></span> <span data-ttu-id="d25b0-210">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="d25b0-210">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="d25b0-211">b.</span><span class="sxs-lookup"><span data-stu-id="d25b0-211">b.</span></span> <span data-ttu-id="d25b0-212">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="d25b0-212">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="d25b0-213">c.</span><span class="sxs-lookup"><span data-stu-id="d25b0-213">c.</span></span> <span data-ttu-id="d25b0-214">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="d25b0-214">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="d25b0-215">d.</span><span class="sxs-lookup"><span data-stu-id="d25b0-215">d.</span></span> <span data-ttu-id="d25b0-216">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="d25b0-216">Click **Create**.</span></span> 

### <a name="creating-a-linkedin-elevate-test-user"></a><span data-ttu-id="d25b0-217">Jogosultságszint-emelés LinkedIn tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="d25b0-217">Creating a LinkedIn Elevate test user</span></span>

<span data-ttu-id="d25b0-218">Csatolt jogosultságszint-emelés alkalmazás támogatja a csak az idő a felhasználók átadása, miután a felhasználók hitelesítésére az alkalmazás automatikusan létrejön.</span><span class="sxs-lookup"><span data-stu-id="d25b0-218">Linked Elevate Application supports Just in time user provisioning and after authentication users will be created in the application automatically.</span></span> <span data-ttu-id="d25b0-219">A rendszergazda a beállítások lapon a portál tükrözés LinkedIn jogosultságszint-emelés a kapcsoló **automatikus hozzárendelése licencek** közvetlenül ahhoz, hogy időben aktív kiépítés, és ez lesz is rendel egy licencet a felhasználó.</span><span class="sxs-lookup"><span data-stu-id="d25b0-219">On the admin settings page on the LinkedIn Elevate portal flip the switch **Automatically Assign licenses** to active to enable Just in time provisioning and this will also assign a license to the user.</span></span>

   ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-linkedinElevate-tutorial/LinkedinUserprovswitch.png)

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="d25b0-221">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="d25b0-221">Assigning the Azure AD test user</span></span>

<span data-ttu-id="d25b0-222">Ebben a szakaszban engedélyezze Britta Simon nyújtó rendszerre való jogosultságszint-emelés LinkedIn Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="d25b0-222">In this section, you enable Britta Simon to use Azure single sign-on by granting her access to LinkedIn Elevate.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="d25b0-224">**Britta Simon hozzárendelése LinkedIn jogosultságszint-emelés, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="d25b0-224">**To assign Britta Simon to LinkedIn Elevate, perform the following steps:**</span></span>

1. <span data-ttu-id="d25b0-225">Az Azure felügyeleti portálra, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="d25b0-225">In the Azure Management portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="d25b0-227">Az alkalmazások listában válassza ki a **LinkedIn jogosultságszint-emelés**.</span><span class="sxs-lookup"><span data-stu-id="d25b0-227">In the applications list, select **LinkedIn Elevate**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinElevate-tutorial/tutorial-linkedinElevate_0001.png) 

3. <span data-ttu-id="d25b0-229">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="d25b0-229">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="d25b0-231">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="d25b0-231">Click **Add** button.</span></span> <span data-ttu-id="d25b0-232">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d25b0-232">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="d25b0-234">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="d25b0-234">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="d25b0-235">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d25b0-235">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="d25b0-236">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d25b0-236">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="d25b0-237">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="d25b0-237">Testing single sign-on</span></span>

<span data-ttu-id="d25b0-238">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="d25b0-238">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="d25b0-239">Ha a hozzáférési Panel LinkedIn jogosultságszint-emelés mozaik gombra kattint, szerezheti be az Azure bejelentkezési oldalra, és a sikeres bejelentkezést, miután szerezheti be a jogosultságszint-emelés LinkedIn alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="d25b0-239">When you click the LinkedIn Elevate tile in the Access Panel, you should get the Azure Sign-on page and on after successful sign-on, you should get into your LinkedIn Elevate application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d25b0-240">További források</span><span class="sxs-lookup"><span data-stu-id="d25b0-240">Additional resources</span></span>

* [<span data-ttu-id="d25b0-241">Oktatóanyag: Konfigurálása LinkedIn jogosultságszint-emelés az automatikus felhasználó kiépítése az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="d25b0-241">Tutorial: Configuring LinkedIn Elevate for automatic user provisioning with Azure Active Directory</span></span>](active-directory-saas-linkedinelevate-provisioning-tutorial.md)
* [<span data-ttu-id="d25b0-242">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="d25b0-242">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="d25b0-243">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="d25b0-243">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_203.png
