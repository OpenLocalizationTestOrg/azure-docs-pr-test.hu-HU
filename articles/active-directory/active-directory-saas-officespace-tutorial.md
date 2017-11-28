---
title: "Oktatóanyag: Azure Active Directory-integráció OfficeSpace szoftverrel |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és OfficeSpace szoftverek között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 95d8413f-db98-4e2c-8097-9142ef1af823
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: 43d2ecfe851d8f6c43cd4ce7fc4bd872818f4137
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-officespace-software"></a><span data-ttu-id="e8476-103">Oktatóanyag: Azure Active Directory-integráció OfficeSpace szoftverrel</span><span class="sxs-lookup"><span data-stu-id="e8476-103">Tutorial: Azure Active Directory integration with OfficeSpace Software</span></span>

<span data-ttu-id="e8476-104">Ebben az oktatóanyagban elsajátíthatja OfficeSpace szoftver integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="e8476-104">In this tutorial, you learn how to integrate OfficeSpace Software with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="e8476-105">OfficeSpace szoftver integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="e8476-105">Integrating OfficeSpace Software with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="e8476-106">Az Azure AD, aki hozzáfér OfficeSpace szoftver szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="e8476-106">You can control in Azure AD who has access to OfficeSpace Software.</span></span>
- <span data-ttu-id="e8476-107">Az Azure AD-fiókok a engedélyezheti a felhasználóknak, hogy automatikusan lekérni bejelentkezett OfficeSpace szoftverre (egyszeri bejelentkezés).</span><span class="sxs-lookup"><span data-stu-id="e8476-107">You can enable your users to automatically get signed-on to OfficeSpace Software (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="e8476-108">A fiók egyetlen központi helyen – az Azure-portálon kezelheti.</span><span class="sxs-lookup"><span data-stu-id="e8476-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="e8476-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="e8476-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e8476-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="e8476-110">Prerequisites</span></span>

<span data-ttu-id="e8476-111">Az Azure AD-integráció konfigurálása OfficeSpace szoftverrel, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="e8476-111">To configure Azure AD integration with OfficeSpace Software, you need the following items:</span></span>

- <span data-ttu-id="e8476-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="e8476-112">An Azure AD subscription</span></span>
- <span data-ttu-id="e8476-113">Egy OfficeSpace szoftver egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="e8476-113">A OfficeSpace Software single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="e8476-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="e8476-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="e8476-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="e8476-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="e8476-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="e8476-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="e8476-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="e8476-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="e8476-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="e8476-118">Scenario description</span></span>
<span data-ttu-id="e8476-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="e8476-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="e8476-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="e8476-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="e8476-121">A gyűjteményből OfficeSpace szoftver hozzáadása</span><span class="sxs-lookup"><span data-stu-id="e8476-121">Adding OfficeSpace Software from the gallery</span></span>
2. <span data-ttu-id="e8476-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="e8476-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-officespace-software-from-the-gallery"></a><span data-ttu-id="e8476-123">A gyűjteményből OfficeSpace szoftver hozzáadása</span><span class="sxs-lookup"><span data-stu-id="e8476-123">Adding OfficeSpace Software from the gallery</span></span>
<span data-ttu-id="e8476-124">Az Azure AD integrálása a OfficeSpace szoftver konfigurálásához kell hozzáadnia OfficeSpace szoftver a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="e8476-124">To configure the integration of OfficeSpace Software into Azure AD, you need to add OfficeSpace Software from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="e8476-125">**OfficeSpace hozzáadása a gyűjteményből, hajtsa végre a következő lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="e8476-125">**To add OfficeSpace Software from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="e8476-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="e8476-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Az Azure Active Directory gomb][1]

2. <span data-ttu-id="e8476-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="e8476-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="e8476-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="e8476-129">Then go to **All applications**.</span></span>

    ![A vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="e8476-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="e8476-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Az új alkalmazás gomb][3]

