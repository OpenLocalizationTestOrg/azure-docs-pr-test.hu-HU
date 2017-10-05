---
title: "Oktatóanyag: Azure Active Directoryval integrált BlueJeans |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és BlueJeans között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: dfc634fd-1b55-4ba8-94a8-b8288429b6a9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/31/2017
ms.author: jeedes
ms.openlocfilehash: 03bf65852b8d3cf14aebf155891a028db86e78d0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-bluejeans"></a><span data-ttu-id="847c1-103">Oktatóanyag: Azure Active Directoryval integrált BlueJeans</span><span class="sxs-lookup"><span data-stu-id="847c1-103">Tutorial: Azure Active Directory integration with BlueJeans</span></span>

<span data-ttu-id="847c1-104">Ebben az oktatóanyagban elsajátíthatja BlueJeans integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="847c1-104">In this tutorial, you learn how to integrate BlueJeans with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="847c1-105">BlueJeans integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="847c1-105">Integrating BlueJeans with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="847c1-106">Megadhatja a BlueJeans hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="847c1-106">You can control in Azure AD who has access to BlueJeans</span></span>
- <span data-ttu-id="847c1-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett BlueJeans (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="847c1-107">You can enable your users to automatically get signed-on to BlueJeans (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="847c1-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="847c1-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="847c1-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="847c1-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="847c1-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="847c1-110">Prerequisites</span></span>

<span data-ttu-id="847c1-111">Konfigurálása az Azure AD-integrációs BlueJeans, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="847c1-111">To configure Azure AD integration with BlueJeans, you need the following items:</span></span>

- <span data-ttu-id="847c1-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="847c1-112">An Azure AD subscription</span></span>
- <span data-ttu-id="847c1-113">Egy BlueJeans egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="847c1-113">A BlueJeans single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="847c1-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="847c1-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="847c1-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="847c1-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="847c1-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="847c1-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="847c1-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="847c1-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="847c1-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="847c1-118">Scenario description</span></span>
<span data-ttu-id="847c1-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="847c1-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="847c1-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="847c1-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="847c1-121">A gyűjteményből BlueJeans hozzáadása</span><span class="sxs-lookup"><span data-stu-id="847c1-121">Adding BlueJeans from the gallery</span></span>
2. <span data-ttu-id="847c1-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="847c1-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-bluejeans-from-the-gallery"></a><span data-ttu-id="847c1-123">A gyűjteményből BlueJeans hozzáadása</span><span class="sxs-lookup"><span data-stu-id="847c1-123">Adding BlueJeans from the gallery</span></span>
<span data-ttu-id="847c1-124">Az Azure AD integrálása a BlueJeans konfigurálásához kell hozzáadnia BlueJeans a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="847c1-124">To configure the integration of BlueJeans into Azure AD, you need to add BlueJeans from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="847c1-125">**A gyűjteményből BlueJeans hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="847c1-125">**To add BlueJeans from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="847c1-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="847c1-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="847c1-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="847c1-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="847c1-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="847c1-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="847c1-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="847c1-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="847c1-133">Írja be a keresőmezőbe, **BlueJeans**.</span><span class="sxs-lookup"><span data-stu-id="847c1-133">In the search box, type **BlueJeans**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bluejeans-tutorial/tutorial_bluejeans_search.png)

5. <span data-ttu-id="847c1-135">Az eredmények panelen válassza ki a **BlueJeans**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="847c1-135">In the results panel, select **BlueJeans**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bluejeans-tutorial/tutorial_bluejeans_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="847c1-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="847c1-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="847c1-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján BlueJeans</span><span class="sxs-lookup"><span data-stu-id="847c1-138">In this section, you configure and test Azure AD single sign-on with BlueJeans based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="847c1-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó BlueJeans a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="847c1-139">For single sign-on to work, Azure AD needs to know what the counterpart user in BlueJeans is to a user in Azure AD.</span></span> <span data-ttu-id="847c1-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a BlueJeans közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="847c1-140">In other words, a link relationship between an Azure AD user and the related user in BlueJeans needs to be established.</span></span>

