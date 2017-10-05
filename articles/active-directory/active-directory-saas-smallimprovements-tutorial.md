---
title: "Oktatóanyag: Azure Active Directory-integráció kis javításait |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és kis fejlesztései között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 59c8a112-41e1-4337-9ef3-3d7029780d61
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 49a8cd3acfc6df15ef6a51171c8421162bc94efc
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-small-improvements"></a><span data-ttu-id="94b5d-103">Oktatóanyag: Azure Active Directoryval integrált kis fejlesztései</span><span class="sxs-lookup"><span data-stu-id="94b5d-103">Tutorial: Azure Active Directory integration with Small Improvements</span></span>

<span data-ttu-id="94b5d-104">Ebben az oktatóanyagban elsajátíthatja kis fejlesztései integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="94b5d-104">In this tutorial, you learn how to integrate Small Improvements with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="94b5d-105">Kis fejlesztései integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="94b5d-105">Integrating Small Improvements with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="94b5d-106">Szabályozhatja az Azure AD, aki hozzáférhet kis fejlesztései</span><span class="sxs-lookup"><span data-stu-id="94b5d-106">You can control in Azure AD who has access to Small Improvements</span></span>
- <span data-ttu-id="94b5d-107">Az Azure AD-fiókok a engedélyezheti a felhasználóknak, hogy automatikusan lekérni aláírt a kis fejlesztései (egyszeri bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="94b5d-107">You can enable your users to automatically get signed-on to Small Improvements (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="94b5d-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="94b5d-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="94b5d-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="94b5d-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="94b5d-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="94b5d-110">Prerequisites</span></span>

<span data-ttu-id="94b5d-111">Az Azure AD-integrációs konfigurálásához kis javításait, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="94b5d-111">To configure Azure AD integration with Small Improvements, you need the following items:</span></span>

- <span data-ttu-id="94b5d-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="94b5d-112">An Azure AD subscription</span></span>
- <span data-ttu-id="94b5d-113">Egy kis fejlesztései az egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="94b5d-113">A Small Improvements single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="94b5d-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="94b5d-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="94b5d-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="94b5d-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="94b5d-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="94b5d-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="94b5d-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió Itt kaphat [próbaverzió ajánlat](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="94b5d-117">If you don't have an Azure AD trial environment, you can get a one-month trial here [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="94b5d-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="94b5d-118">Scenario description</span></span>
<span data-ttu-id="94b5d-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="94b5d-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="94b5d-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="94b5d-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="94b5d-121">Kis fejlesztései hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="94b5d-121">Adding Small Improvements from the gallery</span></span>
2. <span data-ttu-id="94b5d-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="94b5d-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-small-improvements-from-the-gallery"></a><span data-ttu-id="94b5d-123">Kis fejlesztései hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="94b5d-123">Adding Small Improvements from the gallery</span></span>
<span data-ttu-id="94b5d-124">Az Azure AD integrálása kisméretű fejlesztései konfigurálásához kell hozzáadnia kis fejlesztései a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="94b5d-124">To configure the integration of Small Improvements into Azure AD, you need to add Small Improvements from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="94b5d-125">**Adja hozzá a kis fejlesztései a gyűjteményből, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="94b5d-125">**To add Small Improvements from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="94b5d-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="94b5d-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="94b5d-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="94b5d-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="94b5d-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="94b5d-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="94b5d-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="94b5d-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="94b5d-133">Írja be a keresőmezőbe, **kis fejlesztései**.</span><span class="sxs-lookup"><span data-stu-id="94b5d-133">In the search box, type **Small Improvements**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-smallimprovements-tutorial/tutorial_smallimprovements_search.png)

5. <span data-ttu-id="94b5d-135">Az eredmények panelen válassza ki a **kis fejlesztései**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="94b5d-135">In the results panel, select **Small Improvements**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-smallimprovements-tutorial/tutorial_smallimprovements_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="94b5d-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="94b5d-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="94b5d-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján kis fejlesztései.</span><span class="sxs-lookup"><span data-stu-id="94b5d-138">In this section, you configure and test Azure AD single sign-on with Small Improvements based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="94b5d-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó kis fejlesztései a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="94b5d-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Small Improvements is to a user in Azure AD.</span></span> <span data-ttu-id="94b5d-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó a kis fejlesztései közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="94b5d-140">In other words, a link relationship between an Azure AD user and the related user in Small Improvements needs to be established.</span></span>

<span data-ttu-id="94b5d-141">Kis fejlesztései, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="94b5d-141">In Small Improvements, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="94b5d-142">Az Azure AD az egyszeri bejelentkezés kis javításait tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="94b5d-142">To configure and test Azure AD single sign-on with Small Improvements, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="94b5d-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="94b5d-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="94b5d-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="94b5d-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="94b5d-145">**[Kis fejlesztései tesztfelhasználó létrehozása](#creating-a-small-improvements-test-user)**  - való egy megfelelője a Britta Simon kis fejlesztései, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="94b5d-145">**[Creating a Small Improvements test user](#creating-a-small-improvements-test-user)** - to have a counterpart of Britta Simon in Small Improvements that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="94b5d-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="94b5d-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="94b5d-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="94b5d-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="94b5d-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="94b5d-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="94b5d-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és kis fejlesztései alkalmazásában egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="94b5d-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Small Improvements application.</span></span>

<span data-ttu-id="94b5d-150">**Kis javításait konfigurálása az Azure AD egyszeri bejelentkezést, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="94b5d-150">**To configure Azure AD single sign-on with Small Improvements, perform the following steps:**</span></span>

1. <span data-ttu-id="94b5d-151">Az Azure portálon a a **kis fejlesztései** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="94b5d-151">In the Azure portal, on the **Small Improvements** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="94b5d-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="94b5d-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-smallimprovements-tutorial/tutorial_smallimprovements_samlbase.png)

3. <span data-ttu-id="94b5d-155">Az a **kis fejlesztései tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="94b5d-155">On the **Small Improvements Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-smallimprovements-tutorial/tutorial_smallimprovements_url.png)

    <span data-ttu-id="94b5d-157">a.</span><span class="sxs-lookup"><span data-stu-id="94b5d-157">a.</span></span> <span data-ttu-id="94b5d-158">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<subdomain>.small-improvements.com`</span><span class="sxs-lookup"><span data-stu-id="94b5d-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<subdomain>.small-improvements.com`</span></span>

    <span data-ttu-id="94b5d-159">b.</span><span class="sxs-lookup"><span data-stu-id="94b5d-159">b.</span></span> <span data-ttu-id="94b5d-160">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://<subdomain>.small-improvements.com`</span><span class="sxs-lookup"><span data-stu-id="94b5d-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<subdomain>.small-improvements.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="94b5d-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="94b5d-161">These values are not real.</span></span> <span data-ttu-id="94b5d-162">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="94b5d-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="94b5d-163">Ügyfél [kis fejlesztései ügyfél-támogatási csoport](mailto:support@small-improvements.com) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="94b5d-163">Contact [Small Improvements Client support team](mailto:support@small-improvements.com) to get these values.</span></span> 
 
