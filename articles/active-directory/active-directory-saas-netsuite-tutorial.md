---
title: "Oktatóanyag: Azure Active Directoryval integrált Netsuite |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Netsuite között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: dafa0864-aef2-4f5e-9eac-770504688ef4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 4a19ab310212b93a53495a6fc6c25c77dfb82e79
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-netsuite"></a><span data-ttu-id="ab3a9-103">Oktatóanyag: Azure Active Directoryval integrált Netsuite</span><span class="sxs-lookup"><span data-stu-id="ab3a9-103">Tutorial: Azure Active Directory integration with Netsuite</span></span>

<span data-ttu-id="ab3a9-104">Ebben az oktatóanyagban elsajátíthatja Netsuite integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="ab3a9-104">In this tutorial, you learn how to integrate Netsuite with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="ab3a9-105">Netsuite integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="ab3a9-105">Integrating Netsuite with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="ab3a9-106">Megadhatja a Netsuite hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="ab3a9-106">You can control in Azure AD who has access to Netsuite</span></span>
- <span data-ttu-id="ab3a9-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett Netsuite (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="ab3a9-107">You can enable your users to automatically get signed-on to Netsuite (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="ab3a9-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="ab3a9-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="ab3a9-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="ab3a9-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ab3a9-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="ab3a9-110">Prerequisites</span></span>

<span data-ttu-id="ab3a9-111">Konfigurálása az Azure AD-integrációs Netsuite, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="ab3a9-111">To configure Azure AD integration with Netsuite, you need the following items:</span></span>

- <span data-ttu-id="ab3a9-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="ab3a9-112">An Azure AD subscription</span></span>
- <span data-ttu-id="ab3a9-113">Egy Netsuite egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="ab3a9-113">A Netsuite single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="ab3a9-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="ab3a9-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="ab3a9-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="ab3a9-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="ab3a9-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="ab3a9-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="ab3a9-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="ab3a9-118">Scenario description</span></span>
<span data-ttu-id="ab3a9-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="ab3a9-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="ab3a9-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="ab3a9-121">A gyűjteményből Netsuite hozzáadása</span><span class="sxs-lookup"><span data-stu-id="ab3a9-121">Adding Netsuite from the gallery</span></span>
2. <span data-ttu-id="ab3a9-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="ab3a9-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-netsuite-from-the-gallery"></a><span data-ttu-id="ab3a9-123">A gyűjteményből Netsuite hozzáadása</span><span class="sxs-lookup"><span data-stu-id="ab3a9-123">Adding Netsuite from the gallery</span></span>
<span data-ttu-id="ab3a9-124">Az Azure AD integrálása a Netsuite konfigurálásához kell hozzáadnia Netsuite a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-124">To configure the integration of Netsuite into Azure AD, you need to add Netsuite from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="ab3a9-125">**A gyűjteményből Netsuite hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="ab3a9-125">**To add Netsuite from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="ab3a9-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="ab3a9-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="ab3a9-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="ab3a9-131">Kattintson a **új alkalmazás** gombra a párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-131">Click **New application** button on the top of the dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="ab3a9-133">Írja be a keresőmezőbe, **Netsuite**.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-133">In the search box, type **Netsuite**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-netsuite-tutorial/tutorial_netsuite_search.png)

5. <span data-ttu-id="ab3a9-135">Az eredmények panelen válassza ki a **Netsuite**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-135">In the results panel, select **Netsuite**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-netsuite-tutorial/tutorial_netsuite_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="ab3a9-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="ab3a9-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="ab3a9-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján Netsuite</span><span class="sxs-lookup"><span data-stu-id="ab3a9-138">In this section, you configure and test Azure AD single sign-on with Netsuite based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="ab3a9-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Netsuite a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Netsuite is to a user in Azure AD.</span></span> <span data-ttu-id="ab3a9-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Netsuite közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-140">In other words, a link relationship between an Azure AD user and the related user in Netsuite needs to be established.</span></span>

<span data-ttu-id="ab3a9-141">Ez a hivatkozás kapcsolat létesíti értéket rendeli az **felhasználónév** értékeként Azure AD-ben a **felhasználónév** Netsuite a.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Netsuite.</span></span>

