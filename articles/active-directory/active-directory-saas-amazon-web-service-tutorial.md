---
title: "Oktatóanyag: Azure Active Directory-integráció Amazon Web Services (AWS) |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és az Amazon Web Services (AWS) között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 7561c20b-2325-4d97-887f-693aa383c7be
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: 0fb9c8f428368271b548e3f174726fa01ea910c5
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-amazon-web-services-aws"></a><span data-ttu-id="09c73-103">Oktatóanyag: Azure Active Directory-integráció Amazon Web Services (AWS)</span><span class="sxs-lookup"><span data-stu-id="09c73-103">Tutorial: Azure Active Directory integration with Amazon Web Services (AWS)</span></span>

<span data-ttu-id="09c73-104">Ebben az oktatóanyagban elsajátíthatja Amazon Web Services (AWS) integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="09c73-104">In this tutorial, you learn how to integrate Amazon Web Services (AWS) with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="09c73-105">Amazon Web Services (AWS) integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="09c73-105">Integrating Amazon Web Services (AWS) with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="09c73-106">Szabályozhatja, aki hozzáfér az Amazon Web Services (AWS) Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="09c73-106">You can control in Azure AD who has access to Amazon Web Services (AWS)</span></span>
- <span data-ttu-id="09c73-107">Az Azure AD-fiókok a engedélyezheti a felhasználóknak, hogy automatikusan lekérni bejelentkezett az Amazon Web Services (AWS) (egyszeri bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="09c73-107">You can enable your users to automatically get signed-on to Amazon Web Services (AWS) (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="09c73-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="09c73-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="09c73-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="09c73-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

<!--## Overview

To enable single sign-on with Amazon Web Services (AWS), it must be configured to use Azure Active Directory as an identity provider. This guide provides information and tips on how to perform this configuration in Amazon Web Services (AWS).

>[!Note]: 
>This embedded guide is brand new in the new Azure portal, and we’d love to hear your thoughts. Use the Feedback ? button at the top of the portal to provide feedback. The older guide for using the [Azure classic portal](https://manage.windowsazure.com) to configure this application can be found [here](https://github.com/Azure/AzureAD-App-Docs/blob/master/articles/en-us/_/sso_configure.md).-->


## <a name="prerequisites"></a><span data-ttu-id="09c73-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="09c73-110">Prerequisites</span></span>

<span data-ttu-id="09c73-111">Az Azure AD-integráció konfigurálása az Amazon Web Services (AWS), a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="09c73-111">To configure Azure AD integration with Amazon Web Services (AWS), you need the following items:</span></span>

- <span data-ttu-id="09c73-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="09c73-112">An Azure AD subscription</span></span>
- <span data-ttu-id="09c73-113">Amazon Web Services (AWS) egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="09c73-113">Amazon Web Services (AWS) single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="09c73-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="09c73-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="09c73-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="09c73-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="09c73-116">Ne használja az éles környezetben, ha ez nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="09c73-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="09c73-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="09c73-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="09c73-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="09c73-118">Scenario description</span></span>
<span data-ttu-id="09c73-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="09c73-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="09c73-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="09c73-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="09c73-121">Amazon Web Services (AWS) hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="09c73-121">Adding Amazon Web Services (AWS) from the gallery</span></span>
2. <span data-ttu-id="09c73-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="09c73-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-amazon-web-services-aws-from-the-gallery"></a><span data-ttu-id="09c73-123">Amazon Web Services (AWS) hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="09c73-123">Adding Amazon Web Services (AWS) from the gallery</span></span>
<span data-ttu-id="09c73-124">Az Azure AD-be az Amazon Web Services (AWS)-integráció konfigurálása szüksége Amazon Web Services (AWS) hozzáadása a kezelt SaaS-alkalmazások listáját a gyűjteményből.</span><span class="sxs-lookup"><span data-stu-id="09c73-124">To configure the integration of Amazon Web Services (AWS) into Azure AD, you need to add Amazon Web Services (AWS) from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="09c73-125">**Adja hozzá az Amazon Web Services (AWS) a gyűjteményből, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="09c73-125">**To add Amazon Web Services (AWS) from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="09c73-126">Az a  **[Azure Portal](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="09c73-126">In the **[Azure Portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="09c73-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="09c73-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="09c73-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="09c73-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="09c73-131">Kattintson a **Hozzáadás** gombra a párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="09c73-131">Click **Add** button on the top of the dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="09c73-133">Írja be a keresőmezőbe, **Amazon Web Services (AWS)**.</span><span class="sxs-lookup"><span data-stu-id="09c73-133">In the search box, type **Amazon Web Services (AWS)**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_search.png)

5. <span data-ttu-id="09c73-135">Az eredmények panelen válassza ki a **Amazon Web Services (AWS)**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="09c73-135">In the results panel, select **Amazon Web Services (AWS)**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="09c73-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="09c73-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="09c73-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést az Amazon Web Services (AWS) "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="09c73-138">In this section, you configure and test Azure AD single sign-on with Amazon Web Services (AWS) based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="09c73-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Amazon Web Services (AWS) a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="09c73-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Amazon Web Services (AWS) is to a user in Azure AD.</span></span> <span data-ttu-id="09c73-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználói Amazon Web Services (AWS) közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="09c73-140">In other words, a link relationship between an Azure AD user and the related user in Amazon Web Services (AWS) needs to be established.</span></span>

<span data-ttu-id="09c73-141">Ez a hivatkozás kapcsolat létesíti értéket rendeli az **felhasználónév** értékeként Azure AD-ben a **felhasználónév** Amazon Web Services (AWS).</span><span class="sxs-lookup"><span data-stu-id="09c73-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Amazon Web Services (AWS).</span></span>

<span data-ttu-id="09c73-142">Az Azure AD egyszeri bejelentkezést az Amazon Web Services (AWS) tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="09c73-142">To configure and test Azure AD single sign-on with Amazon Web Services (AWS), you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="09c73-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="09c73-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="09c73-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="09c73-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="09c73-145">**[Az Amazon Web Services tesztfelhasználó létrehozása](#creating-an-amazon-web-services-test-user)**  - kell rendelkeznie a Britta Simon megfelelője az Amazon Web Services (AWS), amely csatolva van rá, hogy az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="09c73-145">**[Creating an Amazon Web Services test user](#creating-an-amazon-web-services-test-user)** - to have a counterpart of Britta Simon in Amazon Web Services (AWS) that is linked to the Azure AD representation of her.</span></span>
4. <span data-ttu-id="09c73-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="09c73-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="09c73-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="09c73-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="09c73-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="09c73-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="09c73-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Amazon Web Services (AWS) alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="09c73-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Amazon Web Services (AWS) application.</span></span>

<span data-ttu-id="09c73-150">**Az Amazon Web Services (AWS) konfigurálása az Azure AD egyszeri bejelentkezést, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="09c73-150">**To configure Azure AD single sign-on with Amazon Web Services (AWS), perform the following steps:**</span></span>

1. <span data-ttu-id="09c73-151">Az Azure portálon a a **Amazon Web Services (AWS)** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="09c73-151">In the Azure Portal, on the **Amazon Web Services (AWS)** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="09c73-153">A a **egyszeri bejelentkezés** párbeszédpanel, mint **mód** válasszon **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="09c73-153">On the **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_samlbase.png)

3. <span data-ttu-id="09c73-155">Az a **Amazon Web Services (AWS) tartományhoz és URL-címek** szakaszban, a felhasználó nem rendelkezik, az alkalmazás már előre integrálva van az Azure-ral bármely lépések végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="09c73-155">On the **Amazon Web Services (AWS) Domain and URLs** section, the user does not have to perform any steps as the app is already pre-integrated with Azure.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_url.png)

4. <span data-ttu-id="09c73-157">Az a **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse az XML-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="09c73-157">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the XML file on your computer.</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_certificate.png)

5. <span data-ttu-id="09c73-159">Az Amazon Web Services (AWS) alkalmazás vár a SAML helyességi feltételek egy meghatározott formátumban.</span><span class="sxs-lookup"><span data-stu-id="09c73-159">The Amazon Web Services (AWS) Software application expects the SAML assertions in a specific format.</span></span> <span data-ttu-id="09c73-160">Állítsa be a következő jogcímeket ehhez az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="09c73-160">Please configure the following claims for this application.</span></span> <span data-ttu-id="09c73-161">Ezek az attribútumok értékének kezelheti a "**felhasználói attribútumok**" szakasz alkalmazás integráció lapján.</span><span class="sxs-lookup"><span data-stu-id="09c73-161">You can manage the values of these attributes from the "**User Attributes**" section on application integration page.</span></span> <span data-ttu-id="09c73-162">Az alábbi képernyőfelvételen látható egy példa a.</span><span class="sxs-lookup"><span data-stu-id="09c73-162">The following screenshot shows an example for this.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_attribute.png)

6. <span data-ttu-id="09c73-164">A a **felhasználói attribútumok** a szakasz a **egyszeri bejelentkezés** párbeszédpanelen konfigurálja a SAML-jogkivonat attribútum, a fenti ábrán látható módon, és hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="09c73-164">In the **User Attributes** section on the **Single sign-on** dialog, configure SAML token attribute as shown in the image above and perform the following steps:</span></span>
    
    | <span data-ttu-id="09c73-165">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="09c73-165">Attribute Name</span></span>  | <span data-ttu-id="09c73-166">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="09c73-166">Attribute Value</span></span> | <span data-ttu-id="09c73-167">Namespace</span><span class="sxs-lookup"><span data-stu-id="09c73-167">Namespace</span></span> |
    | --------------- | --------------- | --------------- |
    | <span data-ttu-id="09c73-168">RoleSessionName</span><span class="sxs-lookup"><span data-stu-id="09c73-168">RoleSessionName</span></span> | <span data-ttu-id="09c73-169">User.userPrincipalName</span><span class="sxs-lookup"><span data-stu-id="09c73-169">user.userprincipalname</span></span> | <span data-ttu-id="09c73-170">https://AWS.amazon.com/SAML/Attributes</span><span class="sxs-lookup"><span data-stu-id="09c73-170">https://aws.amazon.com/SAML/Attributes</span></span> |
    | <span data-ttu-id="09c73-171">Szerepkör</span><span class="sxs-lookup"><span data-stu-id="09c73-171">Role</span></span>            | <span data-ttu-id="09c73-172">User.assignedroles</span><span class="sxs-lookup"><span data-stu-id="09c73-172">user.assignedroles</span></span> |  <span data-ttu-id="09c73-173">https://AWS.amazon.com/SAML/Attributes</span><span class="sxs-lookup"><span data-stu-id="09c73-173">https://aws.amazon.com/SAML/Attributes</span></span> |
    
    >[!TIP]
    ><span data-ttu-id="09c73-174">Szeretne beállítani, hogy a felhasználók átadása a szerepkörök beolvasása a AWS konzol az Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="09c73-174">You need to configure the user provisioning in Azure AD to fetch all the roles from AWS Console.</span></span> <span data-ttu-id="09c73-175">Tekintse meg az alábbi üzembe helyezési lépéseket.</span><span class="sxs-lookup"><span data-stu-id="09c73-175">Please refer the provisioning steps below.</span></span>

    <span data-ttu-id="09c73-176">a.</span><span class="sxs-lookup"><span data-stu-id="09c73-176">a.</span></span> <span data-ttu-id="09c73-177">Kattintson a **Hozzáadás attribútum** megnyitásához a **attribútum hozzáadása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="09c73-177">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_attribute_04.png)

    <span data-ttu-id="09c73-179">b.</span><span class="sxs-lookup"><span data-stu-id="09c73-179">b.</span></span> <span data-ttu-id="09c73-180">Az a **neve** szövegmező, írja be az adott sorhoz feltüntetett attribútumot nevét.</span><span class="sxs-lookup"><span data-stu-id="09c73-180">In the **Name** textbox, type the attribute name shown for that row.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="09c73-182">c.</span><span class="sxs-lookup"><span data-stu-id="09c73-182">c.</span></span> <span data-ttu-id="09c73-183">Az a **érték** kilistázásához írja be a sorhoz látható attribútum értéke.</span><span class="sxs-lookup"><span data-stu-id="09c73-183">From the **Value** list, type the attribute value shown for that row.</span></span> <span data-ttu-id="09c73-184">A Namespace érték hozzáadásával a fent megadott.</span><span class="sxs-lookup"><span data-stu-id="09c73-184">Add the Namespace value as given above.</span></span>
    
    <span data-ttu-id="09c73-185">d.</span><span class="sxs-lookup"><span data-stu-id="09c73-185">d.</span></span> <span data-ttu-id="09c73-186">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="09c73-186">Click **Ok**.</span></span>

