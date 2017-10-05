---
title: "Oktatóanyag: Azure Active Directory-integráció SD elemekkel |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és az SD-elemek közötti."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: f0386307-bb3b-4810-8d4b-d0bfebda04f4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.openlocfilehash: 624eff0a0da8f548877e4a4346b21df89cd37b67
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sd-elements"></a><span data-ttu-id="d6b67-103">Oktatóanyag: Azure Active Directoryval integrált SD elemei</span><span class="sxs-lookup"><span data-stu-id="d6b67-103">Tutorial: Azure Active Directory integration with SD Elements</span></span>

<span data-ttu-id="d6b67-104">Ebben az oktatóanyagban elsajátíthatja SD elemek integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="d6b67-104">In this tutorial, you learn how to integrate SD Elements with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="d6b67-105">SD elemek integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="d6b67-105">Integrating SD Elements with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="d6b67-106">Szabályozhatja az Azure AD, aki hozzáfér SD elemek</span><span class="sxs-lookup"><span data-stu-id="d6b67-106">You can control in Azure AD who has access to SD Elements</span></span>
- <span data-ttu-id="d6b67-107">Az Azure AD-fiókok a engedélyezheti a felhasználóknak, hogy automatikusan lekérni bejelentkezett SD elemek (egyszeri bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="d6b67-107">You can enable your users to automatically get signed-on to SD Elements (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="d6b67-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="d6b67-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="d6b67-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="d6b67-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d6b67-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="d6b67-110">Prerequisites</span></span>

<span data-ttu-id="d6b67-111">SD-elemek konfigurálása az Azure AD-integrációs, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="d6b67-111">To configure Azure AD integration with SD Elements, you need the following items:</span></span>

- <span data-ttu-id="d6b67-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="d6b67-112">An Azure AD subscription</span></span>
- <span data-ttu-id="d6b67-113">Egy SD elemek egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="d6b67-113">A SD Elements single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="d6b67-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="d6b67-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="d6b67-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="d6b67-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="d6b67-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="d6b67-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="d6b67-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d6b67-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="d6b67-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="d6b67-118">Scenario description</span></span>
<span data-ttu-id="d6b67-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="d6b67-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="d6b67-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="d6b67-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="d6b67-121">SD elemek hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="d6b67-121">Adding SD Elements from the gallery</span></span>
2. <span data-ttu-id="d6b67-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="d6b67-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-sd-elements-from-the-gallery"></a><span data-ttu-id="d6b67-123">SD elemek hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="d6b67-123">Adding SD Elements from the gallery</span></span>
<span data-ttu-id="d6b67-124">SD elemek integrálása az Azure AD konfigurálásához szüksége SD elemek hozzáadása a kezelt SaaS-alkalmazások listáját a gyűjteményből.</span><span class="sxs-lookup"><span data-stu-id="d6b67-124">To configure the integration of SD Elements into Azure AD, you need to add SD Elements from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="d6b67-125">**A gyűjteményből SD elemek hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="d6b67-125">**To add SD Elements from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="d6b67-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="d6b67-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="d6b67-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="d6b67-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="d6b67-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="d6b67-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="d6b67-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="d6b67-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="d6b67-133">Írja be a keresőmezőbe, **SD elemek**.</span><span class="sxs-lookup"><span data-stu-id="d6b67-133">In the search box, type **SD Elements**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sd-elements-tutorial/tutorial_sdelements_search.png)

5. <span data-ttu-id="d6b67-135">Az eredmények panelen válassza ki a **SD elemek**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="d6b67-135">In the results panel, select **SD Elements**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sd-elements-tutorial/tutorial_sdelements_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="d6b67-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="d6b67-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="d6b67-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján SD elemek.</span><span class="sxs-lookup"><span data-stu-id="d6b67-138">In this section, you configure and test Azure AD single sign-on with SD Elements based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="d6b67-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó SD elemei a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="d6b67-139">For single sign-on to work, Azure AD needs to know what the counterpart user in SD Elements is to a user in Azure AD.</span></span> <span data-ttu-id="d6b67-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a SD elemek közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="d6b67-140">In other words, a link relationship between an Azure AD user and the related user in SD Elements needs to be established.</span></span>

<span data-ttu-id="d6b67-141">SD elemek, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="d6b67-141">In SD Elements, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="d6b67-142">Az Azure AD az egyszeri bejelentkezés SD elemekkel tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="d6b67-142">To configure and test Azure AD single sign-on with SD Elements, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="d6b67-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="d6b67-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="d6b67-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="d6b67-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="d6b67-145">**[SD elemek tesztfelhasználó létrehozása](#creating-a-sd-elements-test-user)**  - való egy megfelelője a Britta Simon SD elemek, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="d6b67-145">**[Creating a SD Elements test user](#creating-a-sd-elements-test-user)** - to have a counterpart of Britta Simon in SD Elements that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="d6b67-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="d6b67-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="d6b67-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="d6b67-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="d6b67-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d6b67-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="d6b67-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az SD-elemek alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="d6b67-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your SD Elements application.</span></span>

<span data-ttu-id="d6b67-150">**Konfigurálja az Azure AD az egyszeri bejelentkezés SD elemekkel, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="d6b67-150">**To configure Azure AD single sign-on with SD Elements, perform the following steps:**</span></span>

1. <span data-ttu-id="d6b67-151">Az Azure portálon a a **SD elemek** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="d6b67-151">In the Azure portal, on the **SD Elements** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="d6b67-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="d6b67-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sd-elements-tutorial/tutorial_sdelements_samlbase.png)

3. <span data-ttu-id="d6b67-155">Az a **SD elemek tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="d6b67-155">On the **SD Elements Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sd-elements-tutorial/tutorial_sdelements_url.png)

    <span data-ttu-id="d6b67-157">a.</span><span class="sxs-lookup"><span data-stu-id="d6b67-157">a.</span></span> <span data-ttu-id="d6b67-158">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://<tenantname>.sdelements.com/sso/saml2/metadata`</span><span class="sxs-lookup"><span data-stu-id="d6b67-158">In the **Identifier** textbox, type a URL using the following pattern: `https://<tenantname>.sdelements.com/sso/saml2/metadata`</span></span>

    <span data-ttu-id="d6b67-159">b.</span><span class="sxs-lookup"><span data-stu-id="d6b67-159">b.</span></span> <span data-ttu-id="d6b67-160">Az a **válasz URL-CÍMEN** szövegmező, adja meg a következő minta használatával URL-címe:`https://<tenantname>.sdelements.com/sso/saml2/acs/`</span><span class="sxs-lookup"><span data-stu-id="d6b67-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://<tenantname>.sdelements.com/sso/saml2/acs/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="d6b67-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="d6b67-161">These values are not real.</span></span> <span data-ttu-id="d6b67-162">Frissítheti ezeket az értékeket a tényleges azonosítója és a válasz URL-CÍMEN.</span><span class="sxs-lookup"><span data-stu-id="d6b67-162">Update these values with the actual Identifier and Reply URL.</span></span> <span data-ttu-id="d6b67-163">Ügyfél [SD elemek támogatási csoport](mailto:support@sdelements.com) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="d6b67-163">Contact [SD Elements support team](mailto:support@sdelements.com) to get these values.</span></span>

