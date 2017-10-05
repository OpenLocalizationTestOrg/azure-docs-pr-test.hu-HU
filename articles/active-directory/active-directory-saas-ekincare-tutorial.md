---
title: "Oktatóanyag: Azure Active Directoryval integrált eKincare |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és eKincare között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 57f56d14-83cf-4cbb-b342-fac4fc60078f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: jeedes
ms.openlocfilehash: 014eaff14974bb6cd551b6fe53409ede6a6dfea1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-ekincare"></a><span data-ttu-id="3a9f1-103">Oktatóanyag: Azure Active Directoryval integrált eKincare</span><span class="sxs-lookup"><span data-stu-id="3a9f1-103">Tutorial: Azure Active Directory integration with eKincare</span></span>

<span data-ttu-id="3a9f1-104">Ebben az oktatóanyagban elsajátíthatja eKincare integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="3a9f1-104">In this tutorial, you learn how to integrate eKincare with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="3a9f1-105">EKincare integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="3a9f1-105">Integrating eKincare with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="3a9f1-106">Megadhatja a eKincare hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="3a9f1-106">You can control in Azure AD who has access to eKincare</span></span>
- <span data-ttu-id="3a9f1-107">Engedélyezheti a felhasználóknak, hogy automatikusan lekérni aláírt a eKincare (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="3a9f1-107">You can enable your users to automatically get signed-on to eKincare (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="3a9f1-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="3a9f1-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="3a9f1-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="3a9f1-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3a9f1-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="3a9f1-110">Prerequisites</span></span>

<span data-ttu-id="3a9f1-111">Konfigurálása az Azure AD-integrációs eKincare, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="3a9f1-111">To configure Azure AD integration with eKincare, you need the following items:</span></span>

- <span data-ttu-id="3a9f1-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="3a9f1-112">An Azure AD subscription</span></span>
- <span data-ttu-id="3a9f1-113">Egy eKincare egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="3a9f1-113">A eKincare single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="3a9f1-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="3a9f1-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="3a9f1-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="3a9f1-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="3a9f1-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="3a9f1-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="3a9f1-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="3a9f1-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="3a9f1-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="3a9f1-118">Scenario description</span></span>
<span data-ttu-id="3a9f1-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="3a9f1-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="3a9f1-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="3a9f1-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="3a9f1-121">A gyűjteményből eKincare hozzáadása</span><span class="sxs-lookup"><span data-stu-id="3a9f1-121">Adding eKincare from the gallery</span></span>
2. <span data-ttu-id="3a9f1-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="3a9f1-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-ekincare-from-the-gallery"></a><span data-ttu-id="3a9f1-123">A gyűjteményből eKincare hozzáadása</span><span class="sxs-lookup"><span data-stu-id="3a9f1-123">Adding eKincare from the gallery</span></span>
<span data-ttu-id="3a9f1-124">Az Azure AD integrálása a eKincare konfigurálásához kell hozzáadnia eKincare a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="3a9f1-124">To configure the integration of eKincare into Azure AD, you need to add eKincare from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="3a9f1-125">**A gyűjteményből eKincare hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="3a9f1-125">**To add eKincare from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="3a9f1-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="3a9f1-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="3a9f1-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="3a9f1-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="3a9f1-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="3a9f1-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="3a9f1-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="3a9f1-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="3a9f1-133">Írja be a keresőmezőbe, **eKincare**.</span><span class="sxs-lookup"><span data-stu-id="3a9f1-133">In the search box, type **eKincare**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-ekincare-tutorial/tutorial_ekincare_search.png)

5. <span data-ttu-id="3a9f1-135">Az eredmények panelen válassza ki a **eKincare**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="3a9f1-135">In the results panel, select **eKincare**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-ekincare-tutorial/tutorial_ekincare_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="3a9f1-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="3a9f1-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="3a9f1-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján eKincare</span><span class="sxs-lookup"><span data-stu-id="3a9f1-138">In this section, you configure and test Azure AD single sign-on with eKincare based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="3a9f1-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó eKincare a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="3a9f1-139">For single sign-on to work, Azure AD needs to know what the counterpart user in eKincare is to a user in Azure AD.</span></span> <span data-ttu-id="3a9f1-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a eKincare közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="3a9f1-140">In other words, a link relationship between an Azure AD user and the related user in eKincare needs to be established.</span></span>

<span data-ttu-id="3a9f1-141">EKincare, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="3a9f1-141">In eKincare, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="3a9f1-142">Az Azure AD egyszeri bejelentkezést a eKincare tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="3a9f1-142">To configure and test Azure AD single sign-on with eKincare, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="3a9f1-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="3a9f1-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="3a9f1-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="3a9f1-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="3a9f1-145">**[EKincare tesztfelhasználó létrehozása](#creating-a-ekincare-test-user)**  - Britta Simon egy partner, a felhasználó az Azure AD ábrázolását kapcsolódó eKincare rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="3a9f1-145">**[Creating a eKincare test user](#creating-a-ekincare-test-user)** - to have a counterpart of Britta Simon in eKincare that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="3a9f1-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="3a9f1-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="3a9f1-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="3a9f1-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="3a9f1-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="3a9f1-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="3a9f1-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az eKincare alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="3a9f1-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your eKincare application.</span></span>

<span data-ttu-id="3a9f1-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés eKincare, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="3a9f1-150">**To configure Azure AD single sign-on with eKincare, perform the following steps:**</span></span>

1. <span data-ttu-id="3a9f1-151">Az Azure portálon a a **eKincare** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="3a9f1-151">In the Azure portal, on the **eKincare** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="3a9f1-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="3a9f1-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ekincare-tutorial/tutorial_ekincare_samlbase.png)

3. <span data-ttu-id="3a9f1-155">Az a **eKincare tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="3a9f1-155">On the **eKincare Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ekincare-tutorial/tutorial_ekincare_url.png)

    <span data-ttu-id="3a9f1-157">a.</span><span class="sxs-lookup"><span data-stu-id="3a9f1-157">a.</span></span> <span data-ttu-id="3a9f1-158">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://<instancename>.ekincare.com/`</span><span class="sxs-lookup"><span data-stu-id="3a9f1-158">In the **Identifier** textbox, type a URL using the following pattern: `https://<instancename>.ekincare.com/`</span></span>

    <span data-ttu-id="3a9f1-159">b.</span><span class="sxs-lookup"><span data-stu-id="3a9f1-159">b.</span></span> <span data-ttu-id="3a9f1-160">Az a **válasz URL-CÍMEN** szövegmező, adja meg a következő minta használatával URL-címe:`https://<instancename>.ekincare.com/hul/saml`</span><span class="sxs-lookup"><span data-stu-id="3a9f1-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://<instancename>.ekincare.com/hul/saml`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="3a9f1-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="3a9f1-161">These values are not real.</span></span> <span data-ttu-id="3a9f1-162">Frissítheti ezeket az értékeket a tényleges azonosítója és a válasz URL-CÍMEN.</span><span class="sxs-lookup"><span data-stu-id="3a9f1-162">Update these values with the actual Identifier and Reply URL.</span></span> <span data-ttu-id="3a9f1-163">Ügyfél [eKincare támogatási csoport](mailto:tech@ekincare.com) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="3a9f1-163">Contact [eKincare support team](mailto:tech@ekincare.com) to get these values.</span></span>
 