7. <span data-ttu-id="09c73-187">Kattintson a **mentése** gombra kattintva mentse a beállításokat az Azure-on.</span><span class="sxs-lookup"><span data-stu-id="09c73-187">Click **Save** button to save the settings on Azure.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="09c73-189">Egy másik böngészőablakban bejelentkezés az Amazon Web Services (AWS) vállalati helyre rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="09c73-189">In a different browser window, sign-on to your Amazon Web Services (AWS) company site as administrator.</span></span>

9. <span data-ttu-id="09c73-190">Kattintson a **konzol otthoni**.</span><span class="sxs-lookup"><span data-stu-id="09c73-190">Click **Console Home**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása][11]

10. <span data-ttu-id="09c73-192">Kattintson a **IAM** a **biztonsági, identitás- & megfelelőségi** szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="09c73-192">Click **IAM** from **Security, Identity & Compliance** service.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása][12]

11. <span data-ttu-id="09c73-194">Kattintson a **identitás-szolgáltatóktól**, és kattintson a **létrehozása szolgáltató**.</span><span class="sxs-lookup"><span data-stu-id="09c73-194">Click **Identity Providers**, and then click **Create Provider**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása][13]

12. <span data-ttu-id="09c73-196">Az a **szolgáltató konfigurálása** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="09c73-196">On the **Configure Provider** dialog page, perform the following steps:</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása][14]
 
    <span data-ttu-id="09c73-198">a.</span><span class="sxs-lookup"><span data-stu-id="09c73-198">a.</span></span> <span data-ttu-id="09c73-199">Mint **szolgáltatótípust**, jelölje be **SAML**.</span><span class="sxs-lookup"><span data-stu-id="09c73-199">As **Provider Type**, select **SAML**.</span></span>

    <span data-ttu-id="09c73-200">b.</span><span class="sxs-lookup"><span data-stu-id="09c73-200">b.</span></span> <span data-ttu-id="09c73-201">Az a **szolgáltatónevet** szövegmező, írja be a szolgáltató nevét (pl.: *fahulladékok*).</span><span class="sxs-lookup"><span data-stu-id="09c73-201">In the **Provider Name** textbox, type a provider name (e.g.: *WAAD*).</span></span>

    <span data-ttu-id="09c73-202">c.</span><span class="sxs-lookup"><span data-stu-id="09c73-202">c.</span></span> <span data-ttu-id="09c73-203">A letöltött metaadat-fájl feltöltése, kattintson a **Choose File**.</span><span class="sxs-lookup"><span data-stu-id="09c73-203">To upload your downloaded metadata file, click **Choose File**.</span></span>

    <span data-ttu-id="09c73-204">d.</span><span class="sxs-lookup"><span data-stu-id="09c73-204">d.</span></span> <span data-ttu-id="09c73-205">Kattintson a **következő lépés**.</span><span class="sxs-lookup"><span data-stu-id="09c73-205">Click **Next Step**.</span></span>

