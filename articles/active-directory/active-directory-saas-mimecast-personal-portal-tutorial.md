---
title: "Oktatóanyag: Azure Active Directory-integráció Mimecast személyes portállal |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és a személyes Portal Mimecast között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 345b22be-d87e-45a4-b4c0-70a67eaf9bfd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: bf46da35a55608d7e4656c9dd3ad9d5f2253e225
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-mimecast-personal-portal"></a><span data-ttu-id="94a10-103">Oktatóanyag: Azure Active Directory-integráció Mimecast személyes portállal</span><span class="sxs-lookup"><span data-stu-id="94a10-103">Tutorial: Azure Active Directory integration with Mimecast Personal Portal</span></span>

<span data-ttu-id="94a10-104">Ebben az oktatóanyagban elsajátíthatja Mimecast személyes Portal integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="94a10-104">In this tutorial, you learn how to integrate Mimecast Personal Portal with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="94a10-105">Mimecast személyes Portal integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="94a10-105">Integrating Mimecast Personal Portal with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="94a10-106">Megadhatja a Mimecast személyes Portal hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="94a10-106">You can control in Azure AD who has access to Mimecast Personal Portal</span></span>
- <span data-ttu-id="94a10-107">Az Azure AD-fiókok a engedélyezheti a felhasználóknak, hogy automatikusan lekérni bejelentkezett Mimecast személyes portál (egyszeri bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="94a10-107">You can enable your users to automatically get signed-on to Mimecast Personal Portal (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="94a10-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="94a10-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="94a10-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="94a10-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="94a10-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="94a10-110">Prerequisites</span></span>

<span data-ttu-id="94a10-111">Az Azure AD-integráció konfigurálása Mimecast személyes portállal, a következő elemeket kell:</span><span class="sxs-lookup"><span data-stu-id="94a10-111">To configure Azure AD integration with Mimecast Personal Portal, you need the following items:</span></span>

- <span data-ttu-id="94a10-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="94a10-112">An Azure AD subscription</span></span>
- <span data-ttu-id="94a10-113">Egy Mimecast személyes Portal egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="94a10-113">A Mimecast Personal Portal single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="94a10-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="94a10-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="94a10-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="94a10-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="94a10-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="94a10-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="94a10-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="94a10-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="94a10-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="94a10-118">Scenario description</span></span>
<span data-ttu-id="94a10-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="94a10-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="94a10-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="94a10-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="94a10-121">Mimecast személyes portál hozzáadását a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="94a10-121">Adding Mimecast Personal Portal from the gallery</span></span>
2. <span data-ttu-id="94a10-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="94a10-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-mimecast-personal-portal-from-the-gallery"></a><span data-ttu-id="94a10-123">Mimecast személyes portál hozzáadását a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="94a10-123">Adding Mimecast Personal Portal from the gallery</span></span>
<span data-ttu-id="94a10-124">Az Azure AD integrálása a Mimecast személyes Portal konfigurálásához kell hozzáadnia Mimecast személyes Portal a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="94a10-124">To configure the integration of Mimecast Personal Portal into Azure AD, you need to add Mimecast Personal Portal from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="94a10-125">**A gyűjteményből Mimecast személyes Portal hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="94a10-125">**To add Mimecast Personal Portal from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="94a10-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="94a10-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="94a10-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="94a10-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="94a10-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="94a10-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="94a10-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="94a10-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="94a10-133">Írja be a keresőmezőbe, **Mimecast személyes Portal**.</span><span class="sxs-lookup"><span data-stu-id="94a10-133">In the search box, type **Mimecast Personal Portal**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_mimecastpersonalportal_search.png)

5. <span data-ttu-id="94a10-135">Az eredmények panelen válassza ki a **Mimecast személyes Portal**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="94a10-135">In the results panel, select **Mimecast Personal Portal**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_mimecastpersonalportal_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="94a10-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="94a10-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="94a10-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD az egyszeri bejelentkezés Mimecast személyes Portal "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="94a10-138">In this section, you configure and test Azure AD single sign-on with Mimecast Personal Portal based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="94a10-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Mimecast személyes portálon a felhasználók az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="94a10-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Mimecast Personal Portal is to a user in Azure AD.</span></span> <span data-ttu-id="94a10-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Mimecast személyes portál közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="94a10-140">In other words, a link relationship between an Azure AD user and the related user in Mimecast Personal Portal needs to be established.</span></span>

<span data-ttu-id="94a10-141">Mimecast személyes portálon, rendelje az értékét a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="94a10-141">In Mimecast Personal Portal, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="94a10-142">Az Azure AD az egyszeri bejelentkezés Mimecast személyes portállal tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="94a10-142">To configure and test Azure AD single sign-on with Mimecast Personal Portal, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="94a10-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="94a10-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="94a10-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="94a10-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="94a10-145">**[Mimecast személyes Portal tesztfelhasználó létrehozása](#creating-a-mimecast-personal-portal-test-user)**  - való egy megfelelője a Britta Simon Mimecast személyes portál, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="94a10-145">**[Creating a Mimecast Personal Portal test user](#creating-a-mimecast-personal-portal-test-user)** - to have a counterpart of Britta Simon in Mimecast Personal Portal that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="94a10-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="94a10-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="94a10-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="94a10-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="94a10-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="94a10-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="94a10-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Mimecast személyes portál alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="94a10-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Mimecast Personal Portal application.</span></span>

<span data-ttu-id="94a10-150">**Konfigurálja az Azure AD az egyszeri bejelentkezés Mimecast személyes portállal, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="94a10-150">**To configure Azure AD single sign-on with Mimecast Personal Portal, perform the following steps:**</span></span>

1. <span data-ttu-id="94a10-151">Az Azure portálon a a **Mimecast személyes Portal** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="94a10-151">In the Azure portal, on the **Mimecast Personal Portal** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="94a10-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="94a10-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_mimecastpersonalportal_samlbase.png)

3. <span data-ttu-id="94a10-155">Az a **Mimecast személyes Portal tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="94a10-155">On the **Mimecast Personal Portal Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_mimecastpersonalportal_url.png)

    <span data-ttu-id="94a10-157">a.</span><span class="sxs-lookup"><span data-stu-id="94a10-157">a.</span></span> <span data-ttu-id="94a10-158">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:</span><span class="sxs-lookup"><span data-stu-id="94a10-158">In the **Sign-on URL** textbox, type a URL using the following pattern:</span></span> 
    | |     
    | ----------------------------------------|
    | `https://webmail-uk.mimecast.com`|
    | `https://webmail-us.mimecast.com`|
    | |
   
    <span data-ttu-id="94a10-159">b.</span><span class="sxs-lookup"><span data-stu-id="94a10-159">b.</span></span> <span data-ttu-id="94a10-160">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:</span><span class="sxs-lookup"><span data-stu-id="94a10-160">In the **Identifier** textbox, type a URL using the following pattern:</span></span>

    | |     
    | --- |
    | `https://webmail-us.mimecast.com/sso/<companyname>`|
    | `https://webmail-uk.mimecast.com/sso/<companyname>`|    
    | `https://webmail-za.mimecast.com/sso/<companyname>`|
    | `https://webmail.mimecast-offshore.com/sso/<companyname>`|
    ||                                                 
    
    > [!NOTE] 
    > <span data-ttu-id="94a10-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="94a10-161">These values are not real.</span></span> <span data-ttu-id="94a10-162">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="94a10-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="94a10-163">Ügyfél [Mimecast személyes portál ügyfél-támogatási csoport](https://www.mimecast.com/customer-success/technical-support/) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="94a10-163">Contact [Mimecast Personal Portal Client support team](https://www.mimecast.com/customer-success/technical-support/) to get these values.</span></span> 
 


4. <span data-ttu-id="94a10-164">Az a **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="94a10-164">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_mimecastpersonalportal_certificate.png) 