4. <span data-ttu-id="3a9f1-164">eKincare alkalmazás vár a SAML helyességi feltételek egy meghatározott formátumban.</span><span class="sxs-lookup"><span data-stu-id="3a9f1-164">eKincare application expects the SAML assertions in a specific format.</span></span> <span data-ttu-id="3a9f1-165">A következő jogcímek alkalmazás konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="3a9f1-165">Configure the following claims for this application.</span></span> <span data-ttu-id="3a9f1-166">Ezek az attribútumok értékének kezelheti a "**felhasználói attribútumok**" szakasz alkalmazás integráció lapján.</span><span class="sxs-lookup"><span data-stu-id="3a9f1-166">You can manage the values of these attributes from the "**User Attributes**" section on application integration page.</span></span> <span data-ttu-id="3a9f1-167">Az alábbi képernyőfelvételen látható egy példa ehhez a konfigurációhoz.</span><span class="sxs-lookup"><span data-stu-id="3a9f1-167">The following screenshot shows an example for this configuration.</span></span>

    <span data-ttu-id="3a9f1-168">A jogcím neve mindig lesz **"employeeid"** , amelyek azt van leképezve user.extensionattribute1, a felhasználó employeeid tartalmazó értéke.</span><span class="sxs-lookup"><span data-stu-id="3a9f1-168">The claim name will always be **"employeeid"** and the value of which we have mapped to user.extensionattribute1, that contains the employeeid of the user.</span></span> <span data-ttu-id="3a9f1-169">Két jogcím neve Egytényezős</span><span class="sxs-lookup"><span data-stu-id="3a9f1-169">The other two claims' name i.e</span></span> <span data-ttu-id="3a9f1-170">**"a szervezeti"** és **"szervezetnév"** mindig ugyanaz lesz, és azok értékeit tartalmazza a felhasználó a szervezet rendre.</span><span class="sxs-lookup"><span data-stu-id="3a9f1-170">**"organizationid"** and **"organizationname"** will always be same and their values contain the details of the organization of the user respectively.</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ekincare-tutorial/attribute.png)
    
