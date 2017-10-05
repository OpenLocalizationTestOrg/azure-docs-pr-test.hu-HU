---
title: "Oktatóanyag: Azure Active Directoryval integrált Trello |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és a Trello között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: cd5ae365-9ed6-43a6-920b-f7814b993949
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: jeedes
ms.openlocfilehash: d93667f16f2d72995e4a42e79e9125b8e3f6b07c
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-trello"></a><span data-ttu-id="00c2d-103">Oktatóanyag: Azure Active Directoryval integrált Trello</span><span class="sxs-lookup"><span data-stu-id="00c2d-103">Tutorial: Azure Active Directory integration with Trello</span></span>

<span data-ttu-id="00c2d-104">Ebben az oktatóanyagban elsajátíthatja Trello integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="00c2d-104">In this tutorial, you learn how to integrate Trello with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="00c2d-105">Trello integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="00c2d-105">Integrating Trello with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="00c2d-106">Megadhatja a Trello hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="00c2d-106">You can control in Azure AD who has access to Trello</span></span>
- <span data-ttu-id="00c2d-107">Engedélyezheti a felhasználóknak, hogy automatikusan lekérni aláírt a Trello (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="00c2d-107">You can enable your users to automatically get signed-on to Trello (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="00c2d-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="00c2d-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="00c2d-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="00c2d-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="00c2d-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="00c2d-110">Prerequisites</span></span>

<span data-ttu-id="00c2d-111">Az Azure AD-integráció konfigurálása a Trello, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="00c2d-111">To configure Azure AD integration with Trello, you need the following items:</span></span>

- <span data-ttu-id="00c2d-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="00c2d-112">An Azure AD subscription</span></span>
- <span data-ttu-id="00c2d-113">A Trello egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="00c2d-113">A Trello single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="00c2d-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="00c2d-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="00c2d-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="00c2d-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="00c2d-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="00c2d-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="00c2d-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="00c2d-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="00c2d-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="00c2d-118">Scenario description</span></span>
<span data-ttu-id="00c2d-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="00c2d-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="00c2d-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="00c2d-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="00c2d-121">A gyűjteményből Trello hozzáadása</span><span class="sxs-lookup"><span data-stu-id="00c2d-121">Adding Trello from the gallery</span></span>
2. <span data-ttu-id="00c2d-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="00c2d-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-trello-from-the-gallery"></a><span data-ttu-id="00c2d-123">A gyűjteményből Trello hozzáadása</span><span class="sxs-lookup"><span data-stu-id="00c2d-123">Adding Trello from the gallery</span></span>
<span data-ttu-id="00c2d-124">Az Azure AD integrálása a Trello konfigurálásához kell hozzáadnia Trello a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="00c2d-124">To configure the integration of Trello into Azure AD, you need to add Trello from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="00c2d-125">**Adja hozzá a Trello a gyűjteményből, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="00c2d-125">**To add Trello from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="00c2d-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="00c2d-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="00c2d-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="00c2d-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="00c2d-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="00c2d-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="00c2d-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="00c2d-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="00c2d-133">Írja be a keresőmezőbe, **Trello**.</span><span class="sxs-lookup"><span data-stu-id="00c2d-133">In the search box, type **Trello**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-trello-tutorial/tutorial_trello_search.png)

5. <span data-ttu-id="00c2d-135">Az eredmények panelen válassza ki a **Trello**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="00c2d-135">In the results panel, select **Trello**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-trello-tutorial/tutorial_trello_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="00c2d-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="00c2d-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="00c2d-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Trello.</span><span class="sxs-lookup"><span data-stu-id="00c2d-138">In this section, you configure and test Azure AD single sign-on with Trello based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="00c2d-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a Trello tartozó felhasználót a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="00c2d-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Trello is to a user in Azure AD.</span></span> <span data-ttu-id="00c2d-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó a Trello közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="00c2d-140">In other words, a link relationship between an Azure AD user and the related user in Trello needs to be established.</span></span>

<span data-ttu-id="00c2d-141">A Trello, rendelje az értékét a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="00c2d-141">In Trello, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="00c2d-142">Az Azure AD egyszeri bejelentkezést a Trello tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="00c2d-142">To configure and test Azure AD single sign-on with Trello, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="00c2d-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="00c2d-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="00c2d-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="00c2d-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="00c2d-145">**[Trello tesztfelhasználó létrehozása](#creating-a-trello-test-user)**  - való Britta Simon valami Trello, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="00c2d-145">**[Creating a Trello test user](#creating-a-trello-test-user)** - to have a counterpart of Britta Simon in Trello that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="00c2d-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="00c2d-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="00c2d-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="00c2d-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="00c2d-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="00c2d-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="00c2d-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és a Trello alkalmazásban egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="00c2d-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Trello application.</span></span>

<span data-ttu-id="00c2d-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés Trello, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="00c2d-150">**To configure Azure AD single sign-on with Trello, perform the following steps:**</span></span>

1. <span data-ttu-id="00c2d-151">Az Azure portálon a a **Trello** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="00c2d-151">In the Azure portal, on the **Trello** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="00c2d-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="00c2d-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-trello-tutorial/tutorial_trello_samlbase.png)

3. <span data-ttu-id="00c2d-155">Az a **Trello tartomány és az URL-címek** szakaszban, ha szeretne beállítani az alkalmazás **IDP kezdeményezett mód**, hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="00c2d-155">On the **Trello Domain and URLs** section, If you wish to configure the application in **IDP initiated mode**, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-trello-tutorial/tutorial_trello_url.png)

    <span data-ttu-id="00c2d-157">Az a **válasz URL-CÍMEN** szövegmező, adja meg a következő minta használatával URL-címe:`https://trello.com/auth/saml/consume/<enterprise>`</span><span class="sxs-lookup"><span data-stu-id="00c2d-157">In the **Reply URL** textbox, type a URL using the following pattern: `https://trello.com/auth/saml/consume/<enterprise>`</span></span>

