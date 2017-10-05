---
title: "Oktatóanyag: Azure Active Directoryval integrált LearnUpon |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és LearnUpon között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: b11c6315-c79d-4f34-9610-bd17070ab7c7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: b6ac8acc244e9029be01ede5e0865c280171217d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-learnupon"></a><span data-ttu-id="c2d3e-103">Oktatóanyag: Azure Active Directoryval integrált LearnUpon</span><span class="sxs-lookup"><span data-stu-id="c2d3e-103">Tutorial: Azure Active Directory integration with LearnUpon</span></span>

<span data-ttu-id="c2d3e-104">Ebben az oktatóanyagban elsajátíthatja LearnUpon integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="c2d3e-104">In this tutorial, you learn how to integrate LearnUpon with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="c2d3e-105">LearnUpon integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="c2d3e-105">Integrating LearnUpon with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="c2d3e-106">Megadhatja a LearnUpon hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="c2d3e-106">You can control in Azure AD who has access to LearnUpon</span></span>
- <span data-ttu-id="c2d3e-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett LearnUpon (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="c2d3e-107">You can enable your users to automatically get signed-on to LearnUpon (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="c2d3e-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="c2d3e-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="c2d3e-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="c2d3e-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c2d3e-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="c2d3e-110">Prerequisites</span></span>

<span data-ttu-id="c2d3e-111">Konfigurálása az Azure AD-integrációs LearnUpon, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="c2d3e-111">To configure Azure AD integration with LearnUpon, you need the following items:</span></span>

- <span data-ttu-id="c2d3e-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="c2d3e-112">An Azure AD subscription</span></span>
- <span data-ttu-id="c2d3e-113">Egy LearnUpon egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="c2d3e-113">A LearnUpon single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="c2d3e-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="c2d3e-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="c2d3e-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="c2d3e-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="c2d3e-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="c2d3e-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="c2d3e-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c2d3e-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="c2d3e-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="c2d3e-118">Scenario description</span></span>
<span data-ttu-id="c2d3e-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="c2d3e-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="c2d3e-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="c2d3e-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="c2d3e-121">A gyűjteményből LearnUpon hozzáadása</span><span class="sxs-lookup"><span data-stu-id="c2d3e-121">Adding LearnUpon from the gallery</span></span>
2. <span data-ttu-id="c2d3e-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="c2d3e-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-learnupon-from-the-gallery"></a><span data-ttu-id="c2d3e-123">A gyűjteményből LearnUpon hozzáadása</span><span class="sxs-lookup"><span data-stu-id="c2d3e-123">Adding LearnUpon from the gallery</span></span>
<span data-ttu-id="c2d3e-124">Az Azure AD integrálása a LearnUpon konfigurálásához kell hozzáadnia LearnUpon a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="c2d3e-124">To configure the integration of LearnUpon into Azure AD, you need to add LearnUpon from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="c2d3e-125">**A gyűjteményből LearnUpon hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="c2d3e-125">**To add LearnUpon from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="c2d3e-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="c2d3e-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="c2d3e-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="c2d3e-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="c2d3e-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="c2d3e-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="c2d3e-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="c2d3e-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="c2d3e-133">Írja be a keresőmezőbe, **LearnUpon**.</span><span class="sxs-lookup"><span data-stu-id="c2d3e-133">In the search box, type **LearnUpon**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_search.png)

5. <span data-ttu-id="c2d3e-135">Az eredmények panelen válassza ki a **LearnUpon**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="c2d3e-135">In the results panel, select **LearnUpon**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="c2d3e-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="c2d3e-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="c2d3e-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján LearnUpon.</span><span class="sxs-lookup"><span data-stu-id="c2d3e-138">In this section, you configure and test Azure AD single sign-on with LearnUpon based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="c2d3e-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó LearnUpon a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="c2d3e-139">For single sign-on to work, Azure AD needs to know what the counterpart user in LearnUpon is to a user in Azure AD.</span></span> <span data-ttu-id="c2d3e-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a LearnUpon közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="c2d3e-140">In other words, a link relationship between an Azure AD user and the related user in LearnUpon needs to be established.</span></span>

<span data-ttu-id="c2d3e-141">LearnUpon, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="c2d3e-141">In LearnUpon, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="c2d3e-142">Az Azure AD egyszeri bejelentkezést a LearnUpon tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="c2d3e-142">To configure and test Azure AD single sign-on with LearnUpon, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="c2d3e-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="c2d3e-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="c2d3e-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="c2d3e-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="c2d3e-145">**[LearnUpon tesztfelhasználó létrehozása](#creating-a-learnupon-test-user)**  - való Britta Simon valami LearnUpon, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="c2d3e-145">**[Creating a LearnUpon test user](#creating-a-learnupon-test-user)** - to have a counterpart of Britta Simon in LearnUpon that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="c2d3e-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="c2d3e-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="c2d3e-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="c2d3e-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="c2d3e-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c2d3e-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="c2d3e-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az LearnUpon alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="c2d3e-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your LearnUpon application.</span></span>

<span data-ttu-id="c2d3e-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés LearnUpon, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="c2d3e-150">**To configure Azure AD single sign-on with LearnUpon, perform the following steps:**</span></span>

1. <span data-ttu-id="c2d3e-151">Az Azure portálon a a **LearnUpon** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="c2d3e-151">In the Azure portal, on the **LearnUpon** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="c2d3e-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="c2d3e-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_samlbase.png)

