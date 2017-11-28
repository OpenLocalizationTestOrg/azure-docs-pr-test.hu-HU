---
title: "Oktatóanyag: Azure Active Directoryval integrált Workfront |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Workfront között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: aab8bd2f-f9dd-42da-a18e-d707865687d7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: jeedes
ms.openlocfilehash: f7ba8d4895474de0da0e04da5f31959963ae65ff
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-workfront"></a><span data-ttu-id="a1bb1-103">Oktatóanyag: Azure Active Directoryval integrált Workfront</span><span class="sxs-lookup"><span data-stu-id="a1bb1-103">Tutorial: Azure Active Directory integration with Workfront</span></span>

<span data-ttu-id="a1bb1-104">Ebben az oktatóanyagban elsajátíthatja Workfront integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="a1bb1-104">In this tutorial, you learn how to integrate Workfront with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="a1bb1-105">Workfront integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="a1bb1-105">Integrating Workfront with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="a1bb1-106">Megadhatja a Workfront hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="a1bb1-106">You can control in Azure AD who has access to Workfront</span></span>
- <span data-ttu-id="a1bb1-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett Workfront (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="a1bb1-107">You can enable your users to automatically get signed-on to Workfront (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="a1bb1-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="a1bb1-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="a1bb1-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="a1bb1-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a1bb1-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="a1bb1-110">Prerequisites</span></span>

<span data-ttu-id="a1bb1-111">Konfigurálása az Azure AD-integrációs Workfront, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="a1bb1-111">To configure Azure AD integration with Workfront, you need the following items:</span></span>

- <span data-ttu-id="a1bb1-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="a1bb1-112">An Azure AD subscription</span></span>
- <span data-ttu-id="a1bb1-113">Egy Workfront egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="a1bb1-113">A Workfront single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="a1bb1-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="a1bb1-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="a1bb1-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="a1bb1-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="a1bb1-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="a1bb1-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="a1bb1-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a1bb1-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="a1bb1-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="a1bb1-118">Scenario description</span></span>
<span data-ttu-id="a1bb1-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="a1bb1-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="a1bb1-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="a1bb1-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="a1bb1-121">A gyűjteményből Workfront hozzáadása</span><span class="sxs-lookup"><span data-stu-id="a1bb1-121">Adding Workfront from the gallery</span></span>
2. <span data-ttu-id="a1bb1-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="a1bb1-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-workfront-from-the-gallery"></a><span data-ttu-id="a1bb1-123">A gyűjteményből Workfront hozzáadása</span><span class="sxs-lookup"><span data-stu-id="a1bb1-123">Adding Workfront from the gallery</span></span>
<span data-ttu-id="a1bb1-124">Az Azure AD integrálása a Workfront konfigurálásához kell hozzáadnia Workfront a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="a1bb1-124">To configure the integration of Workfront into Azure AD, you need to add Workfront from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="a1bb1-125">**A gyűjteményből Workfront hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="a1bb1-125">**To add Workfront from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="a1bb1-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="a1bb1-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="a1bb1-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="a1bb1-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="a1bb1-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="a1bb1-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="a1bb1-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="a1bb1-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="a1bb1-133">Írja be a keresőmezőbe, **Workfront**.</span><span class="sxs-lookup"><span data-stu-id="a1bb1-133">In the search box, type **Workfront**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-workfront-tutorial/tutorial_workfront_search.png)

5. <span data-ttu-id="a1bb1-135">Az eredmények panelen válassza ki a **Workfront**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="a1bb1-135">In the results panel, select **Workfront**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-workfront-tutorial/tutorial_workfront_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="a1bb1-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="a1bb1-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="a1bb1-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján Workfront</span><span class="sxs-lookup"><span data-stu-id="a1bb1-138">In this section, you configure and test Azure AD single sign-on with Workfront based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="a1bb1-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Workfront a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="a1bb1-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Workfront is to a user in Azure AD.</span></span> <span data-ttu-id="a1bb1-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Workfront közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="a1bb1-140">In other words, a link relationship between an Azure AD user and the related user in Workfront needs to be established.</span></span>

