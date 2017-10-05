---
title: "Oktatóanyag: Azure Active Directoryval integrált Mimecast felügyeleti konzol |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és a felügyeleti konzol Mimecast között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 81c50614-f49b-4bbc-97d5-3cf77154305f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/08/2017
ms.author: jeedes
ms.openlocfilehash: f401f592d79ad954aa466de74d3e3fbb18aa9a5b
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="tutorial-azure-active-directory-integration-with-mimecast-admin-console"></a><span data-ttu-id="28a9e-103">Oktatóanyag: Azure Active Directoryval integrált Mimecast felügyeleti konzol</span><span class="sxs-lookup"><span data-stu-id="28a9e-103">Tutorial: Azure Active Directory integration with Mimecast Admin Console</span></span>

<span data-ttu-id="28a9e-104">Ebben az oktatóanyagban elsajátíthatja Mimecast felügyeleti konzol integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="28a9e-104">In this tutorial, you learn how to integrate Mimecast Admin Console with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="28a9e-105">Felügyeleti konzol Mimecast integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="28a9e-105">Integrating Mimecast Admin Console with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="28a9e-106">Az Azure AD, aki hozzáfér Mimecast felügyeleti konzol szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="28a9e-106">You can control in Azure AD who has access to Mimecast Admin Console.</span></span>
- <span data-ttu-id="28a9e-107">Az Azure AD-fiókok a engedélyezheti a felhasználóknak, hogy automatikusan lekérni bejelentkezett Mimecast felügyeleti konzol (egyszeri bejelentkezés).</span><span class="sxs-lookup"><span data-stu-id="28a9e-107">You can enable your users to automatically get signed-on to Mimecast Admin Console (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="28a9e-108">A fiók egyetlen központi helyen – az Azure-portálon kezelheti.</span><span class="sxs-lookup"><span data-stu-id="28a9e-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="28a9e-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="28a9e-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="28a9e-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="28a9e-110">Prerequisites</span></span>

<span data-ttu-id="28a9e-111">Konfigurálása az Azure AD-integrációs Mimecast felügyeleti konzol, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="28a9e-111">To configure Azure AD integration with Mimecast Admin Console, you need the following items:</span></span>

- <span data-ttu-id="28a9e-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="28a9e-112">An Azure AD subscription</span></span>
- <span data-ttu-id="28a9e-113">A felügyeleti konzol Mimecast egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="28a9e-113">A Mimecast Admin Console single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="28a9e-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="28a9e-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="28a9e-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="28a9e-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="28a9e-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="28a9e-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="28a9e-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="28a9e-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="28a9e-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="28a9e-118">Scenario description</span></span>
<span data-ttu-id="28a9e-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="28a9e-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="28a9e-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="28a9e-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="28a9e-121">A gyűjteményből Mimecast felügyeleti konzol hozzáadása</span><span class="sxs-lookup"><span data-stu-id="28a9e-121">Adding Mimecast Admin Console from the gallery</span></span>
2. <span data-ttu-id="28a9e-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="28a9e-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-mimecast-admin-console-from-the-gallery"></a><span data-ttu-id="28a9e-123">A gyűjteményből Mimecast felügyeleti konzol hozzáadása</span><span class="sxs-lookup"><span data-stu-id="28a9e-123">Adding Mimecast Admin Console from the gallery</span></span>
<span data-ttu-id="28a9e-124">Az Azure AD integrálása a Mimecast felügyeleti konzol konfigurálásához kell hozzáadnia Mimecast felügyeleti konzol a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="28a9e-124">To configure the integration of Mimecast Admin Console into Azure AD, you need to add Mimecast Admin Console from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="28a9e-125">**Adja hozzá a Mimecast felügyeleti konzol a gyűjteményből, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="28a9e-125">**To add Mimecast Admin Console from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="28a9e-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="28a9e-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Az Azure Active Directory gomb][1]

2. <span data-ttu-id="28a9e-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="28a9e-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="28a9e-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="28a9e-129">Then go to **All applications**.</span></span>

    ![A vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="28a9e-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="28a9e-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Az új alkalmazás gomb][3]

