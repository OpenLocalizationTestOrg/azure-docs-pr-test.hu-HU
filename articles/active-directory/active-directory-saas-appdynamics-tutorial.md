---
title: "Oktatóanyag: Azure Active Directoryval integrált AppDynamics |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és AppDynamics között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 25fd1df0-411c-4f55-8be3-4273b543100f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: 634e68bdb937eba68b27b824dc62fe2677e24ffe
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-appdynamics"></a><span data-ttu-id="0ce18-103">Oktatóanyag: Azure Active Directoryval integrált AppDynamics</span><span class="sxs-lookup"><span data-stu-id="0ce18-103">Tutorial: Azure Active Directory integration with AppDynamics</span></span>

<span data-ttu-id="0ce18-104">Ebben az oktatóanyagban elsajátíthatja AppDynamics integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="0ce18-104">In this tutorial, you learn how to integrate AppDynamics with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="0ce18-105">AppDynamics integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="0ce18-105">Integrating AppDynamics with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="0ce18-106">Megadhatja a AppDynamics hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="0ce18-106">You can control in Azure AD who has access to AppDynamics</span></span>
- <span data-ttu-id="0ce18-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett AppDynamics (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="0ce18-107">You can enable your users to automatically get signed-on to AppDynamics (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="0ce18-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="0ce18-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="0ce18-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="0ce18-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0ce18-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="0ce18-110">Prerequisites</span></span>

<span data-ttu-id="0ce18-111">Konfigurálása az Azure AD-integrációs AppDynamics, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="0ce18-111">To configure Azure AD integration with AppDynamics, you need the following items:</span></span>

- <span data-ttu-id="0ce18-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="0ce18-112">An Azure AD subscription</span></span>
- <span data-ttu-id="0ce18-113">Egy AppDynamics egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="0ce18-113">An AppDynamics single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="0ce18-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="0ce18-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="0ce18-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="0ce18-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="0ce18-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="0ce18-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="0ce18-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="0ce18-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="0ce18-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="0ce18-118">Scenario description</span></span>
<span data-ttu-id="0ce18-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="0ce18-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="0ce18-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="0ce18-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="0ce18-121">A gyűjteményből AppDynamics hozzáadása</span><span class="sxs-lookup"><span data-stu-id="0ce18-121">Adding AppDynamics from the gallery</span></span>
2. <span data-ttu-id="0ce18-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="0ce18-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-appdynamics-from-the-gallery"></a><span data-ttu-id="0ce18-123">A gyűjteményből AppDynamics hozzáadása</span><span class="sxs-lookup"><span data-stu-id="0ce18-123">Adding AppDynamics from the gallery</span></span>
<span data-ttu-id="0ce18-124">Az Azure AD integrálása a AppDynamics konfigurálásához kell hozzáadnia AppDynamics a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="0ce18-124">To configure the integration of AppDynamics into Azure AD, you need to add AppDynamics from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="0ce18-125">**A gyűjteményből AppDynamics hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="0ce18-125">**To add AppDynamics from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="0ce18-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="0ce18-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="0ce18-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="0ce18-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="0ce18-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="0ce18-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="0ce18-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="0ce18-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="0ce18-133">Írja be a keresőmezőbe, **AppDynamics**.</span><span class="sxs-lookup"><span data-stu-id="0ce18-133">In the search box, type **AppDynamics**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-appdynamics-tutorial/tutorial_appdynamics_search.png)

5. <span data-ttu-id="0ce18-135">Az eredmények panelen válassza ki a **AppDynamics**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="0ce18-135">In the results panel, select **AppDynamics**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-appdynamics-tutorial/tutorial_appdynamics_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="0ce18-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="0ce18-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="0ce18-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján AppDynamics</span><span class="sxs-lookup"><span data-stu-id="0ce18-138">In this section, you configure and test Azure AD single sign-on with AppDynamics based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="0ce18-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó AppDynamics a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="0ce18-139">For single sign-on to work, Azure AD needs to know what the counterpart user in AppDynamics is to a user in Azure AD.</span></span> <span data-ttu-id="0ce18-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a AppDynamics közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="0ce18-140">In other words, a link relationship between an Azure AD user and the related user in AppDynamics needs to be established.</span></span>