<span data-ttu-id="ab3a9-142">Az Azure AD egyszeri bejelentkezést a Netsuite tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="ab3a9-142">To configure and test Azure AD single sign-on with Netsuite, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="ab3a9-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="ab3a9-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="ab3a9-145">**[Netsuite tesztfelhasználó létrehozása](#creating-a-netsuite-test-user)**  - való Britta Simon valami Netsuite, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-145">**[Creating a Netsuite test user](#creating-a-netsuite-test-user)** - to have a counterpart of Britta Simon in Netsuite that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="ab3a9-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="ab3a9-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="ab3a9-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="ab3a9-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="ab3a9-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Netsuite alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Netsuite application.</span></span>

<span data-ttu-id="ab3a9-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés Netsuite, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="ab3a9-150">**To configure Azure AD single sign-on with Netsuite, perform the following steps:**</span></span>

1. <span data-ttu-id="ab3a9-151">Az Azure portálon a a **Netsuite** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-151">In the Azure portal, on the **Netsuite** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="ab3a9-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-netsuite-tutorial/tutorial_netsuite_samlbase.png)

3. <span data-ttu-id="ab3a9-155">Az a **Netsuite tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="ab3a9-155">On the **Netsuite Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-netsuite-tutorial/tutorial_netsuite_url.png)

    <span data-ttu-id="ab3a9-157">Az a **válasz URL-CÍMEN** szövegmező, adja meg a következő minta használatával URL-cím: `https://<tenant-name>.netsuite.com/saml2/acs` `https://<tenant-name>.na1.netsuite.com/saml2/acs` `https://<tenant-name>.na2.netsuite.com/saml2/acs` `https://<tenant-name>.sandbox.netsuite.com/saml2/acs` `https://<tenant-name>.na1.sandbox.netsuite.com/saml2/acs``https://<tenant-name>.na2.sandbox.netsuite.com/saml2/acs`</span><span class="sxs-lookup"><span data-stu-id="ab3a9-157">In the **Reply URL** textbox, type a URL using the following pattern:   `https://<tenant-name>.netsuite.com/saml2/acs` `https://<tenant-name>.na1.netsuite.com/saml2/acs` `https://<tenant-name>.na2.netsuite.com/saml2/acs` `https://<tenant-name>.sandbox.netsuite.com/saml2/acs` `https://<tenant-name>.na1.sandbox.netsuite.com/saml2/acs` `https://<tenant-name>.na2.sandbox.netsuite.com/saml2/acs`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="ab3a9-158">Ez az érték nincs valós értékének.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-158">This value is not real value.</span></span> <span data-ttu-id="ab3a9-159">Frissítse az értéket a tényleges válasz URL-címet.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-159">Update the value with the actual Reply URL.</span></span> <span data-ttu-id="ab3a9-160">Ügyfél [Netsuite támogatási csoport](http://www.netsuite.com/portal/services/support.shtml) lekérni ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-160">Contact [Netsuite support team](http://www.netsuite.com/portal/services/support.shtml) to get this value.</span></span>
 
4. <span data-ttu-id="ab3a9-161">Az a **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse az XML-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-161">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the XML file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-netsuite-tutorial/tutorial_netsuite_certificate.png) 

5. <span data-ttu-id="ab3a9-163">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-163">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-netsuite-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="ab3a9-165">A a **Netsuite konfigurációs** kattintson **konfigurálása Netsuite** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-165">On the **Netsuite Configuration** section, click **Configure Netsuite** to open **Configure sign-on** window.</span></span> <span data-ttu-id="ab3a9-166">Másolás a **SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="ab3a9-166">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-netsuite-tutorial/tutorial_netsuite_configure.png) 

7. <span data-ttu-id="ab3a9-168">Új lap megnyitása a böngészőben, és jelentkezzen be rendszergazdaként egy vállalat Netsuite webhelyét.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-168">Open a new tab in your browser, and sign into your Netsuite company site as an administrator.</span></span>

8. <span data-ttu-id="ab3a9-169">A lap felső eszköztárán kattintson **telepítő**, majd kattintson a **Telepítéskezelő**.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-169">In the toolbar at the top of the page, click **Setup**, then click **Setup Manager**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-Netsuite-tutorial/ns-setup.png)

9. <span data-ttu-id="ab3a9-171">Az a **beállítási feladatok** listáról válassza ki **integrációs**.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-171">From the **Setup Tasks** list, select **Integration**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-Netsuite-tutorial/ns-integration.png)

10. <span data-ttu-id="ab3a9-173">Az a **kezelése hitelesítési** területén kattintson **SAML-alapú egyszeri bejelentkezést**.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-173">In the **Manage Authentication** section, click **SAML Single Sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-Netsuite-tutorial/ns-saml.png)