<span data-ttu-id="847c1-141">BlueJeans, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="847c1-141">In BlueJeans, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="847c1-142">Az Azure AD egyszeri bejelentkezést a BlueJeans tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="847c1-142">To configure and test Azure AD single sign-on with BlueJeans, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="847c1-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="847c1-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="847c1-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="847c1-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="847c1-145">**[BlueJeans tesztfelhasználó létrehozása](#creating-a-bluejeans-test-user)**  - kell rendelkeznie a megfelelője a Britta Simon a BlueJeans, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="847c1-145">**[Creating a BlueJeans test user](#creating-a-bluejeans-test-user)** - to have a counterpart of Britta Simon in BlueJeans that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="847c1-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="847c1-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="847c1-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="847c1-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="847c1-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="847c1-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="847c1-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az BlueJeans alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="847c1-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your BlueJeans application.</span></span>

<span data-ttu-id="847c1-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés BlueJeans, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="847c1-150">**To configure Azure AD single sign-on with BlueJeans, perform the following steps:**</span></span>

1. <span data-ttu-id="847c1-151">Az Azure portálon a a **BlueJeans** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="847c1-151">In the Azure portal, on the **BlueJeans** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="847c1-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="847c1-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bluejeans-tutorial/tutorial_bluejeans_samlbase.png)

3. <span data-ttu-id="847c1-155">Az a **BlueJeans tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="847c1-155">On the **BlueJeans Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bluejeans-tutorial/tutorial_bluejeans_url.png)

    <span data-ttu-id="847c1-157">a.</span><span class="sxs-lookup"><span data-stu-id="847c1-157">a.</span></span> <span data-ttu-id="847c1-158">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<companyname>.BlueJeans.com`</span><span class="sxs-lookup"><span data-stu-id="847c1-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.BlueJeans.com`</span></span>

    <span data-ttu-id="847c1-159">b.</span><span class="sxs-lookup"><span data-stu-id="847c1-159">b.</span></span> <span data-ttu-id="847c1-160">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://<companyname>.BlueJeans.com`</span><span class="sxs-lookup"><span data-stu-id="847c1-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<companyname>.BlueJeans.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="847c1-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="847c1-161">These values are not real.</span></span> <span data-ttu-id="847c1-162">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="847c1-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="847c1-163">Ügyfél [BlueJeans ügyfél-támogatási csoport](https://support.bluejeans.com/contact) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="847c1-163">Contact [BlueJeans Client support team](https://support.bluejeans.com/contact) to get these values.</span></span> 
 
4. <span data-ttu-id="847c1-164">Az a **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="847c1-164">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bluejeans-tutorial/tutorial_bluejeans_certificate.png) 

5. <span data-ttu-id="847c1-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="847c1-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bluejeans-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="847c1-168">A a **BlueJeans konfigurációs** kattintson **konfigurálása BlueJeans** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="847c1-168">On the **BlueJeans Configuration** section, click **Configure BlueJeans** to open **Configure sign-on** window.</span></span> <span data-ttu-id="847c1-169">Másolás a **Sign-Out URL-cím, jelszó URL-cím módosítása és SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="847c1-169">Copy the **Sign-Out URL, Change Password URL and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bluejeans-tutorial/tutorial_bluejeans_configure.png) 

7. <span data-ttu-id="847c1-171">Egy másik webes böngészőablakban, jelentkezzen be a **BlueJeans** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="847c1-171">In a different web browser window, log in to your **BlueJeans** company site as an administrator.</span></span>

8. <span data-ttu-id="847c1-172">Ugrás a **ADMIN \> csoportbeállítások \> biztonsági**.</span><span class="sxs-lookup"><span data-stu-id="847c1-172">Go to **ADMIN \> Group Settings \> Security**.</span></span>
   
   <span data-ttu-id="847c1-173">![Felügyeleti](./media/active-directory-saas-bluejeans-tutorial/IC785868.png "rendszergazda")</span><span class="sxs-lookup"><span data-stu-id="847c1-173">![Admin](./media/active-directory-saas-bluejeans-tutorial/IC785868.png "Admin")</span></span>

