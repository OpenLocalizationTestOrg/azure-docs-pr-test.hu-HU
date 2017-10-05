---
title: "Oktatóanyag: Azure Active Directoryval integrált Hackerone |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Hackerone között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 229d1efb-b6a5-4df8-9839-5d551487db4e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: jeedes
ms.openlocfilehash: 657d8d4c98b7b133698a5cda0aa675da7f68c464
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-hackerone"></a><span data-ttu-id="9c2a9-103">Oktatóanyag: Azure Active Directoryval integrált HackerOne</span><span class="sxs-lookup"><span data-stu-id="9c2a9-103">Tutorial: Azure Active Directory integration with HackerOne</span></span>

<span data-ttu-id="9c2a9-104">Ebben az oktatóanyagban elsajátíthatja HackerOne integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="9c2a9-104">In this tutorial, you learn how to integrate HackerOne with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="9c2a9-105">HackerOne integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="9c2a9-105">Integrating HackerOne with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="9c2a9-106">Megadhatja a HackerOne hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="9c2a9-106">You can control in Azure AD who has access to HackerOne</span></span>
- <span data-ttu-id="9c2a9-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett HackerOne (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="9c2a9-107">You can enable your users to automatically get signed-on to HackerOne (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="9c2a9-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="9c2a9-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="9c2a9-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="9c2a9-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9c2a9-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="9c2a9-110">Prerequisites</span></span>

<span data-ttu-id="9c2a9-111">Konfigurálása az Azure AD-integrációs HackerOne, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="9c2a9-111">To configure Azure AD integration with HackerOne, you need the following items:</span></span>

- <span data-ttu-id="9c2a9-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="9c2a9-112">An Azure AD subscription</span></span>
- <span data-ttu-id="9c2a9-113">Egy HackerOne egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="9c2a9-113">A HackerOne single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="9c2a9-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="9c2a9-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="9c2a9-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="9c2a9-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="9c2a9-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="9c2a9-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="9c2a9-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="9c2a9-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="9c2a9-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="9c2a9-118">Scenario description</span></span>
<span data-ttu-id="9c2a9-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="9c2a9-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="9c2a9-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="9c2a9-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="9c2a9-121">A gyűjteményből HackerOne hozzáadása</span><span class="sxs-lookup"><span data-stu-id="9c2a9-121">Adding HackerOne from the gallery</span></span>
2. <span data-ttu-id="9c2a9-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="9c2a9-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-hackerone-from-the-gallery"></a><span data-ttu-id="9c2a9-123">A gyűjteményből HackerOne hozzáadása</span><span class="sxs-lookup"><span data-stu-id="9c2a9-123">Adding HackerOne from the gallery</span></span>
<span data-ttu-id="9c2a9-124">Az Azure AD integrálása a HackerOne konfigurálásához kell hozzáadnia HackerOne a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="9c2a9-124">To configure the integration of HackerOne into Azure AD, you need to add HackerOne from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="9c2a9-125">**A gyűjteményből HackerOne hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="9c2a9-125">**To add HackerOne from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="9c2a9-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="9c2a9-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="9c2a9-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="9c2a9-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="9c2a9-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="9c2a9-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="9c2a9-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="9c2a9-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="9c2a9-133">Írja be a keresőmezőbe, **HackerOne**.</span><span class="sxs-lookup"><span data-stu-id="9c2a9-133">In the search box, type **HackerOne**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_search.png)

5. <span data-ttu-id="9c2a9-135">Az eredmények panelen válassza ki a **HackerOne**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="9c2a9-135">In the results panel, select **HackerOne**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="9c2a9-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="9c2a9-137">Configuring and testing Azure AD single sign-on</span></span>

<span data-ttu-id="9c2a9-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján HackerOne</span><span class="sxs-lookup"><span data-stu-id="9c2a9-138">In this section, you configure and test Azure AD single sign-on with HackerOne based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="9c2a9-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó HackerOne a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="9c2a9-139">For single sign-on to work, Azure AD needs to know what the counterpart user in HackerOne is to a user in Azure AD.</span></span> <span data-ttu-id="9c2a9-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a HackerOne közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="9c2a9-140">In other words, a link relationship between an Azure AD user and the related user in HackerOne needs to be established.</span></span>