4. <span data-ttu-id="00c2d-158">Az a **Trello tartomány és az URL-címek** szakaszban, ha szeretne beállítani az alkalmazás **Szolgáltató kezdeményezett mód**, hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="00c2d-158">On the **Trello Domain and URLs** section, If you wish to configure the application in **SP initiated mode**, perform the following steps:</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-trello-tutorial/tutorial_trello_url1.png)

    <span data-ttu-id="00c2d-160">a.</span><span class="sxs-lookup"><span data-stu-id="00c2d-160">a.</span></span> <span data-ttu-id="00c2d-161">Kattintson a **megjelenítése speciális URL-beállításainak**.</span><span class="sxs-lookup"><span data-stu-id="00c2d-161">Click on the **Show advanced URL settings**.</span></span>

    <span data-ttu-id="00c2d-162">b.</span><span class="sxs-lookup"><span data-stu-id="00c2d-162">b.</span></span> <span data-ttu-id="00c2d-163">Az a **URL-cím bejelentkezési** szövegmező, adja meg a következő minta használatával URL-címe:`https://trello.com/auth/saml/consume/<enterprise>`</span><span class="sxs-lookup"><span data-stu-id="00c2d-163">In the **Sign On URL** textbox, type a URL using the following pattern: `https://trello.com/auth/saml/consume/<enterprise>`</span></span>

    >[!NOTE]
    ><span data-ttu-id="00c2d-164">Szerezheti be a  **\<vállalati\>**  a Trello slug.</span><span class="sxs-lookup"><span data-stu-id="00c2d-164">You should get the **\<enterprise\>** slug from Trello.</span></span> <span data-ttu-id="00c2d-165">Ha még nem rendelkezik a slug érték, forduljon a [Trello támogatási csoport](mailto:support@trello.com) vállalati lekérni a slug meg.</span><span class="sxs-lookup"><span data-stu-id="00c2d-165">If you don't have the slug value, contact [Trello support team](mailto:support@trello.com) to get the slug for you enterprise.</span></span>
    > 

5. <span data-ttu-id="00c2d-166">Trello alkalmazás vár a SAML helyességi feltételek megadott attribútumokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="00c2d-166">Trello application expects the SAML assertions to contain specific attributes.</span></span> <span data-ttu-id="00c2d-167">Konfigurálja a következő attribútumok ehhez az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="00c2d-167">Configure the following attributes  for this application.</span></span> <span data-ttu-id="00c2d-168">Ezek az attribútumok értékének kezelheti a **"Felhasználói attribútumok"** az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="00c2d-168">You can manage the values of these attributes from the **"User Attributes"** of the application.</span></span> <span data-ttu-id="00c2d-169">Az alábbi képernyőfelvételen látható egy példa a.</span><span class="sxs-lookup"><span data-stu-id="00c2d-169">The following screenshot shows an example for this.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-trello-tutorial/tutorial_trello_attribute.png)