<span data-ttu-id="a1bb1-141">Workfront, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="a1bb1-141">In Workfront, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="a1bb1-142">Az Azure AD egyszeri bejelentkezést a Workfront tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="a1bb1-142">To configure and test Azure AD single sign-on with Workfront, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="a1bb1-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="a1bb1-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="a1bb1-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="a1bb1-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="a1bb1-145">**[Workfront tesztfelhasználó létrehozása](#creating-a-workfront-test-user)**  - való Britta Simon valami Workfront, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="a1bb1-145">**[Creating a Workfront test user](#creating-a-workfront-test-user)** - to have a counterpart of Britta Simon in Workfront that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="a1bb1-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="a1bb1-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="a1bb1-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="a1bb1-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="a1bb1-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="a1bb1-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="a1bb1-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Workfront alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="a1bb1-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Workfront application.</span></span>

<span data-ttu-id="a1bb1-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés Workfront, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="a1bb1-150">**To configure Azure AD single sign-on with Workfront, perform the following steps:**</span></span>

1. <span data-ttu-id="a1bb1-151">Az Azure portálon a a **Workfront** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="a1bb1-151">In the Azure portal, on the **Workfront** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="a1bb1-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="a1bb1-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-workfront-tutorial/tutorial_workfront_samlbase.png)

3. <span data-ttu-id="a1bb1-155">Az a **Workfront tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="a1bb1-155">On the **Workfront Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-workfront-tutorial/tutorial_workfront_url.png)

    <span data-ttu-id="a1bb1-157">a.</span><span class="sxs-lookup"><span data-stu-id="a1bb1-157">a.</span></span> <span data-ttu-id="a1bb1-158">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<companyname>.attask-ondemand.com`</span><span class="sxs-lookup"><span data-stu-id="a1bb1-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.attask-ondemand.com`</span></span>

    <span data-ttu-id="a1bb1-159">b.</span><span class="sxs-lookup"><span data-stu-id="a1bb1-159">b.</span></span> <span data-ttu-id="a1bb1-160">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://<companyname>.attasksandbox.com/SAML2`</span><span class="sxs-lookup"><span data-stu-id="a1bb1-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<companyname>.attasksandbox.com/SAML2`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="a1bb1-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="a1bb1-161">These values are not real.</span></span> <span data-ttu-id="a1bb1-162">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="a1bb1-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="a1bb1-163">Ügyfél [Workfront ügyfél-támogatási csoport](https://www.workfront.com/contact-us/) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="a1bb1-163">Contact [Workfront Client support team](https://www.workfront.com/contact-us/) to get these values.</span></span> 
 
4. <span data-ttu-id="a1bb1-164">Az a **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="a1bb1-164">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the Certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-workfront-tutorial/tutorial_workfront_certificate.png) 

5. <span data-ttu-id="a1bb1-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="a1bb1-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-workfront-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="a1bb1-168">A a **Workfront konfigurációs** kattintson **konfigurálása Workfront** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="a1bb1-168">On the **Workfront Configuration** section, click **Configure Workfront** to open **Configure sign-on** window.</span></span> <span data-ttu-id="a1bb1-169">Másolás a **Sign-Out URL-címet, és a SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="a1bb1-169">Copy the **Sign-Out URL, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-workfront-tutorial/tutorial_workfront_configure.png) 

7. <span data-ttu-id="a1bb1-171">Bejelentkezés a Workfront vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="a1bb1-171">Sign-on to your Workfront company site as administrator.</span></span>

8. <span data-ttu-id="a1bb1-172">Ugrás a **az egyszeri bejelentkezés beállítása**.</span><span class="sxs-lookup"><span data-stu-id="a1bb1-172">Go to **Single Sign On Configuration**.</span></span>

