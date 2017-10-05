---
title: "Oktatóanyag: Azure Active Directoryval integrált Kiteworks |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Kiteworks között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: f7984aaf-ab1f-4a85-9646-a9523f5275d9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2017
ms.author: jeedes
ms.openlocfilehash: 2fd9b346cb6d838069ef94ee9c2a8d113f22779c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-kiteworks"></a><span data-ttu-id="84daf-103">Oktatóanyag: Azure Active Directoryval integrált Kiteworks</span><span class="sxs-lookup"><span data-stu-id="84daf-103">Tutorial: Azure Active Directory integration with Kiteworks</span></span>

<span data-ttu-id="84daf-104">Ebben az oktatóanyagban elsajátíthatja Kiteworks integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="84daf-104">In this tutorial, you learn how to integrate Kiteworks with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="84daf-105">Kiteworks integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="84daf-105">Integrating Kiteworks with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="84daf-106">Megadhatja a Kiteworks hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="84daf-106">You can control in Azure AD who has access to Kiteworks</span></span>
- <span data-ttu-id="84daf-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett Kiteworks (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="84daf-107">You can enable your users to automatically get signed-on to Kiteworks (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="84daf-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="84daf-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="84daf-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="84daf-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="84daf-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="84daf-110">Prerequisites</span></span>

<span data-ttu-id="84daf-111">Konfigurálása az Azure AD-integrációs Kiteworks, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="84daf-111">To configure Azure AD integration with Kiteworks, you need the following items:</span></span>

- <span data-ttu-id="84daf-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="84daf-112">An Azure AD subscription</span></span>
- <span data-ttu-id="84daf-113">Egy Kiteworks egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="84daf-113">A Kiteworks single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="84daf-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="84daf-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="84daf-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="84daf-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="84daf-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="84daf-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="84daf-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="84daf-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="84daf-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="84daf-118">Scenario description</span></span>
<span data-ttu-id="84daf-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="84daf-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="84daf-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="84daf-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="84daf-121">A gyűjteményből Kiteworks hozzáadása</span><span class="sxs-lookup"><span data-stu-id="84daf-121">Adding Kiteworks from the gallery</span></span>
2. <span data-ttu-id="84daf-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="84daf-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-kiteworks-from-the-gallery"></a><span data-ttu-id="84daf-123">A gyűjteményből Kiteworks hozzáadása</span><span class="sxs-lookup"><span data-stu-id="84daf-123">Adding Kiteworks from the gallery</span></span>
<span data-ttu-id="84daf-124">Az Azure AD integrálása a Kiteworks konfigurálásához kell hozzáadnia Kiteworks a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="84daf-124">To configure the integration of Kiteworks into Azure AD, you need to add Kiteworks from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="84daf-125">**A gyűjteményből Kiteworks hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="84daf-125">**To add Kiteworks from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="84daf-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="84daf-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="84daf-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="84daf-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="84daf-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="84daf-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="84daf-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="84daf-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="84daf-133">Írja be a keresőmezőbe, **Kiteworks**.</span><span class="sxs-lookup"><span data-stu-id="84daf-133">In the search box, type **Kiteworks**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kiteworks-tutorial/tutorial_kiteworks_search.png)

5. <span data-ttu-id="84daf-135">Az eredmények panelen válassza ki a **Kiteworks**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="84daf-135">In the results panel, select **Kiteworks**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kiteworks-tutorial/tutorial_kiteworks_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="84daf-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="84daf-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="84daf-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Kiteworks.</span><span class="sxs-lookup"><span data-stu-id="84daf-138">In this section, you configure and test Azure AD single sign-on with Kiteworks based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="84daf-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Kiteworks a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="84daf-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Kiteworks is to a user in Azure AD.</span></span> <span data-ttu-id="84daf-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Kiteworks közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="84daf-140">In other words, a link relationship between an Azure AD user and the related user in Kiteworks needs to be established.</span></span>