13. <span data-ttu-id="09c73-206">Az a **ellenőrizze a szolgáltató adatait** párbeszédpanel lap, kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="09c73-206">On the **Verify Provider Information** dialog page, click **Create**.</span></span> 
    
    ![Egyszeri bejelentkezés konfigurálása][15]

14. <span data-ttu-id="09c73-208">Kattintson a **szerepkörök**, és kattintson a **hozzon létre új szerepkör**.</span><span class="sxs-lookup"><span data-stu-id="09c73-208">Click **Roles**, and then click **Create New Role**.</span></span> 
    
    ![Egyszeri bejelentkezés konfigurálása][16]

15. <span data-ttu-id="09c73-210">Az a **szerepkörnév beállítása** párbeszédpanelen hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="09c73-210">On the **Set Role Name** dialog, perform the following steps:</span></span> 
    
    ![Egyszeri bejelentkezés konfigurálása][17] 

    <span data-ttu-id="09c73-212">a.</span><span class="sxs-lookup"><span data-stu-id="09c73-212">a.</span></span> <span data-ttu-id="09c73-213">Az a **szerepkörnév** szövegmező, írja be a szerepkör nevét (pl.: *tesztfelhasználó néven*).</span><span class="sxs-lookup"><span data-stu-id="09c73-213">In the **Role Name** textbox, type a role name (e.g.: *TestUser*).</span></span> 

    <span data-ttu-id="09c73-214">b.</span><span class="sxs-lookup"><span data-stu-id="09c73-214">b.</span></span> <span data-ttu-id="09c73-215">Kattintson a **következő lépés**.</span><span class="sxs-lookup"><span data-stu-id="09c73-215">Click **Next Step**.</span></span>