9. <span data-ttu-id="a1bb1-173">Az a **egyszeri bejelentkezés** párbeszédpanelen hajtsa végre az alábbi lépéseket</span><span class="sxs-lookup"><span data-stu-id="a1bb1-173">On the **Single Sign-On** dialog, perform the following steps</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása][23]
   
    <span data-ttu-id="a1bb1-175">a.</span><span class="sxs-lookup"><span data-stu-id="a1bb1-175">a.</span></span> <span data-ttu-id="a1bb1-176">Mint **típus**, jelölje be **SAML 2.0**.</span><span class="sxs-lookup"><span data-stu-id="a1bb1-176">As **Type**, select **SAML 2.0**.</span></span>
   
    <span data-ttu-id="a1bb1-177">b.</span><span class="sxs-lookup"><span data-stu-id="a1bb1-177">b.</span></span> <span data-ttu-id="a1bb1-178">Válassza ki **Szolgáltatóazonosító szolgáltatás**.</span><span class="sxs-lookup"><span data-stu-id="a1bb1-178">Select **Service Provider ID**.</span></span>
   
    <span data-ttu-id="a1bb1-179">c.</span><span class="sxs-lookup"><span data-stu-id="a1bb1-179">c.</span></span> <span data-ttu-id="a1bb1-180">Beillesztés a **SAML-alapú egyszeri bejelentkezési URL-címe** azokat a **bejelentkezési portál URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="a1bb1-180">Paste the **SAML Single Sign-On Service URL** into the **Login Portal URL** textbox.</span></span>
   
    <span data-ttu-id="a1bb1-181">d.</span><span class="sxs-lookup"><span data-stu-id="a1bb1-181">d.</span></span> <span data-ttu-id="a1bb1-182">Beillesztés **egyetlen Sign-Out URL-címe** azokat a **Sign-Out URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="a1bb1-182">Paste **Single Sign-Out Service URL** into the **Sign-Out URL** textbox.</span></span>
   
    <span data-ttu-id="a1bb1-183">e.</span><span class="sxs-lookup"><span data-stu-id="a1bb1-183">e.</span></span> <span data-ttu-id="a1bb1-184">Beillesztés **jelszó URL-cím módosítása** azokat a **jelszó URL-cím módosítása** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="a1bb1-184">Paste **Change Password URL** into the **Change Password URL** textbox.</span></span>
   
    <span data-ttu-id="a1bb1-185">f.</span><span class="sxs-lookup"><span data-stu-id="a1bb1-185">f.</span></span> <span data-ttu-id="a1bb1-186">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="a1bb1-186">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="a1bb1-187">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="a1bb1-187">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="a1bb1-188">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="a1bb1-188">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="a1bb1-189">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="a1bb1-189">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="a1bb1-190">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="a1bb1-190">Creating an Azure AD test user</span></span>
<span data-ttu-id="a1bb1-191">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="a1bb1-191">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="a1bb1-193">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="a1bb1-193">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="a1bb1-194">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="a1bb1-194">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-workfront-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="a1bb1-196">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="a1bb1-196">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-workfront-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="a1bb1-198">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="a1bb1-198">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-workfront-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="a1bb1-200">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="a1bb1-200">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-workfront-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="a1bb1-202">a.</span><span class="sxs-lookup"><span data-stu-id="a1bb1-202">a.</span></span> <span data-ttu-id="a1bb1-203">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="a1bb1-203">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="a1bb1-204">b.</span><span class="sxs-lookup"><span data-stu-id="a1bb1-204">b.</span></span> <span data-ttu-id="a1bb1-205">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="a1bb1-205">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="a1bb1-206">c.</span><span class="sxs-lookup"><span data-stu-id="a1bb1-206">c.</span></span> <span data-ttu-id="a1bb1-207">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="a1bb1-207">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="a1bb1-208">d.</span><span class="sxs-lookup"><span data-stu-id="a1bb1-208">d.</span></span> <span data-ttu-id="a1bb1-209">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="a1bb1-209">Click **Create**.</span></span>
 
### <a name="creating-a-workfront-test-user"></a><span data-ttu-id="a1bb1-210">Workfront tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="a1bb1-210">Creating a Workfront test user</span></span>

<span data-ttu-id="a1bb1-211">Ez a szakasz célja Workfront Britta Simon nevű felhasználót létrehozni.</span><span class="sxs-lookup"><span data-stu-id="a1bb1-211">The objective of this section is to create a user called Britta Simon in Workfront.</span></span>

<span data-ttu-id="a1bb1-212">**A felhasználó Britta Simon meghívta Workfront létrehozásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="a1bb1-212">**To create a user called Britta Simon in Workfront, perform the following steps:**</span></span>

