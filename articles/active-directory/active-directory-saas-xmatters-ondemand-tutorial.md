---
title: "Oktatóanyag: Azure Active Directoryval integrált xMatters OnDemand |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és xMatters OnDemand között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: ca0633db-4f95-432e-b3db-0168193b5ce9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 9bfcb44ed19f167872b3cd9119e2dbdd35c82604
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-xmatters-ondemand"></a><span data-ttu-id="fe600-103">Oktatóanyag: Azure Active Directoryval integrált xMatters kötegmérete</span><span class="sxs-lookup"><span data-stu-id="fe600-103">Tutorial: Azure Active Directory integration with xMatters OnDemand</span></span>

<span data-ttu-id="fe600-104">Ebben az oktatóanyagban elsajátíthatja xMatters OnDemand integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="fe600-104">In this tutorial, you learn how to integrate xMatters OnDemand with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="fe600-105">XMatters OnDemand integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="fe600-105">Integrating xMatters OnDemand with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="fe600-106">Megadhatja a xMatters OnDemand hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="fe600-106">You can control in Azure AD who has access to xMatters OnDemand</span></span>
- <span data-ttu-id="fe600-107">Engedélyezheti a felhasználóknak, hogy automatikusan lekérni aláírt a xMatters OnDemand (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="fe600-107">You can enable your users to automatically get signed-on to xMatters OnDemand (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="fe600-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="fe600-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="fe600-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="fe600-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fe600-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="fe600-110">Prerequisites</span></span>

<span data-ttu-id="fe600-111">Konfigurálása az Azure AD-integrációs xMatters OnDemand, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="fe600-111">To configure Azure AD integration with xMatters OnDemand, you need the following items:</span></span>

- <span data-ttu-id="fe600-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="fe600-112">An Azure AD subscription</span></span>
- <span data-ttu-id="fe600-113">Egy OnDemand egyszeri bejelentkezés xMatters előfizetés engedélyezve</span><span class="sxs-lookup"><span data-stu-id="fe600-113">A xMatters OnDemand single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="fe600-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="fe600-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="fe600-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="fe600-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="fe600-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="fe600-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="fe600-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="fe600-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="fe600-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="fe600-118">Scenario description</span></span>
<span data-ttu-id="fe600-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="fe600-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="fe600-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="fe600-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="fe600-121">XMatters OnDemand hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="fe600-121">Adding xMatters OnDemand from the gallery</span></span>
2. <span data-ttu-id="fe600-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="fe600-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-xmatters-ondemand-from-the-gallery"></a><span data-ttu-id="fe600-123">XMatters OnDemand hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="fe600-123">Adding xMatters OnDemand from the gallery</span></span>
<span data-ttu-id="fe600-124">Az Azure AD integrálása a xMatters OnDemand konfigurálásához kell hozzáadnia xMatters OnDemand a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="fe600-124">To configure the integration of xMatters OnDemand into Azure AD, you need to add xMatters OnDemand from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="fe600-125">**A gyűjteményből xMatters OnDemand hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="fe600-125">**To add xMatters OnDemand from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="fe600-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="fe600-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="fe600-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="fe600-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="fe600-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="fe600-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="fe600-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="fe600-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="fe600-133">Írja be a keresőmezőbe, **xMatters OnDemand**.</span><span class="sxs-lookup"><span data-stu-id="fe600-133">In the search box, type **xMatters OnDemand**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_xmattersondemand_search.png)

5. <span data-ttu-id="fe600-135">Az eredmények panelen válassza ki a **xMatters OnDemand**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="fe600-135">In the results panel, select **xMatters OnDemand**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_xmattersondemand_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="fe600-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="fe600-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="fe600-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést a xMatters OnDemand "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="fe600-138">In this section, you configure and test Azure AD single sign-on with xMatters OnDemand based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="fe600-139">Az egyszeri bejelentkezés működéséhez az Azure AD az Azure AD-ben a felhasználó van Ondemanddetection xMatters a párjukhoz felhasználó tudnia kell.</span><span class="sxs-lookup"><span data-stu-id="fe600-139">For single sign-on to work, Azure AD needs to know what the counterpart user in xMatters OnDemand is to a user in Azure AD.</span></span> <span data-ttu-id="fe600-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a xMatters hivatkozás kapcsolatának OnDemand kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="fe600-140">In other words, a link relationship between an Azure AD user and the related user in xMatters OnDemand needs to be established.</span></span>

<span data-ttu-id="fe600-141">XMatters OnDemand, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="fe600-141">In xMatters OnDemand, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="fe600-142">Az Azure AD egyszeri bejelentkezést az xMatters OnDemand tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="fe600-142">To configure and test Azure AD single sign-on with xMatters OnDemand, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="fe600-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="fe600-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="fe600-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="fe600-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="fe600-145">**[XMatters OnDemand tesztfelhasználó létrehozása](#creating-a-xmatters-ondemand-test-user)**  - kell rendelkeznie egy Britta Simon megfelelője a xMatters OnDemand, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="fe600-145">**[Creating a xMatters OnDemand test user](#creating-a-xmatters-ondemand-test-user)** - to have a counterpart of Britta Simon in xMatters OnDemand that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="fe600-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="fe600-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="fe600-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="fe600-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="fe600-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="fe600-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="fe600-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és a xMatters OnDemand-alkalmazás az egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="fe600-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your xMatters OnDemand application.</span></span>

<span data-ttu-id="fe600-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés xMatters OnDemand, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="fe600-150">**To configure Azure AD single sign-on with xMatters OnDemand, perform the following steps:**</span></span>

1. <span data-ttu-id="fe600-151">Az Azure portálon a a **xMatters OnDemand** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="fe600-151">In the Azure portal, on the **xMatters OnDemand** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="fe600-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="fe600-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_xmattersondemand_samlbase.png)

3. <span data-ttu-id="fe600-155">Az a **xMatters OnDemand-tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="fe600-155">On the **xMatters OnDemand Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_xmattersondemand_url.png)
    
    <span data-ttu-id="fe600-157">a.</span><span class="sxs-lookup"><span data-stu-id="fe600-157">a.</span></span> <span data-ttu-id="fe600-158">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:</span><span class="sxs-lookup"><span data-stu-id="fe600-158">In the **Identifier** textbox, type a URL using the following pattern:</span></span>   
    | |
    |--|
    | `https://<companyname>.au1.xmatters.com.au/`|
    | `https://<companyname>.cs1.xmatters.com/`|
    | `https://<companyname>.xmatters.com/`|
    | `https://www.xmatters.com`|
    | `https://<companyname>.xmatters.com.au/`|

    <span data-ttu-id="fe600-159">b.</span><span class="sxs-lookup"><span data-stu-id="fe600-159">b.</span></span> <span data-ttu-id="fe600-160">Az a **válasz URL-CÍMEN** szövegmező, adja meg a következő minta használatával URL-címe:</span><span class="sxs-lookup"><span data-stu-id="fe600-160">In the **Reply URL** textbox, type a URL using the following pattern:</span></span>
    | |
    |--|
    | `https://<companyname>.au1.xmatters.com.au`|
    | `https://<companyname>.xmatters.com/sp/<instancename>`|
    | `https://<companyname>.cs1.xmatters.com/sp/<instancename>`|
    | `https://<companyname>.au1.xmatters.com.au/<instancename>`|

    > [!NOTE] 
    > <span data-ttu-id="fe600-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="fe600-161">These values are not real.</span></span> <span data-ttu-id="fe600-162">Frissítheti ezeket az értékeket a tényleges azonosítója és a válasz URL-CÍMEN.</span><span class="sxs-lookup"><span data-stu-id="fe600-162">Update these values with the actual Identifier and Reply URL.</span></span> <span data-ttu-id="fe600-163">Ügyfél [xMatters OnDemand támogatási csoport](https://www.xmatters.com/company/contact-us/) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="fe600-163">Contact [xMatters OnDemand support team](https://www.xmatters.com/company/contact-us/) to get these values.</span></span>