16. <span data-ttu-id="09c73-216">Az a **szerepkör típusának kiválasztása** párbeszédpanelen hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="09c73-216">On the **Select Role Type** dialog, perform the following steps:</span></span> 
    
    ![Egyszeri bejelentkezés konfigurálása][18] 

    <span data-ttu-id="09c73-218">a.</span><span class="sxs-lookup"><span data-stu-id="09c73-218">a.</span></span> <span data-ttu-id="09c73-219">Válassza ki **Identity Provider hozzáférési szerepkör**.</span><span class="sxs-lookup"><span data-stu-id="09c73-219">Select **Role For Identity Provider Access**.</span></span> 

    <span data-ttu-id="09c73-220">b.</span><span class="sxs-lookup"><span data-stu-id="09c73-220">b.</span></span> <span data-ttu-id="09c73-221">Az a **SAML-szolgáltatók Grant webes egyszeri bejelentkezés (WebSSO) hozzáférést** területen kattintson **válasszon**.</span><span class="sxs-lookup"><span data-stu-id="09c73-221">In the **Grant Web Single Sign-On (WebSSO) access to SAML providers** section, click **Select**.</span></span>

17. <span data-ttu-id="09c73-222">Az a **létre megbízható** párbeszédpanelen hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="09c73-222">On the **Establish Trust** dialog, perform the following steps:</span></span>  
    
    ![Egyszeri bejelentkezés konfigurálása][19] 

    <span data-ttu-id="09c73-224">a.</span><span class="sxs-lookup"><span data-stu-id="09c73-224">a.</span></span> <span data-ttu-id="09c73-225">SAML-szolgáltatóként, válassza ki a korábban létrehozott SAML-szolgáltató (pl.: *fahulladékok*)</span><span class="sxs-lookup"><span data-stu-id="09c73-225">As SAML provider, select the SAML provider you have created previously (e.g.: *WAAD*)</span></span>
  
    <span data-ttu-id="09c73-226">b.</span><span class="sxs-lookup"><span data-stu-id="09c73-226">b.</span></span> <span data-ttu-id="09c73-227">Kattintson a **következő lépés**.</span><span class="sxs-lookup"><span data-stu-id="09c73-227">Click **Next Step**.</span></span>