5. <span data-ttu-id="3a9f1-172">A a **felhasználói attribútumok** a szakasz a **egyszeri bejelentkezés** párbeszédpanelen konfigurálja a SAML-jogkivonat attribútum, a fenti ábrán látható módon, és hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="3a9f1-172">In the **User Attributes** section on the **Single sign-on** dialog, configure SAML token attribute as shown in the image above and perform the following steps:</span></span>
    
    | <span data-ttu-id="3a9f1-173">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="3a9f1-173">Attribute Name</span></span> | <span data-ttu-id="3a9f1-174">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="3a9f1-174">Attribute Value</span></span> |
    | ---------------| --------------- |    
    | <span data-ttu-id="3a9f1-175">EmployeeID</span><span class="sxs-lookup"><span data-stu-id="3a9f1-175">employeeid</span></span> | <span data-ttu-id="3a9f1-176">*User.extensionAttribute1*</span><span class="sxs-lookup"><span data-stu-id="3a9f1-176">*user.extensionattribute1*</span></span> |
    | <span data-ttu-id="3a9f1-177">a szervezeti</span><span class="sxs-lookup"><span data-stu-id="3a9f1-177">organizationid</span></span> | <span data-ttu-id="3a9f1-178">*"uniquevalue"*</span><span class="sxs-lookup"><span data-stu-id="3a9f1-178">*"uniquevalue"*</span></span> |
    | <span data-ttu-id="3a9f1-179">szervezetnév</span><span class="sxs-lookup"><span data-stu-id="3a9f1-179">organizationname</span></span> | <span data-ttu-id="3a9f1-180">*User.CompanyName*</span><span class="sxs-lookup"><span data-stu-id="3a9f1-180">*user.companyname*</span></span> |

    <span data-ttu-id="3a9f1-181">a.</span><span class="sxs-lookup"><span data-stu-id="3a9f1-181">a.</span></span> <span data-ttu-id="3a9f1-182">Kattintson a **Hozzáadás attribútum** megnyitásához a **attribútum hozzáadása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="3a9f1-182">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ekincare-tutorial/04.png)

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ekincare-tutorial/05.png)
    
    <span data-ttu-id="3a9f1-185">b.</span><span class="sxs-lookup"><span data-stu-id="3a9f1-185">b.</span></span> <span data-ttu-id="3a9f1-186">Az a **neve** szövegmező, írja be az adott sorhoz feltüntetett attribútumot nevét.</span><span class="sxs-lookup"><span data-stu-id="3a9f1-186">In the **Name** textbox, type the attribute name shown for that row.</span></span>
    
    <span data-ttu-id="3a9f1-187">c.</span><span class="sxs-lookup"><span data-stu-id="3a9f1-187">c.</span></span> <span data-ttu-id="3a9f1-188">Az a **érték** kilistázásához írja be a sorhoz látható attribútum értéke.</span><span class="sxs-lookup"><span data-stu-id="3a9f1-188">From the **Value** list, type the attribute value shown for that row.</span></span>
    
    <span data-ttu-id="3a9f1-189">d.</span><span class="sxs-lookup"><span data-stu-id="3a9f1-189">d.</span></span> <span data-ttu-id="3a9f1-190">Kattintson a **Ok**</span><span class="sxs-lookup"><span data-stu-id="3a9f1-190">Click **Ok**</span></span>

6. <span data-ttu-id="3a9f1-191">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="3a9f1-191">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ekincare-tutorial/tutorial_ekincare_certificate.png) 