4. <span data-ttu-id="fe600-164">Az a **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** tanúsítvány helyileg a fájlt, majd mentse **c:\\XMatters OnDemand.cer**.</span><span class="sxs-lookup"><span data-stu-id="fe600-164">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file locally as **c:\\XMatters OnDemand.cer**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_xmattersondemand_certificate.png)
    
    > [!IMPORTANT]
    > <span data-ttu-id="fe600-166">A tanúsítvány továbbítani kell a [xMatters OnDemand támogatási csoport](https://www.xmatters.com/company/contact-us/).</span><span class="sxs-lookup"><span data-stu-id="fe600-166">You need to forward the certificate to the [xMatters OnDemand support team](https://www.xmatters.com/company/contact-us/).</span></span> <span data-ttu-id="fe600-167">Az egyszeri bejelentkezés konfigurációs is véglegesítése előtt a xMatters támogatási csoport által feltöltött kell a tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="fe600-167">The certificate needs to be uploaded by the xMatters support team before you can finalize the single sign-on configuration.</span></span> 

5. <span data-ttu-id="fe600-168">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="fe600-168">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="fe600-170">A a **xMatters OnDemand konfigurációs** kattintson **xMatters OnDemand konfigurálása** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="fe600-170">On the **xMatters OnDemand Configuration** section, click **Configure xMatters OnDemand** to open **Configure sign-on** window.</span></span> <span data-ttu-id="fe600-171">Másolás a **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="fe600-171">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_xmattersondemand_configure.png) 