<span data-ttu-id="9c2a9-141">HackerOne, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="9c2a9-141">In HackerOne, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="9c2a9-142">Az Azure AD egyszeri bejelentkezést a HackerOne tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="9c2a9-142">To configure and test Azure AD single sign-on with HackerOne, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="9c2a9-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="9c2a9-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="9c2a9-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="9c2a9-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="9c2a9-145">**[HackerOne tesztfelhasználó létrehozása](#creating-a-hackerone-test-user)**  - való Britta Simon valami HackerOne, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="9c2a9-145">**[Creating a HackerOne test user](#creating-a-hackerone-test-user)** - to have a counterpart of Britta Simon in HackerOne that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="9c2a9-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="9c2a9-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="9c2a9-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="9c2a9-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="9c2a9-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="9c2a9-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="9c2a9-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az HackerOne alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="9c2a9-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your HackerOne application.</span></span>

<span data-ttu-id="9c2a9-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés HackerOne, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="9c2a9-150">**To configure Azure AD single sign-on with HackerOne, perform the following steps:**</span></span>

1. <span data-ttu-id="9c2a9-151">Az Azure portálon a a **HackerOne** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="9c2a9-151">In the Azure portal, on the **HackerOne** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="9c2a9-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="9c2a9-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_samlbase.png)

3. <span data-ttu-id="9c2a9-155">Az a **HackerOne egyszeri bejelentkezési URL-cím és azonosító** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="9c2a9-155">On the **HackerOne Single sign-on URL and Identifier** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_url.png)

    <span data-ttu-id="9c2a9-157">a.</span><span class="sxs-lookup"><span data-stu-id="9c2a9-157">a.</span></span> <span data-ttu-id="9c2a9-158">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://hackerone.com/<company name>/authentication`</span><span class="sxs-lookup"><span data-stu-id="9c2a9-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://hackerone.com/<company name>/authentication`</span></span>

    <span data-ttu-id="9c2a9-159">b.</span><span class="sxs-lookup"><span data-stu-id="9c2a9-159">b.</span></span> <span data-ttu-id="9c2a9-160">Az a **azonosító** szövegmező, adja meg az URL-címet:`https://hackerone.com/users/saml/metadata`</span><span class="sxs-lookup"><span data-stu-id="9c2a9-160">In the **Identifier** textbox, type a URL as:  `https://hackerone.com/users/saml/metadata`</span></span>
    
    > [!NOTE] 
    > <span data-ttu-id="9c2a9-161">Ez az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="9c2a9-161">This value is not real.</span></span> <span data-ttu-id="9c2a9-162">Frissítse ezt az értéket a tényleges bejelentkezési URL-címet.</span><span class="sxs-lookup"><span data-stu-id="9c2a9-162">Update this value with the actual Sign-On URL.</span></span> <span data-ttu-id="9c2a9-163">Ügyfél [HackerOne támogatási csoport](mailto:support@hackerone.com) lekérni ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="9c2a9-163">Contact [HackerOne support team](mailto:support@hackerone.com) to get this value.</span></span> 
 
4. <span data-ttu-id="9c2a9-164">A a **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="9c2a9-164">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_certificate.png) 

5. <span data-ttu-id="9c2a9-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="9c2a9-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hackerone-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="9c2a9-168">A a **HackerOne konfigurációs** kattintson **konfigurálása HackerOne** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="9c2a9-168">On the **HackerOne Configuration** section, click **Configure HackerOne** to open **Configure sign-on** window.</span></span> <span data-ttu-id="9c2a9-169">Másolás a **SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="9c2a9-169">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_configure.png) 

7. <span data-ttu-id="9c2a9-171">Bejelentkezés a HackerOne bérlő rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="9c2a9-171">Sign On to your HackerOne tenant as an administrator.</span></span>

8. <span data-ttu-id="9c2a9-172">A felső menüben kattintson a "**beállítások**."</span><span class="sxs-lookup"><span data-stu-id="9c2a9-172">In the menu on the top, click the "**Settings**."</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_001.png) 

