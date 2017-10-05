---
title: "Oktatóanyag: Azure Active Directoryval integrált Syncplicity |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Syncplicity között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 896a3211-f368-46d7-95b8-e4768c23be08
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: 1321fa71bcd625d6ea754432bfb402d3919e38f3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-syncplicity"></a><span data-ttu-id="bf050-103">Oktatóanyag: Azure Active Directoryval integrált Syncplicity</span><span class="sxs-lookup"><span data-stu-id="bf050-103">Tutorial: Azure Active Directory integration with Syncplicity</span></span>

<span data-ttu-id="bf050-104">Ebben az oktatóanyagban elsajátíthatja Syncplicity integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="bf050-104">In this tutorial, you learn how to integrate Syncplicity with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="bf050-105">Syncplicity integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="bf050-105">Integrating Syncplicity with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="bf050-106">Megadhatja a Syncplicity hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="bf050-106">You can control in Azure AD who has access to Syncplicity</span></span>
- <span data-ttu-id="bf050-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett Syncplicity (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="bf050-107">You can enable your users to automatically get signed-on to Syncplicity (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="bf050-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="bf050-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="bf050-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="bf050-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bf050-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="bf050-110">Prerequisites</span></span>

<span data-ttu-id="bf050-111">Konfigurálása az Azure AD-integrációs Syncplicity, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="bf050-111">To configure Azure AD integration with Syncplicity, you need the following items:</span></span>

- <span data-ttu-id="bf050-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="bf050-112">An Azure AD subscription</span></span>
- <span data-ttu-id="bf050-113">Egy Syncplicity egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="bf050-113">A Syncplicity single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="bf050-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="bf050-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="bf050-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="bf050-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="bf050-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="bf050-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="bf050-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="bf050-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="bf050-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="bf050-118">Scenario description</span></span>
<span data-ttu-id="bf050-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="bf050-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="bf050-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="bf050-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="bf050-121">A gyűjteményből Syncplicity hozzáadása</span><span class="sxs-lookup"><span data-stu-id="bf050-121">Adding Syncplicity from the gallery</span></span>
2. <span data-ttu-id="bf050-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="bf050-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-syncplicity-from-the-gallery"></a><span data-ttu-id="bf050-123">A gyűjteményből Syncplicity hozzáadása</span><span class="sxs-lookup"><span data-stu-id="bf050-123">Adding Syncplicity from the gallery</span></span>
<span data-ttu-id="bf050-124">Az Azure AD integrálása a Syncplicity konfigurálásához kell hozzáadnia Syncplicity a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="bf050-124">To configure the integration of Syncplicity into Azure AD, you need to add Syncplicity from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="bf050-125">**A gyűjteményből Syncplicity hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="bf050-125">**To add Syncplicity from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="bf050-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="bf050-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="bf050-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="bf050-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="bf050-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="bf050-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="bf050-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="bf050-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="bf050-133">Írja be a keresőmezőbe, **Syncplicity**.</span><span class="sxs-lookup"><span data-stu-id="bf050-133">In the search box, type **Syncplicity**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-syncplicity-tutorial/tutorial_syncplicity_search.png)

5. <span data-ttu-id="bf050-135">Az eredmények panelen válassza ki a **Syncplicity**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="bf050-135">In the results panel, select **Syncplicity**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-syncplicity-tutorial/tutorial_syncplicity_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="bf050-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="bf050-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="bf050-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján Syncplicity</span><span class="sxs-lookup"><span data-stu-id="bf050-138">In this section, you configure and test Azure AD single sign-on with Syncplicity based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="bf050-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Syncplicity a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="bf050-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Syncplicity is to a user in Azure AD.</span></span> <span data-ttu-id="bf050-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Syncplicity közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="bf050-140">In other words, a link relationship between an Azure AD user and the related user in Syncplicity needs to be established.</span></span>

