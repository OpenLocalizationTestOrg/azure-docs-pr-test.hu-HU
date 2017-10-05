---
title: "Oktatóanyag: Azure Active Directory-integráció Kantega egyszeri bejelentkezési modellel a Bitbucketből |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és a Bitbucketből Kantega SSO között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c41cdaaf-0441-493c-94c7-569615b7b1ab
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 6656c9abf8483ee98c0cb1a16c06d078e32240f2
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/29/2017
---
# <a name="tutorial-azure-active-directory-integration-with-kantega-sso-for-bitbucket"></a><span data-ttu-id="614a4-103">Oktatóanyag: Azure Active Directory-integráció Kantega egyszeri bejelentkezési modellel a Bitbucketből</span><span class="sxs-lookup"><span data-stu-id="614a4-103">Tutorial: Azure Active Directory integration with Kantega SSO for Bitbucket</span></span>

<span data-ttu-id="614a4-104">Ebben az oktatóanyagban elsajátíthatja a Bitbucketből Kantega SSO integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="614a4-104">In this tutorial, you learn how to integrate Kantega SSO for Bitbucket with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="614a4-105">A Bitbucketből Kantega SSO integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="614a4-105">Integrating Kantega SSO for Bitbucket with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="614a4-106">Szabályozhatja a Bitbucketből Kantega SSO hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="614a4-106">You can control in Azure AD who has access to Kantega SSO for Bitbucket</span></span>
- <span data-ttu-id="614a4-107">Az Azure AD-fiókok a engedélyezheti a felhasználóknak, hogy automatikusan lekérni aláírt a Kantega SSO a Bitbucketből (egyszeri bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="614a4-107">You can enable your users to automatically get signed-on to Kantega SSO for Bitbucket (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="614a4-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="614a4-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="614a4-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="614a4-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="614a4-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="614a4-110">Prerequisites</span></span>

<span data-ttu-id="614a4-111">Az Azure AD-integrációs konfigurálásához Kantega egyszeri bejelentkezési modellel a Bitbucketből, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="614a4-111">To configure Azure AD integration with Kantega SSO for Bitbucket, you need the following items:</span></span>

- <span data-ttu-id="614a4-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="614a4-112">An Azure AD subscription</span></span>
- <span data-ttu-id="614a4-113">Egy Kantega SSO Bitbucket egyszeri bejelentkezést az előfizetés engedélyezve</span><span class="sxs-lookup"><span data-stu-id="614a4-113">A Kantega SSO for Bitbucket single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="614a4-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="614a4-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="614a4-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="614a4-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="614a4-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="614a4-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="614a4-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="614a4-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="614a4-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="614a4-118">Scenario description</span></span>
<span data-ttu-id="614a4-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="614a4-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="614a4-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="614a4-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="614a4-121">A Bitbucketből Kantega SSO hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="614a4-121">Adding Kantega SSO for Bitbucket from the gallery</span></span>
2. <span data-ttu-id="614a4-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="614a4-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-kantega-sso-for-bitbucket-from-the-gallery"></a><span data-ttu-id="614a4-123">A Bitbucketből Kantega SSO hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="614a4-123">Adding Kantega SSO for Bitbucket from the gallery</span></span>
<span data-ttu-id="614a4-124">A Bitbucketből Kantega SSO integrálása az Azure AD konfigurálása, kell hozzáadnia a Bitbucketből Kantega SSO a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="614a4-124">To configure the integration of Kantega SSO for Bitbucket into Azure AD, you need to add Kantega SSO for Bitbucket from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="614a4-125">**Adja hozzá a Bitbucketből Kantega SSO a gyűjteményből, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="614a4-125">**To add Kantega SSO for Bitbucket from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="614a4-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="614a4-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="614a4-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="614a4-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="614a4-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="614a4-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="614a4-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="614a4-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="614a4-133">Írja be a keresőmezőbe, **Kantega SSO a Bitbucketből**.</span><span class="sxs-lookup"><span data-stu-id="614a4-133">In the search box, type **Kantega SSO for Bitbucket**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kantegassoforbitbucket-tutorial/tutorial_kantegassoforbitbucket_search.png)

5. <span data-ttu-id="614a4-135">Az eredmények panelen válassza ki a **Kantega SSO a Bitbucketből**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="614a4-135">In the results panel, select **Kantega SSO for Bitbucket**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kantegassoforbitbucket-tutorial/tutorial_kantegassoforbitbucket_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="614a4-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="614a4-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="614a4-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD az egyszeri bejelentkezés Kantega egyszeri bejelentkezési modellel a Bitbucketből "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="614a4-138">In this section, you configure and test Azure AD single sign-on with Kantega SSO for Bitbucket based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="614a4-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Kantega SSO Bitbucket az a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="614a4-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Kantega SSO for Bitbucket is to a user in Azure AD.</span></span> <span data-ttu-id="614a4-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Bitbucketből Kantega SSO közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="614a4-140">In other words, a link relationship between an Azure AD user and the related user in Kantega SSO for Bitbucket needs to be established.</span></span>

<span data-ttu-id="614a4-141">Bitbucket Kantega egyszeri Bejelentkezést, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="614a4-141">In Kantega SSO for Bitbucket, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="614a4-142">Az Azure AD az egyszeri bejelentkezés Kantega egyszeri bejelentkezési modellel a Bitbucketből tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="614a4-142">To configure and test Azure AD single sign-on with Kantega SSO for Bitbucket, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="614a4-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="614a4-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="614a4-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="614a4-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="614a4-145">**[Egy Kantega SSO Bitbucket teszt felhasználó létrehozása](#creating-a-kantega-sso-for-bitbucket-test-user)**  - való egy megfelelője a Britta Simon Kantega SSO a Bitbucketből, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="614a4-145">**[Creating a Kantega SSO for Bitbucket test user](#creating-a-kantega-sso-for-bitbucket-test-user)** - to have a counterpart of Britta Simon in Kantega SSO for Bitbucket that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="614a4-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="614a4-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="614a4-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="614a4-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="614a4-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="614a4-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="614a4-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és a Kantega SSO Bitbucket alkalmazás egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="614a4-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Kantega SSO for Bitbucket application.</span></span>

<span data-ttu-id="614a4-150">**Konfigurálja az Azure AD az egyszeri bejelentkezés Kantega egyszeri bejelentkezési modellel a Bitbucketből, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="614a4-150">**To configure Azure AD single sign-on with Kantega SSO for Bitbucket, perform the following steps:**</span></span>

1. <span data-ttu-id="614a4-151">Az Azure portálon a a **Kantega SSO a Bitbucketből** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="614a4-151">In the Azure portal, on the **Kantega SSO for Bitbucket** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="614a4-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="614a4-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforbitbucket-tutorial/tutorial_kantegassoforbitbucket_samlbase.png)

3. <span data-ttu-id="614a4-155">A **IDP** indítható módban, a **Kantega SSO Bitbucket tartomány és az URL-címek** szakasz hajtsa végre a következő lépést:</span><span class="sxs-lookup"><span data-stu-id="614a4-155">In **IDP** initiated mode, on the **Kantega SSO for Bitbucket Domain and URLs** section perform the following step:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforbitbucket-tutorial/tutorial_kantegassoforbitbucket_url1.png)

    <span data-ttu-id="614a4-157">a.</span><span class="sxs-lookup"><span data-stu-id="614a4-157">a.</span></span> <span data-ttu-id="614a4-158">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`</span><span class="sxs-lookup"><span data-stu-id="614a4-158">In the **Identifier** textbox, type a URL using the following pattern: `https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`</span></span>

    <span data-ttu-id="614a4-159">b.</span><span class="sxs-lookup"><span data-stu-id="614a4-159">b.</span></span> <span data-ttu-id="614a4-160">Az a **válasz URL-CÍMEN** szövegmező, adja meg a következő minta használatával URL-címe:`https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`</span><span class="sxs-lookup"><span data-stu-id="614a4-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`</span></span>

