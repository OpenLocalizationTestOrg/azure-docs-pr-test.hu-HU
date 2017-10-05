---
title: "Oktatóanyag: Azure Active Directoryval integrált Domo |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Domo között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 058626e4-73b3-4dc2-86ca-b060d002d70a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/11/2017
ms.author: jeedes
ms.openlocfilehash: 919d2262cf9f14159a13370037301005b5b69da2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-domo"></a><span data-ttu-id="54e81-103">Oktatóanyag: Azure Active Directoryval integrált Domo</span><span class="sxs-lookup"><span data-stu-id="54e81-103">Tutorial: Azure Active Directory integration with Domo</span></span>

<span data-ttu-id="54e81-104">Ebben az oktatóanyagban elsajátíthatja Domo integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="54e81-104">In this tutorial, you learn how to integrate Domo with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="54e81-105">Domo integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="54e81-105">Integrating Domo with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="54e81-106">Megadhatja a Domo hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="54e81-106">You can control in Azure AD who has access to Domo</span></span>
- <span data-ttu-id="54e81-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett Domo (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="54e81-107">You can enable your users to automatically get signed-on to Domo (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="54e81-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="54e81-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="54e81-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="54e81-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="54e81-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="54e81-110">Prerequisites</span></span>

<span data-ttu-id="54e81-111">Konfigurálása az Azure AD-integrációs Domo, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="54e81-111">To configure Azure AD integration with Domo, you need the following items:</span></span>

- <span data-ttu-id="54e81-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="54e81-112">An Azure AD subscription</span></span>
- <span data-ttu-id="54e81-113">Egy Domo egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="54e81-113">A Domo single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="54e81-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="54e81-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="54e81-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="54e81-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="54e81-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="54e81-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="54e81-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="54e81-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="54e81-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="54e81-118">Scenario description</span></span>
<span data-ttu-id="54e81-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="54e81-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="54e81-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="54e81-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="54e81-121">A gyűjteményből Domo hozzáadása</span><span class="sxs-lookup"><span data-stu-id="54e81-121">Adding Domo from the gallery</span></span>
2. <span data-ttu-id="54e81-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="54e81-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-domo-from-the-gallery"></a><span data-ttu-id="54e81-123">A gyűjteményből Domo hozzáadása</span><span class="sxs-lookup"><span data-stu-id="54e81-123">Adding Domo from the gallery</span></span>
<span data-ttu-id="54e81-124">Az Azure AD integrálása a Domo konfigurálásához kell hozzáadnia Domo a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="54e81-124">To configure the integration of Domo into Azure AD, you need to add Domo from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="54e81-125">**A gyűjteményből Domo hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="54e81-125">**To add Domo from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="54e81-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="54e81-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="54e81-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="54e81-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="54e81-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="54e81-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="54e81-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="54e81-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="54e81-133">Írja be a keresőmezőbe, **Domo**.</span><span class="sxs-lookup"><span data-stu-id="54e81-133">In the search box, type **Domo**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-domo-tutorial/tutorial_domo_search.png)

5. <span data-ttu-id="54e81-135">Az eredmények panelen válassza ki a **Domo**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="54e81-135">In the results panel, select **Domo**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-domo-tutorial/tutorial_domo_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="54e81-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="54e81-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="54e81-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján Domo</span><span class="sxs-lookup"><span data-stu-id="54e81-138">In this section, you configure and test Azure AD single sign-on with Domo based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="54e81-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Domo a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="54e81-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Domo is to a user in Azure AD.</span></span> <span data-ttu-id="54e81-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Domo közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="54e81-140">In other words, a link relationship between an Azure AD user and the related user in Domo needs to be established.</span></span>