<span data-ttu-id="bf050-141">Syncplicity, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="bf050-141">In Syncplicity, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="bf050-142">Az Azure AD egyszeri bejelentkezést a Syncplicity tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="bf050-142">To configure and test Azure AD single sign-on with Syncplicity, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="bf050-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="bf050-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="bf050-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="bf050-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="bf050-145">**[Syncplicity tesztfelhasználó létrehozása](#creating-a-syncplicity-test-user)**  - való Britta Simon valami Syncplicity, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="bf050-145">**[Creating a Syncplicity test user](#creating-a-syncplicity-test-user)** - to have a counterpart of Britta Simon in Syncplicity that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="bf050-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="bf050-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="bf050-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="bf050-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="bf050-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="bf050-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="bf050-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Syncplicity alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="bf050-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Syncplicity application.</span></span>

<span data-ttu-id="bf050-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés Syncplicity, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="bf050-150">**To configure Azure AD single sign-on with Syncplicity, perform the following steps:**</span></span>

1. <span data-ttu-id="bf050-151">Az Azure portálon a a **Syncplicity** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="bf050-151">In the Azure portal, on the **Syncplicity** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="bf050-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="bf050-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-syncplicity-tutorial/tutorial_syncplicity_samlbase.png)

3. <span data-ttu-id="bf050-155">Az a **Syncplicity tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="bf050-155">On the **Syncplicity Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-syncplicity-tutorial/tutorial_syncplicity_url.png)

    <span data-ttu-id="bf050-157">a.</span><span class="sxs-lookup"><span data-stu-id="bf050-157">a.</span></span> <span data-ttu-id="bf050-158">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<companyname>.syncplicity.com`</span><span class="sxs-lookup"><span data-stu-id="bf050-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.syncplicity.com`</span></span>

    <span data-ttu-id="bf050-159">b.</span><span class="sxs-lookup"><span data-stu-id="bf050-159">b.</span></span> <span data-ttu-id="bf050-160">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://<companyname>.syncplicity.com/sp`</span><span class="sxs-lookup"><span data-stu-id="bf050-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<companyname>.syncplicity.com/sp`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="bf050-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="bf050-161">These values are not real.</span></span> <span data-ttu-id="bf050-162">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="bf050-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="bf050-163">Ügyfél [Syncplicity ügyfél-támogatási csoport](https://www.syncplicity.com/contact-us) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="bf050-163">Contact [Syncplicity Client support team](https://www.syncplicity.com/contact-us) to get these values.</span></span> 
 

4. <span data-ttu-id="bf050-164">A a **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="bf050-164">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-syncplicity-tutorial/tutorial_syncplicity_certificate.png) 

  
5. <span data-ttu-id="bf050-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="bf050-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-syncplicity-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="bf050-168">A a **Syncplicity konfigurációs** kattintson **konfigurálása Syncplicity** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="bf050-168">On the **Syncplicity Configuration** section, click **Configure Syncplicity** to open **Configure sign-on** window.</span></span> <span data-ttu-id="bf050-169">Másolás a **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="bf050-169">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-syncplicity-tutorial/tutorial_syncplicity_configure.png) 

7. <span data-ttu-id="bf050-171">Jelentkezzen be a **Syncplicity** bérlő.</span><span class="sxs-lookup"><span data-stu-id="bf050-171">Sign in to your **Syncplicity** tenant.</span></span>

8. <span data-ttu-id="bf050-172">A felső menüben kattintson a **admin**, jelölje be **beállítások**, és kattintson a **egyéni tartomány és az egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="bf050-172">In the menu on the top, click **admin**, select **settings**, and then click **Custom domain and single sign-on**.</span></span>
   
    <span data-ttu-id="bf050-173">![Syncplicity](./media/active-directory-saas-syncplicity-tutorial/ic769545.png "Syncplicity")</span><span class="sxs-lookup"><span data-stu-id="bf050-173">![Syncplicity](./media/active-directory-saas-syncplicity-tutorial/ic769545.png "Syncplicity")</span></span>