18. <span data-ttu-id="09c73-228">A a **szerepkör megbízható ellenőrizze** párbeszédpanel, kattintson a **tovább**.</span><span class="sxs-lookup"><span data-stu-id="09c73-228">On the **Verify Role Trust** dialog, click **Next Step**.</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása][32]

19. <span data-ttu-id="09c73-230">Az a **csatolása házirend** párbeszédpanel, kattintson a **tovább**.</span><span class="sxs-lookup"><span data-stu-id="09c73-230">On the **Attach Policy** dialog, click **Next Step**.</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása][33]

20. <span data-ttu-id="09c73-232">Az a **felülvizsgálati** párbeszédpanelen hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="09c73-232">On the **Review** dialog, perform the following steps:</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása][34]
 
    <span data-ttu-id="09c73-234">a.</span><span class="sxs-lookup"><span data-stu-id="09c73-234">a.</span></span> <span data-ttu-id="09c73-235">Kattintson a **szerepkör létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="09c73-235">Click **Create Role**.</span></span>

    <span data-ttu-id="09c73-236">b.</span><span class="sxs-lookup"><span data-stu-id="09c73-236">b.</span></span> <span data-ttu-id="09c73-237">Igény szerint annyi szerepköröket hozhat létre, és az identitásszolgáltató való hozzárendelése.</span><span class="sxs-lookup"><span data-stu-id="09c73-237">Create as many roles as needed and map them to the Identity Provider.</span></span>