4. <span data-ttu-id="614a4-161">A **SP** kezdeményezett módban, a jelölőnégyzet **megjelenítése speciális URL-beállításainak** és hajtsa végre a következő lépést:</span><span class="sxs-lookup"><span data-stu-id="614a4-161">In **SP** initiated mode, check **Show advanced URL settings** and  perform the following step:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforbitbucket-tutorial/tutorial_kantegassoforbitbucket_url2.png)
    
    <span data-ttu-id="614a4-163">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`</span><span class="sxs-lookup"><span data-stu-id="614a4-163">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="614a4-164">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="614a4-164">These values are not real.</span></span> <span data-ttu-id="614a4-165">Frissítheti ezeket az értékeket a tényleges azonosítója, válasz URL-CÍMEN és bejelentkezési URL-cím.</span><span class="sxs-lookup"><span data-stu-id="614a4-165">Update these values with the actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="614a4-166">Ezeket az értékeket az oktatóanyag későbbi részében ismertetett Bitbucket beépülő modul konfigurálása során érkezik.</span><span class="sxs-lookup"><span data-stu-id="614a4-166">These values are recieved during the configuration of Bitbucket plugin which is explained later in the tutorial.</span></span>

5. <span data-ttu-id="614a4-167">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="614a4-167">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforbitbucket-tutorial/tutorial_kantegassoforbitbucket_certificate.png) 

6. <span data-ttu-id="614a4-169">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="614a4-169">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforbitbucket-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="614a4-171">Egy másik webes böngészőablakban jelentkezzen be a Bitbucketből felügyeleti portálon rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="614a4-171">In a different web browser window, log in to your Bitbucket admin portal as an administrator.</span></span>

8. <span data-ttu-id="614a4-172">Kattintson a ikonjára, majd kattintson a **található új bővítmények**.</span><span class="sxs-lookup"><span data-stu-id="614a4-172">Click cog and click the **Find new add-ons**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforbitbucket-tutorial/addon1.png)

9. <span data-ttu-id="614a4-174">Keresési **Kantega SSO Bitbucket SAML & Kerberos** kattintson **telepítése** az új SAML-alapú beépülő modul telepítése gombra.</span><span class="sxs-lookup"><span data-stu-id="614a4-174">Search **Kantega SSO for Bitbucket SAML & Kerberos** and click **Install** button to install the new SAML plugin.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforbitbucket-tutorial/addon2.png)

10. <span data-ttu-id="614a4-176">A beépülő modul telepítésének megkezdése.</span><span class="sxs-lookup"><span data-stu-id="614a4-176">The plugin installation starts.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforbitbucket-tutorial/addon31.png)

11. <span data-ttu-id="614a4-178">A telepítés befejeződése után.</span><span class="sxs-lookup"><span data-stu-id="614a4-178">Once the installation is complete.</span></span> <span data-ttu-id="614a4-179">Kattintson a **Bezárás** gombra.</span><span class="sxs-lookup"><span data-stu-id="614a4-179">Click **Close**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforbitbucket-tutorial/addon33.png)

12. <span data-ttu-id="614a4-181">Kattintson a **Kezelés** gombra.</span><span class="sxs-lookup"><span data-stu-id="614a4-181">Click **Manage**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforbitbucket-tutorial/addon34.png)
    
13. <span data-ttu-id="614a4-183">Kattintson a **konfigurálása** konfigurálása az új beépülő modul.</span><span class="sxs-lookup"><span data-stu-id="614a4-183">Click **Configure** to configure the new plugin.</span></span>    

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforbitbucket-tutorial/addon35.png)

14. <span data-ttu-id="614a4-185">Az a **SAML** szakasz.</span><span class="sxs-lookup"><span data-stu-id="614a4-185">In the **SAML** section.</span></span> <span data-ttu-id="614a4-186">Válassza ki **Azure Active Directory (Azure AD)** a a **Hozzáadás identitásszolgáltató** legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="614a4-186">Select **Azure Active Directory (Azure AD)** from the **Add identity provider** dropdown.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforbitbucket-tutorial/addon4.png)

15. <span data-ttu-id="614a4-188">Válassza ki az előfizetési szinten **alapvető**.</span><span class="sxs-lookup"><span data-stu-id="614a4-188">Select subscription level as **Basic**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforbitbucket-tutorial/addon5.png)

16. <span data-ttu-id="614a4-190">Az a **alkalmazás tulajdonságainak** csoportjában hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="614a4-190">On the **App properties** section, perform following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforbitbucket-tutorial/addon6.png)

    <span data-ttu-id="614a4-192">a.</span><span class="sxs-lookup"><span data-stu-id="614a4-192">a.</span></span> <span data-ttu-id="614a4-193">Másolás a **App ID URI** értékét, és használja azt **azonosító, a válasz URL-CÍMEN és a bejelentkezési URL-cím** a a **Kantega SSO Bitbucket tartomány és az URL-címek** szakaszban az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="614a4-193">Copy the **App ID URI** value and use it as **Identifier, Reply URL, and Sign-On URL** on the **Kantega SSO for Bitbucket Domain and URLs** section in Azure portal.</span></span>

    <span data-ttu-id="614a4-194">b.</span><span class="sxs-lookup"><span data-stu-id="614a4-194">b.</span></span> <span data-ttu-id="614a4-195">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="614a4-195">Click **Next**.</span></span>

17. <span data-ttu-id="614a4-196">Az a **metaadatok importálása** csoportjában hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="614a4-196">On the **Metadata import** section, perform following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforbitbucket-tutorial/addon7.png)

    <span data-ttu-id="614a4-198">a.</span><span class="sxs-lookup"><span data-stu-id="614a4-198">a.</span></span> <span data-ttu-id="614a4-199">Válassza ki **metaadat-fájlt a számítógépen lévő**, és az Azure-portálról letöltött feltöltés metaadatait tartalmazó fájl.</span><span class="sxs-lookup"><span data-stu-id="614a4-199">Select **Metadata file on my computer**, and upload metadata file, which you have downloaded from Azure portal.</span></span>

    <span data-ttu-id="614a4-200">b.</span><span class="sxs-lookup"><span data-stu-id="614a4-200">b.</span></span> <span data-ttu-id="614a4-201">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="614a4-201">Click **Next**.</span></span>

18. <span data-ttu-id="614a4-202">Az a **nevét és az egyszeri bejelentkezés hely** csoportjában hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="614a4-202">On the **Name and SSO location** section, perform following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforbitbucket-tutorial/addon8.png)

    <span data-ttu-id="614a4-204">a.</span><span class="sxs-lookup"><span data-stu-id="614a4-204">a.</span></span> <span data-ttu-id="614a4-205">Adja hozzá az identitásszolgáltató neve **identitás szolgáltatónevet** szövegmező (például az Azure AD).</span><span class="sxs-lookup"><span data-stu-id="614a4-205">Add Name of the Identity Provider in **Identity provider name** textbox (e.g Azure AD).</span></span>

    <span data-ttu-id="614a4-206">b.</span><span class="sxs-lookup"><span data-stu-id="614a4-206">b.</span></span> <span data-ttu-id="614a4-207">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="614a4-207">Click **Next**.</span></span>

19. <span data-ttu-id="614a4-208">Ellenőrizze a tanúsítvány aláírása, és kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="614a4-208">Verify the Signing certificate and click **Next**.</span></span>  

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforbitbucket-tutorial/addon9.png)

20. <span data-ttu-id="614a4-210">Az a **Bitbucket felhasználói fiókok** csoportjában hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="614a4-210">On the **Bitbucket user accounts** section, perform following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforbitbucket-tutorial/addon10.png)

    <span data-ttu-id="614a4-212">a.</span><span class="sxs-lookup"><span data-stu-id="614a4-212">a.</span></span> <span data-ttu-id="614a4-213">Válassza ki **felhasználók bitbucket szolgáltatásokhoz tartozó belső könyvtárban szükség esetén hozzon létre** , és írja be a felhasználói csoport megfelelő neve (lehet több nem.</span><span class="sxs-lookup"><span data-stu-id="614a4-213">Select **Create users in Bitbucket's internal Directory if needed** and enter the appropriate name of the group for users (can be multiple no.</span></span> <span data-ttu-id="614a4-214">vesszővel elválasztott csoportok).</span><span class="sxs-lookup"><span data-stu-id="614a4-214">of groups separated by comma).</span></span>

    <span data-ttu-id="614a4-215">b.</span><span class="sxs-lookup"><span data-stu-id="614a4-215">b.</span></span> <span data-ttu-id="614a4-216">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="614a4-216">Click **Next**.</span></span>

21. <span data-ttu-id="614a4-217">Kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="614a4-217">Click **Finish**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforbitbucket-tutorial/addon11.png)

22. <span data-ttu-id="614a4-219">Az a **tartományok ismert az Azure AD** csoportjában hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="614a4-219">On the **Known domains for Azure AD** section, perform following steps:</span></span> 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforbitbucket-tutorial/addon12.png)

    <span data-ttu-id="614a4-221">a.</span><span class="sxs-lookup"><span data-stu-id="614a4-221">a.</span></span> <span data-ttu-id="614a4-222">Válassza ki **tartományok ismert** a lap bal oldali panelen.</span><span class="sxs-lookup"><span data-stu-id="614a4-222">Select **Known domains** from the left panel of the page.</span></span>

    <span data-ttu-id="614a4-223">b.</span><span class="sxs-lookup"><span data-stu-id="614a4-223">b.</span></span> <span data-ttu-id="614a4-224">Adja meg a tartomány nevére a **tartományok ismert** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="614a4-224">Enter domain name in the **Known domains** textbox.</span></span>

    <span data-ttu-id="614a4-225">c.</span><span class="sxs-lookup"><span data-stu-id="614a4-225">c.</span></span> <span data-ttu-id="614a4-226">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="614a4-226">Click **Save**.</span></span>  

> [!TIP]
> <span data-ttu-id="614a4-227">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="614a4-227">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="614a4-228">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="614a4-228">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="614a4-229">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="614a4-229">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="614a4-230">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="614a4-230">Creating an Azure AD test user</span></span>
<span data-ttu-id="614a4-231">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="614a4-231">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="614a4-233">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="614a4-233">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="614a4-234">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="614a4-234">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kantegassoforbitbucket-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="614a4-236">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="614a4-236">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kantegassoforbitbucket-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="614a4-238">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="614a4-238">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kantegassoforbitbucket-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="614a4-240">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="614a4-240">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kantegassoforbitbucket-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="614a4-242">a.</span><span class="sxs-lookup"><span data-stu-id="614a4-242">a.</span></span> <span data-ttu-id="614a4-243">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="614a4-243">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="614a4-244">b.</span><span class="sxs-lookup"><span data-stu-id="614a4-244">b.</span></span> <span data-ttu-id="614a4-245">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="614a4-245">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="614a4-246">c.</span><span class="sxs-lookup"><span data-stu-id="614a4-246">c.</span></span> <span data-ttu-id="614a4-247">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="614a4-247">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="614a4-248">d.</span><span class="sxs-lookup"><span data-stu-id="614a4-248">d.</span></span> <span data-ttu-id="614a4-249">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="614a4-249">Click **Create**.</span></span>
 
### <a name="creating-a-kantega-sso-for-bitbucket-test-user"></a><span data-ttu-id="614a4-250">Egy Kantega SSO a Bitbucketből tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="614a4-250">Creating a Kantega SSO for Bitbucket test user</span></span>

<span data-ttu-id="614a4-251">Ahhoz, hogy az Azure AD-felhasználók Bitbucket bejelentkezni, akkor ki kell építenie Bitbucket be.</span><span class="sxs-lookup"><span data-stu-id="614a4-251">To enable Azure AD users to log in to Bitbucket, they must be provisioned into Bitbucket.</span></span> <span data-ttu-id="614a4-252">A Bitbucketből Kantega egyszeri Bejelentkezést egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="614a4-252">In Kantega SSO for Bitbucket, provisioning is a manual task.</span></span>

<span data-ttu-id="614a4-253">**Felhasználói fiók létrehozásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="614a4-253">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="614a4-254">Jelentkezzen be rendszergazdaként a Bitbucketből vállalati webhely.</span><span class="sxs-lookup"><span data-stu-id="614a4-254">Log in to your Bitbucket company site as an administrator.</span></span>

2. <span data-ttu-id="614a4-255">Kattintson a beállítások ikonra.</span><span class="sxs-lookup"><span data-stu-id="614a4-255">Click on settings icon.</span></span>

    ![Alkalmazott hozzáadása](./media/active-directory-saas-kantegassoforbitbucket-tutorial/user1.png) 

3. <span data-ttu-id="614a4-257">A **felügyeleti** szakasz lapra, majd **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="614a4-257">Under **Administration** tab section, click **Users**.</span></span>

    ![Alkalmazott hozzáadása](./media/active-directory-saas-kantegassoforbitbucket-tutorial/user2.png)

4. <span data-ttu-id="614a4-259">Kattintson a **a felhasználó létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="614a4-259">Click **Create user**.</span></span>

    ![Alkalmazott hozzáadása](./media/active-directory-saas-kantegassoforbitbucket-tutorial/user3.png)     

5. <span data-ttu-id="614a4-261">Az a **felhasználó létrehozása** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="614a4-261">On the **Create User** dialog page, perform the following steps:</span></span>

    ![Alkalmazott hozzáadása](./media/active-directory-saas-kantegassoforbitbucket-tutorial/user4.png) 

    <span data-ttu-id="614a4-263">a.</span><span class="sxs-lookup"><span data-stu-id="614a4-263">a.</span></span> <span data-ttu-id="614a4-264">Az a **felhasználónév** szövegmezőben, az e-mailt a felhasználó típusát, például Brittasimon@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="614a4-264">In the **Username** textbox, type the email of user like Brittasimon@contoso.com.</span></span>
    
    <span data-ttu-id="614a4-265">b.</span><span class="sxs-lookup"><span data-stu-id="614a4-265">b.</span></span> <span data-ttu-id="614a4-266">Az a **teljes nevét** szövegmező, a felhasználó például Britta Simon típus teljes nevét.</span><span class="sxs-lookup"><span data-stu-id="614a4-266">In the **Full Name** textbox, type full name of the user like Britta Simon.</span></span>
    
    <span data-ttu-id="614a4-267">c.</span><span class="sxs-lookup"><span data-stu-id="614a4-267">c.</span></span> <span data-ttu-id="614a4-268">Az a **E-mail cím** szövegmező, a felhasználó e-mail címe típusát, például Brittasimon@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="614a4-268">In the **Email address** textbox, type the email address of user like Brittasimon@contoso.com.</span></span>

    <span data-ttu-id="614a4-269">d.</span><span class="sxs-lookup"><span data-stu-id="614a4-269">d.</span></span> <span data-ttu-id="614a4-270">Az a **jelszó** szövegmező, írja be a felhasználó jelszavát.</span><span class="sxs-lookup"><span data-stu-id="614a4-270">In the **Password** textbox, type the password of user.</span></span>  

    <span data-ttu-id="614a4-271">e.</span><span class="sxs-lookup"><span data-stu-id="614a4-271">e.</span></span> <span data-ttu-id="614a4-272">Az a **jelszó megerősítése** szövegmezőhöz felhasználói jelszó megerősítése.</span><span class="sxs-lookup"><span data-stu-id="614a4-272">In the **Confirm Password** textbox, reenter the password of user.</span></span>

    <span data-ttu-id="614a4-273">f.</span><span class="sxs-lookup"><span data-stu-id="614a4-273">f.</span></span> <span data-ttu-id="614a4-274">Kattintson a **a felhasználó létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="614a4-274">Click **Create user**.</span></span>   

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="614a4-275">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="614a4-275">Assigning the Azure AD test user</span></span>

<span data-ttu-id="614a4-276">Ebben a szakaszban Azure egyszeri bejelentkezéshez használandó hozzáférést biztosít a Bitbucketből Kantega SSO Britta Simon engedélyezi.</span><span class="sxs-lookup"><span data-stu-id="614a4-276">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Kantega SSO for Bitbucket.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="614a4-278">**Britta Simon hozzárendelése a Bitbucketből Kantega SSO, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="614a4-278">**To assign Britta Simon to Kantega SSO for Bitbucket, perform the following steps:**</span></span>

1. <span data-ttu-id="614a4-279">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="614a4-279">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="614a4-281">Az alkalmazások listában válassza ki a **Kantega SSO a Bitbucketből**.</span><span class="sxs-lookup"><span data-stu-id="614a4-281">In the applications list, select **Kantega SSO for Bitbucket**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforbitbucket-tutorial/tutorial_kantegassoforbitbucket_app.png) 

3. <span data-ttu-id="614a4-283">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="614a4-283">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="614a4-285">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="614a4-285">Click **Add** button.</span></span> <span data-ttu-id="614a4-286">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="614a4-286">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="614a4-288">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="614a4-288">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="614a4-289">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="614a4-289">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="614a4-290">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="614a4-290">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="614a4-291">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="614a4-291">Testing single sign-on</span></span>

<span data-ttu-id="614a4-292">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="614a4-292">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="614a4-293">Kattintva a Kantega SSO Bitbucket csempe a hozzáférési panelen, meg kell beolvasása automatikusan bejelentkezett a Kantega egyszeri Bejelentkezést a Bitbucketből alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="614a4-293">When you click the Kantega SSO for Bitbucket tile in the Access Panel, you should get automatically signed-on to your Kantega SSO for Bitbucket application.</span></span>
<span data-ttu-id="614a4-294">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="614a4-294">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="614a4-295">További források</span><span class="sxs-lookup"><span data-stu-id="614a4-295">Additional resources</span></span>

* [<span data-ttu-id="614a4-296">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="614a4-296">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="614a4-297">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="614a4-297">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-kantegassoforbitbucket-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-kantegassoforbitbucket-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-kantegassoforbitbucket-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-kantegassoforbitbucket-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-kantegassoforbitbucket-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-kantegassoforbitbucket-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-kantegassoforbitbucket-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-kantegassoforbitbucket-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-kantegassoforbitbucket-tutorial/tutorial_general_203.png