9. <span data-ttu-id="847c1-174">Az a **biztonsági** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="847c1-174">In the **Security** section, perform the following steps:</span></span>
   
   <span data-ttu-id="847c1-175">![SAML-alapú egyszeri bejelentkezés](./media/active-directory-saas-bluejeans-tutorial/IC785869.png "SAML-alapú egyszeri bejelentkezés")</span><span class="sxs-lookup"><span data-stu-id="847c1-175">![SAML Single Sign On](./media/active-directory-saas-bluejeans-tutorial/IC785869.png "SAML Single Sign On")</span></span>   
   
   <span data-ttu-id="847c1-176">a.</span><span class="sxs-lookup"><span data-stu-id="847c1-176">a.</span></span> <span data-ttu-id="847c1-177">Válassza ki **SAML-alapú egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="847c1-177">Select **SAML Single Sign On**.</span></span>
  
   <span data-ttu-id="847c1-178">b.</span><span class="sxs-lookup"><span data-stu-id="847c1-178">b.</span></span> <span data-ttu-id="847c1-179">Válassza ki **automatikus kiépítés engedélyezéséhez**.</span><span class="sxs-lookup"><span data-stu-id="847c1-179">Select **Enable automatic provisioning**.</span></span>

10. <span data-ttu-id="847c1-180">Helyezze át, amely a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="847c1-180">Move on with the following steps:</span></span>

    <span data-ttu-id="847c1-181">![Tanúsítvány-elérési út](./media/active-directory-saas-bluejeans-tutorial/IC785870.png "tanúsítvány elérési útja")</span><span class="sxs-lookup"><span data-stu-id="847c1-181">![Certificate Path](./media/active-directory-saas-bluejeans-tutorial/IC785870.png "Certificate Path")</span></span>
    
    <span data-ttu-id="847c1-182">a.</span><span class="sxs-lookup"><span data-stu-id="847c1-182">a.</span></span> <span data-ttu-id="847c1-183">Kattintson a **Choose File**, majd töltse fel a letöltött tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="847c1-183">Click **Choose File**, and then upload the downloaded certificate.</span></span>
   
    <span data-ttu-id="847c1-184">b.</span><span class="sxs-lookup"><span data-stu-id="847c1-184">b.</span></span> <span data-ttu-id="847c1-185">Beillesztés **SAML-alapú egyszeri bejelentkezési URL-címe** azokat a **bejelentkezési URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="847c1-185">Paste **SAML Single Sign-On Service URL** into the **Login URL** textbox.</span></span>
   
    <span data-ttu-id="847c1-186">c.</span><span class="sxs-lookup"><span data-stu-id="847c1-186">c.</span></span> <span data-ttu-id="847c1-187">Beillesztés **jelszó URL-cím módosítása** azokat a **URL-cím jelszó módosítása** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="847c1-187">Paste **Change Password URL** into the **Password Change URL** textbox.</span></span>
   
    <span data-ttu-id="847c1-188">d.</span><span class="sxs-lookup"><span data-stu-id="847c1-188">d.</span></span> <span data-ttu-id="847c1-189">Beillesztés **Sign-Out URL-cím** azokat a **kijelentkezési URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="847c1-189">Paste **Sign-Out URL** into the **Logout URL** textbox.</span></span>

