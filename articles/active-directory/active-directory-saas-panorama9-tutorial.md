---
title: "Oktatóanyag: Azure Active Directoryval integrált Panorama9 |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Panorama9 között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 5e28d7fa-03be-49f3-96c8-b567f1257d44
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: jeedes
ms.openlocfilehash: 934c0743464fd32398071aa3d07f7af76fdf7e3b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-panorama9"></a><span data-ttu-id="dd92f-103">Oktatóanyag: Azure Active Directoryval integrált Panorama9</span><span class="sxs-lookup"><span data-stu-id="dd92f-103">Tutorial: Azure Active Directory integration with Panorama9</span></span>

<span data-ttu-id="dd92f-104">Ebben az oktatóanyagban elsajátíthatja Panorama9 integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="dd92f-104">In this tutorial, you learn how to integrate Panorama9 with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="dd92f-105">Panorama9 integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="dd92f-105">Integrating Panorama9 with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="dd92f-106">Megadhatja a Panorama9 hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="dd92f-106">You can control in Azure AD who has access to Panorama9</span></span>
- <span data-ttu-id="dd92f-107">Engedélyezheti a felhasználóknak, hogy automatikusan lekérni aláírt a Panorama9 (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="dd92f-107">You can enable your users to automatically get signed-on to Panorama9 (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="dd92f-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="dd92f-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="dd92f-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="dd92f-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="dd92f-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="dd92f-110">Prerequisites</span></span>

<span data-ttu-id="dd92f-111">Konfigurálása az Azure AD-integrációs Panorama9, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="dd92f-111">To configure Azure AD integration with Panorama9, you need the following items:</span></span>

- <span data-ttu-id="dd92f-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="dd92f-112">An Azure AD subscription</span></span>
- <span data-ttu-id="dd92f-113">Egy Panorama9 egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="dd92f-113">A Panorama9 single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="dd92f-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="dd92f-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="dd92f-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="dd92f-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="dd92f-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="dd92f-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="dd92f-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="dd92f-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="dd92f-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="dd92f-118">Scenario description</span></span>
<span data-ttu-id="dd92f-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="dd92f-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="dd92f-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="dd92f-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="dd92f-121">A gyűjteményből Panorama9 hozzáadása</span><span class="sxs-lookup"><span data-stu-id="dd92f-121">Adding Panorama9 from the gallery</span></span>
2. <span data-ttu-id="dd92f-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="dd92f-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-panorama9-from-the-gallery"></a><span data-ttu-id="dd92f-123">A gyűjteményből Panorama9 hozzáadása</span><span class="sxs-lookup"><span data-stu-id="dd92f-123">Adding Panorama9 from the gallery</span></span>
<span data-ttu-id="dd92f-124">Az Azure AD integrálása a Panorama9 konfigurálásához kell hozzáadnia Panorama9 a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="dd92f-124">To configure the integration of Panorama9 into Azure AD, you need to add Panorama9 from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="dd92f-125">**A gyűjteményből Panorama9 hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="dd92f-125">**To add Panorama9 from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="dd92f-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="dd92f-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="dd92f-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="dd92f-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="dd92f-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="dd92f-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="dd92f-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="dd92f-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="dd92f-133">Írja be a keresőmezőbe, **Panorama9**.</span><span class="sxs-lookup"><span data-stu-id="dd92f-133">In the search box, type **Panorama9**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-panorama9-tutorial/tutorial_panorama9_search.png)

5. <span data-ttu-id="dd92f-135">Az eredmények panelen válassza ki a **Panorama9**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="dd92f-135">In the results panel, select **Panorama9**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-panorama9-tutorial/tutorial_panorama9_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="dd92f-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="dd92f-137">Configuring and testing Azure AD single sign-on</span></span>

<span data-ttu-id="dd92f-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján Panorama9</span><span class="sxs-lookup"><span data-stu-id="dd92f-138">In this section, you configure and test Azure AD single sign-on with Panorama9 based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="dd92f-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Panorama9 a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="dd92f-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Panorama9 is to a user in Azure AD.</span></span> <span data-ttu-id="dd92f-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Panorama9 közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="dd92f-140">In other words, a link relationship between an Azure AD user and the related user in Panorama9 needs to be established.</span></span>

<span data-ttu-id="dd92f-141">Panorama9, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="dd92f-141">In Panorama9, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="dd92f-142">Az Azure AD egyszeri bejelentkezést a Panorama9 tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="dd92f-142">To configure and test Azure AD single sign-on with Panorama9, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="dd92f-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="dd92f-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="dd92f-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="dd92f-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="dd92f-145">**[Panorama9 tesztfelhasználó létrehozása](#creating-a-panorama9-test-user)**  - való Britta Simon valami Panorama9, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="dd92f-145">**[Creating a Panorama9 test user](#creating-a-panorama9-test-user)** - to have a counterpart of Britta Simon in Panorama9 that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="dd92f-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="dd92f-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="dd92f-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="dd92f-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="dd92f-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="dd92f-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="dd92f-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Panorama9 alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="dd92f-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Panorama9 application.</span></span>

<span data-ttu-id="dd92f-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés Panorama9, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="dd92f-150">**To configure Azure AD single sign-on with Panorama9, perform the following steps:**</span></span>

1. <span data-ttu-id="dd92f-151">Az Azure portálon a a **Panorama9** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="dd92f-151">In the Azure portal, on the **Panorama9** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="dd92f-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="dd92f-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-panorama9-tutorial/tutorial_panorama9_samlbase.png)

3. <span data-ttu-id="dd92f-155">Az a **Panorama9 tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="dd92f-155">On the **Panorama9 Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-panorama9-tutorial/tutorial_panorama9_url.png)

    <span data-ttu-id="dd92f-157">a.</span><span class="sxs-lookup"><span data-stu-id="dd92f-157">a.</span></span> <span data-ttu-id="dd92f-158">Az a **bejelentkezési URL-cím** szövegmező, adja meg az URL-címet:`https://dashboard.panorama9.com/saml/access/3262`</span><span class="sxs-lookup"><span data-stu-id="dd92f-158">In the **Sign-on URL** textbox, type a URL as: `https://dashboard.panorama9.com/saml/access/3262`</span></span>

    <span data-ttu-id="dd92f-159">b.</span><span class="sxs-lookup"><span data-stu-id="dd92f-159">b.</span></span> <span data-ttu-id="dd92f-160">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`http://www.panorama9.com/saml20/<tenant-name>`</span><span class="sxs-lookup"><span data-stu-id="dd92f-160">In the **Identifier** textbox, type a URL using the following pattern: `http://www.panorama9.com/saml20/<tenant-name>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="dd92f-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="dd92f-161">These values are not real.</span></span> <span data-ttu-id="dd92f-162">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="dd92f-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="dd92f-163">Ügyfél [Panorama9 ügyfél-támogatási csoport](https://support.panorama9.com) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="dd92f-163">Contact [Panorama9 Client support team](https://support.panorama9.com) to get these values.</span></span> 
 
4. <span data-ttu-id="dd92f-164">Az a **SAML-aláíró tanúsítványa** szakaszban, másolja a **UJJLENYOMAT** tanúsítvány értékét.</span><span class="sxs-lookup"><span data-stu-id="dd92f-164">On the **SAML Signing Certificate** section, copy the **THUMBPRINT** value of certificate.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-panorama9-tutorial/tutorial_panorama9_certificate.png) 

5. <span data-ttu-id="dd92f-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="dd92f-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-panorama9-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="dd92f-168">A a **Panorama9 konfigurációs** kattintson **konfigurálása Panorama9** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="dd92f-168">On the **Panorama9 Configuration** section, click **Configure Panorama9** to open **Configure sign-on** window.</span></span> <span data-ttu-id="dd92f-169">Másolás a **SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="dd92f-169">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-panorama9-tutorial/tutorial_panorama9_configure.png) 

5. <span data-ttu-id="dd92f-171">Egy másik webes böngészőablakban jelentkezzen be a Panorama9 vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="dd92f-171">In a different web browser window, log into your Panorama9 company site as an administrator.</span></span>

6. <span data-ttu-id="dd92f-172">A felső eszköztáron kattintson **kezelése**, és kattintson a **bővítmények**.</span><span class="sxs-lookup"><span data-stu-id="dd92f-172">In the toolbar on the top, click **Manage**, and then click **Extensions**.</span></span>
   
   <span data-ttu-id="dd92f-173">![Bővítmények](./media/active-directory-saas-panorama9-tutorial/ic790023.png "bővítmények")</span><span class="sxs-lookup"><span data-stu-id="dd92f-173">![Extensions](./media/active-directory-saas-panorama9-tutorial/ic790023.png "Extensions")</span></span>
7. <span data-ttu-id="dd92f-174">Az a **bővítmények** párbeszédpanel, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="dd92f-174">On the **Extensions** dialog, click **Single Sign-On**.</span></span>
   
   <span data-ttu-id="dd92f-175">![Egyszeri bejelentkezés](./media/active-directory-saas-panorama9-tutorial/ic790024.png "egyszeri bejelentkezés")</span><span class="sxs-lookup"><span data-stu-id="dd92f-175">![Single Sign-On](./media/active-directory-saas-panorama9-tutorial/ic790024.png "Single Sign-On")</span></span>
8. <span data-ttu-id="dd92f-176">Az a **beállítások** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="dd92f-176">In the **Settings** section, perform the following steps:</span></span>
   
   <span data-ttu-id="dd92f-177">![Beállítások](./media/active-directory-saas-panorama9-tutorial/ic790025.png "beállítások")</span><span class="sxs-lookup"><span data-stu-id="dd92f-177">![Settings](./media/active-directory-saas-panorama9-tutorial/ic790025.png "Settings")</span></span>
   
    <span data-ttu-id="dd92f-178">a.</span><span class="sxs-lookup"><span data-stu-id="dd92f-178">a.</span></span> <span data-ttu-id="dd92f-179">A **identitási szolgáltató URL-cím** szövegmezőhöz illessze be az értékét **egyszeri bejelentkezési URL-címe**, amely az Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="dd92f-179">In **Identity provider URL** textbox, paste the value of **Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>
   
    <span data-ttu-id="dd92f-180">b.</span><span class="sxs-lookup"><span data-stu-id="dd92f-180">b.</span></span> <span data-ttu-id="dd92f-181">A **tanúsítvány-ujjlenyomat** szövegmező, illessze be a **ujjlenyomat** tanúsítványt, amely az Azure-portálon másolta értékét.</span><span class="sxs-lookup"><span data-stu-id="dd92f-181">In **Certificate fingerprint** textbox, paste the **Thumbprint** value of certificate, which you have copied from Azure portal.</span></span>    
         
9. <span data-ttu-id="dd92f-182">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="dd92f-182">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="dd92f-183">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="dd92f-183">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="dd92f-184">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="dd92f-184">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="dd92f-185">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="dd92f-185">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="dd92f-186">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="dd92f-186">Creating an Azure AD test user</span></span>
<span data-ttu-id="dd92f-187">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="dd92f-187">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="dd92f-189">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="dd92f-189">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="dd92f-190">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="dd92f-190">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-panorama9-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="dd92f-192">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="dd92f-192">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-panorama9-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="dd92f-194">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="dd92f-194">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-panorama9-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="dd92f-196">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="dd92f-196">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-panorama9-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="dd92f-198">a.</span><span class="sxs-lookup"><span data-stu-id="dd92f-198">a.</span></span> <span data-ttu-id="dd92f-199">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="dd92f-199">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="dd92f-200">b.</span><span class="sxs-lookup"><span data-stu-id="dd92f-200">b.</span></span> <span data-ttu-id="dd92f-201">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="dd92f-201">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="dd92f-202">c.</span><span class="sxs-lookup"><span data-stu-id="dd92f-202">c.</span></span> <span data-ttu-id="dd92f-203">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="dd92f-203">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="dd92f-204">d.</span><span class="sxs-lookup"><span data-stu-id="dd92f-204">d.</span></span> <span data-ttu-id="dd92f-205">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="dd92f-205">Click **Create**.</span></span>
 
### <a name="creating-a-panorama9-test-user"></a><span data-ttu-id="dd92f-206">Panorama9 tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="dd92f-206">Creating a Panorama9 test user</span></span>

<span data-ttu-id="dd92f-207">Ahhoz, hogy az Azure AD-felhasználók Panorama9 bejelentkezni, akkor ki kell építenie Panorama9 be.</span><span class="sxs-lookup"><span data-stu-id="dd92f-207">In order to enable Azure AD users to log into Panorama9, they must be provisioned into Panorama9.</span></span>  

<span data-ttu-id="dd92f-208">Panorama9, ha egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="dd92f-208">In the case of Panorama9, provisioning is a manual task.</span></span>

<span data-ttu-id="dd92f-209">**Adja meg a felhasználók átadása, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="dd92f-209">**To configure user provisioning, perform the following steps:**</span></span>

1. <span data-ttu-id="dd92f-210">Jelentkezzen be a **Panorama9** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="dd92f-210">Log in to your **Panorama9** company site as an administrator.</span></span>

2. <span data-ttu-id="dd92f-211">Kattintson a felső menüben **kezelése**, és kattintson a **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="dd92f-211">In the menu on the top, click **Manage**, and then click **Users**.</span></span>
   
  <span data-ttu-id="dd92f-212">![Felhasználók](./media/active-directory-saas-panorama9-tutorial/ic790027.png "felhasználók")</span><span class="sxs-lookup"><span data-stu-id="dd92f-212">![Users](./media/active-directory-saas-panorama9-tutorial/ic790027.png "Users")</span></span>

3. <span data-ttu-id="dd92f-213">A felhasználók szakaszban kattintson  **+**  új felhasználó hozzáadásához.</span><span class="sxs-lookup"><span data-stu-id="dd92f-213">In the Users section, Click **+** to add new user.</span></span>

 <span data-ttu-id="dd92f-214">![Felhasználók](./media/active-directory-saas-panorama9-tutorial/ic790028.png "felhasználók")</span><span class="sxs-lookup"><span data-stu-id="dd92f-214">![Users](./media/active-directory-saas-panorama9-tutorial/ic790028.png "Users")</span></span>

4. <span data-ttu-id="dd92f-215">Keresse meg a felhasználói adatok, írja be a felhasználó e-mail címe az egy érvényes Azure Active Directory szeretne kiépíteni a **E-mail** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="dd92f-215">Go to the User data section, type the email address of a valid Azure Active Directory user you want to provision into the **Email** textbox.</span></span>

5. <span data-ttu-id="dd92f-216">A felhasználók szakaszban kattintson tudomást **mentése**.</span><span class="sxs-lookup"><span data-stu-id="dd92f-216">Come to the Users section, Click **Save**.</span></span>
   
> [!NOTE]
    > <span data-ttu-id="dd92f-217">Az Azure Active Directory fióktulajdonos kap egy e-mailt, és a következő hivatkozás a fiók megerősítéséhez, mielőtt aktívvá válik.</span><span class="sxs-lookup"><span data-stu-id="dd92f-217">The Azure Active Directory account holder receives an email and follows a link to confirm their account before it becomes active.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="dd92f-218">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="dd92f-218">Assigning the Azure AD test user</span></span>

<span data-ttu-id="dd92f-219">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Panorama9 Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="dd92f-219">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Panorama9.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="dd92f-221">**Britta Simon hozzárendelése Panorama9, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="dd92f-221">**To assign Britta Simon to Panorama9, perform the following steps:**</span></span>

1. <span data-ttu-id="dd92f-222">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="dd92f-222">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="dd92f-224">Az alkalmazások listában válassza ki a **Panorama9**.</span><span class="sxs-lookup"><span data-stu-id="dd92f-224">In the applications list, select **Panorama9**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-panorama9-tutorial/tutorial_panorama9_app.png) 

3. <span data-ttu-id="dd92f-226">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="dd92f-226">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="dd92f-228">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="dd92f-228">Click **Add** button.</span></span> <span data-ttu-id="dd92f-229">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="dd92f-229">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="dd92f-231">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="dd92f-231">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="dd92f-232">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="dd92f-232">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="dd92f-233">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="dd92f-233">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="dd92f-234">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="dd92f-234">Testing single sign-on</span></span>

<span data-ttu-id="dd92f-235">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="dd92f-235">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="dd92f-236">Ha a hozzáférési panelen Panorama9 csempére kattint, akkor kell beolvasása automatikusan bejelentkezett Panorama9 alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="dd92f-236">When you click the Panorama9 tile in the Access Panel, you should get automatically signed-on to Panorama9 application.</span></span>
<span data-ttu-id="dd92f-237">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="dd92f-237">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="dd92f-238">További források</span><span class="sxs-lookup"><span data-stu-id="dd92f-238">Additional resources</span></span>

* [<span data-ttu-id="dd92f-239">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="dd92f-239">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="dd92f-240">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="dd92f-240">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-panorama9-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-panorama9-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-panorama9-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-panorama9-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-panorama9-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-panorama9-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-panorama9-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-panorama9-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-panorama9-tutorial/tutorial_general_203.png

