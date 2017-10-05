---
title: "Oktatóanyag: Azure Active Directoryval integrált LiquidFiles |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és LiquidFiles között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: cb517134-0b34-4a74-b40c-5a3223ca81b6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: jeedes
ms.openlocfilehash: b858c6d26b78be4641a46b3453f53d103b512356
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/29/2017
---
# <a name="tutorial-azure-active-directory-integration-with-liquidfiles"></a><span data-ttu-id="294d8-103">Oktatóanyag: Azure Active Directoryval integrált LiquidFiles</span><span class="sxs-lookup"><span data-stu-id="294d8-103">Tutorial: Azure Active Directory integration with LiquidFiles</span></span>

<span data-ttu-id="294d8-104">Ebben az oktatóanyagban elsajátíthatja LiquidFiles integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="294d8-104">In this tutorial, you learn how to integrate LiquidFiles with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="294d8-105">LiquidFiles integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="294d8-105">Integrating LiquidFiles with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="294d8-106">Megadhatja a LiquidFiles hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="294d8-106">You can control in Azure AD who has access to LiquidFiles</span></span>
- <span data-ttu-id="294d8-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett LiquidFiles (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="294d8-107">You can enable your users to automatically get signed-on to LiquidFiles (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="294d8-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="294d8-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="294d8-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="294d8-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="294d8-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="294d8-110">Prerequisites</span></span>

<span data-ttu-id="294d8-111">Konfigurálása az Azure AD-integrációs LiquidFiles, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="294d8-111">To configure Azure AD integration with LiquidFiles, you need the following items:</span></span>

- <span data-ttu-id="294d8-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="294d8-112">An Azure AD subscription</span></span>
- <span data-ttu-id="294d8-113">Egy LiquidFiles egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="294d8-113">A LiquidFiles single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="294d8-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="294d8-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="294d8-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="294d8-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="294d8-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="294d8-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="294d8-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="294d8-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="294d8-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="294d8-118">Scenario description</span></span>
<span data-ttu-id="294d8-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="294d8-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="294d8-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="294d8-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="294d8-121">A gyűjteményből LiquidFiles hozzáadása</span><span class="sxs-lookup"><span data-stu-id="294d8-121">Adding LiquidFiles from the gallery</span></span>
2. <span data-ttu-id="294d8-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="294d8-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-liquidfiles-from-the-gallery"></a><span data-ttu-id="294d8-123">A gyűjteményből LiquidFiles hozzáadása</span><span class="sxs-lookup"><span data-stu-id="294d8-123">Adding LiquidFiles from the gallery</span></span>
<span data-ttu-id="294d8-124">Az Azure AD integrálása a LiquidFiles konfigurálásához kell hozzáadnia LiquidFiles a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="294d8-124">To configure the integration of LiquidFiles into Azure AD, you need to add LiquidFiles from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="294d8-125">**A gyűjteményből LiquidFiles hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="294d8-125">**To add LiquidFiles from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="294d8-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="294d8-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="294d8-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="294d8-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="294d8-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="294d8-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="294d8-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="294d8-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="294d8-133">Írja be a keresőmezőbe, **LiquidFiles**.</span><span class="sxs-lookup"><span data-stu-id="294d8-133">In the search box, type **LiquidFiles**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-liquidfiles-tutorial/tutorial_liquidfiles_search.png)

5. <span data-ttu-id="294d8-135">Az eredmények panelen válassza ki a **LiquidFiles**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="294d8-135">In the results panel, select **LiquidFiles**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-liquidfiles-tutorial/tutorial_liquidfiles_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="294d8-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="294d8-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="294d8-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján LiquidFiles.</span><span class="sxs-lookup"><span data-stu-id="294d8-138">In this section, you configure and test Azure AD single sign-on with LiquidFiles based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="294d8-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó LiquidFiles a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="294d8-139">For single sign-on to work, Azure AD needs to know what the counterpart user in LiquidFiles is to a user in Azure AD.</span></span> <span data-ttu-id="294d8-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a LiquidFiles közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="294d8-140">In other words, a link relationship between an Azure AD user and the related user in LiquidFiles needs to be established.</span></span>

