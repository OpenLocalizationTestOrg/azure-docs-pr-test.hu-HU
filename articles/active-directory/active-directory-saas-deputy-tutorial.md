---
title: "Oktatóanyag: Azure Active Directoryval integrált helyettes |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és helyettes között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 5665c3ac-5689-4201-80fe-fcc677d4430d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: 51aed908208b7a40ea2ab710dffe84370b573991
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-deputy"></a><span data-ttu-id="7bcdb-103">Oktatóanyag: Azure Active Directoryval integrált helyettes</span><span class="sxs-lookup"><span data-stu-id="7bcdb-103">Tutorial: Azure Active Directory integration with Deputy</span></span>

<span data-ttu-id="7bcdb-104">Ebben az oktatóanyagban elsajátíthatja helyettes integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="7bcdb-104">In this tutorial, you learn how to integrate Deputy with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="7bcdb-105">Helyettes integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="7bcdb-105">Integrating Deputy with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="7bcdb-106">Megadhatja a helyettes hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="7bcdb-106">You can control in Azure AD who has access to Deputy</span></span>
- <span data-ttu-id="7bcdb-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett helyettes (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="7bcdb-107">You can enable your users to automatically get signed-on to Deputy (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="7bcdb-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="7bcdb-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="7bcdb-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="7bcdb-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7bcdb-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="7bcdb-110">Prerequisites</span></span>

<span data-ttu-id="7bcdb-111">Az Azure AD-integrációs helyettes konfigurálni, kell a következő elemek:</span><span class="sxs-lookup"><span data-stu-id="7bcdb-111">To configure Azure AD integration with Deputy, you need the following items:</span></span>

- <span data-ttu-id="7bcdb-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="7bcdb-112">An Azure AD subscription</span></span>
- <span data-ttu-id="7bcdb-113">A helyettes egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="7bcdb-113">A Deputy single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="7bcdb-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="7bcdb-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="7bcdb-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="7bcdb-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="7bcdb-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="7bcdb-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="7bcdb-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="7bcdb-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="7bcdb-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="7bcdb-118">Scenario description</span></span>
<span data-ttu-id="7bcdb-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="7bcdb-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="7bcdb-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="7bcdb-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="7bcdb-121">Helyettes hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="7bcdb-121">Adding Deputy from the gallery</span></span>
2. <span data-ttu-id="7bcdb-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="7bcdb-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-deputy-from-the-gallery"></a><span data-ttu-id="7bcdb-123">Helyettes hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="7bcdb-123">Adding Deputy from the gallery</span></span>
<span data-ttu-id="7bcdb-124">Az Azure AD integrálása a helyettes konfigurálásához kell hozzáadnia helyettes a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="7bcdb-124">To configure the integration of Deputy into Azure AD, you need to add Deputy from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="7bcdb-125">**A gyűjteményből helyettes hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="7bcdb-125">**To add Deputy from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="7bcdb-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="7bcdb-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="7bcdb-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="7bcdb-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="7bcdb-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="7bcdb-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="7bcdb-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="7bcdb-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="7bcdb-133">Írja be a keresőmezőbe, **helyettes**.</span><span class="sxs-lookup"><span data-stu-id="7bcdb-133">In the search box, type **Deputy**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_search.png)

5. <span data-ttu-id="7bcdb-135">Az eredmények panelen válassza ki a **helyettes**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="7bcdb-135">In the results panel, select **Deputy**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="7bcdb-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="7bcdb-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="7bcdb-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján helyettes</span><span class="sxs-lookup"><span data-stu-id="7bcdb-138">In this section, you configure and test Azure AD single sign-on with Deputy based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="7bcdb-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó helyettes a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="7bcdb-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Deputy is to a user in Azure AD.</span></span> <span data-ttu-id="7bcdb-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó a helyettes közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="7bcdb-140">In other words, a link relationship between an Azure AD user and the related user in Deputy needs to be established.</span></span>

