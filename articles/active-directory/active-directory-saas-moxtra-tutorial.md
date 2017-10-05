---
title: "Oktatóanyag: Azure Active Directoryval integrált Moxtra |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Moxtra között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 2aed2d4b-1dcd-4839-8fed-9419d107c61c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: jeedes
ms.openlocfilehash: db2f041a44b6771b0a4f734e58d899298ef0847b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-moxtra"></a><span data-ttu-id="9ccc1-103">Oktatóanyag: Azure Active Directoryval integrált Moxtra</span><span class="sxs-lookup"><span data-stu-id="9ccc1-103">Tutorial: Azure Active Directory integration with Moxtra</span></span>

<span data-ttu-id="9ccc1-104">Ebben az oktatóanyagban elsajátíthatja Moxtra integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="9ccc1-104">In this tutorial, you learn how to integrate Moxtra with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="9ccc1-105">Moxtra integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="9ccc1-105">Integrating Moxtra with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="9ccc1-106">Megadhatja a Moxtra hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="9ccc1-106">You can control in Azure AD who has access to Moxtra</span></span>
- <span data-ttu-id="9ccc1-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett Moxtra (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="9ccc1-107">You can enable your users to automatically get signed-on to Moxtra (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="9ccc1-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="9ccc1-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="9ccc1-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="9ccc1-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9ccc1-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="9ccc1-110">Prerequisites</span></span>

<span data-ttu-id="9ccc1-111">Konfigurálása az Azure AD-integrációs Moxtra, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="9ccc1-111">To configure Azure AD integration with Moxtra, you need the following items:</span></span>

- <span data-ttu-id="9ccc1-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="9ccc1-112">An Azure AD subscription</span></span>
- <span data-ttu-id="9ccc1-113">Egy Moxtra egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="9ccc1-113">A Moxtra single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="9ccc1-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="9ccc1-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="9ccc1-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="9ccc1-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="9ccc1-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="9ccc1-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="9ccc1-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="9ccc1-118">Scenario description</span></span>
<span data-ttu-id="9ccc1-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="9ccc1-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="9ccc1-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="9ccc1-121">A gyűjteményből Moxtra hozzáadása</span><span class="sxs-lookup"><span data-stu-id="9ccc1-121">Adding Moxtra from the gallery</span></span>
2. <span data-ttu-id="9ccc1-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="9ccc1-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-moxtra-from-the-gallery"></a><span data-ttu-id="9ccc1-123">A gyűjteményből Moxtra hozzáadása</span><span class="sxs-lookup"><span data-stu-id="9ccc1-123">Adding Moxtra from the gallery</span></span>
<span data-ttu-id="9ccc1-124">Az Azure AD integrálása a Moxtra konfigurálásához kell hozzáadnia Moxtra a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-124">To configure the integration of Moxtra into Azure AD, you need to add Moxtra from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="9ccc1-125">**A gyűjteményből Moxtra hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="9ccc1-125">**To add Moxtra from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="9ccc1-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="9ccc1-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="9ccc1-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="9ccc1-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="9ccc1-133">Írja be a keresőmezőbe, **Moxtra**.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-133">In the search box, type **Moxtra**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_search.png)

5. <span data-ttu-id="9ccc1-135">Az eredmények panelen válassza ki a **Moxtra**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-135">In the results panel, select **Moxtra**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="9ccc1-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="9ccc1-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="9ccc1-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Moxtra.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-138">In this section, you configure and test Azure AD single sign-on with Moxtra based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="9ccc1-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Moxtra a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Moxtra is to a user in Azure AD.</span></span> <span data-ttu-id="9ccc1-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Moxtra közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-140">In other words, a link relationship between an Azure AD user and the related user in Moxtra needs to be established.</span></span>

