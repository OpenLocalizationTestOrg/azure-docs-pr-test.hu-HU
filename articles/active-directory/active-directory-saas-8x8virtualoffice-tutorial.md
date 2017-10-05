---
title: "Oktatóanyag: Azure Active Directoryval integrált 8 x 8 virtuális Office |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés az Azure Active könyvtárhoz, és 8 x 8 virtuális Office között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: b34a6edf-e745-4aec-b0b2-7337473d64c5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: d8dcf0171b93fec15347e810a1b525bd815dbf04
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-8x8-virtual-office"></a><span data-ttu-id="81473-103">Oktatóanyag: Azure Active Directoryval integrált 8 x 8 virtuális Office</span><span class="sxs-lookup"><span data-stu-id="81473-103">Tutorial: Azure Active Directory integration with 8x8 Virtual Office</span></span>

<span data-ttu-id="81473-104">Ebben az oktatóanyagban elsajátíthatja 8 x 8 virtuális Office integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="81473-104">In this tutorial, you learn how to integrate 8x8 Virtual Office with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="81473-105">8 x 8 integrálása virtuális Office és az Azure AD segítségével a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="81473-105">Integrating 8x8 Virtual Office with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="81473-106">Megadhatja a 8 x 8 virtuális Office hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="81473-106">You can control in Azure AD who has access to 8x8 Virtual Office</span></span>
- <span data-ttu-id="81473-107">Az Azure AD-fiókok a engedélyezheti a felhasználóknak, hogy automatikusan lekérni bejelentkezett 8 x 8 virtuális Office (egyszeri bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="81473-107">You can enable your users to automatically get signed-on to 8x8 Virtual Office (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="81473-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="81473-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="81473-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="81473-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="81473-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="81473-110">Prerequisites</span></span>

<span data-ttu-id="81473-111">Az Azure AD-integráció konfigurálása 8 x 8 virtuális Office, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="81473-111">To configure Azure AD integration with 8x8 Virtual Office, you need the following items:</span></span>

- <span data-ttu-id="81473-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="81473-112">An Azure AD subscription</span></span>
- <span data-ttu-id="81473-113">Egy 8 x 8 virtuális Office egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="81473-113">A 8x8 Virtual Office single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="81473-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="81473-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="81473-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="81473-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="81473-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="81473-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="81473-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="81473-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="81473-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="81473-118">Scenario description</span></span>
<span data-ttu-id="81473-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="81473-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="81473-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="81473-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="81473-121">8 x 8 virtuális Office hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="81473-121">Adding 8x8 Virtual Office from the gallery</span></span>
2. <span data-ttu-id="81473-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="81473-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-8x8-virtual-office-from-the-gallery"></a><span data-ttu-id="81473-123">8 x 8 virtuális Office hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="81473-123">Adding 8x8 Virtual Office from the gallery</span></span>
<span data-ttu-id="81473-124">Az Azure AD integrálása a 8 x 8 virtuális Office konfigurálásához kell hozzáadnia 8 x 8 virtuális Office a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="81473-124">To configure the integration of 8x8 Virtual Office into Azure AD, you need to add 8x8 Virtual Office from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="81473-125">**A gyűjteményből 8 x 8 virtuális Office hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="81473-125">**To add 8x8 Virtual Office from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="81473-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="81473-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="81473-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="81473-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="81473-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="81473-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="81473-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="81473-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="81473-133">Írja be a keresőmezőbe, **8 x 8 virtuális Office**.</span><span class="sxs-lookup"><span data-stu-id="81473-133">In the search box, type **8x8 Virtual Office**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_search.png)

5. <span data-ttu-id="81473-135">Az eredmények panelen válassza ki a **8 x 8 virtuális Office**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="81473-135">In the results panel, select **8x8 Virtual Office**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="81473-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="81473-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="81473-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést a 8 x 8 virtuális Office "Britta Simon." nevű tesztfelhasználó alapján</span><span class="sxs-lookup"><span data-stu-id="81473-138">In this section, you configure and test Azure AD single sign-on with 8x8 Virtual Office based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="81473-139">Az egyszeri bejelentkezés működéséhez az Azure AD tudnia kell, a partner felhasználó 8 x 8 virtuális Office pedig egy felhasználó számára az Azure AD-ban.</span><span class="sxs-lookup"><span data-stu-id="81473-139">For single sign-on to work, Azure AD needs to know what the counterpart user in 8x8 Virtual Office is to a user in Azure AD.</span></span> <span data-ttu-id="81473-140">Más szóval egy Azure AD-felhasználó és a kapcsolódó felhasználó a 8 x 8 hivatkozás kapcsolatának virtuális Office kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="81473-140">In other words, a link relationship between an Azure AD user and the related user in 8x8 Virtual Office needs to be established.</span></span>

<span data-ttu-id="81473-141">A 8 x 8 virtuális Office, rendelje az értékét a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="81473-141">In 8x8 Virtual Office, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="81473-142">Az Azure AD az egyszeri bejelentkezés 8 x 8 virtuális Office tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="81473-142">To configure and test Azure AD single sign-on with 8x8 Virtual Office, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="81473-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="81473-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="81473-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="81473-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="81473-145">**[8 x 8 virtuális Office tesztfelhasználó létrehozása](#creating-a-8x8-virtual-office-test-user)**  - kell rendelkeznie a megfelelője a Britta Simon 8 x 8 virtuális irodában, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="81473-145">**[Creating a 8x8 Virtual Office test user](#creating-a-8x8-virtual-office-test-user)** - to have a counterpart of Britta Simon in 8x8 Virtual Office that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="81473-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="81473-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="81473-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="81473-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="81473-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="81473-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="81473-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és az 8 x 8 virtuális Office-alkalmazás az egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="81473-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your 8x8 Virtual Office application.</span></span>

<span data-ttu-id="81473-150">**8 x 8 virtuális Office konfigurálása az Azure AD egyszeri bejelentkezést, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="81473-150">**To configure Azure AD single sign-on with 8x8 Virtual Office, perform the following steps:**</span></span>

1. <span data-ttu-id="81473-151">Az Azure portálon a a **8 x 8 virtuális Office** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="81473-151">In the Azure portal, on the **8x8 Virtual Office** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="81473-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="81473-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_samlbase.png)

3. <span data-ttu-id="81473-155">Az a **8 x 8 virtuális Office tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="81473-155">On the **8x8 Virtual Office Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_url.png)

    <span data-ttu-id="81473-157">a.</span><span class="sxs-lookup"><span data-stu-id="81473-157">a.</span></span> <span data-ttu-id="81473-158">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:</span><span class="sxs-lookup"><span data-stu-id="81473-158">In the **Identifier** textbox, type a URL using the following pattern:</span></span>

    <span data-ttu-id="81473-159">| `https://sso.8x8.com/<companyname>` |
    | `https://www.8x8.com/<companyname>` |
    | `https://sso.8x8pilot.com/<companyname>` |</span><span class="sxs-lookup"><span data-stu-id="81473-159">| `https://sso.8x8.com/<companyname>` |
 | `https://www.8x8.com/<companyname>` |
 | `https://sso.8x8pilot.com/<companyname>` |</span></span>

    <span data-ttu-id="81473-160">b.</span><span class="sxs-lookup"><span data-stu-id="81473-160">b.</span></span> <span data-ttu-id="81473-161">Az a **válasz URL-CÍMEN** szövegmező, adja meg a következő minta használatával URL-címe:</span><span class="sxs-lookup"><span data-stu-id="81473-161">In the **Reply URL** textbox, type a URL using the following pattern:</span></span>

    <span data-ttu-id="81473-162">| `https://<subdomain>.8x8.com/saml2` |
    | `https://<subdomain>.8x8pilot.com/saml2`|</span><span class="sxs-lookup"><span data-stu-id="81473-162">| `https://<subdomain>.8x8.com/saml2` |
 | `https://<subdomain>.8x8pilot.com/saml2`|</span></span>

    > [!NOTE] 
    > <span data-ttu-id="81473-163">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="81473-163">These values are not real.</span></span> <span data-ttu-id="81473-164">Frissítheti ezeket az értékeket a tényleges azonosítója és a válasz URL-CÍMEN.</span><span class="sxs-lookup"><span data-stu-id="81473-164">Update these values with the actual Identifier and Reply URL.</span></span> <span data-ttu-id="81473-165">Ügyfél [8 x 8 virtuális Office támogatási csoport](https://www.8x8.com/about-us/contact-us) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="81473-165">Contact [8x8 Virtual Office support team](https://www.8x8.com/about-us/contact-us) to get these values.</span></span>
 


4. <span data-ttu-id="81473-166">A a **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Raw)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="81473-166">On the **SAML Signing Certificate** section, click **Certificate (Raw)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_certificate.png) 