<span data-ttu-id="7bcdb-141">Helyettes, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="7bcdb-141">In Deputy, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="7bcdb-142">Az Azure AD egyszeri bejelentkezést a helyettes tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="7bcdb-142">To configure and test Azure AD single sign-on with Deputy, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="7bcdb-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="7bcdb-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="7bcdb-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="7bcdb-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="7bcdb-145">**[Helyettes tesztfelhasználó létrehozása](#creating-a-deputy-test-user)**  - való egy megfelelője a Britta Simon helyettes, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="7bcdb-145">**[Creating a Deputy test user](#creating-a-deputy-test-user)** - to have a counterpart of Britta Simon in Deputy that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="7bcdb-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="7bcdb-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="7bcdb-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="7bcdb-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="7bcdb-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="7bcdb-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="7bcdb-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az helyettes alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="7bcdb-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Deputy application.</span></span>

<span data-ttu-id="7bcdb-150">**Helyettes konfigurálása az Azure AD egyszeri bejelentkezést, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="7bcdb-150">**To configure Azure AD single sign-on with Deputy, perform the following steps:**</span></span>

1. <span data-ttu-id="7bcdb-151">Az Azure portálon a a **helyettes** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="7bcdb-151">In the Azure portal, on the **Deputy** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="7bcdb-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="7bcdb-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_samlbase.png)

3. <span data-ttu-id="7bcdb-155">Az a **helyettes tartomány és az URL-címek** szakaszban, ha szeretne beállítani az alkalmazás **IDP** kezdeményezett mód:</span><span class="sxs-lookup"><span data-stu-id="7bcdb-155">On the **Deputy Domain and URLs** section, If you wish to configure the application in **IDP** initiated mode:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_url1.png)

    <span data-ttu-id="7bcdb-157">a.</span><span class="sxs-lookup"><span data-stu-id="7bcdb-157">a.</span></span> <span data-ttu-id="7bcdb-158">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:</span><span class="sxs-lookup"><span data-stu-id="7bcdb-158">In the **Identifier** textbox, type a URL using the following pattern:</span></span>
    |  |
    | ----|
    | `https://<subdomain>.<region>.au.deputy.com` |
    | `https://<subdomain>.<region>.ent-au.deputy.com` |
    | `https://<subdomain>.<region>.na.deputy.com`|
    | `https://<subdomain>.<region>.ent-na.deputy.com`|
    | `https://<subdomain>.<region>.eu.deputy.com` |
    | `https://<subdomain>.<region>.ent-eu.deputy.com` |
    | `https://<subdomain>.<region>.as.deputy.com` |
    | `https://<subdomain>.<region>.ent-as.deputy.com` |
    | `https://<subdomain>.<region>.la.deputy.com` |
    | `https://<subdomain>.<region>.ent-la.deputy.com` |
    | `https://<subdomain>.<region>.af.deputy.com` |
    | `https://<subdomain>.<region>.ent-af.deputy.com` |
    | `https://<subdomain>.<region>.an.deputy.com` |
    | `https://<subdomain>.<region>.ent-an.deputy.com` |
    | `https://<subdomain>.<region>.deputy.com` |

    <span data-ttu-id="7bcdb-159">b.</span><span class="sxs-lookup"><span data-stu-id="7bcdb-159">b.</span></span> <span data-ttu-id="7bcdb-160">Az a **válasz URL-CÍMEN** szövegmező, adja meg a következő minta használatával URL-címe:</span><span class="sxs-lookup"><span data-stu-id="7bcdb-160">In the **Reply URL** textbox, type a URL using the following pattern:</span></span>
    | |
    |----|
    | `https://<subdomain>.<region>.au.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.ent-au.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.na.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.ent-na.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.eu.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.ent-eu.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.as.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.ent-as.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.la.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.ent-la.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.af.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.ent-af.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.an.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.ent-an.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.deputy.com/exec/devapp/samlacs.` |

4. <span data-ttu-id="7bcdb-161">Ellenőrizze **megjelenítése speciális URL-beállításainak**.</span><span class="sxs-lookup"><span data-stu-id="7bcdb-161">Check **Show advanced URL settings**.</span></span> <span data-ttu-id="7bcdb-162">Ha szeretne beállítani az alkalmazás **SP** kezdeményezett mód:</span><span class="sxs-lookup"><span data-stu-id="7bcdb-162">If you wish to configure the application in **SP** initiated mode:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_url2.png)

    <span data-ttu-id="7bcdb-164">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<your-subdomain>.<region>.deputy.com`</span><span class="sxs-lookup"><span data-stu-id="7bcdb-164">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<your-subdomain>.<region>.deputy.com`</span></span>
    
    >[!NOTE]
    > <span data-ttu-id="7bcdb-165">Helyettes régió utótag megadása nem kötelező, vagy ezek egyikét kell használni: au |} nA |} EU |}, |} la |} af |} egy |} félellenőrzés-au |} félellenőrzés-na |} félellenőrzés – eu |} félellenőrzés-mint |} félellenőrzés-la |} félellenőrzés-af |} félellenőrzés egy</span><span class="sxs-lookup"><span data-stu-id="7bcdb-165">Deputy region suffix is optional, or it should use one of these: au | na | eu |as |la |af |an |ent-au |ent-na |ent-eu |ent-as | ent-la | ent-af | ent-an</span></span>

    > [!NOTE] 
    > <span data-ttu-id="7bcdb-166">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="7bcdb-166">These values are not real.</span></span> <span data-ttu-id="7bcdb-167">Frissítheti ezeket az értékeket a tényleges azonosítója, válasz URL-CÍMEN és bejelentkezési URL-cím.</span><span class="sxs-lookup"><span data-stu-id="7bcdb-167">Update these values with the actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="7bcdb-168">Ügyfél [helyettes támogatási csoport](https://www.deputy.com/call-centers-customer-support-scheduling-software) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="7bcdb-168">Contact [Deputy support team](https://www.deputy.com/call-centers-customer-support-scheduling-software) to get these values.</span></span> 

5. <span data-ttu-id="7bcdb-169">Az a **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="7bcdb-169">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_certificate.png) 