<span data-ttu-id="54e81-141">Domo, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="54e81-141">In Domo, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="54e81-142">Az Azure AD egyszeri bejelentkezést a Domo tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="54e81-142">To configure and test Azure AD single sign-on with Domo, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="54e81-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="54e81-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="54e81-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="54e81-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="54e81-145">**[Domo tesztfelhasználó létrehozása](#creating-a-domo-test-user)**  - való Britta Simon valami Domo, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="54e81-145">**[Creating a Domo test user](#creating-a-domo-test-user)** - to have a counterpart of Britta Simon in Domo that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="54e81-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="54e81-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="54e81-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="54e81-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="54e81-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="54e81-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="54e81-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Domo alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="54e81-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Domo application.</span></span>

<span data-ttu-id="54e81-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés Domo, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="54e81-150">**To configure Azure AD single sign-on with Domo, perform the following steps:**</span></span>

1. <span data-ttu-id="54e81-151">Az Azure portálon a a **Domo** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="54e81-151">In the Azure portal, on the **Domo** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="54e81-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="54e81-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-domo-tutorial/tutorial_domo_samlbase.png)

3. <span data-ttu-id="54e81-155">Az a **Domo tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="54e81-155">On the **Domo Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-domo-tutorial/tutorial_domo_url.png)

    <span data-ttu-id="54e81-157">a.</span><span class="sxs-lookup"><span data-stu-id="54e81-157">a.</span></span> <span data-ttu-id="54e81-158">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<companyname>.domo.com`</span><span class="sxs-lookup"><span data-stu-id="54e81-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.domo.com`</span></span>

    <span data-ttu-id="54e81-159">b.</span><span class="sxs-lookup"><span data-stu-id="54e81-159">b.</span></span> <span data-ttu-id="54e81-160">Az a **azonosító** szövegmezőhöz URL-címet a következő minták használatával írja be:</span><span class="sxs-lookup"><span data-stu-id="54e81-160">In the **Identifier** textbox, type a URL using the following patterns:</span></span>     

    | |
    |--|    
    | `https://<companyname>.domo.com` |
    | `https://<companyname>.beta.domo.com` |
    | `https://<companyname>.demo.domo.com` |
    | `https://<companyname>.dev.domo.com` | 
    | `https://<companyname>.fastage1.domo.com` |       
    | `https://<companyname>.frdev.domo.com` |       
    | `https://<companyname>.gastage.domo.com` |       
    | `https://<companyname>.load.domo.com` |       
    | `https://<companyname>.local.domo.com` |       
    | `https://<companyname>.qa.domo.com` |
    | `https://<companyname>.stage.domo.com` |
    
    > [!NOTE] 
    > <span data-ttu-id="54e81-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="54e81-161">These values are not real.</span></span> <span data-ttu-id="54e81-162">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="54e81-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="54e81-163">Ügyfél [Domo ügyfél-támogatási csoport](mailto:support@domo.com) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="54e81-163">Contact [Domo Client support team](mailto:support@domo.com) to get these values.</span></span>

4. <span data-ttu-id="54e81-164">Domo alkalmazás vár a SAML helyességi feltételek egy meghatározott formátumban.</span><span class="sxs-lookup"><span data-stu-id="54e81-164">Domo application expects the SAML assertions in a specific format.</span></span> <span data-ttu-id="54e81-165">A következő jogcímek alkalmazás konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="54e81-165">Configure the following claims for this application.</span></span> <span data-ttu-id="54e81-166">Ezek az attribútumok értékének kezelheti a "**felhasználói attribútumok**" szakasz alkalmazás integráció lapján.</span><span class="sxs-lookup"><span data-stu-id="54e81-166">You can manage the values of these attributes from the "**User Attributes**" section on application integration page.</span></span> <span data-ttu-id="54e81-167">Az alábbi képernyőfelvételen látható egy példa ehhez a konfigurációhoz.</span><span class="sxs-lookup"><span data-stu-id="54e81-167">The following screenshot shows an example for this configuration.</span></span> 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-domo-tutorial/tutorial_domo_attributes.png)
    