5. <span data-ttu-id="94a10-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="94a10-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="94a10-168">A a **Mimecast személyes portál konfigurációs** kattintson **Mimecast személyes portál konfigurálása** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="94a10-168">On the **Mimecast Personal Portal Configuration** section, click **Configure Mimecast Personal Portal** to open **Configure sign-on** window.</span></span> <span data-ttu-id="94a10-169">Másolás a **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="94a10-169">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_mimecastpersonalportal_configure.png) 

7. <span data-ttu-id="94a10-171">Egy másik webes böngészőablakban jelentkezzen be a Mimecast személyes portálra rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="94a10-171">In a different web browser window, log into your Mimecast Personal Portal as an administrator.</span></span>

8. <span data-ttu-id="94a10-172">Ugrás a **szolgáltatások \> alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="94a10-172">Go to **Services \> Applications**.</span></span>
   
    <span data-ttu-id="94a10-173">![Alkalmazások](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic794998.png "alkalmazások")</span><span class="sxs-lookup"><span data-stu-id="94a10-173">![Applications](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic794998.png "Applications")</span></span>

9. <span data-ttu-id="94a10-174">Kattintson a **hitelesítési profilok**.</span><span class="sxs-lookup"><span data-stu-id="94a10-174">Click **Authentication Profiles**.</span></span>
   
    <span data-ttu-id="94a10-175">![Hitelesítési profilok](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic794999.png "hitelesítési profilok")</span><span class="sxs-lookup"><span data-stu-id="94a10-175">![Authentication Profiles](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic794999.png "Authentication Profiles")</span></span>