<span data-ttu-id="0ce18-141">AppDynamics, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="0ce18-141">In AppDynamics, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="0ce18-142">Az Azure AD egyszeri bejelentkezést a AppDynamics tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="0ce18-142">To configure and test Azure AD single sign-on with AppDynamics, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="0ce18-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="0ce18-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="0ce18-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="0ce18-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="0ce18-145">**[Egy AppDynamics tesztfelhasználó létrehozása](#creating-an-appdynamics-test-user)**  - való Britta Simon valami AppDynamics, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="0ce18-145">**[Creating an AppDynamics test user](#creating-an-appdynamics-test-user)** - to have a counterpart of Britta Simon in AppDynamics that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="0ce18-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="0ce18-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="0ce18-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="0ce18-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="0ce18-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="0ce18-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="0ce18-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az AppDynamics alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="0ce18-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your AppDynamics application.</span></span>

<span data-ttu-id="0ce18-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés AppDynamics, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="0ce18-150">**To configure Azure AD single sign-on with AppDynamics, perform the following steps:**</span></span>

1. <span data-ttu-id="0ce18-151">Az Azure portálon a a **AppDynamics** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="0ce18-151">In the Azure portal, on the **AppDynamics** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="0ce18-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="0ce18-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-appdynamics-tutorial/tutorial_appdynamics_samlbase.png)

3. <span data-ttu-id="0ce18-155">Az a **AppDynamics tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="0ce18-155">On the **AppDynamics Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-appdynamics-tutorial/tutorial_appdynamics_url.png)

    <span data-ttu-id="0ce18-157">a.</span><span class="sxs-lookup"><span data-stu-id="0ce18-157">a.</span></span> <span data-ttu-id="0ce18-158">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<companyname>.saas.appdynamics.com`</span><span class="sxs-lookup"><span data-stu-id="0ce18-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.saas.appdynamics.com`</span></span>

    <span data-ttu-id="0ce18-159">b.</span><span class="sxs-lookup"><span data-stu-id="0ce18-159">b.</span></span> <span data-ttu-id="0ce18-160">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://<companyname>.saas.appdynamics.com/controller`</span><span class="sxs-lookup"><span data-stu-id="0ce18-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<companyname>.saas.appdynamics.com/controller`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="0ce18-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="0ce18-161">These values are not real.</span></span> <span data-ttu-id="0ce18-162">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="0ce18-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="0ce18-163">Ügyfél [AppDynamics ügyfél-támogatási csoport](https://www.appdynamics.com/support/) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="0ce18-163">Contact [AppDynamics Client support team](https://www.appdynamics.com/support/) to get these values.</span></span> 
 
4. <span data-ttu-id="0ce18-164">A a **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="0ce18-164">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-appdynamics-tutorial/tutorial_appdynamics_certificate.png) 

5. <span data-ttu-id="0ce18-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="0ce18-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-appdynamics-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="0ce18-168">A a **AppDynamics konfigurációs** kattintson **konfigurálása AppDynamics** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="0ce18-168">On the **AppDynamics Configuration** section, click **Configure AppDynamics** to open **Configure sign-on** window.</span></span> <span data-ttu-id="0ce18-169">Másolás a **Sign-Out URL-címet, és a SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="0ce18-169">Copy the **Sign-Out URL, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-appdynamics-tutorial/tutorial_appdynamics_configure.png) 

7. <span data-ttu-id="0ce18-171">Egy másik webes böngészőablakban jelentkezzen be a AppDynamics vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="0ce18-171">In a different web browser window, log in to your AppDynamics company site as an administrator.</span></span>

8. <span data-ttu-id="0ce18-172">A felső eszköztáron kattintson **beállítások**, és kattintson a **felügyeleti**.</span><span class="sxs-lookup"><span data-stu-id="0ce18-172">In the toolbar on the top, click **Settings**, and then click **Administration**.</span></span>
   
    <span data-ttu-id="0ce18-173">![Felügyeleti](./media/active-directory-saas-appdynamics-tutorial/ic790216.png "felügyeleti")</span><span class="sxs-lookup"><span data-stu-id="0ce18-173">![Administration](./media/active-directory-saas-appdynamics-tutorial/ic790216.png "Administration")</span></span>

9. <span data-ttu-id="0ce18-174">Kattintson a **hitelesítési szolgáltató** fülre.</span><span class="sxs-lookup"><span data-stu-id="0ce18-174">Click the **Authentication Provider** tab.</span></span>
   
    <span data-ttu-id="0ce18-175">![Hitelesítésszolgáltató](./media/active-directory-saas-appdynamics-tutorial/ic790224.png "hitelesítési szolgáltató")</span><span class="sxs-lookup"><span data-stu-id="0ce18-175">![Authentication Provider](./media/active-directory-saas-appdynamics-tutorial/ic790224.png "Authentication Provider")</span></span>