<span data-ttu-id="294d8-141">LiquidFiles, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="294d8-141">In LiquidFiles, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="294d8-142">Az Azure AD egyszeri bejelentkezést a LiquidFiles tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="294d8-142">To configure and test Azure AD single sign-on with LiquidFiles, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="294d8-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="294d8-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="294d8-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="294d8-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="294d8-145">**[LiquidFiles tesztfelhasználó létrehozása](#creating-a-liquidfiles-test-user)**  - való Britta Simon valami LiquidFiles, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="294d8-145">**[Creating a LiquidFiles test user](#creating-a-liquidfiles-test-user)** - to have a counterpart of Britta Simon in LiquidFiles that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="294d8-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="294d8-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="294d8-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="294d8-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="294d8-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="294d8-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="294d8-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az LiquidFiles alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="294d8-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your LiquidFiles application.</span></span>

<span data-ttu-id="294d8-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés LiquidFiles, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="294d8-150">**To configure Azure AD single sign-on with LiquidFiles, perform the following steps:**</span></span>

1. <span data-ttu-id="294d8-151">Az Azure portálon a a **LiquidFiles** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="294d8-151">In the Azure portal, on the **LiquidFiles** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="294d8-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="294d8-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-liquidfiles-tutorial/tutorial_liquidfiles_samlbase.png)

3. <span data-ttu-id="294d8-155">Az a **LiquidFiles tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="294d8-155">On the **LiquidFiles Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-liquidfiles-tutorial/tutorial_liquidfiles_url.png)

    <span data-ttu-id="294d8-157">a.</span><span class="sxs-lookup"><span data-stu-id="294d8-157">a.</span></span> <span data-ttu-id="294d8-158">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<YOUR_SERVER_URL>/saml/init`</span><span class="sxs-lookup"><span data-stu-id="294d8-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<YOUR_SERVER_URL>/saml/init`</span></span>

    <span data-ttu-id="294d8-159">b.</span><span class="sxs-lookup"><span data-stu-id="294d8-159">b.</span></span> <span data-ttu-id="294d8-160">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://<YOUR_SERVER_URL>`</span><span class="sxs-lookup"><span data-stu-id="294d8-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<YOUR_SERVER_URL>`</span></span>

    <span data-ttu-id="294d8-161">c.</span><span class="sxs-lookup"><span data-stu-id="294d8-161">c.</span></span> <span data-ttu-id="294d8-162">b.</span><span class="sxs-lookup"><span data-stu-id="294d8-162">b.</span></span> <span data-ttu-id="294d8-163">Az a **válasz URL-CÍMEN** szövegmező, adja meg a következő minta használatával URL-címe:`https://<YOUR_SERVER_URL>/saml/consume`</span><span class="sxs-lookup"><span data-stu-id="294d8-163">In the **Reply URL** textbox, type a URL using the following pattern: `https://<YOUR_SERVER_URL>/saml/consume`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="294d8-164">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="294d8-164">These values are not real.</span></span> <span data-ttu-id="294d8-165">A tényleges bejelentkezési URL-címet, frissítheti ezeket az értékeket azonosítója és válasz URL-CÍMEN.</span><span class="sxs-lookup"><span data-stu-id="294d8-165">Update these values with the actual Sign-On URL, Identifier and, Reply URL.</span></span> <span data-ttu-id="294d8-166">Ügyfél [LiquidFiles ügyfél-támogatási csoport](https://www.liquidfiles.com/support.html) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="294d8-166">Contact [LiquidFiles Client support team](https://www.liquidfiles.com/support.html) to get these values.</span></span> 
 
4. <span data-ttu-id="294d8-167">Az a **SAML-aláíró tanúsítványa** szakaszban, másolja a **UJJLENYOMAT** tanúsítvány értékét.</span><span class="sxs-lookup"><span data-stu-id="294d8-167">On the **SAML Signing Certificate** section, copy the **THUMBPRINT** value of certificate.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-liquidfiles-tutorial/tutorial_liquidfiles_certificate.png) 