<span data-ttu-id="9ccc1-141">Moxtra, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-141">In Moxtra, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="9ccc1-142">Az Azure AD egyszeri bejelentkezést a Moxtra tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="9ccc1-142">To configure and test Azure AD single sign-on with Moxtra, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="9ccc1-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="9ccc1-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="9ccc1-145">**[Moxtra tesztfelhasználó létrehozása](#creating-a-moxtra-test-user)**  - való Britta Simon valami Moxtra, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-145">**[Creating a Moxtra test user](#creating-a-moxtra-test-user)** - to have a counterpart of Britta Simon in Moxtra that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="9ccc1-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="9ccc1-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="9ccc1-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="9ccc1-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="9ccc1-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Moxtra alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Moxtra application.</span></span>

<span data-ttu-id="9ccc1-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés Moxtra, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="9ccc1-150">**To configure Azure AD single sign-on with Moxtra, perform the following steps:**</span></span>

1. <span data-ttu-id="9ccc1-151">Az Azure portálon a a **Moxtra** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-151">In the Azure portal, on the **Moxtra** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="9ccc1-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_samlbase.png)

3. <span data-ttu-id="9ccc1-155">Az a **Moxtra tartomány és az URL-címek** csoportjában hajtsa végre a következő lépést:</span><span class="sxs-lookup"><span data-stu-id="9ccc1-155">On the **Moxtra Domain and URLs** section, perform the following step:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_url.png)

    <span data-ttu-id="9ccc1-157">Az a **bejelentkezési URL-cím** szövegmező, adja meg az URL-címet:`https://www.moxtra.com/service/#login`</span><span class="sxs-lookup"><span data-stu-id="9ccc1-157">In the **Sign-on URL** textbox, type a URL as: `https://www.moxtra.com/service/#login`</span></span>

4. <span data-ttu-id="9ccc1-158">Moxtra alkalmazás vár a SAML helyességi feltételek egy meghatározott formátumban.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-158">Moxtra application expects the SAML assertions in a specific format.</span></span> <span data-ttu-id="9ccc1-159">A következő jogcímek alkalmazás konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-159">Configure the following claims for this application.</span></span> <span data-ttu-id="9ccc1-160">Ezek az attribútumok értékének kezelheti a "**felhasználói attribútumok**" szakasz alkalmazás integráció lapján.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-160">You can manage the values of these attributes from the "**User Attributes**" section on application integration page.</span></span> <span data-ttu-id="9ccc1-161">Az alábbi képernyőfelvételen látható egy példa ehhez a konfigurációhoz.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-161">The following screenshot shows an example for this configuration.</span></span> 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_attributes.png)
    