11. <span data-ttu-id="ab3a9-175">Az a **SAML-alapú telepítő** lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="ab3a9-175">On the **SAML Setup** page, perform the following steps:</span></span>
   
    <span data-ttu-id="ab3a9-176">a.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-176">a.</span></span> <span data-ttu-id="ab3a9-177">Másolás a **SAML-alapú egyszeri bejelentkezési URL-címe** értéket **rövid összefoglaló** szakasza **bejelentkezés konfigurálása** és illessze be azt a **szolgáltató bejelentkezési identitás Lap** Netsuite mezőbe.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-177">Copy the **SAML Single Sign-On Service URL** value from **Quick Reference** section of **Configure sign-on** and paste it into the **Identity Provider Login Page** field in Netsuite.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-netsuite-tutorial/ns-saml-setup.png)
  
    <span data-ttu-id="ab3a9-179">b.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-179">b.</span></span> <span data-ttu-id="ab3a9-180">Válassza ki a Netsuite, **elsődleges hitelesítési módszert**.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-180">In Netsuite, select **Primary Authentication Method**.</span></span>

    <span data-ttu-id="ab3a9-181">c.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-181">c.</span></span> <span data-ttu-id="ab3a9-182">A mező feliratú **SAMLV2 Identity Provider metaadatok**, jelölje be **IDP metaadatait tartalmazó fájl feltöltése**.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-182">For the field labeled **SAMLV2 Identity Provider Metadata**, select **Upload IDP Metadata File**.</span></span> <span data-ttu-id="ab3a9-183">Kattintson a **Tallózás** feltöltése az Azure-portálról letöltött metaadatait tartalmazó fájl.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-183">Then click **Browse** to upload the metadata file that you downloaded from Azure portal.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-netsuite-tutorial/ns-sso-setup.png)

    <span data-ttu-id="ab3a9-185">d.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-185">d.</span></span> <span data-ttu-id="ab3a9-186">Kattintson a **nyújt**.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-186">Click **Submit**.</span></span>

12. <span data-ttu-id="ab3a9-187">Az Azure ad-ben, kattintson a **nézet és egyéb felhasználói attribútumok szerkesztése** jelölőnégyzetet, és adja hozzá attribútumot.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-187">In Azure AD, Click on **View and edit all other user attributes** check-box and add attribute.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-Netsuite-tutorial/ns-attributes.png)

13. <span data-ttu-id="ab3a9-189">Az a **attribútumnév** mezőbe írja be a `account`.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-189">For the **Attribute Name** field, type in `account`.</span></span> <span data-ttu-id="ab3a9-190">Az a **attribútumérték** mezőbe írja be a Netsuite fiók azonosítójaként. Ezt az értéket az állandót és módosítási fiókkal.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-190">For the **Attribute Value** field, type in your Netsuite account ID.This value is constant and change with account.</span></span> <span data-ttu-id="ab3a9-191">A Fiókazonosító található útmutatást az alábbiakban találhatók:</span><span class="sxs-lookup"><span data-stu-id="ab3a9-191">Instructions on how to find your account ID are included below:</span></span>

      ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-Netsuite-tutorial/ns-add-attribute.png)

    <span data-ttu-id="ab3a9-193">a.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-193">a.</span></span> <span data-ttu-id="ab3a9-194">Netsuite, kattintson **telepítő** a felső navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-194">In Netsuite, click **Setup** from the top navigation menu.</span></span>

    <span data-ttu-id="ab3a9-195">b.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-195">b.</span></span> <span data-ttu-id="ab3a9-196">Csoportjában kattintson a **beállítási feladatok** a bal oldali navigációs menü szakasza a **integrációs** szakaszt, és kattintson a **Web Services beállítások**.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-196">Then click under the **Setup Tasks** section of the left navigation menu, select the **Integration** section, and click **Web Services Preferences**.</span></span>

    <span data-ttu-id="ab3a9-197">c.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-197">c.</span></span> <span data-ttu-id="ab3a9-198">A Netsuite Fiókazonosító másolja és illessze be azt a **attribútumérték** mezőjét az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-198">Copy your Netsuite Account ID and paste it into the **Attribute Value** field in Azure AD.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-Netsuite-tutorial/ns-account-id.png)