5. <span data-ttu-id="294d8-169">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="294d8-169">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-liquidfiles-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="294d8-171">A a **LiquidFiles konfigurációs** kattintson **konfigurálása LiquidFiles** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="294d8-171">On the **LiquidFiles Configuration** section, click **Configure LiquidFiles** to open **Configure sign-on** window.</span></span> <span data-ttu-id="294d8-172">Másolás a **Sign-Out URL-címet, és a SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="294d8-172">Copy the **Sign-Out URL, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-liquidfiles-tutorial/tutorial_liquidfiles_configure.png)
 
7. <span data-ttu-id="294d8-174">Bejelentkezés a LiquidFiles vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="294d8-174">Sign-on to your LiquidFiles company site as administrator.</span></span>

8. <span data-ttu-id="294d8-175">Kattintson a **egyszeri bejelentkezés** a a **Admin > konfigurációs** a menüből.</span><span class="sxs-lookup"><span data-stu-id="294d8-175">Click **Single Sign-On** in the **Admin > Configuration** from the menu.</span></span>

9. <span data-ttu-id="294d8-176">Az a **egyszeri bejelentkezés konfigurációs** lapon, a következő lépések</span><span class="sxs-lookup"><span data-stu-id="294d8-176">On the **Single Sign-On Configuration** page, perform the following steps</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-liquidfiles-tutorial/tutorial_single_01.png)

    <span data-ttu-id="294d8-178">a.</span><span class="sxs-lookup"><span data-stu-id="294d8-178">a.</span></span> <span data-ttu-id="294d8-179">Mint **módszer egyszeri bejelentkezési**, jelölje be **SAML 2**.</span><span class="sxs-lookup"><span data-stu-id="294d8-179">As **Single Sign On Method**, select **SAML 2**.</span></span>

    <span data-ttu-id="294d8-180">b.</span><span class="sxs-lookup"><span data-stu-id="294d8-180">b.</span></span> <span data-ttu-id="294d8-181">A a **IDP bejelentkezési URL-cím** szövegmezőhöz illessze be az értékét **SAML-alapú egyszeri bejelentkezési URL-címe**, amely az Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="294d8-181">In the **IDP Login URL** textbox, paste the value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="294d8-182">c.</span><span class="sxs-lookup"><span data-stu-id="294d8-182">c.</span></span> <span data-ttu-id="294d8-183">A a **IDP kijelentkezési URL-cím** szövegmezőhöz illessze be az értékét **Sign-Out URL-cím**, amely az Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="294d8-183">In the **IDP Logout URL** textbox, paste the value of **Sign-Out URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="294d8-184">d.</span><span class="sxs-lookup"><span data-stu-id="294d8-184">d.</span></span> <span data-ttu-id="294d8-185">Az a **IDP tanúsítvány-ujjlenyomat** szövegmező, illessze be a **UJJLENYOMAT** érték, amely az Azure-portálon másolta...</span><span class="sxs-lookup"><span data-stu-id="294d8-185">In the **IDP Cert Fingerprint** textbox, paste the **THUMBPRINT** value which you have copied from Azure portal..</span></span>

    <span data-ttu-id="294d8-186">e.</span><span class="sxs-lookup"><span data-stu-id="294d8-186">e.</span></span> <span data-ttu-id="294d8-187">Az azonosító formátuma szövegmezőben írja be az értéket **urn: oasis: nevek: tc: SAML:1.1:nameid-formátum: emailAddress**.</span><span class="sxs-lookup"><span data-stu-id="294d8-187">In the Name Identifier Format textbox, type the value **urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress**.</span></span>

    <span data-ttu-id="294d8-188">f.</span><span class="sxs-lookup"><span data-stu-id="294d8-188">f.</span></span> <span data-ttu-id="294d8-189">A Authn környezetben szövegmezőben írja be az értéket **urn: oasis: nevek: tc: SAML:2.0:ac:classes:PasswordProtectedTransport**.</span><span class="sxs-lookup"><span data-stu-id="294d8-189">In the Authn Context textbox, type the value **urn:oasis:names:tc:SAML:2.0:ac:classes:PasswordProtectedTransport**.</span></span>

    <span data-ttu-id="294d8-190">g.</span><span class="sxs-lookup"><span data-stu-id="294d8-190">g.</span></span> <span data-ttu-id="294d8-191">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="294d8-191">Click **Save**.</span></span>  