<span data-ttu-id="84daf-141">Kiteworks, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="84daf-141">In Kiteworks, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="84daf-142">Az Azure AD egyszeri bejelentkezést a Kiteworks tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="84daf-142">To configure and test Azure AD single sign-on with Kiteworks, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="84daf-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="84daf-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="84daf-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="84daf-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="84daf-145">**[Kiteworks tesztfelhasználó létrehozása](#creating-a-kiteworks-test-user)**  - való Britta Simon valami Kiteworks, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="84daf-145">**[Creating a Kiteworks test user](#creating-a-kiteworks-test-user)** - to have a counterpart of Britta Simon in Kiteworks that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="84daf-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="84daf-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="84daf-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="84daf-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="84daf-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="84daf-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="84daf-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Kiteworks alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="84daf-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Kiteworks application.</span></span>

<span data-ttu-id="84daf-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés Kiteworks, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="84daf-150">**To configure Azure AD single sign-on with Kiteworks, perform the following steps:**</span></span>

1. <span data-ttu-id="84daf-151">Az Azure portálon a a **Kiteworks** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="84daf-151">In the Azure portal, on the **Kiteworks** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="84daf-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="84daf-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kiteworks-tutorial/tutorial_kiteworks_samlbase.png)

3. <span data-ttu-id="84daf-155">Az a **Kiteworks tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="84daf-155">On the **Kiteworks Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kiteworks-tutorial/tutorial_kiteworks_url.png)

    <span data-ttu-id="84daf-157">a.</span><span class="sxs-lookup"><span data-stu-id="84daf-157">a.</span></span> <span data-ttu-id="84daf-158">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<subdomain>.kiteworks.com`</span><span class="sxs-lookup"><span data-stu-id="84daf-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<subdomain>.kiteworks.com`</span></span>

    <span data-ttu-id="84daf-159">b.</span><span class="sxs-lookup"><span data-stu-id="84daf-159">b.</span></span> <span data-ttu-id="84daf-160">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://<subdomain>.kiteworks.com/sp/module.php/saml/sp/saml2-acs.php/sp-sso`</span><span class="sxs-lookup"><span data-stu-id="84daf-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<subdomain>.kiteworks.com/sp/module.php/saml/sp/saml2-acs.php/sp-sso`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="84daf-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="84daf-161">These values are not real.</span></span> <span data-ttu-id="84daf-162">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="84daf-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="84daf-163">Ügyfél [Kiteworks ügyfél-támogatási csoport](http://accellion.com/support) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="84daf-163">Contact [Kiteworks Client support team](http://accellion.com/support) to get these values.</span></span> 
 
4. <span data-ttu-id="84daf-164">A a **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="84daf-164">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kiteworks-tutorial/tutorial_kiteworks_certificate.png) 

5. <span data-ttu-id="84daf-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="84daf-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kiteworks-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="84daf-168">A a **Kiteworks konfigurációs** kattintson **konfigurálása Kiteworks** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="84daf-168">On the **Kiteworks Configuration** section, click **Configure Kiteworks** to open **Configure sign-on** window.</span></span> <span data-ttu-id="84daf-169">Másolás a **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="84daf-169">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kiteworks-tutorial/tutorial_kiteworks_configure.png) 

7. <span data-ttu-id="84daf-171">Jelentkezzen be rendszergazdaként a Kiteworks vállalati webhely.</span><span class="sxs-lookup"><span data-stu-id="84daf-171">Sign on to your Kiteworks company site as an administrator.</span></span>

8. <span data-ttu-id="84daf-172">A felső eszköztáron kattintson **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="84daf-172">In the toolbar on the top, click **Settings**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kiteworks-tutorial/tutorial_kiteworks_06.png) 

9. <span data-ttu-id="84daf-174">Az a **hitelesítési és engedélyezési** kattintson **egyszeri bejelentkezés beállítása**.</span><span class="sxs-lookup"><span data-stu-id="84daf-174">In the **Authentication and Authorization** section, click **SSO Setup**.</span></span> 
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kiteworks-tutorial/tutorial_kiteworks_07.png)
 