14. <span data-ttu-id="ab3a9-200">Mielőtt a felhasználók Netsuite történő egyszeri bejelentkezéshez hajthatnak végre, akkor először meg kell adni a megfelelő engedélyek szükségesek a Netsuite.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-200">Before users can perform single sign-on into Netsuite, they must first be assigned the appropriate permissions in Netsuite.</span></span> <span data-ttu-id="ab3a9-201">Kövesse az alábbi utasításokat hozzá ezeket az engedélyeket.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-201">Follow the instructions below to assign these permissions.</span></span>

    <span data-ttu-id="ab3a9-202">a.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-202">a.</span></span> <span data-ttu-id="ab3a9-203">Kattintson a felső navigációs menü **telepítő**, majd kattintson a **Telepítéskezelő**.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-203">On the top navigation menu, click **Setup**, then click **Setup Manager**.</span></span>
      
      ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-Netsuite-tutorial/ns-setup.png)

    <span data-ttu-id="ab3a9-205">b.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-205">b.</span></span> <span data-ttu-id="ab3a9-206">Válassza a bal oldali navigációs menü **felhasználók vagy szerepkörök**, majd kattintson a **szerepkörök kezelése**.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-206">On the left navigation menu, select **Users/Roles**, then click **Manage Roles**.</span></span>
      
      ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-Netsuite-tutorial/ns-manage-roles.png)

    <span data-ttu-id="ab3a9-208">c.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-208">c.</span></span> <span data-ttu-id="ab3a9-209">Kattintson a **új szerepkör**.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-209">Click **New Role**.</span></span>

    <span data-ttu-id="ab3a9-210">d.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-210">d.</span></span> <span data-ttu-id="ab3a9-211">Írja be a **neve** az új szerepkör, és válassza ki a **egyszeri bejelentkezés csak** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-211">Type in a **Name** for your new role, and select the **Single Sign-On Only** checkbox.</span></span>
      
      ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-Netsuite-tutorial/ns-new-role.png)

    <span data-ttu-id="ab3a9-213">e.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-213">e.</span></span> <span data-ttu-id="ab3a9-214">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-214">Click **Save**.</span></span>

    <span data-ttu-id="ab3a9-215">f.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-215">f.</span></span> <span data-ttu-id="ab3a9-216">Kattintson a felső menüben **engedélyek**.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-216">In the menu on the top, click **Permissions**.</span></span> <span data-ttu-id="ab3a9-217">Kattintson a **telepítő**.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-217">Then click **Setup**.</span></span>
      
       ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-Netsuite-tutorial/ns-sso.png)

    <span data-ttu-id="ab3a9-219">g.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-219">g.</span></span> <span data-ttu-id="ab3a9-220">Válassza ki **beállítva fel SAM egyszeri bejelentkezés**, és kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-220">Select **Set Up SAM Single Sign-on**, and then click **Add**.</span></span>

    <span data-ttu-id="ab3a9-221">h.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-221">h.</span></span> <span data-ttu-id="ab3a9-222">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-222">Click **Save**.</span></span>

    <span data-ttu-id="ab3a9-223">i.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-223">i.</span></span> <span data-ttu-id="ab3a9-224">Kattintson a felső navigációs menü **telepítő**, majd kattintson a **Telepítéskezelő**.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-224">On the top navigation menu, click **Setup**, then click **Setup Manager**.</span></span>
      
       ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-Netsuite-tutorial/ns-setup.png)

    <span data-ttu-id="ab3a9-226">j.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-226">j.</span></span> <span data-ttu-id="ab3a9-227">Válassza a bal oldali navigációs menü **felhasználók vagy szerepkörök**, majd kattintson a **felhasználók kezelése**.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-227">On the left navigation menu, select **Users/Roles**, then click **Manage Users**.</span></span>
      
       ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-Netsuite-tutorial/ns-manage-users.png)

    <span data-ttu-id="ab3a9-229">k.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-229">k.</span></span> <span data-ttu-id="ab3a9-230">Válassza ki a tesztfelhasználó számára.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-230">Select a test user.</span></span> <span data-ttu-id="ab3a9-231">Kattintson a **szerkesztése**.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-231">Then click **Edit**.</span></span>
      
       ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-Netsuite-tutorial/ns-edit-user.png)

    <span data-ttu-id="ab3a9-233">l.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-233">l.</span></span> <span data-ttu-id="ab3a9-234">Szerepkörök párbeszédpanelen válassza ki a szerepkör, amely hozott létre, és kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-234">On the Roles dialog, select the role that you have created and click **Add**.</span></span>
      
       ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-Netsuite-tutorial/ns-add-role.png)

    <span data-ttu-id="ab3a9-236">m.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-236">m.</span></span> <span data-ttu-id="ab3a9-237">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-237">Click **Save**.</span></span>
    