5. <span data-ttu-id="81473-168">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="81473-168">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="81473-170">A a **8 x 8 virtuális Office konfigurációs** kattintson **konfigurálása 8 x 8 virtuális Office** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="81473-170">On the **8x8 Virtual Office Configuration** section, click **Configure 8x8 Virtual Office** to open **Configure sign-on** window.</span></span> <span data-ttu-id="81473-171">Másolás a **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="81473-171">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_configure.png) 

7. <span data-ttu-id="81473-173">Bejelentkezés a 8 x 8 virtuális Office-bérlő rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="81473-173">Sign-on to your 8x8 Virtual Office tenant as an administrator.</span></span>

8. <span data-ttu-id="81473-174">Válassza ki **virtuális Office fiók Mgr** alkalmazás panelen.</span><span class="sxs-lookup"><span data-stu-id="81473-174">Select **Virtual Office Account Mgr** on Application Panel.</span></span>
   
    ![Alkalmazás ügyféloldali konfigurálása](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_001.png)

9. <span data-ttu-id="81473-176">Válassza ki **üzleti** kezelésére, és kattintson a fiók **bejelentkezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="81473-176">Select **Business** account to manage and click **Sign In** button.</span></span>
   
    ![Alkalmazás ügyféloldali konfigurálása](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_002.png)