6. <span data-ttu-id="00c2d-171">Az a **SAML-jogkivonat attribútumok** párbeszédpanel, az alábbi táblázatban szereplő minden egyes sorára hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="00c2d-171">On the **SAML token attributes** dialog, for each row shown in the table below, perform the following steps:</span></span>
 
    | <span data-ttu-id="00c2d-172">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="00c2d-172">Attribute Name</span></span> | <span data-ttu-id="00c2d-173">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="00c2d-173">Attribute Value</span></span> |
    | --- | --- |
    | <span data-ttu-id="00c2d-174">User.Email</span><span class="sxs-lookup"><span data-stu-id="00c2d-174">User.Email</span></span> | <span data-ttu-id="00c2d-175">User.mail</span><span class="sxs-lookup"><span data-stu-id="00c2d-175">user.mail</span></span> |
    | <span data-ttu-id="00c2d-176">User.FirstName</span><span class="sxs-lookup"><span data-stu-id="00c2d-176">User.FirstName</span></span> | <span data-ttu-id="00c2d-177">User.givenName</span><span class="sxs-lookup"><span data-stu-id="00c2d-177">user.givenname</span></span> |
    | <span data-ttu-id="00c2d-178">User.LastName</span><span class="sxs-lookup"><span data-stu-id="00c2d-178">User.LastName</span></span> | <span data-ttu-id="00c2d-179">User.surname</span><span class="sxs-lookup"><span data-stu-id="00c2d-179">user.surname</span></span> |

    <span data-ttu-id="00c2d-180">a.</span><span class="sxs-lookup"><span data-stu-id="00c2d-180">a.</span></span> <span data-ttu-id="00c2d-181">Kattintson a **Hozzáadás attribútum** megnyitásához a **attribútum hozzáadása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="00c2d-181">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-trello-tutorial/tutorial_officespace_04.png)

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-trello-tutorial/tutorial_officespace_05.png)

    <span data-ttu-id="00c2d-184">b.</span><span class="sxs-lookup"><span data-stu-id="00c2d-184">b.</span></span> <span data-ttu-id="00c2d-185">Az a **neve** szövegmező, írja be az adott sorhoz feltüntetett attribútumot nevét.</span><span class="sxs-lookup"><span data-stu-id="00c2d-185">In the **Name** textbox, type the attribute name shown for that row.</span></span> 

    <span data-ttu-id="00c2d-186">c.</span><span class="sxs-lookup"><span data-stu-id="00c2d-186">c.</span></span> <span data-ttu-id="00c2d-187">Az a **érték** kilistázásához írja be a sorhoz látható attribútum értéke.</span><span class="sxs-lookup"><span data-stu-id="00c2d-187">From the **Value** list, type the attribute value shown for that row.</span></span>
    
    <span data-ttu-id="00c2d-188">d.</span><span class="sxs-lookup"><span data-stu-id="00c2d-188">d.</span></span> <span data-ttu-id="00c2d-189">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="00c2d-189">Click **Ok**.</span></span> 
 
7. <span data-ttu-id="00c2d-190">A a **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="00c2d-190">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-trello-tutorial/tutorial_trello_certificate.png) 

8. <span data-ttu-id="00c2d-192">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="00c2d-192">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-trello-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="00c2d-194">A a **Trello konfigurációs** kattintson **konfigurálása Trello** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="00c2d-194">On the **Trello Configuration** section, click **Configure Trello** to open **Configure sign-on** window.</span></span> <span data-ttu-id="00c2d-195">Másolás a **SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="00c2d-195">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-trello-tutorial/tutorial_trello_configure.png) 