4. <span data-ttu-id="94b5d-164">A a **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="94b5d-164">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-smallimprovements-tutorial/tutorial_smallimprovements_certificate.png) 

5. <span data-ttu-id="94b5d-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="94b5d-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-smallimprovements-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="94b5d-168">A a **kis fejlesztései konfigurációs** kattintson **kis fejlesztései konfigurálása** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="94b5d-168">On the **Small Improvements Configuration** section, click **Configure Small Improvements** to open **Configure sign-on** window.</span></span> <span data-ttu-id="94b5d-169">Másolás a **SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="94b5d-169">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-smallimprovements-tutorial/tutorial_smallimprovements_configure.png) 

7. <span data-ttu-id="94b5d-171">Egy másik böngészőablakban jelentkezzen be a kis fejlesztései vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="94b5d-171">In another browser window, sign on to your Small Improvements company site as an administrator.</span></span>

8. <span data-ttu-id="94b5d-172">A fő irányítópult-oldalon, kattintson az **felügyeleti** a bal oldali gomb.</span><span class="sxs-lookup"><span data-stu-id="94b5d-172">From the main dashboard page, click **Administration** button on the left.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-smallimprovements-tutorial/tutorial_smallimprovements_06.png) 

9. <span data-ttu-id="94b5d-174">Kattintson a **SAML SSO** gombot **integrációja** szakasz.</span><span class="sxs-lookup"><span data-stu-id="94b5d-174">Click the **SAML SSO** button from **Integrations** section.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-smallimprovements-tutorial/tutorial_smallimprovements_07.png) 