3. <span data-ttu-id="c2d3e-155">Az a **LearnUpon tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="c2d3e-155">On the **LearnUpon Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_url.png)

    <span data-ttu-id="c2d3e-157">Az a **válasz URL-CÍMEN** szövegmező, adja meg a következő minta használatával URL-címe:`https://<companyname>.learnupon.com/saml/consumer`</span><span class="sxs-lookup"><span data-stu-id="c2d3e-157">In the **Reply URL** textbox, type a URL using the following pattern: `https://<companyname>.learnupon.com/saml/consumer`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="c2d3e-158">Ne feledje, hogy ez a nem a tényleges érték.</span><span class="sxs-lookup"><span data-stu-id="c2d3e-158">Please note that this is not the real value.</span></span> <span data-ttu-id="c2d3e-159">Ez az érték a tényleges válasz URL-címet frissíteni kell.</span><span class="sxs-lookup"><span data-stu-id="c2d3e-159">you have to update this value with the actual Reply URL.</span></span> <span data-ttu-id="c2d3e-160">Ez az érték ügyfél megszerezni [LearnUpon támogatási csoport](https://www.learnupon.com/features/support/).</span><span class="sxs-lookup"><span data-stu-id="c2d3e-160">To get this value Contact [LearnUpon support team](https://www.learnupon.com/features/support/).</span></span>



4. <span data-ttu-id="c2d3e-161">A a **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Raw)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="c2d3e-161">On the **SAML Signing Certificate** section, click **Certificate (Raw)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_certificate.png) 

5. <span data-ttu-id="c2d3e-163">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="c2d3e-163">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-learnupon-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="c2d3e-165">A a **LearnUpon konfigurációs** kattintson **konfigurálása LearnUpon** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="c2d3e-165">On the **LearnUpon Configuration** section, click **Configure LearnUpon** to open **Configure sign-on** window.</span></span> <span data-ttu-id="c2d3e-166">Másolás a **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="c2d3e-166">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_configure.png) 

7. <span data-ttu-id="c2d3e-168">Nyisson meg egy másik böngészőben példány és a bejelentkezési LearnUpon be rendszergazdai fiókkal.</span><span class="sxs-lookup"><span data-stu-id="c2d3e-168">Open another browser instance and login into LearnUpon with an administrator account.</span></span> 

8. <span data-ttu-id="c2d3e-169">Kattintson a **beállítások** fülre.</span><span class="sxs-lookup"><span data-stu-id="c2d3e-169">Click the **settings** tab.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_06.png)

9. <span data-ttu-id="c2d3e-171">Kattintson a **egyszeri bejelentkezés – SAML**, és kattintson a **általános beállítások** SAML-beállítások konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="c2d3e-171">Click **Single Sign On - SAML**, and then click **General Settings** to configure SAML settings.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_07.png) 