10. <span data-ttu-id="84daf-176">Az egyszeri bejelentkezés beállítása lapon hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="84daf-176">On the SSO Setup page, perform the following steps:</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kiteworks-tutorial/tutorial_kiteworks_09.png)   

    <span data-ttu-id="84daf-178">a.</span><span class="sxs-lookup"><span data-stu-id="84daf-178">a.</span></span> <span data-ttu-id="84daf-179">Válassza ki **hitelesítés egyszeri Bejelentkezéssel keresztül**.</span><span class="sxs-lookup"><span data-stu-id="84daf-179">Select **Authenticate via SSO**.</span></span>

    <span data-ttu-id="84daf-180">b.</span><span class="sxs-lookup"><span data-stu-id="84daf-180">b.</span></span> <span data-ttu-id="84daf-181">Válassza ki **AuthnRequest kezdeményezése**.</span><span class="sxs-lookup"><span data-stu-id="84daf-181">Select **Initiate AuthnRequest**.</span></span>

    <span data-ttu-id="84daf-182">c.</span><span class="sxs-lookup"><span data-stu-id="84daf-182">c.</span></span> <span data-ttu-id="84daf-183">Az a **IDP Entitásazonosító** szövegmezőhöz illessze be az értékét **SAML Entitásazonosító**, amely az Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="84daf-183">In the **IDP Entity ID** textbox, paste the value of **SAML Entity ID**, which you have copied from Azure portal.</span></span> 

    <span data-ttu-id="84daf-184">d.</span><span class="sxs-lookup"><span data-stu-id="84daf-184">d.</span></span> <span data-ttu-id="84daf-185">Az a **egyszeri bejelentkezési URL-címe** szövegmezőhöz illessze be az értékét **SAML-alapú egyszeri bejelentkezési URL-címe**, amely az Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="84daf-185">In the **Single Sign-On Service URL** textbox, paste the value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="84daf-186">e.</span><span class="sxs-lookup"><span data-stu-id="84daf-186">e.</span></span> <span data-ttu-id="84daf-187">Az a **egyetlen kijelentkezési URL-címe** szövegmezőhöz illessze be az értékét **Sign-Out URL-cím**, amely az Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="84daf-187">In the **Single Logout Service URL** textbox, paste the value of **Sign-Out URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="84daf-188">f.</span><span class="sxs-lookup"><span data-stu-id="84daf-188">f.</span></span> <span data-ttu-id="84daf-189">A letöltött tanúsítvány megnyitása a Jegyzettömbben, másolja a tartalmat, és illessze be azt a **RSA nyilvános kulcs tanúsítvány** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="84daf-189">Open your downloaded certificate in Notepad, copy the content, and then paste it into the **RSA Public Key Certificate** textbox.</span></span>
 
    <span data-ttu-id="84daf-190">g.</span><span class="sxs-lookup"><span data-stu-id="84daf-190">g.</span></span> <span data-ttu-id="84daf-191">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="84daf-191">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="84daf-192">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="84daf-192">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="84daf-193">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="84daf-193">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="84daf-194">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="84daf-194">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="84daf-195">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="84daf-195">Creating an Azure AD test user</span></span>
<span data-ttu-id="84daf-196">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="84daf-196">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="84daf-198">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="84daf-198">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="84daf-199">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="84daf-199">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kiteworks-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="84daf-201">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="84daf-201">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kiteworks-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="84daf-203">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="84daf-203">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kiteworks-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="84daf-205">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="84daf-205">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kiteworks-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="84daf-207">a.</span><span class="sxs-lookup"><span data-stu-id="84daf-207">a.</span></span> <span data-ttu-id="84daf-208">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="84daf-208">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="84daf-209">b.</span><span class="sxs-lookup"><span data-stu-id="84daf-209">b.</span></span> <span data-ttu-id="84daf-210">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="84daf-210">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="84daf-211">c.</span><span class="sxs-lookup"><span data-stu-id="84daf-211">c.</span></span> <span data-ttu-id="84daf-212">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="84daf-212">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="84daf-213">d.</span><span class="sxs-lookup"><span data-stu-id="84daf-213">d.</span></span> <span data-ttu-id="84daf-214">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="84daf-214">Click **Create**.</span></span>
 