5. <span data-ttu-id="9ccc1-163">A a **felhasználói attribútumok** a szakasz a **egyszeri bejelentkezés** párbeszédpanelen konfigurálja a SAML-jogkivonat attribútum, az ábrán látható módon, és hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="9ccc1-163">In the **User Attributes** section on the **Single sign-on** dialog, configure SAML token attribute as shown in the image and perform the following steps:</span></span>
    
    | <span data-ttu-id="9ccc1-164">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="9ccc1-164">Attribute Name</span></span> | <span data-ttu-id="9ccc1-165">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="9ccc1-165">Attribute Value</span></span> |
    | ------------------- | -------------------- |    
    | <span data-ttu-id="9ccc1-166">Utónév</span><span class="sxs-lookup"><span data-stu-id="9ccc1-166">firstname</span></span> | <span data-ttu-id="9ccc1-167">User.givenName</span><span class="sxs-lookup"><span data-stu-id="9ccc1-167">user.givenname</span></span> |
    | <span data-ttu-id="9ccc1-168">Vezetéknév</span><span class="sxs-lookup"><span data-stu-id="9ccc1-168">lastname</span></span> | <span data-ttu-id="9ccc1-169">User.surname</span><span class="sxs-lookup"><span data-stu-id="9ccc1-169">user.surname</span></span> |
    | <span data-ttu-id="9ccc1-170">idpid</span><span class="sxs-lookup"><span data-stu-id="9ccc1-170">idpid</span></span>    | <span data-ttu-id="9ccc1-171">< SAML entitás azonosítója ></span><span class="sxs-lookup"><span data-stu-id="9ccc1-171">< SAML Entity ID ></span></span> 

    > [!Note]
    > <span data-ttu-id="9ccc1-172">Értékének **idpid** attribútum nincs valós.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-172">The value of **idpid** attribute is not real.</span></span> <span data-ttu-id="9ccc1-173">A tényleges érték kaphat **rövid összefoglaló** szakaszában **Moxtra konfigurációs**.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-173">You can get the actual value from **Quick reference** section under **Moxtra Configuration**.</span></span>
    
    <span data-ttu-id="9ccc1-174">a.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-174">a.</span></span> <span data-ttu-id="9ccc1-175">Kattintson a **Hozzáadás attribútum** megnyitásához a **attribútum hozzáadása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-175">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-moxtra-tutorial/tutorial_attribute_04.png)

    <span data-ttu-id="9ccc1-177">b.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-177">b.</span></span> <span data-ttu-id="9ccc1-178">Az a **neve** szövegmező, írja be az adott sorhoz feltüntetett attribútumot nevét.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-178">In the **Name** textbox, type the attribute name shown for that row.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-moxtra-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="9ccc1-180">c.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-180">c.</span></span> <span data-ttu-id="9ccc1-181">Az a **érték** kilistázásához írja be a sorhoz látható attribútum értéke.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-181">From the **Value** list, type the attribute value shown for that row.</span></span>

    <span data-ttu-id="9ccc1-182">d.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-182">d.</span></span> <span data-ttu-id="9ccc1-183">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-183">Click **Ok**.</span></span>
    
5. <span data-ttu-id="9ccc1-184">Az a **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-184">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_certificate.png) 

6. <span data-ttu-id="9ccc1-186">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-186">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-moxtra-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="9ccc1-188">A a **Moxtra konfigurációs** kattintson **konfigurálása Moxtra** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-188">On the **Moxtra Configuration** section, click **Configure Moxtra** to open **Configure sign-on** window.</span></span> <span data-ttu-id="9ccc1-189">Másolás a **SAML Entitásazonosító és SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="9ccc1-189">Copy the **SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_configure.png) 

8. <span data-ttu-id="9ccc1-191">Egy másik böngészőablakban jelentkezzen be a Moxtra vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-191">In another browser window, sign on to your Moxtra company site as an administrator.</span></span>

9. <span data-ttu-id="9ccc1-192">A bal oldali eszköztárán kattintson **felügyeleti konzol > SAML-alapú egyszeri bejelentkezést**, és kattintson a **új**.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-192">In the toolbar on the left, click **Admin Console > SAML Single Sign-on**, and then click **New**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_06.png) 