4. <span data-ttu-id="d6b67-164">SD elemek alkalmazás vár a SAML helyességi feltételek egy meghatározott formátumban.</span><span class="sxs-lookup"><span data-stu-id="d6b67-164">SD Elements application expects the SAML assertions in a specific format.</span></span> <span data-ttu-id="d6b67-165">A következő jogcímek alkalmazás konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="d6b67-165">Configure the following claims for this application.</span></span> <span data-ttu-id="d6b67-166">Ezek az attribútumok értékének kezelheti a **"Felhasználói attribútum"** az alkalmazás lapján.</span><span class="sxs-lookup"><span data-stu-id="d6b67-166">You can manage the values of these attributes from the **"User Attribute"** tab of the application.</span></span> <span data-ttu-id="d6b67-167">Az alábbi képernyőfelvételen látható egy példa a.</span><span class="sxs-lookup"><span data-stu-id="d6b67-167">The following screenshot shows an example for this.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sd-elements-tutorial/tutorial_sdelements_attribute.png)

5. <span data-ttu-id="d6b67-169">A a **felhasználói attribútumok** a szakasz a **egyszeri bejelentkezés** párbeszédpanelen konfigurálja a SAML-jogkivonat attribútum, az ábrán látható módon, és hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="d6b67-169">In the **User Attributes** section on the **Single sign-on** dialog, configure SAML token attribute as shown in the image and perform the following steps:</span></span> 

    | <span data-ttu-id="d6b67-170">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="d6b67-170">Attribute Name</span></span> | <span data-ttu-id="d6b67-171">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="d6b67-171">Attribute Value</span></span> |
    | --- | --- |
    | <span data-ttu-id="d6b67-172">E-mailek</span><span class="sxs-lookup"><span data-stu-id="d6b67-172">email</span></span> |<span data-ttu-id="d6b67-173">User.mail</span><span class="sxs-lookup"><span data-stu-id="d6b67-173">user.mail</span></span> |
    | <span data-ttu-id="d6b67-174">Utónév</span><span class="sxs-lookup"><span data-stu-id="d6b67-174">firstname</span></span> |<span data-ttu-id="d6b67-175">User.givenName</span><span class="sxs-lookup"><span data-stu-id="d6b67-175">user.givenname</span></span> |
    | <span data-ttu-id="d6b67-176">Vezetéknév</span><span class="sxs-lookup"><span data-stu-id="d6b67-176">lastname</span></span> |<span data-ttu-id="d6b67-177">User.surname</span><span class="sxs-lookup"><span data-stu-id="d6b67-177">user.surname</span></span> |

    <span data-ttu-id="d6b67-178">a.</span><span class="sxs-lookup"><span data-stu-id="d6b67-178">a.</span></span> <span data-ttu-id="d6b67-179">Kattintson a **Hozzáadás attribútum** megnyitásához a **attribútum hozzáadása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d6b67-179">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sd-elements-tutorial/tutorial_officespace_04.png)

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sd-elements-tutorial/tutorial_officespace_05.png)

    <span data-ttu-id="d6b67-182">b.</span><span class="sxs-lookup"><span data-stu-id="d6b67-182">b.</span></span> <span data-ttu-id="d6b67-183">Az a **neve** szövegmező, írja be az adott sorhoz feltüntetett attribútumot nevét.</span><span class="sxs-lookup"><span data-stu-id="d6b67-183">In the **Name** textbox, type the attribute name shown for that row.</span></span>

    <span data-ttu-id="d6b67-184">c.</span><span class="sxs-lookup"><span data-stu-id="d6b67-184">c.</span></span> <span data-ttu-id="d6b67-185">Az a **érték** kilistázásához írja be a sorhoz látható attribútum értéke.</span><span class="sxs-lookup"><span data-stu-id="d6b67-185">From the **Value** list, type the attribute value shown for that row.</span></span>

    <span data-ttu-id="d6b67-186">d.</span><span class="sxs-lookup"><span data-stu-id="d6b67-186">d.</span></span> <span data-ttu-id="d6b67-187">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="d6b67-187">Click **Ok**.</span></span>
 