7. <span data-ttu-id="3a9f1-193">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="3a9f1-193">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ekincare-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="3a9f1-195">Egyszeri bejelentkezés konfigurálása **eKincare** oldalon kell küldeniük a letöltött **metaadatainak XML-kódja** való [eKincare támogatási csoport](mailto:tech@ekincare.com).</span><span class="sxs-lookup"><span data-stu-id="3a9f1-195">To configure single sign-on on **eKincare** side, you need to send the downloaded **Metadata XML** to [eKincare support team](mailto:tech@ekincare.com).</span></span> <span data-ttu-id="3a9f1-196">Akkor állítsa be ezt a beállítást, hogy a SAML SSO kapcsolat mindkét oldalán megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="3a9f1-196">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="3a9f1-197">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="3a9f1-197">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="3a9f1-198">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="3a9f1-198">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="3a9f1-199">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="3a9f1-199">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="3a9f1-200">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="3a9f1-200">Creating an Azure AD test user</span></span>
<span data-ttu-id="3a9f1-201">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="3a9f1-201">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="3a9f1-203">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="3a9f1-203">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="3a9f1-204">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="3a9f1-204">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-ekincare-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="3a9f1-206">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="3a9f1-206">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-ekincare-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="3a9f1-208">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="3a9f1-208">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-ekincare-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="3a9f1-210">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="3a9f1-210">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-ekincare-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="3a9f1-212">a.</span><span class="sxs-lookup"><span data-stu-id="3a9f1-212">a.</span></span> <span data-ttu-id="3a9f1-213">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="3a9f1-213">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="3a9f1-214">b.</span><span class="sxs-lookup"><span data-stu-id="3a9f1-214">b.</span></span> <span data-ttu-id="3a9f1-215">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="3a9f1-215">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="3a9f1-216">c.</span><span class="sxs-lookup"><span data-stu-id="3a9f1-216">c.</span></span> <span data-ttu-id="3a9f1-217">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="3a9f1-217">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="3a9f1-218">d.</span><span class="sxs-lookup"><span data-stu-id="3a9f1-218">d.</span></span> <span data-ttu-id="3a9f1-219">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="3a9f1-219">Click **Create**.</span></span>
 
### <a name="creating-a-ekincare-test-user"></a><span data-ttu-id="3a9f1-220">EKincare tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="3a9f1-220">Creating a eKincare test user</span></span>

<span data-ttu-id="3a9f1-221">Alkalmazás támogatja a csak az idő a felhasználók átadása, miután a felhasználók hitelesítésére az alkalmazás automatikusan létrejönnek.</span><span class="sxs-lookup"><span data-stu-id="3a9f1-221">Application supports Just in time user provisioning and after authentication users are created in the application automatically.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="3a9f1-222">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="3a9f1-222">Assigning the Azure AD test user</span></span>

<span data-ttu-id="3a9f1-223">Ebben a szakaszban engedélyezze Britta Simon Azure egyszeri bejelentkezéshez használandó eKincare való hozzáférés biztosítása.</span><span class="sxs-lookup"><span data-stu-id="3a9f1-223">In this section, you enable Britta Simon to use Azure single sign-on by granting access to eKincare.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="3a9f1-225">**Britta Simon hozzárendelése eKincare, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="3a9f1-225">**To assign Britta Simon to eKincare, perform the following steps:**</span></span>

1. <span data-ttu-id="3a9f1-226">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="3a9f1-226">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="3a9f1-228">Az alkalmazások listában válassza ki a **eKincare**.</span><span class="sxs-lookup"><span data-stu-id="3a9f1-228">In the applications list, select **eKincare**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ekincare-tutorial/tutorial_ekincare_app.png) 

3. <span data-ttu-id="3a9f1-230">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="3a9f1-230">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="3a9f1-232">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="3a9f1-232">Click **Add** button.</span></span> <span data-ttu-id="3a9f1-233">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="3a9f1-233">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="3a9f1-235">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="3a9f1-235">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="3a9f1-236">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="3a9f1-236">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="3a9f1-237">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="3a9f1-237">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="3a9f1-238">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="3a9f1-238">Testing single sign-on</span></span>

<span data-ttu-id="3a9f1-239">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="3a9f1-239">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="3a9f1-240">Ha a hozzáférési panelen eKincare csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az eKincare alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="3a9f1-240">When you click the eKincare tile in the Access Panel, you should get automatically signed-on to your eKincare application.</span></span>
<span data-ttu-id="3a9f1-241">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md)</span><span class="sxs-lookup"><span data-stu-id="3a9f1-241">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md)</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3a9f1-242">További források</span><span class="sxs-lookup"><span data-stu-id="3a9f1-242">Additional resources</span></span>

* [<span data-ttu-id="3a9f1-243">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="3a9f1-243">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="3a9f1-244">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="3a9f1-244">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-ekincare-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-ekincare-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-ekincare-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-ekincare-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-ekincare-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-ekincare-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-ekincare-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-ekincare-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-ekincare-tutorial/tutorial_general_203.png