6. <span data-ttu-id="7bcdb-171">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="7bcdb-171">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-deputy-tutorial/tutorial_general_400.png)
    
7. <span data-ttu-id="7bcdb-173">A a **helyettes konfigurációs** kattintson **konfigurálása helyettes** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="7bcdb-173">On the **Deputy Configuration** section, click **Configure Deputy** to open **Configure sign-on** window.</span></span> <span data-ttu-id="7bcdb-174">Másolás a **SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="7bcdb-174">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_configure.png) 

8. <span data-ttu-id="7bcdb-176">Keresse meg a következő URL-cím:[https://(your-subdomain).deputy.com/exec/config/system_config]( https://(your-subdomain).deputy.com/exec/config/system_config).</span><span class="sxs-lookup"><span data-stu-id="7bcdb-176">Navigate to the following URL:[https://(your-subdomain).deputy.com/exec/config/system_config]( https://(your-subdomain).deputy.com/exec/config/system_config).</span></span> <span data-ttu-id="7bcdb-177">Ugrás a **biztonsági beállítások** kattintson **szerkesztése**.</span><span class="sxs-lookup"><span data-stu-id="7bcdb-177">Go to **Security Settings** and click **Edit**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_004.png)

9. <span data-ttu-id="7bcdb-179">Ez a **biztonsági beállítások** lapon, a következő lépések végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="7bcdb-179">On this **Security Settings** page, perform below steps.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_005.png)
    
    <span data-ttu-id="7bcdb-181">a.</span><span class="sxs-lookup"><span data-stu-id="7bcdb-181">a.</span></span> <span data-ttu-id="7bcdb-182">Engedélyezése **közösségi bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="7bcdb-182">Enable **Social Login**.</span></span>
   
    <span data-ttu-id="7bcdb-183">b.</span><span class="sxs-lookup"><span data-stu-id="7bcdb-183">b.</span></span> <span data-ttu-id="7bcdb-184">Nyissa meg a Jegyzettömbben az Azure portálról letöltött Base64 kódolású tanúsítvány, a tartalmának másolása a vágólapra és illessze be azt a **OpenSSL tanúsítvány** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="7bcdb-184">Open your Base64 encoded certificate downloaded from Azure portal in notepad, copy the content of it into your clipboard, and then paste it to the **OpenSSL Certificate** textbox.</span></span>
   
    <span data-ttu-id="7bcdb-185">c.</span><span class="sxs-lookup"><span data-stu-id="7bcdb-185">c.</span></span> <span data-ttu-id="7bcdb-186">Írja be a SAML SSO URL-cím beviteli mező`https://<your subdomain>.deputy.com/exec/devapp/samlacs?dpLoginTo=<saml sso url>`</span><span class="sxs-lookup"><span data-stu-id="7bcdb-186">In the SAML SSO URL textbox, type `https://<your subdomain>.deputy.com/exec/devapp/samlacs?dpLoginTo=<saml sso url>`</span></span>
    
    <span data-ttu-id="7bcdb-187">d.</span><span class="sxs-lookup"><span data-stu-id="7bcdb-187">d.</span></span> <span data-ttu-id="7bcdb-188">Cserélje le a SAML SSO URL-cím szövegmező `<your subdomain>` a altartomány együtt.</span><span class="sxs-lookup"><span data-stu-id="7bcdb-188">In the SAML SSO URL textbox, replace `<your subdomain>` with your subdomain.</span></span>
   
    <span data-ttu-id="7bcdb-189">e.</span><span class="sxs-lookup"><span data-stu-id="7bcdb-189">e.</span></span> <span data-ttu-id="7bcdb-190">Cserélje le a SAML SSO URL-cím szövegmező `<saml sso url>` rendelkező a **SAML-alapú egyszeri bejelentkezési URL-címe** másolta az Azure portálról.</span><span class="sxs-lookup"><span data-stu-id="7bcdb-190">In the SAML SSO URL textbox, replace `<saml sso url>` with the **SAML Single Sign-On Service URL** you have copied from the Azure portal.</span></span>
   
    <span data-ttu-id="7bcdb-191">f.</span><span class="sxs-lookup"><span data-stu-id="7bcdb-191">f.</span></span> <span data-ttu-id="7bcdb-192">Kattintson a **beállítások mentése**.</span><span class="sxs-lookup"><span data-stu-id="7bcdb-192">Click **Save Settings**.</span></span>