9. <span data-ttu-id="bf050-174">Az a **egyszeri bejelentkezés (SSO)** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="bf050-174">On the **Single Sign-On (SSO)** dialog page, perform the following steps:</span></span>
   
    <span data-ttu-id="bf050-175">![Egyszeri bejelentkezés \(egyszeri bejelentkezés\)](./media/active-directory-saas-syncplicity-tutorial/ic769550.png "Single Sign-On \\\(SSO\\\)")</span><span class="sxs-lookup"><span data-stu-id="bf050-175">![Single Sign-On \(SSO\)](./media/active-directory-saas-syncplicity-tutorial/ic769550.png "Single Sign-On \\\(SSO\\\)")</span></span>   

    <span data-ttu-id="bf050-176">a.</span><span class="sxs-lookup"><span data-stu-id="bf050-176">a.</span></span> <span data-ttu-id="bf050-177">Az a **egyéni tartomány** szövegmező, írja be annak a tartománynak a nevét.</span><span class="sxs-lookup"><span data-stu-id="bf050-177">In the **Custom Domain** textbox, type the name of your domain.</span></span>
  
    <span data-ttu-id="bf050-178">b.</span><span class="sxs-lookup"><span data-stu-id="bf050-178">b.</span></span> <span data-ttu-id="bf050-179">Válassza ki **engedélyezett** , **az egyszeri bejelentkezés állapot**.</span><span class="sxs-lookup"><span data-stu-id="bf050-179">Select **Enabled** as **Single Sign-On Status**.</span></span>

    <span data-ttu-id="bf050-180">c.</span><span class="sxs-lookup"><span data-stu-id="bf050-180">c.</span></span> <span data-ttu-id="bf050-181">Az a **entitásazonosító** szövegmezőhöz illessze be az értékét **SAML Entitásazonosító** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="bf050-181">In the **Entity Id** textbox, Paste the value of **SAML Entity ID** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="bf050-182">d.</span><span class="sxs-lookup"><span data-stu-id="bf050-182">d.</span></span> <span data-ttu-id="bf050-183">Az a **bejelentkezési URL-címe** szövegmező, illessze be a **SAML-alapú egyszeri bejelentkezési URL-címe** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="bf050-183">In the **Sign-in page URL** textbox, Paste the **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="bf050-184">e.</span><span class="sxs-lookup"><span data-stu-id="bf050-184">e.</span></span> <span data-ttu-id="bf050-185">Az a **kijelentkezési URL-címe** szövegmező, illessze be a **Sign-Out URL-cím** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="bf050-185">In the **Logout page URL** textbox, Paste the **Sign-Out URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="bf050-186">f.</span><span class="sxs-lookup"><span data-stu-id="bf050-186">f.</span></span> <span data-ttu-id="bf050-187">A **szolgáltató Identitástanúsítvány**, kattintson a **fájl kiválasztása**, majd töltse fel az Azure-portálról letöltött tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="bf050-187">In **Identity Provider Certificate**, click **Choose file**, and then upload the certificate which you have downloaded from the Azure portal.</span></span> 

    <span data-ttu-id="bf050-188">g.</span><span class="sxs-lookup"><span data-stu-id="bf050-188">g.</span></span> <span data-ttu-id="bf050-189">Kattintson a **módosítások mentése**.</span><span class="sxs-lookup"><span data-stu-id="bf050-189">Click **SAVE CHANGES**.</span></span>

