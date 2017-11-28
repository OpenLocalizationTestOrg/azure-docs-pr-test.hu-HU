---
title: "Oktatóanyag: Azure Active Directoryval integrált SumoLogic |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és SumoLogic között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: fbb76765-92d7-4801-9833-573b11b4d910
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.openlocfilehash: e739106472ccf930b2942eb810dd844f2b1ade7c
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sumologic"></a><span data-ttu-id="d0294-103">Oktatóanyag: Azure Active Directoryval integrált SumoLogic</span><span class="sxs-lookup"><span data-stu-id="d0294-103">Tutorial: Azure Active Directory integration with SumoLogic</span></span>

<span data-ttu-id="d0294-104">Ebben az oktatóanyagban elsajátíthatja SumoLogic integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="d0294-104">In this tutorial, you learn how to integrate SumoLogic with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="d0294-105">SumoLogic integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="d0294-105">Integrating SumoLogic with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="d0294-106">Megadhatja a SumoLogic hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="d0294-106">You can control in Azure AD who has access to SumoLogic</span></span>
- <span data-ttu-id="d0294-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett SumoLogic (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="d0294-107">You can enable your users to automatically get signed-on to SumoLogic (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="d0294-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="d0294-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="d0294-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="d0294-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d0294-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="d0294-110">Prerequisites</span></span>

<span data-ttu-id="d0294-111">Konfigurálása az Azure AD-integrációs SumoLogic, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="d0294-111">To configure Azure AD integration with SumoLogic, you need the following items:</span></span>

- <span data-ttu-id="d0294-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="d0294-112">An Azure AD subscription</span></span>
- <span data-ttu-id="d0294-113">Egy SumoLogic egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="d0294-113">A SumoLogic single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="d0294-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="d0294-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="d0294-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="d0294-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="d0294-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="d0294-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="d0294-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d0294-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="d0294-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="d0294-118">Scenario description</span></span>
<span data-ttu-id="d0294-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="d0294-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="d0294-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="d0294-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="d0294-121">A gyűjteményből SumoLogic hozzáadása</span><span class="sxs-lookup"><span data-stu-id="d0294-121">Adding SumoLogic from the gallery</span></span>
2. <span data-ttu-id="d0294-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="d0294-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-sumologic-from-the-gallery"></a><span data-ttu-id="d0294-123">A gyűjteményből SumoLogic hozzáadása</span><span class="sxs-lookup"><span data-stu-id="d0294-123">Adding SumoLogic from the gallery</span></span>
<span data-ttu-id="d0294-124">Az Azure AD integrálása a SumoLogic konfigurálásához kell hozzáadnia SumoLogic a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="d0294-124">To configure the integration of SumoLogic into Azure AD, you need to add SumoLogic from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="d0294-125">**A gyűjteményből SumoLogic hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="d0294-125">**To add SumoLogic from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="d0294-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="d0294-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="d0294-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="d0294-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="d0294-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="d0294-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="d0294-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="d0294-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="d0294-133">Írja be a keresőmezőbe, **SumoLogic**.</span><span class="sxs-lookup"><span data-stu-id="d0294-133">In the search box, type **SumoLogic**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sumologic-tutorial/tutorial_sumologic_search.png)

5. <span data-ttu-id="d0294-135">Az eredmények panelen válassza ki a **SumoLogic**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="d0294-135">In the results panel, select **SumoLogic**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sumologic-tutorial/tutorial_sumologic_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="d0294-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="d0294-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="d0294-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján SumoLogic.</span><span class="sxs-lookup"><span data-stu-id="d0294-138">In this section, you configure and test Azure AD single sign-on with SumoLogic based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="d0294-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó SumoLogic a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="d0294-139">For single sign-on to work, Azure AD needs to know what the counterpart user in SumoLogic is to a user in Azure AD.</span></span> <span data-ttu-id="d0294-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a SumoLogic közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="d0294-140">In other words, a link relationship between an Azure AD user and the related user in SumoLogic needs to be established.</span></span>