10. <span data-ttu-id="c2d3e-173">Az a **általános beállítások** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="c2d3e-173">In the **General Settings** section, perform the following steps:</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_08.png)  
  
    <span data-ttu-id="c2d3e-175">a.</span><span class="sxs-lookup"><span data-stu-id="c2d3e-175">a.</span></span> <span data-ttu-id="c2d3e-176">Válassza ki **engedélyezett**.</span><span class="sxs-lookup"><span data-stu-id="c2d3e-176">Select **Enabled**.</span></span>

    <span data-ttu-id="c2d3e-177">b.</span><span class="sxs-lookup"><span data-stu-id="c2d3e-177">b.</span></span> <span data-ttu-id="c2d3e-178">Válassza ki **verzió** , **2.0**.</span><span class="sxs-lookup"><span data-stu-id="c2d3e-178">Select **Version** as **2.0**.</span></span>

    <span data-ttu-id="c2d3e-179">c.</span><span class="sxs-lookup"><span data-stu-id="c2d3e-179">c.</span></span> <span data-ttu-id="c2d3e-180">Válassza ki **feltételek kihagyása** , **nem**.</span><span class="sxs-lookup"><span data-stu-id="c2d3e-180">Select **Skip conditions** as **No**.</span></span>

    <span data-ttu-id="c2d3e-181">d.</span><span class="sxs-lookup"><span data-stu-id="c2d3e-181">d.</span></span> <span data-ttu-id="c2d3e-182">Az a **SAML-jogkivonat utáni param neve** szövegmezőhöz kérelem feladás egy vagy több paramétert az SAML-alapú ügyfél URL-címe nevét a fenti típus, amely tartalmazza a SAML-előfeltétel ellenőrzése és a hitelesített – például a **SAMLResponse**.</span><span class="sxs-lookup"><span data-stu-id="c2d3e-182">In the **SAML Token Post param name** textbox, type the name of request post parameter to the SAML consumer URL indicated above that contains the SAML Assertion to be verified and authenticated - for example **SAMLResponse**.</span></span>

    <span data-ttu-id="c2d3e-183">e.</span><span class="sxs-lookup"><span data-stu-id="c2d3e-183">e.</span></span> <span data-ttu-id="c2d3e-184">Az a **azonosító formátuma** szövegmező, hol található a SAML-előfeltétel a felhasználók azonosítója (E-mail címet) jelző érték található – például típus **urn: oasis: nevek: tc: SAML:1.1:nameid-formátum: emailAddress**.</span><span class="sxs-lookup"><span data-stu-id="c2d3e-184">In the **Name Identifier Format** textbox, type the value that indicates where in your SAML Assertion the users identifier (Email address) resides - for example **urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress**.</span></span>
  
    <span data-ttu-id="c2d3e-185">f.</span><span class="sxs-lookup"><span data-stu-id="c2d3e-185">f.</span></span> <span data-ttu-id="c2d3e-186">Az a **azonosítása szolgáltató helye** szövegmező, írja be az értéket, amely jelzi, ahol a felhasználók kapnak, ha az Azure portál bejelentkezési képernyőjéről a feltöltött ikonra kattintanak.</span><span class="sxs-lookup"><span data-stu-id="c2d3e-186">In the **Identify Provider Location** textbox, type the value that indicates where the users are sent to if they click on your uploaded icon from your Azure portal login screen.</span></span>
  
    <span data-ttu-id="c2d3e-187">g.</span><span class="sxs-lookup"><span data-stu-id="c2d3e-187">g.</span></span> <span data-ttu-id="c2d3e-188">Az a **jelentkezzen ki az URL-cím** szövegmező, illessze be a **Sign-Out URL-cím** , amely az Azure portálról másolta.</span><span class="sxs-lookup"><span data-stu-id="c2d3e-188">In the **Sign out URL** textbox, paste the **Sign-Out URL** which you have copied from the Azure portal.</span></span>
    
    <span data-ttu-id="c2d3e-189">h.</span><span class="sxs-lookup"><span data-stu-id="c2d3e-189">h.</span></span> <span data-ttu-id="c2d3e-190">Kattintson a **ujját megrendelése kezelése**, majd töltse fel az ujjlenyomat a letöltött tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="c2d3e-190">Click **Manage finger prints**, and then upload the finger print of your downloaded certificate.</span></span>

11. <span data-ttu-id="c2d3e-191">Kattintson a **felhasználói beállítások**, majd végezze el az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="c2d3e-191">Click **User Settings**, and then perform the following steps:</span></span>
   
     ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_11.png)  
 
    <span data-ttu-id="c2d3e-193">a.</span><span class="sxs-lookup"><span data-stu-id="c2d3e-193">a.</span></span> <span data-ttu-id="c2d3e-194">Az a **Keresztnév azonosító formátuma** szövegmezőben, az érték, amely közli, amennyiben az a SAML-előfeltétel a felhasználók Keresztnév található – például típusa: **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.</span><span class="sxs-lookup"><span data-stu-id="c2d3e-194">In the **First Name Identifier Format** textbox, type the value that tells us where in your SAML Assertion the users firstname resides - for example: **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.</span></span>
  
    <span data-ttu-id="c2d3e-195">b.</span><span class="sxs-lookup"><span data-stu-id="c2d3e-195">b.</span></span> <span data-ttu-id="c2d3e-196">Az a **utolsó azonosító formátuma** szövegmezőben, az érték, amely közli, amennyiben az a SAML-előfeltétel a felhasználók Vezetéknév található – például típusa: **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**.</span><span class="sxs-lookup"><span data-stu-id="c2d3e-196">In the **Last Name Identifier Format** textbox, type the value that tells us where in your SAML Assertion the users lastname resides - for example: **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**.</span></span>