10. <span data-ttu-id="81473-178">Kattintson a **fiókok** a menülistában lapján.</span><span class="sxs-lookup"><span data-stu-id="81473-178">Click **Accounts** tab in the menu list.</span></span>
   
    ![Alkalmazás ügyféloldali konfigurálása](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_003.png)

11. <span data-ttu-id="81473-180">Kattintson a **egyszeri bejelentkezés** fiókok listájában.</span><span class="sxs-lookup"><span data-stu-id="81473-180">Click **Single Sign On** in the list of Accounts.</span></span>
   
    ![Alkalmazás ügyféloldali konfigurálása](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_004.png)

12. <span data-ttu-id="81473-182">Válassza ki **egyszeri bejelentkezés** hitelesítési módszert, és kattintson a **SAML**.</span><span class="sxs-lookup"><span data-stu-id="81473-182">Select **Single Sign On** under Authentication method and click **SAML**.</span></span>
    
    ![Alkalmazás ügyféloldali konfigurálása](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_005.png)

13. <span data-ttu-id="81473-184">Másolás **SAML SSO URL-cím**, **rang szolgáltatás URL-címe, az egyszeri** és **kiállítójának URL-címe** az Azure AD- **jelentkezzen be az URL-cím**, **jelentkezzen ki az URL-cím** és **kiállítójának URL-címe** 8 x 8 virtuális Office.</span><span class="sxs-lookup"><span data-stu-id="81473-184">Copy **SAML SSO URL**, **Single Sing Out Service URL** and **Issuer URL** from Azure AD to **Sign In URL**, **Sign Out URL** and **Issuer URL** in 8x8 Virtual Office.</span></span> 
    
    ![Alkalmazás ügyféloldali konfigurálása](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_006.png)
    
14. <span data-ttu-id="81473-186">Kattintson a **böngésző** töltse fel a tanúsítványt, amely letöltötte az Azure AD gombra, és kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="81473-186">Click **Browser** button to upload the certificate which you downloaded from Azure AD, and click the **Save** button.</span></span>

> [!TIP]
> <span data-ttu-id="81473-187">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="81473-187">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="81473-188">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="81473-188">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="81473-189">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="81473-189">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="81473-190">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="81473-190">Creating an Azure AD test user</span></span>
<span data-ttu-id="81473-191">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="81473-191">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="81473-193">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="81473-193">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="81473-194">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="81473-194">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="81473-196">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="81473-196">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="81473-198">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="81473-198">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="81473-200">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="81473-200">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="81473-202">a.</span><span class="sxs-lookup"><span data-stu-id="81473-202">a.</span></span> <span data-ttu-id="81473-203">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="81473-203">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="81473-204">b.</span><span class="sxs-lookup"><span data-stu-id="81473-204">b.</span></span> <span data-ttu-id="81473-205">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="81473-205">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="81473-206">c.</span><span class="sxs-lookup"><span data-stu-id="81473-206">c.</span></span> <span data-ttu-id="81473-207">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="81473-207">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="81473-208">d.</span><span class="sxs-lookup"><span data-stu-id="81473-208">d.</span></span> <span data-ttu-id="81473-209">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="81473-209">Click **Create**.</span></span>
 
