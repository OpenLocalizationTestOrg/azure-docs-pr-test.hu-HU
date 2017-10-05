---
title: "Oktatóanyag: Azure Active Directoryval integrált ITRP |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és ITRP között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: e09716a3-4200-4853-9414-2390e6c10d98
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: jeedes
ms.openlocfilehash: fae1c7b6b0e04c1e23123d3aee7913cb3131e645
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-itrp"></a><span data-ttu-id="66b57-103">Oktatóanyag: Azure Active Directoryval integrált ITRP</span><span class="sxs-lookup"><span data-stu-id="66b57-103">Tutorial: Azure Active Directory integration with ITRP</span></span>

<span data-ttu-id="66b57-104">Ebben az oktatóanyagban elsajátíthatja ITRP integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="66b57-104">In this tutorial, you learn how to integrate ITRP with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="66b57-105">ITRP integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="66b57-105">Integrating ITRP with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="66b57-106">Megadhatja a ITRP hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="66b57-106">You can control in Azure AD who has access to ITRP</span></span>
- <span data-ttu-id="66b57-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett ITRP (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="66b57-107">You can enable your users to automatically get signed-on to ITRP (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="66b57-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="66b57-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="66b57-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="66b57-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="66b57-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="66b57-110">Prerequisites</span></span>

<span data-ttu-id="66b57-111">Konfigurálása az Azure AD-integrációs ITRP, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="66b57-111">To configure Azure AD integration with ITRP, you need the following items:</span></span>

- <span data-ttu-id="66b57-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="66b57-112">An Azure AD subscription</span></span>
- <span data-ttu-id="66b57-113">Egy ITRP egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="66b57-113">An ITRP single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="66b57-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="66b57-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="66b57-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="66b57-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="66b57-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="66b57-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="66b57-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="66b57-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="66b57-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="66b57-118">Scenario description</span></span>
<span data-ttu-id="66b57-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="66b57-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="66b57-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="66b57-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="66b57-121">A gyűjteményből ITRP hozzáadása</span><span class="sxs-lookup"><span data-stu-id="66b57-121">Adding ITRP from the gallery</span></span>
2. <span data-ttu-id="66b57-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="66b57-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-itrp-from-the-gallery"></a><span data-ttu-id="66b57-123">A gyűjteményből ITRP hozzáadása</span><span class="sxs-lookup"><span data-stu-id="66b57-123">Adding ITRP from the gallery</span></span>
<span data-ttu-id="66b57-124">ITRP az Azure ad integrálása konfigurálásához szüksége ITRP hozzáadása a kezelt SaaS-alkalmazások listáját a gyűjteményből.</span><span class="sxs-lookup"><span data-stu-id="66b57-124">To configure the integration of ITRP in to Azure AD, you need to add ITRP from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="66b57-125">**A gyűjteményből ITRP hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="66b57-125">**To add ITRP from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="66b57-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="66b57-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="66b57-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="66b57-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="66b57-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="66b57-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="66b57-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="66b57-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="66b57-133">Írja be a keresőmezőbe, **ITRP**.</span><span class="sxs-lookup"><span data-stu-id="66b57-133">In the search box, type **ITRP**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-itrp-tutorial/tutorial_itrp_search.png)

5. <span data-ttu-id="66b57-135">Az eredmények panelen válassza ki a **ITRP**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="66b57-135">In the results panel, select **ITRP**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-itrp-tutorial/tutorial_itrp_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="66b57-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="66b57-137">Configuring and testing Azure AD single sign-on</span></span>

<span data-ttu-id="66b57-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján ITRP</span><span class="sxs-lookup"><span data-stu-id="66b57-138">In this section, you configure and test Azure AD single sign-on with ITRP based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="66b57-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó ITRP a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="66b57-139">For single sign-on to work, Azure AD needs to know what the counterpart user in ITRP is to a user in Azure AD.</span></span> <span data-ttu-id="66b57-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a ITRP közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="66b57-140">In other words, a link relationship between an Azure AD user and the related user in ITRP needs to be established.</span></span>

