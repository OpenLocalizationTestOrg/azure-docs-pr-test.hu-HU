---
title: "Oktatóanyag: Azure Active Directory-integráció Kantega egyszeri bejelentkezési modellel a JIRA |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és a JIRA Kantega SSO között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: e2af4891-e3c8-43b3-bdcb-0500c493e9b4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 06a1d301818f025270137f7eaa9f40e5e4503112
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/29/2017
---
# <a name="tutorial-azure-active-directory-integration-with-kantega-sso-for-jira"></a><span data-ttu-id="9275e-103">Oktatóanyag: Azure Active Directory-integráció Kantega egyszeri bejelentkezési modellel a JIRA</span><span class="sxs-lookup"><span data-stu-id="9275e-103">Tutorial: Azure Active Directory integration with Kantega SSO for JIRA</span></span>

<span data-ttu-id="9275e-104">Ebben az oktatóanyagban elsajátíthatja a JIRA Kantega SSO integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="9275e-104">In this tutorial, you learn how to integrate Kantega SSO for JIRA with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="9275e-105">A JIRA Kantega SSO integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="9275e-105">Integrating Kantega SSO for JIRA with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="9275e-106">Szabályozhatja az JIRA Kantega SSO hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="9275e-106">You can control in Azure AD who has access to Kantega SSO for JIRA</span></span>
- <span data-ttu-id="9275e-107">Engedélyezheti a felhasználóknak, hogy automatikusan lekérni aláírt a Kantega SSO JIRA (egyszeri bejelentkezés) az a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="9275e-107">You can enable your users to automatically get signed-on to Kantega SSO for JIRA (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="9275e-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="9275e-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="9275e-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="9275e-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9275e-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="9275e-110">Prerequisites</span></span>

<span data-ttu-id="9275e-111">Az Azure AD-integrációs konfigurálásához Kantega egyszeri bejelentkezési modellel a JIRA, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="9275e-111">To configure Azure AD integration with Kantega SSO for JIRA, you need the following items:</span></span>

- <span data-ttu-id="9275e-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="9275e-112">An Azure AD subscription</span></span>
- <span data-ttu-id="9275e-113">Egy Kantega SSO JIRA egyszeri bejelentkezést az előfizetés engedélyezve</span><span class="sxs-lookup"><span data-stu-id="9275e-113">A Kantega SSO for JIRA single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="9275e-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="9275e-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="9275e-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="9275e-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="9275e-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="9275e-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="9275e-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="9275e-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="9275e-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="9275e-118">Scenario description</span></span>
<span data-ttu-id="9275e-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="9275e-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="9275e-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="9275e-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="9275e-121">A JIRA Kantega SSO hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="9275e-121">Adding Kantega SSO for JIRA from the gallery</span></span>
2. <span data-ttu-id="9275e-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="9275e-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-kantega-sso-for-jira-from-the-gallery"></a><span data-ttu-id="9275e-123">A JIRA Kantega SSO hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="9275e-123">Adding Kantega SSO for JIRA from the gallery</span></span>
<span data-ttu-id="9275e-124">A JIRA Kantega SSO integrálása az Azure AD konfigurálása, kell hozzáadnia a JIRA Kantega SSO a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="9275e-124">To configure the integration of Kantega SSO for JIRA into Azure AD, you need to add Kantega SSO for JIRA from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="9275e-125">**Adja hozzá a JIRA Kantega SSO a gyűjteményből, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="9275e-125">**To add Kantega SSO for JIRA from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="9275e-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="9275e-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="9275e-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="9275e-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="9275e-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="9275e-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="9275e-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="9275e-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="9275e-133">Írja be a keresőmezőbe, **Kantega SSO a JIRA**.</span><span class="sxs-lookup"><span data-stu-id="9275e-133">In the search box, type **Kantega SSO for JIRA**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kantegassoforjira-tutorial/tutorial_kantegassoforjira_search.png)