10. <span data-ttu-id="94b5d-176">Az egyszeri bejelentkezés beállítása lapon hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="94b5d-176">On the SSO Setup page, perform the following steps:</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-smallimprovements-tutorial/tutorial_smallimprovements_08.png)  

    <span data-ttu-id="94b5d-178">a.</span><span class="sxs-lookup"><span data-stu-id="94b5d-178">a.</span></span> <span data-ttu-id="94b5d-179">Az a **HTTP-végpont** szövegmezőhöz illessze be az értékét **SAML-alapú egyszeri bejelentkezési URL-címe**, amely az Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="94b5d-179">In the **HTTP Endpoint** textbox, paste the value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="94b5d-180">b.</span><span class="sxs-lookup"><span data-stu-id="94b5d-180">b.</span></span> <span data-ttu-id="94b5d-181">A letöltött tanúsítvány megnyitása a Jegyzettömbben, másolja a tartalmat, és illessze be azt a **x509 tanúsítvány** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="94b5d-181">Open your downloaded certificate in Notepad, copy the content, and then paste it into the **x509 Certificate** textbox.</span></span> 

    <span data-ttu-id="94b5d-182">c.</span><span class="sxs-lookup"><span data-stu-id="94b5d-182">c.</span></span> <span data-ttu-id="94b5d-183">Ha szeretne az egyszeri bejelentkezés és bejelentkezés hitelesítési lehetőséget a felhasználók rendelkeznek, majd ellenőrizze a **bejelentkezési azonosító és jelszó hozzáférésének engedélyezése túl** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="94b5d-183">If you wish to have SSO and Login form authentication option available for users, then check the **Enable access via login/password too** option.</span></span>  

    <span data-ttu-id="94b5d-184">d.</span><span class="sxs-lookup"><span data-stu-id="94b5d-184">d.</span></span> <span data-ttu-id="94b5d-185">Adja meg a megfelelő értéket, az Egyszeri bejelentkezés gombra, a nevet a **SAML Rákérdezés** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="94b5d-185">Enter the appropriate value to Name the SSO Login button in the **SAML Prompt** textbox.</span></span>  

    <span data-ttu-id="94b5d-186">e.</span><span class="sxs-lookup"><span data-stu-id="94b5d-186">e.</span></span> <span data-ttu-id="94b5d-187">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="94b5d-187">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="94b5d-188">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="94b5d-188">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="94b5d-189">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="94b5d-189">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="94b5d-190">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="94b5d-190">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="94b5d-191">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="94b5d-191">Creating an Azure AD test user</span></span>
<span data-ttu-id="94b5d-192">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="94b5d-192">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="94b5d-194">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="94b5d-194">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="94b5d-195">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="94b5d-195">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-smallimprovements-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="94b5d-197">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="94b5d-197">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-smallimprovements-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="94b5d-199">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="94b5d-199">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-smallimprovements-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="94b5d-201">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="94b5d-201">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-smallimprovements-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="94b5d-203">a.</span><span class="sxs-lookup"><span data-stu-id="94b5d-203">a.</span></span> <span data-ttu-id="94b5d-204">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="94b5d-204">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="94b5d-205">b.</span><span class="sxs-lookup"><span data-stu-id="94b5d-205">b.</span></span> <span data-ttu-id="94b5d-206">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="94b5d-206">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="94b5d-207">c.</span><span class="sxs-lookup"><span data-stu-id="94b5d-207">c.</span></span> <span data-ttu-id="94b5d-208">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="94b5d-208">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="94b5d-209">d.</span><span class="sxs-lookup"><span data-stu-id="94b5d-209">d.</span></span> <span data-ttu-id="94b5d-210">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="94b5d-210">Click **Create**.</span></span>
 
### <a name="creating-a-small-improvements-test-user"></a><span data-ttu-id="94b5d-211">Kis fejlesztései tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="94b5d-211">Creating a Small Improvements test user</span></span>

<span data-ttu-id="94b5d-212">Ahhoz, hogy az Azure AD-felhasználók kis fejlesztései bejelentkezni, akkor ki kell építenie kis fejlesztései be.</span><span class="sxs-lookup"><span data-stu-id="94b5d-212">To enable Azure AD users to log in to Small Improvements, they must be provisioned into Small Improvements.</span></span> <span data-ttu-id="94b5d-213">Kis fejlesztései, ha egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="94b5d-213">In the case of Small Improvements, provisioning is a manual task.</span></span>

<span data-ttu-id="94b5d-214">**Felhasználói fiók létrehozásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="94b5d-214">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="94b5d-215">Bejelentkezés a kis fejlesztései vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="94b5d-215">Sign-on to your Small Improvements company site as an administrator.</span></span>

2. <span data-ttu-id="94b5d-216">A kezdőlapról, nyissa meg a bal oldali, kattintson a menühöz **felügyeleti**.</span><span class="sxs-lookup"><span data-stu-id="94b5d-216">From the Home page, go to the menu on the left, click **Administration**.</span></span>

3. <span data-ttu-id="94b5d-217">Kattintson a **felhasználói Directory** gomb felhasználókezelés szakaszából.</span><span class="sxs-lookup"><span data-stu-id="94b5d-217">Click the **User Directory** button from User Management section.</span></span> 
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-smallimprovements-tutorial/tutorial_smallimprovements_10.png) 

4. <span data-ttu-id="94b5d-219">Kattintson a **felhasználók hozzáadása az**.</span><span class="sxs-lookup"><span data-stu-id="94b5d-219">Click **Add users**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-smallimprovements-tutorial/tutorial_smallimprovements_11.png) 

