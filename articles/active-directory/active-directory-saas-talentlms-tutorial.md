---
title: "Oktatóanyag: Azure Active Directoryval integrált TalentLMS |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és TalentLMS között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c903d20d-18e3-42b0-b997-6349c5412dde
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: jeedes
ms.openlocfilehash: f28d6fbfad9dae578a20db7218b7e3b174ed859c
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-talentlms"></a><span data-ttu-id="cb638-103">Oktatóanyag: Azure Active Directoryval integrált TalentLMS</span><span class="sxs-lookup"><span data-stu-id="cb638-103">Tutorial: Azure Active Directory integration with TalentLMS</span></span>

<span data-ttu-id="cb638-104">Ebben az oktatóanyagban elsajátíthatja TalentLMS integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="cb638-104">In this tutorial, you learn how to integrate TalentLMS with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="cb638-105">TalentLMS integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="cb638-105">Integrating TalentLMS with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="cb638-106">Megadhatja a TalentLMS hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="cb638-106">You can control in Azure AD who has access to TalentLMS</span></span>
- <span data-ttu-id="cb638-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett TalentLMS (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="cb638-107">You can enable your users to automatically get signed-on to TalentLMS (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="cb638-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="cb638-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="cb638-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="cb638-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cb638-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="cb638-110">Prerequisites</span></span>

<span data-ttu-id="cb638-111">Konfigurálása az Azure AD-integrációs TalentLMS, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="cb638-111">To configure Azure AD integration with TalentLMS, you need the following items:</span></span>

- <span data-ttu-id="cb638-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="cb638-112">An Azure AD subscription</span></span>
- <span data-ttu-id="cb638-113">Egy TalentLMS egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="cb638-113">A TalentLMS single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="cb638-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="cb638-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="cb638-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="cb638-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="cb638-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="cb638-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="cb638-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió Itt kaphat: [próbaverzió ajánlat](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="cb638-117">If you don't have an Azure AD trial environment, you can get a one-month trial here: [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="cb638-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="cb638-118">Scenario description</span></span>
<span data-ttu-id="cb638-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="cb638-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="cb638-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="cb638-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="cb638-121">A gyűjteményből TalentLMS hozzáadása</span><span class="sxs-lookup"><span data-stu-id="cb638-121">Adding TalentLMS from the gallery</span></span>
2. <span data-ttu-id="cb638-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="cb638-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-talentlms-from-the-gallery"></a><span data-ttu-id="cb638-123">A gyűjteményből TalentLMS hozzáadása</span><span class="sxs-lookup"><span data-stu-id="cb638-123">Adding TalentLMS from the gallery</span></span>
<span data-ttu-id="cb638-124">Az Azure AD integrálása a TalentLMS konfigurálásához kell hozzáadnia TalentLMS a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="cb638-124">To configure the integration of TalentLMS into Azure AD, you need to add TalentLMS from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="cb638-125">**A gyűjteményből TalentLMS hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="cb638-125">**To add TalentLMS from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="cb638-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="cb638-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="cb638-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="cb638-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="cb638-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="cb638-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="cb638-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="cb638-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="cb638-133">Írja be a keresőmezőbe, **TalentLMS**.</span><span class="sxs-lookup"><span data-stu-id="cb638-133">In the search box, type **TalentLMS**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-talentlms-tutorial/tutorial_talentlms_search.png)

5. <span data-ttu-id="cb638-135">Az eredmények panelen válassza ki a **TalentLMS**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="cb638-135">In the results panel, select **TalentLMS**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-talentlms-tutorial/tutorial_talentlms_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="cb638-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="cb638-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="cb638-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján TalentLMS</span><span class="sxs-lookup"><span data-stu-id="cb638-138">In this section, you configure and test Azure AD single sign-on with TalentLMS based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="cb638-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó TalentLMS a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="cb638-139">For single sign-on to work, Azure AD needs to know what the counterpart user in TalentLMS is to a user in Azure AD.</span></span> <span data-ttu-id="cb638-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a TalentLMS közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="cb638-140">In other words, a link relationship between an Azure AD user and the related user in TalentLMS needs to be established.</span></span>

