---
title: "Oktatóanyag: Azure Active Directoryval integrált Ceridian Dayforce HCM |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Ceridian Dayforce HCM között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 7adf1eb3-d063-45d6-96a8-fd53b329b3f3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: b2ea3d92f233dab5bd6814e4875f881117eac8e3
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-ceridian-dayforce-hcm"></a><span data-ttu-id="b59c9-103">Oktatóanyag: Azure Active Directoryval integrált Ceridian Dayforce HCM</span><span class="sxs-lookup"><span data-stu-id="b59c9-103">Tutorial: Azure Active Directory integration with Ceridian Dayforce HCM</span></span>

<span data-ttu-id="b59c9-104">Ebben az oktatóanyagban elsajátíthatja Ceridian Dayforce HCM integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="b59c9-104">In this tutorial, you learn how to integrate Ceridian Dayforce HCM with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="b59c9-105">Ceridian Dayforce HCM integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="b59c9-105">Integrating Ceridian Dayforce HCM with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="b59c9-106">Az Azure AD, aki hozzáfér Ceridian Dayforce HCM szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="b59c9-106">You can control in Azure AD who has access to Ceridian Dayforce HCM.</span></span>
- <span data-ttu-id="b59c9-107">Az Azure AD-fiókok a engedélyezheti a felhasználóknak, hogy automatikusan lekérni aláírt a Ceridian Dayforce HCM (egyszeri bejelentkezés).</span><span class="sxs-lookup"><span data-stu-id="b59c9-107">You can enable your users to automatically get signed-on to Ceridian Dayforce HCM (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="b59c9-108">A fiók egyetlen központi helyen – az Azure-portálon kezelheti.</span><span class="sxs-lookup"><span data-stu-id="b59c9-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="b59c9-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="b59c9-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b59c9-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="b59c9-110">Prerequisites</span></span>

<span data-ttu-id="b59c9-111">Konfigurálása az Azure AD-integrációs Ceridian Dayforce HCM, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="b59c9-111">To configure Azure AD integration with Ceridian Dayforce HCM, you need the following items:</span></span>

- <span data-ttu-id="b59c9-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="b59c9-112">An Azure AD subscription</span></span>
- <span data-ttu-id="b59c9-113">Egy Ceridian Dayforce HCM egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="b59c9-113">A Ceridian Dayforce HCM single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="b59c9-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="b59c9-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="b59c9-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="b59c9-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="b59c9-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="b59c9-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="b59c9-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b59c9-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="b59c9-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="b59c9-118">Scenario description</span></span>
<span data-ttu-id="b59c9-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="b59c9-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="b59c9-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="b59c9-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="b59c9-121">A gyűjteményből Ceridian Dayforce HCM hozzáadása</span><span class="sxs-lookup"><span data-stu-id="b59c9-121">Adding Ceridian Dayforce HCM from the gallery</span></span>
2. <span data-ttu-id="b59c9-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="b59c9-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-ceridian-dayforce-hcm-from-the-gallery"></a><span data-ttu-id="b59c9-123">A gyűjteményből Ceridian Dayforce HCM hozzáadása</span><span class="sxs-lookup"><span data-stu-id="b59c9-123">Adding Ceridian Dayforce HCM from the gallery</span></span>
<span data-ttu-id="b59c9-124">Az Azure AD integrálása a Ceridian Dayforce HCM konfigurálásához kell hozzáadnia Ceridian Dayforce HCM a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="b59c9-124">To configure the integration of Ceridian Dayforce HCM into Azure AD, you need to add Ceridian Dayforce HCM from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="b59c9-125">**A gyűjteményből Ceridian Dayforce HCM hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="b59c9-125">**To add Ceridian Dayforce HCM from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="b59c9-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="b59c9-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Az Azure Active Directory gomb][1]

2. <span data-ttu-id="b59c9-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="b59c9-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="b59c9-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="b59c9-129">Then go to **All applications**.</span></span>

    ![A vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="b59c9-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="b59c9-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Az új alkalmazás gomb][3]