10. <span data-ttu-id="94a10-176">Kattintson a **új hitelesítési profil**.</span><span class="sxs-lookup"><span data-stu-id="94a10-176">Click **New Authentication Profile**.</span></span>
   
    <span data-ttu-id="94a10-177">![Új hitelesítési profil](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic795000.png "új hitelesítési profil")</span><span class="sxs-lookup"><span data-stu-id="94a10-177">![New Authentication Profile](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic795000.png "New Authentication Profile")</span></span>

11. <span data-ttu-id="94a10-178">Az a **hitelesítési profil** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="94a10-178">In the **Authentication Profile** section, perform the following steps:</span></span>
   
    <span data-ttu-id="94a10-179">![Hitelesítési profil](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic795001.png "hitelesítési profil")</span><span class="sxs-lookup"><span data-stu-id="94a10-179">![Authentication Profile](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic795001.png "Authentication Profile")</span></span>
   
    <span data-ttu-id="94a10-180">a.</span><span class="sxs-lookup"><span data-stu-id="94a10-180">a.</span></span> <span data-ttu-id="94a10-181">Az a **leírás** szövegmező, adja meg a konfiguráció nevét.</span><span class="sxs-lookup"><span data-stu-id="94a10-181">In the **Description** textbox, type a name for your configuration.</span></span>
   
    <span data-ttu-id="94a10-182">b.</span><span class="sxs-lookup"><span data-stu-id="94a10-182">b.</span></span> <span data-ttu-id="94a10-183">Válassza ki **Mimecast személyes portál SAML-alapú hitelesítés kényszerítéséhez**.</span><span class="sxs-lookup"><span data-stu-id="94a10-183">Select **Enforce SAML Authentication for Mimecast Personal Portal**.</span></span>
   
    <span data-ttu-id="94a10-184">c.</span><span class="sxs-lookup"><span data-stu-id="94a10-184">c.</span></span> <span data-ttu-id="94a10-185">Mint **szolgáltató**, jelölje be **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="94a10-185">As **Provider**, select **Azure Active Directory**.</span></span>
   
    <span data-ttu-id="94a10-186">d.</span><span class="sxs-lookup"><span data-stu-id="94a10-186">d.</span></span> <span data-ttu-id="94a10-187">A **kiállítójának URL-címe** szövegmezőhöz illessze be az értékét **SAML Entitásazonosító** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="94a10-187">In **Issuer URL** textbox, paste the value of **SAML Entity ID** which you have copied from Azure portal.</span></span>
   
    <span data-ttu-id="94a10-188">e.</span><span class="sxs-lookup"><span data-stu-id="94a10-188">e.</span></span> <span data-ttu-id="94a10-189">A **bejelentkezési URL-cím** szövegmezőhöz illessze be az értékét **SAML-alapú egyszeri bejelentkezési URL-címe** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="94a10-189">In **Login URL** textbox, paste the value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>
   
    <span data-ttu-id="94a10-190">f.</span><span class="sxs-lookup"><span data-stu-id="94a10-190">f.</span></span> <span data-ttu-id="94a10-191">A **kijelentkezési URL-cím** szövegmezőhöz illessze be az értékét **Sign-Out URL-cím** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="94a10-191">In **Logout URL** textbox, paste the value of **Sign-Out URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="94a10-192">g.</span><span class="sxs-lookup"><span data-stu-id="94a10-192">g.</span></span> <span data-ttu-id="94a10-193">Nyissa meg a **base-64** kódolt tanúsítvány a Jegyzettömbben az Azure portálról letöltött, annak tartalmának másolása a vágólapra és illessze be azt a **Identitástanúsítvány szolgáltató (Metadata)** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="94a10-193">Open your **base-64** encoded certificate in notepad downloaded from Azure portal, copy the content of it into your clipboard, and then paste it to the **Identity Provider Certificate (Metadata)** textbox.</span></span>

    <span data-ttu-id="94a10-194">h.</span><span class="sxs-lookup"><span data-stu-id="94a10-194">h.</span></span> <span data-ttu-id="94a10-195">Válassza ki **engedélyezése egyszeri bejelentkezéshez**.</span><span class="sxs-lookup"><span data-stu-id="94a10-195">Select **Allow Single Sign On**.</span></span>
   
    <span data-ttu-id="94a10-196">i.</span><span class="sxs-lookup"><span data-stu-id="94a10-196">i.</span></span> <span data-ttu-id="94a10-197">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="94a10-197">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="94a10-198">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="94a10-198">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="94a10-199">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="94a10-199">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="94a10-200">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="94a10-200">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="94a10-201">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="94a10-201">Creating an Azure AD test user</span></span>