<span data-ttu-id="cb638-141">TalentLMS, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="cb638-141">In TalentLMS, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="cb638-142">Az Azure AD egyszeri bejelentkezést a TalentLMS tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="cb638-142">To configure and test Azure AD single sign-on with TalentLMS, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="cb638-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="cb638-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="cb638-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="cb638-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="cb638-145">**[TalentLMS tesztfelhasználó létrehozása](#creating-a-talentlms-test-user)**  - való Britta Simon valami TalentLMS, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="cb638-145">**[Creating a TalentLMS test user](#creating-a-talentlms-test-user)** - to have a counterpart of Britta Simon in TalentLMS that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="cb638-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="cb638-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="cb638-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="cb638-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="cb638-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="cb638-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="cb638-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az TalentLMS alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="cb638-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your TalentLMS application.</span></span>

<span data-ttu-id="cb638-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés TalentLMS, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="cb638-150">**To configure Azure AD single sign-on with TalentLMS, perform the following steps:**</span></span>

1. <span data-ttu-id="cb638-151">Az Azure portálon a a **TalentLMS** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="cb638-151">In the Azure portal, on the **TalentLMS** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="cb638-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="cb638-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-talentlms-tutorial/tutorial_talentlms_samlbase.png)

3. <span data-ttu-id="cb638-155">Az a **TalentLMS tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="cb638-155">On the **TalentLMS Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-talentlms-tutorial/tutorial_talentlms_url.png)

    <span data-ttu-id="cb638-157">a.</span><span class="sxs-lookup"><span data-stu-id="cb638-157">a.</span></span> <span data-ttu-id="cb638-158">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<tenant-name>.TalentLMSapp.com`</span><span class="sxs-lookup"><span data-stu-id="cb638-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<tenant-name>.TalentLMSapp.com`</span></span>

    <span data-ttu-id="cb638-159">b.</span><span class="sxs-lookup"><span data-stu-id="cb638-159">b.</span></span> <span data-ttu-id="cb638-160">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`http://<tenant-name>.talentlms.com`</span><span class="sxs-lookup"><span data-stu-id="cb638-160">In the **Identifier** textbox, type a URL using the following pattern: `http://<tenant-name>.talentlms.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="cb638-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="cb638-161">These values are not real.</span></span> <span data-ttu-id="cb638-162">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="cb638-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="cb638-163">Ügyfél [TalentLMS ügyfél-támogatási csoport](https://www.talentlms.com/contact) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="cb638-163">Contact [TalentLMS Client support team](https://www.talentlms.com/contact) to get these values.</span></span> 
 
4. <span data-ttu-id="cb638-164">Az a **SAML-aláíró tanúsítványa** szakaszban, másolja a **UJJLENYOMAT** érték a tanúsítványból.</span><span class="sxs-lookup"><span data-stu-id="cb638-164">On the **SAML Signing Certificate** section, copy the **THUMBPRINT** value from the certificate.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-talentlms-tutorial/tutorial_talentlms_certificate.png) 

5. <span data-ttu-id="cb638-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="cb638-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-talentlms-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="cb638-168">A a **TalentLMS konfigurációs** kattintson **konfigurálása TalentLMS** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="cb638-168">On the **TalentLMS Configuration** section, click **Configure TalentLMS** to open **Configure sign-on** window.</span></span> <span data-ttu-id="cb638-169">Másolás a **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="cb638-169">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-talentlms-tutorial/tutorial_talentlms_configure.png)  

7. <span data-ttu-id="cb638-171">Egy másik webes böngészőablakban jelentkezzen be a TalentLMS vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="cb638-171">In a different web browser window, log in to your TalentLMS company site as an administrator.</span></span>

8. <span data-ttu-id="cb638-172">Az a **fiók & beállítások** területen kattintson a **felhasználók** fülre.</span><span class="sxs-lookup"><span data-stu-id="cb638-172">In the **Account & Settings** section, click the **Users** tab.</span></span>
   
    <span data-ttu-id="cb638-173">![Fiók & beállítások](./media/active-directory-saas-talentlms-tutorial/IC777296.png "fiók & beállítások")</span><span class="sxs-lookup"><span data-stu-id="cb638-173">![Account & Settings](./media/active-directory-saas-talentlms-tutorial/IC777296.png "Account & Settings")</span></span>

9. <span data-ttu-id="cb638-174">Kattintson a **egyszeri bejelentkezés (SSO)**,</span><span class="sxs-lookup"><span data-stu-id="cb638-174">Click **Single Sign-On (SSO)**,</span></span>

10. <span data-ttu-id="cb638-175">Egyszeri bejelentkezés csoportjában hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="cb638-175">In the Single Sign-On section, perform the following steps:</span></span>
   
    <span data-ttu-id="cb638-176">![Egyszeri bejelentkezés](./media/active-directory-saas-talentlms-tutorial/IC777297.png "egyszeri bejelentkezés")</span><span class="sxs-lookup"><span data-stu-id="cb638-176">![Single Sign-On](./media/active-directory-saas-talentlms-tutorial/IC777297.png "Single Sign-On")</span></span>   

    <span data-ttu-id="cb638-177">a.</span><span class="sxs-lookup"><span data-stu-id="cb638-177">a.</span></span> <span data-ttu-id="cb638-178">Az a **SSO integrációs típus** listáról válassza ki **SAML 2.0**.</span><span class="sxs-lookup"><span data-stu-id="cb638-178">From the **SSO integration type** list, select **SAML 2.0**.</span></span>

    <span data-ttu-id="cb638-179">b.</span><span class="sxs-lookup"><span data-stu-id="cb638-179">b.</span></span> <span data-ttu-id="cb638-180">Az a **identitásszolgáltató (IDP)** szövegmezőhöz illessze be az értékét **SAML Entitásazonosító**, amely az Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="cb638-180">In the **Identity provider (IDP)** textbox, paste the value of **SAML Entity ID**, which you have copied from Azure portal.</span></span>
 
    <span data-ttu-id="cb638-181">c.</span><span class="sxs-lookup"><span data-stu-id="cb638-181">c.</span></span> <span data-ttu-id="cb638-182">Beillesztés a **ujjlenyomat** érték az Azure-portálon a **tanúsítvány-ujjlenyomat** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="cb638-182">Paste the **Thumbprint** value from Azure portal into the **Certificate fingerprint** textbox.</span></span>    

    <span data-ttu-id="cb638-183">d.</span><span class="sxs-lookup"><span data-stu-id="cb638-183">d.</span></span>  <span data-ttu-id="cb638-184">A a **távoli bejelentkezési URL-cím** szövegmezőhöz illessze be az értékét **SAML-alapú egyszeri bejelentkezési URL-címe**, amely az Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="cb638-184">In the **Remote sign-in URL** textbox, paste the value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>
 
    <span data-ttu-id="cb638-185">e.</span><span class="sxs-lookup"><span data-stu-id="cb638-185">e.</span></span> <span data-ttu-id="cb638-186">A a **távoli kijelentkezési URL-cím** szövegmezőhöz illessze be az értékét **Sign-Out URL-cím**, amely az Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="cb638-186">In the **Remote sign-out URL** textbox, paste the value of **Sign-Out URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="cb638-187">f.</span><span class="sxs-lookup"><span data-stu-id="cb638-187">f.</span></span> <span data-ttu-id="cb638-188">Töltse ki a következőket:</span><span class="sxs-lookup"><span data-stu-id="cb638-188">Fill in the following:</span></span> 

    * <span data-ttu-id="cb638-189">Az a **TargetedID** szövegmezőhöz típusa`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`</span><span class="sxs-lookup"><span data-stu-id="cb638-189">In the **TargetedID** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`</span></span>
     
    * <span data-ttu-id="cb638-190">Az a **Keresztnév** szövegmezőhöz típusa`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`</span><span class="sxs-lookup"><span data-stu-id="cb638-190">In the **First name** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`</span></span>
    
    * <span data-ttu-id="cb638-191">Az a **Vezetéknév** szövegmezőhöz típusa`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`</span><span class="sxs-lookup"><span data-stu-id="cb638-191">In the **Last name** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`</span></span>
    
    * <span data-ttu-id="cb638-192">Az a **E-mail** szövegmezőhöz típusa`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`</span><span class="sxs-lookup"><span data-stu-id="cb638-192">In the **Email** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`</span></span>
    
11. <span data-ttu-id="cb638-193">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="cb638-193">Click **Save**.</span></span>
 
> [!TIP]
> <span data-ttu-id="cb638-194">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="cb638-194">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="cb638-195">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="cb638-195">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="cb638-196">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="cb638-196">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="cb638-197">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="cb638-197">Creating an Azure AD test user</span></span>
<span data-ttu-id="cb638-198">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="cb638-198">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="cb638-200">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="cb638-200">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="cb638-201">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="cb638-201">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-talentlms-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="cb638-203">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="cb638-203">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-talentlms-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="cb638-205">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="cb638-205">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-talentlms-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="cb638-207">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="cb638-207">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-talentlms-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="cb638-209">a.</span><span class="sxs-lookup"><span data-stu-id="cb638-209">a.</span></span> <span data-ttu-id="cb638-210">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="cb638-210">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="cb638-211">b.</span><span class="sxs-lookup"><span data-stu-id="cb638-211">b.</span></span> <span data-ttu-id="cb638-212">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="cb638-212">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="cb638-213">c.</span><span class="sxs-lookup"><span data-stu-id="cb638-213">c.</span></span> <span data-ttu-id="cb638-214">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="cb638-214">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="cb638-215">d.</span><span class="sxs-lookup"><span data-stu-id="cb638-215">d.</span></span> <span data-ttu-id="cb638-216">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="cb638-216">Click **Create**.</span></span>
 
### <a name="creating-a-talentlms-test-user"></a><span data-ttu-id="cb638-217">TalentLMS tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="cb638-217">Creating a TalentLMS test user</span></span>

<span data-ttu-id="cb638-218">Ahhoz, hogy az Azure AD-felhasználók TalentLMS bejelentkezni, akkor ki kell építenie a TalentLMS.</span><span class="sxs-lookup"><span data-stu-id="cb638-218">To enable Azure AD users to log in to TalentLMS, they must be provisioned into TalentLMS.</span></span> <span data-ttu-id="cb638-219">TalentLMS, ha egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="cb638-219">In the case of TalentLMS, provisioning is a manual task.</span></span>

<span data-ttu-id="cb638-220">**Felhasználói fiók létrehozásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="cb638-220">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="cb638-221">Jelentkezzen be a **TalentLMS** bérlő.</span><span class="sxs-lookup"><span data-stu-id="cb638-221">Log in to your **TalentLMS** tenant.</span></span>

2. <span data-ttu-id="cb638-222">Kattintson a **felhasználók**, és kattintson a **felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="cb638-222">Click **Users**, and then click **Add User**.</span></span>

3. <span data-ttu-id="cb638-223">Az a **felhasználó hozzáadása** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="cb638-223">On the **Add user** dialog page, perform the following steps:</span></span>
   
    <span data-ttu-id="cb638-224">![Felhasználó hozzáadása](./media/active-directory-saas-talentlms-tutorial/IC777299.png "felhasználó hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="cb638-224">![Add User](./media/active-directory-saas-talentlms-tutorial/IC777299.png "Add User")</span></span>  

    <span data-ttu-id="cb638-225">a.</span><span class="sxs-lookup"><span data-stu-id="cb638-225">a.</span></span> <span data-ttu-id="cb638-226">Az a **Utónév** szövegmező, írja be például a felhasználó utónevét **Britta**.</span><span class="sxs-lookup"><span data-stu-id="cb638-226">In the **First name** textbox, enter the first name of user like **Britta**.</span></span>

    <span data-ttu-id="cb638-227">b.</span><span class="sxs-lookup"><span data-stu-id="cb638-227">b.</span></span> <span data-ttu-id="cb638-228">Az a **Vezetéknév** szövegmező, írja be például a felhasználó vezetéknevét **Simon**.</span><span class="sxs-lookup"><span data-stu-id="cb638-228">In the **Last name** textbox, enter the last name of user like **Simon**.</span></span>
 
    <span data-ttu-id="cb638-229">c.</span><span class="sxs-lookup"><span data-stu-id="cb638-229">c.</span></span> <span data-ttu-id="cb638-230">Az a **E-mail cím** szövegmező, adja meg az e-mail címét, például a felhasználó  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="cb638-230">In the **Email address** textbox, enter the email of user like **brittasimon@contoso.com**.</span></span>

    <span data-ttu-id="cb638-231">d.</span><span class="sxs-lookup"><span data-stu-id="cb638-231">d.</span></span> <span data-ttu-id="cb638-232">Kattintson a **felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="cb638-232">Click **Add User**.</span></span>

>[!NOTE]
><span data-ttu-id="cb638-233">Bármely más TalentLMS felhasználói fiók létrehozása eszközök vagy rendelkezés AAD felhasználói fiókokhoz TalentLMS által nyújtott API-k.</span><span class="sxs-lookup"><span data-stu-id="cb638-233">You can use any other TalentLMS user account creation tools or APIs provided by TalentLMS to provision AAD user accounts.</span></span>
 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="cb638-234">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="cb638-234">Assigning the Azure AD test user</span></span>

<span data-ttu-id="cb638-235">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés TalentLMS Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="cb638-235">In this section, you enable Britta Simon to use Azure single sign-on by granting access to TalentLMS.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="cb638-237">**Britta Simon hozzárendelése TalentLMS, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="cb638-237">**To assign Britta Simon to TalentLMS, perform the following steps:**</span></span>

1. <span data-ttu-id="cb638-238">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="cb638-238">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="cb638-240">Az alkalmazások listában válassza ki a **TalentLMS**.</span><span class="sxs-lookup"><span data-stu-id="cb638-240">In the applications list, select **TalentLMS**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-talentlms-tutorial/tutorial_talentlms_app.png) 

3. <span data-ttu-id="cb638-242">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="cb638-242">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="cb638-244">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="cb638-244">Click **Add** button.</span></span> <span data-ttu-id="cb638-245">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="cb638-245">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="cb638-247">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="cb638-247">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="cb638-248">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="cb638-248">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="cb638-249">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="cb638-249">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="cb638-250">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="cb638-250">Testing single sign-on</span></span>

<span data-ttu-id="cb638-251">Ez a szakasz célja tesztelése az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen.</span><span class="sxs-lookup"><span data-stu-id="cb638-251">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="cb638-252">Ha a hozzáférési panelen TalentLMS csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az TalentLMS alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="cb638-252">When you click the TalentLMS tile in the Access Panel, you should get automatically signed-on to your TalentLMS application</span></span>

## <a name="additional-resources"></a><span data-ttu-id="cb638-253">További források</span><span class="sxs-lookup"><span data-stu-id="cb638-253">Additional resources</span></span>

* [<span data-ttu-id="cb638-254">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="cb638-254">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="cb638-255">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="cb638-255">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-talentlms-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-talentlms-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-talentlms-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-talentlms-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-talentlms-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-talentlms-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-talentlms-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-talentlms-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-talentlms-tutorial/tutorial_general_203.png