4. <span data-ttu-id="b59c9-133">Írja be a keresőmezőbe, **Ceridian Dayforce HCM**, jelölje be **Ceridian Dayforce HCM** eredmény panelen kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="b59c9-133">In the search box, type **Ceridian Dayforce HCM**, select **Ceridian Dayforce HCM** from result panel then click **Add** button to add the application.</span></span>

    ![Az eredménylistában Ceridian Dayforce HCM](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="b59c9-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b59c9-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="b59c9-136">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD az egyszeri bejelentkezés Ceridian Dayforce HCM "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="b59c9-136">In this section, you configure and test Azure AD single sign-on with Ceridian Dayforce HCM based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="b59c9-137">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Ceridian Dayforce HCM a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="b59c9-137">For single sign-on to work, Azure AD needs to know what the counterpart user in Ceridian Dayforce HCM is to a user in Azure AD.</span></span> <span data-ttu-id="b59c9-138">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó a Ceridian Dayforce HCM közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="b59c9-138">In other words, a link relationship between an Azure AD user and the related user in Ceridian Dayforce HCM needs to be established.</span></span>

<span data-ttu-id="b59c9-139">Ceridian Dayforce HCM, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="b59c9-139">In Ceridian Dayforce HCM, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="b59c9-140">Az Azure AD egyszeri bejelentkezést a Ceridian Dayforce HCM tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="b59c9-140">To configure and test Azure AD single sign-on with Ceridian Dayforce HCM, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="b59c9-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="b59c9-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="b59c9-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="b59c9-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="b59c9-143">**[Ceridian Dayforce HCM tesztfelhasználó létrehozása](#create-a-ceridian-dayforce-hcm-test-user)**  - való egy megfelelője a Britta Simon Ceridian Dayforce HCM, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="b59c9-143">**[Create a Ceridian Dayforce HCM test user](#create-a-ceridian-dayforce-hcm-test-user)** - to have a counterpart of Britta Simon in Ceridian Dayforce HCM that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="b59c9-144">**[Rendelje hozzá az Azure AD-teszt felhasználó](#assign-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="b59c9-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="b59c9-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="b59c9-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="b59c9-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b59c9-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="b59c9-147">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Ceridian Dayforce HCM alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="b59c9-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Ceridian Dayforce HCM application.</span></span>

<span data-ttu-id="b59c9-148">**Az Azure AD az egyszeri bejelentkezés konfigurálása Ceridian Dayforce HCM, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="b59c9-148">**To configure Azure AD single sign-on with Ceridian Dayforce HCM, perform the following steps:**</span></span>

1. <span data-ttu-id="b59c9-149">Az Azure portálon a a **Ceridian Dayforce HCM** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="b59c9-149">In the Azure portal, on the **Ceridian Dayforce HCM** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. <span data-ttu-id="b59c9-151">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="b59c9-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_samlbase.png)

3. <span data-ttu-id="b59c9-153">Az a **Ceridian Dayforce HCM tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="b59c9-153">On the **Ceridian Dayforce HCM Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_url.png)
    
    <span data-ttu-id="b59c9-155">a.</span><span class="sxs-lookup"><span data-stu-id="b59c9-155">a.</span></span> <span data-ttu-id="b59c9-156">Az a **URL-cím bejelentkezési** szövegmező, írja be az URL-címet használják-e a felhasználók bejelentkezés az Ceridian Dayforce HCM alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="b59c9-156">In the **Sign On URL** textbox, type the URL used by your users to sign-on to your Ceridian Dayforce HCM application.</span></span>
    
    | <span data-ttu-id="b59c9-157">Környezet</span><span class="sxs-lookup"><span data-stu-id="b59c9-157">Environment</span></span> | <span data-ttu-id="b59c9-158">URL-CÍME</span><span class="sxs-lookup"><span data-stu-id="b59c9-158">URL</span></span> |
    | :-- | :-- |
    | <span data-ttu-id="b59c9-159">Termelési környezetben</span><span class="sxs-lookup"><span data-stu-id="b59c9-159">For production</span></span> | `https://sso.dayforcehcm.com/<DayforcehcmNamespace>` |
    | <span data-ttu-id="b59c9-160">A teszt</span><span class="sxs-lookup"><span data-stu-id="b59c9-160">For test</span></span> | `https://ssotest.dayforcehcm.com/<DayforcehcmNamespace>` |
    
    <span data-ttu-id="b59c9-161">b.</span><span class="sxs-lookup"><span data-stu-id="b59c9-161">b.</span></span> <span data-ttu-id="b59c9-162">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:</span><span class="sxs-lookup"><span data-stu-id="b59c9-162">In the **Identifier** textbox, type a URL using the following pattern:</span></span>
    
    | <span data-ttu-id="b59c9-163">Környezet</span><span class="sxs-lookup"><span data-stu-id="b59c9-163">Environment</span></span> | <span data-ttu-id="b59c9-164">URL-CÍME</span><span class="sxs-lookup"><span data-stu-id="b59c9-164">URL</span></span> |
    | :-- | :-- |
    | <span data-ttu-id="b59c9-165">Termelési környezetben</span><span class="sxs-lookup"><span data-stu-id="b59c9-165">For production</span></span> | `https://ncpingfederate.dayforcehcm.com/sp` |
    | <span data-ttu-id="b59c9-166">A teszt</span><span class="sxs-lookup"><span data-stu-id="b59c9-166">For test</span></span> | `https://fs-test.dayforcehcm.com/sp` |
    
    <span data-ttu-id="b59c9-167">c.</span><span class="sxs-lookup"><span data-stu-id="b59c9-167">c.</span></span> <span data-ttu-id="b59c9-168">Az a **válasz URL-CÍMEN** szövegmező, írja be az URL-címet használják az Azure AD a válaszban küldje.</span><span class="sxs-lookup"><span data-stu-id="b59c9-168">In the **Reply URL** textbox, type the URL used by Azure AD to post the response.</span></span>
    
    | <span data-ttu-id="b59c9-169">Környezet</span><span class="sxs-lookup"><span data-stu-id="b59c9-169">Environment</span></span> | <span data-ttu-id="b59c9-170">URL-CÍME</span><span class="sxs-lookup"><span data-stu-id="b59c9-170">URL</span></span> |
    | :-- | :-- |
    | <span data-ttu-id="b59c9-171">Termelési környezetben</span><span class="sxs-lookup"><span data-stu-id="b59c9-171">For production</span></span> | `https://ncpingfederate.dayforcehcm.com/sp/ACS.saml2` |
    | <span data-ttu-id="b59c9-172">A teszt</span><span class="sxs-lookup"><span data-stu-id="b59c9-172">For test</span></span> | `https://fs-test.dayforcehcm.com/sp/ACS.saml2` |
    
    > [!NOTE] 
    > <span data-ttu-id="b59c9-173">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="b59c9-173">These values are not real.</span></span> <span data-ttu-id="b59c9-174">Ezek az értékek frissíti a tényleges azonosítója, válasz és bejelentkezési URL-címe.</span><span class="sxs-lookup"><span data-stu-id="b59c9-174">Update these values with the actual Identifier, Reply URL and Sign-On URL.</span></span> <span data-ttu-id="b59c9-175">Ügyfél [Ceridian Dayforce HCM ügyfél-támogatási csoport](https://www.ceridian.com/contact-us/index.html) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="b59c9-175">Contact [Ceridian Dayforce HCM Client support team](https://www.ceridian.com/contact-us/index.html) to get these values.</span></span>

4. <span data-ttu-id="b59c9-176">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="b59c9-176">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![A tanúsítvány letöltési hivatkozását](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_certificate.png) 

5. <span data-ttu-id="b59c9-178">A Ceridian Dayforce HCM alkalmazás vár a SAML helyességi feltételek egy meghatározott formátumban.</span><span class="sxs-lookup"><span data-stu-id="b59c9-178">Your Ceridian Dayforce HCM application expects the SAML assertions in a specific format.</span></span> <span data-ttu-id="b59c9-179">Együttműködve [Ceridian Dayforce HCM támogatási csoport](https://www.ceridian.com/contact-us/index.html) először a megfelelő felhasználói azonosítót azonosításához.</span><span class="sxs-lookup"><span data-stu-id="b59c9-179">Work with [Ceridian Dayforce HCM support team](https://www.ceridian.com/contact-us/index.html) first to identify the correct user identifier.</span></span> <span data-ttu-id="b59c9-180">A Microsoft azt javasolja, használja a **"name"** attribútum az felhasználói azonosítóval.</span><span class="sxs-lookup"><span data-stu-id="b59c9-180">Microsoft recommends using the **"name"** attribute as user identifier.</span></span> <span data-ttu-id="b59c9-181">Ezek az attribútumok értékének kezelheti a **felhasználói attribútumok** szakasz alkalmazás integráció lapján.</span><span class="sxs-lookup"><span data-stu-id="b59c9-181">You can manage the values of these attributes from the **User Attributes** section on application integration page.</span></span> <span data-ttu-id="b59c9-182">Az alábbi képernyőfelvételen látható egy példa a.</span><span class="sxs-lookup"><span data-stu-id="b59c9-182">The following screenshot shows an example for this.</span></span>  

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_07.png)

6. <span data-ttu-id="b59c9-184">A a **felhasználói attribútumok** a szakasz a **egyszeri bejelentkezés** párbeszédpanelen konfigurálja a SAML-jogkivonat attribútum, a fenti ábrán látható módon, és hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="b59c9-184">In the **User Attributes** section on the **Single sign-on** dialog, configure SAML token attribute as shown in the image above and perform the following steps:</span></span>
    
    | <span data-ttu-id="b59c9-185">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="b59c9-185">Attribute Name</span></span>  | <span data-ttu-id="b59c9-186">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="b59c9-186">Attribute Value</span></span> |
    | --------------- | -------------------- |    
    | <span data-ttu-id="b59c9-187">név</span><span class="sxs-lookup"><span data-stu-id="b59c9-187">name</span></span>  | <span data-ttu-id="b59c9-188">User.extensionattribute2</span><span class="sxs-lookup"><span data-stu-id="b59c9-188">user.extensionattribute2</span></span> |    

    <span data-ttu-id="b59c9-189">a.</span><span class="sxs-lookup"><span data-stu-id="b59c9-189">a.</span></span> <span data-ttu-id="b59c9-190">Kattintson a **Hozzáadás attribútum** megnyitásához a **attribútum hozzáadása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b59c9-190">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_attribute_04.png)

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_attribute_05.png)
    
    <span data-ttu-id="b59c9-193">b.</span><span class="sxs-lookup"><span data-stu-id="b59c9-193">b.</span></span> <span data-ttu-id="b59c9-194">Az a **neve** szövegmező, írja be az adott sorhoz feltüntetett attribútumot nevét.</span><span class="sxs-lookup"><span data-stu-id="b59c9-194">In the **Name** textbox, type the attribute name shown for that row.</span></span>

    <span data-ttu-id="b59c9-195">c.</span><span class="sxs-lookup"><span data-stu-id="b59c9-195">c.</span></span> <span data-ttu-id="b59c9-196">Az a **érték** listára, válassza ki a megvalósítás a használni kívánt felhasználói attribútum.</span><span class="sxs-lookup"><span data-stu-id="b59c9-196">In the **Value** list, select the user attribute you want to use for your implementation.</span></span>
    <span data-ttu-id="b59c9-197">Például, ha szeretné használni az EmployeeID egyedi felhasználónévvel és azonosító tárolt attribútum értéke a ExtensionAttribute2, majd válassza ki **user.extensionattribute2**.</span><span class="sxs-lookup"><span data-stu-id="b59c9-197">For example, if you want to use the EmployeeID as unique user identifier and you have stored the attribute value in the ExtensionAttribute2, then select **user.extensionattribute2**.</span></span>
    
    <span data-ttu-id="b59c9-198">d.</span><span class="sxs-lookup"><span data-stu-id="b59c9-198">d.</span></span> <span data-ttu-id="b59c9-199">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="b59c9-199">Click **Ok**.</span></span>

7. <span data-ttu-id="b59c9-200">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="b59c9-200">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_400.png)
    