5. <span data-ttu-id="94b5d-221">Az a **felhasználó hozzáadása** párbeszédpanelen hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="94b5d-221">On the **Add Users** dialog, perform the following steps:</span></span> 

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-smallimprovements-tutorial/tutorial_smallimprovements_12.png)
    
    <span data-ttu-id="94b5d-223">a.</span><span class="sxs-lookup"><span data-stu-id="94b5d-223">a.</span></span> <span data-ttu-id="94b5d-224">Adja meg a **Utónév** például a felhasználó **Britta**.</span><span class="sxs-lookup"><span data-stu-id="94b5d-224">Enter the **first name** of user like **Britta**.</span></span>

    <span data-ttu-id="94b5d-225">b.</span><span class="sxs-lookup"><span data-stu-id="94b5d-225">b.</span></span> <span data-ttu-id="94b5d-226">Adja meg a **Vezetéknév** például a felhasználó **Simon**.</span><span class="sxs-lookup"><span data-stu-id="94b5d-226">Enter the **Last name** of user like **Simon**.</span></span>

    <span data-ttu-id="94b5d-227">c.</span><span class="sxs-lookup"><span data-stu-id="94b5d-227">c.</span></span> <span data-ttu-id="94b5d-228">Adja meg a **E-mail** például a felhasználó  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="94b5d-228">Enter the **Email** of user like **brittasimon@contoso.com**.</span></span> 

    <span data-ttu-id="94b5d-229">d.</span><span class="sxs-lookup"><span data-stu-id="94b5d-229">d.</span></span> <span data-ttu-id="94b5d-230">Másik lehetőségként a személyes üzenetet adhat meg a **értesítő e-mail küldése** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="94b5d-230">You can also choose to enter the personal message in the **Send notification email** box.</span></span> <span data-ttu-id="94b5d-231">Ha nem szeretne az értesítés elküldéséhez, majd törölje a jelet a jelölőnégyzetből.</span><span class="sxs-lookup"><span data-stu-id="94b5d-231">If you do not wish to send the notification, then uncheck this checkbox.</span></span>

    <span data-ttu-id="94b5d-232">e.</span><span class="sxs-lookup"><span data-stu-id="94b5d-232">e.</span></span> <span data-ttu-id="94b5d-233">Kattintson a **létrehozása a felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="94b5d-233">Click **Create Users**.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="94b5d-234">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="94b5d-234">Assigning the Azure AD test user</span></span>

<span data-ttu-id="94b5d-235">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés kis fejlesztései Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="94b5d-235">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Small Improvements.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="94b5d-237">**Britta Simon hozzárendelése kis fejlesztései, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="94b5d-237">**To assign Britta Simon to Small Improvements, perform the following steps:**</span></span>

1. <span data-ttu-id="94b5d-238">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="94b5d-238">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="94b5d-240">Az alkalmazások listában válassza ki a **kis fejlesztései**.</span><span class="sxs-lookup"><span data-stu-id="94b5d-240">In the applications list, select **Small Improvements**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-smallimprovements-tutorial/tutorial_smallimprovements_app.png) 

3. <span data-ttu-id="94b5d-242">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="94b5d-242">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="94b5d-244">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="94b5d-244">Click **Add** button.</span></span> <span data-ttu-id="94b5d-245">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="94b5d-245">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="94b5d-247">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="94b5d-247">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="94b5d-248">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="94b5d-248">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="94b5d-249">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="94b5d-249">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="94b5d-250">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="94b5d-250">Testing single sign-on</span></span>

<span data-ttu-id="94b5d-251">Ez a szakasz célja a hozzáférési panelen az Azure AD SSO-konfigurációjának tesztelése.</span><span class="sxs-lookup"><span data-stu-id="94b5d-251">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>  

<span data-ttu-id="94b5d-252">Ha a hozzáférési Panel kis fejlesztései mozaik gombra kattint, akkor kell beolvasása automatikusan bejelentkezett kis fejlesztései Alkalmazásmódosítások.</span><span class="sxs-lookup"><span data-stu-id="94b5d-252">When you click the Small Improvements tile in the Access Panel, you should get automatically signed-on to your Small Improvements application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="94b5d-253">További források</span><span class="sxs-lookup"><span data-stu-id="94b5d-253">Additional resources</span></span>

* [<span data-ttu-id="94b5d-254">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="94b5d-254">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="94b5d-255">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="94b5d-255">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-smallimprovements-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-smallimprovements-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-smallimprovements-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-smallimprovements-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-smallimprovements-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-smallimprovements-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-smallimprovements-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-smallimprovements-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-smallimprovements-tutorial/tutorial_general_203.png