### <a name="creating-a-kiteworks-test-user"></a><span data-ttu-id="84daf-215">Kiteworks tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="84daf-215">Creating a Kiteworks test user</span></span>

<span data-ttu-id="84daf-216">Ez a szakasz célja Kiteworks Britta Simon nevű felhasználót létrehozni.</span><span class="sxs-lookup"><span data-stu-id="84daf-216">The objective of this section is to create a user called Britta Simon in Kiteworks.</span></span>

<span data-ttu-id="84daf-217">Kiteworks támogatja just-in-time kiosztást, amely alapértelmezés szerint van engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="84daf-217">Kiteworks supports just-in-time provisioning, which is by default enabled.</span></span> <span data-ttu-id="84daf-218">Nincs ebben a szakaszban az Ön művelet elem.</span><span class="sxs-lookup"><span data-stu-id="84daf-218">There is no action item for you in this section.</span></span> <span data-ttu-id="84daf-219">Új felhasználó jön létre az Kitewors elérésére, ha még nem létezik tett kísérlet során.</span><span class="sxs-lookup"><span data-stu-id="84daf-219">A new user is created during an attempt to access Kitewors if it doesn't exist yet.</span></span>

>[!NOTE]
><span data-ttu-id="84daf-220">Hozza létre a felhasználó manuálisan kell, ha szeretné-e lépjen kapcsolatba a [Kiteworks támogatási csoport](http://accellion.com/support).</span><span class="sxs-lookup"><span data-stu-id="84daf-220">If you need to create a user manually, you need to contact the [Kiteworks support team](http://accellion.com/support).</span></span>
 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="84daf-221">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="84daf-221">Assigning the Azure AD test user</span></span>

<span data-ttu-id="84daf-222">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Kiteworks Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="84daf-222">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Kiteworks.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="84daf-224">**Britta Simon hozzárendelése Kiteworks, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="84daf-224">**To assign Britta Simon to Kiteworks, perform the following steps:**</span></span>

1. <span data-ttu-id="84daf-225">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="84daf-225">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="84daf-227">Az alkalmazások listában válassza ki a **Kiteworks**.</span><span class="sxs-lookup"><span data-stu-id="84daf-227">In the applications list, select **Kiteworks**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kiteworks-tutorial/tutorial_kiteworks_app.png) 

3. <span data-ttu-id="84daf-229">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="84daf-229">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="84daf-231">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="84daf-231">Click **Add** button.</span></span> <span data-ttu-id="84daf-232">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="84daf-232">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="84daf-234">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="84daf-234">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="84daf-235">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="84daf-235">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="84daf-236">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="84daf-236">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="84daf-237">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="84daf-237">Testing single sign-on</span></span>

<span data-ttu-id="84daf-238">Ez a szakasz célja a hozzáférési panelen az Azure AD SSO-konfigurációjának tesztelése.</span><span class="sxs-lookup"><span data-stu-id="84daf-238">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>  

<span data-ttu-id="84daf-239">Ha a hozzáférési panelen Kiteworks csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az Kiteworks alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="84daf-239">When you click the Kiteworks tile in the Access Panel, you should get automatically signed-on to your Kiteworks application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="84daf-240">További források</span><span class="sxs-lookup"><span data-stu-id="84daf-240">Additional resources</span></span>

* [<span data-ttu-id="84daf-241">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="84daf-241">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="84daf-242">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="84daf-242">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-kiteworks-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-kiteworks-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-kiteworks-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-kiteworks-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-kiteworks-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-kiteworks-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-kiteworks-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-kiteworks-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-kiteworks-tutorial/tutorial_general_203.png