7. <span data-ttu-id="fe600-173">Egy másik webes böngészőablakban jelentkezzen be a XMatters OnDemand vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="fe600-173">In a different web browser window, log in to your XMatters OnDemand company site as an administrator.</span></span>

8. <span data-ttu-id="fe600-174">A felső eszköztáron kattintson **Admin**, és kattintson a **vállalat adatait** a bal oldali navigációs sávon.</span><span class="sxs-lookup"><span data-stu-id="fe600-174">In the toolbar on the top, click **Admin**, and then click **Company Details** in the navigation bar on the left side.</span></span>
   
    <span data-ttu-id="fe600-175">![Felügyeleti](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776795.png "rendszergazda")</span><span class="sxs-lookup"><span data-stu-id="fe600-175">![Admin](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776795.png "Admin")</span></span>

9. <span data-ttu-id="fe600-176">Az a **SAML-alapú konfigurációs** lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="fe600-176">On the **SAML Configuration** page, perform the following steps:</span></span>
   
    <span data-ttu-id="fe600-177">![SAML-alapú konfigurációs](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776796.png "SAML-konfigurációja")</span><span class="sxs-lookup"><span data-stu-id="fe600-177">![SAML configuration](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776796.png "SAML configuration")</span></span>
   
    <span data-ttu-id="fe600-178">a.</span><span class="sxs-lookup"><span data-stu-id="fe600-178">a.</span></span> <span data-ttu-id="fe600-179">Válassza ki **SAML engedélyezése**.</span><span class="sxs-lookup"><span data-stu-id="fe600-179">Select **Enable SAML**.</span></span>
   
    <span data-ttu-id="fe600-180">b.</span><span class="sxs-lookup"><span data-stu-id="fe600-180">b.</span></span> <span data-ttu-id="fe600-181">Beillesztés **SAML Entitásazonosító**, amely az Azure-portálról másolta a **identitás Szolgáltatóazonosító** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="fe600-181">Paste **SAML Entity ID**, which you have copied from the Azure portal into the **Identity Provider ID** textbox.</span></span>
   
    <span data-ttu-id="fe600-182">c.</span><span class="sxs-lookup"><span data-stu-id="fe600-182">c.</span></span> <span data-ttu-id="fe600-183">Beillesztés **SAML-alapú egyszeri bejelentkezési URL-címe**, amely az Azure-portálról másolta a **egyszeri bejelentkezési URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="fe600-183">Paste **SAML Single Sign-On Service URL**, which you have copied from the Azure portal into the **Single Sign On URL** textbox.</span></span>
   
    <span data-ttu-id="fe600-184">d.</span><span class="sxs-lookup"><span data-stu-id="fe600-184">d.</span></span> <span data-ttu-id="fe600-185">Beillesztés **Sign-Out URL-cím**, amely az Azure-portálról másolta a **egyetlen kijelentkezési URL-címet** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="fe600-185">Paste **Sign-Out URL**, which you have copied from the Azure portal into the **Single Logout URL** textbox.</span></span>
   
    <span data-ttu-id="fe600-186">e.</span><span class="sxs-lookup"><span data-stu-id="fe600-186">e.</span></span> <span data-ttu-id="fe600-187">Az oldalon a vállalat adatait a lap tetején kattintson **módosítások mentése**.</span><span class="sxs-lookup"><span data-stu-id="fe600-187">On the Company Details page, at the top, click **Save Changes**.</span></span>
    
    <span data-ttu-id="fe600-188">![A vállalati részletek](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776797.png "vállalati részletei")</span><span class="sxs-lookup"><span data-stu-id="fe600-188">![Company details](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776797.png "Company details")</span></span>