4. <span data-ttu-id="28a9e-133">Írja be a keresőmezőbe, **Mimecast felügyeleti konzol**, jelölje be **Mimecast felügyeleti konzol** eredmény panelen kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="28a9e-133">In the search box, type **Mimecast Admin Console**, select **Mimecast Admin Console** from result panel then click **Add** button to add the application.</span></span>

    ![Az eredménylistában Mimecast felügyeleti konzol](./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_mimecastadminconsole_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="28a9e-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="28a9e-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="28a9e-136">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD az egyszeri bejelentkezés Mimecast felügyeleti konzol "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="28a9e-136">In this section, you configure and test Azure AD single sign-on with Mimecast Admin Console based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="28a9e-137">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Mimecast felügyeleti konzoljában a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="28a9e-137">For single sign-on to work, Azure AD needs to know what the counterpart user in Mimecast Admin Console is to a user in Azure AD.</span></span> <span data-ttu-id="28a9e-138">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó Mimecast felügyeleti konzol közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="28a9e-138">In other words, a link relationship between an Azure AD user and the related user in Mimecast Admin Console needs to be established.</span></span>

<span data-ttu-id="28a9e-139">Mimecast felügyeleti konzolon, rendelje az értékét a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="28a9e-139">In Mimecast Admin Console, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="28a9e-140">Az Azure AD az egyszeri bejelentkezés Mimecast felügyeleti konzol tesztelése és konfigurálása, kell végrehajtani a következő építőelemeket:</span><span class="sxs-lookup"><span data-stu-id="28a9e-140">To configure and test Azure AD single sign-on with Mimecast Admin Console, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="28a9e-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="28a9e-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="28a9e-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="28a9e-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="28a9e-143">**[Felügyeleti konzol Mimecast tesztfelhasználó létrehozása](#create-a-mimecast-admin-console-test-user)**  - való egy megfelelője a Britta Simon Mimecast felügyeleti konzol, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="28a9e-143">**[Create a Mimecast Admin Console test user](#create-a-mimecast-admin-console-test-user)** - to have a counterpart of Britta Simon in Mimecast Admin Console that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="28a9e-144">**[Rendelje hozzá az Azure AD-teszt felhasználó](#assign-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="28a9e-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="28a9e-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="28a9e-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="28a9e-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="28a9e-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="28a9e-147">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és a felügyeleti konzol Mimecast alkalmazásban egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="28a9e-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Mimecast Admin Console application.</span></span>

<span data-ttu-id="28a9e-148">**Mimecast felügyeleti konzol konfigurálása az Azure AD egyszeri bejelentkezést, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="28a9e-148">**To configure Azure AD single sign-on with Mimecast Admin Console, perform the following steps:**</span></span>

1. <span data-ttu-id="28a9e-149">Az Azure portálon a a **Mimecast felügyeleti konzol** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="28a9e-149">In the Azure portal, on the **Mimecast Admin Console** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. <span data-ttu-id="28a9e-151">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="28a9e-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_mimecastadminconsole_samlbase.png)

3. <span data-ttu-id="28a9e-153">Az a **Mimecast felügyeleti konzol tartományát és URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="28a9e-153">On the **Mimecast Admin Console Domain and URLs** section, perform the following steps:</span></span>

    ![Az egyszeri bejelentkezés információk Mimecast felügyeleti konzol tartományát és URL-címek](./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_mimecastadminconsole_url.png)

    <span data-ttu-id="28a9e-155">Az a **bejelentkezési URL-cím** szövegmező, írja be az URL-cím:</span><span class="sxs-lookup"><span data-stu-id="28a9e-155">In the **Sign-on URL** textbox, type the URL:</span></span>
    | |
    | -- |
    | `https://webmail-uk.mimecast.com`|
    | `https://webmail-us.mimecast.com`|

    > [!NOTE] 
    > <span data-ttu-id="28a9e-156">A bejelentkezési URL-címet az adott régió.</span><span class="sxs-lookup"><span data-stu-id="28a9e-156">The sign on URL is region specific.</span></span>

4. <span data-ttu-id="28a9e-157">Az a **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="28a9e-157">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![A tanúsítvány letöltési hivatkozását](./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_mimecastadminconsole_certificate.png) 

5. <span data-ttu-id="28a9e-159">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="28a9e-159">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="28a9e-161">A a **Mimecast felügyeleti konzol konfiguráció** kattintson **konfigurálása Mimecast felügyeleti konzol** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="28a9e-161">On the **Mimecast Admin Console Configuration** section, click **Configure Mimecast Admin Console** to open **Configure sign-on** window.</span></span> <span data-ttu-id="28a9e-162">Másolás a **SAML Entitásazonosító és SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="28a9e-162">Copy the **SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Mimecast felügyeleti konzol konfiguráció](./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_mimecastadminconsole_configure.png) 

7. <span data-ttu-id="28a9e-164">Egy másik webes böngészőablakban jelentkezzen be a Mimecast felügyeleti konzolt rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="28a9e-164">In a different web browser window, log into your Mimecast Admin Console as an administrator.</span></span>

8. <span data-ttu-id="28a9e-165">Ugrás a **szolgáltatások \> alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="28a9e-165">Go to **Services \> Application**.</span></span>

    <span data-ttu-id="28a9e-166">![Szolgáltatások](./media/active-directory-saas-mimecast-admin-console-tutorial/ic794998.png "szolgáltatások")</span><span class="sxs-lookup"><span data-stu-id="28a9e-166">![Services](./media/active-directory-saas-mimecast-admin-console-tutorial/ic794998.png "Services")</span></span>

9. <span data-ttu-id="28a9e-167">Kattintson a **hitelesítési profilok**.</span><span class="sxs-lookup"><span data-stu-id="28a9e-167">Click **Authentication Profiles**.</span></span>

    <span data-ttu-id="28a9e-168">![Hitelesítési profilok](./media/active-directory-saas-mimecast-admin-console-tutorial/ic794999.png "hitelesítési profilok")</span><span class="sxs-lookup"><span data-stu-id="28a9e-168">![Authentication Profiles](./media/active-directory-saas-mimecast-admin-console-tutorial/ic794999.png "Authentication Profiles")</span></span>
    
10. <span data-ttu-id="28a9e-169">Kattintson a **új hitelesítési profil**.</span><span class="sxs-lookup"><span data-stu-id="28a9e-169">Click **New Authentication Profile**.</span></span>

    <span data-ttu-id="28a9e-170">![Új hitelesítési profilok](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795000.png "új hitelesítési profilok")</span><span class="sxs-lookup"><span data-stu-id="28a9e-170">![New Authentication Profiles](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795000.png "New Authentication Profiles")</span></span>

11. <span data-ttu-id="28a9e-171">Az a **hitelesítési profil** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="28a9e-171">In the **Authentication Profile** section, perform the following steps:</span></span>

    <span data-ttu-id="28a9e-172">![Hitelesítési profil](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795015.png "hitelesítési profil")</span><span class="sxs-lookup"><span data-stu-id="28a9e-172">![Authentication Profile](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795015.png "Authentication Profile")</span></span>
    
    <span data-ttu-id="28a9e-173">a.</span><span class="sxs-lookup"><span data-stu-id="28a9e-173">a.</span></span> <span data-ttu-id="28a9e-174">Az a **leírás** szövegmező, adja meg a konfiguráció nevét.</span><span class="sxs-lookup"><span data-stu-id="28a9e-174">In the **Description** textbox, type a name for your configuration.</span></span>
    
    <span data-ttu-id="28a9e-175">b.</span><span class="sxs-lookup"><span data-stu-id="28a9e-175">b.</span></span> <span data-ttu-id="28a9e-176">Válassza ki **Mimecast felügyeleti konzol az SAML-alapú hitelesítés kényszerítéséhez**.</span><span class="sxs-lookup"><span data-stu-id="28a9e-176">Select **Enforce SAML Authentication for Mimecast Admin Console**.</span></span>
    
    <span data-ttu-id="28a9e-177">c.</span><span class="sxs-lookup"><span data-stu-id="28a9e-177">c.</span></span> <span data-ttu-id="28a9e-178">Mint **szolgáltató**, jelölje be **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="28a9e-178">As **Provider**, select **Azure Active Directory**.</span></span>
    
    <span data-ttu-id="28a9e-179">d.</span><span class="sxs-lookup"><span data-stu-id="28a9e-179">d.</span></span> <span data-ttu-id="28a9e-180">Beillesztés **SAML Entitásazonosító**, amely az Azure-portálról másolta a **kiállítójának URL-címe** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="28a9e-180">Paste **SAML Entity ID**, which you have copied from the Azure portal into the **Issuer URL** textbox.</span></span>
    
    <span data-ttu-id="28a9e-181">e.</span><span class="sxs-lookup"><span data-stu-id="28a9e-181">e.</span></span> <span data-ttu-id="28a9e-182">Beillesztés **SAML-alapú egyszeri bejelentkezési URL-címe**, amely az Azure-portálról másolta a **bejelentkezési URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="28a9e-182">Paste **SAML Single Sign-On Service URL**, which you have copied from the Azure portal into the **Login URL** textbox.</span></span>

    <span data-ttu-id="28a9e-183">f.</span><span class="sxs-lookup"><span data-stu-id="28a9e-183">f.</span></span> <span data-ttu-id="28a9e-184">Beillesztés **SAML-alapú egyszeri bejelentkezési URL-címe**, amely az Azure-portálról másolta a **kijelentkezési URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="28a9e-184">Paste **SAML Single Sign-On Service URL**, which you have copied from the Azure portal into the **Logout URL** textbox.</span></span>
    
    >[!NOTE]
    ><span data-ttu-id="28a9e-185">A bejelentkezési URL-címe és kijelentkezési URL-cím értéke ugyanazok a Mimecast felügyeleti konzolon.</span><span class="sxs-lookup"><span data-stu-id="28a9e-185">The Login URL value and the Logout URL value are for the Mimecast Admin Console the same.</span></span>
    
    <span data-ttu-id="28a9e-186">g.</span><span class="sxs-lookup"><span data-stu-id="28a9e-186">g.</span></span> <span data-ttu-id="28a9e-187">Nyissa meg a Jegyzettömbben az Azure portálról letöltött a base-64 tanúsítvány, távolítsa el az első sorban ("*--*") és az utolsó sort ("*--*"), másolja be azt a maradék tartalmat a vágólapra, majd illessze be azt a **Identitástanúsítvány szolgáltató (Metadata)** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="28a9e-187">Open your base-64 certificate downloaded from Azure portal in notepad, remove the first line (“*--*“) and the last line (“*--*“), copy the remaining content of it into your clipboard, and then paste it to the **Identity Provider Certificate (Metadata)** textbox.</span></span>
    
    <span data-ttu-id="28a9e-188">h.</span><span class="sxs-lookup"><span data-stu-id="28a9e-188">h.</span></span> <span data-ttu-id="28a9e-189">Válassza ki **engedélyezése egyszeri bejelentkezéshez**.</span><span class="sxs-lookup"><span data-stu-id="28a9e-189">Select **Allow Single Sign On**.</span></span>
    
    <span data-ttu-id="28a9e-190">i.</span><span class="sxs-lookup"><span data-stu-id="28a9e-190">i.</span></span> <span data-ttu-id="28a9e-191">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="28a9e-191">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="28a9e-192">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="28a9e-192">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="28a9e-193">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="28a9e-193">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="28a9e-194">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="28a9e-194">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="28a9e-195">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="28a9e-195">Create an Azure AD test user</span></span>

<span data-ttu-id="28a9e-196">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="28a9e-196">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Hozzon létre egy Azure AD-teszt felhasználó][100]