9. <span data-ttu-id="00c2d-197">Az alkalmazáshoz konfigurált SSO, keresse fel [Trello vállalati SSO konfigurációs](https://trello.com/sso-configuration) küldendő lap [Trello támogatási csoport](mailto:support@trello.com) a **SAML-alapú egyszeri bejelentkezési URL-címe** , és csatlakoztassa a **tanúsítvány (Base64)**.</span><span class="sxs-lookup"><span data-stu-id="00c2d-197">To get SSO configured for your application, go to [Trello enterprise SSO configuration](https://trello.com/sso-configuration) page to send [Trello support team](mailto:support@trello.com) the **SAML Single Sign-On Service URL** and attach the **Certificate (Base64)**.</span></span>

> [!TIP]
> <span data-ttu-id="00c2d-198">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="00c2d-198">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="00c2d-199">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="00c2d-199">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="00c2d-200">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="00c2d-200">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="00c2d-201">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="00c2d-201">Creating an Azure AD test user</span></span>
<span data-ttu-id="00c2d-202">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="00c2d-202">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="00c2d-204">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="00c2d-204">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="00c2d-205">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="00c2d-205">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-trello-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="00c2d-207">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="00c2d-207">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-trello-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="00c2d-209">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="00c2d-209">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-trello-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="00c2d-211">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="00c2d-211">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-trello-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="00c2d-213">a.</span><span class="sxs-lookup"><span data-stu-id="00c2d-213">a.</span></span> <span data-ttu-id="00c2d-214">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="00c2d-214">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="00c2d-215">b.</span><span class="sxs-lookup"><span data-stu-id="00c2d-215">b.</span></span> <span data-ttu-id="00c2d-216">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="00c2d-216">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="00c2d-217">c.</span><span class="sxs-lookup"><span data-stu-id="00c2d-217">c.</span></span> <span data-ttu-id="00c2d-218">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="00c2d-218">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="00c2d-219">d.</span><span class="sxs-lookup"><span data-stu-id="00c2d-219">d.</span></span> <span data-ttu-id="00c2d-220">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="00c2d-220">Click **Create**.</span></span>
 
### <a name="creating-a-trello-test-user"></a><span data-ttu-id="00c2d-221">Trello tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="00c2d-221">Creating a Trello test user</span></span>

<span data-ttu-id="00c2d-222">Ebben a szakaszban a Trello Britta Simon nevű felhasználó hoz létre.</span><span class="sxs-lookup"><span data-stu-id="00c2d-222">In this section, you create a user called Britta Simon in Trello.</span></span> <span data-ttu-id="00c2d-223">Ebben a szakaszban a Trello Britta Simon nevű felhasználó hoz létre.</span><span class="sxs-lookup"><span data-stu-id="00c2d-223">In this section, you create a user called Britta Simon in Trello.</span></span> <span data-ttu-id="00c2d-224">Trello támogatja a létesítést just-in-time, és új fiók létrehozása az Azure AD bejelentkezés első alkalommal.</span><span class="sxs-lookup"><span data-stu-id="00c2d-224">Trello supports just-in-time provisioning and a new account is created the first time you sign in from Azure AD.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="00c2d-225">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="00c2d-225">Assigning the Azure AD test user</span></span>

<span data-ttu-id="00c2d-226">Ebben a szakaszban Britta Simon által biztosított hozzáférés Trello használatával Azure egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="00c2d-226">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Trello.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="00c2d-228">**Britta Simon hozzárendelése Trello, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="00c2d-228">**To assign Britta Simon to Trello, perform the following steps:**</span></span>

1. <span data-ttu-id="00c2d-229">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="00c2d-229">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="00c2d-231">Az alkalmazások listában válassza ki a **Trello**.</span><span class="sxs-lookup"><span data-stu-id="00c2d-231">In the applications list, select **Trello**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-trello-tutorial/tutorial_trello_app.png) 

3. <span data-ttu-id="00c2d-233">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="00c2d-233">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="00c2d-235">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="00c2d-235">Click **Add** button.</span></span> <span data-ttu-id="00c2d-236">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="00c2d-236">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="00c2d-238">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="00c2d-238">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="00c2d-239">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="00c2d-239">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="00c2d-240">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="00c2d-240">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="00c2d-241">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="00c2d-241">Testing single sign-on</span></span>

<span data-ttu-id="00c2d-242">Ez a szakasz célja a hozzáférési panelen az Azure AD SSO-konfigurációjának tesztelése.</span><span class="sxs-lookup"><span data-stu-id="00c2d-242">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="00c2d-243">Ha a hozzáférési panelen Trello csempére kattint, akkor kell beolvasása automatikusan bejelentkezett a Trello alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="00c2d-243">When you click the Trello tile in the Access Panel, you should get automatically signed-on to your Trello application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="00c2d-244">További források</span><span class="sxs-lookup"><span data-stu-id="00c2d-244">Additional resources</span></span>

* [<span data-ttu-id="00c2d-245">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="00c2d-245">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="00c2d-246">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="00c2d-246">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-trello-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-trello-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-trello-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-trello-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-trello-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-trello-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-trello-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-trello-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-trello-tutorial/tutorial_general_203.png