<span data-ttu-id="d0294-141">SumoLogic, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="d0294-141">In SumoLogic, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="d0294-142">Az Azure AD egyszeri bejelentkezést a SumoLogic tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="d0294-142">To configure and test Azure AD single sign-on with SumoLogic, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="d0294-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="d0294-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="d0294-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="d0294-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="d0294-145">**[SumoLogic tesztfelhasználó létrehozása](#creating-a-sumologic-test-user)**  - való Britta Simon valami SumoLogic, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="d0294-145">**[Creating a SumoLogic test user](#creating-a-sumologic-test-user)** - to have a counterpart of Britta Simon in SumoLogic that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="d0294-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="d0294-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="d0294-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="d0294-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="d0294-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d0294-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="d0294-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az SumoLogic alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="d0294-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your SumoLogic application.</span></span>

<span data-ttu-id="d0294-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés SumoLogic, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="d0294-150">**To configure Azure AD single sign-on with SumoLogic, perform the following steps:**</span></span>

1. <span data-ttu-id="d0294-151">Az Azure portálon a a **SumoLogic** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="d0294-151">In the Azure portal, on the **SumoLogic** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="d0294-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="d0294-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sumologic-tutorial/tutorial_sumologic_samlbase.png)

3. <span data-ttu-id="d0294-155">Az a **SumoLogic tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="d0294-155">On the **SumoLogic Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sumologic-tutorial/tutorial_sumologic_url.png)

    <span data-ttu-id="d0294-157">a.</span><span class="sxs-lookup"><span data-stu-id="d0294-157">a.</span></span> <span data-ttu-id="d0294-158">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<tenantname>.SumoLogic.com`</span><span class="sxs-lookup"><span data-stu-id="d0294-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<tenantname>.SumoLogic.com`</span></span>

    <span data-ttu-id="d0294-159">b.</span><span class="sxs-lookup"><span data-stu-id="d0294-159">b.</span></span> <span data-ttu-id="d0294-160">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:</span><span class="sxs-lookup"><span data-stu-id="d0294-160">In the **Identifier** textbox, type a URL using the following pattern:</span></span>
    | |
    |--|
    | `https://<tenantname>.us2.sumologic.com` |
    | `https://<tenantname>.sumologic.com` |
    | `https://<tenantname>.us4.sumologic.com` |
    | `https://<tenantname>.eu.sumologic.com` |
    | `https://<tenantname>.au.sumologic.com` |

    > [!NOTE] 
    > <span data-ttu-id="d0294-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="d0294-161">These values are not real.</span></span> <span data-ttu-id="d0294-162">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="d0294-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="d0294-163">Ügyfél [SumoLogic ügyfél-támogatási csoport](https://www.sumologic.com/contact-us/) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="d0294-163">Contact [SumoLogic Client support team](https://www.sumologic.com/contact-us/) to get these values.</span></span> 
 
4. <span data-ttu-id="d0294-164">A a **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="d0294-164">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sumologic-tutorial/tutorial_sumologic_certificate.png) 

5. <span data-ttu-id="d0294-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="d0294-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sumologic-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="d0294-168">A a **SumoLogic konfigurációs** kattintson **konfigurálása SumoLogic** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="d0294-168">On the **SumoLogic Configuration** section, click **Configure SumoLogic** to open **Configure sign-on** window.</span></span> <span data-ttu-id="d0294-169">Másolás a **SAML Entitásazonosító és SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="d0294-169">Copy the **SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sumologic-tutorial/tutorial_sumologic_configure.png) 

7. <span data-ttu-id="d0294-171">Egy másik webes böngészőablakban jelentkezzen be a SumoLogic vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="d0294-171">In a different web browser window, log in to your SumoLogic company site as an administrator.</span></span>

8. <span data-ttu-id="d0294-172">Ugrás a **kezelése \> biztonsági**.</span><span class="sxs-lookup"><span data-stu-id="d0294-172">Go to **Manage \> Security**.</span></span>
   
    <span data-ttu-id="d0294-173">![Kezelése](./media/active-directory-saas-sumologic-tutorial/ic778556.png "kezelése")</span><span class="sxs-lookup"><span data-stu-id="d0294-173">![Manage](./media/active-directory-saas-sumologic-tutorial/ic778556.png "Manage")</span></span>