10. <span data-ttu-id="9ccc1-194">Az a **SAML** lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="9ccc1-194">On the **SAML** page, perform the following steps:</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_08.png)   
 
    <span data-ttu-id="9ccc1-196">a.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-196">a.</span></span> <span data-ttu-id="9ccc1-197">Az a **neve** szövegmező, adja meg a konfiguráció nevét (pl.: *SAML*).</span><span class="sxs-lookup"><span data-stu-id="9ccc1-197">In the **Name** textbox, type a name for your configuration (e.g.: *SAML*).</span></span> 
  
    <span data-ttu-id="9ccc1-198">b.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-198">b.</span></span> <span data-ttu-id="9ccc1-199">Az a **IdP Entitásazonosító** szövegmezőhöz illessze be az értékét **SAML Entitásazonosító** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-199">In the **IdP Entity ID** textbox, paste the value of **SAML Entity ID** which you have copied from Azure portal.</span></span> 
 
    <span data-ttu-id="9ccc1-200">c.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-200">c.</span></span> <span data-ttu-id="9ccc1-201">A **bejelentkezési URL-cím** szövegmezőhöz illessze be az értékét **SAML-alapú egyszeri bejelentkezési URL-címe** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-201">In **Login URL** textbox, paste the value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span> 
 
    <span data-ttu-id="9ccc1-202">d.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-202">d.</span></span> <span data-ttu-id="9ccc1-203">Az a **AuthnContextClassRef** szövegmezőhöz típus **urn: oasis: nevek: tc: SAML:2.0:ac:classes:Password**.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-203">In the **AuthnContextClassRef** textbox, type **urn:oasis:names:tc:SAML:2.0:ac:classes:Password**.</span></span> 
 
    <span data-ttu-id="9ccc1-204">e.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-204">e.</span></span> <span data-ttu-id="9ccc1-205">Az a **NameID formátum** szövegmezőhöz típus **urn: oasis: nevek: tc: SAML:1.1:nameid-formátum: emailAddress**.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-205">In the **NameID Format** textbox, type **urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress**.</span></span> 
 
    <span data-ttu-id="9ccc1-206">f.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-206">f.</span></span> <span data-ttu-id="9ccc1-207">Nyissa meg a tanúsítványt, amely a Jegyzettömbben az Azure portálról letöltött másolja a tartalmat, és illessze be azt a **tanúsítvány** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-207">Open certificate which you have downloaded from Azure portal in notepad, copy the content, and then paste it into the **Certificate** textbox.</span></span>    
 
    <span data-ttu-id="9ccc1-208">g.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-208">g.</span></span> <span data-ttu-id="9ccc1-209">A SAML-alapú e-mail tartomány szövegmezőben írja be a SAML-alapú e-mail tartománya.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-209">In the SAML email domain textbox, type your SAML email domain.</span></span>    
  
    >[!NOTE]
    ><span data-ttu-id="9ccc1-210">A tartomány ellenőrzésének lépéseit megtekintéséhez kattintson a "**i**" alatt.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-210">To see the steps to verify the domain, click the "**i**" below.</span></span>

    <span data-ttu-id="9ccc1-211">h.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-211">h.</span></span> <span data-ttu-id="9ccc1-212">Kattintson a **frissítés**.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-212">Click **Update**.</span></span>

> [!TIP]
> <span data-ttu-id="9ccc1-213">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="9ccc1-213">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="9ccc1-214">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-214">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="9ccc1-215">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="9ccc1-215">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="9ccc1-216">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="9ccc1-216">Creating an Azure AD test user</span></span>
<span data-ttu-id="9ccc1-217">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-217">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="9ccc1-219">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="9ccc1-219">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="9ccc1-220">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-220">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-moxtra-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="9ccc1-222">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-222">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-moxtra-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="9ccc1-224">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-224">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-moxtra-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="9ccc1-226">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="9ccc1-226">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-moxtra-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="9ccc1-228">a.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-228">a.</span></span> <span data-ttu-id="9ccc1-229">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-229">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="9ccc1-230">b.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-230">b.</span></span> <span data-ttu-id="9ccc1-231">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-231">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="9ccc1-232">c.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-232">c.</span></span> <span data-ttu-id="9ccc1-233">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-233">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="9ccc1-234">d.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-234">d.</span></span> <span data-ttu-id="9ccc1-235">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-235">Click **Create**.</span></span>
 
### <a name="creating-a-moxtra-test-user"></a><span data-ttu-id="9ccc1-236">Moxtra tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="9ccc1-236">Creating a Moxtra test user</span></span>

<span data-ttu-id="9ccc1-237">Ez a szakasz célja Moxtra Britta Simon nevű felhasználót létrehozni.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-237">The objective of this section is to create a user called Britta Simon in Moxtra.</span></span>

<span data-ttu-id="9ccc1-238">**A felhasználó Britta Simon meghívta Moxtra létrehozásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="9ccc1-238">**To create a user called Britta Simon in Moxtra, perform the following steps:**</span></span>

1. <span data-ttu-id="9ccc1-239">Jelentkezzen be rendszergazdaként a Moxtra vállalati webhely.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-239">Sign on to your Moxtra company site as an administrator.</span></span>