1. <span data-ttu-id="a1bb1-213">Jelentkezzen be rendszergazdaként a Workfront vállalati webhely.</span><span class="sxs-lookup"><span data-stu-id="a1bb1-213">Sign on to your Workfront company site as administrator.</span></span>
2. <span data-ttu-id="a1bb1-214">Kattintson a felső menüben **személyek**.</span><span class="sxs-lookup"><span data-stu-id="a1bb1-214">In the menu on the top, click **People**.</span></span>
3. <span data-ttu-id="a1bb1-215">Kattintson a **új személy**.</span><span class="sxs-lookup"><span data-stu-id="a1bb1-215">Click **New Person**.</span></span> 
4. <span data-ttu-id="a1bb1-216">Új személy párbeszédpanelen hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="a1bb1-216">On the New Person dialog, perform the following steps:</span></span>
   
    ![Hozzon létre egy Workfront tesztfelhasználó számára][21] 
   
    <span data-ttu-id="a1bb1-218">a.</span><span class="sxs-lookup"><span data-stu-id="a1bb1-218">a.</span></span> <span data-ttu-id="a1bb1-219">Az a **Keresztnév** szövegmező, írja be a "Britta."</span><span class="sxs-lookup"><span data-stu-id="a1bb1-219">In the **First Name** textbox, type "Britta."</span></span>
   
    <span data-ttu-id="a1bb1-220">b.</span><span class="sxs-lookup"><span data-stu-id="a1bb1-220">b.</span></span> <span data-ttu-id="a1bb1-221">Az a **Vezetéknév** szövegmező, írja be a "Simon."</span><span class="sxs-lookup"><span data-stu-id="a1bb1-221">In the **Last Name** textbox, type "Simon."</span></span>
   
    <span data-ttu-id="a1bb1-222">c.</span><span class="sxs-lookup"><span data-stu-id="a1bb1-222">c.</span></span> <span data-ttu-id="a1bb1-223">Az a **E-mail cím** szövegmező, írja be a Britta Simon e-mail címét az Azure Active Directoryban.</span><span class="sxs-lookup"><span data-stu-id="a1bb1-223">In the **Email Address** textbox, type Britta Simon's email address in Azure Active Directory.</span></span>
   
    <span data-ttu-id="a1bb1-224">d.</span><span class="sxs-lookup"><span data-stu-id="a1bb1-224">d.</span></span> <span data-ttu-id="a1bb1-225">Kattintson a **személy hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="a1bb1-225">Click **Add Person**.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="a1bb1-226">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="a1bb1-226">Assigning the Azure AD test user</span></span>

<span data-ttu-id="a1bb1-227">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Workfront Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="a1bb1-227">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Workfront.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="a1bb1-229">**Britta Simon hozzárendelése Workfront, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="a1bb1-229">**To assign Britta Simon to Workfront, perform the following steps:**</span></span>

1. <span data-ttu-id="a1bb1-230">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="a1bb1-230">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="a1bb1-232">Az alkalmazások listában válassza ki a **Workfront**.</span><span class="sxs-lookup"><span data-stu-id="a1bb1-232">In the applications list, select **Workfront**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-workfront-tutorial/tutorial_workfront_app.png) 

3. <span data-ttu-id="a1bb1-234">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="a1bb1-234">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="a1bb1-236">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="a1bb1-236">Click **Add** button.</span></span> <span data-ttu-id="a1bb1-237">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="a1bb1-237">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="a1bb1-239">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="a1bb1-239">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="a1bb1-240">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="a1bb1-240">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="a1bb1-241">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="a1bb1-241">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="a1bb1-242">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="a1bb1-242">Testing single sign-on</span></span>

<span data-ttu-id="a1bb1-243">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="a1bb1-243">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="a1bb1-244">Ha a hozzáférési panelen Workfront csempére kattint, Workfront alkalmazás bejelentkezési oldalán szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="a1bb1-244">When you click the Workfront tile in the Access Panel, you should get login page of Workfront application.</span></span>
<span data-ttu-id="a1bb1-245">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="a1bb1-245">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="a1bb1-246">További források</span><span class="sxs-lookup"><span data-stu-id="a1bb1-246">Additional resources</span></span>

* [<span data-ttu-id="a1bb1-247">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="a1bb1-247">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="a1bb1-248">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="a1bb1-248">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-workfront-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-workfront-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-workfront-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-workfront-tutorial/tutorial_general_04.png
[21]:./media/active-directory-saas-workfront-tutorial/tutorial_attask_08.png
[23]:./media/active-directory-saas-workfront-tutorial/tutorial_attask_06.png
[100]: ./media/active-directory-saas-workfront-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-workfront-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-workfront-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-workfront-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-workfront-tutorial/tutorial_general_203.png