9. <span data-ttu-id="d0294-174">Kattintson a **SAML**.</span><span class="sxs-lookup"><span data-stu-id="d0294-174">Click **SAML**.</span></span>
   
    <span data-ttu-id="d0294-175">![Globális biztonsági beállításokat](./media/active-directory-saas-sumologic-tutorial/ic778557.png "globális biztonsági beállításokat")</span><span class="sxs-lookup"><span data-stu-id="d0294-175">![Global security settings](./media/active-directory-saas-sumologic-tutorial/ic778557.png "Global security settings")</span></span>

10. <span data-ttu-id="d0294-176">Az a **konfiguráció válasszon vagy hozzon létre egy új** listára, válassza ki **az Azure AD**, és kattintson a **konfigurálása**.</span><span class="sxs-lookup"><span data-stu-id="d0294-176">From the **Select a configuration or create a new one** list, select **Azure AD**, and then click **Configure**.</span></span>
   
    <span data-ttu-id="d0294-177">![Konfigurálja a SAML 2.0](./media/active-directory-saas-sumologic-tutorial/ic778558.png "SAML 2.0 konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="d0294-177">![Configure SAML 2.0](./media/active-directory-saas-sumologic-tutorial/ic778558.png "Configure SAML 2.0")</span></span>

11. <span data-ttu-id="d0294-178">Az a **konfigurálása a SAML 2.0-s** párbeszédpanelen hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="d0294-178">On the **Configure SAML 2.0** dialog, perform the following steps:</span></span>
   
    <span data-ttu-id="d0294-179">![Konfigurálja a SAML 2.0](./media/active-directory-saas-sumologic-tutorial/ic778559.png "SAML 2.0 konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="d0294-179">![Configure SAML 2.0](./media/active-directory-saas-sumologic-tutorial/ic778559.png "Configure SAML 2.0")</span></span>
   
    <span data-ttu-id="d0294-180">a.</span><span class="sxs-lookup"><span data-stu-id="d0294-180">a.</span></span> <span data-ttu-id="d0294-181">Az a **Konfigurációnévvel** szövegmezőhöz típus **az Azure AD**.</span><span class="sxs-lookup"><span data-stu-id="d0294-181">In the **Configuration Name** textbox, type **Azure AD**.</span></span> 

    <span data-ttu-id="d0294-182">b.</span><span class="sxs-lookup"><span data-stu-id="d0294-182">b.</span></span> <span data-ttu-id="d0294-183">Válassza ki **hibakeresési mód**.</span><span class="sxs-lookup"><span data-stu-id="d0294-183">Select **Debug Mode**.</span></span>

    <span data-ttu-id="d0294-184">c.</span><span class="sxs-lookup"><span data-stu-id="d0294-184">c.</span></span> <span data-ttu-id="d0294-185">Az a **kibocsátó** szövegmezőhöz illessze be az értékét **SAML Entitásazonosító**, amely az Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="d0294-185">In the **Issuer** textbox, paste the value of **SAML Entity ID**, which you have copied from Azure portal.</span></span> 

    <span data-ttu-id="d0294-186">d.</span><span class="sxs-lookup"><span data-stu-id="d0294-186">d.</span></span> <span data-ttu-id="d0294-187">A a **Authn kérelem URL-cím** szövegmezőhöz illessze be az értékét **SAML-alapú egyszeri bejelentkezési URL-címe**, amely az Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="d0294-187">In the **Authn Request URL** textbox, paste the value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="d0294-188">e.</span><span class="sxs-lookup"><span data-stu-id="d0294-188">e.</span></span> <span data-ttu-id="d0294-189">Nyissa meg a base-64 kódolású tanúsítvány a Jegyzettömbben, annak tartalmának másolása a vágólapra és illessze be a teljes tanúsítványt **X.509 tanúsítvány** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="d0294-189">Open your base-64 encoded certificate in notepad, copy the content of it into your clipboard, and then paste the entire Certificate into **X.509 Certificate** textbox.</span></span>

    <span data-ttu-id="d0294-190">f.</span><span class="sxs-lookup"><span data-stu-id="d0294-190">f.</span></span> <span data-ttu-id="d0294-191">Mint **E-mail attribútum**, jelölje be **használható SAML tulajdonos**.</span><span class="sxs-lookup"><span data-stu-id="d0294-191">As **Email Attribute**, select **Use SAML subject**.</span></span>  

    <span data-ttu-id="d0294-192">g.</span><span class="sxs-lookup"><span data-stu-id="d0294-192">g.</span></span> <span data-ttu-id="d0294-193">Válassza ki **Szolgáltató kezdeményezett bejelentkezési konfigurációs**.</span><span class="sxs-lookup"><span data-stu-id="d0294-193">Select **SP initiated Login Configuration**.</span></span>

    <span data-ttu-id="d0294-194">h.</span><span class="sxs-lookup"><span data-stu-id="d0294-194">h.</span></span> <span data-ttu-id="d0294-195">Az a **bejelentkezési elérési** szövegmezőhöz típus **Azure** kattintson **mentése**.</span><span class="sxs-lookup"><span data-stu-id="d0294-195">In the **Login Path** textbox, type **Azure** and click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="d0294-196">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="d0294-196">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="d0294-197">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="d0294-197">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="d0294-198">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="d0294-198">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="d0294-199">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="d0294-199">Creating an Azure AD test user</span></span>
<span data-ttu-id="d0294-200">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="d0294-200">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="d0294-202">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="d0294-202">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="d0294-203">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="d0294-203">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sumologic-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="d0294-205">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="d0294-205">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sumologic-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="d0294-207">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="d0294-207">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sumologic-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="d0294-209">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="d0294-209">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sumologic-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="d0294-211">a.</span><span class="sxs-lookup"><span data-stu-id="d0294-211">a.</span></span> <span data-ttu-id="d0294-212">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="d0294-212">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="d0294-213">b.</span><span class="sxs-lookup"><span data-stu-id="d0294-213">b.</span></span> <span data-ttu-id="d0294-214">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="d0294-214">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="d0294-215">c.</span><span class="sxs-lookup"><span data-stu-id="d0294-215">c.</span></span> <span data-ttu-id="d0294-216">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="d0294-216">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="d0294-217">d.</span><span class="sxs-lookup"><span data-stu-id="d0294-217">d.</span></span> <span data-ttu-id="d0294-218">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="d0294-218">Click **Create**.</span></span>
 
### <a name="creating-a-sumologic-test-user"></a><span data-ttu-id="d0294-219">SumoLogic tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="d0294-219">Creating a SumoLogic test user</span></span>

<span data-ttu-id="d0294-220">Ahhoz, hogy az Azure AD-felhasználók SumoLogic bejelentkezni, akkor ki kell építenie SumoLogic.</span><span class="sxs-lookup"><span data-stu-id="d0294-220">In order to enable Azure AD users to log in to SumoLogic, they must be provisioned to SumoLogic.</span></span>  

* <span data-ttu-id="d0294-221">SumoLogic, ha egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="d0294-221">In the case of SumoLogic, provisioning is a manual task.</span></span>

<span data-ttu-id="d0294-222">**Felhasználói fiók létrehozásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="d0294-222">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="d0294-223">Jelentkezzen be a **SumoLogic** bérlő.</span><span class="sxs-lookup"><span data-stu-id="d0294-223">Log in to your **SumoLogic** tenant.</span></span>

2. <span data-ttu-id="d0294-224">Ugrás a **kezelése \> felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="d0294-224">Go to **Manage \> Users**.</span></span>
   
    <span data-ttu-id="d0294-225">![Felhasználók](./media/active-directory-saas-sumologic-tutorial/ic778561.png "felhasználók")</span><span class="sxs-lookup"><span data-stu-id="d0294-225">![Users](./media/active-directory-saas-sumologic-tutorial/ic778561.png "Users")</span></span>

3. <span data-ttu-id="d0294-226">Kattintson az **Add** (Hozzáadás) parancsra.</span><span class="sxs-lookup"><span data-stu-id="d0294-226">Click **Add**.</span></span>
   
    <span data-ttu-id="d0294-227">![Felhasználók](./media/active-directory-saas-sumologic-tutorial/ic778562.png "felhasználók")</span><span class="sxs-lookup"><span data-stu-id="d0294-227">![Users](./media/active-directory-saas-sumologic-tutorial/ic778562.png "Users")</span></span>

4. <span data-ttu-id="d0294-228">Az a **új felhasználó** párbeszédpanelen hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="d0294-228">On the **New User** dialog, perform the following steps:</span></span>
   
    <span data-ttu-id="d0294-229">![Új felhasználó](./media/active-directory-saas-sumologic-tutorial/ic778563.png "új felhasználó")</span><span class="sxs-lookup"><span data-stu-id="d0294-229">![New User](./media/active-directory-saas-sumologic-tutorial/ic778563.png "New User")</span></span> 
 
    <span data-ttu-id="d0294-230">a.</span><span class="sxs-lookup"><span data-stu-id="d0294-230">a.</span></span> <span data-ttu-id="d0294-231">Írja be a kapcsolódó információkat kiépítése a kívánt Azure AD-fiókot, a **Utónév**, **Vezetéknév**, és **E-mail** szövegmezőből.</span><span class="sxs-lookup"><span data-stu-id="d0294-231">Type the related information of the Azure AD account you want to provision into the **First Name**, **Last Name**, and **Email** textboxes.</span></span>
  
    <span data-ttu-id="d0294-232">b.</span><span class="sxs-lookup"><span data-stu-id="d0294-232">b.</span></span> <span data-ttu-id="d0294-233">Szerepkör kiválasztása.</span><span class="sxs-lookup"><span data-stu-id="d0294-233">Select a role.</span></span>
  
    <span data-ttu-id="d0294-234">c.</span><span class="sxs-lookup"><span data-stu-id="d0294-234">c.</span></span> <span data-ttu-id="d0294-235">Mint **állapot**, jelölje be **aktív**.</span><span class="sxs-lookup"><span data-stu-id="d0294-235">As **Status**, select **Active**.</span></span>
  
    <span data-ttu-id="d0294-236">d.</span><span class="sxs-lookup"><span data-stu-id="d0294-236">d.</span></span> <span data-ttu-id="d0294-237">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="d0294-237">Click **Save**.</span></span>

>[!NOTE]
><span data-ttu-id="d0294-238">Bármely más SumoLogic felhasználói fiók létrehozása eszközök vagy rendelkezés AAD felhasználói fiókokhoz SumoLogic által nyújtott API-k.</span><span class="sxs-lookup"><span data-stu-id="d0294-238">You can use any other SumoLogic user account creation tools or APIs provided by SumoLogic to provision AAD user accounts.</span></span> 
> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="d0294-239">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="d0294-239">Assigning the Azure AD test user</span></span>

<span data-ttu-id="d0294-240">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés SumoLogic Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="d0294-240">In this section, you enable Britta Simon to use Azure single sign-on by granting access to SumoLogic.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="d0294-242">**Britta Simon hozzárendelése SumoLogic, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="d0294-242">**To assign Britta Simon to SumoLogic, perform the following steps:**</span></span>

1. <span data-ttu-id="d0294-243">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="d0294-243">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="d0294-245">Az alkalmazások listában válassza ki a **SumoLogic**.</span><span class="sxs-lookup"><span data-stu-id="d0294-245">In the applications list, select **SumoLogic**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sumologic-tutorial/tutorial_sumologic_app.png) 

3. <span data-ttu-id="d0294-247">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="d0294-247">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="d0294-249">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="d0294-249">Click **Add** button.</span></span> <span data-ttu-id="d0294-250">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d0294-250">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="d0294-252">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="d0294-252">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="d0294-253">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d0294-253">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="d0294-254">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d0294-254">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="d0294-255">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="d0294-255">Testing single sign-on</span></span>

<span data-ttu-id="d0294-256">Ez a szakasz célja tesztelése az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen.</span><span class="sxs-lookup"><span data-stu-id="d0294-256">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="d0294-257">Ha a hozzáférési panelen SumoLogic csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az SumoLogic alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="d0294-257">When you click the SumoLogic tile in the Access Panel, you should get automatically signed-on to your SumoLogic application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d0294-258">További források</span><span class="sxs-lookup"><span data-stu-id="d0294-258">Additional resources</span></span>

* [<span data-ttu-id="d0294-259">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="d0294-259">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="d0294-260">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="d0294-260">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-sumologic-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sumologic-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sumologic-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sumologic-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sumologic-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sumologic-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sumologic-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sumologic-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sumologic-tutorial/tutorial_general_203.png