<span data-ttu-id="94a10-202">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="94a10-202">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="94a10-204">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="94a10-204">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="94a10-205">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="94a10-205">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-mimecast-personal-portal-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="94a10-207">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="94a10-207">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-mimecast-personal-portal-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="94a10-209">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="94a10-209">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-mimecast-personal-portal-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="94a10-211">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="94a10-211">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-mimecast-personal-portal-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="94a10-213">a.</span><span class="sxs-lookup"><span data-stu-id="94a10-213">a.</span></span> <span data-ttu-id="94a10-214">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="94a10-214">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="94a10-215">b.</span><span class="sxs-lookup"><span data-stu-id="94a10-215">b.</span></span> <span data-ttu-id="94a10-216">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="94a10-216">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="94a10-217">c.</span><span class="sxs-lookup"><span data-stu-id="94a10-217">c.</span></span> <span data-ttu-id="94a10-218">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="94a10-218">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="94a10-219">d.</span><span class="sxs-lookup"><span data-stu-id="94a10-219">d.</span></span> <span data-ttu-id="94a10-220">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="94a10-220">Click **Create**.</span></span>
 
### <a name="creating-a-mimecast-personal-portal-test-user"></a><span data-ttu-id="94a10-221">Mimecast személyes Portal tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="94a10-221">Creating a Mimecast Personal Portal test user</span></span>

<span data-ttu-id="94a10-222">Ahhoz, hogy az Azure AD-felhasználók jelentkezzen be személyes Mimecast portálra, akkor ki kell építenie Mimecast személyes portált.</span><span class="sxs-lookup"><span data-stu-id="94a10-222">In order to enable Azure AD users to log into Mimecast Personal Portal, they must be provisioned into Mimecast Personal Portal.</span></span> <span data-ttu-id="94a10-223">Mimecast személyes Portal, ha egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="94a10-223">In the case of Mimecast Personal Portal, provisioning is a manual task.</span></span>

<span data-ttu-id="94a10-224">Regisztrálja a tartományi felhasználók létrehozása előtt kell.</span><span class="sxs-lookup"><span data-stu-id="94a10-224">You need to register a domain before you can create users.</span></span>

<span data-ttu-id="94a10-225">**Adja meg a felhasználók átadása, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="94a10-225">**To configure user provisioning, perform the following steps:**</span></span>

1. <span data-ttu-id="94a10-226">Jelentkezzen be a **Mimecast személyes Portal** rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="94a10-226">Sign on to your **Mimecast Personal Portal** as administrator.</span></span>

2. <span data-ttu-id="94a10-227">Ugrás a **könyvtárak \> belső**.</span><span class="sxs-lookup"><span data-stu-id="94a10-227">Go to **Directories \> Internal**.</span></span>
   
    <span data-ttu-id="94a10-228">![Könyvtárak](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic795003.png "könyvtárak")</span><span class="sxs-lookup"><span data-stu-id="94a10-228">![Directories](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic795003.png "Directories")</span></span>

3. <span data-ttu-id="94a10-229">Kattintson a **regisztrálni az új tartomány**.</span><span class="sxs-lookup"><span data-stu-id="94a10-229">Click **Register New Domain**.</span></span>
   
    <span data-ttu-id="94a10-230">![Új tartomány regisztrálása](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic795004.png "új tartomány regisztrálása")</span><span class="sxs-lookup"><span data-stu-id="94a10-230">![Register New Domain](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic795004.png "Register New Domain")</span></span>

4. <span data-ttu-id="94a10-231">Az új tartomány létrehozása után kattintson **új cím**.</span><span class="sxs-lookup"><span data-stu-id="94a10-231">After your new domain has been created, click **New Address**.</span></span>
   
    <span data-ttu-id="94a10-232">![Új cím](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic795005.png "új cím")</span><span class="sxs-lookup"><span data-stu-id="94a10-232">![New Address](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic795005.png "New Address")</span></span>