6. <span data-ttu-id="d6b67-188">A a **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="d6b67-188">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sd-elements-tutorial/tutorial_sdelements_certificate.png) 

7. <span data-ttu-id="d6b67-190">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="d6b67-190">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sd-elements-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="d6b67-192">A a **SD elemek konfigurációs** kattintson **SD-elemek konfigurálása** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="d6b67-192">On the **SD Elements Configuration** section, click **Configure SD Elements** to open **Configure sign-on** window.</span></span> <span data-ttu-id="d6b67-193">Másolás a **SAML Entitásazonosító és SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="d6b67-193">Copy the **SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sd-elements-tutorial/tutorial_sdelements_configure.png)

9. <span data-ttu-id="d6b67-195">Ahhoz, hogy az egyszeri bejelentkezés engedélyezve van, lépjen kapcsolatba a [SD elemek támogatási csoport](mailto:support@sdelements.com) és adja meg a letöltött fájlt.</span><span class="sxs-lookup"><span data-stu-id="d6b67-195">To get single sign-on enabled, contact your [SD Elements support team](mailto:support@sdelements.com) and provide them with the downloaded certificate file.</span></span> 

10. <span data-ttu-id="d6b67-196">Egy másik böngészőablakban bejelentkezés az SD-elemek bérlő rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="d6b67-196">In a different browser window, sign-on to your SD Elements tenant as an administrator.</span></span>

11. <span data-ttu-id="d6b67-197">A felső menüben kattintson a **rendszer**, majd **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="d6b67-197">In the menu on the top, click **System**, and then **Single Sign-on**.</span></span> 
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sd-elements-tutorial/tutorial_sd-elements_09.png) 