10. <span data-ttu-id="0ce18-176">Az a **hitelesítési szolgáltató** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="0ce18-176">In the **Authentication Provider** section, perform the following steps:</span></span>
   
    <span data-ttu-id="0ce18-177">![SAML-alapú konfigurációs](./media/active-directory-saas-appdynamics-tutorial/ic790225.png "SAML-konfigurációja")</span><span class="sxs-lookup"><span data-stu-id="0ce18-177">![SAML Configuration](./media/active-directory-saas-appdynamics-tutorial/ic790225.png "SAML Configuration")</span></span>   

    <span data-ttu-id="0ce18-178">a.</span><span class="sxs-lookup"><span data-stu-id="0ce18-178">a.</span></span> <span data-ttu-id="0ce18-179">Mint **hitelesítési szolgáltató**, jelölje be **SAML**.</span><span class="sxs-lookup"><span data-stu-id="0ce18-179">As **Authentication Provider**, select **SAML**.</span></span>

    <span data-ttu-id="0ce18-180">b.</span><span class="sxs-lookup"><span data-stu-id="0ce18-180">b.</span></span> <span data-ttu-id="0ce18-181">Az a **bejelentkezési URL-cím** szövegmezőhöz illessze be az értékét **SAML-alapú egyszeri bejelentkezési URL-címe** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="0ce18-181">In the **Login URL** textbox, paste the value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="0ce18-182">c.</span><span class="sxs-lookup"><span data-stu-id="0ce18-182">c.</span></span> <span data-ttu-id="0ce18-183">Az a **kijelentkezési URL-cím** szövegmezőhöz illessze be az értékét **Sign-Out URL-cím** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="0ce18-183">In the **Logout URL** textbox, paste the value of **Sign-Out URL** which you have copied from Azure portal.</span></span>
       
    <span data-ttu-id="0ce18-184">d.</span><span class="sxs-lookup"><span data-stu-id="0ce18-184">d.</span></span> <span data-ttu-id="0ce18-185">Nyissa meg a base-64 kódolású tanúsítvány a Jegyzettömbben, a tartalmának másolása a vágólapra és illessze be azt a **tanúsítvány** szövegmező</span><span class="sxs-lookup"><span data-stu-id="0ce18-185">Open your base-64 encoded certificate in notepad, copy the content of it into your clipboard, and then paste it to the **Certificate** textbox</span></span>

    <span data-ttu-id="0ce18-186">e.</span><span class="sxs-lookup"><span data-stu-id="0ce18-186">e.</span></span> <span data-ttu-id="0ce18-187">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="0ce18-187">Click **Save**.</span></span>

     <span data-ttu-id="0ce18-188">![Mentés](./media/active-directory-saas-appdynamics-tutorial/ic777673.png "mentése")</span><span class="sxs-lookup"><span data-stu-id="0ce18-188">![Save](./media/active-directory-saas-appdynamics-tutorial/ic777673.png "Save")</span></span>

> [!TIP]
> <span data-ttu-id="0ce18-189">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="0ce18-189">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="0ce18-190">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="0ce18-190">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="0ce18-191">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="0ce18-191">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="0ce18-192">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="0ce18-192">Creating an Azure AD test user</span></span>
<span data-ttu-id="0ce18-193">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="0ce18-193">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="0ce18-195">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="0ce18-195">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="0ce18-196">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="0ce18-196">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-appdynamics-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="0ce18-198">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="0ce18-198">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-appdynamics-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="0ce18-200">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="0ce18-200">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-appdynamics-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="0ce18-202">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="0ce18-202">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-appdynamics-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="0ce18-204">a.</span><span class="sxs-lookup"><span data-stu-id="0ce18-204">a.</span></span> <span data-ttu-id="0ce18-205">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="0ce18-205">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="0ce18-206">b.</span><span class="sxs-lookup"><span data-stu-id="0ce18-206">b.</span></span> <span data-ttu-id="0ce18-207">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="0ce18-207">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="0ce18-208">c.</span><span class="sxs-lookup"><span data-stu-id="0ce18-208">c.</span></span> <span data-ttu-id="0ce18-209">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="0ce18-209">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="0ce18-210">d.</span><span class="sxs-lookup"><span data-stu-id="0ce18-210">d.</span></span> <span data-ttu-id="0ce18-211">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="0ce18-211">Click **Create**.</span></span>
 
### <a name="creating-an-appdynamics-test-user"></a><span data-ttu-id="0ce18-212">Egy AppDynamics tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="0ce18-212">Creating an AppDynamics test user</span></span>

<span data-ttu-id="0ce18-213">Ahhoz, hogy az Azure AD-felhasználók AppDynamics bejelentkezni, akkor ki kell építenie a AppDynamics.</span><span class="sxs-lookup"><span data-stu-id="0ce18-213">To enable Azure AD users to log in to AppDynamics, they must be provisioned into AppDynamics.</span></span> <span data-ttu-id="0ce18-214">AppDynamics, ha egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="0ce18-214">In the case of AppDynamics, provisioning is a manual task.</span></span>

<span data-ttu-id="0ce18-215">**Adja meg a felhasználók átadása, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="0ce18-215">**To configure user provisioning, perform the following steps:**</span></span>