<span data-ttu-id="66b57-141">ITRP, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="66b57-141">In ITRP, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="66b57-142">Az Azure AD egyszeri bejelentkezést a ITRP tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="66b57-142">To configure and test Azure AD single sign-on with ITRP, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="66b57-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="66b57-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="66b57-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="66b57-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="66b57-145">**[Tesztfelhasználó létrehozása egy ITRP](#creating-an-itrp-test-user)**  - való Britta Simon valami ITRP, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="66b57-145">**[Creating an ITRP test user](#creating-an-itrp-test-user)** - to have a counterpart of Britta Simon in ITRP that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="66b57-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="66b57-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="66b57-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="66b57-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="66b57-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="66b57-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="66b57-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az ITRP alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="66b57-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your ITRP application.</span></span>

<span data-ttu-id="66b57-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés ITRP, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="66b57-150">**To configure Azure AD single sign-on with ITRP, perform the following steps:**</span></span>

1. <span data-ttu-id="66b57-151">Az Azure portálon a a **ITRP** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="66b57-151">In the Azure portal, on the **ITRP** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="66b57-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="66b57-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-itrp-tutorial/tutorial_itrp_samlbase.png)

3. <span data-ttu-id="66b57-155">Az a **ITRP tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="66b57-155">On the **ITRP Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-itrp-tutorial/tutorial_itrp_url.png)

    <span data-ttu-id="66b57-157">a.</span><span class="sxs-lookup"><span data-stu-id="66b57-157">a.</span></span> <span data-ttu-id="66b57-158">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<tenant-name>.itrp.com`</span><span class="sxs-lookup"><span data-stu-id="66b57-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<tenant-name>.itrp.com`</span></span>

    <span data-ttu-id="66b57-159">b.</span><span class="sxs-lookup"><span data-stu-id="66b57-159">b.</span></span> <span data-ttu-id="66b57-160">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://<tenant-name>.itrp.com`</span><span class="sxs-lookup"><span data-stu-id="66b57-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<tenant-name>.itrp.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="66b57-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="66b57-161">These values are not real.</span></span> <span data-ttu-id="66b57-162">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="66b57-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="66b57-163">Ügyfél [ITRP ügyfél-támogatási csoport](https://www.itrp.com/support) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="66b57-163">Contact [ITRP Client support team](https://www.itrp.com/support) to get these values.</span></span> 
 
4. <span data-ttu-id="66b57-164">Az a **SAML-aláíró tanúsítványa** szakaszban, másolja a **UJJLENYOMAT** tanúsítvány értékét.</span><span class="sxs-lookup"><span data-stu-id="66b57-164">On the **SAML Signing Certificate** section, copy the **THUMBPRINT** value of certificate.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-itrp-tutorial/tutorial_itrp_certificate.png) 

5. <span data-ttu-id="66b57-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="66b57-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-itrp-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="66b57-168">A a **ITRP konfigurációs** kattintson **konfigurálása ITRP** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="66b57-168">On the **ITRP Configuration** section, click **Configure ITRP** to open **Configure sign-on** window.</span></span> <span data-ttu-id="66b57-169">Másolás a **SAML-alapú egyszeri bejelentkezési URL-címe és Sign-Out URL-cím** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="66b57-169">Copy the **SAML Single Sign-On Service URL and Sign-Out URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-itrp-tutorial/tutorial_itrp_configure.png) 

7. <span data-ttu-id="66b57-171">Egy másik webes böngészőablakban jelentkezzen be a ITRP vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="66b57-171">In a different web browser window, log in to your ITRP company site as an administrator.</span></span>

8. <span data-ttu-id="66b57-172">A felső eszköztáron kattintson **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="66b57-172">In the toolbar on the top, click **Settings**.</span></span>
   
    <span data-ttu-id="66b57-173">![ITRP](./media/active-directory-saas-itrp-tutorial/ic775570.png "ITRP")</span><span class="sxs-lookup"><span data-stu-id="66b57-173">![ITRP](./media/active-directory-saas-itrp-tutorial/ic775570.png "ITRP")</span></span>

8. <span data-ttu-id="66b57-174">A bal oldali navigációs ablakból válassza **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="66b57-174">In the left navigation pane, select **Single Sign-On**.</span></span>
   
    <span data-ttu-id="66b57-175">![Egyszeri bejelentkezés](./media/active-directory-saas-itrp-tutorial/ic775571.png "egyszeri bejelentkezés")</span><span class="sxs-lookup"><span data-stu-id="66b57-175">![Single Sign-On](./media/active-directory-saas-itrp-tutorial/ic775571.png "Single Sign-On")</span></span>