> [!TIP]
> <span data-ttu-id="7bcdb-193">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="7bcdb-193">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="7bcdb-194">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="7bcdb-194">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="7bcdb-195">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="7bcdb-195">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="7bcdb-196">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="7bcdb-196">Creating an Azure AD test user</span></span>
<span data-ttu-id="7bcdb-197">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="7bcdb-197">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="7bcdb-199">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="7bcdb-199">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="7bcdb-200">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="7bcdb-200">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-deputy-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="7bcdb-202">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="7bcdb-202">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-deputy-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="7bcdb-204">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="7bcdb-204">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-deputy-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="7bcdb-206">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="7bcdb-206">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-deputy-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="7bcdb-208">a.</span><span class="sxs-lookup"><span data-stu-id="7bcdb-208">a.</span></span> <span data-ttu-id="7bcdb-209">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="7bcdb-209">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="7bcdb-210">b.</span><span class="sxs-lookup"><span data-stu-id="7bcdb-210">b.</span></span> <span data-ttu-id="7bcdb-211">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="7bcdb-211">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="7bcdb-212">c.</span><span class="sxs-lookup"><span data-stu-id="7bcdb-212">c.</span></span> <span data-ttu-id="7bcdb-213">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="7bcdb-213">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="7bcdb-214">d.</span><span class="sxs-lookup"><span data-stu-id="7bcdb-214">d.</span></span> <span data-ttu-id="7bcdb-215">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="7bcdb-215">Click **Create**.</span></span>
 
### <a name="creating-a-deputy-test-user"></a><span data-ttu-id="7bcdb-216">Helyettes tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="7bcdb-216">Creating a Deputy test user</span></span>

<span data-ttu-id="7bcdb-217">Ahhoz, hogy az Azure AD-felhasználók helyettes bejelentkezni, akkor ki kell építenie a helyettes.</span><span class="sxs-lookup"><span data-stu-id="7bcdb-217">To enable Azure AD users to log in to Deputy, they must be provisioned into Deputy.</span></span> <span data-ttu-id="7bcdb-218">Helyettes, ha egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="7bcdb-218">In case of Deputy, provisioning is a manual task.</span></span>

#### <a name="to-provision-a-user-account-perform-the-following-steps"></a><span data-ttu-id="7bcdb-219">Felhasználói fiók létrehozásához hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="7bcdb-219">To provision a user account, perform the following steps:</span></span>
1. <span data-ttu-id="7bcdb-220">Jelentkezzen be rendszergazdaként a helyettes vállalati webhely.</span><span class="sxs-lookup"><span data-stu-id="7bcdb-220">Log in to your Deputy company site as an administrator.</span></span>

2. <span data-ttu-id="7bcdb-221">A felső navigációs ablaktábláján kattintson **személyek**.</span><span class="sxs-lookup"><span data-stu-id="7bcdb-221">On the top navigation pane, click **People**.</span></span>
   
   <span data-ttu-id="7bcdb-222">![Személyek](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_001.png "személyek")</span><span class="sxs-lookup"><span data-stu-id="7bcdb-222">![People](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_001.png "People")</span></span>

