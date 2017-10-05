---
title: "Oktatóanyag: Azure Active Directoryval integrált vászonra Lms |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és a vászonra LMS között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: bfed291c-a33e-410d-b919-5b965a631d45
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/08/2017
ms.author: jeedes
ms.openlocfilehash: 2212b7a81b66d1afd1aa78d1487b07b6d7b84129
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-canvas-lms"></a><span data-ttu-id="45204-103">Oktatóanyag: Azure Active Directoryval integrált vászonra LMS</span><span class="sxs-lookup"><span data-stu-id="45204-103">Tutorial: Azure Active Directory integration with Canvas LMS</span></span>

<span data-ttu-id="45204-104">Ebben az oktatóanyagban elsajátíthatja vászonra integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="45204-104">In this tutorial, you learn how to integrate Canvas with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="45204-105">Vászonra integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="45204-105">Integrating Canvas with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="45204-106">Szabályozhatja az Azure AD, aki hozzáfér a vászonra</span><span class="sxs-lookup"><span data-stu-id="45204-106">You can control in Azure AD who has access to Canvas</span></span>
- <span data-ttu-id="45204-107">Engedélyezheti a felhasználóknak, hogy automatikusan lekérni bejelentkezett vászon (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="45204-107">You can enable your users to automatically get signed-on to Canvas (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="45204-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="45204-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="45204-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="45204-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="45204-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="45204-110">Prerequisites</span></span>

<span data-ttu-id="45204-111">Az Azure AD-integráció konfigurálása a vásznon, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="45204-111">To configure Azure AD integration with Canvas, you need the following items:</span></span>

- <span data-ttu-id="45204-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="45204-112">An Azure AD subscription</span></span>
- <span data-ttu-id="45204-113">A vászon egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="45204-113">A Canvas single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="45204-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="45204-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="45204-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="45204-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="45204-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="45204-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="45204-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="45204-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="45204-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="45204-118">Scenario description</span></span>
<span data-ttu-id="45204-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="45204-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="45204-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="45204-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="45204-121">Felvétele a vászonra a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="45204-121">Adding Canvas from the gallery</span></span>
2. <span data-ttu-id="45204-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="45204-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-canvas-from-the-gallery"></a><span data-ttu-id="45204-123">Felvétele a vászonra a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="45204-123">Adding Canvas from the gallery</span></span>
<span data-ttu-id="45204-124">Az Azure AD integrálása a vászon konfigurálásához kell hozzáadnia vászonra a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="45204-124">To configure the integration of Canvas into Azure AD, you need to add Canvas from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="45204-125">**Adja hozzá a vászonra a gyűjteményből, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="45204-125">**To add Canvas from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="45204-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="45204-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="45204-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="45204-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="45204-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="45204-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="45204-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="45204-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="45204-133">Írja be a keresőmezőbe, **vászonra**.</span><span class="sxs-lookup"><span data-stu-id="45204-133">In the search box, type **Canvas**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-canvas-lms-tutorial/tutorial_canvaslms_search.png)

5. <span data-ttu-id="45204-135">Az eredmények panelen válassza ki a **vászonra**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="45204-135">In the results panel, select **Canvas**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-canvas-lms-tutorial/tutorial_canvaslms_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="45204-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="45204-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="45204-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést a vászon "Britta Simon." nevű tesztfelhasználó alapján</span><span class="sxs-lookup"><span data-stu-id="45204-138">In this section, you configure and test Azure AD single sign-on with Canvas based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="45204-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó a vászonra a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="45204-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Canvas is to a user in Azure AD.</span></span> <span data-ttu-id="45204-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a vászon közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="45204-140">In other words, a link relationship between an Azure AD user and the related user in Canvas needs to be established.</span></span>

<span data-ttu-id="45204-141">A vásznon, rendelje az értékét a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="45204-141">In Canvas, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="45204-142">Az Azure AD egyszeri bejelentkezést a vászon tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="45204-142">To configure and test Azure AD single sign-on with Canvas, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="45204-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="45204-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="45204-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="45204-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="45204-145">**[Vászonra tesztfelhasználó létrehozása](#creating-a-canvas-test-user)**  - való Britta Simon egy megfelelője a vásznon, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="45204-145">**[Creating a Canvas test user](#creating-a-canvas-test-user)** - to have a counterpart of Britta Simon in Canvas that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="45204-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="45204-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="45204-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="45204-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="45204-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="45204-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="45204-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és a vászonra alkalmazásban egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="45204-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Canvas application.</span></span>

<span data-ttu-id="45204-150">**Konfigurálja az Azure AD egyszeri bejelentkezést a vásznon, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="45204-150">**To configure Azure AD single sign-on with Canvas, perform the following steps:**</span></span>

1. <span data-ttu-id="45204-151">Az Azure portálon a a **vászonra** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="45204-151">In the Azure portal, on the **Canvas** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="45204-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="45204-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-canvas-lms-tutorial/tutorial_canvaslms_samlbase.png)

3. <span data-ttu-id="45204-155">Az a **vászonra tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="45204-155">On the **Canvas Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-canvas-lms-tutorial/tutorial_canvaslms_url.png)

    <span data-ttu-id="45204-157">a.</span><span class="sxs-lookup"><span data-stu-id="45204-157">a.</span></span> <span data-ttu-id="45204-158">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<tenant-name>.instructure.com`</span><span class="sxs-lookup"><span data-stu-id="45204-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<tenant-name>.instructure.com`</span></span>

    <span data-ttu-id="45204-159">b.</span><span class="sxs-lookup"><span data-stu-id="45204-159">b.</span></span> <span data-ttu-id="45204-160">Az a **azonosító** szövegmező, írja be az értéket a következő minta használatával:`https://<tenant-name>.instructure.com/saml2`</span><span class="sxs-lookup"><span data-stu-id="45204-160">In the **Identifier** textbox, type the value using the following pattern: `https://<tenant-name>.instructure.com/saml2`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="45204-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="45204-161">These values are not real.</span></span> <span data-ttu-id="45204-162">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="45204-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="45204-163">Ügyfél [vászonra ügyfél-támogatási csoport](https://community.canvaslms.com/community/help) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="45204-163">Contact [Canvas Client support team](https://community.canvaslms.com/community/help) to get these values.</span></span> 
 
4. <span data-ttu-id="45204-164">Az a **SAML-aláíró tanúsítványa** szakaszban, másolja a **UJJLENYOMAT** tanúsítvány értékét.</span><span class="sxs-lookup"><span data-stu-id="45204-164">On the **SAML Signing Certificate** section, copy the **THUMBPRINT** value of certificate.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-canvas-lms-tutorial/tutorial_canvaslms_certificate.png) 