11. <span data-ttu-id="847c1-190">Helyezze át, amely a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="847c1-190">Move on with the following steps:</span></span>
    
    <span data-ttu-id="847c1-191">![Módosítások mentése](./media/active-directory-saas-bluejeans-tutorial/IC785874.png "módosítások mentése")</span><span class="sxs-lookup"><span data-stu-id="847c1-191">![Save Changes](./media/active-directory-saas-bluejeans-tutorial/IC785874.png "Save Changes")</span></span>
    
    <span data-ttu-id="847c1-192">a.</span><span class="sxs-lookup"><span data-stu-id="847c1-192">a.</span></span> <span data-ttu-id="847c1-193">Az a **felhasználóazonosító** szövegmezőhöz típus `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.</span><span class="sxs-lookup"><span data-stu-id="847c1-193">In the **User id** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.</span></span>
   
    <span data-ttu-id="847c1-194">b.</span><span class="sxs-lookup"><span data-stu-id="847c1-194">b.</span></span> <span data-ttu-id="847c1-195">Az a **E-mail** szövegmezőhöz típus `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.</span><span class="sxs-lookup"><span data-stu-id="847c1-195">In the **Email** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.</span></span>
   
    <span data-ttu-id="847c1-196">c.</span><span class="sxs-lookup"><span data-stu-id="847c1-196">c.</span></span> <span data-ttu-id="847c1-197">Kattintson a **módosítások mentése**.</span><span class="sxs-lookup"><span data-stu-id="847c1-197">Click **Save Changes**.</span></span>

> [!TIP]
> <span data-ttu-id="847c1-198">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="847c1-198">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="847c1-199">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="847c1-199">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="847c1-200">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="847c1-200">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="847c1-201">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="847c1-201">Creating an Azure AD test user</span></span>
<span data-ttu-id="847c1-202">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="847c1-202">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="847c1-204">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="847c1-204">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="847c1-205">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="847c1-205">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bluejeans-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="847c1-207">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="847c1-207">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bluejeans-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="847c1-209">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="847c1-209">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bluejeans-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="847c1-211">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="847c1-211">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bluejeans-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="847c1-213">a.</span><span class="sxs-lookup"><span data-stu-id="847c1-213">a.</span></span> <span data-ttu-id="847c1-214">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="847c1-214">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="847c1-215">b.</span><span class="sxs-lookup"><span data-stu-id="847c1-215">b.</span></span> <span data-ttu-id="847c1-216">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="847c1-216">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="847c1-217">c.</span><span class="sxs-lookup"><span data-stu-id="847c1-217">c.</span></span> <span data-ttu-id="847c1-218">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="847c1-218">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="847c1-219">d.</span><span class="sxs-lookup"><span data-stu-id="847c1-219">d.</span></span> <span data-ttu-id="847c1-220">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="847c1-220">Click **Create**.</span></span>
 
### <a name="creating-a-bluejeans-test-user"></a><span data-ttu-id="847c1-221">BlueJeans tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="847c1-221">Creating a BlueJeans test user</span></span>

<span data-ttu-id="847c1-222">Ahhoz, hogy az Azure AD-felhasználók BlueJeans bejelentkezni, akkor ki kell építenie a BlueJeans.</span><span class="sxs-lookup"><span data-stu-id="847c1-222">To enable Azure AD users to log in to BlueJeans, they must be provisioned into BlueJeans.</span></span>  

<span data-ttu-id="847c1-223">BlueJeans, ha egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="847c1-223">In case of BlueJeans, provisioning is a manual task.</span></span>

<span data-ttu-id="847c1-224">**A felhasználói fiókok létrehozásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="847c1-224">**To provision a user accounts, perform the following steps:**</span></span>

1. <span data-ttu-id="847c1-225">Jelentkezzen be a **BlueJeans** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="847c1-225">Log in to your **BlueJeans** company site as an administrator.</span></span>

2. <span data-ttu-id="847c1-226">Ugrás a **ADMIN \> felhasználók kezelése \> felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="847c1-226">Go to **ADMIN \> Manage Users \> Add User**.</span></span>
   
   <span data-ttu-id="847c1-227">![Felügyeleti](./media/active-directory-saas-bluejeans-tutorial/IC785877.png "rendszergazda")</span><span class="sxs-lookup"><span data-stu-id="847c1-227">![Admin](./media/active-directory-saas-bluejeans-tutorial/IC785877.png "Admin")</span></span>
   
   >[!IMPORTANT]
   ><span data-ttu-id="847c1-228">A **felhasználó hozzáadása** lap csak akkor használható, ha az a **Biztonság lap**, **automatikus kiépítés engedélyezéséhez** nincs bejelölve.</span><span class="sxs-lookup"><span data-stu-id="847c1-228">The **Add User** tab is only available if, in the **Security tab**, **Enable automatic provisioning** is unchecked.</span></span> 
   