### <a name="creating-a-8x8-virtual-office-test-user"></a><span data-ttu-id="81473-210">8 x 8 virtuális Office tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="81473-210">Creating a 8x8 Virtual Office test user</span></span>

<span data-ttu-id="81473-211">Ez a szakasz célja Britta Simon meghívta 8 x 8 virtuális Office felhasználó létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="81473-211">The objective of this section is to create a user called Britta Simon in 8x8 Virtual Office.</span></span> <span data-ttu-id="81473-212">8 x 8 virtuális Office támogatja just-in-time kiosztást, amely alapértelmezés szerint van engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="81473-212">8x8 Virtual Office supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="81473-213">Nincs ebben a szakaszban az Ön művelet elem.</span><span class="sxs-lookup"><span data-stu-id="81473-213">There is no action item for you in this section.</span></span> <span data-ttu-id="81473-214">Új felhasználó jön létre, ha még nem létezik 8 x 8 virtuális Office elérésére tett kísérlet során.</span><span class="sxs-lookup"><span data-stu-id="81473-214">A new user is created during an attempt to access 8x8 Virtual Office if it doesn't exist yet.</span></span> 

>[!NOTE]
><span data-ttu-id="81473-215">Hozza létre a felhasználó manuálisan kell, ha szeretné-e lépjen kapcsolatba a [8 x 8 virtuális Office támogatási csoport](https://www.8x8.com/about-us/contact-us).</span><span class="sxs-lookup"><span data-stu-id="81473-215">If you need to create a user manually, you need to contact the [8x8 Virtual Office support team](https://www.8x8.com/about-us/contact-us).</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="81473-216">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="81473-216">Assigning the Azure AD test user</span></span>

<span data-ttu-id="81473-217">Ebben a szakaszban Britta Simon használandó által biztosított hozzáférés 8 x 8 virtuális Office, az Azure egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="81473-217">In this section, you enable Britta Simon to use Azure single sign-on by granting access to 8x8 Virtual Office.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="81473-219">**8 x 8 virtuális Office Britta Simon rendel, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="81473-219">**To assign Britta Simon to 8x8 Virtual Office, perform the following steps:**</span></span>

1. <span data-ttu-id="81473-220">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="81473-220">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="81473-222">Az alkalmazások listában válassza ki a **8 x 8 virtuális Office**.</span><span class="sxs-lookup"><span data-stu-id="81473-222">In the applications list, select **8x8 Virtual Office**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_app.png) 

3. <span data-ttu-id="81473-224">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="81473-224">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="81473-226">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="81473-226">Click **Add** button.</span></span> <span data-ttu-id="81473-227">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="81473-227">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="81473-229">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="81473-229">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="81473-230">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="81473-230">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="81473-231">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="81473-231">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="81473-232">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="81473-232">Testing single sign-on</span></span>

<span data-ttu-id="81473-233">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="81473-233">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="81473-234">Ha a hozzáférési panelen 8 x 8 virtuális Office csempére kattint, akkor kell beolvasása automatikusan bejelentkezett 8 x 8 virtuális Office-alkalmazás számára.</span><span class="sxs-lookup"><span data-stu-id="81473-234">When you click the 8x8 Virtual Office tile in the Access Panel, you should get automatically signed-on to your 8x8 Virtual Office application.</span></span>
<span data-ttu-id="81473-235">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md)</span><span class="sxs-lookup"><span data-stu-id="81473-235">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md)</span></span>

## <a name="additional-resources"></a><span data-ttu-id="81473-236">További források</span><span class="sxs-lookup"><span data-stu-id="81473-236">Additional resources</span></span>

* [<span data-ttu-id="81473-237">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="81473-237">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="81473-238">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="81473-238">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_203.png