5. <span data-ttu-id="45204-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="45204-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="45204-168">Az a **vászonra konfigurációs** területen kattintson **konfigurálása vászonra** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="45204-168">On the **Canvas Configuration** section, click **Configure Canvas** to open **Configure sign-on** window.</span></span> <span data-ttu-id="45204-169">Másolás a **jelszó URL-cím módosítása, Sign-Out URL-címe, SAML Entitásazonosító és SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="45204-169">Copy the **Change Password URL, Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-canvas-lms-tutorial/tutorial_canvaslms_configure.png) 
 
7. <span data-ttu-id="45204-171">Egy másik webes böngészőablakban jelentkezzen be a vászonra vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="45204-171">In a different web browser window, log in to your Canvas company site as an administrator.</span></span>

8. <span data-ttu-id="45204-172">Ugrás a **tanfolyamokat \> fiókok felügyelt \> Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="45204-172">Go to **Courses \> Managed Accounts \> Microsoft**.</span></span>
   
    <span data-ttu-id="45204-173">![Vászonra](./media/active-directory-saas-canvas-lms-tutorial/IC775990.png "vászonra")</span><span class="sxs-lookup"><span data-stu-id="45204-173">![Canvas](./media/active-directory-saas-canvas-lms-tutorial/IC775990.png "Canvas")</span></span>