21. <span data-ttu-id="09c73-238">Mostantól konfigurálhatja a felhasználót a szerepkörök beolvasása a AWS kiépítése</span><span class="sxs-lookup"><span data-stu-id="09c73-238">Now configure the user provisioning to fetch all the roles from AWS</span></span>

    <span data-ttu-id="09c73-239">a.</span><span class="sxs-lookup"><span data-stu-id="09c73-239">a.</span></span> <span data-ttu-id="09c73-240">Az AWS konzol a gyökér-fiókkal történő bejelentkezés a</span><span class="sxs-lookup"><span data-stu-id="09c73-240">In the AWS Console login with your root account</span></span>

    <span data-ttu-id="09c73-241">b.</span><span class="sxs-lookup"><span data-stu-id="09c73-241">b.</span></span> <span data-ttu-id="09c73-242">Jobb felső sarokban kattintson a nevére, és kattintson a **saját biztonsági hitelesítő adatok** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="09c73-242">In the top right corner click your name and then click the **My Security Credentials** option.</span></span> <span data-ttu-id="09c73-243">Figyelmeztető üzenet, ekkor megnyílik egy olyan képernyőt.</span><span class="sxs-lookup"><span data-stu-id="09c73-243">This will open up a screen as a warning message.</span></span> <span data-ttu-id="09c73-244">Kattintson a gombra **biztonsági hitelesítő adatok** gomb a képernyő.</span><span class="sxs-lookup"><span data-stu-id="09c73-244">Click the button **Security Credentials** button to pass the screen.</span></span>
        
       ![Egyszeri bejelentkezés konfigurálása][36]

       ![Egyszeri bejelentkezés konfigurálása][37]

    <span data-ttu-id="09c73-247">c.</span><span class="sxs-lookup"><span data-stu-id="09c73-247">c.</span></span> <span data-ttu-id="09c73-248">A Tárelérési kulcsok szakaszban kattintson a **új kulcs létrehozása** gombra.</span><span class="sxs-lookup"><span data-stu-id="09c73-248">In the Access Keys section click the **Create New Access Key** button.</span></span> <span data-ttu-id="09c73-249">Ezt követően a hozzáférési kulcs Azonosítóját és a token értékét.</span><span class="sxs-lookup"><span data-stu-id="09c73-249">This generates the Access Key ID and a token value.</span></span>
    
       ![Egyszeri bejelentkezés konfigurálása][38]

    <span data-ttu-id="09c73-251">d.</span><span class="sxs-lookup"><span data-stu-id="09c73-251">d.</span></span> <span data-ttu-id="09c73-252">Másolja a következő két értéket, és töltse le a is maga után, hogy ne vesszenek el.</span><span class="sxs-lookup"><span data-stu-id="09c73-252">Copy both these values and also download it, so that you don't lose it.</span></span>

    <span data-ttu-id="09c73-253">e.</span><span class="sxs-lookup"><span data-stu-id="09c73-253">e.</span></span> <span data-ttu-id="09c73-254">Az Azure portálon, az alkalmazás integrációs Amazon Web Services (AWS) lapon kattintson a **kiépítési**.</span><span class="sxs-lookup"><span data-stu-id="09c73-254">In the Azure Portal, on the Amazon Web Services (AWS) application integration page, click **Provisioning**.</span></span>
        
       ![Egyszeri bejelentkezés konfigurálása][35]

    <span data-ttu-id="09c73-256">f.</span><span class="sxs-lookup"><span data-stu-id="09c73-256">f.</span></span> <span data-ttu-id="09c73-257">A kiépítési mód beállítása legyen **automatikus**</span><span class="sxs-lookup"><span data-stu-id="09c73-257">Set the Provisioning mode to **Automatic**</span></span>
        
       ![Egyszeri bejelentkezés konfigurálása][39]

    <span data-ttu-id="09c73-259">g.</span><span class="sxs-lookup"><span data-stu-id="09c73-259">g.</span></span> <span data-ttu-id="09c73-260">Jelenleg a **clientsecret** és **titkos Token** illessze be a megfelelő értékek, amelyeket az AWS konzol másolta.</span><span class="sxs-lookup"><span data-stu-id="09c73-260">Now in the **clientsecret** and **Secret Token** paste the corresponding values, which you have copied from AWS Console.</span></span>
    
    <span data-ttu-id="09c73-261">h.</span><span class="sxs-lookup"><span data-stu-id="09c73-261">h.</span></span> <span data-ttu-id="09c73-262">Kattintson a **kapcsolat tesztelése** a kapcsolat tesztelése gombra.</span><span class="sxs-lookup"><span data-stu-id="09c73-262">You can click the **Test Connection** button to test the connectivity.</span></span> <span data-ttu-id="09c73-263">Ha a művelet sikeres majd elindíthatja az üzembe helyezési összekötő.</span><span class="sxs-lookup"><span data-stu-id="09c73-263">Once that is successful then you can start the provisioning connector.</span></span>
       
       ![Egyszeri bejelentkezés konfigurálása][40]

    <span data-ttu-id="09c73-265">i.</span><span class="sxs-lookup"><span data-stu-id="09c73-265">i.</span></span> <span data-ttu-id="09c73-266">Most már engedélyezheti a kiépítési állapot **a**.</span><span class="sxs-lookup"><span data-stu-id="09c73-266">Now enable the Provisioning Status to **On**.</span></span> <span data-ttu-id="09c73-267">A szerepkörök beolvasása a az alkalmazás elindul.</span><span class="sxs-lookup"><span data-stu-id="09c73-267">This starts fetching the roles from the application.</span></span>

       ![Egyszeri bejelentkezés konfigurálása][41]

    > [!NOTE]
    > <span data-ttu-id="09c73-269">Az Azure AD-kiépítés szolgáltatásának futtatásakor minden szinkronizálása AWS a szerepköröket egy kis idő múlva.</span><span class="sxs-lookup"><span data-stu-id="09c73-269">Azure AD Provisioning service runs every after some time to sync the roles from AWS.</span></span> <span data-ttu-id="09c73-270">Meg kell jelennie az identitásszolgáltató AWS szerepkörök csatolva az Azure AD-be, és az alkalmazások felhasználók vagy csoportok kijelölése közben is használhatja őket.</span><span class="sxs-lookup"><span data-stu-id="09c73-270">You should see all the Identity Provider attached AWS roles into Azure AD and you can use them while assigning the application to users or groups.</span></span>