<span data-ttu-id="28a9e-198">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="28a9e-198">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="28a9e-199">Az Azure portálon a bal oldali ablaktáblán kattintson a **Azure Active Directory** gombra.</span><span class="sxs-lookup"><span data-stu-id="28a9e-199">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Az Azure Active Directory gomb](./media/active-directory-saas-mimecast-admin-console-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="28a9e-201">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok**, és kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="28a9e-201">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    ![A "felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-mimecast-admin-console-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="28a9e-203">Megnyitásához a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** tetején a **minden felhasználó** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="28a9e-203">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![A Hozzáadás gombra.](./media/active-directory-saas-mimecast-admin-console-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="28a9e-205">Az a **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="28a9e-205">In the **User** dialog box, perform the following steps:</span></span>

    ![A felhasználó párbeszédpanel](./media/active-directory-saas-mimecast-admin-console-tutorial/create_aaduser_04.png)

    <span data-ttu-id="28a9e-207">a.</span><span class="sxs-lookup"><span data-stu-id="28a9e-207">a.</span></span> <span data-ttu-id="28a9e-208">Az a **neve** mezőbe írja be **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="28a9e-208">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="28a9e-209">b.</span><span class="sxs-lookup"><span data-stu-id="28a9e-209">b.</span></span> <span data-ttu-id="28a9e-210">Az a **felhasználónév** mezőbe írja be a felhasználó e-mail címe az Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="28a9e-210">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="28a9e-211">c.</span><span class="sxs-lookup"><span data-stu-id="28a9e-211">c.</span></span> <span data-ttu-id="28a9e-212">Válassza ki a **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a megjelenített érték a **jelszó** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="28a9e-212">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="28a9e-213">d.</span><span class="sxs-lookup"><span data-stu-id="28a9e-213">d.</span></span> <span data-ttu-id="28a9e-214">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="28a9e-214">Click **Create**.</span></span>
 
### <a name="create-a-mimecast-admin-console-test-user"></a><span data-ttu-id="28a9e-215">Felügyeleti konzol Mimecast tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="28a9e-215">Create a Mimecast Admin Console test user</span></span>

<span data-ttu-id="28a9e-216">Ahhoz, hogy az Azure AD-felhasználók Mimecast felügyeleti konzol bejelentkezni, akkor ki kell építenie Mimecast felügyeleti konzolhoz.</span><span class="sxs-lookup"><span data-stu-id="28a9e-216">In order to enable Azure AD users to log into Mimecast Admin Console, they must be provisioned into Mimecast Admin Console.</span></span> <span data-ttu-id="28a9e-217">Mimecast felügyeleti konzolon, ha egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="28a9e-217">In the case of Mimecast Admin Console, provisioning is a manual task.</span></span>

* <span data-ttu-id="28a9e-218">Regisztrálja a tartományi felhasználók létrehozása előtt kell.</span><span class="sxs-lookup"><span data-stu-id="28a9e-218">You need to register a domain before you can create users.</span></span>

<span data-ttu-id="28a9e-219">**Adja meg a felhasználók átadása, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="28a9e-219">**To configure user provisioning, perform the following steps:**</span></span>

1. <span data-ttu-id="28a9e-220">Jelentkezzen be a **Mimecast felügyeleti konzol** rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="28a9e-220">Sign on to your **Mimecast Admin Console** as administrator.</span></span>
2. <span data-ttu-id="28a9e-221">Ugrás a **könyvtárak \> belső**.</span><span class="sxs-lookup"><span data-stu-id="28a9e-221">Go to **Directories \> Internal**.</span></span>
   
   <span data-ttu-id="28a9e-222">![Könyvtárak](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795003.png "könyvtárak")</span><span class="sxs-lookup"><span data-stu-id="28a9e-222">![Directories](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795003.png "Directories")</span></span>
3. <span data-ttu-id="28a9e-223">Kattintson a **regisztrálni az új tartomány**.</span><span class="sxs-lookup"><span data-stu-id="28a9e-223">Click **Register New Domain**.</span></span>
   
   <span data-ttu-id="28a9e-224">![Új tartomány regisztrálása](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795004.png "új tartomány regisztrálása")</span><span class="sxs-lookup"><span data-stu-id="28a9e-224">![Register New Domain](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795004.png "Register New Domain")</span></span>
4. <span data-ttu-id="28a9e-225">Az új tartomány létrehozása után kattintson **új cím**.</span><span class="sxs-lookup"><span data-stu-id="28a9e-225">After your new domain has been created, click **New Address**.</span></span>
   
   <span data-ttu-id="28a9e-226">![Új cím](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795005.png "új cím")</span><span class="sxs-lookup"><span data-stu-id="28a9e-226">![New Address](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795005.png "New Address")</span></span>
5. <span data-ttu-id="28a9e-227">Új cím párbeszédpanelen hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="28a9e-227">In the new address dialog, perform the following steps:</span></span>
   
   <span data-ttu-id="28a9e-228">![Mentés](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795006.png "mentése")</span><span class="sxs-lookup"><span data-stu-id="28a9e-228">![Save](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795006.png "Save")</span></span>
   
   <span data-ttu-id="28a9e-229">a.</span><span class="sxs-lookup"><span data-stu-id="28a9e-229">a.</span></span> <span data-ttu-id="28a9e-230">Típus a **E-mail cím**, **globális név**, **jelszó**, és **jelszó megerősítése** attribútumok egy érvényes Azure ad meg fiók szeretné a kapcsolódó szövegmezők kiépíteni.</span><span class="sxs-lookup"><span data-stu-id="28a9e-230">Type the **Email Address**, **Global Name**, **Password**, and **Confirm Password** attributes of a valid Azure AD account you want to provision into the related textboxes.</span></span>

   <span data-ttu-id="28a9e-231">b.</span><span class="sxs-lookup"><span data-stu-id="28a9e-231">b.</span></span> <span data-ttu-id="28a9e-232">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="28a9e-232">Click **Save**.</span></span>

>[!NOTE]
><span data-ttu-id="28a9e-233">Mimecast felügyeleti konzol felhasználói fiók létrehozása eszközök vagy Mimecast felügyeleti konzol által nyújtott API-k segítségével kiépíteni az Azure AD felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="28a9e-233">You can use any other Mimecast Admin Console user account creation tools or APIs provided by Mimecast Admin Console to provision Azure AD user accounts.</span></span> 

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="28a9e-234">Rendelje hozzá az Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="28a9e-234">Assign the Azure AD test user</span></span>

<span data-ttu-id="28a9e-235">Ebben a szakaszban engedélyezze Britta Simon Azure egyszeri bejelentkezés által biztosított hozzáférés Mimecast felügyeleti konzol használatához.</span><span class="sxs-lookup"><span data-stu-id="28a9e-235">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Mimecast Admin Console.</span></span>

![A felhasználói szerepkör hozzárendelése][200] 

<span data-ttu-id="28a9e-237">**Britta Simon hozzárendelése Mimecast felügyeleti konzol, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="28a9e-237">**To assign Britta Simon to Mimecast Admin Console, perform the following steps:**</span></span>

1. <span data-ttu-id="28a9e-238">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="28a9e-238">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="28a9e-240">Az alkalmazások listában válassza ki a **Mimecast felügyeleti konzol**.</span><span class="sxs-lookup"><span data-stu-id="28a9e-240">In the applications list, select **Mimecast Admin Console**.</span></span>

    ![Az alkalmazások listáját a Mimecast felügyeleti konzol hivatkozás](./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_mimecastadminconsole_app.png)  

3. <span data-ttu-id="28a9e-242">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="28a9e-242">In the menu on the left, click **Users and groups**.</span></span>

    ![A "Felhasználók és csoportok" hivatkozásra][202]

4. <span data-ttu-id="28a9e-244">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="28a9e-244">Click **Add** button.</span></span> <span data-ttu-id="28a9e-245">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="28a9e-245">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![A hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="28a9e-247">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="28a9e-247">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="28a9e-248">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="28a9e-248">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="28a9e-249">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="28a9e-249">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="28a9e-250">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="28a9e-250">Test single sign-on</span></span>

<span data-ttu-id="28a9e-251">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="28a9e-251">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="28a9e-252">Ha a hozzáférési panelen Mimecast felügyeleti konzol csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az Mimecast felügyeleti konzol alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="28a9e-252">When you click the Mimecast Admin Console tile in the Access Panel, you should get automatically signed-on to your Mimecast Admin Console application.</span></span>
<span data-ttu-id="28a9e-253">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="28a9e-253">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="28a9e-254">További források</span><span class="sxs-lookup"><span data-stu-id="28a9e-254">Additional resources</span></span>

* [<span data-ttu-id="28a9e-255">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="28a9e-255">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="28a9e-256">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="28a9e-256">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_203.png