> [!TIP]
> <span data-ttu-id="bf050-190">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="bf050-190">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="bf050-191">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="bf050-191">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="bf050-192">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="bf050-192">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="bf050-193">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="bf050-193">Creating an Azure AD test user</span></span>
<span data-ttu-id="bf050-194">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="bf050-194">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="bf050-196">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="bf050-196">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="bf050-197">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="bf050-197">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-syncplicity-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="bf050-199">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="bf050-199">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-syncplicity-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="bf050-201">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="bf050-201">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-syncplicity-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="bf050-203">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="bf050-203">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-syncplicity-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="bf050-205">a.</span><span class="sxs-lookup"><span data-stu-id="bf050-205">a.</span></span> <span data-ttu-id="bf050-206">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="bf050-206">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="bf050-207">b.</span><span class="sxs-lookup"><span data-stu-id="bf050-207">b.</span></span> <span data-ttu-id="bf050-208">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="bf050-208">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="bf050-209">c.</span><span class="sxs-lookup"><span data-stu-id="bf050-209">c.</span></span> <span data-ttu-id="bf050-210">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="bf050-210">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="bf050-211">d.</span><span class="sxs-lookup"><span data-stu-id="bf050-211">d.</span></span> <span data-ttu-id="bf050-212">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="bf050-212">Click **Create**.</span></span>
 
### <a name="creating-a-syncplicity-test-user"></a><span data-ttu-id="bf050-213">Syncplicity tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="bf050-213">Creating a Syncplicity test user</span></span>
<span data-ttu-id="bf050-214">Az AAD-felhasználókat kell jelentkezhetnek be akkor ki kell építenie Syncplicity alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="bf050-214">For AAD users to be able to sign in, they must be provisioned to Syncplicity application.</span></span> <span data-ttu-id="bf050-215">Ez a szakasz ismerteti, hogyan AAD felhasználói fiókok létrehozása a Syncplicity.</span><span class="sxs-lookup"><span data-stu-id="bf050-215">This section describes how to create AAD user accounts in Syncplicity.</span></span>

<span data-ttu-id="bf050-216">**Egy felhasználói fiókot Syncplicity kiépítéséhez, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="bf050-216">**To provision a user account to Syncplicity, perform the following steps:**</span></span>

1. <span data-ttu-id="bf050-217">Jelentkezzen be a **Syncplicity** bérlő (például: `https://company.Syncplicity.com`).</span><span class="sxs-lookup"><span data-stu-id="bf050-217">Log in to your **Syncplicity** tenant (for example: `https://company.Syncplicity.com`).</span></span>

2. <span data-ttu-id="bf050-218">Kattintson a **admin** válassza **felhasználói fiókok**.</span><span class="sxs-lookup"><span data-stu-id="bf050-218">Click **admin** and select **user accounts**.</span></span>

3. <span data-ttu-id="bf050-219">Kattintson a **hozzáadni egy felhasználót**.</span><span class="sxs-lookup"><span data-stu-id="bf050-219">Click **ADD A USER**.</span></span>
   
    <span data-ttu-id="bf050-220">![Felhasználók kezelése](./media/active-directory-saas-syncplicity-tutorial/ic769764.png "felhasználók kezelése")</span><span class="sxs-lookup"><span data-stu-id="bf050-220">![Manage Users](./media/active-directory-saas-syncplicity-tutorial/ic769764.png "Manage Users")</span></span>

4. <span data-ttu-id="bf050-221">Típus a **E-mail címről** egy AAD-fiókba rendelkezés szeretne, válassza ki **felhasználói** , **szerepkör**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="bf050-221">Type the **Email addressess** of an AAD account you want to provision, select **User** as **Role**, and then click **NEXT**.</span></span>
   
    <span data-ttu-id="bf050-222">![Fiókadatok](./media/active-directory-saas-syncplicity-tutorial/ic769765.png "fiókadatok")</span><span class="sxs-lookup"><span data-stu-id="bf050-222">![Account Information](./media/active-directory-saas-syncplicity-tutorial/ic769765.png "Account Information")</span></span>
   
    >[!NOTE]
    ><span data-ttu-id="bf050-223">Az aad-ben fióktulajdonos kap egy e-mailt hivatkozással erősítse meg, és aktiválja a fiókot.</span><span class="sxs-lookup"><span data-stu-id="bf050-223">The AAD account holder  gets an email including a link to confirm and activate the account.</span></span> 
    > 