> [!TIP]
> <span data-ttu-id="ab3a9-238">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="ab3a9-238">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="ab3a9-239">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-239">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="ab3a9-240">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="ab3a9-240">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="ab3a9-241">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="ab3a9-241">Creating an Azure AD test user</span></span>
<span data-ttu-id="ab3a9-242">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-242">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="ab3a9-244">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="ab3a9-244">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="ab3a9-245">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-245">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-netsuite-tutorial/create_aaduser_01.png) 

2.  <span data-ttu-id="ab3a9-247">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-247">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-netsuite-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="ab3a9-249">Kattintson a párbeszédpanel tetején **Hozzáadás** megnyitásához a **felhasználói** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-249">At the top of the dialog, click **Add** to open the **User** dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-netsuite-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="ab3a9-251">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="ab3a9-251">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-netsuite-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="ab3a9-253">a.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-253">a.</span></span> <span data-ttu-id="ab3a9-254">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-254">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="ab3a9-255">b.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-255">b.</span></span> <span data-ttu-id="ab3a9-256">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-256">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="ab3a9-257">c.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-257">c.</span></span> <span data-ttu-id="ab3a9-258">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-258">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="ab3a9-259">d.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-259">d.</span></span> <span data-ttu-id="ab3a9-260">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-260">Click **Create**.</span></span> 

### <a name="creating-a-netsuite-test-user"></a><span data-ttu-id="ab3a9-261">Netsuite tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="ab3a9-261">Creating a Netsuite test user</span></span>

<span data-ttu-id="ab3a9-262">Ebben a szakaszban egy felhasználó Britta Simon nevű Netsuite jön létre.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-262">In this section, a user called Britta Simon is created in Netsuite.</span></span> <span data-ttu-id="ab3a9-263">Netsuite támogatja közvetlenül az időponthoz kötött kiosztást, amely alapértelmezés szerint engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-263">Netsuite supports just-in-time provisioning, which is enabled by default.</span></span>
<span data-ttu-id="ab3a9-264">Nincs ebben a szakaszban az Ön művelet elem.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-264">There is no action item for you in this section.</span></span> <span data-ttu-id="ab3a9-265">Ha a felhasználó nem létezik a Netsuite, egy új Netsuite elérésére tett kísérlet során jön létre.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-265">If a user doesn't already exist in Netsuite, a new one is created when you attempt to access Netsuite.</span></span>


### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="ab3a9-266">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="ab3a9-266">Assigning the Azure AD test user</span></span>

<span data-ttu-id="ab3a9-267">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Netsuite Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-267">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Netsuite.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="ab3a9-269">**Britta Simon hozzárendelése Netsuite, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="ab3a9-269">**To assign Britta Simon to Netsuite, perform the following steps:**</span></span>

1. <span data-ttu-id="ab3a9-270">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-270">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="ab3a9-272">Az alkalmazások listában válassza ki a **Netsuite**.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-272">In the applications list, select **Netsuite**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-netsuite-tutorial/tutorial_netsuite_app.png) 

3. <span data-ttu-id="ab3a9-274">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-274">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="ab3a9-276">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-276">Click **Add** button.</span></span> <span data-ttu-id="ab3a9-277">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-277">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="ab3a9-279">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-279">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="ab3a9-280">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-280">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="ab3a9-281">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-281">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="ab3a9-282">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="ab3a9-282">Testing single sign-on</span></span>

<span data-ttu-id="ab3a9-283">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-283">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="ab3a9-284">Az egyszeri bejelentkezés beállításainak ellenőrzéséhez nyissa meg a hozzáférési panelre a [https://myapps.microsoft.com](https://myapps.microsoft.com/), jelentkezzen be a fiókot, és kattintson a **Netsuite**.</span><span class="sxs-lookup"><span data-stu-id="ab3a9-284">To test your single sign-on settings, open the Access Panel at [https://myapps.microsoft.com](https://myapps.microsoft.com/), sign into the test account, and click **Netsuite**.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ab3a9-285">További források</span><span class="sxs-lookup"><span data-stu-id="ab3a9-285">Additional resources</span></span>

* [<span data-ttu-id="ab3a9-286">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="ab3a9-286">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="ab3a9-287">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="ab3a9-287">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="ab3a9-288">A felhasználók átadása konfigurálása</span><span class="sxs-lookup"><span data-stu-id="ab3a9-288">Configure User Provisioning</span></span>](active-directory-saas-netsuite-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_203.png