> [!TIP]
> <span data-ttu-id="fe600-189">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="fe600-189">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="fe600-190">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="fe600-190">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="fe600-191">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="fe600-191">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="fe600-192">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="fe600-192">Creating an Azure AD test user</span></span>
<span data-ttu-id="fe600-193">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="fe600-193">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="fe600-195">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="fe600-195">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="fe600-196">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="fe600-196">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-xmatters-ondemand-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="fe600-198">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="fe600-198">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-xmatters-ondemand-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="fe600-200">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="fe600-200">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-xmatters-ondemand-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="fe600-202">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="fe600-202">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-xmatters-ondemand-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="fe600-204">a.</span><span class="sxs-lookup"><span data-stu-id="fe600-204">a.</span></span> <span data-ttu-id="fe600-205">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="fe600-205">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="fe600-206">b.</span><span class="sxs-lookup"><span data-stu-id="fe600-206">b.</span></span> <span data-ttu-id="fe600-207">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="fe600-207">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="fe600-208">c.</span><span class="sxs-lookup"><span data-stu-id="fe600-208">c.</span></span> <span data-ttu-id="fe600-209">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="fe600-209">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="fe600-210">d.</span><span class="sxs-lookup"><span data-stu-id="fe600-210">d.</span></span> <span data-ttu-id="fe600-211">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="fe600-211">Click **Create**.</span></span>
 
### <a name="creating-a-xmatters-ondemand-test-user"></a><span data-ttu-id="fe600-212">XMatters OnDemand tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="fe600-212">Creating a xMatters OnDemand test user</span></span>

<span data-ttu-id="fe600-213">Ahhoz, hogy az Azure AD-felhasználók XMatters OnDemand bejelentkezni, akkor ki kell építenie XMatters OnDemand be.</span><span class="sxs-lookup"><span data-stu-id="fe600-213">In order to enable Azure AD users to log in to XMatters OnDemand, they must be provisioned into XMatters OnDemand.</span></span> <span data-ttu-id="fe600-214">XMatters Ondemanddetection, ha egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="fe600-214">In the case of XMatters OnDemand, provisioning is a manual task.</span></span>

### <a name="to-provision-a-user-accounts-perform-the-following-steps"></a><span data-ttu-id="fe600-215">A felhasználói fiókok létrehozásához hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="fe600-215">To provision a user accounts, perform the following steps:</span></span>
1. <span data-ttu-id="fe600-216">Jelentkezzen be a **XMatters OnDemand** bérlő.</span><span class="sxs-lookup"><span data-stu-id="fe600-216">Log in to your **XMatters OnDemand** tenant.</span></span>

2.  <span data-ttu-id="fe600-217">Kattintson a **felhasználók** fülre.</span><span class="sxs-lookup"><span data-stu-id="fe600-217">Click **Users** tab.</span></span> <span data-ttu-id="fe600-218">majd **felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="fe600-218">and then click **Add User**.</span></span>
  
    <span data-ttu-id="fe600-219">![Felhasználók](./media/active-directory-saas-xmatters-ondemand-tutorial/IC781048.png "felhasználók")</span><span class="sxs-lookup"><span data-stu-id="fe600-219">![Users](./media/active-directory-saas-xmatters-ondemand-tutorial/IC781048.png "Users")</span></span>