5. <span data-ttu-id="9275e-135">Az eredmények panelen válassza ki a **Kantega SSO a JIRA**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="9275e-135">In the results panel, select **Kantega SSO for JIRA**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kantegassoforjira-tutorial/tutorial_kantegassoforjira_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="9275e-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="9275e-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="9275e-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD az egyszeri bejelentkezés Kantega egyszeri bejelentkezési modellel, az úgynevezett "Britta Simon" tesztfelhasználó alapján JIRA.</span><span class="sxs-lookup"><span data-stu-id="9275e-138">In this section, you configure and test Azure AD single sign-on with Kantega SSO for JIRA based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="9275e-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Kantega SSO JIRA a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="9275e-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Kantega SSO for JIRA is to a user in Azure AD.</span></span> <span data-ttu-id="9275e-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó a JIRA Kantega SSO közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="9275e-140">In other words, a link relationship between an Azure AD user and the related user in Kantega SSO for JIRA needs to be established.</span></span>

<span data-ttu-id="9275e-141">JIRA Kantega egyszeri Bejelentkezést, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="9275e-141">In Kantega SSO for JIRA, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="9275e-142">Az Azure AD az egyszeri bejelentkezés Kantega egyszeri bejelentkezési modellel a JIRA tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="9275e-142">To configure and test Azure AD single sign-on with Kantega SSO for JIRA, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="9275e-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="9275e-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="9275e-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="9275e-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="9275e-145">**[Egy Kantega SSO JIRA teszt felhasználó létrehozása](#creating-a-kantega-sso-for-jira-test-user)**  - való egy megfelelője a Britta Simon Kantega SSO a JIRA, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="9275e-145">**[Creating a Kantega SSO for JIRA test user](#creating-a-kantega-sso-for-jira-test-user)** - to have a counterpart of Britta Simon in Kantega SSO for JIRA that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="9275e-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="9275e-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="9275e-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="9275e-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="9275e-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="9275e-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="9275e-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és a Kantega SSO JIRA alkalmazáshoz az egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="9275e-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Kantega SSO for JIRA application.</span></span>

<span data-ttu-id="9275e-150">**Az Azure AD az egyszeri bejelentkezés Kantega egyszeri bejelentkezési modellel a JIRA megadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="9275e-150">**To configure Azure AD single sign-on with Kantega SSO for JIRA, perform the following steps:**</span></span>

1. <span data-ttu-id="9275e-151">Az Azure portálon a a **Kantega SSO a JIRA** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="9275e-151">In the Azure portal, on the **Kantega SSO for JIRA** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="9275e-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="9275e-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforjira-tutorial/tutorial_kantegassoforjira_samlbase.png)

3. <span data-ttu-id="9275e-155">A **IDP** indítható módban, a **Kantega SSO JIRA tartomány és az URL-címek** szakasz hajtsa végre a következő lépést:</span><span class="sxs-lookup"><span data-stu-id="9275e-155">In **IDP** initiated mode, on the **Kantega SSO for JIRA Domain and URLs** section perform the following step:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforjira-tutorial/tutorial_kantegassoforjira_url1.png)

    <span data-ttu-id="9275e-157">a.</span><span class="sxs-lookup"><span data-stu-id="9275e-157">a.</span></span> <span data-ttu-id="9275e-158">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`</span><span class="sxs-lookup"><span data-stu-id="9275e-158">In the **Identifier** textbox, type a URL using the following pattern: `https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`</span></span>

    <span data-ttu-id="9275e-159">b.</span><span class="sxs-lookup"><span data-stu-id="9275e-159">b.</span></span> <span data-ttu-id="9275e-160">Az a **válasz URL-CÍMEN** szövegmező, adja meg a következő minta használatával URL-címe:`https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`</span><span class="sxs-lookup"><span data-stu-id="9275e-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`</span></span>