9. <span data-ttu-id="9c2a9-174">Keresse meg "**hitelesítési**", és kattintson a"**SAML beállítások hozzáadása**."</span><span class="sxs-lookup"><span data-stu-id="9c2a9-174">Navigate to "**Authentication**" and click "**Add SAML settings**."</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_003.png) 

10. <span data-ttu-id="9c2a9-176">Az a **SAML beállítások** párbeszédpanelen hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="9c2a9-176">On the **SAML Settings** dialog, perform the following steps:</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_004.png) 

    <span data-ttu-id="9c2a9-178">a.</span><span class="sxs-lookup"><span data-stu-id="9c2a9-178">a.</span></span> <span data-ttu-id="9c2a9-179">Az a **E-mail tartománya** szövegmező, adjon meg egy regisztrált tartományt.</span><span class="sxs-lookup"><span data-stu-id="9c2a9-179">In the **Email Domain** textbox, type a registered domain.</span></span>

    <span data-ttu-id="9c2a9-180">b.</span><span class="sxs-lookup"><span data-stu-id="9c2a9-180">b.</span></span> <span data-ttu-id="9c2a9-181">A **egyszeri bejelentkezési URL-cím** szövegmezőből, illessze be az értékét **SAML-alapú egyszeri bejelentkezési URL-címe** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="9c2a9-181">In  **Single Sign On URL** textboxes, paste the value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="9c2a9-182">c.</span><span class="sxs-lookup"><span data-stu-id="9c2a9-182">c.</span></span> <span data-ttu-id="9c2a9-183">Nyissa meg a **tanúsítványfájl** Azure portálról letöltött fájlt, másolja a vágólapra a tartalmát, és illessze be azt a **X509 tanúsítvány** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="9c2a9-183">Open your **Certificate file** in notepad downloaded from Azure portal, copy the content of it into your clipboard, and then paste it to the **X509 Certificate**  textbox.</span></span>
    
    <span data-ttu-id="9c2a9-184">d.</span><span class="sxs-lookup"><span data-stu-id="9c2a9-184">d.</span></span> <span data-ttu-id="9c2a9-185">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="9c2a9-185">Click **Save**.</span></span>

11. <span data-ttu-id="9c2a9-186">A hitelesítési beállítások párbeszédpanelen hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="9c2a9-186">On the Authentication Settings dialog, perform the following steps:</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_005.png) 

    <span data-ttu-id="9c2a9-188">a.</span><span class="sxs-lookup"><span data-stu-id="9c2a9-188">a.</span></span> <span data-ttu-id="9c2a9-189">Kattintson a **teszt futtatása**.</span><span class="sxs-lookup"><span data-stu-id="9c2a9-189">Click **Run test**.</span></span>

    <span data-ttu-id="9c2a9-190">b.</span><span class="sxs-lookup"><span data-stu-id="9c2a9-190">b.</span></span> <span data-ttu-id="9c2a9-191">Ha értékének a **állapot** mezőbe egyenlő **legutóbbi teszt állapota: létrehozott**, forduljon a [HackerOne támogatási csoport](mailto:support@hackerone.com) lekérni a konfigurációs áttekintése.</span><span class="sxs-lookup"><span data-stu-id="9c2a9-191">If the value of the **Status** field equals **Last test status: created**, contact your [HackerOne support team](mailto:support@hackerone.com) to request a review of your configuration.</span></span>