<!--### Next steps

To ensure users can sign-in to Amazon Web Services (AWS) after it has been configured to use Azure Active Directory, review the following tasks and topics:

- User accounts must be pre-provisioned into Amazon Web Services (AWS) prior to sign-in. To set this up, see Provisioning.
 
- Users must be assigned access to Amazon Web Services (AWS) in Azure AD to sign-in. To assign users, see Users.
 
- To configure access polices for Amazon Web Services (AWS) users, see Access Policies.
 
- For additional information on deploying single sign-on to users, see [this article](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#deploying-azure-ad-integrated-applications-to-users).-->


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="09c73-271">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="09c73-271">Creating an Azure AD test user</span></span>
<span data-ttu-id="09c73-272">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="09c73-272">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="09c73-274">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="09c73-274">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="09c73-275">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="09c73-275">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="09c73-277">Ugrás a **felhasználók és csoportok** kattintson **minden felhasználó** azon felhasználók listájának megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="09c73-277">Go to **Users and groups** and click **All users** to display the list of users.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="09c73-279">Kattintson a párbeszédpanel tetején **Hozzáadás** megnyitásához a **felhasználói** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="09c73-279">At the top of the dialog click **Add** to open the **User** dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="09c73-281">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="09c73-281">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="09c73-283">a.</span><span class="sxs-lookup"><span data-stu-id="09c73-283">a.</span></span> <span data-ttu-id="09c73-284">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="09c73-284">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="09c73-285">b.</span><span class="sxs-lookup"><span data-stu-id="09c73-285">b.</span></span> <span data-ttu-id="09c73-286">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="09c73-286">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="09c73-287">c.</span><span class="sxs-lookup"><span data-stu-id="09c73-287">c.</span></span> <span data-ttu-id="09c73-288">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="09c73-288">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="09c73-289">d.</span><span class="sxs-lookup"><span data-stu-id="09c73-289">d.</span></span> <span data-ttu-id="09c73-290">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="09c73-290">Click **Create**.</span></span>
 
### <a name="creating-an-amazon-web-services-test-user"></a><span data-ttu-id="09c73-291">Az Amazon Web Services tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="09c73-291">Creating an Amazon Web Services test user</span></span>

<span data-ttu-id="09c73-292">Ahhoz, hogy az Azure AD felhasználók jelentkezzenek be az Amazon Web Services (AWS), akkor ki kell építenie az Amazon Web Services (AWS).</span><span class="sxs-lookup"><span data-stu-id="09c73-292">In order to enable Azure AD users to log in to Amazon Web Services (AWS), they must be provisioned into Amazon Web Services (AWS).</span></span> <span data-ttu-id="09c73-293">Esetén Amazon Web Services (AWS), a manuális tevékenység.</span><span class="sxs-lookup"><span data-stu-id="09c73-293">In the case of Amazon Web Services (AWS), provisioning is a manual task.</span></span>

<span data-ttu-id="09c73-294">**Felhasználói fiók létrehozásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="09c73-294">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="09c73-295">Jelentkezzen be a **Amazon Web Services (AWS)** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="09c73-295">Log in to your **Amazon Web Services (AWS)** company site as administrator.</span></span>

2. <span data-ttu-id="09c73-296">Kattintson a **konzol otthoni** ikonra.</span><span class="sxs-lookup"><span data-stu-id="09c73-296">Click the **Console Home** icon.</span></span> 
   
    ![Egyszeri bejelentkezés konfigurálása][11]

3. <span data-ttu-id="09c73-298">Kattintson az identitás és hozzáférés-kezelést.</span><span class="sxs-lookup"><span data-stu-id="09c73-298">Click Identity and Access Management.</span></span> 
   
    ![Egyszeri bejelentkezés konfigurálása][28]

4. <span data-ttu-id="09c73-300">Az irányítópulton kattintson **felhasználók**, és kattintson a **hozzon létre új felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="09c73-300">In the Dashboard, click **Users**, and then click **Create New Users**.</span></span> 
   
    ![Egyszeri bejelentkezés konfigurálása][29]

5. <span data-ttu-id="09c73-302">A felhasználó létrehozása párbeszédpanelen hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="09c73-302">On the Create User dialog, perform the following steps:</span></span> 
   
    ![Egyszeri bejelentkezés konfigurálása][30]   
    
    <span data-ttu-id="09c73-304">a.</span><span class="sxs-lookup"><span data-stu-id="09c73-304">a.</span></span> <span data-ttu-id="09c73-305">Az a **adja meg a felhasználói neveket** szövegmezőből, írja be a Brita Simon felhasználónevet (userprincipalname), az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="09c73-305">In the **Enter User Names** textboxes, type Brita Simon's user name (userprincipalname) in Azure AD.</span></span>

    <span data-ttu-id="09c73-306">b.</span><span class="sxs-lookup"><span data-stu-id="09c73-306">b.</span></span> <span data-ttu-id="09c73-307">Kattintson a **létrehozása.**</span><span class="sxs-lookup"><span data-stu-id="09c73-307">Click **Create.**</span></span>
        
### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="09c73-308">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="09c73-308">Assigning the Azure AD test user</span></span>

<span data-ttu-id="09c73-309">Ebben a szakaszban engedélyezze Britta Simon Azure egyszeri bejelentkezés nyújtó az Amazon Web Services (AWS) használatára.</span><span class="sxs-lookup"><span data-stu-id="09c73-309">In this section, you enable Britta Simon to use Azure single sign-on by granting her access to Amazon Web Services (AWS).</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="09c73-311">**Az Amazon Web Services (AWS) Britta Simon hozzárendeléséhez a következő lépésekkel:**</span><span class="sxs-lookup"><span data-stu-id="09c73-311">**To assign Britta Simon to Amazon Web Services (AWS), perform the following steps:**</span></span>

1. <span data-ttu-id="09c73-312">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="09c73-312">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="09c73-314">Az alkalmazások listában válassza ki a **Amazon Web Services (AWS)**.</span><span class="sxs-lookup"><span data-stu-id="09c73-314">In the applications list, select **Amazon Web Services (AWS)**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_app.png) 