9. <span data-ttu-id="66b57-176">Az egyszeri bejelentkezés konfigurációs szakaszban hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="66b57-176">In the Single Sign-On configuration section, perform the following steps:</span></span>
   
    <span data-ttu-id="66b57-177">![Egyszeri bejelentkezés](./media/active-directory-saas-itrp-tutorial/ic775572.png "egyszeri bejelentkezés")</span><span class="sxs-lookup"><span data-stu-id="66b57-177">![Single Sign-On](./media/active-directory-saas-itrp-tutorial/ic775572.png "Single Sign-On")</span></span>
    
    <span data-ttu-id="66b57-178">![Egyszeri bejelentkezés](./media/active-directory-saas-itrp-tutorial/ic775573.png "egyszeri bejelentkezés")</span><span class="sxs-lookup"><span data-stu-id="66b57-178">![Single Sign-On](./media/active-directory-saas-itrp-tutorial/ic775573.png "Single Sign-On")</span></span>   

    <span data-ttu-id="66b57-179">a.</span><span class="sxs-lookup"><span data-stu-id="66b57-179">a.</span></span> <span data-ttu-id="66b57-180">Kattintson a **engedélyezése**.</span><span class="sxs-lookup"><span data-stu-id="66b57-180">Click **Enable**.</span></span>

    <span data-ttu-id="66b57-181">b.</span><span class="sxs-lookup"><span data-stu-id="66b57-181">b.</span></span> <span data-ttu-id="66b57-182">A **távoli napló kijelentkezési URL-cím** szövegmezőhöz illessze be az értékét **Sign-Out URL-cím**, amely az Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="66b57-182">In **Remote Log Out URL** textbox, paste the value of **Sign-Out URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="66b57-183">c.</span><span class="sxs-lookup"><span data-stu-id="66b57-183">c.</span></span> <span data-ttu-id="66b57-184">A **SAML SSO URL-cím** szövegmezőhöz illessze be az értékét **SAML-alapú egyszeri bejelentkezési URL-címe**, amely az Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="66b57-184">In **SAML SSO URL** textbox, paste the value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="66b57-185">d.In **tanúsítvány-ujjlenyomat** szövegmező, illessze be a **ujjlenyomat** tanúsítványt, amely az Azure-portálon másolta értékét.</span><span class="sxs-lookup"><span data-stu-id="66b57-185">d.In **Certificate Fingerprint** textbox, paste the **Thumbprint** value of certificate, which you have copied from Azure portal.</span></span> 
      
10. <span data-ttu-id="66b57-186">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="66b57-186">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="66b57-187">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="66b57-187">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="66b57-188">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="66b57-188">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="66b57-189">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="66b57-189">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="66b57-190">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="66b57-190">Creating an Azure AD test user</span></span>
<span data-ttu-id="66b57-191">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="66b57-191">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="66b57-193">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="66b57-193">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="66b57-194">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="66b57-194">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-itrp-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="66b57-196">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="66b57-196">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-itrp-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="66b57-198">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="66b57-198">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-itrp-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="66b57-200">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="66b57-200">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-itrp-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="66b57-202">a.</span><span class="sxs-lookup"><span data-stu-id="66b57-202">a.</span></span> <span data-ttu-id="66b57-203">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="66b57-203">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="66b57-204">b.</span><span class="sxs-lookup"><span data-stu-id="66b57-204">b.</span></span> <span data-ttu-id="66b57-205">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="66b57-205">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="66b57-206">c.</span><span class="sxs-lookup"><span data-stu-id="66b57-206">c.</span></span> <span data-ttu-id="66b57-207">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="66b57-207">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="66b57-208">d.</span><span class="sxs-lookup"><span data-stu-id="66b57-208">d.</span></span> <span data-ttu-id="66b57-209">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="66b57-209">Click **Create**.</span></span>
 
### <a name="creating-an-itrp-test-user"></a><span data-ttu-id="66b57-210">Egy ITRP tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="66b57-210">Creating an ITRP test user</span></span>

<span data-ttu-id="66b57-211">Ahhoz, hogy az Azure AD-felhasználók ITRP bejelentkezni, akkor ki kell építenie a ITRP.</span><span class="sxs-lookup"><span data-stu-id="66b57-211">To enable Azure AD users to log in to ITRP, they must be provisioned in to ITRP.</span></span>  

<span data-ttu-id="66b57-212">ITRP, ha egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="66b57-212">In the case of ITRP, provisioning is a manual task.</span></span>

<span data-ttu-id="66b57-213">**Felhasználói fiók létrehozásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="66b57-213">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="66b57-214">Jelentkezzen be a **ITRP** bérlő.</span><span class="sxs-lookup"><span data-stu-id="66b57-214">Log in to your **ITRP** tenant.</span></span>

2. <span data-ttu-id="66b57-215">A felső eszköztáron kattintson **rekordok**.</span><span class="sxs-lookup"><span data-stu-id="66b57-215">In the toolbar on the top, click **Records**.</span></span>
   
    <span data-ttu-id="66b57-216">![Felügyeleti](./media/active-directory-saas-itrp-tutorial/ic775575.png "rendszergazda")</span><span class="sxs-lookup"><span data-stu-id="66b57-216">![Admin](./media/active-directory-saas-itrp-tutorial/ic775575.png "Admin")</span></span>