9. <span data-ttu-id="45204-174">Jelölje ki a bal oldali navigációs ablaktáblán, **hitelesítési**, és kattintson a **új SAML-Config hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="45204-174">In the navigation pane on the left, select **Authentication**, and then click **Add New SAML Config**.</span></span>
   
    <span data-ttu-id="45204-175">![Hitelesítési](./media/active-directory-saas-canvas-lms-tutorial/IC775991.png "hitelesítés")</span><span class="sxs-lookup"><span data-stu-id="45204-175">![Authentication](./media/active-directory-saas-canvas-lms-tutorial/IC775991.png "Authentication")</span></span>

10. <span data-ttu-id="45204-176">Az aktuális integrációs oldalon hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="45204-176">On the Current Integration page, perform the following steps:</span></span>
   
    <span data-ttu-id="45204-177">![Aktuális integrációs](./media/active-directory-saas-canvas-lms-tutorial/IC775992.png "aktuális integráció")</span><span class="sxs-lookup"><span data-stu-id="45204-177">![Current Integration](./media/active-directory-saas-canvas-lms-tutorial/IC775992.png "Current Integration")</span></span>

    <span data-ttu-id="45204-178">a.</span><span class="sxs-lookup"><span data-stu-id="45204-178">a.</span></span> <span data-ttu-id="45204-179">A **IdP Entitásazonosító** szövegmezőhöz illessze be az értékét **SAML Entitásazonosító** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="45204-179">In **IdP Entity ID** textbox, paste the value of **SAML Entity ID** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="45204-180">b.</span><span class="sxs-lookup"><span data-stu-id="45204-180">b.</span></span> <span data-ttu-id="45204-181">A **URL-cím napló** szövegmezőhöz illessze be az értékét **SAML-alapú egyszeri bejelentkezési URL-címe** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="45204-181">In **Log On URL** textbox, paste the value of **SAML Single Sign-On Service URL** which you have copied from Azure portal .</span></span>

    <span data-ttu-id="45204-182">c.</span><span class="sxs-lookup"><span data-stu-id="45204-182">c.</span></span> <span data-ttu-id="45204-183">A **napló kijelentkezési URL-cím** szövegmezőhöz illessze be az értékét **Sign-Out URL-cím** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="45204-183">In **Log Out URL** textbox, paste the value of **Sign-Out URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="45204-184">d.</span><span class="sxs-lookup"><span data-stu-id="45204-184">d.</span></span> <span data-ttu-id="45204-185">A **módosítás jelszó hivatkozásra** szövegmezőhöz illessze be az értékét **jelszó URL-cím módosítása** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="45204-185">In **Change Password Link** textbox, paste the value of **Change Password URL** which you have copied from Azure portal.</span></span> 

    <span data-ttu-id="45204-186">e.</span><span class="sxs-lookup"><span data-stu-id="45204-186">e.</span></span> <span data-ttu-id="45204-187">A **tanúsítvány-ujjlenyomat** szövegmező, illessze be a **ujjlenyomat** érték tanúsítvány, amely az Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="45204-187">In **Certificate Fingerprint** textbox, paste the **Thumbprint** value of certificate which you have copied from Azure portal.</span></span>      
        
    <span data-ttu-id="45204-188">f.</span><span class="sxs-lookup"><span data-stu-id="45204-188">f.</span></span> <span data-ttu-id="45204-189">Az a **bejelentkezési attribútum** listáról válassza ki **NameID**.</span><span class="sxs-lookup"><span data-stu-id="45204-189">From the **Login Attribute** list, select **NameID**.</span></span>

    <span data-ttu-id="45204-190">g.</span><span class="sxs-lookup"><span data-stu-id="45204-190">g.</span></span> <span data-ttu-id="45204-191">Az a **azonosító formátuma** listáról válassza ki **emailAddress**.</span><span class="sxs-lookup"><span data-stu-id="45204-191">From the **Identifier Format** list, select **emailAddress**.</span></span>

    <span data-ttu-id="45204-192">h.</span><span class="sxs-lookup"><span data-stu-id="45204-192">h.</span></span> <span data-ttu-id="45204-193">Kattintson a **hitelesítési beállításainak mentése**.</span><span class="sxs-lookup"><span data-stu-id="45204-193">Click **Save Authentication Settings**.</span></span>