4. <span data-ttu-id="e8476-133">Írja be a keresőmezőbe, **OfficeSpace szoftver**, jelölje be **OfficeSpace szoftver** eredmény panelen kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="e8476-133">In the search box, type **OfficeSpace Software**, select **OfficeSpace Software** from result panel then click **Add** button to add the application.</span></span>

    ![Az eredménylistában OfficeSpace szoftver](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="e8476-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e8476-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="e8476-136">Ebben a szakaszban konfigurálása és tesztelése az Azure AD az egyszeri bejelentkezés OfficeSpace szoftverrel "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="e8476-136">In this section, you configure and test Azure AD single sign-on with OfficeSpace Software based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="e8476-137">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó OfficeSpace szoftver a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="e8476-137">For single sign-on to work, Azure AD needs to know what the counterpart user in OfficeSpace Software is to a user in Azure AD.</span></span> <span data-ttu-id="e8476-138">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó OfficeSpace szoftver közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="e8476-138">In other words, a link relationship between an Azure AD user and the related user in OfficeSpace Software needs to be established.</span></span>

<span data-ttu-id="e8476-139">OfficeSpace szoftver, rendelje az értékét a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="e8476-139">In OfficeSpace Software, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="e8476-140">Az Azure AD az egyszeri bejelentkezés OfficeSpace szoftverrel tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="e8476-140">To configure and test Azure AD single sign-on with OfficeSpace Software, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="e8476-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="e8476-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="e8476-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="e8476-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="e8476-143">**[OfficeSpace szoftver tesztfelhasználó létrehozása](#create-a-officespace-software-test-user)**  - való egy megfelelője a Britta Simon OfficeSpace szoftver, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="e8476-143">**[Create a OfficeSpace Software test user](#create-a-officespace-software-test-user)** - to have a counterpart of Britta Simon in OfficeSpace Software that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="e8476-144">**[Rendelje hozzá az Azure AD-teszt felhasználó](#assign-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="e8476-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="e8476-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="e8476-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="e8476-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e8476-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="e8476-147">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és a OfficeSpace alkalmazás egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="e8476-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your OfficeSpace Software application.</span></span>

<span data-ttu-id="e8476-148">**Konfigurálja az Azure AD az egyszeri bejelentkezés OfficeSpace szoftverrel, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="e8476-148">**To configure Azure AD single sign-on with OfficeSpace Software, perform the following steps:**</span></span>

1. <span data-ttu-id="e8476-149">Az Azure portálon a a **OfficeSpace szoftver** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="e8476-149">In the Azure portal, on the **OfficeSpace Software** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. <span data-ttu-id="e8476-151">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="e8476-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_samlbase.png)

3. <span data-ttu-id="e8476-153">Az a **OfficeSpace szoftver tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="e8476-153">On the **OfficeSpace Software Domain and URLs** section, perform the following steps:</span></span>

    ![Az egyszeri bejelentkezés információk OfficeSpace szoftver tartomány és az URL-címek](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_url.png)

    <span data-ttu-id="e8476-155">a.</span><span class="sxs-lookup"><span data-stu-id="e8476-155">a.</span></span> <span data-ttu-id="e8476-156">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<company name>.officespacesoftware.com/users/sign_in/saml`</span><span class="sxs-lookup"><span data-stu-id="e8476-156">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<company name>.officespacesoftware.com/users/sign_in/saml`</span></span>

    <span data-ttu-id="e8476-157">b.</span><span class="sxs-lookup"><span data-stu-id="e8476-157">b.</span></span> <span data-ttu-id="e8476-158">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`<company name>.officespacesoftware.com`</span><span class="sxs-lookup"><span data-stu-id="e8476-158">In the **Identifier** textbox, type a URL using the following pattern: `<company name>.officespacesoftware.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="e8476-159">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="e8476-159">These values are not real.</span></span> <span data-ttu-id="e8476-160">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="e8476-160">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="e8476-161">Ügyfél [OfficeSpace szoftver ügyfél-támogatási csoport](mailto:support@officespacesoftware.com) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="e8476-161">Contact [OfficeSpace Software Client support team](mailto:support@officespacesoftware.com) to get these values.</span></span> 

4. <span data-ttu-id="e8476-162">OfficeSpace alkalmazás vár a SAML helyességi feltételek egy meghatározott formátumban.</span><span class="sxs-lookup"><span data-stu-id="e8476-162">OfficeSpace Software application expects the SAML assertions in a specific format.</span></span> <span data-ttu-id="e8476-163">Állítsa be a következő jogcímeket ehhez az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="e8476-163">Please configure the following claims for this application.</span></span> <span data-ttu-id="e8476-164">Ezek az attribútumok értékének kezelheti a "**felhasználói attribútumok**" szakasz alkalmazás integráció lapján.</span><span class="sxs-lookup"><span data-stu-id="e8476-164">You can manage the values of these attributes from the "**User Attributes**" section on application integration page.</span></span> <span data-ttu-id="e8476-165">Az alábbi képernyőfelvételen látható egy példa a.</span><span class="sxs-lookup"><span data-stu-id="e8476-165">The following screenshot shows an example for this.</span></span>
    
    ![Attribútum konfigurálása](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_attribute.png)

5. <span data-ttu-id="e8476-167">Az a **felhasználói attribútumok** a szakasz a **egyszeri bejelentkezés** párbeszédablakban válassza **user.mail** , **felhasználói azonosító** és az alábbi táblázatban szereplő minden egyes sorhoz kapcsolódóan végezze el a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="e8476-167">In the **User Attributes** section on the **Single sign-on** dialog, select **user.mail**  as **User Identifier** and for each row shown in the table below, perform the following steps:</span></span>
    
    | <span data-ttu-id="e8476-168">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="e8476-168">Attribute Name</span></span> | <span data-ttu-id="e8476-169">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="e8476-169">Attribute Value</span></span> |
    | --- | --- |    
    | <span data-ttu-id="e8476-170">E-mailek</span><span class="sxs-lookup"><span data-stu-id="e8476-170">email</span></span> | <span data-ttu-id="e8476-171">User.mail</span><span class="sxs-lookup"><span data-stu-id="e8476-171">user.mail</span></span> |
    | <span data-ttu-id="e8476-172">név</span><span class="sxs-lookup"><span data-stu-id="e8476-172">name</span></span> | <span data-ttu-id="e8476-173">User.DisplayName</span><span class="sxs-lookup"><span data-stu-id="e8476-173">user.displayname</span></span> |
    | <span data-ttu-id="e8476-174">Utónév</span><span class="sxs-lookup"><span data-stu-id="e8476-174">first_name</span></span> | <span data-ttu-id="e8476-175">User.givenName</span><span class="sxs-lookup"><span data-stu-id="e8476-175">user.givenname</span></span> |
    | <span data-ttu-id="e8476-176">Vezetéknév</span><span class="sxs-lookup"><span data-stu-id="e8476-176">last_name</span></span> | <span data-ttu-id="e8476-177">User.surname</span><span class="sxs-lookup"><span data-stu-id="e8476-177">user.surname</span></span> |

    <span data-ttu-id="e8476-178">a.</span><span class="sxs-lookup"><span data-stu-id="e8476-178">a.</span></span> <span data-ttu-id="e8476-179">Kattintson a **Hozzáadás attribútum** megnyitásához a **attribútum hozzáadása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e8476-179">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![<span data-ttu-id="e8476-180">Konfigurálása hozzáadása</span><span class="sxs-lookup"><span data-stu-id="e8476-180">Configure Add</span></span> ](./media/active-directory-saas-officespace-tutorial/tutorial_attribute_04.png)

    ![Attribútum konfigurálása](./media/active-directory-saas-officespace-tutorial/tutorial_attribute_05.png)
    
    <span data-ttu-id="e8476-182">b.</span><span class="sxs-lookup"><span data-stu-id="e8476-182">b.</span></span> <span data-ttu-id="e8476-183">Az a **neve** szövegmező, írja be az adott sorhoz feltüntetett attribútumot nevét.</span><span class="sxs-lookup"><span data-stu-id="e8476-183">In the **Name** textbox, type the attribute name shown for that row.</span></span>
    
    <span data-ttu-id="e8476-184">c.</span><span class="sxs-lookup"><span data-stu-id="e8476-184">c.</span></span> <span data-ttu-id="e8476-185">Az a **érték** kilistázásához írja be a sorhoz látható attribútum értéke.</span><span class="sxs-lookup"><span data-stu-id="e8476-185">From the **Value** list, type the attribute value shown for that row.</span></span>
    
    <span data-ttu-id="e8476-186">d.</span><span class="sxs-lookup"><span data-stu-id="e8476-186">d.</span></span> <span data-ttu-id="e8476-187">Kattintson a **Ok**</span><span class="sxs-lookup"><span data-stu-id="e8476-187">Click **Ok**</span></span>
 
6. <span data-ttu-id="e8476-188">Az a **SAML-aláíró tanúsítványa** szakaszban, másolja a **UJJLENYOMAT** érték a tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="e8476-188">On the **SAML Signing Certificate** section, copy the **THUMBPRINT** value of the certificate.</span></span>

    ![A tanúsítvány letöltési hivatkozását](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_certificate.png) 

7. <span data-ttu-id="e8476-190">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="e8476-190">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-officespace-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="e8476-192">A a **OfficeSpace szoftverkonfigurációt** kattintson **OfficeSpace szoftver konfigurálása** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="e8476-192">On the **OfficeSpace Software Configuration** section, click **Configure OfficeSpace Software** to open **Configure sign-on** window.</span></span> <span data-ttu-id="e8476-193">Másolás a **Sign-Out URL-címet, és a SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="e8476-193">Copy the **Sign-Out URL, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![OfficeSpace szoftverkonfigurációjának összeállítása](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_configure.png) 

9. <span data-ttu-id="e8476-195">Egy másik webes böngészőablakban bejelentkezni a OfficeSpace szoftver Bérlői rendszergazda.</span><span class="sxs-lookup"><span data-stu-id="e8476-195">In a different web browser window, log into your OfficeSpace Software tenant as an administrator.</span></span>

10. <span data-ttu-id="e8476-196">Ugrás a **beállítások** kattintson **összekötők**.</span><span class="sxs-lookup"><span data-stu-id="e8476-196">Go to **Settings** and click **Connectors**.</span></span>

    ![Alkalmazás ügyféloldali egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_002.png)

11. <span data-ttu-id="e8476-198">Kattintson a **SAML-alapú hitelesítés**.</span><span class="sxs-lookup"><span data-stu-id="e8476-198">Click **SAML Authentication**.</span></span>

    ![Alkalmazás ügyféloldali egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_003.png)

12. <span data-ttu-id="e8476-200">Az a **SAML-alapú hitelesítés** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="e8476-200">In the **SAML Authentication** section, perform the following steps:</span></span>

    ![Alkalmazás ügyféloldali egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_004.png)

    <span data-ttu-id="e8476-202">a.</span><span class="sxs-lookup"><span data-stu-id="e8476-202">a.</span></span> <span data-ttu-id="e8476-203">Az a **kijelentkezési szolgáltató URL-cím** szövegmezőhöz illessze be az értékét **Sign-Out URL-cím** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="e8476-203">In the **Logout provider url** textbox, paste the value of **Sign-Out URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="e8476-204">b.</span><span class="sxs-lookup"><span data-stu-id="e8476-204">b.</span></span> <span data-ttu-id="e8476-205">Az a **ügyfél idp célként megadott url** szövegmezőhöz illessze be az értékét **SAML-alapú egyszeri bejelentkezési URL-címe** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="e8476-205">In the **Client idp target url** textbox, paste the value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="e8476-206">c.</span><span class="sxs-lookup"><span data-stu-id="e8476-206">c.</span></span> <span data-ttu-id="e8476-207">Beillesztés a **ujjlenyomat** érték, amely az Azure portálról másolta a **ügyfél IDP tanúsítvány-ujjlenyomat** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="e8476-207">Paste the **Thumbprint** value which you have copied from Azure portal, into the **Client IDP certificate fingerprint** textbox.</span></span> 

    <span data-ttu-id="e8476-208">d.</span><span class="sxs-lookup"><span data-stu-id="e8476-208">d.</span></span> <span data-ttu-id="e8476-209">Kattintson a **beállítások mentése**.</span><span class="sxs-lookup"><span data-stu-id="e8476-209">Click **Save Settings**.</span></span>


> [!TIP]
> <span data-ttu-id="e8476-210">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="e8476-210">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="e8476-211">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="e8476-211">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="e8476-212">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="e8476-212">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="e8476-213">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="e8476-213">Create an Azure AD test user</span></span>

<span data-ttu-id="e8476-214">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="e8476-214">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Hozzon létre egy Azure AD-teszt felhasználó][100]

<span data-ttu-id="e8476-216">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="e8476-216">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="e8476-217">Az Azure portálon a bal oldali ablaktáblán kattintson a **Azure Active Directory** gombra.</span><span class="sxs-lookup"><span data-stu-id="e8476-217">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Az Azure Active Directory gomb](./media/active-directory-saas-officespace-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="e8476-219">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok**, és kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="e8476-219">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    ![A "felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-officespace-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="e8476-221">Megnyitásához a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** tetején a **minden felhasználó** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="e8476-221">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![A Hozzáadás gombra.](./media/active-directory-saas-officespace-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="e8476-223">Az a **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="e8476-223">In the **User** dialog box, perform the following steps:</span></span>

    ![A felhasználó párbeszédpanel](./media/active-directory-saas-officespace-tutorial/create_aaduser_04.png)

    <span data-ttu-id="e8476-225">a.</span><span class="sxs-lookup"><span data-stu-id="e8476-225">a.</span></span> <span data-ttu-id="e8476-226">Az a **neve** mezőbe írja be **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="e8476-226">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="e8476-227">b.</span><span class="sxs-lookup"><span data-stu-id="e8476-227">b.</span></span> <span data-ttu-id="e8476-228">Az a **felhasználónév** mezőbe írja be a felhasználó e-mail címe az Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e8476-228">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="e8476-229">c.</span><span class="sxs-lookup"><span data-stu-id="e8476-229">c.</span></span> <span data-ttu-id="e8476-230">Válassza ki a **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a megjelenített érték a **jelszó** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="e8476-230">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="e8476-231">d.</span><span class="sxs-lookup"><span data-stu-id="e8476-231">d.</span></span> <span data-ttu-id="e8476-232">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="e8476-232">Click **Create**.</span></span>
 
### <a name="create-a-officespace-software-test-user"></a><span data-ttu-id="e8476-233">OfficeSpace szoftver tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="e8476-233">Create a OfficeSpace Software test user</span></span>

<span data-ttu-id="e8476-234">Ez a szakasz célja OfficeSpace szoftver Britta Simon nevű felhasználót létrehozni.</span><span class="sxs-lookup"><span data-stu-id="e8476-234">The objective of this section is to create a user called Britta Simon in OfficeSpace Software.</span></span> <span data-ttu-id="e8476-235">OfficeSpace szoftver, amellyel just-in-time átadása, amely alapértelmezés szerint van engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="e8476-235">OfficeSpace Software supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="e8476-236">Nincs ebben a szakaszban az Ön művelet elem.</span><span class="sxs-lookup"><span data-stu-id="e8476-236">There is no action item for you in this section.</span></span> <span data-ttu-id="e8476-237">Új felhasználó jön létre az OfficeSpace szoftver elérésére, ha még nem létezik tett kísérlet során.</span><span class="sxs-lookup"><span data-stu-id="e8476-237">A new user will be created during an attempt to access OfficeSpace Software if it doesn't exist yet.</span></span>

> [!NOTE]
> <span data-ttu-id="e8476-238">Ha manuálisan hozzon létre egy felhasználó van szüksége, lépjen kapcsolatba kell [OfficeSpace szoftver támogatási csoport](mailto:support@officespacesoftware.com).</span><span class="sxs-lookup"><span data-stu-id="e8476-238">If you need to create an user manually, you need to Contact [OfficeSpace Software support team](mailto:support@officespacesoftware.com).</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="e8476-239">Rendelje hozzá az Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="e8476-239">Assign the Azure AD test user</span></span>

<span data-ttu-id="e8476-240">Ebben a szakaszban engedélyezze Britta Simon Azure egyszeri bejelentkezés által biztosított hozzáférés OfficeSpace szoftver használatára.</span><span class="sxs-lookup"><span data-stu-id="e8476-240">In this section, you enable Britta Simon to use Azure single sign-on by granting access to OfficeSpace Software.</span></span>

![A felhasználói szerepkör hozzárendelése][200] 

<span data-ttu-id="e8476-242">**Britta Simon hozzárendelése OfficeSpace szoftver, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="e8476-242">**To assign Britta Simon to OfficeSpace Software, perform the following steps:**</span></span>

1. <span data-ttu-id="e8476-243">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="e8476-243">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="e8476-245">Az alkalmazások listában válassza ki a **OfficeSpace szoftver**.</span><span class="sxs-lookup"><span data-stu-id="e8476-245">In the applications list, select **OfficeSpace Software**.</span></span>

    ![Az alkalmazások listáját a OfficeSpace szoftver hivatkozás](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_app.png)  

3. <span data-ttu-id="e8476-247">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="e8476-247">In the menu on the left, click **Users and groups**.</span></span>

    ![A "Felhasználók és csoportok" hivatkozásra][202]

4. <span data-ttu-id="e8476-249">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="e8476-249">Click **Add** button.</span></span> <span data-ttu-id="e8476-250">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e8476-250">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![A hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="e8476-252">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="e8476-252">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="e8476-253">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e8476-253">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="e8476-254">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e8476-254">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="e8476-255">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="e8476-255">Test single sign-on</span></span>

<span data-ttu-id="e8476-256">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="e8476-256">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="e8476-257">Ha a hozzáférési panelen OfficeSpace szoftver csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az OfficeSpace szoftverfrissítések alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="e8476-257">When you click the OfficeSpace Software tile in the Access Panel, you should get automatically signed-on to your OfficeSpace Software application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e8476-258">További források</span><span class="sxs-lookup"><span data-stu-id="e8476-258">Additional resources</span></span>

* [<span data-ttu-id="e8476-259">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="e8476-259">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="e8476-260">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="e8476-260">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_203.png