> [!TIP]
> <span data-ttu-id="c2d3e-197">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="c2d3e-197">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="c2d3e-198">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="c2d3e-198">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="c2d3e-199">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="c2d3e-199">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="c2d3e-200">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="c2d3e-200">Creating an Azure AD test user</span></span>
<span data-ttu-id="c2d3e-201">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="c2d3e-201">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="c2d3e-203">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="c2d3e-203">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="c2d3e-204">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="c2d3e-204">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-learnupon-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="c2d3e-206">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="c2d3e-206">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-learnupon-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="c2d3e-208">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="c2d3e-208">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-learnupon-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="c2d3e-210">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="c2d3e-210">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-learnupon-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="c2d3e-212">a.</span><span class="sxs-lookup"><span data-stu-id="c2d3e-212">a.</span></span> <span data-ttu-id="c2d3e-213">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="c2d3e-213">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="c2d3e-214">b.</span><span class="sxs-lookup"><span data-stu-id="c2d3e-214">b.</span></span> <span data-ttu-id="c2d3e-215">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="c2d3e-215">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="c2d3e-216">c.</span><span class="sxs-lookup"><span data-stu-id="c2d3e-216">c.</span></span> <span data-ttu-id="c2d3e-217">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="c2d3e-217">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="c2d3e-218">d.</span><span class="sxs-lookup"><span data-stu-id="c2d3e-218">d.</span></span> <span data-ttu-id="c2d3e-219">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="c2d3e-219">Click **Create**.</span></span>
 
### <a name="creating-a-learnupon-test-user"></a><span data-ttu-id="c2d3e-220">LearnUpon tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="c2d3e-220">Creating a LearnUpon test user</span></span>

<span data-ttu-id="c2d3e-221">Ez a szakasz célja LearnUpon Britta Simon nevű felhasználót létrehozni.</span><span class="sxs-lookup"><span data-stu-id="c2d3e-221">The objective of this section is to create a user called Britta Simon in LearnUpon.</span></span> <span data-ttu-id="c2d3e-222">LearnUpon támogatja just-in-time kiosztást, amely alapértelmezés szerint van engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="c2d3e-222">LearnUpon supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="c2d3e-223">Nincs ebben a szakaszban az Ön művelet elem.</span><span class="sxs-lookup"><span data-stu-id="c2d3e-223">There is no action item for you in this section.</span></span> <span data-ttu-id="c2d3e-224">Új felhasználó jön létre az LearnUpon elérésére, ha még nem létezik tett kísérlet során.</span><span class="sxs-lookup"><span data-stu-id="c2d3e-224">A new user will be created during an attempt to access LearnUpon if it doesn't exist yet.</span></span> <span data-ttu-id="c2d3e-225">[Az Azure AD-egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-single-sign-on).</span><span class="sxs-lookup"><span data-stu-id="c2d3e-225">[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-single-sign-on).</span></span>

>[!NOTE]
><span data-ttu-id="c2d3e-226">Ha hozzon létre manuálisan egy felhasználó van szüksége, forduljon a kell [LearnUpon támogatási csoport](https://www.learnupon.com/features/support/).</span><span class="sxs-lookup"><span data-stu-id="c2d3e-226">If you need to create an user manually, you need to contact [LearnUpon support team](https://www.learnupon.com/features/support/).</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="c2d3e-227">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="c2d3e-227">Assigning the Azure AD test user</span></span>

<span data-ttu-id="c2d3e-228">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés LearnUpon Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="c2d3e-228">In this section, you enable Britta Simon to use Azure single sign-on by granting access to LearnUpon.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="c2d3e-230">**Britta Simon hozzárendelése LearnUpon, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="c2d3e-230">**To assign Britta Simon to LearnUpon, perform the following steps:**</span></span>

1. <span data-ttu-id="c2d3e-231">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="c2d3e-231">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="c2d3e-233">Az alkalmazások listában válassza ki a **LearnUpon**.</span><span class="sxs-lookup"><span data-stu-id="c2d3e-233">In the applications list, select **LearnUpon**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_app.png) 

3. <span data-ttu-id="c2d3e-235">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="c2d3e-235">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="c2d3e-237">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="c2d3e-237">Click **Add** button.</span></span> <span data-ttu-id="c2d3e-238">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c2d3e-238">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="c2d3e-240">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="c2d3e-240">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="c2d3e-241">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c2d3e-241">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="c2d3e-242">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c2d3e-242">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="c2d3e-243">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="c2d3e-243">Testing single sign-on</span></span>

<span data-ttu-id="c2d3e-244">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="c2d3e-244">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="c2d3e-245">Ha a hozzáférési panelen LearnUpon csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az LearnUpon alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="c2d3e-245">When you click the LearnUpon tile in the Access Panel, you should get automatically signed-on to your LearnUpon application.</span></span>
<span data-ttu-id="c2d3e-246">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="c2d3e-246">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c2d3e-247">További források</span><span class="sxs-lookup"><span data-stu-id="c2d3e-247">Additional resources</span></span>

* [<span data-ttu-id="c2d3e-248">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="c2d3e-248">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="c2d3e-249">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="c2d3e-249">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_203.png