8. <span data-ttu-id="b59c9-202">A a **Ceridian Dayforce HCM konfigurációs** kattintson **Ceridian Dayforce HCM konfigurálása** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="b59c9-202">On the **Ceridian Dayforce HCM Configuration** section, click **Configure Ceridian Dayforce HCM** to open **Configure sign-on** window.</span></span> <span data-ttu-id="b59c9-203">Másolás a **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="b59c9-203">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Ceridian Dayforce HCM konfiguráció](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_configure.png) 

9. <span data-ttu-id="b59c9-205">Egyszeri bejelentkezés konfigurálása **Ceridian Dayforce HCM** oldalon kell küldeniük a letöltött **metaadatainak XML-kódja** és **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** való [Ceridian Dayforce HCM támogatási csoport](https://www.ceridian.com/contact-us/index.html).</span><span class="sxs-lookup"><span data-stu-id="b59c9-205">To configure single sign-on on **Ceridian Dayforce HCM** side, you need to send the downloaded **Metadata XML** and **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** to [Ceridian Dayforce HCM support team](https://www.ceridian.com/contact-us/index.html).</span></span>

> [!TIP]
> <span data-ttu-id="b59c9-206">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="b59c9-206">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="b59c9-207">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="b59c9-207">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="b59c9-208">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="b59c9-208">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="b59c9-209">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="b59c9-209">Create an Azure AD test user</span></span>

<span data-ttu-id="b59c9-210">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="b59c9-210">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Hozzon létre egy Azure AD-teszt felhasználó][100]