> [!TIP]
> <span data-ttu-id="294d8-192">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="294d8-192">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="294d8-193">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="294d8-193">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="294d8-194">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="294d8-194">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="294d8-195">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="294d8-195">Creating an Azure AD test user</span></span>
<span data-ttu-id="294d8-196">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="294d8-196">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="294d8-198">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="294d8-198">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="294d8-199">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="294d8-199">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-liquidfiles-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="294d8-201">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="294d8-201">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-liquidfiles-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="294d8-203">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="294d8-203">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-liquidfiles-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="294d8-205">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="294d8-205">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-liquidfiles-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="294d8-207">a.</span><span class="sxs-lookup"><span data-stu-id="294d8-207">a.</span></span> <span data-ttu-id="294d8-208">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="294d8-208">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="294d8-209">b.</span><span class="sxs-lookup"><span data-stu-id="294d8-209">b.</span></span> <span data-ttu-id="294d8-210">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="294d8-210">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="294d8-211">c.</span><span class="sxs-lookup"><span data-stu-id="294d8-211">c.</span></span> <span data-ttu-id="294d8-212">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="294d8-212">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="294d8-213">d.</span><span class="sxs-lookup"><span data-stu-id="294d8-213">d.</span></span> <span data-ttu-id="294d8-214">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="294d8-214">Click **Create**.</span></span>
 
### <a name="creating-a-liquidfiles-test-user"></a><span data-ttu-id="294d8-215">LiquidFiles tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="294d8-215">Creating a LiquidFiles test user</span></span>

<span data-ttu-id="294d8-216">Ez a szakasz célja LiquidFiles Britta Simon nevű felhasználót létrehozni.</span><span class="sxs-lookup"><span data-stu-id="294d8-216">The objective of this section is to create a user called Britta Simon in LiquidFiles.</span></span> <span data-ttu-id="294d8-217">A LiquidFiles server rendszergazdát saját maga a LiquidFiles alkalmazás való bejelentkezés előtt felhasználójává dolgozni.</span><span class="sxs-lookup"><span data-stu-id="294d8-217">Work with your LiquidFiles server administrator to get yourself added as a user before logging in to your LiquidFiles application.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="294d8-218">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="294d8-218">Assigning the Azure AD test user</span></span>

<span data-ttu-id="294d8-219">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés LiquidFiles Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="294d8-219">In this section, you enable Britta Simon to use Azure single sign-on by granting access to LiquidFiles.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="294d8-221">**Britta Simon hozzárendelése LiquidFiles, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="294d8-221">**To assign Britta Simon to LiquidFiles, perform the following steps:**</span></span>

1. <span data-ttu-id="294d8-222">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="294d8-222">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="294d8-224">Az alkalmazások listában válassza ki a **LiquidFiles**.</span><span class="sxs-lookup"><span data-stu-id="294d8-224">In the applications list, select **LiquidFiles**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-liquidfiles-tutorial/tutorial_liquidfiles_app.png) 

3. <span data-ttu-id="294d8-226">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="294d8-226">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="294d8-228">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="294d8-228">Click **Add** button.</span></span> <span data-ttu-id="294d8-229">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="294d8-229">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="294d8-231">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="294d8-231">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="294d8-232">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="294d8-232">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="294d8-233">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="294d8-233">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="294d8-234">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="294d8-234">Testing single sign-on</span></span>

<span data-ttu-id="294d8-235">Ez a szakasz célja tesztelése az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen.</span><span class="sxs-lookup"><span data-stu-id="294d8-235">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="294d8-236">Ha a hozzáférési panelen LiquidFiles csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az LiquidFiles alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="294d8-236">When you click the LiquidFiles tile in the Access Panel, you should get automatically signed-on to your LiquidFiles application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="294d8-237">További források</span><span class="sxs-lookup"><span data-stu-id="294d8-237">Additional resources</span></span>

* [<span data-ttu-id="294d8-238">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="294d8-238">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="294d8-239">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="294d8-239">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-liquidfiles-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-liquidfiles-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-liquidfiles-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-liquidfiles-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-liquidfiles-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-liquidfiles-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-liquidfiles-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-liquidfiles-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-liquidfiles-tutorial/tutorial_general_203.png