> [!TIP]
> <span data-ttu-id="9c2a9-192">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="9c2a9-192">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="9c2a9-193">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="9c2a9-193">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="9c2a9-194">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="9c2a9-194">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="9c2a9-195">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="9c2a9-195">Creating an Azure AD test user</span></span>
<span data-ttu-id="9c2a9-196">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="9c2a9-196">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="9c2a9-198">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="9c2a9-198">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="9c2a9-199">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="9c2a9-199">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-hackerone-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="9c2a9-201">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="9c2a9-201">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-hackerone-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="9c2a9-203">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="9c2a9-203">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-hackerone-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="9c2a9-205">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="9c2a9-205">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-hackerone-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="9c2a9-207">a.</span><span class="sxs-lookup"><span data-stu-id="9c2a9-207">a.</span></span> <span data-ttu-id="9c2a9-208">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="9c2a9-208">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="9c2a9-209">b.</span><span class="sxs-lookup"><span data-stu-id="9c2a9-209">b.</span></span> <span data-ttu-id="9c2a9-210">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="9c2a9-210">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="9c2a9-211">c.</span><span class="sxs-lookup"><span data-stu-id="9c2a9-211">c.</span></span> <span data-ttu-id="9c2a9-212">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="9c2a9-212">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="9c2a9-213">d.</span><span class="sxs-lookup"><span data-stu-id="9c2a9-213">d.</span></span> <span data-ttu-id="9c2a9-214">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="9c2a9-214">Click **Create**.</span></span>
 
### <a name="creating-a-hackerone-test-user"></a><span data-ttu-id="9c2a9-215">HackerOne tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="9c2a9-215">Creating a HackerOne test user</span></span>

<span data-ttu-id="9c2a9-216">Ezután hozzon létre egy HackerOne Britta Simon nevű felhasználó.</span><span class="sxs-lookup"><span data-stu-id="9c2a9-216">Next, you create a user called Britta Simon in HackerOne.</span></span> <span data-ttu-id="9c2a9-217">HackerOne támogatja közvetlenül az időponthoz kötött kiosztást, amely alapértelmezés szerint engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="9c2a9-217">HackerOne supports just-in-time provisioning, which is enabled by default.</span></span>

<span data-ttu-id="9c2a9-218">Nincs ebben a szakaszban az Ön művelet elem.</span><span class="sxs-lookup"><span data-stu-id="9c2a9-218">There is no action item for you in this section.</span></span> <span data-ttu-id="9c2a9-219">Amikor HackerOne fér hozzá, egy új felhasználó jön létre, ha még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="9c2a9-219">When you access HackerOne, a new user is created if it doesn't exist yet.</span></span>

>[!NOTE]
><span data-ttu-id="9c2a9-220">Ha a felhasználó manuálisan létrehozásához szükséges, a Certify támogatási csoportjához szeretné.</span><span class="sxs-lookup"><span data-stu-id="9c2a9-220">If you need to create a user manually, you need to contact the Certify support team.</span></span> 
> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="9c2a9-221">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="9c2a9-221">Assigning the Azure AD test user</span></span>

<span data-ttu-id="9c2a9-222">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés HackerOne Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="9c2a9-222">In this section, you enable Britta Simon to use Azure single sign-on by granting access to HackerOne.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="9c2a9-224">**Britta Simon hozzárendelése HackerOne, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="9c2a9-224">**To assign Britta Simon to HackerOne, perform the following steps:**</span></span>

1. <span data-ttu-id="9c2a9-225">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="9c2a9-225">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="9c2a9-227">Az alkalmazások listában válassza ki a **HackerOne**.</span><span class="sxs-lookup"><span data-stu-id="9c2a9-227">In the applications list, select **HackerOne**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_app.png) 

3. <span data-ttu-id="9c2a9-229">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="9c2a9-229">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="9c2a9-231">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="9c2a9-231">Click **Add** button.</span></span> <span data-ttu-id="9c2a9-232">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="9c2a9-232">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="9c2a9-234">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="9c2a9-234">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="9c2a9-235">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="9c2a9-235">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="9c2a9-236">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="9c2a9-236">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="9c2a9-237">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="9c2a9-237">Testing single sign-on</span></span>

<span data-ttu-id="9c2a9-238">Végezetül tesztelése az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen.</span><span class="sxs-lookup"><span data-stu-id="9c2a9-238">Finally, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>  

<span data-ttu-id="9c2a9-239">Ha a hozzáférési panelen HackerOne csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az HackerOne alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="9c2a9-239">When you click the HackerOne tile in the Access Panel, you should get automatically signed-on to your HackerOne application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9c2a9-240">További források</span><span class="sxs-lookup"><span data-stu-id="9c2a9-240">Additional resources</span></span>

* [<span data-ttu-id="9c2a9-241">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="9c2a9-241">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="9c2a9-242">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="9c2a9-242">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_203.png