12. <span data-ttu-id="d6b67-199">Az a **egyszeri bejelentkezési beállítások** párbeszédpanelen hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="d6b67-199">On the **Single Sign-On Settings** dialog, perform the following steps:</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sd-elements-tutorial/tutorial_sd-elements_10.png) 
   
    <span data-ttu-id="d6b67-201">a.</span><span class="sxs-lookup"><span data-stu-id="d6b67-201">a.</span></span> <span data-ttu-id="d6b67-202">Mint **egyszeri bejelentkezés típusa**, jelölje be **SAML**.</span><span class="sxs-lookup"><span data-stu-id="d6b67-202">As **SSO Type**, select **SAML**.</span></span>
   
    <span data-ttu-id="d6b67-203">b.</span><span class="sxs-lookup"><span data-stu-id="d6b67-203">b.</span></span> <span data-ttu-id="d6b67-204">A a **Identity Provider Entitásazonosító** szövegmezőhöz illessze be az értékét **SAML Entitásazonosító**, amely az Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="d6b67-204">In the **Identity Provider Entity ID** textbox, paste the value of **SAML Entity ID**, which you have copied from Azure portal.</span></span> 
   
    <span data-ttu-id="d6b67-205">c.</span><span class="sxs-lookup"><span data-stu-id="d6b67-205">c.</span></span> <span data-ttu-id="d6b67-206">Az a **identitás szolgáltató egyszeri bejelentkezési szolgáltatás** szövegmezőhöz illessze be az értékét **SAML-alapú egyszeri bejelentkezési URL-címe**, amely az Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="d6b67-206">In the **Identity Provider Single Sign-On Service** textbox, paste the value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span> 
   
    <span data-ttu-id="d6b67-207">d.</span><span class="sxs-lookup"><span data-stu-id="d6b67-207">d.</span></span> <span data-ttu-id="d6b67-208">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="d6b67-208">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="d6b67-209">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="d6b67-209">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="d6b67-210">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="d6b67-210">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="d6b67-211">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="d6b67-211">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="d6b67-212">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="d6b67-212">Creating an Azure AD test user</span></span>
<span data-ttu-id="d6b67-213">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="d6b67-213">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="d6b67-215">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="d6b67-215">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="d6b67-216">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="d6b67-216">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sd-elements-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="d6b67-218">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="d6b67-218">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sd-elements-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="d6b67-220">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="d6b67-220">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sd-elements-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="d6b67-222">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="d6b67-222">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sd-elements-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="d6b67-224">a.</span><span class="sxs-lookup"><span data-stu-id="d6b67-224">a.</span></span> <span data-ttu-id="d6b67-225">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="d6b67-225">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="d6b67-226">b.</span><span class="sxs-lookup"><span data-stu-id="d6b67-226">b.</span></span> <span data-ttu-id="d6b67-227">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="d6b67-227">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="d6b67-228">c.</span><span class="sxs-lookup"><span data-stu-id="d6b67-228">c.</span></span> <span data-ttu-id="d6b67-229">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="d6b67-229">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="d6b67-230">d.</span><span class="sxs-lookup"><span data-stu-id="d6b67-230">d.</span></span> <span data-ttu-id="d6b67-231">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="d6b67-231">Click **Create**.</span></span>
 
### <a name="creating-a-sd-elements-test-user"></a><span data-ttu-id="d6b67-232">SD elemek tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="d6b67-232">Creating a SD Elements test user</span></span>

<span data-ttu-id="d6b67-233">Ez a szakasz célja Britta Simon meghívta SD elemek felhasználó létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="d6b67-233">The objective of this section is to create a user called Britta Simon in SD Elements.</span></span> <span data-ttu-id="d6b67-234">SD elemek esetén kézi tevékenység SD elemek felhasználók létrehozásáról.</span><span class="sxs-lookup"><span data-stu-id="d6b67-234">In the case of SD Elements, creating SD Elements users is a manual task.</span></span>

<span data-ttu-id="d6b67-235">**Az SD-elemek Britta Simon létrehozásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="d6b67-235">**To create Britta Simon in SD Elements, perform the following steps:**</span></span>

1. <span data-ttu-id="d6b67-236">Egy böngészőablakban nyílik, a bejelentkezés az SD-elemek vállalati helyre rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="d6b67-236">In a web browser window, sign-on to your SD Elements company site as an administrator.</span></span>

2. <span data-ttu-id="d6b67-237">A felső menüben kattintson a **felhasználókezelés**, majd **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="d6b67-237">In the menu on the top, click **User Management**, and then **Users**.</span></span>
   
    ![SD elemek tesztfelhasználó létrehozása](./media/active-directory-saas-sd-elements-tutorial/tutorial_sd-elements_11.png) 