3. <span data-ttu-id="09c73-316">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="09c73-316">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="09c73-318">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="09c73-318">Click **Add** button.</span></span> <span data-ttu-id="09c73-319">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="09c73-319">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="09c73-321">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="09c73-321">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="09c73-322">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="09c73-322">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="09c73-323">A **Szerepkörválasztás** lapra, válassza ki a megfelelő szerepkört a felhasználó számára.</span><span class="sxs-lookup"><span data-stu-id="09c73-323">On **Select Role** tab, select the appropriate role for the user.</span></span> <span data-ttu-id="09c73-324">Ezek a szerepkörök jelennek meg a szerepkör nevét és az identitás-szolgáltató neve.</span><span class="sxs-lookup"><span data-stu-id="09c73-324">All these roles are shown with the role name and identity provider name.</span></span> <span data-ttu-id="09c73-325">Így egyszerűbb azonosítani a AWS szerepköröket.</span><span class="sxs-lookup"><span data-stu-id="09c73-325">This way you can easily identify the roles from AWS.</span></span>

8. <span data-ttu-id="09c73-326">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="09c73-326">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="09c73-327">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="09c73-327">Testing single sign-on</span></span>

<span data-ttu-id="09c73-328">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="09c73-328">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="09c73-329">Ha a hozzáférési panelen Amazon Web Services (AWS) csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az Amazon Web Services (AWS) alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="09c73-329">When you click the Amazon Web Services (AWS) tile in the Access Panel, you should get automatically signed-on to your Amazon Web Services (AWS) application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="09c73-330">További források</span><span class="sxs-lookup"><span data-stu-id="09c73-330">Additional resources</span></span>

* [<span data-ttu-id="09c73-331">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="09c73-331">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="09c73-332">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="09c73-332">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_203.png
[11]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795031.png
[12]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795032.png
[13]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795033.png
[14]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795034.png
[15]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795035.png
[16]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795022.png
[17]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795023.png
[18]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795024.png
[19]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795025.png
[20]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950351.png
[21]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_80.png
[22]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950352.png
[23]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_81.png
[24]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950353.png
[25]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_15.png

[28]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950321.png
[29]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795037.png
[30]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795038.png
[32]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950251.png
[33]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950252.png
[34]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950253.png
[35]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning.png
[36]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_securitycredentials.png
[37]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_securitycredentials_continue.png
[38]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_createnewaccesskey.png
[39]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_automatic.png
[40]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_testconnection.png
[41]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_on.png