2. <span data-ttu-id="9ccc1-240">A bal oldali eszköztárán kattintson **felügyeleti konzol > felhasználók kezelése**, majd **felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-240">In the toolbar on the left, click **Admin Console > User Management**, and then **Add User**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_10.png) 

3. <span data-ttu-id="9ccc1-242">Az a **felhasználó hozzáadása** párbeszédpanelen hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="9ccc1-242">On the **Add User** dialog, perform the following steps:</span></span>
  
    <span data-ttu-id="9ccc1-243">a.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-243">a.</span></span> <span data-ttu-id="9ccc1-244">Az a **Utónév** szövegmezőhöz típus **Britta**.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-244">In the **First Name** textbox, type **Britta**.</span></span>
  
    <span data-ttu-id="9ccc1-245">b.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-245">b.</span></span> <span data-ttu-id="9ccc1-246">Az a **Vezetéknév** szövegmezőhöz típus **Simon**.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-246">In the **Last Name** textbox, type **Simon**.</span></span>
  
    <span data-ttu-id="9ccc1-247">c.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-247">c.</span></span> <span data-ttu-id="9ccc1-248">Az a **E-mail** szövegmezőhöz Britta tartozó e-mail cím megegyezik az Azure-portál típusa.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-248">In the **Email** textbox, type Britta's email address same as on Azure portal.</span></span>
  
    <span data-ttu-id="9ccc1-249">d.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-249">d.</span></span> <span data-ttu-id="9ccc1-250">Az a **osztás** szövegmezőhöz típus **fejlesztői**.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-250">In the **Division** textbox, type **Dev**.</span></span>
  
    <span data-ttu-id="9ccc1-251">e.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-251">e.</span></span> <span data-ttu-id="9ccc1-252">Az a **részleg** szövegmezőhöz típus **informatikai**.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-252">In the **Department** textbox, type **IT**.</span></span>
  
    <span data-ttu-id="9ccc1-253">f.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-253">f.</span></span> <span data-ttu-id="9ccc1-254">Válassza ki **rendszergazda**.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-254">Select **Administrator**.</span></span>
  
    <span data-ttu-id="9ccc1-255">g.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-255">g.</span></span> <span data-ttu-id="9ccc1-256">Kattintson az **Add** (Hozzáadás) parancsra.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-256">Click **Add**.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="9ccc1-257">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="9ccc1-257">Assigning the Azure AD test user</span></span>

<span data-ttu-id="9ccc1-258">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Moxtra Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-258">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Moxtra.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="9ccc1-260">**Britta Simon hozzárendelése Moxtra, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="9ccc1-260">**To assign Britta Simon to Moxtra, perform the following steps:**</span></span>

1. <span data-ttu-id="9ccc1-261">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-261">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="9ccc1-263">Az alkalmazások listában válassza ki a **Moxtra**.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-263">In the applications list, select **Moxtra**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_app.png) 

3. <span data-ttu-id="9ccc1-265">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-265">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="9ccc1-267">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-267">Click **Add** button.</span></span> <span data-ttu-id="9ccc1-268">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-268">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="9ccc1-270">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-270">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="9ccc1-271">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-271">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="9ccc1-272">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-272">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="9ccc1-273">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="9ccc1-273">Testing single sign-on</span></span>

<span data-ttu-id="9ccc1-274">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-274">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="9ccc1-275">Ha a hozzáférési panelen Moxtra csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az Moxtra alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="9ccc1-275">When you click the Moxtra tile in the Access Panel, you should get automatically signed-on to your Moxtra application.</span></span>
<span data-ttu-id="9ccc1-276">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="9ccc1-276">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9ccc1-277">További források</span><span class="sxs-lookup"><span data-stu-id="9ccc1-277">Additional resources</span></span>

* [<span data-ttu-id="9ccc1-278">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="9ccc1-278">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="9ccc1-279">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="9ccc1-279">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_203.png