1. <span data-ttu-id="0ce18-216">Jelentkezzen be rendszergazdaként a AppDynamics vállalati webhely.</span><span class="sxs-lookup"><span data-stu-id="0ce18-216">Log in to your AppDynamics company site as an administrator.</span></span>

2. <span data-ttu-id="0ce18-217">Ugrás a **felhasználók**, és kattintson a  **+**  megnyitásához a **felhasználó létrehozása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="0ce18-217">Go to **Users**, and then click **+** to open the **Create User** dialog.</span></span>
   
    <span data-ttu-id="0ce18-218">![Felhasználók](./media/active-directory-saas-appdynamics-tutorial/ic790229.png "felhasználók")</span><span class="sxs-lookup"><span data-stu-id="0ce18-218">![Users](./media/active-directory-saas-appdynamics-tutorial/ic790229.png "Users")</span></span>

3. <span data-ttu-id="0ce18-219">Az a **felhasználó létrehozása** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="0ce18-219">In the **Create User** section, perform the following steps:</span></span>
   
    <span data-ttu-id="0ce18-220">![Hozzon létre felhasználói](./media/active-directory-saas-appdynamics-tutorial/ic790230.png "felhasználó létrehozása")</span><span class="sxs-lookup"><span data-stu-id="0ce18-220">![Create User](./media/active-directory-saas-appdynamics-tutorial/ic790230.png "Create User")</span></span>
   
    <span data-ttu-id="0ce18-221">a.</span><span class="sxs-lookup"><span data-stu-id="0ce18-221">a.</span></span> <span data-ttu-id="0ce18-222">Típus a **felhasználónév**, **neve**, **E-mail**, **új jelszó**, **új jelszót ismételje meg a** szeretné azokat a kapcsolódó szövegmezők rendelkezés érvényes AAD-fiók.</span><span class="sxs-lookup"><span data-stu-id="0ce18-222">Type the **Username**, **Name**, **Email**, **New Password**, **Repeat New Password** of a valid AAD account you want to provision into the related textboxes.</span></span>

    <span data-ttu-id="0ce18-223">b.</span><span class="sxs-lookup"><span data-stu-id="0ce18-223">b.</span></span> <span data-ttu-id="0ce18-224">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="0ce18-224">Click **Save**.</span></span>

    >[!NOTE]
    ><span data-ttu-id="0ce18-225">Bármely más AppDynamics felhasználói fiók létrehozása eszközök vagy AppDynamics kiépíteni az Azure AD-felhasználói fiókok által nyújtott API-k.</span><span class="sxs-lookup"><span data-stu-id="0ce18-225">You can use any other AppDynamics user account creation tools or APIs provided by AppDynamics to provision Azure AD user accounts.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="0ce18-226">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="0ce18-226">Assigning the Azure AD test user</span></span>

<span data-ttu-id="0ce18-227">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés AppDynamics Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="0ce18-227">In this section, you enable Britta Simon to use Azure single sign-on by granting access to AppDynamics.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="0ce18-229">**Britta Simon hozzárendelése AppDynamics, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="0ce18-229">**To assign Britta Simon to AppDynamics, perform the following steps:**</span></span>

1. <span data-ttu-id="0ce18-230">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="0ce18-230">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="0ce18-232">Az alkalmazások listában válassza ki a **AppDynamics**.</span><span class="sxs-lookup"><span data-stu-id="0ce18-232">In the applications list, select **AppDynamics**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-appdynamics-tutorial/tutorial_appdynamics_app.png) 

3. <span data-ttu-id="0ce18-234">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="0ce18-234">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="0ce18-236">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="0ce18-236">Click **Add** button.</span></span> <span data-ttu-id="0ce18-237">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="0ce18-237">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="0ce18-239">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="0ce18-239">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="0ce18-240">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="0ce18-240">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="0ce18-241">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="0ce18-241">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="0ce18-242">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="0ce18-242">Testing single sign-on</span></span>

<span data-ttu-id="0ce18-243">Ez a szakasz célja tesztelése az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen.</span><span class="sxs-lookup"><span data-stu-id="0ce18-243">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="0ce18-244">Ha a hozzáférési panelen AppDynamics csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az AppDynamics alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="0ce18-244">When you click the AppDynamics tile in the Access Panel, you should get automatically signed-on to your AppDynamics application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0ce18-245">További források</span><span class="sxs-lookup"><span data-stu-id="0ce18-245">Additional resources</span></span>

* [<span data-ttu-id="0ce18-246">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="0ce18-246">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="0ce18-247">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="0ce18-247">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-appdynamics-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-appdynamics-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-appdynamics-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-appdynamics-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-appdynamics-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-appdynamics-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-appdynamics-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-appdynamics-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-appdynamics-tutorial/tutorial_general_203.png