> [!TIP]
> <span data-ttu-id="45204-194">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="45204-194">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="45204-195">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="45204-195">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="45204-196">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="45204-196">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="45204-197">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="45204-197">Creating an Azure AD test user</span></span>
<span data-ttu-id="45204-198">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="45204-198">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="45204-200">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="45204-200">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="45204-201">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="45204-201">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-canvas-lms-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="45204-203">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="45204-203">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-canvas-lms-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="45204-205">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="45204-205">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-canvas-lms-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="45204-207">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="45204-207">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-canvas-lms-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="45204-209">a.</span><span class="sxs-lookup"><span data-stu-id="45204-209">a.</span></span> <span data-ttu-id="45204-210">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="45204-210">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="45204-211">b.</span><span class="sxs-lookup"><span data-stu-id="45204-211">b.</span></span> <span data-ttu-id="45204-212">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="45204-212">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="45204-213">c.</span><span class="sxs-lookup"><span data-stu-id="45204-213">c.</span></span> <span data-ttu-id="45204-214">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="45204-214">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="45204-215">d.</span><span class="sxs-lookup"><span data-stu-id="45204-215">d.</span></span> <span data-ttu-id="45204-216">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="45204-216">Click **Create**.</span></span>
 
### <a name="creating-a-canvas-test-user"></a><span data-ttu-id="45204-217">Vászonra tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="45204-217">Creating a Canvas test user</span></span>

<span data-ttu-id="45204-218">Ahhoz, hogy az Azure AD-felhasználók jelentkezzen be a vászonra, akkor ki kell építenie a vászonra.</span><span class="sxs-lookup"><span data-stu-id="45204-218">To enable Azure AD users to log in to Canvas, they must be provisioned into Canvas.</span></span>

<span data-ttu-id="45204-219">Esetén a vásznon a felhasználók átadása kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="45204-219">In case of Canvas, user provisioning is a manual task.</span></span>

<span data-ttu-id="45204-220">**Felhasználói fiók létrehozásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="45204-220">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="45204-221">Jelentkezzen be a **vászonra** bérlő.</span><span class="sxs-lookup"><span data-stu-id="45204-221">Log in to your **Canvas** tenant.</span></span>

2. <span data-ttu-id="45204-222">Ugrás a **tanfolyamokat \> fiókok felügyelt \> Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="45204-222">Go to **Courses \> Managed Accounts \> Microsoft**.</span></span>
   
   <span data-ttu-id="45204-223">![Vászonra](./media/active-directory-saas-canvas-lms-tutorial/IC775990.png "vászonra")</span><span class="sxs-lookup"><span data-stu-id="45204-223">![Canvas](./media/active-directory-saas-canvas-lms-tutorial/IC775990.png "Canvas")</span></span>

3. <span data-ttu-id="45204-224">Kattintson a **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="45204-224">Click **Users**.</span></span>
   
   <span data-ttu-id="45204-225">![Felhasználók](./media/active-directory-saas-canvas-lms-tutorial/IC775995.png "felhasználók")</span><span class="sxs-lookup"><span data-stu-id="45204-225">![Users](./media/active-directory-saas-canvas-lms-tutorial/IC775995.png "Users")</span></span>

4. <span data-ttu-id="45204-226">Kattintson a **új felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="45204-226">Click **Add New User**.</span></span>
   
   <span data-ttu-id="45204-227">![Felhasználók](./media/active-directory-saas-canvas-lms-tutorial/IC775996.png "felhasználók")</span><span class="sxs-lookup"><span data-stu-id="45204-227">![Users](./media/active-directory-saas-canvas-lms-tutorial/IC775996.png "Users")</span></span>