5. <span data-ttu-id="94a10-233">Új cím párbeszédpanelen a következő lépésekkel egy érvényes Azure AD fiók kiépítése:</span><span class="sxs-lookup"><span data-stu-id="94a10-233">In the new address dialog, perform the following steps of a valid Azure AD account you want to provision:</span></span>
   
    <span data-ttu-id="94a10-234">![Mentés](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic795006.png "mentése")</span><span class="sxs-lookup"><span data-stu-id="94a10-234">![Save](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic795006.png "Save")</span></span>
   
    <span data-ttu-id="94a10-235">a.</span><span class="sxs-lookup"><span data-stu-id="94a10-235">a.</span></span> <span data-ttu-id="94a10-236">Az a **E-mail cím** szövegmezőhöz típus **E-mail cím** , a felhasználó  **BrittaSimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="94a10-236">In the **Email Address** textbox, type **Email Address** of the user as **BrittaSimon@contoso.com**.</span></span>
    
    <span data-ttu-id="94a10-237">b.</span><span class="sxs-lookup"><span data-stu-id="94a10-237">b.</span></span> <span data-ttu-id="94a10-238">Az a **globális név** szövegmezőhöz típusa a **felhasználónév** , **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="94a10-238">In the **Global Name** textbox, type the **username** as **BrittaSimon**.</span></span>

    <span data-ttu-id="94a10-239">c.</span><span class="sxs-lookup"><span data-stu-id="94a10-239">c.</span></span> <span data-ttu-id="94a10-240">Az a **jelszó**, és **jelszó megerősítése** szövegmezőből, típusa a **jelszó** felhasználó.</span><span class="sxs-lookup"><span data-stu-id="94a10-240">In the **Password**, and **Confirm Password** textboxes, type the **Password** of the user.</span></span>
   
    <span data-ttu-id="94a10-241">b.</span><span class="sxs-lookup"><span data-stu-id="94a10-241">b.</span></span> <span data-ttu-id="94a10-242">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="94a10-242">Click **Save**.</span></span>

>[!NOTE]
><span data-ttu-id="94a10-243">Mimecast személyes portál felhasználói fiók létrehozása eszközök vagy Mimecast személyes portál által szolgáltatott API-k segítségével kiépíteni az Azure AD felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="94a10-243">You can use any other Mimecast Personal Portal user account creation tools or APIs provided by Mimecast Personal Portal to provision Azure AD user accounts.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="94a10-244">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="94a10-244">Assigning the Azure AD test user</span></span>

<span data-ttu-id="94a10-245">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Mimecast személyes portál Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="94a10-245">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Mimecast Personal Portal.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="94a10-247">**Britta Simon hozzárendelése Mimecast személyes Portal, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="94a10-247">**To assign Britta Simon to Mimecast Personal Portal, perform the following steps:**</span></span>

1. <span data-ttu-id="94a10-248">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="94a10-248">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="94a10-250">Az alkalmazások listában válassza ki a **Mimecast személyes Portal**.</span><span class="sxs-lookup"><span data-stu-id="94a10-250">In the applications list, select **Mimecast Personal Portal**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_mimecastpersonalportal_app.png) 

3. <span data-ttu-id="94a10-252">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="94a10-252">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="94a10-254">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="94a10-254">Click **Add** button.</span></span> <span data-ttu-id="94a10-255">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="94a10-255">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="94a10-257">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="94a10-257">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="94a10-258">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="94a10-258">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="94a10-259">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="94a10-259">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="94a10-260">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="94a10-260">Testing single sign-on</span></span>
<span data-ttu-id="94a10-261">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="94a10-261">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="94a10-262">Ha a hozzáférési panelen Mimecast személyes Portal csempére kattint, akkor kell beolvasása automatikusan bejelentkezett a Mimecast személyes portál alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="94a10-262">When you click the Mimecast Personal Portal tile in the Access Panel, you should get automatically signed-on to your Mimecast Personal Portal application.</span></span> <span data-ttu-id="94a10-263">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="94a10-263">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="94a10-264">További források</span><span class="sxs-lookup"><span data-stu-id="94a10-264">Additional resources</span></span>

* [<span data-ttu-id="94a10-265">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="94a10-265">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="94a10-266">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="94a10-266">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_203.png