3. <span data-ttu-id="66b57-217">Válassza ki a helyi menü **személyek**.</span><span class="sxs-lookup"><span data-stu-id="66b57-217">From the popup menu, select **People**.</span></span>
   
    <span data-ttu-id="66b57-218">![Személyek](./media/active-directory-saas-itrp-tutorial/ic775587.png "személyek")</span><span class="sxs-lookup"><span data-stu-id="66b57-218">![People](./media/active-directory-saas-itrp-tutorial/ic775587.png "People")</span></span>

4. <span data-ttu-id="66b57-219">Kattintson a **adja hozzá az új személy** ("+").</span><span class="sxs-lookup"><span data-stu-id="66b57-219">Click **Add New Person** (“+”).</span></span>
   
    <span data-ttu-id="66b57-220">![Felügyeleti](./media/active-directory-saas-itrp-tutorial/ic775576.png "rendszergazda")</span><span class="sxs-lookup"><span data-stu-id="66b57-220">![Admin](./media/active-directory-saas-itrp-tutorial/ic775576.png "Admin")</span></span>

5. <span data-ttu-id="66b57-221">Új személy hozzáadása párbeszédpanelen hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="66b57-221">On the Add New Person dialog, perform the following steps:</span></span>
   
    <span data-ttu-id="66b57-222">![Felhasználói](./media/active-directory-saas-itrp-tutorial/ic775577.png "felhasználó")</span><span class="sxs-lookup"><span data-stu-id="66b57-222">![User](./media/active-directory-saas-itrp-tutorial/ic775577.png "User")</span></span> 
      
    <span data-ttu-id="66b57-223">a.</span><span class="sxs-lookup"><span data-stu-id="66b57-223">a.</span></span> <span data-ttu-id="66b57-224">Típus a **neve**, **E-mail** egy érvényes AAD-fióknevet, amelyet kiépítését.</span><span class="sxs-lookup"><span data-stu-id="66b57-224">Type the **Name**, **Email** of a valid AAD account you want to provision.</span></span>

    <span data-ttu-id="66b57-225">b.</span><span class="sxs-lookup"><span data-stu-id="66b57-225">b.</span></span> <span data-ttu-id="66b57-226">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="66b57-226">Click **Save**.</span></span>

>[!NOTE]
><span data-ttu-id="66b57-227">Bármely más ITRP felhasználói fiók létrehozása eszközök vagy rendelkezés AAD felhasználói fiókokhoz ITRP által nyújtott API-k.</span><span class="sxs-lookup"><span data-stu-id="66b57-227">You can use any other ITRP user account creation tools or APIs provided by ITRP to provision AAD user accounts.</span></span> 
> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="66b57-228">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="66b57-228">Assigning the Azure AD test user</span></span>

<span data-ttu-id="66b57-229">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés ITRP Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="66b57-229">In this section, you enable Britta Simon to use Azure single sign-on by granting access to ITRP.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="66b57-231">**Britta Simon hozzárendelése ITRP, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="66b57-231">**To assign Britta Simon to ITRP, perform the following steps:**</span></span>

1. <span data-ttu-id="66b57-232">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="66b57-232">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="66b57-234">Az alkalmazások listában válassza ki a **ITRP**.</span><span class="sxs-lookup"><span data-stu-id="66b57-234">In the applications list, select **ITRP**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-itrp-tutorial/tutorial_itrp_app.png) 

3. <span data-ttu-id="66b57-236">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="66b57-236">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="66b57-238">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="66b57-238">Click **Add** button.</span></span> <span data-ttu-id="66b57-239">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="66b57-239">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="66b57-241">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="66b57-241">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="66b57-242">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="66b57-242">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="66b57-243">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="66b57-243">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="66b57-244">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="66b57-244">Testing single sign-on</span></span>

<span data-ttu-id="66b57-245">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="66b57-245">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="66b57-246">Ha a hozzáférési panelen ITRP csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az ITRP alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="66b57-246">When you click the ITRP tile in the Access Panel, you should get automatically signed-on to your ITRP application.</span></span>
<span data-ttu-id="66b57-247">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="66b57-247">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="66b57-248">További források</span><span class="sxs-lookup"><span data-stu-id="66b57-248">Additional resources</span></span>

* [<span data-ttu-id="66b57-249">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="66b57-249">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="66b57-250">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="66b57-250">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-itrp-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-itrp-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-itrp-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-itrp-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-itrp-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-itrp-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-itrp-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-itrp-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-itrp-tutorial/tutorial_general_203.png