5. <span data-ttu-id="54e81-169">A a **felhasználói attribútumok** a szakasz a **egyszeri bejelentkezés** párbeszédpanelen konfigurálja a SAML-jogkivonat attribútum, az ábrán látható módon, és hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="54e81-169">In the **User Attributes** section on the **Single sign-on** dialog, configure SAML token attribute as shown in the image and perform the following steps:</span></span>
    
    | <span data-ttu-id="54e81-170">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="54e81-170">Attribute Name</span></span> | <span data-ttu-id="54e81-171">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="54e81-171">Attribute Value</span></span> |
    | ------------------- | -------------------- |    
    | <span data-ttu-id="54e81-172">név</span><span class="sxs-lookup"><span data-stu-id="54e81-172">name</span></span> | <span data-ttu-id="54e81-173">User.DisplayName</span><span class="sxs-lookup"><span data-stu-id="54e81-173">user.displayname</span></span> |
    | <span data-ttu-id="54e81-174">E-mailek</span><span class="sxs-lookup"><span data-stu-id="54e81-174">email</span></span> | <span data-ttu-id="54e81-175">User.mail</span><span class="sxs-lookup"><span data-stu-id="54e81-175">user.mail</span></span> |
    
    <span data-ttu-id="54e81-176">a.</span><span class="sxs-lookup"><span data-stu-id="54e81-176">a.</span></span> <span data-ttu-id="54e81-177">Kattintson a **Hozzáadás attribútum** megnyitásához a **attribútum hozzáadása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="54e81-177">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-domo-tutorial/tutorial_attribute_04.png)

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-domo-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="54e81-180">b.</span><span class="sxs-lookup"><span data-stu-id="54e81-180">b.</span></span> <span data-ttu-id="54e81-181">Az a **neve** szövegmező, írja be az adott sorhoz feltüntetett attribútumot nevét.</span><span class="sxs-lookup"><span data-stu-id="54e81-181">In the **Name** textbox, type the attribute name shown for that row.</span></span>
    
    <span data-ttu-id="54e81-182">c.</span><span class="sxs-lookup"><span data-stu-id="54e81-182">c.</span></span> <span data-ttu-id="54e81-183">Az a **érték** kilistázásához írja be a sorhoz látható attribútum értéke.</span><span class="sxs-lookup"><span data-stu-id="54e81-183">From the **Value** list, type the attribute value shown for that row.</span></span>
    
    <span data-ttu-id="54e81-184">d.</span><span class="sxs-lookup"><span data-stu-id="54e81-184">d.</span></span> <span data-ttu-id="54e81-185">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="54e81-185">Click **Ok**.</span></span> 
 
6. <span data-ttu-id="54e81-186">Az a **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="54e81-186">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-domo-tutorial/tutorial_domo_certificate.png) 

7. <span data-ttu-id="54e81-188">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="54e81-188">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-domo-tutorial/tutorial_general_400.png)


8. <span data-ttu-id="54e81-190">A a **Domo konfigurációs** kattintson **konfigurálása Domo** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="54e81-190">On the **Domo Configuration** section, click **Configure Domo** to open **Configure sign-on** window.</span></span> <span data-ttu-id="54e81-191">Másolás a **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="54e81-191">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>   

   ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-domo-tutorial/tutorial_domo_configure.png) 