5. <span data-ttu-id="bf050-224">Válasszon ki egy csoportot, amely az új felhasználót kell tagjává válik, és kattintson a vállalat **következő**.</span><span class="sxs-lookup"><span data-stu-id="bf050-224">Select a group in your company that your new user should become a member of, and then click **NEXT**.</span></span>
   
    <span data-ttu-id="bf050-225">![Csoporttagságát](./media/active-directory-saas-syncplicity-tutorial/ic769772.png "csoporttagságát")</span><span class="sxs-lookup"><span data-stu-id="bf050-225">![Group Membership](./media/active-directory-saas-syncplicity-tutorial/ic769772.png "Group Membership")</span></span>
   
    >[!NOTE]
    ><span data-ttu-id="bf050-226">Ha nincsenek felsorolva csoportok, kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="bf050-226">If there are no groups listed, click **NEXT**.</span></span> 
    > 

6. <span data-ttu-id="bf050-227">Jelölje ki azokat a mappákat a felhasználó Syncplicity tartozó vezérlőelem alá helyezni, és kattintson a kívánt **következő**.</span><span class="sxs-lookup"><span data-stu-id="bf050-227">Select the folders you would like to place under Syncplicity’s control on the user’s computer, and then click **NEXT**.</span></span>
   
    <span data-ttu-id="bf050-228">![Syncplicity mappák](./media/active-directory-saas-syncplicity-tutorial/ic769773.png "Syncplicity mappák")</span><span class="sxs-lookup"><span data-stu-id="bf050-228">![Syncplicity Folders](./media/active-directory-saas-syncplicity-tutorial/ic769773.png "Syncplicity Folders")</span></span>

>[!NOTE]
><span data-ttu-id="bf050-229">Bármely más Syncplicity felhasználói fiók létrehozása eszközök vagy rendelkezés AAD felhasználói fiókokhoz Syncplicity által nyújtott API-k.</span><span class="sxs-lookup"><span data-stu-id="bf050-229">You can use any other Syncplicity user account creation tools or APIs provided by Syncplicity to provision AAD user accounts.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="bf050-230">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="bf050-230">Assigning the Azure AD test user</span></span>

<span data-ttu-id="bf050-231">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Syncplicity Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="bf050-231">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Syncplicity.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="bf050-233">**Britta Simon hozzárendelése Syncplicity, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="bf050-233">**To assign Britta Simon to Syncplicity, perform the following steps:**</span></span>

1. <span data-ttu-id="bf050-234">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="bf050-234">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="bf050-236">Az alkalmazások listában válassza ki a **Syncplicity**.</span><span class="sxs-lookup"><span data-stu-id="bf050-236">In the applications list, select **Syncplicity**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-syncplicity-tutorial/tutorial_syncplicity_app.png) 

3. <span data-ttu-id="bf050-238">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="bf050-238">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="bf050-240">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="bf050-240">Click **Add** button.</span></span> <span data-ttu-id="bf050-241">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="bf050-241">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="bf050-243">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="bf050-243">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="bf050-244">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="bf050-244">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="bf050-245">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="bf050-245">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="bf050-246">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="bf050-246">Testing single sign-on</span></span>

<span data-ttu-id="bf050-247">Ez a szakasz célja tesztelése az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen.</span><span class="sxs-lookup"><span data-stu-id="bf050-247">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="bf050-248">Ha a hozzáférési panelen Syncplicity csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az Syncplicity alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="bf050-248">When you click the Syncplicity tile in the Access Panel, you should get automatically signed-on to your Syncplicity application.</span></span>
## <a name="additional-resources"></a><span data-ttu-id="bf050-249">További források</span><span class="sxs-lookup"><span data-stu-id="bf050-249">Additional resources</span></span>

* [<span data-ttu-id="bf050-250">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="bf050-250">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="bf050-251">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="bf050-251">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_203.png