3. <span data-ttu-id="fe600-220">Az a **hozzáadni egy felhasználót** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="fe600-220">In the **Add a User** section, perform the following steps:</span></span>
   
    <span data-ttu-id="fe600-221">![Felhasználó hozzáadása](./media/active-directory-saas-xmatters-ondemand-tutorial/IC781049.png "felhasználó hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="fe600-221">![Add a User](./media/active-directory-saas-xmatters-ondemand-tutorial/IC781049.png "Add a User")</span></span>

    <span data-ttu-id="fe600-222">a.</span><span class="sxs-lookup"><span data-stu-id="fe600-222">a.</span></span> <span data-ttu-id="fe600-223">Válassza ki **aktív**.</span><span class="sxs-lookup"><span data-stu-id="fe600-223">Select **Active**.</span></span>

    <span data-ttu-id="fe600-224">b.</span><span class="sxs-lookup"><span data-stu-id="fe600-224">b.</span></span> <span data-ttu-id="fe600-225">Az a **Felhasználóazonosító** szövegmező, a felhasználó a felhasználói azonosító típusát, például Brittasimon@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="fe600-225">In the **User ID** textbox, type the user id of user like Brittasimon@contoso.com.</span></span>
   
    <span data-ttu-id="fe600-226">c.</span><span class="sxs-lookup"><span data-stu-id="fe600-226">c.</span></span> <span data-ttu-id="fe600-227">Az a **Keresztnév** szövegmezőhöz Britta például a felhasználó első nevét.</span><span class="sxs-lookup"><span data-stu-id="fe600-227">In the **First Name** textbox, type first name of the user like Britta.</span></span>

    <span data-ttu-id="fe600-228">d.</span><span class="sxs-lookup"><span data-stu-id="fe600-228">d.</span></span> <span data-ttu-id="fe600-229">Az a **Vezetéknév** szövegmezőhöz típus Simon például a felhasználó vezetékneve.</span><span class="sxs-lookup"><span data-stu-id="fe600-229">In the **Last Name** textbox, type last name of the user like Simon.</span></span>
    
    <span data-ttu-id="fe600-230">e.</span><span class="sxs-lookup"><span data-stu-id="fe600-230">e.</span></span> <span data-ttu-id="fe600-231">Az a **hely** szövegmező, adjon meg egy érvényes Azure érvényes hely rendelkezés kívánt AD-fiókot.</span><span class="sxs-lookup"><span data-stu-id="fe600-231">In the **Site** textbox, Enter the valid site of a valid Azure AD account you want to provision.</span></span>
    
    <span data-ttu-id="fe600-232">f.</span><span class="sxs-lookup"><span data-stu-id="fe600-232">f.</span></span> <span data-ttu-id="fe600-233">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="fe600-233">Click **Save**.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="fe600-234">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="fe600-234">Assigning the Azure AD test user</span></span>

<span data-ttu-id="fe600-235">Ebben a szakaszban engedélyezze Britta Simon Azure egyszeri bejelentkezéshez használandó xMatters OnDemand való hozzáférés biztosítása.</span><span class="sxs-lookup"><span data-stu-id="fe600-235">In this section, you enable Britta Simon to use Azure single sign-on by granting access to xMatters OnDemand.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="fe600-237">**Britta Simon hozzárendelése xMatters OnDemand, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="fe600-237">**To assign Britta Simon to xMatters OnDemand, perform the following steps:**</span></span>

1. <span data-ttu-id="fe600-238">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="fe600-238">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="fe600-240">Az alkalmazások listában válassza ki a **xMatters OnDemand**.</span><span class="sxs-lookup"><span data-stu-id="fe600-240">In the applications list, select **xMatters OnDemand**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_xmattersondemand_app.png) 

3. <span data-ttu-id="fe600-242">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="fe600-242">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="fe600-244">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="fe600-244">Click **Add** button.</span></span> <span data-ttu-id="fe600-245">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="fe600-245">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="fe600-247">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="fe600-247">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="fe600-248">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="fe600-248">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="fe600-249">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="fe600-249">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="fe600-250">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="fe600-250">Testing single sign-on</span></span>

<span data-ttu-id="fe600-251">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="fe600-251">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="fe600-252">Kattintva a xMatters OnDemand csempe a hozzáférési panelen, meg kell beolvasni automatikusan bejelentkezett a xMatters OnDemand alkalmazás számára.</span><span class="sxs-lookup"><span data-stu-id="fe600-252">When you click the xMatters OnDemand tile in the Access Panel, you should get automatically signed-on to your xMatters OnDemand application.</span></span>
<span data-ttu-id="fe600-253">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="fe600-253">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="fe600-254">További források</span><span class="sxs-lookup"><span data-stu-id="fe600-254">Additional resources</span></span>

* [<span data-ttu-id="fe600-255">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="fe600-255">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="fe600-256">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="fe600-256">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_203.png