<span data-ttu-id="b59c9-212">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="b59c9-212">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="b59c9-213">Az Azure portálon a bal oldali ablaktáblán kattintson a **Azure Active Directory** gombra.</span><span class="sxs-lookup"><span data-stu-id="b59c9-213">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Az Azure Active Directory gomb](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="b59c9-215">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok**, és kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="b59c9-215">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    ![A "felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="b59c9-217">Megnyitásához a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** tetején a **minden felhasználó** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="b59c9-217">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![A Hozzáadás gombra.](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="b59c9-219">Az a **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="b59c9-219">In the **User** dialog box, perform the following steps:</span></span>

    ![A felhasználó párbeszédpanel](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_04.png)

    <span data-ttu-id="b59c9-221">a.</span><span class="sxs-lookup"><span data-stu-id="b59c9-221">a.</span></span> <span data-ttu-id="b59c9-222">Az a **neve** mezőbe írja be **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="b59c9-222">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="b59c9-223">b.</span><span class="sxs-lookup"><span data-stu-id="b59c9-223">b.</span></span> <span data-ttu-id="b59c9-224">Az a **felhasználónév** mezőbe írja be a felhasználó e-mail címe az Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b59c9-224">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="b59c9-225">c.</span><span class="sxs-lookup"><span data-stu-id="b59c9-225">c.</span></span> <span data-ttu-id="b59c9-226">Válassza ki a **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a megjelenített érték a **jelszó** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="b59c9-226">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="b59c9-227">d.</span><span class="sxs-lookup"><span data-stu-id="b59c9-227">d.</span></span> <span data-ttu-id="b59c9-228">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="b59c9-228">Click **Create**.</span></span>
 
### <a name="create-a-ceridian-dayforce-hcm-test-user"></a><span data-ttu-id="b59c9-229">Ceridian Dayforce HCM tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="b59c9-229">Create a Ceridian Dayforce HCM test user</span></span>

<span data-ttu-id="b59c9-230">Ez a szakasz célja Ceridian Dayforce HCM Britta Simon nevű felhasználót létrehozni.</span><span class="sxs-lookup"><span data-stu-id="b59c9-230">The objective of this section is to create a user called Britta Simon in Ceridian Dayforce HCM.</span></span> <span data-ttu-id="b59c9-231">Együttműködik az [Ceridian Dayforce HCM támogatási csoport](https://www.ceridian.com/contact-us/index.html) felhasználók felvétele az Ceridian Dayforce HCM alkalmazás eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="b59c9-231">Work with the [Ceridian Dayforce HCM support team](https://www.ceridian.com/contact-us/index.html) to get users added in the Ceridian Dayforce HCM application.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="b59c9-232">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="b59c9-232">Assigning the Azure AD test user</span></span>

<span data-ttu-id="b59c9-233">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Ceridian Dayforce HCM Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="b59c9-233">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Ceridian Dayforce HCM.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="b59c9-235">**Britta Simon hozzárendelése Ceridian Dayforce HCM, hajtsa végre a következő lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="b59c9-235">**To assign Britta Simon to Ceridian Dayforce HCM, perform the following steps:**</span></span>

1. <span data-ttu-id="b59c9-236">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="b59c9-236">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="b59c9-238">Az alkalmazások listában válassza ki a **Ceridian Dayforce HCM**.</span><span class="sxs-lookup"><span data-stu-id="b59c9-238">In the applications list, select **Ceridian Dayforce HCM**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_app.png) 

3. <span data-ttu-id="b59c9-240">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="b59c9-240">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="b59c9-242">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="b59c9-242">Click **Add** button.</span></span> <span data-ttu-id="b59c9-243">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b59c9-243">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="b59c9-245">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="b59c9-245">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="b59c9-246">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b59c9-246">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="b59c9-247">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b59c9-247">Click **Assign** button on **Add Assignment** dialog.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="b59c9-248">Rendelje hozzá az Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="b59c9-248">Assign the Azure AD test user</span></span>

<span data-ttu-id="b59c9-249">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Ceridian Dayforce HCM Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="b59c9-249">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Ceridian Dayforce HCM.</span></span>

![A felhasználói szerepkör hozzárendelése][200] 

<span data-ttu-id="b59c9-251">**Britta Simon hozzárendelése Ceridian Dayforce HCM, hajtsa végre a következő lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="b59c9-251">**To assign Britta Simon to Ceridian Dayforce HCM, perform the following steps:**</span></span>

1. <span data-ttu-id="b59c9-252">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="b59c9-252">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="b59c9-254">Az alkalmazások listában válassza ki a **Ceridian Dayforce HCM**.</span><span class="sxs-lookup"><span data-stu-id="b59c9-254">In the applications list, select **Ceridian Dayforce HCM**.</span></span>

    ![Az alkalmazások listáját a Ceridian Dayforce HCM hivatkozás](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_app.png)  

3. <span data-ttu-id="b59c9-256">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="b59c9-256">In the menu on the left, click **Users and groups**.</span></span>

    ![A "Felhasználók és csoportok" hivatkozásra][202]

4. <span data-ttu-id="b59c9-258">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="b59c9-258">Click **Add** button.</span></span> <span data-ttu-id="b59c9-259">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b59c9-259">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![A hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="b59c9-261">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="b59c9-261">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="b59c9-262">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b59c9-262">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="b59c9-263">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b59c9-263">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="b59c9-264">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="b59c9-264">Test single sign-on</span></span>

<span data-ttu-id="b59c9-265">Ez a szakasz célja tesztelése az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen.</span><span class="sxs-lookup"><span data-stu-id="b59c9-265">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>  
<span data-ttu-id="b59c9-266">Ha a hozzáférési panelen Ceridian Dayforce HCM csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az Ceridian Dayforce HCM alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="b59c9-266">When you click the Ceridian Dayforce HCM tile in the Access Panel, you should get automatically signed-on to your Ceridian Dayforce HCM application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="b59c9-267">További források</span><span class="sxs-lookup"><span data-stu-id="b59c9-267">Additional resources</span></span>

* [<span data-ttu-id="b59c9-268">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="b59c9-268">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="b59c9-269">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="b59c9-269">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_203.png