3. <span data-ttu-id="d6b67-239">Kattintson a **új felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="d6b67-239">Click **Add New User**.</span></span>
   
    ![SD elemek tesztfelhasználó létrehozása](./media/active-directory-saas-sd-elements-tutorial/tutorial_sd-elements_12.png)
 
4. <span data-ttu-id="d6b67-241">Az a **új felhasználó hozzáadása** párbeszédpanelen hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="d6b67-241">On the **Add New User** dialog, perform the following steps:</span></span>
   
    ![SD elemek tesztfelhasználó létrehozása](./media/active-directory-saas-sd-elements-tutorial/tutorial_sd-elements_13.png) 
   
    <span data-ttu-id="d6b67-243">a.</span><span class="sxs-lookup"><span data-stu-id="d6b67-243">a.</span></span> <span data-ttu-id="d6b67-244">Az a **E-mail** szövegmező, adja meg az e-mail címét, például a felhasználó  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="d6b67-244">In the **E-mail** textbox, enter the email of user like **brittasimon@contoso.com**.</span></span>
   
    <span data-ttu-id="d6b67-245">b.</span><span class="sxs-lookup"><span data-stu-id="d6b67-245">b.</span></span> <span data-ttu-id="d6b67-246">Az a **Utónév** szövegmező, írja be például a felhasználó utónevét **Britta**.</span><span class="sxs-lookup"><span data-stu-id="d6b67-246">In the **First Name** textbox, enter the first name of user like **Britta**.</span></span>
   
    <span data-ttu-id="d6b67-247">c.</span><span class="sxs-lookup"><span data-stu-id="d6b67-247">c.</span></span> <span data-ttu-id="d6b67-248">Az a **Vezetéknév** szövegmező, írja be például a felhasználó vezetéknevét **Simon**.</span><span class="sxs-lookup"><span data-stu-id="d6b67-248">In the **Last Name** textbox, enter the last name of user like **Simon**.</span></span>
   
    <span data-ttu-id="d6b67-249">d.</span><span class="sxs-lookup"><span data-stu-id="d6b67-249">d.</span></span> <span data-ttu-id="d6b67-250">Mint **szerepkör**, jelölje be **felhasználói**.</span><span class="sxs-lookup"><span data-stu-id="d6b67-250">As **Role**, select **User**.</span></span> 
   
    <span data-ttu-id="d6b67-251">e.</span><span class="sxs-lookup"><span data-stu-id="d6b67-251">e.</span></span> <span data-ttu-id="d6b67-252">Kattintson a **létrehozza a felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="d6b67-252">Click **Create User**.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="d6b67-253">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="d6b67-253">Assigning the Azure AD test user</span></span>

<span data-ttu-id="d6b67-254">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés SD elemek Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="d6b67-254">In this section, you enable Britta Simon to use Azure single sign-on by granting access to SD Elements.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="d6b67-256">**Britta Simon hozzárendelése SD elemek, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="d6b67-256">**To assign Britta Simon to SD Elements, perform the following steps:**</span></span>

1. <span data-ttu-id="d6b67-257">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="d6b67-257">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="d6b67-259">Az alkalmazások listában válassza ki a **SD elemek**.</span><span class="sxs-lookup"><span data-stu-id="d6b67-259">In the applications list, select **SD Elements**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sd-elements-tutorial/tutorial_sdelements_app.png) 

3. <span data-ttu-id="d6b67-261">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="d6b67-261">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="d6b67-263">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="d6b67-263">Click **Add** button.</span></span> <span data-ttu-id="d6b67-264">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d6b67-264">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="d6b67-266">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="d6b67-266">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="d6b67-267">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d6b67-267">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="d6b67-268">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d6b67-268">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="d6b67-269">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="d6b67-269">Testing single sign-on</span></span>

<span data-ttu-id="d6b67-270">Ez a szakasz célja tesztelése az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen.</span><span class="sxs-lookup"><span data-stu-id="d6b67-270">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>
  
<span data-ttu-id="d6b67-271">Ha a hozzáférési panelen SD elemek csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az SD-elemek alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="d6b67-271">When you click the SD Elements tile in the Access Panel, you should get automatically signed-on to your SD Elements application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d6b67-272">További források</span><span class="sxs-lookup"><span data-stu-id="d6b67-272">Additional resources</span></span>

* [<span data-ttu-id="d6b67-273">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="d6b67-273">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="d6b67-274">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="d6b67-274">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_203.png