4. <span data-ttu-id="9275e-161">A **SP** kezdeményezett módban, a jelölőnégyzet **megjelenítése speciális URL-beállításainak** és hajtsa végre a következő lépést:</span><span class="sxs-lookup"><span data-stu-id="9275e-161">In **SP** initiated mode, check **Show advanced URL settings** and  perform the following step:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforjira-tutorial/tutorial_kantegassoforjira_url2.png)

    <span data-ttu-id="9275e-163">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`</span><span class="sxs-lookup"><span data-stu-id="9275e-163">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="9275e-164">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="9275e-164">These values are not real.</span></span> <span data-ttu-id="9275e-165">Frissítheti ezeket az értékeket a tényleges azonosítója, válasz URL-CÍMEN és bejelentkezési URL-cím.</span><span class="sxs-lookup"><span data-stu-id="9275e-165">Update these values with the actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="9275e-166">Ezek az értékek fogadásának Jira beépülő modul, az oktatóanyag későbbi részében ismertetett konfigurálása során.</span><span class="sxs-lookup"><span data-stu-id="9275e-166">These values are received during the configuration of Jira plugin, which is explained later in the tutorial.</span></span>

5. <span data-ttu-id="9275e-167">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="9275e-167">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforjira-tutorial/tutorial_kantegassoforjira_certificate.png) 

6. <span data-ttu-id="9275e-169">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="9275e-169">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforjira-tutorial/tutorial_general_400.png)
    
7. <span data-ttu-id="9275e-171">Egy másik webes böngészőablakban jelentkezzen be a helyi kiszolgálón JIRA rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="9275e-171">In a different web browser window, log in to your JIRA on premise server as an administrator.</span></span>

8. <span data-ttu-id="9275e-172">Vigye a mutatót a ikonjára, majd kattintson a **bővítmények**.</span><span class="sxs-lookup"><span data-stu-id="9275e-172">Hover on cog and click the **Add-ons**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforjira-tutorial/addon1.png)

9. <span data-ttu-id="9275e-174">A bővítmények lap szakaszban kattintson **található új bővítmények**.</span><span class="sxs-lookup"><span data-stu-id="9275e-174">Under Add-ons tab section, click **Find new add-ons**.</span></span> <span data-ttu-id="9275e-175">Keresési **Kantega SSO JIRA (SAML & Kerberos) a** kattintson **telepítése** az új SAML-alapú beépülő modul telepítése gombra.</span><span class="sxs-lookup"><span data-stu-id="9275e-175">Search **Kantega SSO for JIRA (SAML & Kerberos)** and click **Install** button to install the new SAML plugin.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforjira-tutorial/addon2.png)

10. <span data-ttu-id="9275e-177">A beépülő modul telepítésének megkezdése.</span><span class="sxs-lookup"><span data-stu-id="9275e-177">The plugin installation starts.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforjira-tutorial/addon3.png)

11. <span data-ttu-id="9275e-179">A telepítés befejeződése után.</span><span class="sxs-lookup"><span data-stu-id="9275e-179">Once the installation is complete.</span></span> <span data-ttu-id="9275e-180">Kattintson a **Bezárás** gombra.</span><span class="sxs-lookup"><span data-stu-id="9275e-180">Click **Close**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforjira-tutorial/addon33.png)

12. <span data-ttu-id="9275e-182">Kattintson a **Kezelés** gombra.</span><span class="sxs-lookup"><span data-stu-id="9275e-182">Click **Manage**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforjira-tutorial/addon34.png)
    
13. <span data-ttu-id="9275e-184">Új beépülő modul megtalálható-e **integrációja**.</span><span class="sxs-lookup"><span data-stu-id="9275e-184">New plugin is listed under **INTEGRATIONS**.</span></span> <span data-ttu-id="9275e-185">Kattintson a **konfigurálása** konfigurálása az új beépülő modul.</span><span class="sxs-lookup"><span data-stu-id="9275e-185">Click **Configure** to configure the new plugin.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforjira-tutorial/addon35.png)

14. <span data-ttu-id="9275e-187">Az a **SAML** szakasz.</span><span class="sxs-lookup"><span data-stu-id="9275e-187">In the **SAML** section.</span></span> <span data-ttu-id="9275e-188">Válassza ki **Azure Active Directory (Azure AD)** a a **Hozzáadás identitásszolgáltató** legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="9275e-188">Select **Azure Active Directory (Azure AD)** from the **Add identity provider** dropdown.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforjira-tutorial/addon4.png)

15. <span data-ttu-id="9275e-190">Válassza ki az előfizetési szinten **alapvető**.</span><span class="sxs-lookup"><span data-stu-id="9275e-190">Select subscription level as **Basic**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforjira-tutorial/addon5.png)     

16. <span data-ttu-id="9275e-192">Az a **alkalmazás tulajdonságainak** csoportjában hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="9275e-192">On the **App properties** section, perform following steps:</span></span> 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforjira-tutorial/addon6.png)

    <span data-ttu-id="9275e-194">a.</span><span class="sxs-lookup"><span data-stu-id="9275e-194">a.</span></span> <span data-ttu-id="9275e-195">Másolás a **App ID URI** értékét, és használja azt **azonosító, a válasz URL-CÍMEN és a bejelentkezési URL-cím** a a **Kantega SSO JIRA tartomány és az URL-címek** szakaszban az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="9275e-195">Copy the **App ID URI** value and use it as **Identifier, Reply URL, and Sign-On URL** on the **Kantega SSO for JIRA Domain and URLs** section in Azure portal.</span></span>

    <span data-ttu-id="9275e-196">b.</span><span class="sxs-lookup"><span data-stu-id="9275e-196">b.</span></span> <span data-ttu-id="9275e-197">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="9275e-197">Click **Next**.</span></span>

17. <span data-ttu-id="9275e-198">Az a **metaadatok importálása** csoportjában hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="9275e-198">On the **Metadata import** section, perform following steps:</span></span> 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforjira-tutorial/addon7.png)

    <span data-ttu-id="9275e-200">a.</span><span class="sxs-lookup"><span data-stu-id="9275e-200">a.</span></span> <span data-ttu-id="9275e-201">Válassza ki **metaadat-fájlt a számítógépen lévő**, és az Azure-portálról letöltött feltöltés metaadatait tartalmazó fájl.</span><span class="sxs-lookup"><span data-stu-id="9275e-201">Select **Metadata file on my computer**, and upload metadata file, which you have downloaded from Azure portal.</span></span>

    <span data-ttu-id="9275e-202">b.</span><span class="sxs-lookup"><span data-stu-id="9275e-202">b.</span></span> <span data-ttu-id="9275e-203">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="9275e-203">Click **Next**.</span></span>

18. <span data-ttu-id="9275e-204">Az a **nevét és az egyszeri bejelentkezés hely** csoportjában hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="9275e-204">On the **Name and SSO location** section, perform following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforjira-tutorial/addon8.png)
    
    <span data-ttu-id="9275e-206">a.</span><span class="sxs-lookup"><span data-stu-id="9275e-206">a.</span></span> <span data-ttu-id="9275e-207">Adja hozzá az identitásszolgáltató neve **identitás szolgáltatónevet** szövegmező (például az Azure AD).</span><span class="sxs-lookup"><span data-stu-id="9275e-207">Add Name of the Identity Provider in **Identity provider name** textbox (e.g Azure AD).</span></span>

    <span data-ttu-id="9275e-208">b.</span><span class="sxs-lookup"><span data-stu-id="9275e-208">b.</span></span> <span data-ttu-id="9275e-209">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="9275e-209">Click **Next**.</span></span>

19. <span data-ttu-id="9275e-210">Ellenőrizze a tanúsítvány aláírása, és kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="9275e-210">Verify the Signing certificate and click **Next**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforjira-tutorial/addon9.png)

20. <span data-ttu-id="9275e-212">Az a **JIRA felhasználói fiókok** csoportjában hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="9275e-212">On the **JIRA user accounts** section, perform following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforjira-tutorial/addon10.png)

    <span data-ttu-id="9275e-214">a.</span><span class="sxs-lookup"><span data-stu-id="9275e-214">a.</span></span> <span data-ttu-id="9275e-215">Válassza ki **felhasználók JIRA tartozó belső könyvtárban szükség esetén hozzon létre** , és írja be a felhasználói csoport megfelelő neve (lehet több nem.</span><span class="sxs-lookup"><span data-stu-id="9275e-215">Select **Create users in JIRA's internal Directory if needed** and enter the appropriate name of the group for users (can be multiple no.</span></span> <span data-ttu-id="9275e-216">vesszővel elválasztott csoportok).</span><span class="sxs-lookup"><span data-stu-id="9275e-216">of groups separated by comma).</span></span>

    <span data-ttu-id="9275e-217">b.</span><span class="sxs-lookup"><span data-stu-id="9275e-217">b.</span></span> <span data-ttu-id="9275e-218">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="9275e-218">Click **Next**.</span></span>

21. <span data-ttu-id="9275e-219">Kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="9275e-219">Click **Finish**.</span></span>   

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforjira-tutorial/addon11.png)

22. <span data-ttu-id="9275e-221">Az a **tartományok ismert az Azure AD** csoportjában hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="9275e-221">On the **Known domains for Azure AD** section, perform following steps:</span></span> 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforjira-tutorial/addon12.png)

    <span data-ttu-id="9275e-223">a.</span><span class="sxs-lookup"><span data-stu-id="9275e-223">a.</span></span> <span data-ttu-id="9275e-224">Válassza ki **tartományok ismert** a lap bal oldali panelen.</span><span class="sxs-lookup"><span data-stu-id="9275e-224">Select **Known domains** from the left panel of the page.</span></span>

    <span data-ttu-id="9275e-225">b.</span><span class="sxs-lookup"><span data-stu-id="9275e-225">b.</span></span> <span data-ttu-id="9275e-226">Adja meg a tartomány nevére a **tartományok ismert** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="9275e-226">Enter domain name in the **Known domains** textbox.</span></span>

    <span data-ttu-id="9275e-227">c.</span><span class="sxs-lookup"><span data-stu-id="9275e-227">c.</span></span> <span data-ttu-id="9275e-228">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="9275e-228">Click **Save**.</span></span> 

> [!TIP]
> <span data-ttu-id="9275e-229">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="9275e-229">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="9275e-230">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="9275e-230">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="9275e-231">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="9275e-231">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="9275e-232">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="9275e-232">Creating an Azure AD test user</span></span>
<span data-ttu-id="9275e-233">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="9275e-233">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="9275e-235">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="9275e-235">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="9275e-236">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="9275e-236">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kantegassoforjira-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="9275e-238">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="9275e-238">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kantegassoforjira-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="9275e-240">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="9275e-240">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kantegassoforjira-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="9275e-242">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="9275e-242">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kantegassoforjira-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="9275e-244">a.</span><span class="sxs-lookup"><span data-stu-id="9275e-244">a.</span></span> <span data-ttu-id="9275e-245">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="9275e-245">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="9275e-246">b.</span><span class="sxs-lookup"><span data-stu-id="9275e-246">b.</span></span> <span data-ttu-id="9275e-247">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="9275e-247">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="9275e-248">c.</span><span class="sxs-lookup"><span data-stu-id="9275e-248">c.</span></span> <span data-ttu-id="9275e-249">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="9275e-249">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="9275e-250">d.</span><span class="sxs-lookup"><span data-stu-id="9275e-250">d.</span></span> <span data-ttu-id="9275e-251">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="9275e-251">Click **Create**.</span></span>
 
### <a name="creating-a-kantega-sso-for-jira-test-user"></a><span data-ttu-id="9275e-252">Egy Kantega SSO a JIRA tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="9275e-252">Creating a Kantega SSO for JIRA test user</span></span>

<span data-ttu-id="9275e-253">Ahhoz, hogy az Azure AD-felhasználók JIRA bejelentkezni, akkor ki kell építenie a JIRA.</span><span class="sxs-lookup"><span data-stu-id="9275e-253">To enable Azure AD users to log in to JIRA, they must be provisioned into JIRA.</span></span> <span data-ttu-id="9275e-254">A JIRA Kantega egyszeri Bejelentkezést egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="9275e-254">In Kantega SSO for JIRA, provisioning is a manual task.</span></span>

<span data-ttu-id="9275e-255">**Felhasználói fiók létrehozásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="9275e-255">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="9275e-256">Jelentkezzen be rendszergazdaként a JIRA a helyi kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="9275e-256">Log in to your JIRA on premise server as an administrator.</span></span>

2. <span data-ttu-id="9275e-257">Vigye a mutatót a ikonjára, majd kattintson a **felhasználókezelés**.</span><span class="sxs-lookup"><span data-stu-id="9275e-257">Hover on cog and click the **User management**.</span></span>

    ![Alkalmazott hozzáadása](./media/active-directory-saas-kantegassoforjira-tutorial/user1.png) 

3. <span data-ttu-id="9275e-259">A **felhasználókezelés** szakasz lapra, majd **a felhasználó létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="9275e-259">Under **User management** tab section, click **Create user**.</span></span>

    ![Alkalmazott hozzáadása](./media/active-directory-saas-kantegassoforjira-tutorial/user2.png) 

4. <span data-ttu-id="9275e-261">Az a **"Új felhasználó létrehozása"** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="9275e-261">On the **“Create new user”** dialog page, perform the following steps:</span></span>

    ![Alkalmazott hozzáadása](./media/active-directory-saas-kantegassoforjira-tutorial/user3.png) 

    <span data-ttu-id="9275e-263">a.</span><span class="sxs-lookup"><span data-stu-id="9275e-263">a.</span></span> <span data-ttu-id="9275e-264">Az a **E-mail cím** szövegmező, a felhasználó e-mail címe típusát, például Brittasimon@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="9275e-264">In the **Email address** textbox, type the email address of user like Brittasimon@contoso.com.</span></span>

    <span data-ttu-id="9275e-265">b.</span><span class="sxs-lookup"><span data-stu-id="9275e-265">b.</span></span> <span data-ttu-id="9275e-266">Az a **teljes nevét** szövegmező, a felhasználó például Britta Simon típus teljes nevét.</span><span class="sxs-lookup"><span data-stu-id="9275e-266">In the **Full Name** textbox, type full name of the user like Britta Simon.</span></span>

    <span data-ttu-id="9275e-267">c.</span><span class="sxs-lookup"><span data-stu-id="9275e-267">c.</span></span> <span data-ttu-id="9275e-268">Az a **felhasználónév** szövegmezőben, az e-mailt a felhasználó típusát, például Brittasimon@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="9275e-268">In the **Username** textbox, type the email of user like Brittasimon@contoso.com.</span></span>

    <span data-ttu-id="9275e-269">d.</span><span class="sxs-lookup"><span data-stu-id="9275e-269">d.</span></span> <span data-ttu-id="9275e-270">Az a **jelszó** szövegmező, írja be a felhasználó jelszavát.</span><span class="sxs-lookup"><span data-stu-id="9275e-270">In the **Password** textbox, type the password of user.</span></span>

    <span data-ttu-id="9275e-271">e.</span><span class="sxs-lookup"><span data-stu-id="9275e-271">e.</span></span> <span data-ttu-id="9275e-272">Kattintson a **a felhasználó létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="9275e-272">Click **Create user**.</span></span>   

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="9275e-273">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="9275e-273">Assigning the Azure AD test user</span></span>

<span data-ttu-id="9275e-274">Ebben a szakaszban Azure egyszeri bejelentkezéshez használandó hozzáférést biztosít a JIRA Kantega SSO Britta Simon engedélyezi.</span><span class="sxs-lookup"><span data-stu-id="9275e-274">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Kantega SSO for JIRA.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="9275e-276">**Britta Simon hozzárendelése a JIRA Kantega SSO, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="9275e-276">**To assign Britta Simon to Kantega SSO for JIRA, perform the following steps:**</span></span>

1. <span data-ttu-id="9275e-277">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="9275e-277">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="9275e-279">Az alkalmazások listában válassza ki a **Kantega SSO a JIRA**.</span><span class="sxs-lookup"><span data-stu-id="9275e-279">In the applications list, select **Kantega SSO for JIRA**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforjira-tutorial/tutorial_kantegassoforjira_app.png) 

3. <span data-ttu-id="9275e-281">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="9275e-281">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="9275e-283">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="9275e-283">Click **Add** button.</span></span> <span data-ttu-id="9275e-284">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="9275e-284">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="9275e-286">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="9275e-286">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="9275e-287">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="9275e-287">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="9275e-288">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="9275e-288">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="9275e-289">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="9275e-289">Testing single sign-on</span></span>

<span data-ttu-id="9275e-290">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="9275e-290">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="9275e-291">Kattintva a Kantega SSO JIRA csempe a hozzáférési panelen, meg kell beolvasni automatikusan aláírt a a Kantega SSO JIRA alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="9275e-291">When you click the Kantega SSO for JIRA tile in the Access Panel, you should get automatically signed-on to your Kantega SSO for JIRA application.</span></span>
<span data-ttu-id="9275e-292">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="9275e-292">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="9275e-293">További források</span><span class="sxs-lookup"><span data-stu-id="9275e-293">Additional resources</span></span>

* [<span data-ttu-id="9275e-294">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="9275e-294">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="9275e-295">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="9275e-295">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-kantegassoforjira-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-kantegassoforjira-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-kantegassoforjira-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-kantegassoforjira-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-kantegassoforjira-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-kantegassoforjira-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-kantegassoforjira-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-kantegassoforjira-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-kantegassoforjira-tutorial/tutorial_general_203.png