9. <span data-ttu-id="54e81-193">Egyszeri bejelentkezés konfigurálása **Domo** oldalon kell küldeniük a letöltött **tanúsítvány**, **SAML Entitásazonosító**, a **SAML-alapú egyszeri bejelentkezési URL-címe** és a **Sign-Out URL-cím** való [Domo támogatási csoport](mailto:support@domo.com).</span><span class="sxs-lookup"><span data-stu-id="54e81-193">To configure single sign-on on **Domo** side, you need to send the downloaded **Certificate**, **SAML Entity ID**, the **SAML Single Sign-On Service URL** and the **Sign-Out URL** to [Domo support team](mailto:support@domo.com).</span></span> <span data-ttu-id="54e81-194">Akkor állítsa be ezt a beállítást, hogy a SAML SSO kapcsolat mindkét oldalán megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="54e81-194">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="54e81-195">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="54e81-195">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="54e81-196">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="54e81-196">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="54e81-197">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="54e81-197">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="54e81-198">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="54e81-198">Creating an Azure AD test user</span></span>
<span data-ttu-id="54e81-199">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="54e81-199">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="54e81-201">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="54e81-201">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="54e81-202">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="54e81-202">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-domo-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="54e81-204">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="54e81-204">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-domo-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="54e81-206">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="54e81-206">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-domo-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="54e81-208">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="54e81-208">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-domo-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="54e81-210">a.</span><span class="sxs-lookup"><span data-stu-id="54e81-210">a.</span></span> <span data-ttu-id="54e81-211">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="54e81-211">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="54e81-212">b.</span><span class="sxs-lookup"><span data-stu-id="54e81-212">b.</span></span> <span data-ttu-id="54e81-213">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="54e81-213">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="54e81-214">c.</span><span class="sxs-lookup"><span data-stu-id="54e81-214">c.</span></span> <span data-ttu-id="54e81-215">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="54e81-215">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="54e81-216">d.</span><span class="sxs-lookup"><span data-stu-id="54e81-216">d.</span></span> <span data-ttu-id="54e81-217">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="54e81-217">Click **Create**.</span></span>
 
### <a name="creating-a-domo-test-user"></a><span data-ttu-id="54e81-218">Domo tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="54e81-218">Creating a Domo test user</span></span>

<span data-ttu-id="54e81-219">Ez a szakasz célja Domo Britta Simon nevű felhasználót létrehozni.</span><span class="sxs-lookup"><span data-stu-id="54e81-219">The objective of this section is to create a user called Britta Simon in Domo.</span></span> <span data-ttu-id="54e81-220">Domo támogatja just-in-time kiosztást, amely alapértelmezés szerint van engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="54e81-220">Domo supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="54e81-221">Nincs ebben a szakaszban az Ön művelet elem.</span><span class="sxs-lookup"><span data-stu-id="54e81-221">There is no action item for you in this section.</span></span> <span data-ttu-id="54e81-222">Új felhasználó jön létre az Domo elérésére, ha még nem létezik tett kísérlet során.</span><span class="sxs-lookup"><span data-stu-id="54e81-222">A new user is created during an attempt to access Domo if it doesn't exist yet.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="54e81-223">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="54e81-223">Assigning the Azure AD test user</span></span>

<span data-ttu-id="54e81-224">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Domo Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="54e81-224">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Domo.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="54e81-226">**Britta Simon hozzárendelése Domo, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="54e81-226">**To assign Britta Simon to Domo, perform the following steps:**</span></span>

1. <span data-ttu-id="54e81-227">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="54e81-227">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="54e81-229">Az alkalmazások listában válassza ki a **Domo**.</span><span class="sxs-lookup"><span data-stu-id="54e81-229">In the applications list, select **Domo**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-domo-tutorial/tutorial_domo_app.png) 

3. <span data-ttu-id="54e81-231">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="54e81-231">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="54e81-233">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="54e81-233">Click **Add** button.</span></span> <span data-ttu-id="54e81-234">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="54e81-234">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="54e81-236">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="54e81-236">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="54e81-237">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="54e81-237">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="54e81-238">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="54e81-238">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="54e81-239">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="54e81-239">Testing single sign-on</span></span>

<span data-ttu-id="54e81-240">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="54e81-240">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>
<span data-ttu-id="54e81-241">Ha a hozzáférési panelen Domo csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az Domo alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="54e81-241">When you click the Domo tile in the Access Panel, you should get automatically signed-on to your Domo application.</span></span>

<span data-ttu-id="54e81-242">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="54e81-242">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="54e81-243">További források</span><span class="sxs-lookup"><span data-stu-id="54e81-243">Additional resources</span></span>

* [<span data-ttu-id="54e81-244">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="54e81-244">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="54e81-245">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="54e81-245">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-domo-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-domo-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-domo-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-domo-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-domo-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-domo-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-domo-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-domo-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-domo-tutorial/tutorial_general_203.png