5. <span data-ttu-id="45204-228">A hozzáadása egy új felhasználó párbeszédpanel lap hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="45204-228">On the Add a New User dialog page, perform the following steps:</span></span>
   
   <span data-ttu-id="45204-229">![Felhasználó hozzáadása](./media/active-directory-saas-canvas-lms-tutorial/IC775997.png "felhasználó hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="45204-229">![Add User](./media/active-directory-saas-canvas-lms-tutorial/IC775997.png "Add User")</span></span>
   
   <span data-ttu-id="45204-230">a.</span><span class="sxs-lookup"><span data-stu-id="45204-230">a.</span></span> <span data-ttu-id="45204-231">Az a **teljes nevét** szövegmező, írja be például a felhasználó nevét **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="45204-231">In the **Full Name** textbox, enter the name of user like **BrittaSimon**.</span></span>

   <span data-ttu-id="45204-232">b.</span><span class="sxs-lookup"><span data-stu-id="45204-232">b.</span></span> <span data-ttu-id="45204-233">Az a **E-mail** szövegmező, adja meg az e-mail címét, például a felhasználó  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="45204-233">In the **Email** textbox, enter the email of user like **brittasimon@contoso.com**.</span></span>

   <span data-ttu-id="45204-234">c.</span><span class="sxs-lookup"><span data-stu-id="45204-234">c.</span></span> <span data-ttu-id="45204-235">Az a **bejelentkezési** szövegmező, írja be a felhasználó az Azure AD e-mail címét például  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="45204-235">In the **Login** textbox, enter the user’s Azure AD email address like **brittasimon@contoso.com**.</span></span>

   <span data-ttu-id="45204-236">d.</span><span class="sxs-lookup"><span data-stu-id="45204-236">d.</span></span> <span data-ttu-id="45204-237">Válassza ki **E-mail a felhasználó a fiók létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="45204-237">Select **Email the user about this account creation**.</span></span>

   <span data-ttu-id="45204-238">e.</span><span class="sxs-lookup"><span data-stu-id="45204-238">e.</span></span> <span data-ttu-id="45204-239">Kattintson a **felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="45204-239">Click **Add User**.</span></span>

>[!NOTE]
><span data-ttu-id="45204-240">Bármely más vászonra felhasználói fiók létrehozása eszközök, vagy rendelkezés AAD felhasználói fiókokhoz vászonra által nyújtott API-k.</span><span class="sxs-lookup"><span data-stu-id="45204-240">You can use any other Canvas user account creation tools or APIs provided by Canvas to provision AAD user accounts.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="45204-241">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="45204-241">Assigning the Azure AD test user</span></span>

<span data-ttu-id="45204-242">Ebben a szakaszban engedélyezze Britta Simon Azure egyszeri bejelentkezéshez használandó hozzáférés biztosítása a vászonra.</span><span class="sxs-lookup"><span data-stu-id="45204-242">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Canvas.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="45204-244">**Britta Simon hozzárendelése a vásznon, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="45204-244">**To assign Britta Simon to Canvas, perform the following steps:**</span></span>

1. <span data-ttu-id="45204-245">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="45204-245">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="45204-247">Az alkalmazások listában válassza ki a **vászonra**.</span><span class="sxs-lookup"><span data-stu-id="45204-247">In the applications list, select **Canvas**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-canvas-lms-tutorial/tutorial_canvaslms_app.png) 

3. <span data-ttu-id="45204-249">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="45204-249">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="45204-251">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="45204-251">Click **Add** button.</span></span> <span data-ttu-id="45204-252">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="45204-252">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="45204-254">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="45204-254">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="45204-255">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="45204-255">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="45204-256">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="45204-256">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="45204-257">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="45204-257">Testing single sign-on</span></span>

<span data-ttu-id="45204-258">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="45204-258">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="45204-259">A hozzáférési panelen a vászon csempére kattintva, meg kell beolvasni automatikusan bejelentkezett a vászon alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="45204-259">When you click the Canvas tile in the Access Panel, you should get automatically signed-on to your Canvas application.</span></span>
<span data-ttu-id="45204-260">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="45204-260">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="45204-261">További források</span><span class="sxs-lookup"><span data-stu-id="45204-261">Additional resources</span></span>

* [<span data-ttu-id="45204-262">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="45204-262">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="45204-263">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="45204-263">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_203.png