3. <span data-ttu-id="7bcdb-223">Kattintson a **hozzáadása személyek** gombra, majd kattintson a **hozzáadása egy személy**.</span><span class="sxs-lookup"><span data-stu-id="7bcdb-223">Click the **Add People** button and click **Add a single person**.</span></span>
   
   <span data-ttu-id="7bcdb-224">![Adja hozzá a személyek](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_002.png "személyek hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="7bcdb-224">![Add People](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_002.png "Add People")</span></span>

4. <span data-ttu-id="7bcdb-225">A következő lépésekkel, és kattintson a **meghívása & mentése**.</span><span class="sxs-lookup"><span data-stu-id="7bcdb-225">Perform the following steps and click **Save & Invite**.</span></span>
   
   <span data-ttu-id="7bcdb-226">![Új felhasználó](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_003.png "új felhasználó")</span><span class="sxs-lookup"><span data-stu-id="7bcdb-226">![New User](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_003.png "New User")</span></span>

   <span data-ttu-id="7bcdb-227">a.</span><span class="sxs-lookup"><span data-stu-id="7bcdb-227">a.</span></span> <span data-ttu-id="7bcdb-228">Az a **neve** szövegmező, például a felhasználó neve **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="7bcdb-228">In the **Name** textbox, type name of the user like **BrittaSimon**.</span></span>
   
   <span data-ttu-id="7bcdb-229">b.</span><span class="sxs-lookup"><span data-stu-id="7bcdb-229">b.</span></span> <span data-ttu-id="7bcdb-230">Az a **E-mail** szövegmező, írja be a kívánt kiépítéséhez az Azure AD fiók e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="7bcdb-230">In the **Email** textbox, type the email address of an Azure AD account you want to provision.</span></span>
   
   <span data-ttu-id="7bcdb-231">c.</span><span class="sxs-lookup"><span data-stu-id="7bcdb-231">c.</span></span> <span data-ttu-id="7bcdb-232">Az a **működéséhez** szövegmező, írja be a vállalati nevét.</span><span class="sxs-lookup"><span data-stu-id="7bcdb-232">In the **Work at** textbox, type the business name.</span></span>
   
   <span data-ttu-id="7bcdb-233">d.</span><span class="sxs-lookup"><span data-stu-id="7bcdb-233">d.</span></span> <span data-ttu-id="7bcdb-234">Kattintson a **meghívása & mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="7bcdb-234">Click **Save & Invite** button.</span></span>

5. <span data-ttu-id="7bcdb-235">Az aad-ben fióktulajdonos kap egy e-mailt, és a következő hivatkozás a fiók megerősítéséhez, mielőtt aktívvá válik.</span><span class="sxs-lookup"><span data-stu-id="7bcdb-235">The AAD account holder receives an email and follows a link to confirm their account before it becomes active.</span></span> <span data-ttu-id="7bcdb-236">Bármely más helyettes felhasználói fiók létrehozása eszközök vagy rendelkezés AAD felhasználói fiókokhoz helyettes által nyújtott API-k.</span><span class="sxs-lookup"><span data-stu-id="7bcdb-236">You can use any other Deputy user account creation tools or APIs provided by Deputy to provision AAD user accounts.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="7bcdb-237">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="7bcdb-237">Assigning the Azure AD test user</span></span>

<span data-ttu-id="7bcdb-238">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés helyettes Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="7bcdb-238">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Deputy.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="7bcdb-240">**Britta Simon hozzárendelése helyettes, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="7bcdb-240">**To assign Britta Simon to Deputy, perform the following steps:**</span></span>

1. <span data-ttu-id="7bcdb-241">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="7bcdb-241">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="7bcdb-243">Az alkalmazások listában válassza ki a **helyettes**.</span><span class="sxs-lookup"><span data-stu-id="7bcdb-243">In the applications list, select **Deputy**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_app.png) 

3. <span data-ttu-id="7bcdb-245">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="7bcdb-245">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="7bcdb-247">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="7bcdb-247">Click **Add** button.</span></span> <span data-ttu-id="7bcdb-248">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="7bcdb-248">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="7bcdb-250">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="7bcdb-250">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="7bcdb-251">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="7bcdb-251">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="7bcdb-252">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="7bcdb-252">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="7bcdb-253">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="7bcdb-253">Testing single sign-on</span></span>

<span data-ttu-id="7bcdb-254">Ez a szakasz célja a hozzáférési panelen az Azure AD SSO-konfigurációjának tesztelése.</span><span class="sxs-lookup"><span data-stu-id="7bcdb-254">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="7bcdb-255">Ha a hozzáférési panelen helyettes csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az helyettes alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="7bcdb-255">When you click the Deputy tile in the Access Panel, you should get automatically signed-on to your Deputy application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7bcdb-256">További források</span><span class="sxs-lookup"><span data-stu-id="7bcdb-256">Additional resources</span></span>

* [<span data-ttu-id="7bcdb-257">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="7bcdb-257">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="7bcdb-258">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="7bcdb-258">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_203.png