3. <span data-ttu-id="847c1-229">Az a **felhasználó hozzáadása** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="847c1-229">In the **Add User** section, perform the following steps:</span></span>

    <span data-ttu-id="847c1-230">![Felhasználó hozzáadása](./media/active-directory-saas-bluejeans-tutorial/IC785886.png "felhasználó hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="847c1-230">![Add User](./media/active-directory-saas-bluejeans-tutorial/IC785886.png "Add User")</span></span>
    
    <span data-ttu-id="847c1-231">a.</span><span class="sxs-lookup"><span data-stu-id="847c1-231">a.</span></span> <span data-ttu-id="847c1-232">Adjon meg egy **BlueJeans felhasználónév**, egy **E-mail cím**, egy **BlueJeans értekezlet azonosítója**, egy **moderátor PIN-kód**, egy **teljes nevét**, a **vállalati** szeretné azokat a kapcsolódó szövegmezők rendelkezés érvényes AAD-fiók.</span><span class="sxs-lookup"><span data-stu-id="847c1-232">Type a **BlueJeans Username**, an **Email address**, a **BlueJeans Meeting ID**, a **Moderator Passcode**, a **Full Name**, the **Company** of a valid AAD account you want to provision into the related textboxes.</span></span>
    
    <span data-ttu-id="847c1-233">b.</span><span class="sxs-lookup"><span data-stu-id="847c1-233">b.</span></span> <span data-ttu-id="847c1-234">Kattintson a **felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="847c1-234">Click **Add User**.</span></span>

>[!NOTE]
><span data-ttu-id="847c1-235">Bármely más BlueJeans felhasználói fiók létrehozása eszközök vagy rendelkezés AAD felhasználói fiókokhoz BlueJeans által nyújtott API-k.</span><span class="sxs-lookup"><span data-stu-id="847c1-235">You can use any other BlueJeans user account creation tools or APIs provided by BlueJeans to provision AAD user accounts.</span></span> 
> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="847c1-236">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="847c1-236">Assigning the Azure AD test user</span></span>

<span data-ttu-id="847c1-237">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés BlueJeans Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="847c1-237">In this section, you enable Britta Simon to use Azure single sign-on by granting access to BlueJeans.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="847c1-239">**Britta Simon hozzárendelése BlueJeans, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="847c1-239">**To assign Britta Simon to BlueJeans, perform the following steps:**</span></span>

1. <span data-ttu-id="847c1-240">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="847c1-240">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="847c1-242">Az alkalmazások listában válassza ki a **BlueJeans**.</span><span class="sxs-lookup"><span data-stu-id="847c1-242">In the applications list, select **BlueJeans**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bluejeans-tutorial/tutorial_bluejeans_app.png) 

3. <span data-ttu-id="847c1-244">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="847c1-244">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="847c1-246">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="847c1-246">Click **Add** button.</span></span> <span data-ttu-id="847c1-247">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="847c1-247">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="847c1-249">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="847c1-249">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="847c1-250">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="847c1-250">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="847c1-251">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="847c1-251">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="847c1-252">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="847c1-252">Testing single sign-on</span></span>

<span data-ttu-id="847c1-253">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="847c1-253">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="847c1-254">Ha a hozzáférési panelen BlueJeans csempére kattint, BlueJeans alkalmazás bejelentkezési oldalán szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="847c1-254">When you click the BlueJeans tile in the Access Panel, you should get login page of BlueJeans application.</span></span>
<span data-ttu-id="847c1-255">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="847c1-255">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="847c1-256">További források</span><span class="sxs-lookup"><span data-stu-id="847c1-256">Additional resources</span></span>

* [<span data-ttu-id="847c1-257">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="847c1-257">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="847c1-258">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="847c1-258">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_203.png

